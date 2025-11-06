# Four-in-One-game-
#Four in One game that includes – Maze, Tic Tac Toe, Pong, and Snake using Python (turtle module) 
import turtle
import random
import time

# --- MAIN MENU ---
def main_menu():
    print("F O U R   I N   O N E   G A M E S")
    print("1: Maze")
    print("2: Tic Tac Toe")
    print("3: Pong")
    print("4: Snake")
    choice = input("Enter game number to play: ")
    if choice == "1":
        clear_turtle()
        maze_game()
    elif choice == "2":
        clear_turtle()
        tic_tac_toe_game()
    elif choice == "3":
        clear_turtle()
        pong_game()
    elif choice == "4":
        clear_turtle()
        snake_game()
    else:
        print("Invalid choice!")
        main_menu()

def clear_turtle():
    turtle.clearscreen()
    turtle.hideturtle()
    turtle.tracer(1)
    turtle.delay(0)

# --- 1. MAZE GAME (Simple) ---
def maze_game():
    screen = turtle.Screen()
    screen.title("Maze Game (Press arrow keys to move, Q to quit)")

    wall = turtle.Turtle()
    wall.penup()
    wall.hideturtle()
    wall.speed(0)

    # Draw outer square maze (simple demo)
    wall.goto(-100, -100)
    wall.pendown()
    for _ in range(4):
        wall.forward(200)
        wall.left(90)
    wall.penup()

    goal = turtle.Turtle()
    goal.shape('circle')
    goal.color('green')
    goal.penup()
    goal.goto(80, 80)

    player = turtle.Turtle()
    player.shape('square')
    player.color('blue')
    player.penup()
    player.goto(-90, -90)

    def up():    player.sety(player.ycor() + 20)
    def down():  player.sety(player.ycor() - 20)
    def left():  player.setx(player.xcor() - 20)
    def right(): player.setx(player.xcor() + 20)
    def quit_game():
        turtle.bye()
        main_menu()

    screen.listen()
    screen.onkey(up, "Up")
    screen.onkey(down, "Down")
    screen.onkey(left, "Left")
    screen.onkey(right, "Right")
    screen.onkey(quit_game, "q")

    # Win logic (touches goal)
    while True:
        if player.distance(goal) < 15:
            player.goto(0, 0)
            player.write("You Win!", align="center", font=("Courier", 24, "bold"))
            time.sleep(2)
            break
        screen.update()
    screen.bye()
    main_menu()

# --- 2. TIC TAC TOE ---
def tic_tac_toe_game():
    screen = turtle.Screen()
    screen.title("Tic Tac Toe")
    draw = turtle.Turtle()
    draw.hideturtle()
    draw.speed(0)

    # Draw Board
    for i in range(-60, 120, 60):
        draw.penup()
        draw.goto(i, -90)
        draw.pendown()
        draw.goto(i, 90)
    for i in range(-60, 120, 60):
        draw.penup()
        draw.goto(-90, i)
        draw.pendown()
        draw.goto(90, i)
    board = [['' for _ in range(3)] for _ in range(3)]
    moves = 0
    player_x = True

    marker = turtle.Turtle()
    marker.hideturtle()
    marker.speed(0)

    def draw_x(x, y):
        marker.penup()
        marker.goto(x-25, y-25)
        marker.pendown()
        marker.setheading(45)
        for _ in range(2):
            marker.forward(35)
            marker.backward(35)
            marker.left(90)

    def draw_o(x, y):
        marker.penup()
        marker.goto(x, y-20)
        marker.pendown()
        marker.circle(20)
        
    def cell_coords(row, col):
        return (col*60-60, row*60-60)
    
    def click(x, y):
        nonlocal player_x, moves
        r, c = int((y + 90)//60), int((x + 90)//60)
        if 0 <= r < 3 and 0 <= c < 3 and board[r][c] == '':
            xpos, ypos = cell_coords(r, c)
            if player_x:
                draw_x(xpos, ypos)
                board[r][c] = 'X'
            else:
                draw_o(xpos, ypos)
                board[r][c] = 'O'
            moves += 1
            player_x = not player_x
            winner = check_winner()
            if winner or moves == 9:
                marker.penup()
                marker.goto(0,0)
                marker.pendown()
                if winner:
                    marker.write(f"{winner} Wins!", align="center", font=("Arial", 20))
                else:
                    marker.write("It's a Tie!", align="center", font=("Arial", 20))
                time.sleep(2)
                screen.bye()
                main_menu()
    
    def check_winner():
        for i in range(3):
            if board[i][0]==board[i][1]==board[i][2]!= '':
                return board[i][0]
            if board[0][i]==board[1][i]==board[2][i]!= '':
                return board[0][i]
        if board[0][0]==board[1][1]==board[2][2]!= '':
            return board[0][0]
        if board[0][2]==board[1][1]==board[2][0]!= '':
            return board[0][2]
        return None

    screen.onclick(click)
    screen.mainloop()

# --- 3. PONG ---
def pong_game():
    wn = turtle.Screen()
    wn.title("Pong")
    wn.bgcolor("black")
    wn.setup(width=600, height=400)
    wn.tracer(0)

    # Paddle A
    paddle_a = turtle.Turtle()
    paddle_a.speed(0)
    paddle_a.shape("square")
    paddle_a.color("white")
    paddle_a.shapesize(stretch_wid=5, stretch_len=1)
    paddle_a.penup()
    paddle_a.goto(-250, 0)

    # Paddle B
    paddle_b = turtle.Turtle()
    paddle_b.speed(0)
    paddle_b.shape("square")
    paddle_b.color("white")
    paddle_b.shapesize(stretch_wid=5, stretch_len=1)
    paddle_b.penup()
    paddle_b.goto(250, 0)

    # Ball
    ball = turtle.Turtle()
    ball.speed(40)
    ball.shape("circle")
    ball.color("red")
    ball.penup()
    ball.goto(0, 0)
    ball.dx = 2
    ball.dy = 2

    # Score Display
    pen = turtle.Turtle()
    pen.speed(0)
    pen.color("white")
    pen.penup()
    pen.hideturtle()
    pen.goto(0, 170)
    score_a = 0
    score_b = 0

    # Functions
    def paddle_a_up():   paddle_a.sety(paddle_a.ycor() + 30)
    def paddle_a_down(): paddle_a.sety(paddle_a.ycor() - 30)
    def paddle_b_up():   paddle_b.sety(paddle_b.ycor() + 30)
    def paddle_b_down(): paddle_b.sety(paddle_b.ycor() - 30)
    def quit_game():
        wn.bye()
        main_menu()

    wn.listen()
    wn.onkeypress(paddle_a_up, "w")
    wn.onkeypress(paddle_a_down, "s")
    wn.onkeypress(paddle_b_up, "Up")
    wn.onkeypress(paddle_b_down, "Down")
    wn.onkeypress(quit_game, "q")

    while True:
        wn.update()
        ball.setx(ball.xcor() + ball.dx)
        ball.sety(ball.ycor() + ball.dy)

        # Border checking
        if ball.ycor() > 190 or ball.ycor() < -190:
            ball.dy *= -1
        if ball.xcor() > 290:
            score_a += 1
            ball.goto(0,0)
            ball.dx *= -1
        if ball.xcor() < -290:
            score_b += 1
            ball.goto(0,0)
            ball.dx *= -1

        # Paddle collisions
        if (240 < ball.xcor() < 250) and (paddle_b.ycor()-50 < ball.ycor() < paddle_b.ycor()+50):
            ball.dx *= -1
        if (-250 < ball.xcor() < -240) and (paddle_a.ycor()-50 < ball.ycor() < paddle_a.ycor()+50):
            ball.dx *= -1

        pen.clear()
        pen.write(f"Player A: {score_a}    Player B: {score_b}", align="center", font=("Courier", 16, "normal"))
        time.sleep(0.01)
        # Stop at 5 points
        if score_a == 5 or score_b == 5:
            pen.goto(0,0)
            pen.write("Game Over!", align="center", font=("Arial", 20, "bold"))
            time.sleep(2)
            wn.bye()
            main_menu()
            break

# --- 4. SNAKE ---
def snake_game():
    screen = turtle.Screen()
    screen.title("Snake Game")
    screen.bgcolor("black")
    screen.setup(width=400, height=400)
    screen.tracer(0)

    head = turtle.Turtle()
    head.shape("square")
    head.color("green")
    head.penup()
    head.goto(0, 0)
    head.direction = "Stop"
    segments = []
    score = 0

    food = turtle.Turtle()
    food.shape("circle")
    food.color("red")
    food.penup()
    food.goto(40, 0)

    pen = turtle.Turtle()
    pen.hideturtle()
    pen.color("white")
    pen.penup()
    pen.goto(0,180)

    def go_up():
        if head.direction != "down":
            head.direction = "up"
    def go_down():
        if head.direction != "up":
            head.direction = "down"
    def go_left():
        if head.direction != "right":
            head.direction = "left"
    def go_right():
        if head.direction != "left":
            head.direction = "right"
    def quit_game():
        screen.bye()
        main_menu()
    screen.listen()
    screen.onkeypress(go_up, "Up")
    screen.onkeypress(go_down, "Down")
    screen.onkeypress(go_left, "Left")
    screen.onkeypress(go_right, "Right")
    screen.onkeypress(quit_game, "q")

    def move():
        if head.direction == "up":
            head.sety(head.ycor() + 20)
        if head.direction == "down":
            head.sety(head.ycor() - 20)
        if head.direction == "left":
            head.setx(head.xcor() - 20)
        if head.direction == "right":
            head.setx(head.xcor() + 20)

    while True:
        screen.update()
        if head.direction != "Stop":
            move()

        # Check for border collision
        if abs(head.xcor()) > 190 or abs(head.ycor()) > 190:
            pen.goto(0,0)
            pen.write("Game Over!", align="center", font=("Courier", 24, "bold"))
            time.sleep(2)
            screen.bye()
            main_menu()
            break

        # Check for self collision
        for segment in segments:
            if segment.distance(head) < 20:
                pen.goto(0,0)
                pen.write("Game Over!", align="center", font=("Courier", 24, "bold"))
                time.sleep(2)
                screen.bye()
                main_menu()
                return

        # Check for food collision
        if head.distance(food) < 20:
            x = random.randint(-9, 9)*20
            y = random.randint(-9, 9)*20
            food.goto(x, y)
            new_segment = turtle.Turtle()
            new_segment.shape("square")
            new_segment.color("lightgreen")
            new_segment.penup()
            segments.append(new_segment)
            score += 10

        # Move each segment to the previous position
        for idx in range(len(segments)-1, 0, -1):
            x = segments[idx-1].xcor()
            y = segments[idx-1].ycor()
            segments[idx].goto(x, y)
        if len(segments) > 0:
            segments[0].goto(head.xcor(), head.ycor())

        pen.clear()
        pen.write(f"Score: {score}", align="center", font=("Arial", 16))

        time.sleep(0.1)

if __name__ == "__main__":
    main_menu()
