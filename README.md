# heat_equation-freefem
heat equation - solving initial condition problems by freefem

FreeFemは境界値問題を解くのが基本です。初期値問題を解くには時間方向を差分で近似します。時間方向にはループコマンドを使います。

今回はfreefemに含まれる例題Heat.edpを実行してみます

Heat.edp: original program

FreeFem++ Heat.edp

<img width="1121" alt="Heat_edp" src="https://user-images.githubusercontent.com/1296728/223721328-774c7507-ecfe-4e92-af33-ec07e2991bdd.png">

ここでは領域の設定にはsquareコマンドを使っているのですが、領域の境界のラベルは以下のようになってます。

<img width="592" alt="boundary-label-heat" src="https://user-images.githubusercontent.com/1296728/223726838-1246afd8-79f7-4f5e-9b6c-85e77c261b14.png">

扱う問題を書きます。

$\partial_t u -\nabla\cdot(\kappa\nabla u)=0 \quad {\rm in}\hspace{1pt} \Omega\times(0,T)$

$u(x,y,0)=u_0+x u_1$

$\kappa \frac{\partial u}{\partial n}+\alpha(u-u_e)=0 \quad {\rm on} \hspace{1pt} \Gamma\times(0,T)$

$\kappa=1.8 \quad y<0.5, \quad 0.2 \quad y \ge 0.5$

FreeFEM-documentation.pdfからの引用：

Here the diffusion 𝜅 will take two values, one below the middle horizontal line and ten times less above, so as to simulate a thermostat.

The term $𝛼(𝑢 − 𝑢_𝑒)$ accounts for the loss of temperature by convection in air. Mathematically this boundary condition is of Fourier (or Robin, or mixed) type.

The variational formulation is in $𝐿^2(0,𝑇;𝐻^1(Ω))$; in loose terms and after applying an implicit Euler finite difference approximation in time; we shall seek $𝑢_𝑛(𝑥, 𝑦)$ satisfying for all $𝑤 ∈ 𝐻^1(Ω)$:

ここで $H^1(\Omega)$ は１階微分可能なソボレフ空間です。
$𝐿^2(0,𝑇;𝐻^1(Ω))$ は $t\in[0,T]$ に $u\in H^1(\Omega)$ を対応させる写像の空間です。

弱形式にすると

$\displaystyle\int_\Omega \partial_t u w -\displaystyle\int_\Omega \nabla\cdot(\kappa\nabla u) w=0 \quad {\rm in} \Omega\times(0,T)$

部分積分をすると

$\displaystyle\int_\Omega \partial_t u w -\displaystyle\int_\Gamma \kappa\frac{\partial}{\partial n} u w+\displaystyle\int_\Omega \kappa\nabla u\cdot\nabla w=0 \quad {\rm in} \Omega\times(0,T)$

$\displaystyle\int_\Omega \partial_t u w +\displaystyle\int_\Gamma \alpha(u-u_e) w+\displaystyle\int_\Omega \kappa\nabla u\cdot\nabla w=0 \quad {\rm in} \Omega\times(0,T)$

時間について離散化すると：

$\displaystyle\int_\Omega \left(\frac{u^n-u^{n-1}}{\delta t}w+\kappa \nabla u^n \cdot \nabla w\right)+\int_\Gamma \alpha(u^n-u_e)w=0$

これは時間について陰的な解法で、毎回方程式を解くことになりますが、数値的に安定した計算ができます。

# Heat.edpの説明

この問題を解くためのFreeFemコードHeat.edpを説明します

この部分はパラメータの設定です

<img width="332" alt="Heat-edp-1" src="https://user-images.githubusercontent.com/1296728/224743390-3339f833-7463-46e4-9729-695edd33774c.png">

特に、func k = 1.8*(y < 0.5) + 0.2は関数をあらわします。(y<0.5)は命題関数で、y<0.5が真の時1で、偽のとき０です。

<img width="325" alt="mesh finite-element-space" src="https://user-images.githubusercontent.com/1296728/224755201-16c7e487-1d0e-4528-90ff-6fadd7f88681.png">

この部分はメッシュの定義と有限要素空間の設定です。

square(150, 50, [3*x, y])の[3*x, y]はオプションで、これを入れないと正方形の領域になります。これを入れると、ｘ方向に３倍されます。150,50はそれぞれの方向の境界の分割数です。この命令を実行すると、図のようにメッシュが自動生成されます。(mesh-heat-freefem.edp)

<img width="1121" alt="mesh-heat-freefem" src="https://user-images.githubusercontent.com/1296728/224756737-0644f101-ee10-4b4b-897e-24d78ea36968.png">

このメッシュの上で有限要素空間と有限要素関数を設定します。ここではP1要素＝区分一次関数を基底とする有限要素空間を考えます。　

<img width="202" alt="finite-element-space-and-finite-element-functions" src="https://user-images.githubusercontent.com/1296728/224770356-4e45336d-959b-4b76-930a-9dff3a71000e.png">

