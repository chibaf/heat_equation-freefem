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

![heat_arch](https://user-images.githubusercontent.com/1296728/223750511-4a4a1e41-5ce8-4e51-8c23-935aea77cdf6.jpg)

扱う問題を書きます。

$\partial_t u -\nabla\cdot(\kappa\nabla u)=0 \quad {\rm in} \Omega\times(0,T)$
