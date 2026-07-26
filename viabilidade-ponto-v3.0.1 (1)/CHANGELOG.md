# Changelog

Todas as mudanças notáveis deste projeto serão documentadas neste arquivo.

O formato é baseado em [Keep a Changelog](https://keepachangelog.com/pt-BR/1.0.0/),
e este projeto adere ao [Semantic Versioning](https://semver.org/lang/pt-BR/spec/v2.0.0.html).

---

## [3.0.0] - 2026-07-26

### 🚀 Adicionado

- **Controle de Acesso por Role**
  - Checkbox "Acesso administrativo" na tela de login
  - Variável `USER_ROLE` com valores `"admin"` e `"candidato"`
  - Botão 🤖 do agente **visível apenas para administradores**
  - Painel do agente auto-evolutivo **restrito a admin**
  - Toasts técnicos do agente **só para admin**
  - Função `iniciarAgente()` protegida contra acesso não-autorizado

- **Agente Auto-Evolutivo v3.0**
  - Motor de patches com aplicação automática
  - Logger inteligente com níveis (DEBUG, INFO, AVISO, ERRO, CRITICO)
  - Notificador com toasts não-bloqueantes
  - Changelog automático com exportação em Markdown
  - Detector de falhas em runtime, performance, API, UX e segurança
  - AutoFixer com 8 correções automáticas:
    - Validação de inputs numéricos
    - Melhoria visual dos botões
    - Atalhos de teclado (ESC, Ctrl+K, Ctrl+S)
    - Loader global nas requisições
    - Substituição de alerts por toasts
    - Responsividade mobile do painel
    - Cache de API (Overpass)
    - Proteção null/undefined em funções críticas

- **Painel de Controle do Agente**
  - Botão flutuante 🤖 no canto inferior direito
  - Dashboard com contadores de fixes e falhas
  - Lista de fixes aplicados com status
  - Lista de falhas detectadas com criticidade
  - Botão "Exportar Relatório" (Markdown)
  - Botão "Forçar Verificação"

### 🔒 Segurança

- Chaves de API mascaradas nos logs do console
- Detecção automática de chaves JWT expostas no código fonte
- Alerta de segurança para administradores quando chave é detectada

### 🐛 Corrigido

- Alerts nativos congelando a interface — substituídos por toasts
- Painel cortado em telas menores que 420px
- Valores negativos em inputs numéricos
- Falta de feedback visual durante requisições à API

### 📝 Documentação

- README.md completo com arquitetura de acesso
- Diagrama de fluxo de autenticação por role
- Tabela de endpoints da API
- Guia de instalação e configuração

---

## [2.1.0] - 2026-07-20

### Adicionado

- Mapa de calor comercial (dados reais OSM + fluxo da rede)
- Toggle 🔥 Calor com visualização de densidade comercial
- Cache de requisições Overpass API
- Suporte a links encurtados do Google Maps com aviso

### Corrigido

- Timeout em requisições de geocodificação
- Tratamento de erros na busca por endereço

---

## [2.0.0] - 2026-07-15

### Adicionado

- Agente auto-evolutivo v2.0 (injeção no HTML)
- Sistema de patches automáticos
- Monitoramento de performance
- Detecção de falhas em tempo real

### Alterado

- Estrutura do HTML para suportar injeção do agente
- Sistema de logs do console

---

## [1.2.0] - 2026-07-10

### Adicionado

- Contador de fluxo com cronômetro de 2 minutos
- Registro manual de contagem
- Validação de janela horária (12h-14h)
- Cálculo de média com múltiplas leituras

### Corrigido

- Cálculo de dias distintos no resumo de fluxo
- Formatação de números no padrão pt-BR

---

## [1.1.0] - 2026-07-05

### Adicionado

- Ranking comparativo de pontos
- Handoff para negociação
- Corredores recomendados no mapa
- Análise preliminar com dados parciais

### Corrigido

- Posicionamento do painel em mobile
- Scroll do ranking inferior

---

## [1.0.0] - 2026-07-01

### Adicionado

- Lançamento inicial da aplicação
- Mapa interativo com Leaflet
- Autenticação com Supabase Auth
- Cadastro e login de candidatos/franqueados
- Adição de pontos por clique no mapa ou busca por endereço
- Formulário de dados do imóvel
- Análise automatizada de viabilidade
- Recomendação com semáforo (verde/amarelo/vermelho)
- Estimativa de reforma
- Detecção de dados faltantes
- Listagem de riscos
- Recuperação de senha
- Sessão persistente no localStorage

---

## Roadmap

### [3.1.0] — Previsto
- [ ] Whitelist de e-mails administrativos
- [ ] Senha extra/PIN para acesso admin
- [ ] Logs de auditoria (quem acessou o agente e quando)
- [ ] Backend validation do role via Supabase RLS

### [4.0.0] — Previsto
- [ ] Modularização do código (separar em arquivos)
- [ ] Testes E2E com Playwright
- [ ] CI/CD com GitHub Actions
- [ ] Dashboard administrativo completo

---

<p align="center">
  <a href="https://github.com/lucasaquinosantos26-maker/viabilidade-ponte">GitHub</a> •
  <a href="#">Documentação</a> •
  <a href="#">Suporte</a>
</p>
