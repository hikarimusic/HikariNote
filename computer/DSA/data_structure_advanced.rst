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

Splay Tree 
----------

:insert: :math:`O(\log{N})` (averaged)
:delete: :math:`O(\log{N})` (averaged)
:search: :math:`O(\log{N})` (averaged)

.. tabs::

    .. code-tab:: python

        class Node:
            def __init__(self, data):
                self.data = data
                self.left = None
                self.right = None
                self.parent = None

        class SplayTree:
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

            def splay(self, x):
                while x != self.root:
                    if x == x.parent.left:
                        if x.parent == x.parent.parent.left:
                            self.rotate_right(x.parent.parent)
                            self.rotate_right(x.parent)
                        elif x.parent == x.parent.parent.right:
                            self.rotate_right(x.parent)
                            self.rotate_left(x.parent)
                        else:
                            self.rotate_right(x.parent)
                    else:
                        if x.parent == x.parent.parent.right:
                            self.rotate_left(x.parent.parent)
                            self.rotate_left(x.parent)
                        elif x.parent == x.parent.parent.left:
                            self.rotate_left(x.parent)
                            self.rotate_right(x.parent)
                        else:
                            self.rotate_left(x.parent)

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
                self.splay(x)

            def delete(self, data):
                p = self.null
                c = self.root
                while c != self.null and c.data != data:
                    p = c
                    if data < c.data:
                        c = c.left
                    else:
                        c = c.right
                if c == self.null:
                    self.splay(p)
                else:
                    self.splay(c)
                    if c.left == self.null:
                        self.transplant(c, c.right)
                    else:
                        self.transplant(c, c.left)
                        x = c.left
                        while x.right != self.null:
                            x = x.right
                        self.splay(x)
                        self.append(x, 1, c.right)

            def search(self, data):
                p = self.null
                c = self.root
                while c != self.null and c.data != data:
                    p = c
                    if data < c.data:
                        c = c.left
                    else:
                        c = c.right
                if c == self.null:
                    self.splay(p)
                    return False
                self.splay(c)
                return True

        if __name__ == '__main__':
            tree = SplayTree()
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

B-Tree 
------

:insert: :math:`O(\log{N})`
:delete: :math:`O(\log{N})`
:search: :math:`O(\log{N})`

.. tabs::

    .. code-tab:: python

        class Node:
            def __init__(self, degree):
                self.keys = [0 for i in range(degree)]
                self.child = [None for i in range(degree+1)]
                self.parent = None
                self.cnt = 0
            
        class BTree:
            def __init__(self, degree):
                self.deg = degree
                self.root = Node(self.deg)
            
            def balance_insert(self, x):
                while x.cnt == self.deg:
                    if x == self.root:
                        self.root = Node(self.deg)
                        self.root.child[0] = x
                        x.parent = self.root
                    y = Node(self.deg)
                    for i in range(x.cnt//2):
                        y.keys[i] = x.keys[i+(x.cnt+1)//2]
                    for i in range(x.cnt//2+1):
                        y.child[i] = x.child[i+(x.cnt+1)//2]
                        if y.child[i]:
                            y.child[i].parent = y
                    y.parent = x.parent 
                    y.cnt = x.cnt // 2
                    x.cnt = (x.cnt-1) // 2
                    p = x.parent
                    for i in range(p.cnt+1):
                        if p.child[i] == x:
                            c = i
                    for i in range(p.cnt-1, c-1, -1):
                        p.keys[i+1] = p.keys[i]
                    for i in range(p.cnt, c, -1):
                        p.child[i+1] = p.child[i]
                    p.keys[c] = x.keys[x.cnt]
                    p.child[c+1] = y
                    p.cnt += 1
                    x = p
            
            def balance_delete(self, x):
                while x.cnt < (self.deg-1)//2:
                    if x == self.root:
                        if x.cnt == 0:
                            self.root = x.child[0]
                        break
                    p = x.parent
                    for i in range(p.cnt+1):
                        if p.child[i] == x:
                            c = i
                    if c > 0 and p.child[c-1].cnt > (self.deg-1)//2:
                        s = p.child[c-1]
                        for i in range(x.cnt-1, -1, -1):
                            x.keys[i+1] = x.keys[i]
                        for i in range(x.cnt, -1, -1):
                            x.child[i+1] = x.child[i]
                        x.keys[0] = p.keys[c-1]
                        x.child[0] = s.child[s.cnt]
                        if x.child[0]:
                            x.child[0].parent = x
                        x.cnt += 1
                        p.keys[c-1] = s.keys[s.cnt-1]
                        s.cnt -= 1
                    elif c < p.cnt and p.child[c+1].cnt > (self.deg-1)//2:
                        s = p.child[c+1]
                        x.keys[x.cnt] = p.keys[c]
                        x.child[x.cnt+1] = s.child[0]
                        if x.child[x.cnt+1]:
                            x.child[x.cnt+1].parent = x
                        x.cnt += 1
                        p.keys[c] = s.keys[0]
                        for i in range(1, s.cnt):
                            s.keys[i-1] = s.keys[i]
                        for i in range(1, s.cnt+1):
                            s.child[i-1] = s.child[i]
                        s.cnt -= 1
                    elif c > 0:
                        s = p.child[c-1]
                        s.keys[s.cnt] = p.keys[c-1]
                        for i in range(x.cnt):
                            s.keys[i+s.cnt+1] = x.keys[i]
                        for i in range(x.cnt+1):
                            s.child[i+s.cnt+1] = x.child[i]
                            if s.child[i+s.cnt+1]:
                                s.child[i+s.cnt+1].parent = s
                        s.cnt += (1+x.cnt)
                        for i in range(c, p.cnt):
                            p.keys[i-1] = p.keys[i]
                        for i in range(c+1, p.cnt+1):
                            p.child[i-1] = p.child[i]
                        p.cnt -= 1
                    else:
                        s = p.child[c+1]
                        x.keys[x.cnt] = p.keys[c]
                        for i in range(s.cnt):
                            x.keys[i+x.cnt+1] = s.keys[i]
                        for i in range(s.cnt+1):
                            x.child[i+x.cnt+1] = x.child[i]
                            if x.child[i+x.cnt+1]:
                                x.child[i+x.cnt+1].parent = x
                        x.cnt += (1+s.cnt)
                        for i in range(c+1, p.cnt):
                            p.keys[i-1] = p.keys[i]
                        for i in range(c+2, p.cnt+1):
                            p.child[i-1] = p.child[i]
                        p.cnt -= 1
                    x = p

            def insert(self, data):   
                x = self.root
                c = 0
                fg = False
                while not fg:
                    if c < x.cnt and data > x.keys[c]:
                        c += 1
                    elif x.child[c]:
                        x = x.child[c]
                        c = 0
                    else:
                        fg = True
                for i in range(x.cnt-1, c-1, -1):
                    x.keys[i+1] = x.keys[i]
                x.keys[c] = data
                x.cnt += 1
                self.balance_insert(x)
            
            def delete(self, data):
                x = self.root
                c = 0
                fg = False
                while x and (not fg):
                    if c == x.cnt or data < x.keys[c]:
                        x = x.child[c]
                        c = 0
                    elif data == x.keys[c]:
                        fg = True
                    else:
                        c += 1
                if not fg:
                    return
                if x.child[c]:
                    y = x.child[c]
                    while y.child[y.cnt]:
                        y = y.child[y.cnt]
                    x.keys[c] = y.keys[y.cnt-1]
                    y.cnt -= 1
                    x = y
                else:
                    for i in range(c+1, x.cnt):
                        x.keys[i-1] = x.keys[i]
                    x.cnt -= 1
                self.balance_delete(x)
            
            def search(self, data):
                x = self.root
                c = 0
                fg = False
                while x and (not fg):
                    if c == x.cnt or data < x.keys[c]:
                        x = x.child[c]
                        c = 0
                    elif data == x.keys[c]:
                        fg = True
                    else:
                        c += 1
                if not fg:
                    return False
                return True

        if __name__ == '__main__':
            tree = BTree(4)
            for i in [8, 16, 18, 1, 12, 10, 15, 4, 2, 7, 13, 3, 17, 5, 11, 14, 9, 20, 6, 19]:
                tree.insert(i)
            for i in [4, 6, 3, 8, 1, 15, 14, 20, 16, 13, 19]:
                tree.delete(i)
            for i in range(1, 21):
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

Trie 
----

:insert: :math:`O(S)`
:delete: :math:`O(S)`
:search: :math:`O(S)`

.. tabs::

    .. code-tab:: python

        class Node:
            def __init__(self, alphabet):
                self.child = [None for i in range(alphabet)]
                self.word = False 

        class Trie:
            def __init__(self, alphabet):
                self.alphabet = alphabet
                self.root = Node(self.alphabet)
            
            def insert(self, word):
                x = self.root
                for c in word:
                    if not x.child[ord(c)-ord('a')]:
                        x.child[ord(c)-ord('a')] = Node(self.alphabet)
                    x = x.child[ord(c)-ord('a')]
                x.word = True
            
            def delete(self, word):
                brn = self.root 
                brc = word[0]
                x = self.root
                for c in word:
                    if not x.child[ord(c)-ord('a')]:
                        return
                    cnt = 0
                    for i in range(self.alphabet):
                        if x.child[i]:
                            cnt += 1
                    if cnt > 1:
                        brn = x
                        brc = c
                    x = x.child[ord(c)-ord('a')]
                cnt = 0
                for i in range(self.alphabet):
                    if x.child[i]:
                        cnt += 1
                if x.word == False:
                    return 
                if cnt == 0:
                    brn.child[ord(brc)-ord('a')] = None 
                else:
                    x.word = False 
            
            def search(self, word):
                x = self.root
                for c in word:
                    if not x.child[ord(c)-ord('a')]:
                        return
                    x = x.child[ord(c)-ord('a')]
                if x.word == False:
                    return False
                return True

        if __name__ == '__main__':
            tree = Trie(26)
            tree.insert("glucose")
            tree.insert("gluconeogenesis")
            tree.insert("glycolysis")
            tree.delete("glucose")
            tree.insert("glycogen")
            tree.insert("glycogenesis")
            tree.insert("glycogenolysis")
            tree.delete("glycogen")
            tree.insert("galactose")
            tree.insert("fructose")
            tree.insert("ribose")
            tree.insert("ribulose")
            print(tree.search("glucose"))
            print(tree.search("glycolysis"))
            print(tree.search("ribose"))

    .. code-tab:: c++

        working

Suffix Tree 
-----------

:build: :math:`O(N)`
:search: :math:`O(M)`

.. tabs::

    .. code-tab:: python

        class Node:
            def __init__(self, num, start, end):
                self.num = num
                self.start = start 
                self.end = end
                self.child = {}
                self.link = None
            
            def len(self):
                return self.end - self.start

        class SuffixTree:
            def __init__(self):
                self.root = Node(-1, -1, 0)
                self.text = ""
            
            def build(self, text):
                text += '$'
                self.text = text
                head, tail = 0, -1
                node, edge = self.root, 0
                for c in text:
                    tail += 1
                    last = None
                    nxet = None
                    while tail - head >= 0:
                        adds = False
                        while text[edge] in node.child and (tail-edge) >= node.child[text[edge]].len():
                            node = node.child[text[edge]]
                            edge += node.len()
                        if text[edge] not in node.child:
                            node.child[text[edge]] = Node(head, tail, int(1e9))
                            head += 1
                            adds = True
                            nxet = node
                        elif text[node.child[text[edge]].start+tail-edge] != c:
                            split = Node(-1, node.child[text[edge]].start, node.child[text[edge]].start+tail-edge)
                            leaf = Node(head, tail, int(1e9))
                            head += 1
                            adds = True
                            prev = node.child[text[edge]]
                            prev.start += tail - edge
                            node.child[text[edge]] = split 
                            split.child[c] = leaf
                            split.child[text[prev.start]] = prev
                            nxet = split
                        elif tail - edge == 0:
                            nxet = node
                        if last:
                            last.link = nxet
                        last = nxet
                        if adds == False:
                            break
                        if node == self.root:
                            edge += 1
                        else:
                            node = node.link
            
            def search(self, pattern):
                acn, ace, acl = self.root, '$', -1
                for c in pattern:
                    acl += 1
                    if ace == '$':
                        ace = c
                    elif acl == acn.child[ace].len():
                        acn = acn.child[ace]
                        ace = c
                        acl = 0
                    if ace not in acn.child or self.text[acn.child[ace].start+acl] != c:
                        return []
                acn = acn.child[ace]
                res = []
                stack = [acn]
                while stack:
                    u = stack.pop()
                    if not u.child:
                        res.append(u.num)
                    for v in u.child.values():
                        stack.append(v)
                return res
                        
        if __name__ == '__main__':
            tree = SuffixTree()
            tree.build("abcabxabcd")
            print(tree.search("bc"))

    .. code-tab:: c++

        working

K-Dimensional Tree
------------------

Fibonacci Heap
--------------

Disjoint Set 
------------

:build: :math:`O(N)`
:find: :math:`O(\alpha(N))` (about :math:`O(1)` )
:union: :math:`O(\alpha(N))` (about :math:`O(1)` )

.. tabs::

    .. code-tab:: python

        class DisjointSet:
            def __init__(self, size):
                self.size = size
                self.par = [0 for i in range(size)]
                self.cnt = [0 for i in range(size)]

            def build(self):
                for x in range(self.size):
                    self.par[x] = x
                    self.cnt[x] = 1
            
            def find(self, x):
                if self.par[x] != x:
                    self.par[x] = self.find(self.par[x])
                return self.par[x]
            
            def union(self, x, y):
                a = self.find(x)
                b = self.find(y)
                if a == b:
                    return
                if self.cnt[a] < self.cnt[b]:
                    a, b = b, a
                self.par[b] = a
                self.cnt[a] += self.cnt[b]

        if __name__ == '__main__':
            myset = DisjointSet(8)
            myset.build()
            myset.union(0, 1)
            myset.union(2, 3)
            myset.union(3, 4)
            myset.union(5, 6)
            myset.union(1, 4)
            print(myset.find(0))
            print(myset.find(3))
            print(myset.find(6))

    .. code-tab:: c++

        working