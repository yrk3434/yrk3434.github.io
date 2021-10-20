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
    allowed_domains = ['www.gmarket.co.kr']
    start_urls = ['http://www.gmarket.co.kr/']

    def parse(self, response):
        print(response.text) # pass를 다음 코드로 고치고 저장
```

- 크롤링 실행  <br/>
gmarket 페이지의 html 파일이 터미널에 print 됨
```
scrapy crawl gmarket
```