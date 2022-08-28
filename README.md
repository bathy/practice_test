#  Test problem set 1 -- Gao lab

In this problem set, there are three problems: 1) website development 2) machine learning 3) algorithm and data structure. Please choose at least one of the problems and solve it on your own, then return your full answer in the requested format to me by email or a link in email. For each problem, you need to time yourself and send me how long it took you to achieve your final answer. This time will be used to set the future expectation for your work, so please be honest about it.



## Problem 1 -- Website development

**Problem**: Please use your favorite front-end and back-end framework to reproduce the scaffold of this website: [matrisomedb.org](http://matrisomedb.pepchem.org/) . You can re-use any of the resources from the original website but that's all you have. Your answer doesn't need to be perfect and you can certainly make improvement, please use this opportunity to show off your superpower in web development.

**Answer format**: Please zip your solution and include a README file to explain your design and how long it took you to reach this point. Your solution should be fully deployable on either linux/windows/osx system.



## Problem 2 -- Machine learning

**Problem**: Please download the [pin.7z](https://uofi.box.com/s/mc4nnkbb4afxp80sx8fgmkmjeevwl3pu) file and once extracted, you will have a few .pin files. These are peptide identifications files, in tsv format, with different parameters in different columns. Your goal is to train a model using the following columns (you can use less, but no more than the following columns) to predict the [Label] column: 

| ExpMass | rank | abs_ppm | abs_mass_diff | log10_evalue | hyperscore | delta_hyperscore | matched_ion_num | matched_ion_fraction | peptide_length | ntt  | nmc  | charge_1 | charge_2 | charge_3 | charge_4 | charge_5 | charge_6 | charge_7 |
| ------- | ---- | ------- | ------------- | ------------ | ---------- | ---------------- | --------------- | -------------------- | -------------- | ---- | ---- | -------- | -------- | -------- | -------- | -------- | -------- | -------- |
|         |      |         |               |              |            |                  |                 |                      |                |      |      |          |          |          |          |          |          |          |

This should be your [Label] column (-1 or 1, a binary classification problem), and your classification target for test data.
| Label |
|-------|

For model training and testing, you can split all the data in each of the pin file freely into training and test dataset and verify your model.

**Answer format**: Please download the [test.7z](https://github.com/bathy/practice_test/blob/main/test.7z) file and replace the [Label] column with your predicted value (either 1 or -1, currently all 0, meaning UNKNOWN). Please zip all your codes and your predicted pin file into a single file and share it with me. No need to include the original training file from the pin.7z. You should also include a README file to explain briefly what you did and if any specific packages are needed to run your model. Your model will be evaluated on both speed and accuracy.



## Problem 3 -- Algorithm

**Problem background**: Cleave a protein in silico with various protease specificity
When protease cleaves a protein, it typically has some kind of specificity. (if you are not familiar with protein, just treat it as an equivalent of a long string of letters, like: 'MMKRPQLHRMRQLAQTGSLGRTKPETAEFLGEDL', protease is an enzyme which cuts this sequence)

For example, if you have a protein with the sequence of 'MMKRPQLHRMRQLAQTGSLGRTKPETAEFLGEDL', and if you use trypsin as the protease (trypsin cleaves after K and after R), then it will create the following tryptic peptides after cleavage:

'MMK / R / PQLHR / MR / QLAQTGSLGR / TK / PETAEFLGEDL' ('/' indicates cleavage sites)
Then you will have the following sequence generated: ['MMK', 'R', 'PQLHR', 'MR', 'QLAQTGSLGR', 'TK', 'PETAEFLGEDL' ]

If you consider the more complex rule of trypsin cleavage, trypsin cleaves after K and R but will not cleave before a P (P is steric hindered), then it will create the following peptides: 

'MMK / RPQLHR / MR / QLAQTGSLGR / TKPETAEFLGEDL' ('/' indicates cleavage sites)
Then you will have the following sequence generated: ['MMK', 'RPQLHR', 'MR', 'QLAQTGSLGR', 'TKPETAEFLGEDL']

Here is the full trypsin rule that we are going to use for this problem: 

***trypsin rule*** (\[KR\](?=\[^P\]))|((?<=W)K(?=P))|((?<=M)R(?=P))

it means, trypsin cleaves after K and R, but not before P (so no KP nor RP will be cleaved), unless it is WKP or MRP (means WKP and MRP will still be cleaved).

Sometimes, trypsin will not follow the rule completely, meaning that it can have some errors, there are typically two types of errors:

1. Mis-cleavage, which can be from [1 - 5], means that trypsin missed [1 - 5] cleavage sites. For example, when a maximum of 2 mis-cleavages are allowed, the same protein will result in the following sequence: ['MMK', 'MMKRPQLHR', 'MMKRPQLHRMR', 'RPQLHR', 'RPQLHRMR', 'RPQLHRMRQLAQTGSLGR', 'MR', 'MRQLAQTGSLGR', 'MRQLAQTGSLGRTKPETAEFLGEDL', 'QLAQTGSLGR', 'QLAQTGSLGRTKPETAEFLGEDL', 'TKPETAEFLGEDL']

   Note that in 'M**R**QLAQTGSLG**R**TKPETAEFLGEDL', two R are skipped, so that's 2 miscleavages, but K doesn't count because it is followed by a P, so KP is not a cleavage site.

2. Semi-specific cleavage, which means that at least one end (left end or the right end) of the resulting peptides follows the cleavage rule, the other end may or may not follow the same rule. Please note that the beginning and the end of the protein 'M' and 'L' in this case, are considered as correct cleavage sites. For example, under semi-specific cleavage rule, the following sequences are possible: ['MMK', 'MM' (left end following the rule, right end doesn't), 'MK' (right end following the rule, left end doesn't), 'M' (left end following the rule, right end doesn't) , 'K' (right end following the rule, left end doesn't)].

USE THIS TO VERIFY IF YOU UNDERSTAND THE BACKGROUND CORRECTLY: The cleavage of protein "MMKRPQLHRMRQLAQTGSLGRTKPETAEFLGEDL" using the above cleavage rule (\[KR\](?=\[^P\]))|((?<=W)K(?=P))|((?<=M)R(?=P)), allowing ≤2 miscleavages, and allowing "semi-specific" cleavage will result in total 110 unique sequences.

**Problem and Answer format**: Read the [protein_seq.txt](https://github.com/bathy/practice_test/blob/main/protein_seq.txt) file as input (one protein sequence per line, 100 proteins in total), and write all the unique sequences (sorted by length and alphabetically) into a sequence.txt file, one sequence per line. Please zip  your code, your sequence.txt file together. Your code will be evaluated on accuracy and speed.
