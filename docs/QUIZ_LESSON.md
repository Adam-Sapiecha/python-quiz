# Python Quiz: Learn the Pieces, Build It Yourself


## Learn the pieces. Build the quiz.

A guided lesson with exercises, checkpoints and optional hints

The goal is to understand how your program works, then write it yourself. Examples use books, fruit and deliveries so you can practise the ideas without copying a finished quiz. Your existing main.py stays yours to build.

### How to work through this lesson

Allow two sessions of 60-90 minutes for the lesson, then a separate session to assemble and test your game. Before running each example, predict what it will do. Change one thing, run it again, and explain the difference.

Create a separate practice.py in your project folder. Run it from the VS Code terminal with py practice.py, or your chosen Python interpreter. Keep experiments separate from main.py.

### The learning route

Pages 2-3: read data and write reusable functions. Pages 4-5: read JSON and select random records. Page 6: track changing state. Page 7: debug and test. Page 8: assemble your own game using small missions.

### Understand the starting file you already have

```python
def main():
    pass

if __name__ == "__main__":
    main()
```

def creates a function; it does not run its body. pass is a placeholder that does nothing. The final condition calls main when the file is run directly. Importing the file gives it its module name instead, so this condition is false.

**Your turn:** Add a temporary print("Inside main") inside main. Predict the difference between running the file directly and importing it from practice.py. Remove only your temporary print afterward.

**Checkpoint:** Move on when you can explain the difference between defining a function and calling it, and why importing your game should not start a round.


## 01 / Lists, records and loops

Think about the shape of your data before writing the game

### A list holds items; a dictionary describes one item

```python
books = [
    {"title": "Dune", "pages": 412},
    {"title": "Coraline", "pages": 192},
]

first_book = books[0]
print(first_book["title"])

for book in books:
    print(book["pages"])
```

Square brackets with a number select a list position; positions start at zero. Square brackets with a string select a dictionary key. During the loop, book refers to one complete dictionary, not the whole list.

**Your turn:** Before running the example, write down its three output lines. Then add a third book and change the loop to print each title with its page count.

**Your turn:** Without a loop, retrieve the page count of the second book. Then predict what books[2] would do before a third book is added. Check the error rather than guessing.

### Connect the idea to your question bank

questions.json is a list of 50 records. Each record has question, answer, explanation, category, difficulty and id fields. The answer belongs to that record. Preserve the whole record when selecting a question.

A list position is not an ID. Position zero means the current first item; an ID such as py-001 identifies the same record even after you change the order.

**Your turn:** On paper, draw a list containing two question dictionaries. Draw one arrow to a complete dictionary and another to its question value. Do not separate questions and answers into independent shuffled lists.

**Checkpoint:** Move on when you can say what type each variable contains: books, books[0], books[0]["title"], and books[0]["pages"].


## 02 / Functions and user input

Separate getting data, transforming data and displaying data

```python
def add_tax(price):
    return price * 1.20

final_price = add_tax(10)
print(final_price)
```

price is a parameter: the name used inside the function. 10 is the argument supplied by the caller. return hands the calculated value back. print displays a value; it does not pass that displayed value back to the caller.

**Your turn:** Write your own double_number(number) function. It should return twice the input without printing anything. Use it twice with different arguments, then print both results outside the function.

### Input is text

```python
quantity = input("How many apples? ")
print(type(quantity).__name__)
print(quantity == 3)
print(quantity == "3")
```

Predict the output when you type 3. input returns a string. Your question bank also stores every answer as a string, including numeric answers, so you do not need to convert every player response to an integer.

### Decide what counts as the same answer

The string method strip removes surrounding whitespace. casefold creates text suitable for caseless matching. Neither changes the original string; each returns a new one. == compares values and produces a Boolean.

**Your turn:** In practice.py, start with the text "  Mango  ". Try strip and casefold separately, then combine what you learned. Write a function that returns consistently cleaned text. Think about why both sides of a comparison need the same treatment.

For this game, decide whether letter case should matter. A forgiving answer checker may accept different case, but Python code itself is case-sensitive: True and true are not interchangeable code.

**Checkpoint:** Move on when you can explain why a function that returns cleaned text is easier to check than one that asks for input, cleans it, compares it and prints feedback all at once.


## 03 / From JSON to Python

Reading a file is different from decoding its contents

### Practise with a tiny inventory

Create practice_inventory.json beside practice.py containing the JSON below. JSON uses double quotes for strings. It does not allow comments or a trailing comma after the last item.

```json
[
  {"name": "pencil", "stock": 8},
  {"name": "notebook", "stock": 3}
]
```

```python
import json
from pathlib import Path

folder = Path(__file__).resolve().parent
path = folder / "practice_inventory.json"
with path.open(encoding="utf-8") as stream:
    inventory = json.load(stream)

print(type(inventory).__name__)
print(len(inventory))
```

Path represents a filesystem location; / joins a folder and filename. __file__ names this script, resolve makes its path absolute, and parent selects its folder. The with block closes the file when the block ends. json.load converts the file contents into Python data.

**Your turn:** Predict the two output lines. Add a third inventory record and run again. Then print only the name of the second item using your list and dictionary knowledge.

### Transfer the skill, not the entire example

**Your turn:** In your game, work out which filename you need and what the loaded variable represents. For your first checkpoint, display only the number of records. It should be 50. Do not start randomization or scoring yet.

A file can contain valid JSON but have the wrong structure. A quiz loader should eventually check for a nonempty list of dictionaries with nonempty string fields for question and answer. Keep metadata checks as a later improvement.

**Checkpoint:** Move on when you can distinguish a path, an open file object, the JSON text in the file, and the decoded list in memory.


## 04 / Random selection

Choose complete items and understand replacement

```python
import random

fruit = ["apple", "pear", "mango", "plum"]
one_item = random.choice(fruit)
two_items = random.sample(fruit, k=2)

print(one_item)
print(two_items)
print(fruit)
```

choice returns one item. sample returns a new list of k selections without replacement; it does not reorder the source list. shuffle instead rearranges a list in place and returns None. Do not assign the return value of shuffle as though it were a shuffled list.

**Your turn:** Run the example several times. Can the two sampled fruits repeat within one result? Can apple appear again in a later run? Explain why those are different questions.

**Your turn:** Ask for five fruits from the four-item list. Read the exception. Decide how a program should check the requested round length before selecting any items.

### A subtle point about duplicates

Sampling does not select the same list position twice. If the source contains duplicate values, equal values can still appear in the result. Your supplied question bank has distinct IDs, so selecting whole records without replacement avoids repeated question IDs within one round.

### Selection and display are separate decisions

Sampling dictionaries keeps their fields together. After selection, you still decide which field to display and which field to hold back until the player answers. Never sample question text and answer text independently.

**Your turn:** Use a list of three book dictionaries and select two complete books. Print only their titles. Then describe, without code, how that exercise maps onto your question bank.

For repeatable practice, use a local generator such as random.Random(7) and call its sample method. This helps reproduce a bug with the same starting data; do not assume results are identical across every Python version.

**Checkpoint:** Move on when you can explain what the selected value contains and how to prevent a round size larger than the available pool.


## 05 / State, conditions and feedback

A variable changes over time only when your code updates it

### Trace an unrelated accumulator

```python
weights = [2, 5, 1]
total = 0

for weight in weights:
    total += weight

print(total)
```

**Your turn:** Before running it, make a table with iteration, current weight and total. What happens if total = 0 is moved inside the loop? Explain the difference using your table.

### A condition controls whether an update happens

```python
temperature = 18

if temperature < 15:
    message = "Bring a coat"
else:
    message = "A light jacket may be enough"

print(message)
```

The condition produces True or False. Indentation decides which statements belong to each branch. Statements after the branch run regardless of which path was taken.

**Your turn:** Write a small practice program that counts how many package weights are greater than 3. Work out the answer by hand for [2, 5, 1, 7] before you write the loop. No quiz records are needed for this exercise.

### Design the quiz behavior in plain English

**Your turn:** Describe one turn in five short sentences: what the player sees, what they type, how a decision is made, what can change, and what feedback appears. Decide which action happens only on success and which happens after every answer.

A round has a starting state and an ending state. Decide where the round-level counter starts and where the final result is shown. Use the selected round length as the denominator, not the full 50-question bank.

**Checkpoint:** Move on when you can predict a five-turn round with outcomes correct, incorrect, correct, incorrect, correct, and explain why an incorrect answer should not change the score.


## 06 / Debug and check behavior

Use small, repeatable examples instead of guessing

### Read the error from the bottom upward

The final line names the exception and explains the immediate problem. The traceback above it shows the path through your code. Find the relevant line in your own file, then inspect the values and their types there.

Useful distinctions: FileNotFoundError means the path could not be opened because it does not exist. JSONDecodeError means the JSON text could not be decoded. KeyError means a requested dictionary key is absent. A generic except: pass hides the evidence you need.

**Your turn:** In the practice inventory only, temporarily remove a closing bracket, run the loader and read the error. Restore the bracket. Then temporarily use the wrong filename and compare the error. Keep your original question bank intact.

### Assertions let you check a function without typing

```python
def add_one(number):
    return number + 1

assert add_one(2) == 3
assert add_one(-1) == 0
```

For simple practice, an assertion checks an expectation; a failed one raises AssertionError. Use them as learning checks, not as the only validation of user input. A dedicated test framework is a useful next step after the game works.

**Your turn:** Write checks for your text-cleaning function using leading spaces, trailing spaces, mixed case and an empty string. Decide the expected outputs yourself before calling the function.

### Plan your game checks before finishing it

Test a tiny hand-picked round with all correct, all incorrect and mixed answers. Also check a blank response, a missing file and a requested round size larger than the available pool. For random selection, inspect IDs rather than expecting a particular question every time.

**Your turn:** Use a breakpoint inside a practice loop. Step with F10 and watch the current item and accumulator change. Temporarily printing repr(value) and type(value).__name__ can also reveal whitespace or type mismatches.

**Checkpoint:** Move on when you can reproduce one failure, explain its cause and demonstrate that your change fixes that specific behavior.


## 07 / Your build missions

No finished game here: connect the concepts yourself

### Complete one mission before starting the next

1. Load the real question bank and show its length. Checkpoint: 50 records.
2. Select one complete record and display only its prompt. Checkpoint: the matching answer remains attached to it.
3. Ask for a response and decide whether it matches. Checkpoint: try one right, one wrong and one padded answer.
4. Select a five-record round and repeat the turn. Checkpoint: five distinct IDs are used.
5. Add round-level scoring and feedback. Checkpoint: hand-calculated results match the program.
6. Put related work into small functions. Checkpoint: a data-transforming function can be checked without input().

### Optional hints: try first, then read only what you need

**Cannot access a field?** Identify whether the current variable is a list, one dictionary or a string. Indexing rules differ.
**Right answer marked wrong?** Inspect repr of both values and compare their types. Apply the same cleanup rules to both sides.
**Repeated questions?** Think about replacement and distinct source records.
**Only the last result counts?** Locate where the counter is initialized.
**Result prints too often?** Check indentation and whether it belongs inside the loop.

### Only after the basic round works

Choose one extension: category filtering, explanations after each turn, replay, or a best-score file. Keep separate records for different categories or round lengths. Introduce one feature at a time so a failure has a small possible cause.

### Teach it back

**Your turn:** Without reading your code, explain how JSON becomes Python data, how one question stays with its answer, what changes each turn and why your functions return values. Then change the round length from five to three and check whether the design still works.

### Official references

- [Functions and loops](https://docs.python.org/3/tutorial/controlflow.html)
- [Lists and dictionaries](https://docs.python.org/3/tutorial/datastructures.html)
- [JSON files](https://docs.python.org/3/library/json.html)
- [Random selection](https://docs.python.org/3/library/random.html)
- [Paths and files](https://docs.python.org/3/library/pathlib.html)
- [Errors and exceptions](https://docs.python.org/3/tutorial/errors.html)