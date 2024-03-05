<a href="https://github.com/2023-SMHRD-IS-CLOUD-1/StrongRepo">
        <img src="https://github.com/Limmaji/hyeji/assets/118683437/36549b89-cf1d-493c-95db-2c7824672f35" width = "80%">
        
        
 </a>
 
 # 💊 Drug is Death 💊

> ### Team name : 강력 1팀
>
> #### openCV기반 10대 청소년 대상 마약 예방 교육 자료 및 체험 서비스
>
> #### 역할 : 대시보드 구현, 로그인 및 회원가입 기능 구현
>
> #### 기간 : 2023년 11월 22일 ~ 12월 07일 (21일 소요)
> 

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


## 3. 주요 기능

</br>

<b>대시보드</b>
<b>연령 / 지역 / 년도별 통계 대시보드 구현</b>

![image](https://github.com/Limmaji/hyeji/assets/118683437/6057f0be-d2de-4f87-a764-897ec171a420)
 
</br>

### 3.1. 전체 흐름

</br>

>  1. JQuery 사용, AJAX를 통해 요청 수행
>  2. DB에서 데이터 받아오기
>  3. 차트 생성
> 
> ####  [main.jsp](Strong1team2/src/main/webapp/WEB-INF/main.jsp) </br>
> ####  [FrontController.java](Strong1team2/src/main/java/com/smhrd/frontcontroller/FrontController.java) </br>
> ####  [DashBoardService.java](Strong1team2/src/main/java/com/smhrd/controller/DashBoardService.java) </br>
> ####  [DashBoardMemberVO.java](Strong1team2/src/main/java/com/smhrd/model/DashBoardMemberVO.java) </br>
> ####  [DashBoardDAO.java](Strong1team2/src/main/java/com/smhrd/model/DashBoardDAO.java) </br>
> ####  [DashMemberMapper.xml](Strong1team2/src/main/java/com/smhrd/database/DashMemberMapper.xml) </br>

</br>

### 3.2. Controller

</br>

<summary><b>Controller</b></summary>

<div markdown="1">

~~~java
@WebServlet("*.do")
public class FrontController extends HttpServlet {
	private static final long serialVersionUID = 1L;

	private HashMap<String, Command> map = null;

	@Override
	public void init(ServletConfig config) throws ServletException {
		map = new HashMap<String, Command>();
		
		map.put("DashBoard.do", new DashBoardService());
	}

	protected void service(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {

		request.setCharacterEncoding("UTF-8");

		String uri = request.getRequestURI();

		String cp = request.getContextPath();

		String finaluri = uri.substring(cp.length() + 1);

		String path = null;
		Command com = null;
		if (finaluri.contains("Go")) {
			path = finaluri.substring(2).replaceAll(".do", "");
		} else {
			com = map.get(finaluri);
			path = com.execute(request, response);
		}
		// 3. 페이지 이동
		if (path == null) {
		} else if (path.contains("redirect:/")) {
			response.sendRedirect(path.substring(10));
		} else {
			RequestDispatcher rd = request.getRequestDispatcher("WEB-INF/" + path + ".jsp");
			rd.forward(request, response);
		}

	}

}
~~~
</div>

</br>

### 3.4. Service

<summary><b>Service</b></summary>
<div markdown="1">

~~~java

@WebServlet("/DashBoard")
public class DashBoardService implements Command {

	public String execute(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {

		// 대시보드 보여지게 하는 코드
		DashBoardDAO dao = new DashBoardDAO();

		List<DashBoardMemberVO> result = dao.temp1();
		List<DashBoardMemberVO> result1 = dao.temp2();
		List<DashBoardMemberVO> result2 = dao.temp3();
		List<DashBoardMemberVO> result3 = dao.temp4();
		
		List<DashBoardMemberVO> combinedResult = new ArrayList<>();
		    combinedResult.addAll(result);
		    combinedResult.addAll(result1);
		    combinedResult.addAll(result2);
		    combinedResult.addAll(result3);
		    
		for (DashBoardMemberVO value : result) {
		}
		for (DashBoardMemberVO value : result1) {
		}
		for (DashBoardMemberVO value : result2) {
		}
		for (DashBoardMemberVO value : result3) {
		}
		// 조회된 결과를 JSON으로 변환하여 클라이언트에게 응답
		response.setContentType("application/json");
		response.setCharacterEncoding("UTF-8");

		PrintWriter out = response.getWriter();
	    Gson gson = new Gson();
	    String jsonResult = gson.toJson(combinedResult);
	    out.print(jsonResult);

		return null; // 뷰 이름을 반환하지 않음
	
	}

}
~~~
</div>

</br>

### 3.4. Chart

#### 1. Bar-Chart : 연령별 단속 현황

        
<b>dashboard1.min.js</b>
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

