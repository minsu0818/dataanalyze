런던에 있는 호텔 리뷰에 관한 mobilebert에 관한 긍부정 예측

## [목차]
[1. 개 요 ](#1  개-요)

[2. 대표적인_인공지능_API ](#2-대표적인-인공지능-api)
  - [2.1 구글](#21-구글)
  - [2.2 네이버](#22-네이버)

[3. 프로젝트_1에서 활용해 볼 API 선정 및 설명]()
---

## 1. 개 요

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

이렇듯 호텔 리뷰는 호텔을 예약하는 사람으로 하여금 좋은 참고 자료가 되어준다는 것을 알 수 있습니다.

* a
  * aa
    * aaa
- b
  - bb
    - bbb
+ c
  + cc
    + ccc
----
* 스타일 적용하기
* **진하게**(`Ctrl + B`)
* _기울이기_ (`Ctrl + ㅑ`)
* <s>취소선</s> 
* <u>밑줄</u> 밑줄

>인용문
<details><summary>접고펴기
</summary>
내용작성하기
</details>  

---

### 소스코드 넣기
```
int main ()
{
  float a = 1.0;
  float b = 2.0;
  c = a+b;
  printf("%d\n",c);
}
```
```python
sum = 0
for j in range(10):
    sum += j
 print("sum from 1 to 10: ", sum)
```
```python
import pandas as pd
file_path = "Enter your file path"
df=pd.reead_csv(file_path)
df.head()
```
----

## 2. 대표적인 인공지능 API

### 2.1 구글
* [Vision API](https://cloud.google.com/vision?hl=ko)
![vision_api_logo](https://community.appinventor.mit.edu/uploads/default/optimized/3X/2/a/2ad031bc25a55c4d3f55ff5ead8b2de63cdf28bf_2_200x178.png)

* 이미지 크기 조정 불가
<p align="center">
<img src = "./vision_api_logo.png" width = "200"/>
</p>


* Vision API는 특정 이미지를 인식하여 분류기능을 하는 인공지능 API이다.
----

### 2.2 네이버

---

### 3. 프로젝트_1에서 활용해 볼 API 선정 및 설명
