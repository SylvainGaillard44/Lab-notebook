---
title: Trophic transfert of toxicity - sea snails exposure
author: SG
tags: LEMAR toxicity sea snails
---

I worked on the sea snails data. Make CR and cell consumption calculations. 
I tried different correlations: 

Toxicity (OA+DTXs) and PTXs vs. number of cells consummed during expo (24 and 96h)
# Script 
library(tidyselect)
library(purrr)
library(ggpubr)
library(readxl)
library(ggplot2)
library(cowplot)
library(gridExtra)
library(Publish)
library(car)
library(multcomp)
library(PMCMRplus)
library(RColorBrewer)
library(corrplot)
library(dplyr)
library(ggcorrplot)


#Load file

correlation_snails <- read.csv("LEMAR/Sea-snails_ACR_toxin.csv", header = TRUE)


#Select exposure phase

correlation_expo <- correlation_snails[correlation_snails$flask %in% c(1, 2), ]


#Select variables

correlation_expo <- correlation_expo[, c("species", "flask",
                                         "cumulative_apparent_cells_removed_ind",
                                         "oa_dtx2_ng_g_wet_reported",
                                         "ptx2_ng_g_wet")]

colnames(correlation_expo) <- c("species", "flask", "nb_cells", "DST", "PTX2")


#Convert to numeric

correlation_expo$nb_cells <- as.numeric(correlation_expo$nb_cells)
correlation_expo$DST <- as.numeric(correlation_expo$DST)
correlation_expo$PTX2 <- as.numeric(correlation_expo$PTX2)

correlation_expo


#Spearman correlation - DST (OA + DTX2)


res_spearman_DST <- cor.test(correlation_expo$nb_cells,
                             correlation_expo$DST,
                             method = "spearman",
                             exact = FALSE)

res_spearman_DST


#Scatterplot - DST

scatterplot_DST <- ggscatter(correlation_expo, x = "nb_cells", y = "DST",
                             add = "none", conf.int = FALSE,
                             cor.coef = TRUE, cor.method = "spearman",
                             cor.coeff.args = list(cor.coef.name = "rho"),
                             color = "species",
                             xlab = "Cumulative apparent cell removal (cells/individual)")

scatterplot_DST <- scatterplot_DST +
  geom_smooth(aes(x = nb_cells, y = DST, group = 1),
              method = "lm", se = FALSE,
              color = "black") +
  labs(y = expression(atop("DST concentration",
                           paste("(ng OA + DTX2 g"^{-1}, " wet weight)"))))

scatterplot_DST

ggsave("scatterplot_snails_DST.tiff", dpi = 300)

#Spearman correlation - PTX2


res_spearman_PTX2 <- cor.test(correlation_expo$nb_cells,
                              correlation_expo$PTX2,
                              method = "spearman",
                              exact = FALSE)

res_spearman_PTX2


#Scatterplot - PTX2

scatterplot_PTX2 <- ggscatter(correlation_expo, x = "nb_cells", y = "PTX2",
                              add = "none", conf.int = FALSE,
                              cor.coef = TRUE, cor.method = "spearman",
                              cor.coeff.args = list(cor.coef.name = "rho"),
                              color = "species",
                              xlab = "Cumulative apparent cell removal (cells/individual)")

scatterplot_PTX2 <- scatterplot_PTX2 +
  geom_smooth(aes(x = nb_cells, y = PTX2, group = 1),
              method = "lm", se = FALSE,
              color = "black") +
  labs(y = expression(atop("PTX2 concentration",
                           paste("(ng PTX2 g"^{-1}, " wet weight)"))))

scatterplot_PTX2

ggsave("scatterplot_snails_PTX2.tiff", dpi = 300)

# Results
No apparent correlation.

Toxicity (OA+DTXs) and PTXs vs. number of cells consumed during exposure and depuration. Just to try..

# Script 

#Select variables - All exposure and depuration samples

correlation_all <- correlation_snails[, c("species", "flask",
                                          "cumulative_apparent_cells_removed_ind",
                                          "oa_dtx2_ng_g_wet_reported",
                                          "ptx2_ng_g_wet")]

colnames(correlation_all) <- c("species", "flask", "nb_cells", "DST", "PTX2")


#Convert to numeric

correlation_all$flask <- as.numeric(correlation_all$flask)
correlation_all$nb_cells <- as.numeric(correlation_all$nb_cells)
correlation_all$DST <- as.numeric(correlation_all$DST)
correlation_all$PTX2 <- as.numeric(correlation_all$PTX2)

#Identify exposure and depuration samples

correlation_all$phase <- ifelse(correlation_all$flask %in% c(1, 2),
                                "Exposure", "Depuration")

correlation_all$phase <- factor(correlation_all$phase,
                                levels = c("Exposure", "Depuration"))

correlation_all

#Spearman correlation - DST (OA + DTX2)


res_spearman_DST <- cor.test(correlation_all$nb_cells,
                            correlation_all$DST,
                            method = "spearman",
                            exact = FALSE)

res_spearman_DST


#Scatterplot - DST

scatterplot_DST <- ggscatter(correlation_all, x = "nb_cells", y = "DST",
                            add = "none", conf.int = FALSE,
                            cor.coef = TRUE, cor.method = "spearman",
                            cor.coeff.args = list(cor.coef.name = "rho"),
                            color = "species", shape = "phase",
                            xlab = "Cumulative apparent cell removal (cells/individual)")

scatterplot_DST <- scatterplot_DST +
  geom_smooth(aes(x = nb_cells, y = DST, group = 1),
              method = "lm", se = FALSE,
              color = "black") +
  labs(y = expression(atop("DST concentration",
                          paste("(ng OA + DTX2 g"^{-1}, " wet weight)"))))

scatterplot_DST

ggsave("scatterplot_snails_DST_all.tiff", dpi = 300)

#Spearman correlation - PTX2

res_spearman_PTX2 <- cor.test(correlation_all$nb_cells,
                             correlation_all$PTX2,
                             method = "spearman",
                             exact = FALSE)

res_spearman_PTX2


#Scatterplot - PTX2

scatterplot_PTX2 <- ggscatter(correlation_all, x = "nb_cells", y = "PTX2",
                             add = "none", conf.int = FALSE,
                             cor.coef = TRUE, cor.method = "spearman",
                             cor.coeff.args = list(cor.coef.name = "rho"),
                             color = "species", shape = "phase",
                             xlab = "Cumulative apparent cell removal (cells/individual)")

scatterplot_PTX2 <- scatterplot_PTX2 +
  geom_smooth(aes(x = nb_cells, y = PTX2, group = 1),
              method = "lm", se = FALSE,
              color = "black") +
  labs(y = expression(atop("PTX2 concentration",
                          paste("(ng PTX2 g"^{-1}, " wet weight)"))))

scatterplot_PTX2

ggsave("scatterplot_snails_PTX2_all.tiff", dpi = 300)

