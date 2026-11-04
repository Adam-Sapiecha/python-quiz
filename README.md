# Python Quiz | Learn by Building

A small, guided Python project for learning how to turn a simple idea into a tested command-line application.

**Level:** beginner · **Suggested time:** 3–5 focused hours · **Tools:** Python 3.11+, VS Code, Git

> **Project status:** learning plan and repository setup. The quiz implementation is the next step. Features below are goals, not completed functionality.

## What you will build

A terminal quiz that asks five questions, accepts answers, displays a score, and remembers your best result between sessions.

The project starts with one question and grows in small, understandable steps. The core version uses only Python's standard library.

## Learning objectives

By the end, you should be able to:
/tp DaddyJohn03 MrCiec/tp DaddyJohn03 MrCiec
- Use variables, conditions, loops, lists, and dictionaries together.
- Split a program into functions with clear inputs and return values.
- Normalize user input without mixing it with scoring logic.
- Read questions and save results using JSON files.
- Handle missing files, invalid data, and empty input.
- Write automated tests for behavior that matters.
- Record progress with small, descriptive Git commits.

## Start with a tiny working program

1. Open this project folder in VS Code.
2. Create a file named `main.py`.
3. Display one question, read an answer with `input()`, and print whether it is correct.
4. Run `py main.py` in the Windows terminal, or `python main.py` if that is your interpreter command.
5. Try the same answer with different capitalization and extra spaces. What changes?

**First checkpoint:** one question works before you add menus, files, or randomization.

## Build roadmap

| Stage | Build | You are practicing | Finished when |
| --- | --- | --- | --- |
| 1. One question | Ask, compare, show feedback | Input, strings, conditions | Correct and incorrect answers produce different feedback |
| 2. Five questions | Store questions in a list of dictionaries; count points | Data structures and loops | Five correct answers give 5/5; five incorrect answers give 0/5 |
| 3. Small functions | Separate normalization, asking, and scoring | Function design and return values | Scoring can be checked without interactive input |
| 4. JSON questions | Load questions from `questions.json` | File I/O and validation | Valid data loads; missing or invalid data gives a useful message |
| 5. Replay and record | Randomize question order; save the best score | Standard-library modules and persistence | Questions do not repeat within a round; the record survives restart |
| 6. Tests and review | Test normalization, scoring, and file handling | Debugging and automated testing | Edge cases pass and you can explain the program's data flow |

## Suggested function boundaries

These are design prompts, not a required architecture:

```text
normalize(text)             -> cleaned answer
load_questions(path)       -> validated questions
ask_question(question)     -> True or False
calculate_score(results)   -> number of correct answers
load_best_score(path)      -> previous best score
save_best_score(path, score)
```

Keep keyboard input and printed output separate from functions that transform or calculate data. That makes those functions easier to test.

## Question format

Create at least five questions following this example:

```json
[
  {
    "question": "Which keyword defines a function in Python?",
    "answer": "def"
  }
]
```

Use UTF-8 when opening text files. Decide how your loader should handle an empty question list, missing fields, and values with the wrong type. For the basic version, keep the same five-question set so saved scores stay comparable.

## Completion checklist

- [ ] All five questions are asked exactly once per round.
- [ ] Leading/trailing spaces and letter case do not change a correct answer.
- [ ] Blank or incorrect answers do not earn points.
- [ ] The final score is correct for all-right, all-wrong, and mixed answers.
- [ ] First launch works when no score file exists.
- [ ] A lower score does not overwrite the best result.
- [ ] Missing or malformed question data produces an understandable error.
- [ ] Importing the module for tests does not start an interactive quiz.
- [ ] At least three automated tests cover meaningful behavior.

When you create a `tests` folder, you can run standard-library tests with:

```bash
python -m unittest discover -s tests -v
```

## Stretch challenges

Choose one after completing the basic version:

- **Categories:** let the player choose a topic and keep a separate record for each category.
- **Explanations:** show why an answer is correct after each question.
- **Multiple choice:** validate the selected option and explain how the data format changes.
- **Round history:** save dated results and calculate an average score.

## Reflection questions

1. Why should `normalize()` return a value instead of asking for input itself?
2. What is the difference between a missing score file and a corrupted one?
3. How would you test scoring without typing answers in the terminal?
4. Which parts would stay the same if you replaced the terminal with a graphical interface?

## Keep a learning log

After each stage, record what you built, one bug you fixed, and why the fix worked. Commit a working checkpoint before adding the next feature.

Example commit messages: `Add single-question quiz`, `Extract answer normalization`, `Load questions from JSON`.

## Official references

- [Python tutorial: control flow and functions](https://docs.python.org/3/tutorial/controlflow.html)
- [Python tutorial: data structures](https://docs.python.org/3/tutorial/datastructures.html)
- [Reading and writing JSON](https://docs.python.org/3/library/json.html)
- [Random sampling without replacement](https://docs.python.org/3/library/random.html#random.sample)
- [Testing with unittest](https://docs.python.org/3/library/unittest.html)

Build a little, test it, explain it, then move to the next stage.
