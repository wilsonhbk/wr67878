import re
import os
def login_system():
    print("1 - Register")
    print("2 - Login")

    while True:
        Choice = input('Choose An Option: ')
        if Choice in ['1', '2']:
            break
    if Choice == '1':
        registration()
    else:
        login()

def registration():
    email=input('Enter Username: ')
    password=input('Enter Password: ')
    #email validation
    
        
    symbols = '`~!@#$%^&*()_-+={[}]|\:;.<>'
    flag=0
    
    for i in symbols:           # special char validation
      if i ==email[0]:    
        flag=1
    
    x=re.findall('^\d',email)   # num validation 
    if x!=[]:          
        flag=1
    
    x=re.findall('.+@\..+',email)  # @. validation
    if x!=[]:
        flag=1  
    
    if flag==0:                   #email/username should have "@" and followed by "."
        x = re.findall(".+@.+\.[a-z]+\Z",email)       
        if x!=[]:
            flag=2
        else:
            flag=1
            #print('Enter valid Username')
    
    # password validation
    
    count=0
    
    l=len(password)     # len validation
    if l<5 and l>16:                          
        count=1
    
    x=re.findall('\d',password)      # digit validation     
    if x==[]:                                 
        count=1
    
    x=re.findall('[A-Z]+',password)  # upper validation
    if x==[]:                                  
        count=1
    
    x=re.findall('[a-z]+',password)  # lower validation
    if x==[]:                                  
        count=1
    
    x=re.findall('[@_!$%^&*()<>?/\|}{~:#]',password) #special_characters validation
    if x==[]:                               
        count=1
        #print('invalid password')
    

    #data storing in file
    
    if count==0 and flag==2:
      if os.path.exists('email_file.txt'):
        file=open('email_file.txt','a+')
        file.write(email+' '+password)
        file.write('\n')
        file.close()
      else:
        file=open('email_file.txt','x')
        file=open('email_file.txt','w')
        file.write(email+' '+password)
        file.write('\n')
        file.close()
      print('Successfully Registered!')
    else:
      print('Something Went Wrong')

# login
def login():
    e=[]
    p=[]
    email=input('Enter Username: ')
    password=input('Enter Password: ')
    file=open('email_file.txt','r')
    for i in file:
        a,b=i.split(' ')
        e.append(a)
        p.append(b.strip('\n'))
    if email in e:
        z=e.index(email)
        if password in p:
            y=p.index(password)
            print('Successfully logged in!')
        else:
            print('password does not matched')
            print('press 1 - Forget password')
            print('press 2 - Create New Password')
            while True:
              choice=input('choose An option: ')
              if choice in ['1','2']:
                break
            if choice=='1':
              forget_password()
            else:
              password=input('Enter New Password: ')

    else:
      print("press 1 - Registration")
      print("press 2 - Forget password")
      print("press 3 - Create New Password")
      while True:
        Choice = input("Choose An Option: ")
        if Choice in ['1', '2','3']:
            break
      if Choice == '1':
        registration()
      elif Choice == '2':
        forget_password()
      else:
        New_Password=input('Enter New Password: ')
        print('Password Changed')

# Forget password
def forget_password():
  email=input('Enter Username: ')
  d=[]
  f=[]
  file=open('email_file.txt','r')
  for i in file:
    a,b=i.split(' ')
    d.append(a)
    f.append(b.strip('\n'))
  if email in d:
        pos=d.index(email)
        print('password:',f[pos])
  else:
    registration()

login_system()