# ASSENT v1.2 — Estrutura Reestruturada

## 📁 Estrutura de pastas

```
assent/
├── index.html               ← shell HTML (sidebar + topbar + áreas de páginas)
├── vite.config.js           ← config do Vite
├── package.json
└── src/
    ├── main.js              ← ponto de entrada: utils, state, save, render, auth
    ├── firebase.js          ← config Firebase + auth + helpers (módulo ES)
    ├── state.js             ← variáveis globais (referência — já integradas no main.js)
    ├── config.js            ← applyConfig(), loadConfigUI(), saveConfig()
    ├── ia.js                ← assistente IA + chat flutuante
    ├── backup.js            ← exportarDados(), restaurarBackup()
    ├── auth.js              ← loginComEmail, loginComGoogle, fazerLogout
    ├── styles/
    │   ├── themes.css       ← variáveis CSS (:root dark/light)
    │   ├── layout.css       ← sidebar, topbar, cards, forms, tabelas, modais
    │   ├── responsive.css   ← mobile/PWA + tela de login
    │   └── main.css         ← CSS completo (backup/referência)
    ├── pages/
    │   ├── dashboard.js
    │   ├── clientes.js
    │   ├── produtos.js
    │   ├── servicos.js
    │   ├── vendas.js
    │   ├── caixa.js
    │   ├── despesas.js
    │   ├── fornecedores.js
    │   ├── fiado.js
    │   ├── agenda.js
    │   ├── relatorios.js
    │   └── vendedores.js
    └── modals/
        ├── venda-detalhe.js
        ├── venda-editar.js
        ├── recibo.js
        ├── cliente-hist.js
        └── produto-editar.js
```

## 🚀 Como rodar

```bash
npm install
npm run dev
```

## ⚙️ O que ainda precisa ser feito

### 1. Conectar os módulos JS no index.html

Por enquanto o `src/main.js` contém tudo em um arquivo só (utils + state + init).
Para Vite com módulos, o ideal é:

```html
<!-- index.html, antes do </body> -->
<script type="module" src="src/firebase.js"></script>
<script src="src/main.js" defer></script>
```

Ou mover para imports ES dentro do main.js:
```js
import { renderDashboard } from './pages/dashboard.js'
import { renderClientes }  from './pages/clientes.js'
// etc.
```

### 2. Adicionar export/import nos módulos de páginas

Cada arquivo em `src/pages/` deve exportar suas funções:
```js
// pages/dashboard.js
export function renderDashboard() { ... }
export function setDashPeriod()   { ... }
```

E o `main.js` importa:
```js
import { renderDashboard } from './pages/dashboard.js'
```

### 3. Corrigir responsive.css

- Testar breakpoints em 375px, 768px, 1024px
- Corrigir sidebar mobile (drawer) 
- Garantir que topbar não quebre no mobile

### 4. Login screen no HTML

O `index.html` atual tem `<div id="loginScreen">` vazio.
O HTML do login original está entre as linhas 1249–1319 do arquivo original.
Precisa ser colado dentro desse div.

### 5. Modais HTML

Os modais (detalhe venda, editar venda, recibo, etc.) precisam ser
copiados do original (linhas 2354–2669) para dentro do `index.html`.

## 📋 Estratégia de migração recomendada

**Fase 1 (rápida):** Usar como arquivo único ainda  
Copiar todo o JS original para um `src/app.js` e carregar via `<script src="src/app.js">`.
O CSS já está separado — isso já resolve o maior problema de legibilidade.

**Fase 2 (módulos):** Adicionar `export`/`import` gradualmente, começando pelas páginas
mais isoladas (dashboard, clientes, produtos).

**Fase 3 (Vite build):** Com todos os módulos funcionando, ativar o build do Vite
para produção com tree-shaking e minificação.
