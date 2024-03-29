
 <div id="MENUE" />
 

<div id="INCLASS_1" />

### INCLASS 1

**1)** Create within the R-environment these two vectors: `x=[1L,2L,3L]` and `y=[1,2,3]`. What are the types of x and y?

```r
 x=c(1L,2L,3L)
 y=c(1,2,3)
 class(x) # integer
 class(y) # numeric
```
 
 **2)** Multiply x and y, what are the dimensions and type of the resulting vector?
 
 
```r
 z=x*y
 class(z) # numeric
```
The product of integer and numeric yields a numeric object. The length of z is the same as that of x and y.

 **3)** Add names to x `['x1','x2','x3']`, and, using indexing by name, replace the second entry of x with the value 1.1. What is the type of x after the replacement?
 
 ```r
  names(x)=c('x1','x2','x3')
  x['x2']=1.1
  class(x)
 ```
 When a numeric value is inserted the whole itneger vector is promoted to numeric.
 
 **4)** Create a matrix (W) using `cbind(x,y)`. What is the class of W?
  
  ```r
   x=c(1L,2L,3L)
   y=c(1,2,3)  
   W=cbind(x,y)
  ```
  When a matrix is created by binding a numeric and a integer vector the whole matrix is promoted to numeric.
  
 **5)** Apply the log() function to the W matrix created in (4)
 
 ```r
  log(W)
 ```

What do yo conclude about the behavior of functions that take scalar arguments when we apply them to arrays?

When functions that take scalar inputs are called on arrasy, the function is applied to each entry of the array, the return value has the same dimensions as the input.

 **6)** Pick your two favorite cars and for each define the brand, model, year, and engine size. 
   - Create a list (length 2, one entry by car), each element of the list will contain the brand, model and year. Access to the information of the 1st and 2nd entry of the list using integer-indexing, and using `$`. Hint: see this [example](https://github.com/gdlc/STAT_COMP/blob/master/HANDOUTS/RIntro.md#lists).
   - Replace 2nd entry of the list CARS with your first car and add a third entry to the list with your 2nd car.

```r
 CARS=list()
 CARS[[1]]=list(brand='Toyota',model='Corolla',year=2012,engineSize=1500)
 CARS[[2]]=list(brand='Dodge',model='Ram',year=2010,engineSize=3600)

 CARS[[1]]
 CARS[[2]]
 
 tmp=CARS[[2]]
 
 CARS[[2]]=CARS[[1]]
 CARS[[3]]=tmp
 
```
[back to list](#MENUE)



<div id="INCLASS_2" />

### INCLASS 2

**Reading the data**

```r
 DATA=read.table('https://web.stanford.edu/~hastie/ElemStatLearn/datasets/prostate.data',header=T)
 head(DATA)
 dim(DATA)
 str(DATA)
 tail(DATA)
```

**Writing/reading comma-separated file**

```r
write.table(DATA,file='DATA.csv',sep=',') # consider also write.csv()
DATA2=read.csv('DATA.csv')
all.equal(DATA,DATA2)

DATA3=read.table('DATA.csv',sep=',')
all.equal(DATA,DATA3)

```

**Summary statistics**

```r
 for(i in (1:9)[-5]){
   print(summary(DATA[,i]))
 }
 
 table(DATA[,5])

```

**Using apply**

```r
  apply(FUN=summary,X=DATA,MARGIN=2)
```

**Histograms**

```r
  par(mfrow=c(3,3)) # creates a 3x3 gri
  
  # fills the grid with plots
   for(i in 1:9){
     hist(DATA[,i],main=colnames(DATA)[i])
   }
```

**Scatterplots and boxplots**

```r
  par(mfrow=c(2,4))
   for(i in 1:8){
     if(i!=5){
      plot(lpsa~DATA[,i],main=colnames(DATA)[i],xlab=colnames(DATA)[i],data=DATA)
     }else{
      boxplot(lpsa~DATA[,i],main=colnames(DATA)[i],xlab=colnames(DATA)[i],data=DATA)
     }
   }
```
**Heatmap**

```r
dev.off()
heatmap(cor(as.matrix(DATA[,1:9])),symm=TRUE)
```

**Hierarchical clustering**

```r
 D=dist(t(scale(DATA[,1:9]))) #Euclidean distance between columns, aftern centering and scaling
 HC=hclust(D)
 plot(HC)
```

**Heatmap based on absolute-value correlation**

```r
 heatmap(abs(cor(as.matrix(DATA[,1:9]))),symm=TRUE)
```
[back to list](#MENUE)



<div id="INCLASS_3" />

### INCLASS 3


**1)** For loop

```r
  for(x in 1:5){
    print(x)
  }
  
  for(i in c('a','b','d','c')){
    print(i)
  }
  
  for(z in c(TRUE,FALSE,TRUE,TRUE)){ print(z) }
``` 

**2)** Nested loops

Write code with a loop nested within another loop. For the first iterator use `(i in 1:5)`, for the inner loop use `(j in c('a','b'))`, inside the inner loop, print `i` and `j`, e.g., `print(paste(i,j))`.

```r
for(i in 1:5){
  for(j in c('a','b')){
     message(i,j)
  }
}
```

**3)** While loop
```r
i=0
 while(i<=5){
  message(i)
  i=i+1
 }
 
 print(i)
```

**4)**  Recoding: 3-strategies

**Note**: The examples below are meant to illustrate how to create functions. For data recoding you can use built-in function such as `map()` or `cut()`. 


 The goal is to recode the `lgleason` score variable into three levels, `<=6`, `7`, and `>=8`. We will consider three strategies: 
   - `for` loop with `if(){}` statment inside
   - `ifelse` this function takes three arguments, a boolean, a vector for the TRUE entries and a vector for the FALSE entries, e.g., `ifelse(c(1,2,3)<=2, "A","B")`) 
      Hint: consider nesting an `ifelse` statmente within another `ifelse`.
   - `cut`, try `help(cut)`.
 
 ```r
  DATA$gleason_1=NA
  for(i in 1:nrow(DATA)){
    if( DATA$gleason[i]<=6){
      DATA$gleason_1[i]="G<=6"
    }else{
      if(DATA$gleason[i]==7){
        DATA$gleason_1[i]="G=7"
      }else{
        DATA$gleason_1[i]="G>=8"
     }
   }
  }
  boxplot(gleason~gleason_1,data=DATA)
```  

```r
  DATA$gleason_2=ifelse(DATA$gleason<=6,"G<=6",ifelse(DATA$gleason<8,"G=7","G>=8"))
  table(DATA$gleason_1,DATA$gleason_2)
```

```r
  DATA$gleason_3=cut(DATA$gleason,breaks=c(0,6,7,12))
 table(DATA$gleason_1,DATA$gleason_3)
```
 
**5)** Functions 

```r
 recodeOne=function(x,breaks){
  ans="G<=6"
  if(x==7){
    ans="G=7"
  }
  
  if(x>7){
   ans="G>=8"
  }
  return(ans)
 }

```
Note: the above function recodes one entry of the vector, to recode anentire vector we can use either `sapply()` or ifelse. 

```r
 recode=function(x,breaks){
  sapply(FUN=recodeOne,X=x,breaks=breaks)
 }
 DATA$gleason_4=recode(DATA$gleason,c(6,7))
 table(DATA$gleason_1,DATA$gleason_4)
```
[back to list](#MENUE)


<div id="INCLASS_4" />

### INCLASS 4

**A function to produce the transpose of a matrix**

```r
  myT=function(X){
   nRows=nrow(X)
   nCols=ncol(X)
   W=matrix(nrow=nCols,ncol=nRows,NA)
   rownames(W)=colnames(X)
   colnames(W)=rownames(X)
   
   for(i in 1:nCols){
     for(j in 1:nRows){
       W[i,j]=X[j,i]
     }
   }
  
  return(W)
}


## Test
 X=matrix(nrow=5,ncol=4,rnorm(20))
 Xt=myT(X)
 all.equal(Xt,t(X))
```
**A function to produce matrix producs using scalar operations**

```r
 matProd=function(A,B){
   conform=ncol(A)==nrow(B)
   if(conform){
     W=matrix(nrow=nrow(A),ncol=ncol(B),NA)
     for(i in 1:nrow(A)){
      for(j in 1:ncol(B)){
       W[i,j]=sum(A[i,]*B[,j])
      }
     }
     return(W)
   }else{
    stop('Matrix do not conform')
   }
 }
 
 A=matrix(nrow=5,ncol=3,rnorm(15))
 B=matrix(nrow=3,ncol=4,runif(12))
 TMP=matProd(A,B)
```
[back to list](#MENUE)

<div id="INCLASS_5" />

### INCLASS 5

**A function to fit models via OLS**

```r
 fitXy=function(y,X){
   C=crossprod(X) #X'X, the 'Coefficients Matrix '
   rhs=crossprod(X,y) # X'y the 'right-hand-side'
   bHat=solve(C,rhs)
   return(bHat)
 }

```
**Testing the function**

```r
n=300
 x1=rbinom(size=1,n=n,prob=.5)
 x2=rnorm(n)
 mu=100
 b1=2
 b2=-3
 
 signal=mu + x1*b1 + x2*b2
 error=rnorm(n)
 y=signal+error
 
 coef(lm(y~x1+x2))
 
 fitXy(y,X=cbind(1,x1,x2))
 
```

**Using a formula interface**

```r
 ## First a formula to generate the incidence matrix from a fomrula
 
 
 getXy=function(formula, ...){

	formula=as.character(formula)[-1]
	response=formula[1]
	predictors=formula[-1]
	X=model.matrix(formula(paste0('~',predictors)),...)
	
	y=get(response,...)
	
	return(list(y=y,X=X))	
}

## Now a function that takes a formula as an input
fitOLS=function(formula,...){
	tmp=getXy(formula,...)
	X=tmp$X
	y=tmp$y
	fm=fitXy(X=X,y=y)
	return(fm)
}

```

**Testing**

```r
 fitOLS(y~x1+x2)
 
 ## passing variables trhough a data frame
 DATA=data.frame(y=y,z1=x1,z2=x2)
 fitOLS(y~z1+z2,DATA)
 
 ## Interested to learn about R-environments? See http://adv-r.had.co.nz/Environments.html
```
**Estimates, SE, t-stat, and p-values**

```r
 fitXy=function(y,X){
   C=crossprod(X) #X'X, the 'Coefficients Matrix '
   rhs=crossprod(X,y) # X'y the 'right-hand-side'
   CInv=solve(C)
   bHat=CInv%*%rhs
   eHat=y-X%*%bHat
   RSS=sum(eHat^2)
   DF=ncol(X)
   n=nrow(X)
   vE=RSS/(n-DF)
   
   VCOV=CInv*vE
   SE=sqrt(diag(VCOV))
   t_stat=bHat/SE
   p_value=pt(abs(t_stat),lower.tail=FALSE,df=DF)*2
   
   RES=cbind('Estimate'=bHat,'SE'=SE,'t_stat'=t_stat,'pVal'=p_value)
   rownames(RES)=colnames(X)
   return(RES)
 }


fitXy(y,X=cbind(1,x1,x2))
summary(lm(y~x1+x2))

```
[back to list](#MENUE)



### INCLASS 6

<div id="INCLASS_6" />

**The data**
```r
 n=500
 p=5
 X=matrix(nrow=n,ncol=p,data=rnorm(n*p))
 y=X[,3]-X[,5]+rnorm(n)
 
 C=crossprod(X)
 r=crossprod(X,y)
```

**A function to solve the system Cx=r using QR**

```r
 solveSysQR=function(C,r){
 	QR=qr(C)
	Q=qr.Q(QR)
	R=qr.R(QR)
	sol=backsolve(R,crossprod(Q,r))
	return(sol)
 }
 
 # testing
 solve(C,r)
 solveSysQR(C,r)

```

**A function to fit a linear model via QR**


```r
 fitLMQR=function(X,y){
  	QR=qr(X)
	Q=qr.Q(QR)
	R=qr.R(QR)
	Qy=crossprod(Q,y)
	sol=backsolve(R,Qy)
	return(sol)
 }
 
 fitLMQR(cbind(1,X),y)
 coef(lm(y~X))

```

**Gauss Seidel**


```r
  
 solveSysGS=function(C,rhs,tol=1e-5,maxIter=1000){
 	p=nrow(C)
	b=rhs/diag(C) # initial values
	
	for(i in 1:maxIter){
	  b0=b
	  
	  # Gauss Seidel iterations
	  for(j in 1:p){
	     b[j]=(rhs[j]-sum(C[j,-j]*b[-j]))/C[j,j]
	  }
	  if(max(abs(b0-b))<tol){
	  	break()
	  }
	}
	if(i==maxIter){ 
	   message('Algorithm did not converge before ', i,' iterations.')
	}else{
	   message('Converged after ', i,' iterations.')
	}
	return(b)
  }
  
  solveSysGS(crossprod(X),crossprod(X,y))
  coef(lm(y~X-1))
```

[back to list](#MENUE)



### INCLASS 7


**Note**: In this case, because we are using the `DF` argument to define the spline, models are not strictly nested and therefore, I would not use the f-test in this example. To use the F-test we need to be sure models are nested, that could be achieved if we start with a set of knots and keep adding knots one at a time...


<div id="INCLASS_7" />


```r

set.seed(195021)
x<-seq(from=0, to=2*pi,by=0.05)
f0<-function(x){ 100+sin(2*x)+cos(x/2) }
R2<-0.5
y<-f0(x)+rnorm(n=length(x),sd=sqrt(var(f0(x))*(1-R2)/R2))
plot(y~x)
lines(x=x,y=f0(x),col='red',lwd=2)


library(splines)
RES=data.frame(DF=seq(from=4,to=20,by=2),RSS=NA,RSq=NA,AdjRSq=NA,FStat=NA,pValue=NA,BIC=NA,AIC=NA)

fm0=lm(y~1)


for(i in 1:nrow(RES)){
	Z=bs(degree=3,x=x,df=RES$DF[i],intercept=FALSE) # Note index [i] in DF
	fm=lm(y~Z)
	
	RES$RSS[i]=sum(residuals(fm)^2)
	RES$RSq[i]=summary(fm)$r.sq
	RES$AdjRSq[i]=summary(fm)$adj.r.sq

	
	ANOVA=anova(fm0,fm)
	
	RES$FStat[i]= ANOVA[[5]][2]
	RES$pValue[i]= ANOVA[[6]][2]
	
	RES$AIC[i]=AIC(fm)
	
	RES$BIC[i]=BIC(fm)
	
	## Now HA becomes H0
	fm0=fm
}

par(mfrow=c(2,2))
plot(AdjRSq~DF,col=2,type='o',data=RES);abline(v=RES$DF,col=8,lty=2)

plot(AIC~DF,col=2,type='o',data=RES);abline(v=RES$DF,col=8,lty=2)
plot(BIC~DF,col=2,type='o',data=RES);abline(v=RES$DF,col=8,lty=2)
plot(-log10(RES$pValue)~RES$DF,type='o');abline(h=-log10(.05),lty=2); abline(v=RES$DF,col=8,lty=2)

```


<div id="INCLASS_11" />

### INCLASS 11


**Reading the data and fitting the model**
```r
DATA=read.table('https://raw.githubusercontent.com/gdlc/STAT_COMP/master/DATA/goutData.txt',
                    header=TRUE)
   DATA$y=ifelse(DATA$gout=="Y",1,0)
   fm=glm(y~su,data=DATA,family='binomial')
   summary(fm) 
```

**CI using the methods we discussed previously**


```r
 su.grid=seq(from=4,to=10,by=.1)
 phat=predict(fm,type='response',newdata=data.frame(su=su.grid))
 
 LP=predict(fm,newdata=data.frame(su=su.grid),se.fit=TRUE)
 CI.LP=cbind('LB'=LP$fit-1.96*LP$se.fit,'UB'=LP$fit+1.96*LP$se.fit)
 
 CI.PROB=exp(CI.LP)/(1+exp(CI.LP))
 
 plot(phat~su.grid,col=2,xlab='Serum urate',ylab='P(Gout)',type='l',ylim=c(0,.5))
 lines(x=su.grid,y=CI.PROB[,'LB'],col='blue',lty=2)
 lines(x=su.grid,y=CI.PROB[,'UB'],col='blue',lty=2)

```

**Bootstrap**

```r
 PHAT=matrix(nrow=length(su.grid),ncol=5000,NA)
 for(i in 1:5000){
    rows=sample(1:nrow(DATA),size=nrow(DATA),replace=TRUE)
    tmpData=DATA[rows,]
    fm=glm(y~su,data=tmpData,family='binomial')
    phat.boostrap=predict(fm,type='response',newdata=data.frame(su=su.grid))
    PHAT[,i]=phat.boostrap
 }
 
 ## Quantiles
 LB=apply(FUN=quantile,X=PHAT,MARGIN=1,prob=0.0275)
 UB=apply(FUN=quantile,X=PHAT,MARGIN=1,prob=0.975)
 lines(x=su.grid,y=LB,col='red',lty=2)
 lines(x=su.grid,y=UB,col='red',lty=2)
 
```

**A demonstration of conceptual repeated sampling**

Here, what I do is to add a line for the predicted curve of each bootstrap sample...

```r

 plot(phat~su.grid,col=2,xlab='Serum urate',ylab='P(Gout)',type='l',ylim=c(0,.5))

 for(i in 1:500){
    rows=sample(1:nrow(DATA),size=nrow(DATA),replace=TRUE)
    tmpData=DATA[rows,]
    fm=glm(y~su,data=tmpData,family='binomial')
    phat.bootstrap=predict(fm,type='response',newdata=data.frame(su=su.grid))
    lines(x=su.grid,y=phat.bootstrap,lwd=.5,col='grey')
 }
 lines(x=su.grid,y=phat,col='red',lwd=2)
 lines(x=su.grid,y=LB,col='red',lty=2)
 lines(x=su.grid,y=UB,col='red',lty=2)
 
```


[back to list](#MENUE)


<div id="INCLASS_12" />

### INCLASS 12


##### 1)

```r
 pnorm(q=8,mean=10,sd=2)
 pnorm(q=11,mean=10,sd=2,lower.tail=FALSE)
 pnorm(q=11,mean=10,sd=2)-pnorm(q=8,mean=10,sd=2)
```


##### 2)

If X~N(10,VAR=4), then Z=(X-10)/2  ~N(0,1)

[Note you get the same results as in 1, think about why]

```r 
 pnorm(q= -1)
 pnorm(q=1/2,lower.tail=FALSE)
 pnorm(q=0.5)-pnorm(q= -1)
```


##### 3)

```r
 n=c(10,20,30)
 
 dbinom(x=3,size=n,prob=0.07)
 
 pbinom(q=3,size=n,prob=0.07,lower.tail=FALSE)
 
 pbinom(q=3,size=n,prob=0.07)
 
 
 pbinom(q=3,size=n,prob=0.07)+(1- pbinom(q=3,size=n,prob=0.07))
  
```

##### 4)

```r
 n=10000
 lambda=0.05*50
 X=rpois(lambda=lambda,n=n)
 Y=rbinom(prob=0.05,size=50,n=n)
 
 mean(X)
 mean(Y)
 var(X)
 var(Y)
 
 par(mfrow=c(1,2))
 hist(X,xlim=c(0,12))
 hist(Y,xlim=c(0,12)) 
```

##### 5)

```r
 zStat=0.83/0.045
 pnorm(-abs(zStat))*2
 
 pt(-abs(zStat),df=20)*2
 
```


##### 6)

```r
  DATA=read.table('https://raw.githubusercontent.com/gdlc/STAT_COMP/master/DATA/wages.txt',header=TRUE)
  str(DATA) # inspect the types of each variable! Do variables have the correct type?

  HA=lm(wage~education+sex+union+region+ethnicity,data=DATA)
  H0=lm(wage~education+sex+union+region,data=DATA)
```

**Likelihood ratio test**

```r
 LRT=-2*(logLik(H0)-logLik(HA))
 pchisq(LRT, df=length(coef(HA)-length(coef(H0))))
 pchisq(LRT, df=length(coef(HA))-length(coef(H0)),lower.tail=FALSE)
 
 anova(H0,HA)
 
```

##### 7)

P(Infection)=1-P(Not infected)

P(Not infected)= (1-3/100)*(1-3/99)*(1-3/98)*(1-3/97)


We can also obtain this probability from the hypergeometric distribution


```r
 P1=1-(1-3/100)*(1-3/99)*(1-3/98)*(1-3/97)
 P2=1-dhyper(m=3,n=97,k=4,x=0)
 P3=phyper(m=3,n=97,k=4,q=0,lower.tail=FALSE)
 c(P1,P2,P3)
 
```

[back to list](#MENUE)


<div id="INCLASS_13" />

### INCLASS 13

**Joint distribution**

```r
 PXY=rbind(c(.1,.1),c(.2,.6))
 colnames(PXY)=paste0('Y=',0:1)
 rownames(PXY)=paste0('X=',0:1)
```
**1) Composition Sampling**

Here, I first sample X from its marginal distribution, then Y from p(Y|X).

To do this, I need the marignal distribiton of X and the conditional distribution of Y given X.

```r
 # Marginal distributions 
  MX=rowSums(PXY)
  
 # Conditional Distribution of Y given X
  pYgX0=PXY[1,]/sum(PXY[1,])
  pYgX1=PXY[2,]/sum(PXY[2,])
  
  # these are the success probabilities for Y given X=0 and given X=1
  pYgX=c(pYgX0[2],pYgX1[2])
```

*Sampling*

```r
 n=100000 # sample size
 X=rep(NA,n)
 Y=rep(NA,n)

 for(i in 1:n){
   X[i]=rbinom(size=1,n=1,prob=MX[2])
   Y[i]=rbinom(size=1,n=1,prob=pYgX[X[i]+1])
 }
 table(X,Y)/n # compare with PXY

```

Now the other way around: sample Y from its marginal dist and X from p(X|Y).

```r
 # Marginal distributions 
  MY=colSums(PXY)
  
 # Conditional Distribution of X given Y
  pXgY0=PXY[,1]/sum(PXY[,1])
  pXgY1=PXY[,2]/sum(PXY[,2])
  
  # these are the success probabilities for Y given X=0 and given X=1
  pXgY=c(pXgY0[2],pXgY1[2])
```

*Sampling*

```r
 n=100000 # sample size
 X2=rep(NA,n)
 Y2=rep(NA,n)

 for(i in 1:n){
   Y2[i]=rbinom(size=1,n=1,prob=MY[2])
   X2[i]=rbinom(size=1,n=1,prob=pXgY[Y2[i]+1])
 }
 table(X2,Y2)/n # compare with PXY

```


**2) Gibbs Sampling**

In Gibbs sampling, we always sample from the fully conditional distributions. 

Here is an example

```r
 X3=rep(NA,n)
 Y3=rep(NA,n)
 
 # First we need to initialize one of the variables
 X3[1]=1 # can initalize to any value with non-zero marginal probability
 Y3[1]=0
 for(i in 2:n){
   # For X[i] we condition on Y[i-1]
   X3[i]=rbinom(size=1,n=1,prob=pXgY[Y3[i-1]+1])
   
   # For Y[i] we conditon on X[i]
   Y3[i]=rbinom(size=1,n=1,prob=pYgX[X3[i]+1])
}
 table(X3,Y3)/n
```

**Effective Sample Size**

 - Composition sampling generate IID samples
 - However, in Gibbs sampling there is autocorrelation between samples because we always conditon on the most recent update

```r
library(coda)
effectiveSize(X1)
effectiveSize(X2)
effectiveSize(X3)
```


<div id="INCLASS_15" />

### INCLASS 15

**Part 1**


```r

 N=100
 b=1
 
 pValues=rep(NA,5000)
 
 for(i in 1:5000){
   
  signal_to_noise_ratio=0.1
  x=rnorm(N)
  signal=100+x*b 
  vE=var(signal)/signal_to_noise_ratio
  error=rnorm(sd=sqrt(vE),n=N) 
  y=signal+error
  fm=lm(y~x)
  pValues[i]=summary(fm)$coef[2,4]
 }

```

**Part 2**

```r

 SCEN=expand.grid(n=c(10,30,50,100,200),SNR=c(.05,.1,.15),b=1,power=NA) # SNR=signal_to_noise_ratio
 for(h in 1:nrow(SCEN)){
   
   N=SCEN$n[h]
   signal_to_noise_ratio=SCEN$SNR[h]
 
    pValues=rep(NA,5000)
 
    for(i in 1:5000){
      x=rnorm(N)
      signal=100+x*b 
      vE=var(signal)/signal_to_noise_ratio
      error=rnorm(sd=sqrt(vE),n=N) 
      y=signal+error
      fm=lm(y~x)
      pValues[i]=summary(fm)$coef[2,4]
    }    
    SCEN$power[h]=mean(pValues<0.05)
 }
```

[back to list](#MENUE)



<div id="INCLASS_16" />

### INCLASS 16



```r
set.seed(1950)
 X=matrix(nrow=1000,ncol=3,rbinom(size=2,n=3000,prob=0.2))
 b=c(1,0,1)
 signal=scale(X%*%b)*sqrt(0.1)
 error=rnorm(nrow(X),sd=sqrt(0.8))
 y=signal+error
 fm0=lm(y~X)

 pVals=rep(NA,10000)
 n=nrow(X)
 for(i in 1:length(pVals)){
   z=sample(y,size=n,replace=FALSE)
   fm=lm(z~X)
   pVals[i]=min(summary(fm)$coef[-1,4])
   
 }
 quantile(pVals,prob=0.05)
 
 # compare with Bonferroni's method (a method we will discuss later on)
 0.05/3 #3 here is the number of tests
```
[back to list](#MENUE)



<div id="INCLASS_17" />

### INCLASS 17

```r

DATA=read.table('https://raw.githubusercontent.com/gdlc/STAT_COMP/master/DATA/wages.txt',header=TRUE)

M1="wage~sex+education+experience"
M2="wage~."

fm1=lm(M1,data=DATA)
fm2=lm(M2,data=DATA)
       
PVE=matrix(nrow=100,ncol=2,NA)

nTst=100
n=nrow(DATA)

for(i in 1:100){
  tst=sample(1:n,size=nTst)
  
  tmp1=lm(M1,data=DATA[-tst,])
  tmp2=lm(M2,data=DATA[-tst,])
  
  yHat1=predict(tmp1,newdata=DATA[tst,])
  yHat2=predict(tmp2,newdata=DATA[tst,])
  y=DATA[tst,]$wage
  PVE[i,2]=1-sum((y-yHat2)^2)/sum((y-mean(y))^2)
  PVE[i,1]=1-sum((y-yHat1)^2)/sum((y-mean(y))^2)
}
```

[back to list](#MENUE)



<div id="INCLASS_18" />

### INCLASS 18

**Simulation**

```r
 pH0=0.95
 nTests=5000
 n=1000 # sample size
 pVals=rep(NA,nTests)
 isHA=runif(nTests)>pH0
 varB=.03 #  variance explained if Ha holds
 
 for(i in 1:nTests){
   x=rnorm(n)
   y=rnorm(n)
   if(isHA[i]){
     y=y+x*rnorm(1,sd=sqrt(varB)) # adding an effect if Ha
   }
   pVals[i]=summary(lm(y~x))$coef[2,4]
 }
 ```

**Adjusting p-values**

```r
 pADJ.Bonf=p.adjust(pVals,method='bonferroni')
 pADJ.Holm=p.adjust(pVals,method='holm')
 pADJ.FDR=p.adjust(pVals,method='fdr')
```

**Decision rules**

```r
 rejBonf=pADJ.Bonf<.05
 rejHolm=pADJ.Holm<.05
 rejFDR=pADJ.FDR<.05
```


**Determination of False Discoevery Proportion and the proportion of Ha included in the discovery set (PWR)**

Note: Type-I error rate is the E[FDP] and power is E[PWR], these are results from just one MC replicate.

```r
FDP=c('Bonf'=sum(rejBonf&(!isHA))/sum(rejBonf),'Holm'=sum(rejHolm&(!isHA))/sum(rejHolm),FDR=sum(rejFDR&(!isHA))/sum(rejFDR))
PWR=c('Bonf'=sum(rejBonf&isHA)/sum(isHA),'Holm'=sum(rejHolm&isHA)/sum(isHA),'FDR'=sum(rejFDR&isHA)/sum(isHA))
rbind(FDP,PWR)
```

[back to list](#MENUE)



<div id="INCLASS_19" />

### INCLASS 19

```r
 DATA=read.table('https://hastie.su.domains/ElemStatLearn/datasets/prostate.data',header=TRUE)
 head(DATA)
 train=DATA[,'train']
 DATA=DATA[,-ncol(DATA)]
 
 TRN.DATA=DATA[train,]
 TST.DATA=DATA[!train,]
 
 
 ## Forward regression
  fm0=lm(lpsa~1,data=TRN.DATA)
  fullModel='lpsa ~ lcavol+lweight+age+lbph+svi+lcp+gleason+pgg45'
  fwd=step(fm0,scope=fullModel,direction='forward',data=TRN.DATA)
 
  path=names(coef(fwd)[-1])
  FWD=data.frame(path=path,df=1:length(path),sqCorTST=NA)
 
 
  for(i in 1:nrow(FWD)){
 	y=TRN.DATA[,'lpsa']
	X=as.matrix(TRN.DATA[,path[1:i]])
 	fm=lm(y~X)
	yHatTST=as.matrix(TST.DATA[,path[1:i],drop=FALSE])%*%coef(fm)[-1]+coef(fm)[1]
	yTST=TST.DATA[,'lpsa']
	FWD$sqCorTST[i]=cor(yTST,yHatTST)^2
  }
 plot(FWD$sqCor,type='o');abline(v=which.max(FWD$sqCor));abline(h=max(FWD$sqCor,na.rm=TRUE))

 
 library(glmnet)
 y=TRN.DATA[,'lpsa']
 X=as.matrix(TRN.DATA[,colnames(DATA)!='lpsa'])
 fmL=glmnet(y=y,x=X,alpha=1)
 LASSO=data.frame(lambda=fmL$lambda,df=fmL$df,sqCorTST=NA)
 yTST=TST.DATA[,'lpsa']
 X.TST=as.matrix(TST.DATA[,colnames(DATA)!='lpsa'])
 for(i in 2:nrow(LASSO)){
   yHat=X.TST%*%as.numeric(fmL$beta[,i])
   LASSO$sqCorTST[i]=cor(yTST,yHat)^2
 }
  
 plot(LASSO$sqCor,type='o');abline(v=which.max(LASSO$sqCor));abline(h=max(LASSO$sqCor,na.rm=TRUE))
 
 max(LASSO$sqCorTST,na.rm=TRUE)
 max(FWD$sqCorTST,na.rm=TRUE)

```

[back to list](#MENUE)
