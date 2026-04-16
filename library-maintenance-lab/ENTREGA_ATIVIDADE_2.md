# Documento de Entrega – Atividade 2

## Bugs Corrigidos

### Bug 1 – `LegacyDatabase.countOpenLoansByBook` filtrava por campo errado

**Arquivo:** `src/LegacyDatabase.java` (linha 184)

**Descrição:** O método `countOpenLoansByBook(int bookId)` comparava o parâmetro `bookId` com o campo `userId` do empréstimo, retornando contagens inconsistentes de empréstimos abertos por livro.

**Como reproduzir (antes):**
1. Cadastrar um livro com ID 1 e um usuário com ID 1.
2. Criar um empréstimo do livro 1 para o usuário 1.
3. Chamar `countOpenLoansByBook(1)` — retornava a contagem de empréstimos do **usuário** 1, não do **livro** 1.
4. Se o livro tivesse ID diferente do usuário, a contagem seria incorreta.

**Como reproduzir (depois):**
1. Mesmo cenário acima.
2. `countOpenLoansByBook(1)` agora filtra corretamente pelo campo `bookId`, retornando a contagem real de empréstimos abertos para aquele livro.

**Correção:** Alterado `loan.get("userId")` para `loan.get("bookId")` na comparação.

---

### Bug 2 – `ReportGenerator.generateSimpleReport` com totalizadores incorretos

**Arquivo:** `src/ReportGenerator.java` (linhas 24–35)

**Descrição:** Dois problemas:
- `totalLoans` era calculado como `loans.size() + 1`, inflando o total em 1.
- `closedLoans` incrementava para **todo** empréstimo, independente do status, em vez de contar apenas os com status `CLOSED`.

**Como reproduzir (antes):**
1. Executar `java -cp src Main --report`.
2. Com 1 empréstimo CLOSED no sistema, o relatório exibia:
   - `Loans: 2` (inflado)
   - `Closed loans: 1` (contava todos, não apenas CLOSED)

**Como reproduzir (depois):**
1. Executar `java -cp src Main --report`.
2. O relatório agora exibe corretamente:
   - `Loans: 1`
   - `Closed loans: 1`
   - `Open loans: 0`

**Correção:** Removido o `+ 1` de `totalLoans`; `closedLoans` agora incrementa apenas quando `status.equals("CLOSED")`.

---

### Bug 3 – `LoanManager.returnBook` ignorava empréstimo inexistente silenciosamente

**Arquivo:** `src/LoanManager.java` (linha 93)

**Descrição:** Quando `returnBook` era chamado com um `loanId` inexistente, o método apenas registrava um log e retornava silenciosamente (`return`), sem informar o usuário sobre o erro.

**Como reproduzir (antes):**
1. Chamar `returnBook` com um ID de empréstimo que não existe (ex: 999).
2. O sistema não exibia nenhuma mensagem de erro e finalizava normalmente, dando a falsa impressão de que a devolução ocorreu.

**Como reproduzir (depois):**
1. Chamar `returnBook` com um ID inexistente.
2. O sistema lança `RuntimeException("Loan not found: 999")`, que é capturada pelo handler do menu e exibe uma mensagem de erro ao usuário.

**Correção:** Substituído `return` silencioso por `throw new RuntimeException("Loan not found: " + loanId)`.

---

### Bug 4 – `LoanManager.returnBook` diminuía dívida em vez de aumentar

**Arquivo:** `src/LoanManager.java` (linha 119)

**Descrição:** Quando um empréstimo era devolvido com multa, o cálculo `debt = debt - fine` **subtraía** a multa da dívida do usuário, quando deveria **somar**.

**Como reproduzir (antes):**
1. Criar um empréstimo e devolver com atraso (gerando multa).
2. A dívida do usuário diminuía em vez de aumentar.

**Como reproduzir (depois):**
1. Mesmo cenário: devolver com atraso.
2. A dívida do usuário agora aumenta corretamente pelo valor da multa.

**Correção:** Alterado `debt = debt - fine` para `debt = debt + fine`.

---

### Bug 5 – `BookManager.listBooksSimple` crashava com lista vazia

**Arquivo:** `src/BookManager.java` (linha 62)

**Descrição:** Quando não havia livros cadastrados, o código executava `temp.get(0)` em uma lista vazia, causando `IndexOutOfBoundsException`.

**Como reproduzir (antes):**
1. Iniciar o sistema sem dados seed ou após remover todos os livros.
2. Executar a listagem de livros (opção 5 ou `--list`).
3. O sistema crashava com exceção.

**Como reproduzir (depois):**
1. Mesmo cenário sem livros.
2. O sistema exibe `"No books registered."` e retorna normalmente.

**Correção:** Substituído `System.out.println(temp.get(0))` por mensagem informativa e `return`.

---

### Correção Adicional – Empréstimo duplicado no canal SMS

**Arquivo:** `src/LoanManager.java` (linha 44)

**Descrição:** No método `borrowBook`, quando o canal era `"sms"`, um segundo empréstimo idêntico era criado no banco com nota `"loan-created-sync"`, duplicando registros de empréstimo abertos.

**Correção:** Removido o bloco de código que criava o empréstimo duplicado para o canal SMS.

---

## Funcionalidade Implementada – Histórico de Empréstimos por Usuário

**Arquivos alterados:**
- `src/LoanManager.java` — novo método `listLoanHistoryByUser(int userId)`
- `src/LibrarySystem.java` — nova opção de menu `10 - Loan history by user` e handler `handleLoanHistoryByUser()`

**Descrição:** O usuário pode consultar o histórico completo de empréstimos de um usuário específico, informando o ID do usuário. O sistema exibe:
- Nome do usuário
- Todos os empréstimos (abertos e fechados) com: ID do empréstimo, ID e título do livro, data de empréstimo, data prevista, data de devolução, status e multa
- Total de empréstimos e total de multas acumuladas

**Como usar:**
1. Executar o sistema: `java -cp src Main`
2. Selecionar opção `10 - Loan history by user`
3. Informar o ID do usuário (ex: `3`)
4. O sistema exibe o histórico completo

**Exemplo de saída:**
```
Loan history for user: Carlos (ID 3)
ID | BOOK | TITLE | BORROW | DUE | RETURNED | STATUS | FINE
1 | 4 | Legacy Java | 2026-04-16 | 2026-04-16 +14 | 2026-04-16 | CLOSED | 0.0
Total loans: 1 | Total fines: 0.0
```

---

## Impactos e Riscos Conhecidos

### Impactos

- **countOpenLoansByBook:** A correção pode alterar o comportamento de validações de empréstimo que dependiam da contagem incorreta. Agora a validação de cópias disponíveis por empréstimos abertos funciona corretamente.
- **Relatório:** Os totais agora refletem os valores reais. Relatórios anteriores que foram salvos podem apresentar diferença nos números.
- **returnBook:** Código que dependia do retorno silencioso para empréstimos inexistentes agora receberá exceção. O handler do menu já trata essa exceção adequadamente.
- **Dívida:** Usuários que tiveram devoluções com multa agora terão a dívida acumulada corretamente.
- **SMS duplicado:** O sistema não cria mais empréstimos duplicados no canal SMS. Dados legados com empréstimos duplicados permanecem inalterados.

### Riscos

- As correções alteram comportamento em tempo de execução. Testes manuais foram realizados para os fluxos principais (`--list`, `--report`), mas cenários de borda (ex: dívidas muito altas, muitos empréstimos simultâneos) devem ser validados manualmente.
- A nova funcionalidade de histórico não altera dados existentes — é somente leitura.
- Nenhum framework externo foi adicionado; a estrutura do projeto foi preservada.

---

## Evidências de Execução

> Todas as saídas abaixo foram capturadas em 16/04/2026 após aplicação de todas as correções e da nova funcionalidade.

### 1. Compilação

```
C:\...\library-maintenance-lab> javac src/*.java
(sem erros — exit code 0)
```

### 2. Fluxo principal — Listagem (`--list`)

```
C:\...\library-maintenance-lab> java -cp src Main --list
Starting legacy library system...
Mode: LEGACY
EMAIL: Loan created for user Carlos and book Legacy Java due 2026-04-16 +14
EMAIL: Book returned: Legacy Java by Carlos, fine=0.0
--------------------------------------------
Books
--------------------------------------------
ID | TITLE | AUTHOR | Y | CAT | AV
1 | Clean Code | Robert C. Martin | 2008 | Software | 3
2 | Design Patterns | GoF | 1994 | Software | 2
3 | Refactoring | Martin Fowler | 1999 | Software | 4
4 | Legacy Java | Unknown | 2010 | CS | 2
--------------------------------------------
Users
--------------------------------------------
ID | NAME | EMAIL | TYPE | CITY | STATUS | DEBT
1 | Ana | ana@mail.com | student | Maringa | ACTIVE | 0.0
2 | Bruno | bruno@mail.com | teacher | Maringa | ACTIVE | 0.0
3 | Carlos | carlos@mail.com | student | Maringa | ACTIVE | 0.0
--------------------------------------------
Loans
--------------------------------------------
ID | USER | BOOK | BORROW | DUE | RETURNED | STATUS | FINE
1 | 3 | 4 | 2026-04-16 | 2026-04-16 +14 | 2026-04-16 | CLOSED | 0.0
```

### 3. Fluxo principal — Relatório (`--report`)

```
C:\...\library-maintenance-lab> java -cp src Main --report
Starting legacy library system...
Mode: LEGACY
EMAIL: Loan created for user Carlos and book Legacy Java due 2026-04-16 +14
EMAIL: Book returned: Legacy Java by Carlos, fine=0.0
=== REPORT: Startup Report ===
mode=1 manager=main helper=helper
Books: 4
Users: 3
Loans: 1
Open loans: 0
Closed loans: 1

Books detail:
 - 1 | Clean Code | Robert C. Martin | year=2008 | cat=Software | av=3
 - 2 | Design Patterns | GoF | year=1994 | cat=Software | av=2
 - 3 | Refactoring | Martin Fowler | year=1999 | cat=Software | av=4
 - 4 | Legacy Java | Unknown | year=2010 | cat=CS | av=2

Users with debt:

Recent logs:
 * book-manager-register-4
 * user-added-3
 * user-manager-register-3
 * loan-added-1
 * notify-loan-3-4
 * loan-policy-default-demo
 * loan-created-ok-1
 * fine-book-even-handler
 * notify-return-3-4-CLOSED
 * loan-return-ok-1-demo-handler
```

### 4. Nova funcionalidade — Histórico de empréstimos por usuário (opção 10)

**Roteiro:** Executar `java -cp src Main`, selecionar opção `10`, informar User ID `3` (Carlos).

```
C:\...\library-maintenance-lab> java -cp src Main
Starting legacy library system...
Mode: LEGACY
EMAIL: Loan created for user Carlos and book Legacy Java due 2026-04-16 +14
EMAIL: Book returned: Legacy Java by Carlos, fine=0.0
--------------------------------------------
Legacy University Library
--------------------------------------------
--------------------------------------------
1 - Register book
2 - Register user
3 - Borrow book
4 - Return book
5 - List books
6 - Generate report
7 - List users
8 - List loans
9 - Debug area
10 - Loan history by user
0 - Exit
--------------------------------------------
Select option: 10
User ID: 3
--------------------------------------------
Loan History
--------------------------------------------
Loan history for user: Carlos (ID 3)
ID | BOOK | TITLE | BORROW | DUE | RETURNED | STATUS | FINE
1 | 4 | Legacy Java | 2026-04-16 | 2026-04-16 +14 | 2026-04-16 | CLOSED | 0.0
Total loans: 1 | Total fines: 0.0
--------------------------------------------
Select option: 0
bye
```
