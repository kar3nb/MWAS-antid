# MWAS-antid

#formatting assay data (into matrix)

data <- readRDS("mvalues.chr14.rds") 

mvalues <- as.matrix(data)

class(mvalues)

[1] "matrix" "array"

#creating phenotype data 

data_t <- t(data)

data_t1 <- data_t[c(1)]

vec <- c(sample(0:1,size=5101,replace=TRUE))

pheno_smoking <- cbind(data_t1, smoking = vec)

pheno_smoking1 <- pheno_smoking[,c(2)]

#producing an annotated data frame

pheno_smoking1 <- as.data.frame(pheno_smoking1)

pheno_ann <- new("AnnotatedDataFrame",
+ data=pheno_smoking1)

pheno_ann
An object of class 'AnnotatedDataFrame'
  rowNames: 200816840050_R01C01 200816840050_R03C01 ...
    200793300140_R06C01 (5101 total)
  varLabels: pheno_smoking1
  varMetadata: labelDescription

#creating expression set

eSet <- ExpressionSet(assayData=mvalues,
+ phenoData=pheno_ann)

eSet
ExpressionSet (storageMode: lockedEnvironment)
assayData: 29367 features, 5101 samples
  element names: exprs
protocolData: none
phenoData
  sampleNames: 200816840050_R01C01 200816840050_R03C01 ...
    200793300140_R06C01 (5101 total)
  varLabels: pheno_smoking1
  varMetadata: labelDescription
featureData: none
experimentData: use 'experimentData(object)'
Annotation: 

#fitting model

design <- model.matrix(~eSet$pheno_smoking1)

fit <- lmFit(eSet, design)



