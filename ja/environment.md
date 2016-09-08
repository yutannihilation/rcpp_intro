# Environment

`Environment` クラスを用いるとアクセスしたい環境を変数として保持し、その環境中の変数や関数にアクセスすることができます。

## Environment オブジェクトの作成

`Environment ` クラスのオブジェクトを作成する方法を下に示します。

```
Environment env();                           //グローバル環境
Environment env = Environment::global_env(); //グローバル環境
Environment env("package:stats");            // パッケージ stats 内の環境
Environment env(1); // オブジェクトのサーチパスの i 番目にある環境（i=1はグローバル環境）
```

オブジェクトサーチパスを確認するには R の `search()` 関数を利用します。

```
> search()
 [1] ".GlobalEnv"        "tools:RGUI"        "package:stats"    
 [4] "package:graphics"  "package:grDevices" "package:utils"    
 [7] "package:datasets"  "package:methods"   "Autoloads"        
[10] "package:base"   
```

## 環境にあるオブジェクトにアクセスする

Environment オブジェクトを通して環境中の変数や関数にアクセスするためには `[]` 演算子または `get()` メンバ関数を用います。もしも、その環境に存在しない変数や関数にアクセスした場合には `R_NilValue` (`NULL`) が返ります。

```
// グローバル環境を取得します
Environment env = Environment::global_env();

//グローバル環境にある変数を取得します
NumericVector x = env["x"];

//グローバル環境にある変数 x の値を変更します
x[0] = 100;
```

## 新しい環境を作成する

関数 `new_env()` 関数を用いることで新しい空の環境を作成することができます。

#### new_env(size = 29)



#### new_env(parent, size = 29)

|`new_env(size = 29)`|新しい環境を返します。size は作成される環境のハッシュテーブルの初期サイズを指定します。|
|`new_env(parent, size = 29)`|parent を親環境とする新しい環境を返します。size は作成される環境のハッシュテーブルの初期サイズを指定します。|



## メンバ関数

#### get(name)

この環境から name で指定された名前のオブジェクトを取得します。
見つからない場合は R_NilValue (NULL) を返す。

#### ls(all)

この環境にあるオブジェクトの一覧を返す。

論理値 all が true なら全てのオブジェクト、false なら名前が `.` から始まるオブジェクトは除外します。

#### find(name)

この環境、および、この環境の（全ての）親環境から、name で指定された名前のオブジェクトを探して取得します。見つからない場合は "binding not found:" エラーが thow されます。

#### exists(name)

この環境に指定された名前のオブジェクトがあるかどうか。論理値を返す。

#### assign( name, x )

この環境にある name で指定された名前のオブジェクトに、x を代入します。成功した場合には true を返す。



#### isLocked()

この環境がロックされているかどうか。

#### remove(name)

この環境から name で指定された名前のオブジェクトを削除します。成功した場合には true を返す。

#### lock(bindings = false)

この環境をロックします。

binding = true なら、この環境の binding もロックします。

#### lockBinding(name)

この環境にある name で指定された名前の binding をロックします。

詳細は `?bindingIsLocked` を参照

指定された binding が見つからない場合は `"no_such_binding"` を throw します。

#### unlockBinding(name){

この環境から name で指定された名前の binding のロックを解除します。

指定された binding が見つからない場合は `"no_such_binding"` を throw します。

#### bindingIsLocked(name)

この環境にある name で指定された名前の binding がロックされているかどうか。

指定された binding が見つからない場合は `"no_such_binding"` を throw します。

#### bindingIsActive(name)

この環境にある name で指定された名前の binding がアクティブかどうか。

指定された binding が見つからない場合は `"no_such_binding"` を throw します。

#### is_user_database()

この環境がユーザーが定義したデータベースであるかどうか。

#### parent()

この環境の親環境を返す。

#### new_child(hashed)

この環境を親とした、新しい環境を作成します。

hashed が true なら??


## static メンバ関数

####Environment::global_env()

グローバル環境を返す。 

詳細は `?globalenv` を参照


####Environment::empty_env()

空環境を返す。 

詳細は `?emptyenv` を参照

####Environment::base_env()

baseパッケージの環境を返す。 

詳細は `?baseenv` を参照

####Environment::base_namespace()

baseパッケージの名前空間を返す。 

#### Environment::Rcpp_namespace()

Rcpp パッケージの名前空間を返す。 

#### Environment::namespace_env(package)

パッケージ名 package を指定して、その名前空間を得る。

`namespace_env()`を使うと、export されていないパッケージ関数にもアクセスできます。
また、R であらかじめパッケージをロードしておく必要がない。

指定されたパッケージが見つからない場合には、 `"no_such_namespace"`が throw されます。



## その他の関数

```
Environment new_env(int size = 29)
```

```
Environment new_env(SEXP parent, int size = 29)
```

