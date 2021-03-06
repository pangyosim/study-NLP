## BERT

### 1. NLP에서의 사전 훈련(Pre-training)

#### 1) 사전 훈련된 워드 임베딩

  임베딩을 사용하는 방법으로는 두 가지가 있습니다. 임베딩 층(Embedding layer)를 랜덤 초기화해서 처음부터 학습하는 방법과 방대한 데이터로 Word2Vec 등과 같은 임베딩 알고리즘으로 이미 학습된 임베딩 벡터들을 사용하여 학습시키는 방법입니다. 만약에 데이터가 적은 테스크에서 사전 훈련된 임베딩을 사용하면 성능을 높일 수 있습니다. 

기존 임베딩 벡터는 아래 그림과 같이 하나의 단어가 각각 하나의 벡터값으로 맵핑되므로 문맥을 고려하지 못하는 문제점이 있었습니다. 또한, 다의어나 동음이의어를 구분하지는 못하는 문제점도 있었습니다. 예를 들어, 한국어에는 '사과🍎'라는 단어가 먹는 과일의 의미도 있고, 용서를 빈다는 의미로도 사용됩니다. 그러나 기존 임베딩 벡터는 '사과'라는 벡터에 벡터값 한개를 맵핑하므로 두 가지 의미를 구분할 수 없었습니다.

![1 PNG](https://user-images.githubusercontent.com/87213815/132116831-02f11b33-3bdd-45bf-b4fa-84fe3478cb53.png)

#### 2) 사전 훈련된 언어 모델

![BERT PNG](https://user-images.githubusercontent.com/87213815/132334280-eb8577ef-d97e-4825-90c3-2132e95f0891.png)

  BERT는 트랜스포머의 encoder를 기반으로 수많은 어텐션 헤드를 가진 레이어로 사전학습, 전이학습을 통해 여러 자연어 처리에 사용되는 취지에서 처음에 Google AI Research 팀에서 개발하였습니다. 사전학습의 경우에는 레이블되지않은 대량의 데이터를 학습하는데 학습 시에 데이터의 일부를 마스킹하여 모델이 마스킹한 부분을 맞추게하는 Masked Language Modeling과 질의응답에 중요한 문장별 관계를 학습하는 Next Sentence Prediction이 적용되었고, Birectional Encoder를 이용하여 모든 토큰에 양방향으로 어텐션이 적용되도록 설계되어있습니다. 

Fine-tuning은 사전학습된 모델을 다른 Task에 사용하기 위해 학습시키는 과정입니다. 사전학습된 데이터와 유사한 특징을 가진 데이터 양이 적어도  Fine-tuning 하여 모델을 도메인에 최적화 시키고 보다 좋은 성능을 얻을 수 있습니다.  예를들어, 데이터 증식, 하이퍼 파라미터 튜닝, 파인튜닝 네트워크 추가 같은 방법이 있습니다. 

![11 PNG](https://user-images.githubusercontent.com/87213815/132334384-c0a55c95-1185-4c01-b6a0-02e4fb6a9845.png)

  2015년 Google의 'Semi-surpervised Sequence Learning' 논문에서는 LSTM 언어 모델을 학습하고나서 텍스트 분류에 추가하는 학습을 제안했습니다. 이 언어 모델은 주어진 텍스트로부터 이전 단어들로부터 다음 단어를 예측하도록 학습시키기 때문에 별도의 레이블이 부착되지 않은 텍스트 데이터로도 학습 가능합니다. 레이블이 없는 데이터로 학습된 LSTM이랑 가중치가 랜덤으로 된 LSTM을 두고, IMDB 리뷰 데이터를 학습하여 정확도를 테스트하면 레이블이 없는 데이터의 LSTM이 더 좋은 성능을 얻습니다.
  
![123123 PNG](https://user-images.githubusercontent.com/87213815/132334478-2470c920-14d4-41d8-9f89-e5e34a8716b4.png)

* SA-LSTM : Sequence Auto encoder를 사용하여 initialize 시킨 LSTM

  적은 데이터가 아닌 방대한 데이터를 LSTM 언어 모델을 학습해두고, 이러한 언어모델을 다른 Task에 적용 시키는 ELMO같은 아이디어도 있습니다.
  
![124412 PNG](https://user-images.githubusercontent.com/87213815/132334615-87e37ac3-038e-4ae2-91da-3895ab07f6a4.png)

  ELMO는 순방향 언어모델, 역방향 언어 모델을 각각 따로 학습시켜준 후에, 사전학습된 언어 모델로부터 임베딩 값을 얻는 아이디어입니다. 사전훈련한 모델로 NLP Task에 Fine-tuning 시키면 같은 'read'란 단어도 현재형인가, 과거형인가 구분할 수 있습니다.

![1114 PNG](https://user-images.githubusercontent.com/87213815/132334672-3321abf1-61cb-448f-8394-27cf5edc543c.png)

  이 후에 트랜스포머가 등장하고나서, 더이상 LSTM으로 사전학습을 시키지 않고 트랜스포머를 사용하기 시작했습니다.
  
![1](https://user-images.githubusercontent.com/87213815/132334719-051bc561-563f-467d-b8b8-855290bf1215.png)

  기존 사전학습 모델은 Text의 Unlabled 데이터는 많지만, Labled 데이터는 풍부하지 못하고 빈약한 상황입니다. 따라서, 다양한 Unlabled된 text corpus에서 Language Model을 Pre-taining하고 각 task에 맞게 Fine-tuning하면 모델의 성능을 높일 수 있습니다.  그래서 GPT-1은 Unsupervised pretaining과 supervised-fine-tuning을 결합한 semi-supervise 접근을 사용하여 다양한 task에 약간의 조정만으로도 번역할 수 있는 representation을 학습시킬 수 있습니다.
  
  
  
![121](https://user-images.githubusercontent.com/87213815/132334754-3e8c11d7-8cea-4d1c-98b8-2ba83c34cb2f.png)

  단방향 언어모델은 여태까지 배운 전형적인 언어모델 입니다. 토큰 <SOS>가 들어가면, 다음 단어 'I'를 예측하고 그 다음단어 'am'을 예측합니다. 이와 다르게, 양방향 언어모델에서 초록섹 LSTM 셀은 순방향 언어 모델로 <sos> 토큰을 받아 'I'를 예측하고, 그 다음 단어 'am'을 예측합니다. 그런데 'am'을 예측할 때, 출력층은 주황색 LSTM 셀인 역방향 언어모델의 정보도 가지고 있습니다. 그런데 'am'을 예측하는 시점에서 역방향 언어모델이 관측한 단어들은 'a','am','I' 이렇게 3가지 단어입니다. 따라서, 양방향 언어 모델은 텍스트 분류나, 개체명 인식에서 좋은 성능을 얻을 수 있습니다. 
  
#### 3) 마스크드 언어 모델 (Masked Language Model)
  
  마스크드 언어 모델은 입력으로 사용하는 문장의 토큰중 15%의 확률로 선택된 토큰을 [MASK] 토큰으로 변환시키고, 언어모델을 통해 변환되기전 [MASK] 토큰을 예측하는 언어모델입니다. 
