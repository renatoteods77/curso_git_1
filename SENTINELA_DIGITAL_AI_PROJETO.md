# Sentinela Digital AI — Projeto Completo de Produto

## 1) Visão do produto

**Nome da plataforma:** Sentinela Digital AI  
**Tagline:** Seu guarda-costas digital contra phishing, fraudes e vulnerabilidades.

**Proposta de valor:**
Uma plataforma SaaS de defesa digital contínua que combina detecção de phishing, varredura de vulnerabilidades, classificação de risco com IA e resposta guiada para usuários e empresas.

## 2) Objetivo estratégico

Transformar risco técnico em decisão prática:

- detectar ameaças cedo;
- explicar impacto em linguagem clara;
- priorizar o que corrige primeiro;
- orientar a remediação com playbooks.

## 3) Pilares de arquitetura e conformidade

A arquitetura deve seguir os princípios do **NIST CSF 2.0**:

- **Governar:** políticas, responsabilidade, métricas, auditoria;
- **Identificar:** inventário de ativos, superfícies de ataque, dependências;
- **Proteger:** controles preventivos (MFA, RBAC, hardening, educação);
- **Detectar:** monitoramento contínuo e análise de anomalias;
- **Responder:** playbooks, contenção, comunicação e tratamento;
- **Recuperar:** restauração, lições aprendidas e melhoria contínua.

Cobertura técnica sugerida:

- **OWASP Top 10 2025** (aplicações web);
- **OWASP Mobile Top 10 2024** (aplicações mobile);
- mapeamento para CVE/CWE/CAPEC quando aplicável.

## 4) Módulos do produto

### 4.1 Módulo anti-phishing e engenharia social

**Entradas:** e-mail, SMS, WhatsApp (integrações permitidas), QR Code, links, anexos, páginas acessadas.  
**Detecções:**

- spoofing e lookalike domains;
- URL encurtada/mascarada;
- pedido de credenciais/PIX/token;
- urgência artificial e linguagem coercitiva;
- divergência entre remetente, link e conteúdo;
- anexos suspeitos (macro, executável, formato incomum).

**Saída para usuário (explicável):**

- Risco detectado;
- Nível (baixo/médio/alto/crítico);
- Motivo do alerta;
- Impacto provável;
- Ação imediata recomendada.

### 4.2 Módulo de varredura de vulnerabilidades

**Escopo técnico:**

- portas/serviços expostos;
- versões e patching;
- certificados e TLS;
- CVEs conhecidas;
- configuração insegura;
- autenticação fraca e ausência de MFA;
- APIs expostas;
- web app scanning (DAST leve + regras de segurança);
- endpoints críticos.

**Princípios operacionais:**

- varredura contínua + sob demanda;
- janela de manutenção configurável;
- autorização explícita por ativo/cliente;
- trilha de auditoria completa.

### 4.3 Módulo de classificação inteligente de risco (IA)

Transforma achados técnicos em narrativa de negócio:

- **Risco técnico** (ex.: RDP aberto);
- **Risco de negócio** (ex.: acesso indevido e indisponibilidade);
- **Risco operacional** (ex.: paralisação, vazamento);
- **Prioridade** (crítica/alta/média/baixa);
- **SLA sugerido de correção** por criticidade.

Modelo sugerido de score composto (0–100):

- severidade técnica (CVSS e exploitabilidade);
- exposição do ativo (internet/interno);
- sensibilidade do dado processado;
- probabilidade de abuso;
- impacto financeiro/operacional.

### 4.4 Módulo de monitoramento contínuo

Monitora eventos em tempo real:

- novos dispositivos/ativos;
- mudanças de configuração;
- instalação de software suspeito;
- login anômalo (geo/horário/dispositivo);
- acesso a domínio malicioso;
- indícios de credencial vazada.

### 4.5 Módulo de resposta guiada

Playbooks parametrizados por ameaça:

1. bloquear IOC/domínio/IP;
2. quarentenar e-mail/artefato;
3. revogar sessão e token;
4. forçar troca de senha + MFA;
5. investigar logs relacionados;
6. abrir incidente e registrar evidências.

## 5) Telas essenciais

### Dashboard principal

- risco global da organização;
- ativos monitorados;
- ameaças detectadas nas últimas 24h;
- top usuários/ativos de maior risco;
- vulnerabilidades críticas abertas;
- status de correções por SLA.

### Tela de alerta

- título do risco;
- origem e timestamp;
- severidade e score;
- explicação em linguagem simples;
- impacto provável;
- ações recomendadas;
- botão **Corrigir agora**.

### Tela de ativo

- identificação do ativo;
- IP/host e sistema operacional;
- serviços e portas relevantes;
- vulnerabilidades detectadas;
- última varredura;
- tendência de risco (7/30 dias).

### Tela de compliance e auditoria

- políticas habilitadas;
- trilha de auditoria por ação;
- exceções de risco aprovadas;
- exportação de evidências.

## 6) Arquitetura técnica (alto nível)

```txt
[Web/Mobile/Extensão/Agente]
          |
          v
[API Gateway + Auth]
          |
  ---------------------------
  |           |             |
  v           v             v
[Risk API] [Scan API] [Phishing API]
  |           |             |
  ---------[Event Bus]-------
               |
        [Correlation Engine]
               |
      [AI Explain & Prioritize]
               |
   [Alerting + Playbooks Engine]
               |
      [SIEM/EDR/Email/WhatsApp]

[PostgreSQL] [Redis] [Object Storage] [Audit Log Imutável]
```

## 7) Fluxo backend/frontend

### 7.1 Fluxo de ingestão e análise

1. frontend envia evento (link, e-mail suspeito, ativo novo);
2. API valida tenant e permissões (RBAC);
3. evento entra na fila (Kafka/RabbitMQ);
4. motores especializados processam (phishing, CVE, config);
5. correlação calcula score e prioridade;
6. IA gera explicação para usuário e técnico;
7. sistema dispara alerta + recomenda playbook;
8. ação executada volta como evidência para auditoria.

### 7.2 Fluxo de remediação guiada

1. usuário clica em **Corrigir agora**;
2. backend lista automações permitidas para o perfil;
3. operador escolhe execução automática ou assistida;
4. execução gera logs e confirmações;
5. risco recalculado após mitigação.

## 8) Modelo SaaS

### Segmentos

- **B2C:** proteção pessoal e familiar;
- **B2B:** PMEs e enterprise por ativo/usuário;
- **B2G:** órgãos públicos por unidade/volume.

### Planos sugeridos

- **Starter (B2C):** proteção de links, alertas básicos, score pessoal;
- **Business:** inventário de ativos, scanner recorrente, dashboard corporativo;
- **Enterprise:** automação avançada, APIs, SIEM/EDR, SOC-ready e SLA dedicado.

### Métricas de negócio

- MRR/ARR;
- churn;
- tempo médio de detecção (MTTD);
- tempo médio de resposta (MTTR);
- % de alertas resolvidos dentro do SLA;
- redução de incidentes críticos.

## 9) Roadmap em 3 fases

### Fase 1 — MVP (60–90 dias)

- onboarding de usuários e ativos;
- scanner básico de vulnerabilidades;
- analisador de links suspeitos;
- score de risco inicial;
- dashboard web;
- alertas por e-mail/WhatsApp.

### Fase 2 — Profissional (90–150 dias)

- extensão de navegador;
- análise de anexos e e-mail corporativo;
- classificação com IA explicável;
- relatórios PDF/CSV;
- ambiente multiempresa (multi-tenant).

### Fase 3 — Avançada (150+ dias)

- agente de endpoint;
- resposta semiautomática/automática;
- integração SOC/SIEM/EDR;
- detecção comportamental;
- threat hunting assistido por IA.

## 10) Segurança e governança do produto

Obrigatórios:

- consentimento explícito de escaneamento;
- escopo técnico e jurídico definido em contrato;
- criptografia em trânsito e em repouso;
- segregação por tenant;
- RBAC e MFA obrigatórios;
- trilha de auditoria inviolável;
- gestão de retenção e descarte de dados;
- logs de segurança e de acesso administrativo.

## 11) Prompt pronto para Base44

```txt
Crie uma plataforma SaaS chamada "Sentinela Digital AI" para proteção digital contínua.

Objetivo:
Detectar phishing e vulnerabilidades, classificar risco com IA e guiar resposta em tempo real para usuários e empresas.

Perfis de usuário:
1) Usuário comum
2) Analista de segurança
3) Gestor executivo
4) Admin da organização

Módulos obrigatórios:
- Anti-phishing (links, mensagens, QR codes, anexos)
- Scanner de vulnerabilidades (portas, serviços, CVEs, configurações)
- Motor de score de risco (0-100) com severidade e prioridade
- IA explicável (impacto técnico, negócio e recomendação)
- Resposta guiada com playbooks
- Monitoramento contínuo de ativos e eventos

Telas obrigatórias:
1) Dashboard geral
2) Detalhe de alerta
3) Detalhe de ativo
4) Compliance e auditoria
5) Configuração de integrações

Requisitos de UX:
- Linguagem clara e não técnica para usuário comum
- Visual por cores (verde, amarelo, laranja, vermelho)
- Botão “Corrigir agora” com ações recomendadas
- Histórico e timeline de incidentes

Requisitos técnicos:
- Multi-tenant
- RBAC + MFA
- API-first
- Fila de eventos
- Banco relacional + cache
- Registro de auditoria
- Integração com e-mail, SIEM e EDR

Requisitos de segurança:
- Consentimento explícito por escopo
- Criptografia em trânsito/repouso
- Isolamento de dados por tenant
- Trilhas de auditoria de todas as ações

Entregáveis esperados:
- Estrutura de dados (entidades principais)
- Fluxos de backend/frontend
- Prototipação das telas
- APIs essenciais
- Regras de cálculo de score de risco
- Playbooks iniciais para phishing e credencial comprometida
```

## 12) Frase de posicionamento final

**Sentinela Digital AI é uma plataforma de defesa digital inteligente que transforma sinais técnicos de ameaça em ações claras e imediatas para reduzir fraudes, invasões e interrupções de negócio.**
