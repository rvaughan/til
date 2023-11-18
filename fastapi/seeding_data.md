# Seeding Data

I found a need to be able to seed data in a SQLite database which was behind a FastAPI instance without having to run external tools. The simplest way of doing this seems to be:

```python
# The data you want to seed the database with
SEED_DATA = {
      '<table_1_name>': [
            {
                  'id': 0,
                  'field_1': 'some value',
                  'field_2': 'another value'
            },
            {
                  'id': 1,
                  'field_1': 'yet another value',
                  'field_2': 'final value'
            }
      ],
      '<table_2_name>': [
            {'column1': 'value', 'column2': 'value'}
      ]
}

# This method receives a table, a connection and inserts data to that table.
def initialize_table(target, connection, **kw):
    tablename = str(target)
    if tablename in SEED_DATA and len(SEED_DATA[tablename]) > 0:
        connection.execute(target.insert(), SEED_DATA[tablename])

from sqlalchemy import event
from models.<table> import <Table>

# This event gets set up before the app is created
event.listen(<Table>.__table__, 'after_create', initialize_table)

...

app = FastAPI()
```

This seems to work well, and seems fine if you only need to seed a couple of values into a few tables. If you need to do more than that then a more complex solution is going to be needed.

## References

* https://gist.github.com/jsmsalt/26bf25844870d59eee17997727e3a631
