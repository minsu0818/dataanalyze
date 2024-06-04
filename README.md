![80632473_1713854345213_37_600x600](https://github.com/minsu0818/dataanalyze/assets/144076842/e8006771-f4f5-4537-ad4d-dea8d1f3208f)

# MobileBERT를 활용한 런던의 호텔 리뷰 분석

## [목차]
### [1. 서론]
 - [1.1 호텔 리뷰가 미치는 영향 ]
 - [1.2 런던을 선택한 이유  ]

### [2. 데이터 들여다 보기]
 - [2.1 사용할 데이터]
 - [2.2 데이터 구성 ]
 - [2.3 데이터 시각화  ]
---

## 1. 서론

### 1.1 숙박 산업과 사용자의 리뷰

코로나 펜데믹으로 인해서 숙박 산업이 큰 타격을 입었다.<br/>

![image](https://github.com/minsu0818/dataanalyze/assets/144076842/559f5bb7-5355-4657-b2c2-e708898b809c)
> "코로나19 충격이 호텔산업에 미치는 영향 : 명동 소재 A 호텔 사례를 중심으로" 논문에서 일부 발췌<br/>

코로나가 터진 시점인 2020년 1월경부터 전년도 같은 달 대비 절반의 매출 실적도 내지 못했다는 것을 알 수 있다. <br/>
이렇듯 이 시기의 숙박산업은 말로 표현하지 못할 정도의 경기 침체를 겪고 있었다

그렇다면 코로나가 독감과 같은 4등급 질병으로 분류된 현재 2024년에도 숙박 산업이 침체됐을까? 

![image](https://github.com/minsu0818/dataanalyze/assets/144076842/1c7cded2-052b-412d-8222-af85171df911)
> 호스피탈리티 테크 기업 온다(ONDA)의 2023 상반기 호스피탈리티 데이터 & 트렌드 리포트

 위 리포트를 본다면 침체와는 거리가 먼 행보를 걷고 있다는 것을 알 수 있다.<br/>
 코로나가 종식된 2022년 부터는 숙박 산업이 회복세이 이르면서, 수요가 증가하는 전년도 대비 1.5 증가하는 현상을 보였고 다음 해인 2023년에는 26.8프로의 증가율로 꾸준한 상승을 곡선을 그리고 있다 . <br/>
 오히려 코로나라는 족쇄가 사람들의 마음에 여행이라는 두 단어를 더 강하게 각인시킨 계기가 된 것이 아닌가 라는 생각이 든다.<br/><br/>
 숙박 산업이 상승세를 타고 있는 현재 여행을 성공적으로 마치기 위해서는 어떠한 요소가이 중요할까?<br/>
 여행을 성공적으로 끝맺기 위해서는 개인의 성향에 맞는 숙소를 정는게 중요하다 생각한다. 그래서 사람들은 호텔에 대한 정보를 찾는데 트립어드바이저인 booking.com 경우에는 전세계 10만개 호텔에 100만개의 리뷰가 있다. 그렇기 때문에 사람들이 호텔 부킹하기 전에 참조를 많이한다.<br/>
 이러한 현상은 연구에서도 밝혀졌다.
 



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
주요국의 관광지가 호텔 사람들에게 매력이 많은 만큼 관광객들이 많을 것 같기 떄문에 주요국들 위주로만 할 것이다. 그중 런던을 선택했다<br/>

유로모니터 인터내셔널이 발표한 2023년 세계에서 가장 많이 방문한 도시는 순서대로 ▲튀르키예 이스탄불 ▲영국 런던 ▲아랍에미리트 두바이 ▲튀르키예 안탈리아 ▲프랑스 파리 ▲홍콩 ▲태국 방콕 ▲미국 뉴욕 ▲멕시코 칸쿤 ▲사우디아라비아 메카다.

출처 : 숙박매거진(https://www.sukbakmagazine.com)<br/>
런던이 2023년에 2위를 할 정도로 많은 관광객들이 많이 방문한 도시이다<br/>

런던은 오랜 역사를 자랑하는 박물관, 근엄함의 상징인 영국 왕실, 런던의 상징으로 통하는 이층버스와 튜브, 명품 뮤지컬과 축구 종주국이라는 타이틀 등 런던을 설명하는 키워드는 그야말로 무궁무진하다.<br/><br/>
그렇기 때문에 년에 몇백, 몇천만명의 관광객들이 다녀가는 런던에 호텔이나 모텔과 같은 숙박업이 많이 발달을 한 도시다. 
숙박업이 많이 발달한 도시인 만큼 호텔도 많기 때문에 리뷰 또한 많다. 리뷰가 많다면 긍정적인 리뷰와 부정적인 리뷰가 둘다 많다는 건데 이것들이 호텔에 영향을 얼마나 끼치는 알아 보기 위해 저는 mobile bert를 이용한 런던 호텔 리뷰의 긍부정을 해볼까 한다   <br/><br/>
 
## 2. 데이터 들여다 보기

* 데이터 구성 요소<br/><br/>
 이 데이터는 총 515737건이고 각 항목들은 이렇게 된다<br/><br/>
 
|Hotel_Address|Additional_Number_of_Scoring|Review_Date|Average_Score|Hotel_Name|Reviewer_Nationality|Negative_Review|Review_Total_Negative_Word_Counts|Total_Number_of_Reviews|Positive_Review|Review_Total_Positive_Word_Counts|Total_Number_of_Reviews_Reviewer_Has_Given|Reviewer_Score|Tags|days_since_review|lat|lng|
|--------------|-----------------------------|------------|--------------|----------|---------------------|----------------|------------------------------|----------------------|---------------|-------------------------------|------------------------------------------|--------------|-----|------------------|---|---|
|호텔 주소|추가 점수 수|리뷰 작성 날짜|평균 점수|호텔 이름|리뷰어 국적|부정적 리뷰 내용|부정적 리뷰 단어 수|리뷰 총 개수|긍정적 리뷰 내용|긍정적 리뷰 단어 수|리뷰어가 작성한 총 리뷰 수|리뷰어 점수|태그|리뷰 작성 후 경과 일수|위도|경도|<br/>



* 데이터 구조 예시


 |-|Hotel_Address|Additional_Number_of_Scoring|Review_Date|Average_Score|Hotel_Name|Reviewer_Nationality|Negative_Review|Review_Total_Negative_Word_Counts|Total_Number_of_Reviews|Positive_Review|Review_Total_Positive_Word_Counts|Total_Number_of_Reviews_Reviewer_Has_Given|Reviewer_Score|Tags|days_since_review|lat|lng|
|-|--------------|-----------------------------|------------|--------------|----------|---------------------|----------------|------------------------------|----------------------|---------------|-------------------------------|------------------------------------------|--------------|-----|------------------|---|---|
|1|s Gravesandestraat 55 Oost 1092 AA Amsterdam Netherlands|194|8/3/2017|7.7|Hotel Arena|Russia|"I am so angry that i made this post available..."|397|1403|"Only the park outside of the hotel was beautiful"|11|7|2.9|[' Leisure trip ', ' Couple ', ' Duplex Double Room ', ' Stayed 3 nights ']|0 days|52.360576|4.915968|
|2|s Gravesandestraat 55 Oost 1092 AA Amsterdam Netherlands|194|8/3/2017|7.7|Hotel Arena|Ireland|"No Negative"|0|1403|"No real complaints the hotel was great great ..."|105|7|7.5|[' Leisure trip ', ' Couple ', ' Duplex Double Room ', ' Stayed 3 nights ']|0 days|52.360576|4.915968|
|3|s Gravesandestraat 55 Oost 1092 AA Amsterdam Netherlands|194|7/31/2017|7.7|Hotel Arena|Australia|"Rooms are nice but for elderly a bit difficult..."|42|1403|"Location was good and staff were ok It is cut..."|21|9|7.1|[' Leisure trip ', ' Family with young children ', ' Duplex Double Room ', ' Stayed 3 nights ']|3 days|52.360576|4.915968|
|..|...|...|.....|.....|...|...|..|
|515737|Wurzbachgasse 21 15 Rudolfsheim F nfhaus 1150 Vienna|168|8/9/2015	|8.1|Atlantis Hotel Vienna	|Hungary|"I was in 3rd floor It didn t work Free Wife	"|13|2823|"staff was very kind"|6|1|8.3|[' Leisure trip ', ' Family with young children ', ' 2 rooms ', ' Stayed 2 nights ']|725 days|48.203745|16.335677|
<br/>

도시별로 호텔의 분포<br/><br/>
![image](https://github.com/minsu0818/dataanalyze/assets/144076842/087e9519-7ed1-48f8-8507-13c5f2db6ebb)


### 2.1 추출 데이터<br/>

도시별 호텔 데이터에서 런던을 추출하여 분석을 진행하고 자한다.<br/><br/>

런던에 있는 호텔의 총 갯수<br/>
| City   | Unique Hotels |
|--------|---------------|
| London | 400           |<br/>

런던의 호텔의 리뷰 수 상위 15개만 추출 
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

위 15개의 호텔들의 리뷰수 총합이 대략 5만개로 학습을 진행하기 적합한 갯수이다. <br/><br/>

호텔 위치 <br/>
![image](https://github.com/minsu0818/dataanalyze/assets/144076842/444aba63-7d37-43ad-a4eb-c4df88ebb01a)<br/>
겹친 지점 상세 보기 <br/>
![image](https://github.com/minsu0818/dataanalyze/assets/144076842/fcb15d38-846e-4797-a5aa-c32368c2c87a)




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

### 3. 학습 데이터 만들기

* 3.1 서론<br/>
총 5만개의 데이터에는 데이터가 크기가 큰만큼 정말 다양한 평점들이 존재한다. 그렇기 때문에 확실하게 좋다,나쁘다와 같은 이분적인 평점들도 존재하지만 적당합니다,나쁘지 않습니다 와 같이 애매한 리뷰들도 존재합니다. 하지만 더 높은 학습률을 위해서는 애매한 리뷰들 보다는 이분적으로 확실하게 나뉘는 데이터들만 갖고 학습을 시키기로 했다. 

| Negative_Review | Positive_Review | pn |
|-----------------|-----------------|----|
| The car park was small and unpleasant People ...  | The location was excellent for getting to the O2  |0|
| We weren t told that the only spa facility op... |  No Negative    | 0  |
|..|...|...|
| The hotel and area around it was a building | location|  1  |<br/>

데이터의 갯수는 44557개이다.<br/><br/>
이때 pn은 평점이 8.5 이상이면 1 아니면 모두 0으로 라벨링한 것이다.<br/>

0과 1의 갯수<br/>
| pn   | 갯수 |
|----|----|
| 0  | 22936|
| 1  | 21621|<br/>

pn이 1인 데이터들은 평점이 8.5이상인 리뷰들만 모은 것들이기 때문에 확실하게 좋다라고 말할 수 있다. 하지만 pn이 0인 데이터들은 평점이 0점에서 부터 8.49까지들의 평점이기 때문에 범위가 너무 표괄적이여서 좋다 나쁘다라고 나누기가 애매하다. 그렇기 때문에 booking.com에서 평점 기준을 참조하여 6.5이하인 데이터들만 0으로 취급하고 나머지 중간에 끼어있는 데이터들은 날릴 것이다.   

* 3.2 학습 데이터 구축<br/>

| Negative_Review | Positive_Review | pn |
|-----------------|-----------------|----|
| Hot stuffy room air con not working properly ...   |  The bed was OK|0|
|  Construction going on all around the hotel so... | Could control the room temperature with the A...  | 0  |
|..|...|...|
|No Negative| location and lovely staff |  1  |<br/>
데이터의 갯수는 28038개이다.<br/><br/>  


0과 1의 갯수<br/>
| pn   | 갯수 |
|----|----|
| 0  | 22936|
| 1  | 6417|<br/>

pn을 새로운 기준을 새워 다시 라벨링을 해보니 데이터의 갯수가 대략 16000개가 줄었다.<br/>
이를 통해 애매한 데이터들의 갯수가 16000개씩이나 있었다는 것을 알 수 있었다.<br/><br/>

이 데이터 중 2000개만을 추출하여 학습 데이터를 구성할 것이다.<br/>
학습 데이터를 추출할때 2가지로 만들건데 첫번쨰 학습 데이터는 0과1을 5:5비율로 추출하고 두번쨰 학습 데이터는  0과 1의 비율을 원본 데이터의 비율 그대로 추출 할 것이다.<br/>
데이터에 긍정 리뷰와 부정 리뷰가 둘다 있기 때문에 둘을 negpos라는 새로운 열을 만들어 합칠 것이다. <br/><br/>


* 학습 데이터<br/>


| Negpos |   pn |
|-----------------|----|
|We travelled from Australia and spent a total... | 0|
|  We were put in a room at the back of the hote...|1|
|..|...|...|
| No Negative  Great staff and a great location ...|1|<br/>



데이터의 갯수는 2000개이다.<br/><br/>

첫 번째 데이터 <br/>
| pn   | 갯수 |
|----|----|
| 0  | 1000|
| 1  | 1000|<br/><br/>

두 번째 데이터 <br/>
| pn   | 갯수 |
|----|----|
| 0  | 1540|
| 1  | 460|<br/><br/>





  







  









