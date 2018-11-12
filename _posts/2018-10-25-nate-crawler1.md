---
layout: post
title:  "네이트 뉴스 크롤러를 작성해보자"
date:   2018-10-25 16:22:25
categories: Data_science
permalink:

---

**네이트뉴스 크롤러를 만들어보자**
===================

강의 시간에 윤호 교수님께서 앞으로는 R보다 Python이 더욱 폭넓게 활용될거라고 말씀해주셨다. 수업 이후로 Python과 얼른 친해지고 싶었다.

```
import requests
from bs4 import BeautifulSoup
import xlwt
import xlrd
import datetime
import pandas
```
```
dt_index = pandas.date_range(start='20160101', end='20161231')
date1 = dt_index.strftime("%Y%m%d").tolist()
```
```
number1 = list(range(0,38000))
```



###**첫번째 크롤링 방식, 날짜로 크롤링해보기**

```
def newscrawling1(end_date):
    wbk = xlwt.Workbook()
    sheet = wbk.add_sheet('네이트뉴스')
    sheet.write(0,0,'date')
    sheet.write(0,1,'cateogory')
    sheet.write(0,2,'title')
    sheet.write(0,3,'contents')
    sheet.write(0,4,'best_comment')
    sheet.write(0,5,'none_best_comment')
    i=-1
    sheet_write = 0
    for date in date1:
        if date > str(end_date):
            break
        else:
            i=i+1
            for number in number1:
                number = number+1 
                url = 'http://news.nate.com/view/'+str(date1[i])+'n'+str(number).zfill(5)
                source_code = requests.get(url)
                plain_text = source_code.text
                soup = BeautifulSoup(plain_text, 'html.parser')
                url2='http://comm.news.nate.com/Comment/ArticleComment/List?artc_sq='+str(date1[i])+'n'+str(number).zfill(5)
                source_code2 = requests.get(url2)
                plain_text2 = source_code2.text
                soup2 = BeautifulSoup(plain_text2, 'html.parser')
                print(url)
                try:
                    for comment_yesno in soup2.find_all('div',{'class':'replytab_wrap'}):
                        comment_yesno.text
                        print(comment_yesno.text)
                        if '댓글' in comment_yesno.text:
                            sheet_write = sheet_write+1
                            if '네이트 연예' in soup.text:
                                sheet.write(sheet_write,1,'연예')
                                sheet.write(sheet_write,0,soup.select('span.firstDate > em')[0].text)
                            elif '네이트 스포츠' in soup.text:
                                sheet.write(sheet_write,1,'스포츠')
                                sheet.write(sheet_write,0,soup.select('dd > em')[0].text)
                            elif '네이트 뉴스' in soup.text:
                                sheet.write(sheet_write,1,soup.select('div > h3 > a')[0].text)
                                sheet.write(sheet_write,0,soup.select('span.firstDate > em')[0].text)
                            else:
                                    pass
                            for best_comment in soup2.find_all('div',{'class':'cmt_best'}):
                                best_comment.text
                                sheet.write(sheet_write,4,best_comment.text)
                            else:
                                pass
                            for nonbest_comment in soup2.find_all('div',{'class':'cmt_list'}):
                                nonbest_comment.text
                                sheet.write(sheet_write,5,nonbest_comment.text)
                            else:
                                pass
                            for title in soup.find_all('h3',{'class':'articleSubecjt'}):
                                sheet.write(sheet_write,2,title.text)
                            else:
                                for title in soup.find_all('h3',{'class':'viewTite'}):
                                    sheet.write(sheet_write,2,title.text)
                                else:
                                    pass
                            for body in soup.find_all('div',{'id':'realArtcContents'}):
                                body.text
                                sheet.write(sheet_write,3,body.text)
                            else:
                                for body in soup.find_all('div',{'id':'articleContetns'}):
                                    body.text
                                    sheet.write(sheet_write,3,body.text)
                                else:
                                    pass
                        else:
                            pass
                except:
                    pass
                wbk.save('네이트뉴스0.csv')
```
```
newscrawling1(20180309)
```


###**두번째 크롤링 방식, csv 행 개수가 조절되도록 크롤링하기**

csv파일을 뽑을 때 '가0, 가1, 가2, 나1 ...' 으로 제목이 붙여지도록 만들어보았다.
```
def newscrawling2(end_row):
    s=0
    wbk = xlwt.Workbook()
    sheet = wbk.add_sheet('네이트뉴스{}'.format(s),cell_overwrite_ok=True)
    sheet.write(0,0,'date')
    sheet.write(0,1,'cateogory')
    sheet.write(0,2,'title')
    sheet.write(0,3,'contents')
    sheet.write(0,4,'best_comment')
    sheet.write(0,5,'none_best_comment')
    wbk.save('네이트뉴스다{}.csv'.format(s))
    i=-1
    sheet_write = 0
    for date in date1:
        workbook = xlrd.open_workbook('네이트뉴스다{}.csv'.format(s))
        worksheet_name = workbook.sheet_by_index(0)
        num_rows = worksheet_name.nrows
        if num_rows > end_row:
            s=s+1
            sheet_write=0
            i=i+1
            wbk = xlwt.Workbook()
            sheet = wbk.add_sheet('네이트뉴스{}'.format(s),cell_overwrite_ok=True)
            sheet.write(0,0,'date')
            sheet.write(0,1,'cateogory')
            sheet.write(0,2,'title')
            sheet.write(0,3,'contents')
            sheet.write(0,4,'best_comment')
            sheet.write(0,5,'none_best_comment')
            for number in number1: #0부터 45000
                number = number+1 
                url = 'http://news.nate.com/view/'+str(date1[i])+'n'+str(number).zfill(5)
                source_code = requests.get(url)
                plain_text = source_code.text
                soup = BeautifulSoup(plain_text, 'html.parser')
                url2='http://comm.news.nate.com/Comment/ArticleComment/List?artc_sq='+str(date1[i])+'n'+str(number).zfill(5)
                source_code2 = requests.get(url2)
                plain_text2 = source_code2.text
                soup2 = BeautifulSoup(plain_text2, 'html.parser')
                print(url)
                try:
                    for comment_yesno in soup2.find_all('div',{'class':'replytab_wrap'}):
                        comment_yesno.text
                        print(comment_yesno.text)
                        if '댓글' in comment_yesno.text:
                            sheet_write = sheet_write+1
                            if '네이트 연예' in soup.text:
                                sheet.write(sheet_write,1,'연예')
                                sheet.write(sheet_write,0,soup.select('span.firstDate > em')[0].text)
                            elif '네이트 스포츠' in soup.text:
                                sheet.write(sheet_write,1,'스포츠')
                                sheet.write(sheet_write,0,soup.select('dd > em')[0].text)
                            elif '네이트 뉴스' in soup.text:
                                sheet.write(sheet_write,1,soup.select('div > h3 > a')[0].text)
                                sheet.write(sheet_write,0,soup.select('span.firstDate > em')[0].text)
                            else:
                                pass
                            for best_comment in soup2.find_all('div',{'class':'cmt_best'}):
                                best_comment.text
                                sheet.write(sheet_write,4,best_comment.text)
                            else:
                                pass
                            for nonbest_comment in soup2.find_all('div',{'class':'cmt_list'}):
                                nonbest_comment.text
                                sheet.write(sheet_write,5,nonbest_comment.text)
                            else:
                                pass
                            for title in soup.find_all('h3',{'class':'articleSubecjt'}):
                                sheet.write(sheet_write,2,title.text)
                            else:
                                for title in soup.find_all('h3',{'class':'viewTite'}):
                                    sheet.write(sheet_write,2,title.text)
                                else:
                                    pass
                            for body in soup.find_all('div',{'id':'realArtcContents'}):
                                body.text
                                sheet.write(sheet_write,3,body.text)
                            else:
                                for body in soup.find_all('div',{'id':'articleContetns'}):
                                    body.text
                                    sheet.write(sheet_write,3,body.text)
                                else:
                                    pass
                        else:
                            pass
                except:
                    pass
                wbk.save('네이트뉴스다{}.csv'.format(s))
        else:
            i=i+1
            for number in number1:
                number = number+1 
                url = 'http://news.nate.com/view/'+str(date1[i])+'n'+str(number).zfill(5)
                source_code = requests.get(url)
                plain_text = source_code.text
                soup = BeautifulSoup(plain_text, 'html.parser')
                url2='http://comm.news.nate.com/Comment/ArticleComment/List?artc_sq='+str(date1[i])+'n'+str(number).zfill(5)
                source_code2 = requests.get(url2)
                plain_text2 = source_code2.text
                soup2 = BeautifulSoup(plain_text2, 'html.parser')
                print(url)
                try:
                    for comment_yesno in soup2.find_all('div',{'class':'replytab_wrap'}):
                        comment_yesno.text
                        print(comment_yesno.text)
                        if '댓글' in comment_yesno.text:
                            sheet_write = sheet_write+1
                            if '네이트 연예' in soup.text:
                                sheet.write(sheet_write,1,'연예')
                                sheet.write(sheet_write,0,soup.select('span.firstDate > em')[0].text)
                            elif '네이트 스포츠' in soup.text:
                                sheet.write(sheet_write,1,'스포츠')
                                sheet.write(sheet_write,0,soup.select('dd > em')[0].text)
                            elif '네이트 뉴스' in soup.text:
                                sheet.write(sheet_write,1,soup.select('div > h3 > a')[0].text)
                                sheet.write(sheet_write,0,soup.select('span.firstDate > em')[0].text)
                            else:
                                    pass
                            for best_comment in soup2.find_all('div',{'class':'cmt_best'}):
                                best_comment.text
                                sheet.write(sheet_write,4,best_comment.text)
                            else:
                                pass
                            for nonbest_comment in soup2.find_all('div',{'class':'cmt_list'}):
                                nonbest_comment.text
                                sheet.write(sheet_write,5,nonbest_comment.text)
                            else:
                                pass
                            for title in soup.find_all('h3',{'class':'articleSubecjt'}):
                                sheet.write(sheet_write,2,title.text)
                            else:
                                for title in soup.find_all('h3',{'class':'viewTite'}):
                                    sheet.write(sheet_write,2,title.text)
                                else:
                                    pass
                            for body in soup.find_all('div',{'id':'realArtcContents'}):
                                body.text
                                sheet.write(sheet_write,3,body.text)
                            else:
                                for body in soup.find_all('div',{'id':'articleContetns'}):
                                    body.text
                                    sheet.write(sheet_write,3,body.text)
                                else:
                                    pass
                        else:
                            pass
                except:
                    pass
                wbk.save('네이트뉴스다{}.csv'.format(s))
```
```
newscrawling2(20000)
```

지금까지 뽑힌 csv 파일들을 보니까 아직은 내 코드가 고장이 나지 않았나부다 ^3^
