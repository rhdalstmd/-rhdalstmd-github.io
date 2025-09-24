과제 수행 과정
1. 공공데이터 API 분석

공공데이터포털에서 제공하는 경상북도 고령군 코로나19 검사 현황 API를 선택하여 활용하였다.

해당 API는 JSON 형식으로 데이터를 제공하며, 서비스 키(serviceKey)를 포함해 호출해야 한다.

사용한 엔드포인트:

https://api.odcloud.kr/api/15065039/v1/uddi:50586d19-36a2-4b44-9764-9b6f80965c78

2. 데이터 요청 (fetch)

JavaScript의 fetch() 함수를 사용하여 API에 요청을 보냈다.

응답을 await res.json()으로 받아 문자열(JSON) → 자바스크립트 객체로 변환하였다.

실제 데이터 배열은 json.data에 포함되어 있어, 이를 전역 변수 globalData에 저장하였다.

const res = await fetch(`${API_URL}?page=1&perPage=100&serviceKey=${API_KEY}&returnType=JSON`);
const json = await res.json();
globalData = json.data;

3. 데이터 파싱

확보한 JSON 객체를 표(table) 형태로 가공하였다.

Object.keys()를 통해 속성명(열 이름)을 추출 → <th> 태그에 출력.

Object.values()를 통해 각 행의 데이터 값들을 추출 → <td> 태그에 삽입.

반복문(forEach)을 사용하여 모든 데이터를 HTML 테이블 구조로 변환하였다.

Object.keys(data[0]).forEach(key => { html += `<th>${key}</th>`; });
data.forEach(row => {
  html += "<tr>";
  Object.values(row).forEach(val => { html += `<td>${val}</td>`; });
  html += "</tr>";
});

4. 브라우저 출력

완성된 HTML 코드를 document.getElementById("table-container").innerHTML에 삽입.

사용자가 버튼을 클릭하면 공공데이터 API에서 가져온 데이터가 표로 표시되도록 구현하였다.

document.getElementById("table-container").innerHTML = html;

5. 추가 기능 (CSV 저장)

파싱된 데이터를 CSV 파일로 변환해 다운로드할 수 있는 기능을 추가하였다.

Blob 객체와 URL.createObjectURL()을 활용하여 사용자가 버튼을 클릭하면 CSV 파일이 자동 생성·저장되도록 구현하였다.

const blob = new Blob([csvContent], { type: "text/csv;charset=utf-8;" });
const link = document.createElement("a");
link.href = URL.createObjectURL(blob);
link.download = "goryeong_covid19.csv";
link.click();

✅ 결론

이번 과제를 통해 공공데이터 API에서 데이터 확보 → JSON 파싱 → HTML 테이블 변환 → 브라우저 출력 과정을 성공적으로 구현하였다.

과제에서 요구한 기능을 충족했으며, 추가로 CSV 저장 기능까지 구현하여 활용성을 높였다.
