# 성장중인 개발자 임혜지의 포트폴리오입니다.
- ### 희망 분야 : 인공지능 / java back-end
- ### 이메일 : cisl159@naver.com
- ### 깃허브 : https://github.com/Limmaji/Limmaji
</br>

# 기술 스택
> ### Back-end
> Java / JSP / Spring Boot / JPA
> ### Front-end
> HTML / CSS / React.js
> ### DB
> MySQL / Oracle

</br>

# 개인 스터디
### 1. [코랩](https://github.com/Limmaji/study_model)
### 2. [AWS]()
</br>

# 프로젝트

<a href="https://github.com/KIMGUUNI/A_EyeF/">
        <img src="https://github.com/Limmaji/hyeji/assets/118683437/dd5ab833-a1d1-4136-8821-d3df838b5cd2" width = "80%">
 </a>
 
 ## 🖥 A-EYE 🖥

> #### 🏆 수상 : 우수상
> 
> ### Team name : A_eye
> 
> #### AI 기반 CCTV 분석을 통한 맞춤 광고 재공 서비스
> 
> #### 역할 : Modeling
>
> #### 기간 : 2024년 02월 01일 ~ 02월 27일 (27일 소요)
>
> #### [프로젝트 보기](https://github.com/KIMGUUNI/A_EyeF/)
 <details>
 <summary><b>팀 프로젝트</b></summary>
 
</br>

## 1. 사용 기술
#### `Back-end`
  - Spring
  - JQuery
  - JavaScript
  - Oracle
  - AWS

#### `Front-end`
  - React
  - HTML
  - CSS
  - JavaScript
  - mui

</br>


## 2. ERD 설계
![image](https://github.com/KIMGUUNI/A_EyeF/assets/142488051/d002a731-40eb-4bec-9337-c2773b836a6a)

</br>


## 3. 담당 기능
 - 나이 예측 모델 구현
 - 성별 예측 모델 구현
 - 로그아웃 기능 구현
 - 모델 연결
 - Queue 전송

   </br>
   
[코드보러가기](https://github.com/Limmaji/age_gender_model)
   
</br>
</br>


## 4. 데이터 수집 및 전처리

### 4.1. 데이터 수집

> #### [AI-hub 안면 인식 에이징 데이터](https://www.aihub.or.kr/aihubdata/data/view.do?currMenu=&topMenu=&aihubDataSe=data&dataSetSn=71415)
>
> 원천 데이터	
> - 참여자 1인당 50장의 유아~현재까지의 연령대별 사진
> - 초상권 사용 동의를 얻는 참여자 안면 제외하고 사진 블러링 처리 (PNG) 50,250장
>
>  라벨링 데이터
> - 참여자 출생년도, 현재 나이, 사진 상 나이, 원본 사진 촬영 기기 등의 수집 메타 정보
> - 얼굴 바운딩박스 및 5점 키포인트 가공 정보 (JSON) 50,250개

</br>

- #### 데이터 분포

<table>
  <tr>
    <td align="center">남자</td>
    <td align="center">여자</td>
  </tr>
  <tr>
    <td align="center"><strong>44.6%</strong></td>
    <td align="center"><strong>55.4%</strong></td>
  </tr>
  <tr>
    <td align="center"><b>22,400건</b></td>
    <td align="center"><b>27,850건</b></td>
  </tr>
</table>

 <table>
  <tr>
    <td align="center">20~35세</td>
    <td align="center">36~49세</td>
    <td align="center">50세 이상</td>
  </tr>
  <tr>
    <td align="center"><strong>41.6%</strong></td>
    <td align="center"><strong>35.7%</strong></td>
    <td align="center"><strong>22.7%</strong></td>
  </tr>
  <tr>
    <td align="center"><b>20,900건</b></td>
    <td align="center"><b>17,950건</b></td>
    <td align="center"><b>11,400건</b></td>
  </tr>
</table>

</br>



### 4.2. 데이터 전처리

![image](https://github.com/Limmaji/hyeji/assets/118683437/81acfc4f-affb-498e-8aa2-594da258fdf3)

- 압축 풀기
- 라벨링 데이터 추출
- 데이터 프레임으로 변환
- 얼굴 영역 추출
- 라벨 데이터 원핫인코딩
- 이미지 0~1 사이값으로 정규화

  </br>
  </br>

#### 4.2.1. 압축 풀기

</br>


#### 4.2.2. 라벨링 데이터 특성 추출
  - age_past
  - imgsize
  - width
  - height
  - gender
  - annotation (box, x, y, w, h)

</br>

> - 15 ~ 59세의 나이 데이터만 추출
> (데이터 특성상 1 ~ 19세의 데이터가 많아, 소비력이 있다고 판단되는 15세부터 데이터를 추출하였다.)
>
> - 10대 ~ 50대로 범주화


</br>

#### 4.2.3. 이미지에서 얼굴 영역만 추출

- 배열로 저장


</br>

#### 4.2.4. 나이 데이터 범주화

- 원 핫 인코딩 진행
- 0~1 값으로 정규화 진행 

</br>

### 4.3. 전처리 후 데이터 분포

- 50,250개 => 25,125개

##### train data 분포 예시
![image](https://github.com/Limmaji/hyeji/assets/118683437/d5556432-6c40-41ad-a668-d777638e2476)

</br>

### 5. Modeling
#### 정확도
- age_model : 48.1%

- gender_model : 85.4%


</br>



### 5.5. 모델 예측결과

![image](https://github.com/Limmaji/hyeji/assets/118683437/f6589a48-3790-4108-b18f-69fe09192595)

</br>


## 6. 모델 연결하기

- dlib 라이브러리를 활용하여 얼굴을 감지
- 얼굴 감지 시, 나이, 성별 예측 모델 실행



- 실행결과
   
![image](https://github.com/Limmaji/hyeji/assets/118683437/fe1e80e3-3cfc-4ea0-8442-3b63a3421a4a)

</br>

## 7. Queue 전송

- SQS Message Body에 예측 값을 담아 전송해주기
- 중복 피하기하기 위한 로직 구현
- 우선순위 부여

> - 한번 전송된 객체는 우선순위에서 제외 되어야 한다. </br>
> - 인식된 객체들의 값을 우선순서대로 크기가 15인 배열A에 저장한다. </br>
> - 가장 첫번째 값을 전송하고 전송된 값은 크기가 3인 배열B에 저장한다. </br>
> - 새롭게 객체가 인식되고 배열에 저장되었을때, 배열B와 값을 비교하여 다른 값을 가질 때, 값을 전송하고 배열B에 넣어준다. </br>
> - 만약 배열A에 배열B와 다른 값을 가지는 값이 없을 때, 배열B를 초기화 하고, 배열A의 첫번째 값을 보내준 후 배열B에 저장한다. </br>


 </details>


</br>
</br>
</br>




<a href="https://github.com/2023-SMHRD-IS-CLOUD-1/StrongRepo">
        <img src="https://github.com/Limmaji/hyeji/assets/118683437/36549b89-cf1d-493c-95db-2c7824672f35" width = "80%">
        
        
 </a>
 
 ## 💊 Drug is Death 💊

> ### Team name : 강력 1팀
>
> #### openCV기반 10대 청소년 대상 마약 예방 교육 자료 및 체험 서비스
>
> #### 역할 : 대시보드 구현, 로그인 및 회원가입 기능 구현
>
> #### 기간 : 2023년 11월 22일 ~ 12월 07일 (21일 소요)
>
> #### [프로젝트 보기](https://github.com/2023-SMHRD-IS-CLOUD-1/StrongRepo)
 <details>
 <summary><b>팀 프로젝트</b></summary>
 
</br>

## 1. 사용 기술
#### `Back-end`
  - Java 8
  - JQuery
  - JavaScript
  - Oracle
  - Chart.js
  - BootStrap
  - JSP

#### `Front-end`
  - HTML
  - CSS
  - JavaScript

</br>


## 2. ERD 설계
![image](https://github.com/Limmaji/hyeji/assets/118683437/bafbbaf7-6c10-4ab6-b4f5-752477a2b423)

</br>


## 3. 담당 기능
### MVC 패턴을 이용한 대시보드 구현
   - 연령 / 지역 / 년도별 통계에 따른 대시보드 구현

![image](https://github.com/Limmaji/hyeji/assets/118683437/6057f0be-d2de-4f87-a764-897ec171a420)
 
</br>

### 3.1. 전체 흐름

>  #### 1. JQuery 사용, AJAX를 통해 요청 수행
>  #### 2. DB에서 데이터 받아오기
>  #### 3. 차트 생성
> 
> ####  - [main.jsp](Strong1team2/src/main/webapp/WEB-INF/main.jsp) </br>
> ####  - [FrontController.java](Strong1team2/src/main/java/com/smhrd/frontcontroller/FrontController.java) </br>
> ####  - [DashBoardService.java](Strong1team2/src/main/java/com/smhrd/controller/DashBoardService.java) </br>
> ####  - [DashBoardMemberVO.java](Strong1team2/src/main/java/com/smhrd/model/DashBoardMemberVO.java) </br>
> ####  - [DashBoardDAO.java](Strong1team2/src/main/java/com/smhrd/model/DashBoardDAO.java) </br>
> ####  - [DashMemberMapper.xml](Strong1team2/src/main/java/com/smhrd/database/DashMemberMapper.xml) </br>

</br>


### 3.1. Chart

#### 1. Bar-Chart : 연령별 단속 현황

<details>
        
<summary><b>dashboard1.min.js</b></summary>
<div markdown="1">                

       
~~~java

$.ajax({

		// 데이터 요청 주소
		url: "DashBoard.do",

		// 데이터 타입
		dataType: "json",

		// 성공
		success: function(result) {
			console.log("성공");
			console.log("데이터: ", result); // 객체 전체를 출력해 보기 위해 콤마(,)를 사용하여 분리
			console.log("DashBoard.jsp2");

			// 서버로부터 받은 JSON 객체의 c_year와 c_num 속성값 출력
			if (result) {
				new Chartist.Bar(".net-income",
					{
						labels: ["0-19세", "20대", "30대", "40대", "50대", "60세 이상"],
						series: [[result[48].a_num + result[49].a_num, result[47].a_num, result[46].a_num, result[45].a_num, result[44].a_num, result[43].a_num ]]
					},
					e, [["screen and (max-width: 640px)",
						{
							seriesBarDistance: 7,
							axisX: { labelInterpolationFnc: function(e) { return e[0] } }
						}]])

			} else {
				// 예외 처리: res가 비어있거나 유효한 데이터가 없는 경우
				console.log("데이터가 없거나 형식이 맞지 않습니다.");
			}
		},
		error: function(xhr, status, error) {
			console.log("Error: ", error); // 에러 로그 출력
			console.log("DashBoard.jsp3");
		}
	});
~~~
</details>



#### 2. Polar-Chart : 직업별 단속 현황
<details>
        
<summary><b>chartjs.init2.js</b></summary>
<div markdown="1">        

~~~java

$(function() {
	"use strict";

	$.ajax({

		// 데이터 요청 주소
		url: "DashBoard.do",

		// 데이터 타입
		dataType: "json",

		// 성공
		success: function(result) {
			console.log("성공");
			console.log("데이터: ", result); // 객체 전체를 출력해 보기 위해 콤마(,)를 사용하여 분리
			console.log("DashBoard.jsp2");

			// 서버로부터 받은 JSON 객체의 c_year와 c_num 속성값 출력
			if (result) {

				//Polar Chart
				new Chart(document.getElementById("polar-chart"), {
					type: 'polarArea',
					data: {
						labels: [result[26].j_job_group, result[27].j_job_group, result[29].j_job_group, result[28].j_job_group + result[39].j_job_group, result[30].j_job_group, result[33].j_job_group, result[32].j_job_group, result[31].j_job_group, result[34].j_job_group],
						datasets: [
							{

								label: "Population (millions)",
								backgroundColor: ["#aee5ed", "#74c8d5", "#45a9b9", "#56abb9", "#60969f", "#3c838f", "#106e7d","#026777", "#00525f"],
								data: [result[26].j_num, result[27].j_num, result[29].j_num, result[28].j_num + result[39].j_num, result[30].j_num, result[33].j_num, result[32].j_num, result[31].j_num, result[34].j_num]
							}





						]
					},
					options: {
						title: {
							display: true,
							text: '마약사범 직업'
						}
					}
				});
			} else {
				// 예외 처리: res가 비어있거나 유효한 데이터가 없는 경우
				console.log("데이터가 없거나 형식이 맞지 않습니다.");
			}
		},
		// 예외
		error: function(xhr, status, error) {
			console.log("Error: ", error); // 에러 로그 출력
			console.log("DashBoard.jsp3");
		}
	});


}); 
~~~
</details>


#### 3. Morris-Line-Chart : 단속 현황

<details>

<summary><b>morris-data2.js</b></summary>
<div markdown="1">  
        
~~~java

        $(function() {
	"use strict";

	$.ajax({

		// 데이터 요청 주소
		url: "DashBoard.do",

		// 데이터 타입
		dataType: "json",

		// 성공
		success: function(result) {
			console.log("성공");
			console.log("데이터: ", result); // 객체 전체를 출력해 보기 위해 콤마(,)를 사용하여 분리
			console.log("DashBoard.jsp2");

			// 서버로부터 받은 JSON 객체의 c_year와 c_num 속성값 출력
			if (result) {
				var line = new Morris.Line({
					element: 'morris-line-chart',
					resize: true,
					data: [
						{ y: result[0].c_year.toString() , item1: result[0].c_num },
						{ y: result[1].c_year.toString(), item1: result[1].c_num },
						{ y: result[2].c_year.toString(), item1: result[2].c_num },
						{ y: result[3].c_year.toString(), item1: result[3].c_num },
						{ y: result[4].c_year.toString(), item1: result[4].c_num },
						{ y: result[5].c_year.toString(), item1: result[5].c_num },
						{ y: result[6].c_year.toString(), item1: result[6].c_num },
						{ y: result[7].c_year.toString(), item1: result[7].c_num },
						{ y: result[8].c_year.toString(), item1: result[8].c_num },
						{ y: result[9].c_year.toString(), item1: result[9].c_num }

					],
					xkey: 'y',
					ykeys: ['item1'],
					labels: ['Item 1'],
					gridLineColor: '#eef0f2',
					lineColors: ['#e4281c'],
					lineWidth: 3,
					hideHover: 'auto'


				});

			} else {
				// 예외 처리: res가 비어있거나 유효한 데이터가 없는 경우
				console.log("데이터가 없거나 형식이 맞지 않습니다.");
			}
		},
		error: function(xhr, status, error) {
			console.log("Error: ", error); // 에러 로그 출력
			console.log("DashBoard.jsp3");
		}
	});
});
~~~


</details>
</br>

## 4. 회고 / 느낀점


</br>

> 웹 페이지를 개발하는 프로젝트를 처음 진행하면서 배운 점도 아쉬운 점도 많았습니다.</br>
> 수업 시간에 배운 코드를 직접 작성하고 기능들을 구현하는 과정에서 평소 어렵게 느꼈던 JQuery 문법에 대해 더욱 자세히 알게 되었습니다.</br>
> 또한 AJAX(비동기 통신)로 대시보드에 필요한 데이터를 직접 받아오는 과정에서 눈에 보이지 않는 데이터의 흐름에 대해 이해도가 높아졌습니다.</br>
> 개발은 약 2주간의 짧은 시간이었지만 저의 능력을 확인하고 더욱 열심히 하게 되는 계기가 되었습니다.</br>
> 프로젝트를 진행하며 아쉬웠던 점은 첫 번째로 적절한 팀 회의를 진행하지 못한 것입니다.</br>
> 저희 팀은 프로젝트 초반 적절한 회의가 이루어지지 않아 개발 단계에서 버려지는 하루가 생기기도 했습니다.</br>
> 여섯명의 다양한 아이디어를 합쳐 더 좋은 웹페이지를 개발할 수 있는 기회였지만, 회의를 통해 풀어나갈 수 없었던 점이 아쉬웠습니다.</br>
> 두 번째는 주제에 대한 아쉬움이었습니다.</br>
> 저희 팀은 사업성 아닌 공공성을 목표로 교육용 웹 페이지를 개발했습니다. 기존의 교육용 페이지와 차별성을 두기 위해 다양한 체험 기능을 추가하고자 했습니다.</br>
> 하지만 기획 단계에서 아이디어가 나오지 않았고, 웹 페이지의 볼륨이 작아졌습니다.</br>
> 차별성 있는 좋은 주제라도 기획 단계에서 구현할 수 있는 기능들이 얼마 되지 않을 때 주제를 바꾸는 것도 하나의 방법이라는 것을 배웠습니다.</br>

</details>



