# Sqlalchemy串接MySQL

首先安裝pymysql和sqlalchemy：

    pip install pymysql  
    pip install sqlalchemy

連線資料庫：

    from sqlalchemy import create_engine
    
    engine=create_engine("mysql+pymysql://[帳號]:[密碼]@[ip]:[port]/[database]",echo=True)

建立表：

    from sqlalchemy import MetaData, Table
    from sqlalchemy import Column, String, Integer
    
    metadata = MetaData(engine)
    table_name = Table('table_name',metadata,
        Column('id', Integer, primary_key=True),
        Column('column1', Integer),
        Column('column2', String(20), nullable=True),
    )
    metadata.create_all()

查詢表：

    from sqlalchemy.orm import sessionmaker
    
    DBSession = sessionmaker(bind=engine)
    session = DBSession()
    table_name = Table('table_name', metadata, autoload=True)
    query = session.query(table_name)
    for d in query.all():
        print d

插入表：

    engine.execute(table_name.insert(), column1=value) # value指的是要給欄位的值
    
關閉資料庫

    session.close()
    
更新ing