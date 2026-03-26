# Avaliação — Engenharia de Software
**Sistema Integrado de Gestão de Farmácia — MVP Definido pelo Estudante**

Aluno: *Gustavo Oliveira Luna Nascimento*  
RA: *24000461*  
Data: *26/03/2026*  

---

Meu MVP cobre o processo de venda de produtos em uma unidade da farmácia, desde a identificação ou cadastro do cliente até a finalização da venda e emissão do comprovante, incluindo a verificação e atualização do estoque.”

Dentro do MVP
O MVP inclui as funcionalidades essenciais para o funcionamento básico da farmácia no dia a dia:
Identificação de cliente (com opção de cadastro rápido)
Consulta de produtos (por nome, código ou fabricante)
Verificação de estoque em tempo real
Registro de venda à vista
Registro de venda a prazo (com geração automática de conta a receber)
Atualização automática do estoque após a venda
Emissão de comprovante de venda
Tratamento de exceções (produto inexistente, estoque insuficiente, cliente não cadastrado)

Fora do MVP
Para manter o escopo enxuto, as seguintes funcionalidades ficaram de fora:
Gestão completa de compras e fornecedores
Controle de contas a pagar
Relatórios gerenciais e indicadores
Controle avançado de permissões de usuários
Integração entre múltiplas unidades
Alertas de estoque mínimo
Gestão de produtos (cadastro/edição por gerente)
Transferência de estoque entre unidades

Justificativa das escolhas
A escolha do escopo foi baseada na priorização do processo mais crítico da farmácia: a venda. Esse é o fluxo central do negócio, pois impacta diretamente o faturamento e o atendimento ao cliente.
Ao focar nesse processo, o MVP garante:
Funcionamento básico da operação da farmácia
Redução de erros manuais no registro de vendas
Controle mínimo de estoque
Registro inicial de dados financeiros (contas a receber)
As funcionalidades excluídas, embora importantes, foram consideradas secundárias para o primeiro momento, pois podem ser adicionadas posteriormente sem comprometer a operação básica do sistema.

---

# 2. Regras de Negócio (mínimo: 5)

RN01 — Venda condicionada ao estoque
Uma venda só pode ser concluída se houver quantidade suficiente do produto no estoque da unidade. Caso contrário, o sistema deve impedir a operação.

RN02 — Atualização automática de estoque
Toda venda realizada deve atualizar automaticamente o estoque, reduzindo a quantidade do produto vendido.

RN03 — Cadastro obrigatório para vendas a prazo
Vendas a prazo só podem ser realizadas para clientes previamente cadastrados no sistema.

RN04 — Geração de conta a receber em vendas a prazo
Toda venda a prazo deve gerar automaticamente um registro em contas a receber, com status inicial “Aberta” e data de vencimento definida.

RN05 — Emissão obrigatória de comprovante
Ao final de cada venda, o sistema deve emitir um comprovante contendo os dados da operação, como produtos, quantidades, valores e forma de pagamento.


---

# 3. Requisitos Funcionais (mínimo: 8)

**RF01 — Identificar cliente
O sistema deve permitir identificar um cliente por meio de nome, CPF ou outro dado cadastral.**

**RF02 — Cadastrar cliente
O sistema deve permitir o cadastro rápido de um cliente caso ele não esteja previamente registrado.**

**RF03 — Consultar produtos
O sistema deve permitir a busca de produtos por nome, código de barras ou fabricante.**

**RF04 — Verificar estoque
O sistema deve verificar automaticamente se há quantidade disponível do produto antes de permitir a venda.**

**RF05 — Registrar venda à vista
O sistema deve permitir o registro de vendas com pagamento imediato.**

**RF06 — Registrar venda a prazo
O sistema deve permitir o registro de vendas a prazo para clientes cadastrados.**

**RF07 — Gerar conta a receber
O sistema deve gerar automaticamente uma conta a receber ao registrar uma venda a prazo.**

**RF08 — Atualizar estoque
O sistema deve atualizar automaticamente o estoque após a conclusão de uma venda.**



---

# 🛡 4. Requisitos Não Funcionais (mínimo: 4)

**RNF01 — Segurança de acesso
O sistema deve garantir que apenas usuários autenticados possam acessar as funcionalidades, por meio de login e senha.**

**RNF02 — Tempo de resposta
O sistema deve responder às operações de consulta e registro de vendas em até 2 segundos.**

**RNF03 — Disponibilidade
O sistema deve estar disponível durante o horário de funcionamento da farmácia, sem interrupções que prejudiquem as vendas.**

**RNF04 — Integridade dos dados
O sistema deve garantir que os dados de vendas e estoque sejam consistentes, evitando perdas ou divergências de informação.**

---

# 5. Casos de Uso (mínimo: 10)
@startuml
left to right direction

actor Atendente
actor Cliente
actor Farmaceutico

rectangle "Sistema Saúde & Vida" {

  (Realizar Venda) as RV
  (Consultar Produto) as CP
  (Verificar Estoque) as VE
  (Identificar Cliente) as IC
  (Cadastrar Cliente) as CC
  (Registrar Venda a Prazo) as VP
  (Gerar Conta a Receber) as CR
  (Emitir Comprovante) as EC
  (Autorizar Medicamento) as AM
  (Calcular Total) as CT

  Atendente --> RV
  Atendente --> IC
  Atendente --> CP

  RV --> CP : <<include>>
  RV --> VE : <<include>>
  RV --> CT : <<include>>
  RV --> EC : <<include>>

  VP --> RV : <<extend>>
  VP --> CR : <<include>>

  CC --> IC : <<extend>>

  AM --> RV : <<extend>>

  Farmaceutico --> AM
}

@enduml

---

# 6. Documentação dos Casos de Uso
Perfeito, vou te entregar os **10 casos de uso completos**, já prontos pra colar no `.md` 👇

---

## **UC01 — Realizar Venda**

**Ator(es):** Atendente
**Descrição:** Registrar a venda de produtos para um cliente.
**Pré-condições:** Sistema ativo e produtos cadastrados.
**Pós-condições:** Venda registrada e estoque atualizado.

### Fluxo Principal

1. Atendente inicia a venda
2. Identifica o cliente
3. Consulta produto
4. Verifica estoque
5. Informa quantidade
6. Sistema calcula total
7. Atendente confirma venda
8. Sistema registra venda
9. Sistema atualiza estoque
10. Sistema emite comprovante

### Fluxos Alternativos / Exceções

* FA01 — Produto sem estoque → sistema exibe erro
* FA02 — Cliente não cadastrado → aciona cadastro

### Relacionamentos

* **Include:** Consultar Produto, Verificar Estoque, Calcular Total, Emitir Comprovante
* **Extend:** Registrar Venda a Prazo, Autorizar Medicamento

### Diagrama de Atividade

```id="uc01"
Início → Identificar cliente → Consultar produto → Verificar estoque
→ [sem estoque?] → sim: erro → fim
→ não → Informar quantidade → Calcular total → Confirmar
→ Registrar venda → Atualizar estoque → Emitir comprovante → Fim
```

---
<img width="174" height="742" alt="image" src="https://github.com/user-attachments/assets/1cee483d-044d-4643-9ecb-ed70903a47b9" />

## **UC02 — Consultar Produto**

**Ator(es):** Atendente
**Descrição:** Buscar produtos no sistema.
**Pré-condições:** Produto cadastrado.
**Pós-condições:** Produto exibido.

### Fluxo Principal

1. Informar termo de busca
2. Sistema pesquisa
3. Sistema exibe resultados

### Fluxos Alternativos / Exceções

* FA01 — Produto não encontrado

### Relacionamentos

* **Include:** —
* **Extend:** —

### Diagrama

```id="uc02"
Início → Buscar produto → [encontrado?]
→ não: erro → fim
→ sim: exibir produto → fim
```

---
<img width="274" height="319" alt="image" src="https://github.com/user-attachments/assets/0cd75714-5da8-41b0-8a44-4098dd1ce473" />

## **UC03 — Verificar Estoque**

**Ator(es):** Sistema
**Descrição:** Validar disponibilidade de produto.
**Pré-condições:** Produto selecionado.
**Pós-condições:** Retorno de disponibilidade.

### Fluxo Principal

1. Sistema consulta estoque
2. Sistema retorna quantidade

### Fluxos Alternativos / Exceções

* FA01 — Estoque insuficiente

### Relacionamentos

* **Include:** —
* **Extend:** —

### Diagrama

```id="uc03"
Início → Consultar estoque → [suficiente?]
→ não: retorno negativo → fim
→ sim: retorno positivo → fim
```

---
<img width="341" height="253" alt="image" src="https://github.com/user-attachments/assets/99915da5-100d-4df7-b3d8-12e04b57ed90" />

## **UC04 — Identificar Cliente**

**Ator(es):** Atendente
**Descrição:** Identificar cliente no sistema.
**Pré-condições:** —
**Pós-condições:** Cliente identificado.

### Fluxo Principal

1. Informar dados do cliente
2. Sistema busca cliente
3. Sistema exibe dados

### Fluxos Alternativos / Exceções

* FA01 — Cliente não encontrado

### Relacionamentos

* **Include:** —
* **Extend:** Cadastrar Cliente

### Diagrama

```id="uc04"
Início → Informar dados → Buscar cliente
→ [existe?]
→ não: cadastrar → fim
→ sim: exibir → fim
```

---
<img width="327" height="307" alt="image" src="https://github.com/user-attachments/assets/6b7c853f-5217-4da2-86c6-353ec08377d0" />

## **UC05 — Cadastrar Cliente**

**Ator(es):** Atendente
**Descrição:** Registrar novo cliente.
**Pré-condições:** Cliente inexistente.
**Pós-condições:** Cliente cadastrado.

### Fluxo Principal

1. Inserir dados
2. Sistema valida
3. Sistema salva

### Fluxos Alternativos / Exceções

* FA01 — Dados inválidos

### Relacionamentos

* **Include:** —
* **Extend:** Identificar Cliente

### Diagrama

```id="uc05"
Início → Inserir dados → Validar
→ [válido?]
→ não: erro → fim
→ sim: salvar → fim
```

---
<img width="244" height="319" alt="image" src="https://github.com/user-attachments/assets/f0fa07a3-f758-40ea-9d82-eaa6b76f9323" />

## **UC06 — Registrar Venda a Prazo**

**Ator(es):** Atendente
**Descrição:** Registrar venda com pagamento futuro.
**Pré-condições:** Cliente cadastrado.
**Pós-condições:** Conta a receber criada.

### Fluxo Principal

1. Selecionar opção a prazo
2. Informar vencimento
3. Confirmar venda
4. Gerar conta a receber

### Fluxos Alternativos / Exceções

* FA01 — Cliente não cadastrado

### Relacionamentos

* **Include:** Gerar Conta a Receber
* **Extend:** Realizar Venda

### Diagrama

```id="uc06"
Início → Escolher prazo → Definir vencimento → Confirmar
→ Gerar conta → fim
```

---
<img width="199" height="297" alt="image" src="https://github.com/user-attachments/assets/4611398d-4545-4bba-958f-b15cc2e8a332" />

## **UC07 — Gerar Conta a Receber**

**Ator(es):** Sistema
**Descrição:** Criar registro financeiro.
**Pré-condições:** Venda a prazo.
**Pós-condições:** Conta registrada.

### Fluxo Principal

1. Receber dados da venda
2. Criar registro
3. Definir status aberta

### Fluxos Alternativos / Exceções

* FA01 — Erro ao salvar

### Relacionamentos

* **Include:** —
* **Extend:** —

### Diagrama

```id="uc07"
Início → Criar conta → Salvar → fim
```

---
<img width="198" height="297" alt="image" src="https://github.com/user-attachments/assets/ea494ad3-4ec1-4f84-8c95-28fd89972072" />

## **UC08 — Emitir Comprovante**

**Ator(es):** Sistema
**Descrição:** Emitir comprovante da venda.
**Pré-condições:** Venda finalizada.
**Pós-condições:** Comprovante gerado.

### Fluxo Principal

1. Gerar dados
2. Formatar
3. Exibir/imprimir

### Fluxos Alternativos / Exceções

* FA01 — Falha na impressão

### Relacionamentos

* **Include:** —
* **Extend:** —

### Diagrama

```id="uc08"
Início → Gerar dados → Exibir comprovante → fim
```

---
<img width="184" height="243" alt="image" src="https://github.com/user-attachments/assets/8884f17b-9d8e-4c33-b790-89cb799433c0" />

## **UC09 — Autorizar Medicamento**

**Ator(es):** Farmacêutico
**Descrição:** Autorizar venda de medicamento controlado.
**Pré-condições:** Produto controlado.
**Pós-condições:** Venda autorizada.

### Fluxo Principal

1. Sistema solicita autorização
2. Farmacêutico valida receita
3. Autoriza venda

### Fluxos Alternativos / Exceções

* FA01 — Receita inválida

### Relacionamentos

* **Include:** —
* **Extend:** Realizar Venda

### Diagrama

```id="uc09"
Início → Validar receita → [válida?]
→ não: negar → fim
→ sim: autorizar → fim
```

---
<img width="258" height="265" alt="image" src="https://github.com/user-attachments/assets/78c1f7c2-37b4-4e8e-90d1-610e0abe7f69" />

## **UC10 — Calcular Total**

**Ator(es):** Sistema
**Descrição:** Calcular valor total da venda.
**Pré-condições:** Produtos selecionados.
**Pós-condições:** Valor total calculado.

### Fluxo Principal

1. Somar itens
2. Aplicar valores
3. Retornar total

### Fluxos Alternativos / Exceções

* FA01 — Erro de cálculo

### Relacionamentos

* **Include:** —
* **Extend:** —

### Diagrama

```id="uc10"
Início → Somar valores → Exibir total → fim
```
<img width="192" height="243" alt="image" src="https://github.com/user-attachments/assets/f189bca4-e64b-455f-b608-723ba8ee0457" />




