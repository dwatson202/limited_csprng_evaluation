# The impact of creating proir knowedge by reducing search spaces through constraining valid output on CSPRNGs
Open work and thoughts on CSPRNGs with limited range output and the conquences of asssumed randominess. This work is to evaluate how improper implmenation of constraits and manipluiation on the output may impact the security of bitstream CSPRNG.

First I would like to thank the CRC Publishers and their great Authors for great books on Crypto:
Algorithmic Cyrpotananlysis by Antonine Joux
Handbook of Applied Cyprtophgraphy by Alfred J Menezes, Paul c van Ooorschot Scott A Vanstone.

<b>
Warning:
This document will have spelling errors.
No warranty is given and use at your own risk.
</b>
<h2>This is an open thought experiment</h2>
<h2>Goals:</h2>
<ul>
  <li>Investagion of what impact this has on randominize.</li>
  <li>Various ways to impliement it (code agnoisticly)
  <li>The impact on the resulting numbers distrubtions.</li>
  <li>Detecting of flaws in random numbers by the choice of a coder to solve this problem</li>
  <li>Exploiting these flaws to try to prodict or gain in increase chance of guessing</li>
  <li>List recommended methods for preventing these type of coding flaws</li>
 </ul>
<h2>How this project started:</h2>
<p>
  While evaluating the odds of 3 ball 0-9 game for random walk and finding the games were long term convergent or deversge to or away from 0. After finding it converges to 0 because of the games win rate was less than 1/2 (~48%) with the optium betting stratigy. This made me think about how Random Number Genrators work overall. After completing cryptograph class less than a year ago most of these ideas are still freash and releivent. The problem also has a bit statistics/problimistic problem solving.
  </p>

<h2>What is this project example based apon:</h2>
I hope to highlight what happens to CSPRNGs when limited by output. For example, in a 10 ball game (0-9), 6 possible bit combos are left unused. The full set of space is 2^4, but it a 10 ball game, only numbers 0x0000 to 0x1001 are wanted. <th>Assumtions:</th>
<ul>
  <li>CSPRNG is perfect (or statiscily perfect;not broken)</li>
  <li>Bitstring CSPRNG (Stream)</li>
  <li>Source or Seed of is unknownable</li>
  <li>Unsigned Binary Integers (to start with)</li>
</ul>

<h2>Evaluating a 0-9 for valid ouput:</h2>
To store in any binary computer (almost all computers) 9 Decial in binary requires a minium of 4 bits Unsigned Binary. Since a CSPRNG will generate all possible combos of 4 bit length strings equally likely (its perfect). So, 6/16 possible strings of length 4 are invalid output. If a program where to call the CSPRNG for a length of 4 bits, only 10/16 would be valid returns. How does a program handle this depends on the hardware, software, and the program/programmer. It's these biases that could impact a good or even prefect CSPRNG.

<h3>List of Methods for correcting and validating stratagies of CSPRNG:
<h4>By no means no-exhaustive nor correctly solve the problem</h4>
<dl>
  <dt>Bit by bit:</dt>
    <dd>Generate each bit indpendently, if the key bits (explained later) don't match, regenerate.</dd>
  <dt>Muliple Genreate:</dt>
    <dd>Genreate more sets of length N (max required length) and test each one for correctness, return first found.</dd>
  <dt>Long and trunkate:</dt>
  <dd>Generate a long string several orders larger than N. Select a subset length N evaluate.</dd>
  <dt>Shift/Add/Subtract:</dt>
  <dd>Apply some bit opreation to change the output to a valid/correct output.</dd>
  <dt>Test and Set:</dt>
  <dd>Any method that tests and sets to a valid output; includes floors and ceilings. eg: drawn bitstring = 0x1011: => 0x1001, set to 0x1001.</dd>
  <dt>Weak Crpyto:</dt>
  <dd>Use subustions ciphers to map the invalid space to valid spaces.</dd>
  <dt>Logic bit opreations:</dt>
  <dd>Could be classified same as shift/add/subtract.</dd>
  <dt>Count the set bets/dt>
  <dd>Could the number of set bits in a N long bitstring. Return the percentage of set bits. </dd>
  <dt>Subset Selection</dt>
  <dd>Variant of Long and Trunkate. Cretae substring from a long N bitstring, incrument the starting point of the substring by one until a match on the range constraint is found.</dd>
</dl>

<h2>Key Bits:</h2>
<p>
Key Bits are bits that have the most impact on the possible evaluation for a constraint.
In the example of 0-9 in unsigned binary int, the key bit(s) is/are the number place with the fewest number of set bits than the other sets. The lesser key bits have the next fewest number of set bits, and the insignificant bits have the same number set as unset.
</p>
<h2>End-ness impacts on evaluation:</h2>
<p>
Each of these methods could be impacted by the Endenace of the system they to which they run apon. All stream systems take in one unit and return on unit. They opreate in the method of the processor. In the example of 0-9 ball game, if a CSPRNG draws the first bit of a 4 bit combo to be a 1, then it must be a 8 or 9 (big endian unsigned integer) to be valid return with the constraites placed on it. When compared with the other numbers in the set, only 2 values in the range of 0-9 start with the key bit set in the far left (8: 0x1000. 9: 0x1001). An intersting sidenote: some numbers are special as they are symetrical such as 9, 0x1001.
</p>
<h2>Impact of each method</h2>
<h3>Bit by bit:</h3>
<p>
When evaluating the constraitment from numbers using a bit by bit method. Since the CSPRNG is perfect, each next bit is unguessable. The most significant bit is generated and stored first (Big Endian). It is checked, it accepted because the 0x1000 is less than 0x1010. Regardless if the first digit is 0 or 1, the odds of regenerating the next bit is 6/10 in the 0-9 example (4/10 not regenerate). [TO-DO:Bayse's therem math here]
</p>

<h2>Search Spaces and Constraints</h2>
Number storage for unsigned binary integer in D in decimal is floor((log(D)/log(2)) + 1). If each bit was randomly generated, the search space would be 2^n, where n = floor((log(D)/log(2)) + 1).
