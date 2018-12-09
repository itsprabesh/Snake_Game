import tkinter as tk
import random

# Global Variables
screen_width = 800
screen_height = 600
segment_size = 20
status = True
SCORE = 0
speed = 200


def setUpWindow():
    global win
    global my_canvas

    win = tk.Tk()
    win.title("Snake Game")

    my_canvas = tk.Canvas(win, width=screen_width, height=screen_height, bg="#000000")
    my_canvas.grid()


def restart():
    global segment
    global snake
    global status
    global speed
    global SCORE
    win.destroy()
    setUpWindow()

    segment = [Segment(segment_size, segment_size), Segment(segment_size * 2, segment_size),
               Segment(segment_size * 3, segment_size)]
    snake = Snake(segment)
    my_canvas.bind("<KeyPress>", snake.changeDirection)
    my_canvas.focus_set()
    status=True

    createApple()

    main()
    SCORE=0
    speed=200


def createApple():  # Creates an apple to be made
    global apple
    x = segment_size * random.randint(1, (screen_width - segment_size) / segment_size)
    y = segment_size * random.randint(1, (screen_height - segment_size) / segment_size)
    apple = my_canvas.create_oval(x, y, x + segment_size, y + segment_size, fill="red")


class Segment:
    def __init__(self, x, y):
        self.instance = my_canvas.create_oval(x, y, x + segment_size, y + segment_size, fill="white")


class Snake:
    def __init__(self, segment):
        self.segment = segment
        # Creating possible moves
        self.mapping = {"Down": (0, 1), "Right": (1, 0), "Up": (0, -1),
                        "Left": (-1, 0)}  # Use of dictionary to define the directions
        # Begining
        self.vector = self.mapping["Right"]

    def move(self):  # Move the snake
        for i in range(len(self.segment) - 1):
            segment = self.segment[i].instance
            x1, y1, x2, y2 = my_canvas.coords(self.segment[i + 1].instance)
            my_canvas.coords(segment, x1, y1, x2, y2)

        x1, y1, x2, y2 = my_canvas.coords(self.segment[-2].instance)
        my_canvas.coords(self.segment[-1].instance, x1 + self.vector[0] * segment_size,
                         y1 + self.vector[1] * segment_size, x2 + self.vector[0] * segment_size,
                         y2 + self.vector[1] * segment_size)

    def addSegment(self):  # Add segment to the snake
        last_segment = my_canvas.coords(self.segment[0].instance)
        x = last_segment[2] - segment_size
        y = last_segment[3] - segment_size
        self.segment.insert(0, Segment(x, y))

    def changeDirection(self, event):  # change the direction of snake
        if event.keysym in self.mapping:
            self.vector = self.mapping[event.keysym]


# Main--Handling the game process

def main():
    global status
    global SCORE
    global speed
    if status:
        snake.move()
        head = my_canvas.coords(snake.segment[-1].instance)
        x1, y1, x2, y2 = head
        # Collision check
        if x2 > screen_width or x1 < 0 or y1 < 0 or y2 > screen_height:
            status = False
        elif SCORE == 20:
            status = False

        # Eating Apples
        elif head == my_canvas.coords(apple):
            snake.addSegment()
            SCORE = SCORE + 1
            speed = speed - int(50 / SCORE)
            my_canvas.delete(apple)
            createApple()

        # Eating Itself
        else:
            for j in range(len(snake.segment) - 1):
                if head == my_canvas.coords(snake.segment[j].instance):
                    status = False
        win.after(speed, main)


    else:
        if SCORE == 20:
            my_canvas.create_text(screen_width / 2, screen_height / 2, text="You Win! \n Score: " + str(SCORE),
                                  font="Impact 30", fill="green")
        else:
            my_canvas.create_text(screen_width / 2, screen_height / 2, text="Game Over! \n Score: " + str(SCORE),
                                  font="Impact 30", fill="red")
            restart_button = tk.Button(win, text="Restart")
            restart_button['command'] = restart
            restart_button.grid(row=20, columnspan=20)

# Setting up window


setUpWindow()


# creating segment and the snake
segment = [Segment(segment_size, segment_size), Segment(segment_size * 2, segment_size),
           Segment(segment_size * 3, segment_size)]
snake = Snake(segment)
# key bindings
my_canvas.bind("<KeyPress>", snake.changeDirection)
# catching keypress

my_canvas.focus_set()

createApple()
main()
win.mainloop()
