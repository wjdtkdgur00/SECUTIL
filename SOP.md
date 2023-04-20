# Same Origin Policy (SOP)
브라우저는 인증 정보로 사용될 수 있는 쿠키를 브라우저 내부에 보관합니다. 그리고 <U>이용자가 웹 서비스에 접속할 때, 브라우저는 해당 웹 서비스에서 사용하는 인증 정보인 쿠키를 HTTP 요청에 포함시켜 전달합니다.</U> 이와 같은 특징은 사이트에 직접 접속하는 것에만 한정되지 않습니다. 브라우저는 웹 리소스를 통해 간접적으로 타 사이트에 접글할 때도 인증 정보인 쿠키를 함께 전송하는 특징을 가지고 있습니다.

이 특징 때문에 악의적인 페이지가 클라이언트의 권한을 이용해 대상 사이트에 HTTP 요청을 보내고, HTTP 응답 정보를 획득 하는 코드를 실행할 수 있습니다. 이는 정보 유출과 같은 보안 위협이 생길 수 있는 요소가 됩니다. 따라서, 클라이언트 입장에서는 가져온 데이터를 악의적인 페이지에서 읽을 수 없도록 해야합니다. 이것이 바로 브라우저의 보안 메커니즘인 <U>동일 출처 정책 (Same Origin Policy, SOP)</U> 입니다.
# Same Origin Policy의 오리진 (Origin) 구분 방법
<U>오리진은 프로토콜 (Protocol, Scheme), 포트 (Port), 호스트(Host) 로 구성</U>됩니다. 구성 요소가 <U>모두 일치해야</U> 동일한 오리진이라고 합니다. 
```https://same-origin.com/```
라는 오리진과 아래 URL을 비교했을 때 결과는 다음과 같습니다
```
1. https://same-origin.com/frame.html : Same Origin (Path만 다름)

2. http://same-origin.com/frame.html : Cross Origin (Scheme이 다름)

3. https://cross.same-origin.com/frame.html : Cross Origin (Host가 다름)

4. https://same-origin.com:1234/ : Cross Origin (Port가 다름)
```
# Same Origin Policy 제한 완화
SOP는 클라이언트 사이드 웹 보안에서 중요한 요소입니다. 하지만, 브라우저가 이러한 SOP에 구애 받지 않고 외부 출처에 대한 접근을 허용해주는 경우가 존재합니다. 예를 들면, 이미지나 자바스크립트, CSS 등의 리소스를 불러오는 
```<img>, <style>, <script>``` 등의 태그는 SOP의 영향을 받지 않습니다.

위 경우들 외에도 <U>웹 서비스에서 동일 출처 정책인 SOP를 완화하여 다른 출처의 데이터를 처리 해야 하는 경우도 있습니다.</U> 예를 들어 특정 포털 사이트가 카페, 블로그, 메일 서비스를 아래의 주소로 운영하고 있을때 브라우저는 각 서비스의 Host가 다르기에 각 사이트의 오리진이 다르다고 인식합니다.
```
ex)
카페: https://cafe.dreamhack.io

블로그: https://blog.dreamhack.io

메일: https://mail.dreamhack.io

메인: https://dreamhack.io
```
이러한 환경에서, 이용자가 수신한 메일의 개수를 메인 페이지에 출력하려면, 개발자는 메인 페이지에서 메일 서비스에 관련된 리소스를 요청하도록 해야합니다. 이 때, 두 사이트는 오리진이 다르므로 SOP를 적용받지 않고 리소스를 공유할 방법이 필요합니다.

위와 같은 상황에서 자원을 공유하기 위해 사용할 수 있는 공유 방법을 **교차 출처 리소스 공유 (Cross Origin Resource Sharing, CORS)** 라고 합니다. 교차 출처의 자원을 공유하는 방법은 CORS와 관련된 HTTP 헤더를 추가하여 전송하는 방법을 사용합니다. 이 외에도 JSON with Padding (JSONP) 방법을 통해 CORS를 대체할 수 있습니다.
# Cross Origin Resource Sharing
**교차 출처 리소스 공유 (Cross Origin Resource Sharing, CORS)** 는 HTTP 헤더에 기반하여 Cross Origin 간에 리소스를 공유하는 방법입니다. 발신측에서 CORS 헤더를 설정해 요청하면, 수신측에서 헤더를 구분해 정해진 규칙에 맞게 데이터를 가져갈 수 있도록 설정합니다.

**Figure 2** 와 **Figure 3**은 각각 웹 리소스를 요청하는 발신측 코드의 일부와 발신측의 HTTP 요청입니다.

```javascript
Figure 2

/*
    XMLHttpRequest 객체를 생성합니다. 
    XMLHttpRequest는 웹 브라우저와 웹 서버 간에 데이터 전송을
    도와주는 객체 입니다. 이를 통해 HTTP 요청을 보낼 수 있습니다.
*/

xhr = new XMLHttpRequest();

/* https://theori.io/whoami 페이지에 POST 요청을 보내도록 합니다. */

xhr.open('POST', 'https://theori.io/whoami');

/* HTTP 요청을 보낼 때, 쿠키 정보도 함께 사용하도록 해줍니다. */

xhr.withCredentials = true;

/* HTTP Body를 JSON 형태로 보낼 것이라고 수신측에 알려줍니다. */

xhr.setRequestHeader('Content-Type', 'application/json');

/* xhr 객체를 통해 HTTP 요청을 실행합니다. */

xhr.send("{'data':'WhoAmI'}");
```
```http
Figure 3

OPTIONS /whoami HTTP/1.1
Host: theori.io
Connection: keep-alive
Access-Control-Request-Method: POST
Access-Control-Request-Headers: content-type
Origin: https://dreamhack.io
Accept: */*
Referer: https://dreamhack.io/
```
표를 살펴보면, 발신측에서 POST 방식으로 HTTP 요청을 보냈으나, OPTIONS 메소드를 가진 HTTP 요청이 전달된 것을 확인할 수 있습니다. 이를 <U>CORS preflight</U>라고 하며, 수신측에 웹 리소스를 요청해도 되는지 질의하는 과정입니다.

**Figure 3**을 살펴보면, "Access-Control-Request"로 시작하는 헤더가 존재하는 것을 확인할 수 있습니다. 해당하는 헤더 뒤에 따라오는 **Method**와 **Header**는 각각 메소드와 헤더를 추가적으로 사용할 수 있는지 질의합니다.

위처럼 질의하면 서버는 **Figure 4**와 같이 응답하며, 다음은 응답 결과에 대한 설명입니다.
```
Header

Access-Control-Allow-Origin : 헤더 값에 해당하는 Origin에서 들어오는 요청만 처리합니다.

Access-Control-Allow-Methods : 헤더 값에 해당하는 메소드의 요청만 처리합니다.

Access-Control-Allow-Credentials : 쿠키 사용 여부를 판단합니다. 예시의 경우 쿠키의 사용을 허용합니다.

Access-Control-Allow-Headers : 헤더 값에 해당하는 헤더의 사용 가능 여부를 나타냅니다.
```
위 과정을 마치면 <U>브라우저는 수신측의 응답이 발신측의 요청과 상응하는지 확인하고, 그때야 비로소 POST 요청을 보내 수신측의 웹 리소스를 요청하는 HTTP 요청을 보냅니다.</U>
# JSON with Padding (JSONP)
JSONP 방식은 이미지나 자바스크립트, CSS 등의 리소스는 SOP에 구애 받지 않고 외부 출처에 대해 접근을 허용한다는 특징을 이용해 ```<script>``` 태그로 Cross Origin의 데이터를 불러옵니다. 하지만 ```<script>``` 태그 내에서는 데이터를 자바스크립트의 코드로 인식하기 때문에 **Callback** 함수를 활용해야 합니다. Cross Origin에 요청할 때 callback 파라미터에 어떤 함수로 받아오는 데이터를 핸들링할지 넘겨주면, 대상 서버는 전달된 Callback으로 데이터를 감싸 응답합니다.

다만 JSONP는 CORS가 생기기 전에 사용하던 방법으로 현재는 거의 사용하지 않는 추세이기 때문에, 새롭게 코드를 작성할 때에는 CORS를 사용해야 합니다.
# 용어 정리
```
* Same Origin Policy (SOP): 동일 출처 정책, 현재 페이지의 출처가 아닌 다른 출처로부터 온 데이터를 읽지 못하게 하는 브라우저의 보안 메커니즘

* Same Origin: 현재 페이지와 동일한 출처

* Cross Origin: 현재 페이지와 다른 출처

* Cross Origin Resource Sharing (CORS): 교차 출처 리소스 공유, SOP의 제한을 받지 않고 Cross Origin의 데이터를 처리 할 수 있도록 해주는 메커니즘
```