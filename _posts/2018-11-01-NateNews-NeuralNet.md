---
layout: post
title:  "Nate crawling: 내맘대로 작성해 본 Neural Net in R"
date:   2018-11-01 16:22:25
categories: Data_science
permalink:

---

**R로 뉴럴넷을 만들어볼까?**
===================

나는 아직 파이썬을 잘하지 못하는 Python toddler이다. 그렇다고 R을 잘하는 편은 아니지만 그나마 편하게 쓸 수 있는 언어가 R이다.

```
setwd("/Users/yejinpark/Documents")

normal <- read.csv("일반기사(cp949)-낚시.csv", encoding="cp949")
hoax <- read.csv("낚시성기사(cp949)-낚시.csv", encoding="cp949")

data <- rbind(hoax, normal); data$hoax <- as.factor(data$hoax)

test.index <- sample(nrow(data), 0.2*nrow(data)) 
test.data <- data[test.index,]; train.data <- data[-test.index,]

library(nnet)
nnet <- nnet( hoax ~ feature1 + feature2.1 + feature2.2 + feature2.3 + feature3 + feature4 + feature5, data = train.data, size=10 )
tune.nnet <- tune.nnet( hoax ~ feature1 + feature2.1 + feature2.2 + feature2.3 + feature3 + feature4 + feature5, data = train.data,
                        size=10, decay= )
summary(tune.nnet)

predicted.prob <- predict( nnet, newdata=test.data )
predicted <- ifelse(predicted.prob > 0.5, 1, 0)

( accuracy <- sum( ifelse(test.data$hoax == predicted, 1, 0) ) / nrow(test.data) )
( recall <- sum( test.data$hoax==1 & predicted==1 ) / sum( test.data$hoax==1 ) )
( misclassification.table <- table(test.data$hoax, predicted) )
```



