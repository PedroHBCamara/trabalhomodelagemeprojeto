# Arquitetura de Software

## Visão Geral

O sistema **SmartClass** foi projetado utilizando uma **Arquitetura em Camadas (Layered Architecture)**, visando simplicidade, manutenibilidade, segurança e facilidade de evolução.

Essa abordagem foi escolhida por atender adequadamente às necessidades do domínio educacional, permitindo a separação clara das responsabilidades e facilitando futuras expansões do sistema.

---

## Justificativa da Arquitetura

Após a análise dos requisitos funcionais e das regras de negócio, concluiu-se que uma arquitetura em camadas é a alternativa mais adequada para o MVP do projeto.

### Benefícios

- Separação clara de responsabilidades;
- Facilidade de manutenção e testes;
- Menor acoplamento entre componentes;
- Escalabilidade gradual;
- Curva de aprendizado reduzida para novos desenvolvedores.

---

## Estrutura Arquitetural

```text
┌───────────────────────────────┐
│      Camada de Apresentação   │
│         (Frontend Web)        │
└───────────────┬───────────────┘
                │
                ▼
┌───────────────────────────────┐
│      Camada de Aplicação      │
│   (Controladores e Serviços)  │
└───────────────┬───────────────┘
                │
                ▼
┌───────────────────────────────┐
│      Camada de Negócio        │
│ (Regras e Validações do Domínio)│
└───────────────┬───────────────┘
                │
                ▼
┌───────────────────────────────┐
│    Camada de Persistência     │
│      (Repositórios/DAO)       │
└───────────────┬───────────────┘
                │
                ▼
┌───────────────────────────────┐
│   Banco de Dados (PostgreSQL) │
└───────────────────────────────┘
```

---

## Descrição das Camadas

### Camada de Apresentação

Responsável pela interação com os usuários do sistema. Consome a API REST exposta pela Camada de Aplicação e **não contém lógica de negócio**.

Funcionalidades:

- Login e autenticação via JWT;
- Visualização de turmas e atividades;
- Envio e correção de atividades;
- Consulta de notas e feedbacks;
- Exibição de notificações.

---

### Camada de Aplicação

Responsável por coordenar os fluxos de execução da aplicação. Recebe as requisições do frontend, aciona a Camada de Negócio e retorna respostas formatadas. **Não toma decisões de negócio — apenas orquestra.**

Exemplos:

- Criar atividade (delega validação à Camada de Negócio);
- Enviar atividade (verifica prazo via regra de negócio antes de persistir);
- Registrar nota (aplica permissão de turma via Camada de Negócio);
- Disparar notificação (aciona o módulo de notificações após operação concluída).

---

### Camada de Negócio

Contém as regras de negócio do sistema. **É a única camada que toma decisões sobre o domínio.** Não acessa o banco de dados diretamente — recebe dados da Camada de Aplicação e retorna decisões.

Exemplos:

- RN01 – Apenas professores podem criar atividades;
- RN03 – Atividades possuem prazo de entrega;
- RN04 – Entregas fora do prazo devem ser registradas com marcação de atraso;
- RN05 – Professores podem atribuir notas apenas às suas próprias turmas.

---

### Camada de Persistência

Responsável pelo acesso e manipulação dos dados armazenados. Implementa o padrão **Repository / DAO**, isolando o restante do sistema de detalhes do banco de dados.

Entidades gerenciadas:

- Usuários (Aluno, Professor, Coordenador, Administrador);
- Turmas;
- Atividades;
- Entregas;
- Notas;
- Feedbacks.

---

### Banco de Dados

Armazena todas as informações do sistema de forma persistente utilizando **PostgreSQL**. O modelo é relacional, refletindo os relacionamentos naturais entre as entidades.

Principais entidades:

| Entidade   | Relacionamentos principais              |
|------------|-----------------------------------------|
| Usuário    | Pode ser Aluno, Professor, Coordenador ou Administrador |
| Turma      | Possui Alunos e Atividades              |
| Atividade  | Pertence a uma Turma, gera Entregas     |
| Entrega    | Feita por um Aluno, referencia Atividade|
| Nota       | Atribuída a uma Entrega                 |
| Feedback   | Vinculado a uma Entrega ou Nota         |

---

## Stack Tecnológica

| Camada               | Tecnologia                        | Justificativa                        |
|----------------------|-----------------------------------|--------------------------------------|
| Apresentação         | React / Next.js                   | SPA moderna, SSR opcional            |
| Aplicação / Negócio  | Node.js + Express ou Spring Boot  | API REST, ecossistema maduro         |
| Persistência         | Prisma (ORM) ou Hibernate         | Abstração segura do banco            |
| Banco de Dados       | PostgreSQL                        | Relacional, open-source, robusto     |
| Autenticação         | JWT                               | Stateless, controle por perfil       |

---

# Decisões Arquiteturais

## DA01 – Arquitetura em Camadas

### Decisão

Utilizar uma arquitetura em camadas para organizar os componentes do sistema.

### Justificativa

O projeto possui regras de negócio bem definidas e um escopo de MVP compatível com esse modelo. A arquitetura em camadas é amplamente conhecida pela equipe, reduzindo riscos técnicos.

### Benefícios

- Organização e clareza de responsabilidades;
- Facilidade de manutenção e onboarding;
- Evolução progressiva para microsserviços, se necessário.

---

## DA02 – Autenticação baseada em JWT

### Decisão

Utilizar JSON Web Token (JWT) para autenticação e controle de acesso dos usuários.

### Justificativa

O sistema possui quatro perfis de acesso distintos: Aluno, Professor, Coordenador e Administrador. O JWT permite autenticação stateless, com o perfil embutido no token, sem necessidade de consulta ao banco a cada requisição.

### Benefícios

- Segurança com controle de permissões por perfil;
- Escalabilidade — sem sessão armazenada no servidor;
- Baixo custo de processamento por requisição.

---

## DA03 – Módulo de Notificações como Componente Separado

### Decisão

Implementar o envio de notificações como um **módulo separado dentro da Camada de Aplicação**, com interface bem definida, preparado para evolução futura para serviço independente com fila de mensagens.

### Justificativa

No MVP, as notificações serão disparadas de forma **síncrona** pela Camada de Aplicação (chamada direta ao módulo). A separação em módulo isolado facilita a evolução futura para comunicação **assíncrona via fila** (ex: RabbitMQ, Redis Pub/Sub), sem impacto nas demais camadas.

> ⚠️ O desacoplamento completo com fila de mensagens está **fora do escopo do MVP**, mas a arquitetura do módulo foi projetada para facilitar essa transição.

### Benefícios

- Código de notificação isolado e reutilizável;
- Fácil evolução para e-mail, push ou SMS;
- Caminho claro para adoção de mensageria assíncrona no futuro.

---

## DA04 – Banco de Dados Relacional (PostgreSQL)

### Decisão

Utilizar um banco de dados relacional (PostgreSQL) para persistência de todos os dados do sistema.

### Justificativa

As entidades do SmartClass possuem relacionamentos naturais e bem definidos: Turma agrupa Alunos, Atividades pertencem a Turmas, Entregas referenciam Atividades e geram Notas. Um modelo relacional representa esses vínculos com integridade referencial nativa.

### Benefícios

- Integridade referencial garantida por chaves estrangeiras;
- Suporte a transações ACID;
- Consultas complexas com JOINs eficientes;
- Open-source, maduro e com amplo suporte da comunidade.

---

# Requisitos Não Funcionais Atendidos

| Requisito             | Solução Arquitetural                                        | Referência |
|-----------------------|-------------------------------------------------------------|------------|
| Segurança             | JWT com controle de permissões por perfil                   | DA02       |
| Confiabilidade        | Validação de prazos e regras centralizada na Camada de Negócio | —       |
| Manutenibilidade      | Arquitetura em Camadas com responsabilidades isoladas       | DA01       |
| Escalabilidade        | Módulos desacoplados; evolução para microsserviços possível (fora do escopo do MVP) | DA01, DA03 |
| Integridade dos Dados | PostgreSQL com chaves estrangeiras e transações ACID        | DA04       |
| Extensibilidade       | Módulo de notificações preparado para fila de mensagens     | DA03       |

---

# Conclusão

A arquitetura proposta atende aos requisitos funcionais e não funcionais do SmartClass, fornecendo uma base sólida para o desenvolvimento do MVP e para futuras expansões da plataforma.

A separação em camadas com responsabilidades claras — onde a Camada de Aplicação **orquestra** e a Camada de Negócio **decide** —, combinada ao uso de autenticação JWT, banco de dados relacional PostgreSQL e um módulo de notificações extensível, garante uma solução organizada, segura e de fácil manutenção, adequada ao domínio de ambiente de aula online.
