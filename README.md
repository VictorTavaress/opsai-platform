# ShopMinds OpsAI Platform 🚀

Plataforma de Operações de IA para escalabilidade do time de Trust & Safety.

## 📋 O Problema
O time de moderadores humanos enfrenta dois gargalos críticos:
1. **Gargalo de Homologação:** Esforço manual exaustivo para criar dados de teste.
2. **Gargalo de Auditoria:** Impossibilidade de revisar 50k+ decisões diárias, resultando em erros silenciados.

## 🏗️ Arquitetura do Sistema
A solução utiliza uma arquitetura orientada a eventos para garantir que a auditoria e simulação não afetem a performance do marketplace principal.

```mermaid
graph TD
    subgraph "External/Core Systems"
        A[Marketplace Reviews] -->|Events| B(Message Broker - RabbitMQ/Kafka)
    end

    subgraph "OpsAI Platform - Backend"
        B --> C[Auditor Service]
        C --> D[(PostgreSQL - Metadata/Logs)]
        E[Simulation Engine] --> F[Red Teaming API]
    end

    subgraph "AI Layer"
        G[Moderator AI - LLM]
        F --> G
        C --> G
    end

    subgraph "Frontend & Human-in-the-loop"
        H[Moderator Dashboard - React]
        H <--> D
        H --> E
    end

    subgraph "Data & Feedback Loop"
        D --> I[Regression Test Suite]
        I --> E
    end
