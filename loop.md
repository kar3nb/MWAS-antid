# MWAS-antid

data <- readRDS("mvalues.chr14.rds")

data <- t(data)

data <- data[1000,1000]

 vec <- c(sample(0:1,size=1000,replace=TRUE))
 
data1 <- data

data1 <- cbind(data, smoking = vec)

data1 = as.data.frame(data1)

cpg <- data1$data

modsummaries <- list()

for (i in cpg){
+ modsummaries <- summary(
+ lm(cpg ~ smoking, data = data1))
+ }

