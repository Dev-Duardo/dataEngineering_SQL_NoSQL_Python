**Base de Dados de Exemplo: Empresa Exemplo Ltda.**

Para ilustrar os comandos SQL, usaremos um conjunto simplificado de tabelas representando uma empresa fictícia:

1.  **`Departamentos`**: Armazena informações sobre os departamentos.

    - `DepartamentoID` (INT, Chave Primária - PK)
    - `NomeDepartamento` (VARCHAR(100), NOT NULL, UNIQUE)
    - `Localizacao` (VARCHAR(50))

2.  **`Funcionarios`**: Armazena informações sobre os funcionários.

    - `FuncionarioID` (INT, PK)
    - `PrimeiroNome` (VARCHAR(100), NOT NULL)
    - `Sobrenome` (VARCHAR(100), NOT NULL)
    - `Email` (VARCHAR(150), UNIQUE)
    - `Telefone` (VARCHAR(20))
    - `DataContratacao` (DATE)
    - `Cargo` (VARCHAR(100))
    - `Salario` (DECIMAL(10, 2))
    - `GerenteID` (INT, Chave Estrangeira - FK -> `Funcionarios`.`FuncionarioID`, pode ser NULL)
    - `DepartamentoID` (INT, FK -> `Departamentos`.`DepartamentoID`, pode ser NULL)

3.  **`Projetos`**: Armazena informações sobre os projetos.

    - `ProjetoID` (INT, PK)
    - `NomeProjeto` (VARCHAR(150), NOT NULL)
    - `DataInicio` (DATE)
    - `DataFimPrevista` (DATE)
    - `Orcamento` (DECIMAL(15, 2))

4.  **`Funcionario_Projetos`**: Tabela de associação para o relacionamento muitos-para-muitos entre Funcionários e Projetos.
    - `FuncionarioID` (INT, PK, FK -> `Funcionarios`.`FuncionarioID`)
    - `ProjetoID` (INT, PK, FK -> `Projetos`.`ProjetoID`)
    - `HorasAlocadasSemana` (INT)

_(Nota: Para exemplos de DML, assumiremos que essas tabelas já foram criadas e populadas com alguns dados.)_

---

**SQL Desmistificado: Um Guia Completo do Básico ao Avançado**

**1. Introdução ao SQL**

- **1.1. O que é SQL (Structured Query Language)?**
  `Structured Query Language`, ou **`SQL`**, é a linguagem de programação padrão universalmente reconhecida para a criação, gerenciamento e consulta de bancos de dados relacionais. Um banco de dados relacional organiza informações em tabelas, que consistem em linhas e colunas, representando diferentes atributos dos dados e as relações entre eles. `SQL` fornece a interface para interagir com esses dados estruturados.
  Apesar de sua sintaxe frequentemente utilizar palavras-chave comuns em inglês, o que pode sugerir simplicidade, `SQL` é uma linguagem extremamente poderosa e versátil. Ela permite desde a recuperação básica de dados até manipulações complexas, análises intrincadas e gerenciamento robusto da estrutura e segurança do banco de dados. Essa dualidade – acessibilidade na sintaxe fundamental e profundidade funcional avançada – é uma característica marcante do `SQL`, tornando-o uma ferramenta indispensável para uma vasta gama de profissionais, incluindo analistas de dados, desenvolvedores de software, administradores de banco de dados e cientistas de dados.

- **1.2. Propósito e Importância no Gerenciamento de Dados**
  O propósito central do `SQL` é servir como ponte de comunicação entre o usuário (ou aplicação) e o Sistema de Gerenciamento de Banco de Dados Relacional (`RDBMS`). Através de suas instruções, o `SQL` permite realizar um conjunto completo de operações essenciais para o ciclo de vida dos dados:

  - **Definição de Dados (`DDL` - Data Definition Language):** Criar, modificar e excluir a própria estrutura do banco de dados, como tabelas e índices.
  - **Manipulação de Dados (`DML` - Data Manipulation Language):** Inserir novos dados, atualizar dados existentes, excluir dados e, fundamentalmente, consultar (pesquisar e recuperar) informações armazenadas.
  - **Controle de Dados (`DCL` - Data Control Language):** Gerenciar permissões de acesso, garantindo que apenas usuários autorizados possam visualizar ou modificar dados específicos.
  - **Controle de Transações (`TCL` - Transaction Control Language):** Gerenciar unidades de trabalho (transações) para garantir a consistência e integridade dos dados durante operações de manipulação.

  Além dessas operações fundamentais, `SQL` se integra facilmente com outras linguagens de programação, como `Java`, `Python` e `C#`, permitindo o desenvolvimento de aplicações complexas e de alta performance que dependem de acesso eficiente a bancos de dados. Dada a prevalência de bancos de dados relacionais em quase todos os setores – desde sistemas financeiros e de comércio eletrônico até aplicações web e científicas – o `SQL` se estabelece como a espinha dorsal da interação com dados estruturados. Sua padronização garante que as habilidades adquiridas sejam amplamente aplicáveis em diferentes plataformas de `RDBMS`, tornando o conhecimento de `SQL` um ativo valioso e essencial para a tomada de decisões informadas e baseadas em dados.

- **1.3. Uma Breve História do SQL**
  A história do `SQL` está intrinsecamente ligada ao nascimento e desenvolvimento do modelo de banco de dados relacional. A jornada começou em 1970, quando Edgar F. (Ted) Codd, um cientista da computação da `IBM`, publicou seu artigo seminal, "A Relational Model of Data for Large Shared Data Banks". Este trabalho introduziu o revolucionário modelo relacional, que propunha organizar dados em tabelas (ou relações) e usar a matemática da teoria dos conjuntos e lógica de predicados para manipulá-los.
  Para explorar e implementar as ideias de Codd, a `IBM` iniciou o projeto de pesquisa System R em 1973. Dentro deste projeto, Donald D. Chamberlin e Raymond F. Boyce foram encarregados de criar uma linguagem de consulta para o modelo relacional. Influenciados pelas linguagens propostas por Codd (álgebra relacional e cálculo relacional), eles buscaram criar algo mais acessível e estruturado. Sua primeira tentativa foi a linguagem `SQUARE` (Specifying Queries as Relational Expressions).
  Posteriormente, adaptaram `SQUARE` para se assemelhar mais à estrutura de uma frase em inglês, visando facilitar o aprendizado e o uso. Essa nova linguagem foi chamada de `SEQUEL` (Structured English Query Language). Devido a uma disputa de marca registrada com uma companhia aérea britânica, o nome foi posteriormente abreviado para `SQL`. Chamberlin e Boyce publicaram artigos influentes sobre `SEQUEL` no final dos anos 1970, detalhando suas capacidades de manipulação (`DML`) e definição de dados (`DDL`).
  Desde então, `SQL` foi adotado por outros fornecedores de bancos de dados e passou por processos de padronização pelo American National Standards Institute (`ANSI`) em 1986 e pela International Organization for Standardization (`ISO`) em 1987. Várias revisões do padrão `SQL` foram lançadas ao longo dos anos (`SQL-89`, `SQL-92`, `SQL:1999`, `SQL:2003`, `SQL:2008`, `SQL:2011`, `SQL:2016`, `SQL:2023`), adicionando novas funcionalidades e refinando a linguagem. Hoje, `SQL` é o padrão de fato da indústria para bancos de dados relacionais, utilizado extensivamente por empresas de todos os tamanhos, incluindo gigantes da tecnologia. Sua origem, como uma resposta prática à necessidade de interagir com o inovador modelo relacional de Codd, solidificou seu lugar como uma das linguagens de programação mais importantes e duradouras da história da computação.

**2. Pilares dos Bancos de Dados Relacionais**

Para compreender plenamente o `SQL`, é essencial entender os conceitos fundamentais do modelo de banco de dados relacional sobre o qual ele opera.

- **2.1. Entendendo o Modelo Relacional**
  Um banco de dados relacional é uma coleção de dados percebida como um conjunto de **tabelas**. O termo "relacional" deriva da teoria matemática das relações. A principal característica é como os **relacionamentos** entre diferentes tabelas são estabelecidos: através de valores comuns. Essa abordagem estruturada permite organizar informações complexas de maneira lógica, facilitando gerenciamento e consulta.

- **2.2. Componentes Essenciais: Tabelas, Colunas e Linhas**

  - **Tabelas (Relações):** Unidades de armazenamento. Organizam dados sobre uma entidade (ex: `Funcionarios`, `Departamentos`). Formato de grade bidimensional.
  - **Colunas (Atributos):** Representam características da entidade (ex: `Nome`, `Salario`). Têm nome único na tabela e um **tipo de dado** (`INT`, `VARCHAR`, `DATE`, etc.).
  - **Linhas (Registros/Tuplas):** Representam uma instância única da entidade (ex: os dados de um funcionário específico). Contêm um valor para cada coluna.

- **2.3. Identificação Única: Chaves Primárias (`Primary Keys`)**
  Para identificar cada linha unicamente, usa-se a **Chave Primária (`PK`)**. É uma coluna (ou conjunto de colunas) com valores que devem ser:

  - **Únicos:** Não podem se repetir na tabela.
  - **Não Nulos (`NOT NULL`):** Devem ter um valor.
    Ex: `FuncionarioID` na tabela `Funcionarios`. Fundamental para identificar registros e criar relacionamentos.

- **2.4. Conectando Dados: Chaves Estrangeiras (`Foreign Keys`)**
  **Chaves Estrangeiras (`FK`)** estabelecem e reforçam relacionamentos entre tabelas. É uma coluna (ou conjunto) em uma tabela "filha" cujos valores correspondem aos valores da chave primária de uma tabela "pai".
  Ex: A coluna `DepartamentoID` na tabela `Funcionarios` é uma `FK` que referencia `DepartamentoID` (PK) na tabela `Departamentos`. Isso liga cada funcionário ao seu departamento.
  `FKs` implementam **integridade referencial**, garantindo que não se possa criar um funcionário para um departamento inexistente, ou excluir um departamento com funcionários associados (dependendo das regras `ON DELETE`/`ON UPDATE`). São a base para combinar dados com `JOIN`.

- **2.5. Definindo Dados: Tipos Comuns (`INT`, `VARCHAR`, `DATE`, etc.)**
  Cada coluna tem um **tipo de dado**, definindo a natureza dos valores e as operações possíveis. A escolha correta impacta:

  - **Integridade:** Garante dados válidos.
  - **Armazenamento:** Otimiza espaço (`TINYINT` vs `BIGINT`).
  - **Desempenho:** Operações mais eficientes em tipos nativos.
    Nomes e detalhes podem variar entre `RDBMS`, mas os tipos comuns são padronizados.

  **Tabela: Resumo dos Tipos de Dados SQL Comuns**

  | Categoria       | Tipo de Dado (Exemplos)           | Descrição Breve                                         | Exemplo de Uso na Criação  | Observações                                                   |
  | :-------------- | :-------------------------------- | :------------------------------------------------------ | :------------------------- | :------------------------------------------------------------ |
  | Numérico Exato  | `INT` / `INTEGER`                 | Número inteiro (geralmente 4 bytes)                     | `idade INT`                | Intervalo típico: -2bi a +2bi.                                |
  |                 | `SMALLINT`                        | Número inteiro pequeno (geralmente 2 bytes)             | `num_dependentes SMALLINT` | Intervalo típico: -32k a +32k.                                |
  |                 | `BIGINT`                          | Número inteiro grande (geralmente 8 bytes)              | `populacao BIGINT`         | Para números muito grandes.                                   |
  |                 | `DECIMAL(p, s)` / `NUMERIC(p, s)` | Número decimal com precisão (p) e escala (s) fixas      | `preco DECIMAL(10, 2)`     | p: total dígitos, s: dígitos decimais. Ideal para monetários. |
  | Numérico Aprox. | `FLOAT(p)` / `REAL`               | Número de ponto flutuante (precisão aproximada)         | `medida FLOAT`             | `REAL` (4 bytes), `FLOAT` (geralmente 8 bytes).               |
  | String (Texto)  | `VARCHAR(n)`                      | String de caracteres de comprimento variável (máximo n) | `nome VARCHAR(100)`        | Comum para nomes, emails. Armazena só o necessário.           |
  |                 | `CHAR(n)`                         | String de caracteres de comprimento fixo (exatamente n) | `sigla_estado CHAR(2)`     | Preenche com espaços. Eficiente para tamanho fixo.            |
  |                 | `TEXT`                            | String de caracteres de comprimento variável longo      | `descricao TEXT`           | Limite depende do `RDBMS`. Para textos longos.                |
  | Data e Hora     | `DATE`                            | Armazena apenas a data (Ano, Mês, Dia)                  | `data_nascimento DATE`     | Formato padrão: 'YYYY-MM-DD'.                                 |
  |                 | `TIME`                            | Armazena apenas a hora (Hora, Minuto, Segundo)          | `hora_reuniao TIME`        |                                                               |
  |                 | `DATETIME` / `TIMESTAMP`          | Armazena data e hora. `TIMESTAMP` pode incluir fuso.    | `data_cadastro TIMESTAMP`  | Implementação/fuso variam.                                    |
  | Booleano        | `BOOLEAN` / `BOOL` / `BIT`        | Valor lógico: Verdadeiro (`TRUE`) ou Falso (`FALSE`)    | `ativo BOOLEAN`            | Implementação varia (`BIT(1)`, `BOOLEAN`, `TINYINT(1)`).      |

**3. O Arsenal de Comandos SQL**

Comandos `SQL` são classificados por função: `DDL`, `DML`, `DCL`, `TCL`. `DQL` (para `SELECT`) é às vezes separado, mas geralmente parte do `DML`.

**Tabela: Categorias de Comandos SQL**

| Categoria | Nome Completo                | Propósito Principal                                      | Comandos Comuns                        | Comportamento de Commit |
| :-------- | :--------------------------- | :------------------------------------------------------- | :------------------------------------- | :---------------------- |
| `DDL`     | Data Definition Language     | Definir/modificar estrutura do BD                        | `CREATE`, `ALTER`, `DROP`, `TRUNCATE`  | Geralmente Auto-Commit  |
| `DML`     | Data Manipulation Language   | Manipular (consultar, inserir, atualizar, excluir) dados | `SELECT`, `INSERT`, `UPDATE`, `DELETE` | Controlado por `TCL`    |
| `DCL`     | Data Control Language        | Controlar acesso e permissões                            | `GRANT`, `REVOKE`                      | Geralmente Auto-Commit  |
| `TCL`     | Transaction Control Language | Gerenciar transações (unidades de trabalho `DML`)        | `COMMIT`, `ROLLBACK`, `SAVEPOINT`      | Controla commit `DML`   |

- **Comportamento Transacional:** `DDL` e `DCL` são tipicamente _auto-commit_ (alterações permanentes e imediatas). `DML` opera dentro de transações controladas por `TCL`, permitindo agrupar operações e confirmá-las (`COMMIT`) ou desfazê-las (`ROLLBACK`) como unidade, garantindo consistência.

- **3.1. DDL: Moldando a Estrutura (`CREATE TABLE`, `ALTER TABLE`, `DROP TABLE`)**
  Comandos para criar, modificar, excluir objetos (tabelas, índices, etc.).

  - **`CREATE TABLE`**: Cria nova tabela. Define nome, colunas (nome, tipo, restrições).

    - _Exemplo Simples:_ Criação da tabela `Departamentos` da nossa base.
      ```sql
      CREATE TABLE Departamentos (
          DepartamentoID INT PRIMARY KEY, -- Define Chave Primária
          NomeDepartamento VARCHAR(100) NOT NULL UNIQUE, -- Não nulo e Único
          Localizacao VARCHAR(50)
      );
      ```
    - _Exemplo Mais Completo:_ Criação da tabela `Funcionarios` com `PK`, `FK`, `NOT NULL`, `UNIQUE`, `DEFAULT`, `CHECK`.

      ```sql
      CREATE TABLE Funcionarios (
          FuncionarioID INT PRIMARY KEY,
          PrimeiroNome VARCHAR(100) NOT NULL,
          Sobrenome VARCHAR(100) NOT NULL,
          Email VARCHAR(150) UNIQUE,
          Telefone VARCHAR(20),
          DataContratacao DATE,
          Cargo VARCHAR(100) DEFAULT 'Não Definido', -- Valor padrão
          Salario DECIMAL(10, 2) CHECK (Salario >= 0), -- Restrição de verificação
          GerenteID INT, -- Será FK para Funcionarios(FuncionarioID)
          DepartamentoID INT, -- Será FK para Departamentos(DepartamentoID)

          -- Define Chave Estrangeira para Departamentos
          FOREIGN KEY (DepartamentoID) REFERENCES Departamentos(DepartamentoID)
              ON DELETE SET NULL  -- Se departamento for excluído, define FK como NULL
              ON UPDATE CASCADE,  -- Se ID do depto mudar, atualiza aqui

          -- Define Chave Estrangeira para Gerente (auto-referência)
          FOREIGN KEY (GerenteID) REFERENCES Funcionarios(FuncionarioID)
              ON DELETE SET NULL -- Se gerente for excluído, define FK como NULL
      );
      ```

  - **`ALTER TABLE`**: Modifica tabela existente (adicionar/remover/modificar colunas/restrições). Sintaxe varia entre `RDBMS`.

    - _Exemplo Simples:_ Adicionar coluna `Status` (ativo/inativo) à tabela `Funcionarios`.
      ```sql
      ALTER TABLE Funcionarios
      ADD COLUMN Status VARCHAR(10) DEFAULT 'Ativo';
      ```
    - _Exemplo Mais Completo:_ Modificar tamanho do `Email`, remover `Telefone`, adicionar restrição `UNIQUE` combinada.

      ````sql
      -- Modificar tamanho da coluna Email (sintaxe varia)
      -- Exemplo PostgreSQL/SQL Server:
      ALTER TABLE Funcionarios ALTER COLUMN Email TYPE VARCHAR(200);
      -- Exemplo MySQL:
      -- ALTER TABLE Funcionarios MODIFY COLUMN Email VARCHAR(200);

          -- Remover coluna Telefone
          ALTER TABLE Funcionarios DROP COLUMN Telefone;

          -- Adicionar restrição UNIQUE combinando PrimeiroNome e Sobrenome
          -- (para evitar homônimos exatos na mesma tabela, exemplo didático)
          ALTER TABLE Funcionarios ADD CONSTRAINT UQ_NomeCompleto UNIQUE (PrimeiroNome, Sobrenome);
          ```

      _(Cuidado ao modificar colunas com dados existentes)._
      ````

  - **`DROP TABLE`**: Exclui tabela permanentemente (estrutura, dados, índices). **Usar com extrema cautela!**

    - _Exemplo:_
      ```sql
      DROP TABLE Funcionarios_Antigos; -- Exclui tabela Funcionarios_Antigos
      ```

  - **`TRUNCATE TABLE`**: Remove _todas_ as linhas rapidamente. Geralmente não pode ser desfeito (`ROLLBACK`). Mais eficiente que `DELETE` para limpar tabelas grandes. Estrutura permanece. \* _Exemplo:_
    `sql
TRUNCATE TABLE LogsTemporarios;
`
    Comandos `DDL` impactam imediatamente a estrutura; cuidado em produção.

- **3.2. DML: Manipulando os Dados (`SELECT`, `INSERT INTO`, `UPDATE`, `DELETE`)**
  Comandos para interagir com os _dados_ dentro das tabelas.

  - **`SELECT`**: Recupera dados. Detalhado na Seção 4.

    - _Exemplo Básico:_
      ```sql
      SELECT FuncionarioID, PrimeiroNome, Salario FROM Funcionarios WHERE DepartamentoID = 1;
      ```

  - **`INSERT INTO`**: Adiciona novas linhas.

    - _Exemplo Simples (Especificando Colunas):_
      ```sql
      INSERT INTO Departamentos (DepartamentoID, NomeDepartamento, Localizacao)
      VALUES (1, 'Recursos Humanos', 'Bloco A');
      ```
    - _Exemplo Mais Completo (Múltiplas Linhas):_
      ```sql
      INSERT INTO Funcionarios (FuncionarioID, PrimeiroNome, Sobrenome, Email, DataContratacao, Cargo, Salario, DepartamentoID) VALUES
      (101, 'Carlos', 'Silva', 'carlos.silva@email.com', '2023-01-15', 'Analista de RH', 5000.00, 1),
      (102, 'Ana', 'Pereira', 'ana.pereira@email.com', '2022-11-01', 'Engenheira de Software', 8000.00, 2), -- Assume Depto 2 (Engenharia) existe
      (103, 'Pedro', 'Santos', 'pedro.santos@email.com', '2023-03-20', 'Designer Gráfico', 6000.00, 3); -- Assume Depto 3 (Marketing) existe
      ```
    - _Exemplo (Inserir a partir de `SELECT`):_
      ```sql
      -- Copia funcionários do RH para uma tabela de auditoria
      INSERT INTO Auditoria_Funcionarios (ID_Func, Nome, Depto, Data_Acao)
      SELECT FuncionarioID, PrimeiroNome || ' ' || Sobrenome, DepartamentoID, CURRENT_TIMESTAMP
      FROM Funcionarios
      WHERE DepartamentoID = 1;
      ```

  - **`UPDATE`**: Modifica valores em linhas existentes. **`WHERE` é crucial!**

    - _Exemplo Simples:_ Atualizar localização do departamento de RH.
      ```sql
      UPDATE Departamentos
      SET Localizacao = 'Bloco B - Andar 3'
      WHERE DepartamentoID = 1;
      ```
    - _Exemplo Mais Completo:_ Dar aumento de 10% e mudar cargo de todos os engenheiros de software do depto 2.
      ```sql
      UPDATE Funcionarios
      SET
          Salario = Salario * 1.10,
          Cargo = 'Engenheiro de Software Sênior'
      WHERE
          DepartamentoID = 2 AND Cargo = 'Engenheiro de Software';
      ```

  - **`DELETE`**: Remove linhas inteiras. **`WHERE` é crucial!** \* _Exemplo Simples:_ Remover funcionário com ID 103.
    `sql
DELETE FROM Funcionarios
WHERE FuncionarioID = 103;
` \* _Exemplo Mais Completo:_ Remover todos os projetos com data de fim prevista anterior a 2024.
    `sql
-- Assumindo tabela Projetos(ProjetoID, ..., DataFimPrevista)
DELETE FROM Projetos
WHERE DataFimPrevista < '2024-01-01';
`
    Operações `DML` geralmente ocorrem dentro de transações (`TCL`), permitindo `COMMIT` ou `ROLLBACK`.

- **3.3. DCL: Controlando o Acesso (`GRANT`, `REVOKE`)**
  Gerencia permissões de usuários/roles.

  - **`GRANT`**: Concede privilégios (`SELECT`, `INSERT`, `UPDATE`, `DELETE`, `EXECUTE`, etc.) em objetos (tabelas, views).

    - _Exemplo Simples:_ Dar permissão de leitura na tabela `Funcionarios` para o usuário `analista_junior`.
      ```sql
      GRANT SELECT ON Funcionarios TO analista_junior;
      ```
    - _Exemplo Mais Completo:_ Criar um papel `gerente_projeto`, dar permissões e atribuir a um usuário.

      ```sql
      -- 1. Criar o papel
      CREATE ROLE gerente_projeto;

      -- 2. Conceder permissões ao papel
      GRANT SELECT ON Funcionarios TO gerente_projeto;
      GRANT SELECT, INSERT, UPDATE ON Projetos TO gerente_projeto;
      GRANT SELECT, INSERT, UPDATE, DELETE ON Funcionario_Projetos TO gerente_projeto;

      -- 3. Atribuir o papel a um usuário (ex: 'maria_lider')
      GRANT gerente_projeto TO maria_lider;
      ```

  - **`REVOKE`**: Remove privilégios previamente concedidos. \* _Exemplo Simples:_ Remover permissão `DELETE` da tabela `Projetos` do papel `gerente_projeto`.
    `sql
REVOKE DELETE ON Projetos FROM gerente_projeto;
`
    `DCL` implementa princípio do menor privilégio, crucial para segurança.

- **3.4. TCL: Gerenciando Transações (`COMMIT`, `ROLLBACK`, `SAVEPOINT`)**
  Lida com transações (sequência de `DML` como unidade lógica) para garantir propriedades `ACID` (Atomicidade, Consistência, Isolamento, Durabilidade).

  - **Transação:** Ou todas as operações são concluídas com sucesso, ou nenhuma é (atômica).
  - **`COMMIT`**: Finaliza transação, tornando alterações permanentes.
    - _Exemplo:_
      ```sql
      START TRANSACTION; -- Ou BEGIN TRANSACTION;
      UPDATE Funcionarios SET Salario = Salario + 500 WHERE FuncionarioID = 101;
      INSERT INTO Log_Alteracoes (Descricao) VALUES ('Aumento para Func 101');
      COMMIT; -- Confirma ambas as operações
      ```
  - **`ROLLBACK`**: Desfaz todas as modificações desde o último `COMMIT`/`ROLLBACK`. Restaura estado consistente.
    - _Exemplo:_
      ```sql
      START TRANSACTION;
      DELETE FROM Funcionario_Projetos WHERE FuncionarioID = 102;
      -- Tenta deletar o funcionário, mas FK pode impedir se ele for gerente
      DELETE FROM Funcionarios WHERE FuncionarioID = 102; -- Suponha que falhe
      -- Se falhou (detectado pela aplicação):
      ROLLBACK; -- Desfaz a remoção de Funcionario_Projetos
      ```
  - **`SAVEPOINT`**: Define pontos de salvamento nomeados dentro de uma transação. `ROLLBACK TO nome_savepoint;` reverte até aquele ponto.
    - _Exemplo:_
      ```sql
      START TRANSACTION;
      UPDATE Projetos SET Orcamento = Orcamento * 1.1 WHERE ProjetoID = 5;
      SAVEPOINT orcamento_atualizado;
      -- Tenta alocar funcionário, mas há um erro
      INSERT INTO Funcionario_Projetos (FuncionarioID, ProjetoID) VALUES (999, 5); -- Funcionario 999 não existe!
      -- Se erro detectado:
      ROLLBACK TO orcamento_atualizado; -- Desfaz o INSERT, mas mantém o UPDATE do orçamento
      -- Pode tentar outra alocação ou finalizar
      COMMIT;
      ```
  - **_`Autocommit`_**: Modo padrão em muitos BDs (cada `DML` é uma transação). Desabilitar (`SET AUTOCOMMIT = 0;`) ou usar `START TRANSACTION` para transações multi-instrução.
    `TCL` é indispensável para integridade e consistência, especialmente em ambientes concorrentes.

**4. Dominando a Consulta: O Comando `SELECT`**

Comando mais usado para extrair informações. Permite selecionar colunas, filtrar linhas, ordenar, agrupar.

- **4.1. Seleção de Colunas: Específicas vs. Todas (`*`)**

  - **Específicas:** Listar nomes das colunas desejadas (`SELECT PrimeiroNome, Email FROM Funcionarios;`). Melhora clareza, desempenho e robustez.
  - **Todas (`*`):** Seleciona todas as colunas (`SELECT * FROM Funcionarios;`). Conveniente para exploração, mas evitar em produção.
  - **Aliases (`AS`):** Renomear colunas no resultado (`SELECT Salario * 12 AS SalarioAnual FROM Funcionarios;`).
  - **Seleção Única (`DISTINCT`):** Remover linhas duplicadas (`SELECT DISTINCT Cargo FROM Funcionarios;`).

- **4.2. Filtrando Linhas com `WHERE`**
  Filtra linhas com base em condições lógicas. Ocorre _antes_ de `GROUP BY`. Crucial para performance (usar índices!).

  **Tabela: Operadores Comuns da Cláusula WHERE**
  _(Conteúdo original mantido, com formatação de código e exemplos adaptados ao schema)_

  | Operador             | Descrição                | Exemplo de Uso                                                                 |
  | :------------------- | :----------------------- | :----------------------------------------------------------------------------- |
  | `=`                  | Igual a                  | `WHERE DepartamentoID = 2;`                                                    |
  | `>`, `<`, `>=`, `<=` | Comparação numérica/data | `WHERE Salario > 75000;` `WHERE DataContratacao < '2023-01-01';`               |
  | `!=` ou `<>`         | Diferente de             | `WHERE DepartamentoID <> 1;`                                                   |
  | `LIKE`               | Comparação de padrões    | `WHERE Sobrenome LIKE 'S%';` `WHERE Email LIKE '%@exemplo.com';`               |
  | `IN`                 | Verifica em lista        | `WHERE DepartamentoID IN (2, 3);` `WHERE Cargo IN ('Analista', 'Engenheiro');` |
  | `BETWEEN`            | Verifica em intervalo    | `WHERE Salario BETWEEN 50000 AND 70000;`                                       |
  | `IS NULL`            | Verifica se é nulo       | `WHERE GerenteID IS NULL;`                                                     |
  | `IS NOT NULL`        | Verifica se não é nulo   | `WHERE Email IS NOT NULL;`                                                     |
  | `AND`                | E lógico                 | `WHERE DepartamentoID = 2 AND Salario > 80000;`                                |
  | `OR`                 | OU lógico                | `WHERE DepartamentoID = 1 OR Cargo = 'Gerente';`                               |
  | `NOT`                | NÃO lógico               | `WHERE NOT Cargo = 'Estagiário';` `WHERE Email NOT LIKE '%@teste.com';`        |

- **4.3. Ordenando Resultados com `ORDER BY` (`ASC`, `DESC`)**
  Classifica o resultado final. Ocorre quase no final da execução lógica.

  - **Sintaxe:** `ORDER BY coluna1 [ASC|DESC], coluna2 [ASC|DESC];`
  - **Direção:** `ASC` (ascendente, padrão), `DESC` (descendente).
  - _Exemplo Simples:_
    ```sql
    -- Funcionários por data de contratação (mais recentes primeiro)
    SELECT PrimeiroNome, Sobrenome, DataContratacao FROM Funcionarios ORDER BY DataContratacao DESC;
    ```
  - _Exemplo Múltiplo:_
    `sql
-- Por Departamento (asc), depois por Salário (desc)
SELECT PrimeiroNome, Sobrenome, DepartamentoID, Salario
FROM Funcionarios
ORDER BY DepartamentoID ASC, Salario DESC;
`
    _(Evitar ordenar por posição da coluna)._ Índices ajudam performance.

- **4.4. Agrupando Dados com `GROUP BY`**
  Agrupa linhas com valores iguais para aplicar funções de agregação. Colapsa múltiplas linhas em uma linha de resumo por grupo.

  - **Sintaxe:** `SELECT col_grupo, AGG_FUNC(...) FROM ... [WHERE ...] GROUP BY col_grupo [HAVING ...];`
  - **Regra Fundamental:** Colunas no `SELECT` que _não_ são agregadas _devem_ estar no `GROUP BY`.

- **4.5. Funções de Agregação (`COUNT`, `SUM`, `AVG`, `MIN`, `MAX`)**
  Operam sobre um conjunto de valores (grupo) e retornam um único valor resumo.

  **Tabela: Funções de Agregação Comuns**
  _(Conteúdo original mantido, com formatação de código e exemplos adaptados)_

  | Função                    | Descrição                       | Tipo de Dado | Exemplo de Uso com `GROUP BY`                                                            |
  | :------------------------ | :------------------------------ | :----------- | :--------------------------------------------------------------------------------------- |
  | `COUNT(*)` / `COUNT(col)` | Contar linhas / não nulos       | Qualquer     | `SELECT Cargo, COUNT(*) FROM Funcionarios GROUP BY Cargo;`                               |
  | `COUNT(DISTINCT col)`     | Contar valores únicos não nulos | Qualquer     | `SELECT COUNT(DISTINCT DepartamentoID) FROM Funcionarios;`                               |
  | `SUM()`                   | Calcular soma                   | Numérico     | `SELECT DepartamentoID, SUM(Salario) FROM Funcionarios GROUP BY DepartamentoID;`         |
  | `AVG()`                   | Calcular média                  | Numérico     | `SELECT Cargo, AVG(Salario) FROM Funcionarios GROUP BY Cargo;`                           |
  | `MIN()`                   | Encontrar mínimo                | Comparável   | `SELECT DepartamentoID, MIN(DataContratacao) FROM Funcionarios GROUP BY DepartamentoID;` |
  | `MAX()`                   | Encontrar máximo                | Comparável   | `SELECT DepartamentoID, MAX(Salario) FROM Funcionarios GROUP BY DepartamentoID;`         |

- **4.6. Filtrando Grupos com `HAVING`**
  Filtra resultados _após_ a agregação (`GROUP BY`), permitindo condições sobre funções agregadas.

  - **Sintaxe:** `... GROUP BY ... HAVING condicao_agregada;`
  - _Exemplo:_ Departamentos com mais de 3 funcionários.
    ```sql
    SELECT D.NomeDepartamento, COUNT(F.FuncionarioID) AS QtdFuncionarios
    FROM Departamentos D
    JOIN Funcionarios F ON D.DepartamentoID = F.DepartamentoID
    GROUP BY D.NomeDepartamento
    HAVING COUNT(F.FuncionarioID) > 3;
    ```

- **4.7. A Diferença Fundamental: `WHERE` vs. `HAVING`**
  Baseada na **ordem lógica de execução**:

  1.  `FROM` / `JOIN`
  2.  **`WHERE`** (Filtra linhas ANTES de agregar)
  3.  `GROUP BY` (Agrupa linhas)
  4.  **`HAVING`** (Filtra grupos DEPOIS de agregar)
  5.  `SELECT` / Funções de Janela
  6.  `DISTINCT`
  7.  `ORDER BY`
  8.  `LIMIT` / `OFFSET`

  - _Exemplo Combinando Tudo:_
    ```sql
    -- Selecionar cargos e a média salarial, apenas para funcionários contratados desde 2023,
    -- mas somente para cargos com mais de 2 funcionários nessa condição
    -- e cuja média salarial seja maior que 6000. Ordenar pela média.
    SELECT
        Cargo,
        AVG(Salario) AS MediaSalarialCargo
    FROM
        Funcionarios
    WHERE
        DataContratacao >= '2023-01-01' -- Filtra linhas ANTES
    GROUP BY
        Cargo
    HAVING
        COUNT(*) > 2                      -- Filtra grupos pela contagem
        AND AVG(Salario) > 6000.00        -- Filtra grupos pela média
    ORDER BY
        MediaSalarialCargo DESC;          -- Ordena resultado final
    ```

**5. Unindo Forças: Operações `JOIN`** _(Renumerado)_

Combina informações de múltiplas tabelas relacionadas (base para normalização).

- **5.1. Por que Unir Tabelas?** Normalização evita redundância. `JOIN` recombina dados para análise.

- **5.2. `INNER JOIN` (ou `JOIN`)**

  - Retorna apenas linhas com correspondência em **ambas** as tabelas. Interseção.
  - _Exemplo:_ Funcionários e nomes de seus departamentos (exclui funcionários sem depto e deptos sem funcionários).
    ```sql
    SELECT F.PrimeiroNome, F.Sobrenome, D.NomeDepartamento
    FROM Funcionarios F
    INNER JOIN Departamentos D ON F.DepartamentoID = D.DepartamentoID;
    ```

- **5.3. `LEFT JOIN` (ou `LEFT OUTER JOIN`)**

  - Retorna **todas** as linhas da tabela da **esquerda** e correspondentes da direita (`NULL` se não houver match). Essencial para enriquecimento sem perder dados.
  - _Exemplo:_ Todos os funcionários e seus departamentos (funcionários sem depto terão `NomeDepartamento` = `NULL`).
    ```sql
    SELECT F.PrimeiroNome, F.Sobrenome, D.NomeDepartamento
    FROM Funcionarios F
    LEFT JOIN Departamentos D ON F.DepartamentoID = D.DepartamentoID;
    ```
  - _Exemplo (Encontrar quem NÃO tem correspondência):_ Funcionários sem departamento.
    ```sql
    SELECT F.PrimeiroNome, F.Sobrenome
    FROM Funcionarios F
    LEFT JOIN Departamentos D ON F.DepartamentoID = D.DepartamentoID
    WHERE D.DepartamentoID IS NULL;
    ```

- **5.4. `RIGHT JOIN` (ou `RIGHT OUTER JOIN`)**

  - Retorna **todas** as linhas da tabela da **direita** e correspondentes da esquerda (`NULL` se não houver match). Menos comum (pode ser reescrito como `LEFT JOIN`).
  - _Exemplo:_ Todos os departamentos e seus funcionários (deptos sem funcionários terão nomes = `NULL`).
    ```sql
    SELECT F.PrimeiroNome, F.Sobrenome, D.NomeDepartamento
    FROM Funcionarios F
    RIGHT JOIN Departamentos D ON F.DepartamentoID = D.DepartamentoID;
    ```

- **5.5. `FULL OUTER JOIN` (ou `FULL JOIN`)**

  - Retorna **todas** as linhas de **ambas** as tabelas (`NULL` onde não há match).
  - _Exemplo:_ Todos funcionários e todos departamentos, mostrando associações, funcionários sem depto e deptos sem funcionários.
    `sql
SELECT F.PrimeiroNome, F.Sobrenome, D.NomeDepartamento
FROM Funcionarios F
FULL OUTER JOIN Departamentos D ON F.DepartamentoID = D.DepartamentoID;
`
    _(Não suportado em todos os RDBMS; pode ser simulado com `LEFT JOIN` + `UNION` + `RIGHT JOIN`)._

- **5.6. `JOIN` com Múltiplas Tabelas:** Encadeamento de `JOIN`s.

  - _Exemplo:_ Funcionários, seus departamentos e projetos alocados.
    ```sql
    SELECT
        F.PrimeiroNome,
        D.NomeDepartamento,
        P.NomeProjeto,
        FP.HorasAlocadasSemana
    FROM
        Funcionarios F
    LEFT JOIN -- Para incluir funcionários mesmo sem departamento
        Departamentos D ON F.DepartamentoID = D.DepartamentoID
    LEFT JOIN -- Para incluir funcionários mesmo sem projetos
        Funcionario_Projetos FP ON F.FuncionarioID = FP.FuncionarioID
    LEFT JOIN -- Para incluir a alocação mesmo se o projeto foi excluído (idealmente FK impediria)
        Projetos P ON FP.ProjetoID = P.ProjetoID
    ORDER BY
        F.PrimeiroNome, P.NomeProjeto;
    ```

- **5.7. Visualizando os JOINs:** _(Descrição textual dos diagramas de Venn mantida)_ `INNER` (interseção), `LEFT` (círculo esquerdo + interseção), `RIGHT` (círculo direito + interseção), `FULL OUTER` (união completa).

**Tabela: Resumo dos Tipos de JOIN**

| Tipo de JOIN      | Descrição                                                       | Palavra-chave SQL                  | Representação Venn (Descrição)         |
| :---------------- | :-------------------------------------------------------------- | :--------------------------------- | :------------------------------------- |
| `INNER JOIN`      | Retorna apenas linhas com correspondência em ambas as tabelas.  | `INNER JOIN` ou `JOIN`             | Apenas a interseção dos círculos.      |
| `LEFT JOIN`       | Retorna todas linhas da esquerda e correspondências da direita. | `LEFT JOIN` ou `LEFT OUTER JOIN`   | Todo o círculo esquerdo (inclui int.). |
| `RIGHT JOIN`      | Retorna todas linhas da direita e correspondências da esquerda. | `RIGHT JOIN` ou `RIGHT OUTER JOIN` | Todo o círculo direito (inclui int.).  |
| `FULL OUTER JOIN` | Retorna todas as linhas de ambas as tabelas.                    | `FULL OUTER JOIN` ou `FULL JOIN`   | União completa de ambos os círculos.   |

**6. Explorando o SQL Avançado** _(Renumerado)_

Recursos para consultas mais complexas e analíticas.

- **6.1. Subconsultas (Subqueries): Consultas Dentro de Consultas**
  `SELECT` embutido dentro de outra instrução `SQL`.

  - **Na Cláusula `SELECT` (Escalar):** Retorna 1 valor por linha externa.
    - _Exemplo Simples:_ Salário do funcionário e média geral.
      ```sql
      SELECT FuncionarioID, PrimeiroNome, Salario, (SELECT AVG(Salario) FROM Funcionarios) AS MediaGeral FROM Funcionarios;
      ```
    - _Exemplo Complexo (Correlacionada):_ Salário do funcionário e média do _seu_ departamento.
      ```sql
      SELECT
          F1.FuncionarioID, F1.PrimeiroNome, F1.Salario, F1.DepartamentoID,
          (SELECT AVG(F2.Salario) FROM Funcionarios F2 WHERE F2.DepartamentoID = F1.DepartamentoID) AS MediaDoDepartamento
      FROM Funcionarios F1;
      ```
  - **Na Cláusula `FROM` (Tabela Derivada):** Cria resultado temporário tratado como tabela (requer alias).
    - _Exemplo Simples:_ Deptos com mais de 5 funcionários.
      ```sql
      SELECT DepartamentoID, QtdFuncionarios
      FROM (SELECT DepartamentoID, COUNT(*) AS QtdFuncionarios FROM Funcionarios GROUP BY DepartamentoID) AS Contagem
      WHERE QtdFuncionarios > 5;
      ```
  - **Na Cláusula `WHERE`:** Filtra consulta externa. \* _Com `=, >, <` (Subconsulta retorna 1 valor):_ Funcionário com maior salário.
    `sql
SELECT PrimeiroNome, Salario FROM Funcionarios WHERE Salario = (SELECT MAX(Salario) FROM Funcionarios);
` \* _Com `IN`, `NOT IN` (Subconsulta retorna lista):_ Funcionários nos deptos 'TI' ou 'Engenharia'.
    `sql
SELECT PrimeiroNome FROM Funcionarios
WHERE DepartamentoID IN (SELECT DepartamentoID FROM Departamentos WHERE NomeDepartamento IN ('TI', 'Engenharia'));
` \* _Com `ANY`, `ALL` (Comparação com múltiplos valores):_ Funcionários com salário > que _todos_ do depto 3.
    `sql
SELECT PrimeiroNome, Salario FROM Funcionarios
WHERE Salario > ALL (SELECT Salario FROM Funcionarios WHERE DepartamentoID = 3);
` \* _Com `EXISTS`, `NOT EXISTS` (Verifica se retorna alguma linha):_ Deptos com funcionários.
    `sql
SELECT NomeDepartamento FROM Departamentos D WHERE EXISTS (SELECT 1 FROM Funcionarios F WHERE F.DepartamentoID = D.DepartamentoID);
`
    Subconsultas são flexíveis, mas podem ficar ilegíveis e ineficientes (especialmente correlacionadas).

- **6.2. Expressões de Tabela Comuns (CTEs): Simplificando a Complexidade**
  Conjuntos de resultados nomeados temporários (`WITH ... AS (...)`). Melhoram legibilidade, manutenção e permitem reutilização e recursividade.

  - **Sintaxe:** `WITH cte1 AS (...), cte2 AS (...) SELECT ... FROM cte1 JOIN cte2 ...;`
  - _Exemplo (Reescrevendo Subconsulta `FROM`):_ Deptos com mais de 5 funcionários.
    ```sql
    WITH ContagemPorDepto AS (
        SELECT DepartamentoID, COUNT(*) AS QtdFuncionarios
        FROM Funcionarios
        WHERE DepartamentoID IS NOT NULL
        GROUP BY DepartamentoID
    )
    SELECT D.NomeDepartamento, C.QtdFuncionarios
    FROM ContagemPorDepto C
    JOIN Departamentos D ON C.DepartamentoID = D.DepartamentoID
    WHERE C.QtdFuncionarios > 5;
    ```
  - _Exemplo (Múltiplas CTEs Encadeadas):_ Calcular bônus (5% do salário) para funcionários em projetos com orçamento > 1M.
    ```sql
    WITH ProjetosGrandes AS (
        SELECT ProjetoID FROM Projetos WHERE Orcamento > 1000000
    ),
    FuncionariosEmProjetosGrandes AS (
        SELECT DISTINCT FP.FuncionarioID
        FROM Funcionario_Projetos FP
        JOIN ProjetosGrandes PG ON FP.ProjetoID = PG.ProjetoID
    )
    SELECT F.FuncionarioID, F.PrimeiroNome, F.Salario * 0.05 AS Bonus
    FROM Funcionarios F
    JOIN FuncionariosEmProjetosGrandes FPG ON F.FuncionarioID = FPG.FuncionarioID;
    ```
  - _Exemplo (CTE Recursiva):_ Mostrar hierarquia de gerentes (simplificado).
    `sql
WITH RECURSIVE HierarquiaGerencial (FuncionarioID, NomeCompleto, GerenteID, Nivel) AS (
    -- Âncora: Funcionários sem gerente (nível 0)
    SELECT FuncionarioID, PrimeiroNome || ' ' || Sobrenome, GerenteID, 0
    FROM Funcionarios
    WHERE GerenteID IS NULL
    UNION ALL
    -- Passo Recursivo: Junta funcionários com seus gerentes diretos da CTE
    SELECT F.FuncionarioID, F.PrimeiroNome || ' ' || F.Sobrenome, F.GerenteID, H.Nivel + 1
    FROM Funcionarios F
    INNER JOIN HierarquiaGerencial H ON F.GerenteID = H.FuncionarioID
)
SELECT Nivel, FuncionarioID, NomeCompleto, GerenteID FROM HierarquiaGerencial ORDER BY Nivel, GerenteID;
`
    CTEs são principalmente para organização; impacto no desempenho varia.

- **6.3. Funções de Janela (`Window Functions`): Cálculos Inteligentes**
  Realizam cálculos sobre um conjunto de linhas ("janela") relacionadas à linha atual, _sem colapsar_ as linhas. Poderosas para análise.

  - **Cláusula `OVER()`:** Define a janela (`PARTITION BY`, `ORDER BY`, `ROWS`/`RANGE BETWEEN ...`).
  - **Categorias e Exemplos:**

    **Tabela: Funções de Janela Comuns**
    _(Conteúdo original mantido, com formatação de código e exemplos adaptados)_

    | Categoria    | Função (Exemplo)              | Descrição                                                   | Exemplo de Uso com `OVER()`                                                                                                                                               |
    | :----------- | :---------------------------- | :---------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
    | Ranking      | `ROW_NUMBER()`                | Número sequencial único na partição/ordem.                  | `ROW_NUMBER() OVER(PARTITION BY DepartamentoID ORDER BY Salario DESC)`                                                                                                    |
    |              | `RANK()`                      | Rank com pulos para empates.                                | `RANK() OVER(ORDER BY SUM(ValorTotalPedido) DESC)` (Rank clientes por vendas)                                                                                             |
    |              | `DENSE_RANK()`                | Rank sem pulos para empates.                                | `DENSE_RANK() OVER(PARTITION BY Cargo ORDER BY DataContratacao ASC)`                                                                                                      |
    |              | `NTILE(n)`                    | Divide em `n` grupos (quartis, decis, etc.).                | `NTILE(4) OVER(ORDER BY Salario DESC)` (Funcionários por quartil salarial)                                                                                                |
    | Deslocamento | `LAG(col, [off, def])`        | Valor da linha anterior na partição/ordem.                  | `LAG(Salario, 1, 0) OVER(PARTITION BY DepartamentoID ORDER BY DataContratacao)`                                                                                           |
    |              | `LEAD(col, [off, def])`       | Valor da linha seguinte na partição/ordem.                  | `LEAD(DataPedido) OVER(PARTITION BY ClienteID ORDER BY DataPedido)` (Próximo pedido)                                                                                      |
    |              | `FIRST_VALUE(col)`            | Valor da primeira linha na janela.                          | `FIRST_VALUE(DataContratacao) OVER(PARTITION BY DepartamentoID ORDER BY Salario DESC)`                                                                                    |
    |              | `LAST_VALUE(col)`             | Valor da última linha na janela (cuidado com frame padrão). | `LAST_VALUE(Salario) OVER(PARTITION BY DepartamentoID ORDER BY DataContratacao ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING)` (Salário mais recente no depto) |
    | Agregação    | `SUM(coluna)`                 | Soma na janela (ex: acumulada com `ORDER BY`).              | `SUM(ValorTotalPedido) OVER(PARTITION BY ClienteID ORDER BY DataPedido)` (Venda acumulada cliente)                                                                        |
    |              | `AVG(coluna)`                 | Média na janela (ex: móvel com `ROWS BETWEEN`).             | `AVG(Salario) OVER(PARTITION BY DepartamentoID ORDER BY DataContratacao ROWS BETWEEN 3 PRECEDING AND CURRENT ROW)` (Média móvel 4 contratações)                           |
    |              | `COUNT(coluna ou *)`          | Contagem na janela.                                         | `COUNT(*) OVER(PARTITION BY DepartamentoID)` (Total funcionários no depto)                                                                                                |
    |              | `MIN(coluna)` / `MAX(coluna)` | Mínimo/Máximo na janela.                                    | `MAX(Salario) OVER(PARTITION BY Cargo)` (Maior salário do cargo)                                                                                                          |

  - _Exemplo Adicional Complexo:_ Para cada funcionário, mostrar seu salário, o salário médio do departamento, e o percentual do seu salário em relação ao total do departamento.
    `sql
WITH SalarioDepartamento AS (
    SELECT
        DepartamentoID,
        SUM(Salario) AS TotalSalarioDept,
        AVG(Salario) AS MediaSalarioDept
    FROM Funcionarios
    WHERE DepartamentoID IS NOT NULL
    GROUP BY DepartamentoID
)
SELECT
    F.FuncionarioID,
    F.PrimeiroNome,
    F.DepartamentoID,
    F.Salario,
    SD.MediaSalarioDept,
    SD.TotalSalarioDept,
    -- Calculando percentual usando Window Function (alternativa ao JOIN com CTE acima)
    (F.Salario * 100.0 / SUM(F.Salario) OVER (PARTITION BY F.DepartamentoID)) AS PercentualSalarioNoDepto
FROM Funcionarios F
LEFT JOIN SalarioDepartamento SD ON F.DepartamentoID = SD.DepartamentoID -- Pode omitir se usar Window Function SUM
WHERE F.DepartamentoID IS NOT NULL;
`
    Funções de janela são indispensáveis para análises como % do total, comparações com média do grupo, rankings, etc., mantendo o detalhe da linha.

**7. Acelerando Consultas: Índices** _(Renumerado)_

Estruturas para otimizar recuperação de dados (`SELECT`, `WHERE`, `JOIN`).

- **7.1. O que são Índices e Por Que Usá-los?**
  Estrutura auxiliar com cópia ordenada de colunas + ponteiros para linhas. Analogia: índice remissivo de livro. Aceleram buscas evitando _full table scan_. Otimizam `WHERE`, `JOIN`, `ORDER BY`, `GROUP BY`. Índices `UNIQUE` impõem unicidade.

- **7.2. Criando Índices: `CREATE INDEX`**

  - **Sintaxe:** `CREATE [UNIQUE] INDEX nome_idx ON tabela (col1 [ASC|DESC], col2...);`
  - **Simples:** `CREATE INDEX idx_func_sobrenome ON Funcionarios (Sobrenome);`
  - **Único:** `CREATE UNIQUE INDEX idx_func_email ON Funcionarios (Email);`
  - **Composto:** `CREATE INDEX idx_func_depto_cargo ON Funcionarios (DepartamentoID, Cargo);` (Ordem importa!).

- **7.3. Removendo Índices: `DROP INDEX`**

  - **Sintaxe:** `DROP INDEX nome_idx;` (Pode precisar `ON tabela`).

- **7.4. O Custo do Desempenho: Trade-offs dos Índices**
  Índices aceleram leitura (`SELECT`) mas têm custos:
  - **Desempenho Escrita:** `INSERT`/`UPDATE`/`DELETE` ficam mais lentos (precisam atualizar índices). Muitos índices = escrita lenta.
  - **Armazenamento:** Ocupam espaço em disco/memória.
  - **Manutenção:** Podem fragmentar, exigindo `REORGANIZE`/`REBUILD`.
  - **Overhead Otimizador:** Raramente, podem levar a planos ruins.
    **Estratégia:** Não indexar tudo! Identificar queries lentas (`EXPLAIN`), indexar seletivamente colunas em `WHERE`/`JOIN`/`ORDER BY`. Monitorar impacto. Encontrar equilíbrio leitura vs escrita.

**8. Conclusão** _(Renumerado)_

`SQL` permanece indispensável para bancos de dados relacionais. ...Exploramos desde fundamentos (modelo relacional, chaves) até comandos (`DDL`, `DML`, `DCL`, `TCL`). Aprofundamos em `SELECT` (filtros, ordenação, agregação) e `JOIN`s. Vimos recursos avançados (subconsultas, `CTEs`, Funções Janela) e otimização com índices.
Dominar `SQL` é jornada contínua... Prática constante, experimentação e consulta a recursos são essenciais...

---
