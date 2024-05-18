# lilsims

A bunch of small scripts I wrote, left here for easy reference!

## POP growth

Logistic growth curve for small pop sizes and growth rates.

```py
import random
from matplotlib import pyplot as plt

carrying_capacity = 200
starting_pop = 100
years_to_sim = 100000

growth_rate = 0.001

males_per_100_females = 104

teen_age = 16 # Above this, they can reproduce
elder_age = 70
max_age = 80
instant_death_age = 120


pop = []

history_pop_size = []


class Pop:
	def __init__(self):
		self.age = 0
		self.starting_age = 0
		self.female = True
		if random.random() < males_per_100_females / (males_per_100_females + 100.0):
			self.famale = False

def age():
	to_kill = []
	to_birth = 0

	breeding_chance = growth_rate * len(pop) * (1.0 - len(pop) / float(carrying_capacity))

	for p in pop:
		p.age += 1
		if p.age > elder_age:
			if random.random() < 1.0 / (max_age - elder_age):
				to_kill.append(p)
			elif p.age > instant_death_age:
				to_kill.append(p)
		elif p.age > teen_age and p.female:
			if random.random() < breeding_chance:
				to_birth += 1

	for _ in range(to_birth):
		pop.append(Pop())

	for p in to_kill:
		pop.remove(p)

	history_pop_size.append(len(pop))


def init(size):
	for r in range(size):
		p = Pop()
		pop.append(p)
		p.age = int(random.random() * elder_age)
		p.starting_age = p.age

def print_pop():
	print('== pop ==')
	for p in pop:
		if p.female:
			print('f: ', p.age, ' -- from ', p.starting_age)
		else:
			print('m: ', p.age, ' -- from ', p.starting_age)

init(starting_pop)

for x in range(years_to_sim):
	age()

print_pop()

plt.hist([x.age for x in pop])
plt.show()
```
