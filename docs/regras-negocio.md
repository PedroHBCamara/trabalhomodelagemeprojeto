# Regras de Negócio

## RN01 – Somente professores podem criar atividades

**Descrição:**
Somente usuários com perfil de professor podem criar e publicar atividades para uma turma.

### Impactos

#### User Story

> Como professor, desejo criar atividades para disponibilizar tarefas aos estudantes.

#### BPMN

* Apenas o ator **Professor** executa a tarefa **Criar Atividade**.

#### Arquitetura

* Necessidade de mecanismos de **autenticação** e **autorização por perfil de usuário**.

#### Requisito Não Funcional (RNF)

* **Segurança**.

---

## RN02 – Somente alunos matriculados podem entregar atividades

**Descrição:**
A entrega de atividades é permitida apenas para alunos que estejam matriculados na turma correspondente.

### Impactos

#### User Story

> Como aluno matriculado, desejo entregar atividades para registrar minhas tarefas na disciplina.

#### BPMN

* Antes da entrega, o sistema valida se o aluno pertence à turma.

#### Arquitetura

* Necessidade de validação de vínculo entre aluno e turma.

#### Requisito Não Funcional (RNF)

* **Integridade dos dados**.

---

## RN03 – Atividades possuem prazo de entrega

**Descrição:**
Toda atividade deve possuir uma data limite para submissão.

### Impactos

#### User Story

> Como estudante, desejo visualizar o prazo das atividades para realizar minhas entregas dentro do período permitido.

#### BPMN

* Verificação automática da data de entrega.

#### Arquitetura

* Serviço responsável pela validação de datas e horários.

#### Requisito Não Funcional (RNF)

* **Confiabilidade**.

---

## RN04 – Após o prazo, a entrega fica bloqueada

**Descrição:**
O sistema não deve permitir novas entregas após o encerramento do prazo.

### Impactos

#### User Story

> Como professor, desejo que o sistema bloqueie entregas atrasadas para garantir o cumprimento dos prazos.

#### BPMN

* Decisão automática: "Prazo expirado?".

#### Arquitetura

* Validação automática antes do armazenamento da entrega.

#### Requisito Não Funcional (RNF)

* **Confiabilidade**.

---

## RN05 – Professores podem atribuir notas apenas às suas turmas

**Descrição:**
Um professor só pode corrigir e atribuir notas para atividades das turmas sob sua responsabilidade.

### Impactos

#### User Story

> Como professor, desejo corrigir apenas as atividades das minhas turmas.

#### BPMN

* Verificação da vinculação do professor à turma.

#### Arquitetura

* Controle de autorização baseado em permissões.

#### Requisito Não Funcional (RNF)

* **Segurança**.

---

## RN06 – Cada aluno pode enviar apenas uma versão final da atividade

**Descrição:**
Após a confirmação da entrega, o sistema considera apenas a versão final submetida pelo aluno.

### Impactos

#### User Story

> Como aluno, desejo enviar minha versão final da atividade para avaliação.

#### BPMN

* Registro único da entrega final.

#### Arquitetura

* Controle de versionamento ou bloqueio de reenvio.

#### Requisito Não Funcional (RNF)

* **Consistência dos dados**.

---

## RN07 – Notificações devem ser enviadas quando uma atividade for publicada

**Descrição:**
Os alunos devem ser notificados automaticamente quando uma nova atividade for disponibilizada.

### Impactos

#### User Story

> Como aluno, desejo receber notificações para não perder novos trabalhos e atividades.

#### BPMN

* Evento automático após a publicação da atividade.

#### Arquitetura

* Serviço de notificações desacoplado da aplicação principal.

#### Requisito Não Funcional (RNF)

* **Disponibilidade**.

---

## RN08 – Apenas administradores podem cadastrar novos professores

**Descrição:**
O cadastro de professores é uma operação restrita aos administradores do sistema.

### Impactos

#### User Story

> Como administrador, desejo cadastrar professores para manter o controle dos usuários da plataforma.

#### BPMN

* Apenas o ator **Administrador** executa o processo de cadastro.

#### Arquitetura

* Controle de acesso baseado em papéis (RBAC).

#### Requisito Não Funcional (RNF)

* **Segurança**.
