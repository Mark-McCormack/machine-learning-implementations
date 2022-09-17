---
title: "First Genetic Algorithm"
date: 2022-09-16T18:09:37+01:00
draft: false
---

a

```python
import random
import numpy as np
import os
```

The first step in this algorithm is generating the initial population. Here, I have chosen to allow the user to specify the number of characters in an individuals 'DNA' and from that construct the binary string to represent them. This will be added to the population and repeat a given number of times, where it'll return all individuals at the end.

```python
def generation(size, length):
    population = []

    for i in range(size):
        member = ""
        for j in range(length):
            member += str(random.randint(0, 1))

        population.append(member)

    return population
```

Next, we calculate the fitness of the population. This in our case means we count the amount of 1's that appear in an individuals DNA and those that have more are considered fitter. This is done by taking one individual at a time and summing the contents of it's DNA, returning this total at the end.

```python
def fitness(member):
    total = 0

    for i in member:
        total += int(i)

    return total
```

Now that we have all of the individuals fitness score, we will sort the population by this factor. The reason for this is so if we want to change the number of individuals allowed to go to the next generation, we only need increase the end index we select with from the population.

```python
def sort(population):
    for i in range(len(population)):
        for j in range(len(population) - 1):
            if fitness(population[j]) > fitness(population[j+1]):
                population[j], population[j+1] = population[j+1], population[j]

    return population
```

Here we select the individuals in the population we want to move to the next generation. As we mentioned before, we do this by using indices to choose how many of the population we want from the array. To do this, we pass the population and the size of which we want our output to be.

```python
def selection(population, size):
    population = sort(population)
    return population[-size:]
```

Now that we have the next population's parents, we should mutate the individuals so that they can come up with novel solutions we might not have considered before. Here, the chance of mutation is controlled by a random number and parameter to compare against, allowing for experimenting with the probabilities used.

```python
def mutation(population):
    for i in range(len(population)):
        for j in range(len(population[i])):
            chance = random.randint(1,10)
            current = []

            if(chance > 9 and list(population[i])[j] == '0'):

                current = list(population[i])
                current[j] = '1'
                population[i] = ''.join(current)

            elif(chance > 9 and list(population[i])[j] == '1'):

                current = list(population[i])
                current[j] = '0'
                population[i] = ''.join(current)

    return population
```

Now that we have setup the population's parents, it's time we use crossover to generate our next population. To do this, we choose a random pivot point for a set of parents, mixing the values before and after this pivot point between parents and from this we can create many new individuals.

```python
def crossover(population, originalSize):
    newPopulation = []

    for i in range(int(len(population)/2)):
        for j in range(int(originalSize/len(population))):
            pivotPoint = random.randint(1,len(population[i]))

            parentOne = list(population[i])
            parentTwo = list(population[i+1])

            parentOne[pivotPoint:], parentTwo[pivotPoint:] = parentTwo[pivotPoint:], parentOne[pivotPoint:]

            population[i] = ''.join(parentOne)
            population[i+1] = ''.join(parentTwo)


    return population
```

```python
# HYPERPARAMETERS
start = 100                 # How many individuals to create at the beginning of the progam
size = 30                   # How many points in the DNA each individual has
selectionSize = 50          # Defines how many individuals to choose from previous generations
printGenerations = False    # Parameter to enable logging to visualise the data
answer = False              # Determines when an answer is found to stop searching solutions
counter = 0                 # Tracks the cycle of calculation the program is on
best = '1'                  # Used to find the fittest member from all iterations to be saved

currentPopulation = generation(start, size)
```

```python
while(answer != True):
    selectedPopulation = selection(currentPopulation, selectionSize)
    mutatedPopulation = mutation(selectedPopulation)
    crossoverPopulation = crossover(mutatedPopulation, start)

    os.system('clear')

    if(printGenerations):
        print('Selected: ' + str(selectedPopulation) + '\n\nMutated: ' + str(mutatedPopulation) + '\n\nCrossover: ' + str(crossoverPopulation) + '\n\n')

    for item in crossoverPopulation:
        if(fitness(item) > fitness(best)):
            best = item

        if(fitness(item) > 27):
            print(item)
            answer = True

    counter += 1
    print('No Results: Cycle ' + str(counter))
    print('Current Best: ' + best + ' of fitness ' + str(fitness(best)))

    if(counter > 2000):
        break

    currentPopulation = crossoverPopulation
```

    No Results: Cycle 1
    Current Best: 110011111111011111101110110001 of fitness 22
    No Results: Cycle 2
    Current Best: 110011111111011111101110110001 of fitness 22
    No Results: Cycle 3
    Current Best: 011111101011111011011101111011 of fitness 23
    No Results: Cycle 4
    Current Best: 011111101011111011011101111011 of fitness 23
    No Results: Cycle 5
    Current Best: 011111101011111011011101111011 of fitness 23
    No Results: Cycle 6
    Current Best: 011111101011111011011101111011 of fitness 23
    No Results: Cycle 7
    Current Best: 011111101011111011011101111011 of fitness 23
    No Results: Cycle 8
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 9
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 10
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 11
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 12
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 13
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 14
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 15
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 16
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 17
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 18
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 19
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 20
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 21
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 22
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 23
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 24
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 25
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 26
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 27
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 28
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 29
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 30
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 31
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 32
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 33
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 34
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 35
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 36
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 37
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 38
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 39
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 40
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 41
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 42
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 43
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 44
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 45
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 46
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 47
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 48
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 49
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 50
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 51
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 52
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 53
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 54
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 55
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 56
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 57
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 58
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 59
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 60
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 61
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 62
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 63
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 64
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 65
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 66
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 67
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 68
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 69
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 70
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 71
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 72
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 73
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 74
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 75
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 76
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 77
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 78
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 79
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 80
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 81
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 82
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 83
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 84
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 85
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 86
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 87
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 88
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 89
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 90
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 91
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 92
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 93
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 94
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 95
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 96
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 97
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 98
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 99
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 100
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 101
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 102
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 103
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 104
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 105
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 106
    Current Best: 111010111011111011010111111111 of fitness 24
    No Results: Cycle 107
    Current Best: 111111111110111110101111111111 of fitness 27
    111111111110111110111111111111
    No Results: Cycle 108
    Current Best: 111111111110111110111111111111 of fitness 28

<style>
    .max-w-prose{
        max-width: 100%;
    }
</style>
