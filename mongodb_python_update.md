# mongoDB

## 파이썬으로 mongoDB 데이터 수정하기

- Document Update 하기
  - find_one() 메서드 : 가장 먼저 검색되는 한 Document만 수정하기
 


        db_exercise.find_one( { "exercise":"Dumbell row"} )
        
        {'_id': ObjectId('60dc7cf85e16ae94ed7dd5d6'),
         'subject': 'back exercise',
         'exercise': 'Dumbell row',
         'set': 5,
         'reps': 13}


        # exercise 키의 값이 Dumbell row인 document 안의 set 키의 값을 10으로 변경
        db_exercise.update_one( {"exercise" : "Dumbell row"},
             { 
                 "$set" : { "set" : 10 } 
             }
        )
        
        <pymongo.results.UpdateResult at 0x25f11417640>



        docs = db_exercise.find({"exercise":"Dumbell row"})
        for doc in docs:
            print(doc)

        {'_id': ObjectId('60dc7cf85e16ae94ed7dd5d6'), 'subject': 'back exercise', 'exercise': 'Dumbell row', 'set': 10, 'reps': 13}


update_many() : 조건에 맞는 모든 Document 수정하기
        

        # subject의 값이 back exercise인 모든 Document 안의 set의 값을 10으로 변경
        db_exercise.update_many( { "subject" : "back exercise" }, {"$set" : { "set" :10 }}
        <pymongo.results.UpdateResult at 0x25f114bbe40>
        

        
        docs = db_exercise.find({"subject":"back exercise"})
        for doc in docs:
            print(doc)
        
        
        {'_id': ObjectId('60dc7cf85e16ae94ed7dd5d5'), 'subject': 'back exercise', 'exercise': 'Barbell row', 'set': 10, 'reps': 13}
        {'_id': ObjectId('60dc7cf85e16ae94ed7dd5d6'), 'subject': 'back exercise', 'exercise': 'Dumbell row', 'set': 10, 'reps': 13}
        {'_id': ObjectId('60dc7cf85e16ae94ed7dd5d7'), 'subject': 'back exercise', 'exercise': 'Pull Up', 'set': 10, 'reps': 13}
        {'_id': ObjectId('60dc7cf85e16ae94ed7dd5d8'), 'subject': 'back exercise', 'exercise': 'Deadlift', 'set': 10, 'reps': 13}