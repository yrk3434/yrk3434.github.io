---  
title: Crawling
category: Programming  
order: 3  
---  

## Reference
- 인프런 강의: 현존 최강 크롤링 기술: Scrapy와 Selenium 정복<br/>
-> 저작권 침해 방지를 위해 독자적 예제 사용

## 개요

데이터 분석에서 종종 외부 정보가 필요하다. 예컨대, 날씨 데이터, 공공기관 데이터, 뉴스 데이터 등이 빈번하게 외부정보로 활용한다. 이러한 정보를 이용하려면 크롤링이 필요하다.

웹크롤링란 웹 사이트로부터 원하는 정보를 추출하는 행위다. 파이썬에서 beautifulsoup을 통해 크롤링을 할 수도 있지만, 동적 크롤링, 검색을 통한 웹페이지 크롤링 등을 하는 데에 한계가 있어 selenium을 이용해 크롤링하고자 한다.


# Contents
1. [셀레니움을 이용한 크롤링](#1-셀레니움을-이용한-크롤링) <br/>
2. 동적크롤링 <br/>
3. xpath <br/>
4. [Scrapy를 이용한 크롤링](#4-scrapy를-이용한-크롤링) <br/>

---

# 1. 셀레니움을 이용한 크롤링

## 1.1 크롬 드라이버 설치
[드라이버 설치 사이트 클릭](https://chromedriver.chromium.org/downloads)
크롬 버전 확인 후 ChromeDriver exe 설치, 단 설치 경로를 기억할 것

---

## 1.2. 크롬 드라이버 실행 & 크롤링 기본 구조
[네이버 부동산 뉴스 예제](https://land.naver.com/news/newsRead.nhn?type=headline&prsco_id=119&arti_id=0002494744)의 기사 제목 크롤링

### 1.2.1. 드라이버 실행

```
chromedriver = '.../chromedriver' # 드라이버 위치
driver = webdriver.Chrome(chromedriver) #
```

### 1.2.2. 사이트 & 크롤링 대상 입력

```
driver.get('https://land.naver.com/news/newsRead.nhn?type=headline&prsco_id=119&arti_id=0002494744') 
result = driver.find_element_by_tag_name('h3')
```

- driver.get에는 크롤링할 사이트를 입력한다.
- driver.find_element_by_tag_name에는 크롤링할 웹 요소를 입력한다.  element 부분을 카피하면 다음과 같다. element를 카피하는 방법은 목차 3에 기재했다.

```
<h3 tabindex="0">시범운영에도 부작용 '속속'…전월세신고제, 임차인 보호 의구심 여전</h3>
```

element가 h3이므로 driver.find_element_by_tag_name에 h3을 넣는다.

#### element vs elements

- driver.find_element_by_tag_name
- driver.find_elements_by_tag_name

단, 이 예제는 가장 첫번째 h3 요소이기 때문에 문제가 없지만, 두 번째 혹은 그보다 더 뒤의 h3 요소면 문제가 된다.
이 기사에는 4개의 h3 요소가 다음과 같이 있다. 

> 1. 시범운영에도 부작용 '속속'…전월세신고제, 임차인 보호 의구심 여전
> 2. 데일리안 관련뉴스해당 언론사에서 선정하며 언론사 페이지(아웃링크)로 이동해 볼 수 있습니다.
> 3. 이슈 투기근절 대책
> 4. 우리동네뉴스

위와 같은 경우 element가 아닌 elements를 사용하여 해당 순서의 값을 추출한다. 예컨대, '이슈 투기근절 대책'을 추출하고 싶다면 다음과 같이 코드를 짜야 한다.

```
result = driver.find_elements_by_tag_name('h3')
print(result[2].text)
```

### 전체 코드

```
from selenium import webdriver
from selenium.webdriver.common.keys import Keys
import time

chromedriver = '.../chromedriver' # 드라이버 위치
driver = webdriver.Chrome(chromedriver) # 실행시 크롬 브라우저 뜸 -> driver.get에 크롤링하려는 사이트 넣고 실행
driver.get('https://land.naver.com/news/newsRead.nhn?type=headline&prsco_id=119&arti_id=0002494744')  # 해당 사이트로 이동
print(driver.title) # 웹 페이지 타이틀
result = driver.find_element_by_tag_name('h3')
print(result.text)

driver.quit() # 크롬 브라우저 닫기

##########  실행 결과   ##########
# driver.title
# 뉴스 : 네이버 부동산
# result.text
# 시범운영에도 부작용 '속속'…전월세신고제, 임차인 보호 의구심 여전
```

---

## 1.3. 크롤링 대상 찾기

- Step1. Ctrl + Shift + C 또는
more tools > develeopers tool > 마우스 커서 모양(Select an element in the page to inspect it) 클릭
- Step2. 크롤링할 요소 클릭
- Step3. 오른쪽 develeopers tool에서 선택된 요소(하늘색) 우클릭 > copy > copy element 
- Step4. Ctrl + V

네 가지 step에 따라 크롤링 요소를 복사하면 다음과 같다.

```
<h3 tabindex="0">시범운영에도 부작용 '속속'…전월세신고제, 임차인 보호 의구심 여전</h3>
```

---

## 1.4. 검색을 통한 크롤링 예제

- 네이버에서 날씨 검색을 통해 현재 온도 크롤링하기

[네이버 사이트](https://www.naver.com/)의 검색창의 요소를 복사하면 다음과 같다. 검색창의 id 이름이 'query'이다. 따라서 driver.find_element_by_id 함수를 통해 검색창을 명명한다.

```
<input id="query" name="query" type="text" title="검색어 입력" maxlength="255" class="input_text" tabindex="1" accesskey="s" style="ime-mode:active;" autocomplete="off" placeholder="검색어를 입력해 주세요." onclick="document.getElementById('fbm').value=1;" value="" data-atcmp-element="">
```

검색창에 '날씨'를 검색했을 때 온도를 나타내는 요소를 복사하면 다음과 같다. class 이름이 'todyatemp'다. driver.find_elements_by_class_name를 통해 온도 정보를 저장한다.

```
<span class="todaytemp">18</span>
```

time.sleep을 통해 검색 시간을 늦추는 이유는 타이핑이 채 되기도 전에 엔터를 치면 blank 값을 검색하기 때문이다.


### 전체 코드

```
from selenium import webdriver
from selenium.webdriver.common.keys import Keys
import time #검색해서 크롤링하는 상황이면, 검색 소요 시간 고려해서 time 라이브러리도 불러오자

chromedriver = '.../chromedriver'
driver = webdriver.Chrome(chromedriver)
driver.get("https://www.naver.com/")

elem = driver.find_element_by_id("query") # 검색창 
elem.clear() # 검색창 clear 
elem.send_keys('날씨') # 검색창에 입력할 값
time.sleep(3) # 타이핑 시간 3초 기다려줌
elem.send_keys(Keys.ENTER) # 엔터

# 날씨 검색 후 페이지에서 온도 요소 찾음
value=driver.find_element_by_class_name('todaytemp')
print(value.text) # 오늘의 온도

driver.quit() # 크롬 브라우저 닫기
```

---

# 4. Scrapy를 이용한 크롤링

## 4.1. 객체지향 프로그래밍

- 절차지향 프로그래밍 vs 객체지향 프로그래밍([참고](https://brownbears.tistory.com/407#recentEntries))
    - 두 개념이 반대는 아님
    - 절차지향 프로그래밍은 데이터를 중심으로, 객체지향 프로그래밍은 기능을 중심으로 함수 구현

- 객체지향 프로그래밍
    - class: 설계도를 만듦
    - object: 설계도 기반으로 사물1객체를 만듦
    - call: 사물1 객체의 기능을 호출
        - attribute(속성): 객체의 변수
        - method(동작): 객체의 함수

```
class Quad: # 설계도
    # attribute 선언
    height = 0
    width = 0
    color = ''

    # method 선언
    def get_area(self):
        return self.height * self.width

# 객체
quad1 = Quad()
# 객체 속성 바꾸기 
quad1.height = 4
quad1.width = 3
quad1.color = 'red'

# 메소드 호출
quad1.get_area()

# 속성 확인
dir(Quad) 
```

## 4.2. Scrapy

-  scrapy 설치

```
# 아나콘다 프롬프트 or 터미널
pip install scrapy
```

- 프로젝트 생성
터미널에서 원하는 디렉토리를 current directory로 만든 후 다음을 실행

```
scrapy startporject ecommerce # scrapy startproject 프로젝트이름

# scarpy -h # help, scrapy 명령어 설명

mkdir ecommerce
cd ecommerce # 위에서 생성한 프로젝트 디렉토리로 이동
```

미리 작성된 크롤링 템플릿 확인 가능

```
scrapy.cfg

# ---- scarpy.cfg 내용 -----
# Automatically created by: scrapy startproject
#
# For more information about the [deploy] section see:
# https://scrapyd.readthedocs.io/en/latest/deploy.html

# [settings]
# default = ecommerce.settings

# [deploy]
#url = http://localhost:6800/
# project = ecommerce
```

- 크롤러(Spider) 작성

~\ecommerce\ecommerce 여기에 spider 폴더 생성되어 있음 -> 이 위치에서 스파이더 작성해야 함

```
# scrapy genspider <크롤러이름> <크롤링페이지주소> 
scrapy genspider gmarket www.gmarket.co.kr # http 삭제하고 main 주소만!

#---- 결과
# Created spider 'gmarket' using template 'basic' in module:
#  ecommerce.spiders.gmarket # 생성된 폴더 경로
```

<br/>
~\ecommerce\ecommerce\spiders\gmarket.py 생성되어있음 <br/>
gmarket.py 내용은 다음과 같다.

```
import scrapy


class GmarketSpider(scrapy.Spider):
    name = 'gmarket'
    allowed_domains = ['www.gmarket.co.kr'] # 여기에 언급된 페이지만 크롤링 허용, 옵션이므로 지워도 됨
    start_urls = ['http://www.gmarket.co.kr']

    def parse(self, response):
        print(response.text) # pass를 다음 코드로 고치고 저장
```

- 크롤링 실행  <br/>
gmarket 페이지의 html 파일이 터미널에 print 됨

```
scrapy crawl gmarket
```

## 4.3. 크롤러(Spider) 수정하기

위의 과정대로 Test1.py라는 크롤러를 생성했다고 가정하자.

```
import scrapy


class Test1Spider(scrapy.Spider):
    name = 'test1'
    # star_urls 리스트의 마지막값부터 크롤링됨(CouponZone -> BestSellers 순으로 크롤링)
    start_urls = ['http://corners.gmarket.co.kr/Bestsellers', 'http://promotion.gmarket.co.kr/Event/CouponZone.asp']

    def parse(self, response):
        print(response.url) # 크롤링 실행시 어떤 주소 실행되는지 보여줌
```

## 4.4. Scrapy Shell - response 사용법 이해

[scrapy shell]

- 주피터 노트북 스타일로 response 익히기
- 아래 코드 실행하면 ln [1] 이런식으로 주피터 노트북처럼 response 관련 명령어 한 줄씩 실행 가능
- 시작: scrapy shell <url>
- 종료: exit

```
scrapy shell "http://corners.gmarket.co.kr/Bestsellers" # window에서는 작은 따옴표('') 에러남, 큰 따옴표("") 써야함

# [s] Available Scrapy objects:
# [s]   scrapy     scrapy module (contains scrapy.Request, scrapy.Selector, etc)
# [s]   crawler    <scrapy.crawler.Crawler object at 0x0000020913B96280>
# [s]   item       {}
# [s]   request    <GET http://corners.gmarket.co.kr/Bestsellers>
# [s]   response   <200 http://corners.gmarket.co.kr/Bestsellers>
# [s]   settings   <scrapy.settings.Settings object at 0x0000020913B960A0>
# [s]   spider     <DefaultSpider 'default' at 0x209140f8d30>
# [s] Useful shortcuts:
# [s]   fetch(url[, redirect=True]) Fetch URL and update local objects (by default, redirects are followed)
# [s]   fetch(req)                  Fetch a scrapy.Request and update local objects
# [s]   shelp()           Shell help (print this help)
# [s]   view(response)    View response in a browser
```

쉘 열고나서 다음 명령어들 실행 가능

```
view(reponse) # gmarket 페이지 브라우저에 열기
shelp() # shell 내 사용 가능한 명령어 보여줌
response.url # 크롤링 대상 url
response.text # 크롤링한 텍스트
```

### 4.4.1. css selector

원하는 데이터 선택: response.css

- ex. 페이지 제목 크롤링
- ::text -> 태그 없애고 출력

```
response.css('head > title').get() # 요소 하나 크롤링
# '<title>G마켓 - G마켓 베스트</title>'

response.css('head > title::text').get() #  ::text -> 태그 없애고 출력
# 'G마켓 - G마켓 베스트'

response.css('head > title').getall() # 동일한 요소 여러 개일 때 getall로 크롤링
# ['<title>G마켓 - G마켓 베스트</title>']
```

- ex. 베스트 아이템 크롤링 <br/>
css selector로 크롤링 안돼서 요소 보고 아래 작성

```
# <div class=best-list">
#  <ul>
#    <li class="first">

response.css('div.best-list li a::text').getall()   

# 특정 인덱스 값 가져오기, eg. 3
response.css('div.best-list li a::text')[3].get() 
```

### 4.4.2. xpath

- 요소 선택해서 xpath 확인해보면 "//*[@id='gBestWrap']/div/div[3]/div[2]/ul/li[1]/a" -> 베스트 상품 하나만 선택됨, 우리는 모든 베스트 상품 선택하고 싶음 -> xpath 문법 사용해서 작성해야 함
- div.best-list li a::text를 xpath 버전으로 바꿔 만들어보면 //div[@class='best-list']/ul/li/a
- xpath/text() -> 태그 없애고 출력

```
response.xpath("//div[@class='best-list']/ul/li/a").getall()   

response.xpath("//div[@class='best-list']/ul/li/a/text()").getall()   # /text() 태그 없애고 출력
```


### 4.4.3. 정규표현식

- 정규표현식따라 관련 키워드 추출 
- \w: 문자, 숫자 포함 텍스트
- +: 한 번 이상 등장

```
response.css('div.best-list li > a::text')[0].get()
#  '(당일발송)라꽁비에뜨 버터 꽃소금 450g 30개입 {특가}'

#----- css version
response.css('div.best-list li > a::text')[0].re('(\w+)')
# ['당일발송', '라꽁비에뜨', '버터', '꽃소금', '450g', '30개입', '특가']

#----- xpath version
response.xpath("//div[@class='best-list']/ul/li/a/text()")[0].re('(\w+)')
# ['당일발송', '라꽁비에뜨', '버터', '꽃소금', '450g', '30개입', '특가']
```

## 요약 및 적용

1. 터미널에 다음 실행

```
cd scrapyproject
scrapy startproject ecommerce
cd ecommerce\ecommerce
scrapy genspider gmarket_best corners.gmarket.co.kr/Bestsellers
```

2. 데이터 저장 함수

- \ecommerce\ecommerce 폴더 하위에 자동생성된 items.py 파일 수정
- \ecommerce\ecommerce\gmarket_best.py에서 크롤링한 내용을 전달받아 items.py에서 저장 수행 
- itemps.py를 다음과 같이 수정
- 저장할 변수 이름을 scrapy.Field() 이용해 선언

```
import scrapy


class EcommerceItem(scrapy.Item):
    title = scrapy.Field()
    price = scrapy.Field()
```

3. scrapyproject\ecommerce\ecommerce\spiders\gmarket_best.py 수정 후 저장

- dictionary처럼 아이템 지정 후 yield 저장

```
import scrapy
# 크롤링 내용을 저장하기 위해 프로젝트 생성시 자동생성된 함수 import
from ecommerce.items import EcommerceItem 

class GmarketBestSpider(scrapy.Spider):
    name = 'gmarket_best'
    allowed_domains = ['corners.gmarket.co.kr/Bestsellers']
    start_urls = ['http://corners.gmarket.co.kr/Bestsellers/']

    def parse(self, response):
        titles = response.css('div.best-list li > a::text').getall()
        prices = response.css('div.best-list ul li div.item_price div.s-price strong span::text').getall()
        
        #-- 아래 수행할 때마다 item.py에 데이터 쌓임
        for num, title in enumerate(titles): 
            doc = EcommerceItem()
            doc['title'] = title
            doc['price'] = prices[num]
            print(prices[num])
            yield doc

```

4. shell에 다음 실행

- scrapy crawl 크롤러명 -o 저장파일명 -t 저장포맷
- 저장포맷 csv, xml, json 모두 가능

- csv 버전

```
# scrapy crawl gmarket_best

scrapy crawl gmarket_best -o gmarket_products.csv -t csv
#----- 결과
# price,title
# "19,900원",(당일발송)라꽁비에뜨 버터 꽃소금 450g 30개입 {특가}
# "9,900원",[슈프림]남녀공용 후드티 남성여성 빅사이즈티셔츠 긴팔맨투맨
```

- xml 버전

```
scrapy crawl gmarket_best -o gmarket_products.xml -t xml

#----- 결과
# <?xml version="1.0" encoding="utf-8"?>
# <items>
# <item><title>(당일발송)라꽁비에뜨 버터 꽃소금 450g 30개입 {특가}</title><price>19,900원</price></item>
# <item><title>[슈프림]남녀공용 후드티 남성여성 빅사이즈티셔츠 긴팔맨투맨</#title><price>9,900원</price></item>
```

- json 버전 <br/>
한글 포맷 깨짐 -> settings.py 인코딩 수정해야 함

```
scrapy crawl gmarket_best -o gmarket_products.json -t json

#----- 결과 
# {"title": "(\ub2f9\uc77c\ubc1c\uc1a1)\ub77c\uaf41\ube44\uc5d0\ub728 \ubc84\ud130 \uaf43\uc18c\uae08 450g 30\uac1c\uc785 {\ud2b9\uac00}", "price": "19,900\uc6d0"},
```

5. 추가: 설정 바꾸기 scrapyproject\ecommerce\ecommerce\settings.py

- settings.py에 다음 코드를 추가

```
Feed_EXPORT_ENCODING = 'utf-8'
```

- gmarket_prodjcts.json 기존 파일 지우고 터미널에 다음 실행

```

scrapy crawl gmarket_best -o gmarket_products.json -t json

#----- 결과 
# {"title": "(당일발송)라꽁비에뜨 버터 꽃소금 450g 30개입 {특가}", "price": "19,900원"},
# {"title": "[슈프림]남녀공용 후드티 남성여성 빅사이즈티셔츠 긴팔맨투맨", "price": "9,900원"}
```