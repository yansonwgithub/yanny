import time
import random
import threading #necessary when you want the timer method to run in the background while the rest of the code runs

class Node:
    def __init__(self,key):
        self.key=key
        self.right=None
        self.left=None
        
class BST:
    def __init__(self):
        self.root=None
        
    def insert_recursive(self, key):#code from lab 6
        self.root = self.insert_recursivehelper(self.root,key)
    
    def insert_recursivehelper(self,cur, key):
        new_node=Node(key)
        if cur is None:
            cur=new_node
        elif key < cur.key:
            if cur.left:
                cur.left=self.insert_recursivehelper(cur.left,key)
            else:
                cur.left=Node(key)
        elif key > cur.key:
            if cur.right:
                cur.right=self.insert_recursivehelper(cur.right,key)
            else:
                cur.right=Node(key)
        return cur
                        
                      
    def password_generator(self):#generates a random three digit password(https://www.geeksforgeeks.org/python-randint-function/)
        store=[]
        for i in range(3):
            num = str(random.randint(0,9))
            store.append(num)
        password = ''.join(store)#makes it a string
        return password
        
        
        
def countdown_timer(sec):#input is seconds https://medium.com/@sarahisdevs/how-to-create-a-countdown-timer-using-python-6a09fea53e67
    global timerDone
    while (sec > 0):
        time.sleep(1)  # Delay for 1 second
        sec -= 1
    if sec == 0:
        timerDone = True
        print(timerDone)

def game(pwd):#https://www.geeksforgeeks.org/multithreading-python-set-1/
    print('Welcome to the number guessing game with a twist!\nI will generate a random password that you have to guess.\nGuess the right password before the time limit or the bomb will explode!')
    print('Here is a hint!....Its a three digit number combo')
    global timerDone
    timethread = threading.Thread(target=countdown_timer,args=(5,))
    timethread.start()
    while (timerDone == False):
        print(timerDone)
        guess = input("Enter your 3-digit password: ")
        if guess == pwd:
            print('Congrats! Bomb has been diffused')
            return
        else:
            print('Incorrect, keep trying!')
    print('Time is up! Bomb has exploded!')
                    
    
def main():
    global timerDone
    timerDone = False
    bst=BST()
    password = bst.password_generator()
    for i in password:
        bst.insert_recursive(int(i))#inserts the generated password into the binary search tree
    game(password)
    
if __name__=='__main__':
    main()
