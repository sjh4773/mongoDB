# mongoDB

## mongoDB 텍스트 검색, 인덱스, 정규표현식 활용

### mongodb 인덱스(INDEX)
- 검색을 더 빠르게 수행하고자 만든 추가적인 data structure
    - index 데이터 구조가 없다면, 컬렉션에 있는 데이터를 하나씩 조회하는 방식으로 검색이 됨

### 기본 인덱스 _id
- 모든 mongodb 컬렉션은 기본적으로 _id 필드를 기반으로 기본 인덱스가 생성됨

            

            # mongodb 접속 기본 코드
            
            input:
            from bs4 import BeautifulSoup
            import requests
            import pymongo
            import re
            
            conn = pymongo.MongoClient()
            actor_db = conn.cine21
            actor_collection = actor_db.actor_collection
            
            actor_collection.find_one({})
            docs = actor_collection.find({}).limit(3)
            for doc in docs:
                print(doc)
            actor = actor_collection
            
            output:
            {'_id': ObjectId('60ebe76f8753c67e46a0bd4d'), '배우이름': '조우진', '흥행지수': 255195, '출연영화': ['발신제한', '국가부도의 날'], '랭킹': '1', '직업': '배우', '생년월일': '1979-01-16', '성별': '남', '다른이름': '조신제'}
            {'_id': ObjectId('60ebe76f8753c67e46a0bd4e'), '배우이름': '지창욱', '흥행지수': 204193, '출연영화': ['발신제한', '너의 이름은.', '조작된 도시'], '랭킹': '2', '직업': '배우', '생년월일': '1987-07-05', '성별': '남', '홈페이지': '\nhttps://twitter.com/jichangwook\nhttps://www.instagram.com/jichangwook/\n', '신장/체중': '180cm, 65kg', '학교': '단국대학교 공연영상학부', '특기': '노래, 유도, 수영', '소속사': '(주)글로리어스'}
            {'_id': ObjectId('60ebe76f8753c67e46a0bd4f'), '배우이름': '진경', '흥행지수': 153115, '출연영화': ['발신제한'], '랭킹': '3', '직업': '배우', '생년월일': '1972-03-27', '성별': '여'}


### Single(단일) 필드 인덱스
- index 생성 : create_index()
- 특정 인덱스 삭제  ex) actor.drop_index([('배우이름', 1)])


            # index 정보 확인 : create_index()
            input:
            actor.index_information()
             
            output:   
            {'_id_': {'v': 2, 'key': [('_id', 1)]},
             '배우이름_1': {'v': 2, 'key': [('배우이름', 1)]},
             '랭킹_1': {'v': 2, 'key': [('랭킹', 1)]},
             '흥행지수_1': {'v': 2, 'key': [('흥행지수', 1)]},
             '출연영화_text': {'v': 2,
              'key': [('_fts', 'text'), ('_ftsx', 1)],
              'weights': SON([('출연영화', 1)]),
              'default_language': 'english',
              'language_override': 'language',
              'textIndexVersion': 3},
             '직업_-1': {'v': 2, 'key': [('직업', -1)]}}

- key : {'필드명', direction}
  - direction
      - pymongo.ASCENDING = 1
      - pymongo.DESCENDING = -1
      - pymongo.TEXT = 'text'

            
            # 기본 인덱스 외의 인덱스 삭제
            
            input:
            actor.drop_indexes()
            actor.index_information()
            
            output:
            {'_id_': {'v': 2, 'key': [('_id', 1)]}}


            # 여러개의 single index 생성 가능

            input:
            actor.create_index('배우이름')
            output:
            '배우이름_1'
        
            input:
            actor.create_index('랭킹')
            output:            
            '랭킹_1'
            
            input:
            actor.create_index('흥행지수')
            output:
            '흥행지수_1'


            # 부분 문자열 검색을 위해, text index 생성
            
            input:
            actor.create_index([('직업','text')])
            output:
            '직업_text'

            # index 정보 확인 : create_index()
            
            input:
            actor.index_information()

            output:
            {'_id_': {'v': 2, 'key': [('_id', 1)]},
             '배우이름_1': {'v': 2, 'key': [('배우이름', 1)]},
             '랭킹_1': {'v': 2, 'key': [('랭킹', 1)]},
             '흥행지수_1': {'v': 2, 'key': [('흥행지수', 1)]},
             '직업_text': {'v': 2,
              'key': [('_fts', 'text'), ('_ftsx', 1)],
              'weights': SON([('직업', 1)]),
              'default_language': 'english',
              'language_override': 'language',
              'textIndexVersion': 3}}


            # 부분 문자열 검색: $text

            input:
            docs = actor.find({'$text': {'$search': '가수'}})
            for doc in docs:
                print(doc)

            output:
            {'_id': ObjectId('60ebe76f8753c67e46a0bd90'), '배우이름': '준호', '흥행지수': 119, '출연영화': ['기방도령'], '랭킹': '68', '직업': '가수', '생년월일': '1990-01-25', '성별': '남', '홈페이지': '\nhttps://www.instagram.com/le2jh/\nhttps://twitter.com/dlwnsghek\n', '신장/체중': '178cm, 67kg', '학교': '호원대학교', '취미': '음악감상, 춤동영상보기, 작곡공부, 독서하기', '특기': '비트박스, 춤, 노래', '소속사': 'JYP엔터테인먼트', '다른이름': '이준호; 2pm; 투피엠'}
            {'_id': ObjectId('60ebe76f8753c67e46a0bd8c'), '배우이름': '흐른', '흥행지수': 142, '출연영화': ['우리는 매일매일'], '랭킹': '64', '직업': '가수', '성별': '여', '홈페이지': '\nhttp://www.facebook.com/flowing.seoul\n', '다른이름': '강정임'}
            {'_id': ObjectId('60ebe76f8753c67e46a0bd76'), '배우이름': '황인선', '흥행지수': 607, '출연영화': ['이번엔 잘 되겠지'], '랭킹': '42', '직업': '가수', '생년월일': '1987-00-00', '성별': '여', '홈페이지': '\nhttp://twitter.com/showworksinsun\nhttp://www.facebook.com/showworksinsun\nhttp://instagram.com/showworks_insun\n'}
            {'_id': ObjectId('60ebe76f8753c67e46a0bd6c'), '배우이름': '양정원', '흥행지수': 1184, '출연영화': ['빛나는 순간'], '랭킹': '32', '직업': '가수', '성별': '남'}
            {'_id': ObjectId('60ebe76f8753c67e46a0bd64'), '배우이름': '함은정', '흥행지수': 2191, '출연영화': ['아이윌 송'], '랭킹': '24', '직업': '가수', '생년월일': '1988-12-12', '성별': '여', '홈페이지': '\nhttps://twitter.com/taraeunjung1212\nhttps://instagram.com/sweetgirlej/\n', '신장/체중': '167cm, 47kg', '학교': '동국대학교 공연예술학 휴학', '소속사': '(주)코어콘텐츠미디어', '다른이름': '은정; 티아라'}
            {'_id': ObjectId('60ebe76f8753c67e46a0bd5d'), '배우이름': '박소진', '흥행지수': 4744, '출연영화': ['괴기맨숀', '좀비크러쉬: 헤이리'], '랭킹': '17', '원어명': '소진', '직업': '가수', '생년월일': '1986-05-21', '성별': '여', '홈페이지': '\nhttps://twitter.com/girls_day_sojin\nhttps://instagram.com/ssozi_sojin/\n', '다른이름': "걸스데이;Girl's Day"}

### Compound(복합) 필드 인덱스
- 최대 31개의 필드로 만들 수 있음


            actor.drop_indexes()
            
            input:
            actor.create_index([('출연영화',pymongo.TEXT), ('직업',pymongo.TEXT), ('흥행지수',pymongo.DESCENDING)])
            
            output:
            '출연영화_text_직업_text_흥행지수_-1'


            input:
            docs = actor.find({'$text': {'$search': '가수'}})
            for doc in docs:
                print(doc)
            
            output:
            {'_id': ObjectId('60ebe76f8753c67e46a0bd90'), '배우이름': '준호', '흥행지수': 119, '출연영화': ['기방도령'], '랭킹': '68', '직업': '가수', '생년월일': '1990-01-25', '성별': '남', '홈페이지': '\nhttps://www.instagram.com/le2jh/\nhttps://twitter.com/dlwnsghek\n', '신장/체중': '178cm, 67kg', '학교': '호원대학교', '취미': '음악감상, 춤동영상보기, 작곡공부, 독서하기', '특기': '비트박스, 춤, 노래', '소속사': 'JYP엔터테인먼트', '다른이름': '이준호; 2pm; 투피엠'}
            {'_id': ObjectId('60ebe76f8753c67e46a0bd8c'), '배우이름': '흐른', '흥행지수': 142, '출연영화': ['우리는 매일매일'], '랭킹': '64', '직업': '가수', '성별': '여', '홈페이지': '\nhttp://www.facebook.com/flowing.seoul\n', '다른이름': '강정임'}
            {'_id': ObjectId('60ebe76f8753c67e46a0bd76'), '배우이름': '황인선', '흥행지수': 607, '출연영화': ['이번엔 잘 되겠지'], '랭킹': '42', '직업': '가수', '생년월일': '1987-00-00', '성별': '여', '홈페이지': '\nhttp://twitter.com/showworksinsun\nhttp://www.facebook.com/showworksinsun\nhttp://instagram.com/showworks_insun\n'}
            {'_id': ObjectId('60ebe76f8753c67e46a0bd6c'), '배우이름': '양정원', '흥행지수': 1184, '출연영화': ['빛나는 순간'], '랭킹': '32', '직업': '가수', '성별': '남'}
            {'_id': ObjectId('60ebe76f8753c67e46a0bd64'), '배우이름': '함은정', '흥행지수': 2191, '출연영화': ['아이윌 송'], '랭킹': '24', '직업': '가수', '생년월일': '1988-12-12', '성별': '여', '홈페이지': '\nhttps://twitter.com/taraeunjung1212\nhttps://instagram.com/sweetgirlej/\n', '신장/체중': '167cm, 47kg', '학교': '동국대학교 공연예술학 휴학', '소속사': '(주)코어콘텐츠미디어', '다른이름': '은정; 티아라'}
            {'_id': ObjectId('60ebe76f8753c67e46a0bd5d'), '배우이름': '박소진', '흥행지수': 4744, '출연영화': ['괴기맨숀', '좀비크러쉬: 헤이리'], '랭킹': '17', '원어명': '소진', '직업': '가수', '생년월일': '1986-05-21', '성별': '여', '홈페이지': '\nhttps://twitter.com/girls_day_sojin\nhttps://instagram.com/ssozi_sojin/\n', '다른이름': "걸스데이;Girl's Day"}


            input:
            docs = actor.find({'$text': {'$search': '발신제한'}})
            for doc in docs:
                print(doc)
            
            output:
            {'_id': ObjectId('60ebe76f8753c67e46a0bd51'), '배우이름': '김지호', '흥행지수': 51038, '출연영화': ['발신제한'], '랭킹': '5', '직업': '배우', '생년월일': '1974-07-22', '성별': '여', '신장/체중': '167cm, 45kg', '학교': '서울여자대학교 영문학', '취미': '비디오 감상, 클래식 음반 모으기', '특기': '수영, 영어회화', '소속사': '이야기엔터테인먼트'}
            {'_id': ObjectId('60ebe76f8753c67e46a0bd50'), '배우이름': '이재인', '흥행지수': 102081, '출연영화': ['발신제한', '아이 캔 스피크'], '랭킹': '4', '직업': '배우', '생년월일': '2004-02-06', '성별': '여'}
            {'_id': ObjectId('60ebe76f8753c67e46a0bd4f'), '배우이름': '진경', '흥행지수': 153115, '출연영화': ['발신제한'], '랭킹': '3', '직업': '배우', '생년월일': '1972-03-27', '성별': '여'}
            {'_id': ObjectId('60ebe76f8753c67e46a0bd4e'), '배우이름': '지창욱', '흥행지수': 204193, '출연영화': ['발신제한', '너의 이름은.', '조작된 도시'], '랭킹': '2', '직업': '배우', '생년월일': '1987-07-05', '성별': '남', '홈페이지': '\nhttps://twitter.com/jichangwook\nhttps://www.instagram.com/jichangwook/\n', '신장/체중': '180cm, 65kg', '학교': '단국대학교 공연영상학부', '특기': '노래, 유도, 수영', '소속사': '(주)글로리어스'}
            {'_id': ObjectId('60ebe76f8753c67e46a0bd4d'), '배우이름': '조우진', '흥행지수': 255195, '출연영화': ['발신제한', '국가부도의 날'], '랭킹': '1', '직업': '배우', '생년월일': '1979-01-16', '성별': '남', '다른이름': '조신제'}

### 다양한 문자열 검색
 
- 정규표현식 ($text operator 는 $search operator 와 함께 사용됨)



            
            input:
            result = actor.find( {'출연영화' : {'$regex' : '발신' } } )
            for record in result:
                print(record)

            output:
            {'_id': ObjectId('60ebe76f8753c67e46a0bd4d'), '배우이름': '조우진', '흥행지수': 255195, '출연영화': ['발신제한', '국가부도의 날'], '랭킹': '1', '직업': '배우', '생년월일': '1979-01-16', '성별': '남', '다른이름': '조신제'}
            {'_id': ObjectId('60ebe76f8753c67e46a0bd4e'), '배우이름': '지창욱', '흥행지수': 204193, '출연영화': ['발신제한', '너의 이름은.', '조작된 도시'], '랭킹': '2', '직업': '배우', '생년월일': '1987-07-05', '성별': '남', '홈페이지': '\nhttps://twitter.com/jichangwook\nhttps://www.instagram.com/jichangwook/\n', '신장/체중': '180cm, 65kg', '학교': '단국대학교 공연영상학부', '특기': '노래, 유도, 수영', '소속사': '(주)글로리어스'}
            {'_id': ObjectId('60ebe76f8753c67e46a0bd4f'), '배우이름': '진경', '흥행지수': 153115, '출연영화': ['발신제한'], '랭킹': '3', '직업': '배우', '생년월일': '1972-03-27', '성별': '여'}
            {'_id': ObjectId('60ebe76f8753c67e46a0bd50'), '배우이름': '이재인', '흥행지수': 102081, '출연영화': ['발신제한', '아이 캔 스피크'], '랭킹': '4', '직업': '배우', '생년월일': '2004-02-06', '성별': '여'}
            {'_id': ObjectId('60ebe76f8753c67e46a0bd51'), '배우이름': '김지호', '흥행지수': 51038, '출연영화': ['발신제한'], '랭킹': '5', '직업': '배우', '생년월일': '1974-07-22', '성별': '여', '신장/체중': '167cm, 45kg', '학교': '서울여자대학교 영문학', '취미': '비디오 감상, 클래식 음반 모으기', '특기': '수영, 영어회화', '소속사': '이야기엔터테인먼트'}


            # 새로운 collection 및 document  생성
            from bs4 import BeautifulSoup
            import requests
            import pymongo
            import re
            
            conn = pymongo.MongoClient()
            actor_db = conn.cine21
            text_collection = actor_db.text_collection
            
            text_collection.insert_many([
                { "name" : "Java Hut", "description": "Coffee and cakes", "ranking": 1},
                { "name" : "Burger Buns", "description": "Java hamburgers", "ranking": 2},
                { "name" : "Coffee Shop", "description": "Just coffee", "ranking": 3},
                { "name" : "Clothes Clothes Clothes", "description": "Discount clothing", "ranking": 4},
                { "name" : "Java Shopping", "description": "Indonesian goods", "ranking": 5}
            ])
    
            
            input:
            docs = text_collection.find({'name': {'$regex': 'Co.*'}})
            for doc in docs:
                print(doc)

            output:
            {'_id': ObjectId('60ec4634f98501247e3a1c31'), 'name': 'Coffee Shop', 'description': 'Just coffee', 'ranking': 3}


            input:
            docs = text_collection.find({'name': {'$regex': 'Sh.*'}})
            for doc in docs:
                print(doc)

            output:
            {'_id': ObjectId('60ec4634f98501247e3a1c31'), 'name': 'Coffee Shop', 'description': 'Just coffee', 'ranking': 3}
            {'_id': ObjectId('60ec4634f98501247e3a1c33'), 'name': 'Java Shopping', 'description': 'Indonesian goods', 'ranking': 5}


            input:
            text_collection.create_index([('name',pymongo.TEXT)])
            
            output:
            'name_text'


            input:
            text_collection.index_information()
            
            output:
            {'_id_': {'v': 2, 'key': [('_id', 1)]},
             'name_text': {'v': 2,
              'key': [('_fts', 'text'), ('_ftsx', 1)],
              'weights': SON([('name', 1)]),
              'default_language': 'english',
              'language_override': 'language',
              'textIndexVersion': 3}}

            input:
            docs = text_collection.find({'$text': {'$search': 'coffee'}})
            for doc in docs:
                print(doc)
            
            output:
            {'_id': ObjectId('60ec4634f98501247e3a1c31'), 'name': 'Coffee Shop', 'description': 'Just coffee', 'ranking': 3}


            # 검색할 때 부분 문자열로 따로 본다.

            input:
            docs = text_collection.find({'$text': {'$search': 'Java coffee'}})
            for doc in docs:
                print(doc)

            output:
            {'_id': ObjectId('60ec4634f98501247e3a1c31'), 'name': 'Coffee Shop', 'description': 'Just coffee', 'ranking': 3}
            {'_id': ObjectId('60ec4634f98501247e3a1c33'), 'name': 'Java Shopping', 'description': 'Indonesian goods', 'ranking': 5}
            {'_id': ObjectId('60ec4634f98501247e3a1c2f'), 'name': 'Java Hut', 'description': 'Coffee and cakes', 'ranking': 1}


            # 'java shop' 라는 full sentence를 검색하고 싶을 경우
            input:
            docs = text_collection.find({'$text': {'$search': '\"java shop\"'}})
            for doc in docs:
                print(doc)

            output:
            {'_id': ObjectId('60ec4634f98501247e3a1c33'), 'name': 'Java Shopping', 'description': 'Indonesian goods', 'ranking': 5}

            # 소문자와 대문자를 구별하고 싶을 경우 -> 해당 되는것이 없으므로 출력 x

            docs = text_collection.find({'$text': {'$search': 'coffee', '$caseSensitive': True}})
            for doc in docs:
                print(doc)

            # 배우중에 중앙대학교를 나온 배우를 흥행지수 순으로 최대 10명을 출력

            input:
            result = actor.find({'학교' : {'$regex' : '중앙대학교'}}).sort('흥행지수',pymongo.DESCENDING).limit(10)
            for record in result:
                print(record)

            output:
            {'_id': ObjectId('60ebe76f8753c67e46a0bd52'), '배우이름': '진기주', '흥행지수': 41156, '출연영화': ['미드나이트'], '랭킹': '6', '직업': '모델', '생년월일': '1989-01-26', '성별': '여', '홈페이지': '\nhttps://www.instagram.com/jinkijoo/\n', '학교': '중앙대학교 컴퓨터공학부'}
            {'_id': ObjectId('60ebe76f8753c67e46a0bdce'), '배우이름': '박철민', '흥행지수': 23, '출연영화': ['아이 캔 스피크'], '랭킹': '130', '직업': '배우', '생년월일': '1967-00-00', '성별': '남', '학교': '중앙대학교 경영학 학사', '소속사': '씨에스엑터스'}
