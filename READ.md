# N1 de Estrutura de Dados - Código de Análise de Casos Judiciários do TJDFT em C


Este projeto é um programa em C que analisa um grande volume de dados de processos judiciais do Tribunal de Justiça do Distrito Federal e Territórios (TJDFT) a partir de um arquivo CSV. Ele faz análises quantitativas, gera o relatório de casos por categoria e gera estatísticas de  previsão do tempo de resolução, assim ajudando a extrair informações importantes para estudo ou pesquisa.


---

## Funcionalidades 

* **Análise de Desempenho:** Calcula o tempo de execução total do programa e o tempo gasto no processamento dos dados.
* **Leitura de Dados:** Processa um arquivo CSV com mais de 200.000 registros de processos.

* **Contagem de Categorias:** Identifica e conta processos relacionados a:
    * Causas ambientais
    * Causas envolvendo quilombolas e indígenas
    * Violência doméstica, feminicídio, infância e juventude
* **Análise de Tempo de Resolução:** Calcula o tempo (em dias) entre o início e a resolução de cada processo.
* **Busca de Dados:** Permite encontrar o processo mais antigo e o "último Órgão Julgador" de um processo específico pelo seu ID.
* **Cálculo de Meta:** Analisa e calcula o percentual de cumprimento da Meta 1, que visa julgar mais processos do que os que entram.
* **Geração de Relatório:** Cria um novo arquivo CSV contendo apenas os processos que foram "julgados" de acordo com a Meta 1.

---

## Estrutura do Projeto

O projeto é modular e dividido em três arquivos principais:

* `main.c`: A função principal do programa, que orquestra a execução das demais funcionalidades (leitura, análise, contagem e geração de relatórios).
* `processo.c`: Contém a lógica de todas as funções do projeto, incluindo a leitura de dados, análises, cálculos de meta e contagens.
* `processo.h`: Define a estrutura de dados `Processo` e os protótipos de todas as funções públicas.

### Estrutura de Dados

A principal estrutura de dados utilizada é a `struct Processo`, que armazena todas as informações de um único caso judicial. O programa armazena os dados em um array dessa estrutura.

```c
typedef struct {
    long long int id_processo;
    char numero_sigilo[128];
    // ... outros campos
} Processo;
```
## Análise de Dados e Lógica de Negócio
Leitura de CSV: A função lerDados lê o arquivo CSV linha por linha, separando os campos pelo delimitador ponto e vírgula (:).

Data Parsing: A função utilitária dataParaDias converte strings de data no formato "YYYY-MM-DD" para um número de dias a partir de "1970-01-01", facilitando o cálculo de diferenças de tempo.

Contagem de Categorias: Funções como contarViolenciaDomestica e contarFeminicidio percorrem o array de processos e somam os valores dos flags binários (flag_violencia_domestica, flag_feminicidio, etc.) para indicar a presença daquele tipo de ocorrência.

Cálculo da Meta 1: A função calcularCumprimentoMeta1 implementa a fórmula: percentual = (julgadom1 / (cnm1 + desm1 - susm1)) * 100.

Geração de CSV: A função gerarCSVJulgados opera sobre o array de processos e escreve em um novo arquivo CSV apenas os registros que possuem o flag julgadom1 com valor maior que 0.


## Compilação e Execução 
Pré-requisitos: Possuir o GCC (ou um compilador C compatível) instalado na sua máquina.

Arquivos Necessários: Deve-se ter os arquivos main.c, processo.c, processo.h, TJDFT_filtrado.csv e TJDF_amostra.csv e eles devem ficar na mesma pasta

Compilação
Abra o terminal na pasta do projeto e execute o seguinte comando:

```Bash
gcc main.c processo.c -o analise_tjdft
```

Execução
Para rodar o programa, execute o arquivo gerado:

```Bash

./analise_tjdft
```
