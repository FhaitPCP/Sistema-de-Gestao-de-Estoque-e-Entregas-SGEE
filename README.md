# 📦 SGEE - Sistema de Gestão de Estoque e Entregas 🚚

> [!NOTE]
> O **SGEE** é uma plataforma distribuída para a gestão integral de fluxos logísticos comerciais. O sistema orquestra, em tempo real, a entrada e saída de produtos, a emissão de faturas e o acompanhamento de frota, garantindo consistência através de uma arquitetura de microsserviços.

<table>
  <tr>
    <td width="800px">
      <div align="justify">
        Este <b>README.md</b> documenta a infraestrutura e o funcionamento do SGEE. O projeto foi desenhado para resolver o problema de acoplamento em sistemas logísticos tradicionais, separando os domínios de <b>Inventário</b>, <b>Faturação</b> e <b>Entregas</b> em serviços independentes. Adotando boas práticas de engenharia de software, o sistema garante resiliência na comunicação assíncrona, segurança centralizada e interfaces fluídas para Administradores, Colaboradores e Entregadores. O objetivo desta documentação é facilitar a reprodutibilidade, manutenção e colaboração no projeto, promovendo clareza técnica e padronização.
      </div>
    </td>
  </tr> 
</table>

---

## 🚧 Status do Projeto

[![GitHub Workflow Status](https://img.shields.io/github/actions/workflow/status/seu-usuario/sgee/main.yml?branch=main)](#)
[![Versão](https://img.shields.io/badge/Versão-v1.0.0-blue)](#)
![Java](https://img.shields.io/badge/Java-17-007ec6?style=for-the-badge&logo=openjdk&logoColor=white) 
![Spring Boot](https://img.shields.io/badge/Spring_Boot-3.3-007ec6?style=for-the-badge&logo=springboot&logoColor=white) 
![Node.js](https://img.shields.io/badge/Node.js-20-007ec6?style=for-the-badge&logo=nodedotjs&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-Compose-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-16-336791?style=for-the-badge&logo=postgresql&logoColor=white)

---

## 📚 Índice
- [Sobre o Projeto](#-sobre-o-projeto)
- [Funcionalidades Principais](#-funcionalidades-principais)
- [Tecnologias Utilizadas](#-tecnologias-utilizadas)
- [Arquitetura](#-arquitetura)
- [Instalação e Execução](#-instalação-e-execução)
- [Estrutura de Pastas](#-estrutura-de-pastas)
- [Demonstração](#-demonstração)
- [Licença](#-licença)

---

## 📝 Sobre o Projeto
O **SGEE** foi concebido para resolver os desafios operacionais de empresas que necessitam de um controlo rigoroso sobre as suas mercadorias, do armazém até às mãos do cliente final. 

**Problema resolvido:** Falta de rastreabilidade entre o momento em que um produto sai do armazém, a emissão do documento fiscal associado e a efetivação da entrega.
**Onde pode ser utilizado:** Distribuidoras, armazéns logísticos e empresas de comércio eletrónico com frota própria.

O sistema divide-se no acesso para três atores principais:
1. **Administrador:** Focado na visão estratégica (lucros, custos e auditoria global).
2. **Colaborador:** Focado na operação interna (contagem, entrada/saída de produtos e faturação).
3. **Entregador:** Focado na operação externa (baixa de entregas e submissão de comprovativos).

---

## ✨ Funcionalidades Principais
- 🔐 **Autenticação e Autorização:** Gateway centralizado com Spring Security e tokens JWT para controlo de acessos (Admin, Colaborador, Entregador).
- 📦 **Gestão de Estoque:** Registo de entradas, saídas, auditorias de contagem física e alertas de stock mínimo.
- 🧾 **Faturação Integrada:** Cálculo automatizado de impostos e emissão de notas fiscais de compra e venda.
- 🚚 **Logística de Entregas:** Agendamento de rotas, atualizações de estado em tempo real e captura de comprovativos ou devoluções.
- 📊 **Relatórios e Auditoria:** Levantamento de custos, lucros e movimentações agregadas.

---

## 🛠 Tecnologias Utilizadas

### 💻 Front-end
* **Runtime/Ambiente:** Node.js v20
* **Framework Web:** React.js / Vite (Recomendado para SPA)
* **Comunicação:** Axios ou Fetch API para consumo REST

### 🖥️ Back-end (Microsserviços)
* **Linguagem:** Java 17 (JDK)
* **Framework Principal:** Spring Boot 3.x
* **Segurança e Roteamento:** Spring Cloud Gateway, Spring Security
* **Bases de Dados:** PostgreSQL (Padrão Database-per-Service)
* **Mensageria Assíncrona:** RabbitMQ (Para comunicação entre Faturação e Entregas)

### ⚙️ Infraestrutura & DevOps
* **Containerização:** Docker e Docker Compose

---

## 🏗 Arquitetura

O SGEE adota uma **Arquitetura de Microsserviços** isolada por domínio de negócio, comunicando através de uma infraestrutura gerida por Docker.

* **API Gateway:** Ponto único de entrada. Interceta requisições do Front-end em Node.js, valida o token JWT e faz o roteamento.
* **Microsserviço de Estoque:** Gere as entidades `Produto` e `Movimentacao`.
* **Microsserviço de Faturação:** Gere as entidades `NotaFiscal` e `ItemNota`.
* **Microsserviço de Entregas:** Gere as entidades `Entrega` e `Comprovativo`.
* **Broker de Mensagens:** Assegura que, quando a Faturação conclui uma venda, um evento é publicado na fila para que o serviço de Entregas crie o agendamento sem criar um acoplamento síncrono.

---

## 🔧 Instalação e Execução

### Pré-requisitos
* **Java JDK 17+**
* **Node.js v20+**
* **Docker e Docker Compose** (Crucial para executar as 3 bases de dados e o broker de mensagens simultaneamente).

### 🔑 Variáveis de Ambiente (Exemplo local)

Configuração no `.env` da raiz do projeto (utilizado pelo `docker-compose`):

```env
POSTGRES_USER=sgee_admin
POSTGRES_PASSWORD=sgee_secure_pass
RABBITMQ_DEFAULT_USER=rabbit_admin
RABBITMQ_DEFAULT_PASS=rabbit_pass
JWT_SECRET=super_chave_secreta_para_geracao_de_tokens_jwt
```

---

## Execução Local Completa com Docker Compose
Esta é a forma recomendada de subir o sistema inteiro de uma só vez.

* Clone o repositório:
git clone [https://github.com/seu-usuario/sgee.git](https://github.com/seu-usuario/sgee.git)]
cd sgee

## 📂 Estrutura de Pastas
A organização física dos diretórios respeita o isolamento lógico da arquitetura de microsserviços:

```.
├── /frontend                    # 📁 Aplicação cliente (Node.js)
│   ├── /src
│   │   ├── /pages               # Telas de interface (Admin, Colaborador, Entregador)
│   │   └── /services            # Camada de comunicação via API Rest (Axios/Fetch)
│
├── /backend                     # 📁 Ecossistema de microsserviços (Spring Boot)
│   ├── /api-gateway             # Roteamento e camada de segurança (Porta 8080)
│   ├── /ms-estoque              # Domínio de inventário e movimentações (Porta 8081)
│   ├── /ms-faturacao            # Domínio financeiro e cálculo de impostos (Porta 8082)
│   └── /ms-entregas             # Domínio logístico e comprovantes (Porta 8083)
│
├── docker-compose.yml           # 🐳 Arquivo de orquestração de contêineres e redes
├── .env                         # 🔑 Variáveis de ambiente da infraestrutura
└── README.md                    # 📘 Documentação central do projeto
```
