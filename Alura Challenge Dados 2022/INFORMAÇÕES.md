# Semana 1 - Alura Challenge
  - Informações obtidas para a semana 1 do Alura Challenge Dados (Alura cash). 

 ## Objetivos
   O principal objetivo desta primeira semana foi conhecer um pouco melhor os dados fornecidos. Desta forma, foram realizadas algumas análises, 
   no dump do banco de dados, utilizando o MySQL. Após essas análises, foram realizados uma tradução de colunas, junção das tabelas e posteriormente,
   conversão de arquivo (.sql para .csv).

## Primeiras Impressões
   O banco de dados fornecido, era composto por quatro tabelas: dados_mutuarios, emprestimos, historico_banco e id. No qual,
   foram realizadas algumas queries em busca de conhecer melhor sobre cada tabela e encontrar possiveis inconsistências.
   Abaixo temos essas primeiras impressões para cada tabela e possíveis inconsistências.
### dados_mutuarios
- Composto por 5 colunas (person_id, person_age, person_income, person_home_ownership, person_emp_length)

Tabela contendo os dados pessoais de cada solicitante

| Feature | Característica |
| --- | --- |
|`person_id`|ID da pessoa solicitante|
| `person_age` | Idade da pessoa - em anos - que solicita empréstimo |
| `person_income` | Salário anual da pessoa solicitante |
| `person_home_ownership` | Situação da propriedade que a pessoa possui: *Alugada* (`Rent`), *Própria* (`Own`), *Hipotecada* (`Mortgage`) e *Outros casos* (`Other`) |
| `person_emp_length` | Tempo - em anos - que a pessoa trabalhou |


- QUANTIDADE DE REGISTROS EM CADA COLUNA
    * person_id : 34489 registros
    * person_age : 34168 registros
    * person_income : 34153 registros
    * person_home_ownership : 34489 registros
    * person_emp_length : 33235 registros

 - Comando utilizado para encontrar a quantidade de registros em cada coluna
 ----
     SELECT 
        count(person_id) as 'contagem person_id', 
        count(person_age) as 'contagem person_age',
        count(person_income) as 'contagem person_income',
        count(person_home_ownership) as 'contagem person_home_ownership',
        count(person_emp_length) as 'contagem person_emp_length'
     FROM dados_mutuarios
---

#### INCONSISTENCIAS: 
        Idade máxima = 144 anos
        Tempo máximo de trabalho = 123 anos
        
 

### empréstimos:

   - Composto por 7 colunas (loan_id, loan_intent, loan_grade, loan_amnt, loan_int_rate, loan_status, loan_percent_income)

   Tabela contendo as informações do empréstimo solicitado

| Feature | Característica |
| --- | --- |
|`loan_id`|ID da solicitação de empréstico de cada solicitante|
| `loan_intent` | Motivo do empréstimo: *Pessoal* (`Personal`), *Educativo* (`Education`), *Médico* (`Medical`), *Empreendimento* (`Venture`), *Melhora do lar* (`Homeimprovement`), *Pagamento de débitos* (`Debtconsolidation`) |
| `loan_grade` | Pontuação de empréstimos, por nível variando de `A` a `G` |
| `loan_amnt` | Valor total do empréstimo solicitado |
| `loan_int_rate` | Taxa de juros |
| `loan_status` | Possibilidade de inadimplência |
| `loan_percent_income` | Renda percentual entre o *valor total do empréstimo* e o *salário anual* |

   - Quantidade de registros em cada coluna
        * loan_id : 34489 registros
        * loan_intent : 34489 registros
        * loan_grade : 34489 registros
        * loan_amnt : 34158 registros
        * loan_int_rate : 30862 registros
        * loan_status : 34146 registros
        * loan_percent_income : 34173  registros

 - Comando utilizado para encontrar a quantidade de registros em cada coluna
 ----
    [ SELECT 
        count(loan_id) as 'contagem loan_id', 
        count(loan_intent) as 'contagem loan_intent',
        count(loan_grade) as 'contagem loan_grade',
        count(loan_amnt) as 'contagem loan_amnt',
        count(loan_int_rate) as 'contagem loan_int_rate',
        count(loan_status) as 'contagem loan_status',
        count(loan_percent_income) as 'contagem loan_percent_income'
     FROM emprestimos ] 
----

   - Valores únicos em cada coluna (incluindo Vazio/Nulo)
        * loan_id : 34489 registros
        * loan_intent : 7 registros
        * loan_grade : 8 registros ('Homeimprovement','Venture','Personal','Medical','Education','Debtconsolidation')
        * loan_amnt : 753 registros
        * loan_int_rate : 348 registros
        * loan_status : 2 registros 
        * loan_percent_income : 77  registros

 - Comando utilizado para encontrar a quantidade de unicos em cada coluna
----
    [SELECT  
        count(DISTINCT loan_id) as 'contagem valores diferentes loan_id', 
        count(DISTINCT loan_intent) as 'contagem valores diferentes loan_intent',
        count(DISTINCT loan_grade) as 'contagem valores diferentes loan_grade',
        count(DISTINCT loan_amnt) as 'contagem valores diferentes loan_amnt',
        count(DISTINCT loan_int_rate) as 'contagem valores diferentes loan_int_rate',
        count(DISTINCT loan_status) as 'contagem valores diferentes loan_status',
        count(DISTINCT loan_percent_income) as 'contagem valores diferentes loan_percent_income'
      FROM emprestimos]
----
    
        
   - Valores máximos e mínimos das colunas numéricas:
     * loan_int_rate maximo: 23.22 loan_int_rate minimo: 5.42
     * loan_amnt maximo: 35000, loan_amnt minimo: 500
     * loan_percent_income maximo: 0.83  loan_percent_income minimo: 0

 - Comando utilizado para adiquirir o valor máximo/mínimo das colunas numéricas
 ----
    [ SELECT 
       max(loan_int_rate) as 'loan_int_rate maximo', min(loan_int_rate) as 'loan_int_rate minimo',
       max(loan_amnt) as 'loan_amnt maximo', min(loan_amnt) as 'loan_amnt minimo'
       max(loan_percent_income) as 'loan_percent_income maximo', min(loan_percent_income) as 'loan_percent_income minimo',
      FROM emprestimos ]

----


#### INCONSISTENCIAS:
    Uma possivel inconcistencia são pessoas com empréstimos altos que possuem score baixo e possibilidade de inadimplência.
    Valores de emprestimos nulo com o percentual do emprestimo e o salario anual.


### historicos_banco:
- Composto por 3 colunas (cb_id, cb_person_default_on_file, cb_person_cred_hist_length)

Tabela contendo as informações do historicos_banco, histórico de emprestimos solicitados


| Feature | Característica |
| --- | --- |
|`cb_id`|ID do histórico de cada solicitante|
| `cb_person_default_on_file` | Indica se a pessoa já foi inadimplente: sim (`Y`,`YES`) e não (`N`,`NO`) |
| `cb_person_cred_hist_length` | Tempo - em anos - desde a primeira solicitação de crédito ou aquisição de um cartão de crédito |

   

   - Quantidade de registros em cada coluna
      
       * cb_id : 34489 registros
       * cb_person_default_on_file : 34489 registros
       * cb_person_cred_hist_length : 34488 registros

   - Comando utilizado para encontrar a quantidade de registros em cada coluna
 ----
    [ SELECT 
        count(cb_id) as 'contagem cb_id', 
        count(cb_person_default_on_file) as 'contagem cb_person_default_on_file',
        count(cb_person_cred_hist_length) as 'contagem cb_person_cred_hist_length'
        
        FROM historicos_banco ]

 ----
 
 ### id
- Composto por 3 colunas (cb_id, loan_id, person_id)
Tabela que relaciona os IDs de cada informação da pessoa solicitante

| Feature | Característica |
| --- | --- |
|`person_id`|ID da pessoa solicitante|
|`loan_id`|ID da solicitação de empréstico de cada solicitante|
|`cb_id`|ID do histórico de cada solicitante|

- Quantidade de registros
  * count(person_id) : 14952

- Comando utilizado para encontrar a quantidade de registros na tabela
----
        [ SELECT count(*) from id ]
----
  
#### INCONSISTENCIAS: 
        O número de registros fornecidos nessa tabela é menor do que nas demais

### União das tabelas:
 - Comando para junção das tabelas

----    
    SELECT  
        id.person_id AS id_pessoa,
		id.cb_id AS id_emprestimo,
        id.loan_id AS id_historico,
        -- DADOS DA TABELA dados_mutuarios
        dados_mutuarios.person_age AS idade_pessoa,
        dados_mutuarios.person_income AS salario_pessoa,
        dados_mutuarios.person_home_ownership AS situacao_propriedade,
        dados_mutuarios.person_emp_length AS tempo_trabalho,
		-- DADOS DA TABELA emprestimos
        emprestimos.loan_intent AS motivo_emprestimo,
        emprestimos.loan_grade AS pontuacao_emprestimo,
        emprestimos.loan_amnt AS valor_emprestimo,
        emprestimos.loan_int_rate AS taxa_juros,
        emprestimos.loan_status AS possibilidade_inadimplencia,
        emprestimos.loan_percent_income AS relacao_emprestimo_salario,
        -- DADOS DA TABELA historicos_banco
        historicos_banco.cb_person_default_on_file AS possui_inadimplencia,
        historicos_banco.cb_person_cred_hist_length AS tempo_ultima_solicitacao
        

    FROM id

    INNER JOIN historicos_banco ON id.cb_id = historicos_banco.cb_id
    INNER JOIN dados_mutuarios ON id.person_id = dados_mutuarios.person_id
    INNER JOIN emprestimos ON id.loan_id = emprestimos.loan_id
 ---- 
