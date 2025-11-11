# A* + MST (POIs: Terminal de Ônibus)

## 👥 Equipe
- Jucinara Melo  
- Pablo Arthur 

---

## 🎥 Vídeo de Apresentação

https://drive.google.com/drive/folders/1d2m3NjQGRyPr-4WaeEATNGguh1IuAr0S?usp=sharing

---

## 🎯 Objetivo do Projeto

Este trabalho visa estimar o comprimento mínimo de vias reais (em quilômetros) necessário para interligar um conjunto de Pontos de Interesse (POIs) em diversas cidades, utilizando a combinação dos algoritmos de Busca A* e a Árvore Geradora Mínima (MST) para determinar a conectividade de menor custo.

---

## ⚙️ Metodologia

**1.1 POIs (Pontos de Interesse)**

POI escolhido: Terminais Rodoviários (Bus Hubs).

- Justificativa: A escolha dos Terminais Rodoviários é estratégica para medir a eficiência em grande escala da rede viária. Os terminais representam os principais polos logísticos e são naturalmente dispersos.

**1.2 Modelagem do Grafo Viário**

Para cada cidade:

- O grafo viário foi obtido com OSMnx, através de graph_from_place.

- O grafo foi projetado em coordenadas UTM para permitir o cálculo preciso de distâncias métricas.

- Cada estação de ônibus foi associada ao nó mais próximo do grafo.

**1.3 Cálculo de Rotas com A-star**

Para cada par de POIs:

- O algoritmo A* foi aplicado para encontrar a rota mais curta entre as estações.

- A heurística utilizada foi a distância euclidiana entre os nós.

- Foram registradas as distâncias totais (em km) e os caminhos percorridos.

**1.4 Construção da MST**

- Foi criado um grafo completo entre os POIs, onde as arestas foram ponderadas pelos custos calculados via A*.

- A partir desse grafo, foi calculada a MST utilizando o algoritmo de Kruskal, determinando a distância mínima total necessária para conectar todas as estações.

- As rotas reais correspondentes às arestas da MST foram reconstruídas na malha viária.

**1.5 Comparação entre Cidades**

O procedimento foi repetido em 8 cidades do nordeste:

- Natal
- Aracaju
- Fortaleza
- São Luís
- João Pessoa
- Campina Grande
- Mossoró
- Maceió

Para cada cidade, foram calculados:

- Comprimento total da MST (km)

- Média e desvio padrão por POI

- Visualização das rotas da MST sobre o mapa viário

## 🔍 Explicação dos Algoritmos

Os cálculos de otimização foram realizados pela combinação de dois algoritmos:

**A-Star**

O algoritmo A* é usado para encontrar o caminho mais curto entre dois pontos em um grafo.
Ele combina:

-o custo real do caminho percorrido até o momento (g);
-uma estimativa da distância restante até o destino (h), chamada heurística.

A heurística usada neste projeto foi a distância euclidiana (em linha reta) entre os nós.
Isso faz com que o A* seja mais eficiente que o Dijkstra, pois ele prioriza caminhos que provavelmente levam ao destino mais rápido.

  **Árvore Geradora Mínima (MST)**
  
Conecta todos os pontos de um grafo com o menor custo total possível, sem formar ciclos.
Neste projeto, cada cidade tem um grafo formado pelos POIs (terminais de ônibus), onde o peso de cada aresta é a distância calculada pelo A*. O algoritmo de Kruskal foi utilizado para encontrar a MST, garantindo a menor distância total necessária para interligar todas as estações.

## 📊 Resultados e Análises

📝 Análise de Natal 

<img width="438" height="636" alt="image" src="https://github.com/user-attachments/assets/f1ebd5c1-b304-4d73-9ee5-5b529c628a1e" />

O caso de Natal ilustra claramente a natureza do desafio logístico. O grafo gerado acima mostra que a MST precisa percorrer um longo caminho nas zonas norte e sul, sendo alongada pela presença do Rio Potengi/litoral. 
A rede mínima forma uma clara rota em "C", desviando do estuário e das áreas centrais densas.

Isso significa que o A* é obrigado a usar rotas indiretas, elevando o custo total da MST e o valor do km/POI. 

O método, portanto, reflete o custo real da superação de barreiras geográficas e urbanísticas, que é a principal limitação para a otimização da rede viária em cidades costeiras.


### **Tabela Comparativa Consolidada**

<img width="898" height="331" alt="image" src="https://github.com/user-attachments/assets/45f9bc31-54b7-483d-8e4f-76ed765875de" />

As métricas consolidadas abaixo (Média e Desvio Padrão) foram geradas a partir da tabela:

* **Média (km/POI):** 2.75 km
* **Desvio Padrão (km/POI):** 0.97 km
* **Média (km/Aresta MST):** 3.13 km
* **Desvio Padrão (km/Aresta MST):** 1.01 km

📈 **Análise:**

- Em média, **2,75 km** são necessários para conectar cada POI, com um desvio padrão de **0,97 km**.
- A distância média por aresta na MST é de **3,13 km**, com desvio padrão de **1,01 km**.
- **São Luís** apresentou as maiores distâncias médias (4,43 km/POI), indicando POIs mais espalhados.
- **Campina Grande** apresentou a menor (1,21 km/POI), sugerindo maior proximidade entre as estações.
- **Natal** e **Maceió** possuem redes mais complexas, com mais de 20 POIs e comprimento total superior a 60 km.
- No geral, as variações refletem a estrutura urbana e a distribuição dos terminais de ônibus em cada cidade.

Esses resultados mostram como a densidade e a organização da malha viária influenciam diretamente no custo total de interligação entre os pontos de interesse.

📉 **Análise Crítica:**

A comparação dos resultados mostra que a eficiência de conectividade é inversamente proporcional à escala urbana e à densidade dos POIs. As capitais de médio e grande porte, como Maceió, Natal e João Pessoa, apresentam elevado comprimento total da MST e maior número de POIs, mas mantêm um custo intermediário (entre 2,68 e 3,02 km/POI). Isso indica que, embora as redes sejam amplas, os terminais de ônibus não estão excessivamente dispersos.

O destaque vai para São Luís, com o maior custo médio (4,43 km/POI), resultado de uma malha urbana fragmentada por barreiras geográficas — como o estuário e as ilhas — que obrigam o algoritmo A* a traçar rotas mais longas.

Em contrapartida, cidades de porte menor, como Campina Grande (1,21 km/POI) e Mossoró (1,70 km/POI), exibem custos bem mais baixos. Nesses casos, o número reduzido e centralizado de terminais (3 a 4 POIs) forma uma rede MST mais compacta e eficiente.

Assim, o método evidencia não apenas as distâncias físicas, mas também o impacto de fatores geográficos e estruturais. Em grandes centros litorâneos, essas barreiras tornam-se o principal obstáculo à otimização da rede viária.

⚠️ **Limitações:**

- A qualidade dos dados do OpenStreetMap pode variar por cidade.

- O número de POIs disponíveis influencia a precisão das análises.

- A heurística euclidiana simplifica a distância real, podendo gerar pequenas distorções.
  
