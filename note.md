# 웹팩 ?
- 웹팩이란 최신 프론트엔드 프레임워크에서 가장 많이 사용되는 `모듈 번들러`이다.

- 모듈 번들러란 웹 애플리케이션을 구성하는 자원(HTML, CSS, Javascript, Images 등)을 모두 각각의 모듈로 보고 이들을 조합하여 병합된 하나의 결과물을 만드는 도구를 의미한다.

# 모듈 ?
- `모듈`이란 프로그래밍 관점에서 특정 기능을 갖는 작은 코드 단위를 의미한다.

- 자바스크립트 파일을 예로 들면 성격이 비슷한 여러 기능 함수들을 하나의 의미 있는 파일로 분리하여 관리하면 모듈이라고 부를 수 있다.

# 웹팩에서의 모듈 ?
- 웹팩에서 지칭하는 모듈이라는 개념은 위와 같이 자바스크립트 모듈에만 국한되지 않고 `웹 애플리케이션을 구성하는 모든 자원`을 의미한다.

- 웹 애프리케이션을 제작할 때 HTML, CSS, Javascrpit, Images, Font 등 많은 파일들이 필요한데, 이러한 자원들에 해당하는 파일 하나하나가 모두 모듈에 해당한다.

# 모듈 번들링 ?
- `모듈 번들링`은 아래 그림과 같이 웹 애플리케이션을 구성하는 많은 자원들을 하나의 파일로 병합 및 압축하는 동작을 말한다.

- 빌드, 번들링, 변환 이 세 단어는 모두 같은 의미이다.

![](https://joshua1988.github.io/webpack-guide/assets/img/webpack-bundling.e79747a1.png)

# 웹팩의 등장 배경
웹팩이 등장한 이유는 아래와 같이 크게 3가지 이유가 존재합니다.

1. 파일 단위의 자바스크립트 모듈 관리의 필요성
2. 웹 개발 작업 자동화 도구
3. 웹 애플리케이션의 빠른 로딩 속도와 높은 성능

## 파일 단위의 자바스크립트 모듈 관리
- 자바스크립트의 변수 유효 범위는 기본적으로 전역 범위를 가진다.

- 최대한 넓은 변수 범위를 갖기 때문에 어디에서도 접근하기 편리하지만, 이러한 장점이 실제 웹 애플리케이션을 개발할 때는 아래와 같은 문제점으로 변하게 된다.

```javascript
<!-- index.html -->
<html>
  <head>
    <!-- ... -->
  </head>
  <body>
    <!-- ... -->
    <script src="./app.js"></script>
    <script src="./main.js"></script>
    <script>
      getNum(); // 20
    </script>
  </body>
</html>
```

```javascript
// app.js
var num = 10;
function getNum() {
  console.log(num);
}
```

```javascript
// main.js
var num = 20;
function getNum() {
  console.log(num);
}
```

- 위와 같은 코드에서 `index.html`에서 두 자바스크립트 파일을 로딩하고 같은 이름의 변수를 조회하면 `20`이라는 값만 조회되어지며 중복된 이름의 변수 데이터 관리가 어려워집니다.

- 웹팩을 활용하면 위와 같은 중복에 대한 문제를 파일 단위로 변수를 관리하기 용이한 장점이 있다.

## 웹 개발 작업 자동화 도구
- 이전부터 프런트엔드 개발 업무를 할 때 가장 많이 반복하는 작업은 텍스트 편집기에서 코드를 수정하고 저장한 뒤 브라우저에서 새로 고침을 눌러야만 화면에 변경된 내용을 볼 수 있었다.

- 그 외에도 웹 서비스를 개발하고 웹 서버에 배포할 때 아래와 같은 작업들을 병행하는 도구들이 필요했다.
  - HTML, CSS, JS 압축
  - 이미지 압축
  - CSS 전처리기 변환

- 이러한 일들을 자동화 해주는 도구들이 필요하여 등장한 도구가 Grunt, Gulp와 같은 도구이며 위의 작업들에 추가로 `모듈 기능`까지 제공하는 도구가 `Webpack`인 것이다.

## 웹 애플리케이션의 빠른 로딩 속도와 높은 성능
- 웹 사이트의 로딩 속도를 높이기 위해 많은 노력들이 있었다. 그 중 대표적인 노력이 `브라우저에서 서버로 요청하는 파일 숫자를 줄이는 것` 이였고, 이를 위해 앞에서 살펴본 웹 태스크 매니저를 이용해 파일들을 압축하고 병합하는 작업들을 진행해야 했었다.

- 하지만 이러한 기존의 웹 태스크 매니저(Grunt, Gulp)의 압축과 병합과정이 아닌 상황에서의 라이브러리 의존성에 대한 성능 저하 문제가 존재했다.

- 위와 같은 상황의 문제점을 보완하고 `초기 페이지 로딩 속도를 높이기 위해 나중에 필요한 자원들은 나중에 요청하는 레이지 로딩(Lazy Loading) 기술`이 적용가능한 도구가 바로 Webpack이다.

- 웹팩은 기본적으로 `필요한 자원은 미리 로딩하는게 아니라 그 때 그 때 요청하자는 철학`을 가지고 있다.

# 웹팩의 4가지 주요 속성
## Entry
- `entry` 속성은 웹팩에서 웹 자원을 변환하기 위해 필요한 최초 진입점으로 자바스크립트 파일 경로를 뜻한다.

- 아래 코드는 웹팩 실행시 `src` 폴더의 `index.js` 를 경로로 하여 웹팩이 빌드를 수행한다.

```javascript
// webpack.config.js
module.exports = {
  entry: './src/index.js'
}
```

### Entry 파일에 들어가야 하는 내용
- `entry` 속성에 지정된 파일에는 웹 애플리케이션의 전반적인 구조와 내용이 담겨져 있어야 한다.

- 웹팩이 해당 파일을 가지고 `웹 애플리케이션에서 사용되는 모듈들의 연관관계를 이해하고 분석`하기 때문에 애플리케이션을 동작시킬 수 있는 내용들이 포함되어야 하는 것 이다.

### Entry 유형
- entry 포인트가 1개가 일반적으로 사용되지만 아래와 같이 여러 개가 될 수 있다.

- 이는 싱글 페이지 애플리케이션이 아닌 특정 페이지로 진입시 서버에서 해당 정보를 내려주는 형태의 멀티 페이지 애플리케이션에 적합하다.

```javascript
module.exports = {
  entry: {
    login: './src/LoginView.js',
    main: './src/MainView.js'
  }
}
```

## Output
- `output` 속성은 웹팩을 돌리고 난 결과물의 파일 경로를 의미한다.
- `entry` 속성과 달리 무조건 객체 형태로 옵션들을 추가해야 한다.

```javascript
// webpack.config.js
module.exports = {
  output: {
    filename: 'bundle.js'
  }
}
```

### Output 속성 옵션 형태
- 최소한 `filename` 속성은 지정해줘야 하며 일반적으로 아래와 같이 `path` 속성을 함께 정의해야 한다.
- `filename` : 웹팩으로 빌드한 파일의 이름을 의미한다.
- `path` : 해당 파일의 경로를 의미한다.
- `resolve()` : 인자로 넘어온 경로(`./dist`)들을 조합하여 유효한 파일 경로를 만들어주는 Nodejs Api이다.

```javascript
// webpack.config.js
const path = require('path');

module.exports = {
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, './dist')
  }
}
```

### Output 파일 이름 옵션
- `filename` 속성에 여러 가지 옵션을 지정할 수 있다.
- 결과 파일 이름에 `entry` 속성을 포함하는 옵션
```javascript
module.exports = {
  output: {
    filename: '[name].bundle.js'
  }
};
```
- 결과 파일 이름에 웹팩 내부적으로 사용하는 `모듈 ID`를 포함하는 옵션
```javascript
module.exports = {
  output: {
    filename: '[id].bundle.js'
  }
};
```
- 빌드를 할 때 마다 `고유 해시 값`을 포함하는 옵션
```javascript
module.exports = {
  output: {
    filename: '[name].[hash].bundle.js'
  }
};
```
- 웹팩의 각 모듈 내용을 기준으로 `생성된 해시 값`을 포함하는 옵션
```javascript
module.exports = {
  output: {
    filename: '[chunkhash].bundle.js'
  }
};
```
- 이와 같은 옵션들을 사용하면 애플리케이션의 개발 코드 변화가 있을 경우 강제적인 새로고침을 통한 확인이 아닌 `가시적으로 표시한 값들을 통해 변화를 감지`할 수 있는 편리함이 있다.

## Loader
- `Loader`는 웹팩이 웹 애플리케이션을 해석할 때 자바스크립트 파일이 아닌 웹 자원(HTML, CSS, Images, 폰트 등)들을 변환할 수 있도록 도와주는 속성이다.

- entry나 output 속성과는 다르게 `module`이라는 속성을 사용한다.

```javascript
// webpack.config.js
module.exports = {
  module: {
    rules: []
  }
}
```

### Loder가 필요한 이유
- 웹팩으로 애플리케이션을 빌드할 때 Loader를 사용하지 않는다면 에러가 발생한다.

- 이는 Loader가 없을 경우 빌드 과정에서 js가 아닌 `다른 웹 자원들(css, image 등)에 대한 해석이 불가능`하기에 발생하는 에러이다.

### Loader 적용
- 여러 웹 자원들에 대한 Loader 종류는 다양하다.

- 여러 종류의 Loader 중에서 CSS Loader를 적용하는 방법을 예시로 알아보자. 대부븐 Loader 별로 적용하는데 큰 차이가 없다.

```javascript
// 라이브러리 구성
yarn add -D css-loader
```

```javascript
// webpack.config.js
module.exports = {
  entry: './app.js',
  output: {
    filename: 'bundle.js'
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ['css-loader']
      }
    ]
  }
}
```

- 위의 코드에서 Loader를 적용하는 핵심 속성은 `module` 속성이다.

- `rules` 배열에 객체 한 쌍을 추가하고 그 객체에 2개의 속성을 추가한다.

- `test` : `Loader를 적용할 파일 유형`으로 일반적으로 정규 표현식을 사용한다.

- `use` : `해당 파일에 적용할 Loader 이름`을 나열한다.

- 위 코드를 통해 해당 프로젝트의 모든 CSS 파일에 대해서 CSS loader를 적용할 수 있다.
### 자주 사용되는 Loader 종류
- 위에서 말했듯이 다양한 웹 자원들에 대한 Loader의 종류는 다양하다.
```javascript
- Babel Loader
- Sass Loader
- File Loader
- Vue Loader
- TS Loader
...
```

- 위에서 적용한 CSS loader 뿐 아니라 여러 종류의 loader를 적용하려면 필요한 loader 라이브러리를 package.json에 추가하고 아래와 같이 `rules` 배열에 loader 옵션을 추가해야한다.

```javascript
// webpack.config.js
module.exports = {
  entry: './app.js',
  output: {
    filename: 'bundle.js'
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ['css-loader']
      },
      {
        test: /\.ts$/,
        use: ['ts-loader']
      }
    ]
  }
}
```

### Loader 적용 순서
- 특정 파일에 대해 `여러 개의 로더를 사용하는 경우 로더가 적용되는 순서에 주의`해야 한다.

- Loader는 `기본적으로 오른쪽에서 왼쪽 순으로 적용`된다.

```javascript
// webpack.config.js
// CSS의 확장 문법인 SCSS 파일에 로더를 적용하는 경우
module.exports = {
  entry: './app.js',
  output: {
    filename: 'bundle.js'
  },
  module: {
    rules: [
      {
        test: /\.scss$/,
        use: ['css-loader', 'scss-loader']
      }
    ]
  }
}
```

- 위 코드는 scss 파일에 대해 먼저 scss-loader를 통해 css 파일로 전처리를 진행하고 css 파일을 웹팩에서 해석할 수 있도록 동작하게 해준다.

## Plugin
- 플러그인은 `웹팩의 기본적인 동작에 추가적인 기능을 제공`하는 속성이다.

- 로더랑 비교해보면 `로더는 파일을 해석하고 변환하는 번들링 과정에 관여`하지만, `플러그인은 번들링이 이루어진 결과물의 형태를 바꾸는 역할`을 한다.

```javascript
// webpack.config.js
var webpack = require('webpack');
var HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  // ...

  plugins: [
    new HtmlWebpackPlugin(),
    new webpack.ProgressPlugin()
  ]

  // ...
}
```

- 위 코드와 같이 plugins 배열에 `생성자 함수를 통해 생성한 객체 인스턴스`를 추가하여 사용한다.

- 플러그인 역시 로더와 마찬가지로 종류가 다양하므로 특정 상황에 필요에 의해 사용하면 된다.

- `HtmlWebpackPlugin`: 웹팩으로 빌드한 결과물로 HTML 파일을 생성해준다.
- `ProgressPlugin`: 웹팩의 빌드 진행율을 표시해주는 플러그인