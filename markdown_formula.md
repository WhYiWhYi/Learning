## 1 公式标记
* 行内公式：```$ $```
* 整行公式：```$$ $$```
## 2 希腊字母
|名称|大写|输入|小写|输入|名称|大写|输入|小写|输入|
|---|---|---|---|---|---|---|---|---|---|
|alpha|$A$|A|$\alpha$|\alpha|nu|$N$|N|$\nu$|\nu|
|beta|$B$|B|$\beta$|\beta|xi|$\Xi$|Xi|$\xi$|xi|
|gamma|$\Gamma$|\Gamma|$\gamma$|\gamma|omicron|$O$|O|$\omicron$|\omicron|
|delta|$\Delta$|\Delta|$\delta$|\delta|pi|$\Pi$|\Pi|$\pi$|\pi|
|epsilon|$E$|E|$\epsilon$|\epsilon|rho|$P$|P|$\rho$|\rho|
|zeta|$Z$|Z|$\zeta$|\zeta|sigma|$\Sigma$|\Sigma|$\sigma$|\sigma|
|eta|$H$|H|$\eta$|\eta|tau|$T$|T|$\tau$|\tau|
|theta|$\Theta$|\Theta|$\theta$|\theta|upsilon|$\Upsilon$|\Upsilon|$\upsilon$|\upsilon|
|iota|$I$|I|$\iota$|\iota|phi|$\Phi$|\Phi|$\phi$|\phi|
|kappa|$K$|K|$\kappa$|\kappa|chi|$X$|X|$\chi$|\chi|
|lambda|$\Lambda$|\Lambda|$\lambda$|\lambda|psi|$\Psi$|\Psi|$\psi$|\psi|
|mu|$M$|M|$\mu$|\mu|omega|$\Omega$|\Omega|$\omega$|\omega|
## 3 上标与下标
* 上标：```^```，如 ```x_i^2``` ： $x_i^2$
* 下标：```_```
* 多字符（>1）上下标则需要使用 ```{}``` 进行**分组标记**，如 ```$10^{10}$```： $10^{10}$ 
* 左右均有上下标则可以使用 ```\sideset```，如 ```$\sideset{^1_2}{^3_4}\bigotimes$```：$\sideset{^1_2}{^3_4}\bigotimes$
## 4 括号
* 小括号：```()```
* 方括号：```[]```
* 大括号：```\{\}```，或可使用 ```\lbrace \rbrace```
* 尖括号：```\langle \rangle```
* 向上取整：```\lceil \rceil```
* 向下取整：```\lfloor \rfloor```
* 自适应括号：```\left(...\right)```，如 ```\left(\frac12\right)```：$\left( \frac12 \right)$
## 5 求和与积分
* 求和：```\sum```，其下标和上标分别表示求和的下限和上限，如 ```\sum_1^n```：$\sum_1^n$
* 积分：```\int```、```\iint```，其下标和上标分别表示积分的下限和上限，如 ```\int_1^\infty```：$\int_1^\infty$
* 类似的符号还有 ```\prod```：$\prod$、```\bigcup```：$\bigcup$、```bigcap```：$\bigcap$
## 6 分式与根式
* 分式：```\frac a b```，如 ```\frac {a+1}{b+1}```：$\frac {a+1}{b+1}$
* 根式：```\sqrt[a]b```，方括号内的值表示开几次方，省略方括号即为开平方，如```\sqrt[4]{\frac xy}```：$\sqrt[4]{\frac xy}$
## 7 特殊运算符
* 关系运算符
	|显示|输入|显示|输入|显示|输入|显示|输入|
	|---|---|---|---|---|---|---|---|
	|$\pm$|\pm|$\mp$|\mp|$\times$|\times|$\div$|\div|
	|$\mid$|\mid|$\nmid$|\nmid|$\circ$|\circ|$\bullet$|\bullet|
	|$\cdot$|\cdot|$\ast$|\ast|$\odot$|\odot|$\bigodot$|\bigodot|
	|$\otimes$|\otimes|$\bigotimes$|\bigotimes|$\oplus$|\oplus|$bigoplus$|\bigopuls|
	|$\lt$|\lt|$\gt$|\gt|$\leq$|\leq|$\geq$|\geq|
	|$\neq$|\neq|$\approx$|\approx|$\equiv$|\equiv|$\sim$|\sim|
	|$\simeq$|\simeq|$\cong$|\cong|$\prec$|\prec|$\lhd$|\lhd|
	|$\sum$|\sum|$\prod$|\prod|$\coprod$|\coprod|
* 集合运算符
	|显示|输入|显示|输入|显示|输入|显示|输入|
	|---|---|---|---|---|---|---|---|
	|$\emptyset$|\emptyset|$\varnothing$|\varnothing|$\in$|\in|$\notin$|\notin|
	|$\subset$|\subset|$\supset$|\supset|$\cup$|\cup|$\cap$|\cap|
	|$\subseteq$|\subseteq|$\supseteq$|\supseteq|$\subsetneq$|\subsetneq|$\supsetneq$|\supsetneq|
	|$\vee$|\vee|$\wedge$|\wedge|
	|$\bigcup$|\bigcup|$\bigcap$|\bigcap|$\bigvee$|\bigvee|$\bigwedge$|\bigwedge|
	|$\uplus$|\uplus|$\biguplus$|\biguplus|$\sqcup$|\sqcup|$\bigsqcup$|\bigsqcup|
* 对数运算符
	|显示|输入|显示|输入|显示|输入|
	|---|---|---|---|---|---|
	|$\log$|\log|$\lg$|\lg|$\ln$|\ln|
* 三角运算符
	|显示|输入|显示|输入|显示|输入|
	|---|---|---|---|---|---|
	|$\bog$|\bog|$\angle$|\angle|$30^\circ$|30^\circ|
	|$\sin$|\sin|$\cos$|\cos|$\tan$|\tan|
	|$\cot$|\sec|$\sec$|\sec|$\csc$|\csc|
* 微积分运算符
	|显示|输入|显示|输入|显示|输入|
	|---|---|---|---|---|---|
	|$\prime$|\prime|$\int$|\int|$\iint$|\iint|
	|$\iiint$|\iiint|$\iiiint$|\iiiint|$\oint$|\oint|
	|$\lim$|\lim|$\infty$|\infty|$\nabla$|\nabla|
* 逻辑运算符
	|显示|输入|显示|输入|显示|输入|显示|输入|
	|---|---|---|---|---|---|---|---|
	|$\because$|\because|$\therefore$|\therefore|$\forall$|\forall|$\exists$|\exists|
	|$\not=$|\not=|$\lnot$|\lnot|$\vdash$|\vdash|$\vDash$|\vDash|
	|$\land$|\land|$\lor$|\lor|$\top$|\top|$\bot$|\bot|
* 箭头符号
	|显示|输入|显示|输入|显示|输入|显示|输入|
	|---|---|---|---|---|---|---|---|
	|$\uparrow$|\uparrow|$\downarrow$|\downarrow|$\rightarrow(\to)$|\rightarrow(\to)|$\leftarrow$|\leftarrow|
	|$\Uparrow$|\Uparrow|$\Downarrow$|\Downarrow|$\Rightarrow$|\Rightarrow|$\Leftarrow$|\Leftarrow|
	|$\longrightarrow$|\longrightarrow|$\longleftarrow$|\longleftarrow|$\Longrightarrow$|\Longrightarrow|$\mapsto$|\mapsto|
* 排列：使用 ```{n+1 \choose 2k}``` 或 ```\binom{n+1}{2k}```：$\binom{n+1}{2k}$
* 模运算：使用 ```\pmod```，如 ```a\equiv b\pmod n```：$a\equiv b\pmod n$
* 省略号：```\ldots``` 或 ```\codts```。前者的dots位置稍低，cdots的位置稍居中
## 8 顶部/底部符号
* 连线符号（顶部或底部）：```\overline``` 或 ```\underline```，如 ```\overline{a+b+c+d}```：$\overline{a+b+c+d}$，或 ```\underline{a+b+c+d}```：$\underline{a+b+c+d)}$
* 连线符号（波浪符号）：```\widetilde```，如```\widetilde{xyz}```：$\widetilde{xyz}$
* 上大括号：```\overbrace{算式}```，如 ```\overbrace{a+b+c+d}^{2.0}```：$\overbrace{a+b+c+d}^{2.0}$
* 下大括号：```\underbrace{算式}```，如 ```a+\underbrace{b+c}_{1.0_\text{times}}+d```：$a+\underbrace{b+c}_{1.0_\text{times}}+d$
* 常用顶部符号：单字符 ```\hat x```：$\hat x$，多字符 ```\widehat {xy}```：$\widehat {xy}$
* 类似的符号还有：```\check x```：$\check x$、```\breve x```：$\breve x$、```\bar x```：$\bar x$、```\vec x```：$\vec x$、```\overrightarrow x```：$\overrightarrow x$、```\underleftarrow x```：$\underleftarrow x$、```\overleftrightarrow {xyz}```：$\overleftrightarrow {xyz}$、```\dot x```：$\dot x$、```\ddot x```：$\ddot x$
## 9 空格/占位符
在书写公式的时候，无论a和b之间无论输入多少空格，最后都会贴在一起，因此需要以特殊字符产生空格
* 小空格：```\,```，如 ```x \, y```：$x \, y$
* 中空格：```\:```，如 ```x \: y```：$x \: y$
* 大空格：```\```，如 ```x \ y```：$x \ y$
* 紧贴：```\!```，如 ```x \! y```：$x \! y$
* quad空格：```\quad```，如 ```x \quad y```：$x \quad y$
* 两个quad空格：```\qquad```，如 ```x \qquad y```：$x \qquad y$
## 10 字体
* ```\it``` (意大利体，公式默认字体)：$\it{ABCDEFGHIJKLMN}$
* ```\mathbb``` （黑板粗体）：$\mathbb{ABCDEFGHIJKLMN}$
* ```\mathbf``` （黑体）：$\mathbf{ABCDEFGHIJKLMN}$
* ```\mathtt``` （打印机字体）：$\mathtt{ABCDEFGHIJKLMN}$
* ```\mathrm``` （罗马体）：$\mathrm{ABCDEFGHIJKLMN}$
* ```\mathsf``` （等线体）：$\mathsf{ABCDEFGHIJKLMN}$
* ```\mathcal``` （艺术字体）：$\mathcal{ABCDEFGHIJKLMN}$
* ```\mathscr``` （手写花体）：$\mathscr{ABCDEFGHIJKLMN}$
* ```\mathfrak``` （Fraktur字体，老式德国字体）$\mathfrak{ABCDEFGHIJKLMN}$
* ```\mit``` （数学斜体）：$\mit{1234567890}$
* 字体控制：```\displaystyle``` 可使字体变大，如```\displaystyle \frac{x+y}{y+z}```：$\displaystyle \frac{x+y}{y+z}$
## 11 其他
* 矩阵

	使用 ```$$\begin{matrix}...\end{matrix}$$```的形式来表示矩阵，在 ```\begin``` 和 ```\end``` 之间加入矩阵中的元素即可。矩阵的行之间使用 ```\\``` 分隔，列之间使用 ```&``` 分隔
	$$
	\begin{matrix}
	1 & x & x^2 \\
	1 & y & y^2 \\
	1 & z & z^z \\
	\end{matrix}
	$$
	加括号：将 ```{matrix}``` 替换为 ```{pmatrix}```（圆括号）、```{bmatrix}```（方括号）、```{Bmatrix}```（大括号）、```{vmatrix}```（竖线）、```{Vmatrix}```（双竖线）
	$$
	\begin{Vmatrix}
	1 & x & x^2 \\
	1 & y & y^2 \\
	1 & z & z^z \\
	\end{Vmatrix}
	$$
	省略元素：可使用 ```\cdots```：$\cdots$、```\ddots```：$\ddots$、```\vdots```：$\vdots$ 来省略矩阵中的元素
* 分类表达式
	使用 ```$$\begin{cases}...\end{cases}$$```的形式来表示分类表达式。表达式中使用 ```\``` 来分类，使用 ```&``` 指示需要对齐的位置（从符号位置处开始对齐）
	$$
	f(n)=
	\begin{cases}
	n/2,&\text{if $n$ is even}\\
	3n+1,&\text{if $n$ is odd}
	\end{cases}
	$$
	如果想将分类之间的垂直间隔增大，可以将行末的 ```\``` 替换为 ```\\[2ex]```（1ex为原始距离，其余可取值包括3ex、4ex）
* 公式对齐
	使一系列公式中的等号对齐，可使用 ```$$\begin{align}...\end{align}$$```。表达式中 ```&```指示对齐的位置
	$$
	\begin{align}
	\sqrt{37}&=\sqrt{\frac{73^2-1}{12^2}}\\
	&=\sqrt{\frac{73^2}{12^2}\cdot\frac{73^2-1}{73^2}}\\
	&=\frac{73}{12}\sqrt{1-\frac{1}{73^2}}\\
	&\approx\frac{73}{12}\left(1-\frac{1}{2\cdot73^2}\right)
	\end{align}
	$$

