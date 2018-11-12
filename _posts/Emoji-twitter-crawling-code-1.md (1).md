---
layout: post
title:  "Emoji를 추천해주는 기능은 없을까?"
date:   2018-10-09 15:01:29
categories: Data_science
permalink: pretty
---


# **앞 문맥을 바탕으로 이모지를 예측해보자!**

1) 처음에는 제설함 배치 프로젝트를 하려고 했다..! (따로 md파일 만들기)
2) 이전에는 네이트뉴스 크롤링을 하였다 (따로 md 파일 만들기-'네이트 크롤링 시리즈')
3) 빅분실 수업 때는 또 다른 걸 한다 (따로 md 파일 만들기-series로 만들기-'트롤링 시리즈')
4) Emoji 논문에 대한 설명과 이모지별 코드 (따로 md 파일 만들기)

그러다가 어느 한 논문을 보게 되었고,
평소 텍스트마이닝에 관심을 가지고 있어 이 논문 구현을 실제로 해보기로 했다.
윈도우에서 하기 번거로운 부분을 맥에서 나름 편하게(?) 활용할 수 있다


```
> setwd("/Users/yejinpark/Documents/BOAZ/twitter_crawling_output/181111")

getwd()
install.packages(c("twitteR", "ROAuth", "base64enc"))

library("twitteR")
library("ROAuth")
library("base64enc")
```

```
> consumerKey <-    "5AHA6WnwL5CmIw4Hg8cwPPoAx"
> consumerSecret <- "b85xJBKZ7Kmc7bUZJCxXL3yMxCiLgxbh5uZtbsZpTZNjnGE3Ky"

> accessToken <-    "1055056704302284800-ZlKcFBGOq6JDyosmNBJvQIWlcWYioA"

> accessTokenSecret <- "SGHvNhv6MBJPTuoxdEndLwl0BwgXRBw4nIkPctGFodAVT"
```

```
setup_twitter_oauth(consumerKey, consumerSecret, accessToken, accessTokenSecret)

> keyword <-'\U0001F64F'
> bigdata <- searchTwitter(keyword,n=50000,since = '2018-11-01', until = '2018-11-01', lang="ko")
bigdata
```

```
> df <- do.call("rbind", lapply(bigdata, as.data.frame))
write.csv(df,"1F64F.csv")
```

> Written with [StackEdit](https://stackedit.io/).