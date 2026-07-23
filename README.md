<img width="600" height="280" alt="images" src="https://github.com/user-attachments/assets/a9dcd7c8-6607-47df-a38e-d04ec4a7248c" />

# KantoDB
---

<p align="center">
  <strong>Desafio Final - Módulo 2</strong>
</p>

<p align="center">
Banco de dados relacional inspirado no universo Pokémon, desenvolvido para representar treinadores, espécies, tipos, capturas e batalhas.

O projeto foi criado com foco em modelagem de dados, integridade referencial e consultas SQL aplicadas a um mini-mundo autoral.
</p>

---

## Sobre o projeto

O KantoDB simula um sistema de gerenciamento de treinadores Pokémon.

O banco permite registrar:

* treinadores;
* espécies de Pokémon;
* tipos;
* capturas;
* batalhas;
* participações em batalhas;
* resultados e pontuações.

Os dados relacionados às espécies de Pokémon são baseados na franquia Pokémon. Os dados de treinadores, capturas, locais, batalhas e resultados foram criados de forma fictícia exclusivamente para fins acadêmicos.

## Mini-mundo

O sistema considera as seguintes regras de negócio:

1. Um treinador pode realizar várias capturas.
2. Cada captura pertence a apenas um treinador.
3. Uma espécie de Pokémon pode estar presente em várias capturas.
4. Cada captura representa um indivíduo de uma espécie Pokémon.
5. Um Pokémon pode possuir um ou mais tipos.
6. Um tipo pode estar associado a vários Pokémon.
7. Uma batalha pode possuir várias participações.
8. Um treinador pode participar de várias batalhas.
9. Cada participação utiliza um Pokémon previamente capturado pelo treinador.
10. Um treinador pode existir sem possuir capturas.
11. Um Pokémon pode existir no catálogo sem ter sido capturado.
12. Uma captura pode possuir ou não um apelido.

## Modelo entidade-relacionamento

O banco possui as seguintes entidades:

* `treinador`
* `pokemon`
* `tipo`
* `pokemon_tipo`
* `captura`
* `batalha`
* `participacao_batalha`

### Relações N:N

O projeto possui duas relações muitos-para-muitos principais.

#### Treinador e Pokémon

Um treinador pode capturar vários Pokémon, e uma mesma espécie pode ser capturada por vários treinadores.

Essa relação é resolvida pela entidade associativa `captura`.

```text
TREINADOR 1:N CAPTURA N:1 POKEMON
```

#### Pokémon e tipo

Um Pokémon pode possuir mais de um tipo, e um tipo pode estar associado a vários Pokémon.

Essa relação é resolvida pela tabela associativa `pokemon_tipo`.

```text
POKEMON 1:N POKEMON_TIPO N:1 TIPO
```

## Estrutura das tabelas

### treinador

Armazena os treinadores cadastrados no sistema.

| Campo         | Tipo    | Restrição |
| ------------- | ------- | --------- |
| id_treinador  | INT     | PK        |
| nome          | VARCHAR | NOT NULL  |
| cidade        | VARCHAR | NOT NULL  |
| data_cadastro | DATE    | NOT NULL  |

### pokemon

Armazena as espécies de Pokémon.

| Campo            | Tipo    | Restrição        |
| ---------------- | ------- | ---------------- |
| id_pokemon       | INT     | PK               |
| nome             | VARCHAR | NOT NULL, UNIQUE |
| altura           | DECIMAL | NOT NULL         |
| peso             | DECIMAL | NOT NULL         |
| experiencia_base | INT     | NOT NULL         |

### tipo

Armazena os tipos dos Pokémon.

| Campo   | Tipo    | Restrição        |
| ------- | ------- | ---------------- |
| id_tipo | INT     | PK               |
| nome    | VARCHAR | NOT NULL, UNIQUE |

### pokemon_tipo

Tabela associativa entre Pokémon e tipo.

| Campo      | Tipo | Restrição |
| ---------- | ---- | --------- |
| id_pokemon | INT  | PK, FK    |
| id_tipo    | INT  | PK, FK    |

### captura

Representa um Pokémon capturado por um treinador.

| Campo         | Tipo    | Restrição    |
| ------------- | ------- | ------------ |
| id_captura    | INT     | PK           |
| id_treinador  | INT     | FK, NOT NULL |
| id_pokemon    | INT     | FK, NOT NULL |
| apelido       | VARCHAR | NULL         |
| nivel         | INT     | NOT NULL     |
| data_captura  | DATE    | NOT NULL     |
| local_captura | VARCHAR | NOT NULL     |

### batalha

Armazena as batalhas realizadas.

| Campo         | Tipo    | Restrição |
| ------------- | ------- | --------- |
| id_batalha    | INT     | PK        |
| data_batalha  | DATE    | NOT NULL  |
| local_batalha | VARCHAR | NOT NULL  |
| modalidade    | VARCHAR | NOT NULL  |

### participacao_batalha

Registra a participação de um treinador e de uma captura em uma batalha.

| Campo           | Tipo    | Restrição    |
| --------------- | ------- | ------------ |
| id_participacao | INT     | PK           |
| id_batalha      | INT     | FK, NOT NULL |
| id_treinador    | INT     | FK, NOT NULL |
| id_captura      | INT     | FK, NOT NULL |
| resultado       | VARCHAR | NOT NULL     |
| pontos          | INT     | NOT NULL     |

## Tecnologias utilizadas

* PostgreSQL
* DBeaver
* dbdiagram.io
* Git
* GitHub

## Estrutura do repositório

```text
kantodb/
│
├── README.md
├── docs/
│   ├── diagrama-er.png
│
├── sql/
│   ├── 01-create-database.sql
│   ├── 02-create-tables.sql
│   ├── 03-insert-data.sql
│   └── 04-queries.sql
│
└── database.dbml
```

## Consultas desenvolvidas

O projeto inclui uma bateria de dez consultas práticas, conforme solicitado no enunciado do desafio.

### 1. Pokémon de um tipo específico

Lista os Pokémon associados a um determinado tipo.

Recursos utilizados:

* `JOIN`;
* filtro com `WHERE`.

### 2. Capturas acima de determinado nível

Lista capturas cujo nível seja superior a um valor definido.

Recursos utilizados:

* `WHERE`;
* operadores de comparação;
* ordenação.

### 3. Quantidade de capturas por treinador

Exibe o total de Pokémon capturados por cada treinador.

Recursos utilizados:

* `COUNT`;
* `GROUP BY`;
* `JOIN`.

### 4. Média de nível por treinador

Calcula a média de nível das capturas de cada treinador.

Recursos utilizados:

* `AVG`;
* `GROUP BY`;
* `ROUND`.

### 5. Pokémon com mais de um tipo

Identifica espécies associadas a dois ou mais tipos.

Recursos utilizados:

* `COUNT`;
* `GROUP BY`;
* `HAVING`.

### 6. Treinadores sem capturas

Lista todos os treinadores, incluindo aqueles que ainda não capturaram nenhum Pokémon.

Recursos utilizados:

* `LEFT JOIN`;
* verificação de `NULL`.

### 7. Treinador, Pokémon e tipo

Exibe o treinador, o Pokémon capturado e seu tipo.

Recursos utilizados:

* múltiplos `JOIN`;
* combinação de quatro tabelas.

### 8. Pokémon mais capturados

Identifica as espécies com maior número de capturas.

Recursos utilizados:

* CTE;
* `COUNT`;
* ordenação.

### 9. Ranking de treinadores

Cria um ranking com base na pontuação obtida em batalhas.

Recursos utilizados:

* `SUM`;
* `GROUP BY`;
* `RANK()`;
* função de janela.

### 10. Maior captura de cada treinador

Exibe o Pokémon de maior nível de cada treinador.

Recursos utilizados:

* CTE;
* `ROW_NUMBER()`;
* `PARTITION BY`.

## Organização dos dados

Os dados do projeto foram divididos em duas categorias:

### Dados baseados na franquia

* nomes das espécies;
* tipos;
* altura;
* peso;
* experiência-base.

### Dados fictícios

* treinadores;
* cidades;
* datas;
* capturas;
* níveis;
* apelidos;
* batalhas;
* resultados;
* pontuações.

## Contexto acadêmico

Projeto desenvolvido como atividade prática de banco de dados, contemplando:

* criação de mini-mundo autoral;
* diagrama entidade-relacionamento;
* quatro ou mais tabelas;
* relação N:N;
* script DDL;
* script DML;
* no mínimo cinco registros por tabela;
* registros sem correspondência;
* dez consultas práticas;
* filtros;
* agregações;
* `GROUP BY`;
* múltiplos `JOIN`;
* `LEFT JOIN`;
* CTE;
* Window Function.

## Aviso

Pokémon e seus respectivos nomes são propriedades de seus detentores legais.

Este projeto não possui finalidade comercial e foi desenvolvido exclusivamente para fins de estudo.
