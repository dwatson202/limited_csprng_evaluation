# limited csprng evaluation
# The impact of proir knowedge on reducing search spaces through constraining valid output
Open work and thoughts on CSPRNGs with limited range output and the conquences of randominess

First I would like to thank the CRC Publishers and their great Authors for great books on Crypto:
Algorithmic Cyrpotananlysis by Antonine Joux
Handbook of Applied Cyprtophgraphy by Alfred J Menezes, Paul c van Ooorschot Scott A Vanstone.

Warning:
This document will have spelling errors.
No warranty is given and use at your own risk.

How this project started:
While evaluating the odds of 3 ball 0-9 game for random walk and finding the games were long term convergent or deversge to or away from 0. After finding it converges to 0 because of the games win rate was less than 1/2 (~48%) with the optium betting stratigy. Made me think about how Random Number Genrators work overall. After completing cryptograph class less than a year ago most of these ideas are still freash and releivent. The problem also has a bit statistics/problimistic problem solving.

This is a open thought thought experiment. In this paper I am opennly collecting my ideas and evaluating it publicly.

What is this project:
I hope to highlight what happens to CSPRNGs when limited by output. For example, in a 10 ball game (0-9), 6 possible bit combos are left unused. The full set of space is 2^4, but it a 10 ball game, only numbers 0x0000 to 0x1001 are wanted. An investagion of what impact this has on randominize. Various ways to impliement it (code agnoisticly) and the impact on the resulting numbers. The work of this paper is to evaluate the constraits and how they may bias a CSPRNG.

<th>Assumtions:</th>
<ul>
  <li>CSPRNG is perfect (or statiscily perfect;not broken)</li>
  <li>Bitstring CSPRNG (Stream)</li>
  <li>Source or Seed of is unknownable</li>
  <li>Unsigned Binary Integers (to start with)</li>
</ul>

Evaluating a 0-9 valid ouput:
To store in any binary computer (almost all computers) 9 Decial in binary requires a minium of 4 bits Unsigned Binary. Since a CSPRNG will generate all possible combos of 4 bit length strings equally likely (its perfect). So, 6/16 possible strings of length 4 are invalid output. If a program where to call the CSPRNG for a length of 4 bits, only 10/16 would be valid returns. How does a program handle this depends on the hardware, software, and the program/programmer. It's these biases that could impact a good or even prefect CSPRNG.

List of Methods for correcting and validating stratagies of CSPRNG (no-exhaustive):
Bit by bit:
Generate each bit indpendently, if the key bits (explained later) don't match, regenerate.

Muliple Genreate:
Genreate more sets of length N (max required length) and test each one for correctness, return first found.

Long and trunkate:
Generate a long string several orders larger than N. Select a subset length N evaluate.

Shift/Add/Subtract:
Apply some bit opreation to change the output to a valid/correct output.

Key Bits:
Key Bits are bits that have the most impact on the possible evaluation for a constraint.
In the example of 0-9 in unsigned binary int, the key bit(s) is/are the number place with the fewest number of set bits than the other sets. The lesser key bits have the next fewest number of set bits, and the insgienvificant bits have the same number set as unset.

Each of these methods could be impacted by the Endenace of the system they to which they run apon. All stream systems take in one unit and return on unit. They opreate in the method of the processor. In the example of 0-9 ball game, if a CSPRNG draws the first bit of a 4 bit combo to be a 1, then it must be a 8 or 9 (big endian unsigned integer) to be valid return with the constraites placed on it. When compared with the other numbers in the set, only 2 start with the key bit set in the far left. This name "key bit" is because the value impacts the results of the odds for the whole output. An interesting side note is that some numbers are special because they are symmetrical like 9 is 1001, so it would be the same in each other way regardless of the endian type of the system arctuatire. (This might be exploitable?)

When evaluating the constraitment from numbers using a bit by bit method. Since the CSPRNG is perfect, each next bit is unguessable. Assume the system only has enough memory to store 4 bits for the number. The most significant bit is generated and stored first (Big Endian). It is checked, it accepted because the 0x1000 is less than 0x1010. Regardless if the first digit is 0 or 1, the odds of regenerating the next bit is 6/10 in the 0-9 example (4/10 not regenerate). [TO-DO:Bayse's therem math here]
