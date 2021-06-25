---
title: NLP_Introduction&Application
category: Theory
order: 3
use_math: true
comments: true
toc: true
toc_sticky: true
toc_label: 목차
---


# 자연어 응용_책 정리1

<font size='5'> 파이썬으로 배우는 응용 텍스트 분석(벤자민 벵포트 외 2인) <font/> <br/>
<font size='4'> - 언어 인식 데이터 제품 개발을 위한 머신러닝 <font/> <br/>
<br/>	
- 이 책은 자연어 처리 기본의 전반을 익힐 수 있으며 영어 텍스트에 대한 분석을 배울 수 있다.
- 이론보다는 코드와 응용에 포커스가 맞춰진 책이다.
- 코드 설명 중 중요한 함수가 쓰인 부분만 발췌했다.
- 한국어 버전의 코드는 추후 추가하겠다.

# Contents
1. 언어와 계산
2. 사용자 정의 말뭉치 구축
3. 말뭉치의 전처리와 가공
4. 텍스트 벡터화와 변환 파이프라인
5. 텍스트 분석을 위한 분류
6. 텍스트 유사성을 위한 군집화(토픽모델링)
7. 문맥 인식 테스트 분석
8. 텍스트 시각화
9. 텍스트의 그래프 분석

# CH1. 언어와 계산

- 언어 계산 모델: 불완전한 phrase를 받더라도 발화를 완료할 가능성이 높은 후속 단어를 추론(text is predictable)
- 유사성: 엔트로피를 통해 파악

### 자질
- **언어 자질**: 단어 집합 count를 통해 언어 자질 파악 <br/> eg; 텍스트 성별 식별 <br/> 파이썬 nltk.sent_tokenze(text) -> nltk.word_tokenize(sentence) -> collections.Counters
- **맥락 자질**: 언어자질이 아닌 **어감**에 의존, 같은 단어여도 맥락 따라 긍정일수도 부정일수도 있음 eg. 정서분석, 단어주머니(단어 동시출현 고려), 엔그램분석(n개 주변 단어 고려) 
- **구조적 자질**:  단순 의미가 아닌 논리적 추론 적용 가능한 데이터 분석, 구문론적 분석(문장, 구 단위 분석)

# CH2. 사용자 정의 말뭉치 구축

- 특징분석: 언어를 표현하는 방식, 구문론적 패턴을 성분 별로 분해하여 통계적 메커니즘 적용
	
### 1. 말뭉치(Corpora)

- 자연어가 들어 있는 관련 문서들의 모음집, eg. 이메일, 트윗, 책
- 지도 학습: 말뭉치에 주석
- 비지도학습: 토픽 모델링, 문서 군집화
- 단락 > 구문 단위 문장 > 문장 > 어휘 > 단어(음절, 음소, 접사, 문자 조합)
- 영역 특정 말뭉치
	- 특정 분야의 언어 모델링이 일반 언어 모델링보다 잘 작동, 범위를 좁혀 예측 공간이 작아지기 때문

<font size='5'> [Baleen 수집 엔진] <font/>
- 말뭉치 작성 오픈소스, 스포츠, 게임 등 12범주에서 RSS 피드 수집

## 말뭉치 데이터 관리
- 말뭉치 디스크: 개별파일 저장하지 않는 것 권장 eg. MBox 형식 저장, 트윗은 메타 데이터 포함 JSON 구조 

## 말뭉치 리더

텍스트 데이터는 여러 방식으로 접근 가능하다.

### NLTK를 사용한 스트리밍 데이터 엑세스
- NLTK 패키지 역할:말뭉치, 토큰 생성, 형태소분석, 품사 태깅 , [참고](https://datascienceschool.net/03%20machine%20learning/03.01.01%20NLTK%20%EC%9E%90%EC%97%B0%EC%96%B4%20%EC%B2%98%EB%A6%AC%20%ED%8C%A8%ED%82%A4%EC%A7%80.html)

	- PlaintextCorpusReader: 빈 줄로 단락 구분
	- TaggedCorpusReader: 문장이 한 줄, 토큰들이 구분되는 말뭉치
	- BracketParseCorpusReader: 괄호로 구분된 Parse Tree
	- ChunkedCorpusReader; 괄호로 묶인 청크(선택적 태그 지정)
	- TwitterCorpusReader: 줄 구분 JSON
	- WordListCorpusReader: 단어들이 한 줄에 한 개씩 목록
	- XMLCorpusReader
	- CategorizedCorpuseReader

```
from nltk.corpus.reader.plaintext import CategorizedPlaintextCorpusReader

corpus = CategorizedPlaintextCorpusReader(path, DOC_PATTERN, cat_pattern= CAT_PATTERN) # pattern은 정규표현식

corpus.categories()
# ['Star Trek', 'Star Wars']

corpus.fileids()
# ['Star Trek/Star Trek - Balance of Terror.txt', ...]
```
 
### HTML 말뭉치 읽기
```
from nltk.corpus.reader.api import CorpusReader
from nltk.corpus.reader.api import CategorizedCorpusReader

TAGS = ['h1', 'h2', 'h3', 'h4' ,'h5', 'h6' ,'h7', 'p', 'li']

CategorizedCorpusReader.__init__(self,kwargs)
CorpusReader.__init__
```


### 데이터베이스에서 말뭉치 읽기
```
import sqlite3

_cur = sqlite3.connect(path).cursor()
_cur.execute("SELECT score From reviews")
for score in iter(_cur.fetchon, None):
	yield score
```

# CH3. 말뭉치의 전처리와 가공
- 이 챕터에서는 텍스트를 작은 단위로 쪼개가는 과정을 코드를 통해 보여준다.
- raw 말뭉치 -> (HTML -> 단락 -> 문장 -> 토큰 -> 태그) ->...

## 문서 쪼개 보기
 - NLTK corpus reader: 
	 - raw(): 전처리 없이 텍스트 엑세스
	 - sents(): 개별 문장 생성자
	 - words(): 텍스트를 단어로 토큰화

### 1. 핵심 내용 식별 및 추출
- Unparseable, Document: HTML 텍스트 추출, 정리
	
```
from readability.readability import Unparseable
from readability.readability import Document as Paper

for doc in docs(fileid, cotegries):
	try: 
		# 스크립트가 아닌 모든 텍스트와 스타일 태그를 제거
		yield Paper(doc).summary() 
	except Unparseable as e:
		print("Could not parse HTML: {}".format(e))
		continue

# 로그에 대한 경고 생성
import logging
log = logging.getLogger("readability.readability")
log.setLevel("warning)
```

### 2. 단락 나누기
beautifulsoup을 통해 html 텍스트로부터 단락을 추출한다.
```
import bs4
TAGS = ['h1', 'h2', 'h3', 'h4' ,'h5', 'h6' ,'h7', 'p', 'li']

soup = bs4.Beautifulsoup(html, 'lxml') # lxml: HTML 구문 분석기
for element in soup.fild_all(TAGS):
	yield element.text
soup.decompose() # 메모리 확보 위해 트리 파괴
```
	
### 3. 문장 나누기(분할, segmentation)

문장의 시작과 끝인 단어와 구두점을 사전 학습한 sent_tokenizer를 이용한다. (sent_tokenizer는 PunktSentenceTokenizer 토크나이저 사용, 영어 텍스트에 대해 훈련, 유럽 언어도 잘 작동) 구두점이 약어, 줄임말 등에도 사용되므로 비표준 텍스트에 사용하면 제대로 작동하지 않을 수도 있다.

```
from nltk import sent_tokenize

for paragraph in paras(fileids, categories):
	for sentence in sent_tokenize(paragraph):
		yield sentence

# output 예시
# ['Beautiful is better than ugly, ...]
```

### 4. 개별 토큰 식별(tokenization)
- 공백, 구두를 떼어 내고 영문자, 비영문자 반환
- 합성어, 축약형 등 처리 기준에 따라 토크나이저 선택, eg. TreebankWordTokenizer, WordPunctTokenize, PunktWordTokenizer

```
from nltk import wordpunct_tokenize

for sentence in sents(fileids, categories):
	for sentence in wordpunct_tokenize(paragraph):
		yield token
```

### 5. 품사 태깅(part-of-speech-tagging)
- (태그, 토큰) 튜플 꼴로 아웃풋 반환
- NLTK 품사 태거를 위한 옵션 eg. DefaultTagger, RegexpTagger, UnigramTagger, BrillTagger
```
from nltk import pos_tag, sent_tokenize, wordpunct_tokenize
for paragraph in paras(fileids=fileids):
	yield [
		pos_tag(wordpunct(tokenize(sent))
		for sent in sent_tokenize(paragraph)
		]
		
# Penn Treebank 태그셋 NN: 명사, NNS: 복수명사, JJ: 형용사 ... & 시제표시
# output 예시
# [[('The', 'DT'), ('old', 'JJ'),...,('.','.')],
#  [('The', 'DT'), ('contractors', 'NNS'), ..., ('.','.')]]
```


# ch4.  텍스트 벡터화와 변환 파이프라인
- 텍스트 데이터를 머신러닝에 적용하기 위해 row(obs), column(feature) 꼴의 수치 데이터로 변환한다. 즉. 문서의 특징을 다차원 특징 공간(multidimensional feature space, manifold)에 맵핑(투영)하는 것이다. 이를 벡터화 또는 특징 추출이라고 부른다. 
- 잘 투영된 특징공간은 **유사한 의미(의미 공간, semantic space)의 문서를 유사한 거리로 인코딩**한 것이다.  가장 간단한 인코딩 방법은 **단어 주머니(bag-of-word)**다.
- **NLTK, 사이킷런, Gensim**은 재사용 가능한 인코딩 변환기다.

## 공간 내 단어
- 단어의 벡터 표현 방법은 다음 네 가지가 있다.
	- 빈도 벡터: 용어의 빈도 사용, 베이즈 모델
	- 원핫 인코딩: 용어 이진화, 신경망
	- TF-IDF(term frequency - inverse document frequency): 모든 문서와 해당 문서 비교, 일반적인 용도
	- 분산 표현: 용어의 맥락 상 유사도(연속값) 사용, 복잡한 관계 모델링

### 1. 빈도 벡터
- 문서 내 단어 빈도를 벡터에 채우는 방법이다.
- 모든 단어를 벡터화하다보니 0 요소가 많아 벡터가 희박(sparse)해질 수 있다.
- 문법과 문서의 상대적 위치 무시 -> long tail, Zipfian distribution 문제

eg. Bats can see via echolocation. See the bat sight sneeze!

|at|bat|can|...|see|...|
|:----:|:----:|:----:|:----:|:----:|:----:|
|0|2|0|...|2|...|

- NLTK
```
from collections import defaultdict

def vectorize(doc):
	features = defualtdict(int)
	for token in tokenize(doc):
		features[token] += 1
	return features

vectors = map(vectorize, corpus)
```
- 사이킷런
```
from sklearn.fearure_extraction.text import CountVetorizer

vectorizer = CountVectorizer()
vectors = vectorizer.fit_transform(corpus)
```
- Gensim
```
import gensim

corpus = [tokenize(doc) for doc in corpus]
id2word = gensim.corpora.Dictionary(corpus)
vectors = [id2word.doc2bow(doc) for doc in corpus]
```

### 2. 원핫 인코딩
- 문서 내 단어가 존재하면 1, 아니면 0
- 빈도 벡터 방식과 달리 토큰 분포의 불균형 문제 완화
- 인공 신경망에서 일반적으로 사용
- 문서 수준의 유사성은 알 수 있지만, 단어별 유사성은 알 수 없음(모든 단어가 똑같은 거리)

eg. The elephant sneezed at the sight of potatoes.

|at|bat|can|...|see|...|
|:----:|:----:|:----:|:----:|:----:|:----:|
|1|0|0|...|0|...|

- NLTK
```
def vectorize(doc):
return { token: True for token in doc }
vectors = map(vectorize, corpus)
```
- 사이킷런 <br/>
주의. sklearn.preprocessing 모듈의 OneHotEncoder는 각 벡터 column을 이진이 아닌 독립적인 범주형 변수로 처리
```
from sklearn.preprocessing import Binarizer

freq = CountVectorizer()
corpus = freq.fit_transform(corpus)

onehot = Binarizer()
corpus = onehot.fit_transform(corpus.toarray())
```
- Gensim <br/>
빈도 벡터화를 확장해 원핫 인코딩
```
corpus = [tokenize(doc) for doc in corpus]
id2word = gensim.corpora.Dictionary(corpus) # 빈도 벡터화
vectors = [
	[(token[0], 1) for token in id2word.doc2bow(doc)] # id2word 이용한 원핫 인코딩
	for doc in corpus
]			
```

### 3. 용어빈도-역문서빈도(TF-IDF, term frequency - inverse document frequency)
-  [참고](https://wikidocs.net/31698)
- 다른 문서에 비해 특정 문서에서 갖는 특정 토큰의 상대적 중요도 계산
- 빈도 방식과 원핫 인코딩은 말뭉치의 맥락을 고려하지 않은 반면 TF-IDF는 맥락 고려
- 토큰의 빈도를 다른 문서와 비교 eg 야구 텍스트에서 '심판', '베이스' 등 토큰은 중요, 스포츠 말뭉치 전반의 '뛴다', '점수' 등의 토큰은 덜 중요
- 말뭉치 전반에 나타나는 용어(불용어, eg. a, the, of) 문제 해결, bag-of-word에 널리 사용됨
	- $ N$ : 문서 개수
	- $ n_t $: 모든 문서에서 용어 t의 발생횟수
	- $ d $: 특정 문서
	- $ D $: 전체 문서
	- 	용어빈도: $ tf(t,f) = 1 + log f_{t,d} $ , 특정 문서에서 특정 용어 t의 등장 횟수에 관한 식
	- 역문서빈도: $ idf(t,D) = log (1 + \frac{N}{n_t}) $ , 전체 문서에서 특정 용어 t의 등장횟수에 관한 식 
	- **TF-IDF: $ tfidf(t,d,D) = tf(t,d) \cdot  idf(t,D) $**
	- 1에 가까울수록 해당 용어가 중요, 0에 가까울수록 덜 중요
- NLTK
```
from nltk.text import TextCollection

def vectorize(corpus):
	corpus = [tokenize(doc) for doc in corpus]
	texts = TextCollection(corpus)
	
	for doc in corpus:
		yield { term: texts.tf_idf(term, doc) for term in doc }
```
- 사이킷런
```
from sklearn.feature_selection.text import TFidVectorizer

tfidf = TfidVectorizer()
corpus = tfidf.fit_transform(corpus)

# output
# ((doc, term), tfidf)
```
- Gensim
```
corpus = [tokenize(doc) for doc in corpus]
lexicon = gensim.corpora.Dictionary(corpus)
tfidf = gensim.models.TfidfModel(dictionary = lexicon, normalize =T) # tfidf 인스턴스
vectors = [tfidf[lexicon.doc2bow(doc)] for doc in corpus]

# save model
lexicon.save_as_text('lexicon.txt', sort_by_word = T)
tfidf.save('tfidf.pkl')

# load model
lexicon = gensim.corpora.Dictionary.load_from_text('lexicon.txt')
tfidf = gensim.models.TfidfModel.load('tfidf.pkl')
```

### 4. 분산 표현
- 연속값으로 텍스트 벡터를 임베딩(1,2,3번 방법은 non-negative 값으로 인코딩하는 반면, 분산 표현은 음수도 가짐)
- 맥락의 유사성을 인코딩하는 것이 가능
- word2vec: CBOW(continous bag-of-words) 기반으로 단어 표현 훈련
- doc2vec: word2vec의 확장
- 규모가 큰 말뭉치의 경우 PCA, SVD 통해 차원축소

- Gensim
```
from gensim.models.doc2vec import TaggedDocument, Doc2Vec

corpus = [list(tokenize(doc)) for doc in corpus]
corpus = [ TaggedDocument(words, ['d{}'.format(idx)])  # word2vec 분산 표현, (words, tags)
			for idx, words in enumenrate(corpus) ]

model = Doc2Vec(corpus, size=5, min_count=0) # size보다 작은 빈도의 토큰 무시
print(model.docvecs[0])
# [0.01797, -0.01509,...]
```
