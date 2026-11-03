---
title: Litterature on accumulation
author: SG
tags: REPHY grazer
---
Using the REPHYTOX data set downloaded at https://www.seanoe.org/data/00361/47251/ I isolated the tox. info in grazers, only two occurences in Crepidula and Littorina, see script : 

#Load REPHYTOX CSV file

rephytox<-read.csv2("/Users/sylvain_gaillard/Downloads/REPHYTOX_1987-2022.csv",header=TRUE,stringsAsFactors=FALSE,fileEncoding="Latin1")

head(rephytox)
summary(rephytox)
colnames(rephytox)
View(rephytox)


new_df<-rephytox[,c("Echantillon...Libellé.du.taxon.support","Echantillon...Libellé.du.support")]
new_df
summary(new_df)
unique(new_df[[2]])

gastropodes<-new_df[new_df[[2]]=="Gastéropode",]
View(gastropodes)

grazers<-rephytox[rephytox$Echantillon...Libellé.du.taxon.support %in% c("Crepidula fornicata","Littorina littorea"),]

View(grazers)

# Data grazers:
Gisement de l'Auberlac'h 26/10/2005 Crepidula fornicata domoic acid 0.15 mg/kg in whole soft tissues HPLC UV
Pointe du Chateau 16/08/2012 Littorina littorea PST 33 µg 100g in whole soft tissue MBA but corrected in 2014, true value < LD... 
