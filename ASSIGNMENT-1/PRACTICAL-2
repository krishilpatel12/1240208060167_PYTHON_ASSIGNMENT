class TrieNode:
    def __init__(self):
        self.children = {}
        self.end = False


class Trie:
    def __init__(self):
        self.root = TrieNode()

    def insert(self, word):
        node = self.root

        for ch in word.lower():
            if ch not in node.children:
                node.children[ch] = TrieNode()
            node = node.children[ch]

        node.end = True

    def contains_word(self, text):
        text = text.lower()

        for i in range(len(text)):
            node = self.root

            for j in range(i, len(text)):
                if text[j] not in node.children:
                    break

                node = node.children[text[j]]

                if node.end:
                    return True

        return False


b = int(input())

trie = Trie()

for _ in range(b):
    trie.insert(input().strip())

n = int(input())

for index in range(1, n + 1):
    password = input().strip()

    if len(password) < 6 or len(password) > 12:
        print(f"{index}: WEAK_LENGTH")
        continue

    has_lower = any(c.islower() for c in password)
    has_upper = any(c.isupper() for c in password)
    has_digit = any(c.isdigit() for c in password)
    has_special = any(c in "$#@" for c in password)

    if not (has_lower and has_upper and has_digit and has_special):
        print(f"{index}: WEAK_PATTERN")
        continue

    if trie.contains_word(password):
        print(f"{index}: COMPROMISED")
        continue

    repeated = False

    for i in range(3, len(password)):
        if password[i] == password[i-1] == password[i-2] == password[i-3]:
            repeated = True
            break

    if repeated:
        print(f"{index}: WEAK_PATTERN")
    else:
        print(f"{index}: STRONG")
