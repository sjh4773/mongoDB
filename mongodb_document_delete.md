# mongoDB

## Document 삭제 - removeOne, removeMany

- removeOne - 매칭되는 한개의 document 삭제

- removeMany - 매칭되는 list of document 삭제

## Document 삭제 문법

        // 내가 선택한 데이터베이스에 users라는 collection에 deleteMany라는 명령어를 써서 tatus가 "reject"인 document를 모두 삭제해라
        db.users.deleteMany( { status: "reject" } )
        
        // db라는 나의 데이터베이스에서 people이라는 collection을 찾아서 status가 "D"인 document를 모두 삭제해라.
        db.people.deleteMany( { status: "D" } )
        -> DELETE FROM people WHERE status = "D"
        
        // 조건이 없으면 결과적으로 collection 내의 모든 데이터를 선택한다.
        // collection 내부의 모든 데이터를 삭제
        db.people.deleteMany( { } )
        -> DELETE FROM people

## 연습

- age가 30보다 높은 Document 삭제하기

        -> db.people.deleteMany( { age: { $gt: 30 } } )
