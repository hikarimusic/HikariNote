数学
====

極限
----

数列の極限
^^^^^^^^^^

- **極限の基本**

  - :math:`\lim_{n\to\infty}ka_n=k\alpha\quad\lim_{n\to\infty}(a_n+b_n)=\alpha+\beta`
  - :math:`\lim_{n\to\infty}a_n b_n=\alpha\beta\quad\lim_{n\to\infty}\frac{a_n}{b_n}=\frac{\alpha}{\beta}\;(\beta\neq0)`

- **はさみうちの原理**

  - すべての :math:`n` で :math:`a_n\leq b_n`： :math:`\alpha\leq\beta`
  - すべての :math:`n` で :math:`a_n\leq c_n\leq b_n` かつ :math:`\alpha=\beta`： :math:`\lim_{n\to\infty}c_n=\alpha`

- :math:`\mathbf{n^k}` **の極限**

  - :math:`k>0` ： :math:`\lim_{n\to\infty}n^k=\infty`
  - :math:`k<0` ： :math:`\lim_{n\to\infty}n^k=0`

- :math:`\mathbf{r^n}` **の極限**

  - :math:`r>1` ： :math:`\lim_{n\to\infty}r^n=\infty`
  - :math:`r=1` ： :math:`\lim_{n\to\infty}r^n=1`
  - :math:`-1<r<1` ： :math:`\lim_{n\to\infty}r^n=0`
  - :math:`r\leq-1` ：極限がない

無限級数
^^^^^^^^

- **定義**

  - :math:`S_n=\sum_{k=1}^n a_k` が :math:`S` に収束：無限級数が :math:`S` 収束
  - :math:`S_n=\sum_{k=1}^n a_k` が発散：無限級数が発散

- **収束・発散**

  - 無限級数 :math:`\sum_{n=1}^\infty a_n` が収束： :math:`\lim_{n\to\infty}a_n=0`
  - :math:`\lim_{n\to\infty}a_n\neq0` ：無限級数 :math:`\sum_{n=1}^\infty a_n` が発散

- **無限等比級数**

  - :math:`a\neq0,\;|r|<1` ：収束、和は :math:`\frac{a}{1-r}`
  - :math:`a\neq0,\;|r|\geq1` ：発散
  - :math:`a=0` ：収束、和は :math:`0`

関数の極限
^^^^^^^^^^

- **極限の基本**

  - :math:`\lim_{x\to a}kf(x)=k\alpha\quad\lim_{x\to a}\left\{f(x)+g(x)\right\}=\alpha+\beta`
  - :math:`\lim_{x\to a}f(x)g(x)=\alpha\beta\quad\lim_{x\to a}\frac{f(x)}{g(x)}=\frac{\alpha}{\beta}\;(\beta\neq0)`

- **はさみうちの原理**

  - :math:`x` が :math:`a` に近い時 :math:`f(x)\leq g(x)`： :math:`\alpha\leq\beta`
  - :math:`x` が :math:`a` に近い時 :math:`f(x)\leq h(x)\leq g(x)` かつ :math:`\alpha=\beta`： :math:`\lim_{x\to a}h(x)=\alpha`

- **様々な極限**

  - :math:`\lim_{x\to\infty}a^x=\infty\quad\lim_{x\to -\infty}a^x=0` ( :math:`a>1` )
  - :math:`\lim_{x\to\infty}a^x=0\quad\lim_{x\to -\infty}a^x=\infty` ( :math:`0<a<1` )
  - :math:`\lim_{x\to\infty}\log_a x=\infty\quad\lim_{x\to +0}a^x=-\infty` ( :math:`a>1` )
  - :math:`\lim_{x\to\infty}\log_a x=-\infty\quad\lim_{x\to +0}\log_a x=\infty` ( :math:`0<a<1` )
  - :math:`\lim_{x\to 0}\frac{\sin x}{x}=1`
  - :math:`\lim_{h\to 0}(1+h)^{\frac{1}{h}}=e\quad\lim_{h\to 0}\frac{e^h-1}{h}=e`

- **連続性と中間値の定理**

  - :math:`f(x)` が :math:`x=a` で連続： :math:`\lim_{x\to a}f(x)=f(a)`
  - :math:`f(x)` が区間 :math:`[a, b]` で連続、 :math:`f(a)\neq f(b)` ： :math:`f(a)` と :math:`f(b)` の間の :math:`k` に対して :math:`f(c)=k,\;a<c<b` を満たす :math:`c` が存在する

微分法
------

導関数
^^^^^^

- **定義**

  - :math:`f'(x)=\lim_{h\to 0}\frac{f(x+h)-f(x)}{h}`

- **公式**

  - :math:`(x^\alpha)'=\alpha x^{\alpha-1}`
  - :math:`\left\{kf(x)\right\}'=kf'(x)\quad\left\{f(x)+g(x)\right\}'=f'(x)+g'(x)`
  - :math:`\left\{f(x)g(x)\right\}'=f'(x)g(x)+f(x)g'(x)\quad\left\{\frac{f(x)}{g(x)}\right\}'=\frac{f'(x)g(x)-f(x)g'(x)}{{g(x)}^2}`
  - :math:`\frac{dy}{dx}=\frac{dy}{du}\frac{du}{dx}`
  - :math:`\frac{dy}{dx}=\frac{1}{\frac{dx}{dy}}`

様々な導関数
^^^^^^^^^^^^

- **様々な導関数**

  - :math:`(\sin x)'=\cos x'\quad (\cos x)'=-\sin x\quad (\tan x)'=\frac{1}{\cos^2 x}`
  - :math:`(\log|x|)'=\frac{1}{x}\quad (\log_a|x|)'=\frac{1}{x\log a}`
  - :math:`(e^x)'=e^x\quad (a^x)'=a^x\log a`

- **その他**

  - 高次導関数： :math:`f^{(n)}(x)=\frac{d^n}{dx^n}f(x)`
  - 陰関数の微分： :math:`F(x, y)=0` の両辺を :math:`x` で微分する、 :math:`\frac{df(y)}{dx}=\frac{df(y)}{dy}\frac{dy}{dx}`
  - 媒介変数の微分： :math:`\frac{dy}{dx}=\frac{\frac{dy}{dt}}{\frac{dx}{dt}}`

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