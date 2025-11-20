# Projeto-SQL-Sistema-de-Gest-o-de-Terrenos-e-Propriet-rios
Este trabalho apresenta o desenvolvimento de um projeto SQL voltado para a criação de um sistema simples de gerenciamento de proprietários e terrenos. Foram elaborados scripts para criação das tabelas, inserção de dados e consultas, aplicando conceitos fundamentais de Banco de Dados.
Curso: Tecnologia em Análise e Desenvolvimento de Sistemas
Disciplina: Banco de Dados / SQL
Trabalho: Desenvolvimento de Estruturas e Consultas SQL
Aluna: Valdineia Cordeiro Reinaldo
Ano: 2025
README.md
# 📌 Projeto SQL – Sistema de Gestão de Terrenos e Proprietários

Este repositório contém os scripts SQL desenvolvidos para a atividade prática da disciplina, incluindo criação de tabelas, inserção de dados e consultas.  
O objetivo é demonstrar o uso de comandos fundamentais de **DDL (Data Definition Language)** e **DML (Data Manipulation Language)**.

---

## 📁 Estrutura do Projeto



projeto-sql/
│
├── scripts/
│ ├── 01_create_tables.sql
│ ├── 02_inserts.sql
│ └── 03_queries.sql
│
└── README.md


---

## 🗂️ Descrição dos Arquivos

### **01_create_tables.sql**
Contém os comandos para criar as tabelas principais do banco:

- `proprietario`
- `terreno`

Inclui:
- Chave primária
- Relação 1:N entre proprietário e terrenos
- Campos obrigatórios

### **02_inserts.sql**
Contém os dados de exemplo para popular as tabelas e permitir testes de consultas.

### **03_queries.sql**
Contém consultas SQL para:
- Listar terrenos com seus proprietários  
- Calcular área total por proprietário  
- Buscar proprietário específico  
- Outras consultas úteis

---

## 🛠️ Como Executar os Scripts

Você pode rodar os scripts em qualquer banco relacional, como:

- PostgreSQL
- MySQL / MariaDB
- Oracle
- SQL Server
- SQLite (com pequenas adaptações)

### ✔ Recomendado: PostgreSQL

### 1. Criar o banco:
```sql
CREATE DATABASE terrenos_db;

2. Acessar o banco:
psql -d terrenos_db

3. Executar os scripts na ordem sugerida:
\i scripts/01_create_tables.sql;
\i scripts/02_inserts.sql;
\i scripts/03_queries.sql;

📊 Modelo Lógico Simplificado
proprietario( id PK, nome, cpf )
        │
        └───< terreno( id PK, descricao, area_m2, proprietario_id FK )


Relacionamento:

Um proprietário → muitos terrenos

📝 Scripts SQL (código completo)
01_create_tables.sql
CREATE TABLE proprietario (
  id SERIAL PRIMARY KEY,
  nome VARCHAR(100) NOT NULL,
  cpf VARCHAR(14)
);

CREATE TABLE terreno (
  id SERIAL PRIMARY KEY,
  descricao VARCHAR(255) NOT NULL,
  area_m2 NUMERIC(10,2),
  proprietario_id INT NOT NULL REFERENCES proprietario(id)
);

02_inserts.sql
INSERT INTO proprietario (nome, cpf) VALUES
('João Silva','123.456.789-00'),
('Maria Souza','987.654.321-10'),
('Carlos Mendes','111.222.333-44');

INSERT INTO terreno (descricao, area_m2, proprietario_id) VALUES
('Sítio Primavera', 1500.00, 1),
('Chácara Bela Vista', 2800.50, 2),
('Terreno Urbano', 450.75, 3),
('Terreno Rural Norte', 3800.00, 1);

03_queries.sql
-- 1. Listar todos os terrenos com o nome do proprietário
SELECT t.id, t.descricao, t.area_m2, p.nome AS proprietario
FROM terreno t
JOIN proprietario p ON p.id = t.proprietario_id;

-- 2. Somar área total por proprietário
SELECT p.nome, SUM(t.area_m2) AS area_total
FROM proprietario p
JOIN terreno t ON t.proprietario_id = p.id
GROUP BY p.nome
ORDER BY area_total DESC;

-- 3. Buscar terrenos de um proprietário específico
SELECT t.*
FROM terreno t
JOIN proprietario p ON p.id = t.proprietario_id
WHERE p.nome ILIKE '%Maria%';

-- 4. Mostrar proprietários que possuem mais de um terreno
SELECT p.nome, COUNT(t.id) AS total_terrenos
FROM proprietario p
JOIN terreno t ON t.proprietario_id = p.id
GROUP BY p.nome
HAVING COUNT(t.id) > 1;

-- 5. Listar todos os proprietários mesmo que não tenham terrenos
SELECT p.nome, t.descricao
FROM proprietario p
LEFT JOIN terreno t ON t.proprietario_id = p.id;
