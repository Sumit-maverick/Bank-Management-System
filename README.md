# Bank-Management-System
#Python 3.12.0 (tags/v3.12.0:0fb18b0, Oct  2 2023, 13:03:39) [MSC v.1935 64 bit (AMD64)] on win32
#Type "help", "copyright", "credits" or "license()" for more information.
import os
from os import system
import pathlib
import pickle


def clear_screen():
    """Clear the terminal display for a more graphical CLI experience."""
    system('cls' if os.name == 'nt' else 'clear')


def print_title(title):
    width = max(len(title) + 8, 50)
    print('\n' + '*' * width)
    print('*' + title.center(width - 2) + '*')
    print('*' * width)


class Account :
  accNo = 0
  name = ''
  deposit=0 
  type = ''

  def createAccount(self):
    self.accNo= int(input("Enter the account no : ")) 
    self.name = input("Enter the account holder name : ") 
    self.type = input("Ente the type of account [C/S] : ")
    if self.type == "C":
      self.deposit = int(input("Enter The Initial amount(>=500 for Current) : "))
      if self.deposit < 500:
        print("\n\n\nInvailid Ammount")
      else:
        print("\n\n\nAccount Created")
    elif self.type == "c":
      self.deposit = int(input("Enter The Initial amount(>=1000 for Current) : "))
      if self.deposit < 500:
        print("\n\n\nInvailid Ammount")
      else:
        print("\n\n\nAccount Created")
    elif self.type == "S":
      self.deposit = int(input("Enter The Initial amount(>=1000 for Saving) : "))
      if self.deposit < 1000:
        print("\n\n\nInvailid Ammount")
      else:
        print("\n\n\nAccount Created")
    elif self.type == "s":
      self.deposit = int(input("Enter The Initial amount(>=1000 for Saving) : "))
      if self.deposit < 1000:
        print("\n\n\nInvailid Ammount")
      else:
        print("\n\n\nAccount Created")
      

  def showAccount(self):
    print("Account Number : ",self.accNo) 
    print("Account Holder Name : ", self.name) 
    print("Type of Account",self.type) 
    print("Balance : ",self.deposit)

  def modifyAccount(self):
    print("Account Number : ",self.accNo)
    self.name = input("Modify Account Holder Name :") 
    self.type = input("Modify type of Account :") 
    self.deposit = int(input("Modify Balance :"))

  def depositAmount(self,amount): 
    self.deposit += amount

  def withdrawAmount(self,amount):
    self.deposit -= amount

  def report(self):
    print(self.accNo, " ",self.name ," ",self.type," ", self.deposit)


  def getAccountNo(self):
     return self.accNo
  def getAcccountHolderName(self):
     return self.name
  def getAccountType(self):
     return self.type
  def getDeposit(self):
     return self.deposit



def intro():
    clear_screen()
    print_title('BANK MANAGEMENT SYSTEM')
    print('Welcome to the Bank Management CLI.')
    print('Use this interface to create, update, and view accounts.')
    input('\nPress Enter to continue...')


def draw_menu():
    clear_screen()
    print_title('MAIN MENU')
    print('  1. NEW ACCOUNT'.ljust(34) + '5. ALL ACCOUNT HOLDER LIST')
    print('  2. DEPOSIT AMOUNT'.ljust(34) + '6. CLOSE AN ACCOUNT')
    print('  3. WITHDRAW AMOUNT'.ljust(34) + '7. MODIFY AN ACCOUNT')
    print('  4. BALANCE ENQUIRY'.ljust(34) + '8. EXIT')
    print('\nSelect Your Option (1-8): ')


def writeAccount():
  account = Account()
  account.createAccount()
  writeAccountsFile(account)


def displayAll():
  file = pathlib.Path("accounts.data") 
  if file.exists ():
    infile = open('accounts.data','rb') 
    mylist = pickle.load(infile)

    for item in mylist :
      print(item.accNo," ", item.name, " ",item.type, " ",item.deposit )
    infile.close() 
  else :
    print("No records to display")



def displaySp(num):
  file = pathlib.Path("accounts.data") 
  if file.exists ():
    infile = open('accounts.data','rb')
    mylist = pickle.load(infile) 
    infile.close()
    found = False
    for item in mylist :
      if item.accNo == num :
        print("Your account Balance is = ",item.deposit)
        found = True
  else :
    print("No records to Search")
  if not found :
    print("No existing record with this number")


def depositAndWithdraw(num1,num2):
  file = pathlib.Path("accounts.data")
  if file.exists ():
    infile = open('accounts.data','rb')
    mylist = pickle.load(infile) 
    infile.close() 
    os.remove('accounts.data')
    for item in mylist :
      if item.accNo == num1: 
        if num2 == 1 :
          amount = int(input("Enter the amount to deposit : "))
          item.deposit += amount
          print("Your account is updted") 
        elif num2 == 2 :
          amount = int(input("Enter the amount to withdraw :"))
          if amount <= item.deposit : 
            item.deposit -=amount
          else :
            print("You cannot withdraw larger amount")


  else :
    print("No records to Search")
  outfile = open('newaccounts.data','wb')
  pickle.dump(mylist, outfile) 
  outfile.close()
  os.rename('newaccounts.data', 'accounts.data')



def deleteAccount(num):
  file = pathlib.Path("accounts.data") 
  if file.exists ():
    infile = open('accounts.data','rb') 
    oldlist = pickle.load(infile) 
    infile.close()
    newlist = []
    for item in oldlist :
      if item.accNo != num :
        newlist.append(item)
    os.remove('accounts.data')
    outfile = open('newaccounts.data','wb') 
    pickle.dump(newlist, outfile) 
    outfile.close()
    os.rename('newaccounts.data', 'accounts.data')

def modifyAccount(num):
  file = pathlib.Path("accounts.data") 
  if file.exists ():
    infile = open('accounts.data','rb') 
    oldlist = pickle.load(infile) 
    infile.close()        
    os.remove('accounts.data')
    for item in oldlist :
      if item.accNo == num :
         item.name = input("Enter the account holder name : ")
         item.type = input("Enter the account Type : ") 
         item.deposit = int(input("Enter the Amount : "))

    outfile = open('newaccounts.data','wb')             
    pickle.dump(oldlist, outfile) 
    outfile.close()
    os.rename('newaccounts.data', 'accounts.data')




def writeAccountsFile(account) :


  file = pathlib.Path("accounts.data") 
  if file.exists ():
    infile = open('accounts.data','rb') 
    oldlist = pickle.load(infile)                 
    oldlist.append(account) 
    infile.close() 
    os.remove('accounts.data')
  else :
    oldlist = [account]
  outfile = open('newaccounts.data','wb')       
  pickle.dump(oldlist, outfile) 
  
  outfile.close()

  os.rename('newaccounts.data', 'accounts.data')




# start of the program 
intro()

while True:
    draw_menu()
    ch = input().strip()

    if ch == '1':
        writeAccount()
    elif ch == '2':
        num = int(input('Enter the account No.: '))
        depositAndWithdraw(num, 1)
    elif ch == '3':
        num = int(input('Enter the account No.: '))
        depositAndWithdraw(num, 2)
    elif ch == '4':
        num = int(input('Enter the account No.: '))
        displaySp(num)
    elif ch == '5':
        displayAll()
    elif ch == '6':
        num = int(input('Enter the account No.: '))
        deleteAccount(num)
    elif ch == '7':
        num = int(input('Enter the account No.: '))
        modifyAccount(num)
    elif ch == '8':
        clear_screen()
        print_title('THANK YOU')
        print('Thanks for using the Bank Management System.')
        break
    else:
        print('\nInvalid choice. Please select a number between 1 and 8.')
        input('\nPress Enter to continue...')

    input('\nPress Enter to return to the main menu...')
