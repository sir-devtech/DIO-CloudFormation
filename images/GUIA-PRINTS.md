# Guia de Capturas de Tela para o Desafio DIO

Este guia lista **exatamente quais prints** você deve capturar durante a prática e como organizá-los na pasta `/images`.

## Como capturar (Windows)

- **Tecla Print Screen** — captura a tela inteira
- **Win + Shift + S** — recorte seletivo (recomendado)
- Salve os arquivos com nomes descritivos, ex.: `01-console-aws-home.png`

## Prints obrigatórios (mínimo para entrega)

### 1. Console AWS — Página inicial
**Arquivo sugerido:** `01-console-aws-home.png`

- Acesse https://console.aws.amazon.com/
- Confirme a região **América do Sul (São Paulo)** no canto superior direito
- Capture a página inicial com seus serviços recentes

> Você já tem um print similar — pode reutilizar se a região estiver visível.

---

### 2. CloudFormation — Lista de stacks
**Arquivo sugerido:** `02-cloudformation-stacks.png`

1. Pesquise **CloudFormation** na barra de busca do console
2. Clique em **Stacks**
3. Capture a tela com sua stack listada (status **CREATE_COMPLETE**)

---

### 3. CloudFormation — Criação da stack (upload do template)
**Arquivo sugerido:** `03-create-stack-template.png`

1. Clique em **Create stack** → **With new resources (standard)**
2. Selecione **Upload a template file**
3. Faça upload de `templates/infra-basica.yaml`
4. Capture a tela mostrando o arquivo selecionado antes de clicar em **Next**

---

### 4. CloudFormation — Parâmetros da stack
**Arquivo sugerido:** `04-stack-parameters.png`

1. Na etapa **Specify stack details**
2. Preencha o nome da stack: `dio-cf-infra-basica`
3. Capture os parâmetros `ProjectName` e `Environment` visíveis

---

### 5. CloudFormation — Eventos da stack
**Arquivo sugerido:** `05-stack-events.png`

1. Após iniciar a criação, abra sua stack
2. Vá na aba **Events**
3. Capture quando o status estiver **CREATE_COMPLETE** (todos os eventos verdes)

---

### 6. CloudFormation — Recursos criados
**Arquivo sugerido:** `06-stack-resources.png`

1. Na mesma stack, abra a aba **Resources**
2. Capture a lista mostrando o bucket S3 e a bucket policy

---

### 7. CloudFormation — Outputs
**Arquivo sugerido:** `07-stack-outputs.png`

1. Abra a aba **Outputs**
2. Capture os valores: `BucketName`, `BucketArn`, `StackRegion`, `ProjectTag`

---

### 8. S3 — Bucket criado
**Arquivo sugerido:** `08-s3-bucket.png`

1. Pesquise **S3** no console
2. Localize o bucket criado pela stack (nome contém `dio-cloudformation-lab`)
3. Capture a tela do bucket com as propriedades visíveis (versionamento, tags)

---

## Prints opcionais (enriquecem a documentação)

### 9. Infrastructure Composer / Designer
**Arquivo sugerido:** `09-infrastructure-composer.png`

1. CloudFormation → **Create stack**
2. Explore **Infrastructure Composer** ou **Designer**
3. Capture a interface visual (como visto nas aulas)

---

### 10. IaC Generator
**Arquivo sugerido:** `10-iac-generator.png`

1. No menu lateral do CloudFormation, clique em **IaC generator**
2. Capture a tela com os 3 passos (Scan → Create template → Import)

---

### 11. Template YAML no editor
**Arquivo sugerido:** `11-template-yaml.png`

1. Abra o arquivo `templates/infra-basica.yaml` no VS Code ou Cursor
2. Capture a seção de Parameters e Resources

---

### 12. Stack LAMP (se implantou)
**Arquivo sugerido:** `12-lamp-website.png`

1. Se implantou `lamp-single-instance.yaml`
2. Abra a `WebsiteURL` dos Outputs no navegador
3. Capture a página "LAMP Stack - DIO CloudFormation Lab"

---

### 13. Exclusão da stack
**Arquivo sugerido:** `13-stack-delete.png`

1. Selecione a stack → **Delete**
2. Capture a confirmação ou o status **DELETE_COMPLETE**

---

## Checklist rápido

```
[ ] 01 - Console AWS (região visível)
[ ] 02 - Lista de stacks
[ ] 03 - Upload do template
[ ] 04 - Parâmetros preenchidos
[ ] 05 - Events CREATE_COMPLETE
[ ] 06 - Resources
[ ] 07 - Outputs
[ ] 08 - Bucket S3 no console
[ ] (opcional) 09 - Infrastructure Composer
[ ] (opcional) 10 - IaC Generator
[ ] (opcional) 11 - Template no editor
[ ] (opcional) 12 - Site LAMP no browser
[ ] (opcional) 13 - Stack deletada
```

## Como adicionar ao repositório

Após capturar, copie os arquivos para a pasta `images/`:

```powershell
# Exemplo no PowerShell (ajuste o caminho de origem)
Copy-Item "C:\Users\moise\Pictures\Screenshots\*.png" `
  -Destination "c:\Users\moise\OneDrive\Documentos\DIO PROJETOS\AWS CLOUD FORMATION\images\"
```

Depois faça commit e push:

```powershell
cd "c:\Users\moise\OneDrive\Documentos\DIO PROJETOS\AWS CLOUD FORMATION"
git add images/
git commit -m "docs: adiciona capturas de tela do laboratorio CloudFormation"
git push origin main
```

## Referenciar no README

Após adicionar as imagens, você pode incluir no README:

```markdown
## Evidências da prática

![Console AWS](images/01-console-aws-home.png)
![Stack CREATE_COMPLETE](images/05-stack-events.png)
![Outputs da Stack](images/07-stack-outputs.png)
```

---

*Siga este guia na ordem enquanto executa o deploy — assim você não precisa refazer etapas só para tirar prints.*
