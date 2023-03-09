# heat_equation-freefem
heat equation - solving initial condition problems by freefem

FreeFemは境界値問題を解くのが基本です。初期値問題を解くには時間方向を差分で近似します。時間方向にはループコマンドを使います。

今回はfreefemに含まれる例題Heat.edpを実行してみます

Heat.edp: original program

FreeFem++ Heat.edp

<img width="1121" alt="Heat_edp" src="https://user-images.githubusercontent.com/1296728/223721328-774c7507-ecfe-4e92-af33-ec07e2991bdd.png">

ここでは領域の設定にはsquareコマンドを使っているのですが、領域の境界のラベルは以下のようになってます。

<img width="592" alt="boundary-label-heat" src="https://user-images.githubusercontent.com/1296728/223726838-1246afd8-79f7-4f5e-9b6c-85e77c261b14.png">

ここではポアソン問題で使った領域を使うことにします。: 

![heat_arch](https://user-images.githubusercontent.com/1296728/223824948-6d1a18b3-7f84-4016-a003-5972d099b1ea.jpg)

扱う問題を書きます。

$\partial_t u -\nabla\cdot(\kappa\nabla u)=0 \quad {\rm in} \Omega\times(0,T)$

$u(x,y,0)=u_0+x u_1$

$\kappa \frac{\partial u}{\partial n}+\alpha(u-u_e)=0 \quad {\rm on \,} \Gamma\times(0,T)$

$\kappa=1.8 \quad y<0.5, \quad 0.2 \quad y \ge 0.5$

FreeFEM-documentation.pdfからの引用：

Here the diffusion 𝜅 will take two values, one below the middle horizontal line and ten times less above, so as to simulate a thermostat.

The term $𝛼(𝑢 − 𝑢_𝑒)$ accounts for the loss of temperature by convection in air. Mathematically this boundary condition is of Fourier (or Robin, or mixed) type.

The variational formulation is in $𝐿^2(0,𝑇;𝐻^1(Ω))$; in loose terms and after applying an implicit Euler finite difference approximation in time; we shall seek $𝑢_𝑛(𝑥, 𝑦)$ satisfying for all $𝑤 ∈ 𝐻^1(Ω)$:

ここで $H^1(\Omega)$ は１階微分可能なソボレフ空間です。

弱形式にすると

$\displaystyle\int_\Omega \partial_t u -\displaystyle\int_\Omega \nabla\cdot(\kappa\nabla u)=0 \quad {\rm in} \Omega\times(0,T)$


部分積分をすると



時間について離散化すると：

$\displaystyle\int_\Omega \left(\frac{u^n-u^{n-1}}{\delta t}w+\kappa \nabla u^n \cdot \nabla w\right)+\int_\Gamma \alpha(u^n-u_e)w=0$

これは時間について陰的な解法で、毎回方程式を解くことになりますが、数値的に安定した計算ができます。
