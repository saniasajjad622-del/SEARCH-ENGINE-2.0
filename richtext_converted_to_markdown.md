class ListNode:

def \_\_init\_\_(self, doc\_id):

self.doc\_id = doc\_id

self.next = None

class LinkedList:

def \_\_init\_\_(self):

self.head = None

def insert(self, doc\_id):

temp = self.head

while temp is not None:

if temp.doc\_id == doc\_id:

return

temp = temp.next

new\_node = ListNode(doc\_id)

new\_node.next = self.head

self.head = new\_node

def get\_all\_docs(self):

docs = \[\]

temp = self.head

while temp is not None:

docs.append(temp.doc\_id)

temp = temp.next

return docs

class BSTNode:

def \_\_init\_\_(self, word):

self.word = word

self.left = None

self.right = None

class BST:

def \_\_init\_\_(self):

self.root = None

def insert\_word(self, word):

self.root = self.insert(self.root, word)

def insert(self, node, word):

if node is None:

return BSTNode(word)

if word < node.word:

node.left = self.insert(node.left, word)

elif word > node.word:

node.right = self.insert(node.right, word)

return node

class MaxHeap:

def \_\_init\_\_(self):

self.heap = \[\]

def insert(self, doc\_id, score):

self.heap.append(\[doc\_id, score\])

self.heapify\_up(len(self.heap) - 1)

def heapify\_up(self, index):

while index > 0:

parent = (index - 1) // 2

if self.heap\[index\]\[1\] > self.heap\[parent\]\[1\]:

temp = self.heap\[index\]

self.heap\[index\] = self.heap\[parent\]

self.heap\[parent\] = temp

index = parent

else:

break

def heapify\_down(self, index):

size = len(self.heap)

while True:

left = 2 \* index + 1

right = 2 \* index + 2

largest = index

if left < size and self.heap\[left\]\[1\] > self.heap\[largest\]\[1\]:

largest = left

if right < size and self.heap\[right\]\[1\] > self.heap\[largest\]\[1\]:

largest = right

if largest != index:

temp = self.heap\[index\]

self.heap\[index\] = self.heap\[largest\]

self.heap\[largest\] = temp

index = largest

else:

break

def extract\_max(self):

if len(self.heap) == 0:

return None

if len(self.heap) == 1:

return self.heap.pop()

root = self.heap\[0\]

self.heap\[0\] = self.heap.pop()

self.heapify\_down(0)

return root

class SearchEngine:

def \_\_init\_\_(self):

self.index = {}

self.documents = {}

self.tree = BST()

def add\_document(self, doc\_id, text):

self.documents\[doc\_id\] = text

words = text.lower().split()

for word in words:

if word not in self.index:

self.index\[word\] = LinkedList()

self.tree.insert\_word(word)

self.index\[word\].insert(doc\_id)

def search(self, keyword):

keyword = keyword.lower()

if keyword not in self.index:

print("\\nNo documents found.")

return

doc\_list = self.index\[keyword\].get\_all\_docs()

heap = MaxHeap()

for doc\_id in doc\_list:

text = self.documents\[doc\_id\]

words = text.lower().split()

score = 0

for w in words:

if w == keyword:

score = score + 1

heap.insert(doc\_id, score)

print("\\nRanked Search Results:")

while True:

result = heap.extract\_max()

if result is None:

break

print("Document ID:", result\[0\], "| Relevance Score:", result\[1\])

engine = SearchEngine()

engine.add\_document(1, "Electric field theory and applications")

engine.add\_document(2, "Electric circuits and electric machines")

engine.add\_document(3, "Field theory in electromagnetic systems")

engine.add\_document(4, "Electric power systems and electric control")

while True:

print("\\n========= MINI SEARCH ENGINE =========")

print("1. Search Keyword")

print("2. Exit")

choice = input("Enter choice: ")

if choice == "1":

keyword = input("Enter keyword: ")

engine.search(keyword)

elif choice == "2":

print("Exiting program.")

break

else:

print("Invalid choice.")