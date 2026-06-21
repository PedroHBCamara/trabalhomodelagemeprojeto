# Arquitetura de Software

## Visão Geral

O sistema **EduClass** foi projetado utilizando uma **Arquitetura em Camadas (Layered Architecture)**, visando simplicidade, manutenibilidade, segurança e facilidade de evolução.

Essa abordagem foi escolhida por atender adequadamente às necessidades do domínio educacional, permitindo a separação clara das responsabilidades e facilitando futuras expansões do sistema.

---

## Justificativa da Arquitetura

Após a análise dos requisitos funcionais e das regras de negócio, concluiu-se que uma arquitetura em camadas é a alternativa mais adequada para o MVP do projeto.

### Benefícios

* Separação de responsabilidades;
* Facilidade de manutenção;
* Menor acoplamento entre componentes;
* Maior facilidade para testes;
* Escalabilidade gradual;
* Curva de aprendizado reduzida para novos desenvolvedores.

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
│ (Regras e Validações do Domínio)
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
│        Banco de Dados         │
└───────────────────────────────┘
```

---

## Descrição das Camadas

### Camada de Apresentação

Responsável pela interação com os usuários do sistema.

Funcionalidades:

* Login;
* Visualização de turmas;
* Envio de atividades;
* Correção de atividades;
* Consulta de notas;
* Exibição de notificações.

---

### Camada de Aplicação

Responsável por coordenar os fluxos de execução da aplicação.

Exemplos:

* Criar atividade;
* Enviar atividade;
* Corrigir atividade;
* Registrar nota;
* Enviar notificações.

---

### Camada de Negócio

Contém as regras de negócio do sistema.

Exemplos:

* RN01 – Apenas professores podem criar atividades;
* RN03 – Atividades possuem prazo de entrega;
* RN04 – Entregas fora do prazo devem ser registradas como atraso;
* RN05 – Professores podem atribuir notas apenas às suas turmas.

Essa camada garante que todas as operações respeitem as regras definidas pela instituição.

---

### Camada de Persistência

Responsável pelo acesso aos dados armazenados.

Exemplos:

* Usuários;
* Turmas;
* Atividades;
* Entregas;
* Notas;
* Feedbacks.

---

### Banco de Dados

Armazena todas as informações do sistema de forma persistente.

Principais entidades:

* Usuário
* Professor
* Aluno
* Turma
* Atividade
* Entrega
* Nota
* Feedback

---

# Decisões Arquiteturais

## DA01 – Arquitetura em Camadas

### Decisão

Utilizar uma arquitetura em camadas para organizar os componentes do sistema.

### Justificativa

O projeto possui regras de negócio bem definidas e um escopo compatível com esse modelo arquitetural.

### Benefícios

* Organização;
* Facilidade de manutenção;
* Evolução simplificada.

---

## DA02 – Autenticação baseada em JWT

### Decisão

Utilizar JSON Web Token (JWT) para autenticação dos usuários.

### Justificativa

O sistema possui diferentes perfis de acesso:

* Aluno;
* Professor;
* Coordenador;
* Administrador.

O JWT permite autenticação segura e controle de acesso baseado em permissões.

### Benefícios

* Segurança;
* Escalabilidade;
* Baixo custo de processamento.

---

## DA03 – Serviço de Notificações Desacoplado

### Decisão

Implementar o envio de notificações como um serviço independente.

### Justificativa

Diversas funcionalidades dependem de notificações:

* Envio de atividade;
* Publicação de atividade;
* Disponibilização de nota.

Separar essa responsabilidade reduz o acoplamento entre componentes.

### Benefícios

* Facilidade de manutenção;
* Reutilização;
* Evolução futura para e-mail, push ou SMS.

---

# Requisitos Não Funcionais Atendidos

| Requisito             | Solução Arquitetural                    |
| --------------------- | --------------------------------------- |
| Segurança             | JWT e controle de permissões            |
| Confiabilidade        | Validação de prazos e regras de negócio |
| Manutenibilidade      | Arquitetura em Camadas                  |
| Escalabilidade        | Serviços desacoplados                   |
| Integridade dos Dados | Persistência centralizada               |

---

# Conclusão

A arquitetura proposta atende aos requisitos funcionais e não funcionais do EduClass, fornecendo uma base sólida para o desenvolvimento do MVP e para futuras expansões da plataforma. A separação em camadas, associada ao uso de autenticação JWT e serviços desacoplados, garante uma solução organizada, segura e de fácil manutenção.
