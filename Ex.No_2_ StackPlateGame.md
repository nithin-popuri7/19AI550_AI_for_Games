# Ex.No: 2 Implementation of Stack Plate game using Queue 
### DATE: 09.08.2024                                                                            
### REGISTER NUMBER : 212221240037
### AIM: 
To write a python program to simulate the process of stacking plates.
### Algorithm:
1. Initialize the Stack
2. Create an empty list to represent the stack.
3. Push the plate on top of stack
4. Pop the plate from top.
5. Display the plate details.
6. Create an interactive menu and display it.
### Program:
```py
import queue

class Plate:
    def _init_(self, color, size):
        self.color = color
        self.size = size

    def _str_(self):
        return f"{self.color} plate, size {self.size}"

class StackPlateGame:
    def _init_(self):
        self.stack = queue.LifoQueue()

    def push_plate(self, plate):
        self.stack.put(plate)
        print(f"Pushed {plate} onto the stack")

    def pop_plate(self):
        if self.stack.empty():
            print("Stack is empty!")
        else:
            plate = self.stack.get()
            print(f"Popped {plate} from the stack")
            return plate

    def display_plates(self):
        print("Plates in the stack:")
        temp_queue = queue.Queue()
        while not self.stack.empty():
            plate = self.stack.get()
            print(plate)
            temp_queue.put(plate)
        while not temp_queue.empty():
            self.stack.put(temp_queue.get())

def main():
    game = StackPlateGame()
    while True:
        print("\nStack Plate Game Menu:")
        print("1. Push plate onto stack")
        print("2. Pop plate from stack")
        print("3. Display plates in stack")
        print("4. Quit")
        choice = input("Enter your choice: ")
        if choice == "1":
            color = input("Enter plate color: ")
            size = input("Enter plate size: ")
            plate = Plate(color, size)
            game.push_plate(plate)
        elif choice == "2":
            game.pop_plate()
        elif choice == "3":
            game.display_plates()
        elif choice == "4":
            break
        else:
            print("Invalid choice. Please try again.")

if _name_ == "_main_":
    main()
```

### Output:
![op2](https://github.com/user-attachments/assets/19eb894f-5f91-404c-975d-c959684cebce)

### Result:
Thus the simple Stack plate game was implemented using data structure Stack.
