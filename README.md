# Verificação Formal de Propriedades de Redes Neurais Recorrentes

Projeto de Iniciação Científica desenvolvido na **Universidade Federal do ABC (UFABC)**, com apoio financeiro do **Conselho Nacional de Desenvolvimento Científico e Tecnológico (CNPq)**.
### English

This project was developed at the Federal University of ABC (UFABC), with financial support from the Brazilian National Council for Scientific and Technological Development (CNPq).

## Sobre o projeto

Este projeto investiga a representação e a verificação formal de propriedades de **Redes Neurais Recorrentes (RNNs)**, com foco em **Simple Recurrent Networks (SRNs)**, utilizando a lógica infinitamente valorada de **Łukasiewicz** ($Ł_\infty$).

A abordagem utilizada explora a relação entre funções contínuas lineares por partes e a lógica de Łukasiewicz para representar formalmente a dinâmica de uma rede recorrente. A partir dessa representação, são formuladas propriedades da rede que podem ser submetidas a procedimentos automáticos de verificação.

O projeto concentra-se principalmente em duas propriedades:

- **Acessibilidade**: verifica se a rede pode atingir determinado valor ou limiar de saída para alguma entrada;
- **Robustez**: verifica se determinadas condições sobre uma entrada e uma perturbação da mesma preservam uma propriedade desejada da saída.

## Rede neural

Foi implementada uma **SRN do tipo Elman** para classificação binária de sentimentos.

A rede recebe uma sequência de tokens, representados por índices de palavras, e os transforma em vetores por meio de uma camada de *embedding*. Esses vetores são processados sequencialmente pela camada recorrente, cujo estado é atualizado de acordo com:

$$
\mathbf{h}_t =
g(W\mathbf{x}_t + U\mathbf{h}_{t-1}),
$$

onde:

- $\mathbf{x}_t$ é a entrada no instante $t$;
- $\mathbf{h}_t$ é o estado oculto;
- $W$ é a matriz de pesos associada à entrada;
- $U$ é a matriz de pesos recorrentes;
- $g$ é a função de ativação.

A função de ativação utilizada é a identidade truncada:

$$
g(z)=\max(0,\min(1,z)).
$$

A saída da rede é obtida a partir do último estado oculto.

### Configuração utilizada

| Parâmetro | Valor |
|---|---:|
| Tipo de rede | SRN / Elman |
| Tarefa | Classificação binária de sentimentos |
| Dimensão dos estados ($N$) | 4 |
| Vocabulário | 5000 palavras |
| Comprimento máximo | 32 tokens |
| Tamanho do lote | 16 |
| Épocas | 30 |
| Taxa de aprendizado | $10^{-3}$ |
| Vieses | Não utilizados |

## Dados

A rede foi treinada utilizando a base **IMDb**, disponibilizada pelo Keras.

Após o treinamento, foram utilizadas aproximadamente **2.000 resenhas do The Movie Database (TMDb)** para testes externos. As resenhas foram convertidas para a mesma representação de índices de palavras utilizada pela rede.

A característica temporal explorada pela SRN corresponde à **ordem dos tokens dentro de cada resenha**. Assim, o estado produzido após o processamento de um token é utilizado no processamento do token seguinte.

## Representação lógica

A dinâmica da SRN é traduzida para uma representação em lógica de Łukasiewicz. Para cada passo da sequência são introduzidas variáveis que representam as entradas e os estados da rede, juntamente com as fórmulas que relacionam esses valores.

Para a configuração utilizada nos experimentos, com $N=4$ e comprimento máximo de sequência igual a $32$, são necessárias:

$$
4 \times 32 = 128
$$

variáveis para representar as entradas da rede.

A representação obtida é utilizada para formular as propriedades de acessibilidade e robustez em $Ł_\infty$.

## Verificação formal

Os experimentos de verificação foram realizados utilizando o **LukaSol**.

### Acessibilidade

A acessibilidade é formulada como um problema de **satisfatibilidade**. Em termos gerais, busca-se determinar se existe uma entrada para a qual a rede atinge determinado valor ou limiar de saída.

Nos experimentos realizados, foram testados diferentes valores de saída:

- $0{,}50$ → `SAT`
- $0{,}75$ → `SAT`
- $0{,}78$ → `UNSAT` 

### Robustez

A robustez é formulada como uma **consequência lógica**, relacionando a representação da rede original à representação correspondente à entrada perturbada.

Para as condições consideradas no experimento, o LukaSol retornou:

```text
VALID
```

## Notebook

A implementação, o treinamento da rede, a tradução para a representação lógica e os experimentos de verificação estão disponíveis no Google Colab:

🔗 **[Abrir notebook no Google Colab](https://colab.research.google.com/drive/12DUH0XtI_R6918WQuLwMn4BwWuH5UfzF?usp=sharing)**

## Tecnologias utilizadas

- Python
- TensorFlow
- Keras
- LukaSol
- Lógica de Łukasiewicz

## Referências principais

- HÁJEK, Petr. *Metamathematics of Fuzzy Logic*. 1998.
- MCNAUGHTON, Robert. *A Theorem About Infinite-Valued Sentential Logic*. 1951.
- MUNDICI, Daniele. *A Constructive Proof of McNaughton's Theorem in Infinite-Valued Logic*. 1994.
- PRETO, Sandro; FINGER, Marcelo. *Efficient Representation of Piecewise Linear Functions into Łukasiewicz Logic Modulo Satisfiability*. 2022.
- PRETO, Sandro; FINGER, Marcelo. *Proving Properties of Binary Classification Neural Networks via Łukasiewicz Logic*. 2023.
- PRETO, Sandro. *Towards Logical Representations of Recurrent Neural Networks*. 2025.
- ELMAN, Jeffrey L. *Finding Structure in Time*. 1990.

## Autoria

Projeto desenvolvido na **Universidade Federal do ABC (UFABC)** com apoio financeiro do **CNPq**.
