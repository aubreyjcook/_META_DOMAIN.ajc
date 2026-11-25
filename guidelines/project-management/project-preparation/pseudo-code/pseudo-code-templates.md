# Pseudo-Code Templates

## Overview

Pseudo-code is a language-agnostic way to sketch a program's logic before writing real code. It combines plain-English imperative statements with conventional control-flow keywords (`if`, `while`, `for`, `function`) so the logic can be read and reviewed without committing to any one language's syntax.

It belongs in the "Pre-Dev" phase of a project — written before implementation and kept alongside the [initial README](../../../../templates/project-init-readme.md) and the [problem-solution statement](../../../../templates/problem-solution-statement.md).

## Conventions

Pseudo-code has no formal standard; the point is consistency within a project. These templates follow these conventions:

- One statement per line.
- Indent (2 or 4 spaces) to show nesting and scope.
- `Function name(arg1, arg2):` declares a function or procedure.
- `return` returns a value.
- `if` / `else if` / `else` for conditionals.
- `while`, `for i from a to b`, and `for each item in collection` for loops.
- `input` / `output` for I/O.
- `// comment` for notes.
- `x = value` for assignment.
- Meaningful names; avoid language-specific symbols (`{}`, `;`, `->`, `$`).

## Templates

### Program skeleton

```
Start
    // initialization
    // main logic
    // cleanup
End
```

For a top-down outline of a whole program:

```
Main:
    Initialize
    Process
    Output
```

### Function

```
Function name(param1, param2):
    // body
    return result
```

### Conditional

```
if condition:
    // do this
else if otherCondition:
    // do that
else:
    // otherwise do this
```

### Loops

```
while condition:
    // repeat while condition is true
```

```
for i from 1 to n:
    // repeat n times
```

```
for each item in collection:
    // process item
```

### Switch / case

```
switch value:
    case A:
        // handle A
    case B:
        // handle B
    default:
        // fallback
```

### Input / output

```
output "prompt text"
input userValue
```

### Error handling

```
try:
    // operation that may fail
catch error:
    // handle the failure
```

Some styles prefer `on error:` or simply note "if the operation fails, then …". Pick one and stay consistent.

### Data structures

```
list = [item1, item2, item3]
map  = {key: value}
set  = {item1, item2}
```

Operations are stated in plain words rather than method syntax:

```
add item to list
remove item from list
look up key in map
```

## Worked Examples

### Example 1: Find the maximum value in a list

```
Function findMax(list):
    if list is empty:
        return null
    max = list[0]
    for each item in list:
        if item > max:
            max = item
    return max
```

### Example 2: Recursive factorial

```
Function factorial(n):
    if n <= 1:
        return 1
    return n * factorial(n - 1)
```

### Example 3: Complete program outline

This mirrors the example used in the [problem-solution template](../../../../templates/problem-solution-statement.md):

```
Start
    Initialize application
    Load user data
    Display main interface

Function addTask(task):
    Create a new task object
    Add task to the task list
    Save task list

Function prioritizeTask(taskId, priorityLevel):
    Find task by taskId
    Set task priority to priorityLevel
    Save task list

Function setReminder(taskId, reminderTime):
    Find task by taskId
    Set reminder for task at reminderTime
    Save task list

Function displayAnalytics():
    Calculate completed tasks
    Calculate pending tasks
    Display analytics data
End
```

## When to Use Each

- **Program skeleton** — to outline a whole program or script before filling in details.
- **Function** — to define reusable logic with inputs and a return value.
- **Conditional / loops / switch** — to express branching and repetition.
- **Input / output / error handling** — to mark the edges of the program (where it touches the user, files, or network).
- **Data structures** — to describe what data is held and how it is manipulated.

## See Also

- [Project initialization overview](../project-initializations-overview.md)
- [Problem-solution statement template](../../../../templates/problem-solution-statement.md)
- [Project init README template](../../../../templates/project-init-readme.md)
