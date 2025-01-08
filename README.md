# Count-Prefix-and-Suffix-Pairs-I

You are given a 0-indexed string array words.
Let's define a boolean function isPrefixAndSuffix that takes two strings, str1 and str2:
isPrefixAndSuffix(str1, str2) returns true if str1 is both a 
prefix and a suffix of str2, and false otherwise.
For example, isPrefixAndSuffix("aba", "ababa") is true because "aba" is a prefix of "ababa" and also a suffix, but isPrefixAndSuffix("abc", "abcd") is false.

Return an integer denoting the number of index pairs (i, j) such that i < j, and isPrefixAndSuffix(words[i], words[j]) is true.

class PrefSuffTrieNode:
    def __init__(self):
        self.children = {}
        self.poss_ends = 0

class PrefSuffTrie:
    def __init__(self):
        self.root = PrefSuffTrieNode()

    def insert(self, word):
        cur = self.root
        for i in range(len(word)):
            pr = word[i]
            su = word[len(word) - 1 - i]
            if (pr, su) not in cur.children:
                cur.children[(pr, su)] = PrefSuffTrieNode()
            cur = cur.children[(pr, su)]
            cur.poss_ends += 1

    def search(self, word):
        cur = self.root
        for i in range(len(word)):
            pr = word[i]
            su = word[len(word) - 1 - i]
            if (pr, su) not in cur.children:
                return 0
            cur = cur.children[(pr, su)]
        return cur.poss_ends 


class Solution:
    def countPrefixSuffixPairs(self, words: List[str]) -> int:
        res = 0
        trie = PrefSuffTrie()
        
        for word in words[::-1]:
            res += trie.search(word)
            trie.insert(word)
          
        return res
