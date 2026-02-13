🎓 Sistema Acadêmico (CRUD Java + JDBC)
Este é um sistema de gerenciamento escolar desenvolvido em Java que utiliza JDBC para conexão com banco de dados PostgreSQL. O projeto implementa um CRUD (Create, Read, Update, Delete) completo para gerenciar Pessoas, Alunos, Professores, Disciplinas e Notas.

🚀 Funcionalidades
O sistema funciona via terminal (Console) e permite:

Gerenciar Pessoas: Cadastro genérico com herança para Alunos e Professores.

Gerenciar Alunos: Cadastro completo com matrícula e dados do responsável.

Gerenciar Professores: Cadastro com formação e salário.

Gerenciar Disciplinas: Controle de nome, carga horária e ementa.

Lançar Notas: Associação de notas bimestrais a um Aluno e uma Disciplina.

Relatórios: Listagem de todos os registros cadastrados.

🛠️ Tecnologias Utilizadas
Java (JDK 8+): Linguagem principal.

PostgreSQL: Banco de dados relacional.

JDBC (Java Database Connectivity): Para comunicação entre Java e SQL.

Padrão DAO (Data Access Object): Para separar a lógica de negócios do acesso a dados.

Padrão Singleton/Factory: Para gerenciamento da conexão (ConnectionFactory).

⚙️ Configuração do Banco de Dados
Para rodar o projeto, você precisa criar o banco de dados escola e rodar o script abaixo para criar as tabelas e sequências corretamente:

--TABELAS--------------------------------------------------------------------------------------------------------------------------------
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
-------------------------------------------------------------------------------------------------------------------------------------------

⚠️ Importante: Correção de Sequências
Se você inseriu dados manualmente ou teve erros de ID duplicado, execute estes comandos para sincronizar as sequências do banco:

SELECT setval('public.pessoa_id_seq', (SELECT MAX(id) FROM pessoa));
SELECT setval('public.disciplina_id_seq', (SELECT MAX(id) FROM disciplina));
SELECT setval('public.nota_id_seq', (SELECT MAX(id) FROM nota));

▶️ Como Rodar
Clone este repositório.

Importe o projeto na sua IDE favorita (Eclipse, IntelliJ, NetBeans).

Verifique a classe ConnectionFactory e ajuste o usuário/senha do seu banco de dados local.

Execute a classe Main.java.

Siga as instruções no menu do console.

Desenvolvido por Francieverton Projeto de estudo sobre Orientação a Objetos e Banco de Dados.
