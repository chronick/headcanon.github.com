---
layout: post
title: "Levenshtein Distance"
description: ""
category: 
tags: []
---
{% include JB/setup %}

#Levenshtein Distance

##Background

While attempting to solve one of the puzzles on Facebook's careers page, I came across an interesting problem. This puzzle involved finding the edit distance between two words, or rather, how many atomic changes must be made to the first word in order for it to match the second. An atomic change in a word is defined as one of three things:

-an addition of a letter
-a removal of a letter
-replacement of a letter with another letter

So for instance, the edit distance between the words "kitten" and "sitting" is 3:

    kitten -> sitten
    sitten -> sittin
    sittin -> sitting

An algorithm to solve this problem was invented in 1965 by Russian-born Vladimir Levenshtein. Basically, it involves building a matrix of the edit distance between each substring of each word starting at the first letter and progressing towards the last letter of each word. When the algorithm is finished, the total edit distance is located in the topmost cell (or the last column and last row).


##Pseudocode

**found on wikipedia. See references below**

    int LevenshteinDistance(char s[1..m], char t[1..n])
    {
     // for all i and j, d[i,j] will hold the Levenshtein distance between
     // the first i characters of s and the first j characters of t;
     // note that d has (m+1)x(n+1) values
     declare int d[0..m, 0..n]
    
     for i from 0 to m
       d[i, 0] := i // the distance of any first string to an empty second string
     for j from 0 to n
       d[0, j] := j // the distance of any second string to an empty first string
    
     for j from 1 to n
     {
       for i from 1 to m
       {
         if s[i] = t[j] then  
           d[i, j] := d[i-1, j-1]       // no operation required
         else
           d[i, j] := minimum
                      (
                        d[i-1, j] + 1,  // a deletion
                        d[i, j-1] + 1,  // an insertion
                        d[i-1, j-1] + 1 // a substitution
                      )
       }
     }
     return d[m,n]
   }

For the words "kitten" and "sitting", this algorithm will generate the following matrix:

          k  i  t  t  e  n
      [0  1  2  3  4  5  6]
    s [1  1  2  3  4  5  6]
    i [2  2  1  2  3  4  5]
    t [3  3  2  1  2  3  4]
    t [4  4  3  2  1  2  3]
    i [5  5  4  3  2  2  3]
    n [6  6  5  4  3  3  2]
    g [7  7  6  5  4  4  3] <-- final answer is 3


##How It Works

So, in order to understand how this works, consider the top left cell, which is 0. This means that the edit distance between two empty words is, quite obviously, zero. Now consider the last element of the first row, which is 6. This means that in order for the word "kitten" to equal to an empty string, then 6 letters must be removed, thus the edit distance of 6. Next, consider the cell two below that (last column, 3rd row), which is 5. This means that the edit distance between the word "kitten" and "si" is 5, since 4 letters must be removed, and 'k' must be switched to 's'. Since the i is there, then the edit disance is one less. This progresses as such until all the letters that are similar are removed, and one more is added in this case to account for the extra letter that must be added, since the second word is longer than the first.  Hopefully this should give you an idea of how it works. To try it out for yourself, I have included below an implementation of this algorithm in python.


##Further Reading

http://en.wikipedia.org/wiki/Levenshtein_distance


Implementation

    import sys

    def listprintstr(astr):
     return '  '.join(list(astr))

    def printLevDistMatrix(str1,str2,d):
     print "     ",listprintstr(str1)
     for i in range(len(d)):
      print (" "+str2)[i],'['+'  '.join([str(a) for a in d[i]])+']'

    def LevDist(str1,str2,debug=False):
     d = []
     m = len(str1)
     n = len(str2)

     for i in range(n+1):
      d.append([])
      for i in range(m+1):
       d[-1].append(0)
     
     for i in range(m+1):
      d[0][i] = i

     for j in range(n+1):
      d[j][0] = j
     
     for j in range(1,m+1):
      for i in range(1,n+1):

       if str1[j-1] == str2[i-1]:
        d[i][j] = d[i-1][j-1]
       else:
        d[i][j] = min(
           d[i-1][j] + 1,
           d[i][j-1] + 1,
           d[i-1][j-1] + 1
           )

     if debug:
      printLevDistMatrix(str1,str2,d)
     
     return d[n][m]

    def main():
     if len(sys.argv) < 2:
      print "usage: %s <word1> <word2>"%(sys.argv[0])
     
     print LevDist(sys.argv[1],sys.argv[2],True)
     
    if __name__ == '__main__':
     main()
