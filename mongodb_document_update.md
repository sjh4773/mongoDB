# mongoDB

## Document 수정 문법

- updateOne - 매칭되는 한개의 document 업데이트
- updateMany - 매칭되는 list of document 업데이트
- $set: field 값 설정
- $inc: field 값을 증가시키거나, 감소시킴 
  
        ex) $inc: { age: 2 } - age 값을 본래의 값에서 2 증가



        // $lt : less than
        // age가 18보다 적은 document들을 모두 찾아서 document 내의 status 값을 "reject"로 변경하라
        db.users.updateMany( { age: { $lt: 18 } }, { $set: { status: "reject" } } )
        
        // $gt : greater than
        // age가 25보다 큰 값을 가지는 document를 찾아서 status라는 값을 "C"로 바꿔라
        db.people.updateMany( { age: { $gt: 25 } }, { $set: { status: "C" } } )
        -> UPDATE people SET status = "C" WHERE age > 25
        
        // age가 25보다 큰 값을 가지는 document를 하나 찾아서 status를 "C"로 바꿔라
        db.people.updateOne( { age: { $gt" 25 } }, { $set" { status: "C" } } )
        
        
        // status의 값이 "A" 값인 document를 모두 찾아서 각각의 document 내에 age 값을 3씩 증가시킨다.
        db.people.updateMany( { status: "A" }, $inc: { age : 3 } } ) 
        -> UPDATE people SET age = age + 3 WHERE status = "A"


// 연습

        1. people document 생성
        
        db.createCollection("people")
        
        2. list of document 생성
        
        db.people.insertMany(
            [
                { user_id: "abc123", age: 55, status: "A" },
                { user_id: "abc234", age: 55, status: "BB" },
                { user_id: "abc345", age: 55, status: "A" },
                { user_id: "abc457", age: 55, status: "A" },
                { user_id: "abc567", age: 55, status: "b" }
            ]
        )
        
        3. age가 40보다 큰 Document의 status를 B로 변환하기
        
        db.people.updateMany( { age: { $gt: 40 } }, { $set: { status: "B" } } )
