---
title: "That's what the numbers said!"
subtitle: "O quê os números dizem sobre uma das séries de comédia mais famosas do século XXI?"
excerpt: "O quê os números dizem sobre uma das séries de comédia mais famosas do século XXI? Foi esta pergunta que eu tentei responder em meu trabalho de conclusão do curso [**R para Ciência de Dados I**](https://curso-r.com/cursos/r4ds-1/) da [Curso-R](https://curso-r.com/)."
date: 2021-11-20
author: "Luísa Gisele Böck"
draft: false
# layout options: single, single-sidebar
layout: single
categories:
- R
- TV Series
tags:
- The Office
bibliography: packages.bib
---

<script src="{{< blogdown/postref >}}index_files/clipboard/clipboard.min.js"></script>
<link href="{{< blogdown/postref >}}index_files/xaringanExtra-clipboard/xaringanExtra-clipboard.css" rel="stylesheet" />
<script src="{{< blogdown/postref >}}index_files/xaringanExtra-clipboard/xaringanExtra-clipboard.js"></script>
<script>window.xaringanExtraClipboard(null, {"button":"Copy Code","success":"Copied!","error":"Press Ctrl+C to Copy"})</script>
<script src="{{< blogdown/postref >}}index_files/htmlwidgets/htmlwidgets.js"></script>
<link href="{{< blogdown/postref >}}index_files/datatables-css/datatables-crosstalk.css" rel="stylesheet" />
<script src="{{< blogdown/postref >}}index_files/datatables-binding/datatables.js"></script>
<script src="{{< blogdown/postref >}}index_files/jquery/jquery-3.6.0.min.js"></script>
<link href="{{< blogdown/postref >}}index_files/dt-core/css/jquery.dataTables.min.css" rel="stylesheet" />
<link href="{{< blogdown/postref >}}index_files/dt-core/css/jquery.dataTables.extra.css" rel="stylesheet" />
<script src="{{< blogdown/postref >}}index_files/dt-core/js/jquery.dataTables.min.js"></script>
<link href="{{< blogdown/postref >}}index_files/crosstalk/css/crosstalk.min.css" rel="stylesheet" />
<script src="{{< blogdown/postref >}}index_files/crosstalk/js/crosstalk.min.js"></script>

<img src="https://img.nbc.com/sites/nbcunbc/files/scet/photos/22/7535/0416_NUP_143665_1136.jpg?impolicy=nbc_com&imwidth=1080&imdensity=1" style="display: block; margin: auto;" />

<div style="text-align: right">

<small><a href="https://www.nbc.com/the-office/photos">NBC Photos: ‘Michael’s Last Dundies’</a></small>

</div>

<br>

<div style="text-align: justify">

Em agosto de 2020, um trecho do documentário *O Fórum* viralizou na internet mostrando, nos bastidores do Fórum Econômico Mundial, ocorrido em 2019, na cidade de Davos (Suíça), um diálogo sobre a Amazônia entre Jair Bolsonaro e o ex-vice-presidente dos Estado Unidos Al Gore, reconhecido ativista ambiental. Impossível não relacionar a situação constrangedor, vivida pelo atual presidente brasileiro, com uma cena protagonizada por Michael Scott, personagem interpretado por Steve Carrel, no seriado **The Office**. Série que redefiniu a situação de vergonha alheia.

O presente trabalho visa realizar uma análise sobre o seriado americano **The Office**, como audiência, melhores episódios, roteiros, direção e elenco. As informações observadas foram integralmente coletadas da base de dados [IMDb (*Internet Movie Database*)](https://www.imdb.com)[^1] - base de dados *online* sobre entretenimento, a avaliação é composta por notas dadas por espectadores e críticos.

Para iniciar a análise, é necessário carregar os pacotes, as bases de dados[^2] e a fonte das letras[^3] utilizadas nos gráficos. Os códigos das saídas apresentadas neste texto (gráficos e tabela) podem ser vistos - e copiados - em \`Veja o código em R’.

``` r
library(tidyverse)

# carrega a base com os dados gerais do seriado
theoffice_dados <-
  readxl::read_excel("data/the_office_series.xls") %>%
  janitor::clean_names()

# carrega a base com os nomes dos personagens e respectivos atores
theoffice_personagens <-
  readxl::read_excel("data/the_office_series.xls", 
                     sheet = "elenco_personagens") %>%
  janitor::clean_names() %>%
  rename(elenco_nome = elenco)

# lê a tabela de fontes e registra no R (somente para usuários de Windows)
# extrafont::loadfonts(device = "win") 
```

### A Série

**The Office** é uma série americana, originalmente transmitida pela NBC entre 25 de março de 2005 e 16 de maio de 2013, com um total de 188 episódios[^4]. A série, uma adaptação do programa britânico de mesmo nome, é exibida em formato de *pseudodocumentário*, e retrata o cotidiano dos funcionários de um escritório filial da *Dunder Mifflin Paper Company*, uma empresa que vende papel localizada na cidade de Scranton, na Pensilvânia. A série venceu 51 prêmios das 193 indicações que recebeu, entre elas consta, principalmente, 1 *Golden Globes* (2006), 2 *Screen Actors Guild Awards* (2007 e 2008) e 5 *Primetime Emmy Awards* (2006, 2007, 2009 e 2013), incluindo melhor série de comédia.

<img src="{{< blogdown/postref >}}index_files/figure-html/episodio-por-temporada-plot-1.png" width="672" style="display: block; margin: auto;" />

<details>
<summary>
Veja o código em R
</summary>

``` r
theoffice_dados %>%
  ggplot(aes(x = temporada)) +
  geom_bar(color = "black",
           fill = RColorBrewer::brewer.pal(9, "Paired")) +
  geom_text(
    aes(label = ..count..),
    stat = "count",
    vjust = -1,
    family = "StaffMeetingPlain"
  ) +
  labs(x = "Temporada",
       y = "Episodios",
       caption = "Figura 1: Episodios por temporada") +
  scale_y_continuous(limits = c(0, 28), breaks = seq(0, 30, 5)) +
  scale_x_continuous(limits = c(0, 10), breaks = seq(1, 9, 1)) +
  theme_minimal() +
  theme(
    panel.grid.major.x = element_blank(),
    panel.grid.minor = element_blank(),
    text = element_text(family = "StaffMeetingPlain"),
    legend.position = c(.95, .95),
    legend.justification = c("right", "top"),
    legend.box.just = "right"
  )
```

</details>

<br>

É dividida em nove temporadas com uma média de 20 episódios cada uma. A primeira temporada é a mais curta de toda a série, com apenas seis episódios e duração de 137 minutos. Em oposição, as temporadas mais duradouras são a quinta e a sexta, com 26 episódios cada e com 751 e 755 minutos respectivamente.

<img src="{{< blogdown/postref >}}index_files/figure-html/duracao-por-temporada-plot-1.png" width="672" style="display: block; margin: auto;" />

<details>
<summary>
Veja o código em R
</summary>

``` r
theoffice_dados %>%
  group_by(temporada) %>%
  summarise(duracao_min_temporada = sum(duracao)) %>%
  ggplot(aes(x = temporada, y = duracao_min_temporada)) +
  geom_col(color = "black",
           fill = RColorBrewer::brewer.pal(9, "Paired")) +
  geom_text(aes(label = round(duracao_min_temporada)),
            vjust = -1,
            family = "StaffMeetingPlain") +
  labs(x = "Temporada",
       y = "Duracao (em minutos)",
       caption = "Figura 2: Duracao total (em minutos) de cada temporada") +
  scale_x_continuous(limits = c(0, 10), breaks = seq(1, 9, 1)) +
  scale_y_continuous(limits = c(0, 800), breaks = seq(0, 750, 100)) +
  theme_minimal() +
  theme(
    panel.grid.major.x = element_blank(),
    panel.grid.minor = element_blank(),
    text = element_text(family = "StaffMeetingPlain"),
    legend.position = c(.95, .95),
    legend.justification = c("right", "top"),
    legend.box.just = "right"
  )
```

</details>

<br>

### Audiência[^5]

Em 2019, **The Office** superou *Friends* como a série mais assistida da *Netflix* nos Estados Unidos.[^6] Segundo dados revelados por Scott Lazerson, executivo da empresa de *streaming*, o seriado protagonizado por Steve Carrel acumulou um total de 52,98 bilhões de minutos assistidos, contra 42,6 bilhões de *Friends*. Apesar do êxito nos dias atuais, **The Office** nunca foi um sucesso de audiência quando exibida nos EUA pela NBC. O programa quase foi cancelado entre a segunda e terceira temporada e só foi salvo por suas boas vendas no iTunes.

Conforme mostra a *Figura 3*, a audiência média de todas as temporadas do seriado não ultrapassa os 9 milhões de telespectadores, sendo a quinta e a nona (e última) temporada as com melhor e pior média - 8.76 e 4,14 milhões de telespctadores, respectivamente. É possível observar, após a saída de Steve Carrel do elenco, ao final da 7ª temporada, uma queda bastante considerável na audiência média das últimas duas temporadas.

<img src="{{< blogdown/postref >}}index_files/figure-html/audiencia-media-plot-1.png" width="672" style="display: block; margin: auto;" />

<details>
<summary>
Veja o código em R
</summary>

``` r
theoffice_dados %>%
  group_by(temporada) %>%
  summarise(mean_aud_season = mean(qntd_telespectadores_eua_milhoes)) %>%
  ggplot(aes(x = temporada, y = mean_aud_season)) +
  geom_col(color = "black",
           fill = RColorBrewer::brewer.pal(9, "Paired")) +
  geom_text(aes(label = round(mean_aud_season, 2)),
            vjust = -1,
            family = "StaffMeetingPlain") +
  labs(x = "Temporada", y = "Media da audiencia (em milhoes) nos EUA",
       caption = "Figura 3: Audiencia media (em milhoes) nos EUA") +
  coord_cartesian(ylim = c(4, 9)) +
  scale_x_continuous(limits = c(0, 10), breaks = seq(1, 9, 1)) +
  scale_y_continuous(breaks = seq(4, 9, .5)) +
  theme_minimal() +
  theme(
    panel.grid.major.x = element_blank(),
    panel.grid.minor = element_blank(),
    text = element_text(family = "StaffMeetingPlain"),
    legend.position = c(.95, .95),
    legend.justification = c("right", "top"),
    legend.box.just = "right"
  )
```

</details>

<br>

A *Figura 4* demonstra a audiência (em milhões de espectadores) para cada episódio da série. Todos os episódios, exceto um, apresentam uma média constante e abaixo dos 10 milhões de espectadores. A exceção é ***Stress Relief***, décimo terceiro episódio da quinta temporada, exibido no dia 1 de fevereiro de 2009 - imediatamente após a transmissão do *SuperBowl XLIII* pela NBC.

<img src="{{< blogdown/postref >}}index_files/figure-html/audiencia-coluna-plot-1.png" width="672" style="display: block; margin: auto;" />

<details>
<summary>
Veja o código em R
</summary>

``` r
theoffice_dados %>%
  mutate(temporada = as.factor(temporada)) %>%
  ggplot(aes(
    x = episodio,
    width = 0.5,
    y = qntd_telespectadores_eua_milhoes,
    fill = temporada
  )) +
  geom_col() +
  labs(
    x = "Episodios",
    y = "Audiencia (em milhoes) nos EUA",
    fill = "Temporada",
    caption = "Figura 4: Audiencia de todos os 188 episodios"
  ) +
  scale_y_continuous(limits = c(0, 23), breaks = seq(0, 25, 5)) +
  scale_x_continuous(breaks = seq(0, 188, 20)) +
  scale_fill_manual(
    values = c(
      "#A6CEE3", "#1F78B4", "#B2DF8A",
      "#33A02C", "#FB9A99", "#E31A1c",
      "#FDBF6F", "#FF7F00", "#CAB2D6"
    )
  ) +
  theme_minimal() +
  theme(
    panel.grid.major.x = element_blank(),
    panel.grid.minor = element_blank(),
    text = element_text(family = "StaffMeetingPlain"),
    legend.position = c(.95, .95),
    legend.justification = c("right", "top"),
    legend.box.just = "right"
  )
```

</details>

<br>

***Stress Relief***, escrito por Paul Lieberstein e dirigido por Jeffrey Blitz, foi o episódio mais assistido da série, com 22.91 milhões de espectadores. Por se tratrar de um episódio especial, possui uma hora de duração (com os comerciais inclusos) e contou com participações especiais de Jack Black, Jessica Alba e Cloris Leachman que aparecem em um filme exibido dentro do episódio.

Nele, Dwight encena um incêndio no escritório para testar as habilidades de segurança contra incêndios de seus colegas, mas a situação perde o controle e se torna um caos, especialmente quando Stanley sofre um ataque cardíaco. Para aliviar o estresse no escritório, Michael tenta relaxar seus funcionários com sessões de ioga e meditação. Entretanto, percebe que ele próprio é a fonte do estresse do escritório. Enquanto isso, Andy, Jim e Pam assistem a um filme baixado ilegalmente no trabalho, e Pam precisa lidar com a eminente separação de seus pais. Michael acredita que o estresse no escritório existe porque seus funcionários resistem em expressar seus sentimentos para o chefe, então organiza o *‘The Roast of Michael Scott’*, uma apresentação em que os funcionários podem xingá-lo, sem ressentimentos posteriores. No início ele parece gostar dos insultos, mas termina a apresentação visivelmente chateado e ofendido. Michael volta para o escritório apenas no dia seguinte, onde devolve as piadas para seus colegas de trabalho. Quando chega a vez de Stanley, Michael diz: *“Stanley, you crush your wife during sex and your heart sucks”*[^7], Stanley ri com tanta vontade que acaba quebrando a tensão existente entre os colegas e seu chefe.

### Melhores episódios

Entre todos os programas de TV avaliados pelo site IMDb, **The Office** ocupa a 43ª posição com 8.9 estrelas. Nas avaliações dos usuários do IMDb, a sequência da 3ª, a 4ª e a 5ª temporadas como as três melhores da série, obtendo, respectivamente, 8.59, 8.56 e 8.49 estrelas; por outro lado, a 8ª temporada recebeu a menor avaliação, com 7.58 estrelas. A *Figura 5* apresenta a avaliação média para todas as temporadas.

<img src="{{< blogdown/postref >}}index_files/figure-html/media_imdb_por_temporada-plot-1.png" width="672" style="display: block; margin: auto;" />

<details>
<summary>
Veja o código em R
</summary>

``` r
theoffice_dados %>%
  group_by(temporada) %>%
  summarise(mean_imdb_season = mean(estrelas_imdb)) %>%
  ggplot(aes(x = temporada, y = mean_imdb_season)) +
  geom_col(color = "black",
           fill = RColorBrewer::brewer.pal(9, "Paired")) +
  geom_text(aes(label = round(mean_imdb_season, 2)),
            vjust = -1,
            family = "StaffMeetingPlain") +
  labs(x = "Temporada",
       y = "Estrelas (IMDb)",
       caption = "Figura 5: Media de estrelas (IMDb) recebidas em cada temporada") +
  coord_cartesian(ylim = c(7, 8.8)) +
  scale_x_continuous(limits = c(0, 10), breaks = seq(1, 9, 1)) +
  scale_y_continuous(breaks = seq(7, 8.8, .5)) +
  theme_minimal() +
  theme(
    panel.grid.major.x = element_blank(),
    panel.grid.minor = element_blank(),
    text = element_text(family = "StaffMeetingPlain"),
    legend.position = c(.95, .95),
    legend.justification = c("right", "top"),
    legend.box.just = "right"
  )
```

</details>

<br>

A *Figura 6* realça os 10 episódios - no universo de 188 - com melhor avaliação segundo o IMDb, todos com mais de 9.3 estrelas. Destaque para ***Goodbye, Michael*** e ***Finale***, ambas com 9.8 estrelas; ***Stress Relief*** com 9.7 estrelas; ***Dinner Party*** e ***A.A.R.M*** com 9.5. Menção especial para ***Niagara: Part 1*** e ***Niagara: Part 2***, episódios que retraram o (tão esperado) casamento entre Jim e Pam, e obtiveram 9.4 estrelas cada um.

***Goodbye, Michael*** é o 21º episódio da 7ª temporada, foi exibido no dia 28 de abril de 2011 e é, como o nome já diz, o episódio de despedida de Michael Scott. Escrito por Greg Daniels e dirigido por Paul Feig, marca a última aparição de Steve Carrel (Michael Scott) como parte do elenco regular. Durante 50 minutos, Michael se prepara para partir rumo ao Colorado com sua namorada Holly e passa o último dia no escritório se despedindo individualmente de cada um dos seus colegas; enquanto isso, o novo gerente Deangelo e Andy tentam ficar com os maiores clientes de Michael.

***Finale*** é o 23º episódio da 9ª temporada, foi exibido no dia 16 de maio de 2013 e é o último episódio da série. Foi escrito por Greg Daniels e dirigido por Ken Kwapis (que dirigiu o episódio ***Pilot***). O enredo consiste em um reencontro com os antigos e atuais trabalhadores da *Dunder Mifflin Paper Company* para o casamento de Angela e Dwight e, também, uma rodada final de entrevistas - um ano após a exibição do documentário.

<img src="{{< blogdown/postref >}}index_files/figure-html/top10-estrelas-imdb-plot-1.png" width="672" style="display: block; margin: auto;" />

<details>
<summary>
Veja o código em R
</summary>

``` r
theoffice_dados %>%
  arrange(desc(estrelas_imdb)) %>%
  slice_head(n = 10) %>%
  ggplot(aes(x = estrelas_imdb,
             y = reorder(titulo, +estrelas_imdb))) +
  geom_col(color = "black",
           fill = RColorBrewer::brewer.pal(10, "Paired")) +
  geom_text(aes(label = round(estrelas_imdb, 2)),
            hjust = -0.5,
            family = "StaffMeetingPlain") +
  labs(x = "Estrelas (IMDb)",
       y = "Episodio",
       caption = "Figura 6: Top 10 episodios com melhor avaliacao (IMDb)") +
  coord_cartesian(xlim = c(9, 9.9)) +
  scale_x_continuous(breaks = seq(9, 10, .2)) +
  theme_minimal() +
  theme(
    panel.grid.minor = element_blank(),
    panel.grid.major.y = element_blank(),
    text = element_text(family = "StaffMeetingPlain")
  )
```

</details>

<br>

***Dinner Party*** é o 9º episódio da 4ª temporada, exibido em 10 de abril de 2008. Dirigido por Paul Feig e escrito por Gene Stupnitsky e Lee Eisenberg é [o único roteiro da série que não foi reescrito](https://rollingstone.uol.com.br/noticia/conheca-episodio-perfeito-de-office-o-unico-cujo-roteiro-nao-foi-reescrito/). O episódio mostra a realização de um sonho de Michael Scott: fazer com que o casal Jim e Pam aceitasse um convite para jantar na casa dele. Andy e Angela também são convidados, Dwight (com ciúmes) consegue entrar de penetra com uma companheira (antiga babá e, por vezes, amante dele). O episódio inteiro gira em torno dos personagens anteriormente citados e Jan, companheira de Michael, na casa do casal.

Considerado pela produção e elenco como o “episódio perfeito,” é repleto de momentos extremamente engraçados e (muito) constrangedores. É um dos mais adorados pelos fãs da série, mas, na época em que foi exibido, o público não aceitou bem a ideia. Segundo o diretor Paul Feig:

<br>

> “Nós fizemos as pessoas sentirem-se tão constrangidas que elas simplesmente não aguentavam. Porém, com o passar dos anos, os fãs da série passaram a amar \`Dinner Party’. O que aconteceu agora é que, depois de ver uma vez e saber o que está por vir, você pode realmente aproveitá-lo.”

<br>

Em compensação, grande parte da crítica especializada considerou o episódio como um dos melhores de **The Office**. Travis Fickett da IGN escreveu que [“este é um daqueles grandes episódios de **The Office** que é histérico e difícil de assitir ao mesmo tempo”](https://www.ign.com/articles/2008/04/11/the-office-dinner-party-review).

É interessante observar que os dois episódios com melhor avaliação (***Goodbye, Michael*** e ***Finale***) carregam uma carga emotiva superior à comédia, uma vez que ambos tratam de duas despedidas: o primeiro, de Michael; o segundo, da série em si.

<img src="{{< blogdown/postref >}}index_files/figure-html/estrelas_imdb_histograma-plot-1.png" width="672" style="display: block; margin: auto;" />

<details>
<summary>
Veja o código em R
</summary>

``` r
theoffice_dados %>%
  ggplot() +
  geom_histogram(
    aes(x = estrelas_imdb),
    bins = 12,
    color = "black",
    fill = RColorBrewer::brewer.pal(12, "Paired")
  ) +
  labs(x = "Estrelas (IMDB)",
       y = NULL,
       caption = "Figura 7: Histograma (distribuicao) de todas estrelas (IMDb)") +
  scale_x_continuous(breaks = seq(6, 10, .5)) +
  scale_y_continuous(breaks = seq(0, 40, 10)) +
  theme_minimal() +
  theme(
    panel.grid.major.x = element_blank(),
    panel.grid.minor = element_blank(),
    text = element_text(family = "StaffMeetingPlain"),
    legend.position = c(.95, .95),
    legend.justification = c("right", "top"),
    legend.box.just = "right"
  )
```

</details>

<br>

Nas avaliações de todos os 188 episódios, há um intervalo de avaliação entre 6.5 e 9.8 estrelas, sendo uma predominância na quantidade de estrelas entre 7.75 e 8.75. O histograma acima (*Figura 7*) revela a frequência das estrelas dadas pelos usuários do IMDb para todos os episódios.

### Roteiristas

Durante os nove anos em que esteve no ar, 40 roteiristas diferentes assinaram ao menos um episódio do seriado. O gráfico abaixo (*Figura 8*) realça os 10 roteiristas que foram mais vezes creditados.

**Mindy Kaling** encabeça a lista. Ela, que além de interpretar a personagem Kelly Kapoor, roteirizou 22 episódios, entre eles: *Niagara: Part 1*, *Niagara: Part 2*; escritos juntamente com Greg Daniels.

Em segundo, **Paul Lieberstein**, interprete do depressivo representante do RH no escritório e odiado por Michael Scott - Toby Flenderson -, participou de 16 roteiros; entre eles, *Stress Relief*, o episódio mais visto de toda a série e o terceiro melhor avaliado segundo IMDb.

<img src="{{< blogdown/postref >}}index_files/figure-html/roteiristas-plot-1.png" width="672" style="display: block; margin: auto;" />

<details>
<summary>
Veja o código em R
</summary>

``` r
theoffice_dados %>%
  separate(
    col = roteiro,
    into = c("roteirista1", "roteirista2", "roteirista3"),
    sep = ", "
  ) %>%
  select(episodio, titulo, starts_with("roteirista")) %>%
  pivot_longer(
    cols = starts_with("roteirista"),
    names_to = "roteirista_num",
    values_to = "roteirista_nome"
  ) %>%
  count(roteirista_nome) %>%
  na.omit() %>%
  rename(roteirista = roteirista_nome, qnt_episodios = n) %>%
  arrange(desc(qnt_episodios)) %>%
  slice_head(n = 10) %>%
  ggplot(aes(x = qnt_episodios,
             y = reorder(roteirista, +qnt_episodios))) +
  geom_col(color = "black",
           fill = RColorBrewer::brewer.pal(10, "Paired")) +
  geom_text(aes(label = round(qnt_episodios), 2),
            hjust = -0.5,
            family = "StaffMeetingPlain") +
  labs(x = "Quantidade de episodios",
       y = "Roteiristas",
       caption = "Figura 8: Top 10 roteiristas que escreveram episodios") +
  coord_cartesian(xlim = c(9, 23)) +
  scale_x_continuous(breaks = seq(10, 23, 3)) +
  theme_minimal() +
  theme(
    panel.grid.minor = element_blank(),
    panel.grid.major.y = element_blank(),
    text = element_text(family = "StaffMeetingPlain")
  ) 
```

</details>

<br>

**B. J. Novak**, o estagiário Ryan Howard, roteirizou 15 episódios; além de roteirista, Novak foi a primeira pessoa a entrar para o elenco. Ele é responsável por assinar o episódio *Threat Level Midnight*, o sexto melhor segundo o ranking do IMDb.

**Greg Daniels**, o responsável por desenvolver o seriado americano, assina 13 roteiros, entre eles: *Pilot*, *Niagara: Part 1*, *Niagara: Part 2*, *Goodbye, Michael* e *Finale*. Destes 5 episódios citados, 4 estão entre os 10 com melhor avaliação do IMDb.

**Michael Schur**, que também está no elenco como o Mose - primo do Dwight -, fecha o TOP 10 com dez episódios escritos.

### Diretores

Ao todo, 55 diretores participaram da série nesssas nove temporadas. **Randall Einhorn** e **Paul Feig** encabeçam a lista com 15 episódios cada. Paul Feig é o responsável pela direção de 4 dos 10 episódios com melhor avalição, são eles: *Goodbye, Michael*, *Dinner Party*, *Niagara: Part 1* e *Niagara: Part 2*. Ken Kwapis dirigiu 13 episódios, entre eles: *Pilot*, *Diversity Day* e *Finale* - primeiro, segundo e último episódios, respectivamente.

A lista segue com Greg Daniels, Jeffrey Blitz, Ken Whittingham, David Rogers, Matt Sohn, Charles McDougall e Paul Lieberstein fecha o Top 10.

<img src="{{< blogdown/postref >}}index_files/figure-html/diretores-plot-1.png" width="672" style="display: block; margin: auto;" />

<details>
<summary>
Veja o código em R
</summary>

``` r
theoffice_dados %>%
  separate(col = direcao,
           into = c("diretor1", "diretor2"),
           sep = ", ") %>%
  pivot_longer(cols = starts_with("diretor"),
               names_to = "diretor_num",
               values_to = "diretor_nome") %>%
  rename(direcao = diretor_nome) %>%
  count(direcao) %>%
  na.omit() %>%
  rename(qnt_episodios = n) %>%
  arrange(desc(qnt_episodios)) %>%
  slice_head(n = 10) %>%
  ggplot(aes(x = qnt_episodios,
             y = reorder(direcao, +qnt_episodios)),
         fill = "cyan") +
  geom_col(color = "black",
           fill = RColorBrewer::brewer.pal(10, "Paired")) +
  geom_text(aes(label = qnt_episodios),
            hjust = -0.5,
            family = "StaffMeetingPlain") +
  labs(x = "Quantidade de episodios",
       y = "Diretores",
       caption = "Figura 9: Top 10 diretores que dirigiram episodios") +
  coord_cartesian(xlim = c(0, 16)) +
  scale_x_continuous(breaks = seq(0, 15, 3)) +
  theme_minimal() +
  theme(
    panel.grid.minor = element_blank(),
    panel.grid.major.y = element_blank(),
    text = element_text(family = "StaffMeetingPlain")
  )  
```

</details>

<br>

Outros integrantes do elenco regular também dirigiram ao menos um episódio, como: Mindy Kaling, Ed Helms, Brian Baumgartner, Steve Carell, John Krasinski, Rainn Wilson e B.J. Novak. Além destes, profissionais consagrados em outros trabalhos, contribuíram para a direção de episódios em **The Office**, por exemplo: Bryan Cranston, J.J. Abrams e Marc Webb entre outros.

### Elenco

Muitos atores e atrizes fizeram parte do elenco ao longo dos 188 episódios, seja para em um papel regular, uma participação especial ou apenas uma aparição relâmpago (Conan O’ Brian, por exemplo). A *Tabela 1* registra as participações das 787 pessoas que apareceram na série, em ordem decrescente - de quem possui mais aparições para quem possui menos.

O elenco regular (funcionários do escritório) se concentram nas primeiras posições da tabela. Angela Kinsey, Brian Baumgartner, Jenna Fischer, John Krasinski, Leslie David Baker, Phyllis Smith e Rainn Wilson foram creditados em todos os episódios.

Rashinda Jones entrou na terceira temporada, juntamente com Ed Helms, e permaneceu somente por 26 episódios. Saindo para compor o elenco de *Parks and Recreation*, série criada pelos mesmos produtores de **The Office**.

<br>

<div id="htmlwidget-1" style="width:100%;height:auto;" class="datatables html-widget"></div>
<script type="application/json" data-for="htmlwidget-1">{"x":{"filter":"none","vertical":false,"caption":"<caption>Tabela 1: Número de episódios em que cada ator está creditado<\/caption>","data":[["1","2","3","4","5","6","7","8","9","10","11","12","13","14","15","16","17","18","19","20","21","22","23","24","25","26","27","28","29","30","31","32","33","34","35","36","37","38","39","40","41","42","43","44","45","46","47","48","49","50","51","52","53","54","55","56","57","58","59","60","61","62","63","64","65","66","67","68","69","70","71","72","73","74","75","76","77","78","79","80","81","82","83","84","85","86","87","88","89","90","91","92","93","94","95","96","97","98","99","100","101","102","103","104","105","106","107","108","109","110","111","112","113","114","115","116","117","118","119","120","121","122","123","124","125","126","127","128","129","130","131","132","133","134","135","136","137","138","139","140","141","142","143","144","145","146","147","148","149","150","151","152","153","154","155","156","157","158","159","160","161","162","163","164","165","166","167","168","169","170","171","172","173","174","175","176","177","178","179","180","181","182","183","184","185","186","187","188","189","190","191","192","193","194","195","196","197","198","199","200","201","202","203","204","205","206","207","208","209","210","211","212","213","214","215","216","217","218","219","220","221","222","223","224","225","226","227","228","229","230","231","232","233","234","235","236","237","238","239","240","241","242","243","244","245","246","247","248","249","250","251","252","253","254","255","256","257","258","259","260","261","262","263","264","265","266","267","268","269","270","271","272","273","274","275","276","277","278","279","280","281","282","283","284","285","286","287","288","289","290","291","292","293","294","295","296","297","298","299","300","301","302","303","304","305","306","307","308","309","310","311","312","313","314","315","316","317","318","319","320","321","322","323","324","325","326","327","328","329","330","331","332","333","334","335","336","337","338","339","340","341","342","343","344","345","346","347","348","349","350","351","352","353","354","355","356","357","358","359","360","361","362","363","364","365","366","367","368","369","370","371","372","373","374","375","376","377","378","379","380","381","382","383","384","385","386","387","388","389","390","391","392","393","394","395","396","397","398","399","400","401","402","403","404","405","406","407","408","409","410","411","412","413","414","415","416","417","418","419","420","421","422","423","424","425","426","427","428","429","430","431","432","433","434","435","436","437","438","439","440","441","442","443","444","445","446","447","448","449","450","451","452","453","454","455","456","457","458","459","460","461","462","463","464","465","466","467","468","469","470","471","472","473","474","475","476","477","478","479","480","481","482","483","484","485","486","487","488","489","490","491","492","493","494","495","496","497","498","499","500","501","502","503","504","505","506","507","508","509","510","511","512","513","514","515","516","517","518","519","520","521","522","523","524","525","526","527","528","529","530","531","532","533","534","535","536","537","538","539","540","541","542","543","544","545","546","547","548","549","550","551","552","553","554","555","556","557","558","559","560","561","562","563","564","565","566","567","568","569","570","571","572","573","574","575","576","577","578","579","580","581","582","583","584","585","586","587","588","589","590","591","592","593","594","595","596","597","598","599","600","601","602","603","604","605","606","607","608","609","610","611","612","613","614","615","616","617","618","619","620","621","622","623","624","625","626","627","628","629","630","631","632","633","634","635","636","637","638","639","640","641","642","643","644","645","646","647","648","649","650","651","652","653","654","655","656","657","658","659","660","661","662","663","664","665","666","667","668","669","670","671","672","673","674","675","676","677","678","679","680","681","682","683","684","685","686","687","688","689","690","691","692","693","694","695","696","697","698","699","700","701","702","703","704","705","706","707","708","709","710","711","712","713","714","715","716","717","718","719","720","721","722","723","724","725","726","727","728","729","730","731","732","733","734","735","736","737","738","739","740","741","742","743","744","745","746","747","748","749","750","751","752","753","754","755","756","757","758","759","760","761","762","763","764","765","766","767","768","769","770","771","772","773","774","775","776","777","778","779","780","781","782","783","784","785","786","787"],["Angela Kinsey","Brian Baumgartner","Jenna Fischer","John Krasinski","Leslie David Baker","Phyllis Smith","Rainn Wilson","Kate Flannery","Creed Bratton","Oscar Nuñez","B.J. Novak","Mindy Kaling","Ed Helms","Steve Carell","Paul Lieberstein","Craig Robinson","Ellie Kemper","Zach Woods","Melora Hardin","Andy Buckley","Catherine Tate","David Denman","Rashida Jones","James Spader","Robert R. Shafer","Hugh Dane","Jake Lacy","Clark Duke","Mark Proksch","Amy Ryan","David Koechner","Calvin Tenner","Ameenah Kaplan","Jack Coleman","Michael Schur","Lindsey Broad","Linda Purl","Charles Esten","Hidetoshi Imura","Kathy Bates","Nancy Carell","Idris Elba","Karly Rothenberg","Sam Richardson","Stephen Saux","Ursula Burton","Amy Pietz","Chris Diamantopoulos","Joanne Carlsen","Kelen Coleman","Lee Eisenberg","Nora Kirkpatrick","Sienna Paige Strull","Adam Lustick","Algerita Wynn","Bailey Kate Strull","Ben Silverman","Blake Robbins","Devon Abner","Eleanor Seigler","Gene Stupnitsky","James Urbaniak","Lisa K. Wyatt","Marcus A. York","Michael Tuba Heatherton","Nelson Franklin","Noel Petok","Perry Smith","Rick Overton","Suzanne Watson","Tom Chick","Trish Gates","Will Ferrell","Allan Havey","Amy Adams","Cody Horn","Dan Cole","Delaney Ruth Farrell","Georgia Engel","Hudson Phillips","Jackie Debatin","John Hartmann","Kamala Jones","Kelly Daly","Matt Jones","Michael Patrick McGill","Mike Bruner","Nicholas D'Agosto","Patrice O'Neal","Rob Huebel","Robert Pine","Sam Daly","Sendhil Ramamurthy","Spencer Daniels","Taylar Hollomon","Aaron Rodgers","Adam Jamal Craig","Alec Gray","Ali Louise Hartman","Andrew Ortiz","Angelina Ganiere","Anna Camp","Beth Grant","Blake Garrett Rosenthal","Chris Gethard","Cici Leah Campbell","Claire Scanlon","Clay Aiken","Dale Raoul","Daniel Amerman","Elvy","Emily Evan Rae","Eric Wareheim","Fred Cross","Jahmilla Jackson","Jake Kalender","Jazz Raycole","Jean Villepique","Jennifer Ann Burton","Jerry Minor","Jon Morgan Woodward","Josh Groban","Katherine Flynn","Keeshan Giles","Ken Kreps","Lance Krall","Larry Wilmore","Lori Murphy Saux","Mary Gillis","Michael Daniel Cassady","Mike E. Winfield","Omi Vaidya","Owen Daniels","Peggy Stewart","Phil Abrams","Ranjit Chowdhry","Rich Sommer","Ricky Gervais","Roseanne Barr","Santigold","Sean L. Malin","Susanne Allan Hartman","Timothy Olyphant","Tina Huang","Tug Coker","Vivianne Collins","Wayne Wilderson","A.J. Adelman","Aaron Pushkar","Aaron Shure","Abe Spigner","Abraham Chaidez","Aimee Shyn","Alan Fudge","Alan Mueting","Alexander McCaslin","Alexander Sibaja","Alexis Teague","Alfred Rubin Thompson","Alina Andrei","Alison Martin","Alison White","Allan Wasserman","Allison Jones","Allison Silverman","Allyson Everitt","Alyssa Larsen","Alyssa Preston","Amanda Warren","Amelie Gillette","Americus Abesamis","Amy Cale Peterson","Amy Heidt","Amy Hill","Amy Rieckelman","Amy Weaver","Analeis Lorig","Ananya Kepper","Andi Carnick","Andrea Kelley","Andrew Daly","Andrew Donnelly","Andrew O'Shanick","Andrew Santino","Andrew Secunda","Angel Tyson","Angela Campolla-Sanders","Angela Shin","Angelo Middione","Anita Kapila","Ann Maddox","Annabelle Kopack","Anne Santiago","Annie Bravo","Annie Sertich","Anthony Russell","Antony Teofilo","April Eden","Ashley Adler","Ashley Walsh","Austin Michael Scott","Ava Nisbet","Avu Chokalingam","Barak Hardley","Barbara A. Fisher","Barbara Allyne Bennet","Barry Sigismondi","Basilina Butler","Beau Wirick","Ben Carroll","Ben Harris","Ben Kacsandi","Ben Wang","Benjamin Scott Panock","Bennie the Cat","Bethany Dwyer","Betty Murphy","Bill Bryce","Bill Coelius","Bill Hader","Blaise Godbe Lipman","Bob Gebert","Bob Glouberman","Bob Harrell","Bob Odenkirk","Bonita Dorssom","Boris Kievsky","Brad Graiff","Brad Morris","Brad Slaughter","Brad William Henke","Brady Rubin","Brandon Borror-Chappell","Brandon Slagle","Brenda Withers","Brent Forrester","Brett Gelman","Brett Gipson","Brey Chanadet","Brian A. Gutierrez","Brian Gattas","Brian Hatton","Brian Patrick Mulligan","Brian Stack","Brian Wittle","Britain Spellings","Brittany Ishibashi","Brooke Dillman","Bruno Gunn","Bruno Oliver","Caitlin Williams","Candace Sciarra","Carla Rudy","Carolyn Wilson","Carrie Clifford","Carrie Kemper","Carter Hastings","Casey Washington","Cassie Fliegel","Cassie Jordan","Cham","Charles C. Stevenson Jr.","Charles Hirsch","Charles Miller","Charlie Hartsock","Charlie McDermott","Charlie Sanders","Charlotte Daniels","Charlotte Stewart","Chatree 'Chad' Yodvisotsak","Chealy Jean","Chelsey Crisp","Chris Bauer","Chris Ellis","Chris Haston","Chris Moss","Christian S. Anderson","Christian Slater","Christina de Leon","Christopher Gay","Christopher Nicholas Smith","Christopher Raczynski","Christopher T. Wood","Cindy Buck","Cindy Drummond","Clifford Bañagale","Clint Corley","Cloris Leachman","Cole Coleman","Colleen Smith","Collette Wolfe","Conan O'Brien","Connie Sawyer","Constance Broge","Constance Jewell Lopez","Cooper Thornton","Cora Skinner","Craig Anton","Cris D'Annunzio","Crystal Havens","Dakota Johnson","Dale Waddington","Damani Roberts","Dan Bakkedahl","Dan Castellaneta","Dan Desmond","Dan Gill","Dan Goor","Dan Levy","Dan Sterling","Dana Powell","Daniel Hepner","Danielle E. Hawkins","Danilo Di Julio","Dante Acosta","Darren Bailey","Dave Anthony","David Britz","David Costabile","David Daskal","David Doty","David Ferguson","David Grant Wright","David Kirk Grant","David L. Marston","David Mate","David Mazouz","David Rogers","Deb Hiett","Deborah Puette","Debra Leigh","Dee Ryan","Dee Wallace","Deena Adar","Denise Gossett","Denise Vasquez","Dennis Garber","Deonte Gordon","Donna Bryce","Donna Lazar","Donovan Estrada","Dorian Frankel","Douglas Sarine","Drew Powell","Duane R. Shepard Sr.","Ed Begley Jr.","Ed Lauter","Eduardo Antonio Garcia","Edward James Gage","Eli Vargas","Eliza Coleman","Elizabeth Moore","Elizabeth Payne","Emerson Brooks","Emil Beheshti","Eric Bradley","Eric Christie","Eric La Barr","Eric Watson","Eric Zuckerman","Erica Hanrahan-Ball","Erica Mer","Erica Tazel","Erica Vittina Phillips","Erin Pickett","Eugene Cordero","Evan Gaustad","Evan Peters","Eve Sigall","Fay DeWitt","Fletcher Sheridan","Frank Birney","Frank Maharajh","Frederik Pohl IV","Fredrick Burns","Gagandeep Bedi","Gary Buckner","Gary Weeks","Genevieve Levin","Geoffrey Gould","George Gaus","George Ives","George Thomas Mansel","Gil Glasgow","Graham Wagner","Greg Collins","Greg Daniels","Greg Tuculescu","Greg Worswick","Gregory Graham","Gregory Schmauss","Griffin Gluck","Gustin Smith","Haley Daniels","Hannah Baker","Hansford Rowe","Heather Marie Marsden","Heidi Edsall","Henriette Mantel","Holly Maples","Ian Novotny","Isabel Schnebelie","Isheba Renee","Ithamar Enriquez","Jack Axelrod","Jack Black","Jade Catta-Preta","Jaime Jorn","Jake Lucas","Jake Radaker","James Gregory Capps","James M. Halty","James McMann","James O. Kerry","Jamila Webb","Janell Winkler","Janet Hoskins","Janet Song","Janine Poreba","Jason Kessler","Jason Rogel","Jason Walsh","Jay Falk","Jaysha Patel","Jeff Bee","Jeff Hatch","Jeff Loveness","Jeff Witzke","Jeffrey Muller","Jen Reiter","Jennie Tan","Jennifer Andreacola","Jennifer Celotta","Jennifer Hale","Jennifer Hasty","Jennifer Peo","Jenny Leonhardt","Jera Sky","Jeremy Radin","Jeremy Shouldis","Jerome Bettis","Jesse Mackey","Jessica Alba","Jessica St. Clair","Jill Maragos","Jim Carrey","Jim Jansen","Jim Petersmith","Jim Wisniewski","Jim Woods","Joan Cusack","Jobeth Wagner","Joe Davis","Joel-Ryan Armamento","Joey Basu","Joey Slotnick","John Alton","John C. McLaughlin","John F. Schaffer","John Gemberling","John H. Tobin","John Harrington Bland","John Ingle","John Kelly","John McColgan","John Phillips","Johnnie Battistessa","Jonah Platt","Jonathan Browning","Jonathan Hughes","Jonathan Nail","Jonathan Pintoff","Josh Hodell","Julia Cho","Julia Prud'homme","Julie David","Julie Dove","Julie Remala","Julien Zuccolin","Julius Erving","June Squibb","Justin Meloni","Justin Spitzer","Kaily Smith Westbrook","Kami Koren","Kara Elizabeth Silva","Karishma Sawhney","Kat Ahn","Kate Comer","Kate Quigley","Katharine Leonard","Katie Aselton","Katy Bodenhamer","Kaye Marie Talise","Kayla Mae Maloney","Keili Lefkovitz","Keith Valcourt","Kelii Miyata","Kelly Cantley-Kashima","Kelly Ebsary","Kelly Snow","Kelsea Button","Ken Bernfield","Ken Howard","Ken Jeong","Kendra Cannoy","Kenny Cooper","Kevin Carlson","Kevin Dorff","Kevin McHale","Kie Spring","Kim Kim","Kim Stodel","Kimberly Douglas","Kimberly Evan","Kimberly Manion","Kristin Mellian","Kristin Quick","Kulap Vilaysack","Kunal Sharma","Kurt Scholler","Kwame Boateng","Kyle Bornheimer","Kyle McLaughlin","L.A. Landgraf","Lamont Ferrell","Lance Patrick","Landon H. Lewis Jr.","Larkin Campbell","Laura Jean Leal","Laurel Coppock","Lauren Rissman","Laurie Okin","Lee Kirk","Leon Simmons","Lianne Lin","Linda Taylor","Linda Weinrib","Lindsey Stoddart","Lisa Darr","Liz Ross","Lynnanne Zager","Lynsey Nicole Harris","Maile Flanagan","Majandra Delfino","Malcolm Barrett","Marilyn Brett","Mark Heidelberger","Mark McGrath","Mark Parrish","Mark Tomesek","Marla Garlin","Martha Middleton","Mary Kathleen Gordon","Mary Wall","Matt DeCaro","Matt McKane","Matt Price","Matt Prokop","Matt Selman","Matt Sohn","Matt Warburton","Matthew Brent","Matthew Craig","Matthew Lenhart","Maura Tierney","Max Carver","Maxwell Glick","Melinda Chilton","Melinda Haugh","Melissa Bickerton","Melissa Rauch","Micah Williams","Michael 'Mick' Harrity","Michael A. Templeton","Michael August","Michael Blake","Michael Imperioli","Michael Kaiser","Michael Lanahan","Michael McCartney","Michael Naughton","Michael Patrick Breen","Michael Potter","Michael Rosinsky","Michael Weston","Michelle Faraone","Michelle Gunn","Mike Kruzel","Mike McCaul","Mike Nojun Park","Mike Starr","Mike Storc","Miriam Tolan","Miss Kitty the Cat","Mitch Poulos","Molly Bryant","Molly Burk","Monnae Michaell","Myles Cranford","Nakul Dev Mahajan","Nancy Lantis","Natalie Bain","Nathan Blank","Nicholas Costello","Nicholas Daly Clark","Nicholas Rutherford","Nicholas Shaffer","Nicholas Strong","Nick Armstrong","Nick Cafero","Nick Lashaway","Nico Evers-Swindell","Nikki McCauley","Noah Blake","Oliver Vaquer","Oscar Blanco","Patrick Bradford","Patrick Faucette","Patrick LoSasso","Patrick O'Connor","Paul Faust","Paul Feig","Paul Sass","Pete Pastore","Peter A. Hulne","Peter Gannon","Phil Hawn","Phil Reeves","Porter Kelly","Potsch Boyd","R.C. Ormond","R.F. Daley","Rachael Harris","Rachel Crow","Rae Latt","Ralph Kampshoff","Randall Barnwell","Randall Park","Randy Cordray","Randy Guiaya","Randy Vinneau","Ransford Doherty","Ray Romano","Reid Gormly","Rene Gube","Renee Riess","Richard Augustine","Richard Schimmelpfenneg","Rick L. Dean","Rick Scarry","Rita Sehmi","Rob Brownstein","Rob Riggle","Robbie Kaller","Robert Bagnell","Robert Foreman","Robert Mammana","Robert Stilwell","Robin Dale Meyers","Robin Lynch","Robin Swenson","Rodger Arlen","Rodney J. Hobbs","Rohan Vora","Romel De Silva","Ron Canada","Roscoe Myrick","Ross Mackenzie","Ryan Bailey","Ryan Howard","Ryan Martin","Sahlima","Sandra L. Feeley","Sandra Tsing Loh","Sangita Sanyal","Sara Chase","Sara Van Horn","Sarah Baker","Sarah Baldwin","Sarah Bastian","Sarah Zimmerman","Sarah Zinsser","Scot Robinson","Scott Adsit","Scott Beehner","Scott Damian","Scott Thewes","Seamus McCarthy","Sean Bury","Sean Davis","Sean McDonald","Sean R. Lake","Selah Victor","Seth Bailey","Seth Coltan","Seth Meyers","Sethward","Shannon Cochran","Shannon Mary Dixon","Sharon Blackwood","Sherrie Lewandowski","Sherry Landry","Shira Scott Astrof","Siddharth Jain","Silvia McClure","Skyler Caleb","Spencer John Olson","Stanley Ullman","Stefan Kumor","Stephanie Lesh-Farrell","Stephanie McVay","Stephen Colbert","Stephen Collins","Stephen Mitchell","Stephen Pisani","Steve Hely","Steve Little","Steve Moore","Steve Seagren","Steve Zissis","Steven Adkins","Steven Cortinas","Stewart Skelton","Sue Nelson","Sue Redman","Summer Malone","Sunah Bilsted","Susan Foley","Susan Pinckney","Susanne Daniels","Susie Geiser","Swati Chokalingam","Taji Coleman","Takayo Fischer","Tanveer K. Atwal","Tate Hanyok","Teena Strube","Terrence Beasor","Terry James","Thaddeus John Potter","Thomas F. Evans","Thomas Fowler","Thomas Michael Ventimiglio","Thomas Middleditch","Tig Notaro","Tim Kang","Tim Meadows","Timothy Michael Gould","Toby Huss","Todd Aaron Brotze","Todd Jeffries","Tom Bower","Tom Konkle","Tom Virtue","Tom Yi","Tommy Gerrits","Tony Pasqualini","Travis Samuel Clark","Travis Seaborn","Trevor Anthony Pitzel","Trevor Einhorn","Trish Suhr","Tucker Albrizzi","Valeri Ross","Vali Chandrasekaran","Vanessa Ragland","Varun Gurunath","Victor Taylor","Vincent Angelo","Virginia Newcomb","Wally Amos","Ward Edmondson","Warren Buffett","Warren Lieberstein","Warren Sweeney","Wendi McLendon-Covey","Wendy McColm","Weston Nathanson","Will Arnett","Will C.","Will Greenberg","Will McCormack","Wyatt Cenac","Yvette Nicole Brown","Zabeth Russell","Zoe Jarman"],["Angela Martin | Angela Martin-Lipton | Angela Schrute","Kevin Malone","Pam Beesly","Jim Halpert","Stanley Hudson","Phyllis Lapin | Phyllis Vance","Dwight Schrute","Meredith Palmer","Creed Bratton","Oscar Martinez","Ryan Howard","Kelly Kapoor","Andy Bernard","Michael Scott","Toby Flenderson","Darryl Philbin","Erin Hannon","Gabe Lewis","Jan Levinson | Jan Levinson-Gould","David Wallace","Nellie Bertram","Roy Anderson","Karen Filippelli","Robert California","Bob Vance","Hank","Pete Miller","Clark","Nate","Holly Flax","Todd Packer","Calvin | Glenn","Val","State Senator Rob Lipton","Mose","Cathy Simms | Cathy","Helene Beesly","Josh Porter","Hidetoshi Hasagawa","Jo Bennett","Carol Stills","Charles Miner","Madge","Colin, Athlead Employee","Justin Spitzer","Hannah Smotridge-Barr","Donna | Donna Newton","Brian | Cameraman","Teri Hudson","Isabel Poreba","Gino","Esther","Cecelia Halpert","Athlead Employee","Cynthia","Cecelia Halpert","Isaac","Tom Halpert","Devon White | Devon","Jessica","Leo","Rolf","Lynn","Billy Merchant","The Minister","Nick | Graphic Design Guy","Troy Undercook | Troy Underbridge","Betsy Halpert | Susan","Mr. Beesly","Stephanie","Gil","Stacy","Deangelo Vickers","Mr. Bruegger","Katy","Jordan Garfield","Dan Gore","Sasha Flenderson | Sasha","Irene","Athlead Office Employee","Elizabeth the Stripper","Kendall","Edna","Bertha","Zeke","Kenny Anderson","Tony Gardner","Hunter","Lonny","A. J.","Gerald Halpert","Matt","Ravi","Jake Palmer | Jakey the Stripper","Jada","Aaron Rodgers","Rolando","Blonde Kid | Bumble Bee Trick-or-Treater","Phillip Halstead Lipton","Mole","Dwight's Younger Sister | Little Girl Having Picnic","Penny Beesly","Melvina | Dwight's Babysitter","Cameron | Kid on Hay Ride","Trevor","Shrute Family Member","Warehouse party guest | Nashua Employee","Clay Aiken","Ronni","Russell","Megan","Rebecca Prince","Gabor","Wesley Silver","Darryl's Sister","Young Michael Scott","Melissa Hudson","Rachel Wallace","NY Corporate Employee","Brandon","Shareholder | Albany Employee","Walter Bernard, Jr.","Receptionist","Hospital Employee | Orderly","The Minister","Sensei Ira","Mr. Brown","Nellie's Mom | Announcer","Aunt Shirley","Gideon",null,"Sadiq","Teddy | Eight-Year-Old Kid","Sylvia","Shelby Thomas Weems","Vikram","Alex","David Brent","Carla Fern","Santigold","Failed Finalist | Andy's Competition","Phillip Halstead Lipton","Danny Cordray","On-Air Reporter","Pete Halpert","Headquarter Receptionist","Martin","Bar Patron","Bartender","Waiter","Shawn","Felipe","Receptionist","Alan Brand","Shareholder",null,"Neighbour","Ashley","Store Owner","Party Girl",null,"Purchaser's Wife","Nick Figaro","Woman Guarding PBS Swag at Warehouse","Audience Member","Chili's Hostess",null,"Jessica",null,"Amelie","Bouncer","Laurie","Cellphone's POS Attendant","Nail Salon Manager","Annie","Keena Gifford","Athlead Employee","Tiffany",null,"Student","Ben Franklin | Gordon","Mike","Treble","Buyer of Andy's boat","Volunteer for Dogs' Care",null,"Receptionist","Manicurist | Asian Lady","Bass Player - Kevins Garage Band","Bollywood Dancer","Telemarketer","Fairy Princess Trick-or-Treater","Line Coordinator","Business School Student","Justin's Date","Chad Lite","Penguin","Deborah Shoshlefski","Office worker","Dog Groomer Woman","Underage Kid #2","Sherry Trick-or-Treater","Kelly's Dad",null,"Usher","Old Lady","WB Jones","Nurse","Frat Guy","Master of Ceremonies","Todd","Trivia Contestant","Chinese Restaurant Waiter","Bouncer","Cat","Beth","Irate Shareholder",null,"Restaurant Manager","Bill Hader","Brett Bailey","Mr. Schofield","Mr. Romanko",null,"Mark","Shareholder","Cleaner","Young Guy","Glenn","Hot Yogi","Frank","Woman on Park Bench","Treble","Trivia Player","Brenda","Audience Member","The Magician","Law Intern","Schrute Kid","Drake","Xander","Randy","Mark","Buffalo branch employee",null,"Craps Dealer","Cindy","Margaret","Water Delivery Man","Mr. Barr","Tiffany","Olympic Figure Skater","Last Supper Tableau","Minister Gail","Nurse Ruth","Molly",null,null,"Tall Girl #1","Stanley's Date","Acapella Idol Contestant",null,"Busines School Student","Treble","Travel Agent","Student","Cake Delivery Man","Flower Girl","Woman","The Chef","Waitress","Convention Worker","Harry Jannerone","Chris O'Keefe","The Photographer","Dancer",null,"Self","Pratt Student",null,null,"Police Officer","Chilli's Manager","Audience Member","Ryan's Mom","Filipino Teen","Lawyer","Lily Hanaday","Sales Analyst playing arcade game","Stephanie","Alice","Conan O'Brien","Nana","The Storyteller",null,"Dennis","Waitress","Craig","Joe The Hot Dog Vendor","Cousin's Wife","Dakota, Kevin's Replacement","Kaitlyn","Chet Montgomery","Roger Prince, Jr.","Mr. Ramish","Roger Prince, Sr.","Philadelphia's Trivia Host","Bar Patron","Stonewall Alliance's Trivia Host","Audience Member","Megan","Groomsman","Dancer | Rapper","Economy Passenger Phil Shea","Blazer","Airplane Passenger","The Client","Caterer","Eric Ward","Textbook Business School Student","The Doctor","Skateland DJ","Mr. Sylvie","Henry Saine (a.k.a. Conrad)","Bart Heart in Second Life","Flex","Bert California","Guy Wearing Ray-Bans","Stewardess Beth","Alice","Sheila Davis","Marybeth","Ellen Bernard","Woman at Bar",null,"Shareholder","James P. Albini","Reporter #1","Mrs. Anderson","Woman in Crowd","Pure Bred","Woman",null,"The Bartender",null,"Erin's Biological Father","Sam Stone, Sr.","Ernesto","Larry Meyers",null,"Helmsman",null,null,"Air Force Recruiter","Doug McPherson","Here Comes Treble Singer","Al","Cashier","Simon Realty Employee","Store Salesman","Johanna","Blue Shirted Kid #2","Julia","Justine","Shareholder","Gas Station Attendant","Dale","Luke Cooper","Senior Home Resident","Lady at the Gym","Here Comes Treble Singer","Old Man","Dandy Dale","Neighbor","Student","Ravi","Funeral Home Director","Cop #1","Famous Amos Girl","Presentation Attendee","Lion Trick-or-Treater","Uncle Al","Cowboy","Dwight and Jim's Customer","Wedding Guest","Assassinsode, 201","Michael's neighbor","Tom","Kenny","Upset Bar Patron","Angry Shareholder","Half Bred","Rich Jones","Abby","Underage Girl","Albert Lapin","Mrs. Lovett | Kevin's Sister","Young Mom","Office Worker","Diane Kelly","Conference Goer",null,"Mikela Lasker","Paramedic",null,"Sam","Jeb's Wife","Tattoo Artist","Waiter #2","Boy at Table","Paramedic","Bartender","Warehouse Guy","Delivery Man","Fast Food Manager","Patty Grossman","Seminar Attendee",null,"Amy","Flower Delivery Man #1","Erik",null,"Halpert's Home Purchaser","Neepa","Rafe","Limo Driver",null,"Commercial Actor","Mark",null,"Audience Member","Game Player at Picnic","Wedding Guest","Donna Muraski","Receptionist","Cocktail Waitress","Yoga Student","Play Patron",null,"Busines School Student","Jerome Bettis","Jumpsuit Guy","Sophie","Casey Dean","Mom","The Fingerlakes Guy","High School Principal","Professor",null,"Bowling Alley Manager","Erin's Biological Mother","Pratt Art Student","Joe","The Prop Man","Partygoer","Jerry","FAX Machine Guy",null,"Photographer","Caterer Greeting Guests","Dealer","Bill","Robert Dunder",null,null,"The Director","Featured Kid","Frat Boy","Steve",null,"Tourist","Bassist","Josh Hodell","Asian Woman #1","The Technician","Bridesmaid","Photographer Toni","Nurse","Florida Cousin","Julius Erving","Michael's Mom","Delivery Boy","Doctor","Kim | Receptionist","Mary the Cocktail Waitress","Sex Shop Clerk","Bollywood Dancer","Amy","The Photographer","Casino Waitress","Chloe","Glove Girl","Youth Group Member","Scranton Family Member","Jocelyn","Wendy's employee","Ted","Mori","Admitting Nurse","Torrie Sullivan","Treble","Young Club girl","Doctor Speaking through Glass","Ed Truck","Bill","Emily","Fake Stanley","Edward R. Meow","Aaron Grandy","Delivery Kid","Stanley's Date","Korean Woman #2",null,"Grillmaster","Country Club Snob","Creative Impulse Vendor","Waitstaff #1","Dwight's Teammate","Nikki","Teenage Food Server","Ty Platt","Derrick","Adman",null,"Airplane Passenger","Michael","Karate Student",null,"Shane","Teacher","Stephanie","Exiting Co-Worker","Julie","Clark","Julius","Waitress","Guitarist Lisa","Bridesmaid","Dana","Barbara Allen","Woman in Suit","GPS","Basketball Girl","Phyllis' Sister","Frannie","Stanley's Replacement","Linda","Karate Student","Mark McGrath","Bartender","Kenny","Buffalo branch employee","Bookstore Clerk","Telemarketer wearing green sweater",null,"Jerry","Frat Guy","Evan","Underage Kid #3","Blogger","Documentary Crew Member","Blogger","Business School Student","Dad",null,"Mrs. California","Eric","Tobias","Natural Redhead","Waitstaff #2","Stacy","Cathy","Lefervre","Policeman #2","Restaurant Patron","Rochester Volleyball Player #2","Mike - Game Player","Sensei Billy","Underage Kid #1","Mike","Nashua Guy","Chris","Airport Passenger","The Mediator","Concerned Panel Fan","Roger","Dundies Guest","Hotel Receptionist",null,"Office Worker","Student","Grotti",null,"Tina Fey Look-a-Like","Cat","Peter Rowley","Albany Gal","Jan's Daughter","The Principle","Horse Man","Bollywood Dancer","Amanda Fields-Shad","Waitress","Frisbee Student","Youth Group Member","Orderly #2","The Waiter","Hotel Manager",null,"Technician","Treble","Telemarketer","Cpl. Miller","Mark's Girlfriend","Waiter","Doctor","Hispanic Boy","Additional","Policeman #1","Russell","Lester Snyder","Paul Faust","The Animal Trainer","Foppy McGee","Chief Buyer Steve Nash","The HRPDC Executive","Stu","Park Ranger","Phil Maguire","Woman in Line","Mennonite Priest","Panicked Man","O'Malley","Rachael","Gabriella","Mother","Bank Manager","Madsen","Asian Jim Halpert","Ship's Captain","Pirelli","Conference Goer","Event Security","Merv Bronte","Dave","Athlead Male Receptionist","Godmother","Tractor Salesman","Corporate Volleyball Player #1","Wedding Guest","Minister","Bollywood Dancer","Salesman","Captain Jack","Blogger","Alan","Groomsman","Sweeney Todd","Simon","Chelsea's Mother","Rose","Curtis Dorough","Tequila Man","Pharmacist","Bollywood Dancer","Filipino Teen","Mr. Haskins","Waiter","Dennis","Producer","Self","Clerk","Valerie","Cindy Halpert","Professor","Bollywood Dancer","Laura","Store Manager","Josie","Lady at Bar","Bookstore Customer","Other Pam",null,"The Bartender","The Photographer",null,"Caterer","Male Audience Member","Leader","Barista","Reed","Treble","Sci Fi Attendee","Sheri | Mother Harvest","Nashua Employee","Randy","Seth Meyers",null,"Pam's Mom","Figure Skater","Linda Prince","Sherrie","Waitress","Jamie","Bollywood Dancer","Nurse","CPU Guy","Schrute Cousin",null,"Pratt Student",null,"Sconesy Cider","Broccoli Rob","Walter Bernard, Sr.","Waiter | Bartender","Hospital Visitor","Audience Member","Conventioneer","Drummer","Cop #2","Dwight's Ignored Customer","Office Employee","Pizza Delivery Guy","Dipido Smith","Ms. Trudy","Lauren","Jim's Niece","Waitress","Delivery Woman","Scranton Family Member","PBS Panel Moderator","Marcy","Kelly's Mom","Tall Girl #2","Meredith's Hospital Roommate","Rupa","Rhee","Angela's Mom","Bill Kress",null,"Treble","Pretzel Vendor",null,"Walker","Jeb","Single Mom","Koh","Christian","Scranton Basketball Boy","Todd Packer","Robert from WeyerHammer Paper",null,"Heinrich","English Teacher",null,"Tim Dockery","Boy at Picnic","Potential Canadian Client","Dog Groomer's Guy","Jan's Assistant","Caterer","Justin Polznik","Self","Son","Stenographer","Wali","Linda","Bollywood Dancer","Auditionee","Vendor","Assistant","Wally Amos","Theater Patron","Interviewee","Rory Flenderson","Richard Corey","Concierge Marie","Esther's Sister",null,"Fred Henry","Bartender","Sam Stone, Jr.","Wolf","Improv Class Student","Paris","Bachelorette Party Guest","Carla"],[188,188,188,188,188,188,188,187,180,177,169,161,152,142,141,120,102,51,43,37,34,32,27,25,25,23,21,19,19,17,16,15,14,14,13,12,9,8,8,8,8,7,7,6,6,6,5,5,5,5,5,5,5,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1]],"container":"<table class=\"display\">\n  <thead>\n    <tr>\n      <th> <\/th>\n      <th>Ator/Atriz<\/th>\n      <th>Personagem(ns)<\/th>\n      <th>Número de episódios<\/th>\n    <\/tr>\n  <\/thead>\n<\/table>","options":{"columnDefs":[{"className":"dt-right","targets":3},{"orderable":false,"targets":0}],"order":[],"autoWidth":false,"orderClasses":false}},"evals":[],"jsHooks":[]}</script>

<br>

<details>
<summary>
Veja o código em R
</summary>

``` r
theoffice_dados %>%
  separate(col = elenco,
           into = c("elenco_01", "elenco_02", "elenco_03",
                    "elenco_04", "elenco_05", "elenco_06",
                    "elenco_07", "elenco_08", "elenco_09",
                    "elenco_10", "elenco_11", "elenco_12",
                    "elenco_13", "elenco_14", "elenco_15",
                    "elenco_16", "elenco_17", "elenco_18",
                    "elenco_19", "elenco_20", "elenco_21",
                    "elenco_22", "elenco_23", "elenco_24",
                    "elenco_25", "elenco_26", "elenco_27",
                    "elenco_28", "elenco_29", "elenco_30",
                    "elenco_31", "elenco_32", "elenco_33",
                    "elenco_34", "elenco_35", "elenco_36",
                    "elenco_37", "elenco_38", "elenco_39",
                    "elenco_40", "elenco_41", "elenco_42",
                    "elenco_43", "elenco_44", "elenco_45",
                    "elenco_46", "elenco_47", "elenco_48",
                    "elenco_49", "elenco_50", "elenco_51",
                    "elenco_52", "elenco_53", "elenco_54",
                    "elenco_55", "elenco_56", "elenco_57",
                    "elenco_58", "elenco_59", "elenco_60",
                    "elenco_61", "elenco_62", "elenco_63",
                    "elenco_64", "elenco_65", "elenco_66",
                    "elenco_67", "elenco_68", "elenco_69",
                    "elenco_70", "elenco_71", "elenco_72",
                    "elenco_73", "elenco_74", "elenco_75",
                    "elenco_76", "elenco_77", "elenco_78"),
           sep = ", ") %>%
  select(episodio, titulo, starts_with("elenco")) %>%
  pivot_longer(cols = starts_with("elenco"),
               names_to = "elenco_num",
               values_to = "elenco_nome") %>%
  count(elenco_nome) %>%
  arrange(desc(n)) %>%
  drop_na() %>%
  inner_join(theoffice_personagens, by = "elenco_nome") %>%
  select(elenco_nome, personagem_ns, n) %>%
  DT::datatable(
    colnames = c("Ator/Atriz", "Personagem(ns)", "Número de episódios"),
    caption = "Tabela 1: Número de episódios em que cada ator está creditado"
  )
```

</details>

<br>

Segundo a tabela, Ed Helms possui mais aparições que Steve Carrel (ator principal que saiu ao final da sétima temporada). Após a saída de Steve, outros atores entraram para participações especiais, como: Zach Woods, Catherine Tate, Robert R. Shafer, James Spader, Kathy Bates, Will Ferrell, entre outros.

A maior parte dos atores creditados fizeram apenas uma participação em algum (ou outro) episódio. Em 2005, por exemplo, quando interpretou a “garota da bolsa” no episódio *Hot Girl*, Amy Adams era uma atriz relativamente desconhecida. Não se imaginava o quão famosa ela se tornaria futuramente.

### Conclusão

Nascida em 2005 e encerrada em 2013, **The Office** conquista novos fãs até os dias de hoje. Recheada de personagens marcantes, cada um com a sua personalidade e com suas características, a série se tornou um fenômeno da cultura pop ao permitir que o espectador se identifique com os personagens e, assim, compreenda seu próprio cotidiano.

### Agradecimentos

-   Aos professores [Beatriz Milz](https://twitter.com/BeaMilz) e [William Amorim](https://twitter.com/wamorim_), pelos conhecimentos compartilhados e pelo incentivo na realização deste blog.

<!-- * À [**Curso-R**](https://curso-r.com/) por ofecer seus cursos em formato *on-line*, expandindo, assim, os conhecimentos de <span style="color:darkblue">**R**</span> para vários lugares do Brasil. -->

-   À minha namorada Samantha de Moraes, por *sempre* acreditar em mim (mesmo quando nem eu mesma acredito). Te amo! :heart:

### Referências

Este texto foi elaborado com o R (R Core Team 2021) e os pacotes: `{dplyr}` (Wickham, François, et al. 2021), `{extrafont}` (Winston Chang 2014), `{ggplot2}` (Wickham, Chang, et al. 2021; Wickham 2016), `{janitor}` (Firke 2021), `{magrittr}` (Bache and Wickham 2020), `{readr}` (Wickham and Hester 2021), `{readxl}` (Wickham and Bryan 2019), `{rmarkdown}` (Allaire et al. 2021; Xie, Allaire, and Grolemund 2018; Xie, Dervieux, and Riederer 2020), `{tidyr}` (Wickham 2021a), `{tidyverse}` (Wickham 2021b; Wickham et al. 2019) e `{xaringanExtra}` (Aden-Buie and Warkentin 2021).

<br>

<!-- [ˆ1]: Este trabalho foi realizado, originalmente, para a obtenção do certificado do curso [**R para Ciência de Dados I**](https://curso-r.com/cursos/r4ds-1/) da [Curso-R](https://curso-r.com/). O texto entregue pode ser conferido [neste *link*](https://curso-r.github.io/202010-r4ds-1/trabalho_final/Luisa_Bock.html). Abaixo, está o texto com algumas alterações sugeridas pelos professores, além dos códigos utilizados. -->

</div>

<div id="refs" class="references csl-bib-body hanging-indent">

<div id="ref-R-xaringanExtra" class="csl-entry">

Aden-Buie, Garrick, and Matthew T. Warkentin. 2021. *xaringanExtra: Extras and Extensions for Xaringan Slides*. <https://github.com/gadenbuie/xaringanExtra>.

</div>

<div id="ref-R-rmarkdown" class="csl-entry">

Allaire, JJ, Yihui Xie, Jonathan McPherson, Javier Luraschi, Kevin Ushey, Aron Atkins, Hadley Wickham, Joe Cheng, Winston Chang, and Richard Iannone. 2021. *Rmarkdown: Dynamic Documents for r*. <https://CRAN.R-project.org/package=rmarkdown>.

</div>

<div id="ref-R-magrittr" class="csl-entry">

Bache, Stefan Milton, and Hadley Wickham. 2020. *Magrittr: A Forward-Pipe Operator for r*. <https://CRAN.R-project.org/package=magrittr>.

</div>

<div id="ref-R-janitor" class="csl-entry">

Firke, Sam. 2021. *Janitor: Simple Tools for Examining and Cleaning Dirty Data*. <https://github.com/sfirke/janitor>.

</div>

<div id="ref-R-base" class="csl-entry">

R Core Team. 2021. *R: A Language and Environment for Statistical Computing*. Vienna, Austria: R Foundation for Statistical Computing. <https://www.R-project.org/>.

</div>

<div id="ref-ggplot22016" class="csl-entry">

Wickham, Hadley. 2016. *Ggplot2: Elegant Graphics for Data Analysis*. Springer-Verlag New York. <https://ggplot2.tidyverse.org>.

</div>

<div id="ref-R-tidyr" class="csl-entry">

———. 2021a. *Tidyr: Tidy Messy Data*. <https://CRAN.R-project.org/package=tidyr>.

</div>

<div id="ref-R-tidyverse" class="csl-entry">

———. 2021b. *Tidyverse: Easily Install and Load the Tidyverse*. <https://CRAN.R-project.org/package=tidyverse>.

</div>

<div id="ref-tidyverse2019" class="csl-entry">

Wickham, Hadley, Mara Averick, Jennifer Bryan, Winston Chang, Lucy D’Agostino McGowan, Romain François, Garrett Grolemund, et al. 2019. “Welcome to the <span class="nocase">tidyverse</span>.” *Journal of Open Source Software* 4 (43): 1686. <https://doi.org/10.21105/joss.01686>.

</div>

<div id="ref-R-readxl" class="csl-entry">

Wickham, Hadley, and Jennifer Bryan. 2019. *Readxl: Read Excel Files*. <https://CRAN.R-project.org/package=readxl>.

</div>

<div id="ref-R-ggplot2" class="csl-entry">

Wickham, Hadley, Winston Chang, Lionel Henry, Thomas Lin Pedersen, Kohske Takahashi, Claus Wilke, Kara Woo, Hiroaki Yutani, and Dewey Dunnington. 2021. *Ggplot2: Create Elegant Data Visualisations Using the Grammar of Graphics*. <https://CRAN.R-project.org/package=ggplot2>.

</div>

<div id="ref-R-dplyr" class="csl-entry">

Wickham, Hadley, Romain François, Lionel Henry, and Kirill Müller. 2021. *Dplyr: A Grammar of Data Manipulation*. <https://CRAN.R-project.org/package=dplyr>.

</div>

<div id="ref-R-readr" class="csl-entry">

Wickham, Hadley, and Jim Hester. 2021. *Readr: Read Rectangular Text Data*. <https://CRAN.R-project.org/package=readr>.

</div>

<div id="ref-R-extrafont" class="csl-entry">

Winston Chang. 2014. *Extrafont: Tools for Using Fonts*. <https://github.com/wch/extrafont>.

</div>

<div id="ref-rmarkdown2018" class="csl-entry">

Xie, Yihui, J. J. Allaire, and Garrett Grolemund. 2018. *R Markdown: The Definitive Guide*. Boca Raton, Florida: Chapman; Hall/CRC. <https://bookdown.org/yihui/rmarkdown>.

</div>

<div id="ref-rmarkdown2020" class="csl-entry">

Xie, Yihui, Christophe Dervieux, and Emily Riederer. 2020. *R Markdown Cookbook*. Boca Raton, Florida: Chapman; Hall/CRC. <https://bookdown.org/yihui/rmarkdown-cookbook>.

</div>

</div>

[^1]: Dados coletados até o dia 07 de novembro de 2021, às 17:43, [neste link](https://www.imdb.com/title/tt0386676/).

[^2]: O arquivo .xls está disponível [neste link](https://github.com/lgiselebock/luisa/tree/blog/content/blog/2021-11-13-the-office/data). Nesta pasta, há, também, dois arquivos .rds já tratados e que podem ser lidos com o pacote `{readr}` (Wickham and Hester 2021).

[^3]: A fonte utilizada está disponível em: [dafontfree.net/staffmeeting-plain](https://www.dafontfree.net/staffmeeting-plain/f178453.htm)

[^4]: Dependendo da fonte utilizada, há divergências em relação ao número total de episódios. Enquanto o IMDb caracteriza episódios com duração superior a 40 minutos como apenas uma parte, outras fontes dividem estes episódios em duas partes (como ocorreu com *Niágara*).

[^5]: Número de telespectadores (nos Estados Unidos) que assistiram o episódio na noite em que ele foi transmitido. Segundo [The Office Nielsen Ratings Achives](https://www.officetally.com/archive/the-office-nielsen-ratings).

[^6]: [The Office ultrapassa Friends como a série mais vista da Netflix nos EUA](https://entretenimento.uol.com.br/noticias/redacao/2019/10/29/the-office-ultrapassa-friends-como-a-serie-mais-vista-da-netflix-nos-eua.htm)

[^7]: “Stanley, você esmaga sua mulher no sexo e seu coração não presta.”
