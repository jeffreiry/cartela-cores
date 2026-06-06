# Design System — Outono Quente

Tokens e padrões visuais do app, **extraídos do `index.html` atual** e
formalizados aqui como fonte de verdade. Nenhuma cor, fonte ou medida deve ser
introduzida no código sem estar registrada neste documento primeiro.

Estética de referência: **Nike / Adidas** — bold, editorial, limpo, funcional.
Uppercase onde importa, bordas nítidas, sem arredondamentos excessivos.

---

## Cores

### Fundos (backgrounds)

| Token | Hex | Uso |
|-------|-----|-----|
| `--bg` | `#F5EFE6` | Fundo principal da página |
| `--bg2` | `#EDE4D8` | Fundo alternativo (inputs, áreas secundárias) |
| `--bg3` | `#D8CFC4` | Fundo terciário (barras de progresso, bordas internas) |

### Superfícies

| Token | Hex | Uso |
|-------|-----|-----|
| `--surface` | `#FFFFFF` | Cards, modais, campos de formulário |
| `--surface2` | `#FAF7F3` | Superfícies levemente aquecidas (body de seção wizard) |

### Texto

| Token | Hex | Uso |
|-------|-----|-----|
| `--text` | `#1A1208` | Texto de corpo padrão |
| `--text2` | `#5C4535` | Texto secundário (meta, descrições) |
| `--text3` | `#9C8270` | Texto terciário (labels, hints) |
| `--text4` | `#C4AE98` | Texto desabilitado / placeholder |

### Tinta (escuro editorial)

| Token | Hex | Uso |
|-------|-----|-----|
| `--ink` | `#1A1208` | Cabeçalho (topbar), headings, botões primários |
| `--ink2` | `#2E2010` | Variação escura (hover do ink, estados profundos) |

### Acento — Terracota (cor da marca)

| Token | Hex | Uso |
|-------|-----|-----|
| `--accent` | `#C1603B` | **Primária** — CTAs, tab ativa, scrollbar, seleção |
| `--accent2` | `#B5731A` | Âmbar / dourado — badges premium, destaques cálidos |
| `--accent3` | `#C9A227` | Ouro — badges "favorito / star", tips de informação |

### Cores semânticas (status)

| Cor | Hex | Uso |
|-----|-----|-----|
| Verde closet | `#4E7A1A` | Item "Em uso" — checkbox marcado, badge "IN" |
| Âmbar alerta | `#997A10` | Score médio |
| Laranja baixo | `#B04F2A` | Score baixo |
| Vermelho crítico | `#8B2020` | Score muito baixo |
| Azul offline | `#5060A8` | Item "Fora de temporada" — badge "OFF", badge `.bo` |

### Bordas

| Token | Valor | Uso |
|-------|-------|-----|
| `--border` | `rgba(42,31,22,.09)` | Bordas padrão de cards e inputs |
| `--border2` | `rgba(193,96,59,.3)` | Borda de ênfase (accent) |

### Sombras

| Token | Valor | Uso |
|-------|-------|-----|
| `--sh` | `0 1px 2px rgba(42,31,22,.06), 0 3px 12px rgba(42,31,22,.05)` | Sombra sutil — cards em repouso |
| `--sh2` | `0 4px 20px rgba(42,31,22,.12)` | Sombra de elevação — hover, modais |

---

## Tipografia

Fonte única: **Plus Jakarta Sans** (Google Fonts)
Pesos em uso: 300 · 400 · 500 · 600 · 700 · 800

`font-size` base: `14px` no `html`.

### Escala de tamanhos (em `rem` relativo ao root de 14px)

| Papel | Tamanho | Peso | Observação |
|-------|---------|------|------------|
| Page heading / H2 | `2rem` (28px) | 900 | Uppercase, `letter-spacing: -1px` |
| Título de card / nome | `0.85rem` (11.9px) | 800 | — |
| Body / input | `0.85rem` | 400–500 | — |
| Section heading / H3 | `1rem` (14px) | 900 | Uppercase, `letter-spacing: 1px` |
| Tab / botão primário | `0.65rem` (9.1px) | 900 | Uppercase, `letter-spacing: 1.5px` |
| Label / meta | `0.65–0.68rem` | 700–800 | Uppercase, espaçado |
| Nota / caption | `0.6–0.64rem` | 600–700 | Às vezes itálico |
| Badge / pill | `0.65rem` | 900 | Uppercase, `letter-spacing: 0.8–1px` |

**Regras gerais:**
- `letter-spacing: 0` no corpo; espaçamento positivo em badges, labels e botões uppercase.
- Links sem sublinhado; cor `--ink`; hover `--ink2` ou `--accent`.
- `line-height` de corpo: 1.6–1.7. Headings: 1.

---

## Espaçamento e layout

| Token / uso | Valor | Onde |
|-------------|-------|------|
| Container máximo | `1400px` | `.hi`, `.main` |
| Padding horizontal do container | `24px` | — |
| Padding vertical da página | `36px` top · `60px` bottom | `.main` |
| Gap entre cards (grid) | `10px` | `.il` (grade de itens) |
| Gap geral de layout | `6 / 8 / 10 / 12 / 14 / 16 / 20 / 24 / 32px` | Conforme contexto |
| Margem entre seções | `28px` top | `.sh` |
| Topbar height | `56px` | `#header .ht` |

### Grid de itens

- **Desktop:** `repeat(2, 1fr)` — 2 colunas
- **Mobile (≤ 600px):** `repeat(2, 1fr)` — mantém 2 colunas (cards menores)
- Aspect ratio da miniatura: `3/4` (retrato)

### Breakpoints

| Faixa | Largura |
|-------|---------|
| Desktop | ≥ 1024px |
| Tablet | 601px – 1023px |
| Mobile | ≤ 600px |

---

## Border radius

| Token | Valor | Uso |
|-------|-------|-----|
| `--r` | `4px` | **Padrão global** — cards, inputs, badges, wizard |
| Sharp | `2px` | Botões de ação, pills, toast, badges inline |
| Round | `50%` | Avatares, dots decorativos, step-numbers |
| Form/modal interno | `4px` | Inputs dentro de modal |

> Filosofia: cantos **quase quadrados** — referência Nike. Evitar `8px+` exceto em modais (máx `4px`) e bulk-action-bar (`8px`, exceção justificada pela flutuação).

---

## Componentes

### Topbar (`#header`)
- Fundo: `--ink` (`#1A1208`)
- Height: `56px`; sticky no topo; `z-index: 90`
- Logo: título uppercase peso 900 em branco + subtitle em `--accent` letra-espaçado
- Tabs centralizadas com `border-bottom: 3px solid` no estado ativo (`--accent`)
- Botão primário "Novo": `--accent`, uppercase, `border-radius: 4px`

### Tabs de navegação
- Cor inativa: `rgba(255,255,255,.45)`
- Cor hover: `rgba(255,255,255,.8)`
- Cor ativa: `#fff` + borda inferior `--accent`
- Badge de contagem: fundo `--accent`, `border-radius: 2px`

### Cards de item (visual grid `.icv`)
- Borda: `1.5px solid --border`; fundo `--surface`
- Hover: `border-color: --ink` + `translateY(-3px)` + sombra suave
- Selecionado: borda `--accent` + `box-shadow: 0 0 0 2px rgba(193,96,59,.25)`
- Em uso (checked): borda `#4E7A1A` + `box-shadow` verde
- Aposentado: `opacity: .45` + `filter: grayscale(.6)`

### Pills / filtros
- Inativo: borda `--border`, texto `--text3`, fundo transparente
- Hover: borda `--ink`, texto `--ink`
- Ativo: fundo `--ink`, texto `#fff`
- `border-radius: 2px`; uppercase; peso 800

### Botões

| Classe | Aparência | Uso |
|--------|-----------|-----|
| `.btn-p` | Fundo `--ink`, texto `#fff`; hover → `--accent` | Primário (ex: "Analisar") |
| `.btn-analyze` | Igual ao primário, largura 100% | CTA de seção |
| `.btn-clear` | Borda `--border`, texto `--text3`; hover → borda `--ink` | Secundário / limpar |
| `.topbar-new-btn` | Fundo `--accent`; hover `#A04E2E` | Ação rápida no header |
| `.bulk-btn-add` | Fundo `--accent` | Bulk action positiva |
| `.bulk-btn-retire` | `rgba(255,255,255,.12)` + borda branca | Bulk action neutra |

### Badges inline

| Classe | Cor fundo | Cor texto | Uso |
|--------|-----------|-----------|-----|
| `.bc` | `rgba(201,162,39,.15)` + borda âmbar | `#7A5E00` | Premium / favorito |
| `.bo` | `rgba(80,100,200,.08)` + borda azul | `#5060A8` | Off-season |
| `.icv-badge-in` | `rgba(26,18,8,.72)` | `#fff` | "Em uso" sobre thumbnail |
| `.icv-badge-off` | `rgba(80,100,200,.15)` + borda | `#5060A8` | "Off" sobre thumbnail |
| `.icv-star` | `rgba(201,162,39,.92)` | `#fff` | Favorito / star sobre thumbnail |

### Toast
- Fundo `--ink`, texto `#fff`
- `border-radius: 2px`; uppercase; peso 700
- Posição: `fixed`, `bottom: 80px`, centralizado horizontalmente
- Animação: `translateY` + `opacity`

### Page headings
- `font-size: 2rem`, peso 900, uppercase, `letter-spacing: -1px`
- Separado por `border-bottom: 2px solid --ink`
- Subtexto: `0.82rem`, cor `--text3`, `line-height: 1.7`

### Wizard (`.wz-`)
- Navegação: fundo `--ink`, pílulas de step com `border-radius: 2px`
- Step ativo: fundo `--accent`
- Header de seção: fundo `--ink`, `border-radius: 4px 4px 0 0`
- Body de seção: fundo `--surface2`, borda `--border`

### Modais
- Overlay: `rgba(0,0,0,.5)`
- Container: `--surface`, `border-radius: 4px`, borda `--border`
- Max-width: `500px`

### Score / gauge
- Número: `2.4rem`, peso 900, `letter-spacing: -2px`
- Label abaixo: `0.62rem`, uppercase, `letter-spacing: 2.5px`
- Tips: fundo semitransparente com borda esquerda colorida (verde/laranja/ouro)

### Padrões de interação
- Hover de cards: `translateY(-3px)` ou `translateX(2px)` dependendo do layout
- Transições: `0.12s – 0.25s ease` ou `cubic-bezier(.4,0,.2,1)` para transforms
- Animação de entrada de página: `fadeIn + translateY(8px→0)` em `0.2s`
- Bulk action bar: `fixed bottom`, entra/sai com `opacity + translateY`

---

## Scrollbar customizada
- Largura: `4px`
- Track: `--bg2`
- Thumb: `--accent`, `border-radius: 2px`

---

## Como isto vira código

- Todos os tokens definidos como **CSS custom properties** em `:root` no `index.html`.
- Sem bibliotecas de UI — estilo 100% manual em CSS vanilla.
- Nenhum hex solto no markup: sempre referenciar via `var(--token)`.
- Ao criar nova peça visual, registrar aqui antes de implementar.
