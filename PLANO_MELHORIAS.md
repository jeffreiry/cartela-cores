# Plano de Melhorias — Outono Quente (Closet & Combinador)

> Checklist priorizado. Marque `[x]` conforme concluir.
> Estado atual: app de página única (`index.html`, ~3.730 linhas), Firebase Auth + Firestore funcionando, deploy automático na Vercel via GitHub (`jeffreiry/cartela-cores`).

---

## 🔴 Prioridade Alta — Rápido e alto impacto

### Limpeza técnica
- [ ] Remover os 14 `console.log` de debug deixados durante o desenvolvimento (linhas ~59, 61, 69, 71, 3463, 3469, 3472, 3477 etc.). Manter apenas os `console.error` legítimos de tratamento de erro.
- [x] Remover o `Cartela_Cores.zip` do diretório (backup manual — usar Git para versionamento).
- [x] Inicializar um repositório Git no projeto (hoje não é repo). Permite histórico, desfazer e deploy contínuo.

### SEO / Compartilhamento
- [ ] Adicionar `<meta name="description">` no `<head>`.
- [ ] Adicionar tags Open Graph (`og:title`, `og:description`, `og:image`) para o link ficar bonito quando compartilhado.
- [ ] Adicionar um `favicon` (hoje dá 404 no console).

### PWA — instalar no celular e usar offline
- [ ] Criar `manifest.json` (nome, ícones, cor de tema, `display: standalone`).
- [ ] Adicionar um Service Worker básico para cache do app shell (funciona offline / abre rápido).
- [ ] Permitir "Adicionar à tela inicial" — uso natural: consultar o closet no celular enquanto se arruma.

---

## 🟡 Prioridade Média — Robustez e experiência

### Imagens
- [ ] Migrar as fotos de `raw.githubusercontent.com` para um host adequado (Firebase Storage ou CDN). O raw do GitHub não é CDN, pode ser lento e tem rate limit.
- [ ] Padronizar formato das imagens (hoje há `.jpg`, `.webp`, `.jfif`, `.avif` misturados) — converter tudo para `.webp`.
- [ ] Adicionar `loading="lazy"` nas imagens do catálogo (carregar só o que aparece na tela).
- [ ] Definir uma imagem placeholder para quando a foto falhar (hoje só some com `onerror`).

### Backup e dados
- [ ] Botão "Exportar dados" (baixar looks + sugestões como JSON).
- [ ] Botão "Importar dados" (restaurar de um JSON).
- [ ] Considerar mover o **catálogo** (`items`) para o Firestore por usuário, permitindo editar/adicionar peças sem mexer no código. *(Adiado conscientemente — ver [LICOES_APRENDIDAS.md](LICOES_APRENDIDAS.md))*

### Upload de peças no app
- [ ] Permitir cadastrar nova peça com upload de foto direto (Firebase Storage), em vez de subir manualmente no GitHub e colar URL.

---

## 🟢 Prioridade Baixa — Polimento e features

### Funcionalidades de domínio (closet)
- [ ] **Registro de uso**: marcar quando um look foi usado e em que data — evita repetir roupa e mostra peças esquecidas.
- [ ] **Estatísticas**: peça mais/menos usada, cores favoritas, "custo por uso".
- [x] **Sugestão do dia**: integrar clima (API de previsão) para sugerir looks adequados à temperatura.
- [ ] **Modo viagem**: montar mala/cápsula a partir de N peças que combinam entre si.
- [x] **Detalhe do look**: abrir um look em tela cheia com foto grande + peças + score.

### UX / Acessibilidade
- [ ] Estados de carregamento (skeleton) ao sincronizar com o Firestore, em vez de só toast.
- [ ] Revisar contraste de cores e navegação por teclado.
- [ ] Adicionar `alt` descritivo em todas as imagens.
- [x] Confirmar antes de excluir um look (evitar perda acidental).

### Organização de código
- [ ] Separar o arquivo único em `index.html` + `styles.css` + `app.js` (manutenção mais fácil que 3.730 linhas).
- [ ] Extrair o array de `items` (52 peças) para um `data.json` separado.
- [x] Remover o `server.js` se o fluxo é só Live Server + Netlify (ou documentar para que serve).

---

## 📌 Notas de arquitetura
- Auth e sincronização por usuário estão corretos e documentados em [LICOES_APRENDIDAS.md](LICOES_APRENDIDAS.md).
- Catálogo é **compartilhado** (localStorage, igual para todos) por decisão consciente — não é bug.
- Looks/Sugestões são **isolados por usuário** (Firestore por `uid`).

---

## Sugestão de ordem de execução
1. Limpeza técnica + Git (base limpa para o resto)
2. PWA (maior ganho de experiência para o uso real no celular)
3. Imagens (performance perceptível)
4. Backup/Export (segurança dos dados)
5. Features de domínio conforme interesse

---

# 🎨 Melhorias de UX Design

> Foco em como o app *comunica* e *guia*, não só no que ele faz.

## 🔴 Alto impacto — específicas deste app

### O Combinador (tela principal)
- [x] **Layout de "manequim" vertical**: empilhar as peças na anatomia do corpo (topo → meio → inferior → calçado) em vez de lado a lado. Um look tem ordem visual — o empilhamento vertical é lido instantaneamente como "uma roupa montada", não "uma fileira de itens".
- [x] **Explicar o score em tempo real**: ao lado do número de harmonia, mostrar *qual peça* está ajudando ou atrapalhando (ex: "Camiseta Brasil puxa o score pra baixo — fora da cartela"). Transforma o score de caixa-preta em consultor de estilo.
- [x] **Feedback claro quando uma regra bloqueia**: quando não dá para adicionar (ex: 2ª camiseta), mostrar o motivo no momento do clique, não só desabilitar o item.
- [ ] **Drag-and-drop** para montar o look (além do clique), com áreas de "soltar" por categoria.

### Onboarding e estados vazios
- [ ] **Primeiro acesso guiado**: ao logar pela 1ª vez, tudo está vazio. Trocar os "Nenhum look ainda" por um call-to-action que leva à ação (ex: "Monte seu primeiro look no Combinador →").
- [ ] **Estado vazio do Combinador** com dica visual de como começar (não só "Selecione peças no closet").

### Clareza conceitual
- [ ] **Diferenciar "Sugestão" de "Look Usado"** visualmente (ícone/cor/etiqueta consistentes). A distinção é sutil e pode confundir — deixar explícito o que cada um significa.

## 🟡 Médio — consistência e legibilidade

### Tipografia
- [x] Revisar as **36 ocorrências de fonte < 0.6rem** (rótulos com `.46rem`, `.48rem`, `.58rem`). Texto tão pequeno é difícil de ler, especialmente no mobile. Definir um tamanho mínimo (~0.65rem) e uma escala tipográfica consistente.

### Consistência visual das fotos
- [ ] **Unificar o fundo das imagens** do catálogo (hoje há fotos com fundo branco e outras com fundo colorido/produto — visual "remendado"). Aplicar um tratamento de fundo uniforme ou padronizar no upload.
- [ ] Garantir **enquadramento consistente** entre peças (algumas preenchem o card, outras ficam pequenas).

### Ícones
- [ ] Substituir **emojis usados como ícones de UI** por um conjunto de ícones (SVG/lib). Emojis renderizam diferente em cada SO/navegador e quebram a consistência visual.

### Feedback e segurança
- [ ] **Skeletons de carregamento** ao sincronizar com Firestore (em vez de só toast) — a tela não "pisca" vazia antes de preencher.
- [x] **Confirmar antes de excluir** um look/sugestão (evitar perda acidental).
- [ ] **Micro-interações** ao adicionar/remover peça (transição suave reforça a ação).

## 🟢 Refinamento

### Acessibilidade (crítico num app de cor)
- [x] **Não sinalizar só por cor**: o score usa verde/amarelo/vermelho. Adicionar rótulo textual + ícone (ex: ✓ Excelente / ⚠ Revisar) para daltônicos.
- [ ] Revisar **contraste** do texto secundário (`--text3 #9C8270`, `--text4 #C4AE98`) sobre o fundo claro — pode reprovar no WCAG AA.

### Mobile
- [ ] **Repensar o Combinador no mobile**: as duas colunas (closet + canvas) são apertadas em telas pequenas. Considerar bottom-sheet para o closet ou abas internas.

### Home / Dashboard
- [x] Fortalecer a tela inicial como um verdadeiro **"hub"**: look do dia, atalho para o último look, estatística rápida — em vez de só ponto de entrada.

### Visão de look
- [x] **Detalhe do look em tela cheia** (foto grande + peças + score + observações), acionável a partir dos cards.

### Tema
- [ ] Avaliar um **modo escuro** opcional (a paleta atual é quente/clara).

---

# 🎨 Ajustes Finos Paleta

> Melhorias específicas na aba Cartela e no sistema de cores, identificadas após alinhamento com o `guia-cartela.md`.

## 🔴 Alta prioridade

- [ ] **Cores Premium interativas**: ao clicar em uma cor premium (Jambu, Fogueira etc.), filtrar o catálogo mostrando peças com cores próximas — conecta a teoria com o guarda-roupa real.
- [ ] **Score das Cores Premium no PAL**: Azul Pantanal, Figo em Calda, Jambu, Sol do Jalapão, Fogueira e Chá Mate não estão no array `PAL` — adicionar com scores adequados para que o sistema de harmonia as reconheça.

## 🟡 Média prioridade

- [ ] **Combinações clicáveis**: nas seções de combinações da aba Cartela, clicar em uma combinação (ex: "Marinho + bege") filtra o catálogo mostrando peças nessas cores.
- [ ] **Coringa no Combinador**: destacar visualmente no canvas quando um slot está preenchido com uma peça coringa (ex: badge ⭐ sutil).
- [ ] **Sugestão baseada em celebridade**: no dashboard ou combinador, modo "inspire-se em Taron Egerton" que prioriza peças e combinações do perfil daquele ator.

## 🟢 Refinamento

- [ ] **Harmonia explicada na Cartela**: abaixo de cada combinação sugerida, um exemplo prático com peças reais do guarda-roupa (ex: "Marinho + bege → Calça Sarja Marinho + Camiseta Lisa Bege").
- [ ] **Cartela vs. Guarda-roupa**: seção mostrando quais das 9 cores-chave do guia você já tem representadas e quais estão faltando.
- [ ] **Revisão periódica de coringas**: alerta quando uma peça coringa não aparece em nenhum look nos últimos 30 dias — "peça esquecida".

---

# 🔬 Sistema de Scoring — Melhorias

> Baseado na fórmula CIE ΔE documentada em `guia-cartela.md`. Hoje os scores são manuais/hardcoded no array `PAL`. O objetivo é torná-los calculados automaticamente.

## 🔴 Alta prioridade

- [ ] **Implementar cálculo ΔE no app**: substituir os scores manuais do array `PAL` pela fórmula `HEX → Lab → ΔE → score`. Isso permite que qualquer nova cor inserida receba score automaticamente sem editar o código.
- [ ] **Adicionar Cores Premium à paleta âncora**: Azul Pantanal (`#153c52`), Figo em Calda (`#4d6046`), Jambu (`#ed6707`), Sol do Jalapão (`#f26b3a`), Fogueira (`#8c2f0d`), Chá Mate (`#5c2d11`) não estão no `PAL` — o sistema de harmonia não as reconhece. Adicionar com scores definidos.
- [ ] **Penalidade de temperatura**: cores frias (hue > 150°) são superestimadas em ~15–23%. Aplicar `penalidade = (hue - 150) / 15` pontos para Branco Puro, Cinza Frio, Rosa Frio, Azuis frios.

## 🟡 Média prioridade

- [ ] **Badges 4 níveis**: hoje o app usa só "NA CARTELA" / "FORA". Expandir para os 4 níveis da fórmula: ✅ Na paleta (90–100%) / ✅ Aceita (70–89%) / ⚠️ Pontual (40–69%) / ⚠️ Fora da paleta (0–39%).
- [ ] **Score dinâmico ao cadastrar peça**: ao informar o hex de uma nova peça no modal de cadastro, calcular e exibir o score em tempo real — feedback imediato antes de salvar.
- [ ] **Histórico de score por peça**: registrar quando o score muda (ex: paleta âncora atualizada) para mostrar evolução.

## 🟢 Refinamento

- [ ] **Extração automática de cor dominante**: ao subir foto de uma peça, usar ColorThief ou similar para sugerir o hex dominante automaticamente — elimina seleção manual de cor.
- [ ] **Validação da fórmula contra catálogo atual**: rodar o cálculo ΔE nas 52 peças e comparar com os scores manuais — identificar quais estão mais desalinhados.

---

# 🖌️ Identidade Visual & Modernização

> Análise crítica como Product Designer Sênior, focada em tornar a identidade visual
> mais moderna e coesa no **desktop** e no **mobile**.
> Direção escolhida: **editorial afiada** (refinar a pegada Nike/Adidas — cantos
> nítidos, bold, uppercase com propósito — não trocá-la por uma estética soft).
> Referência de tokens: [docs/design-system.md](docs/design-system.md).

## Diagnóstico

O app tem uma boa intenção de identidade ("Nike/Adidas: bold, editorial, sharp"),
mas ela se **diluiu na prática**. Três tensões enfraquecem a percepção de qualidade:

1. **Drift de border-radius.** O design system declara cantos afiados (`--r: 4px`,
   pills/botões em `2px`), mas as telas mais novas vazaram para `8px` (cards do
   dashboard, modais de detalhe, `.lotd-card`, `combo sheets`) e até `12px`
   (filtro mobile). O resultado é um app que não é nem afiado nem suave — lê-se
   como "remendado", o oposto de uma identidade forte.
2. **Tudo grita.** Uppercase + `letter-spacing` está em *todo* rótulo, botão,
   badge e título. Quando tudo é maiúsculo e espaçado, nada tem hierarquia — e o
   efeito é "painel administrativo de 2018", não "marca de moda 2025".
3. **Fundações frágeis.** Base de `14px` (pequena), fonte única, dezenas de hex
   cravados no markup fora dos tokens, e durações de transição ad-hoc
   (`.12 / .15 / .2 / .25 / .3 / .35 / .5 / .7 / .8 / .9s`). Sem fundação
   consistente, qualquer polish fica superficial.

A boa notícia: a base é sólida (tokens em `:root`, micro-interações já existem,
bottom-nav mobile correto). É **consolidação**, não reconstrução.

## 🔴 Fundações — decidir a identidade (desktop + mobile)

- [ ] **Unificar a linguagem de raio em UMA lógica afiada.** Definir só dois
  tokens: `--r: 4px` (cards/superfícies) e `--r-sm: 2px` (botões/pills/badges).
  Eliminar todo `8px` e `12px` solto (dashboard cards ~L1259/1271, `.lotd-card`
  L1214, modais de detalhe L1028/1076, `combo sheets`, filtro mobile L975).
  *Por quê:* coerência de raio é o sinal nº 1 de um sistema maduro.
- [ ] **Subir a base tipográfica para 16px** (`html{font-size}` L167) e revisar a
  escala em `rem`. Hoje `0.65rem` = 9px e há rótulos em `0.46–0.6rem` (ilegíveis,
  pior no mobile). *Cross-ref:* item de "fonte < 0.6rem" na seção UX — tratar
  como **escala tipográfica única**, não correções pontuais.
- [ ] **Reduzir o uppercase a eyebrows e labels.** Manter caps em: tags de seção,
  rótulos de campo, badges curtos. Remover de: nomes de peça, títulos de card,
  textos de botão longos. Hierarquia deve vir de **peso + tamanho**, não de caps.
- [ ] **Adotar uma fonte de display** para títulos (H2/page-heading/hero),
  mantendo Plus Jakarta Sans no corpo. Um par display+corpo é o atalho mais rápido
  para sofisticação editorial — num app de moda, um display com caráter (ou um
  serif para headers) eleva muito a percepção. *(Portfolio usa Manrope+Inter como
  referência de pareamento.)*
- [ ] **Tokenizar TODAS as cores cravadas no markup.** Hoje há `#7B3F2B`
  (`.lotd-card` L1214), `#A04E2E` (hover de botões), `#4E7A1A`, `#5060A8`,
  `#7A5E00`, `#3A5A0A`, `#9A3820` etc. espalhados. Mover para `:root` como tokens
  semânticos (`--status-in`, `--status-off`, `--accent-hover`…). *Pré-requisito
  para dark mode e para consistência real.*
- [ ] **Criar uma escala de espaçamento (4 / 8pt).** Substituir os gaps ad-hoc
  (6/8/10/12/14/16/20/24/32) por uma régua fixa (`4 8 12 16 24 32 48 64`). Ritmo
  consistente é o que separa "alinhado" de "quase alinhado".
- [ ] **Tokens de movimento.** Padronizar 2–3 durações (`--t-fast:120ms`,
  `--t:200ms`, `--t-slow:320ms`) e um easing (`cubic-bezier(.4,0,.2,1)`). Aplicar
  em todas as `transition`. *Movimento consistente é identidade percebida.*

## 🟡 Componentes & hierarquia

- [ ] **Sistema de botões com 4 papéis claros.** Hoje quase todo botão é o mesmo
  bloco `--ink` → `--accent` no hover (`.btn-p`, `.btn-analyze`, `.lk-btn`,
  `.lh-use-btn`, `.side-cta`, `.cp`…). Definir: **primário** (ink), **secundário**
  (outline), **terciário/ghost** (texto), **destrutivo** (vermelho). Sem
  hierarquia de botão, a tela não diz "o que é a ação principal aqui".
- [ ] **Sistema de elevação (sombras em camadas).** As sombras atuais são chapadas
  e monocromáticas. Definir 3 níveis (`--elev-1/2/3`) com sombras suaves e
  levemente quentes — usar elevação (não só borda `1.5px`) para separar superfícies.
- [ ] **Diferenciar cards por hierarquia.** Looks, canvas do Combinador e Look do
  Dia são conteúdo primário; stats e meta são secundários. Hoje quase tudo é
  "surface + borda 1.5px + mesmo raio". Variar tratamento (peso de borda, fundo,
  elevação) para criar foco.
- [ ] **Substituir o emoji-logo (🍂) por um logomark desenhado.** É a assinatura da
  marca (topbar L184, login L1593, side-brand) e hoje renderiza diferente em cada
  SO. Um símbolo SVG simples já muda o patamar de seriedade. *Cross-ref:* item de
  "emojis como ícones" — tratar logo + ícones de UI no mesmo movimento (set SVG).
- [ ] **Revisar contraste dos textos terciários** (`--text3 #9C8270`,
  `--text4 #C4AE98`) sobre fundo claro — provavelmente reprovam WCAG AA.
  Identidade moderna inclui acessibilidade. *Cross-ref:* item de contraste na UX.

## 🟢 Telas-chave — aplicar a identidade

- [ ] **Login como momento de marca (desktop + mobile).** É a primeira impressão e
  hoje é um card centralizado com emoji e estilos inline (L1591–1605). Merece
  tratamento editorial: logomark, tipografia de display, talvez um split-screen no
  desktop (imagem/cor à esquerda, form à direita) e full-bleed no mobile.
- [ ] **Dashboard mais editorial.** O bento (`6fr 4fr`, L1211) é um bom começo.
  Levar adiante: o "Look do Dia" como herói com tipografia de display grande, e os
  stats com mais respiro e hierarquia (não como caixas uniformes).
- [ ] **Combinador no mobile.** As 3 colunas do desktop (closet 240px + canvas +
  sugestões 240px, L1313–1315) colapsam para um closet de 220px de altura
  empilhado (L1560) — apertado. Repensar como **bottom-sheet** para o closet ou
  abas internas, deixando o canvas do look protagonista na tela pequena.
  *Cross-ref:* item "Repensar o Combinador no mobile" na seção UX.
- [ ] **Dark mode como consequência da tokenização.** Depois que todas as cores
  forem tokens semânticos, um tema escuro vira troca de `:root` — e combina com a
  pegada editorial (modo "noturno" quente). Fazer só após a tokenização.

## Ordem sugerida (identidade)
1. Fundações primeiro (raio, tipografia, tokens de cor/espaço/movimento) — destravam tudo.
2. Componentes (botões, elevação, logomark/ícones).
3. Telas-chave (login, dashboard, combinador mobile).
4. Dark mode por último, como colheita da tokenização.
