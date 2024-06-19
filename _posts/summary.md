# Preface

To really understand and develop a good model. There are some parts of knowledge that you need to know, which are related to different phase of modeling building process. All these models form the equation of likelihood function of each observation in a popPK analysis.

1. Structural model
In most cases, some information is available beforehand about the structural model based on knowledge on the underlying system. E.g., PK equations

2. Inter-individual variability:
Information about IIV can also be available in terms of physiological features of the study population such as polymorphism in metabolic enzymes or age-related alterations of particular organs.

3. residual variability
That part accounts for all remaining variability which is not explained by the structural or parameter-variability model parts. This RUV arises for example from physiological intra-individual variation, assay error, errors in independent variables and model misspecification.

All of this will form the mathematical formula that are assessed to evaluate the model fit. In NONMEM, The Nonmem methodology was based on maximum likelihood methods that use various approximations to compute and minimize the objective function (-2 log likelihood of the model parameters given the data) in order to estimate population PK and PK-PD model parameters

