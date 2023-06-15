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