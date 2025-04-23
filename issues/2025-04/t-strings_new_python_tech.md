# Python 3.14의 새로운 문자열 기능 : `t-strings`

- 날짜 : 2025 - 04 - 22
- 출처 : [GeekNews](https://news.hada.io/topic?id=20463)
- 의견

## 요약(Summary)
- Python 3.14에서 도입된 `t-strings`는 문자열을 보다 안전하고 유연하게 처리하기 위해 설계된 새로운 기능

- 기존의 `f-strings`와 달리, `t-strings`는 즉시 문자열로 평가되지 않음

- 대신 Template 객체로 반환 후 추가적인 가공을 통해 출력

## 상세 내용(Details)

### 기존 `f-strings`의 한계
- `f-strings`는 즉시 문자열로 평가되기 때문에, 사용자 입력을 포함한 코드에서 **SQL Injection** 이나 **XSS** 와 같은 보안 취약점 발생 가능

    - #### SQL INjection
        - SQL 주입 공격
        - 사용자가 입력한 값이 SQL 쿼리 문장에 그대로 들어가면서 **데이터 베이스 명령어를 해킹처럼 실행**하게 만드는 공격 방법
        - 비유 : 콘서트 예약을 하려면 이름을 적어 내야 하는데, 이름 대신 "예약 테이블 다 보여줘!"라고 써서 예약 명단 테이블 정보가 다 노출됨

        ```python
        user_input = "' OR 1=1 --"  # OR 1=1: 항상 참. 모든 조건을 무시, --: 뒤의 쿼리를 주석처리해 무력화
        query = f"SELECT * FROM users WHERE name = '{user_input}'"

        # 결과: 모든 사용자 정보가 노출됨 😱
        ```
    - #### XSS (Cross-site Scripting)
        - 사용자가 입력한 값이 HTML로 출력될 때, javascript같은 악성 코드도 함께 실행되도록 만드는 공격
        - 비유 : 웹사이트 방명록에 "안녕하세요." 멘트 대신 "폭탄 설치"버튼을 넣는 느낌

        ```python
        user_input = "<script>alert('해킹!');</script>"
        html = f"<div>{user_input}</div>"

        # 브라우저는 이걸 보고 경고창을 띄움 🚨
        ```

### `t-strings`의 도입
- `t-strings`는 t"..." 문법이 사용되며, Template 객체로 반환
- 출력 전 명시적인 가공 과정을 거쳐 보안상 안전한 문자열 처리 가능

```python
# 기존 f-string
user_input = "<script>alert('XSS')</script>"
html = f"<div>{user_input}</div>"

# 결과: <div><script>alert('XSS')</script></div>
# 그냥 "붙이기"만 함
```

```python
# 새로운 t-string
evil = "<script>alert('bad')</script>"
template = t"<p>{evil}</p>"  # 내부적으로 Template 객체 만듦
safe = html(template)  # html()함수로 자동 보안 필터 적용

# 결과: "<p>&lt;script&gt;alert('bad')&lt;/script&gt;</p>"
# 특수문자 이스케이프 처리
```

### 구조 및 API
- Template 객체는 `.strings`, `.values` 속성을 통해 원본 텍스트와 대입 값을 분리하여 제공
- interpolations 속성을 통해 포맷 세부 정보에 접근 가능
- 수동으로 Template 객체 생성 가능

```python
from string.templatelib import Template, Interpolation

template = Template(
    "Hello ",
    Interpolation(value="World", expression="name"),
    "!"
)
```

### 활용 예시
- #### HTML 이스케이프 처리

    ```python
    evil = "<script>alert('bad')</script>"
    template = t"<p>{evil}</p>"
    safe = html(template)

    # 결과: "<p>&lt;script&gt;alert('bad')&lt;/script&gt;</p>"
    ```

- #### 속성 자동 삽입
    ```python
    attributes = {"src": "image.jpg", "alt": "An image"}  # scr(어떤 것을 보여줄지), alt(설명이 무엇인지) 등이 속성(attribute)
    template = t"<img {attributes} />"  # 속성값들을 모아 자동 삽입
    element = html(template)

    # 결과: "<img src='image.jpg' alt='An image' />"
    ```

- #### Pig Latin 변환기
    ```python
    def pig_latin(template: Template) -> str:
        # 변환 로직 구현
        pass

    name = "world"
    template = t"Hello {name}!"  # 자유롭게 변환 로직 짤 수 있음
    assert pig_latin(template) == "Hello orldway!"
    ```

- #### SQL 쿼리
    ```python
    city = 'London'
    min_age = 21

    # query = f"SELECT * FROM users WHERE city = '{city}' AND age > {min_age}"  # 기존 방식

    users = db.get(t'''
        SELECT * FROM users
        WHERE city={city} AND age>{min_age}
    ''')
    ```

### 커뮤니티 반응
- 특히 SQL에서 **가독성과 보안성**을 동시에 향상시킬 수 있다는 점이 강조되어 긍정적인 평가를 받고 있다.
- Python의 기능이 점점 복잡해지고 있다는 우려가 있다.