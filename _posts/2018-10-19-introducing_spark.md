---
layout: post
title:  "Spark toddler가 되어볼까?"
date:   2018-10-19 10:01:09
categories: Data_science
permalink:

---

**Spark toddler가 되어볼까?**
===================

최근에 Spark의 기초에 대하여 공부하고 있다. 과연 내가 Spark를 활용하는 분야로 갈 수 있을지는 아직 잘 모르겠지만, 그래도 지금 흥미를 가지고 있는 분야이므로 계속해서 공부하고자 한다. 

공부 내용을 정리해보았다.


Spark 시작하기
===================

왜 Spark인가?
-------------

빅데이터는 그 이름에서 알 수 있듯이, 과거와는 다르게 규모가 크다는 특징이 있다. 과거에는 메가바이트 규모라고 한다면, 그 몇 천배 이상으로 크다.

또한 발생하는 출처가 다양하다. 종전에는 '숫자'로 된 재무제표, 시계열 데이터 등 정량적 데이터 또는 'Hard Data'가 많았다면, 빅데이터의 경우는 다르다. SNS에 사람들이 남긴 글, 센서에서 발생하는 데이터, 사람들의 핸드폰에서 발생하는 다양한 디지털 지문 등 다양한 형태로 발생한다. 
발생하는 출처가 다양하므로, 그 데이터 형식이 비구조적이다. 소문이나 평판과 같은 'Soft Data'도 분석하는 틀을 정하기 어렵다.

따라서 한 대의 컴퓨터가 가지고 있는 저장용량으로는 감당할 수 없기 때문에 데이터가 분산될 수 밖에 없다.

그러면 당연히 데이터의 처리도 나누어서 수행되어야 한다. 따라서 이러한 분산작업을 제어하는 기능이 필요하게 된다. 대량의 데이터를 여러 컴퓨터에 나누어 처리하고, 집계하는 맵리듀스 MapReduce와 같은 알고리즘이 많이 사용 되고 있다고 한다.

이러한 데이터웨어하우스를 구성하기 위해 ETL (Extract, Transform, and Load) 작업이 필요하다.

단계     | 설명
-------- | ---
Extract | 다양한 입력에서 데이터를 추출 (HDFS, files, JSON 등등..)
Transform    | 데이터의 변환
Load     | 다양한 형식의 저장 그리고 예측, 분류, 추천모델 등을 지원


Hadoop과 Spark에 대해 더 알아보자
-------------

데이터가 대량으로 증가하면서, 이를 처리하기 위한 분산 프레임워크 Hadoop이 개발되어 쓰이고 있다.

최근에는 Spark의 사용이 늘어나고 있어서 Hadoop과 비교되고 있다.

Hadoop은 데이터를 수집하는 목적으로 주로 사용된다.

Hadoop 자체의 파일시스템 HDFS (Hadoop Distributed File System)와
맵리듀스map-reduce 작업으로 데이터를 추출할 수 있다.

반면 Spark는 수집한 데이터를 분석하는 용도로 사용된다.

자체 파일 시스템이 없고, RDD를 통해 Hadoop의 파일시스템 HDFS를 사용할 수 있다.

Spark는 Machine Learning 라이브러리를 가지고 있다.

Hadoop과 달리 메모리에서 처리하기 때문에 빠르다 (pipeline). 빅데이터를 빠르게 map-reduce 할 수 있다.

SQL을 안 쓰는 회사는 없으며, 꼭 알아야 하는 언어라고 한다. 그리고 나같은 비전공자는 NoSQL과 json을 알아두면 좋을 것이라고 한다.

구분   | 구성  |  설명
-------- | --- | ---
Spark engine | Spark Core | 작업 배분, 입출력 등 분산작업에 필요한 기능
Spark Application Frameworks   | Spark SQL | Dataframes
| Spark Streaming | 다양한 형식의 저장 그리고 예측, 분류, 추천모델 등을 지원



**머신러닝 = 기계학습 = AI**


지금은 Databricks의 클라우드를 써 볼 예정!
나는 나중에 따로 Spark를 설치해야겠다.


* Databricks 커뮤니티의 페이지를 들어가서 활용해보자!(https://community.cloud.databricks.com)