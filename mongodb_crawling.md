# mongoDB

## mongoDB 저장을 위한 크롤링

### 1.라이브러리 import

            from bs4 import BeautifulSoup
            import requests
            import pymongo
            # 정규표현식은 re라는 라이브러리를 통해 사용
            import re


### 2.mongodb connection


            conn = pymongo.MongoClient()
            actor_db = conn.cine21
            actor_collection = actor_db.actor_collection


### 3. 크롤링 주소 requests

            cine21_url = 'http://www.cine21.com/rank/person/content'
            post_data = dict()
            post_data['section'] = 'actor'
            post_data['period_start'] = '2021-07'
            post_data['gender'] = 'all'
            post_data['page'] = 1
            
            res = requests.post(cine21_url, data=post_data)

### 4. parsing과 데이터 추출
- 개발자모드 -> Element -> 좌측 상단의 화살표 아이콘(Select an element in the page to inspect it) 클릭
- 각각의 배우이름이 적혀있는 줄 마우스 우측 버튼 클릭 -> COPY 내의 CSS selector 클릭
- rank_holder > ul > li:nth-child(1) > div > a  부적절
- <li class ="people_li"> == $0 : li는 list를 나타내는 html 태그
- li.people_li 내부의 div.name을 가져와야함

![조우진3](https://user-images.githubusercontent.com/76901290/125187500-e5845d80-e26a-11eb-8a4b-a7b0d3137438.png)
            
            input:
            soup = BeautifulSoup(res.content, 'html.parser')
            
            actors = soup.select('li.people_li div.name')
            for actor in actors:
                print(actor.text)

            output:
            조우진(2편)
            지창욱(3편)
            진경(1편)
            이재인(2편)
            김지호(1편)
            진기주(1편)
            위하준(1편)

            
            input:
            # 이름 뒤에 괄호 없애기
            test_data = '주지훈(6편)'
            
            # re.sub(정규표현식, 바꿀 내용, 정규표현식에서 검색할 데이터)
            # \w : 문자,숫자를 찾음
            # 반복 ?,*,+
            # ?는 앞 문자가 0번 또는 1번 표시되는 패턴(없어도 되고, 한번 있어도 되는 패턴)
            # *는 앞 문자가 0번 또는 그 이상 반복되는 패턴
            # +는 앞 문자가 1번 또는 그 이상 반복되는 패턴
            # 괄호안에 어떤 문자나 숫자가 하나가 있던 그 이상 있는 경우 정규표현식 조건에 해당됨
            re.sub('\(\w*\)','',test_data)

            output:
            '주지훈'


            input:
            actors = soup.select('li.people_li div.name')
            for actor in actors:
            print(re.sub('\(\w*\)','',actor.text))

            output:
            조우진
            지창욱
            진경
            이재인
            김지호
            진기주
            위하준



### 5. 배우 상세 정보 추출

![조우진](https://user-images.githubusercontent.com/76901290/125182780-d3df8d80-e24b-11eb-8910-b0c8de0920ea.png)


            <a href="/db/person/info/?person_id=98611">조우진(2편)</a> -> a라는 태그에 링크를 걸어놓은 것
            
            a라는 링크로 둘러쌓여 있는 텍스트를 마우스 클릭하면 해당 정보로 넘어갈 수 있게끔 하는 것
            
            input:
            actors = soup.select('li.people_li div.name')
            for actor in actors:
                print(actor)

            output:
            <div class="name"><a href="/db/person/info/?person_id=98611">조우진(2편)</a></div>
            <div class="name"><a href="/db/person/info/?person_id=70477">지창욱(3편)</a></div>
            <div class="name"><a href="/db/person/info/?person_id=30235">진경(1편)</a></div>
            <div class="name"><a href="/db/person/info/?person_id=100454">이재인(2편)</a></div>
            <div class="name"><a href="/db/person/info/?person_id=5152">김지호(1편)</a></div>
            <div class="name"><a href="/db/person/info/?person_id=96526">진기주(1편)</a></div>
            <div class="name"><a href="/db/person/info/?person_id=99422">위하준(1편)</a></div>


            input:
            actors = soup.select('li.people_li div.name')
            for actor in actors:
            print(actor.select_one('a'))

            output:
            <a href="/db/person/info/?person_id=98611">조우진(2편)</a>
            <a href="/db/person/info/?person_id=70477">지창욱(3편)</a>
            <a href="/db/person/info/?person_id=30235">진경(1편)</a>
            <a href="/db/person/info/?person_id=100454">이재인(2편)</a>
            <a href="/db/person/info/?person_id=5152">김지호(1편)</a>
            <a href="/db/person/info/?person_id=96526">진기주(1편)</a>
            <a href="/db/person/info/?person_id=99422">위하준(1편)</a>


            input:
            # select_one('a') : a태그 데이터 전체
            # 그 안에서 .attrs['속성이름']을 하면 해당 속성에 대한 값이 나온다
            actors = soup.select('li.people_li div.name')
            for actor in actors:
                print(actor.select_one('a').attrs['href'])

            output:
            /db/person/info/?person_id=98611
            /db/person/info/?person_id=70477
            /db/person/info/?person_id=30235
            /db/person/info/?person_id=100454
            /db/person/info/?person_id=5152
            /db/person/info/?person_id=96526
            /db/person/info/?person_id=99422

            
            input:
            actors = soup.select('li.people_li div.name')
            for actor in actors:
                print('http://www.cine21.com' + actor.select_one('a').attrs['href'])

            output:
            http://www.cine21.com/db/person/info/?person_id=98611
            http://www.cine21.com/db/person/info/?person_id=70477
            http://www.cine21.com/db/person/info/?person_id=30235
            http://www.cine21.com/db/person/info/?person_id=100454
            http://www.cine21.com/db/person/info/?person_id=5152
            http://www.cine21.com/db/person/info/?person_id=96526
            http://www.cine21.com/db/person/info/?person_id=99422
                

![조우진2](https://user-images.githubusercontent.com/76901290/125182900-fe7e1600-e24c-11eb-8ca3-ce589151a412.png)

        
            # 페이지를 html파일로 읽고 배우의 기본정보를 추출해야함

            input:
            actors = soup.select('li.people_li div.name')
            for actor in actors:
                actor_link = 'http"//www.cine21.com' + actor.select_one('a').attrs['href']
                response_actor = requests.get(actor_link)
                soup_actor = BeautifulSoup(response_actor.content, 'html.parser')
                print(soup_actor.select_one(ul.default_info))
    
            output:
            <ul class="default_info">
            <li><span class="tit">다른 이름</span>조신제</li>
            <li><span class="tit">직업</span>배우</li>
            <li><span class="tit">생년월일</span>1979-01-16</li>
            <li><span class="tit">성별</span>남</li>
            </ul>
            <ul class="default_info">
            <li><span class="tit">직업</span>배우</li>
            <li><span class="tit">생년월일</span>1987-07-05</li>
            <li><span class="tit">성별</span>남</li>
            <li><span class="tit">홈페이지</span>
            <a href="https://twitter.com/jichangwook" target="_blank">https://twitter.com/jichangwook</a><br/>
            <a href="https://www.instagram.com/jichangwook/" target="_blank">https://www.instagram.com/jichangwook/</a><br/>
            </li>
            <li><span class="tit">신장/체중</span>180cm, 65kg</li>
            <li><span class="tit">학교</span>단국대학교 공연영상학부</li>
            <li><span class="tit">특기</span>노래, 유도, 수영</li>
            <li><span class="tit">소속사</span>(주)글로리어스</li>
            </ul>
            <ul class="default_info">
            <li><span class="tit">직업</span>배우</li>
            <li><span class="tit">생년월일</span>1972-03-27</li>
            <li><span class="tit">성별</span>여</li>
            </ul>
            <ul class="default_info">
            <li><span class="tit">직업</span>배우</li>
            <li><span class="tit">생년월일</span>2004-02-06</li>
            <li><span class="tit">성별</span>여</li>
            </ul>
            <ul class="default_info">
            <li><span class="tit">직업</span>배우</li>
            <li><span class="tit">생년월일</span>1974-07-22</li>
            <li><span class="tit">성별</span>여</li>
            <li><span class="tit">신장/체중</span>167cm, 45kg</li>
            <li><span class="tit">학교</span>서울여자대학교 영문학</li>
            <li><span class="tit">취미</span>비디오 감상, 클래식 음반 모으기</li>
            <li><span class="tit">특기</span>수영, 영어회화</li>
            <li><span class="tit">소속사</span>이야기엔터테인먼트</li>
            </ul>
            <ul class="default_info">
            <li><span class="tit">직업</span>모델</li>
            <li><span class="tit">생년월일</span>1989-01-26</li>
            <li><span class="tit">성별</span>여</li>
            <li><span class="tit">홈페이지</span>
            <a href="https://www.instagram.com/jinkijoo/" target="_blank">https://www.instagram.com/jinkijoo/</a><br/>
            </li>
            <li><span class="tit">학교</span>중앙대학교 컴퓨터공학부</li>
            </ul>
            <ul class="default_info">
            <li><span class="tit">직업</span>배우</li>
            <li><span class="tit">생년월일</span>1991-08-05</li>
            <li><span class="tit">성별</span>남</li>
            </ul>






            input:
            actors = soup.select('li.people_li div.name')
            for actor in actors:
                actor_link = 'http://www.cine21.com' + actor.select_one('a').attrs['href']
                response_actor = requests.get(actor_link)
                soup_actor = BeautifulSoup(response_actor.content, 'html.parser')
                default_info = soup_actor.select_one('ul.default_info')
                actor_details = default_info.select('li')
                for actor_item in actor_details:
                    print(actor_item)

            output:
            <li><span class="tit">다른 이름</span>조신제</li>
            <li><span class="tit">직업</span>배우</li>
            <li><span class="tit">생년월일</span>1979-01-16</li>
            <li><span class="tit">성별</span>남</li>
            <li><span class="tit">직업</span>배우</li>
            <li><span class="tit">생년월일</span>1987-07-05</li>
            <li><span class="tit">성별</span>남</li>
            <li><span class="tit">홈페이지</span>
            <a href="https://twitter.com/jichangwook" target="_blank">https://twitter.com/jichangwook</a><br/>
            <a href="https://www.instagram.com/jichangwook/" target="_blank">https://www.instagram.com/jichangwook/</a><br/>
            </li>
            <li><span class="tit">신장/체중</span>180cm, 65kg</li>
            <li><span class="tit">학교</span>단국대학교 공연영상학부</li>
            <li><span class="tit">특기</span>노래, 유도, 수영</li>
            <li><span class="tit">소속사</span>(주)글로리어스</li>
            <li><span class="tit">직업</span>배우</li>
            <li><span class="tit">생년월일</span>1972-03-27</li>
            <li><span class="tit">성별</span>여</li>
            <li><span class="tit">직업</span>배우</li>
            <li><span class="tit">생년월일</span>2004-02-06</li>
            <li><span class="tit">성별</span>여</li>
            <li><span class="tit">직업</span>배우</li>
            <li><span class="tit">생년월일</span>1974-07-22</li>
            <li><span class="tit">성별</span>여</li>
            <li><span class="tit">신장/체중</span>167cm, 45kg</li>
            <li><span class="tit">학교</span>서울여자대학교 영문학</li>
            <li><span class="tit">취미</span>비디오 감상, 클래식 음반 모으기</li>
            <li><span class="tit">특기</span>수영, 영어회화</li>
            <li><span class="tit">소속사</span>이야기엔터테인먼트</li>
            <li><span class="tit">직업</span>모델</li>
            <li><span class="tit">생년월일</span>1989-01-26</li>
            <li><span class="tit">성별</span>여</li>
            <li><span class="tit">홈페이지</span>
            <a href="https://www.instagram.com/jinkijoo/" target="_blank">https://www.instagram.com/jinkijoo/</a><br/>
            </li>
            <li><span class="tit">학교</span>중앙대학교 컴퓨터공학부</li>
            <li><span class="tit">직업</span>배우</li>
            <li><span class="tit">생년월일</span>1991-08-05</li>
            <li><span class="tit">성별</span>남</li>



#### 참고: 특수한 정규 표현식
- Greedy(.*) vs Non-Greedy(.*?)
    - https://regexr.com
     - Greedy(<.*>) : 정규표현식이 매칭되는 최대치를 가져온다.      
                    
            <li><span class="tit">다른 이름</span>조신제</li>
    
    - Non-Greedy(<span.*?>)
            
            <span class="tit">

- .문자는 줄바꿈 문자인 \n을 제외한 모든 문자 한개를 의미함
- *는 앞 문자가 0번 또는 그 이상 반복되는 패턴
- .*는 문자가 0번 또는 그 이상 반복되는 패턴


            input:
            actors = soup.select('li.people_li div.name')
            for actor in actors:
                actor_link = 'http://www.cine21.com' + actor.select_one('a').attrs['href']
                response_actor = requests.get(actor_link)
                soup_actor = BeautifulSoup(response_actor.content, 'html.parser')
                default_info = soup_actor.select_one('ul.default_info')
                actor_details = default_info.select('li')
                for actor_item in actor_details:
                    print(actor_item)
                    print(actor_item.select_one('span.tit').text)
                    print(re.sub('<span.*?>.*?</span>','',str(actor_item)))


            output:
            <li><span class="tit">다른 이름</span>조신제</li>
            다른 이름
            <li>조신제</li>
            <li><span class="tit">직업</span>배우</li>
            직업
            <li>배우</li>
            <li><span class="tit">생년월일</span>1979-01-16</li>
            생년월일
            <li>1979-01-16</li>
            <li><span class="tit">성별</span>남</li>
            성별
            <li>남</li>
            <li><span class="tit">직업</span>배우</li>
            직업
            <li>배우</li>
            <li><span class="tit">생년월일</span>1987-07-05</li>
            생년월일
            <li>1987-07-05</li>
            <li><span class="tit">성별</span>남</li>
            성별
            <li>남</li>
            <li><span class="tit">홈페이지</span>
            <a href="https://twitter.com/jichangwook" target="_blank">https://twitter.com/jichangwook</a><br/>
            <a href="https://www.instagram.com/jichangwook/" target="_blank">https://www.instagram.com/jichangwook/</a><br/>
            </li>
            홈페이지
            <li>
            <a href="https://twitter.com/jichangwook" target="_blank">https://twitter.com/jichangwook</a><br/>
            <a href="https://www.instagram.com/jichangwook/" target="_blank">https://www.instagram.com/jichangwook/</a><br/>
            </li>
            <li><span class="tit">신장/체중</span>180cm, 65kg</li>
            신장/체중
            <li>180cm, 65kg</li>
            <li><span class="tit">학교</span>단국대학교 공연영상학부</li>
            학교
            <li>단국대학교 공연영상학부</li>
            <li><span class="tit">특기</span>노래, 유도, 수영</li>
            특기
            <li>노래, 유도, 수영</li>
            <li><span class="tit">소속사</span>(주)글로리어스</li>
            소속사
            <li>(주)글로리어스</li>
            <li><span class="tit">직업</span>배우</li>
            직업
            <li>배우</li>
            <li><span class="tit">생년월일</span>1972-03-27</li>
            생년월일
            <li>1972-03-27</li>
            <li><span class="tit">성별</span>여</li>
            성별
            <li>여</li>
            <li><span class="tit">직업</span>배우</li>
            직업
            <li>배우</li>
            <li><span class="tit">생년월일</span>2004-02-06</li>
            생년월일
            <li>2004-02-06</li>
            <li><span class="tit">성별</span>여</li>
            성별
            <li>여</li>
            <li><span class="tit">직업</span>배우</li>
            직업
            <li>배우</li>
            <li><span class="tit">생년월일</span>1974-07-22</li>
            생년월일
            <li>1974-07-22</li>
            <li><span class="tit">성별</span>여</li>
            성별
            <li>여</li>
            <li><span class="tit">신장/체중</span>167cm, 45kg</li>
            신장/체중
            <li>167cm, 45kg</li>
            <li><span class="tit">학교</span>서울여자대학교 영문학</li>
            학교
            <li>서울여자대학교 영문학</li>
            <li><span class="tit">취미</span>비디오 감상, 클래식 음반 모으기</li>
            취미
            <li>비디오 감상, 클래식 음반 모으기</li>
            <li><span class="tit">특기</span>수영, 영어회화</li>
            특기
            <li>수영, 영어회화</li>
            <li><span class="tit">소속사</span>이야기엔터테인먼트</li>
            소속사
            <li>이야기엔터테인먼트</li>
            <li><span class="tit">직업</span>모델</li>
            직업
            <li>모델</li>
            <li><span class="tit">생년월일</span>1989-01-26</li>
            생년월일
            <li>1989-01-26</li>
            <li><span class="tit">성별</span>여</li>
            성별
            <li>여</li>
            <li><span class="tit">홈페이지</span>
            <a href="https://www.instagram.com/jinkijoo/" target="_blank">https://www.instagram.com/jinkijoo/</a><br/>
            </li>
            홈페이지
            <li>
            <a href="https://www.instagram.com/jinkijoo/" target="_blank">https://www.instagram.com/jinkijoo/</a><br/>
            </li>
            <li><span class="tit">학교</span>중앙대학교 컴퓨터공학부</li>
            학교
            <li>중앙대학교 컴퓨터공학부</li>
            <li><span class="tit">직업</span>배우</li>
            직업
            <li>배우</li>
            <li><span class="tit">생년월일</span>1991-08-05</li>
            생년월일
            <li>1991-08-05</li>
            <li><span class="tit">성별</span>남</li>
            성별
            <li>남</li>




#### 각각의 배우 데이터를 사전 데이터로 만들고 그 사전 타입 여러개를 리스트로 만든다.

            

            input:
            actors = soup.select('li.people_li div.name')
            
            actors_info_list = list()
            
            for actor in actors:
                actor_link = 'http://www.cine21.com' + actor.select_one('a').attrs['href']
                response_actor = requests.get(actor_link)
                soup_actor = BeautifulSoup(response_actor.content, 'html.parser')
                default_info = soup_actor.select_one('ul.default_info')
                actor_details = default_info.select('li')
                
                actor_info_dict = dict()
                
                # 한 명이 actor에 대한 정보를 추출하는 반복문
                for actor_item in actor_details:
                    actor_item_field = actor_item.select_one('span.tit').text # 필드명
                    actor_item_value = re.sub('<span.*?>.*?</span>','',str(actor_item))
                    actor_item_value = re.sub('<.*?>','',actor_item_value) # 값
                    actor_info_dict[actor_item_field] = actor_item_value
                actors_info_list.append(actor_info_dict)
                
            actors_info_list

            output:
            [{'다른 이름': '조신제', '직업': '배우', '생년월일': '1979-01-16', '성별': '남'},
             {'직업': '배우',
              '생년월일': '1987-07-05',
              '성별': '남',
              '홈페이지': '\nhttps://twitter.com/jichangwook\nhttps://www.instagram.com/jichangwook/\n',
              '신장/체중': '180cm, 65kg',
              '학교': '단국대학교 공연영상학부',
              '특기': '노래, 유도, 수영',
              '소속사': '(주)글로리어스'},
             {'직업': '배우', '생년월일': '1972-03-27', '성별': '여'},
             {'직업': '배우', '생년월일': '2004-02-06', '성별': '여'},
             {'직업': '배우',
              '생년월일': '1974-07-22',
              '성별': '여',
              '신장/체중': '167cm, 45kg',
              '학교': '서울여자대학교 영문학',
              '취미': '비디오 감상, 클래식 음반 모으기',
              '특기': '수영, 영어회화',
              '소속사': '이야기엔터테인먼트'},
             {'직업': '모델',
              '생년월일': '1989-01-26',
              '성별': '여',
              '홈페이지': '\nhttps://www.instagram.com/jinkijoo/\n',
              '학교': '중앙대학교 컴퓨터공학부'},
             {'직업': '배우', '생년월일': '1991-08-05', '성별': '남'}]
    

### 6. 배우 흥행 지수 추출 및 출연 영화 리스트 추가
- ul.num_info > li > strong


            input:
            actors = soup.select('li.people_li div.name')
            hits = soup.select('ul.num_info > li > strong')
            
            for index, actor in enumerate(actors):
                print("배우이름:",re.sub('\(\w*\)','',actor.text))
                print("흥행지수:",int(hits[index].text.replace(',',''))) # 문자열을 숫자형으로 바꿔줌

            output:
            배우이름: 조우진
            흥행지수: 253824
            배우이름: 지창욱
            흥행지수: 203098
            배우이름: 진경
            흥행지수: 152294
            배우이름: 이재인
            흥행지수: 101533
            배우이름: 김지호
            흥행지수: 50765
            배우이름: 진기주
            흥행지수: 42833
            배우이름: 위하준
            흥행지수: 35694



![조우진4](https://user-images.githubusercontent.com/76901290/125188083-8d028f80-e26d-11eb-8abf-894d699aa072.png)


            input:
            actors = soup.select('li.people_li div.name')
            hits = soup.select('ul.num_info > li > strong')
            movies = soup.select('ul.mov_list')
            
            for index, actor in enumerate(actors):
                print("배우이름:",re.sub('\(\w*\)','',actor.text))
                print("흥행지수:",int(hits[index].text.replace(',',''))) # 문자열을 숫자형으로 바꿔줌
                movie_titles = movies[index].select('li a span')
                movie_title_list = list()
                for movie_title in movie_titles:
                    movie_title_list.append(movie_title.text)
                print("출연영화:",movie_title_list)

            output:
            배우이름: 조우진
            흥행지수: 253824
            출연영화: ['발신제한', '국가부도의 날']
            배우이름: 지창욱
            흥행지수: 203098
            출연영화: ['발신제한', '너의 이름은.', '조작된 도시']
            배우이름: 진경
            흥행지수: 152294
            출연영화: ['발신제한']
            배우이름: 이재인
            흥행지수: 101533
            출연영화: ['발신제한', '아이 캔 스피크']
            배우이름: 김지호
            흥행지수: 50765
            출연영화: ['발신제한']
            배우이름: 진기주
            흥행지수: 42833
            출연영화: ['미드나이트']
            배우이름: 위하준
            흥행지수: 35694
            출연영화: ['미드나이트']


### 7. 수집한 데이터 기반 사전 만들기


            input:
            actors_info_list = list()
            
            actors = soup.select('li.people_li div.name')
            hits = soup.select('ul.num_info > li > strong')
            movies = soup.select('ul.mov_list')
            
            
            
            for index, actor in enumerate(actors):    
                
                actor_name = re.sub('\(\w*\)','',actor.text)
                actor_hits = int(hits[index].text.replace(',','')) # 문자열을 숫자형으로 바꿔줌
                movie_titles = movies[index].select('li a span')
                movie_title_list = list()
                
                for movie_title in movie_titles:
                    movie_title_list.append(movie_title.text)
                movie_title_list                        
                                    
                actor_info_dict = dict()
                actor_info_dict['배우이름'] = actor_name
                actor_info_dict['흥행지수'] = actor_hits
                actor_info_dict['출연영화'] = movie_title_list    
                                    
                                    
                actor_link = 'http://www.cine21.com' + actor.select_one('a').attrs['href']
                response_actor = requests.get(actor_link)
                soup_actor = BeautifulSoup(response_actor.content, 'html.parser')
                default_info = soup_actor.select_one('ul.default_info')
                actor_details = default_info.select('li')
            
                
                # 한 명이 actor에 대한 정보를 추출하는 반복문
                for actor_item in actor_details:
                    actor_item_field = actor_item.select_one('span.tit').text # 필드명
                    actor_item_value = re.sub('<span.*?>.*?</span>','',str(actor_item))
                    actor_item_value = re.sub('<.*?>','',actor_item_value) # 값
                    actor_info_dict[actor_item_field] = actor_item_value
                actors_info_list.append(actor_info_dict)
                
            actors_info_list

            


            output:
            [{'배우이름': '조우진',
              '흥행지수': 253824,
              '출연영화': ['발신제한', '국가부도의 날'],
              '다른 이름': '조신제',
              '직업': '배우',
              '생년월일': '1979-01-16',
              '성별': '남'},
             {'배우이름': '지창욱',
              '흥행지수': 203098,
              '출연영화': ['발신제한', '너의 이름은.', '조작된 도시'],
              '직업': '배우',
              '생년월일': '1987-07-05',
              '성별': '남',
              '홈페이지': '\nhttps://twitter.com/jichangwook\nhttps://www.instagram.com/jichangwook/\n',
              '신장/체중': '180cm, 65kg',
              '학교': '단국대학교 공연영상학부',
              '특기': '노래, 유도, 수영',
              '소속사': '(주)글로리어스'},
             {'배우이름': '진경',
              '흥행지수': 152294,
              '출연영화': ['발신제한'],
              '직업': '배우',
              '생년월일': '1972-03-27',
              '성별': '여'},
             {'배우이름': '이재인',
              '흥행지수': 101533,
              '출연영화': ['발신제한', '아이 캔 스피크'],
              '직업': '배우',
              '생년월일': '2004-02-06',
              '성별': '여'},
             {'배우이름': '김지호',
              '흥행지수': 50765,
              '출연영화': ['발신제한'],
              '직업': '배우',
              '생년월일': '1974-07-22',
              '성별': '여',
              '신장/체중': '167cm, 45kg',
              '학교': '서울여자대학교 영문학',
              '취미': '비디오 감상, 클래식 음반 모으기',
              '특기': '수영, 영어회화',
              '소속사': '이야기엔터테인먼트'},
             {'배우이름': '진기주',
              '흥행지수': 42833,
              '출연영화': ['미드나이트'],
              '직업': '모델',
              '생년월일': '1989-01-26',
              '성별': '여',
              '홈페이지': '\nhttps://www.instagram.com/jinkijoo/\n',
              '학교': '중앙대학교 컴퓨터공학부'},
             {'배우이름': '위하준',
              '흥행지수': 35694,
              '출연영화': ['미드나이트'],
              '직업': '배우',
              '생년월일': '1991-08-05',
              '성별': '남'}]


### 8. 여러 페이지의 배우 상세 정보 추출하기


            from bs4 import BeautifulSoup
            import requests
            import pymongo
            import re
            
            actors_info_list = list()
            
            cine21_url = 'http://www.cine21.com/rank/person/content'
            post_data = dict()
            post_data['section'] = 'actor'
            post_data['period_start'] = '2021-07'
            post_data['gender'] = 'all'
            
            for index in range(1, 21):
                post_data['page'] = index
            
                res = requests.post(cine21_url, data=post_data)
            
                soup = BeautifulSoup(res.content, 'html.parser')
            
                actors = soup.select('li.people_li div.name')
                hits = soup.select('ul.num_info > li > strong')
                movies = soup.select('ul.mov_list')
                rankings = soup.select('li.people_li > span.grade')
            
                for index, actor in enumerate(actors):    
            
                    actor_name = re.sub('\(\w*\)','',actor.text)
                    actor_hits = int(hits[index].text.replace(',','')) # 문자열을 숫자형으로 바꿔줌
                    movie_titles = movies[index].select('li a span')
                    movie_title_list = list()
            
                    for movie_title in movie_titles:
                        movie_title_list.append(movie_title.text)
                    movie_title_list                        
            
                    actor_info_dict = dict()
                    actor_info_dict['배우이름'] = actor_name
                    actor_info_dict['흥행지수'] = actor_hits
                    actor_info_dict['출연영화'] = movie_title_list
                    actor_info_dict['랭킹'] = rankings[index].text
            
            
                    actor_link = 'http://www.cine21.com' + actor.select_one('a').attrs['href']
                    response_actor = requests.get(actor_link)
                    soup_actor = BeautifulSoup(response_actor.content, 'html.parser')
                    default_info = soup_actor.select_one('ul.default_info')
                    actor_details = default_info.select('li')
            
            
                    # 한 명이 actor에 대한 정보를 추출하는 반복문
                    for actor_item in actor_details:
                        actor_item_field = actor_item.select_one('span.tit').text # 필드명
                        actor_item_value = re.sub('<span.*?>.*?</span>','',str(actor_item))
                        actor_item_value = re.sub('<.*?>','',actor_item_value) # 값
                        actor_info_dict[actor_item_field] = actor_item_value
                    actors_info_list.append(actor_info_dict)
                    
            print(actors_info_list)
                    


### 9. mongodb에 크롤링 데이터 저장하기



            conn = pymongo.MongoClient()
            actor_db = conn.cine21 # cine21이라는 데이터베이스를 만듬
            actor_collection = actor_db.actor_collection # actor_collection이라는 collection을 만듬
            
            actor_collection.insert_many(actors_info_list)


![로보](https://user-images.githubusercontent.com/76901290/125194636-255c3c80-e28d-11eb-8bbe-e8801b21fd9e.PNG)