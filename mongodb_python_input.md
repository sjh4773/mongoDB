# mongoDB

## 파이썬으로 mongoDB 데이터 입력하기

- Document INSERT 하기
  - insert_one()
  - mongodb shell 명령어 : insertOne()

    
    post = { "subject": "back exercise", "exercise": "Barbell row", "set": 5, "reps": 13}
    
    db_exercise.insert_one(post)

- insert_many()


    db.exercise.insert_many(
        [
            {"subject": "back exercise", "exercise": "Dumbell row", "set": 5, "reps": 13},
            {"subject": "back exercise", "exercise": "Pull Up", "set": 5, "reps": 13}
            
        ]
    )

- Document INSERT 하면, _id(primary key)를 확인하는 방법

    
    post = {"subject": "back exercise", "exercise": "Deadlift", "set": 5, "reps": 13}
    post_id = db_exercise.insert_one(post)
    
    post_id
    <pymongo.results.InsertOneResult at 0x16425885a80>
    
    post_id.inserted_id
    ObjectId('60d870a38f6e65d651043c48')

    import pymongo
    conn= pymongo.MongoClient() # 주소를 안적었을 경우 무조건 자기 pc의 port 27017번에 접속된다.

- count() 메서드는 컬렉션 객체와 함께 쓰여서 총 Document 수를 알려줌
  - count_documents({})
  - estimated_document_count()
  - count() 함수는 최신 pymongo 라이브러리에서는 사용 권장되지 않음
  
    

    # 리스트, 객체 삽입 가능 
    db_exercise.insert_one( { 'title' : 'weight traning', 'routine' : ['chest', 'back', 'shoulder', 'leg']})
    db_exercise.insert_one(
        {
            'title' : 'bulk up routine',
            'routine' : ['chest', 'back', 'leg', 'shoulder'],
            'exercise intensity' :
            {
                'exercise time' : '1hour',
                'weight' : 'very heavy',
                'set' :
                {
                    '30 sets or more' :
                    { 
                        'reps per set' : 13
                    }
                }
            }
        }
    )


    data = list()
    data.append( {'bodybuilder name' : 'Kim min soo', 'event' : 'Physique' })
    data.append( {'bodybuilder name' : 'Raymont edmonds', 'event' : 'Physique' })
    data.append( {'bodybuilder name' : 'Brandom Hendrickson', 'event' : 'Physique' })
    
    db_exercise.insert_many(data)