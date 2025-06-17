# 📍 CheckIN – Plataforma de Descoberta Social com IA

**CheckIN** é uma aplicação web e mobile que une descoberta inteligente de lugares com conexão social baseada em contexto. Utilizando inteligência artificial generativa, o sistema sugere experiências reais de lazer com base na localização, preferências, presença de amigos e estilo de rolê.

Desenvolvido por uma equipe multidisciplinar, o projeto busca responder de forma prática às perguntas:
- _“Pra onde eu vou sair hoje?”_
- _“Com quem?”_

---

## 🎯 Objetivos do Projeto

- Reduzir o tempo de decisão para sair
- Ajudar usuários a encontrar lugares ativos, seguros e com boa vibe
- Permitir encontros sociais mais naturais e conectados ao momento
- Oferecer suporte a estabelecimentos na organização de fluxo, reservas e divulgação
- Proporcionar uma experiência fluida, leve e personalizada com ajuda da IA

---

## 🧱 Estrutura dos Repositórios

Este projeto está dividido em três repositórios principais:

| Repositório        | Finalidade                                           |
|--------------------|------------------------------------------------------|
| [`checkin-docs`](https://github.com/gabrielalimasotero/checkin-docs)     | Documentação geral, design, requisitos e arquitetura |
| [`checkin-front`](https://github.com/CHMFC/checkin-front)                | Aplicação mobile (React Native + Expo)               |
| [`checkin-back`](https://github.com/CHMFC/checkin-back)                  | API backend (FastAPI + PostgreSQL)                   |

---

## 🧩 Arquitetura e Design

A aplicação é estruturada em uma arquitetura cliente-servidor, com integração de IA e foco em modularidade.

📌 Diagramas disponíveis em:  
[`checkin-docs/04-arquitetura/c4model`](https://github.com/gabrielalimasotero/checkin-docs/tree/main/04-arquitetura/c4model)

- **Frontend:** Aplicativo em React Native via Expo
- **Backend:** API RESTful com FastAPI, autenticação JWT e banco PostgreSQL
- **Design System:** Identidade visual baseada em tons de pele (inclusão e calor humano)
- **IA:** Geração de sugestões com base em comportamento e contexto

---

## 📚 Para Navegar na Documentação

A documentação completa está em [`checkin-docs`](https://github.com/gabrielalimasotero/checkin-docs), organizada por tema:

01-visao-geral/ → Proposta, problema, personas, diferencial
02-requisitos/ → Histórias de usuário, critérios, backlog
03-design/ → Identidade visual, wireframes, user-flow
04-arquitetura/ → Frontend, backend, modelo de dados, C4
05-planejamento/ → Roadmap, sprints, organização da equipe
06-testes-validacao/ → Estratégias de validação, métricas
99-anexos/ → Referências e materiais complementares


---

## 🔗 Recursos Importantes

- 🗂️ [Board de Tarefas no Trello (Scrumban)](https://trello.com/b/97MLpiuS/checkin-scrumban)
- 🎨 [Protótipos no Figma](#) *(botar link)*
- 📎 [Documentação da API interativa (Swagger)](botar link)

---

## ⚙️ Como Rodar o Projeto Localmente

Consulte o arquivo [BUILD.md](./BUILD.md) para instruções completas de instalação e execução local dos três repositórios.

---

## 🤝 Contribuição

Contribuições externas são bem-vindas, mas passam por curadoria da equipe original.  
Veja as diretrizes em: [CONTRIBUTING.md](./CONTRIBUTING.md)

---

## 👥 Equipe

- Gabriela Lima Sotero *(Líder de Equipe, PO, Designer)*
- Henrique Fontaine *(Arquiteto Técnico e Dev Backend)*
- João Pedro de Albuquerque Maranhão Marinho *(Frontend)*
- João Victor Oliveira Santos *(Backend)*
- Lucas Emmanuel Gomes de Lucena *(Modelagem e Infraestrutura)*
- Lucas Luis de Souza *(Frontend)*

---

## 📄 Licença

Este projeto está licenciado sob a [MIT License](./LICENSE).
