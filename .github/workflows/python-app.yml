import mysql.connector as t
import random
import statistics

mydb=t.connect(host='localhost',user='root',passwd='Sri*1977',database='crime')
mycursor=mydb.cursor()

#function for register

def register():
    try:
        name=input("Enter your name :")
        des=input("Enter your post :")
        id=random.randint(1000,9999)
        print("Your Agency_Id :",id)
        mycursor.execute("Insert into agency values(%s,%s,%s)",(id,name,des))
        mydb.commit()
        print("REGISTRATION SUCCESSFULL")
    except:
        print("Registration Failed!! Please try again")
        
#function for login        

def login():
    id=int(input("Enter your Agency_Id :"))
    mycursor.execute("Select Agency_Id from agency")
    rec=mycursor.fetchall()
    for r in rec:
        if id in r:
            print("WELCOME BACK")
            print("You are successfully logged in")
            det()
            break
    else:
        print("INVALID ID")

#function for accessing data for logged user        
            
def det():
    global mycursor
    ans='y'
    while ans.lower()=='y':
        print("Options")
        print("1.Add Criminal")
        print("2.Search Criminal by FIR_ID")
        print("3.Search Criminal by crime")
        print("4.Update crime records")
        print("5.Delete Criminals")
        print("6.Statistics")
        print("7.Complains")
        print("8.Exit")
        
        c=int(input("Enter your choice (1-8) :"))
        if c==1:
            id=int(input("Enter the FIR_ID :"))
            name=input("Enter the name of the criminal :")
            age=int(input("Enter the age :"))
            gen=input("Enter the gender :")
            ca=int(input("Enter no of cases filed on the criminal :"))
            crimetype=input("Enter the type of crime :")
            city=input("Enter the city :")
            mycursor.execute("Insert into criminal values(%s,%s,%s,%s,%s)",(id,name,age,gen,ca))
            mycursor.execute("Insert into crime_details(FIR_ID,CrimeType,City) values(%s,%s,%s)",(id,crimetype,city))
            mydb.commit()
            print("Criminal Added Successfully")
            
        elif c==2:
            id=int(input("Enter the FIR_ID :"))
            mycursor.execute("Select FIR_Id from criminal")
            rec=mycursor.fetchall()
            for r in rec:
                if id in r:
                    mycursor.execute(''' SELECT criminal.Criminal_Name, criminal.Age, criminal.Gender, criminal.No_of_Cases, crime_details.CrimeType, crime_details.City
                                                  FROM criminal JOIN crime_details ON criminal.FIR_ID = crime_details.FIR_ID
                                                  WHERE criminal.FIR_ID = %s''', (id,))
                    ab=mycursor.fetchone()
                    print("Name of the criminal :",ab[0])
                    print("Age :",ab[1])
                    print("Gender :",ab[2])
                    print("No of cases filed :",ab[3])
                    print("Crime filed :",ab[4])
                    print("City :",ab[5])
                elif r not in rec:
                    print("ID not found")
                    
        elif c==3:
            crime=input("Enter the crime type :")
            mycursor.execute("Select crimetype from crime_details")
            rec=mycursor.fetchall()
            mycursor.close()          
            s=[]
            for e in rec:
                if crime in e:
                    mycursor=mydb.cursor()  
                    mycursor.execute('''Select crime_details.FIR_ID,criminal.Criminal_Name from criminal,crime_details
                                                   where criminal.FIR_ID=crime_details.FIR_ID and crimetype=%s''',(crime,))
                    ss=mycursor.fetchall()
                    for sss in ss:
                        s.append(sss[1])
                    print("Criminals List")
                    for a in s:
                        print(a)
                elif crime in e:
                        print("No such crime")
                        
        elif c==4:
            id=int(input("Enter the FIR_ID for the updation :"))
            mycursor.execute("Select FIR_ID from criminal")
            t=mycursor.fetchall()
            for j in t:
                if id in j:
                    print("Update :")
                    print("1.Criminal Name")
                    print("2.Age")
                    print("3.Gender")
                    print("4.No of Cases filed")
                    print("5.Crime Type")
                    print("6.City")
                    print("7.Status of Crime")
                    z=int(input("Enter your choice :"))
                    if z==1:
                        uname=input("Enter the new name to be updated :")
                        mycursor.execute("Update criminal set Criminal_Name=%s where FIR_ID=%s",(uname,id))
                        mydb.commit()
                        print("Name Updated")
                    elif z==2:
                        uage=int(input("Enter the new age :"))
                        mycursor.execute("Update criminal set Age=%s where FIR_ID=%s",(uage,id))
                        mydb.commit()
                        print("Age Updated")
                    elif z==3:
                        ugen=input("Enter the new gender :")
                        mycursor.execute("Update criminal set Gender=%s where FIR_ID=%s",(ugen,id))
                        mydb.commit()
                        print("Gender Updated")
                    elif z==4:
                        uno=int(input("Enter the new no of cases filed :"))
                        mycursor.execute("Update criminal set No_of_cases=%s where FIR_ID=%s",(uno,id))
                        mydb.commit()
                        print("No of cases Updated")
                    elif z==5:
                        ucrime=input("Enter the new crime :")
                        mycursor.execute("Update crime_details set crimetype=%s where FIR_ID=%s",(ucrime,id))
                        mydb.commit()
                        print("Crime Type Updated")
                    elif z==6:
                        ucity=input("Enter the new city :")
                        mycursor.execute("Update crime_details set city=%s where FIR_ID=%s",(ucity,id))
                        mydb.commit()
                        print("City Updated")
                    elif z==7:
                        ustatus=input("Enter the status of the crime :")
                        mycursor.execute("Update crime_details set Status=%s where FIR_ID=%s",(ustatus,id))
                        mydb.commit()
                        print("Status Updated Successfully")
                    else:
                        print("INVALID choice")
                elif id not in j:
                    print("No such FIR_ID")
                    
        elif c==5:
            del_id=int(input("Enter the FIR_ID to be deleted :"))
            anss=input("The records of "+str(del_id)+" will be deleted permanently, Are you sure to delete?(y/n) :")
            if anss.lower()=='y':
                mycursor.execute("Delete from criminal where FIR_ID=%s",(del_id,))
                mycursor.execute("Delete from crime_details where FIR_ID=%s",(del_id,))
                mydb.commit()
                print("Record deleted successfully")
            else:
                break
            
        elif c==6:
            mycursor.execute("Select criminal_name from criminal order by no_of_cases DESC")
            t=mycursor.fetchall()
            print("Most WANTED Criminal :",t[0][0])
            mycursor.execute("Select sum(no_of_cases) from criminal")
            m=mycursor.fetchall()
            print("Total no of cases :",m[0][0])
            mycursor.execute("Select count(*) from complain")
            n=mycursor.fetchall()
            print("Total no of Complaints :",n[0][0])
            mycursor.execute("Select crimetype from crime_details")
            o=mycursor.fetchall()
            for a in o:
                aa=statistics.mode(a)
            print("Most occured Crime :",aa)
            
        elif c==7:
            d={}
            mycursor.execute("Select Com_Name,Complaint from complain")
            com=mycursor.fetchall()
            for a in com:
                d[a[0]]=a[1]
            print("Dictionary contains the details of complains")
            print("Dictionary is in the form of {Complaintant:Complain}")
            print(d)
            
        else:
            break
        ans=input("Want to access records?(y/n) :")
        
#function for people to complain      
        
def complain():
    cname=input("Enter the name of the complaintant :")
    age=int(input("Enter the age of the complaintant :"))
    gender=input("Enter your Gender :")
    comp=input("Enter complaint in one word :")
    ccname=input("Enter the criminal`s name :")
    contact=int(input("Enter your contact number :"))
    mycursor.execute("Insert into complain values(%s,%s,%s,%s,%s)",(cname,age,gender,comp,contact))
    mydb.commit()
    mycursor.execute("Select Criminal_name from criminal")
    t=mycursor.fetchall()
    for a in t:
        if ccname in a:
            print(ccname,"is already in custody")
            print("So, Don`t worry")
    else:
        print("Within 24 hours,we will find the criminal")

#__main__
    
print("CRIME MANAGEMENT SYSTEM")
print("1.Register")
print("2.Login")
print("3.Complaintant")
print("4.Exit")
ch=int(input("Enter your choice (1-4) :"))
while True:
    if ch==1:
        register()
        break
    elif ch==2:
        login()
        break
    elif ch==3:
        complain()
        break
    elif ch==4:
        break
    else:
        print("INVALID choice")
        break
mydb.close()
