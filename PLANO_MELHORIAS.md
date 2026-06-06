# Plano de Melhorias — Outono Quente (Closet & Combinador)

> Checklist priorizado. Marque `[x]` conforme concluir.
> Estado atual: app de página única (`index.html`, ~3.730 linhas), Firebase Auth + Firestore funcionando, deploy no Netlify.

---

## 🔴 Prioridade Alta — Rápido e alto impacto

### Limpeza técnica
- [ ] Remover os 14 `console.log` de debug deixados durante o desenvolvimento (linhas ~59, 61, 69, 71, 3463, 3469, 3472, 3477 etc.). Manter apenas os `console.error` legítimos de tratamento de erro.
- [ ] Remover o `Cartela_Cores.zip` do diretório (backup manual — usar Git para versionamento).
- [ ] Inicializar um repositório Git no projeto (hoje não é repo). Permite histórico, desfazer e deploy contínuo.

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
- [ ] **Sugestão do dia**: integrar clima (API de previsão) para sugerir looks adequados à temperatura.
- [ ] **Modo viagem**: montar mala/cápsula a partir de N peças que combinam entre si.
- [ ] **Detalhe do look**: abrir um look em tela cheia com foto grande + peças + score.

### UX / Acessibilidade
- [ ] Estados de carregamento (skeleton) ao sincronizar com o Firestore, em vez de só toast.
- [ ] Revisar contraste de cores e navegação por teclado.
- [ ] Adicionar `alt` descritivo em todas as imagens.
- [x] Confirmar antes de excluir um look (evitar perda acidental).

### Organização de código
- [ ] Separar o arquivo único em `index.html` + `styles.css` + `app.js` (manutenção mais fácil que 3.730 linhas).
- [ ] Extrair o array de `items` (52 peças) para um `data.json` separado.
- [ ] Remover o `server.js` se o fluxo é só Live Server + Netlify (ou documentar para que serve).

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
