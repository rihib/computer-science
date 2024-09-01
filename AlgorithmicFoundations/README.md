# Algorithmic Foundations

## 基本的なデータ構造とアルゴリズム

### ADT（抽象データ型）

ADT（Abstract Data Type）とは、具体的な実装は定義されておらず、単にどのような操作を持つかということのみが定義された抽象的なデータ型。例えば、スタックというADTはpush、popという操作を持つデータ型であるが、具体的な実装は規定されていない。他に、リスト、セット、辞書、キュー、ツリー、グラフなどもADTである。

例えばマップは、キーとそれに対応する値のペアを持ち、挿入（insert）、削除（delete）、検索（find）などの操作を持つADTだが、具体的な実装方法は規定されておらず、GoやPythonでは内部的にはハッシュテーブルを用いて実装されているのに対し、C++では平衡二分探索木（赤黒木）を用いて実装されている。

対してデータ型は、データの内部表現や操作方法がプログラミング言語やユーザーによって明確に定義されたデータ型である。例えば、int、float、charなどの基本データ（primitive）型や、配列、構造体、クラスなどの複合データ型、列挙型（Enum）などがデータ型であると言える。