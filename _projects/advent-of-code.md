---
layout: page
title: Advent of Code
description: Problem solving with Python
img: assets/img/advent.jpg
importance: 6
category: personal
---
<style>
.image-container {
    height: 500px; /* Adjust as needed */
    border-radius: .25rem; /* To match rounded style */
    background-position: center center; /* Adjust as needed */
    background-size: cover;
    box-shadow: 0 2px 2px 0 rgba(0, 0, 0, 0.14), 0 3px 1px -2px rgba(0, 0, 0, 0.12), 0 1px 5px 0 rgba(0, 0, 0, 0.2); /* For z-depth-1 effect */
}
.float-img-left {
    float: left; /* Float image to the right */
    margin: 1em 1em 1em 1em; /* Add margin for spacing */
    max-width: 300px; /* Maximum width of the image */
    height: auto; /* Maintain aspect ratio */
}
</style>

<div class="row justify-content-sm-center">
    <!-- <div class="col-sm-5 mt-3 mt-md-0"> -->
    <!--     <div class="image-container" style="background-image: url('/assets/img/aoc-full.png');"></div> -->
    <!-- </div> -->
    <!-- <div class="col-sm-7 mt-3 mt-md-0"> -->
    <div class="col-sm-12 mt-3 mt-md-0">
        <img src="/assets/img/aoc-full.png" alt="Advent map" class="float-img-left">
        <p>
        <a href="https://adventofcode.com">Advent of code</a> is a programming challenge taking place every December. Each day presents a short Christmas-themed puzzle with two parts. The second can be unlocked by solving the first and they usually require you to save the day from gross Elf <a href="https://adventofcode.com/2023/day/1">negligence</a>. 2023 is the first year I participated, and I've enjoyed so much that I expect this to be a yearly tradition.
        </p>
        <p>
        The challenge has been a great way to work on coding and problem solving skills. I find that the problems seem complex on the first read, but I've been able to solve most of the puzzles with just <a href="/projects/practical-python/">basic Python</a>. Applying basic skills in a creative way has been really powerful, and my solutions often match users from the <a href="https://adventofcode.com/2023/leaderboard">leaderboard</a>. That said, I've learned new things along the way and have been able to apply them repeatedly, which really helps with learning these concepts. Some other users have been really helpful and I've cited them in my solutions when implementing their logic (usually for part 2 when I find out my brute force part 1 solution doesn't scale).
        </p>
        <p>
        My goal with this project is to learn and have fun. I haven't competed for the leaderboard, but I've really enjoyed automating some aspects of this to reduce friction. I have implemented some scripting which allows me to retrieve the puzzle input, test against examples, and submit a solution without leaving the text editor. Another interesting side effect of the challenge is that it has led me to set up a small linux server on AWS and ssh client on my ipad so that I could avoid travelling with my computer for the holidays. Merry Christmas.
        </p>
    </div>

</div>
<div class="caption">
    My submissions can be found in the <a href="https://github.com/NickSager/advent-of-code">Advent of Code</a> repository on my GitHub.
</div>



#### Example Solution
One <a href="https://adventofcode.com/2023/day/14">problem</a> I liked required "tilting" a platform of rocks to redistribute their weight. Round rocks are able to roll past stationary ones, and the goal was to calculate the load on one side of the platform. The first part required moving the rocks to the top boundary. After parsing the input, I transposed the matrix to avoid dealing with moving things in columns. Then I passed each row to a function which used the two pointer method to move the rocks, while allowing them to get stuck on the stationary ones. Easy so far.
```python
# Advent of code 2023
# Day 14
import io

def move(row):
    # Move O's to the left of a row
    w = 0
    for i in range(len(row)):
        if row[i] == 'O':
            row[i], row[w] = row[w], row[i]
            w += 1
            while w < len(row) and row[w] == '#':
                w += 1
        elif row[i] == '#':
            w = i+1

def p1(f):
    total = 0
    rows = f.read().strip().split('\n')

    rows_t = [list(row) for row in zip(*rows)]
    for row in rows_t:
        move(row)
        total += sum((row[i] == "O")*(len(row)-i) for i in range(len(row)))
    return total
```
The second part required moving the rocks to all four sides (I figured as much) and then repeating the cycle one billion times. This obviously wouldn't work with brute force. First, I looked at memo-izing the `move()` function but even if all the states were picked up quickly, it would still take too long. Next I guessed that with such a large number of iterations, there would be a cycle. With the states being deterministic, any states thereafter would just repeat themselves. I would then be able to 'fast forward' by cycles. A dictionary to record states found a cycle length of ~135, allowing part 2 to run in just a few milliseconds.

My solution ended up closely matching some top scorers from the leaderboard, which was really rewarding. GPT-4 helped me figure out how to rotate a two dimensional matrix, but I was able to piece together the rest with really simple concepts. Here is the solution:
```python
def p2(f):
    total = 0
    rows = f.read().strip().split('\n')

    num_cycles = 1000000000
    count = 0
    seen = {tuple(tuple(row) for row in rows): 0}
    cycle = False

    while count < num_cycles:
        for _ in range(4):
            rows = [list(row) for row in zip(*rows)]
            for row in rows:
                move(row)
            rows = [row[::-1] for row in rows] # Reversing -> clockwise 90

        count += 1

        if tuple(tuple(row) for row in rows) in seen and not cycle:
            print(f"Found repeat at {count}")
            cycle = count - seen[tuple(tuple(row) for row in rows)]
            count += (num_cycles - count) // cycle * cycle
            if count == num_cycles:
                break

        seen[tuple(tuple(row) for row in rows)] = count

        # Optional print debugging
        # print('-'*10)
        # for row in rows:
        #     print(row, end='\n')

    rows = [list(row) for row in zip(*rows)]
    for row in rows:
        total += sum((row[i] == "O")*(len(row)-i) for i in range(len(row)))
    return total

example = io.StringIO("""
O....#....
O.OO#....#
.....##...
OO.#O....O
.O.....O#.
O.#..O.#.#
..O..#O..O
.......O..
#....###..
#OO..#....
""")  #.strip()

# For debugging purposes
def main():
    print(p1(example))
    example.seek(0)
    print(p2(example))

if __name__ == '__main__':
    main()
```
