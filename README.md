# 🎓 Sistema Acadêmico (CRUD Java + JDBC)

![Java](https://img.shields.io/badge/Java-ED8B00?style=for-the-badge&logo=java&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![JDBC](https://img.shields.io/badge/JDBC-blue?style=for-the-badge)

## 🎯 Por que este projeto existe?

### O Problema e a Dor Resolvida
A gestão de dados escolares exige estruturação e relacionamentos sólidos entre diversas entidades, como alunos, professores, disciplinas e notas. Compreender como essas informações fluem da aplicação para o banco de dados é um pilar fundamental no desenvolvimento de software.

Este projeto foi desenvolvido como uma solução de gerenciamento escolar focada em consolidar o aprendizado prático sobre **Orientação a Objetos e Banco de Dados**. Diferente de sistemas que utilizam frameworks robustos (como Hibernate/JPA) que escondem a complexidade, este sistema utiliza JDBC puro. Ele permite entender "por baixo dos panos" como as conexões são feitas, como as queries SQL são executadas e como aplicar padrões de projeto arquiteturais para manter o código limpo e organizado.

### Para quem é útil?
* **Estudantes e Desenvolvedores** que buscam entender a fundo o funcionamento de persistência de dados em Java usando JDBC e padrões de projeto (DAO, Factory).
* **Instituições ou Usuários** que precisem de um sistema via terminal rápido e direto para gerenciar rotinas acadêmicas simples.

## 🔄 O Fluxo Principal (Como funciona?)

1. O banco de dados relacional é estruturado para suportar o cadastro e a hierarquia de entidades (Pessoas, Alunos, Professores).
2. O usuário interage com o sistema exclusivamente através do terminal (Console), navegando por um menu interativo.
3. As operações de CRUD (Criar, Ler, Atualizar, Deletar) são convertidas pela camada DAO em instruções SQL diretas, garantindo a manipulação segura e organizada das notas, matrículas e disciplinas.

---

## ✨ Funcionalidades Principais

* **Gerenciar Pessoas:** Cadastro genérico utilizando o conceito de herança, servindo de base para Alunos e Professores.
* **Gerenciar Alunos:** Cadastro completo incluindo geração de matrícula e armazenamento de dados do responsável.
* **Gerenciar Professores:** Cadastro focado no corpo docente, registrando informações de formação e salário.
* **Gerenciar Disciplinas:** Controle estrutural das matérias oferecidas, incluindo nome, carga horária e ementa.
* **Lançar Notas:** Sistema de associação de notas bimestrais que conecta diretamente um Aluno a uma Disciplina específica.
* **Relatórios:** Capacidade de listagem e leitura de todos os registros já cadastrados no banco de dados.

---

## 🛠️ Stack de Tecnologias

* **Linguagem principal:** Java (JDK 8+)
* **Banco de Dados:** PostgreSQL (Banco de dados relacional)
* **Comunicação de Dados:** JDBC (Java Database Connectivity) para integração direta entre Java e SQL.
* **Arquitetura e Boas Práticas:** 
  * Padrão **DAO (Data Access Object)**: Para separar a lógica de negócios do acesso a dados.
  * Padrão **Singleton/Factory**: Para gerenciamento eficiente e centralizado da conexão (`ConnectionFactory`).

---

## 🚀 Como Rodar Localmente

Siga o passo a passo abaixo para configurar o banco e executar o sistema acadêmico na sua máquina.

### 1. Clonar e Importar
```bash
# Clone o repositório
git clone https://github.com/Francieverton/Escola.git

Importe o projeto na sua IDE favorita (Eclipse, IntelliJ, NetBeans).

### 2. Configurando o Banco de Dados (Pré-requisito Crítico)
Para que o sistema funcione, você precisa criar o banco de dados chamado `escola` no seu PostgreSQL e preparar as tabelas.

1. Acesse o seu PostgreSQL (via pgAdmin, DBeaver ou terminal) e crie o banco: `CREATE DATABASE escola;`
2. Conecte-se ao banco `escola` e execute o script abaixo para criar as tabelas e relacionamentos:

```sql
CREATE TABLE pessoa (
    id SERIAL PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    cpf VARCHAR(14) UNIQUE NOT NULL,
    email VARCHAR(100),
    telefone VARCHAR(20),
    data_nascimento DATE
);

CREATE TABLE aluno (
    id_pessoa INT PRIMARY KEY REFERENCES pessoa(id) ON DELETE CASCADE,
    matricula VARCHAR(20) UNIQUE,
    data_matricula DATE,
    status VARCHAR(20),
    nome_responsavel VARCHAR(100),
    cpf_responsavel VARCHAR(14),
    telefone_responsavel VARCHAR(20)
);

CREATE TABLE professor (
    id_pessoa INT PRIMARY KEY REFERENCES pessoa(id) ON DELETE CASCADE,
    formacao VARCHAR(100),
    salario DECIMAL(10, 2)
);

CREATE TABLE disciplina (
    id SERIAL PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    carga_horaria INT,
    ementa TEXT
);

CREATE TABLE nota (
    id SERIAL PRIMARY KEY,
    id_aluno INT REFERENCES aluno(id_pessoa) ON DELETE CASCADE,
    id_disciplina INT REFERENCES disciplina(id) ON DELETE CASCADE,
    valor_nota DECIMAL(5, 2),
    bimestre INT
);
```
⚠️ Importante: Correção de Sequências
Se você inserir dados manualmente no banco ou tiver erros de ID duplicado durante os testes, execute os comandos abaixo para sincronizar as sequências do PostgreSQL com os dados atuais:
```sql
SELECT setval('public.pessoa_id_seq', (SELECT MAX(id) FROM pessoa));
SELECT setval('public.disciplina_id_seq', (SELECT MAX(id) FROM disciplina));
SELECT setval('public.nota_id_seq', (SELECT MAX(id) FROM nota));
```

### 3. Configurando a Conexão Java

Antes de rodar, verifique a classe ConnectionFactory no seu projeto Java e ajuste as credenciais (usuário e senha) para corresponderem ao seu banco de dados local PostgreSQL.

### 4. Executando o Sistema
Com o banco configurado e as credenciais ajustadas:

Navegue até a classe principal Main.java e execute-a.

O sistema será iniciado no console da sua IDE. Siga as instruções e navegue pelo menu interativo via terminal!

Desenvolvido por Francieverton — Projeto de estudo sobre Orientação a Objetos e Banco de Dados.
