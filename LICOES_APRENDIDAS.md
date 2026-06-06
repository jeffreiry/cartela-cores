# Lições Aprendidas — Integração Firebase

## Problema: Dados compartilhados entre usuários

### Causa raiz
`loadData()` chamava `saveData()` internamente. Como `saveData()` escrevia no Firestore usando `window._currentUser`, o Firestore do novo usuário era sobrescrito com dados do localStorage (que pertenciam ao usuário anterior).

### Solução
- `loadData()` nunca deve chamar `saveData()`
- Looks **não devem ser salvos no localStorage** — apenas no Firestore
- localStorage só é usado para `items` (catálogo de roupas, igual para todos)

---

## Problema: Render com dados do usuário anterior

### Causa raiz
O app renderizava antes do Firestore responder, exibindo `looksHistory` ainda em memória do usuário anterior.

### Solução
- Zerar `window.looksHistory` e `looksHistory` **imediatamente** ao detectar mudança de usuário
- Mover todo o render para **dentro do `.then()`** do Firestore
- Nunca renderizar antes de confirmar quais dados pertencem ao usuário atual

---

## Problema: Save contaminando Firestore de outro usuário

### Causa raiz
`onAuthStateChanged` dispara duas vezes durante troca de usuário (logout + login). Na primeira disparada, `window._currentUser` ainda apontava para o usuário anterior, causando save no Firestore errado.

### Solução
- Flag `window._authReady = false` bloqueia saves durante carregamento
- `_authReady = true` só é definido **após** o Firestore carregar (dentro do `.then()`)
- Ao fazer logout, `_authReady = false` imediatamente

---

## Problema: Regras do Firestore bloqueando tudo

### Causa raiz
Ao criar o Firestore no modo produção, a regra padrão é `allow read, write: if false`.

### Solução
Usar a regra:
```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{userId} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
  }
}
```

---

## Problema: Login Google com erro Cross-Origin-Opener-Policy

### Causa raiz
`signInWithPopup` no Chrome gera warnings de `Cross-Origin-Opener-Policy` que podem interromper o fluxo assíncrono com `await`.

### Solução
Trocar `await` por `.then()` no carregamento do Firestore — o erro do popup não interrompe o `.then()`.

---

## Arquitetura final correta

```
Login
  └── onAuthStateChanged(user)
        ├── _authReady = false
        ├── looksHistory = []
        ├── Mostrar app
        └── _fbLoadLooks(user.uid).then(cloudLooks => {
              looksHistory = cloudLooks
              _authReady = true
              renderSuggestionsHistory()
              renderLooksHistory()
            })

Logout
  └── onAuthStateChanged(null)
        ├── _authReady = false
        ├── _currentUser = null
        ├── looksHistory = []
        └── Mostrar tela de login

Save
  └── saveData()
        ├── localStorage → apenas items (catálogo)
        └── Firestore → apenas se _currentUser && _authReady
```

---

---

## Problema: Conteúdo desalinhado em relação ao topbar (telas largas)

### Causa raiz
Havia uma regra de media query herdada da sidebar antiga:
```css
@media(min-width:769px){
  .main{margin-left:0;padding-top:36px}
}
```
O `margin-left:0` sobrescrevia o `margin:0 auto` do `.main`, fixando a margem esquerda em 0 e jogando todo o espaço restante para a direita. Em telas de 1920px isso gerava 0px de margem esquerda e 520px de margem direita.

### Solução
Remover o `margin-left:0` do media query. O `.main` e o `.hi` (topbar) devem ter apenas `max-width:1400px; margin:0 auto` para alinhar corretamente.

### Como diagnosticar
No DevTools (F12), selecionar `.hi` e `.main` e comparar o box model — margens laterais devem ser iguais nos dois.

---

## Decisão: Layout dos slots no Combinador

### Problema
Slots com altura fixa em pixels (`grid-template-rows: 280px ...`) ignoravam a proporção das imagens e ficavam horizontais demais.

### Solução
- Slots 1 e 2 (Topo e Camiseta): `aspect-ratio:1/1` aplicado no `.combo-zone-slot` com `grid-template-rows: auto` na primeira linha
- Colunas independentes com **flexbox** (`display:flex; flex-direction:column`) em vez de grid compartilhado — evita que a altura dos acessórios interfira na altura dos slots principais
- Slot 3 (Inferior): mesmo `aspect-ratio:1/1` que slots 1 e 2
- Slot 7 (Calçado): `aspect-ratio:2/1` para altura equivalente a 50% dos demais

---

## Decisão: Exibição de imagens no Catálogo e Combinador

### Problema
`object-fit:cover` cortava as peças. `object-fit:contain` com fundo beige (var(--bg2)) deixava espaços vazios visíveis ao redor de fotos com fundo branco.

### Solução
- `object-fit:contain` para mostrar a peça completa sem corte
- `background:#fff` no container da imagem — elimina o contraste do espaço vazio ao redor de fotos com fundo branco
- `aspect-ratio:3/4` nos cards do catálogo (proporção retrato, igual aos slots do combinador)
- Remover `transform:scale(1.06)` no hover — com `contain` o zoom fica estranho

---

## Dica: Depuração de usuários no Firestore

O console do Firebase não consegue listar documentos quando a regra exige `request.auth.uid == userId`. Para inspecionar ou deletar dados durante desenvolvimento, use temporariamente `allow read, write: if true` e restaure depois.
