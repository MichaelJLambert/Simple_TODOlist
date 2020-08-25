from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy import Column, Integer, String, Date
from datetime import datetime, timedelta
from sqlalchemy.orm import sessionmaker
days = {0: 'Monday', 1: 'Tuesday', 2: 'Wednesday', 3: 'Thursday', 4: 'Friday', 5: 'Saturday', 6: 'Sunday'}

engine = create_engine('sqlite:///todo.db?check_same_thread=False')
Base = declarative_base()


class Table(Base):
    __tablename__ = 'task'
    id = Column(Integer, primary_key=True)
    task = Column(String, default='default_value')
    deadline = Column(Date, default=datetime.today())

    def __repr__(self):
        return self.task


Base.metadata.create_all(engine)
Session = sessionmaker(bind=engine)
session = Session()


def menu():
    print("1) Today's tasks\n2) Week's tasks\n3) All tasks\n4) Missed Tasks\n5) Add task\n6) Delete Task\n0) Exit")


def task_of_the_day():
    today = datetime.today()
    rows = session.query(Table).filter(Table.deadline == today.date()).all()
    print("Today:")
    if not rows:
        print("Nothing to do!")
    else:
        print(rows[0].task)
    print()


def weeks_tasks():
    today = datetime.today()
    for i in range(7):
        ith_day = today + timedelta(days=i)
        rows = session.query(Table).filter(Table.deadline == ith_day.date()).all()
        print(f"{days[ith_day.weekday()]} {ith_day.day} {ith_day.strftime('%b')}:")
        if not rows:
            print("Nothing to do!")
        else:
            for task in rows:
                print(f"{task}")
        print()


def all_tasks():
    rows = session.query(Table).order_by(Table.deadline).all()
    print()
    print("All tasks:")
    for i, task in enumerate(rows):
        print(f"{i}. {task}. {task.deadline.day} {task.deadline.strftime('%b')}")
    print()


def missed_tasks():
    print()
    print("Missed tasks:")
    rows = session.query(Table).filter(Table.deadline < datetime.today().date()).all()
    for i, task in enumerate(rows, start=1):
        print(f"{i}. {task}. {task.deadline.day} {task.deadline.strftime('%b')}")
    print()


def add_task():
    print("Enter task")
    new_task = input()
    print("Enter deadline")
    new_task_deadline = input()
    new_row = Table(task=new_task, deadline=datetime.strptime(f'{new_task_deadline}', '%Y-%m-%d').date())
    session.add(new_row)
    session.commit()
    print("The task has been added!")
    print()


def delete_task():
    print()
    print("Choose the number of the task you want to delete:")
    rows = session.query(Table).order_by(Table.deadline).all()
    for i, task in enumerate(rows, start=1):
        print(f"{i}. {task}. {task.deadline.day} {task.deadline.strftime('%b')}")
    deleted = int(input())
    session.delete(rows[deleted-1])
    session.commit()
    print("The task has been deleted!")
    print()


while True:
    menu()
    choice = input()
    if choice == '1':
        task_of_the_day()
    elif choice == '2':
        weeks_tasks()
    elif choice == '3':
        all_tasks()
    elif choice == '4':
        missed_tasks()
    elif choice == '5':
        add_task()
    elif choice == '6':
        delete_task()
    elif choice == '0':
        print("Bye!")
        exit()
