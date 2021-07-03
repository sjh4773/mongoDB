# mongoDB

## 파이썬으로 mongoDB 데이터 삭제하기

- Document Delete 하기
  - delete_one() 메서드 : 가장 먼저 검색되는 한 Document만 삭제하기
 
        
        #  subject가 back exercise인 Document 검색
        input:
        docs = db_exercise.find({"subject":"back exercise"})

        for doc in docs:
            print(doc)
        
        output:
        {'_id': ObjectId('60e05e80ed9fd6d9339924bc'), 'subject': 'back exercise', 'exercise': 'Barbell row', 'set': 10, 'reps': 13}
        {'_id': ObjectId('60e05e80ed9fd6d9339924bd'), 'subject': 'back exercise', 'exercise': 'Dumbell row', 'set': 10, 'reps': 13}
        {'_id': ObjectId('60e05e80ed9fd6d9339924be'), 'subject': 'back exercise', 'exercise': 'Pull Up', 'set': 10, 'reps': 13}
        {'_id': ObjectId('60e05e80ed9fd6d9339924bf'), 'subject': 'back exercise', 'exercise': 'Deadlift', 'set': 10, 'reps': 13}

         #  subject가 back exercise인 Document 중에 가장 먼저 검색되는 항목 삭제
        input:
        db_exercise.delete_one( {"subject" : "back exercise"} )  
        
        output:
        <pymongo.results.DeleteResult at 0x1a2d165e640>


        # 결과 확인
        input:
        docs = db_exercise.find( {"subject" : "back exercise"} )
        
        for doc in docs:
            print(doc)
        
        output:
        {'_id': ObjectId('60e05e80ed9fd6d9339924bd'), 'subject': 'back exercise', 'exercise': 'Dumbell row', 'set': 10, 'reps': 13}
        {'_id': ObjectId('60e05e80ed9fd6d9339924be'), 'subject': 'back exercise', 'exercise': 'Pull Up', 'set': 10, 'reps': 13}
        {'_id': ObjectId('60e05e80ed9fd6d9339924bf'), 'subject': 'back exercise', 'exercise': 'Deadlift', 'set': 10, 'reps': 13}



delete_many() : 조건에 맞는 모든 Document 삭제하기
        
        # 조건에 들어 맞는 모든 Document 삭제
        input:
        db_exercise.delete_many( {"subject" : "back exercise"} )
        
        output:
        <pymongo.results.DeleteResult at 0x1a2d1616180>

        # document의 수가 0이 나온것으로 보아 모든 Document가 삭제되었음을 알 수 있다.
        input:
        db_exercise.count_documents( {"subject" : "back exercise"} )
      
        output:
        0


        # 실습을 위한 새로운 데이터베이스 생성


        import pymongo
        conn = pymongo.MongoClient()
        books = conn.books
        it_book = books.it_book
        
        data = list()
        for index in range(100):
            data.append({ "author":"Hun", "publisher":"sjh4773.github.io", "number": index })
        
        
        # CRUD - Create(Insert)
        
        input:
        it_book.insert_many(data)
        
        output:
        <pymongo.results.InsertManyResult at 0x1a2d16666c0>
        
        
        # CRUD - Read
        
        input:
        docs = it_book.find()
        for doc in docs:
            print(doc)
        
        
        output:
        {'_id': ObjectId('60e06db6ed9fd6d9339924c6'), 'author': 'Hun', 'publisher': 'sjh4773.github.io', 'number': 0}
        {'_id': ObjectId('60e06db6ed9fd6d9339924c7'), 'author': 'Hun', 'publisher': 'sjh4773.github.io', 'number': 1}
        {'_id': ObjectId('60e06db6ed9fd6d9339924c8'), 'author': 'Hun', 'publisher': 'sjh4773.github.io', 'number': 2}
        {'_id': ObjectId('60e06db6ed9fd6d9339924c9'), 'author': 'Hun', 'publisher': 'sjh4773.github.io', 'number': 3}
        {'_id': ObjectId('60e06db6ed9fd6d9339924ca'), 'author': 'Hun', 'publisher': 'sjh4773.github.io', 'number': 4}
        {'_id': ObjectId('60e06db6ed9fd6d9339924cb'), 'author': 'Hun', 'publisher': 'sjh4773.github.io', 'number': 5}
        {'_id': ObjectId('60e06db6ed9fd6d9339924cc'), 'author': 'Hun', 'publisher': 'sjh4773.github.io', 'number': 6}
        {'_id': ObjectId('60e06db6ed9fd6d9339924cd'), 'author': 'Hun', 'publisher': 'sjh4773.github.io', 'number': 7}
        {'_id': ObjectId('60e06db6ed9fd6d9339924ce'), 'author': 'Hun', 'publisher': 'sjh4773.github.io', 'number': 8}
        {'_id': ObjectId('60e06db6ed9fd6d9339924cf'), 'author': 'Hun', 'publisher': 'sjh4773.github.io', 'number': 9}
        {'_id': ObjectId('60e06db6ed9fd6d9339924d0'), 'author': 'Hun', 'publisher': 'sjh4773.github.io', 'number': 10}
        ....
        {'_id': ObjectId('60e06db6ed9fd6d933992520'), 'author': 'Hun', 'publisher': 'sjh4773.github.io', 'number': 90}
        {'_id': ObjectId('60e06db6ed9fd6d933992521'), 'author': 'Hun', 'publisher': 'sjh4773.github.io', 'number': 91}
        {'_id': ObjectId('60e06db6ed9fd6d933992522'), 'author': 'Hun', 'publisher': 'sjh4773.github.io', 'number': 92}
        {'_id': ObjectId('60e06db6ed9fd6d933992523'), 'author': 'Hun', 'publisher': 'sjh4773.github.io', 'number': 93}
        {'_id': ObjectId('60e06db6ed9fd6d933992524'), 'author': 'Hun', 'publisher': 'sjh4773.github.io', 'number': 94}
        {'_id': ObjectId('60e06db6ed9fd6d933992525'), 'author': 'Hun', 'publisher': 'sjh4773.github.io', 'number': 95}
        {'_id': ObjectId('60e06db6ed9fd6d933992526'), 'author': 'Hun', 'publisher': 'sjh4773.github.io', 'number': 96}
        {'_id': ObjectId('60e06db6ed9fd6d933992527'), 'author': 'Hun', 'publisher': 'sjh4773.github.io', 'number': 97}
        {'_id': ObjectId('60e06db6ed9fd6d933992528'), 'author': 'Hun', 'publisher': 'sjh4773.github.io', 'number': 98}
        {'_id': ObjectId('60e06db6ed9fd6d933992529'), 'author': 'Hun', 'publisher': 'sjh4773.github.io', 'number': 99}
        
        
        # CRUD - Update
        
        input:
        it_book.update_many( {}, { "$set" : { "publisher":"www.sjh4773.github.io" } } )
        
        output:
        <pymongo.results.UpdateResult at 0x1a2d16677c0>
        
        # Read
        input:
        docs = it_book.find()
        for doc in docs:
            print(doc)
        
        output:
        {'_id': ObjectId('60e06db6ed9fd6d9339924c6'), 'author': 'Hun', 'publisher': 'www.sjh4773.github.io', 'number': 0}
        {'_id': ObjectId('60e06db6ed9fd6d9339924c7'), 'author': 'Hun', 'publisher': 'www.sjh4773.github.io', 'number': 1}
        {'_id': ObjectId('60e06db6ed9fd6d9339924c8'), 'author': 'Hun', 'publisher': 'www.sjh4773.github.io', 'number': 2}
        {'_id': ObjectId('60e06db6ed9fd6d9339924c9'), 'author': 'Hun', 'publisher': 'www.sjh4773.github.io', 'number': 3}
        {'_id': ObjectId('60e06db6ed9fd6d9339924ca'), 'author': 'Hun', 'publisher': 'www.sjh4773.github.io', 'number': 4}
        {'_id': ObjectId('60e06db6ed9fd6d9339924cb'), 'author': 'Hun', 'publisher': 'www.sjh4773.github.io', 'number': 5}
        {'_id': ObjectId('60e06db6ed9fd6d9339924cc'), 'author': 'Hun', 'publisher': 'www.sjh4773.github.io', 'number': 6}
        {'_id': ObjectId('60e06db6ed9fd6d9339924cd'), 'author': 'Hun', 'publisher': 'www.sjh4773.github.io', 'number': 7}
        {'_id': ObjectId('60e06db6ed9fd6d9339924ce'), 'author': 'Hun', 'publisher': 'www.sjh4773.github.io', 'number': 8}
        {'_id': ObjectId('60e06db6ed9fd6d9339924cf'), 'author': 'Hun', 'publisher': 'www.sjh4773.github.io', 'number': 9}
        {'_id': ObjectId('60e06db6ed9fd6d9339924d0'), 'author': 'Hun', 'publisher': 'www.sjh4773.github.io', 'number': 10}
        ....
        {'_id': ObjectId('60e06db6ed9fd6d933992520'), 'author': 'Hun', 'publisher': 'www.sjh4773.github.io', 'number': 90}
        {'_id': ObjectId('60e06db6ed9fd6d933992521'), 'author': 'Hun', 'publisher': 'www.sjh4773.github.io', 'number': 91}
        {'_id': ObjectId('60e06db6ed9fd6d933992522'), 'author': 'Hun', 'publisher': 'www.sjh4773.github.io', 'number': 92}
        {'_id': ObjectId('60e06db6ed9fd6d933992523'), 'author': 'Hun', 'publisher': 'www.sjh4773.github.io', 'number': 93}
        {'_id': ObjectId('60e06db6ed9fd6d933992524'), 'author': 'Hun', 'publisher': 'www.sjh4773.github.io', 'number': 94}
        {'_id': ObjectId('60e06db6ed9fd6d933992525'), 'author': 'Hun', 'publisher': 'www.sjh4773.github.io', 'number': 95}
        {'_id': ObjectId('60e06db6ed9fd6d933992526'), 'author': 'Hun', 'publisher': 'www.sjh4773.github.io', 'number': 96}
        {'_id': ObjectId('60e06db6ed9fd6d933992527'), 'author': 'Hun', 'publisher': 'www.sjh4773.github.io', 'number': 97}
        {'_id': ObjectId('60e06db6ed9fd6d933992528'), 'author': 'Hun', 'publisher': 'www.sjh4773.github.io', 'number': 98}
        {'_id': ObjectId('60e06db6ed9fd6d933992529'), 'author': 'Hun', 'publisher': 'www.sjh4773.github.io', 'number': 99}`


## number가 6 이상(>=)인 doc 삭제하기


        # CRUD - Delete

        input:
        #it_book.delete_many( { "number" : { "$gte" : 6 } } )
        it_book.delete_many( { "number" : { "$gt" : 5 } } )
        
        output:
        <pymongo.results.DeleteResult at 0x1a2d16425c0>
        
        # Read
        
        input:
        docs = it_book.find()
        for doc in docs:
            print(doc)
        
        output:
        {'_id': ObjectId('60e06db6ed9fd6d9339924c6'), 'author': 'Hun', 'publisher': 'www.sjh4773.github.io', 'number': 0}
        {'_id': ObjectId('60e06db6ed9fd6d9339924c7'), 'author': 'Hun', 'publisher': 'www.sjh4773.github.io', 'number': 1}
        {'_id': ObjectId('60e06db6ed9fd6d9339924c8'), 'author': 'Hun', 'publisher': 'www.sjh4773.github.io', 'number': 2}
        {'_id': ObjectId('60e06db6ed9fd6d9339924c9'), 'author': 'Hun', 'publisher': 'www.sjh4773.github.io', 'number': 3}
        {'_id': ObjectId('60e06db6ed9fd6d9339924ca'), 'author': 'Hun', 'publisher': 'www.sjh4773.github.io', 'number': 4}
        {'_id': ObjectId('60e06db6ed9fd6d9339924cb'), 'author': 'Hun', 'publisher': 'www.sjh4773.github.io', 'number': 5}