# 📦 Documentação de Deploy — SmartHouse 3D

> Guia completo para publicar, manter e atualizar o projeto no GitHub e GitHub Pages.

---

## Índice

1. [Pré-requisitos](#pré-requisitos)
2. [Criar repositório no GitHub](#criar-repositório-no-github)
3. [Organizar os arquivos](#organizar-os-arquivos)
4. [Primeiro commit e push](#primeiro-commit-e-push)
5. [Publicar via GitHub Pages](#publicar-via-github-pages)
6. [Acessar o projeto online](#acessar-o-projeto-online)
7. [Atualizar o projeto (versões futuras)](#atualizar-o-projeto-versões-futuras)
8. [Manter o projeto](#manter-o-projeto)
9. [Dicas e boas práticas](#dicas-e-boas-práticas)

---

## Pré-requisitos

Antes de começar, você precisa ter:

- Conta no [GitHub](https://github.com) (gratuita)
- [Git](https://git-scm.com/downloads) instalado no computador
- Um editor de texto (VS Code recomendado)

Verifique se o Git está instalado:
```bash
git --version
# git version 2.x.x
```

---

## Criar repositório no GitHub

### Opção A — Pelo site (mais fácil)

1. Acesse [github.com/new](https://github.com/new)
2. Preencha:
   - **Repository name**: `smarthouse3d` (ou o nome que preferir)
   - **Description**: `Gerador procedural de plantas 3D com Three.js`
   - **Visibility**: `Public` (necessário para GitHub Pages gratuito)
   - ❌ Não marque "Add a README file" (já temos um)
3. Clique em **Create repository**
4. Copie a URL do repositório (ex: `https://github.com/seunome/smarthouse3d.git`)

### Opção B — Via terminal

```bash
# Se tiver GitHub CLI instalado:
gh repo create smarthouse3d --public --description "Gerador procedural de plantas 3D"
```

---

## Organizar os arquivos

Crie uma pasta local para o projeto:

```bash
mkdir smarthouse3d
cd smarthouse3d
```

Mova os arquivos para esta pasta:

```
smarthouse3d/
├── index.html            ← RENOMEIE maquete3d_v10.html para index.html
├── README.md
└── DOCUMENTACAO.md
```

> ⚠️ **Importante**: Renomeie `maquete3d_v10.html` para **`index.html`**.  
> O GitHub Pages serve automaticamente o arquivo `index.html` como página principal.

```bash
# No terminal, dentro da pasta:
mv maquete3d_v10.html index.html
```

---

## Primeiro commit e push

```bash
# 1. Entrar na pasta do projeto
cd smarthouse3d

# 2. Inicializar o repositório Git local
git init

# 3. Adicionar todos os arquivos
git add .

# 4. Criar o primeiro commit
git commit -m "feat: SmartHouse 3D v10 - edição universal + lógica de banheiros"

# 5. Definir o branch principal como 'main'
git branch -M main

# 6. Conectar ao repositório remoto (substitua pela sua URL)
git remote add origin https://github.com/seunome/smarthouse3d.git

# 7. Enviar para o GitHub
git push -u origin main
```

Se pedir autenticação, use seu usuário e um **Personal Access Token** (não a senha).  
Crie um token em: *GitHub → Settings → Developer Settings → Personal Access Tokens → Tokens (classic)*

---

## Publicar via GitHub Pages

### Pelo site do GitHub

1. Abra o repositório no GitHub
2. Clique em **Settings** (aba superior)
3. No menu lateral, clique em **Pages**
4. Em **Source**, selecione:
   - Branch: `main`
   - Folder: `/ (root)`
5. Clique em **Save**

Aguarde 1–2 minutos e o site estará disponível.

### Via terminal (GitHub CLI)

```bash
gh repo edit --enable-pages --pages-branch main
```

---

## Acessar o projeto online

Após publicar, a URL será:

```
https://seunome.github.io/smarthouse3d/
```

Exemplo real:
```
https://joaosilva.github.io/smarthouse3d/
```

Você pode compartilhar essa URL com qualquer pessoa — sem necessidade de servidor ou hospedagem paga.

---

## Atualizar o projeto (versões futuras)

Sempre que fizer melhorias no arquivo `index.html`:

```bash
# 1. Verificar o que mudou
git status
git diff index.html

# 2. Adicionar as mudanças
git add index.html

# 3. Criar commit com mensagem descritiva
git commit -m "feat: adiciona nova funcionalidade X"
# ou
git commit -m "fix: corrige bug no modo edição"
# ou
git commit -m "refactor: melhora lógica de banheiros"

# 4. Enviar para o GitHub
git push origin main
```

O GitHub Pages atualiza automaticamente em ~1 minuto após o push.

### Padrão de mensagens de commit

Use prefixos para organizar o histórico:

| Prefixo | Quando usar |
|---|---|
| `feat:` | Nova funcionalidade |
| `fix:` | Correção de bug |
| `refactor:` | Melhoria interna sem mudar comportamento |
| `docs:` | Atualização de documentação |
| `style:` | Mudanças visuais/CSS |
| `perf:` | Melhoria de performance |

---

## Manter o projeto

### Criar tags de versão

Para marcar versões importantes:

```bash
# Criar tag v10
git tag -a v10.0 -m "SmartHouse 3D v10 - Edição Universal + Banheiro Social"

# Enviar tag para o GitHub
git push origin v10.0

# Listar todas as tags
git tag
```

### Ver histórico de commits

```bash
# Histórico resumido
git log --oneline

# Histórico completo
git log
```

### Voltar para uma versão anterior

```bash
# Ver o hash do commit que quer restaurar
git log --oneline

# Restaurar arquivo específico de um commit anterior
git checkout abc1234 -- index.html

# Ou criar um branch a partir de uma versão antiga
git checkout -b v9-backup abc1234
```

### Criar branches para novas funcionalidades

```bash
# Criar e mudar para novo branch
git checkout -b feature/melhoria-cozinha

# Trabalhar no arquivo...

# Commit na nova branch
git add .
git commit -m "feat: melhora layouts da cozinha"

# Mesclar na main quando pronto
git checkout main
git merge feature/melhoria-cozinha

# Enviar
git push origin main
```

---

## Dicas e boas práticas

### ✅ Fazer sempre
- Commit com mensagens claras e descritivas
- Testar localmente antes de fazer push
- Usar `git status` para verificar o que será commitado
- Manter o `README.md` atualizado a cada versão

### ❌ Evitar
- Commitar arquivos temporários (`.DS_Store`, `thumbs.db`, etc.)
- Fazer push direto sem testar
- Mensagens de commit genéricas como "update" ou "fix"

### .gitignore recomendado

Crie um arquivo `.gitignore` na pasta do projeto:

```
# Sistema operacional
.DS_Store
Thumbs.db
desktop.ini

# Editor
.vscode/
*.swp
*.swo

# Temporários
*.tmp
*.bak
```

```bash
# Adicionar o .gitignore
git add .gitignore
git commit -m "docs: adiciona .gitignore"
git push
```

### Domínio personalizado (opcional)

Para usar seu próprio domínio (ex: `smarthouse3d.com.br`):

1. Crie um arquivo chamado `CNAME` na raiz do projeto com o conteúdo:
   ```
   smarthouse3d.com.br
   ```
2. No seu provedor de domínio, configure os registros DNS:
   ```
   CNAME  www   seunome.github.io
   A      @     185.199.108.153
   A      @     185.199.109.153
   A      @     185.199.110.153
   A      @     185.199.111.153
   ```
3. No GitHub Pages, informe o domínio personalizado

---

## Estrutura recomendada do repositório

```
smarthouse3d/
│
├── index.html            # Ferramenta principal (SmartHouse 3D)
├── README.md             # Documentação da ferramenta
├── DOCUMENTACAO.md       # Este guia de deploy
├── .gitignore            # Arquivos ignorados pelo Git
│
└── extras/               # (opcional) arquivos extras
    └── archviz_promo.html  # Landing page / propaganda
```

---

## Checklist de publicação

Antes de publicar uma nova versão, verifique:

- [ ] O arquivo principal está nomeado como `index.html`
- [ ] Abriu o arquivo no navegador e testou todas as funções
- [ ] O Tour 360° funciona
- [ ] A geração de planta funciona
- [ ] O modo Editar funciona
- [ ] Os objetos externos (piscina, garagem etc.) surgem fora da casa
- [ ] Não há erros no console do navegador (F12)
- [ ] O README.md está atualizado com as novidades da versão
- [ ] Commit feito com mensagem descritiva
- [ ] Push enviado com `git push origin main`

---

*SmartHouse 3D — Documentação de Deploy · GitHub Pages*
