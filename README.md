# RCVMCplus
Study Ranked Choice Voting models using Monte Carlo, where voter preferences and candidate positions can change based on polling results

mw: can you write me a python class that generates and stores the ranked preferences of voters for N arbitrary candidates based on their proximity in a 2 dimensional ideological spectrum?

cg: Sure, here's an example Python class that generates and stores the ranked preferences of voters for N arbitrary candidates based on their proximity in a 2-dimensional ideological spectrum:

```
import random

class Voter:
    def __init__(self, num_candidates):
        self.num_candidates = num_candidates
        self.preferences = self.generate_preferences()

    def generate_preferences(self):
        # Generate random x and y coordinates for each candidate
        candidates = []
        for i in range(self.num_candidates):
            x = random.uniform(-1, 1)
            y = random.uniform(-1, 1)
            candidates.append((x, y))

        # Sort the candidates by their x-coordinate (ideological position)
        candidates = sorted(candidates, key=lambda c: c[0])

        # Assign a preference ranking to each candidate based on their proximity in the y-coordinate (degree of preference)
        preferences = {}
        for i, candidate in enumerate(candidates):
            rank = i + 1
            preferences[candidate] = rank

        return preferences
```
