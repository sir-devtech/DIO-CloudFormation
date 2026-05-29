# Anotações e Insights — AWS CloudFormation

Documentação das reflexões e aprendizados adquiridos durante o laboratório da DIO.

---

## O que é AWS CloudFormation?

O **AWS CloudFormation** é um serviço de **Infraestrutura como Código (IaC)** que permite modelar, provisionar e gerenciar recursos AWS de forma automatizada. Em vez de criar recursos manualmente no console, descrevemos a infraestrutura desejada em um **template** (JSON ou YAML) e a AWS cuida da criação e do ciclo de vida.

### Benefícios identificados na prática

1. **Reprodutibilidade** — O mesmo template pode recriar a infraestrutura em qualquer conta ou região
2. **Versionamento** — Templates podem ser versionados no Git junto com o código da aplicação
3. **Consistência** — Elimina erros humanos de configuração manual
4. **Rollback automático** — Se a criação falhar, a stack pode reverter ao estado anterior
5. **Dependências implícitas** — CloudFormation resolve a ordem de criação dos recursos
6. **Integração nativa** — Funciona diretamente no console AWS, sem instalação extra

---

## Estrutura de um template CloudFormation

Durante o laboratório, trabalhamos com as seções principais:

| Seção | Função |
|-------|--------|
| `AWSTemplateFormatVersion` | Versão do formato do template |
| `Description` | Descrição legível da stack |
| `Parameters` | Valores de entrada configuráveis |
| `Mappings` | Valores estáticos por região ou ambiente |
| `Conditions` | Regras condicionais para criação de recursos |
| `Resources` | Recursos AWS a serem criados (obrigatório) |
| `Outputs` | Valores exportados após a implantação |
| `Metadata` | Metadados para organização da UI no console |

### Exemplo prático — Parameters

Conforme visto nas aulas, parâmetros permitem customizar a stack sem editar o template:

```yaml
Parameters:
  InstanceType:
    Type: String
    Default: t2.micro
    AllowedValues:
      - t2.micro
      - t2.small
    Description: Tipo de instancia EC2
```

Isso gera uma interface amigável no assistente de criação de stack, com validação automática.

---

## Infrastructure Composer e IaC Generator

### Infrastructure Composer

Ferramenta visual da AWS para desenhar arquiteturas e gerar templates CloudFormation. Nas aulas, foi demonstrado o template **LAMP Single Instance**, que inclui:

- Parâmetros como `KeyName`, `InstanceType`, `DBRootPassword`
- Instância EC2 com stack LAMP (Linux, Apache, MySQL, PHP)
- Security Groups e configuração via UserData

### IaC Generator

Permite escanear recursos existentes na conta e gerar templates automaticamente. O fluxo é:

1. **Scan** — Descobre recursos e relacionamentos na conta
2. **Create template** — Seleciona recursos para incluir no template
3. **Import** — Importa para CloudFormation ou converte para CDK

> Insight: O IaC Generator é útil para **documentar infraestrutura legada**, mas templates gerados automaticamente podem precisar de refinamento manual.

---

## CloudFormation vs Terraform

Durante a aula, foi apresentada a comparação entre as duas ferramentas:

| Critério | CloudFormation | Terraform |
|----------|---------------|-----------|
| Escopo | Apenas AWS | Multi-cloud |
| Curva de aprendizado | YAML/JSON + conceitos AWS | HCL + providers |
| Estado | Gerenciado pela AWS | Arquivo `.tfstate` |
| Comunidade | Ecossistema AWS | HashiCorp + comunidade ampla |
| Caso de uso | Projetos 100% AWS | Ambientes multi-cloud |

**Conclusão pessoal:** CloudFormation é a escolha natural quando o ecossistema é exclusivamente AWS e se deseja integração profunda com o console e serviços nativos. Terraform brilha em cenários multi-cloud ou quando a equipe já possui expertise em HCL.

---

## Experiência prática — Stack básica (S3)

### O que foi feito

1. Criação do template `infra-basica.yaml` com bucket S3 seguro
2. Implantação via console CloudFormation
3. Validação dos Outputs (nome do bucket, ARN, região)
4. Verificação das tags aplicadas automaticamente
5. Exclusão da stack ao finalizar

### Boas práticas aplicadas

- **BucketName** com sufixo `${AWS::AccountId}` para garantir unicidade global
- **PublicAccessBlock** habilitado em todos os flags
- **BucketPolicy** negando tráfego não-HTTPS
- **Versioning** habilitado para rastreabilidade
- **Tags** padronizadas (`Project`, `Environment`, `ManagedBy`)
- **DeletionPolicy: Delete** para facilitar limpeza em ambientes de lab

---

## Experiência prática — Stack LAMP (opcional)

### O que demonstra

- Uso de `AWS::EC2::KeyPair::KeyName` como tipo de parâmetro especial
- `NoEcho: true` para senhas (não exibidas no console)
- `AllowedPattern` e `ConstraintDescription` para validação
- `UserData` com script bash para bootstrap da instância
- Resolução dinâmica de AMI via SSM Parameter Store

### Cuidados importantes

- EC2 gera custo — usar `t2.micro` no Free Tier quando possível
- Restringir `SSHLocation` ao seu IP (`/32`) em vez de `0.0.0.0/0`
- Sempre deletar a stack após o laboratório
- AMIs podem variar por região — o template usa SSM para obter a AMI mais recente

---

## Fluxo de trabalho recomendado

```
1. Escrever/revisar template YAML
2. Validar sintaxe (CloudFormation Designer ou cfn-lint)
3. Criar stack no console ou via CLI
4. Monitorar Events até CREATE_COMPLETE
5. Validar Resources e Outputs
6. Documentar com screenshots
7. Deletar stack ao finalizar
```

---

## Ferramentas complementares

| Ferramenta | Uso |
|------------|-----|
| **CloudFormation Designer** | Edição visual de templates |
| **Infrastructure Composer** | Design de arquitetura + geração de código |
| **IaC Generator** | Reverse engineering de recursos existentes |
| **AWS CLI** | Automação via linha de comando |
| **cfn-lint** | Validação local de templates |

---

## Conclusão

O laboratório reforçou que **Infraestrutura como Código** não é apenas uma tendência, mas uma prática essencial para:

- Ambientes escaláveis e auditáveis
- Deployments consistentes entre dev, homolog e produção
- Redução de tempo em tarefas repetitivas
- Documentação viva da arquitetura (o template *é* a documentação)

O AWS CloudFormation, por ser nativo da plataforma, oferece integração profunda e curva de aprendizado acessível para quem já trabalha com serviços AWS. Este repositório serve como material de referência para futuras implementações.

---

*Documento elaborado como parte do desafio DIO — Implementando Infraestrutura Automatizada com AWS CloudFormation.*
