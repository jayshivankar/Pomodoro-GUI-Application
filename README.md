Main Features

The app uses a counting-down timer to break work into 25 minute-long intervals, separated by short, 5-minute breaks. Each work and short break interval is called a pomodoro.

Each interval type (work or rest) shows as a header in green above the tomato graphic. The timer itself counts down in white integers within the tomato graphic.

After completing 4 pomodoros, the timer then sets to a 20 minute "long" break.

Usage & Requirements
This project uses two libraries:


TkInter,
Math

    REPS += 1

    work_sec = WORK_MIN * 60
    short_break_sec = SHORT_BREAK_MIN * 60
    long_break_sec = LONG_BREAK_MIN * 60

    if REPS % 8 == 0:
        title.config(text="Long Break", fg=RED)
        count_down(long_break_sec)
    elif REPS % 2 == 0:
        title.config(text="Short Break", fg=PINK)
        count_down(short_break_sec)
    else:
        title.config(text="Work")
        count_down(work_sec)
Once the start_timer() function knows the amount of time to count, it then calls to the count_down() function which animates the timer at the top of the interface, informing the user how much time is left in each interval:

def count_down(count):
    global REPS, TIMER

    count_min = math.floor(count / 60)
    count_sec = count % 60

    if count_sec < 10:
        count_sec = f"0{count_sec}"
    canvas.itemconfig(timer_text, text=f"{count_min}:{count_sec}")
    if count > 0:
        TIMER = window.after(1000, count_down, count - 1)
    else:
        start_timer()
        marks = ""
        work_sessions = math.floor(REPS/2)
        for _ in range(work_sessions):
            marks += "✔"
        check.config(text=marks)
Pomorodo Timer Showing a Work Interval and counting down from 25 minutes

Lastly, to reset the timer and start the intervals from the beginning, we add the reset() function:

def reset():
    global TIMER, REPS
    window.after_cancel(TIMER)
    title.config(text="Timer", fg=GREEN)
    canvas.itemconfig(timer_text, text="00:00")
    check.config(text="")
    REPS = 0
