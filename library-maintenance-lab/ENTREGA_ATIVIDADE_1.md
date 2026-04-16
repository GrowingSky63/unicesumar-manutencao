# Entrega – Atividade 1: Análise de Código e Manutenção Preventiva

## 1. Problemas de Manutenibilidade Identificados (10+)

| #  | Arquivo | Classe/Método | Code Smell | Classificação | Descrição |
|----|---------|---------------|------------|---------------|-----------|
| 1  | `BookManager.java` | `listBooksSimple()` | Edge Case mal tratado | Robustez / Defeito latente | Acessa `temp.get(0)` quando a lista está vazia, causando `IndexOutOfBoundsException`. |
| 2  | `LegacyDatabase.java` | `countOpenLoansByBook()` | Poor Naming / Lógica inconsistente | Correção / Semântica | O nome indica filtro por livro (`bookId`), mas a implementação filtrava por `userId`, retornando contagens incorretas. |
| 3  | `LegacyDatabase.java` | `clearLogsIfTooBig()` | Magic Numbers | Legibilidade / Manutenibilidade | Limites `500` e `400` são números mágicos sem semântica explícita no código. |
| 4  | `LoanManager.java` | `borrowBook(...)` | Deep Nesting | Complexidade / Legibilidade | 7 níveis de `if/else` aninhados para validações sequenciais tornam o fluxo difícil de ler e modificar. |
| 5  | `ReportGenerator.java` | `generateSimpleReport()` | Magic Number / Cálculo errado | Defeito / Lógica frágil | `totalLoans` inflado com `+1` sem justificativa de negócio. |
| 6  | `ReportGenerator.java` | `generateSimpleReport()` | Calculation Bug | Defeito / Lógica frágil | `closedLoans` incrementava para **todo** empréstimo (aberto ou fechado), não apenas os fechados. |
| 7  | `LoanManager.java` | `returnBook(...)` | Inconsistent Error Handling | Confiabilidade / Manutenibilidade | Quando `loan` é nulo, retornava silenciosamente em vez de falhar explicitamente como nos demais caminhos de erro. |
| 8  | `LibrarySystem.java` | `handleDebugArea()` | Deep Nesting | Complexidade / Legibilidade | Cadeia de `if/else` aninhados em 6 níveis para opções de menu sequenciais. |
| 9  | `LoanManager.java` | `borrowBook(...)` | Long Parameter List | Manutenibilidade / Coesão | Assinatura com 8 parâmetros primitivos aumenta risco de chamada incorreta. |
| 10 | `LibrarySystem.java` | `LibrarySystem` (classe) | God Class | Coesão / SRP | Centraliza menu, orquestração, entrada de dados, debug, logs e cenário de demonstração. |
| 11 | `BookManager.java` | `registerBook(...)` | Validation Smell | Robustez | Título em branco é aceito por workaround legado (`title = " "`), contrariando regra de negócio. |
| 12 | `LoanManager.java` | `notificationService` | Tight Coupling | Acoplamento / Testabilidade | Dependência concreta instanciada internamente com `new`, impossibilitando substituição em testes. |
| 13 | `LegacyDatabase.java` | Campos estáticos | Hidden Dependencies | Acoplamento / Testabilidade | Estado global compartilhado por todo o sistema torna testes dependentes de ordem de execução. |

## 2. Classificação dos Code Smells

| Categoria | Smells encontrados |
|-----------|-------------------|
| **Complexidade / Legibilidade** | Deep Nesting (`borrowBook`, `handleDebugArea`), Long Method |
| **Defeito / Lógica frágil** | Calculation Bug (`totalLoans +1`, `closedLoans`), Edge Case (`listBooksSimple`) |
| **Semântica / Naming** | Poor Naming / Lógica inconsistente (`countOpenLoansByBook` filtrava por `userId`) |
| **Manutenibilidade** | Magic Numbers (`clearLogsIfTooBig`), Long Parameter List (`borrowBook`) |
| **Confiabilidade** | Inconsistent Error Handling (`returnBook` retornava silenciosamente) |
| **Coesão / SRP** | God Class (`LibrarySystem`), Validation Smell (`registerBook`) |
| **Acoplamento / Testabilidade** | Tight Coupling (`notificationService`), Hidden Dependencies (estado global) |

## 3. Refatorações Realizadas (7)

### Refatoração 1 – Fix edge case em `BookManager.listBooksSimple()`
- **Arquivo:** `src/BookManager.java`
- **Smell corrigido:** Edge Case mal tratado
- **Antes:** `if (temp.size() == 0) { System.out.println(temp.get(0)); }` — crash com lista vazia.
- **Depois:** Early return com mensagem `"No books registered."` quando não há livros cadastrados.
- **Comportamento preservado:** Listagem normal continua funcionando; cenário de lista vazia agora é tratado sem crash.

### Refatoração 2 – Fix filtro em `LegacyDatabase.countOpenLoansByBook()`
- **Arquivo:** `src/LegacyDatabase.java`
- **Smell corrigido:** Poor Naming / Lógica inconsistente
- **Antes:** Filtrava por `loan.get("userId")` — retornava contagem por usuário, não por livro.
- **Depois:** Filtro corrigido para `loan.get("bookId")`, alinhando implementação com o nome do método.
- **Comportamento preservado:** O método agora retorna o valor correto esperado pelo nome.

### Refatoração 3 – Extrair constantes em `LegacyDatabase.clearLogsIfTooBig()`
- **Arquivo:** `src/LegacyDatabase.java`
- **Smell corrigido:** Magic Numbers
- **Antes:** `if (logs.size() > 500)` e `for (int i = 400; ...)` — valores sem contexto semântico.
- **Depois:** Constantes `LOG_MAX_SIZE = 500` e `LOG_TRIM_KEEP_FROM = 400` com nomes que documentam a intenção.
- **Comportamento preservado:** Lógica de truncamento idêntica, apenas com nomes explícitos.

### Refatoração 4 – Guard clauses em `LoanManager.borrowBook()`
- **Arquivo:** `src/LoanManager.java`
- **Smell corrigido:** Deep Nesting
- **Antes:** 7 níveis de `if/else` aninhados para validações sequenciais (profundidade máxima ~8).
- **Depois:** Cada validação é uma guard clause com `throw` imediato; profundidade máxima reduzida para 2.
- **Comportamento preservado:** Mesmas validações, mesmas mensagens de erro, mesma ordem de verificação.

### Refatoração 5 – Fix cálculos em `ReportGenerator.generateSimpleReport()`
- **Arquivo:** `src/ReportGenerator.java`
- **Smell corrigido:** Calculation Bug / Magic Numbers
- **Antes:** `totalLoans = loans.size() + 1` (inflado); `closedLoans++` executava para todo empréstimo.
- **Depois:** `totalLoans = loans.size()` (correto); `closedLoans++` apenas no bloco `else` (status != OPEN).
- **Comportamento preservado:** Relatório agora exibe valores precisos em vez de inflados.

### Refatoração 6 – Error handling consistente em `LoanManager.returnBook()`
- **Arquivo:** `src/LoanManager.java`
- **Smell corrigido:** Inconsistent Error Handling
- **Antes:** `if (loan == null) { return; }` — retorno silencioso que mascara erros.
- **Depois:** `if (loan == null) { throw new RuntimeException("Loan not found: " + loanId); }` — fail-fast consistente com os demais caminhos.
- **Comportamento preservado:** Fluxo normal (loan existente) inalterado; cenário de loan inexistente agora falha explicitamente.

### Refatoração 7 – Redução de aninhamento em `LibrarySystem.handleDebugArea()`
- **Arquivo:** `src/LibrarySystem.java`
- **Smell corrigido:** Deep Nesting
- **Antes:** Cadeia de `if { } else { if { } else { if { ... } } }` em 6 níveis.
- **Depois:** Estrutura plana `if/else if/else if` sem aninhamento desnecessário.
- **Comportamento preservado:** Todas as opções de debug funcionam exatamente como antes.

## 4. Evidência de Preservação de Comportamento

### 4.1 Execução ANTES das refatorações (código original)

#### Comando: `java Main --report`

```
Starting legacy library system...
Mode: LEGACY
EMAIL: Loan created for user Carlos and book Legacy Java due 2026-04-16 +14
EMAIL: Book returned: Legacy Java by Carlos, fine=0.0
=== REPORT: Startup Report ===
mode=1 manager=main helper=helper
Books: 4
Users: 3
Loans: 2                        <-- BUG: totalLoans inflado com +1
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

#### Comando: `java Main --list`

```
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

---

### 4.2 Execução DEPOIS das refatorações (código refatorado)

#### Comando: `java Main --report`

```
Starting legacy library system...
Mode: LEGACY
EMAIL: Loan created for user Carlos and book Legacy Java due 2026-04-16 +14
EMAIL: Book returned: Legacy Java by Carlos, fine=0.0
=== REPORT: Startup Report ===
mode=1 manager=main helper=helper
Books: 4
Users: 3
Loans: 1                        <-- CORRIGIDO: totalLoans agora é loans.size() real
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

#### Comando: `java Main --list`

```
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

---

### 4.3 Análise Comparativa

| Aspecto | Antes | Depois | Observação |
|---------|-------|--------|------------|
| Compilação (`javac src/*.java`) | OK | OK | Sem erros em ambos |
| Demo scenario (borrow + return) | OK | OK | Fluxo completo preservado |
| Listagem de livros (`--list`) | Idêntica | Idêntica | Nenhuma diferença |
| Listagem de usuários (`--list`) | Idêntica | Idêntica | Nenhuma diferença |
| Listagem de empréstimos (`--list`) | Idêntica | Idêntica | Nenhuma diferença |
| Relatório: `Loans` | **2** (bug: +1) | **1** (correto) | Correção intencional do bug de cálculo |
| Relatório: `Closed loans` | 1 | 1 | Valor correto em ambos (coincidência: apenas 1 loan e é CLOSED) |
| Relatório: demais campos | Idênticos | Idênticos | Books, Users, detail, logs |
| Notificações (EMAIL/SMS) | Idênticas | Idênticas | Mensagens preservadas |
| Logs gerados | Idênticos | Idênticos | Mesma sequência de logs |

### 4.4 Diferenças Intencionais (correção de bugs)

As únicas diferenças entre antes/depois são **correções de bugs** identificados como code smells:

1. **`Loans: 2` → `Loans: 1`** — O valor original era inflado por `loans.size() + 1`. Agora usa `loans.size()` corretamente.
2. **`countOpenLoansByBook`** — Filtro corrigido de `userId` para `bookId`. Sem impacto visível no demo (sem empréstimos abertos ao gerar relatório), mas corrige comportamento para cenários reais.
3. **`listBooksSimple`** — Lista vazia agora exibe mensagem em vez de crash. Sem impacto no demo (há livros cadastrados).
4. **`returnBook` com loan nulo** — Agora lança exceção em vez de retorno silencioso. Sem impacto no demo (loan existe).

O comportamento funcional do sistema permanece o mesmo para todos os fluxos normais.
