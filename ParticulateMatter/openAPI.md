## 미세먼지 API 사용법

### 공공데이터 포털에서 제공하는 OPEN API 사용하는 방법

1) <https://www.data.go.kr> 에 로그인.

2) **미세먼지** 검색

<img width="1280" alt="openAPI_01" src="https://user-images.githubusercontent.com/13485924/76332747-1f221d00-6334-11ea-90f1-9f981810b97d.png">

<br>

3) 검색결과를 보면 파일데이터, 오픈 API 의 데이터들을 확인할 수 있다. 

<img width="1280" alt="openAPI_02" src="https://user-images.githubusercontent.com/13485924/76332910-411b9f80-6334-11ea-9fcd-960dc51874a3.png">

- 여기에서 조회수 및 활용신청건수가 가장 많은 **한국환경공단_대기오염정보** 를 활용

<br>

4) 활용신청 및 참고문서 다운

<img width="1055" alt="openAPI_03" src="https://user-images.githubusercontent.com/13485924/76332933-4842ad80-6334-11ea-96b2-a66011aa11bf.png">

- 활용신청 버튼을 통해 활용을 신청하면 된다. (개발계정 신청)
- 참고문서란에 있는 docx 파일을 다운받으면, API 에 대한 내용들을 확인할 수 있다. 
- End Point, 데이터포맷, API 유형을 확인할 수 있다. 

<br>

5) 개발계정 신청

<img width="422" alt="openAPI_04" src="https://user-images.githubusercontent.com/13485924/76333969-c489c080-6335-11ea-9dba-33df88e47e9e.png">

- 활용신청을 하면 대략 위와 같은 페이지로 이동이 된다. 
- 심의여부 : 자동승인
- 활용기간 : 승일일로부터 24개월간 활용
- 시스템 유형 : 일반
- 일일 트래픽 확인. (**한국환경공단_대기오염정보** 의 경우 500 으로 제한되어 있다.)
- 상세기능정보 체크 후 신청하기.

<br>

6) 개발계정 API키 받기

**마이페이지> 오픈API > 개발계정** 으로 이동하면 아래와 같은 화면을 확인할 수 있다. 

<img width="1058" alt="openAPI_05" src="https://user-images.githubusercontent.com/13485924/76334697-c7d17c00-6336-11ea-832b-faccd61e2de3.png">

- **대기오염정보 조회 서비스** 를 클릭.

<br>

<img width="1074" alt="openAPI_06" src="https://user-images.githubusercontent.com/13485924/76334792-ecc5ef00-6336-11ea-9faa-f9d7dc0cebab.png">

- **일반 인증키 받기** 버튼을 클릭.

<br>

<img width="1037" alt="openAPI_07" src="https://user-images.githubusercontent.com/13485924/76335002-36aed500-6337-11ea-9cda-850cfb4bd082.png">

- **일반 인증키** 라는 것을 확인할 수 있는데, 이 키를 사용해서 API 서버에 요청하면 결과값을 받을 수 있다. 

<br>

7) 문서확인 (위에서 다운받은 참고문서, docx 파일)

<img width="534" alt="openAPI_08" src="https://user-images.githubusercontent.com/13485924/76335722-4aa70680-6338-11ea-87af-acb0a6700b4d.png">

- 목차를 보면 측정소정보 조회 서비스, 대기오염정보 조회 서비스 등을 확인할 수 있다.

<br>

<img width="530" alt="openAPI_09" src="https://user-images.githubusercontent.com/13485924/76335925-9659b000-6338-11ea-92b8-91ee37dac580.png">

- 문서에서 대기오염정보 조회 서비스의 경우 위와 같은 내용을 확인할 수 있다.
- 서비스 인증/권한은 서비스 Key 만 있으면 된다.
- 인터페이스 표준으로는 REST GET 방식으로 요청한다.
- 교환 데이터 표준의 경우 XML, JSON 을 지원한다.
- 메세지 교환 유형은 Request-Response 방식이다 .
- JSON 방식으로 호출을 원하면 **&_returnType=json** 을 추가하면 된다.

<br>

<img width="542" alt="openAPI_10" src="https://user-images.githubusercontent.com/13485924/76336647-aaea7800-6339-11ea-8b12-7163e550b753.png">

- 요청 / 응답 메세지 예제를 확인할 수 있다.
- **서비스 키**에는 발급받은 서비스키를 입력하고, `&_returnType=json` 를 추가하면 JSON 형식의 응답 메세지를 확인할 수 있다.

<br>

8) 브라우저 테스트 (JSON)

<img width="1277" alt="openAPI_11" src="https://user-images.githubusercontent.com/13485924/76336859-ef761380-6339-11ea-8463-be2cd825048a.png">

- 브라우저에서 확인한 JSON 형식의 응답 메세지이다.

<br>

9) JS 코드

```js
const axios = require('axios')
const key = '%2B%2BKoCiedn%2FxxVDEbkNoKDypjWPZwCxnsXs...'
const url = `http://openapi.airkorea.or.kr/openapi/services/rest/ArpltnInforInqireSvc/getMsrstnAcctoRltmMesureDnsty?stationName=${encodeURI('종로구')}&dataTerm=month&pageNo=1&numOfRows=10&ServiceKey=${key}&ver=1.3&_returnType=json`

const f = async () => {
  try {
    const {data} = await axios.get(url)
    console.log(data.list)
  } catch(e) {
    console.log({e})
  }
}

f()
```

- axios 사용시, 한글의 경우 `encodeURI()` 를 적용해줘야 한다. 
- url 의 경우 `endPoint/operation?요청메세지항목=값` 의 구조를 가진다.

<br>

10) 실행결과

```js
[ { _returnType: 'json',
    coGrade: '1',
    coValue: '0.4',
    dataTerm: '',
    dataTime: '2020-03-11 10:00',
    khaiGrade: '2',
    khaiValue: '54',
    mangName: '도시대기',
    no2Grade: '1',
    no2Value: '0.012',
    numOfRows: '10',
    o3Grade: '1',
    o3Value: '0.026',
    pageNo: '1',
    pm10Grade: '2',
    pm10Grade1h: '2',
    pm10Value: '34',
    pm10Value24: '34',
    pm25Grade: '1',
    pm25Grade1h: '1',
    pm25Value: '10',
    pm25Value24: '14',
    resultCode: '',
    resultMsg: '',
    rnum: 0,
    serviceKey: '',
    sidoName: '',
    so2Grade: '1',
    so2Value: '0.003',
    stationCode: '',
    stationName: '',
    totalCount: '',
    ver: '' },
  { _returnType: 'json',
    coGrade: '1',
    coValue: '0.4',
    dataTerm: '',
    dataTime: '2020-03-11 09:00',
    khaiGrade: '2',
    khaiValue: '54',
    mangName: '도시대기',
    no2Grade: '1',
    no2Value: '0.016',
    numOfRows: '10',
    o3Grade: '1',
    o3Value: '0.021',
    pageNo: '1',
    pm10Grade: '2',
    pm10Grade1h: '2',
    pm10Value: '31',
    pm10Value24: '34',
    pm25Grade: '2',
    pm25Grade1h: '1',
    pm25Value: '8',
    pm25Value24: '16',
    resultCode: '',
    resultMsg: '',
    rnum: 0,
    serviceKey: '',
    sidoName: '',
    so2Grade: '1',
    so2Value: '0.004',
    stationCode: '',
    stationName: '',
    totalCount: '',
    ver: '' }, 
 {
    ...
    } ]
```

- 위와 같은 결과를 얻을 수 있다. 
- 각 항목들에 대한 내용은 문서를 통해 확인 가능하다. 

<br>

11)  오퍼레이션 및 요청 메시지 명세

<img width="531" alt="openAPI_12" src="https://user-images.githubusercontent.com/13485924/76417113-9bbe0580-63df-11ea-951a-406900517085.png">

<img width="524" alt="openAPI_13" src="https://user-images.githubusercontent.com/13485924/76417188-c4de9600-63df-11ea-91c7-de59ad16069c.png">

- 요청 메세지 명세는 오퍼레이션에 따라 상이하다. 

<br>

#### Reference

<https://jeong-pro.tistory.com/143>
