# 📐 Requisitos Não-Funcionais do CheckIN

Os requisitos abaixo garantem que o sistema seja confiável, escalável, seguro e utilizável na prática.

---

## 🚀 Desempenho

- O tempo de resposta da API deve ser inferior a **500 ms** para requisições padrão (sugestão, check-in, RSVP).
- O tempo máximo para carregar a home do app (feed de sugestões) deve ser de até **2 segundos** com conexão estável (4G/Wi-Fi).
- O sistema deve suportar pelo menos **1.000 usuários simultâneos** sem degradação perceptível.

---

## 🔐 Segurança

- Os dados do usuário devem ser protegidos por autenticação baseada em **tokens JWT**.
- Todas as requisições sensíveis devem trafegar por **HTTPS**.
- As permissões de acesso devem ser validadas tanto no frontend quanto no backend.
- Informações de geolocalização e presença devem ser compartilhadas **somente com consentimento**.

---

## 📱 Usabilidade

- A interface deve ser intuitiva para pessoas com pouca familiaridade com apps sociais.
- Deve ser possível realizar a ação “Quero sair” com **no máximo 2 toques**.
- A navegação deve seguir os princípios do design acessível (contraste, ícones compreensíveis, texto claro).
- Feedbacks rápidos devem ser fornecidos após ações importantes (ex: check-in realizado, convite enviado).

---

## 📡 Disponibilidade

- O sistema deve manter **disponibilidade de 99,9%** no backend em produção.
- O app deve funcionar em **modo offline parcial**, exibindo histórico recente de lugares e eventos salvos.
- O backend deve ser resiliente a falhas em serviços externos, mantendo funcionalidades essenciais ativas.

---

## 💾 Persistência

- O sistema deve manter o histórico de check-ins por usuário, por no mínimo **6 meses**.
- Os dados dos eventos, convites e avaliações devem ser armazenados com versionamento.

---

## 🌍 Portabilidade

- O frontend deve funcionar em dispositivos **Android 8+ e iOS 13+**.
- O app deve se adaptar a diferentes tamanhos de tela (smartphones e tablets).
- A API deve seguir o padrão **RESTful** e ser consumível por outros sistemas no futuro.

---

## ⚙️ Manutenibilidade

- O código deve seguir boas práticas de organização e versionamento (`main`, `dev`, `feature/*`).
- O backend deve ter documentação automática (Swagger).
- A base de dados deve usar versionamento com migrações (via Alembic).

---

## 📊 Monitoramento e Logs

- O backend deve registrar logs de acesso, falhas e eventos críticos.
- O app deve registrar eventos de uso para avaliação de métricas como:
  - Tempo médio até decisão
  - Cliques em sugestões
  - Retorno à plataforma após check-ins

