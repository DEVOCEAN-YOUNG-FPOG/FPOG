# 03. Lighthouse 툴을 이용한 페이지 검사


## 📌 3.1 Lighthouse로 검사하기

<img width="600" alt="Untitled" src="https://github.com/DEVOCEAN-YOUNG-FPOG/FPOG/assets/70098708/264e16b9-599c-482f-b9b0-0146bfedb260">

- 크롬 개발자 도구의 메뉴에서 Lighthouse 툴을 찾을 수 있다.
- 사용법
    - **Mode**: `Navigation`
    - **Device**: `Desktop`
    - **Categories**: `Performance` (웹 페이지의 성능을 측정한다.)
- **Mode**
    - `Navigation` : 초기 페이지 로딩 시 발생하는 성능 문제를 분석한다.
    - `Timespan` : 사용자가 정의한 시간 동안 발생한 성능 문제를 분석한다.
    - `Snapshot` : 현재 상태의 성능 문제를 분석한다.
- **Categories**
    - `Peformance` : 웹 페이지의 로딩 과정에서 발생하는 성능 문제를 분석한다.
    - `Accessibility` : 서비스의 사용자 접근성 문제를 분석한다.
    - `Best practices` : 웹사이트의 보안 측면과 웹 개발의 최신 표준에 중점을 두고 분석한다.
    - `SEO` : 얼마나 잘 크롤링되고 검색 결과에 표시되는지 분석한다.
    - `Progressive Web App` : PWA와 관련된 문제를 분석한다.

## 📌 3.2 Lighthouse 검사 결과


### 검사 결과

<img width="600" alt="Untitled2" src="https://github.com/DEVOCEAN-YOUNG-FPOG/FPOG/assets/70098708/05197af3-ccbc-4dd0-82e1-dd66f9e175f7">

- 종합성능점수
    
    : 6가지 지표(웹 바이탈)에 가중치를 적용해 평균 낸 점수
    

### 웹 바이탈(Web Vitals)

<img width="700" alt="Untitled3" src="https://github.com/DEVOCEAN-YOUNG-FPOG/FPOG/assets/70098708/c2c859c9-b1bb-49d8-9fb4-d426a261a1ad">

1. **FCP(First Contentful Paint)**
    
    : 페이지 로드시 브라우저가 DOM 콘텐츠의 첫 번째 부분을 렌더링 하는데 걸리는 시간에 관한 지표이다.
    
    - 가중치 `10%`
2. **SI(Speed Index)**
    
    : 페이지 로드 중에 콘텐츠가 시각적으로 표시되는 속도를 나타내는 지표이다.
    
    - 가중치 `10%`
3. **LCP(Largest Contentful Paint)**
    
    : 화면 내에 있는 가장 큰 이미지나 텍스트 요소가 렌더링되기까지 걸리는 시간을 나타내는 지표이다.
    
    - 가중치 `25%`
4. **TTI(Time to Interactive)**
    
    : 사용자가 페이지와 상호 작용 (ex. 클릭 또는 키보드 누름)이 걸리는 시점까지 걸리는 시간을 측정한 지표이다.
    
    - 가중치 `10%`
5. **TBT(Total Blocking Time)**
    
    : 페이지가 사용자 입력에 응답하지 않도록 차단된 시간을 총합한 지표이다.
    
    - FCP —> TBT 측정 —> TTI
    - 가중치 `30%`
6. **CLS(Cumulative Layout Shift)**
    
    : 페이지 로드 과정에서 예기치 못한 레이아웃 이동을 측정한 지표이다.
    
    - 가중치 `15%`

### Opportunities

<img width="400" alt="Untitled4" src="https://github.com/DEVOCEAN-YOUNG-FPOG/FPOG/assets/70098708/11852d33-3702-429e-839f-006b7ebac8be">

- 페이지를 더욱 빨리 로드하는데 잠재적으로 도움되는 제안을 알려준다.

### Diagnostics

<img width="400" alt="Untitled5" src="https://github.com/DEVOCEAN-YOUNG-FPOG/FPOG/assets/70098708/d30a34f2-84cd-4dd9-b950-8ed2685e609a">

- 로드 속도와 직접적인 관계는 없지만 성능과 관련된 기타 정보를 보여준다.

### 검사 환경

<img width="600" alt="Untitled6" src="https://github.com/DEVOCEAN-YOUNG-FPOG/FPOG/assets/70098708/6eb9066e-30bb-4207-bf6f-79ce3aa4ce0f">

- **Emulated Desktop -** CPU throttling
 
    <img width="333" alt="Untitled7" src="https://github.com/DEVOCEAN-YOUNG-FPOG/FPOG/assets/70098708/106388f4-e34f-4a25-b98f-c5b1900dbc25">
    
    - 기기의 CPU 성능을 어느 정도 제한하여 검사를 진행했는지 알려준다.
    - Device 설정이 Mobile 이라면 4x로 더 제한될 것이다.
    
- **Custom throttling -** Network throttling

    <img width="329" alt="Untitled8" src="https://github.com/DEVOCEAN-YOUNG-FPOG/FPOG/assets/70098708/6beedf1f-f5a2-445f-b146-ff6eb45c5e1e">

    - 네트워크를 제한하여 어느정도 고정된 네트워크 환경에서 성능을 측정했다.
    - Device 설정이 Mobile 이라면 더욱 제한될 것이다.
