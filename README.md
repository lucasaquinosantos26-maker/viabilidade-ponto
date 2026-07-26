# 🗺 Viabilidade de Ponto — Ótica Center

Aplicação web para análise de viabilidade de pontos comerciais para franquias Ótica Center. Inclui mapa interativo, contador de fluxo de pessoas, análise automatizada de pontos e **agente auto-evolutivo** para monitoramento e correções em tempo real.

---

## 📋 Índice

- [Visão Geral](#visão-geral)
- [Arquitetura de Acesso](#arquitetura-de-acesso)
- [Tecnologias](#tecnologias)
- [Estrutura do Projeto](#estrutura-do-projeto)
- [Instalação](#instalação)
- [Uso](#uso)
- [Agente Auto-Evolutivo](#agente-auto-evolutivo)
- [API](#api)
- [Segurança](#segurança)
- [Changelog](#changelog)

---

## 🎯 Visão Geral

O sistema permite que **candidatos a franqueados** avaliem pontos comerciais de forma estruturada, com:

- 📍 **Mapa interativo** (Leaflet + OpenStreetMap)
- 🔥 **Mapa de calor comercial** (dados OSM + fluxo da rede)
- 📊 **Análise automatizada** de viabilidade (semaforo: verde/amarelo/vermelho)
- ⏱️ **Contador de fluxo** padronizado (2 minutos, 12h-14h)
- 📈 **Ranking comparativo** entre pontos
- 🤖 **Agente auto-evolutivo** (visível apenas para administradores)

---

## 🔐 Arquitetura de Acesso

O sistema opera com **dois perfis de usuário** distintos:

```
┌─────────────────────────────────────────────────────────────┐
│                     TELA DE LOGIN                          │
├─────────────────────────────────────────────────────────────┤
│  E-mail: [____________________]                            │
│  Senha:  [____________________]                            │
│                                                            │
│  ☑ Acesso administrativo (visão do agente 🤖)             │
│     └─ Marque para acessar o painel do agente              │
│        auto-evolutivo, relatórios e configurações.          │
│                                                            │
│  [ Entrar ]                                                │
└─────────────────────────────────────────────────────────────┘
                              │
              ┌───────────────┴───────────────┐
              ▼                               ▼
    ┌─────────────────────┐         ┌─────────────────────┐
    │   👤 CANDIDATO      │         │   🛡️ ADMINISTRADOR  │
    │   / Franqueado      │         │                     │
    └─────────────────────┘         └─────────────────────┘
              │                               │
    ┌─────────┴─────────┐         ┌───────────┴───────────┐
    │ • Mapa interativo │         │ • Tudo do Candidato │
    │ • Adicionar pontos│         │ • Botão 🤖 flutuante│
    │ • Analisar pontos │         │ • Painel do agente  │
    │ • Contador fluxo  │         │ • Toasts técnicos   │
    │ • Ranking         │         │ • Relatórios        │
    │ • Salvar dados    │         │ • Logs de sistema   │
    │ • Handoff         │         │ • Forçar fixes      │
    │                   │         │ • Exportar relatório│
    │ ❌ Sem agente     │         │                     │
    │ ❌ Sem relatórios │         │                     │
    └───────────────────┘         └─────────────────────┘
```

### Fluxo de Autenticação

```javascript
// 1. Login com role
const USER_ROLE = document.getElementById("chkAdmin")?.checked 
  ? "admin" 
  : "candidato";

// 2. Inicialização condicional
function iniciar(email, role) {
  USER_ROLE = role || "candidato";

  // Botão 🤖 só para admin
  document.getElementById("agenteToggle").style.display = 
    (USER_ROLE === "admin") ? "flex" : "none";

  // Toasts técnicos só para admin
  document.getElementById("agente-toasts").style.display = 
    (USER_ROLE === "admin") ? "flex" : "none";

  // Agente só executa para admin
  if (USER_ROLE === "admin") {
    iniciarAgente();
  }
}

// 3. Proteção das funções do agente
function iniciarAgente() {
  if (USER_ROLE !== "admin") {
    console.log("[Agente] Acesso negado — apenas administradores");
    return;
  }
  // ... inicialização do agente
}
```

### Variáveis de Estado

| Variável | Tipo | Descrição |
|----------|------|-----------|
| `TOKEN` | `string` | JWT de autenticação Supabase |
| `USER_ROLE` | `"admin" \| "candidato"` | Perfil do usuário logado |
| `pontoAtivo` | `string` | ID do ponto aberto no painel |
| `marcadores` | `Object` | Referências aos pins do mapa |
| `calorOn` | `boolean` | Estado do mapa de calor |

---

## 🛠 Tecnologias

| Tecnologia | Uso |
|------------|-----|
| **HTML5** | Estrutura da aplicação (single-file) |
| **CSS3** | Estilização com variáveis CSS |
| **Vanilla JS** | Lógica da aplicação (sem frameworks) |
| **Leaflet** | Mapa interativo |
| **Leaflet.heat** | Mapa de calor comercial |
| **Supabase** | Backend (Auth + Database + Edge Functions) |
| **Nominatim** | Geocodificação reversa (OpenStreetMap) |
| **Overpass API** | Dados de POIs comerciais |

---

## 📁 Estrutura do Projeto

```
viabilidade-ponto/
├── viabilidade_ponto_com_agente.html   # Aplicação completa (single-file)
├── README.md                             # Este arquivo
├── CHANGELOG.md                          # Histórico de versões
├── .env.example                          # Variáveis de ambiente
└── docs/
    ├── arquitetura.md                    # Diagramas de arquitetura
    └── api.md                            # Documentação da API
```

> **Nota:** O projeto é intencionalmente um **single-file HTML** para facilitar deploy em qualquer servidor estático (GitHub Pages, Netlify, Vercel, etc.).

---

## ⚡ Instalação

### Pré-requisitos
- Conta no [Supabase](https://supabase.com)
- Projeto Supabase configurado com tabelas e Edge Functions

### Passos

1. **Clone o repositório**
   ```bash
   git clone https://github.com/lucasaquinosantos26-maker/viabilidade-ponto.git
   cd viabilidade-ponto
   ```

2. **Configure as variáveis de ambiente**
   ```bash
   cp .env.example .env
   ```

   Edite `.env`:
   ```env
   VITE_SUPABASE_URL=https://seu-projeto.supabase.co
   VITE_SUPABASE_ANON_KEY=sua_chave_anon_aqui
   VITE_GOOGLE_MAPS_KEY=sua_chave_google_opcional
   ```

3. **Configure o Supabase**
   - Crie as tabelas: `pontos`, `fluxos`, `corredores`
   - Deploy das Edge Functions: `api-viabilidade`, `analisar-ponto`, `registrar-fluxo`
   - Configure RLS policies

4. **Abra o arquivo**
   ```bash
   # Opção 1: Servidor local
   npx serve .

   # Opção 2: Direto no navegador
   open viabilidade_ponto_com_agente.html
   ```

---

## 🎮 Uso

### Para Candidatos/Franqueados

1. Acesse a aplicação e faça login
2. Clique no mapa para adicionar um ponto
3. Preencha os dados do imóvel (fachada, salão, aluguel)
4. Faça a contagem de fluxo (2 minutos, 12h-14h)
5. Clique em **"Analisar ponto"** para ver a recomendação
6. Compare pontos no ranking inferior
7. Clique em **"Seguir para negociação"** quando decidir

### Para Administradores

1. Faça login **marcando o checkbox** "Acesso administrativo"
2. O botão 🤖 aparecerá no canto inferior direito
3. Clique em 🤖 para abrir o painel do agente
4. Veja:
   - **Fixes aplicados** em tempo real
   - **Falhas detectadas** com criticidade
   - **Relatório exportável** em Markdown
   - **Botão "Forçar Verificação"** para reavaliar

---

## 🤖 Agente Auto-Evolutivo

O agente é um sistema de monitoramento e correção automática que roda no cliente.

### Funcionalidades

| Módulo | Descrição |
|--------|-----------|
| **Detector** | Monitora erros de runtime, performance, API, UX e segurança |
| **AutoFixer** | Aplica correções automaticamente em tempo de execução |
| **Notificador** | Exibe toasts não-bloqueantes para o admin |
| **Changelog** | Registra todas as ações do agente no localStorage |
| **Painel de Controle** | UI flutuante com status, fixes e exportação |

### Fixes Automáticos

- ✅ **Validação de inputs** — limites numéricos em tempo real
- ✅ **Melhorar botões** — efeito visual ao clicar
- ✅ **Atalhos de teclado** — ESC fecha painel, Ctrl+K foca busca
- ✅ **Loader global** — barra de progresso nas requisições
- ✅ **Substituir alerts** — toasts não-bloqueantes
- ✅ **Responsividade mobile** — ajuste automático do painel
- ✅ **Cache de API** — reduz chamadas repetidas
- ✅ **Proteção null** — guards em funções críticas

### Acesso Restrito

> ⚠️ **O agente só é visível e executável para usuários com `USER_ROLE === "admin"`.**
>
> Candidatos e franqueados **não veem** o botão 🤖, os toasts técnicos, o painel de controle ou qualquer traço do agente na interface.

---

## 🔌 API

### Endpoints (Supabase Edge Functions)

| Método | Endpoint | Descrição |
|--------|----------|-----------|
| `POST` | `/auth/v1/token` | Login com e-mail/senha |
| `POST` | `/auth/v1/signup` | Criar conta |
| `POST` | `/auth/v1/recover` | Recuperar senha |
| `GET`  | `/functions/v1/api-viabilidade/pontos` | Listar pontos |
| `POST` | `/functions/v1/api-viabilidade/pontos` | Criar ponto |
| `GET`  | `/functions/v1/api-viabilidade/pontos/:id` | Detalhes do ponto |
| `PATCH`| `/functions/v1/api-viabilidade/pontos/:id` | Atualizar ponto |
| `POST` | `/functions/v1/api-viabilidade/enriquecimento/:id` | Dados enriquecidos |
| `POST` | `/functions/v1/analisar-ponto` | Executar análise |
| `POST` | `/functions/v1/registrar-fluxo` | Registrar contagem |
| `GET`  | `/functions/v1/registrar-fluxo?ponto_id=` | Resumo de fluxo |
| `POST` | `/functions/v1/api-viabilidade/handoff` | Encaminhar para negociação |
| `GET`  | `/functions/v1/api-viabilidade/ranking` | Ranking de pontos |
| `GET`  | `/functions/v1/api-viabilidade/corredores` | Corredores recomendados |
| `GET`  | `/functions/v1/api-viabilidade/calor` | Dados de calor |

---

## 🔒 Segurança

### Recomendações

1. **Mover ANON_KEY para variável de ambiente**
   ```javascript
   // ❌ Não fazer
   const ANON = "eyJhbGciOiJIUzI1NiIs...";

   // ✅ Fazer
   const ANON = import.meta.env.VITE_SUPABASE_ANON_KEY;
   ```

2. **Validar role no backend**
   - Implementar RLS policies no Supabase
   - Validar `role` nas Edge Functions
   - Não confiar apenas no frontend

3. **Rate limiting**
   - Limitar requisições por IP
   - Proteger endpoints de análise

4. **HTTPS obrigatório**
   - Sempre em produção
   - Configurar HSTS

---

## 📝 Changelog

Veja [CHANGELOG.md](./CHANGELOG.md) para o histórico completo de versões.

---

## 👥 Contribuição

1. Fork o repositório
2. Crie uma branch: `git checkout -b feature/nome-da-feature`
3. Commit suas mudanças: `git commit -m "feat: descrição"`
4. Push para a branch: `git push origin feature/nome-da-feature`
5. Abra um Pull Request

---

## 📄 Licença

Proprietário — Ótica Center. Todos os direitos reservados.

---

<p align="center">
  <b>Ótica Center</b> — Expansão Inteligente 🚀
</p>
