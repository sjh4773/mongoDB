# mongoDB

## Document 입력 - insertOne, insertMany

- insertOne : 한개의 document 생성

        db.users.insertOne(
            {
                name: "sue",
                age: 26,
                status: "pending"
            }
        )

- insertMany : list of document 생성

        db.users.insertMany(
            [
                { subject: "back exercise", exercise: "Barbell row", set: 5, reps: 13},
                { subject: "back exercise", exercise: "Lat pulldown", set: 5, reps: 13},
                { subject: "back exercise", exercise: "Barbell deadlift", set: 5, reps: 13},
                { subject: "back exercise", exercise: "Wide dumbell row", set: 5, reps: 13},
                { subject: "back exercise", exercise: "Pull up", set: 5, reps: 13}
            ]
        )

        db.employees.insertMany(
            [
                { user_id: "bcd001", age: 45, status: "A"},
                { user_id: "bcd002", age: 25, status: "B"},
                { user_id: "bcd003", age: 50, status: "A"},
                { user_id: "bcd004", age: 35, status: "A"},
                { user_id: "abc001", age: 28, status: "B"}
            ]
        )