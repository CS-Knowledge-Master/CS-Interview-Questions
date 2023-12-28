# google.com 등 url을 입력했을 때 Rander까지 일어나는 일

1. 도메인 입력시 그 도메인의 맞는 IP를 찾기위해 호스트파일, 캐시에서 탐색한다.
   > 있다면 도메인의 맞는 IP 를 반환하고 아니라면 2번
2. DNS 서버에 요청한다.

- ISP를 통하여 서버의 IP를 찾기위해 DNS query를 전달하여 찾을 때 까지 재귀적으로 반복한다.

3. 전달받은 IP주소를 TCP(신뢰성, 연결성)를 사용하여 서버로 전송한다.
   > IP는 비신뢰성과 비연결성이 특징이기 때문에, IP만으로는 통신할 수 없다.
4. WAS와 DB 에서 동적파일(java, python js{node, express ...}), 정적파일(html, css, javaScript을 서로 담당하여 파일을 작업한다.
5. WAS에서 웹서버로 파일을 전송하고, 웹서버는 브라우저에게 html 결과를 전달한다.
   > 이때 과정에서 status code를 통해 전달한다.
6. Critical Rendering Path 를 이용하여 Rander한다 (프론트 단)
   > HTML parsing -> DOM tree -> CSS parsing -> CSSOM tree
   > -> 위의 두 가지 tree를 혼합하여 Render tree
   > -> Rendering engine 이 레이아웃을 계산하고 페인팅으로 픽셀을 그리면서 화면에 출력한다.
