# 🛍️ Visual Recommendation System (Deep Learning)

Este projeto implementa um **Sistema de Recomendação Baseado em Conteúdo Visual** (*Visual Recommendation System*). Ao contrário dos sistemas tradicionais que recomendam itens usando metadados textuais (como preço, marca ou tags de texto), esta solução analisa exclusivamente a **aparência física** dos produtos (formas, texturas, cores e silhuetas) para encontrar correspondências visuais semelhantes.

---

## 📐 Como o Sistema Funciona?

O pipeline de processamento do recomendador segue três etapas principais:

1. **Extração de Embeddings (Assinatura Visual):** Utilizamos a rede neural profunda **MobileNetV2** pré-treinada no dataset *ImageNet*. Removemos a última camada de classificação para utilizar o modelo como um extrator de características de alto nível, transformando qualquer imagem de entrada em um vetor numérico unidimensional de 1280 dimensões (representando características espaciais, texturas e cores).
2. **Espaço Vetorial de Características:** Cada imagem do catálogo é representada como uma "seta" direcionada em um espaço multidimensional.
3. **Métrica de Similaridade:** O sistema calcula o ângulo entre o vetor da imagem de busca e os vetores de todo o catálogo. Quanto menor o ângulo entre duas setas, mais parecido é o aspecto visual delas. Esse cálculo é quantificado usando a **Similaridade de Cosseno**:

$$Similaridade = \cos(\theta) = \frac{\mathbf{A} \cdot \mathbf{B}}{\|\mathbf{A}\| \|\mathbf{B}\|}$$

---

## 🛠️ Tecnologias e Ferramentas Utilizadas

* **Linguagem:** Python
* **Deep Learning Framework:** TensorFlow / Keras (MobileNetV2)
* **Manipulação e Matemática:** NumPy
* **Cálculo Científico:** Scikit-Learn (`cosine_similarity`)
* **Visualização:** Matplotlib & Pillow (PIL)
* **Ambiente de Desenvolvimento:** Google Colab

---

## 📁 Estrutura Simplificada do Código

O projeto foi dividido de forma modular e lógica no notebook para facilitar o entendimento:

* **Inicialização do Modelo:** Carga da `MobileNetV2` utilizando *Transfer Learning* (pesos congelados do *ImageNet*).
* **Pré-processamento:** Pipeline para redimensionar as imagens para 224x224 pixels e normalizar os canais de cores (RGB).
* **Geração do Catálogo:** Varredura recursiva de pastas e geração em lote dos vetores de características para todo o inventário de imagens.
* **Motor de Busca:** Função de recomendação que recebe um produto, calcula o score de proximidade e renderiza graficamente o produto de busca ao lado dos resultados mais semelhantes.

---

## 🚀 Como Executar no Google Colab

1. Baixe o arquivo `.ipynb` deste repositório.
2. Abra o [Google Colab](https://colab.research.google.com/) e faça o upload do notebook.
3. Execute as células em ordem sequencial.
4. *(Opcional)* O próprio script contém uma célula que baixa automaticamente um conjunto mínimo de imagens públicas (relógios, campsites, calçados) para que você possa testar o funcionamento imediatamente sem precisar subir arquivos manuais.

---

## 🚀 Próximos Passos & Sugestão de Atualização: Escalando com FAISS

Atualmente, o projeto utiliza a função `cosine_similarity` do *Scikit-Learn* para realizar a busca linear (Força Bruta / $O(N)$) varrendo todo o catálogo. Embora essa abordagem seja excelente e precisa para datasets pequenos e médios, ela gera gargalos de latência em cenários de produção com milhares ou milhões de imagens.

### Proposta de Evolução: **FAISS (Facebook AI Similarity Search)**
Como melhoria futura de arquitetura, propõe-se substituir a busca linear pelo **FAISS**, uma biblioteca de código aberto desenvolvida pelo time de pesquisa de IA do Facebook para busca rápida de vetores densos em grande escala.

* **O que muda na arquitetura?** 
  Em vez de comparar a busca com cada imagem individualmente, os vetores de características gerados pela *MobileNetV2* passam a ser normalizados (L2) e adicionados a um índice indexado do FAISS (como `IndexFlatIP`).
* **Qual o benefício?**
  Utilizando algoritmos de **Vizinhos Próximos Aproximados (ANN)**, o FAISS agrupa vetores semelhantes em clusters. Em grandes produções, o tempo de resposta da recomendação cai de segundos para poucos **milissegundos**, permitindo escalar o catálogo para milhões de produtos com eficiência máxima de hardware e sem perda perceptível de qualidade na similaridade.
