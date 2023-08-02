# HTTP Status Code

> Status Code란 클라이언트가 보낸 HTTP request 에 대한 서버의 응답 코드이다

응답코드는 크게 5가지의 분류로 나뉜다. 

- 100 번대 
- 200 번대
- 300 번대
- 400 번대
- 500 번대



## Status Code 1--

요청을 받았으며 작업을 계속함을 나타내는 Status Code



## Status Code 2--

클라이언트가 요청한 HTTP request를 성공적으로 수신해 이해했고, 성공적으로 처리됨을 나타내는 Status Code

- 200 : 클라이언트의 요청 성공
- 201 : 리소스 생성 성공
- 204 : 요청 성공, body값은 보내지 않음



## Status Code 3--

요청을 완료하기 위해 추가 작업 조치가 필요함을 나타내는 Status Code



## Status Code 4--

클라이언트의 요청에 문제가 있을 경우 나타내는 Status Code

- 400 : 클라이언트가 잘못된 요청방식으로 요청을 했을 경우 (잘못된 body)
- 401 : 인증이 되지 않은 상태에서 리소스에 접근 할 경우 
- 403 : 인증은 되었지만 해당 리소스에 접근 권한이 없는 경우 



## Status Code 5--

서버에서 예상치 못한 에러가 발생한 경우 나타내는 Status Code

- 502 : 서버에서 예상하지 못한 에러가 발생 