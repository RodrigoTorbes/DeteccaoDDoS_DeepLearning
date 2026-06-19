# DDoS Attack Detection Using SelectKBest and LSTM

Este repositório contém a implementação de um framework computacional otimizado para a detecção automatizada e classificação granular de ataques de Negação de Serviço Distribuída (DDoS). O projeto combina o algoritmo estatístico **SelectKBest** para seleção automatizada de atributos com uma rede neural recorrente **Long Short-Term Memory (LSTM)**.

O modelo foi treinado e validado utilizando o dataset de referência internacional **CICDDoS2019**, alcançando métricas de assertividade robustas entre **98% e 99%**.

Este trabalho foi baseado no artigo "Rodrigo-Torbes_IEEE_DDoS_LSTM.pdf".

---

## 🚀 Funcionalidades e Pipeline

O ecossistema do projeto segue os paradigmas de *Knowledge Discovery in Databases* (KDD), divididos em três etapas fundamentais:

### 1. Pré-processamento de Dados
* **Neutralidade de Viés:** Remoção explícita de identificadores a nível de socket (`Flow ID`, `Source IP`, `Source Port`, `Destination IP`, `Destination Port`, `Protocol`, `SimillarHTTP`) e indicadores cronológicos (`Timestamp`) para evitar sobreajuste temporal ou geográfico.
* **Tratamento de Dados:** Descarte de valores nulos (`NaN`) e normalização dos atributos restantes via escala linear dentro do intervalo rígido de $[-1, 1]$.

### 2. Seleção Automatizada de Atributos (Feature Selection)
* Utilização do algoritmo estatístico **SelectKBest** para mitigar o problema da dimensionalidade.
* Aplicação da métrica `f_classif` (ANOVA F-value) para ranquear e reter apenas as $k$ variáveis mais informativas dentre os 80 parâmetros originais da rede.

### 3. Arquitetura do Modelo (Deep Learning)
A rede possui uma topografia sequencial de 5 camadas:
* **Camada de Entrada:** 20 células de memória LSTM.
* **Camadas Intermediárias:** 3 camadas recorrentes contendo 10 unidades LSTM cada.
* **Regularização:** Taxa de *Dropout* de 30% (0.3) para mitigar o *overfitting*.
* **Camada de Saída:** Camada densa com função de ativação *Sigmoid* para classificação binária precisa.
* **Compilação:** Otimizador `RMSprop` e função de perda configurada como *Mean Squared Error* (MSE).

---

## 📊 Resultados Obtidos

Os testes foram realizados utilizando aceleração por hardware (GPU) no ambiente Google Colab, com janelas fixas de 50 épocas por classe de ataque. O modelo demonstrou alta performance na detecção e isolamento de quatro vetores principais:

| Categoria do Ataque | Vetor DDoS | Acurácia | F1-Score |
| :--- | :--- | :---: | :---: |
| **LDAP** | Reflection | 98.85% | 0.988 |
| **NetBIOS** | Reflection | 99.12% | 0.991 |
| **SYN** | Flooding | 98.54% | 0.985 |
| **UDP-Lag** | Exploitation | 99.03% | 0.990 |

*(Métricas consolidadas a partir da extração de Matrizes de Confusão)*

---

## 🛠️ Tecnologias Utilizadas

* **Python** (Linguagem base do pipeline)
* **Google Colab** com aceleração por GPU
* **Scikit-Learn** (para implementação do SelectKBest e pré-processamento)
* **Keras / TensorFlow** (para estruturação e treino das camadas LSTM)
* **CICDDoS2019 Benchmark Dataset**

---

## 👥 Autores

* **Rodrigo Horvath Torbes** – Unidade Acadêmica Online de Pesquisa e Educação, UNISINOS (Caxias do Sul, Brasil) — `horvathtorbes@gmail.com`
* **Daniel Stefani Marcon** – Unidade Acadêmica Online de Pesquisa e Educação, UNISINOS (São Leopoldo, Brasil) — `danielsm@unisinos.br`

---

## 📚 Referências do Conjunto de Dados

* Iman Sharafaldin, Arash Habibi Lashkari, Saqib Hakak e Ali A. Ghorbani, "Desenvolvendo um conjunto de dados e uma taxonomia realistas para ataques de negação de serviço distribuída (DDoS)", 53ª Conferência Internacional Carnahan sobre Tecnologia de Segurança do IEEE, Chennai, Índia, 2019. Disponível em: https://www.unb.ca/cic/datasets/ddos-2019.html
