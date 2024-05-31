 ![80632473_1713854345213_37_600x600](https://github.com/minsu0818/dataanalyze/assets/144076842/e8006771-f4f5-4537-ad4d-dea8d1f3208f)

## MobileBERT를 활용한 런던의 호텔 리뷰 분석

## [목차]
# [1. 서론]
 - [1.1 호텔 리뷰가 미치는 영향 ]
 - [1.2 런던을 선택한 이유  ]

# [2. 데이터 들여다 보기]
 - [2.1 사용할 데이터]
 - [2.2 데이터 구성 ]
 - [2.3 데이터 시각화  ]
---

## 1. 서론

### 1.1 호텔 리뷰가 미치는 영향 

Positive reviews have a significant impact on consumer hotel booking decisions.
Consistent with the findings of the majority of researchers, positive online reviews contribute to
establishing a positive brand image for hotels. Positive user reputation leads to word-of-mouth
promotion and positively influences consumer decision-making. More positive reviews also attract
56 more new customers to book the hotel, resulting in a long-term and stable increase in hotel bookings.  
--Research on the Impact of Online Reviews on Hotel Selection for Travelers 논문에서 일부 발췌

긍정적인 리뷰는 호텔의 긍적적인 브랜드 이미지와 관련이 되며 호텔 부킹에도 긍정적인 영향을 끼친다고 합니다.
실제로 이 논문의 연구에서도 많은 긍정적인 리뷰가 있는 호텔은 56명의 신규 손님이 생겼다고 했습니다

Negative reviews have a significant impact on consumer hotel booking decisions.
Negative online reviews have a substantial impact on consumer hotel booking decisions. Due to
the inherent risks in the hotel booking process, consumers extensively browse online reviews before
making their bookings. Excessive negative information about a hotel has a serious influence on
consumer decisions. An excessive number of negative reviews also leads to a decline in overall
consumer ratings of the hotel, ultimately causing a severe negative impact on hotel bookings. 
--Research on the Impact of Online Reviews on Hotel Selection for Travelers 논문에서 일부 발췌

부정적인 리뷰는 손님의 호텔 결정에 안 좋은 영향을 끼친다고 합니다. 
손님들은 대게 호텔에 관한 리스크를 줄이기 위해 호텔에 관한 사전조사를 진행합니다. 
이때 대표적으로 살펴보는 것이 호텔 리뷰인데 부정적인 리뷰는 부킹을 하는 사람으로 하여금 그 호텔을 기피하게 하여 호텔의 손님이 줄어들게 한다고 합니다

이렇듯 호텔 리뷰는 호텔을 예약하는 사람에게 많은 영향을 준다는 것을 알 수 있습니다.

### 1.2 런던을 선택한 이유 

유로모니터 인터내셔널이 발표한 2023년 세계에서 가장 많이 방문한 도시는 순서대로 ▲튀르키예 이스탄불 ▲영국 런던 ▲아랍에미리트 두바이 ▲튀르키예 안탈리아 ▲프랑스 파리 ▲홍콩 ▲태국 방콕 ▲미국 뉴욕 ▲멕시코 칸쿤 ▲사우디아라비아 메카다.

출처 : 숙박매거진(https://www.sukbakmagazine.com)

런던 아이(London Eye )는 런던의 템스강 사우스뱅크에 있는 캔틸레버식 대관람차이다. 2000년 개관하여 밀레니엄 휠(Millennium Wheel )이라고도 부른다.[1] 영국의 에서 가장 인기 있는 유료 관광 명소로 매년 300만 명 이상이 방문한다.

출처: 위키백과 

이렇듯 런던은 런던아이와 버킹엄 궁전과 같은 세계적인 관광지로 이름 나있다고 합니다. 그렇기 때문에 년에 몇백, 몇천만명의 관광객들이 다녀가는 런던에 호텔이나 모텔과 같은 숙박업이 많이 발달을 한 도시입니다. 
숙박업이 많이 발달한 도시인 만큼 호텔도 많기 때문에  리뷰 또한 많습니다. 리뷰가 많다면 긍정적인 리뷰와 부정적인 리뷰가 둘다 많다는 건데 이것들이 호텔에 영향을 얼마나 끼치는 알아 보기 위해 저는 mobile bert를 이용한 런던 호텔 리뷰의 긍부정을 해볼까 합니다   
   
### 1.3 사용할 데이터
https://www.kaggle.com/datasets/jiashenliu/515k-hotel-reviews-data-in-europe?resource=download
사용할 데이터는 kaggle에 있는 515K Hotel Reviews Data in Europe의 데이터를 사용할 것입니다만 데이터의 수가 51만개로 너무나 방대하기 떄문에 5만개만 따로 추출하는 과정을 거칠 것입니다
   
![image](https://github.com/minsu0818/dataanalyze/assets/144076842/46104985-2120-42fa-a248-26abdccf52e1)<br/>

![image](https://github.com/minsu0818/dataanalyze/assets/144076842/4a86c89a-50d9-4432-a686-24538a9c68e3)<br/>





데이터를 불러와서 리뷰수가 더 많은 호텔의 리뷰에 관한 신뢰도가 높을 것 같아서, 호텔의 리뷰수의 갯수에 관해 내리차순으로 정렬하여 15개의 상위 항목만 뽑은 결과입니다. 여기 적혀있는 모든 호텔들은 런던에 존재하는 호텔들로 총 데이터의 갯수도 5만개로 적당합니다. 물론 위에 적혀있는 호텔들이 런던에 있는 호텔들의 전부는 아니지만 이 이상 호텔의 수를 늘리면 데이터의 갯수도 증가하기 떄문에 포기하고 위의 15개만의 호텔로 작업을 진행할 것입니다.

## 2. 데이터 들여다 보기

### 2.1 데이터 구성 
* 데이터 열 이름    
'Hotel_Address', 'Additional_Number_of_Scoring', 'Review_Date',
       'Average_Score', 'Hotel_Name', 'Reviewer_Nationality',
       'Negative_Review', 'Review_Total_Negative_Word_Counts',
       'Total_Number_of_Reviews', 'Positive_Review',
       'Review_Total_Positive_Word_Counts',
       'Total_Number_of_Reviews_Reviewer_Has_Given', 'Reviewer_Score', 'Tags',
       'days_since_review', 'lat', 'lng'

* 데이터 구조 예시
![image](https://github.com/minsu0818/dataanalyze/assets/144076842/e3d67665-5d29-4d9b-bfc6-d9beea8a2af7)
![image](https://github.com/minsu0818/dataanalyze/assets/144076842/1183677c-8a43-4fd7-b99c-2573106e3597)
![image](https://github.com/minsu0818/dataanalyze/assets/144076842/996bf40e-ef32-4cd5-bcab-038c8f6393a9)


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

   ![image](https://github.com/minsu0818/dataanalyze/assets/144076842/2ac0c15d-ee96-43a9-bad3-8175dbf6408c)

  *긍정적 단어수에서 부정적인 단어수를 뺐을떄 x축의 값이 양수이면 긍정적인 단어를 포함한 리뷰가 많았던거고 음수이면 부정적인 단어를 포함한 리뷰였던것을  알 수 있었습니다. 그에 따른 평점 분포를 나타낸 그래프입니다<br/>
     
  *그래프를 분석해 보면 일정 경향성을 갖추지 않고 마구잡이로 난잡하게 그려진 것을 볼 수 있는데 이러한 경우들은 긍정적인 단어를 포함한 리뷰가 많았지만 리뷰 평점은 부정적이었거나
   부정적인 단어를 포함한 리뷰가 많았지만 리뷰 평점은 긍정적이었던 경우라고 생각합니다.<br/>
   
* 전체 문장의 단어수에 따른 평점 분포

  ![image](https://github.com/minsu0818/dataanalyze/assets/144076842/d30a23e2-ccb9-463a-ac5c-cc2f91f227e6)


  *각 문장의 전체의 단어수에 따른 평점 분포를 나타낸 그래프입니다<br/>
     
  *그래프를 분석해 보면 일정 경향성을 갖추지 않고 마구잡이로 난잡하게 그려져 있는 것을 보아 문장의 길이는 리뷰의 평점과 아무런 연관이 없다는 것을 알 수 있습니다.<br/>
  
* 호텔 기간별 평균 평점<br/>
  오래된 리뷰 평점: 호텔 리뷰의 데이터 중 가장 끝에 있는 데이터의 날짜에서부터 전체 데이터의 중간에 위치 하는 데이터까지의 전체 평균<br/>
  새로운 리뷰 평점: 호텔 리뷰의 데이터 중 가장 끝에 있는 데이터의 날짜에서부터 전체 데이터의 가장 처음에 위치 하는 데이터까지의 전체 평균<br/>
  평점 증가량 : 새로운 리뷰 평점 - 오래된 리뷰 평점<br/>


  
  ![image](https://github.com/minsu0818/dataanalyze/assets/144076842/3beee1b8-8527-4bf7-9aac-b263d46efb7d)<br/>
  그래프를 봤을떄 모든 호텔들이 기간이 길어져 전체 리뷰수가 늘어남에 따라 평점이 떨어졌다는 것을 볼수 있었다. 

### 3. 학습 데이터 만들기

* 3.1 서론<br/>
총 5만개의 데이터에는 데이터가 크기가 큰만큼 정말 다양한 평점들이 존재한다. 그렇기 때문에 확실하게 좋다,나쁘다와 같은 이분적인 평점들도 존재하지만 적당합니다,나쁘지 않습니다. 와 같이 분 확실한 기준을 가진 데이터에서 학습 데이터를 추출하는 것이 더 좋은 학습률을 가지지 않을까


