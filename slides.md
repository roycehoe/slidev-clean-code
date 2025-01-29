---
theme: default
background: https://cover.sli.dev
title: Welcome to Slidev
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
# apply unocss classes to the current slide
class: text-center
# https://sli.dev/features/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations.html#slide-transitions
# transition: slide-left
# enable MDC Syntax: https://sli.dev/features/mdc
mdc: true
---

# Python Code Smells

<div class="abs-br m-6 text-xl">
  <button @click="$slidev.nav.openInEditor" title="Open in Editor" class="slidev-icon-btn">
    <carbon:edit />
  </button>
  <a href="https://github.com/slidevjs/slidev" target="_blank" class="slidev-icon-btn">
    <carbon:logo-github />
  </a>
</div>

<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

---
layout: cover
transition: fade-out
---

# Part 1: Clean code

---
transition: fade-out
level: 2
---

# What is clean code?

 - ✨ Readable – Code should be easy to read and understand at a glance.
 - 📏 Consistent – Follows a uniform style throughout the codebase.
 - 🎯 Focused – Each function, class, and module should have a single responsibility.
 - 🔍 Self-documenting – Uses meaningful names and clear logic, reducing the need for comments.
 - 🏗️ Well-structured – Organized logically, with small, cohesive components.
 - 📦 Minimal dependencies – Avoids unnecessary coupling between components.
 - 🚀 Efficient – Written to perform well without premature optimization.
 - 🛠️ Easily maintainable – Designed for future changes with minimal impact.
 - ❌ Avoids duplication – Follows DRY (Don’t Repeat Yourself) principles.
 - 🔄 Encapsulated – Keeps details hidden and exposes only what’s necessary.
 - 🎭 Expresses intent clearly – Shows what it does, not just how it does it.
 - 📜 Follows SOLID principles – Ensures flexibility and robustness in design.
 - 🧩 Minimizes side effects – Keeps functions pure when possible.
 - 🏎️ Simple over clever – Prioritizes clarity over smart but unreadable solutions.


---
transition: fade-out
level: 2
---

# What is clean code?

 - 🛠️ Easily maintainable – Designed for future changes with minimal impact.

---
layout: cover
transition: fade-out
---

# Part 2: Code smell

---
transition: fade-out
level: 2
---

# What is a code smell?

A surface-level indicator in the source code that suggests deeper problems in maintainability.



<div v-click>
```python
    for fq in query_filters:
        # NOTE: checking for nested filter received from FE
        if "any" in fq:
            fq = fq["any"]
            if len(fq) < 1:
                continue
            else:
                fq = fq[0]

        solr_field = fe_solr_map[fq['field']]

        if fq['type'] == 'range_field':
            if fq['field'] == 'nested_properties.units':
                print("ENTERED nested units field range:", fq['selectedRange'])
                for filter_value in fq['selectedRange']:
                    lower_selected_condition = "(value.upper_bound:[{0} TO *])".format(
                        filter_value["from"])
                    upper_selected_condition = "(value.lower_bound:[* TO {0}])".format(
                        filter_value["to"])
                    # NOTE: filter on props_.ppi.id intentionally as parent ppi ids may or may not have different units and will cause double counting if different units
                    ppi_condition = "mprops_ppi.id:({0})".format(filter_value["value"])
```
</div>


---
transition: fade-out
layout: cover
---

# Part 3: Refactoring Exercise


---
class: px-20
---

# Refactoring exercise

````md magic-move {lines: true}
```python
# Rule of thumb 1: Always type your code

def get_hof_and_gt_users(users):
    hof_users = []
    gt_users = []

    for u in users:
    # Optimization: only have to loop through all users once
        if u.get("user_id") is not None:
            uid = u.get("user_id")
            stats = get_user_statistics(user_statistics)
            ks = stats.get("kill_streak") 
            if ks is not None 
              if ks >= 100: # 100 is the criteion for hof
                  hof_users.append(uid)
              if ks >= 25: # 25 is the criteion for gt
                  gt_users.append(uid)
    return hof_users, gt_users
```
```python
# Rule of thumb 1: Always type your code

def get_hof_and_gt_users(users: list[dict]) -> tuple[list[int], list[int]]:
    hof_users: list[int] = []
    gt_users: list[int] = []

    for u in users:
    # Optimization: only have to loop through all users once
        if u.get("user_id") is not None:
            uid = u.get("user_id")
            stats = get_user_statistics(user_statistics)
            ks = stats.get("kill_streak") 
            if ks is not None 
              if ks >= 100: # 100 is the criteion for hof
                  hof_users.append(uid)
              if ks >= 25: # 25 is the criteion for gt
                  gt_users.append(uid)
    return hof_users, gt_users
```
```python
# Rule of thumb 2: Avoid abbreviations

def get_hof_and_gt_users(users: list[dict]) -> tuple[list[int], list[int]]:
    hof_users: list[int] = []
    gt_users: list[int] = []

    for u in users:
    # Optimization: only have to loop through all users once
        if u.get("user_id") is not None:
            uid = u.get("user_id")
            stats = get_user_statistics(user_statistics)
            ks = stats.get("kill_streak") 
            if ks is not None 
              if ks >= 100: # 100 is the criteion for hof
                  hof_users.append(uid)
              if ks >= 25: # 25 is the criteion for gt
                  gt_users.append(uid)
    return hof_users, gt_users
```
```python
# Rule of thumb 2: Avoid abbreviations

def get_hall_of_fame_users_and_gold_tier_users(users: list[dict]) -> tuple[list[int], list[int]]:
    hall_of_fame_users: list[int] = []
    gold_tier_users: list[int] = []

    for user in users:
    # Optimization: only have to loop through all users once
        if user.get("user_id") is not None:
            user_id = user.get("user_id")
            user_statistics = get_user_statistics(user_statistics)
            kill_streak = user_statistics.get("kill_streak") 
            if kill_streak is not None 
              if kill_streak >= 100: 
                  # 100 is the criteion for hall of fame
                  hall_of_fame_users.append(user_id)
              if kill_streak >= 25:
                  # 25 is the criteion for gold tier
                  gold_tier_users.append(user_id)
    return hall_of_fame_users, gold_tier_users
```

```python
# Rule of thumb 3: If your function contains the word "and", or your function returns a tuple,  
# your function is likely violating the single responsibility principle. 
# Break your function up into many functions

def get_hall_of_fame_users_and_gold_tier_users(users: list[dict]) -> tuple[list[int], list[int]]:
    hall_of_fame_users: list[int] = []
    gold_tier_users: list[int] = []

    for user in users:
    # Optimization: only have to loop through all users once
        if user.get("user_id") is not None:
            user_id = user.get("user_id")
            user_statistics = get_user_statistics(user_statistics)
            kill_streak = user_statistics.get("kill_streak") 
            if kill_streak is not None 
                if kill_streak >= 100: 
                    # 100 is the criteion for hall of fame
                    hall_of_fame_users.append(user_id)
                if kill_streak >= 25:
                    # 25 is the criteion for gold tier
                    gold_tier_users.append(user_id)
    return hall_of_fame_users, gold_tier_users
```

```python
# Rule of thumb 3: If your function contains the word "and", or your function returns a tuple,  
# your function is likely violating the single responsibility principle. 
# Break your function up into many functions

def get_hall_of_fame_users(users: list[dict]) -> list[int]:
    hall_of_fame_users: list[int] = []

    for user in users:
        if user.get("user_id") is not None:
            user_id = user.get("user_id")
            user_statistics = get_user_statistics(user_statistics)
            kill_streak = user_statistics.get("kill_streak") 
            if kill_streak is not None 
                if kill_streak >= 100: 
                    # 100 is the criteion for hall of fame
                    hall_of_fame_users.append(user_id)
    return hall_of_fame_users

def get_gold_tier_users(users: list[dict]) -> list[int]:
  ...
```

```python
# Rule of thumb 4: Magic numbers can typically be refactored into constants

def get_hall_of_fame_users(users: list[dict]) -> list[int]:
    hall_of_fame_users: list[int] = []

    for user in users:
        if user.get("user_id") is not None:
            user_id = user.get("user_id")
            user_statistics = get_user_statistics(user_statistics)
            kill_streak = user_statistics.get("kill_streak") 
            if kill_streak is not None 
                if kill_streak >= 100: 
                    # 100 is the criteion for hall of fame
                    hall_of_fame_users.append(user_id)
    return hall_of_fame_users

def get_gold_tier_users(users: list[dict]) -> list[int]:
  ...
```

```python
# Rule of thumb 4: Magic numbers can typically be refactored into constants

HALL_OF_FAME_KILL_STREAK_CRITERION = 100


def get_hall_of_fame_users(users: list[dict], kill_streak_criterion: int = HALL_OF_FAME_KILL_STREAK_CRITERION) -> list[int]:
    hall_of_fame_users: list[int] = []

    for user in users:
        if user.get("user_id") is not None:
            user_id = user.get("user_id")
            user_statistics = get_user_statistics(user_statistics)
            kill_streak = user_statistics.get("kill_streak") 
            if kill_streak is not None:
                if kill_streak >= kill_streak_criterion: 
                    # 100 is the criteion for hall of fame
                    hall_of_fame_users.append(user_id)
    return hall_of_fame_users

def get_gold_tier_users(users: list[dict]) -> list[int]:
  ...
```

```python
# Rule of thumb 5: As far as possible, your code should be self-commenting
# Remove unneccesary comments

HALL_OF_FAME_KILL_STREAK_CRITERION = 100


def get_hall_of_fame_users(users: list[dict], kill_streak_criterion: int = HALL_OF_FAME_KILL_STREAK_CRITERION) -> list[int]:
    hall_of_fame_users: list[int] = []

    for user in users:
        if user.get("user_id") is not None:
            user_id = user.get("user_id")
            user_statistics = get_user_statistics(user_statistics)
            kill_streak = user_statistics.get("kill_streak") 
            if kill_streak is not None:
                if kill_streak >= kill_streak_criterion: 
                    # 100 is the criteion for hall of fame
                    hall_of_fame_users.append(user_id)
    return hall_of_fame_users

def get_gold_tier_users(users: list[dict]) -> list[int]:
  ...
```
```python
# Rule of thumb 6: As far as possible, your code should be self-commenting
# Remove unneccesary comments

HALL_OF_FAME_KILL_STREAK_CRITERION = 100


def get_hall_of_fame_users(users: list[dict], kill_streak_criterion: int = HALL_OF_FAME_KILL_STREAK_CRITERION) -> list[int]:
    hall_of_fame_users: list[int] = []

    for user in users:
        if user.get("user_id") is not None:
            user_id = user.get("user_id")
            user_statistics = get_user_statistics(user_statistics)
            kill_streak = user_statistics.get("kill_streak") 
            if kill_streak is not None:
                if kill_streak >= kill_streak_criterion: 
                    hall_of_fame_users.append(user_id)
    return hall_of_fame_users

def get_gold_tier_users(users: list[dict], kill_streak_criterion: int) -> list[int]:
  ...
```

```python
# Rule of thumb 7: Avoid nesting code by using early termination

HALL_OF_FAME_KILL_STREAK_CRITERION = 100


def get_hall_of_fame_users(users: list[dict], kill_streak_criterion: int = HALL_OF_FAME_KILL_STREAK_CRITERION) -> list[int]:
    hall_of_fame_users: list[int] = []

    for user in users:
        if user.get("user_id") is not None:
            user_id = user.get("user_id")
            user_statistics = get_user_statistics(user_statistics)
            kill_streak = user_statistics.get("kill_streak") 
            if kill_streak is not None:
                if kill_streak >= kill_streak_criterion: 
                    hall_of_fame_users.append(user_id)
    return hall_of_fame_users
```

```python
# Rule of thumb 7: Avoid nesting code by using early termination

HALL_OF_FAME_KILL_STREAK_CRITERION = 100


def get_hall_of_fame_users(users: list[dict], kill_streak_criterion: int = HALL_OF_FAME_KILL_STREAK_CRITERION) -> list[int]:
    hall_of_fame_users: list[int] = []

    for user in users:
        if user.get("user_id") is None:
            continue
        user_id = user.get("user_id")
        user_statistics = get_user_statistics(user_statistics)
        kill_streak = user_statistics.get("kill_streak") 
        if kill_streak is None:
            continue
        if kill_streak <= kill_streak_criterion:
            continue
        hall_of_fame_users.append(user_id)
    return hall_of_fame_users
```

```python
# Rule of thumb 8: Use whitespace to group logical statements together to improve readability

HALL_OF_FAME_KILL_STREAK_CRITERION = 100


def get_hall_of_fame_users(users: list[dict], kill_streak_criterion: int = HALL_OF_FAME_KILL_STREAK_CRITERION) -> list[int]:
    hall_of_fame_users: list[int] = []

    for user in users:
        if user.get("user_id") is None:
            continue
        user_id = user.get("user_id")
        user_statistics = get_user_statistics(user_statistics)
        kill_streak = user_statistics.get("kill_streak") 
        if kill_streak is None:
            continue
        if kill_streak <= kill_streak_criterion:
            continue
        hall_of_fame_users.append(user_id)
    return hall_of_fame_users
```

```python
# Rule of thumb 8: Use whitespace to group logical statements together to improve readability

HALL_OF_FAME_KILL_STREAK_CRITERION = 100


def get_hall_of_fame_users(users: list[dict], kill_streak_criterion: int = HALL_OF_FAME_KILL_STREAK_CRITERION) -> list[int]:
    hall_of_fame_users: list[int] = []

    for user in users:
        if user.get("user_id") is None:
            continue
        user_id = user.get("user_id")
        user_statistics = get_user_statistics(user_statistics)
        kill_streak = user_statistics.get("kill_streak") 

        if kill_streak is None:
            continue
        if kill_streak <= kill_streak_criterion:
            continue

        hall_of_fame_users.append(user_id)
    return hall_of_fame_users
```

```python
# Rule of thumb 9: Ensure that your function does what it says it does

HALL_OF_FAME_KILL_STREAK_CRITERION = 100


def get_hall_of_fame_users(users: list[dict], kill_streak_criterion: int = HALL_OF_FAME_KILL_STREAK_CRITERION) -> list[int]:
    hall_of_fame_users: list[int] = []

    for user in users:
        if user.get("user_id") is None:
            continue
        user_id = user.get("user_id")
        user_statistics = get_user_statistics(user_statistics)
        kill_streak = user_statistics.get("kill_streak") 

        if kill_streak is None:
            continue
        if kill_streak <= kill_streak_criterion:
            continue

        hall_of_fame_users.append(user_id)
    return hall_of_fame_users
```

```python
# Rule of thumb 9: Ensure that your function does what it says it does

HALL_OF_FAME_KILL_STREAK_CRITERION = 100


def get_hall_of_fame_user_ids(users: list[dict], kill_streak_criterion: int = HALL_OF_FAME_KILL_STREAK_CRITERION) -> list[int]:
    hall_of_fame_users: list[int] = []

    for user in users:
        if user.get("user_id") is None:
            continue
        user_id = user.get("user_id")
        user_statistics = get_user_statistics(user_statistics)
        kill_streak = user_statistics.get("kill_streak") 

        if kill_streak is None:
            continue
        if kill_streak <= kill_streak_criterion:
            continue

        hall_of_fame_user_ids.append(user_id)
    return hall_of_fame_user_ids
```

```python
# Rule of thumb 10: Simplify

HALL_OF_FAME_KILL_STREAK_CRITERION = 100


def get_hall_of_fame_user_ids(users: list[dict], kill_streak_criterion: int = HALL_OF_FAME_KILL_STREAK_CRITERION) -> list[int]:
    hall_of_fame_users: list[int] = []

    for user in users:
        if user.get("user_id") is None:
            continue
        user_id = user.get("user_id")
        user_statistics = get_user_statistics(user_statistics)
        kill_streak = user_statistics.get("kill_streak") 

        if kill_streak is None:
            continue
        if kill_streak <= kill_streak_criterion:
            continue

        hall_of_fame_user_ids.append(user_id)
    return hall_of_fame_user_ids
```

```python
# Rule of thumb 10: Simplify

HALL_OF_FAME_KILL_STREAK_CRITERION = 100


def get_hall_of_fame_user_ids(users: list[dict], kill_streak_criterion: int = HALL_OF_FAME_KILL_STREAK_CRITERION) -> list[int]:
    hall_of_fame_users: list[int] = []

    for user in users:
        user_id = user.get("user_id")
        if user_id is None:
            continue
        user_statistics = get_user_statistics(user_statistics)
        kill_streak = user_statistics.get("kill_streak") 

        if kill_streak is None:
            continue
        if kill_streak <= kill_streak_criterion:
            continue

        hall_of_fame_user_ids.append(user_id)
    return hall_of_fame_user_ids
```

```python
# Rule of thumb 11: Reuse code

HALL_OF_FAME_KILL_STREAK_CRITERION = 100


def get_hall_of_fame_user_ids(users: list[dict], kill_streak_criterion: int = HALL_OF_FAME_KILL_STREAK_CRITERION) -> list[int]:
    hall_of_fame_users: list[int] = []

    for user in users:
        user_id = user.get("user_id")
        if user_id is None:
            continue
        user_statistics = get_user_statistics(user_statistics)
        kill_streak = user_statistics.get("kill_streak") 

        if kill_streak is None:
            continue
        if kill_streak <= kill_streak_criterion:
            continue

        hall_of_fame_user_ids.append(user_id)
    return hall_of_fame_user_ids
```

```python
# Rule of thumb 11: Reuse code

HALL_OF_FAME_KILL_STREAK_CRITERION = 100


def _is_user_within_kill_streak(user: dict, kill_streak_criterion: int) -> bool:
    user_id = user.get("user_id")
    if user_id is None:
        return False

    user_statistics = get_user_statistics(user_statistics)
    kill_streak = user_statistics.get("kill_streak") 

    if kill_streak is None:
        return False
    if kill_streak <= kill_streak_criterion:
        return False

    return True

def get_hall_of_fame_user_ids(users: list[dict], kill_streak_criterion: int = HALL_OF_FAME_KILL_STREAK_CRITERION) -> list[int]:
    ...

```

```python
# Rule of thumb 11: Reuse code

HALL_OF_FAME_KILL_STREAK_CRITERION = 100

def _is_user_within_kill_streak(user: dict, kill_streak_criterion: int) -> bool:
    ...

def get_hall_of_fame_user_ids(users: list[dict], kill_streak_criterion: int = HALL_OF_FAME_KILL_STREAK_CRITERION) -> list[int]:
    hall_of_fame_users: list[int] = []

    for user in users:
        if not _is_user_within_kill_streak(users, kill_streak_criterion):
            continue
        user_id = user.get("user_id")
        hall_of_fame_user_ids.append(user_id)
    return hall_of_fame_user_ids

```


```python
# Rule of thumb 11: Reuse code

HALL_OF_FAME_KILL_STREAK_CRITERION = 100
GOLD_TIER_KILL_STREAK_CRITERION = 100

def _is_user_within_kill_streak(user: dict, kill_streak_criterion: int) -> bool:
    ...

def get_hall_of_fame_user_ids(users: dict, kill_streak_criterion: int = HALL_OF_FAME_KILL_STREAK_CRITERION) -> list[int]:
    ...

def get_gold_tier_user_ids(users: dict, kill_streak_criterion: int = GOLD_TIER_KILL_STREAK_CRITERION) -> list[int]:
    ...
```

```python
# Rule of thumb 12: Use an autoformatter

HALL_OF_FAME_KILL_STREAK_CRITERION = 100
GOLD_TIER_KILL_STREAK_CRITERION = 100

def _is_user_within_kill_streak(user: dict, kill_streak_criterion: int) -> bool:
    ...

def get_hall_of_fame_user_ids(users: dict, kill_streak_criterion: int = HALL_OF_FAME_KILL_STREAK_CRITERION) -> list[int]:
    ...

def get_gold_tier_user_ids(users: dict, kill_streak_criterion: int = GOLD_TIER_KILL_STREAK_CRITERION) -> list[int]:
    ...
```

```python
# Rule of thumb 12: Use an autoformatter

HALL_OF_FAME_KILL_STREAK_CRITERION = 100
GOLD_TIER_KILL_STREAK_CRITERION = 100


def _is_user_within_kill_streak(user: dict, kill_streak_criterion: int) -> bool: ...


def get_hall_of_fame_user_ids(
    users: dict, kill_streak_criterion: int = HALL_OF_FAME_KILL_STREAK_CRITERION
) -> list[int]: ...


def get_gold_tier_user_ids(
    users: dict, kill_streak_criterion: int = GOLD_TIER_KILL_STREAK_CRITERION
) -> list[int]: ...

```
````

---
transition: fade-out
layout: cover
---

# Part 4: Refactoring and Beyond

Dictionaries (a little rant)


---
transition: slide-up
level: 2
---

# #1: Unintended side effects

A side effect occurs when an operation impacts something beyond its primary task

````md magic-move {lines: true}
```python {*|1-4|6-9|11-14|16-20|11-16}
def get_user():
  return {
    user_id: 1
  }

def get_user_score():
  return {
    score: 30
  }

def get_user_with_user_score(user, user_score):
  user = get_user()
  user_score = get_user_score()
  ...

def main():
  user = get_user()
  user_score = get_user_score()
  user_with_user_score = get_user_with_user_score(user, user_score)
  return user_with_user_score
```

```python {11-13}
def get_user():
  return {
    user_id: 1
  }

def get_user_score():
  return {
    score: 30
  }

def get_user_with_user_score(user, user_score):
  user["score"] = user_score["score"]
  return user

def main():
  user = get_user()
  user_score = get_user_score()
  user_with_user_score = get_user_with_user_score(user, user_score)
  return user_with_user_score
```

```python {11-13}
def get_user():
  return {
    user_id: 1
  }

def get_user_score():
  return {
    score: 30
  }

def get_user_with_user_score(user, user_score):
  user["score"] = user_score if "score" in user_score else None
  return user

def main():
  user = get_user()
  user_score = get_user_score()
  user_with_user_score = get_user_with_user_score(user, user_score)
  return user_with_user_score
```

```python {11-13}
def get_user():
  return {
    user_id: 1
  }

def get_user_score():
  return {
    score: 30
  }

def get_user_with_user_score(user, user_score):
  user["score"] = user_score.get("score")
  return user

def main():
  user = get_user()
  user_score = get_user_score()
  user_with_user_score = get_user_with_user_score(user, user_score)
  return user_with_user_score
```

```python {11-13,16,18}
def get_user():
  return {
    user_id: 1
  }

def get_user_score():
  return {
    score: 30
  }

def get_user_with_user_score(user, user_score):
  user["score"] = user_score.get("score")
  return user

def main():
  user = get_user() 
  user_score = get_user_score()
  user_with_user_score = get_user_with_user_score(user, user_score)
  return user_with_user_score
```

```python {*}
import copy


def get_user_with_user_score(user, user_score):
  user = get_user()
  result = user.copy()

  user_score = get_user_score()
  result["score"] = user_score["score"]
  return result
  

def main():
  user = get_user()
  user_score = get_user_score()
  user_with_user_score = get_user_with_user_score(user, user_score)
  return user_with_user_score
```

```python {*}
def get_user_with_user_score(user, user_score):
  result = {}
  user = get_user()
  user_score = get_user_score()

  result["user_id"] = user["user_id"]
  result["score"] = user_score["score"]
  return result
  

def main():
  user = get_user()
  user_score = get_user_score()
  user_with_user_score = get_user_with_user_score(user, user_score)
  return user_with_user_score
```

```python {*}
def get_user_with_user_score(user, user_score):
  result = {}
  user = get_user()
  user_score = get_user_score()

  return {
    "user_id": user["user_id"]
    "score": user_score["score"]
  }
  

def main():
  user = get_user()
  user_score = get_user_score()
  user_with_user_score = get_user_with_user_score(user, user_score)
  return user_with_user_score
```

```python {*}
def get_user_with_user_score(user, user_score):
  user = get_user()
  user_score = get_user_score()
  return {
    **user,
    **user_score
  }

def main():
  user = get_user()
  user_score = get_user_score()
  user_with_user_score = get_user_with_user_score(user, user_score)
  return user_with_user_score
```
````
<br>
<br>

<v-click>Takeaway: Beware of impure functions</v-click>


---
transition: slide-up
level: 2
---

# #2: Defining default values manually

````md magic-move {lines: true}
```python {*}
def get_user():
  return { user_id: 1 }

def get_user_statistics():
  # No statistics if user has not started playing at least 1 game
  return {
    "games_played": 7,
    "score": 100,
    "kill_streak": 2,
    "total_enemies_killed": 58,
    "favourite_gun": "M16"
  }

def get_user_with_user_statistics(user, user_statistics):
  # If no user statistics is available, return None for all values
  ...

```

```python
def get_user_with_user_statistics(user, user_statistics):
  # If no user statistics is available, return None for all values
  user = get_user()
  statistics = get_user_statistics()
  return {
    "user_id": user["user_id"],
    "games_played": (
        statistics["games_played"] 
        if "games_played" in statistics else 0
    ),
    "score": statistics["score"] 
        if "score" in statistics else 0,
    "kill_streak": statistics["kill_streak"] 
        if "kill_streak" in statistics else 0,
    "total_enemies_killed": (
        statistics["total_enemies_killed"]
        if "total_enemies_killed" in statistics else 0
    ),
    "faviourite_gun": (
        statistics["faviourite_gun"] 
        if "faviourite_gun" in statistics else None
    ),
  }
```
```python
def get_user_with_user_statistics(user, user_statistics):
  # If no user statistics is available, return None for all values
  user = get_user()
  statistics = get_user_statistics()
  return {
      "user_id": user["user_id"],
      "games_played": statistics.get("games_played", 0),
      "score": statistics.get("score", 0),
      "kill_streak": statistics.get("kill_streak", 0),
      "total_enemies_killed": statistics.get("total_enemies_killed", 0),
      "favourite_gun": statistics.get("favourite_gun", None)
  }
```
```python {6-11}
def get_user_with_user_statistics(user, user_statistics):
  # If no user statistics is available, return None for all values
  user = get_user()
  statistics = get_user_statistics()
  return {
      "user_id": user["user_id"],
      "games_played": statistics.get("games_played", 0),
      "score": statistics.get("score", 0),
      "kill_streak": statistics.get("kill_streak", 0),
      "total_enemies_killed": statistics.get("total_enemies_killed", 0),
      "favourite_gun": statistics.get("favourite_gun", None)
  }
```
````

---
transition: slide-up
level: 2
---

# The problem with Dictionaries

Dictionaries are opaque data structures!

Other languages deal with this problem through the use of:
 - Structs (C, Golang)
 - Interfaces (Typescript)
 - Classes (Java)

---
level: 2
---

# Advantages of using Dataclasses

1. 🧩 **Schema Documentation and Type Safety (Limited)** - Leverages type annotations to ensure better code reliability and catch potential type-related errors during development.

2. 🛠️ **Automatic Method Generation** - Automatically generates essential methods like __init__, __repr__, and __eq__, saving time and reducing boilerplate code.

3. ✍️ **Simplified Syntax** - With the @dataclass decorator, defining classes becomes cleaner and more concise compared to manually writing initialization and utility methods.

4. 🔒 **Immutability** - By setting `frozen=True`, you can create immutable objects, ensuring the instance cannot be modified after creation.

---
transition: slide-up
level: 2
---

# #2: Defining default values manually

````md magic-move {lines: true}
```python
def get_user_with_user_statistics(user, user_statistics):
  # If no user statistics is available, return None for all values
  user = get_user()
  statistics = get_user_statistics()
  return {
      "user_id": user["user_id"],
      "games_played": statistics.get("games_played", 0),
      "score": statistics.get("score", 0),
      "kill_streak": statistics.get("kill_streak", 0),
      "total_enemies_killed": statistics.get("total_enemies_killed", 0),
      "favourite_gun": statistics.get("favourite_gun", None)
  }
```
```python
from dataclasses import dataclass
from typing import Optional

@dataclass
class UserWithUserStatistics:
  user_id: int
  games_played: int = 0
  score: int = 0
  kill_streak: int = 0
  total_enemies_killed: int = 0
  faviourite_gun: Optional[str] = None


def get_user_with_user_statistics(user, user_statistics):
  # If no user statistics is available, return None for all values
  user = get_user()
  statistics = get_user_statistics()
  return UserWithUserStatistics(**user, **statistics)
```

```python
from dataclasses import dataclass
from typing import Optional

@dataclass
class UserWithUserStatistics:
  user_id: int
  games_played: int = 0
  score: int = 0
  kill_streak: int = 0
  total_enemies_killed: int = 0
  faviourite_gun: Optional[str] = None


def get_user_with_user_statistics(user: dict, user_statistics: dict) -> UserWithUserStatistics:
  # If no user statistics is available, return None for all values
  user = get_user()
  statistics = get_user_statistics()
  return UserWithUserStatistics(**user, **statistics)
```
````
<br>
<v-click>Takeaway: keep up to date with the standard library as much as possible</v-click>




---

# Clicks Animations

You can add `v-click` to elements to add a click animation.

<div v-click>

This shows up when you click the slide:

```html
<div v-click>This shows up when you click the slide.</div>
```

</div>

<br>

<v-click>

The <span v-mark.red="3"><code>v-mark</code> directive</span>
also allows you to add
<span v-mark.circle.orange="4">inline marks</span>
, powered by [Rough Notation](https://roughnotation.com/):

```html
<span v-mark.underline.orange>inline markers</span>
```

</v-click>

<div mt-20 v-click>

[Learn more](https://sli.dev/guide/animations#click-animation)

</div>

---

# Motions

Motion animations are powered by [@vueuse/motion](https://motion.vueuse.org/), triggered by `v-motion` directive.

```html
<div
  v-motion
  :initial="{ x: -80 }"
  :enter="{ x: 0 }"
  :click-3="{ x: 80 }"
  :leave="{ x: 1000 }"
>
  Slidev
</div>
```

<div class="w-60 relative">
  <div class="relative w-40 h-40">
    <img
      v-motion
      :initial="{ x: 800, y: -100, scale: 1.5, rotate: -50 }"
      :enter="final"
      class="absolute inset-0"
      src="https://sli.dev/logo-square.png"
      alt=""
    />
    <img
      v-motion
      :initial="{ y: 500, x: -100, scale: 2 }"
      :enter="final"
      class="absolute inset-0"
      src="https://sli.dev/logo-circle.png"
      alt=""
    />
    <img
      v-motion
      :initial="{ x: 600, y: 400, scale: 2, rotate: 100 }"
      :enter="final"
      class="absolute inset-0"
      src="https://sli.dev/logo-triangle.png"
      alt=""
    />
  </div>

  <div
    class="text-5xl absolute top-14 left-40 text-[#2B90B6] -z-1"
    v-motion
    :initial="{ x: -80, opacity: 0}"
    :enter="{ x: 0, opacity: 1, transition: { delay: 2000, duration: 1000 } }">
    Slidev
  </div>
</div>

<!-- vue script setup scripts can be directly used in markdown, and will only affects current page -->
<script setup lang="ts">
const final = {
  x: 0,
  y: 0,
  rotate: 0,
  scale: 1,
  transition: {
    type: 'spring',
    damping: 10,
    stiffness: 20,
    mass: 2
  }
}
</script>

<div
  v-motion
  :initial="{ x:35, y: 30, opacity: 0}"
  :enter="{ y: 0, opacity: 1, transition: { delay: 3500 } }">

[Learn more](https://sli.dev/guide/animations.html#motion)

</div>

---

# LaTeX

LaTeX is supported out-of-box. Powered by [KaTeX](https://katex.org/).

<div h-3 />

Inline $\sqrt{3x-1}+(1+x)^2$

Block
$$ {1|3|all}
\begin{aligned}
\nabla \cdot \vec{E} &= \frac{\rho}{\varepsilon_0} \\
\nabla \cdot \vec{B} &= 0 \\
\nabla \times \vec{E} &= -\frac{\partial\vec{B}}{\partial t} \\
\nabla \times \vec{B} &= \mu_0\vec{J} + \mu_0\varepsilon_0\frac{\partial\vec{E}}{\partial t}
\end{aligned}
$$

[Learn more](https://sli.dev/features/latex)

---

# Diagrams

You can create diagrams / graphs from textual descriptions, directly in your Markdown.

<div class="grid grid-cols-4 gap-5 pt-4 -mb-6">

```mermaid {scale: 0.5, alt: 'A simple sequence diagram'}
sequenceDiagram
    Alice->John: Hello John, how are you?
    Note over Alice,John: A typical interaction
```

```mermaid {theme: 'neutral', scale: 0.8}
graph TD
B[Text] --> C{Decision}
C -->|One| D[Result 1]
C -->|Two| E[Result 2]
```

```mermaid
mindmap
  root((mindmap))
    Origins
      Long history
      ::icon(fa fa-book)
      Popularisation
        British popular psychology author Tony Buzan
    Research
      On effectiveness<br/>and features
      On Automatic creation
        Uses
            Creative techniques
            Strategic planning
            Argument mapping
    Tools
      Pen and paper
      Mermaid
```

```plantuml {scale: 0.7}
@startuml

package "Some Group" {
  HTTP - [First Component]
  [Another Component]
}

node "Other Groups" {
  FTP - [Second Component]
  [First Component] --> FTP
}

cloud {
  [Example 1]
}

database "MySql" {
  folder "This is my folder" {
    [Folder 3]
  }
  frame "Foo" {
    [Frame 4]
  }
}

[Another Component] --> [Example 1]
[Example 1] --> [Folder 3]
[Folder 3] --> [Frame 4]

@enduml
```

</div>

Learn more: [Mermaid Diagrams](https://sli.dev/features/mermaid) and [PlantUML Diagrams](https://sli.dev/features/plantuml)

---
foo: bar
dragPos:
  square: -56,0,0,0
---

# Draggable Elements

Double-click on the draggable elements to edit their positions.

<br>

###### Directive Usage

```md
<img v-drag="'square'" src="https://sli.dev/logo.png">
```

<br>

###### Component Usage

```md
<v-drag text-3xl>
  <div class="i-carbon:arrow-up" />
  Use the `v-drag` component to have a draggable container!
</v-drag>
```

<v-drag pos="663,206,261,_,-15">
  <div text-center text-3xl border border-main rounded>
    Double-click me!
  </div>
</v-drag>

<img v-drag="'square'" src="https://sli.dev/logo.png">

###### Draggable Arrow

```md
<v-drag-arrow two-way />
```

<v-drag-arrow pos="67,452,253,46" two-way op70 />

---
src: ./pages/imported-slides.md
hide: false
---

---

# Monaco Editor

Slidev provides built-in Monaco Editor support.

Add `{monaco}` to the code block to turn it into an editor:

```ts {monaco}
import { ref } from 'vue'
import { emptyArray } from './external'

const arr = ref(emptyArray(10))
```

Use `{monaco-run}` to create an editor that can execute the code directly in the slide:

```py {monaco-run}
print("hello world")
```

---
layout: center
class: text-center
---

# Learn More

[Documentation](https://sli.dev) · [GitHub](https://github.com/slidevjs/slidev) · [Showcases](https://sli.dev/resources/showcases)

<PoweredBySlidev mt-10 />
