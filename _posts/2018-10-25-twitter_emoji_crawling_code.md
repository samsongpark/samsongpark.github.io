---
layout: post
title:  "Emoji를 추천해주는 기능은 없을까?"
date:   2018-10-25 15:01:29
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
setwd("/Users/yejinpark/Documents/BOAZ/twitter_crawling_output/181111")

getwd()

# 패키지를 install한다
install.packages(c("twitteR", "ROAuth", "base64enc"))

# 라이브러리를 불러온다
library("twitteR")
library("ROAuth")
library("base64enc")
```
 
>트위터에서 크롤링을 하기 위해서는 트위터 앱 사이트(http://apps.twitter.com)에서 Key값을 발급받아야 한다.

Step 1. 트위터에 로그인 후, http://apps.twitter.com에서 "Create new app"을 클릭한다.

Step 2. Name, Description, Website 입력 후 동의 (Yes, I have~)부분을 체크한 후,
"Create your Twitter application"을 선택한다.
* Name : Application 이름 (32글자 이내로 적어야 함)
* Description : Application에 대한 설명 입력 (10~200 글자 이내)
* Website : 자신의 블로그 URL 주소나 다른 웹사이트의 URL 주소를 입력한다.

Step 3. Keys and Access Tokens 탭에서 Consumer Key(API Key), Consumer Secret(API Secret)을 확인한다.
그리고 "Create my access token" 을 선택한다.

Step 4. 발급된 Access token과 Access token secret을 확인한다.


이렇게 해서 트위터 dev 전용 앱 사이트에서 크롤링을 위한 Key와 Access 토큰을 발급받았다! Yay:D


이제 트위터 계정에서 발급받은 키 값을 입력한다.
```
> consumerKey <-    "5AHA6WnwL5CmIw4Hg8cwPPoAx"
> consumerSecret <- "b85xJBKZ7Kmc7bUZJCxXL3yMxCiLgxbh5uZtbsZpTZNjnGE3Ky"

> accessToken <-    "1055056704302284800-ZlKcFBGOq6JDyosmNBJvQIWlcWYioA"

> accessTokenSecret <- "SGHvNhv6MBJPTuoxdEndLwl0BwgXRBw4nIkPctGFodAVT"
```


그리고 setup_twitter_oauth 함수를 이용하여 oauth 인증 파일을 저장한다.
```
setup_twitter_oauth(consumerKey, consumerSecret, accessToken, accessTokenSecret)

그 다음, 내가 고른 이모지인 'pray'를 키워드로 저장한다
> keyword <-'\U0001F64F'

**그리고 크롤링할 트위터 수(n=500)와 언어(lang="ko") 그리고 크롤링할 기간을 설정한다.**
> bigdata <- searchTwitter(keyword,n=50000,since = '2018-11-01', until = '2018-11-01', lang="ko")
bigdata
```
번거롭지만 하루에 50,000개씩 뽑아내기로 했다.

마지막으로 크롤링한 csv파일 저장 잊지 말고 해주기!
```
> df <- do.call("rbind", lapply(bigdata, as.data.frame))
write.csv(df,"1F64F.csv")
```

