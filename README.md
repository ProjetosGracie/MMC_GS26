# Global Solution 2026.2 – CitizenTech
## Mobilidade Urbana Inteligente: Modelagem Matemática para a Gestão do Transporte Público

**Disciplina:** Modelagem Matemática e Computacional  
**Instituição:** FIAP – Faculdade de Informática e Administração Paulista  
**Curso:** Ciência da Computação – 1CCPJ  

### Integrantes

- **Conrado Gracie** – RM 569157
- **Renato Sandreschi** – RM 569156

---

## 1. Sobre o projeto

Este projeto foi desenvolvido para a Global Solution 2026.2 – CitizenTech, com o objetivo de aplicar conceitos de Modelagem Matemática, Álgebra Linear e Computação na resolução de um problema relacionado à mobilidade urbana.

O cenário proposto apresenta uma empresa municipal de transporte público que enfrenta dificuldades na distribuição de sua frota de ônibus entre quatro regiões de uma cidade: Norte, Sul, Leste e Oeste.

Após uma falha no sistema de gerenciamento, os registros individuais da quantidade de ônibus em cada região foram perdidos. Entretanto, algumas informações operacionais e estimativas de demanda de passageiros foram preservadas.

A partir desses dados, desenvolvemos uma solução computacional em Python capaz de:

- Reconstruir a distribuição original dos 72 ônibus.
- Verificar matematicamente se existe uma solução única para o problema.
- Identificar desequilíbrios entre a frota disponível e a demanda de passageiros.
- Desenvolver uma política de redistribuição dos veículos.
- Aplicar transformações lineares utilizando matrizes.
- Validar matematicamente os resultados obtidos.

## 2. Tecnologias utilizadas

| Tecnologia | Aplicação |
|---|---|
| Python | Linguagem utilizada no desenvolvimento |
| NumPy | Operações matriciais e resolução de sistemas lineares |
| Jupyter Notebook | Ambiente de desenvolvimento e execução |
| Álgebra Linear | Fundamentação matemática do projeto |

### Biblioteca principal

O projeto utiliza a biblioteca NumPy para trabalhar com matrizes, vetores, determinantes, postos e sistemas de equações lineares.

```python
import numpy as np
```

## 3. Desenvolvimento do projeto

O trabalho foi dividido em duas etapas principais, conforme solicitado no enunciado da Global Solution.

### Etapa 1 – Diagnóstico e modelagem da situação atual

Nesta etapa, o objetivo foi recuperar a quantidade de ônibus existente em cada região utilizando os registros operacionais disponibilizados pela empresa.

Foram definidas quatro variáveis:

- **N:** quantidade de ônibus na região Norte.
- **S:** quantidade de ônibus na região Sul.
- **L:** quantidade de ônibus na região Leste.
- **O:** quantidade de ônibus na região Oeste.

A partir das informações fornecidas, construímos o seguinte sistema de equações lineares:

\[
\begin{cases}
N+S+L+O=72\\
N+2S+O=76\\
S+2L=52\\
N+L+2O=76
\end{cases}
\]

O sistema também foi representado matricialmente:

\[
A \cdot x=b
\]

Para investigar a existência de uma solução única, utilizamos dois conceitos de Álgebra Linear:

- Determinante da matriz dos coeficientes.
- Posto (rank) da matriz.

A resolução computacional foi realizada com a função `np.linalg.solve()`, que calcula a solução do sistema linear.

**Resultado encontrado:**

| Região | Quantidade de ônibus | Percentual da frota |
|---|---:|---:|
| Norte | 12 | 16,67% |
| Sul | 20 | 27,78% |
| Leste | 16 | 22,22% |
| Oeste | 24 | 33,33% |
| **Total** | **72** | **100%** |

O resultado demonstra que o sistema possui uma única solução, permitindo reconstruir a distribuição original da frota.

Também foram realizadas verificações utilizando `assert`, garantindo que todas as equações fossem satisfeitas e que os valores encontrados fossem inteiros, não negativos e respeitassem o limite de 72 ônibus.

### Etapa 2 – Desenvolvimento de uma política de redistribuição

Na segunda etapa, utilizamos os dados de passageiros estimados para identificar as necessidades de atendimento de cada região.

A empresa informou que cada ônibus possui capacidade para transportar 40 passageiros no período analisado.

Assim, calculamos a quantidade mínima de veículos necessária por região utilizando a função teto matemático:

\[
Necessidade_i =
\left\lceil \frac{Passageiros_i}{40} \right\rceil
\]

**Demanda estimada:**

| Região | Passageiros | Frota atual | Frota necessária |
|---|---:|---:|---:|
| Norte | 560 | 12 | 14 |
| Sul | 760 | 20 | 19 |
| Leste | 720 | 16 | 18 |
| Oeste | 840 | 24 | 21 |
| **Total** | **2.880** | **72** | **72** |

Com esses resultados, identificamos os seguintes desequilíbrios:

- Norte: necessidade de receber 2 ônibus.
- Sul: possibilidade de ceder 1 ônibus.
- Leste: necessidade de receber 2 ônibus.
- Oeste: possibilidade de ceder 3 ônibus.

Como o excedente das regiões Sul e Oeste corresponde exatamente à necessidade das regiões Norte e Leste, foi possível desenvolver uma política de redistribuição sem aumentar a frota total.

## 4. Transformação linear e matriz de redistribuição

Para representar matematicamente a política proposta, utilizamos uma transformação linear:

\[
T(x)=M\cdot x
\]

A matriz de redistribuição desenvolvida foi:

\[
M=
\begin{bmatrix}
1 & 1/20 & 0 & 1/24\\
0 & 19/20 & 0 & 0\\
0 & 0 & 1 & 2/24\\
0 & 0 & 0 & 21/24
\end{bmatrix}
\]

Cada coeficiente representa a proporção da frota de uma região destinada a outra região.

A política funciona da seguinte maneira:

- Norte mantém seus 12 ônibus e recebe 1 ônibus do Sul e 1 do Oeste.
- Sul transfere 1 ônibus para o Norte e mantém 19.
- Leste mantém seus 16 ônibus e recebe 2 ônibus do Oeste.
- Oeste transfere 1 ônibus para o Norte e 2 para o Leste, mantendo 21.

**Resultado após a transformação linear:**

| Região | Frota inicial | Frota final |
|---|---:|---:|
| Norte | 12 | 14 |
| Sul | 20 | 19 |
| Leste | 16 | 18 |
| Oeste | 24 | 21 |
| **Total** | **72** | **72** |

A matriz foi construída de forma que a soma dos elementos de cada coluna seja igual a 1. Dessa maneira, a transformação preserva a quantidade total de veículos.

Além disso, todos os coeficientes são não negativos, garantindo que a redistribuição não produza quantidades negativas de ônibus.

## 5. Validação dos resultados

Para garantir a consistência da solução, implementamos verificações matemáticas no código utilizando a função `assert`.

Foram verificadas as seguintes condições:

1. Todas as equações do sistema linear original são satisfeitas.
2. A frota reconstruída possui valores inteiros e não negativos.
3. A quantidade total de ônibus permanece igual a 72.
4. A soma das colunas da matriz de redistribuição é igual a 1.
5. A nova distribuição atende à necessidade estimada de cada região.
6. A transformação linear não produz alocações negativas.

As verificações permitem identificar possíveis erros nos cálculos e confirmar se a política de redistribuição respeita as condições estabelecidas no problema.

## 6. Conclusão

O desenvolvimento deste projeto demonstrou como conceitos matemáticos e ferramentas computacionais podem auxiliar na resolução de problemas relacionados à mobilidade urbana.

Na primeira etapa, conseguimos reconstruir a distribuição original da frota utilizando sistemas de equações lineares e operações matriciais.

Na segunda etapa, analisamos a demanda de passageiros e identificamos regiões com déficit e superávit de veículos. Com base nessa análise, desenvolvemos uma matriz de transformação linear capaz de redistribuir os ônibus de maneira equilibrada.

A solução final mantém os 72 veículos disponíveis, sem necessidade de aquisição de novos ônibus, e permite que todas as regiões alcancem a quantidade mínima de veículos necessária para atender à demanda estimada.

Concluímos que a modelagem matemática, quando aplicada com ferramentas computacionais, pode contribuir para decisões mais eficientes na gestão de recursos públicos e no planejamento do transporte coletivo.

**Observação:** os dados utilizados são hipotéticos e representam uma simplificação da operação real de transporte público, conforme estabelecido no enunciado da atividade.

---

## 7. Como executar o projeto

### Pré-requisitos

É necessário possuir:

- Python 3 instalado.
- Jupyter Notebook ou JupyterLab.
- Biblioteca NumPy.

### Instalação da biblioteca

No terminal, execute:

```bash
pip install numpy
```

### Executando o notebook

1. Clone este repositório ou faça o download dos arquivos.
2. Abra a pasta do projeto no computador.
3. Inicie o Jupyter Notebook.
4. Abra o arquivo `Global_Solution_2_Semestre_MMC.ipynb`.
5. Execute as células em sequência para acompanhar os cálculos, as matrizes e as validações.

Para iniciar o Jupyter pelo terminal:

```bash
jupyter notebook
```

---

## 8. Estrutura do projeto

```text
Global-Solution-MMC/
│
├── Global_Solution_2_Semestre_MMC.ipynb
│
└── README.md
```

---

**FIAP – Ciência da Computação**  
**Global Solution 2026.2 – CitizenTech**  
**Modelagem Matemática e Computacional**
