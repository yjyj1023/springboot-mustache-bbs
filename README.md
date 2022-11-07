# springboot-mustache-bbs

# 2. SpringBoot MVC
## 2.1 이번주 목표
- CRUD 기능의 게시판 만들어서 Docker로 배포하기
> CRUD 기능이란?
>- Create - 글 쓰기
>- Read - 쓴 글 읽기
>- Update - 쓴 글 수정
>- Delete - 쓴 글 지우기

- 게시판 예시
![](https://velog.velcdn.com/images/lyj1023/post/154dc327-1ce4-4024-b85b-8001af37ae12/image.png)

- 참고 소스코드: https://github.com/eternityhwan/osc-board-back

## 2.2 MVC 패턴이란?
- 아래 그림은 웹서비스의 동작 방식이다.

![](https://velog.velcdn.com/images/lyj1023/post/9404f6ac-1392-4742-9837-37e008d65a9e/image.png)

1) 웹 서비스는 클라이언트와 서버의 소통으로 이루어져 있다.
2) 스프링 부트가 서버의 역할을 수항한다.
3) MODLE, VIEW, CONTROLLER의 역할 분담으로 서버 역할을 수항한다.

- 아래 그림은 MVC 패턴이다.

![](https://velog.velcdn.com/images/lyj1023/post/6665e363-8202-47cf-9587-d0c010613f1a/image.png)

1) 먼저 클라이언트가 컨트롤러에게 요청을 보낸다.
2) 뷰는 최종 페이지를 만들어주고
3) 모델은 데이터를 최종페이지(뷰)에게 보내주는 역할을 수행한다.

- 위 MVC패턴을 식당으로 비유하자면 클라이언트가 웨이터(컨트롤러)에게 주문을 넣으면 웨이터가 주문을 받고 주방장(뷰)이 요리를하고 식재료 담당자(모델)가 식재료(데이터)를 준비해서 다시 웨이터를 통해 클라이언트에게 전달하는 방식과 같다.

![](https://velog.velcdn.com/images/lyj1023/post/d7c9bc9a-c511-4448-9a0b-f3b738220b64/image.png)

- 정리해보자면 컨트롤러는 클라이언트로부터 요청을 받고 뷰는 최종 페이지를 보여주고 모델은 뷰 페이지에 데이터를 넘겨준다고 할 수 있다.

![](https://velog.velcdn.com/images/lyj1023/post/4ef8db49-669f-4a2f-896f-8d7e770f2e1b/image.png)


## 2.3 Mustache 사용하기
### 2.3.1 프로젝트 생성하기(커뮤니티 버젼)
- https://start.spring.io/
- 위 홈페이지에 접속해서 아래와 같이 프로젝트를 설정하고 인텔리제이에서 연다.
![](https://velog.velcdn.com/images/lyj1023/post/2a2fc27b-3b15-476b-ba09-284fe9958742/image.png)
> __주의!__ 처음에는 의존성 추가에 jpa를 넣지 않는다(추가하면 DB정보를 넣어줘야함)

- application.properties를 application.yml로 바꾼후 깃에 커밋한다.
![](https://velog.velcdn.com/images/lyj1023/post/8e557db3-c494-4f65-8150-b297e02d7a2f/image.png)

### 2.3.2 Controller 만들기
```java
@Controller
public class MustacheController {

    @GetMapping(value = "/hi")
    public String mustacheCon(Model model){
        model.addAttribute("username","rok");
        return "greetings";
    }
}
```
1) 클래스에 컨트롤러라고 선언해주는 @Controller 어노테이션을 붙인다.
2) mustacheCon 메서드가 /hi 라는 요청으로 동작한다.(hi 라는 Endpoint매핑)
3) Model model을 파라미터로 받는다.(MVC의 M)
4) mustacheCon 메서드는 반환값으로 greetings 페이지를 보여준다.(view리턴)


### 2.3.3 greetings.mustache 만들기
- 아래 경로에 greetings.mustache을 만든다.

![](https://velog.velcdn.com/images/lyj1023/post/edfa888e-8cff-44fd-931e-a4fa34f87913/image.png)

```
<!--- content --->
<div >
    <h1>{{username}} bootstrap + mustache</h1>
</div>
```
- Mustache프레임웍은 .mustache 가 붙어있는 파일을 읽어서 View로 쓴다.
- controller 메소드의 `model.addAttribute("username", "rok");`를 {{username}}(중괄호 두개로 감싼 자리)에 값을 바인딩한다.

### 2.3.4 실행하기
- 위와 같이 설정해서 MustacheBnsApplication을 실행하고 아래 주소에 접속해서 잘 나오는지 확인한다.
- http://localhost:8080/hi

![](https://velog.velcdn.com/images/lyj1023/post/5e952cbe-7415-4501-88c7-9ae3a88bfeb8/image.png)

### 2.3.5 PathVariable로부터 넘어온 값 넘기기
- id라는 PathVariable을 생성해 url에서 넘어온 값을 넘길수도 있다.

`[MustacheController.java]`
```java
@Controller
public class MustacheController {

    @GetMapping(value = "/hi")
    public String mustacheCon(Model model){
        model.addAttribute("username","rok"); // view에 값을 넘긴다.
        return "greetings"; // greetings라는 이름의 view를 리턴한다.
    }

    @GetMapping(value = "/hi/{id}")
    public String mustacheCon2(@PathVariable String id, Model model){
        model.addAttribute("username","rok");
        model.addAttribute("id",id);
        return "greetings";
    }
}
```

`[greetings.mustache]`
```
<!--- content --->
<div >
    <h1>{{id}} {{username}} Hello!</h1>
</div>
```
- 결과
   - id, username, Hello!가 차례대로 출력되는 것을 확인 할 수 있다.
![](https://velog.velcdn.com/images/lyj1023/post/70ac38a9-e768-42ab-9443-592cf5b071c4/image.png)

> __한글 깨질 때__
>- application.yml에 아래와 같이 추가해준다.
>`[application.yml]`
>```
>server:
>  port: 8080
>  servlet:
>    encoding:
>      force-response: true
>```
>![](https://velog.velcdn.com/images/lyj1023/post/3c34f953-67af-4804-ac69-a5ce94911c8e/image.png)


## 2.4 부트스트랩 활용하기
- 부트스트랩을 활용하면 웹페이지를 예쁘게 꾸밀수 있다.
- 위에서 출력했던 웹페이지에 부트스트랩을 활용해 레이아웃을 추가해보자.

### 2.4.1 부트스트랩 시작하기
- https://getbootstrap.com/docs/5.2/getting-started/introduction/#quick-start
- 위 홈페이지에서 퀵스타트 2번을 적용해보자
```html
<!doctype html>
<html lang="en">
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Bootstrap demo</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.2/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-Zenh87qX5JnK2Jl0vWa8Ck2rdkQ2Bzep5IDxbcnCeuOxjzrPF/et3URy9Bv1WTRi" crossorigin="anonymous">
</head>
<body>
<h1>{{id}} {{username}} Hello!</h1>
<h1><button>버튼</button></h1>

<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.2.2/dist/js/bootstrap.bundle.min.js" integrity="sha384-OERcA2EqjJCMA+/3y+gxIOqMEjwtxJY7qPCqsdltbNJuaOe923+mo//f6V8Qbsw3" crossorigin="anonymous"></script>
</body>
</html>
```
![](https://velog.velcdn.com/images/lyj1023/post/ad6c80e9-e96f-437a-bb88-426afa6ed9af/image.png)

> __html적용하기__
> - html을 수정하고 새로 run할 필요 없이 아래 그림의 망치모양만 클릭해도 홈페이지에 적용된다.

>![](https://velog.velcdn.com/images/lyj1023/post/afa4786b-287e-465f-9dbf-5575b2640de8/image.png)


### 2.4.2 여러 요소들 넣어보기
- body태그에 각종 버튼과 리스트 그룹을 추가했다.
```html
<button type="button" class="btn btn-primary">Primary</button>
<button type="button" class="btn btn-secondary">Secondary</button>
<button type="button" class="btn btn-success">Success</button>
<button type="button" class="btn btn-danger">Danger</button>
<button type="button" class="btn btn-warning">Warning</button>
<button type="button" class="btn btn-info">Info</button>
<button type="button" class="btn btn-light">Light</button>
<button type="button" class="btn btn-dark">Dark</button>
<ul class="list-group">
    <li class="list-group-item active" aria-current="true">An active item</li>
    <li class="list-group-item">A second item</li>
    <li class="list-group-item">A third item</li>
    <li class="list-group-item">A fourth item</li>
    <li class="list-group-item">And a fifth one</li>
</ul>
```

![](https://velog.velcdn.com/images/lyj1023/post/5e3af631-b5dc-45ce-a58e-a4a96c810e7b/image.png)

> __주의할 점__
>- 부트스트랩은 모바일, PC, 태블릿 등 반응형이기 때문에 모양이 제한될 수 있다.
>- Grid 사용법을 익히고 커스터마이징 할 것!

### 2.4.3 네비게이션바 추가하기
- body태그의 본문 위쪽에 navbar를 추가한다.
```html
<body>
    <nav class="navbar navbar-expand-lg bg-light">
        <div class="container-fluid">
            <a class="navbar-brand" href="#">Navbar</a>
            <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
                <span class="navbar-toggler-icon"></span>
            </button>
            <div class="collapse navbar-collapse" id="navbarSupportedContent">
                <ul class="navbar-nav me-auto mb-2 mb-lg-0">
                    <li class="nav-item">
                        <a class="nav-link active" aria-current="page" href="#">Home</a>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link" href="#">Link</a>
                    </li>
                    <li class="nav-item dropdown">
                        <a class="nav-link dropdown-toggle" href="#" role="button" data-bs-toggle="dropdown" aria-expanded="false">
                            Dropdown
                        </a>
                        <ul class="dropdown-menu">
                            <li><a class="dropdown-item" href="#">Action</a></li>
                            <li><a class="dropdown-item" href="#">Another action</a></li>
                            <li><hr class="dropdown-divider"></li>
                            <li><a class="dropdown-item" href="#">Something else here</a></li>
                        </ul>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link disabled">Disabled</a>
                    </li>
                </ul>
                <form class="d-flex" role="search">
                    <input class="form-control me-2" type="search" placeholder="Search" aria-label="Search">
                    <button class="btn btn-outline-success" type="submit">Search</button>
                </form>
            </div>
        </div>
    </nav>
<h1>{{id}} {{username}} Hello!</h1>
<button type="button" class="btn btn-primary">버튼</button>

<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.2.2/dist/js/bootstrap.bundle.min.js" integrity="sha384-OERcA2EqjJCMA+/3y+gxIOqMEjwtxJY7qPCqsdltbNJuaOe923+mo//f6V8Qbsw3" crossorigin="anonymous"></script>
</body>
```
![](https://velog.velcdn.com/images/lyj1023/post/a08e6d25-a28e-4e70-88f8-8a7574d3b097/image.png)

### 2.4.4 footer 추가하기
- body태그의 본문 아래쪽에 footer를 추가한다.
```html
<div class="mb-5 container-fluid">
    <hr>
    <p>@ RokBoard <a href="localhost:8080">Privacy</a> <a href="#">Terms</a></p>
</div>
```
![](https://velog.velcdn.com/images/lyj1023/post/6a13ff09-5b15-4f9f-b257-29349d81b556/image.png)

## 2.5 레이아웃 파일 나누기
- header와 footer부분은 고정으로 수정되지 않으므로 따로 아래와 같이 파일을 나누어 greetings.mustache에만 수정되는 부분이 남을 수 있도록 파일을 분리한다.

![](https://velog.velcdn.com/images/lyj1023/post/51f1ac88-7ddf-4c5c-bc92-783a77631938/image.png)

- header.mustache에는 본문 위쪽을 전부 옮긴다.

`[header.mustache]`
```html
<!doctype html>
<html lang="en">
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Bootstrap demo</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.2/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-Zenh87qX5JnK2Jl0vWa8Ck2rdkQ2Bzep5IDxbcnCeuOxjzrPF/et3URy9Bv1WTRi" crossorigin="anonymous">
</head>
<body>
<nav class="navbar navbar-expand-lg bg-light">
    <div class="container-fluid">
        <a class="navbar-brand" href="#">Navbar</a>
        <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
            <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarSupportedContent">
            <ul class="navbar-nav me-auto mb-2 mb-lg-0">
                <li class="nav-item">
                    <a class="nav-link active" aria-current="page" href="#">Home</a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="#">Link</a>
                </li>
                <li class="nav-item dropdown">
                    <a class="nav-link dropdown-toggle" href="#" role="button" data-bs-toggle="dropdown" aria-expanded="false">
                        Dropdown
                    </a>
                    <ul class="dropdown-menu">
                        <li><a class="dropdown-item" href="#">Action</a></li>
                        <li><a class="dropdown-item" href="#">Another action</a></li>
                        <li><hr class="dropdown-divider"></li>
                        <li><a class="dropdown-item" href="#">Something else here</a></li>
                    </ul>
                </li>
                <li class="nav-item">
                    <a class="nav-link disabled">Disabled</a>
                </li>
            </ul>
            <form class="d-flex" role="search">
                <input class="form-control me-2" type="search" placeholder="Search" aria-label="Search">
                <button class="btn btn-outline-success" type="submit">Search</button>
            </form>
        </div>
    </div>
</nav>
```

- footer.mustache에는 본문 아래쪽을 전부 옮긴다.

`[footer.mustache]`
```html
<div class="mb-5 container-fluid">
    <hr>
    <p>@ RokBoard <a href="localhost:8080">Privacy</a> <a href="#">Terms</a></p>
</div>

<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.2.2/dist/js/bootstrap.bundle.min.js" integrity="sha384-OERcA2EqjJCMA+/3y+gxIOqMEjwtxJY7qPCqsdltbNJuaOe923+mo//f6V8Qbsw3" crossorigin="anonymous"></script>
</body>
</html>
```

- greetings.mustache는 아래처럼 수정한다.
- `{{>layouts/header}}`는 레이아웃 디렉토리에 있는 header 라는 파일을 가져다 쓰겠다는 뜻이다.
- id가 들어오는 부분에 에러가 생겨서 id가 들어오면 id를 출력하고 id가 들어오지 않으면 출력하지 않도록 수정한다.

`[greetings.mustache]`
```html
{{>layouts/header}}
<div>
    {{# id}}
        <h1>{{id}} {{username}} Hello! </h1>
    {{/ id}}
    {{^ id}}
        <h1>{{username}} Hello! </h1>
    {{/ id}}
</div>
<button type="button" class="btn btn-primary">버튼</button>
{{>layouts/footer}}
```

## 2.6 폼 추가하기
### 2.6.1 컨트롤러 추가
- Controller 디렉토리에 ArticleController 클래스를 추가한다.
- @RequestMapping을 쓰면 @GetMapping에 /articles 이하만 추가해주면 된다.
- /articles/new url로 들어오면 artices/new.mustache를 출력하는 메소드이다.

`[ArticleController.java]`
```java
@Controller
@RequestMapping("/articles")
@Slf4j // 로깅을 위한 어노테이션 log를 사용할 수있다.
public class ArticleController {
    @GetMapping(value = "/new")
    public String newArticleForm() {
        return "articles/new";
    }
}
```

### 2.6.2 new.mustache 추가
- 아래처럼 mustache 파일을 추가한다.

![](https://velog.velcdn.com/images/lyj1023/post/fc0e2d47-8c67-4552-94a6-b4fba7f56136/image.png)

`[new.mustache]`
```html
{{>layouts/header}}
<form action="/articles/posts" method="post">
    <input type="text" name="title">
    <input type="text" name="content">
    <button type="submit" class="btn btn-primary">Submit</button>
    <a href="/articles">back</a>
</form>
{{>layouts/footer}}
```
- `<form` 으로 시작한다.
- `action`: http request가 어디로 갈지 경로
- `method`: post 없으면 기본값은 get이다.
- `input name="title"`: title이라고 서버에서 받아주어야 한다.
- `input name="content"`: content이라고 서버에서 받아주어야 한다.
- `type=submit` : 이 속성이 들어있는 버튼 등을 누르면 데이터(title, content)를 전송한다.

### 2.6.3 결과 화면
- http://localhost:8080/articles/new
- 위 url로 들어갔을 때 아래와 같이 나오면 성공

![](https://velog.velcdn.com/images/lyj1023/post/8bcf3b00-a636-4f20-b6a4-17772df3883b/image.png)

### 2.6.4 createArticle 메소드 추가
- 이 폼 페이지에 작성한 글을 저장할 수 있는 메소드를 만들어 보자.

`[ArticleController.java]`
```java
@PostMapping(value = "/posts")
public String createArticle(ArticleDto form) {
    log.info(form.toString());

    return "";
}
```

- 롬복을 활용해 Getter와 ToString메소드를 넣지 않고도 사용할 수 있다.

`[ArticleDto.java]`
```java
package com.mustache.bbs.domain.dto;

import lombok.Getter;
import lombok.ToString;

@Getter
@ToString
public class ArticleDto {
    private Long id;
    private String title;
    private String content;

    public ArticleDto(Long id, String title, String content) {
        this.id = id;
        this.title = title;
        this.content = content;
    }
}
```

- 위 코드를 추가해서 실행한 후 아래와 같이 article/new에 title과 contents를 넣는다.

![](https://velog.velcdn.com/images/lyj1023/post/3bc9461a-5f86-4f2a-a086-32c49bb89943/image.png)

- 홈페이지는 에러가 나지만 로그를 확인해 보면 데이터를 받아오는 것을 확인 할 수 있다.

![](https://velog.velcdn.com/images/lyj1023/post/660338dd-22d1-46a5-a482-f715fd1f71f2/image.png)

- 아래 그림은 Form에서 Post의 처리 흐름이다.

![](https://velog.velcdn.com/images/lyj1023/post/60b8ad81-179c-4436-9cdd-eca1f20cfc46/image.png)

## 2.7 JPA 사용하기
### 2.7.1 JPA란?
- query작성을 해주는 framework이다.
- Entity = Row
- DB의 1개의(Row, Record)가 1개의 Class가 된다.


### 2.7.2 의존성 추가하기
- 아래처럼 build.gradle에 의존성을 추가한다.

![](https://velog.velcdn.com/images/lyj1023/post/aff48661-1eed-494a-8b2e-1ddb550253db/image.png)

### 2.7.3 Entity 생성하기

![](https://velog.velcdn.com/images/lyj1023/post/abfbe331-a805-4ef0-ac61-a8cac8cf15f8/image.png)

- 위 경로에 아래처럼 Entity(Article)을 생성한다.

`[Article.java]`
```java
package com.mustache.bbs.domain.entity;

import lombok.Getter;
import lombok.NoArgsConstructor;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.Id;

@Entity
@NoArgsConstructor
@Getter
public class Article {
    @Id
    @GeneratedValue
    private Long id;
    private String title;
    private String contents;

    public Article(String title, String contents) {
        this.title = title;
        this.contents = contents;
    }
}
```
- `@Entity`: JPA에서 객체로 다루겠다는 선언
- `@Id`: @Entity가 붙어있다면 꼭 붙여주어야 한다. Primary Key를 의미한다.
- `@GeneratedValue`: ID를 직접 생성하지 않고 자동으로 생성하도록 한 경우 붙인다.

### 2.7.4 ArticleDto에 toEntity() 생성하기
- ArticleDto에서 Article을 만들어 주는 toEntity()를 만든다.

`[ArticleDto.java]`
```java
package com.mustache.bbs.domain.dto;

import com.mustache.bbs.domain.entity.Article;
import lombok.Getter;
import lombok.ToString;

@Getter
@ToString
public class ArticleDto {
    private Long id;
    private String title;
    private String content;

    public ArticleDto(Long id, String title, String content) {
        this.id = id;
        this.title = title;
        this.content = content;
    }

    public Article toEntity(){
        return new Article(title, content);
    }
}
```

### 2.7.5 ArticleController 수정하기
- ArticleController의 createArticle를 수정한다.
- ArticleDto를 엔티티로 변환시켜 준다.

`[ArticleController.java]`
```java
@PostMapping(value = "/posts")
public String createArticle(ArticleDto form) {
    log.info(form.toString());
    Article article = form.toEntity();
    return "";
}
```

### 2.7.6  DB 정보를 추가하기
- JPA를 실행하려면 DB정보를 추가해야 한다.
- application.yml에 아래와 같이 추가해준다.

`[application.yml]`
```
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:8080/likelion-db
    username: root
    password: password
```

- MustacheBbsApplication.java의 환경변수를 아래와 같이 설정해 준다.

![](https://velog.velcdn.com/images/lyj1023/post/ca69259b-aea9-4e00-9729-3f5b9f4b128d/image.png)

### 2.7.7 Repository 만들기
- Repository를 만들어 보자(DAO의 추상화된 버전)

![](https://velog.velcdn.com/images/lyj1023/post/50116282-864e-4303-a6a3-7942b9deeada/image.png)

- 위 경로에 ArticleRepository를 생성한 후 아래와 같이 사용한다.

`[ArticleRepository.java]`
```java
package com.mustache.bbs.repository;

import com.mustache.bbs.domain.entity.Article;
import org.springframework.data.jpa.repository.JpaRepository;

public interface ArticleRepository extends JpaRepository<Article, Long> {
}
```

- @Autowired 대신 final을 사용해서 먼저 선언하고 save를 이용해 Repository에게 Entity를 DB에 저장하게 한다.

`[ArticleController.java]`
```java
public class ArticleController {
	
    //@Autowired 대신 final 사용
    private final ArticleRepository articleRepository;

    public ArticleController(ArticleRepository articleRepository) {
        this.articleRepository = articleRepository;
    }
    
...

    @PostMapping(value = "/posts")
    public String createArticle(ArticleDto form) {
        log.info(form.toString());
        Article article = form.toEntity();
        articleRepository.save(article);  //save메소드를 사용해 DB에 저장
        return "";
    }
}
```

### 2.7.8 JPA 설정하기
- application.yml에 아래와 같이 추가해주면 자동으로 table을 생성해 준다.

`[application.yml]`
```
jpa:
  show-sql: true
  database-platform: org.hibernate.dialect.MySQL8Dialect
  database : mysql
  hibernate.ddl-auto : update
```
- `show-sql` : jpa가 자동으로 만들어주는 query문을 console에서 볼 것인지 여부
- `database-platform` : 사용할 DB의 Dialect설정
- `database` : 사용할 DB종류
- `hibernate.ddl-auto` : 가장 중요한 옵션 (__초보자일 때 create쓰지 말것! __)

> __hibernate.ddl-auto의 옵션들__
>
>![](https://velog.velcdn.com/images/lyj1023/post/140b6a9d-e138-45e7-9f3d-785be97e28f9/image.png)
>- create와 update는 조심해서 써야한다.


- 위와 같이 설정하고 DB를 확인해 보면 자동으로 article 테이블이 만들어져 있는것을 확인 할 수 있다.

![](https://velog.velcdn.com/images/lyj1023/post/f5d8e199-55b9-447b-ab2d-790bb49bd27b/image.png)

- http://localhost:8080/articles/new 에서 아래와 같이 넣고 submit하면

![](https://velog.velcdn.com/images/lyj1023/post/585f1ef9-8aaa-4eed-a1af-682852c2d216/image.png)

- 콘솔창에서 아래와 같이 로그가 찍히고

![](https://velog.velcdn.com/images/lyj1023/post/33385858-df4e-4090-b68f-44605e4b7394/image.png)

- DB에 한줄이 추가된 것을 확인 할 수 있다

![](https://velog.velcdn.com/images/lyj1023/post/a6c62140-a9c1-4e40-b185-f2467f5342d3/image.png)
