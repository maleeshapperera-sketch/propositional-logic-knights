# propositional-logic-knights
Knights - CS50 Introduction to AI with Python
A logic puzzle solver that uses propositional logic and model checking to determine the solutions to "Knights and Knaves" puzzles.

⚔️ The Problem
In a land where knights always tell the truth and knaves always lie, determine which characters are knights and which are knaves based on their statements.

The Rules:

Knights 👑 - Always tell the truth

Knaves 🃏 - Always lie

🧩 Puzzle Logic
Example Puzzle 0
text
A says: "I am both a knight and a knave."
If A is a knight, statement must be true → impossible (can't be both)

If A is a knave, statement must be false → A is NOT both knight and knave ✓

Conclusion: A is a knave

Example Puzzle 1
text
A says: "We are both knaves."
B says: nothing
If A is knight → "We are both knaves" is true → contradicts A being knight

If A is knave → statement is false → They are NOT both knaves → B must be knight

Conclusion: A is knave, B is knight

🛠️ Implementation Approach
Propositional Logic
Define variables for each person:

python
AKnight      # A is a knight
AKnave       # A is a knave
BKnight      # B is a knight
BKnave       # B is a knave
Key Logical Relationships
Knight/Knave relationship: AKnight != AKnave (exactly one true)

Truth-telling condition: If knight, statement is true; if knave, statement is false

Statement Encoding
python
# "I am a knave"
AKnight <=> AKnave    # Not possible without contradictions

# "We are both knaves"
AKnight <=> (AKnave and BKnave)

# "At least one of us is a knight"
AKnight <=> (AKnight or BKnight)

# "If I am a knight, then B is a knave"
AKnight <=> (AKnight => BKnave)
📁 Files
logic.py - Propositional logic classes (provided)

puzzle.py - Knight and knave puzzle solutions

harry.py - Harry Potter themed logic puzzles

🚀 Usage
bash
python puzzle.py
Output:

text
Puzzle 0:
A is a Knave

Puzzle 1:
A is a Knave
B is a Knight

Puzzle 2:
A is a Knight
B is a Knave
C is a Knight
🔧 Knowledge Base Structure
Basic Setup for Each Puzzle
python
# Create empty knowledge base
kb = KnowledgeBase()

# Add that each person is either knight or knave (not both)
for person in symbols:
    kb.add(Or(person + "Knight", person + "Knave"))
    kb.add(Not(And(person + "Knight", person + "Knave")))
Truth-Telling Condition
python
# For each statement S made by person P
# (PKnight => S) AND (PKnave => Not(S))
kb.add(Implication(PKnight, statement))
kb.add(Implication(PKnave, Not(statement)))
💡 Translating Statements to Logic
English Statement	Logical Form
"I am a knight"	AKnight
"I am a knave"	AKnave
"We are both knights"	And(AKnight, BKnight)
"At least one is a knight"	Or(AKnight, BKnight)
"We are the same type"	Biconditional(AKnight, BKnight)
"We are different types"	Not(Biconditional(AKnight, BKnight))
"If I am knight, then B is knave"	Implication(AKnight, BKnave)
🧠 Logic Classes Used
From logic.py:

Symbol(name) - Propositional variable

Not(formula) - Logical negation

And(formula1, formula2) - Conjunction

Or(formula1, formula2) - Disjunction

Implication(formula1, formula2) - Conditional

Biconditional(formula1, formula2) - If and only if

model_check(knowledge, query) - Model checking algorithm

🎯 Puzzle 2 Example
Statements:

A says: "We are both knaves"

B says: "I am a knight and C is a knave"

C says: "A is a knave and B is a knight"

Implementation:

python
# A says: We are both knaves
kb.add(Biconditional(AKnight, And(AKnave, BKnave)))

# B says: I am a knight and C is a knave
kb.add(Biconditional(BKnight, And(BKnight, CKnave)))

# C says: A is a knave and B is a knight
kb.add(Biconditional(CKnight, And(AKnave, BKnight)))
🎭 Harry Potter Puzzle (harry.py)
Additional puzzle with themed characters:

Harry, Ron, Hermione, Draco

Multiple statements with interlocking logic

Tests deeper reasoning capabilities

💡 Model Checking
The model_check() function:

Generates all possible truth assignments

Filters assignments that satisfy the knowledge base

Checks if the query is true in ALL satisfying models

Returns True if query is entailed by knowledge base

python
# Check if A must be knight
if model_check(kb, AKnight):
    print("A is a Knight")
elif model_check(kb, AKnave):
    print("A is a Knave")
