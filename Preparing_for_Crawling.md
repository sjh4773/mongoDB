# mongoDB

## 크롤링 준비 - 씨네 21

- F12를 눌러 개발자 도구 확인
    - 개발자 도구에서 Network -> Preserve log 박스 체크 -> ALL 클릭
        - http://www.cine21.com/rank/person/content -> Request URL 확인
        - Request Method: POST -> Request Method 방식 확인




- Click 할 때마다 웹페이지의 주소가 바뀔 경우 GET이라는 방식으로 HTML 파일을 요청하는것이 대부분임
- Click 할 때 웹페이지는 바뀌는데 웹페이지의 주소가 바뀌지 않는 경우 보통은 POST 방식을 사용하는 것임
- Request Method를 POST로 할 것인가 GET으로 할 것인가를 명시해줘야함



- get 방식으로 요청할 때 get라는 함수를 사용
    - request.get
- post 방식으로 요청할 때 post라는 함수를 사용
    - request.post

- GET 방식에서는 웹페이지가 변할 경우 그것에 대한 정보를 웹페이지 주소에 명시해주는 반면에
- POST 방식에서는 웹페이지 주소에 명시하지 않고 정보가 뒤로 전달이 된다.

- 개발자 도구의 Headers 부분에서 Form Data 부분
    - Form Data

           section: actor
           period_start: 2019-02
           gender: all
           page: 1
            
           
           
        ------------------------------------------------------------------------------------
            Request URL:
            Request Method: POST
            
            -------- 이 정보만 바꿔주면 내가 원하는 HTML파일을 가져올 수 있음 -----------
            section = 'actor'
            period_start = '2016-08'
            gender = 'all'
            page = 3
            ------------------------------------------------------------------------------------
            
            request.get
            request.post
            ------------------------------------------------------------------------------------ 