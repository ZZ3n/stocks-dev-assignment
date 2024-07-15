# 준비 작업
## Github
- 본 리포지토리를 본인 Github 계정에 포크떠서 개발을 진행해주시기 바랍니다.
- 작업은 feature branch를 만들어 커밋을 해주시고, 해당 branch를 fork 하신 개인 repository에 push 해주시기 바랍니다.
- **작업 완료 후 PR 주시고 최대한 의미 있는 작업 단위로 커밋을 해주시기 바랍니다.**

## Tech Spec
- 언어: 자바
- 프레임워크: Springboot 3.x
- 기타: 개발에 필요한 라이브러리 등은 자유롭게 선택하시어 `build.gradle`에 추가해주시면 됩니다.

## 구현 상세
- 특정 기업의 주가(종가) 정보를 조회하여 응답을 주는 API 서버를 개발해주시기 바랍니다.
- 해당 정보를 조회하기 위해 아래 테스트 데이터베이스에는 2개의 테이블이 있습니다.

  ```
  company
  stocks_history
  ```

- `company`는 기업 정보를 저장한 테이블이며, `stocks_history`는 기업의 주식 정보를 저장한 테이블입니다.
- 사용자는 **기업의 종목 코드** (`company` 테이블의 `company_code` 컬럼), 조회 **시작 날짜**와 **종료 날짜**(`stocks_history` 테이블의 `trade_date` 컬럼)로 API를 호출하게 됩니다.
- `company` 테이블과 `stocks_history` 테이블의 join 컬럼은 `company_code` 컬럼입니다.

- API 응답 값에는 아래 정보들이 포함되어야 합니다.
  ```
  * companyName
    - type : String
    - 관련 테이블 : `company` 테이블의 `company_name`

  * tradeDate
    - type : String
    - format : yyyy-mm-dd
    - 관련 테이블 : `stocks_history` 테이블의 `trade_date`

  * closingPrice
    - type : long
    - 관련 테이블 : `stocks_history` 테이블의 `trade_price`
  ```

- 또한, 모든 API는 API 키가 쿼리 파라미터 혹은 헤더로 넘어왔을 때에만 응답을 반환해야 합니다.
  - **헤더**에 API Key를 포함하는 경우 헤더 키 값은 `x-api-key`이며 **쿼리 파라미터**로 포함시킬 경우 파라미터의 키값은 `apikey`입니다.
  - 만약 요청에 키가 없다고 판단 될 경우 `400`, 값이 아래 `API Key`가 아닌 경우, `403` 에러를 반환해주시기 바랍니다.
  - 마찬가지로 Endopoint에서 필수 파라미터로 정의하신 파라미터가 오지 않을 경우 `400` 에러를 반환해주시면 되며, 존재하지 않는 API를 호출 할 경우, `404` 에러를 반환해주시기 바랍니다.

- API 및 응답 포맷은 아래 추가 고려 사항을 고려하여 자유롭게 정의해주시기 바랍니다.

### API key
- `c18aa07f-f005-4c2f-b6db-dff8294e6b5e`


### DB 연결 정보
- **(중요) max connection 갯수는 5 이하로 제한해주시기 바랍니다.**
```
host : dev-assignment.cjugioimm614.ap-northeast-2.rds.amazonaws.com
port : 3306
user : {별도제공}
pw : {별도제공}
```

- DB 및 ORM 관련 라이브러리(Hibernate, Jdbc 등)는 자유롭게 사용하셔도 됩니다.

## 추가 고려 사항
### 필수: API 디자인
- API는 사용자 와의 계약입니다.
- 배포 된 OPEN API 서비스를 변경한다는 건 쉽지 않은 작업입니다.
- 따라서 확장성 있고, 서비스 전체적으로 일관성 있는 API 및 파라미터 설계에 처음부터 많은 노력이 필요합니다.
- 변경이 불가피하게 필요 할 경우를 대비하여 API 버저닝이 고려된 API를 설계해 주시기 바랍니다.
- 가능하다면 API 설계에 대한 철학을 Readme.md 에 작성해 주시기 바랍니다. 또한 작성 된 철학을 어떻게 녹여냈는지 코드와 함께 설명하기 바랍니다.

### 필수: API 응답 값
- 호출되는 API와 마찬가지로 배포되어 리얼 서비스가 시작 된 경우 API의 응답 값도 역시도 변경하기가 쉽지 않습니다.
- 따라서 처음부터 일관성 있는 공통 응답 포맷에 대한 설계가 중요합니다.
- 성공 및 실패 모두에 대한 일관성 있는 응답 값을 설계해 주시기 바랍니다.
- 특정 응답 값을 설계한 이유를 코드에 주석으로 달아 주시기 바랍니다.)

### 옵션: Test Code 
- API의 특성상 API를 사용자의 관점에서 접근할 필요가 있습니다. 
- 스펙(설계)을 기반으로 테스트를 수행하고 이를 통해 기능뿐만 아니라 스펙에 대한 검증을 동시에 진행해야 합니다.
- 구현한 API에 대해 단위 테스트와 통합 테스트를 구현해 주시기 바랍니다.

### 옵션: 응답 형식
- 사용자에게 다앙햔 응답 포맷 옵션을 제공해 주는 것이 중요 할 수 있습니다.
- API 호출 시, 확장자(예: `.json`, `.xml`)나 request parameter(예: `format=json`, `format=xml`)로 원하는 응답 방식을 제공해주면 좋습니다.

### 옵션: API Throttling
- 사용자가 너무 많은 요청을 보내게 되면 API 서버 전체가 불안정해질 수 있습니다.
- 따라서 대부분의 오픈 API 서버는 1초에 N건, 혹은 1분에 N건 등 요청을 제한하는 Quota를 설정 할 수 있습니다.
- 특정 API 키에 대한 호출을 10초에 10건으로 제한하는 Quota 기능을 구현해 주시기 바랍니다.
- API 서버는 1대만 있다고 가정하고 서버의 메모리에 해당 기능을 구현하셔도 됩니다.