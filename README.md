# 風
- 風とは、「東、南、西、北」からなる集合である。
- 風の要素に対して、「次の風」とは、以下で定義される全単射である。
    - 東の次の風は南である。
    - 南の次の風は西である。
    - 西の次の風は北である。
    - 北の次の風は南である。
- 風の要素に対して、「前の風」とは、「次の風」の逆写像である。


# 牌
## 牌の種類
- 牌は数牌と字牌からなる。
- 数牌は、色、数字、赤かどうかの属性を持つ。色は「萬、索、筒」のいずれか、数字は $1$ 以上 $9$ 以下のいずれかの整数である。
- 字牌は、字という属性をもち、それは風のうちのひとつ、あるいは「白、發、中」のいずれかである。

## 牌の枚数
- ゲームは、以下の136枚の牌を利用する。
    - 各色、5を除く数字に対し、赤でない4枚の牌 (合計 $3\times8\times4=96$ 枚)。
    - 各色に対し、数字が5である牌であって、赤であるもの1枚と、赤でないもの3枚 (合計 $3\times(1+3)=12$ 枚)。
    - 各字に対して4枚の牌 (合計 $7\times4=28$ 枚)。

## 次の牌
- 各牌に対して、「次の牌」とは以下に定義される、要素数 $4$ の牌の集合である。
    - 各数牌に対して、その色を $c$, その数を $x$ としたとき、次の牌は色が $c$ であり数が $x+1$ を $9$ で割った余りであるような数牌の集合である。
    - 各字牌に対して、その字を $x$ としたとき、次の牌は字が $f(x)$ である字牌の集合である。ただし、$f$ は以下で定義される。
        - $x$ が風に含まれるなら、$f(x)$ はその次の風である。
        - $f($白$) = $發。
        - $f($發$) = $中。
        - $f($中$) = $白。


# 試合の進行
## プレイヤー
- $4$ 人のプレイヤーが参加する。

## 試合の状態
- 試合は以下の状態を持つ。
    - 供託点数（整数）。初期状態は $0$。
    - 場風（東あるいは南）。初期状態は東。
    - 局数（$1$ 以上 $4$ 以下の整数）。初期状態は $1$。
    - 本場（非負整数）。初期状態は $0$。
    - さらに、各プレイヤーが以下の状態を持つ。
        - 点数（整数）。初期状態は $25000$。
        - 自風（風）。
            - $4$ 人のプレイヤーの自風は常に相異なる。
            - 自風が東であるプレイヤーを「親」と呼ぶ。
- 各局は以下の状態を持つ。初期状態は「局の開始」で定義される。
    - 牌山（牌の列）。
    - 槓の回数（$0$ 以上 $4$ 以下の整数）。
    - 手番のプレイヤー（プレイヤー）。
    - さらに、各プレイヤーは以下の状態を持つ。
        - 手牌（牌の集合）。
        - 副露牌（牌の集合の集合）。
        - 暗槓牌（牌の集合の集合）。
        - 捨て牌（牌の集合）。
        - リーチしているかどうか（真偽）。

## 次のプレイヤー
- 各プレイヤーに対して、その次のプレイヤーとは、そのプレイヤーの自風の次の風を自風にもつプレイヤーである。

## プレイヤーの知識
- 局の開始から終了までの間、各プレイヤーは以下の情報を知らない。
    - 自分以外のプレイヤーの手牌。
    - 牌山のうち、以下の牌を除くもの。
        - 現在の槓の回数を $x$ としたとき、最後から $6-x, 6-x+2, \cdots, 6+2x$ 番目 (1-indexed) のもの。


# 局
## 局の開始
- 牌山を全ての牌からなり、順序が一様ランダムな列とする。
- 状態を以下のように初期化する。
    - 槓の回数を $0$ とする。
    - 手番のプレイヤーを親とする。
    - 各プレイヤーについて、手牌、副露牌、暗槓牌、捨て牌を空集合とし、リーチしているかどうかを偽とする。
- 各プレイヤーが以下の操作を行う。
    - 牌山の先頭から $13$ 枚を引き、それを自分の手牌に加える。
- 「自摸操作」を開始する。

## 自摸操作
- 手番のプレイヤーが、牌山の先頭から $1$ 枚を引き、これを「自摸牌」として「牌を自摸ったときの操作」を行う。

## 牌を自摸ったときの操作
- この操作は「自摸牌」を引数に取る。
- 手番のプレイヤー（以下「自分」と呼ぶ）が、以下の操作を選ぶ。
    - 自摸牌を自分の捨て牌に加える。
        - このとき、「リーチ前かどうか」を偽として、「牌が捨てられたときの操作」を実行する。
    - 自摸牌を自分の手牌に加えた後、自分の手牌から $1$ 枚を選んで取り除き、自分の捨て牌に加える。
        - これは、自分の「リーチしているかどうか」が偽であるときに実行可能である。
        - このとき、「リーチ前かどうか」を偽として、「牌が捨てられたときの操作」を実行する。
    - ツモを宣言する。
        - これは、以下の条件が満たされるときに実行可能である。
            - 自分の手牌に自摸牌を加えたときに、役ありの和了可能である。
        - このとき、「和了者」を自分、「和了牌」を自摸牌として、「放銃者」を空集合として、「和了時の操作」を実行する。
    - リーチを宣言する。
        - これは、以下の条件が満たされるときに実行可能である。
            - 牌山が $15$ 枚以上ある。
            - 副露牌が空集合である。
            - 自分の手牌、あるいは自摸牌の中に、以下の条件を満たすものが存在する。これを捨てる牌と呼ぶ。
                - 手牌を、現在の手牌と自摸牌から除いたときに、自分が聴牌である。
        - このとき、以下の操作を実行する。
            - 自分の手牌に自摸牌を加える。
            - 自分の手牌から捨てる牌を除き、自分の捨て牌に加える。
            - 「リーチ前かどうか」を真として、「牌が捨てられたときの操作」を実行する。
    - カンを宣言する。
        - これは、以下の条件が満たされるときに実行可能である。
            - 牌山が $15$ 枚以上ある。
            - 以下のいずれかを満たす。これを副露材と呼ぶ。
                - 自分の手牌の中に含まれる $3$ 枚の牌であって、自摸牌を加えると槓子となるものが存在する。（これを暗槓の場合と呼ぶ。）
                - 自分の副露牌の中に含まれる刻子であって、自摸牌を加えると槓子となるものが存在する。（これを加槓の場合と呼ぶ。）
            - 自分がリーチをしていないか、以下の条件を満たす。
                - 自分の手牌のすべての聴牌形に関して、その4枚を槓子にしても和了可能である。(TODO)
            - 槓の回数が $4$ 未満である。
        - このとき、以下の操作を実行する。
            - 以下のどちらかをする。
                - 暗槓の場合、副露材を手牌から除き、副露材に自摸牌を加えたものを暗槓牌に加える。
                - 加槓の場合、副露材に自摸牌を加える。
            - 加槓の場合、以下の操作を行う。
                - 自分以外の各プレイヤーが独立かつ同時に、以下の操作から一つを選ぶ。
                    - 何も宣言しない。
                    - ロンを宣言する。
                        - これは、「捨て牌」を自摸牌として、「ロンが可能な条件」が満たされるときに実行可能である。
                - $1$ 人以上のプレイヤーがロンを宣言したなら、「ロンが宣言されたときの操作」を行う。（局は終了し、以後の手順は実行されない。）
            - 槓の回数に $1$ を加える。
            - 牌山の末尾から $1$ 枚を引き、「自摸牌」をこの牌として「牌を自摸ったときの操作」を行う。

## 牌が捨てられたときの操作
- この操作は「リーチ前かどうか」を引数に取る。
- 手番のプレイヤー以外の各プレイヤーが独立かつ同時に、以下の操作から一つを選ぶ。
    - 何も宣言しない。
    - ロンを宣言する。
        - これは、「捨て牌」を最後に捨てられた牌として、「ロンが可能な条件」が満たされるときに実行可能である。
    - チーを宣言する。
        - これは、以下の条件が満たされるときに実行可能である。
            - 牌山が $15$ 枚以上ある。
            - 自分のプレイヤーは手番のプレイヤーの次のプレイヤーである。
            - 自分の手牌に含まれる $2$ 枚の牌であって、自摸牌を加えると順子となるものが存在する。これを副露材と呼ぶ。
            - 自分の手牌に含まれるが副露する手牌に含まれない牌であって、副露する手牌に加えると順子とならないものが存在する。これを代わりに捨てる牌と呼ぶ。
        - これを宣言したプレイヤー（以下「鳴いたプレイヤー」）が、鳴きを成立させるとは、以下の操作を行うこととする。
            - 副露材に最後の捨て牌を加えたものを、副露牌に加える。
            - 代わりに捨てる牌を自分の手牌から取り除き、捨て牌に加える。
            - 「リーチ前かどうか」を偽として、「牌が捨てられたときの操作」を実行する。
    - ポンを宣言する。
        - これは、以下の条件が満たされるときに実行可能である。
            - 牌山が $15$ 枚以上ある。
            - 自分の手牌の中に含まれる $2$ 枚の牌であって、自摸牌を加えると刻子となるものが存在する。これを副露材と呼ぶ。
            - 自分の手牌に含まれるが副露する手牌に含まれない牌であって、副露する手牌に加えると刻子とならないものが存在する。これを代わりに捨てる牌と呼ぶ。
        - これが成立した場合、これを宣言したプレイヤーは以下の操作を行う。
            - 副露材に最後の捨て牌を加えたものを、副露牌に加える。
            - 代わりに捨てる牌を自分の手牌から取り除き、捨て牌に加える。
            - 「リーチ前かどうか」を偽として、「牌が捨てられたときの操作」を実行する。
    - カンを宣言する。
        - これは、以下の条件が満たされるときに実行可能である。
            - 牌山が $15$ 枚以上ある。
            - 自分の手牌の中に含まれる $3$ 枚の牌であって、自摸牌を加えると槓子となるものが存在する。これを副露材と呼ぶ。
        - これが成立した場合、これを宣言したプレイヤーは以下の操作を行う。
            - 副露材に最後の捨て牌を加えたものを、副露牌に加える。
            - 槓の回数に $1$ を加える。
            - 牌山の末尾から $1$ 枚を引き、「自摸牌」をこの牌として「牌を自摸ったときの操作」を行う。
- 以下の処理を行う。
    - $1$ 人以上のプレイヤーがロンを宣言したなら、「ロンが宣言されたときの操作」を行う。（局は終了し、以後の手順は実行されない。）
    - 牌山の枚数がちょうど $14$ 枚なら、「流局時の操作」を行う。（局は終了し、以後の手順は実行されない。）
    - 「リーチ前かどうか」が真ならば、以下の操作を行う。
        - 手番のプレイヤーの「リーチしているかどうか」を真とする。
        - 手番のプレイヤーから $1000$ を減算し、供託点数に $1000$ を加算する。
    - 以下を実行する。
        - いずれかのプレイヤーがポンあるいはカンを宣言したなら、そのプレイヤーが鳴きを成立させる。
        - それ以外の場合、いずれかのプレイヤーがチーを宣言したなら、そのプレイヤーが鳴きを成立させる。
        - それ以外の場合、手番のプレイヤーを現在の手番のプレイヤーの次のプレイヤーとし、「自摸操作」を行う。

## ロンが可能な条件
- この条件は「捨て牌」を引数に取る。
- ロンが可能な条件は、以下のすべてが満たされることである。
    - 自分の手牌に最後の捨て牌を加えたときに、役ありの和了可能である。
    - 自分の捨て牌のどの牌も、自分の待ち牌に含まれない。
    - 最後に自分が牌を捨ててから（もしそれが存在しなければ、ゲーム開始時点から）任意のプレイヤーが捨てたどの牌も、自分の待ち牌に含まれない。

## ロンが宣言されたときの操作
- ロンを宣言したプレイヤーのうち、以下のリスト内で最初に現れるプレイヤーを、和了したプレイヤーとする。
    1. 手番のプレイヤーの次のプレイヤー。
    2. その次のプレイヤー。
    3. その次のプレイヤー。
- このとき、「和了者」を和了したプレイヤー、「和了牌」を最後の捨て牌、「放銃者」を手番のプレイヤーとして、「和了時の操作」を実行する。

## 和了ったときの操作
- この操作は、「和了者」（プレイヤー）、「和了牌」（牌）、「放銃者」（プレイヤーまたは空集合）を引数に取る。
- 和了者の手と和了牌が満たすすべての手牌型のうち、基本点の最大値を $x$ とする。
- 以下を実行する。
    - 放銃者が空集合なら、和了者以外の各プレイヤー $p$ に対して同時に、以下を実行する。
        - 整数 $m$ を、和了者か $p$ のいずれかが親であるなら $2$、それ以外の場合は $1$ とする。
        - 整数 $y \coloneqq \operatorname{ceil}_{100}(mx)$ とする。
        - $p$ の点数から $y$ を減算し、和了者の点数に $y$ を加算する。
    - それ以外の場合、以下を実行する。
        - 整数 $m$ を、和了者が親であるなら $6$、それ以外の場合は $4$ とする。
        - 整数 $y \coloneqq \operatorname{ceil}_{100}(mx)$ とする。
        - 放銃者の点数から $y$ を減算し、和了者の点数に $y$ を加算する。
- 和了者の点数に供託点数を加算し、供託点数を $0$ とする。
- 以下を実行する。
    - 和了者が親である場合は、以下を実行する。
        - 本場に $1$ を足す。
    - それ以外の場合は、以下を実行する。
        - 「親を進める操作」を行う。
        - 本場を $0$ とする。
- 「局の開始」を行う。

## 流局時の操作
- 東、南、西、北の順に、以下の操作を行う。
    - その風を自風にもつプレイヤーが、以下のいずれかを選択する。
        - テンパイを宣言する。
            - これは、そのプレイヤーの手牌が聴牌形であるときに実行可能である。
            - このとき、そのプレイヤーの手牌を、全プレイヤーに公開する。
        - ノーテンを宣言する。
- テンパイを宣言した人の人数を $n$、ノーテンを宣言した人の人数を $m$ とする。
    - もし $n\neq0$ かつ $m\neq0$ なら、各プレイヤーに対して同時に以下を行う。
        - テンパイを宣言した場合、そのプレイヤーの点数に $3000/n$ を加算する。
        - ノーテンを宣言した場合、そのプレイヤーの点数から $3000/m$ を減算する。
- 親がノーテンを宣言した場合、「局を進める操作」を実行する。
- 本場に $1$ を足す。
- 「局の開始」を行う。

## 親を進める操作
- 以下の操作を行う。
    - 局数が $4$ である場合は、以下の操作を行う。
        - 場風が南である場合は、試合を終了する。
        - 場風が東である場合は、場風を南として、局数を $1$ とする。
    - 局数が $3$ 以下である場合は、局数に $1$ を足す。
- 各プレイヤーに対して同時に、自風を前の風とする。


# 手牌形
