<h1 align="center">📚 Biblioteca Escolar — SQL com JOINs</h1>

<p align="center">
  Exercício prático de modelagem e consultas em <b>PostgreSQL</b> usando
  <code>INNER JOIN</code>, <code>LEFT JOIN</code> e <code>FOREIGN KEY</code>.
</p>

---

## 📑 Sumário

- [Parte 1: Montar o banco](#-parte-1-montar-o-banco)
- [Parte 2: Consultas](#-parte-2-consultas)

---

## 🛠️ Parte 1: Montar o banco

### 👤 Tabela de alunos

```sql
CREATE TABLE alunos (
    id   SERIAL PRIMARY KEY,
    nome VARCHAR(50) NOT NULL UNIQUE
);
```

### 📖 Tabela de empréstimos

```sql
CREATE TABLE emprestimos (
    id       SERIAL PRIMARY KEY,
    livro    VARCHAR(100) NOT NULL,
    id_aluno INT,
    FOREIGN KEY (id_aluno) REFERENCES alunos(id)
);
```

### ➕ Inserindo os alunos

```sql
INSERT INTO alunos (nome) VALUES
    ('Joao'),
    ('Marcos'),
    ('Rafael'),
    ('Beatriz'),
    ('Matheus'),
    ('Larissa'),
    ('Gustavo'),
    ('Camila'),
    ('Felipe'),
    ('Julia'),
    ('Lucas'),
    ('Amanda'),
    ('Henrique'),
    ('Sofia'),
    ('Vinicius');
```

### ➕ Inserindo os empréstimos

```sql
INSERT INTO emprestimos (livro, id_aluno) VALUES
    ('O Senhor dos Aneis', 1),
    ('Percy Jackson e o Ladrao de Raios', 2),
    ('O Hobbit', 3),
    ('Jogos Vorazes', 4),
    ('Diario de um Banana', 5),
    ('As Cronicas de Narnia', 6),
    ('A Ilha Perdida', 7),
    ('Alice no Pais das Maravilhas', 8),
    ('Peter Pan', 9),
    ('O Pequeno Principe', 10);
```

---

## 🔍 Parte 2: Consultas

### 1️⃣ Mostrar todos os dados das tabelas

**Alunos**

```sql
SELECT * FROM alunos;
```

<p align="center">
  <img src="![alt text](image.png)" alt="Resultado do SELECT na tabela alunos" width="600">
</p>

**Empréstimos**

```sql
SELECT * FROM emprestimos;
```

<p align="center">
  <img src="![alt text](image-1.png)" alt="Resultado do SELECT na tabela emprestimos" width="600">
</p>

---

### 2️⃣ `INNER JOIN`: nome do aluno e o livro

> Retorna apenas os alunos que possuem empréstimo.

```sql
SELECT alunos.nome, emprestimos.livro
FROM alunos
INNER JOIN emprestimos
    ON alunos.id = emprestimos.id_aluno;
```

<p align="center">
  <img src="![alt text](image-2.png)" alt="Resultado do INNER JOIN entre alunos e emprestimos" width="600">
</p>

---

### 3️⃣ `LEFT JOIN`: todos os alunos e o livro de cada um

> Retorna **todos** os alunos; quem não pegou livro aparece com `NULL`.

```sql
SELECT alunos.nome, emprestimos.livro
FROM alunos
LEFT JOIN emprestimos
    ON alunos.id = emprestimos.id_aluno;
```

<p align="center">
  <img src="![alt text](image-3.png)" alt="Resultado do LEFT JOIN entre alunos e emprestimos" width="600">
</p>

---

### 4️⃣ Quem **nunca** pegou livro

> Usa o `LEFT JOIN` filtrando pelos registros sem correspondência (`IS NULL`).

```sql
SELECT alunos.nome
FROM alunos
LEFT JOIN emprestimos
    ON alunos.id = emprestimos.id_aluno
WHERE emprestimos.id IS NULL;
```

<p align="center">
  <img src="![alt text](image-4.png)" alt="Alunos que nunca pegaram livro" width="600">
</p>

Nesse caso, os alunos que nunca pegaram livro são:

- Lucas
- Amanda
- Henrique
- Sofia
- Vinicius

---

### 5️⃣ Registrar empréstimo para um aluno inexistente (id 50)

> Testa a integridade referencial da `FOREIGN KEY`.

```sql
INSERT INTO emprestimos (livro, id_aluno)
VALUES ('Turma da Monica', 50);
```

❌ **Erro esperado:**

```text
ERROR: insert or update on table "emprestimos" violates foreign key constraint "emprestimos_id_aluno_fkey"
```

<p align="center">
  <img src="![alt text](image-5.png)" alt="Erro de violação de chave estrangeira ao inserir aluno 50" width="600">
</p>

O banco rejeita a inserção porque não existe aluno com `id = 50`, garantindo que nenhum empréstimo fique "órfão".

---

## 💾 Executando tudo

Para reproduzir o banco rapidamente, execute o arquivo [`schema.sql`](schema.sql).

O arquivo criará as tabelas `alunos` e `emprestimos`, adicionará os alunos e cadastrará os empréstimos.

---