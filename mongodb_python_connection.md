# mongoDB

## 파이썬으로 mongoDB 제어

1. pymongo 라이브러리 import
2. mongodb 접속
3. 내가 사용할 database, collection 생성 또는 선택
4. 해당 database의 collection에 CRUD 명령하는 방법

## mongodb with EC2 Connection via using pymongo

- pymongo 라이브러리 import

        import pymongo

    import pymongo
    conn= pymongo.MongoClient() # 주소를 안적었을 경우 무조건 자기 pc의 port 27017번에 접속된다.

# mongodb 접속
    connection = pymongo.MongoClient('mongodb://13.124.94.52') # 나의 AWS 서버에 원격으로 접속

# test Database 사용하기
# 만약 test라는 데이터베이스가 없으면 새로 생성된다.
    db = conn.test
    
    db = conn["test"] # 또 다른 방법
    
    print(db)
    
    Database(MongoClient(host=['localhost:27017'], document_class=dict, tz_aware=False, connect=True), 'test')
    
    print(dir(db)) # 객체가 가지고 있는 변수와 함수를 나열
    
    ['_BaseObject__codec_options', '_BaseObject__read_concern', '_BaseObject__read_preference',
    '_BaseObject__write_concern', '_Database__client', '_Database__incoming_copying_manipulators',
    '_Database__incoming_manipulators', '_Database__name', '_Database__outgoing_copying_manipulators',
    '_Database__outgoing_manipulators', '__call__', '__class__', '__delattr__', '__dict__', '__dir__',
    '__doc__', '__eq__', '__format__', '__ge__', '__getattr__', '__getattribute__', '__getitem__', '__gt__',
    '__hash__', '__init__', '__init_subclass__', '__iter__', '__le__', '__lt__', '__module__', '__ne__', '__new__',
    '__next__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__',
    '__weakref__', '_apply_incoming_copying_manipulators', '_apply_incoming_manipulators', '_command',
    '_create_or_update_user', '_current_op', '_default_role', '_fix_incoming', '_fix_outgoing', '_list_collections',
    '_read_preference_for', '_retryable_read_command', '_write_concern_for', 'add_son_manipulator', 'add_user',
    'aggregate', 'authenticate', 'client', 'codec_options', 'collection_names', 'command', 'create_collection',
    'current_op', 'dereference', 'drop_collection', 'error', 'eval', 'get_collection', 'incoming_copying_manipulators', 'incoming_manipulators', 'last_status', 'list_collection_names',
    'list_collections', 'logout', 'name', 'next', 'outgoing_copying_manipulators', 'outgoing_manipulators',
    'previous_error', 'profiling_info', 'profiling_level', 'read_concern', 'read_preference', 'remove_user', 'reset_error_history', 'set_profiling_level', 'system_js', 'validate_collection', 'watch', 'with_options', 'write_concern']
    
    print(db.name) # 해당 데이터베이스의 name 확인

## db_exercise라는 collection 사용하기(없으면 만들어진다.)

    db_exercise = db.excercise
    
    db_exercise = db["excercise"]
    
    db_exercise # db_exercise라는 변수는 데이터베이스가 test이고 그안에 exercise라는 collection을 지칭하고 있다.
    
    Collection(Database(MongoClient(host=['localhost:27017'], document_class=dict, tz_aware=False, connect=True), 'test'), 'excercise')
    
