# 建構第一個 App

透過 React-Native command line tool 創建一個 project 名稱為 **FirstProject**
```bash
react-native init FirstProject
```

產生的目錄結構
```
./
|-- __tests__
|   |-- index.android.js
|   `-- index.ios.js
|-- android // About Android platform template
|   |-- app
|   |-- build.gradle
|   |-- gradle
|   |-- gradle.properties
|   |-- gradlew
|   |-- gradlew.bat
|   |-- keystores
|   `-- settings.gradle
|-- index.android.js //React code
|-- index.ios.js //React code
|-- ios // About iOS platform template
|   |-- FirstProject
|   |-- FirstProject.xcodeproj
|   `-- FirstProjectTests
|-- node_modules
|   |-- babel-jest
|   |-- babel-preset-react-native
|   |-- jest
|   |-- jest-react-native
|   |-- react
|   |-- react-native
|   |-- react-test-renderer
|   `-- whatwg-fetch
`-- package.json
```

目錄後方有簡略註解資料夾用途。