# scmrstudio
Foreign Direct Investment through Synthetic Control Method 

#install.packages("Synth")
library(readxl)
library(Synth)
library(SCtools)
library(Synth)


data <- read_excel("/Users/senyanimsaritennakoon/Desktop/examplesenya3 .xlsx")

data <- as.data.frame(data)

#data(synthdata)
#read_excel(Final_data_overview_)
#read.csv(Final_data_overview_csv.csv)
#data<-read_excel(Final_data_overview_) 
#demo <- data[, c("country","year", "id","ifdi","gdp","inflation","trade","gdpcap","gcf","consumption","erate")]
#mode(demo$id)

dataprep.out.sl.ifdi <-
  dataprep(
    foo = data
    ,predictors= c("gdp",
                   "gdpcap",
                   "consumption",
                   "erate"
    )
    ,predictors.op = c("mean")
    ,dependent     = c("ifdi")
    ,unit.variable = c("id")
    ,time.variable = c("year")
    ,special.predictors = list(
      list("inflation",1970:1976,c("mean")),
      list("trade",seq(1970,1976,2),c("mean")),
      list("gcf",1976,c("mean")))
    
    ,treatment.identifier  = 1
    ,controls.identifier   = c(6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24)
    ,time.predictors.prior = c(1970:1976)
    ,time.optimize.ssr = c(1970:1976)
    ,unit.names.variable = c("country")
    ,time.plot = c(1970:1990)
  )


synth.out <- synth(data.prep.obj = dataprep.out.sl.ifdi)

synth_tab.br.polity  <- synth.tab(
  dataprep.res = dataprep.out.sl.ifdi,
  synth.res = synth.out
) 

print(synth_tab.br.polity)

path.plot(synth.res = synth.out,
          dataprep.res = dataprep.out.sl.ifdi,
          Ylab = c("ifdi_growth"),
          Xlab = c("year"),
          Ylim = c(-1000000,100000000), Legend=c("Srilanka","Syntheticsrilanka"),
          Legend.position = "bottomright"
)

abline(v = 1977)

gaps.plot(dataprep.res = dataprep.out.sl.ifdi,
          synth.res = synth.out,
          Ylab = c("ifdi_growth"),
          Xlab = c('year')
)
abline(v = 1977)

install.packages("stringi")
require(stringi)

tdf.ifdi <- generate.placebos(
  dataprep.out=dataprep.out.sl.ifdi,
  synth.out=synth.out.sl.ifdi)
)
