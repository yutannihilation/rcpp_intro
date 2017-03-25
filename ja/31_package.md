# Rcppを用いたパッケージ作成

このセクションでは自作のパッケージで Rcpp を利用する方法を解説します。

http://dirk.eddelbuettel.com/code/rcpp/Rcpp-package.pdf


通常のユーザーはパッケージを作成する必要はないと考えるかもしれません。しかし、Rcpp で書いた関数はパッケージとして保存しておくことを薦めます。なぜなら、パッケージ化しないでRcppで作成した関数を利用する場合には、毎回 `sourceCppRcpp` でコンパイルしてRにロードする必要があります。それに対して、パッケージ化すると、コンパイル済みのRcpp関数を保存しておき、使うときには `library()` でロードするだけで済むようになるためです。

#　Rcppを用いたパッケージ作成の手順


## 手動で自作パッケージを Rcpp に対応させる

自作パッケージを手動で設定する方法を以下に示します。（作成しているパッケージの名前を `myPackage` とします。）

* src/ フォルダを作成する

C/C++ のソース・ファイルはこのフォルダの中に配置します。

* `DESCRIPTION`ファイルに以下の記述を追加する

```
LinkingTo: Rcpp
Imports: Rcpp
```

`LinkingTo:` は、ここで指定したパッケージが提供するC/C++のヘッダファイルを、自作パッケージから利用できるようにします。

`Imports:` は、ここで指定したパッケージが提供するRの関数を、この自作パッケージから利用可能にします。また、この自作パッケージがここで指定したパッケージに依存していることを明示します。

(`Depends:` も、ここで指定したパッケージが提供するRの関数を、この自作パッケージから利用可能にします。Importとの違いは、この自作パッケージをRにlibrary()でロードしたときに、ここで指定したパッケージもlibrary()でロードされるということ。)


* `NAMESPACE`ファイルに以下の記述を追加する

```
useDynLib(myPackage)
exportPattern("^[[:alpha:]]+")
importFrom(Rcpp, sourceCpp)
```

`useDynLib(myPackage)`は、この自作パッケージ自身に含まれるコンパイルされた関数を、この自作パッケージ自身の R 関数から利用可能にします。（このパッケージのコンパイルされた関数のポインタ（アドレス）がこのパッケージのRの関数からアクセスできるようにする？）

`exportPattern("^[[:alpha:]]+")` は、このパッケージで定義された R の関数のうち、どれをパッケージのユーザーから利用できるようにするか指定しています。ここでは正規表現を使って全ての関数を指定しています。（Rcppを使うと `// [[Rcpp::export]]` を付けて定義した C++ の関数と同名の R の関数が自動で作成される。それを `exportPattern("^[[:alpha:]]+")` によって、全てをRから利用可能にしている）。ちなみに `export(foo, bar)` とすると定義されたRの関数 `foo` と `bar` のみがユーザーから利用可能になります。

`importFrom(Rcpp, sourceCpp)` は、このパッケージ内の R の関数から、Rcpp パッケージの `sourceCpp` 関数を `::` なしで呼び出し可能にしています。（このパッケージ内で `sourceCpp` と書くと `Rcpp::sourceCpp` と解釈される ）。ちなみに `import(foo, bar)` と書くと、このパッケージの内部でパッケージ `foo` と `bar` の全ての関数が `::` なしで呼び出し可能になります。これはこのパッケージの中で `library()` を使ったのと同じ効果を持ちます。しかし `import()` を使って他のパッケージの関数を自作パッケージ内から利用可能にすることはできる限り避けた方が良いでしょう。その理由は２つあって、１つは他のパッケージと関数名の衝突が起こる恐れがあること、もう１つは、利用している関数のどれがどのパッケージの物なのかわかりにくくなるためです。

自作パッケージの中で他のパッケージの関数を利用する場合は、 `DESCRIPTION` ファイルの `Imports:` フィールドに他のパッケージを指定し、自作関数内では `パッケージ名::関数名` の形式で利用するのが良いでしょう。どうしても必要なもの（例えば `dplyr::"%>%"` など）だけ `NAMESPACE` ファイルで `importfrom(dplyr, "%>%")` で加えましょう。




## Rcpp.package.skeleton() を利用する

Rcppを用いたパッケージを新規に作成する際に、最も簡単な方法は `Rcpp::Rcpp.package.skeleton()` を用いる方法です。

```
Rcpp.package.skeleton(
  # パッケージ名
  name = "anRpackage",
  # パッケージに含めたいオブジェクトの名前（文字列ベクトルで指定）
  list = character(),
  # 引数 list で指定したオブジェクトを探す環境
  environment = .GlobalEnv,
  # 作成するパッケージのフォルダの保存先
  path = ".",
  # TRUEなら既存のパッケージのフォルダを上書きする
  force = FALSE,
  # パッケージに含めたいコードが書かれたRファイルへのパスを指定する
  code_files = character(),
  # パッケージに含めたいコードが書かれたC++ファイルへのパスを指定する
  cpp_files = character(),
  # TRUEならRcppを用いたC++コード例をパッケージに追加する
	example_code = TRUE,
  # TRUEならパッケージに含めるC++コード例は Rcpp attributes を利用する
  attributes = TRUE,
  # TRUEなら作成するパッケージの雛形にModuleの例を含める
  module = FALSE,
  # パッケージの著者
	author = "Your Name"
  # パッケージのメンテナー
	maintainer = if(missing( author)) "Your Name" else author,
  # パッケージのメンテナーのメールアドレス
	email = "your@email.com",
  # パッケージのライセンス
	license = "GPL (>= 2)"
	)
```

使用例

```
Rcpp::Rcpp.package.skeleton("myPackage")
```

上のコードを実行すると myRcppPackage というパッケージのフォルダが作成されます。


## devtools::use_rcpp() を用いる

既存の自作パッケージに Rcpp を利用した関数を追加したい場合には、`devtools::use_rcpp()` を利用する方法もあります。

```
use_rcpp(
  # パッケージのフォルダへのパスを指定する
  pkg = "." 
  )
```

次の例では、最初に `package.skeleton()` でパッケージの雛形を作成してから、`devtools::use_rcpp()` でそのパッケージをRcppを利用するために必要な設定を行っています。

```
> package.skeleton("myPackage")
Creating directories ...
Creating DESCRIPTION ...
Creating NAMESPACE ...
Creating Read-and-delete-me ...
Saving functions and data ...
Making help files ...
Done.
Further steps are described in './myPackage/Read-and-delete-me'.

> devtools::use_rcpp("myPackage")
Adding Rcpp to LinkingTo and Imports
* Creating `src/`.
* Ignoring generated binary files.
Next, include the following roxygen tags somewhere in your package:

#' @useDynLib myPackage
#' @importFrom Rcpp sourceCpp
NULL

Then run document()
```

この方法を用いた場合には、`NAMESPACE` には設定が追加されません。そこで、このパッケージのRコードのどこかに次の記述を追加してから `devtools::document()`を実行します。
詳しくは次のセクションを参照ください。

```
#' @useDynLib myPackage
#' @importFrom Rcpp sourceCpp
NULL
```


# roxygen2 パッケージを用いてパッケージのパッケージのドキュメントを作成する

RコードやC++コードの中にroxygenタグと呼ばれる特殊な文字列を記述することで、パッケージ関数のヘルプやNAMESPACEファイルの内容を生成させることができます。

roxygenタグからドキュメントのソースファイルを生成するには `devtools::document()` を実行します。

例えば、下の記述をパッケージ内のRコードのどこかに記述してから `devtools::document()` を実行すると、

```
#' @useDynLib myPackage
#' @importFrom Rcpp sourceCpp
NULL
```
`NAMESPACE` ファイルに次の記述が加えられます。

```
importFrom(Rcpp,sourceCpp)
useDynLib(myPackage)
```








# 自作パッケージで他のパッケージのC/C++関数を利用する

Rのパッケージによっては、そのパッケージで定義されたC/C++の関数やクラスを他のパッケージから利用できるように公開しているものもあります。自作パッケージから、他のパッケージが公開しているパッケージを利用するには以下の方法を用います。








# 自作パッケージでサードパーティのC/C++ライブラリを利用する
