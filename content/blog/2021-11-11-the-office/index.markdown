---
title: "Analise de The Office"
subtitle: "How to add panelsets in plain markdown posts."
excerpt: "O quê os números dizem sobre uma das séries de comédia mais famosas do século XXI? Foi esta pergunta que eu tentei responder em meu trabalho de conclusão do curso [**R para Ciência de Dados I**](https://blog.curso-r.com/posts/2020-12-03-dicas-relatorios-r4ds1_tabelas/) da [Curso-R](https://curso-r.com/)."
date: 2021-11-11
author: "Luísa Gisele Böck"
draft: false
# layout options: single, single-sidebar
layout: single
categories:
- R
- TV Series
tags:
- The Office
---
<script src="{{< blogdown/postref >}}index_files/clipboard/clipboard.min.js"></script>
<link href="{{< blogdown/postref >}}index_files/xaringanExtra-clipboard/xaringanExtra-clipboard.css" rel="stylesheet" />
<script src="{{< blogdown/postref >}}index_files/xaringanExtra-clipboard/xaringanExtra-clipboard.js"></script>
<script>window.xaringanExtraClipboard(null, {"button":"Copy Code","success":"Copied!","error":"Press Ctrl+C to Copy"})</script>





<img src="gay-witch-hunt-featured.jpg" width="540" style="display: block; margin: auto;" />

<div style="text-align: right">

<i><small>NBC</small></i>

<div style="text-align: justify">

Em agosto de 2020, um trecho do documentário *O Fórum* viralizou na internet mostrando, nos bastidores do Fórum Econômico Mundial, ocorrido em 2019, na cidade de Davos (Suíça), um diálogo sobre a Amazônia entre Jair Bolsonaro e o ex-vice-presidente dos Estado Unidos Al Gore, reconhecido ativista ambiental. Impossível não relacionar a situação constrangedor, vivida pelo atual presidente brasileiro, com uma cena protagonizada por Michael Scott, personagem interpretado por Steve Carrel, no seriado **The Office**. Série que redefiniu a situação de vergonha alheia.

O presente trabalho visa realizar uma análise sobre o seriado americano **The Office**, como audiência, melhores episódios, roteiros, direção e elenco. As informações observadas foram integralmente coletadas da base de dados [IMDb (*Internet Movie Database*)](https://www.imdb.com)[^1] - base de dados *online* sobre entretenimento, a avaliação é composta por notas dadas por espectadores e críticos.

[^1]: Dados coletados até o dia 07 de novembro de 2021, às 17:43.

Para iniciar a análise, é necessário carregar os pacotes, as bases de dados e a fonte das letras utilizadas nos gráficos:


```r
library(tidyverse)

theoffice_dados <- readr::read_rds("data/theoffice_dados.rds")
theoffice_personagens <- readr::read_rds("data/theoffice_personagens.rds")

extrafont::loadfonts(device = "win") # lê a tabela de fontes e registra no R
```

### A Série

**The Office** é uma série americana, originalmente transmitida pela NBC entre 25 de março de 2005 e 16 de maio de 2013, com um total de 188 episódios[^2]. A série, uma adaptação do programa britânico de mesmo nome, é exibida em formato de *pseudodocumentário*, e retrata o cotidiano dos funcionários de um escritório filial da *Dunder Mifflin Paper Company*, uma empresa que vende papel localizada na cidade de Scranton, na Pensilvânia. A série venceu 51 prêmios das 193 indicações que recebeu, entre elas consta, principalmente, 1 *Golden Globes* (2006), 2 *Screen Actors Guild Awards* (2007 e 2008) e 5 *Primetime Emmy Awards* (2006, 2007, 2009 e 2013), incluindo melhor série de comédia.

[^2]: Dependendo da fonte utilizada, há divergências em relação ao número total de episódios. Enquanto o IMDb caracteriza episódios com duração superior a 40 minutos como apenas uma parte, outras fontes dividem estes episódios em duas partes (como ocorreu com *Niágara*).

{{< panelset class="episodio-por-temporada" >}}
{{< panel name="Gráfico" >}}

<img src="{{< blogdown/postref >}}index_files/figure-html/episodio-por-temporada-plot-1.png" width="672" style="display: block; margin: auto;" />

{{< /panel >}}
{{< panel name="Código R" >}}


```r
theoffice_dados %>%
  ggplot() +
  geom_bar(aes(x = temporada),
           color = "black",
           fill = RColorBrewer::brewer.pal(9, "Paired")) +
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

{{< /panel >}}
{{< /panelset >}}

É dividida em nove temporadas com uma média de 20 episódios cada uma. A primeira temporada é a mais curta de toda a série, com apenas seis episódios e duração de 137 minutos. Em oposição, as temporadas mais duradouras são a quinta e a sexta, com 26 episódios cada e com 751 e 755 minutos respectivamente.

{{< panelset class="duracao_por_temporada" >}}
{{< panel name = "Gráfico" >}}

<img src="{{< blogdown/postref >}}index_files/figure-html/duracao-por-temporada-plot-1.png" width="672" style="display: block; margin: auto;" />

{{< /panel >}}
{{< panel name = "Código R" >}}


```r
theoffice_dados %>%
  group_by(temporada) %>%
  summarise(duracao_min_temporada = sum(duracao)) %>%
  ggplot() +
  geom_col(
    aes(x = temporada, y = duracao_min_temporada),
    color = "black",
    fill = RColorBrewer::brewer.pal(9, "Paired")
  ) +
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

{{< /panel >}}
{{< /panelset >}}

### Audiência[^3]

[^3]: Número de telespectadores (nos Estados Unidos) que assistiram o episódio na noite em que ele foi transmitido.

Em 2019, **The Office** superou *Friends* como a série mais assistida da *Netflix* nos Estados Unidos. Segundo dados revelados por Scott Lazerson, executivo da empresa de *streaming*, o seriado protagonizado por Steve Carrel acumulou um total de 52,98 bilhões de minutos assistidos, contra 42,6 bilhões de *Friends*. Apesar do êxito nos dias atuais, **The Office** nunca foi um sucesso de audiência quando exibida nos EUA pela NBC. O programa quase foi cancelado entre a segunda e terceira temporada e só foi salvo por suas boas vendas no iTunes.

Conforme mostra a *Figura 3*, a audiência média de todas as temporadas do seriado não ultrapassa os 9 milhões de telespectadores, sendo a quinta e a nona (e última) temporada as com melhor e pior média - 8.76 e 4,14 milhões de telespctadores, respectivamente. É possível observar, após a saída de Steve Carrel do elenco, ao final da 7ª temporada, uma queda bastante considerável na audiência média das últimas duas temporadas.

{{< panelset class="audiencia_media" >}}
{{< panel name="Gráfico" >}}

<img src="{{< blogdown/postref >}}index_files/figure-html/audiencia-media-plot-1.png" width="672" style="display: block; margin: auto;" />

{{< /panel>}}
{{< panel name = "Código R" >}}


```r
theoffice_dados %>%
  group_by(temporada) %>%
  summarise(mean_aud_season = mean(qntd_telespectadores_eua_milhoes)) %>%
  ggplot() +
  geom_col(
    aes(x = temporada, y = mean_aud_season),
    color = "black",
    fill = RColorBrewer::brewer.pal(9, "Paired")
  ) +
  geom_text(
    aes(
      x = temporada,
      y = mean_aud_season,
      label = round(mean_aud_season, 2)
    ),
    vjust = -1,
    family = "StaffMeetingPlain"
  ) +
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

{{< /panel >}}
{{< /panelset >}}

A *Figura 4* demonstra a audiência (em milhões de espectadores) para cada episódio da série. Todos os episódios, exceto um, apresentam uma média constante e abaixo dos 10 milhões de espectadores. A exceção é ***Stress Relief***, décimo terceiro episódio da quinta temporada, exibido no dia 1 de fevereiro de 2009 - imediatamente após a transmissão do *SuperBowl XLIII* pela NBC.

{{< panelset class="audiencia_coluna" >}}
{{< panel name = "Gráfico" >}}

<img src="{{< blogdown/postref >}}index_files/figure-html/audiencia-coluna-plot-1.png" width="672" style="display: block; margin: auto;" />

{{< /panel >}}
{{< panel name = "Código R" >}}


```r
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
      "#A6CEE3",
      "#1F78B4",
      "#B2DF8A",
      "#33A02C",
      "#FB9A99",
      "#E31A1c",
      "#FDBF6F",
      "#FF7F00",
      "#CAB2D6"
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

{{< /panel >}}
{{< /panelset >}}

***Stress Relief***, escrito por Paul Lieberstein e dirigido por Jeffrey Blitz, foi o episódio mais assistido da série, com 22.91 milhões de espectadores. Por se tratrar de um episódio especial, possui uma hora de duração (com os comerciais inclusos) e contou com participações especiais de Jack Black, Jessica Alba e Cloris Leachman que aparecem em um filme exibido dentro do episódio.

Nele, Dwight encena um incêndio no escritório para testar as habilidades de segurança contra incêndios de seus colegas, mas a situação perde o controle e se torna um caos, especialmente quando Stanley sofre um ataque cardíaco. Para aliviar o estresse no escritório, Michael tenta relaxar seus funcionários com sessões de ioga e meditação. Entretanto, percebe que ele próprio é a fonte do estresse do escritório. Enquanto isso, Andy, Jim e Pam assistem a um filme baixado ilegalmente no trabalho, e Pam precisa lidar com a eminente separação de seus pais. Michael acredita que o estresse no escritório existe porque seus funcionários resistem em expressar seus sentimentos para o chefe, então organiza o *'The Roast of Michael Scott'*, uma apresentação em que os funcionários podem xingá-lo, sem ressentimentos posteriores. No início ele parece gostar dos insultos, mas termina a apresentação visivelmente chateado e ofendido. Michael volta para o escritório apenas no dia seguinte, onde devolve as piadas para seus colegas de trabalho. Quando chega a vez de Stanley, Michael diz: *"Stanley, you crush your wife during sex and your heart sucks"*[^4], Stanley ri com tanta vontade que acaba quebrando a tensão existente entre os colegas e seu chefe.

[^4]: "Stanley, você esmaga sua mulher no sexo e seu coração não presta."

### Melhores episódios

Entre todos os programas de TV avaliados pelo site IMDb, **The Office** ocupa a 43ª posição com 8.9 estrelas. Nas avaliações dos usuários do IMDb, a sequência da 3ª, a 4ª e a 5ª temporadas como as três melhores da série, obtendo, respectivamente, 8.59, 8.56 e 8.49 estrelas; por outro lado, a 8ª temporada recebeu a menor avaliação, com 7.58 estrelas. A *Figura 5* apresenta a avaliação média para todas as temporadas.

{{< panelset class="media_imdb_por_temporada" >}}
{{< panel name = "Gráfico" >}}

<img src="{{< blogdown/postref >}}index_files/figure-html/media_imdb_por_temporada-plot-1.png" width="672" style="display: block; margin: auto;" />

{{< /panel >}}
{{< panel name = "Código R" >}}


```r
theoffice_dados %>%
  group_by(temporada) %>%
  summarise(mean_imdb_season = mean(estrelas_imdb)) %>%
  ggplot() +
  geom_col(
    aes(x = temporada, y = mean_imdb_season),
    color = "black",
    fill = RColorBrewer::brewer.pal(9, "Paired")
  ) +
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


{{< /panel >}}
{{< /panelset >}}

A *Figura 6* realça os 10 episódios - no universo de 188 - com melhor avaliação segundo o IMDb, todos com mais de 9.3 estrelas. Destaque para ***Goodbye, Michael*** e ***Finale***, ambas com 9.8 estrelas; ***Stress Relief*** com 9.7 estrelas; ***Dinner Party*** e ***A.A.R.M*** com 9.5. Menção especial para ***Niagara: Part 1*** e ***Niagara: Part 2***, episódios que retraram o (tão esperado) casamento entre Jim e Pam, e obtiveram 9.4 estrelas cada um.

***Goodbye, Michael*** é o 21º episódio da 7ª temporada, foi exibido no dia 28 de abril de 2011 e é, como o nome já diz, o episódio de despedida de Michael Scott. Escrito por Greg Daniels e dirigido por Paul Feig, marca a última aparição de Steve Carrel (Michael Scott) como parte do elenco regular. Durante 50 minutos, Michael se prepara para partir rumo ao Colorado com sua namorada Holly e passa o último dia no escritório se despedindo individualmente de cada um dos seus colegas; enquanto isso, o novo gerente Deangelo e Andy tentam ficar com os maiores clientes de Michael.

***Finale*** é o 23º episódio da 9ª temporada, foi exibido no dia 16 de maio de 2013 e é o último episódio da série. Foi escrito por Greg Daniels e dirigido por Ken Kwapis (que dirigiu o episódio ***Pilot***). O enredo consiste em um reencontro com os antigos e atuais trabalhadores da *Dunder Mifflin Paper Company* para o casamento de Angela e Dwight e, também, uma rodada final de entrevistas - um ano após a exibição do documentário.

{{< panelset class ="top10_estrelas_imdb" >}}
{{< panel name = "Gráfico" >}}

<img src="{{< blogdown/postref >}}index_files/figure-html/top10-estrelas-imdb-plot-1.png" width="672" style="display: block; margin: auto;" />

{{< /panel >}}
{{< panel name = "Código R" >}}


```r
theoffice_dados %>%
  arrange(desc(estrelas_imdb)) %>%
  slice_head(n = 10) %>%
  select(titulo, estrelas_imdb) %>%
  ggplot() +
  geom_col(
    aes(x = estrelas_imdb, y = reorder(titulo, +estrelas_imdb)),
    color = "black",
    fill = RColorBrewer::brewer.pal(10, "Paired")
  ) +
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

{{< /panel >}}
{{< /panelset >}}

***Dinner Party*** é o 9º episódio da 4ª temporada, exibido em 10 de abril de 2008. Dirigido por Paul Feig e escrito por Gene Stupnitsky e Lee Eisenberg é [o único roteiro da série que não foi reescrito](https://rollingstone.uol.com.br/noticia/conheca-episodio-perfeito-de-office-o-unico-cujo-roteiro-nao-foi-reescrito/). O episódio mostra a realização de um sonho de Michael Scott: fazer com que o casal Jim e Pam aceitasse um convite para jantar na casa dele. Andy e Angela também são convidados, Dwight (com ciúmes) consegue entrar de penetra com uma companheira (antiga babá e, por vezes, amante dele). O episódio inteiro gira em torno dos personagens anteriormente citados e Jan, companheira de Michael, na casa do casal.

Considerado pela produção e elenco como o "episódio perfeito", é repleto de momentos extremamente engraçados e (muito) constrangedores. É um dos mais adorados pelos fãs da série, mas, na época em que foi exibido, o público não aceitou bem a ideia. Segundo o diretor Paul Feig:

<br>

> "Nós fizemos as pessoas sentirem-se tão constrangidas que elas simplesmente não aguentavam. Porém, com o passar dos anos, os fãs da série passaram a amar \`Dinner Party'. O que aconteceu agora é que, depois de ver uma vez e saber o que está por vir, você pode realmente aproveitá-lo."

<br>

Em compensação, grande parte da crítica especializada considerou o episódio como um dos melhores de **The Office**. Travis Fickett da IGN escreveu que ["este é um daqueles grandes episódios de **The Office** que é histérico e difícil de assitir ao mesmo tempo"](https://www.ign.com/articles/2008/04/11/the-office-dinner-party-review).

É interessante observar que os dois episódios com melhor avaliação (***Goodbye, Michael*** e ***Finale***) carregam uma carga emotiva superior à comédia, uma vez que ambos tratam de duas despedidas: o primeiro, de Michael; o segundo, da série em si.

{{< panelset class="estrelas_imdb_histograma" >}}
{{< panel name = "Gráfico" >}}

<img src="{{< blogdown/postref >}}index_files/figure-html/estrelas_imdb_histograma-plot-1.png" width="672" style="display: block; margin: auto;" />

{{< /panel >}}
{{< panel name = "Código R" >}}


```r
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

{{< /panel >}}
{{< /panelset >}}

Nas avaliações de todos os 188 episódios, há um intervalo de avaliação entre 6.5 e 9.8 estrelas, sendo uma predominância na quantidade de estrelas entre 7.75 e 8.75. O histograma acima (*Figura 7*) revela a frequência das estrelas dadas pelos usuários do IMDb para todos os episódios.

### Roteiristas

Durante os nove anos em que esteve no ar, 40 roteiristas diferentes assinaram ao menos um episódio do seriado. O gráfico abaixo (*Figura 8*) realça os 10 roteiristas que foram mais vezes creditados.

**Mindy Kaling** encabeça a lista. Ela, que além de interpretar a personagem Kelly Kapoor, roteirizou 22 episódios, entre eles: *Niagara: Part 1*, *Niagara: Part 2*; escritos juntamente com Greg Daniels.

Em segundo, **Paul Lieberstein**, interprete do depressivo representante do RH no escritório e odiado por Michael Scott - Toby Flenderson -, participou de 16 roteiros; entre eles, *Stress Relief*, o episódio mais visto de toda a série e o terceiro melhor avaliado segundo IMDb.

{{< panelset class="roteiristas" >}}
{{< panel name = "Gráfico" >}}

<img src="{{< blogdown/postref >}}index_files/figure-html/roteiristas-plot-1.png" width="672" style="display: block; margin: auto;" />

{{< /panel >}}
{{< panel name = "Código R" >}}


```r
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
  ggplot() +
  geom_col(
    aes(x = qnt_episodios,
        y = reorder(roteirista, +qnt_episodios)),
    color = "black",
    fill = RColorBrewer::brewer.pal(10, "Paired")
  ) +
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

{{< /panel >}}
{{< /panelset >}}

**B. J. Novak**, o estagiário Ryan Howard, roteirizou 15 episódios; além de roteirista, Novak foi a primeira pessoa a entrar para o elenco. Ele é responsável por assinar o episódio *Threat Level Midnight*, o sexto melhor segundo o ranking do IMDb.

**Greg Daniels**, o responsável por desenvolver o seriado americano, assina 13 roteiros, entre eles: *Pilot*, *Niagara: Part 1*, *Niagara: Part 2*, *Goodbye, Michael* e *Finale*. Destes 5 episódios citados, 4 estão entre os 10 com melhor avaliação do IMDb.

**Michael Schur**, que também está no elenco como o Mose - primo do Dwight -, fecha o TOP 10 com dez episódios escritos.

### Diretores

Ao todo, 55 diretores participaram da série nesssas nove temporadas. **Randall Einhorn** e **Paul Feig** encabeçam a lista com 15 episódios cada. Paul Feig é o responsável pela direção de 4 dos 10 episódios com melhor avalição, são eles: *Goodbye, Michael*, *Dinner Party*, *Niagara: Part 1* e *Niagara: Part 2*. Ken Kwapis dirigiu 13 episódios, entre eles: *Pilot*, *Diversity Day* e *Finale* - primeiro, segundo e último episódios, respectivamente.

A lista segue com Greg Daniels, Jeffrey Blitz, Ken Whittingham, David Rogers, Matt Sohn, Charles McDougall e Paul Lieberstein fecha o Top 10.

{{< panelset class="diretores" >}}
{{< panel name = "Gráfico" >}}

<img src="{{< blogdown/postref >}}index_files/figure-html/diretores-plot-1.png" width="672" style="display: block; margin: auto;" />

{{< /panel >}}
{{< panel name = "Código R" >}}


```r
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
  ggplot(fill = "cyan") +
  geom_col(
    aes(x = qnt_episodios,
        y = reorder(direcao, +qnt_episodios)),
    color = "black",
    fill = RColorBrewer::brewer.pal(10, "Paired")
  ) +
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

{{< /panel >}}
{{< /panelset >}}

Outros integrantes do elenco regular também dirigiram ao menos um episódio, como: Mindy Kaling, Ed Helms, Brian Baumgartner, Steve Carell, John Krasinski, Rainn Wilson e B.J. Novak. Além destes, profissionais consagrados em outros trabalhos, contribuíram para a direção de episódios em **The Office**, por exemplo: Bryan Cranston, J.J. Abrams e Marc Webb entre outros.

### Elenco

Muitos atores e atrizes fizeram parte do elenco ao longo dos 188 episódios, seja para em um papel regular, uma participação especial ou apenas uma aparição relâmpago (Conan O' Brian, por exemplo). A *Tabela 1* registra as participações das 787 pessoas que apareceram na série, em ordem decrescente - de quem possui mais aparições para quem possui menos.

O elenco regular (funcionários do escritório) se concentram nas primeiras posições da tabela. Angela Kinsey, Brian Baumgartner, Jenna Fischer, John Krasinski, Leslie David Baker, Phyllis Smith e Rainn Wilson foram creditados em todos os episódios.

Rashinda Jones entrou na terceira temporada, juntamente com Ed Helms, e permaneceu somente por 26 episódios. Saindo para compor o elenco de *Parks and Recreation*, série criada pelos mesmos produtores de **The Office**.

`tabela`

Segundo a tabela, Ed Helms possui mais aparições que Steve Carrel (ator principal que saiu ao final da sétima temporada). Após a saída de Steve, outros atores entraram para participações especiais, como: Zach Woods, Catherine Tate, Robert R. Shafer, James Spader, Kathy Bates, Will Ferrell, entre outros.

A maior parte dos atores creditados fizeram apenas uma participação em algum (ou outro) episódio. Em 2005, quando interpretou a "garota da bolsa" no episódio *Hot Girl*, Amy Adams era uma atriz relativamente desconhecida. Não se imaginava o quão famosa ela se tornaria futuramente.






[^1]: Dados coletados até o dia 07 de novembro de 2021, às 17:43.
[^2]: Dependendo da fonte utilizada, há divergências em relação ao número total de episódios. Enquanto o IMDb caracteriza episódios com duração superior a 40 minutos como apenas uma parte, outras fontes dividem estes episódios em duas partes (como ocorreu com *Niágara*).
[^3]: Número de telespectadores (nos Estados Unidos) que assistiram o episódio na noite em que ele foi transmitido.
[^4]: “*Stanley, você esmaga sua mulher no sexo e seu coração não presta.*”
