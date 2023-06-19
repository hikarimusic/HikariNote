Data Structure (Advanced)
=========================

AVL Tree
--------

:insert: :math:`O(\log{N})`
:delete: :math:`O(\log{N})`
:search: :math:`O(\log{N})`

.. tabs::

    .. code-tab:: python

        class Node:
            def __init__(self, data):
                self.data = data
                self.height = 0
                self.left = None
                self.right = None
                self.parent = None

        class AVLTree:
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

            def rotate_left(self, x):
                y = x.right
                self.append(x, 1, y.left)
                self.transplant(x, y)
                self.append(y, 0, x)
                x.height = 1 + max(x.left.height, x.right.height)
                y.height = 1 + max(y.left.height, y.right.height)
            
            def rotate_right(self, x):
                y = x.left
                self.append(x, 0, y.right)
                self.transplant(x, y)
                self.append(y, 1, x)
                x.height = 1 + max(x.left.height, x.right.height)
                y.height = 1 + max(y.left.height, y.right.height)

            def balance(self, x):
                while x != self.null:
                    x.height = 1 + max(x.left.height, x.right.height)
                    if x.right.height - x.left.height > 1:
                        if x.right.right.height - x.right.left.height < 0:
                            self.rotate_right(x.right)
                        self.rotate_left(x)
                    elif x.right.height - x.left.height < -1:
                        if x.left.right.height - x.left.left.height > 0:
                            self.rotate_left(x.left)
                        self.rotate_right(x)
                    x = x.parent

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
                self.balance(x)
            
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
                    z = x.right
                elif x.right == self.null:
                    self.transplant(x, x.left)
                    z = x.left
                else:
                    y = x.right
                    while y.left != self.null:
                        y = y.left
                    z = y.right
                    self.transplant(y, y.right)
                    self.append(y, 0, x.left)
                    self.append(y, 1, x.right)
                    self.transplant(x, y)
                self.balance(z)
                
            
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
            tree = AVLTree()
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

Red-Black Tree
--------------

:insert: :math:`O(\log{N})`
:delete: :math:`O(\log{N})`
:search: :math:`O(\log{N})`

.. tabs::

    .. code-tab:: python

        class Node:
            def __init__(self, data):
                self.data = data
                self.color = 0
                self.left = None
                self.right = None
                self.parent = None
            
        class RedBlackTree:
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

            def rotate_left(self, x):
                y = x.right
                self.append(x, 1, y.left)
                self.transplant(x, y)
                self.append(y, 0, x)
                
            def rotate_right(self, x):
                y = x.left
                self.append(x, 0, y.right)
                self.transplant(x, y)
                self.append(y, 1, x)
            
            def balance_insert(self, x):
                while x.parent.color == 1:
                    if x.parent == x.parent.parent.left:
                        if x.parent.parent.right.color == 1:
                            x.parent.color = 0
                            x.parent.parent.right.color = 0
                            x.parent.parent.color = 1
                            x = x.parent.parent
                        elif x == x.parent.right:
                            self.rotate_left(x.parent)
                            x = x.left
                        else:
                            x.parent.color = 0
                            x.parent.parent.color = 1
                            self.rotate_right(x.parent.parent)
                    else:
                        if x.parent.parent.left == 1:
                            x.parent.color = 0
                            x.parent.parnet.left.color = 0
                            x.parent.parent.color = 1
                            x = x.parent.parent
                        elif x == x.parent.left:
                            self.rotate_right(x.parent)
                            x = x.right
                        else:
                            x.parent.color = 0
                            x.parent.parent.color = 1
                            self.rotate_left(x.parent.parent)
                self.root.color = 0

            def balance_delete(self, x):
                while x != self.root and x.color == 0:
                    if x == x.parent.left:
                        if x.parent.right.color == 1:
                            x.parent.right.color = 0
                            x.parent.color = 1
                            self.rotate_left(x.parent)
                        elif x.parent.right.right.color == 1:
                            x.parent.right.right.color = 0
                            x.parent.right.color = x.parent.color
                            x.parent.color = 1
                            self.rotate_left(x.parent)
                            x = x.parent
                        elif x.parent.right.left.color == 1:
                            x.parent.right.left.color = 0
                            x.parent.right.color = 1
                            self.rotate_right(x.parent.right)
                        else:
                            x.parent.right.color = 1
                            x = x.parent
                    else:
                        if x.parent.left.color == 1:
                            x.parent.left.color = 0
                            x.parent.color = 1
                            self.rotate_right(x.parent)
                        elif x.parent.left.left.color == 1:
                            x.parent.left.left.color = 0
                            x.parent.left.color = x.parent.color
                            x.parent.color = 1
                            self.rotate_right(x.parent)
                            x = x.parent
                        elif x.parent.left.right.color == 1:
                            x.parent.left.right.color = 0
                            x.parent.left.color = 1
                            self.rotate_left(x.parent.left)
                        else:
                            x.parent.left.color = 1
                            x = x.parent
                x.color = 0

            def insert(self, data):
                x = Node(data)
                x.color = 1
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
                self.balance_insert(x)

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
                    c = x.color
                    z = x.right
                elif x.right == self.null:
                    self.transplant(x, x.left)
                    c = x.color
                    z = x.left
                else:
                    y = x.right
                    while y.left != self.null:
                        y = y.left
                    c = y.color
                    z = y.right
                    self.transplant(y, y.right)
                    self.append(y, 0, x.left)
                    self.append(y, 1, x.right)
                    self.transplant(x, y)
                    y.color = x.color
                if c == 0:
                    self.balance_delete(z)
                

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
            tree = RedBlackTree()
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

Treap
-----

:insert: :math:`O(\log{N})` (averaged)
:delete: :math:`O(\log{N})` (averaged)
:search: :math:`O(\log{N})` (averaged)

.. tabs::

    .. code-tab:: python

        import random

        class Node:
            def __init__(self, data):
                self.data = data
                self.prior = 0
                self.left = None
                self.right = None 
                self.parent = None

        class Treap:
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
            
            def rotate_left(self, x):
                y = x.right
                self.append(x, 1, y.left)
                self.transplant(x, y)
                self.append(y, 0, x)
            
            def rotate_right(self, x):
                y = x.left
                self.append(x, 0, y.right)
                self.transplant(x, y)
                self.append(y, 1, x)

            def insert(self, data):
                x = Node(data)
                x.prior = random.randrange(100)
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
                while x != self.root and x.prior > x.parent.prior:
                    if x == x.parent.left:
                        self.rotate_right(x.parent)
                    else:
                        self.rotate_left(x.parent)

            def delete(self, data):
                x = self.root
                while x != self.null and x.data != data:
                    if data < x.data:
                        x = x.left
                    else:
                        x = x.right
                if x == self.null:
                    return
                fg = 0
                while fg == 0:
                    if x.left == self.null:
                        self.transplant(x, x.right)
                        fg = 1
                    elif x.right == self.null:
                        self.transplant(x, x.left)
                        fg = 1
                    else:
                        if x.left.prior > x.right.prior:
                            self.rotate_right(x)
                        else:
                            self.rotate_left(x)
            
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
            tree = Treap()
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

Segment Tree
------------

:build: :math:`O(N)`
:query: :math:`O(\log{N})`
:update: :math:`O(\log{N})`

.. tabs::

    .. code-tab:: python

        class SegmentTree:
            def __init__(self, size):
                self.size = size
                self.tree = [0 for i in range(4*size)]
            
            def build_util(self, v, tl, tr, array):
                if tl == tr:
                    self.tree[v] = array[tl]
                    return
                tm = (tl + tr) // 2
                self.build_util(v*2+1, tl, tm, array)
                self.build_util(v*2+2, tm+1, tr, array)
                self.tree[v] = self.tree[v*2+1] + self.tree[v*2+2]

            def query_util(self, v, tl, tr, l, r):
                if l > r:
                    return 0
                if tl == l and tr == r:
                    return self.tree[v]
                tm = (tl + tr) // 2
                s1 = self.query_util(v*2+1, tl, tm, l, min(tm, r))
                s2 = self.query_util(v*2+2, tm+1, tr, max(tm+1, l), r)
                return s1 + s2
            
            def update_util(self, v, tl, tr, pos, val):
                if tl == tr:
                    self.tree[v] = val
                    return
                tm = (tl + tr) // 2
                if pos <= tm:
                    self.update_util(v*2+1, tl, tm, pos, val)
                else:
                    self.update_util(v*2+2, tm+1, tr, pos, val)
                self.tree[v] = self.tree[v*2+1] + self.tree[v*2+2]

            def build(self, array):
                self.build_util(0, 0, self.size-1, array)
            
            def query(self, l, r):
                return self.query_util(0, 0, self.size-1, l, r)
            
            def update(self, pos, val):
                self.update_util(0, 0, self.size-1, pos, val)

        if __name__ == '__main__':
            tree = SegmentTree(10)
            tree.build([3, 9, 7, 9, 5, 6, 8, 10, 1, 10])
            print(tree.query(4, 7))
            tree.update(3, 2)
            print(tree.query(0, 4))
            tree.update(7, 4)
            print(tree.query(0, 9))

    .. code-tab:: c++

        working

Fenwick Tree
------------

:build: :math:`O(N)`
:query: :math:`O(\log{N})`
:update: :math:`O(\log{N})`

.. tabs::

    .. code-tab:: python

        class FenwickTree:
            def __init__(self, size):
                self.size = size
                self.tree = [0 for i in range(size)]
            
            def query_util(self, r):
                res = 0
                while r >= 0:
                    res += self.tree[r]
                    r = (r & (r+1)) - 1
                return res

            def build(self, array):
                for i in range(self.size):
                    self.tree[i] += array[i]
                    j = i | (i+1)
                    if j < self.size:
                        self.tree[j] += self.tree[i]

            def query(self, l, r):
                return self.query_util(r) - self.query_util(l-1)
            
            def update(self, i, v):
                add = v - self.query(i, i)
                while i < self.size:
                    self.tree[i] += add
                    i = i | (i+1)

        if __name__ == '__main__':
            tree = FenwickTree(10)
            tree.build([3, 9, 7, 9, 5, 6, 8, 10, 1, 10])
            print(tree.query(4, 7))
            tree.update(3, 2)
            print(tree.query(0, 4))
            tree.update(7, 4)
            print(tree.query(0, 9))

    .. code-tab:: c++

        working