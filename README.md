Fluxo de Potência Ótimo Reativo (FPO-R) com Pontos Interiores
Este repositório contém a modelagem matemática e o código-fonte desenvolvidos durante o projeto de Iniciação Científica (PIBIC): "Uma implementação do fluxo de potência ótimo reativo solucionado pelo método primal-dual de pontos interiores".

O foco principal do projeto é a formulação e resolução do problema de Programação Não Linear (PNL) associado ao Fluxo de Potência Ótimo Reativo, com o objetivo de minimizar as perdas de potência ativa na transmissão. A solução respeita rigorosamente as equações de balanço nodal (Leis de Kirchhoff) e os limites operacionais de tensão e geração reativa.

Arquivos e Ordem de Execução
O desenvolvimento computacional foi dividido em etapas progressivas de complexidade e generalização, documentadas nos seguintes Jupyter Notebooks:

1. IC_AG.ipynb
Descrição: Primeira etapa do projeto, contendo as modelagens iniciais e a infraestrutura de dados base. Este arquivo estabelece as premissas matemáticas e a estrutura preliminar de otimização adotada antes da consolidação do método analítico final.

Objetivo: Validação de conceitos e estruturação lógica do problema.

2. fpo14_primal_dual_colab_final (2).ipynb
Descrição: Implementação focada na resolução do sistema-teste IEEE 14 barras. O código estrutura a matriz Hessiana, o gradiente da função Lagrangiana e o sistema de Newton para as condições KKT (Karush-Kuhn-Tucker).

Objetivo: Validação do núcleo algorítmico primal-dual de pontos interiores. Os resultados obtidos neste notebook servem como prova de conceito, apresentando convergência idêntica às implementações de referência na literatura acadêmica.

3. fpo_generico_ieee_30_57_FINAL.ipynb
Descrição: Versão final, generalizada e escalonável do algoritmo. Substitui o uso de dados fixos por um leitor automatizado de arquivos no padrão IEEE Common Data Format (CDF).

Objetivo: Aplicação do método primal-dual a sistemas de maior porte, especificamente os sistemas IEEE 30 e 57 barras. Comprova a robustez da formulação vetorial em memória para lidar com o aumento exponencial das matrizes de derivadas sem a necessidade de reescrever a topologia da rede.

Metodologia Computacional
Para garantir o desempenho e a auditabilidade do código, a implementação adotou as seguintes diretrizes técnicas:

Processamento em Memória (In-Memory): Utilização intensiva das bibliotecas NumPy e Pandas para manipulação matricial e vetorial, eliminando a latência de bancos de dados relacionais.

Diferenciação Simbólica: As derivadas de primeira e segunda ordem (Gradiente e Hessiana) são extraídas computacionalmente, o que previne erros manuais na formulação e garante exatidão matemática nas iterações do método de Newton.

Regularização Numérica: Implementação de técnicas de ajuste (como Levenberg-Marquardt) para manter o bom condicionamento numérico da matriz ao longo das atualizações dos passos primal e dual.

Dependências e Requisitos
Para executar os notebooks localmente ou em ambiente de nuvem (como o Google Colab), certifique-se de ter as seguintes bibliotecas instaladas em seu ambiente Python 3.x:

numpy

pandas

sympy (para a diferenciação simbólica)

Jupyter Notebook / Google Colab

Autoria e Agradecimentos
Autor: Andrick Rodrigues Alves de Souza

Orientadora: Profa. Dra. Jéssica Antônio Delgado

Fomento: Programa Institucional de Bolsas de Iniciação Científica (PIBIC)
