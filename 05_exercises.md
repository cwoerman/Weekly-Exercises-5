---
title: 'Weekly Exercises #5'
author: "Cheyenne Woerman"
output: 
  html_document:
    keep_md: TRUE
    toc: TRUE
    toc_float: TRUE
    df_print: paged
    code_download: true
    theme: cosmo
---





```r
library(tidyverse)      
library(googlesheets4)  
library(ggimage)        
library(lubridate)      
library(openintro)      
library(palmerpenguins) 
library(maps)           
library(ggmap)          
library(gplots)         
library(RColorBrewer)   
library(sf)             
library(leaflet)        
library(ggthemes)       
library(plotly)         
library(gganimate)     
library(transformr)     
library(shiny)          
gs4_deauth()            
theme_set(theme_minimal())
```


```r
small_trains <- read_csv("https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2019/2019-02-26/small_trains.csv") 

garden_harvest <- read_sheet("https://docs.google.com/spreadsheets/d/1DekSazCzKqPS2jnGhKue7tLxRU3GVL1oxi-4bEM5IWw/edit?usp=sharing") %>% 
  mutate(date = ymd(date))

mallorca_bike_day7 <- read_csv("https://www.dropbox.com/s/zc6jan4ltmjtvy0/mallorca_bike_day7.csv?dl=1") %>% 
  select(1:4, speed)

panama_swim <- read_csv("https://raw.githubusercontent.com/llendway/gps-data/master/data/panama_swim_20160131.csv")

panama_bike <- read_csv("https://raw.githubusercontent.com/llendway/gps-data/master/data/panama_bike_20160131.csv")

panama_run <- read_csv("https://raw.githubusercontent.com/llendway/gps-data/master/data/panama_run_20160131.csv")

covid19 <- read_csv("https://raw.githubusercontent.com/nytimes/covid-19-data/master/us-states.csv")

kids <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2020/2020-09-15/kids.csv')


sales <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2020/2020-09-29/sales.csv')

census_pop_est_2018 <- read_csv("https://www.dropbox.com/s/6txwv3b4ng7pepe/us_census_2018_state_pop_est.csv?dl=1") %>% 
  separate(state, into = c("dot","state"), extra = "merge") %>% 
  select(-dot) %>% 
  mutate(state = str_to_lower(state))
```

## Put your homework on GitHub!

Go [here](https://github.com/llendway/github_for_collaboration/blob/master/github_for_collaboration.md) or to previous homework to remind yourself how to get set up. 

Once your repository is created, you should always open your **project** rather than just opening an .Rmd file. You can do that by either clicking on the .Rproj file in your repository folder on your computer. Or, by going to the upper right hand corner in R Studio and clicking the arrow next to where it says Project: (None). You should see your project come up in that list if you've used it recently. You could also go to File --> Open Project and navigate to your .Rproj file. 

## Instructions

* Put your name at the top of the document. 

* **For ALL graphs, you should include appropriate labels.** 

* Feel free to change the default theme, which I currently have set to `theme_minimal()`. 

* Use good coding practice. Read the short sections on good code with [pipes](https://style.tidyverse.org/pipes.html) and [ggplot2](https://style.tidyverse.org/ggplot2.html). **This is part of your grade!**

* **NEW!!** With animated graphs, add `eval=FALSE` to the code chunk that creates the animation and saves it using `anim_save()`. Add another code chunk to reread the gif back into the file. See the [tutorial](https://animation-and-interactivity-in-r.netlify.app/) for help. 

* When you are finished with ALL the exercises, uncomment the options at the top so your document looks nicer. Don't do it before then, or else you might miss some important warnings and messages.

## Warm-up exercises from tutorial

  1. Choose 2 graphs you have created for ANY assignment in this class and add interactivity using the `ggplotly()` function.

```r
taylor_swift_graph <- sales %>% 
  filter(artist %in% c("Taylor Swift")) %>% 
  group_by(title) %>% 
  summarize(sum_album_sales = sum(sales)) %>% 
    ggplot(aes(x = sum_album_sales, 
               y = fct_reorder(title, sum_album_sales,))) +
    geom_col(color = "hotpink4", fill = "indianred2") +
    scale_x_continuous(labels=scales::dollar_format())+
  theme_igray()+
  labs(title = "Taylor Swift's Album Sales",
       subtitle = "By: Cheyenne Woerman",
       x = "Total Album Sales ($)",
       y = "Album Name")
ggplotly(taylor_swift_graph)
```

<!--html_preserve--><div id="htmlwidget-12bb3e18bc7ceeb481b3" style="width:672px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-12bb3e18bc7ceeb481b3">{"x":{"data":[{"orientation":"v","width":[18445200,20289000,4605955,11414000,7271000,9863281,5720000],"base":[5.55,6.55,0.55,4.55,2.55,3.55,1.55],"x":[9222600,10144500,2302977.5,5707000,3635500,4931640.5,2860000],"y":[0.9,0.9,0.9,0.9,0.9,0.9,0.9],"text":["sum_album_sales: 18445200<br />fct_reorder(title, sum_album_sales, ): 1989","sum_album_sales: 20289000<br />fct_reorder(title, sum_album_sales, ): Fearless","sum_album_sales:  4605955<br />fct_reorder(title, sum_album_sales, ): Lover","sum_album_sales: 11414000<br />fct_reorder(title, sum_album_sales, ): Red","sum_album_sales:  7271000<br />fct_reorder(title, sum_album_sales, ): Reputation","sum_album_sales:  9863281<br />fct_reorder(title, sum_album_sales, ): Speak Now","sum_album_sales:  5720000<br />fct_reorder(title, sum_album_sales, ): Taylor Swift"],"type":"bar","marker":{"autocolorscale":false,"color":"rgba(238,99,99,1)","line":{"width":1.88976377952756,"color":"rgba(139,58,98,1)"}},"showlegend":false,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null}],"layout":{"margin":{"t":46.2864259028643,"r":7.97011207970112,"b":43.8356164383562,"l":104.408468244085},"plot_bgcolor":"rgba(255,255,255,1)","paper_bgcolor":"rgba(229,229,229,1)","font":{"color":"rgba(0,0,0,1)","family":"","size":15.9402241594022},"title":{"text":"Taylor Swift's Album Sales","font":{"color":"rgba(0,0,0,1)","family":"","size":19.1282689912827},"x":0,"xref":"paper"},"xaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[-1014450,21303450],"tickmode":"array","ticktext":["$0","$5,000,000","$10,000,000","$15,000,000","$20,000,000"],"tickvals":[0,5000000,10000000,15000000,20000000],"categoryorder":"array","categoryarray":["$0","$5,000,000","$10,000,000","$15,000,000","$20,000,000"],"nticks":null,"ticks":"outside","tickcolor":"rgba(51,51,51,1)","ticklen":3.98505603985056,"tickwidth":0.724555643609193,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":12.7521793275218},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(229,229,229,1)","gridwidth":0.724555643609193,"zeroline":false,"anchor":"y","title":{"text":"Total Album Sales ($)","font":{"color":"rgba(0,0,0,1)","family":"","size":15.9402241594022}},"hoverformat":".2f"},"yaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[0.4,8.6],"tickmode":"array","ticktext":["Lover","Taylor Swift","Reputation","Speak Now","Red","1989","Fearless","Folklore"],"tickvals":[1,2,3,4,5,6,7,8],"categoryorder":"array","categoryarray":["Lover","Taylor Swift","Reputation","Speak Now","Red","1989","Fearless","Folklore"],"nticks":null,"ticks":"outside","tickcolor":"rgba(51,51,51,1)","ticklen":3.98505603985056,"tickwidth":0.724555643609193,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":12.7521793275218},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(229,229,229,1)","gridwidth":0.724555643609193,"zeroline":false,"anchor":"x","title":{"text":"Album Name","font":{"color":"rgba(0,0,0,1)","family":"","size":15.9402241594022}},"hoverformat":".2f"},"shapes":[{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":1,"y0":0,"y1":1}],"showlegend":false,"legend":{"bgcolor":"rgba(229,229,229,1)","bordercolor":"transparent","borderwidth":2.06156048675734,"font":{"color":"rgba(0,0,0,1)","family":"","size":12.7521793275218}},"hovermode":"closest","barmode":"relative"},"config":{"doubleClick":"reset","showSendToCloud":false},"source":"A","attrs":{"6a1c6ea05f47":{"x":{},"y":{},"type":"bar"}},"cur_data":"6a1c6ea05f47","visdat":{"6a1c6ea05f47":["function (y) ","x"]},"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->


```r
state_child_spending <- kids%>%
  filter(state %in% c("Florida", "Georgia", "Alabama", "Mississippi", "Louisiana", "South Carolina", "North Carolina"))%>%
  arrange(state)%>%
  group_by(state,year)%>%
  summarize(inf_adj_perchild = sum(inf_adj_perchild, na.rm = TRUE))%>%
  ggplot(aes(y = inf_adj_perchild, x = year, color = state))+
  geom_line()+
  labs(title = "Average State Spending Per Child; Deep South", x= "Year (1997-2016)", y= "$ Per Child (inflation adjust)")
ggplotly(state_child_spending)
```

<!--html_preserve--><div id="htmlwidget-3f072bbf61562da61f18" style="width:672px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-3f072bbf61562da61f18">{"x":{"data":[{"x":[1997,1998,1999,2000,2001,2002,2003,2004,2005,2006,2007,2008,2009,2010,2011,2012,2013,2014,2015,2016],"y":[9.81225177645683,15.8016969300807,16.55626937747,17.3541410677135,18.1888580694795,19.1333228349686,19.7343671396375,20.2817930467427,19.6365020796657,19.8629641383886,20.3544502966106,20.7041211761534,22.6923878751695,23.9973653480411,23.7873550131917,23.3869794718921,23.6831220574677,23.6967268511653,23.6247481163591,24.0516448989511],"text":["year: 1997<br />inf_adj_perchild:  9.812252<br />state: Alabama","year: 1998<br />inf_adj_perchild: 15.801697<br />state: Alabama","year: 1999<br />inf_adj_perchild: 16.556269<br />state: Alabama","year: 2000<br />inf_adj_perchild: 17.354141<br />state: Alabama","year: 2001<br />inf_adj_perchild: 18.188858<br />state: Alabama","year: 2002<br />inf_adj_perchild: 19.133323<br />state: Alabama","year: 2003<br />inf_adj_perchild: 19.734367<br />state: Alabama","year: 2004<br />inf_adj_perchild: 20.281793<br />state: Alabama","year: 2005<br />inf_adj_perchild: 19.636502<br />state: Alabama","year: 2006<br />inf_adj_perchild: 19.862964<br />state: Alabama","year: 2007<br />inf_adj_perchild: 20.354450<br />state: Alabama","year: 2008<br />inf_adj_perchild: 20.704121<br />state: Alabama","year: 2009<br />inf_adj_perchild: 22.692388<br />state: Alabama","year: 2010<br />inf_adj_perchild: 23.997365<br />state: Alabama","year: 2011<br />inf_adj_perchild: 23.787355<br />state: Alabama","year: 2012<br />inf_adj_perchild: 23.386979<br />state: Alabama","year: 2013<br />inf_adj_perchild: 23.683122<br />state: Alabama","year: 2014<br />inf_adj_perchild: 23.696727<br />state: Alabama","year: 2015<br />inf_adj_perchild: 23.624748<br />state: Alabama","year: 2016<br />inf_adj_perchild: 24.051645<br />state: Alabama"],"type":"scatter","mode":"lines","line":{"width":1.88976377952756,"color":"rgba(248,118,109,1)","dash":"solid"},"hoveron":"points","name":"Alabama","legendgroup":"Alabama","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[1997,1998,1999,2000,2001,2002,2003,2004,2005,2006,2007,2008,2009,2010,2011,2012,2013,2014,2015,2016],"y":[9.9228684976697,13.8089514225721,14.3916443437338,14.6221886053681,15.3599874041975,16.1234727092087,16.9993902146816,17.7405367940664,18.0776481963694,18.2163337469101,18.9828955568373,19.9494839981198,21.3303001821041,23.2236214466393,23.28261731565,22.2903279364109,21.8454397544265,21.921245187521,21.9298269376159,21.394100789912],"text":["year: 1997<br />inf_adj_perchild:  9.922868<br />state: Florida","year: 1998<br />inf_adj_perchild: 13.808951<br />state: Florida","year: 1999<br />inf_adj_perchild: 14.391644<br />state: Florida","year: 2000<br />inf_adj_perchild: 14.622189<br />state: Florida","year: 2001<br />inf_adj_perchild: 15.359987<br />state: Florida","year: 2002<br />inf_adj_perchild: 16.123473<br />state: Florida","year: 2003<br />inf_adj_perchild: 16.999390<br />state: Florida","year: 2004<br />inf_adj_perchild: 17.740537<br />state: Florida","year: 2005<br />inf_adj_perchild: 18.077648<br />state: Florida","year: 2006<br />inf_adj_perchild: 18.216334<br />state: Florida","year: 2007<br />inf_adj_perchild: 18.982896<br />state: Florida","year: 2008<br />inf_adj_perchild: 19.949484<br />state: Florida","year: 2009<br />inf_adj_perchild: 21.330300<br />state: Florida","year: 2010<br />inf_adj_perchild: 23.223621<br />state: Florida","year: 2011<br />inf_adj_perchild: 23.282617<br />state: Florida","year: 2012<br />inf_adj_perchild: 22.290328<br />state: Florida","year: 2013<br />inf_adj_perchild: 21.845440<br />state: Florida","year: 2014<br />inf_adj_perchild: 21.921245<br />state: Florida","year: 2015<br />inf_adj_perchild: 21.929827<br />state: Florida","year: 2016<br />inf_adj_perchild: 21.394101<br />state: Florida"],"type":"scatter","mode":"lines","line":{"width":1.88976377952756,"color":"rgba(196,154,0,1)","dash":"solid"},"hoveron":"points","name":"Florida","legendgroup":"Florida","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[1997,1998,1999,2000,2001,2002,2003,2004,2005,2006,2007,2008,2009,2010,2011,2012,2013,2014,2015,2016],"y":[9.7339046522975,14.0544416643679,14.2706549577415,14.4852000623941,15.8485649637878,17.2590282745659,17.5358137860894,17.885852009058,17.4209905192256,17.4010843262076,18.115073364228,18.7017884030938,19.4045460596681,20.5730202104896,20.3807975873351,19.8988207653165,19.7037401385605,19.3041443619877,19.1760233528912,18.9449275657535],"text":["year: 1997<br />inf_adj_perchild:  9.733905<br />state: Georgia","year: 1998<br />inf_adj_perchild: 14.054442<br />state: Georgia","year: 1999<br />inf_adj_perchild: 14.270655<br />state: Georgia","year: 2000<br />inf_adj_perchild: 14.485200<br />state: Georgia","year: 2001<br />inf_adj_perchild: 15.848565<br />state: Georgia","year: 2002<br />inf_adj_perchild: 17.259028<br />state: Georgia","year: 2003<br />inf_adj_perchild: 17.535814<br />state: Georgia","year: 2004<br />inf_adj_perchild: 17.885852<br />state: Georgia","year: 2005<br />inf_adj_perchild: 17.420991<br />state: Georgia","year: 2006<br />inf_adj_perchild: 17.401084<br />state: Georgia","year: 2007<br />inf_adj_perchild: 18.115073<br />state: Georgia","year: 2008<br />inf_adj_perchild: 18.701788<br />state: Georgia","year: 2009<br />inf_adj_perchild: 19.404546<br />state: Georgia","year: 2010<br />inf_adj_perchild: 20.573020<br />state: Georgia","year: 2011<br />inf_adj_perchild: 20.380798<br />state: Georgia","year: 2012<br />inf_adj_perchild: 19.898821<br />state: Georgia","year: 2013<br />inf_adj_perchild: 19.703740<br />state: Georgia","year: 2014<br />inf_adj_perchild: 19.304144<br />state: Georgia","year: 2015<br />inf_adj_perchild: 19.176023<br />state: Georgia","year: 2016<br />inf_adj_perchild: 18.944928<br />state: Georgia"],"type":"scatter","mode":"lines","line":{"width":1.88976377952756,"color":"rgba(83,180,0,1)","dash":"solid"},"hoveron":"points","name":"Georgia","legendgroup":"Georgia","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[1997,1998,1999,2000,2001,2002,2003,2004,2005,2006,2007,2008,2009,2010,2011,2012,2013,2014,2015,2016],"y":[8.98101733624935,14.3891737852246,15.1001120731235,15.5745041295886,16.1766938567162,16.8693056479096,17.7310271188617,18.5849521458149,18.8982434570789,20.6174877844751,23.5333451218903,27.8030898496509,26.6086636260152,26.834353171289,26.0424563325942,25.5972134042531,25.1920927222818,25.0004551429302,24.092189244926,24.3283968493342],"text":["year: 1997<br />inf_adj_perchild:  8.981017<br />state: Louisiana","year: 1998<br />inf_adj_perchild: 14.389174<br />state: Louisiana","year: 1999<br />inf_adj_perchild: 15.100112<br />state: Louisiana","year: 2000<br />inf_adj_perchild: 15.574504<br />state: Louisiana","year: 2001<br />inf_adj_perchild: 16.176694<br />state: Louisiana","year: 2002<br />inf_adj_perchild: 16.869306<br />state: Louisiana","year: 2003<br />inf_adj_perchild: 17.731027<br />state: Louisiana","year: 2004<br />inf_adj_perchild: 18.584952<br />state: Louisiana","year: 2005<br />inf_adj_perchild: 18.898243<br />state: Louisiana","year: 2006<br />inf_adj_perchild: 20.617488<br />state: Louisiana","year: 2007<br />inf_adj_perchild: 23.533345<br />state: Louisiana","year: 2008<br />inf_adj_perchild: 27.803090<br />state: Louisiana","year: 2009<br />inf_adj_perchild: 26.608664<br />state: Louisiana","year: 2010<br />inf_adj_perchild: 26.834353<br />state: Louisiana","year: 2011<br />inf_adj_perchild: 26.042456<br />state: Louisiana","year: 2012<br />inf_adj_perchild: 25.597213<br />state: Louisiana","year: 2013<br />inf_adj_perchild: 25.192093<br />state: Louisiana","year: 2014<br />inf_adj_perchild: 25.000455<br />state: Louisiana","year: 2015<br />inf_adj_perchild: 24.092189<br />state: Louisiana","year: 2016<br />inf_adj_perchild: 24.328397<br />state: Louisiana"],"type":"scatter","mode":"lines","line":{"width":1.88976377952756,"color":"rgba(0,192,148,1)","dash":"solid"},"hoveron":"points","name":"Louisiana","legendgroup":"Louisiana","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[1997,1998,1999,2000,2001,2002,2003,2004,2005,2006,2007,2008,2009,2010,2011,2012,2013,2014,2015,2016],"y":[9.00341898947954,13.9811284989119,15.0101200193167,16.2352620661259,17.2916943505406,18.4288655892015,19.2809868380427,20.1123576238751,20.5345288254321,20.4467422328889,20.725719736889,21.6849200055003,23.9288115333766,25.0070212613791,24.898754093796,25.0586392488331,25.2868518326432,24.9817270077765,25.4674270320684,26.4743971824646],"text":["year: 1997<br />inf_adj_perchild:  9.003419<br />state: Mississippi","year: 1998<br />inf_adj_perchild: 13.981128<br />state: Mississippi","year: 1999<br />inf_adj_perchild: 15.010120<br />state: Mississippi","year: 2000<br />inf_adj_perchild: 16.235262<br />state: Mississippi","year: 2001<br />inf_adj_perchild: 17.291694<br />state: Mississippi","year: 2002<br />inf_adj_perchild: 18.428866<br />state: Mississippi","year: 2003<br />inf_adj_perchild: 19.280987<br />state: Mississippi","year: 2004<br />inf_adj_perchild: 20.112358<br />state: Mississippi","year: 2005<br />inf_adj_perchild: 20.534529<br />state: Mississippi","year: 2006<br />inf_adj_perchild: 20.446742<br />state: Mississippi","year: 2007<br />inf_adj_perchild: 20.725720<br />state: Mississippi","year: 2008<br />inf_adj_perchild: 21.684920<br />state: Mississippi","year: 2009<br />inf_adj_perchild: 23.928812<br />state: Mississippi","year: 2010<br />inf_adj_perchild: 25.007021<br />state: Mississippi","year: 2011<br />inf_adj_perchild: 24.898754<br />state: Mississippi","year: 2012<br />inf_adj_perchild: 25.058639<br />state: Mississippi","year: 2013<br />inf_adj_perchild: 25.286852<br />state: Mississippi","year: 2014<br />inf_adj_perchild: 24.981727<br />state: Mississippi","year: 2015<br />inf_adj_perchild: 25.467427<br />state: Mississippi","year: 2016<br />inf_adj_perchild: 26.474397<br />state: Mississippi"],"type":"scatter","mode":"lines","line":{"width":1.88976377952756,"color":"rgba(0,182,235,1)","dash":"solid"},"hoveron":"points","name":"Mississippi","legendgroup":"Mississippi","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[1997,1998,1999,2000,2001,2002,2003,2004,2005,2006,2007,2008,2009,2010,2011,2012,2013,2014,2015,2016],"y":[10.3068986646831,15.6230234354734,16.3513361439109,17.2131110727787,17.9346016198397,18.8284549489617,19.0337543711066,19.2767124548554,19.568909086287,19.1298450939357,19.0120718628168,19.686897713691,22.0130783803761,23.746675927192,23.33902329579,24.1584201678634,24.1914482004941,22.8658685162663,23.2559841759503,23.4813238400966],"text":["year: 1997<br />inf_adj_perchild: 10.306899<br />state: North Carolina","year: 1998<br />inf_adj_perchild: 15.623023<br />state: North Carolina","year: 1999<br />inf_adj_perchild: 16.351336<br />state: North Carolina","year: 2000<br />inf_adj_perchild: 17.213111<br />state: North Carolina","year: 2001<br />inf_adj_perchild: 17.934602<br />state: North Carolina","year: 2002<br />inf_adj_perchild: 18.828455<br />state: North Carolina","year: 2003<br />inf_adj_perchild: 19.033754<br />state: North Carolina","year: 2004<br />inf_adj_perchild: 19.276712<br />state: North Carolina","year: 2005<br />inf_adj_perchild: 19.568909<br />state: North Carolina","year: 2006<br />inf_adj_perchild: 19.129845<br />state: North Carolina","year: 2007<br />inf_adj_perchild: 19.012072<br />state: North Carolina","year: 2008<br />inf_adj_perchild: 19.686898<br />state: North Carolina","year: 2009<br />inf_adj_perchild: 22.013078<br />state: North Carolina","year: 2010<br />inf_adj_perchild: 23.746676<br />state: North Carolina","year: 2011<br />inf_adj_perchild: 23.339023<br />state: North Carolina","year: 2012<br />inf_adj_perchild: 24.158420<br />state: North Carolina","year: 2013<br />inf_adj_perchild: 24.191448<br />state: North Carolina","year: 2014<br />inf_adj_perchild: 22.865869<br />state: North Carolina","year: 2015<br />inf_adj_perchild: 23.255984<br />state: North Carolina","year: 2016<br />inf_adj_perchild: 23.481324<br />state: North Carolina"],"type":"scatter","mode":"lines","line":{"width":1.88976377952756,"color":"rgba(165,138,255,1)","dash":"solid"},"hoveron":"points","name":"North Carolina","legendgroup":"North Carolina","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[1997,1998,1999,2000,2001,2002,2003,2004,2005,2006,2007,2008,2009,2010,2011,2012,2013,2014,2015,2016],"y":[9.92498570680618,15.6302732229233,16.4318430125713,17.3201138675213,18.4757814668119,19.7406707555056,20.2517688982189,20.7367166429758,21.5171215832233,21.0036405809224,21.7994243502617,22.3629396557808,24.0102298483253,25.6089911572635,25.3900947980583,24.7545506898314,24.374427434057,25.0541839580983,25.7806757092476,26.1017952263355],"text":["year: 1997<br />inf_adj_perchild:  9.924986<br />state: South Carolina","year: 1998<br />inf_adj_perchild: 15.630273<br />state: South Carolina","year: 1999<br />inf_adj_perchild: 16.431843<br />state: South Carolina","year: 2000<br />inf_adj_perchild: 17.320114<br />state: South Carolina","year: 2001<br />inf_adj_perchild: 18.475781<br />state: South Carolina","year: 2002<br />inf_adj_perchild: 19.740671<br />state: South Carolina","year: 2003<br />inf_adj_perchild: 20.251769<br />state: South Carolina","year: 2004<br />inf_adj_perchild: 20.736717<br />state: South Carolina","year: 2005<br />inf_adj_perchild: 21.517122<br />state: South Carolina","year: 2006<br />inf_adj_perchild: 21.003641<br />state: South Carolina","year: 2007<br />inf_adj_perchild: 21.799424<br />state: South Carolina","year: 2008<br />inf_adj_perchild: 22.362940<br />state: South Carolina","year: 2009<br />inf_adj_perchild: 24.010230<br />state: South Carolina","year: 2010<br />inf_adj_perchild: 25.608991<br />state: South Carolina","year: 2011<br />inf_adj_perchild: 25.390095<br />state: South Carolina","year: 2012<br />inf_adj_perchild: 24.754551<br />state: South Carolina","year: 2013<br />inf_adj_perchild: 24.374427<br />state: South Carolina","year: 2014<br />inf_adj_perchild: 25.054184<br />state: South Carolina","year: 2015<br />inf_adj_perchild: 25.780676<br />state: South Carolina","year: 2016<br />inf_adj_perchild: 26.101795<br />state: South Carolina"],"type":"scatter","mode":"lines","line":{"width":1.88976377952756,"color":"rgba(251,97,215,1)","dash":"solid"},"hoveron":"points","name":"South Carolina","legendgroup":"South Carolina","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null}],"layout":{"margin":{"t":43.7625570776256,"r":7.30593607305936,"b":40.1826484018265,"l":37.2602739726027},"font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"title":{"text":"Average State Spending Per Child; Deep South","font":{"color":"rgba(0,0,0,1)","family":"","size":17.5342465753425},"x":0,"xref":"paper"},"xaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[1996.05,2016.95],"tickmode":"array","ticktext":["2000","2005","2010","2015"],"tickvals":[2000,2005,2010,2015],"categoryorder":"array","categoryarray":["2000","2005","2010","2015"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"y","title":{"text":"Year (1997-2016)","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"yaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[8.03991371057928,28.7441934753209],"tickmode":"array","ticktext":["10","15","20","25"],"tickvals":[10,15,20,25],"categoryorder":"array","categoryarray":["10","15","20","25"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"x","title":{"text":"$ Per Child (inflation adjust)","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"shapes":[{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":1,"y0":0,"y1":1}],"showlegend":true,"legend":{"bgcolor":null,"bordercolor":null,"borderwidth":0,"font":{"color":"rgba(0,0,0,1)","family":"","size":11.689497716895},"y":0.93503937007874},"annotations":[{"text":"state","x":1.02,"y":1,"showarrow":false,"ax":0,"ay":0,"font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"xref":"paper","yref":"paper","textangle":-0,"xanchor":"left","yanchor":"bottom","legendTitle":true}],"hovermode":"closest","barmode":"relative"},"config":{"doubleClick":"reset","showSendToCloud":false},"source":"A","attrs":{"6a1c744d11ae":{"x":{},"y":{},"colour":{},"type":"scatter"}},"cur_data":"6a1c744d11ae","visdat":{"6a1c744d11ae":["function (y) ","x"]},"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

  2. Use animation to tell an interesting story with the `small_trains` dataset that contains data from the SNCF (National Society of French Railways). These are Tidy Tuesday data! Read more about it [here](https://github.com/rfordatascience/tidytuesday/tree/master/data/2019/2019-02-26).


```r
small_trains_gganim <- small_trains %>%
  group_by(departure_station, 
           arrival_station) %>% 
  mutate(jtime_avg = sum(journey_time_avg)/n()) %>% 
  ggplot(aes(x = jtime_avg,
             y = arrival_station,
             color = arrival_station))+
  geom_point()+
  labs(title = "Average Journey Time; Departure to Arrival Station",
       subtitle = "departure_station:{closest_state}",
       x = "Journey Time (min)",
       y = "Arrival Station")+
  transition_states(departure_station,
                    transition_length = 5,
                    state_length = 4)+
  exit_shrink()+
  theme(legend.position = "none")
animate(small_trains_gganim, duration = 30)
```


```r
anim_save("trains.gif")
```


```r
knitr::include_graphics("trains.gif")
```

![](trains.gif)<!-- -->
## Garden data

  3. In this exercise, you will create a stacked area plot that reveals itself over time (see the `geom_area()` examples [here](https://ggplot2.tidyverse.org/reference/position_stack.html)). You will look at cumulative harvest of tomato varieties over time. You should do the following:
  * From the `garden_harvest` data, filter the data to the tomatoes and find the *daily* harvest in pounds for each variety.  
  * Then, for each variety, find the cumulative harvest in pounds.  

```r
tomatoes <- garden_harvest %>% 
  filter(vegetable %in% c("tomatoes")) %>% 
  complete(variety, date, fill = list(weight = 0)) %>% 
  group_by(variety, date) %>% 
  summarise(daily_harvest_lb = sum(weight)*0.00220462) %>%
  mutate(cum_harvest_lb = cumsum(daily_harvest_lb),
         total_harvest = sum(daily_harvest_lb)) %>% 
  arrange(desc(total_harvest))
```
  * Use the data you just made to create a static cumulative harvest area plot, with the areas filled with different colors for each vegetable and arranged (HINT: `fct_reorder()`) from most to least harvested (most on the bottom).  
  

```r
tomatoes %>% 
 ggplot(aes(x = date, 
            y= cum_harvest_lb,
            fill = fct_reorder(variety, total_harvest)))+
  geom_area()+
  scale_fill_brewer(palette = "Paired")+
  labs(title = "Cumulative Tomato Harvest by Variety",
       x = "date",
       y = "cumulative harvest (lbs)",
       fill = "Variety")
```

  * Add animation to reveal the plot over date. 
  

```r
tomatoes %>% 
 ggplot(aes(x = date, 
            y= cum_harvest_lb,
            fill = fct_reorder(variety, total_harvest)))+
  geom_area()+
  scale_fill_brewer(palette = "Paired")+
  labs(title = "Cumulative Tomato Harvest by Variety",
       x = "date",
       y = "cumulative harvest (lbs)",
       fill = "Variety")+
  transition_reveal(date)
```


```r
anim_save("tomatoes.gif")
```


```r
knitr::include_graphics("tomatoes.gif")
```

![](tomatoes.gif)<!-- -->

## Maps, animation, and movement!

  4. Map my `mallorca_bike_day7` bike ride using animation! 
  Requirements:
  * Plot on a map using `ggmap`.  
  * Show "current" location with a red point. 
  * Show path up until the current point.  
  * Color the path according to elevation.  
  * Show the time in the subtitle.  
  * CHALLENGE: use the `ggimage` package and `geom_image` to add a bike image instead of a red point. You can use [this](https://raw.githubusercontent.com/llendway/animation_and_interactivity/master/bike.png) image. See [here](https://goodekat.github.io/presentations/2019-isugg-gganimate-spooky/slides.html#35) for an example. 
  * Add something of your own! And comment on if you prefer this to the static map and why or why not.


```r
bike_pic <- "https://raw.githubusercontent.com/llendway/animation_and_interactivity/master/bike.png"

mallorca_map <- get_stamenmap(
  bbox = c(left =2.28, bottom=39.41, right=3.03, top=39.8),
  maptype = "terrain",
  zoom = 11)

mallorca_bike_day7 <- mallorca_bike_day7 %>% 
  mutate(image = bike_pic)

ggmap(mallorca_map) +
  geom_path(data = mallorca_bike_day7,
            aes(x = lon, y = lat, color = ele),
            size = .6)+
  geom_point(data = mallorca_bike_day7,
             aes(x = lon, y = lat), 
                 color = "red", size = .10) +
  geom_image(data = mallorca_bike_day7, 
             aes(x = lon, y = lat, 
                 image = bike_pic),
             size = 0.06) +
  labs(title = "Lisa's Mallorca Bike Ride",
       subtitle = "Time: {frame_along}") +
  theme_map()+
  theme(legend.background = element_blank())+
  transition_reveal(time)
```


```r
anim_save("bike.gif")
```


```r
knitr::include_graphics("bike.gif")
```

![](bike.gif)<!-- -->


**I prefer this animated map to the static map; it's just more fun to look at, and it just feels good to see my code work for once :D**


  5. In this exercise, you get to meet my sister, Heather! She is a proud Mac grad, currently works as a Data Scientist at 3M where she uses R everyday, and for a few years (while still holding a full-time job) she was a pro triathlete. You are going to map one of her races. The data from each discipline of the Ironman 70.3 Pan Am championships, Panama is in a separate file - `panama_swim`, `panama_bike`, and `panama_run`. Create a similar map to the one you created with my cycling data. You will need to make some small changes:combine the files (HINT: `bind_rows()`, 2. make the leading dot a different color depending on the event (for an extra challenge, make it a different image using `geom_image()!), 3. CHALLENGE (optional): color by speed, which you will need to compute on your own from the data. You can read Heather's race report [here](https://heatherlendway.com/2016/02/10/ironman-70-3-pan-american-championships-panama-race-report/). She is also in the Macalester Athletics [Hall of Fame](https://athletics.macalester.edu/honors/hall-of-fame/heather-lendway/184) and still has records at the pool. 
  

```r
panama_all <- bind_rows(panama_swim,  
                        panama_bike,panama_run) 
panama_map <- get_stamenmap(
  bbox = c(left = -79.6015, bottom = 8.9059, 
           right =-79.4419, top = 9.0000),
  maptype = "terrain",
  zoom = 13)

ggmap(panama_map)+
  geom_path(data = panama_all,
            aes(x = lon, y = lat,
                group = event),
            size = .6)+
  geom_point(data = panama_all,
             aes(x = lon, y = lat, color = event), 
             size = 5)+
    labs(title = "Lisa's Sister's Panama Triatholon",
       subtitle = "Time: {frame_along}") +
  theme_map()+
  theme(legend.position = "right",
        legend.background = element_blank())+
  transition_reveal(time)
```


```r
anim_save("panama_map.gif")
```


```r
knitr::include_graphics("panama_map.gif")
```

![](panama_map.gif)<!-- -->

## COVID-19 data

  6. In this exercise, you are going to replicate many of the features in [this](https://aatishb.com/covidtrends/?region=US) visualization by Aitish Bhatia but include all US states. Requirements:
 * Create a new variable that computes the number of new cases in the past week (HINT: use the `lag()` function you've used in a previous set of exercises). Replace missing values with 0's using `replace_na()`.  
  * Filter the data to omit rows where the cumulative case counts are less than 20.  
  * Create a static plot with cumulative cases on the x-axis and new cases in the past 7 days on the x-axis. Connect the points for each state over time. HINTS: use `geom_path()` and add a `group` aesthetic.  Put the x and y axis on the log scale and make the tick labels look nice - `scales::comma` is one option. This plot will look pretty ugly as is.
  * Animate the plot to reveal the pattern by date. Display the date as the subtitle. Add a leading point to each state's line (`geom_point()`) and add the state name as a label (`geom_text()` - you should look at the `check_overlap` argument).  
  * Use the `animate()` function to have 200 frames in your animation and make it 30 seconds long. 
  * Comment on what you observe.

```r
covid_weekly <- covid19 %>% 
  group_by(state) %>% 
  mutate(cases_weekly = lag(cases, n = 7, order_by = date),
         new_cases_weekly = (cases - cases_weekly),
         new_cases_weekly = replace_na(new_cases_weekly, 0)) %>% 
  filter(new_cases_weekly >= 20)
```


```r
covid_gganim <- covid_weekly %>% 
  ggplot(aes(x = cases,
             y = new_cases_weekly,
             colour = state))+
  scale_x_log10(
    breaks = scales::trans_breaks
                ("log10", function(x) 10^x),
                labels = scales::math_format(10^.x))+
  scale_y_log10(
    breaks = scales::trans_breaks
                ("log10", function(x) 10^x),
                labels = scales::math_format(10^.x))+
  geom_point()+
  geom_text(aes(label = state), check_overlap = TRUE) +
  scale_color_viridis_d("viridis") +
  geom_path(aes(group = state)) +
  labs(title = "COVID Cases Weekly by State",
       subtitle = "Date: {frame_along}",
       x = "Cumulative Case Count",
       y = "New Cases Weekly")+
  theme(legend.position = "none")+
  transition_reveal(date)
animate(covid_gganim, nframes = 200, duration = 30)
```


```r
anim_save("covid_gganim.gif")
```


```r
knitr::include_graphics("covid_gganim.gif")
```

![](covid_gganim.gif)<!-- -->


**It seems the most heavily populated states really jumped the gun when COVID-19 broke out. You could see some spiked in every state at around 15 seconds, however at that same time the number of cases were trending down for every state. Also, I don't think this is a great graph to display this data; she's kinda ugly, haha**


  7. In this exercise you will animate a map of the US, showing how cumulative COVID-19 cases per 10,000 residents has changed over time. This is similar to exercises 11 & 12 from the previous exercises, with the added animation! So, in the end, you should have something like the static map you made there, but animated over all the days. Put date in the subtitle. Comment on what you see.

```r
us_map <- map_data("state")

latest_covid <- covid19 %>% 
  mutate(state = str_to_lower(state)) %>% 
  left_join(census_pop_est_2018,covid19,
            by = "state") %>% 
  group_by(state, date, est_pop_2018) %>% 
  mutate(covid_per_10000 = cases/est_pop_2018*10000)

latest_covid %>% 
  mutate(state = str_to_lower(state), weekday = wday(date, label = TRUE)) %>% 
  filter(weekday == "Fri") %>% 
  ggplot() +
  geom_map(aes(map_id = state,
               fill = covid_per_10000, group = date),
           map = us_map)+
  expand_limits(x = us_map$long, y = us_map$lat)+
  theme(legend.position = "right") +
  labs(title = "Most Recent Cumulative Number of COVID19 Cases in America",
       subtitle = "Date: {closest_state}",
       fill = "Case Count") +
  theme_map() +
  transition_states(date, transition_length = 0)
```


```r
anim_save("covid_map.gif")
```


```r
knitr::include_graphics("covid_map.gif")
```

![](covid_map.gif)<!-- -->


**Again, it seems more populated states/states with big cities jumped with really high COVID counts, whereas less populated states (like Montana; there's, like, 6 people, a horse, and maybe a cow)have relatively low counts the whole animation. Otherwise, about halfway through the animation, the rate at which each states' covid case increases/decreases starts to level out.**


## GitHub link

  8. Below, provide a link to your GitHub page with this set of Weekly Exercises. Specifically, if the name of the file is 05_exercises.Rmd, provide a link to the 05_exercises.md file, which is the one that will be most readable on GitHub. If that file isn't very readable, then provide a link to your main GitHub page.
 [GitHub Link.md](https://github.com/cwoerman/Weekly-Exercises-5/blob/master/05_exercises.md)

