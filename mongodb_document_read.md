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