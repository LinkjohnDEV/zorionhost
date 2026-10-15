<div align="center">

<img src="https://raw.githubusercontent.com/LinkjohnDEV/zorionhostpanel/main/assets/logo.png" alt="Zorion Host Logo" width="120" onerror="this.style.display='none'"/>

# ⚡ Zorion Host — Next-Gen Cloud & VPS Platform

**A mais moderna plataforma brasileira de provisionamento e gerenciamento autônomo de VPS e infraestrutura em nuvem.**

Projetada para substituir com superioridade softwares legados como WHMCS, cPanel e VirtFusion, unindo interface de usuário ultramoderna, automação profunda de hypervisors Proxmox VE, gerenciamento dinâmico de rede via MikroTik e cobrança instantânea via Pix.

---

[![Next.js 14](https://img.shields.io/badge/Next.js-14.2-000000?style=for-the-badge&logo=next.js&logoColor=white)](https://nextjs.org/)
[![Fastify](https://img.shields.io/badge/Fastify-4.x-000000?style=for-the-badge&logo=fastify&logoColor=white)](https://fastify.dev/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.x-3178C6?style=for-the-badge&logo=typescript&logoColor=white)](https://www.typescriptlang.org/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-15+-4169E1?style=for-the-badge&logo=postgresql&logoColor=white)](https://www.postgresql.org/)
[![Proxmox VE](https://img.shields.io/badge/Proxmox-VE_8.x-E57000?style=for-the-badge&logo=proxmox&logoColor=white)](https://www.proxmox.com/)
[![MikroTik RouterOS](https://img.shields.io/badge/MikroTik-RouterOS_v7-224466?style=for-the-badge&logo=mikrotik&logoColor=white)](https://mikrotik.com/)
[![Redis & BullMQ](https://img.shields.io/badge/BullMQ-Redis-DC382D?style=for-the-badge&logo=redis&logoColor=white)](https://redis.io/)
[![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-3.x-38B2AC?style=for-the-badge&logo=tailwind-css&logoColor=white)](https://tailwindcss.com/)

</div>

---

## 🌐 Ecossistema em Produção

| Serviço | Endereço | Descrição |
|---|---|---|
| 🖥️ **Site Oficial** | [zorionhost.com](https://zorionhost.com) | Portal institucional e catálogo de planos |
| 🎛️ **Painel de Controle** | [painel.zorionhost.com](https://painel.zorionhost.com) | Gestão de VPS, consoles VNC/SSH e faturamento |
| 🔌 **API Central** | [api.zorionhost.com](https://api.zorionhost.com) | Engine REST de orquestração e webhooks |
| 🗄️ **S3 Object Storage** | [s3.zorionhost.com](https://s3.zorionhost.com) | Armazenamento de backups e snapshots MinIO |

---

## ✨ Principais Diferenciais e Funcionalidades

### 🚀 Automação & Virtualização
- **Provisionamento Zero-Touch**: Servidor entregue, instalado e conectado à internet em menos de 60 segundos após a confirmação do pagamento.
- **Orquestração Proxmox VE nativa**: Criação de clones linked/full, dimensionamento de vCPU, alocação de memória RAM e expansão de storage em LVM-thin / ZFS.
- **Catálogo de Sistemas Operacionais**:
  - Ubuntu (22.04 LTS / 24.04 LTS)
  - Debian (11 / 12)
  - CentOS & AlmaLinux (7 / 8 / 9)
  - Rocky Linux
  - Templates Customizados (FXServer FiveM / RedM, Celular Virtual Android/Waydroid)
- **Controle Total de Ciclo de Vida**: Ligar, desligar suave (`ACPI shutdown`), forçar parada (`kill`), reiniciar e reinstalação com 1 clique.
- **Console Web Integrado**: Acesso NoVNC via WebSocket diretamente no navegador e terminal SSH seguro via web.

### 🌐 Rede Dinâmica & Borda
- **Automação MikroTik RouterOS v7**:
  - Alocação e desalocação dinâmica de IPs dedicados do pool.
  - Criação de regras de firewall DST-NAT em tempo real para portas de jogos, serviços e SSH externo sob demanda.
- **Reverse Proxy Nginx Automatizado**:
  - Cadastro de domínios personalizados (`seusite.com`) com proxy reverso direto para a porta interna da VPS.
  - Emissão e renovação automática de certificados SSL gratuitos via Let's Encrypt / Certbot com tratamento resiliente de transient errors.

### 💰 Faturamento & Automação Financeira
- **Pagamentos Instantâneos via Pix**: Geração de QR Code e Pix Copia e Cola dinâmicos com confirmação via Webhook em segundos.
- **Cobrança Recorrente e Gestão de Vencimento**: Suspensão e encerramento automatizados de instâncias inadimplentes via worker BullMQ agendado.
- **Histórico Completo de Faturas**: Emissão de faturas em PDF e extrato financeiro unificado no painel do cliente.

### 📲 Notificações Multicanal
- **WhatsApp Integrado**: Alertas instantâneos de provisionamento, envio de faturas, avisos de vencimento e credenciais de acesso via WhatsApp.
- **Failover para Email (SMTP)**: Estratégia de redundância ativa — se o canal do WhatsApp estiver indisponível, a notificação é imediatamente roteada via SMTP seguro.

---

## 🏛️ Arquitetura de Alto Nível

```
                      ┌─────────────────────────────────────────┐
                      │              CLIENT BROWSER             │
                      └────────────────────┬────────────────────┘
                                           │
                         HTTPS (443) / WSS │ (Terminal & VNC)
                                           ▼
             ┌───────────────────────────────────────────────────────────┐
             │               NGINX REVERSE PROXY & SSL                   │
             │   - Certbot Let's Encrypt SSL                             │
             │   - WebSocket Upgrades (VNC & SSH)                        │
             │   - Roteamento por Hostname                               │
             └─────────────┬───────────────────────────────┬─────────────┘
                           │                               │
             [Host: painel.zorionhost.com]    [Host: api.zorionhost.com]
                           │                               │
                           ▼                               ▼
             ┌───────────────────────────┐   ┌───────────────────────────┐
             │     FRONTEND APPLICATION  │   │        CORE REST API      │
             │      (Next.js 14 App)     │──▶│     (Fastify + Prisma)    │
             │   Porta 3000 / SSR & RSC  │   │         Porta 3001        │
             └───────────────────────────┘   └─────────────┬─────────────┘
                                                           │
                      ┌────────────────────────────────────┼────────────────────────────────────┐
                      ▼                                    ▼                                    ▼
        ┌───────────────────────────┐        ┌───────────────────────────┐        ┌───────────────────────────┐
        │        POSTGRESQL         │        │       REDIS & BULLMQ      │        │      ORQUESTRAÇÃO REDE    │
        │   - Banco relacional       │        │   - Filas de provision    │        │   - MikroTik RouterOS     │
        │   - Schema com Prisma     │        │   - Métricas periódicas   │        │   - Pool de IPs dinâmicos │
        │   - Auditoria e Faturas   │        │   - Monitor de vencimento │        │   - Regras DST-NAT        │
        └───────────────────────────┘        └─────────────┬─────────────┘        └───────────────────────────┘
                                                           │
                                                           ▼
                                             ┌───────────────────────────┐
                                             │      HYPERVISOR CLUSTER   │
                                             │       (Proxmox VE API)    │
                                             │   - Provisionamento QEMU  │
                                             │   - Cloud-Init Injection  │
                                             │   - NoVNC Proxy / Snap    │
                                             └───────────────────────────┘
```

---

## 📂 Estrutura do Repositório

```bash
zorionhost/
├── apps/
│   ├── api/                      # Backend REST API em Fastify + TypeScript
│   │   ├── src/
│   │   │   ├── config/           # Validação Zod de variáveis de ambiente
│   │   │   ├── lib/              # Prisma client e conexões auxiliares
│   │   │   ├── plugins/          # Plugins Fastify (JWT, CORS, Swagger)
│   │   │   ├── routes/           # Rotas da aplicação:
│   │   │   │   ├── admin.ts      # Controle total de nós, IPs e clientes
│   │   │   │   ├── auth.ts       # Autenticação JWT, Refresh Tokens e 2FA
│   │   │   │   ├── invoices.ts   # Emissão e reconciliação de cobranças
│   │   │   │   ├── servers.ts    # Gestão de VPS, power, portas e VNC
│   │   │   │   └── webhooks.ts   # Callbacks de gateways de pagamento
│   │   │   ├── services/         # Clientes de integração externa:
│   │   │   │   ├── mikrotik.ts   # Conector RouterOS API v7
│   │   │   │   ├── nginxProxy.ts # Automação de vhosts e Certbot
│   │   │   │   ├── proxmox.ts    # Cliente HTTP Proxmox VE REST API
│   │   │   │   └── email.ts      # Transporte SMTP resiliente
│   │   │   └── workers/          # Background workers BullMQ:
│   │   │       ├── provision.ts  # Worker de criação/destruição de VMs
│   │   │       ├── invoiceCheck.ts# Worker de verificação de faturas
│   │   │       └── metricsCollector.ts # Coleta periódica de telemetria
│   │   ├── prisma/
│   │   │   └── schema.prisma     # Modelagem completa do banco de dados
│   │   └── tsconfig.json
│   │
│   └── panel/                    # Frontend moderno em Next.js 14
│       ├── app/                  # App Router (páginas públicas e autenticadas)
│       │   ├── (auth)/           # Telas de Login, Registro e Redefinição
│       │   ├── (dashboard)/      # Dashboard principal, lista de servidores e billing
│       │   ├── admin/            # Painel administrativo executivo
│       │   └── servidores/[id]/  # Gerenciamento aprofundado da VPS
│       ├── components/           # Componentes modulares UI (Shadcn + Radix)
│       └── public/               # Ativos estáticos, branding e ícones
│
├── ecosystem.config.js           # Gerenciador de processos PM2
└── package.json                  # Workspace monorepo
```

---

## 🛠️ Tecnologias Utilizadas

| Camada | Tecnologia | Propósito |
|---|---|---|
| **Frontend** | **Next.js 14** (React 18) | Renderização híbrida SSR/Client, SEO e roteamento |
| **Estilização** | **Tailwind CSS + Lucide Icons** | Design system fluido, dark mode nativo e micro-animações |
| **Backend** | **Fastify 4** | Framework HTTP de altíssima performance para Node.js |
| **Linguagem** | **TypeScript 5** | Tipagem estática fim-a-fim e robustez de código |
| **Persistência** | **PostgreSQL 15 + Prisma ORM** | Modelagem robusta de relacionamentos e migrações seguras |
| **Mensageria** | **BullMQ + Redis 7** | Filas assíncronas concorrentes para operações pesadas |
| **Virtualização** | **Proxmox VE 8 (KVM/QEMU)** | Hypervisor open-source para execução isolada das VPS |
| **Provisionamento** | **Cloud-Init** | Injeção automática de usuários, senhas root e chaves SSH |
| **Rede** | **MikroTik RouterOS API** | Automação de NAT, balanceamento e liberação de portas |
| **Reverse Proxy**| **Nginx + Certbot** | Terminação TLS/SSL, balanceamento e repasse WebSocket |
| **Processos** | **PM2** | Monitoramento de runtime, auto-recovery e clustering |

---

## 🚀 Como Executar em Ambiente Local

### Pré-requisitos
- **Node.js**: v20.x ou superior
- **PostgreSQL**: v14 ou superior
- **Redis**: v6 ou superior
- Acesso a um cluster Proxmox VE (com token de API criado) ou modo mock

### 1. Clonando o projeto
```bash
git clone https://github.com/LinkjohnDEV/zorionhost.git
cd zorionhost
```

### 2. Instalando as dependências
```bash
npm install
```

### 3. Configuração de Variáveis de Ambiente
Copie o exemplo do `.env`:
```bash
cp .env.example .env
```

Preencha as configurações fundamentais no arquivo `.env`:
```env
# Banco de Dados & Cache
DATABASE_URL="postgresql://usuario:senha@localhost:5432/zorionhost?schema=public"
REDIS_URL="redis://localhost:6379"

# Segurança & Auth
JWT_SECRET="seu-jwt-secret-super-seguro"
JWT_REFRESH_SECRET="seu-jwt-refresh-secret-super-seguro"

# Proxmox VE (Nó Primário)
PROXMOX_HOST="https://10.0.1.50:8006"
PROXMOX_NODE="prox1"
PROXMOX_TOKEN_ID="root@pam!token-zorion"
PROXMOX_TOKEN_SECRET="seu-token-secret"
PROXMOX_VM_STORAGE="local-lvm"

# MikroTik (Gateway)
MIKROTIK_HOST="10.255.255.1"
MIKROTIK_PORT=3540
MIKROTIK_USER="Zorionhost"
MIKROTIK_PASSWORD="sua-senha-mikrotik"
MIKROTIK_PUBLIC_IP="200.155.145.70"

# URLs dos Serviços
API_PUBLIC_URL="https://api.zorionhost.com"
PANEL_URL="https://painel.zorionhost.com"
NEXT_PUBLIC_API_URL="http://localhost:3001"
```

### 4. Sincronizando o Banco de Dados
```bash
npx prisma db push --schema=apps/api/prisma/schema.prisma
```

### 5. Compilando e Rodando o Ecossistema
Para compilar a API TypeScript:
```bash
npx tsc -p apps/api/tsconfig.json
```

Para subir os serviços via **PM2**:
```bash
pm2 start ecosystem.config.js
pm2 save
```

Ou em modo desenvolvimento:
```bash
# Terminal 1 - Backend API:
npm --prefix apps/api run dev

# Terminal 2 - Frontend Panel:
npm --prefix apps/panel run dev
```

---

## 🔒 Segurança & Boas Práticas

- **Sem armazenamento de senhas em texto puro**: Senhas de usuários e clientes utilizam hash criptográfico forte via `bcrypt`.
- **Autenticação em Dois Fatores (2FA)**: Suporte a TOTP (Google Authenticator / Authy) nativo.
- **Isolamento de Máquinas Virtuais**: Nenhuma VM tem acesso não autorizado à sub-rede de gerenciamento; o tráfego é segmentado por VLANs e regras de firewall restritivas.
- **Audit Logs Completos**: Cada clique de ligar/desligar, recriação de máquina ou alteração de domínio gera um evento assinado com IP e identificador de usuário na tabela `audit_logs`.

---

## 👨‍💻 Autor & Mantenedores

Desenvolvido e mantido por **[LinkjohnDEV](https://github.com/LinkjohnDEV)** e equipe de infraestrutura **STARCORE DC**.

<div align="center">
  <sub>© 2026 Zorion Host. Todos os direitos reservados. Feito com paixão pela nuvem brasileira. 🇧🇷</sub>
</div>
