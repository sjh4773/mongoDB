# mongDB

## Document 읽기 - findOne, find

- findOne : 매칭되는 한개의 document 검색
- find : 매칭되는 list of document 검색


        db.people.find() - SELECT * FROM people
        
        // 대괄호는 특별한 조건없이 전체 데이터를 가져온다는 의미
        // {  }, {user_id: 1, status: 1} -> 전체 데이터를 가져오되 그중에 user_id와 status만 가져오라는 의미
        // 각각의 document에는 원래 _id가 default로 존재한다.
        db.people.find({  }, { user_id: 1, status: 1 } ) - SELECT _id, user_id, status FROM people
        
        // _id를 제거하고 싶으면 명시적으로 _id를 적어야하며 _id: 0을 작성함으로써 제거할 수 있다.
        db.people.find({  }, { user_id: 1, status: 1, _id: 0 }) - SELECT user_id, status FROM people
        
        // 수 많은 document 데이터 중에 status라는 키값의 value가 A인것만 찾고 싶을 때 다음과 같이 작성한다.
        db.people.find( { status: "A" } ) - SELECT * FROM people WHERE status = "A"
        
        // 두 가지의 조건을 가질 때
        db.people.find( { status: "A", age: 50 }) - SELECT * FROM people WHERE status = "A" AND age = 50
        
        // status가 A이거나 age가 50인 데이터를 찾을 경우
        db.people.find( { $or: [ { status: "A" }, { age: 50 } ] }) - SELECT * FROM people WHERE status = "A" OR age = 50
        
        // user_id가 "bcd002"인 데이터를 찾아라
        db.employees.find({ user_id: "bcd002"})
        
        // user_id가 "bcd003"인 데이터를 찾고 _id를 제외해라.
        db.employees.find({ user_id: "bcd003"}, { user_id:1, age:1, status:1, _id:0 })
        
        // user_id가 "bcd004"이거나 age가 28인 데이터를 찾고 _id를 제외해라.
        db.employees.find({ $or: [ { user_id: "bcd004"}, { age: 28} ] }, {_id:0})

## 비교 문법

        $eq  :  =  , 지정된 값과 같은 값을 일치시킵니다.
        $gt  :  >  , 지정된 값보다 큰 값을 일치시킵니다.
        $gte :  >= , 지정된 값보다 크거나 같은 값을 일치시킵니다.
        $lt  :  <  , 지정된 값보다 작은 값을 일치시킵니다.
        $lte :  <= , 지정된 값보다 작거나 같은 값을 일치시킵니다.
        $ne  :  != , 지정된 값과 같지 않은 모든 값을 일치시킵니다.
        $nin :       어레이에 지정된 값과 일치하지 않는 것을 찾는다.

        db.people.find({ age : { $gt: 25 } }) - SELECT * FROM people WHERE age > 25
        db.people.find({ age : { $lt: 25 } }) - SELECT * FROM people WHERE age < 25
        db.people.find({ age : { $gt: 25, $lte: 50 } }) - SELECT * FROM people WHERE age > 25, age <= 50
        
        db.people.find( { user_id: /bc/ })
        db.people.find( { user_id: { $regex: /^bc/ } } ) - SELELCT * FROM people WHERE user_id LIKE "bc%"
        
        db.people.find( { status: "A" } ).sort( { user_id: 1 } ) - SELECT * people WHERE status = "A" ORDER BY user_id ASC
        db.people.find( { status: "A" } ).sort( { user_id: -1 } ) - SELECT * people WHRER status = "A" ORDER BY user_id DESC
        
        db.people.count()
        db.people.find().count() - SELECT COUNT(*) FROM people
        
        db.people.count( { user_id: { $exists: true } } )
        db.people.find( { user_id: { $exists: true } } ).count() - SELECT COUNT(user_id) FROM people
        
        db.people.count( { age: { $gt: 30 } } )
        db.people.find( { age: { $gt: 30 } } ).count() - SELECT COUNT(*) FROM people WHERE age > 30
        
        db.people.distinct( "status" ) - SELECT DISTINCT(status) FROM people
        
        db.people.findOne()
        db.people.find().limit(1) - SELECT * FROM people LIMIT 1
