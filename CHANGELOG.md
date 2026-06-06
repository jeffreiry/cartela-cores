# Changelog — Outono Quente

> Registro cronológico de alterações. Atualizado a cada sessão de trabalho.

---

## [Não publicado] — Em desenvolvimento local

### Estrutura — Topbar
- **Sidebar removida** — substituída por topbar horizontal permanente (desktop + mobile)
- **Topbar**: logo à esquerda, abas centralizadas, usuário + "Nova Peça" à direita
- **Mobile**: abas movidas para bottom nav (comportamento anterior mantido)
- Todas as 7 abas disponíveis no topbar: Início, Catálogo, Combinador, Sugestões, Looks, Compras, Cartela
- Filter panel reposicionado para abrir abaixo do topbar

### Combinador — Montador Inteligente
- **Zonas sempre visíveis**: canvas mostra as 5 zonas do corpo (Topo, Camiseta, Acessórios, Inferior, Calçado) mesmo vazias
- **Slots vazios com sugestão**: cada zona vazia mostra a melhor peça sugerida (dimmed) com botão "+ Adicionar"
- **Trocar peça por zona**: botão "↺" em cada slot abre picker com alternativas ordenadas por score
- **Picker de alternativas**: bottom sheet com todas as peças da zona, ordenadas por harmonia
- **Score em cada slot**: badge "✓ 87%" (verde) ou "↓ Fora" (vermelho) em cada peça
- **Score explicado**: card flutuante mostra qual peça ancora ou puxa o score pra baixo

### Catálogo — Multi-seleção e Aposentar
- **Checkboxes** nos cards do catálogo para seleção múltipla
- **Barra de ações flutuante** ao selecionar peças: "Adicionar ao Combinador" e "Aposentar"
- **Aposentar**: marca peças como inativas, ocultas por padrão — só visíveis via filtro "🗄 Aposentadas"
- **Reativar**: botão nos cards de peças aposentadas para voltar ao catálogo ativo
- Filtro **"Aposentadas"** adicionado nas pills do catálogo

### Looks / Sugestões
- Modal de **detalhe do look em tela cheia**: foto grande, peças com thumbnail, score de harmonia, observações e ações (Usar, Editar, Excluir)
- Cards de look agora exibem a **foto** quando disponível (preview inline)
- Clique em qualquer card abre o modal de detalhe

### Dashboard (Hub)
- Adicionado bloco de **stats rápidas** (looks usados, sugestões, peças na cartela) com clique direto na aba correspondente
- Adicionado card **"Último Look Salvo"** com foto (se tiver), peças em swatches de cor, data e botão "Usar →"
- Adicionada grade de **Acesso Rápido** com 4 atalhos: Combinador, Catálogo, Looks, Cartela

---

## 2026-06-03

### Acessibilidade
- Todas as fontes abaixo de `0.6rem` elevadas para `0.65rem` (36 ocorrências corrigidas)
- Score de harmonia agora exibe ícone + rótulo textual além da cor (✓ Excelente / ↑ Ótimo / – Regular / ↓ Revisar) — não depende apenas de cor para comunicar qualidade
- Adicionado botão 🗑 nos cards de look/sugestão com confirmação antes de excluir
- Motivo do bloqueio de regra exibido inline no card da peça (ex: "🚫 Já existe um Calçado no look") além do toast

### Firebase / Backend
- Integração completa com Firebase Authentication (email/senha + Google)
- Tela de login obrigatória — app bloqueado até autenticação
- Looks e sugestões isolados por usuário no Firestore (`users/{uid}`)
- Catálogo (`items`) permanece no localStorage (compartilhado, por decisão consciente)
- Corrigido bug crítico: `loadData()` chamava `saveData()` que sobrescrevia Firestore do usuário errado durante troca de sessão
- Flag `_authReady` bloqueia saves durante carregamento do Firestore — evita contaminação entre usuários
- Renders movidos para dentro do `.then()` do Firestore — garante dados corretos antes de exibir

### Looks / Sugestões
- Botão "Salvar Como" no Combinador com dropdown: "💡 Nova Sugestão" e "👕 Look Usado"
- Sugestões salvas na aba "Sugestões", looks usados na aba "Looks"
- Modal de edição de look: nome + link de foto com preview
- Função `deleteLook()` com confirmação

### Combinador
- Layout alterado de cascading/overlapping para linear lado a lado
- Ordem: Outerwear → Camiseta → Acessórios → Inferiores → Calçados
- Painel "💡 Próximas peças sugeridas" com expander/collapser
- Sistema de scoring de sugestões: harmonia (30%), estilos (25%), categoria (20%), coringa (15%), ocasião (10%)

### Catálogo
- `object-fit: contain` nas imagens para exibir peças sem corte
- `aspect-ratio: 5/4` nos cards (mais horizontal, melhor para calçados)
- Filtro "Selecionado" nas pills do catálogo

### Sidebar
- Colapsar/expandir sidebar (mostra apenas ícones quando colapsado)
- Estado persistido em localStorage (`sb-collapsed`)
- Botão de login/usuário na parte inferior da sidebar

### Fotos
- Todas as 52 peças com URLs de foto no GitHub (`jeffreiry/outono-quente-fotos`)
- Extensões variadas suportadas: `.jpg`, `.webp`, `.jfif`, `.avif`

---

## 2026-06-04

### Fotos
- Adicionada imagem `32-calca-chino-oliva.webp` ao repositório de fotos
- URL da Calça Chino Oliva (id:32) atualizada de `.jpg` para `.webp`
- URLs das Calças Chino Bege (id:33) e Caramelo (id:31) atualizadas para `.webp`

### Combinador — Redesign do Canvas
- Layout dos slots migrado de grid CSS para **duas colunas flex independentes** — evita que alturas da coluna direita (acessórios) interfiram nos slots principais
- 7 slots fixos: Topo, Camiseta, Inferior, Acessório 1/2/3, Calçado
- Proporção dos slots: `aspect-ratio:1/1` nos slots 1/2/3, `aspect-ratio:2/1` no calçado
- Empty states aparecem em todos os slots mesmo sem peças selecionadas
- Sugestões automáticas por slot removidas — slots mostram apenas peças selecionadas
- Botão **🎲 Sortear** gera look aleatório baseado nas regras de harmonia e no guia da cartela
- Análise de outfit movida para **modal** — abre ao clicar em "Analisar →"

### Combinador — Fluxo de Salvar
- Combinador salva apenas como **💡 Sugestão** (botão "Salvar Look" sempre pede nome)
- Botão **👕 Look Usado** movido para aba Sugestões — marca uma sugestão como usada
- **Editar** em Sugestões carrega as peças no combinador para retrabalho
- **Editar** em Looks abre modal simples de nome + data (não volta ao combinador)
- Ao editar e salvar no combinador, look existente é atualizado; look novo pede nome

### Catálogo — Imagens
- `aspect-ratio:3/4` nos cards (proporção retrato, igual aos slots do combinador)
- Fundo branco (`background:#fff`) no container da imagem — elimina contraste com fotos de fundo branco
- Efeito hover `scale(1.06)` removido

### Dashboard
- **Sugestão do dia** baseada em clima e horário: busca temperatura atual de Porto Alegre via Open-Meteo (sem API key)
- Critérios: temperatura (frio <16°C / ameno 16–23°C / quente ≥24°C), período do dia (manhã/tarde/noite), dia da semana, previsão de chuva
- **Widget de clima** adicionado na sidebar direita do dashboard (temperatura + dica de vestimenta)
- Gradiente de fundo do card de sugestão removido — cor sólida Marrom Avermelhado (`#7B3F2B`)
- Grid do bento ajustado de `8fr 4fr` para `6fr 4fr` (mais equilibrado)

### Alinhamento
- Corrigido desalinhamento entre topbar e conteúdo em telas largas (1920px+)
- Causa: `margin-left:0` residual da sidebar antiga sobrescrevia `margin:0 auto` no `.main`
- Padding lateral unificado em `24px` em `.hi` e `.main`

---

## 2026-06-06

### Infraestrutura
- `server.js` removido — servia apenas arquivos estáticos localmente; projeto é 100% estático
- Repositório Git inicializado e conectado ao GitHub (`jeffreiry/cartela-cores`)
- Deploy automático configurado via Vercel — todo `git push` gera deploy sem intervenção manual
- Migração de upload manual → Vercel (mesmo fluxo do Portfolio e Painel Saúde)

---

## 2026-06-05

### Aba Cartela — alinhamento com guia-cartela.md
- Conteúdo editorial revisado e alinhado 100% com o `guia-cartela.md`
- Removido: Guia de Maquiagem (sem base no guia)
- Adicionado: seção **Melhores Cores** com 9 cores-chave e swatches
- Adicionado: seção **Referências de Celebridades** (Taron Egerton, Garret Hedlund, Benedict Cumberbatch, Eddie Redmayne, Martin Freeman)
- Adicionado: seção **Regra das Peças Coringa** (4 critérios visuais)
- Adicionado: **Recado Final** do guia
- Adicionado: **Sugestões de Uso** (acessórios como ponte, metais próximos ao rosto)
- **Cores Premium** atualizadas com nomes editoriais do guia: Azul Pantanal, Figo em Calda, Jambu, Sol do Jalapão, Fogueira, Chá Mate
- Cores Premium agora são hardcoded — renderização dinâmica removida do `renderPalette()`

### Catálogo — Regra Coringa
- Regra formalizada no `guia-cartela.md`: score ≥ 85%, lisa/discreta, cor neutra, ≥ 3 ocasiões
- 10 itens corrigidos de `coringa:true` → `false` (ids 18, 19, 23, 27, 29, 36, 41, 47, 48, 51)
- Filtro **⭐ Coringa** adicionado nas pills do catálogo

### Correções técnicas
- `useLookHistory` e look detail agora buscam peças por `id` antes de fallback por `name` — evita ambiguidade com nomes duplicados
- `nid` (ID de novas peças) inicializado dinamicamente como `max(ids) + 1` — evita colisão de IDs

---

## 2026-06-02

### Funcionalidades iniciais
- Redesign completo da interface (design system Outono Quente)
- Catálogo com grade visual, filtros por categoria/ocasião/cor/estilo
- Combinador com regras de outfit (limites por categoria, pares bloqueados)
- Análise de harmonia de cores com score percentual
- Aba Looks com histórico de looks
- Aba Sugestões com looks gerados por estilo
- Aba Compras
- Aba Cartela de Cores (paleta + análise de harmonia)
- Dashboard com estatísticas e look do dia
- Deploy no Netlify

---

## Arquivos de referência
- [LICOES_APRENDIDAS.md](LICOES_APRENDIDAS.md) — problemas técnicos resolvidos e como
- [PLANO_MELHORIAS.md](PLANO_MELHORIAS.md) — backlog priorizado de melhorias
