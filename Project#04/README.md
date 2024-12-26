# Project #4: The second-order Moller-Plesset perturbation (MP2) energy
Unlike the Hartree-Fock energy, correlation energies like the MP2 energy are usually expressed in terms of MO-basis quantities (integrals, MO energies). 
The most expensive part of the calculation is the transformation of the two-electron integrals from the AO to the MO basis representation. 
The purpose of this project is to understand this transformation and fundamental techniques for its efficient implementation.
The theoretical background and a concise set of instructions for this project may be found [here](./project4-instructions.pdf).

## Step #1: Read the Two-Electron Integrals
The Mulliken-ordered integrals are defined as:

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="./figures/dark/eri.png">
  <source media="(prefers-color-scheme: light)" srcset="./figures/eri.png">
  <img src="./figures/eri.png" height="30">
</picture>

Concise instructions for this step can be found in [Project #3](../Project%2303).

## Step #2: Obtain the MO Coefficients and MO Energies 
Use the values you computed in the Hartree-Fock program of [Project #3](../Project%2303).
## Step #3: Transform the Two-Electron Integrals to the MO Basis 
### The Noddy Algorithm 

The most straightforward expression of the AO/MO integral transformation is

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="./figures/dark/noddy-transform.png">
  <source media="(prefers-color-scheme: light)" srcset="./figures/noddy-transform.png">
  <img src="./figures/noddy-transform.png" height="50">
</picture>

This approach is easy to implement (hence the word "[noddy](http://www.hackerslang.com/noddy.html)" above), but is expensive due to its N<sup>8</sup> computational order.  Nevertheless, you should start with this algorithm to get your code working, and run timings (use the UNIX "time" command) for the test cases below to get an idea of its computational cost.

  * [Hint 1](./hints/hint1.md): Noddy transformation code
### The Smarter Algorithm

Notice that none of the *C* coefficients in the above expression have any indices in common.  Thus, the summation could be rearranged such that:

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="./figures/dark/smart-transform.png">
  <source media="(prefers-color-scheme: light)" srcset="./figures/smart-transform.png">
  <img src="./figures/smart-transform.png" height="60">
</picture>

This means that each summation within brackets could be carried out separately, 
starting from the innermost summation over <html>&#963;</html>, if we store the results at each step.  This reduces the N<sup>8</sup> algorithm above to four N<sup>5</sup> steps.

Be careful about the permutational symmetry of the integrals and intermediates at each step: 
while the AO-basis integrals have eight-fold permutational symmetry, the partially transformed integrals have much less symmetry.

After you have the noddy algorithm working and timed, modify it to use this smarter algorithm and time the test cases again.  Enjoy!

  * [Hint 2](./hints/hint2.md): Smarter transformation code

## Step #4: Compute the MP2 Energy

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="./figures/dark/mp2-energy.png">
  <source media="(prefers-color-scheme: light)" srcset="./figures/mp2-energy.png">
  <img src="./figures/mp2-energy.png" height="60">
</picture>

where *i* and *j* denote doubly-occupied orbitals and *a* and *b* unoccupied orbitals, and the denominator involves the MO energies.

  * [Hint 3](./hints/hint3.md): Occupied versus unoccupied orbitals

## Test Cases
The input structures, integrals, etc. for these examples are found in the [input directory](./input).

* STO-3G Water | [output](./output/h2o/STO-3G/output.txt)
* DZ Water | [output](./output/h2o/DZ/output.txt)
* DZP Water | [output](./output/h2o/DZP/output.txt)
* STO-3G Methane | [output](./output/ch4/STO-3G/output.txt)

