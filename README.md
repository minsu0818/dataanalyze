![80632473_1713854345213_37_600x600](https://github.com/minsu0818/dataanalyze/assets/144076842/e8006771-f4f5-4537-ad4d-dea8d1f3208f)

# MobileBERT를 활용한 런던의 호텔 리뷰 분석
<img src="https://img.shields.io/badge/PyTorch-E34F26?style=flat-square&logo=PyTorch&logoColor=white"/></a>
<img src="https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=Python&logoColor=white"/></a>

## 1. 개요

### 1.1 숙박 산업과 사용자의 리뷰


![image](https://github.com/minsu0818/dataanalyze/assets/144076842/559f5bb7-5355-4657-b2c2-e708898b809c)
> "코로나19 충격이 호텔산업에 미치는 영향 : 명동 소재 A 호텔 사례를 중심으로" 논문에서 일부 발췌<br/>


코로나 판데믹이 숙박 산업에 큰 피해를 입혔다. 위 자료를 보면 코로나가 터진 시점인 2020년 1월경부터 전년도 같은 달 대비 절반의 매출 실적도 내지 못했다는 것을 알 수 있다. <br/>
이렇듯 이 시기의 숙박산업은 말로 표현하지 못할 정도의 경기 침체를 겪고 있었다

그렇다면 코로나가 독감과 같은 4등급 질병으로 분류된 현재 2024년에도 숙박 산업이 침체됐을까? 

![image](https://github.com/minsu0818/dataanalyze/assets/144076842/1c7cded2-052b-412d-8222-af85171df911)
> 호스피탈리티 테크 기업 온다(ONDA)의 2023 상반기 호스피탈리티 데이터 & 트렌드 리포트

 위 리포트를 본다면 침체와는 거리가 먼 행보를 걷고 있다는 것을 알 수 있다.<br/>
 코로나가 종식된 2022년 부터는 숙박 산업이 회복세이 이르면서, 수요가 증가하는 전년도 대비 1.5 증가하는 현상을 보였고 다음 해인 2023년에는 26.8프로의 증가율로 꾸준한 상승을 곡선을 그리고 있다 . <br/>
 오히려 코로나라는 족쇄가 사람들의 마음에 여행이라는 두 단어를 더 강하게 각인시킨 계기가 된 것이 아닌가 라는 생각이 든다.<br/><br/>
 숙박 산업이 상승세를 타고 있는 현재 여행을 성공적으로 마치기 위해서는 어떠한 요소가 중요할까?<br/>
 
 여행을 성공적으로 끝맺기 위해서는 개인의 성향에 맞는 숙소를 정하는게 중요하다 생각한다. 그래서 사람들은 호텔에 대한 정보를 찾는데 트립어드바이저인 booking.com 경우에는 전세계 10만개 호텔에 100만개의 리뷰가 있다. 그렇기 때문에 사람들이 호텔 부킹하기 전에 참조를 많이한다.<br/>
 



> Positive reviews have a significant impact on consumer hotel booking decisions.
Consistent with the findings of the majority of researchers, positive online reviews contribute to
establishing a positive brand image for hotels. Positive user reputation leads to word-of-mouth
promotion and positively influences consumer decision-making. More positive reviews also attract
56 more new customers to book the hotel, resulting in a long-term and stable increase in hotel bookings.  
--Research on the Impact of Online Reviews on Hotel Selection for Travelers 논문에서 일부 발췌

긍정적인 리뷰는 호텔의 긍적적인 브랜드 이미지와 관련이 되며 호텔 부킹에도 긍정적인 영향을 끼친다고 한다.
실제로 이 논문의 연구에서도 많은 긍정적인 리뷰가 있는 호텔은 56명의 신규 손님이 생겼다고 했다.


이렇듯  숙박업체의 리뷰가 숙박을 운영하는 주체도, 이것을 이용하는 손님도 모두 중요한 영향을 미친다. 그래서 호텔과 관련된 리뷰를 분석하는 프로젝트를 해보고자한다.


### 1.2 런던을 선택한 이유 
> 주요국의 관광지가 호텔 사람들에게 매력이 많은 만큼 관광객들이 많을 것 같기 떄문에 주요국들 위주로만 할 것이다. 그중 런던을 선택했다<br/>
>
> 유로모니터 인터내셔널이 발표한 2023년 세계에서 가장 많이 방문한 도시는 순서대로 ▲튀르키예 이스탄불 ▲영국 런던 ▲아랍에미리트 두바이 ▲튀르키예 안탈리아 ▲프랑스 파리 ▲홍콩 ▲태국 방콕 ▲미국 뉴욕 ▲멕시코 칸쿤 ▲사우디아라비아 메카다.
>
> 출처 : 숙박매거진(https://www.sukbakmagazine.com)<br/>

런던이 2023년에 2위를 할 정도로 많은 관광객들이 많이 방문한 도시이다. 런던만의 차별점으로는 오랜 역사를 자랑하는 박물관, 근엄함의 상징인 영국 왕실, 런던의 상징으로 통하는 이층버스와 튜브, 명품 뮤지컬과 축구 종주국이라는 타이틀 등이 존재한다. 이렇듯 런던을 설명하는 키워드는 그야말로 무궁무진하다.<br/><br/>
그렇기 때문에 년에 몇백, 몇천만명의 관광객들이 다녀가는 런던에 호텔이나 모텔과 같은 숙박업이 많이 발달을 한 도시다. 
숙박업이 많이 발달한 도시인 만큼 호텔도 많기 때문에 리뷰 또한 많다. 리뷰가 많다면 긍정적인 리뷰와 부정적인 리뷰가 둘다 많다는 건데 이러한 요소들이 호텔의 평점에 영향을 얼마나 미치는지 알아 보기 위해 mobile bert를 이용한 런던 호텔 리뷰의 긍부정을 해볼까 한다   <br/><br/>
 
## 2. 데이터 들여다 보기

* 데이터명<br/>

|Hotel_Address|Additional_Number_of_Scoring|Review_Date|Average_Score|Hotel_Name|Reviewer_Nationality|Negative_Review|Review_Total_Negative_Word_Counts|Total_Number_of_Reviews|Positive_Review|Review_Total_Positive_Word_Counts|Total_Number_of_Reviews_Reviewer_Has_Given|Reviewer_Score|Tags|days_since_review|lat|lng|
|--------------|-----------------------------|------------|--------------|----------|---------------------|----------------|------------------------------|----------------------|---------------|-------------------------------|------------------------------------------|--------------|-----|------------------|---|---|
|호텔 주소|추가 점수 수|리뷰 작성 날짜|평균 점수|호텔 이름|리뷰어 국적|부정적 리뷰 내용|부정적 리뷰 단어 수|리뷰 총 개수|긍정적 리뷰 내용|긍정적 리뷰 단어 수|리뷰어가 작성한 총 리뷰 수|리뷰어 점수|태그|리뷰 작성 후 경과 일수|위도|경도|<br/>



* 활용할 데이터 예시


 |-|Hotel_Address|Additional_Number_of_Scoring|Review_Date|Average_Score|Hotel_Name|Reviewer_Nationality|Negative_Review|Review_Total_Negative_Word_Counts|Total_Number_of_Reviews|Positive_Review|Review_Total_Positive_Word_Counts|Total_Number_of_Reviews_Reviewer_Has_Given|Reviewer_Score|Tags|days_since_review|lat|lng|
|-|--------------|-----------------------------|------------|--------------|----------|---------------------|----------------|------------------------------|----------------------|---------------|-------------------------------|------------------------------------------|--------------|-----|------------------|---|---|
|1|s Gravesandestraat 55 Oost 1092 AA Amsterdam Netherlands|194|8/3/2017|7.7|Hotel Arena|Russia|"I am so angry that i made this post available..."|397|1403|"Only the park outside of the hotel was beautiful"|11|7|2.9|[' Leisure trip ', ' Couple ', ' Duplex Double Room ', ' Stayed 3 nights ']|0 days|52.360576|4.915968|
|2|s Gravesandestraat 55 Oost 1092 AA Amsterdam Netherlands|194|8/3/2017|7.7|Hotel Arena|Ireland|"No Negative"|0|1403|"No real complaints the hotel was great great ..."|105|7|7.5|[' Leisure trip ', ' Couple ', ' Duplex Double Room ', ' Stayed 3 nights ']|0 days|52.360576|4.915968|
|3|s Gravesandestraat 55 Oost 1092 AA Amsterdam Netherlands|194|7/31/2017|7.7|Hotel Arena|Australia|"Rooms are nice but for elderly a bit difficult..."|42|1403|"Location was good and staff were ok It is cut..."|21|9|7.1|[' Leisure trip ', ' Family with young children ', ' Duplex Double Room ', ' Stayed 3 nights ']|3 days|52.360576|4.915968|
|..|...|...|.....|.....|...|...|..|
|515737|Wurzbachgasse 21 15 Rudolfsheim F nfhaus 1150 Vienna|168|8/9/2015	|8.1|Atlantis Hotel Vienna	|Hungary|"I was in 3rd floor It didn t work Free Wife	"|13|2823|"staff was very kind"|6|1|8.3|[' Leisure trip ', ' Family with young children ', ' 2 rooms ', ' Stayed 2 nights ']|725 days|48.203745|16.335677|
<br/>

 이 데이터는 총 515737건이고 평점(rating)은 1점부터 10점까지 구성되어있다.<br/><br/>

* 도시별 호텔의 분포<br/><br/>
![image](https://github.com/minsu0818/dataanalyze/assets/144076842/087e9519-7ed1-48f8-8507-13c5f2db6ebb)<br/>


### 2.1 추출 데이터<br/>


* 런던에 있는 호텔의 총 갯수<br/>

| City   | Unique Hotels |
|--------|---------------|
| London | 400           |<br/>

* 런던의 호텔의 리뷰 수 상위 15개만 추출 <br/>
  
|Index|Hotel_Name|Count|
|-|----------|-----|
|1|Britannia International Hotel Canary Wharf|4789|
|2|Strand Palace Hotel|4256|
|3|Park Plaza Westminster Bridge London|4169|
|4|Copthorne Tara Hotel London Kensington|3578|
|5|DoubleTree by Hilton Hotel London Tower of London|3212|
|6|Grand Royale London Hyde Park|2958|
|7|Holiday Inn London Kensington|2768|
|8|Hilton London Metropole|2628|
|9|Millennium Gloucester Hotel London|2565|
|10|Intercontinental London The O2|2551|
|11|Park Grand Paddington Court|2288|
|12|Hilton London Wembley|2227|
|13|Park Plaza County Hall London|2223|
|14|Blakemore Hyde Park|2178|
|15|Park Plaza London Riverbank|2167|<br/>

* 호텔 위치 <br/>

![image](https://github.com/minsu0818/dataanalyze/assets/144076842/444aba63-7d37-43ad-a4eb-c4df88ebb01a)<br/>

* 겹친 지점 상세 보기 <br/>
![image](https://github.com/minsu0818/dataanalyze/assets/144076842/fcb15d38-846e-4797-a5aa-c32368c2c87a)<br/>

전체 400개의 호텔중 리뷰 수 상위 15개 호텔들의 리뷰수 총합이 대략 5만개로 학습을 진행하기 적합한 갯수여서 추출 데이터로 선정했다.




### 2.2 데이터 시각화 

* 평점에 따른 데이터 분포   
![image](https://github.com/minsu0818/dataanalyze/assets/144076842/bdf445ee-71e7-4072-8814-95cb09607a24)   
전체적인 평점 분포는 이렇게 되어있습니다. 하지만 가시성이 너무나 떨어지는 데이터이기 때문에 따로 기준을 잡아서 데이터를 수정하도록 하겠습니다.
  * 평점 데이터 수정   
    평점이 2.5점 보다 적은 것을 bad   
    평점이 2.5점과 5점 사이인 것을 average   
    평점이 5점과 7.5점 사이인 것을 good   
    평점이 7.5과 10점 사이인 것을 very good
             
    이렇게 설정을 하고 데이터를 다시 시각화 합니다

    ![image](https://github.com/minsu0818/dataanalyze/assets/144076842/4728e6bf-1818-4d6c-ad85-d40c4e4eca1e)

    수정 후 데이터를 봤을떄 평점이 2.5점이 이하인 호텔은 아예 없고 대부분의 호텔들이 평점들이 좋다는 사실을 알수 있었습니다

* 긍부정 단어 수의 차이에 따른 평점 분포

   ![image](https://github.com/minsu0818/dataanalyze/assets/144076842/6a49031e-1ec3-4d64-bd9c-f80b63a59cbc)<br/>
  * 긍정적 단어수에서 부정적인 단어수를 뺐을떄 x축의 값이 양수이면 긍정적인 단어를 포함한 리뷰가 많았던거고 음수이면 부정적인 단어를 포함한 리뷰였던것을  알 수 있었습니다. 그에 따른 평점 분포를 나타낸 그래프입니다<br/>
  
  * 긍부정 단어 수의 차이의 수와 평점간의 상관 계수는 0.45로 서로간의 관계가 별로 없다는 것을 알 수 있었다.
       
* 전체 문장의 단어수에 따른 평점 분포

  ![image](https://github.com/minsu0818/dataanalyze/assets/144076842/cd684b63-4151-486f-bf82-fd78cbc9f18c)<br/>
  * 각 문장의 전체의 단어수에 따른 평점 분포를 나타낸 그래프입니다<br/>

  * 각 문장의 전체의 단어수와 평점간의 상관계수는 -0.19로 서로 별로 영향을 주지 못하고 있다는 것을 알 수 있다.
  
     
  
* 호텔 기간별 평균 평점<br/>
  오래된 리뷰 평점: 호텔 리뷰의 데이터 중 가장 끝에 있는 데이터의 날짜에서부터 전체 데이터의 중간에 위치 하는 데이터까지의 전체 평균<br/>
  새로운 리뷰 평점: 호텔 리뷰의 데이터 중 가장 끝에 있는 데이터의 날짜에서부터 전체 데이터의 가장 처음에 위치 하는 데이터까지의 전체 평균<br/>
  평점 증가량 : 새로운 리뷰 평점 - 오래된 리뷰 평점<br/>


  
  ![image](https://github.com/minsu0818/dataanalyze/assets/144076842/3beee1b8-8527-4bf7-9aac-b263d46efb7d)<br/>
  그래프를 봤을떄 모든 호텔들이 기간이 길어져 전체 리뷰수가 늘어남에 따라 평점이 떨어졌다는 것을 볼수 있었다. 

## 3. 학습 데이터 구축

총 5만개의 데이터에는 데이터가 크기가 큰만큼 정말 다양한 평점들이 존재한다. 그렇기 때문에 확실하게 좋다,나쁘다와 같은 이분적인 평점들도 존재하지만 적당합니다,나쁘지 않습니다 와 같이 애매한 리뷰들도 존재합니다. 하지만 더 높은 학습률을 위해서는 애매한 리뷰들 보다는 이분적으로 확실하게 나뉘는 데이터들만 갖고 학습을 시키기로 했다. <br/>

### 3-1.전체 분석 데이터 
*negative 0 /positive 1* 
| | Negative_Review | Positive_Review | pn |
|-|-----------------|-----------------|----|
|1| The car park was small and unpleasant People ...  | The location was excellent for getting to the O2  |0|
|2| We weren t told that the only spa facility op... |  No Negative    | 0  |
|..|...|...|
|44557| The hotel and area around it was a building | location|  1  |<br/><br/>

![image](https://github.com/minsu0818/dataanalyze/assets/144076842/1e83633a-f602-4962-8291-b9af104bed2d)



positive 인 데이터들은 평점이 8.5이상인 리뷰들만 모은 것들이기 때문에 확실하게 좋다라고 말할 수 있다. 하지만 negative인 데이터들은 평점이 0점에서 부터 8.49까지들의 평점이기 때문에 범위가 너무 표괄적이여서 좋다 나쁘다라고 나누기가 애매하다. 그렇기 때문에 booking.com에서의 평점 기준을 참조하여 6.5이하인 데이터들만 negative 로 취급하고 나머지 중간에 끼어있는 데이터들은 삭제할 것이다.   

###  3.2 학습 데이터 구축<br/>

|| Negative_Review | Positive_Review | pn |
|-|-----------------|-----------------|----|
|1| Hot stuffy room air con not working properly ...   |  The bed was OK|0|
|2|  Construction going on all around the hotel so... | Could control the room temperature with the A...  | 0  |
|..|...|...|
|28038|No Negative| location and lovely staff |  1  |<br/>


![image](https://github.com/minsu0818/dataanalyze/assets/144076842/f891195a-5393-40c3-a712-0de846419649)<br/>


이 데이터 중 2000개만을 추출하여 학습 데이터를 구성할 것이다. 학습 데이터를 추출할때 2가지로 만들건데 첫번쨰 학습 데이터는 negative과 positive 을 5:5비율로 추출하고 두번쨰 학습 데이터는  0과 1의 비율을 원본 데이터의 비율 그대로 추출 할 것이다. 마지막으로 데이터에 긍정 리뷰와 부정 리뷰가 둘다 있기 때문에 둘을 negpos라는 새로운 열을 만들어 합칠 것이다. <br/><br/>


###  3.3 학습 데이터 <br/>


|| Negpos |   pn |
|-|-----------------|----|
|1|We travelled from Australia and spent a total... | 0|
|2|  We were put in a room at the back of the hote...|1|
|...|...|...|
|2000| No Negative  Great staff and a great location ...|1|<br/>





![image](https://github.com/minsu0818/dataanalyze/assets/144076842/d97098a7-daa6-4dff-8a20-5b7a89613c92)
![image](https://github.com/minsu0818/dataanalyze/assets/144076842/c65bf2c9-8862-436f-bf91-19017e67911e)



## 4. MobileBert 학습 결과
### 개발환경

<img src="https://img.shields.io/badge/python-%233776AB.svg?&style=for-the-badge&logo=python&logoColor=white" /><img src="https://img.shields.io/badge/pycharm-%23000000.svg?&style=for-the-badge&logo=pycharm&logoColor=white" />

### 패키지

<img src="https://img.shields.io/badge/pandas-%23150458.svg?&style=for-the-badge&logo=pandas&logoColor=white" /><img src="https://img.shields.io/badge/pytorch-%23EE4C2C.svg?&style=for-the-badge&logo=pytorch&logoColor=white" /><img src="https://img.shields.io/badge/tensorflow-%23FF6F00.svg?&style=for-the-badge&logo=tensorflow&logoColor=white" /><img src="https://img.shields.io/badge/numpy-%23013243.svg?&style=for-the-badge&logo=numpy&logoColor=white" />



  
### 4.1 학습 결과
| 첫 번째 학습 데이터| epoch | 0  | 1  | 2  | 3  |두 번째 학습 데이터| epoch | 0  | 1  | 2  | 3  |
|-----|-----|------|-----|------|-----|-----|------|-----|------|-----|-----|
|training loss | 138952.63 | 0.32 | 0.93 | 0.2 | | training loss |25534.58|22.52| 7.9 | 1.91| 
|Validation accuracy | 0.895 | 0.89 | 0.905 | 0.9 | | Validation accuracy | 0.775 | 0.815 |0.885 | 0.865 | <br/>

![image](https://github.com/minsu0818/dataanalyze/assets/144076842/fe2dac4d-a065-4560-9cd7-e8a10c605259)<br/>
 첫번째 모델의 training loss가 처음에는 138952.63이라는 말도 안되는 수치를 보여줬지만 점차 감소하여 0.2까지 떨어져 안정적인 수치를 보여줬고, Validation accuracy는 처음부터 0.895라는 안정적인 수치로 시작하여 큰 변화 없이 0.9로 마무리하여 큰 고저 없이 안정적인 성능을 보여줬다. 두번째 모델의 training loss도 처음에는 25534.58이라는 비장적으로 높은 수치로 시작하여 1.91이라는 낮은 수치까지 감소했고, Validation accuracy는 처음부터 0.775이라는 비교적 낮은 수치로 시작했지만 0.865라는 나름 만족스러운 수치를 기록했다. 결과적으로 두 모델 다 긍부정을 분류하는데 성공했지만 학습 단계에세는 첫번째 모델이 두번쩨 모델보다 성능적으로 살짝 우세하다는 결론이 나왔다.

### 4.2 학습 모델을 원본 데이터에 적용한 결과값<br/>
![image](https://github.com/minsu0818/dataanalyze/assets/144076842/5266a16c-1ff5-4907-9a56-fe8df8178a08)![image](https://github.com/minsu0818/dataanalyze/assets/144076842/f049e815-3f2f-4a8f-8762-9eebb464ecb5)<br/>

첫번째 모델의 결과값이 0.88로 높게 나왔고 두번째 모델의 결과값도 0.86으로 높게 나왔지만 둘의 성능을 비교해봤을때 첫번째 모델이 더 잘 나왔다.


## 5. 결론 및 배운 점<br/>
두 모델에 대해 데이터 자체의 특수성 때문에 모델의 정확도를 학습하는 단계에서 조차 난항을 겪었다. 다른 데이터들과 달리 긍정 및 부정 평가가 따로 나누어져 있어 이를 하나로 합쳤는데, 그 영향으로 인해 결과값이 0.5에서 0.95까지 다양하게 나오는 현상이 발생했다. 이를 통해 데이터 전처리의 중요성을 다시 한번 실감하게 되었다.<br/>

또한, 학습 데이터의 비율이 학습부터 검증까지 많은 영향을 미친다는 것을 깨달았다. 첫 번째 모델은 긍정과 부정의 비율을 1:1로 잡고 학습을 진행하여, 모델의 학습 단계부터 안정적인 Validation Accuracy를 보여주었다. 반면, 불균형한 데이터 비율로 학습을 시킨 두 번째 모델은 초기에는 Validation Accuracy가 낮았지만 마지막에는 향상되는 경향을 보이며 다소 불안정한 모습을 나타났다. 마지막 검증 단계에서도 첫 번째 모델이 두 번째 모델보다 더 높은 정확도를 보여주었다. 더불어 이 과정에서 과적합과 저적합 현상을 확인할 수 있었다. 첫 번째 모델은 균형 잡힌 데이터 비율 덕분에 학습 단계에서부터 검증 단계까지 비교적 일관된 성능을 보여주었고, 과적합을 피할 수 있었다. 반면, 두 번째 모델은 불균형한 데이터로 인해 초기에는 저적합 상태였지만, 학습이 진행됨에 따라 모델이 점차 데이터를 잘 반영하게 되어 마지막에는 Validation Accuracy가 개선되었다. <br/>

이 프로젝트를 통해 데이터 전처리시 더 많은 기준을 세워 더 세분하게 구분하고 데이터 학습과 검정의 과정을 더 많이 거쳐 최적의 비율을 찾아내야하겠다는 것을 느꼈다.














  









