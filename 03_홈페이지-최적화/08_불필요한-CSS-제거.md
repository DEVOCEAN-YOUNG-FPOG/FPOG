# 08. 불필요한 CSS 제거

이번 장에서는 불필요한 CSS를 제거하여 최적화하는 기법을 알아본다.

### Lighthouse 검사

이 방법을 적용하기 전 `npm run serve` 스크립트로 실행된 서비스를 **Lighthouse**로 검사해본다.  
우리가 주목해야할 부분은 **Opportunities 섹션**의 **'Reduce unused CSS'** 항목이다.
<img width="536" alt="image" src="https://github.com/DEVOCEAN-YOUNG-FPOG/FPOG/assets/117897253/99c484b5-dd83-42ba-8be5-5a04c2b73780">

해당 항목은 사용하지 않는 CSS 코드를 제거하면 성능에 긍정적인 영향을 줄 수 있다는 의미이다.  
세부 내용을 살펴보면 main.chuck.css가 609.7KiB인데, 사용하지 않는 코드를 제거하면 605.8 KiB를 줄일 수 있다고 한다.

### Coverage 검사

Coverage 패널은 페이지에서 사용하는 자바스크립트 및 CSS 리소스에서 실제로 실행하는 코드가 얼마나 되는지 알려 주며 그 비율을 표시해준다.

<img width="314" alt="image" src="https://github.com/DEVOCEAN-YOUNG-FPOG/FPOG/assets/117897253/7b6865fc-ac74-4f0d-b2ad-d2ee3e3b13af">

Coverage 패널은 크롬 개발자 도구에서 찾을 수 있고, 열어보면 다음과 같은 화면을 볼 수 있다.

<img width="428" alt="image" src="https://github.com/DEVOCEAN-YOUNG-FPOG/FPOG/assets/117897253/045303ac-b4ad-44ba-a449-b87016fa83f5">

상단의 새로고침 버튼을 클릭하면, 그 과정에서 실행한 코드를 리소스별로 표시해준다.

<img width="428" alt="image" src="https://github.com/DEVOCEAN-YOUNG-FPOG/FPOG/assets/117897253/3c90031b-c67a-4e22-bdc0-96ebc0cbdfdc">

오른쪽 **Unused Bytes(사용하지 않는 바이트)** 와 **Usage Visualization(사용량 시각화)** 항목에서 전체 코드 대비 실행된 코드의 양을 비율로 보여 준다.  
 자바스크립트 리소스 파일은 if문 같은 조건이 걸려 있어 분기되는 코드가 많아 페이지를 이동하고 기타 동작들을 해보면 점점 코드 사용 비율이 증가합니다.  
그러나, CSS코드는 자바스크립트와 다르게 별다른 분기가 있지 않기에, 더욱 신경써야 합니다.  
 해당 항목을 눌러 보면 어떤 코드가 실행되었고(파란 막대), 어떤 코드가 실행되지 않았는지(빨간 막대) 확인해 볼 수 있습니다.

Tailwind CSS라이브러리를 사용하면, 개발할 때는 미리 만들어진 클래스를 통해 쉽고 빠르게 스타일을 적용할 수 있다는 장점이 있었지만, _빌드 후에 사용하지 않는 스타일도 함께 빌드되어 파일의 사이즈를 크게 만든다는 단점_ 있기에 다음과 같이 해결한다.

## 📌8.1 PurgeCSS

사용하지 않는 CSS코드를 제거하는 방법은 여러 가지가 있지만, 여기에서는 **PurgeCSS** 라는 툴을 사용하여 해결한다.
<img width="625" alt="image" src="https://github.com/DEVOCEAN-YOUNG-FPOG/FPOG/assets/117897253/f28a6b16-45e0-4e29-97a4-4fb920807bbd">

**PurgeCSS**는 _파일에 들어 있는 모든 키워드를 추출하여 해당 키워드를 이름으로 갖는 CSS 클래스만 보존하고 나머지 매칭되지 않은 클래스는 모두 지우는 방식으로 CSS파일을 최적화_ 한다.

```javascript
<figure class="bg-slate-100 rounded-xl">
  <div class="pt-6 space-y-4">PurgeCSS</div>
</figure>
```

예를 들어 위와 같은 텍스트 파일이 있을 때, 키워드를 추출하면 **figure, class, bg-slate-100, rounded-xl, p-8, div, pt-6, space-y-4** 등이 추출된다.  
그러면 추출된 키워드와 Tailwind CSS에서 제공하는 유틸리티 클래스의 이름을 비교하여 일치하는 클래스만 남기는 방식이다. 이렇게 하면 코드 내에서 사용하지 않은 클래스는 매칭되지 않으므로 CSS파일에서 제거될 것이다.  
사용 방법은 다양하지만 그중 CLI를 이용한 방법을 사용해보자.  
먼저 npm을 이용하여 툴을 설치해줍니다.

```
$ npm install -save-dev purgeCss
```

그리고 키워드를 추출하고자 하는 파일과 불필요한 클래스를 제거할 CSS 파일을 지정한다.

```
purgecss --css ./build/static/css/*.css --output ./build/static/css/ --content ./build/index.html ./build/static/js/*.js
```

여기서는 불필요한 클래스를 제거할 CSS(-css)로 빌드된 CSS 파일을 선택하였고, 아웃풋(--output)으로는 동일한 위치를 지정함으로써 새로운 파일을 생성하는 대신 기존 CSS 파일을 덮어 쓰도록 한다. 그리고 키워드를 추출할 파일(--content)로는 빌드된 HTML과 자바스크립트 파일 전부를 넣어 준다. 이렇게 하면 빌드된 HTML과 자바스크립트 파일의 텍스트 키워드를 모두 추출하여 빌드된 CSS 파일의 클래스와 비교하고 최적화하게 된다.  
 앞서 설치한 PurgeCSS는 프로젝트의 devDependency로 설치되었으므로, 위 스크립트를 그대로 실행하면 제대로 실행되지 않는다. 그렇기에 우리는 package.json의 scripts에 넣어준다.

```
  "scripts": {
   "start": "npm run build:style && react-scripts --openssl-legacy-provider start",
   "build": "npm run build:style && react-scripts --openssl-legacy-provider build",
   "build:style": "postcss src/tailwind.css -o src/styles.css",
   "serve": "node ./server/server.js",
   "server": "node ./node_modules/json-server/lib/cli/bin.js --watch ./server/database.json -c ./server/config.json",
   "purge": "purgecss --css ./build/static/css/*.css --output ./build/static/css/ --content ./build/index.html ./build/static/js/*.js"
 },
```

스크립트를 추가한 후, **npm run purge**를 실행 별다른 메세지 없이 실행된다.  
 하지만 서비스를 재시작한 후, Coverage 패널을 살펴보면 CSS파일의 사이즈와 사용되지 않은 코드의 비율이 달라진 것을 볼 수 있지만, 일부 스타일이 제대로 적용되지 않는 문제점이 발생한다.  
 이 현상은 PurgeCSS가 텍스트 키워드를 추출할 때 클론(:) 문자를 하나의 키워드로 인식하지 못하고 잘라 버렸기에 생기는 현상으로 **PurgeCSS의 defaultExtractor 옵션**을 통해 해결한다.

**defaultExtractor**라는 옵션은 PurgeCSS가 키워드를 어떤 기준으로 추출할 지 정의하는 옵션이다. 이 옵션을 설정하기 위해 purgecss.config.js라는 이름으로 아래와 같이 설정을 추가하고 프로젝트 최상단 경로에서 생성한다.

```
module.exports = {
  defaultExtractor: (content) => content.match(/[\w\:\-]+/g) || []
}
```

defaultExtractor 옵션에는 함수가 들어가는데, 이 함수는 인자로 대상 파일의 전체 코드를 넘겨받고, match메서드를 통해 정규식을 만족하는 키워드를 배열 형태로 추출한다. 그리고 이 문자열 배열이 클래스를 필터링할 텍스트 키워드가 된다.  
최종적으로 스크립트에서 설정 파일의 경로(--config)를 지정한다.

```
  "scripts": {
   "start": "-",
   "build": "-",
   "build:style": "-",
   "serve": "-",
   "server": "-",
   "purge": "purgecss --css ./build/static/css/*.css --output ./build/static/css/ --content ./build/index.html ./build/static/js/*.js --config ./purgecss.config.js"
 },
```

수정한 내용을 적용하기 위해 빌드 후 PurgeCSS를 다시 실행한다

```
$ npm run build
$ npm run purge
```

이후 서비스를 확인해 보면 스타일이 제대로 반영되었고, CSS 파일도 굉장히 작아졌음을 확인할 수 있다.

### 🗯️PurgeCSS로 사용하지 않는 스타일을 지웠는데도 왜 사용하지 않는 코드 비율이 높을까❓

Coverage 패널은 파일의 코드가 실행되었는지 아닌지 체크한다. 만약 if같은 조건문을 통해 분기가 일어났면 일부 코드는 실행되지 않을 것이고, Coverage 패널에서도 해당 코드는 실행되지 않았다고 체크할 것이다. 이후 사용자가 서비스를 이용하다 해당 코드가 실해오디면, 그때 해당 코드가 실행되었ㄷ고 표시하며 Coverage 패널의 결과도 변한다. CSS도 마찬가지로 페이지에 따라 아직 표시되지 않은 요소들이 있기에 처음부터 모든 CSS 코드가 실행되지 않는다. 즉, 코드의 실행 비율은 사용자가 서비스를 이용하면서 조금씩 증가한다.
