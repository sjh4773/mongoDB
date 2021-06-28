# mongoDB

## 파이썬으로 mongoDB 데이터 검색
- Document 검색 하기
  - find_one() 메서드 : 가장 빨리 검색되는 하나 검색하기

    
      db_exercise.find_one()
      
      {'_id': ObjectId('60d86e4b8f6e65d651043c40'),
       'subject': 'back exercise',
       'exercise': 'Dumbell row',
       'set': 5,
       'reps': 13}          


- find_one({ 키 : 값})
    - 조건을 적용
      
      
      db_exercise.find_one( { "exercise" : "Deadlift"})
      
      {'_id': ObjectId('60d870a38f6e65d651043c48'),
       'subject': 'back exercise',
       'exercise': 'Deadlift',
       'set': 5,
       'reps': 13}

- find() 메서드 : 검색되는 모든 Document 읽어오기
    - find를 쓰면 결과가 list 변수로 넘어온다


      docs = db_exercise.find()
      
      for doc in docs:
          print(doc)


      {'_id': ObjectId('60d86e4b8f6e65d651043c40'), 'subject': 'back exercise', 'exercise': 'Dumbell row', 'set': 5, 'reps': 13}
      {'_id': ObjectId('60d86e4b8f6e65d651043c41'), 'subject': 'back exercise', 'exercise': 'Pull Up', 'set': 5, 'reps': 13}
      {'_id': ObjectId('60d870a38f6e65d651043c45'), 'subject': 'back exercise', 'exercise': 'Barbell row', 'set': 5, 'reps': 13}
      {'_id': ObjectId('60d870a38f6e65d651043c46'), 'subject': 'back exercise', 'exercise': 'Dumbell row', 'set': 5, 'reps': 13}
      {'_id': ObjectId('60d870a38f6e65d651043c47'), 'subject': 'back exercise', 'exercise': 'Pull Up', 'set': 5, 'reps': 13}
      {'_id': ObjectId('60d870a38f6e65d651043c48'), 'subject': 'back exercise', 'exercise': 'Deadlift', 'set': 5, 'reps': 13}
      {'_id': ObjectId('60d875e58f6e65d651043c49'), 'title': 'weight traning', 'routine': ['chest', 'back', 'shoulder', 'leg']}
      {'_id': ObjectId('60d875e58f6e65d651043c4a'), 'title': 'bulk up routine', 'routine': ['chest', 'back', 'leg', 'shoulder'], 'exercise intensity': {'exercise time': '1hour', 'weight': 'very heavy', 'set': {'30 sets or more': {'reps per set': 13}}}}
      {'_id': ObjectId('60d877b98f6e65d651043c4b'), 'bodybuilder name': 'Kim min soo', 'event': 'Physique'}
      {'_id': ObjectId('60d877b98f6e65d651043c4c'), 'bodybuilder name': 'Raymont edmonds', 'event': 'Physique'}
      {'_id': ObjectId('60d877b98f6e65d651043c4d'), 'bodybuilder name': 'Brandom Hendrickson', 'event': 'Physique'}
      {'_id': ObjectId('60d9d4b5a76c85002f3d3f66'), 'subject': 'back exercise', 'exercise': 'Barbell row', 'set': 5, 'reps': 13}
      {'_id': ObjectId('60d9d4b5a76c85002f3d3f67'), 'subject': 'back exercise', 'exercise': 'Dumbell row', 'set': 5, 'reps': 13}
      {'_id': ObjectId('60d9d4b5a76c85002f3d3f68'), 'subject': 'back exercise', 'exercise': 'Pull Up', 'set': 5, 'reps': 13}
      {'_id': ObjectId('60d9d4b5a76c85002f3d3f69'), 'subject': 'back exercise', 'exercise': 'Deadlift', 'set': 5, 'reps': 13}
      {'_id': ObjectId('60d9d4b5a76c85002f3d3f6a'), 'title': 'weight traning', 'routine': ['chest', 'back', 'shoulder', 'leg']}
      {'_id': ObjectId('60d9d4b5a76c85002f3d3f6b'), 'title': 'bulk up routine', 'routine': ['chest', 'back', 'leg', 'shoulder'], 'exercise intensity': {'exercise time': '1hour', 'weight': 'very heavy', 'set': {'30 sets or more': {'reps per set': 13}}}}
      {'_id': ObjectId('60d9d4b5a76c85002f3d3f6c'), 'bodybuilder name': 'Kim min soo', 'event': 'Physique'}
      {'_id': ObjectId('60d9d4b5a76c85002f3d3f6d'), 'bodybuilder name': 'Raymont edmonds', 'event': 'Physique'}
      {'_id': ObjectId('60d9d4b5a76c85002f3d3f6e'), 'bodybuilder name': 'Brandom Hendrickson', 'event': 'Physique'}


- 조건에 해당하는 데이터만 출력


      docs = db_exercise.find( {"subject" : "back exercise"})
      
      for doc in docs:
          print(doc)

      
      {'_id': ObjectId('60d86e4b8f6e65d651043c40'), 'subject': 'back exercise', 'exercise': 'Dumbell row', 'set': 5, 'reps': 13}
      {'_id': ObjectId('60d86e4b8f6e65d651043c41'), 'subject': 'back exercise', 'exercise': 'Pull Up', 'set': 10.0, 'reps': 13}
      {'_id': ObjectId('60d870a38f6e65d651043c45'), 'subject': 'back exercise', 'exercise': 'Barbell row', 'set': 5, 'reps': 13}
      {'_id': ObjectId('60d870a38f6e65d651043c46'), 'subject': 'back exercise', 'exercise': 'Dumbell row', 'set': 5, 'reps': 13}
      {'_id': ObjectId('60d870a38f6e65d651043c47'), 'subject': 'back exercise', 'exercise': 'Pull Up', 'set': 10.0, 'reps': 13}
      {'_id': ObjectId('60d870a38f6e65d651043c48'), 'subject': 'back exercise', 'exercise': 'Deadlift', 'set': 5, 'reps': 13}
      {'_id': ObjectId('60d9d4b5a76c85002f3d3f66'), 'subject': 'back exercise', 'exercise': 'Barbell row', 'set': 5, 'reps': 13}
      {'_id': ObjectId('60d9d4b5a76c85002f3d3f67'), 'subject': 'back exercise', 'exercise': 'Dumbell row', 'set': 5, 'reps': 13}
      {'_id': ObjectId('60d9d4b5a76c85002f3d3f68'), 'subject': 'back exercise', 'exercise': 'Pull Up', 'set': 10.0, 'reps': 13}
      {'_id': ObjectId('60d9d4b5a76c85002f3d3f69'), 'subject': 'back exercise', 'exercise': 'Deadlift', 'set': 5, 'reps': 13}


- count_documents() 함수로 조건에 맞는 검색 데이터 갯수 알아내기


      db_exercise.count_documents( {"subject":"back exercise"})

      10


- sort()와 함께 쓰기


      docs = db_exercise.find().sort("set")
      for doc in docs:
          print(doc)


        {'_id': ObjectId('60d875e58f6e65d651043c49'), 'title': 'weight traning', 'routine': ['chest', 'back', 'shoulder', 'leg']}
        {'_id': ObjectId('60d875e58f6e65d651043c4a'), 'title': 'bulk up routine', 'routine': ['chest', 'back', 'leg', 'shoulder'], 'exercise intensity': {'exercise time': '1hour', 'weight': 'very heavy', 'set': {'30 sets or more': {'reps per set': 13}}}}
        {'_id': ObjectId('60d877b98f6e65d651043c4b'), 'bodybuilder name': 'Kim min soo', 'event': 'Physique'}
        {'_id': ObjectId('60d877b98f6e65d651043c4c'), 'bodybuilder name': 'Raymont edmonds', 'event': 'Physique'}
        {'_id': ObjectId('60d877b98f6e65d651043c4d'), 'bodybuilder name': 'Brandom Hendrickson', 'event': 'Physique'}
        {'_id': ObjectId('60d9d4b5a76c85002f3d3f6a'), 'title': 'weight traning', 'routine': ['chest', 'back', 'shoulder', 'leg']}
        {'_id': ObjectId('60d9d4b5a76c85002f3d3f6b'), 'title': 'bulk up routine', 'routine': ['chest', 'back', 'leg', 'shoulder'], 'exercise intensity': {'exercise time': '1hour', 'weight': 'very heavy', 'set': {'30 sets or more': {'reps per set': 13}}}}
        {'_id': ObjectId('60d9d4b5a76c85002f3d3f6c'), 'bodybuilder name': 'Kim min soo', 'event': 'Physique'}
        {'_id': ObjectId('60d9d4b5a76c85002f3d3f6d'), 'bodybuilder name': 'Raymont edmonds', 'event': 'Physique'}
        {'_id': ObjectId('60d9d4b5a76c85002f3d3f6e'), 'bodybuilder name': 'Brandom Hendrickson', 'event': 'Physique'}
        {'_id': ObjectId('60d86e4b8f6e65d651043c40'), 'subject': 'back exercise', 'exercise': 'Dumbell row', 'set': 5, 'reps': 13}
        {'_id': ObjectId('60d870a38f6e65d651043c45'), 'subject': 'back exercise', 'exercise': 'Barbell row', 'set': 5, 'reps': 13}
        {'_id': ObjectId('60d870a38f6e65d651043c46'), 'subject': 'back exercise', 'exercise': 'Dumbell row', 'set': 5, 'reps': 13}
        {'_id': ObjectId('60d870a38f6e65d651043c48'), 'subject': 'back exercise', 'exercise': 'Deadlift', 'set': 5, 'reps': 13}
        {'_id': ObjectId('60d9d4b5a76c85002f3d3f66'), 'subject': 'back exercise', 'exercise': 'Barbell row', 'set': 5, 'reps': 13}
        {'_id': ObjectId('60d9d4b5a76c85002f3d3f67'), 'subject': 'back exercise', 'exercise': 'Dumbell row', 'set': 5, 'reps': 13}
        {'_id': ObjectId('60d9d4b5a76c85002f3d3f69'), 'subject': 'back exercise', 'exercise': 'Deadlift', 'set': 5, 'reps': 13}
        {'_id': ObjectId('60d86e4b8f6e65d651043c41'), 'subject': 'back exercise', 'exercise': 'Pull Up', 'set': 10.0, 'reps': 13}
        {'_id': ObjectId('60d870a38f6e65d651043c47'), 'subject': 'back exercise', 'exercise': 'Pull Up', 'set': 10.0, 'reps': 13}
        {'_id': ObjectId('60d9d4b5a76c85002f3d3f68'), 'subject': 'back exercise', 'exercise': 'Pull Up', 'set': 10.0, 'reps': 13}

- 중복되지 않은 exercise 필드의 개수를 구하기위해 distinct 함수 사용


      counts = db_exercise.distinct(key='exercise', filter={'set': 5})

      counts

      ['Barbell row', 'Deadlift', 'Dumbell row']

      len(counts)

      3