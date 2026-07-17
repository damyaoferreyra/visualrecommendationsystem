# 🛍️ Visual Recommendation System (Deep Learning)

Este projeto implementa um **Sistema de Recomendação Baseado em Conteúdo Visual** (*Visual Recommendation System*). Ao contrário dos sistemas tradicionais que recomendam itens usando metadados textuais (como preço, marca ou tags de texto), esta solução analisa exclusivamente a **aparência física** dos produtos (formas, texturas, cores e silhuetas) para encontrar correspondências visuais semelhantes.

---

## 📐 Como o Sistema Funciona?

O pipeline de processamento do recomendador segue três etapas principais:

1. **Extração de Embeddings (Assinatura Visual):** Utilizamos a rede neural profunda **MobileNetV2** pré-treinada no dataset *ImageNet*. Removemos a última camada de classificação para utilizar o modelo como um extrator de características de alto nível, transformando qualquer imagem de entrada em um vetor numérico unidimensional de 1280 dimensões (representando características espaciais, texturas e cores).
2. **Espaço Vetorial de Características:** Cada imagem do catálogo é representada como uma "seta" direcionada em um espaço multidimensional.
3. **Métrica de Similaridade:** O sistema calcula o ângulo entre o vetor da imagem de busca e os vetores de todo o catálogo. Quanto menor o ângulo entre duas setas, mais parecido é o aspecto visual delas. Esse cálculo é quantificado usando a **Similaridade de Cosseno**.

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
4. *(Opcional)* O próprio script contém uma célula que baixa automaticamente um conjunto mínimo de imagens públicas (relógios, camisetas, calçados) para que você possa testar o funcionamento imediatamente sem precisar subir arquivos manuais.

---

> **Nota Técnica sobre o Comportamento:** Como o extrator utilizado é genérico (treinado para objetos do dia a dia), o modelo é altamente sensível a blocos dominantes de cores e contornos geométricos. Em cenários reais de e-commerce, é recomendável aplicar um filtro prévio por categoria (ex: buscar calçados apenas na subpasta de calçados) ou realizar o *fine-tuning* da rede em um dataset de moda específico (como o *Fashion MNIST*)
