# MVC 패턴의 이해와 실습
### 1.뷰 템플릿과 MVC패턴
----------------------------------------------------
* 1-1 뷰템플릿이란?

  * 웹페이지에 로그인시 사용자의 수만큼 웹페이지가 있는 것 일까? 만약 그렇다면 개발자들은 상당히 괴롭고 힘들것이다
  * 새로운 사용자가 가입할 때마다 매번 새로운 웹 페이지를 만들어 줘야 하기 떄문이다.
  * 이러한 문제점을 해결하기 위해 개발자들은 뷰템플릿이란 기술을 만들었다.
  * 뷰 템플릿(View template)은 화면을 담당하는 기술이다
  * 웹 페이지(view)를 하나의 틀(template)로 만들고 여기에 변수를 삽입하여 서로 다른 페이지를 보여준다.
  * 여기서는 Mustache 라는 템플릿엔진을 사용했다.(개인적인 생각으로 학원에서 프로젝트를 할 때 사용했던 Thymeleaf 보다 사용하기 쉬운것 같다.)

* 1-2 MVC 패턴

  * 화면을 담당하는 뷰 템플릿은 간단하게 `뷰`라고 한다
  * 뷰는 컨트롤러와 모델과 같이 사용된다.
  * 컨트롤러(`controller`)는 클라이언트 요청에 따라 서버에서 이를 처리하는 역할을 한다
  * 모델(`model`)은 데이터를 관리하는 역할을 한다
  * 웹페이지를 화면(`view`)에 보여주고, 클라이언트의 요청을 받아 처리하고(`controller`), 데이터를 관리하는(`model`) 역할을 나누는  기법을 MVC패턴이라도 한다

### 2.MVC패턴을 활용한 뷰 템플릿 페이지 만들기
---------------------------------------------------
* 2-1 뷰템플릿 페이지 만들기
  * src > main > resources > template 에 greetings.mustache 파일을 만든다.
    ```html
     <body>
      <h1> xx님,  반갑습니다.</h1>
    </body>
    ```
  * 이 페이지를 웹 브라우저에서 보려면 컨트롤러와 모델을 이용하여야 한다.
* 2-2 컨트롤러를 만들고 실행하기

  * src > java > main 안에 있는 패키지 com.example.firstproject에 controller패키지를 생성하고
  * FirstController 클래스를 생성한다
    ```java
     @GetMapping("/hi")
    public String niceToMeetYou(Model model) {
        return "greetings";//greetings.mustache 파일 반환
    }
    ```
  * localhost:8080/hi 로 접속하면 greetings.mustache 파일을 찾아 반환하라는 뜻

         > `???, ????!` 이라는 깨집현상 발생시 application.properties 파일에
         >  server.servlet.encodin.force=true 추가
 * 2-3 모델 추가하기
  * FirstController - 모델을 추가하여 컨트롤러의 매개변수를 받아옴
    ```java
    @GetMapping("/hi")
    public String niceToMeetYou(Model model) {
        model.addAttribute("username", "인모"); //model.addAttribute("변수명", 변숫값);
        return "greetings";//greetings.mustache 파일 반환
    }
    ```
   * greetings.mustache - 머스테치 문법을 사용해 템플릿에 변수 삽입
     ```html
       <body>
       <h1> {{username}}님,  반갑습니다.</h1>
       </body>
     ``` 

### 3. MVC의 역할과 실행 흐름 이해하기
 * 웹 서비스는 클라이언트의 요청에 대한 서버의 응답으로 동작한다. 스프링부트가 서버의 역할을 한다
 * 컨트롤러가 클라이언트의 요청을 받고, 뷰가 최종 페이지를 만들고, 모델이 최종 페이지에 쓰일 데이터를 뷰에 전달한다

* 3-1 /hi 페이지의 실행 흐름

### 4. 뷰 템플릿 페이지에 레이아웃 적용하기
 *  4-1 레이아웃이란 화면에 요소를 배치하는 일을 말한다.

             > <header>  -> 내비게이션
             > <content> -> 사용자가 볼 핵심내용
             > <footer>  -> 사이트 정보
 * greetings.mustache 파일을 쉽고 빠르게 꾸미기 위해 부트스트랩을 사용했다 (`v5.0.2`  사용)
 * <navgation> - header
   
  ```html
   <nav class="navbar navbar-expand-lg navbar-light bg-light">
    (중략)
   </nav>
   ```
 *  <site info> - footer
```html
 <div class="mb-5 container-fluid">
    <hr>
    <p> &copy; CloudStudying | <a href="#">Privacy</a> | <a href="#"> Term </a></p>

</div>
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/js/bootstrap.bundle.min.js" integrity="(중략)" crossorigin="anonymous"></script>
```

<cnotent> 

 ```html
<!--content-->
<div class="bg-dark text-white p-5">
    <h1>{{username}}님, 반갑습니다.</h1>
</div>
```
 * 이렇게 한 후  localhost:8080/hi 로 접속하면
  ![image](https://github.com/user-attachments/assets/e599d268-7ef7-4414-8329-731dd507d12f)
 * 레이 아웃이 적용된 /hi 페이지가 나온다
* 4-2 템플릿 화
  * 템플릿화 한다는 말은 코드를 하나의 틀로 만들어 변수화한다는 말이다
  * greetings.mustache 의 컨텐츠 영역만 남기고 그 위와 아래 코드를 지운 후 {{>header}}, {{>footer}} 파일로 만들어 보자
  * template 디렉터리에 layouts 디렉터리를 추가한다.
  * header.mustache / footer.mustache 파일을 만든다
  * 컨텐츠 영역을 제외한 위 아래 부분을 각각의 파일에 붙여 넣는다
  * {{>layouts/header}}, {{>layouts/footer}} 를 필요한 구역에 입력한다

     ```html
    {{>layouts/header}}

    <div class="bg-dark text-white p-5">
    <h1>{{username}}님, 반갑습니다.</h1>
    </div>
    {{>layouts/footer}}
    ```
    ![image](https://github.com/user-attachments/assets/641b1196-aadd-49e4-82a4-84861b80641c)

  * 이렇게 한 후 localhost:8080/hi 로 접속하면 템플릿화 하기 전에 페이지와 동일하게 적용된다
    

       

