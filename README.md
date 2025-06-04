# Challenge HP – Sprint 1: Estruturação de Dados e Análise Visual de Anúncios

## Integrantes
- Alan Maximiano (RM557088)
- André Rovai (RM555848)
- Antônio Vinicius (RM558014)
- Leonardo Zago (RM558691)
- Renan de França (RM558413)
- Thiago Almança (RM558108)

## 1. Introdução
Este projeto corresponde à Sprint 1 do Challenge HP, com o objetivo principal de estruturar dados visuais de anúncios publicitários e realizar testes iniciais com abordagens de Inteligência Artificial (IA) para identificar e analisar diferenças visuais entre eles. A capacidade de diferenciar o conteúdo visual de campanhas é fundamental para estratégias de marketing, análise de concorrência e manutenção da identidade da marca.

Nesta sprint, exploramos o uso de Redes Neurais Convolucionais (CNNs) para classificação de conteúdo e a aplicação de embeddings de imagem para medição de similaridade visual, validando essas técnicas em um conjunto de dados reduzido.

## 2. Hipóteses Definidas
Quatro hipóteses foram delineadas para guiar a exploração:

*   **H1: Classificação de Conteúdo/Tipo de Anúncio (CNN)**
    *   **Descrição:** Uma CNN pode ser treinada para distinguir anúncios com base no tipo de produto ou conteúdo principal (ex: "HP Original" vs. "Outros").
    *   **Como indica diferença:** Anúncios classificados em categorias distintas são considerados diferentes sob o critério da classificação.
    *   **Status:** Testada e validada preliminarmente.

*   **H2: Similaridade Visual com Embeddings (Modelos Pré-treinados)**
    *   **Descrição:** Embeddings extraídos de modelos como ResNet50 capturam características visuais semânticas e estilísticas.
    *   **Como indica diferença:** Baixa similaridade de cosseno entre embeddings ou pertencimento a clusters distintos indicam anúncios visualmente diferentes.
    *   **Status:** Testada e validada preliminarmente.

*   **H3: Classificação de Estilo/Atributo (CNN) – Conceitual**
    *   **Descrição:** Uma CNN poderia ser treinada para classificar anúncios por atributos estilísticos (e.g., minimalista, colorido).
    *   **Status:** Conceitual para esta sprint; requer dataset rotulado mais extenso.

*   **H4: Detecção de Anomalias (Autoencoders) – Conceitual**
    *   **Descrição:** Um autoencoder treinado em anúncios "padrão" da HP poderia identificar anúncios que se desviam significativamente desse padrão.
    *   **Status:** Conceitual para esta sprint; requer dataset ampliado.

## 3. Metodologia e Desenvolvimento
As hipóteses H1 e H2 foram implementadas e testadas utilizando um notebook Google Colab.

### 3.1. Conjuntos de Dados
Foram utilizados os seguintes conjuntos de imagens, hospedados no Google Drive e processados localmente no Colab:
*   **Para H1 (Classificação):**
    *   `HP_Original`: 30 imagens
    *   `Outros`: 30 imagens
*   **Para H2 (Embeddings e Similaridade):**
    *   `general_ads_for_embeddings`: 60 imagens diversas
*   **Pré-processamento:** Todas as imagens foram validadas, convertidas para o formato JPEG e modo RGB.

### 3.2. Ferramentas
*   Python 3.x, Google Colab
*   TensorFlow e Keras para modelos de Deep Learning
*   Scikit-learn para métricas e clustering
*   Pillow (PIL) para manipulação de imagens
*   Matplotlib para visualizações

### 3.3. Implementação H1 – Classificação CNN
*   **Modelo:** CNN sequencial com 3 camadas convolucionais (filtros 16, 32, 64) intercaladas com MaxPooling, Dropout (0.3), e camadas Dense para classificação binária (sigmoide).
*   **Treinamento:** Otimizador 'adam', perda 'binary_crossentropy', 10 épocas.
*   **Dataset:** Carregado com `image_dataset_from_directory`, split de validação de 20%.

### 3.4. Implementação H2 – Embeddings com ResNet50
*   **Modelo:** ResNet50 pré-treinado (ImageNet) para extração de features (vetores de 2048 dimensões).
*   **Análise:** Cálculo de similaridade de cosseno entre pares de anúncios e aplicação de KMeans (k=3) para agrupamento.

## 4. Resultados e Discussão (Resumo)

*   **H1 – Classificação CNN:**
    *   O modelo CNN alcançou **100% de acurácia de treino e validação** nas últimas épocas para a tarefa de classificar entre "HP_Original" e "Outros" no dataset de 60 imagens. A perda também foi minimizada.
    *   A matriz de confusão para o conjunto de validação (5 imagens "HP_Original", 7 "Outros") mostrou **100% de classificações corretas**.
    *   **Discussão H1:** O desempenho foi excelente no dataset reduzido, indicando boa separabilidade das classes. No entanto, é crucial expandir o dataset e aplicar técnicas como data augmentation e validação cruzada para garantir a generalização do modelo e evitar conclusões baseadas em possível overfitting.

*   **H2 – Similaridade Visual com Embeddings:**
    *   A matriz de similaridade de cosseno revelou agrupamentos coerentes: anúncios visualmente similares apresentaram scores > 0.85, enquanto estilos distintos tiveram scores < 0.5.
    *   O KMeans (k=3) conseguiu agrupar os 60 anúncios em clusters com características visuais discerníveis (e.g., anúncios escuros, produtos isolados, estilo lifestyle).
    *   **Discussão H2:** A abordagem com embeddings demonstrou ser eficaz para comparação visual automatizada e agrupamento exploratório sem a necessidade de rótulos prévios, constituindo uma ferramenta valiosa para benchmarking visual.

## 5. Utilização nas Próximas Etapas do Challenge HP
As estratégias desta sprint estabelecem uma base para:
*   **Expansão e Rotulagem de Dados:** Coleta de um dataset mais amplo de anúncios da HP e concorrentes, com rótulos para estilos, produtos e campanhas.
*   **Desenvolvimento de Análises Automatizadas:**
    *   Classificadores CNN mais robustos para tipo de anúncio, estilo, e outros atributos (H1, H3).
    *   Modelos de detecção de anomalias para identificar anúncios fora do padrão visual da HP (H4).
*   **Benchmarking Visual com Embeddings (H2):**
    *   Criação de mapas visuais do mercado para posicionar a marca HP.
    *   Comparação visual de campanhas específicas.
*   **Aplicações Estratégicas:**
    *   Avaliação da consistência da identidade visual da marca.
    *   Detecção de tendências emergentes no design de anúncios.
    *   Suporte ao processo criativo de novas campanhas.

## 6. Conclusão do Sprint 1
O Sprint 1 validou tecnicamente a aplicação de CNNs para classificação de conteúdo (H1) e o uso de embeddings para análise de similaridade visual (H2) em anúncios. Os resultados preliminares foram positivos, especialmente considerando o dataset limitado. As hipóteses H3 (classificação de estilo) e H4 (detecção de anomalias) foram confirmadas conceitualmente como direções futuras promissoras, dependentes da expansão do conjunto de dados.

As técnicas exploradas têm o potencial de fornecer à HP ferramentas poderosas para monitorar sua identidade visual, analisar a concorrência e embasar decisões criativas. O próximo passo crucial é a ampliação e o enriquecimento do dataset.

## 7. Código Fonte
O código desenvolvido para este sprint, incluindo o pré-processamento de dados, treinamento do modelo CNN (H1) e a análise de similaridade com embeddings (H2), está disponível no seguinte notebook Google Colab:

[Image_Analysis_for_Ad_Differentiation_PT1.ipynb](Image_Analysis_for_Ad_Differentiation_PT1.ipynb)

*(Nota: O link acima assume que o arquivo .ipynb está no mesmo diretório do README.md. Se estiver hospedado online, substitua pelo link direto para o Google Colab.)*

Para executar o notebook:
1.  Abra o link no Google Colab.
2.  Certifique-se de que seu Google Drive esteja montado e que o caminho `original_dataset_root_path_drive` no código aponte para a pasta correta contendo as subpastas `HP_Original`, `Outros`, e `general_ads_for_embeddings` com as respectivas imagens.
3.  Execute as células sequencialmente.
