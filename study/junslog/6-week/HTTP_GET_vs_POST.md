### GET과 POST의 차이점에 대해 설명해주세요.

```
HTTP에서 GET 메서드는 서버에서 데이터 혹은 리소스를 가져오기 위한 목적으로 사용됩니다. ( Retrieve )
반면, POST 메서드는 서버에서 데이터 혹은 리소스를 생성 혹은 수정하기 위한 목적으로 사용됩니다. ( Create, Update )
GET메서드는 서버로 보내는 데이터가 URL에 노출되어 보안에 취약한 문제가 있고,
POST메서드는 서버로 보내는 데이터가 Request의 Body에 들어있기에 보안에 좀 더 안전합니다.
```

`HTTP GET`
- 서버의 데이터와 리소스를 가져오기 위한 요청
- 서버의 리소스를 생성/수정하면 안됨.
- 요청을 위한 추가 정보 ( 파라미터 )들은 URL에 Query String 형태로 포함됨.
    - 이런 이유료, 서버에 많은 데이터를 보내지 못함. ( 일반적으로 255 길이 문자 )
    - 요청 URL에 서버에 보내는 데이터가 노출되어 보안 이슈가 있음.
    - 보안이 필요없는 요청인 경우 적합
- 요청의 결과가 브라우저 캐시/히스토리/북마크에 남음
- ASCII character만 허용됨.
- Encoding Type은 `application/x-www-form-urlencoded`
- Spring의 `@RequestParam`


`HTTP POST`
- 서버에 특정 데이터/리소스를 생성하거나 수정하기 위한 요청
- 서버로 보내는 정보는 RequestBody에 들어있음.
    - 보낼 수 있는 데이터의 양에 제한이 없음.
    - GET 메서드를 통한 요청보다 좀 더 안전함 (보안)
- 요청의 결과가 브라우저 캐시/히스토리/북마크에 남지 않음
- ASCII character가 아닌 모든 데이터 타입이 허용됨.
- Encoding Type은 `application/x-www-form-urlencoded` 이거나, `multipart/form-data`를 사용함.
- Binary Data를 사용하는 경우 `multipart/form-data`를 사용함.
- Spring의 `@RequestBody`