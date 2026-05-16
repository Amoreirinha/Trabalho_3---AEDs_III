# Trabalho Prático 3 - Algoritmos de Caminho Mais Curto

**Disciplina:** AEDS 3 (DCE797)  
**Professor:** Iago Augusto de Carvalho  

---

## 📌 Objetivo

Este trabalho tem como objetivo implementar e avaliar **três algoritmos de caminho mais curto** em grafos ponderados, conexos e não orientados. Os algoritmos devem encontrar a menor distância entre um vértice de origem e todos os demais vértices do grafo.

Os algoritmos implementados são:

1. **Dijkstra** (versão clássica com vetor de distâncias)
2. **Duan** (algoritmo inovador de 2025, com heap mínimo)
3. **Bellman-Ford** (terceiro algoritmo escolhido)

Além da implementação, o trabalho inclui:
- Geração de grafos de teste (3 topologias diferentes, 500 a 10000 vértices)
- Análise comparativa de tempo de execução
- Relatório em PDF e apresentação de slides

---

## 🧠 Funcionamento dos Algoritmos

### 1. Algoritmo de Dijkstra

**Complexidade teórica:** O(V²) com matriz de adjacência

**Funcionamento:**
- Mantém um conjunto de vértices visitados e um vetor de distâncias mínimas
- A cada iteração, seleciona o vértice não visitado com menor distância
- Relaxa (atualiza) as distâncias dos vizinhos desse vértice
- Repete até que todos os vértices sejam visitados

**Características:**
- Funciona apenas com pesos não negativos
- Implementação simples com vetor (sem heap)
- Garante a solução ótima

### 2. Algoritmo de Duan (2025)

**Complexidade teórica:** O((V+E) log V) com heap mínimo

**Funcionamento:**
- Similar ao Dijkstra, mas utiliza uma **fila de prioridade (heap mínimo)**
- O heap permite extrair o vértice de menor distância mais eficientemente
- Inserções e extrações são O(log V)
- Reduz a complexidade para grafos esparsos

**Características:**
- Versão otimizada do Dijkstra
- Ideal para grafos com muitas arestas
- Foi proposta em 2025 e ainda não foi caracterizada empiricamente

### 3. Bellman-Ford (Terceiro Algoritmo)

**Complexidade teórica:** O(V × E)

**Funcionamento:**
- Inicializa distâncias como infinito (exceto origem = 0)
- Relaxa todas as arestas (V-1) vezes
- Cada iteração propaga as distâncias mínimas pelo grafo
- Permite detectar ciclos de peso negativo

**Características:**
- **Único** dos três que lida com pesos negativos
- Mais lento que os outros para grafos densos
- Útil quando há restrições de pesos negativos
- Implementado com três loops aninhados

---

## 📊 Comparação Teórica

| Algoritmo | Complexidade | Pesos Negativos | Estrutura | Uso Ideal |
|-----------|--------------|-----------------|-----------|-----------|
| Dijkstra | O(V²) | ❌ Não | Vetor | Grafos densos |
| Duan | O((V+E) log V) | ❌ Não | Heap | Grafos esparsos |
| Bellman-Ford | O(V×E) | ✅ Sim | Vetor | Pesos negativos |

---

## 🛠️ Como Compilar e Executar

### Pré-requisitos

- Compilador GCC
- Make (opcional, mas recomendado)
- Sistema Linux/Unix ou WSL no Windows

### Estrutura do Projeto
```text
TRABALHO_3---AEDS_III/
├── docs/ 
    └── results.csv
├── instance/
    ├── generate_graphs.py
    ├── generator.py
    └── requirements.txt
├── scr/
    ├── output/
    ├── algorithms.c
    ├── algorithms.h
    ├── base.c
    ├── Makefile 
    ├── run_test.py
    └── exemplo.dat
├── Trabalho 3 - Informações/
    ├── descricao.pdf
    ├── duan.pdf
    ├── gerador de instancias/
        ├── generator.py
        └── requirements.txt
    └── codigo base/
        ├── algorithms.c
        ├── algorithms.h
        ├── base.c
        ├── Makefile 
        ├── exemplo.dat
        ├── generator.py
        └── requirements.txt
└── README.md

```


### Compilação com Make

```bash
    # Compilar o programa
    make

    # Limpar arquivos objeto e executável
    make clean

    # Criar arquivo de exemplo para teste
    make create-example

    # Executar com o arquivo padrão
    make run

    # Mostrar ajuda
    make help
```

## Compilação Manual (sem Make)

### Compilar os arquivos objeto
```bash
gcc -Wall -Wextra -O2 -c base.c -o base.o
gcc -Wall -Wextra -O2 -c algorithms.c -o algorithms.o

# Linkar e criar o executável
gcc -Wall -Wextra -O2 -o programa base.o algorithms.o -lm

# Executar
./programa instancia_exemplo.dat
```
## Execução
```bash

# Sintaxe
./programa <arquivo_grafo>

# Exemplo
./programa instancia_exemplo.dat
```
### 📁 Formato dos Arquivos de Grafo

Os arquivos de grafo devem seguir este formato:
```text
<V> <E>
<u1> <v1> <peso1>
<u2> <v2> <peso2>
...
<uE> <vE> <pesoE>
```
Exemplo (instancia_exemplo.dat):
```text
5 6
0 1 4
0 2 2
1 2 1
1 3 5
2 3 8
2 4 10
```
Legenda:

* Primeira linha: número de vértices (V) e número de arestas (E)
* Linhas seguintes: vértice de origem, vértice de destino e peso da aresta
* Vértices são indexados de 0 a V-1
* O grafo é não orientado (a aresta é adicionada nos dois sentidos)

## 📊 Saída do Programa

O programa imprime uma linha para cada algoritmo:
```text

Dijkstra: <custo_total> <tempo_segundos>
Duan: <custo_total> <tempo_segundos>
Outro: <custo_total> <tempo_segundos>
```
Exemplo de saída:
```text

Dijkstra: 31.000000 0.000034
Duan: 31.000000 0.000028
Outro: 31.000000 0.000056
```
* custo_total: Soma das distâncias mínimas da origem (vértice 0) a todos os vértices
* tempo_segundos: Tempo de execução do algoritmo em segundos

## 🛠️ Como utilizar o `generator.py`

O script `generator.py` é uma ferramenta de linha de comando desenvolvida em Python que utiliza a biblioteca NetworkX para gerar diferentes tipos de grafos conexos. Ele atribui posições aleatórias, classes aos nós e pesos (`w1`) às arestas.

## Pré-requisitos

Antes de executar o script, é necessário instalar as dependências listadas no projeto. O arquivo `requirements.txt` exige as bibliotecas `networkx` e `scipy`. Para instalá-las, execute:

```bash
pip install -r requirements.txt
```
Sintaxe Básica

A execução do gerador segue o seguinte padrão via terminal:
```bash

./generator.py <topologia_do_grafo> <numero_de_nos> [argumentos_adicionais...]
```

### Topologias Suportadas e Argumentos

O script suporta diversas topologias de grafos, sendo que o primeiro argumento após o tipo de grafo é sempre o número de nós (n). Alguns modelos exigem parâmetros adicionais específicos:

* complete (Grafo Completo):
    
        Uso: python3 generator.py complete <n>

* erdos (Grafo de Erdős-Rényi):

        Uso: python3 generator.py erdos <n> <p>

        p (float): Probabilidade de criação de aresta.

* watts (Grafo de Watts-Strogatz):

        Uso: python3 generator.py watts <n> <k> <p>

        k (int): Cada nó é unido aos seus k vizinhos mais próximos (topologia em anel).

        p (float): Probabilidade de reconectar cada aresta.

* barabasi (Grafo de Barabási-Albert):

        Uso: python3 generator.py barabasi <n> <m>

        m (int): Número de arestas a serem anexadas de um novo nó aos nós existentes.

* turan (Grafo de Turán):

        Uso: python3 generator.py turan <n> <r>

        r (int): Número de partições.

* powerlaw (Powerlaw Cluster Graph):

        Uso: python3 generator.py powerlaw <n> <m> <p>

        m (int): Número de arestas a anexar a partir de um novo nó.

        p (float): Probabilidade de formar um triângulo após adicionar uma aresta aleatória.

* regular (Grafo Regular Aleatório):

        Uso: python3 generator.py regular <n> <d>

        d (int): Grau de cada nó.

* udg_u (Unit Disk Graph - Disposição Uniforme):

        Uso: python3 generator.py udg_u <n> <radius>

        radius (float): Valor do limite do raio.

* udg_r (Unit Disk Graph - Disposição Aleatória):

        Uso: python3 generator.py udg_r <n> <radius>

        radius (float): Valor do limite do raio.

### Exemplo de Execução

Para gerar um grafo de Erdős-Rényi com 10 nós e uma probabilidade de 0.5 (50%) de criação de arestas, execute:
```Bash
python3 generator.py erdos 10 0.5
```
---

# 👥 Grupo

Integrantes:

* Joaquim Pedro do Nascimento Moreira de Jesus
* Victória Almeida Tambasco
* Murilo Antonio da Silva
* Luiz Gabriel da Silva Cabrera
* Luiz Fernando Ferreira Cabral

Trabalho desenvolvido em grupo conforme as diretrizes da disciplina.

python3 generate_graphs.py -n 5 -min 500 -max 10000

python3 run_test.py