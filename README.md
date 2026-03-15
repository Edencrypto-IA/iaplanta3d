# 🏠 SmartHouse 3D — Gerador Procedural de Plantas Arquitetônicas

> Ferramenta web para geração e visualização de plantas baixas em 3D, com mobiliário automático, área externa interativa, edição universal de objetos e tour 360°.

---

## 📋 Índice

1. [O que é](#o-que-é)
2. [Funcionalidades](#funcionalidades)
3. [Como rodar localmente](#como-rodar-localmente)
4. [Como usar](#como-usar)
5. [Lógica de planta](#lógica-de-planta)
6. [Lógica de suítes e banheiro social](#lógica-de-suítes-e-banheiro-social)
7. [Modo Edição Universal](#modo-edição-universal)
8. [Dependências](#dependências)
9. [Estrutura de arquivos](#estrutura-de-arquivos)

---

## O que é

**SmartHouse 3D** é uma ferramenta single-page (HTML único, zero backend) que gera plantas baixas residenciais proceduralmente em 3D diretamente no navegador. O usuário informa as dimensões do terreno, o programa de necessidades (quartos, suítes, banheiros, salas, etc.) e a ferramenta constrói automaticamente a planta com paredes, pisos, janelas, portas, mobiliário e área externa.

---

## Funcionalidades

### 🏗️ Gerador de Planta IA
- Entrada de dimensões do terreno (largura × comprimento)
- Configuração de: quartos, suítes, banheiros sociais, sala, cozinha, garagem, área de serviço
- Geração procedural automática da planta 3D
- Análise de viabilidade (Alta / Média / Baixa) com observações arquitetônicas
- Variações da planta com 1 clique

### 🏠 Estrutura 3D Completa
- Paredes externas com janelas e portas cortadas
- Paredes internas divisórias
- Piso diferenciado por ambiente (madeira / cerâmica)
- Teto com laje e telha
- Iluminação pontual por cômodo

### 🛋️ Mobiliário Automático por Cômodo
| Cômodo | Móveis gerados |
|---|---|
| **Sala** | Sofá, rack+TV, mesa de centro, tapete, luminária, quadro |
| **Quarto / Suíte** | Cama, guarda-roupa, mesinha de cabeceira, tapete |
| **Cozinha** | 3 layouts (linear, L, compacta+mesa), geladeira, fogão, pia, bancada, armário suspenso, banquetas |
| **Banheiro** | Box+chuveiro, pia+espelho, vaso sanitário |

### 🌳 Área Externa (Objetos Arrastáveis)
- **Piscina Retangular** com deck de madeira e escada
- **Prainha / Raia** com palmeiras e pedras decorativas
- **Churrasqueira** com coifa, bancada e cuba
- **Espreguiçadeiras** com sombrinha e mesinha
- **Pergolado** estrutural
- **Mesa + Cadeiras** externas
- **Garagem** com teto transparente, portão aberto e 2 carros 3D
- **Paisagismo**: árvores, jardim com flores, cerca, calçada

Todos os objetos acima são **arrastáveis** (drag & drop), **rotacionáveis** e **removíveis** via gizmo.

### ✏️ Modo Edição Universal
- Clique em **Editar** para entrar no modo de edição
- Selecione e mova **qualquer objeto da cena**: móveis, casa, telhado, externos, garagem, piscina
- BoxHelper dourado (móveis internos) e ciano (objetos externos / estrutura)
- ESC para sair do modo de edição

### 🎥 Câmera e Navegação
- Vista Perspectiva e Vista Superior
- Zoom suave com inércia
- Rotação com damping
- Pan com botão direito
- **Tour 360°** automático cinematográfico com órbita contínua

### 🎨 Decoração
- Troca de cor de paredes, sofá, piso, tapete e teto em tempo real

---

## Como rodar localmente

Não há instalação ou backend necessário. Basta:

```bash
# 1. Baixe o arquivo
# maquete3d_v10.html

# 2. Abra no navegador
# Duplo clique no arquivo OU arraste para a janela do navegador
```

**Requisitos mínimos:**
- Navegador moderno com WebGL 2 (Chrome 90+, Firefox 88+, Edge 90+, Safari 15+)
- GPU integrada ou dedicada
- Conexão com internet (apenas para carregar as fontes Google Fonts)

> **Offline:** funciona completamente offline exceto pelas fontes tipográficas (Cormorant Garamond + Jost), que podem ser embutidas se necessário.

---

## Como usar

### Passo 1 — Gerar a planta
1. Clique em **Planta IA** na barra inferior
2. Informe as dimensões do terreno em metros
3. Configure quartos, suítes, banheiros sociais e ambientes desejados
4. Clique em **Gerar Planta 3D**

### Passo 2 — Navegar na cena
- **Arrastar** (botão esquerdo): rotacionar câmera
- **Scroll**: zoom suave
- **Botão direito + arrastar**: pan (mover câmera)
- **Tour 360°**: órbita automática cinematográfica

### Passo 3 — Personalizar área externa
1. Use o **Painel Esquerdo** para ativar: piscinas, churrasqueira, espreguiçadeiras, garagem, paisagismo
2. Os objetos surgem **fora da casa** prontos para posicionar
3. Clique no objeto para selecionar → arraste para reposicionar
4. Use o **gizmo** (barra que aparece) para rotacionar ou remover

### Passo 4 — Edição livre
1. Clique em **Editar** na barra inferior
2. Clique em qualquer elemento da cena para selecioná-lo
3. Arraste para mover livremente
4. Pressione **ESC** ou clique **Editar** novamente para sair

### Passo 5 — Decoração
- Clique no **botão dourado** (canto inferior direito)
- Troque cores de paredes, sofá, piso, teto e tapete

---

## Lógica de planta

A ferramenta divide o terreno em duas zonas:

```
┌─────────────────┬─────────────────┐
│                 │                 │
│  ZONA PRIVADA   │  ZONA SOCIAL    │
│  (x: 0..midX)  │  (x: midX..W)  │
│                 │                 │
│  Quartos        │  Sala           │
│  Suítes         │  Cozinha        │
│  Banheiros      │  Garagem        │
│                 │  Área Serviço   │
└─────────────────┴─────────────────┘
```

**Zona Privada (esquerda):**
- Os quartos são distribuídos verticalmente dividindo a profundidade do terreno
- Suítes ficam nos primeiros quartos (topo)
- Banheiros de suíte ficam integrados ao quarto correspondente
- Banheiros sociais ficam na circulação comum

**Zona Social (direita):**
- Garagem fica no topo (próxima à entrada)
- Cozinha + sala ocupam o restante
- Área de serviço fica no fundo

---

## Lógica de Suítes e Banheiro Social

### Regra principal
A ferramenta **diferencia claramente** suíte (banheiro integrado ao quarto) de banheiro social (acessível pela circulação comum).

### Campos no painel
| Campo | Descrição |
|---|---|
| **Quartos (total)** | Número total de quartos da zona privada |
| **Suítes** | Quantos desses quartos têm banheiro próprio integrado |
| **Banh. Sociais** | Quantos banheiros ficam na área de circulação comum |

### Exemplos
```
Quartos: 2 | Suítes: 1 | Banh. Sociais: 1
→ Suíte Casal (com banheiro integrado)
→ Quarto 02 (sem banheiro próprio)
→ Banheiro Social (na circulação)
Total: 2 banheiros

Quartos: 3 | Suítes: 2 | Banh. Sociais: 1
→ Suíte Casal + banheiro integrado
→ Suíte 2 + banheiro integrado
→ Quarto 03 (simples)
→ Banheiro Social
Total: 3 banheiros

Quartos: 2 | Suítes: 0 | Banh. Sociais: 1
→ Quarto 01 (simples)
→ Quarto 02 (simples)
→ Banheiro Social
Total: 1 banheiro
```

### Posicionamento arquitetônico
- **Banheiro de suíte**: fica no fundo do quarto correspondente, com acesso privado
- **Banheiro social**: posicionado na zona de circulação da área privada, entre os quartos

---

## Modo Edição Universal

### Como funciona
1. Pressione **Editar** na barra inferior (ou **ESC** para sair)
2. No modo ativo, **clique em qualquer objeto** da cena:
   - Móveis internos (sofá, cama, mesa, etc.)
   - Casa / estrutura inteira
   - Telhado
   - Garagem, piscina, churrasqueira
   - Árvores, jardim, cerca (quando visíveis)
3. O objeto selecionado recebe um **highlight colorido** (box wireframe)
4. **Arraste** para mover livremente pelo terreno
5. Para remover (móveis/externos): botão **Remover Item**

### Prioridade de seleção
O sistema usa **raycasting** por profundidade — o objeto mais próximo da câmera na linha de visão é selecionado.

### Limitações
- A **estrutura da casa** pode ser movida mas não deletada (é a planta principal)
- O **telhado** segue a estrutura

---

## Dependências

Todas carregadas via CDN (sem npm / node_modules):

| Biblioteca | Versão | Uso |
|---|---|---|
| **Three.js** | r128 | Motor 3D (WebGL) |
| **GSAP** | 3.12.2 | Animações e transições |
| **Google Fonts** | — | Cormorant Garamond + Jost |

---

## Estrutura de arquivos

```
smarthouse3d/
│
├── maquete3d_v10.html    # Arquivo principal (tudo em um único arquivo)
├── README.md             # Este documento
├── DOCUMENTACAO.md       # Guia de deploy e GitHub
└── archviz_promo.html    # Propaganda / landing page
```

> O projeto é **zero-dependency localmente** — tudo está no HTML único. As bibliotecas são carregadas via CDN quando há internet.

---

## Histórico de versões

| Versão | Principais mudanças |
|---|---|
| v1–v3 | Estrutura base, câmera, materiais |
| v4 | Mobiliário por cômodo, câmera melhorada, Tour 360° |
| v5 | 2 opções de piscina, espreguiçadeiras, garagem com carro 3D |
| v6 | Painel lateral visível, badges "Novo" |
| v7 | Drag & drop externo, gizmo de seleção, fix posicionamento |
| v8 | Remoção de objetos estáticos duplicados do buildExternal |
| v9 | Piscinas arrastáveis, garagem corrigida (teto transparente, portão aberto) |
| **v10** | **Edição Universal** (move qualquer objeto), **Banheiro Social × Suíte** separados |

---

*SmartHouse 3D — Gerador Procedural · Desenvolvido com Three.js + GSAP*
