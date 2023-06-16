Data Structure
==============

Binary Search Tree
------------------

:insert: :math:`O(N)`
:delete: :math:`O(N)`
:search: :math:`O(N)`

.. tabs::

    .. code-tab:: python

        class Node:
            def __init__(self, data):
                self.data = data
                self.left = None
                self.right = None
                self.parent = None

        class BinarySearchTree:
            def __init__(self):
                self.null = Node(0)
                self.root = self.null

            def append(self, p, lr, x):
                if p == self.null:
                    self.root = x
                elif lr == 0:
                    p.left = x
                else:
                    p.right = x
                x.parent = p
            
            def transplant(self, x, y):
                if x.parent == self.null:
                    self.root = y
                elif x == x.parent.left:
                    x.parent.left = y
                else:
                    x.parent.right = y
                y.parent = x.parent
            
            def insert(self, data):
                x = Node(data)
                x.left = self.null
                x.right = self.null
                p = self.null
                c = self.root
                lr = 0
                while c != self.null:
                    p = c
                    if data < c.data:
                        c = c.left
                        lr = 0
                    else:
                        c = c.right
                        lr = 1
                self.append(p, lr, x)
            
            def delete(self, data):
                x = self.root
                while x != self.null and x.data != data:
                    if data < x.data:
                        x = x.left
                    else:
                        x = x.right
                if x == self.null:
                    return
                if x.left == self.null:
                    self.transplant(x, x.right)
                elif x.right == self.null:
                    self.transplant(x, x.left)
                else:
                    y = x.right
                    while y.left != self.null:
                        y = y.left
                    self.transplant(y, y.right)
                    self.append(y, 0, x.left)
                    self.append(y, 1, x.right)
                    self.transplant(x, y)
            
            def search(self, data):
                x = self.root
                while x != self.null and x.data != data:
                    if data < x.data:
                        x = x.left
                    else:
                        x = x.right
                if x == self.null:
                    return False
                return True

        if __name__ == '__main__':
            tree = BinarySearchTree()
            tree.insert(7)
            tree.insert(2)
            tree.insert(10)
            tree.insert(8)
            tree.insert(9)
            tree.delete(8)
            tree.insert(6)
            tree.insert(1)
            tree.insert(4)
            tree.delete(7)
            tree.delete(4)
            tree.insert(3)
            tree.delete(6)
            tree.insert(5)
            for i in range(1, 11):
                if tree.search(i):
                    print(i)

    .. code-tab:: c++

        working

Binary Heap
-----------

:push: :math:`O(\log{N})`
:pop: :math:`O(\log{N})`
:peek: :math:`O(1)`

.. tabs::
    
    .. code-tab:: python

        class BinaryHeap:
            def __init__(self, capicity):
                self.capicity = capicity
                self.tree = [0 for x in range(capicity)]
                self.size = 0
            
            def heapify_up(self, x):
                y = x
                if int((x-1)/2) >= 0:
                    if self.tree[int((x-1)/2)] < self.tree[y]:
                        y = int((x-1)/2)
                if x != y:
                    self.tree[x], self.tree[y] = self.tree[y], self.tree[x]
                    self.heapify_up(y)
            
            def heapify_down(self, x):
                y = x
                if x*2+1 < self.size:
                    if self.tree[x*2+1] > self.tree[y]:
                        y = x*2+1
                if x*2+2 < self.size:
                    if self.tree[x*2+2] > self.tree[y]:
                        y = x*2+2
                if x != y:
                    self.tree[x], self.tree[y] = self.tree[y], self.tree[x]
                    self.heapify_down(y)
            
            def push(self, data):
                if self.size == self.capicity:
                    return
                self.tree[self.size] = data
                self.size += 1
                self.heapify_up(self.size-1)
            
            def pop(self):
                if self.size == 0:
                    return
                self.tree[0] = self.tree[self.size-1]
                self.size -= 1
                self.heapify_down(0)
            
            def peek(self):
                return self.tree[0]

        if __name__ == '__main__':
            heap = BinaryHeap(100)
            heap.push(2)
            heap.push(6)
            heap.push(1)
            heap.pop()
            heap.push(10)
            heap.push(3)
            heap.push(9)
            heap.pop()
            heap.push(5)
            heap.push(7)
            heap.push(4)
            heap.push(8)
            heap.pop()
            print(heap.peek())
            heap.pop()
            print(heap.peek())
            heap.pop()
            print(heap.peek())

    .. code-tab:: c++

        working