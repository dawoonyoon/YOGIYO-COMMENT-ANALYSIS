# 기업 연계 프로젝트 고객 댓글 분석

**기업연계 프로젝트 고객 댓글 분석**

**크롤링 대상** : 요기요 댓글 

**선정 이유** 

- 특정 음식점 자료수집의 용이성
- 고객의 댓글로 광고가 많은 SNS 등에 비해 평판 분석에 적합

**전체 구조**



<!-- ![img](https://lh4.googleusercontent.com/uRtrRAHdkTUVMIMXXjHv-NP5b36cWqgVx8YGO4W5WmAok6sngG3Lu2GW1G33c5m2Wl4bR-IP-bYuAIaDrjkPSjXoJgWtbr3NtI297-t9_I3DJn8dOaxFIpXS3uYTuRYjgevusN1P) -->

<image src="https://lh4.googleusercontent.com/uRtrRAHdkTUVMIMXXjHv-NP5b36cWqgVx8YGO4W5WmAok6sngG3Lu2GW1G33c5m2Wl4bR-IP-bYuAIaDrjkPSjXoJgWtbr3NtI297-t9_I3DJn8dOaxFIpXS3uYTuRYjgevusN1P" style="height:80px;"></image>


**I _ 크롤링**

1. 동적 크롤링 방식의 셀레니움 사용

2. 요기요에서 특정 점포를 검색한 후 스크롤바가 더 이상 내려가지 않을 때까지 크롤링

3. 크롤링한 데이터를 DataFrame 형태로 저장

   

**II _ Word Cloud**

 1. 요기요 댓글을 크롤링한 Dataframe 로드 

 2. Pandas와 정규표현식을 이용해 기본적인 전처리, 가공

 3. WordCloud 라이브러리를 통해 Word Cloud 작성하여 최빈단어 확인 가능

    

    ![img](https://lh3.googleusercontent.com/zfv3p5TNr2ELNKiYcEoulsnruudHopALlWqw-mHqv-68-Iqxb_tt15EotQBSw9EaoIqqKxnt-3LxGPxvFLEys1op7pjwQIgVsyaENuaWUM2NCdUnKDcZSA2Newz-z-FdVnYX52Nv)

    <center>Figure 1. WordCloud</center>

    문제점 : Word Cloud를 생성하는 것만으로는 긍정, 부정 판단이 어려움

    

**III _ Word2Vec**

1. 요기요 댓글 크롤링 DataFrame에 gensim 라이브러리를 적용, Word2Vec 모델 훈련

2. 훈련된 모델에 WordCloud를 통해 추출된 최빈단어들을 입력

3. 최빈단어들의 연관단어 상위 10개를 추출 

   

   ![img](https://lh4.googleusercontent.com/PmLlNNafEwInXlrIGdcq4fuYga9nZKV6lzsxn2xuW7ZGlAGJ7VG_j4qxV-4c1YI2sjAaK71N9nEuJK6PSVh6AqBotCI8II9U1MZ7qC0BEK4FhnQP4n3VS6luezSzTSoAcjteTFUO)

   <center>Figure 2. Word2Vec</center>

   

**IV _ BERT**

1. 긍정, 부정 댓글을 구분하여 각각의 그룹에서 최빈단어 추출 시도

2. BERT 텍스트 분류 모델을 네이버 영화 댓글 데이터를 통해 학습 → 학습에 필요한 데이터를 구하기 어려워 긍정, 부정 분류에 자주 사용되는 네이버 영화 댓글 데이터를 통해 BERT를 학습 

3. 모델을 통해 긍정, 부정 댓글을 구분하여 WordCloud로 각각의 최빈단어를 확인할 수 있었지만, Acurracy 0.87 수준의 분류 모델, 영화 댓글이 아닌 배달앱 댓글 분석이라는 내용의 차이로 적용에 한계점이 존재. 보다 사용 분야에 적합한 데이터를 이용하여 학습시키는 방법을 통해 개선 필요

   

   ![img](https://lh4.googleusercontent.com/wFgZHxyphIl9tamNbXEnLSYeLindNDf9Qej6FbS1lO3nxsfll0kSJRk1sAS2S0qk_FW2pvYUfIPIqwnLbo8XbS46zKGMc-1he8jS87lZX3Coh_mFxygYoQ26onpfbX-I9rW3pgl8)

   <center>Figure 3. Sentiment_Analysis</center>

   

**V _ KoGPT2**

1. 언어 생성 모델은 GPT모델을 Finetuning하여 실제 존재하는 댓글은 아니지만 특정 키워드를 입력하여 전체 댓글에 기반한 평판, 가상의 댓글을 생성할 수 있는지 테스트

2. Epochs를 늘려 과적합을 시킬 경우 특정 키워드 입력으로 실제로 존재했던 댓글을 생성시킬 수는 있지만, 과적합을 시키지 않을 경우 댓글에 존재하지 않는 완전히 새로운 내용을 생성함으로서 신뢰도 문제가 발생

3. Finetuning에 사용되는 데이터 자체가 매우 적다는 구조적 문제 존재

4. 생성된 문장을 다른 모델을 통하여 전체 댓글을 기반으로 할 때 T/F 판단을 해준 후 T일 경우에는 문장을 출력하는 등의 개선 필요

   

![img](https://lh6.googleusercontent.com/V4zj2NbpylkgUXNUojOBKk8-3m3nj5kzNi4mwaObL8QdH05VyQsN1b1AtfUyOmJSQ2GHlqgLvl-mGzvF66yIp-MPq5vxwDOOhEulM5GOSBr_fsMkJeTl4Nbo8kJvgKI1J2R9_7EN)

<center>Figure 4. Text_Generation</center>



# 파일 설명

YOGIYO_v2_1.ipynb : 크롤링, 워드클라우드, bert 감성분석 후 워드클라우드   
bert_sentiment_train.ipynb : bert 감성분석 학습파일   
KoGPT2.ipynb : KoGPT yogiyo 댓글 finetuning 파일   
test.txt : KoGPT2에 사용하는 댓글 모음 텍스트 파일

# 참고자료
yogiyo 크롤링 파일 참고 자료 :https://acton21.tistory.com/22   
bert 학습 파일 참고자료 : https://github.com/deepseasw/bert-naver-movie-review/blob/master/bert_naver_movie.ipynb   
KoGPT2 파일 참고 자료 : https://github.com/NLP-kr/tensorflow-ml-nlp-tf2/blob/master/7.PRETRAIN_METHOD/7.4.1.gpt2_finetune_LM.ipynb
