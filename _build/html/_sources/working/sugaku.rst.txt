数学
====

積分法
------

不定積分
^^^^^^^^

- **基本的な積分**

  - :math:`\int x^\alpha\,dx=\frac{x^{\alpha+1}}{\alpha+1}+C\quad\int\frac{dx}{x}=\log|x|+C`
  - :math:`\int\sin x\,dx=-\cos x+C\quad\int\cos x\,dx=\sin x+C`
  - :math:`\int\frac{dx}{\cos^2 x}=\tan x +C\quad\int\frac{dx}{\sin^2 x}=-\frac{1}{\tan x}+C`
  - :math:`\int e^x\,dx=e^x+C\quad\int a^x\,dx=\frac{a^x}{\log a}+C`

- **置換積分法**

  - :math:`\int f(g(x))g'(x)\,dx=\int f(u)\,du,\;u=g(x)`

- **部分積分法**

  - :math:`\int f(x)g'(x)\,dx=f(x)g(x)-\int f'(x)g(x)\,dx`

- **分数関数の積分**

  - 分母が :math:`(ax+b)^n` ： :math:`ax+b=t` 置換
  - 分母が :math:`a(x+\alpha)(x+\beta)` ： 部分分数に分解
  - 分母が :math:`a(x+p)^2+q` ： :math:`\sqrt{\frac{a}{q}}(x+p)=\tan\theta` 置換

- **無理関数の積分**

  - :math:`\sqrt{ax+b}` ： :math:`\sqrt{ax+b}=t` 置換
  - :math:`\sqrt{x^2+A}` ： :math:`x+\sqrt{x^2+A}=t` 置換
  - :math:`\sqrt{a^2-x^2}` ： :math:`x=a\sin\theta` 置換

- **三角関数の積分**

  - 次数を１次に下げ
  - :math:`f(\sin\theta)\cos\theta` , :math:`f(\cos\theta)\sin\theta` にして置換

定積分
^^^^^^

- **偶関数・奇関数の積分**

  - :math:`f(x)` が偶関数： :math:`\int_{-a}^a f(x)\,dx=2\int_0^a f(x)\,dx`
  - :math:`f(x)` が奇関数： :math:`\int_{-a}^a f(x)\,dx=0`

- **定積分関数の微分**

  - :math:`\frac{d}{dx}\int_a^x f(t)\,dt=f(x)`
  - :math:`\frac{d}{dx}\int_{h(x)}^{g(x)}f(t)\,dt=f(g(x))g'(x)-f(h(x))h'(x)`

- **区分求積法**

  - :math:`\int_a^b f(x)\,dx=\lim_{n\to\infty}\sum_{k=0}^{n-1}f(x_k)\Delta x`
  - :math:`\int_0^1 f(x)\,dx=\lim_{n\to\infty}\frac{1}{n}\sum_{k=0}^{n-1}f\left(\frac{k}{n}\right)`

- **定積分と不定式**

  - 区間 :math:`[a, b]` で :math:`f(x)\geq g(x)` ： :math:`\int_a^b f(x)\,dx\geq\int_a^b g(x)\,dx`