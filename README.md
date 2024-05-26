# Bank-Management-system-using-Python-and-MySQL
'''import mysql.connector

mydb=mysql.connector.connect(
    host='localhost',
    user='root',
    password='$Qwerty456',
    database='bank')
"""
mycursor=mydb.cursor()
mycursor.execute("create database bank")
mydb.commit()

mycursor=mydb.cursor()
mycursor.execute("Alter table account add column mob int")
mydb.commit()

mycursor=mydb.cursor()
mycursor.execute("Alter table account modify column mob varchar(10)")
mydb.commit()

mycursor=mydb.cursor()
mycursor.execute("create table account(accno int not null,name varchar(30),age int,address varchar(100),amt double(20,5),primary key(accno))")
mydb.commit()

mycursor.execute("create table amt(accno int,amtdeposit double(20,5),month varchar(10))")
mydb.commit()
mycursor=mydb.cursor()
mycursor.execute("Alter table amt add foreign key(accno) references account(accno)")
mydb.commit()
mycursor=mydb.cursor()
mycursor.execute(
    "CREATE TRIGGER before_insert
    BEFORE INSERT ON amt
    FOR EACH ROW
    BEGIN
        UPDATE account 
        SET amt = amt + NEW.amtdeposit 
        WHERE accno = NEW.accno;
    END"
)
mycursor=mydb.cursor()
mycursor.execute("Alter table amt add column amtwithdraw double(10,5)")
mydb.commit()

mydb.commit()

mycursor=mydb.cursor()
mycursor.execute("
    CREATE TRIGGER withdraw
    BEFORE INSERT ON amt
    FOR EACH ROW
    BEGIN
        UPDATE account 
        SET amt = amt - NEW.amtwithdraw 
        WHERE accno = NEW.accno;
    END"
)
mydb.commit()
import mysql.connector
import pandas as pd
mydb=mysql.connector.connect(
    host='localhost',
    user='root',
    password='$Qwerty456')"""

mycursor=mydb.cursor()
def get_account_number():
    # Fetch the last account number from the database
    mycursor.execute("SELECT MAX(accno) FROM account")
    result = mycursor.fetchone()
    if result[0]:
        return result[0] + 1  # Increment the last account number by 1
    else:
        return 100000  # Start from 100000 if no account exists in the database

def accopen():
    l=[]
    accno=get_account_number()
    l.append(accno)
    name=input("Enter the customer name:")
    l.append(name)
    age=int(input("Enter age of customer:"))
    l.append(age)
    address=input("Enter address of customer:")
    l.append(address)
    mob=int(input("Enter mobile no:"))
    l.append(mob)
    amt=float(input("Enter the money deposited:"))
    l.append(amt)
    upload=(l)
    sql="insert into account(accno,name,age,address,mob,amt)values (%s,%s,%s,%s,%s,%s)"
    mycursor.execute(sql,upload)
    mydb.commit()
    print("Account successfully created.\n Your account number:",accno)
    print("Would you like to continue the service:")
    user_choice=int(input("1: Yes"
          "\n2: No"
          "\nEnter your choice:"))
    if user_choice==1:
        menuset()
    else:
        pass          

def accdeposit():
    l=[]
    accno=int(input("Enter the account no:"))
    mycursor.execute("select accno from account")
    result=[item[0] for item in mycursor.fetchall()]
    if accno in result:
        l.append(accno)
        amtdeposit=float(input("Enter the amount to be deposited:"))
        l.append(amtdeposit)
        month=input("Enter the month:")
        l.append(month)
        amtwithdraw=0
        l.append(amtwithdraw)
        upload=(l)
        sql="insert into amt(accno,amtdeposit,month,amtwithdraw)values(%s,%s,%s,%s)"
        mycursor.execute(sql,upload)
        mydb.commit()
    else:
        print("Invalid Account Number")
    print("Would you like to continue the service:")
    user_choice=int(input("1: Yes"
          "\n2: No"
          "\nEnter your choice:"))
    if user_choice==1:
        menuset()
    else:
        pass

def withdraw():
    l=[]
    accno=int(input("Enter the account no:"))
    mycursor.execute("select accno from account")
    result=[item[0] for item in mycursor.fetchall()]
    if accno in result:
        l.append(accno)
        amtdeposit=0
        l.append(amtdeposit)
        amtwithdraw=float(input("Enter the amount to be deposited:"))
        mycursor.execute("SELECT amt FROM account WHERE accno = %s", (accno,))
        result=mycursor.fetchone()
        if amtwithdraw<=result[0]:
            l.append(amtwithdraw)
            month=input("Enter the month:")
            l.append(month)
            upload=(l)
            sql="insert into amt(accno,amtdeposit,amtwithdraw,month)values(%s,%s,%s,%s)"
            mycursor.execute(sql,upload)
            mydb.commit()
            print("withdrawn successfully")
        else:
            print("Insufficient balance!")
    else:
        print("Account does not exist")
    
    print("Would you like to continue the service:")
    user_choice=int(input("1: Yes"
          "\n2: No"
          "\nEnter your choice:"))
    if user_choice==1:
        menuset()
    else:
        pass 

def accview():
    print("Please enter the details to view the Money details:")
    accno=int(input("Enter the account number :"))
    mycursor.execute("select accno from account")
    result=[item[0] for item in mycursor.fetchall()] # list comprehension
    if accno in result:
        sql="select * from account where accno=%s"
        rl=(accno,)
        mycursor.execute(sql,rl)
        res=mycursor.fetchall()
        for x in res:
            print(x)
    else:
        print("invalid account number")
    print("Would you like to continue the service:")
    user_choice=int(input("1: Yes"
          "\n2: No"
          "\nEnter your choice:"))
    if user_choice==1:
        menuset()
    else:
        pass 

def closeacc():
    accno = int(input("Enter the account number of the customer to be closed: "))
    mycursor.execute("select accno from account")
    result=[item[0] for item in mycursor.fetchall()]
    if accno in result:
        rl = (accno,)
        sql = "DELETE FROM account WHERE accno=%s"
        mycursor.execute(sql, rl)
        mydb.commit()
        print("Account succesfully closed")
    else:
        print("Please enter a valid account number")

def menuset():
    userinput=int(input("Welcome"
                        "\n 1: To open account"
                        "\n 2: To Deposit Money"
                        "\n 3: To withdraw Money"
                        "\n 4: To Close Account"
                        "\n 5: To View All Details of a Customer"
                        "\n 6: Quit"
                        "\n Enter your choice:"))
    if userinput == 1:
        accopen()
    elif userinput == 2:
        accdeposit()
    elif userinput == 3:
        withdraw()
    elif userinput == 4:
        closeacc()
    elif userinput == 5:
        accview()
    elif userinput == 6:
        print("Thankyou \nVisit Again")
    else:
        print("Enter correct choice....")
menuset()



    
              
    
'''
