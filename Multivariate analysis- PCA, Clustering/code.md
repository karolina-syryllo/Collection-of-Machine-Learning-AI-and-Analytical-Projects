# Downloading dataset and creating subsets
dd=read.csv("Final_dataset4.csv", header=TRUE, row.names = 1)
dd=dd[,order(colnames(dd))]
dd.econ=dd[,1:7]
dd.env=dd[,8:10]
dd.health=dd[,11:14]
dd.h.env=dd[,8:14]
dd.h.econ=dd[,c(11,12,13,14,1,2,3,4,5,6,7)]
dd.env.econ=dd[,1:10]
#correlation analysis for variables in group health
install.packages("GGally")
library("GGally")
dd.health.log=log(dd.health)
ggpairs(dd.health.log)
round(cor(dd.health.log),2)
cor.test(dd.health.log$health1, dd.health.log$health2)
cor.test(dd.health.log$health1, dd.health.log$health4)
cor.test(dd.health.log$health2, dd.health.log$health4)
#principal component analysis
library("factoextra")
d1=dd[,1:14]
pp=princomp(d1, cor=TRUE)
pp$loadings displays loadings
fviz_pca_biplot(pp, created biplot
col.var = "red",
col.ind = "black")
summary(pp) shows how much variance is explained by each component
fviz_contrib(pp, choice = "var", axes = 1, top = 6)  displays plot showing variable importance for first component
#clustering analysis
dds=scale(d1)
km.res <- kmeans(dds, 3, nstart = 25)
fviz_cluster(km.res, data = dds,
ellipse.type = "convex",
palette = "jco",
ggtheme = theme_minimal())  displays cluster plot
# discriminant analysis
d1=dd[dd[,15]=="Low income",1:14]
d2=dd[dd[,15]=="High income",1:14]
m1=apply(d1, 2, mean)
m2=apply(d2, 2, mean)
v1=var(d1)
v2=var(d2)
n1=length(d1)
n2=length(d2)
vpool=((n1-1)*v1 + (n2-1)*v2)/(n1+n2-2)
avector=solve(vpool)%*%(m1-m2)
#round(avector)
astar=diag(vpool)^(-0.5)*avector
round(astar, 3)
#takes out UK from observations
ddnew=dd[-103,]
d1=ddnew[ddnew[,15]=="Low income",1:14]
d2=ddnew[ddnew[,15]=="High income",1:14]
m1=apply(d1,2,mean)
m2=apply(d2,2,mean)
v1=var(d1)
v2=var(d2)
n1=length(d1)
n2=length(d2)
vpool=((n1-1)*v1 + (n2-1)*v2)/(n1+n2-2)
avector=solve(vpool)%*%(m1-m2)
astar=diag(vpool)^(-0.5)*avector
sum(astar*dd[103,1:13])
d1m=as.matrix(d1)
d2m=as.matrix(d2)
z1=d1m%*%astar
z2=d2m%*%astar
hist(z1,20)
hist(z2, 20)
