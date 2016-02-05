# Environment

`Environment` を用いると環境中の変数や関数にアクセスすることができる。

##作成

```
Environment env();     //グローバル環境
Environment env(env1); // env1 をコピー
Environment env(1); //サーチパスの i 番目の環境（i=1はグローバル環境）
Environment env("package:stats"); //パッケージの環境
```



##メンバ関数

#### get(name)

この環境から name で指定された名前のオブジェクトを取得する。
見つからない場合は R_NilValue (NULL) を返す。

#### ls(all)

この環境にあるオブジェクトの一覧を返す。

論理値 all が true なら全てのオブジェクト、false なら名前が `.` から始まるオブジェクトは除外する。

#### find(name)

この環境、および、この環境の（全ての）親環境から、name で指定された名前のオブジェクトを探して取得する。見つからない場合は "binding not found:" エラーが thow される。

#### exists(name)

この環境に指定された名前のオブジェクトがあるかどうか。論理値を返す。

#### assign( name, x )

この環境にある name で指定された名前のオブジェクトに、x を代入する。成功した場合には true を返す。



#### isLocked()

この環境がロックされているかどうか。

#### remove(name)

この環境から name で指定された名前のオブジェクトを削除する。成功した場合には true を返す。

#### lock(bindings = false)

この環境をロックする。

binding = true なら、この環境の binding もロックする。

#### lockBinding(name)

この環境にある name で指定された名前の binding をロックする。

詳細は `?bindingIsLocked` を参照

指定された binding が見つからない場合は `"no_such_binding"` を throw する。

#### unlockBinding(name){

この環境から name で指定された名前の binding のロックを解除する。

指定された binding が見つからない場合は `"no_such_binding"` を throw する。

#### bindingIsLocked(name)

この環境にある name で指定された名前の binding がロックされているかどうか。

指定された binding が見つからない場合は `"no_such_binding"` を throw する。

#### bindingIsActive(name)

この環境にある name で指定された名前の binding がアクティブかどうか。

指定された binding が見つからない場合は `"no_such_binding"` を throw する。

#### is_user_database()

この環境がユーザーが定義したデータベースであるかどうか。

#### parent()

この環境の親環境を返す。

#### new_child(hashed)

この環境を親とした、新しい環境を作成する。

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

`namespace_env()`を使うと、export されていないパッケージ関数にもアクセスできる。
また、R であらかじめパッケージをロードしておく必要がない。

指定されたパッケージが見つからない場合には、 `"no_such_namespace"`が throw される。



## その他の関数

```
Environment new_env(int size = 29)
```

```
Environment new_env(SEXP parent, int size = 29)
```

