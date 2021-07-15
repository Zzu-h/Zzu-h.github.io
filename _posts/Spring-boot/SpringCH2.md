# 서블릿 내부 동작
서블릿 - JVM 기반에서 웹 개발을 하기 위한 API 

## 서블릿의 생명 주기
서블릿은 자신만의 생명주기를 가지고 있다.    
초기화, 서비스, 소멸의 3단계로 구성
- 초기화
    - 로드한 서블릿의 인스턴스를 생성하고 리소스를 로드하는 등 클래스 생성자의 초기화 작업과 동일한 역할을 수행
- 서비스
    - 클라이어트의 요청에 따라서 호출할 메서드를 결정함
- 소멸
    - 서블릿이 언로드
    - 언로드는 런타임 오류가 발생하거나 서블릿 컨테이너가 종료되었을 때 발생함
        - 이때는 메서드 호출 결과가 정상적으로 호출 되지 않음

### 서브릿 초기화 & init 메서드
```java
@WebServlet("/init")
public class InitServlet extends HttpServlet{
    @Override
    public void init() throws ServletException{
        System.out.println("init call");
    }
}
```
HttpSevlet을 상속받아서 서블릿을 만든다.
- 초기화 메서드 이므로 한 번만호출된다.
- URL 매핑은 WebServlet 어노테이션으로 작성
- 초기화 시점에 init 메서드가 호출 되므로 새로고침을 누르더라도 "init call"은 출력되지 않는다.
- 파라미터를 전달하고 싶은 경우 servletConfig를 사용한다.

# 서블릿 활용
## HTTP 요청과 응답
### GET 요청 처리
doGet메서드를 통해 GET 메서드 방식의 요청을 응답받을 수 있다.    
HttpServletRequest와 HttpServletResponse를 파라미터로 전달받을 수 있다.
- HttpServletRequest
    - 요청에 대한 정보
- HttpServletResponse
    - 브라우저에서 정보를 표현하기 위해 사용
```java
@WebServlet(name="HelloServlet", urlPartterns ={"/helloget"})
public class HelloServlet extends HttpServlet{
    protected void doGet(HttpServletRequest request, HttpServletResponse response)
    throws SErvletException, IOException {
        System.out.println("doGet 메서드 호출");
        response.setCharacterEncoding("utf-8");
        PrintWriter writer = response.getWriter();
        // contentType 정의
        response.setContentType("text/html");
        writer.println("<html>");
        writer.println("<head>jpub java werservice</head>");
        writer.println("<body>get 요청 예제 입니다.</body>");
        writer.println("</html>");
    }
}
```

### POST 요청 처리
```java
@WebServlet(name="HelloServlet2", urlPartterns ={"/hellopost"})
public class HelloServlet2 extends HttpServlet{
    protected void doPost(HttpServletRequest request, HttpServletResponse response)
    throws SErvletException, IOException {
        System.out.println("doPost 메서드 호출");
    }
}
```
- Post 요청에 대해서만 처리할 수 있는 메서드라 URL로 접속 시 에러 발생

### HTML 폼 데이터 전송
FORM 태그의 두 가지 속성
- action
    - 요청을 보낼 경로
- method
    - input 태그의 속성
```html
<form method="post" action="postend">
    <input type="text" name="user" placeholder="Username">
    <input type="password" name="pwd" placeholder="Password">
    <input type="submit" class="login login-submit" value="login">
</form>
```
사용자 폼 태그에서 입력한 user필드와 pwd 필드의 값을 읽을 수 있도록 하려면 서블릿의 urlPatterns의 값을 폼의 action 속성값과 동일하게 postend와 일치해야 한다.
```java
@WebServlet(name="LoginServlet", urlPartterns ={"/postend"})
public class LoginServlet extends HttpServlet{
    protected void doPost(HttpServletRequest req, HttpServletResponse resp)
    throws SErvletException, IOException {
        System.out.println("doPost 메서드 호출");
        resp.setCharacterEncoding("UTF-8");
        req.setCharacterEncoding("UTF-8");
        PrintWriter writer = resp.getWriter();

        resp.setContetType("text/html");

        String user = req.getParameter("user");
        String pwd = req.getParameter("pwd");
        writer.println("<html>");
        writer.println("<head><title>Login Servlet</title></head>");
        writer.println("<body>");
        writer.println("전달받은 이름은" + user+ "이고" + "<br/>" + "비밀번호는 "+pwd+"입니다.");
        writer.println("</body>");
        writer.println("</html>");
    }
}
```

## 멀티파트
멀티파트는 바이너리 데이터 전송을 위해 사용 - 파일 업로드 등
```html
<form method="post" action="upload" enctype="multipart/form-data">
    File:
    <input type="file" name="file" id="file">
    업로드할 서버 경로:
    <input type="text" value="c:/upload" name="destination">
    <br/>
    <input type="submit" value="upload">
</form>
```
enctype="multipart/form-data"을 설정해 주어야 파일 업로드 기능을 수행
- 이 예제는 c드라이브에 upload 디렉토리를 만든다.
```java
@WebServlet(name="uploadServlet", urlPartterns ={"/upload"})
@MultiPartConfig(
    fileSizeThreshold = 1024 * 1024 * 2, // 2mb
    maxFileSize = 1024 * 1024 * 10, // 10mb
    maxrequestSize = 1024 * 1024 * 50, // 50mb
    location = "c:/upload" // 파일 저장 위치
)
public class UploadServlet extends HttpServlet{
    protected void doPost(HttpServletRequest request, HttpServletResponse response)
    throws SErvletException, IOException {
        System.out.println("doPost 메서드 호출");
        response.setCharacterEncoding("UTF-8");
        request.setCharacterEncoding("UTF-8");
        // 경로
        final String path = request.getParameter("destination");
        // 파일
        final Part filePart = request.getPart("file");
        // 파일 이름
        final String fileName = getFileName(filePart);
        PrintWriter writer = response.getWriter();

         try (OutputStream out = new FileOutputStream(new File(path + File.separator + fileName)); InputStream filecontent = filePart.getInputStream()) {
            int read = 0;
            final byte[] bytes = new byte[1024];

            while ((read = filecontent.read(bytes)) != -1) {
                out.write(bytes, 0, read);
            }

            writer.print("new File: " + fileName + path + "에 생성되었습니다.");

        } catch (FileNotFoundException fne) {
            System.out.println(fne.getMessage());
        }
    }


    private String getFileName(final Part part) {
        final String partHeader = part.getHeader("content-disposition");
        System.out.println("Part Header = {0}" + partHeader);
        for (String content: part.getHeader("content-disposition").split(";")) {
            if (content.trim().startsWith("filename")) {
                return content.substring(
                        content.indexOf('=') + 1).trim().replace("\"", "");
            }
        }
        return null;
    }
}
```

# 서블릿 관련 객체들
## 필터
웹 클라이언트의 요청에 대해서 필요한 사전작업이 있을 경우 필터를 사용한다.
- 필터는 서블릿의 생명주기처럼 init과 destroy 메서드가 존재
- 필터 기능 사용을 위한 doFilter 메서드 존재
```java
@WebFilter("*.jsp")
public class FilterEx implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {

    }

    @Override
    public void doFilter(ServletRequest req, ServletResponse res, FilterChain chain) throws IOException, ServletException {
        res.setContentType("text/html");
        res.setCharacterEncoding("UTF-8");
        PrintWriter out = res.getWriter();
        out.println("필터 동작 전");
        chain.doFilter(req, res);
        out.println("필터 동작 후");
    }

    @Override
    public void destroy() {

    }
}
```
- `*.jsp*`: URL에 상관없이 jsp 파일에 대해서 적용하기 위함
- 필터는 URL을 기준으로 요청에 대한 전처리를 할 수 있다.
- 또한, 서블릿에 대해서도 매핑이 가능함
    - URL 대신 서블릿을 기준으로 할 땐 서블릿 이름으로 매핑함
- 필터 체인(filter chain)
    - 필터는 여러 개를 등록 사용 가능
    - 여러 개의 필터를 등록해서 처리하는 것을 의미함

## 쿠키
- 쿠키
    - 사용자가 사이트를 방문했을 때 사용자의 컴퓨터에 저장되는 정보
- 구성 요소
    - 이름
        - 각각의 쿠키의 값을 식별하기 위한 Key
    - 값
        - 특정 이름으로 쿠키에 지정된 값
    - 유효 시간
        - 쿠키의 유지 시간
    - 도메인
        - 쿠키를 전송할 도메인
    - 경로
        - 쿠키를 전송할 요청 경로
- 쿠키는 HTTP 헤더 정보에 포함되어 전달됨

### 쿠키 생성
쿠키 생성자를 통해 쿠키를 생성한다.    
`Cookie jcookie = new Cookie(name, value);`

```java
@WebServlet(urlPatterns = "/newcookie")
public class CookieCreateServlet extends HttpServlet {
    @Override
    public void doGet(HttpServletRequest req,
                      HttpServletResponse resp) throws ServletException,
            IOException {
        resp.setCharacterEncoding("UTF-8");
        resp.setContentType("text/html");
        PrintWriter out = resp.getWriter();
        out.println("<html><head><title> 쿠키 예제</title></head><body>");
        out.println("<br/>");

        Cookie jcookie = new Cookie("jpub", "books");
        jcookie.setMaxAge(3600);
        resp.addCookie(jcookie);
        out.println("<a href='/readcookie'>readcookie</a></body></html>");
    }
}
```
- 쿠키를 사용할 때 일반적으로 도메인 기반으로 사용

### 쿠키값 수정 및 삭제
쿠키값을 변경하려면 같은 이름으로 쿠키를 생성하고 새로운 값을 지정한다.    
`Cookie jcookie = new Cookie(name, New value);`
```java
@WebServlet(urlPatterns = "/modicookie")
public class CookieModifyServlet extends HttpServlet{
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        resp.setCharacterEncoding("UTF-8");
        resp.setContentType("text/html");
        PrintWriter out = resp.getWriter();
        out.println("<html><head><title> cookie 수정 </title></head>");
        out.println("<body>");
        Cookie[] cookies = req.getCookies();
        if (cookies != null) {
            for (Cookie cookie : cookies) {
                if (cookie.getName().equals("jpub")) {
                    Cookie modifiedCookie = new Cookie("jpub", "read");
                    resp.addCookie(modifiedCookie);
                }
            }
        }
        out.println("<a href='/readcookie'>readcookie</a></body></html>");
    }

}
```
- 쿠키를 삭제하는 API는 존재하지 않는다.
    - 하지만 쿠키의 유효시간은 바꿀 수 있다.
    - setMaxAGe(0)를 통해서 쿠키값을 무효화할 수 있다.
- 쿠키값 한글 입력 시 `URLEncoder`를 이용해서 문자열을 감싸 준다.
    - `Cookie newCookie = new Cookie("kor", URLEncoder.encode("데이터","UTF-8"))`

## 세션
- 세션(session)
    - 서버와 클라이언트의 유효한 커넥션을 식별하는 정보
- 서버는 클라이언트가 요청을 보내면 요청을 식별할 수 있는 ID를 부여함
    - 이때 ID를 **세션 ID**라 한다.
    - 세션 ID는 JSESSIONID 이름으로 쿠키로 저장됨
    - 클라이언트가 사이트에 재접속할 때마다 세션ID를 서버에 전달함

### 세션 생성
현재 생성된 세션 정보는 request 객체에 꺼내서 사용할 수 있다.    
`request.getSession()`

```java
@WebServlet("/session")
public class DefaultSessionServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        resp.setContentType("text/html");
        resp.setCharacterEncoding("UTF-8");
        PrintWriter out = resp.getWriter();
        out.println("<html><head><title>세션</title></head><body>");
        HttpSession session = req.getSession();
        out.println("sessionId::" + session.getId() + "<br/>");
        out.println("session created::" + session.getCreationTime() + "<br/>");
        out.println("session lastAccessTime" + session.getLastAccessedTime() + "<br/>");
        out.println("</body></html>");
    }
}
```
- `getId`: 세션의 고유 아이디를 얻을 수 있는 메서드
- `getCreationTime`: 세션이 생성된 시간을 얻을 수 있는 메서드
- `getLastAccessTime`: 클라이언트가 가장 마지막에 세션에 접근한 시간을 얻을 수 있는 메서드

### 세션 값 저장 및 삭제
`Seesion.setAttribute('이름', 값)` 을 통해 저장할 수 있다.

```java
@WebServlet(urlPatterns = "/createse")
public class CreateSessionValueServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        resp.setContentType("text/html");
        resp.setCharacterEncoding("UTF-8");
        PrintWriter out = resp.getWriter();
        out.println("<html><head><title>세션</title></head><body>");

        HttpSession session = req.getSession();
        session.setAttribute("jpub", "book");
        out.println("세션이 생성되었습니다.");
        out.println("<a href='/readse'>세션 읽기</a></body></html>");
    }
}
```

# 디자인 패턴 활용
- Java EE 패턴
    - 자바 기반의 엔터프라이즈 웹 애플리케이션 개발을 위한 패턴
- Java EE 패턴 목록
    - Intercepting Filter
        - 요청에 대한 전처리 및 후처리
    - Front Controller
        - 요청에 대한 처리를 관리하는 중앙 컨트롤러
    - View Helper
        - 뷰의 표현을 위해 비즈니스 로직을 가지고 있는 개념상의 Helper
    - Compsite View
        - 레고 블럭 같은 작은 뷰ㅜ들을 조합해서 만드는 전체의 뷰
    - Service to worker
        - Front Controller와 View Helper Pattern을 이용해 dispatcher 컴포넌트를 구성
    - Dispatcher View
        - Service to Worker와 동일
        - 차이점은 뷰에 대한 처리 중에 수행되어야 함
    
## Front Controller Pattern
- 컨트롤러가 공통 요청을 먼저 수행하고 뷰를 호출하는 패턴
- 예를 들어, 게시판의 리스트 화면, 쓰기 화면 요청에 대해서 화면에 바로 전달하지 않고 컨트롤러를 통해 전달하여 뷰를 받음
- 서버 측에서 메서드를 이용하여 화면을 전환하는 방법에는 두 가지 존재
    - Response 객체의 sendRedirect 메서드
        - 속성을 저장할 수 없고 다른 로직을 추가할 수 없다.
    - RequestDispatcher 객체의 forward 메서드
        - 서버 내부에서만 르름이 이동하므로 속성을 저장할 수 있다.
        - 클라이언트에게 바로 전달하지 않고 원하는 작업을 처리한 후에 응답을 ㅈ넌환할 수 이ㅏㅆ다.
            - 따라서 컨트롤러를 만들 때 많이 사용하는 메서드이다.
### sendRedirect
- HttpSErvletResponse에 속한 메서드이며 다음과 같이 사용
- `response.sendRedirect(경로);`
### forward
- forward 메서드를 사용하기 위해서 requestDispatcher 객체를 생성해야 함
    ```java
    RequestDispatcher rd = request.getRequestDispatcher(경로);
    rd.forward(ServletRequest request, ServletResponse response);
    ```
- RequestDispatcher 객체의 경로는 **절대경로로 지정한다**.
    - '../'와 같이 상대경로를 이용할 수 없다.
- 이 메소드를 사용 시 ServletRequest와 ServletResponse 객체를 전달한다.
    - 따라서, session에 속성을 저장하고 포워딩한 곳에서 사용이 가능함

```java
if(url == "list"){
    RequestDispatcher rd = req.getRequestDispatcher(url);
    rd.forward(request, response);
}
else if(url = "write"){
        RequestDispatcher rd = req.getRequestDispatcher(url);
    rd.forward(request, response);
}
```
- 컨트롤러에서 화면을 보여주는 구문은 if문으로 분기처리하게 된다.
    - 이는 URL이 변경되거나 뷰가 변경될 때마다 컨트롤러를 변경하게 된다.
    - 즉, 유지보수가 어려워 짐
- 위 문제를 해결하기 위해 command pattern을 이용함

### command pattern
- 커맨드 패턴
    - 명령을 객체 안에 캡슐화해서 저장함
    - 이를 통해 컨트롤러와 같은 클래스를 수정하지 않고 재사용할 수 있게 하는 패턴
![command pattern](https://upload.wikimedia.org/wikipedia/commons/8/8e/Command_Design_Pattern_Class_Diagram.png)

- Invoker 역할은 컨트롤러가 담당
- 다음의 [링크](https://github.com/thecodinglive/JPub-JavaWebService/tree/master/ch02/src/main/java/info/thecodinglive/pattern)르 참고
