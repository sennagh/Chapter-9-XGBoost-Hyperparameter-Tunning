# Chapter-9-XGBoost-Hyperparameter-Tunning
#Page 222
model<-"xgboost"
cfg<-getMlConfig(
target=target,
model=model,
data= dfCensus,
task.type=task.type,
nobs= nobs,
nfactors=nfactors,
nnumericals=nnumericals,
cardinality=cardinality,
data.seed=data.seed,
prop= 2/3
)

#Page 224
nFeatures <- sum(task$task.desc$n.feat)
modelCfg <- getModelConf(
task.type = task.type,
model = model,
nFeatures = nFeatures
)

#Page 225 & 226
result <- spot(
x = NULL,
fun = objf,
lower = cfg$lower,
upper = cfg$upper,
control = list(
types = cfg$type,
time = list(maxTime = timebudget / 60),
noise = TRUE,
OCBA = TRUE,
OCBABudget =3,
seedFun = 123,
designControl = list(
replicates = Rinit,
size = initSizeFactor * length(cfg$lower)
),
replicates =2,
funEvals = Inf,
modelControl = list(
target = "ei",
useLambda = TRUE,
reinterpolate = FALSE
),
optimizerControl = list(funEvals = 200 * length(cfg$lower)),
multiStart =2,
parNames = cfg$tunepars,
yImputation = list(
handleNAsMethod = handleNAsMean,
imputeCriteriaFuns = list(is.infinite, is.na, is.nan),
penaltyImputation =3
)
)
)
load("supplementary/ch09-CaseStudyII/xgboost00001.RData")

#Page 233 & 234
target <- "age"
task.type <- "classif"
nobs <- 1e4
nfactors <- "high"
nnumericals <- "high"
cardinality <- "high"
data.seed <- 1
cachedir <- "oml.cache"
dfCensus <- getDataCensus(
task.type = task.type,
nobs = nobs,
nfactors = nfactors,
nnumericals = nnumericals
cardinality = cardinality,
data.seed = data.seed,
cachedir = cachedir,
target = target
)
task <- getMlrTask(
dataset = dfCensus,
task.type = "classif",
data.seed = 1
)
model <- "xgboost"
cfg <- getMlConfig(
target = target,
model = model,
data = dfCensus,
task.type = task.type,
nobs = nobs,
nfactors = nfactors,
nnumericals = nnumericals,
cardinality = cardinality,
data.seed = data.seed,
prop = 2 / 3
)
transformX(cfg$defaults, cfg$transformations)
