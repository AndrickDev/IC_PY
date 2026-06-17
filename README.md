# Fluxo de Potência Ótimo Reativo (FPO-R) via Método Primal-Dual de Pontos Interiores

Este repositório contém a modelagem matemática e o código-fonte desenvolvidos
durante o projeto de Iniciação Científica (PIBIC): **"Uma implementação do fluxo
de potência ótimo reativo solucionado pelo método primal-dual de pontos
interiores"**.

O foco do projeto é a formulação e a resolução do problema de Programação Não
Linear (PNL) associado ao Fluxo de Potência Ótimo Reativo (FPO-R), com o objetivo
de **minimizar as perdas de potência ativa na transmissão**. A solução respeita
as equações de balanço nodal (Leis de Kirchhoff) e os limites operacionais de
magnitude de tensão e de geração de potência reativa.

---

## Formulação do Problema

Minimiza-se a perda ativa total na transmissão, somada sobre todos os ramos
(linhas e transformadores) que conectam as barras *k* e *m*:
min  f(x) = Σ  g_km · ( V_k²/t_km² + V_m² − 2·V_k·V_m/t_km · cos θ_km )

k,m ∈ L∪T

**Sujeito a:**

- Balanço de potência ativa e reativa em cada barra (restrições de igualdade);
- Limites de magnitude de tensão: 0,95 ≤ V_k ≤ 1,10 p.u.;
- Limites de geração de potência reativa: Q_min ≤ Q_g ≤ Q_max;
- Taps de transformadores tratados como parâmetros fixos.

O problema é resolvido pelo método **primal-dual de pontos interiores**, com
barreira logarítmica para as restrições de desigualdade e o sistema de Newton
aplicado às condições de otimalidade de Karush-Kuhn-Tucker (KKT).

---

## Arquivos e Ordem de Execução

O desenvolvimento foi dividido em etapas progressivas de complexidade e
generalização, documentadas em três Jupyter Notebooks:

### 1. `IC_AG.ipynb` — Modelagens iniciais
Primeira etapa do projeto, contendo as modelagens preliminares e a estruturação
lógica do problema de otimização. Estabelece as premissas matemáticas adotadas
antes da consolidação do método final.
**Objetivo:** validação de conceitos e estruturação do problema.

### 2. `fpo14_primal_dual_colab_final.ipynb` — Validação no IEEE 14
Implementação focada no sistema-teste IEEE 14 barras, com os dados da rede
definidos diretamente no código. Constrói o gradiente e a matriz Hessiana da
função Lagrangiana e resolve o sistema de Newton associado às condições KKT.
**Objetivo:** validação do núcleo algorítmico. O resultado obtido
(**0,1228 p.u.** de perda ativa) coincide com a implementação de referência em
MATLAB da orientadora, com diferença inferior a 0,01% — atribuível à precisão
numérica entre os ambientes.

### 3. `fpo_generico_ieee_30_57_FINAL.ipynb` — Versão generalizada
Versão final e escalonável do algoritmo. Substitui os dados fixos por um leitor
automatizado de arquivos no padrão **IEEE Common Data Format (CDF)**, permitindo
resolver qualquer sistema apenas trocando os arquivos de entrada.
**Objetivo:** aplicação do método aos sistemas IEEE 14, 30 e 57 barras a partir
do mesmo núcleo algorítmico, sem reescrever a topologia da rede.

---

## Resultados de Validação

| Sistema  | Variáveis | Perda final (MW) | Iterações |
|----------|-----------|------------------|-----------|
| IEEE 14  | 125       | 12,281           | 8         |
| IEEE 30  | 256       | 16,297           | 8         |
| IEEE 57  | 475       | 24,469           | 10        |

Em todos os casos houve convergência (tolerância 10⁻³), com as tensões finais
dentro dos limites operacionais e redução monotônica das perdas ao longo das
iterações.

---

## Metodologia Computacional

- **Processamento em memória (in-memory):** uso de NumPy e Pandas para
  manipulação matricial e vetorial, eliminando a latência de bancos de dados
  relacionais.
- **Diferenciação simbólica:** o gradiente e a matriz Hessiana são obtidos
  computacionalmente via SymPy, o que previne erros na dedução manual das
  derivadas e garante exatidão nas iterações do método de Newton.
- **Regularização numérica:** técnica do tipo Levenberg-Marquardt para manter o
  bom condicionamento da Hessiana ao longo das atualizações dos passos primal e
  dual.

> **Nota sobre desempenho:** o tempo total é dominado pela construção simbólica
> das derivadas, que cresce de forma acentuadamente superlinear com a dimensão
> do sistema (fenômeno de *expression swell*). Como perspectiva de trabalho
> futuro, pretende-se substituir a diferenciação simbólica por diferenciação
> automática ou dedução analítica fechada, reduzindo o tempo de montagem das
> matrizes para sistemas de grande porte (e.g., IEEE 118 e 300 barras).

---

## Dependências e Execução

Os notebooks foram desenvolvidos em **Python 3.x** e podem ser executados
localmente ou no **Google Colab**.

```bash
pip install numpy pandas sympy notebook
```

Bibliotecas utilizadas:
- `numpy` — operações matriciais e vetoriais
- `pandas` — leitura e estruturação dos dados da rede
- `sympy` — diferenciação simbólica do gradiente e da Hessiana
- Jupyter Notebook / Google Colab — ambiente de execução

### Como executar
1. Abra o notebook desejado (recomenda-se começar pelo item 2, IEEE 14).
2. Para o notebook genérico (item 3), envie ao ambiente os arquivos de dados no
   formato CDF (`B14.txt`/`L14.txt`, `B30.txt`/`L30.txt`, `B57.txt`/`L57.txt`) e
   selecione o sistema no parâmetro de configuração.
3. Execute as células na ordem (no Colab: *Ambiente de execução → Executar tudo*).

---

## Autoria e Agradecimentos

**Autor:** Andrick Rodrigues Alves de Souza
**Orientadora:** Profa. Dra. Jéssica Antônio Delgado
**Fomento:** Programa Institucional de Bolsas de Iniciação Científica (PIBIC)
