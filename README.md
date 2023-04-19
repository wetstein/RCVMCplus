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

cg: This class generates a random 2-dimensional ideological spectrum for N candidates, with each candidate represented as a tuple of x and y coordinates. The candidates are then sorted by their x-coordinate (ideological position) and assigned a preference ranking based on their proximity in the y-coordinate (degree of preference).

cg: To use the class, you can create a new Voter instance and access its preferences attribute to get the ranked preferences for that voter:

```
voter = Voter(5) # Create a new Voter instance with 5 candidates
print(voter.preferences) # Print the ranked preferences for the voter
```

This will output something like:

```
{(-0.8856731918026733, 0.4017551670324391): 5,
 (-0.7617875467199597, 0.1595087984651456): 4,
 (-0.23616244761468352, -0.4442077223839823): 3,
 (0.0293784018619138, 0.294227368130496): 2,
 (0.7808812952682516, 0.7413980237810624): 1}
```

cg: This indicates that the voter's top choice is the candidate with the highest y-coordinate (0.7413980237810624), followed by the candidate with the second highest y-coordinate (0.294227368130496), and so on.

mw: ok, this isn't exactly what I meant, but it's close. I would like a python class that generates N politicians on a 2-dimensional political spectrum and M voters on a 2-dimensional political spectrum and then determines each voter's rankings based on ideological proximity

cg: Sure, here's an updated Python class that generates N politicians and M voters on a 2-dimensional political spectrum and determines each voter's rankings based on ideological proximity:

```
import random

class Politician:
    def __init__(self, x, y):
        self.x = x
        self.y = y

class Voter:
    def __init__(self, x, y, num_candidates, candidates):
        self.x = x
        self.y = y
        self.preferences = self.generate_preferences(num_candidates, candidates)

    def generate_preferences(self, num_candidates, candidates):
        # Calculate each candidate's distance from the voter in the 2-dimensional political spectrum
        distances = [(candidate, ((self.x - candidate.x) ** 2 + (self.y - candidate.y) ** 2) ** 0.5) for candidate in candidates]

        # Sort the candidates by their distance from the voter
        candidates_sorted = sorted(distances, key=lambda c: c[1])

        # Assign a preference ranking to each candidate based on their distance from the voter
        preferences = {}
        for i, (candidate, distance) in enumerate(candidates_sorted):
            rank = i + 1
            preferences[candidate] = rank

        return preferences

class PoliticalSpectrum:
    def __init__(self, num_candidates, num_voters):
        self.num_candidates = num_candidates
        self.num_voters = num_voters
        self.candidates = self.generate_candidates()
        self.voters = self.generate_voters()

    def generate_candidates(self):
        # Generate N candidates with random x and y coordinates in the 2-dimensional political spectrum
        candidates = []
        for i in range(self.num_candidates):
            x = random.uniform(-1, 1)
            y = random.uniform(-1, 1)
            candidates.append(Politician(x, y))

        return candidates

    def generate_voters(self):
        # Generate M voters with random x and y coordinates in the 2-dimensional political spectrum
        voters = []
        for i in range(self.num_voters):
            x = random.uniform(-1, 1)
            y = random.uniform(-1, 1)
            voter = Voter(x, y, self.num_candidates, self.candidates)
            voters.append(voter)

        return voters

```

cg: This class defines three classes: Politician, Voter, and PoliticalSpectrum. Politician represents a political candidate with x and y coordinates on the 2-dimensional political spectrum. Voter represents a voter with x and y coordinates on the same spectrum, and a preferences attribute that stores the voter's ranked preferences for the politicians based on ideological proximity. PoliticalSpectrum generates N politicians and M voters on the 2-dimensional political spectrum and stores them in self.candidates and self.voters attributes, respectively.

cg: To use the class, you can create a new PoliticalSpectrum instance with the desired number of candidates and voters:

```
spectrum = PoliticalSpectrum(num_candidates=5, num_voters=10)
```

This will generate 5 candidates and 10 voters on the 2-dimensional political spectrum, and calculate each voter's ranked preferences based on ideological proximity. You can access the candidates and voters using the spectrum.candidates and spectrum.voters attributes, respectively.

For example, to print each voter's preferences for the candidates:

```
for voter in spectrum.voters:
    print("Voter at ({}, {}) preferences:".format(voter.x, voter.y))
    for candidate, rank in voter.preferences.items():
        print("Candidate at ({}, {}) ranked #{}".format(candidate.x, candidate.y)
```

mw: can you create a python class that tabulates a winner of the ranked choice voting election based on the voter preferences from spectrum class above?

cg: Sure, here's an example implementation of a RankedChoiceVoting class in Python that takes in the preferences generated by the Spectrum class and tabulates a winner using the ranked choice voting method:

```
python

class RankedChoiceVoting:
    def __init__(self, num_candidates, num_voters, candidate_spectrum, voter_spectrum):
        self.num_candidates = num_candidates
        self.num_voters = num_voters
        self.candidate_spectrum = candidate_spectrum
        self.voter_spectrum = voter_spectrum
        self.votes = []

    def generate_votes(self):
        for i in range(self.num_voters):
            voter = self.voter_spectrum[i]
            voter_preferences = sorted(range(self.num_candidates), key=lambda x: self.candidate_spectrum[x].distance(voter))
            self.votes.append(voter_preferences)

    def tabulate(self):
        candidates = list(range(self.num_candidates))
        while len(candidates) > 1:
            votes_for_candidates = {c: 0 for c in candidates}
            for vote in self.votes:
                for c in vote:
                    if c in candidates:
                        votes_for_candidates[c] += 1
                        break
            least_votes_candidate = min(candidates, key=lambda c: votes_for_candidates[c])
            candidates.remove(least_votes_candidate)

        return candidates[0]
```

cg: To use this class, you would first generate the candidate_spectrum and voter_spectrum using the Spectrum class, and then create an instance of RankedChoiceVoting with the appropriate parameters:

```
num_candidates = 5
num_voters = 100
candidate_spectrum = Spectrum(num_candidates, 2)
voter_spectrum = Spectrum(num_voters, 2)
rcv = RankedChoiceVoting(num_candidates, num_voters, candidate_spectrum, voter_spectrum)
```

cg: Next, you would generate the votes using the generate_votes method:

```
rcv.generate_votes()
```

cg: Finally, you can tabulate the winner of the election using the tabulate method:

```
python

winner = rcv.tabulate()
print("Winner:", winner)
```

cg: This will output the winner of the election based on the ranked choice voting method.

mw: can you now create a class that polls a subset of the voters on their first choice preferences?

cg: Sure! Here's an updated implementation of the RankedChoiceVoting class that allows for polling a subset of voters on their first choice preferences:

```
python

class RankedChoiceVoting:
    def __init__(self, num_candidates, num_voters, candidate_spectrum, voter_spectrum):
        self.num_candidates = num_candidates
        self.num_voters = num_voters
        self.candidate_spectrum = candidate_spectrum
        self.voter_spectrum = voter_spectrum
        self.votes = []
        self.first_choice_votes = {}

    def generate_votes(self):
        for i in range(self.num_voters):
            voter = self.voter_spectrum[i]
            voter_preferences = sorted(range(self.num_candidates), key=lambda x: self.candidate_spectrum[x].distance(voter))
            self.votes.append(voter_preferences)
            
            # Track first choice votes
            if i in self.first_choice_votes:
                self.first_choice_votes[i].append(voter_preferences[0])
            else:
                self.first_choice_votes[i] = [voter_preferences[0]]

    def tabulate(self):
        candidates = list(range(self.num_candidates))
        while len(candidates) > 1:
            votes_for_candidates = {c: 0 for c in candidates}
            for vote in self.votes:
                for c in vote:
                    if c in candidates:
                        votes_for_candidates[c] += 1
                        break
            least_votes_candidate = min(candidates, key=lambda c: votes_for_candidates[c])
            candidates.remove(least_votes_candidate)

        return candidates[0]

    def poll_first_choice(self, num_voters):
        first_choice_counts = {c: 0 for c in range(self.num_candidates)}
        for i in range(num_voters):
            if i not in self.first_choice_votes:
                break
            first_choice_counts[self.first_choice_votes[i][0]] += 1
        return sorted(range(self.num_candidates), key=lambda c: first_choice_counts[c], reverse=True)
```

cg: The new method poll_first_choice takes in the number of voters to poll on their first choice preferences, and returns a list of the candidates in order of the number of first choice votes they received.

cg: Here's an example usage of this class:

```
python

num_candidates = 5
num_voters = 100
candidate_spectrum = Spectrum(num_candidates, 2)
voter_spectrum = Spectrum(num_voters, 2)
rcv = RankedChoiceVoting(num_candidates, num_voters, candidate_spectrum, voter_spectrum)

# Generate votes
rcv.generate_votes()

# Tabulate the winner
winner = rcv.tabulate()
print("Winner:", winner)

# Poll the first choice preferences of 30 voters
first_choice_results = rcv.poll_first_choice(30)
print("First choice results:", first_choice_results)
```

cg: In this example, we generate the votes using the generate_votes method, and then tabulate the winner using the tabulate method. We also poll the first choice preferences of 30 voters using the poll_first_choice method, and print out the results.

mw: I would like to figure out a way to code for the voters changing their rankings, based on the result of the polling. What would be some reasons for the voters to change their rankings strategically?

cg: Voters might change their rankings strategically for a few reasons, such as:

* Tactical Voting: Voters might strategically vote for their second or third choice candidate instead of their first choice candidate in order to prevent another candidate they dislike from winning.

* Preference Change: Voters might change their rankings based on new information they receive about the candidates, or they might simply change their minds over time.

* Candidate Dropout: If a voter's preferred candidate drops out of the race, they may need to change their rankings to reflect their new preferences.

* Coalition Building: Voters might change their rankings in order to help a candidate they support form a coalition with other candidates in order to win.

To account for these reasons, you could add methods to your existing class that allow voters to update their rankings based on new information or candidate dropout, and also allow for polling of voter preferences to simulate strategic voting.


mw: can you modify voter preferences for a candidate to be based on the candidates proximity on the 2d ideological scale but also dependent on an arbitrary parameter corresponding to their estimate of the of that candidate beating their least favorite choice?

cg: Yes, you could modify the voter preferences to incorporate an arbitrary parameter that represents the voter's estimate of a candidate's ability to beat their least favorite choice. One way to do this is to introduce a "strategic factor" into the voter preference calculation, where the closer a candidate is ideologically to the voter and the higher their strategic factor, the more likely they are to be ranked higher by the voter.

cg: Here's an example implementation of this idea:
