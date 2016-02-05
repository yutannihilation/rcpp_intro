# Environment

`Environment` を用いると環境中の変数や関数にアクセスすることができる。

##作成

```
Environment env();
Environment env(env1);
Environment env("package:stats");
Environment env(1); //サーチパスの i 番目の環境（i=1はグローバル環境）
```



##メンバ関数

#### get(name)

この環境空 name で指定された名前のオブジェクトを取得する。
見つからない場合は R_NilValue (NULL) を返す。

#### ls(all)

この環境にあるオブジェクトの一覧を返す。

論理値 all が true なら全てのオブジェクト、false なら名前が `.` から始まるオブジェクトは除外する。

```
SEXP get(const std::string& name) const
```

```
SEXP find( const std::string& name) const
```
Get an object from the environment or one of its parents

```
bool exists( const std::string& name ) const
```

```
bool assign( const std::string& name, SEXP x )
```
```
template <typename WRAPPABLE>
        bool assign( const std::string& name, const WRAPPABLE& x) const
```

```
bool isLocked() const
```

```
bool remove( const std::string& name )
```

```
void lock(bool bindings = false)
```
?lockEnvironment
bindings also lock the bindings of this environment ?


```
void lockBinding(const std::string& name)
```
Locks the given binding in the environment.

see ?bindingIsLocked

throw no_such_binding if there is no such binding in this environment


```
void unlockBinding(const std::string& name){
```

* unlocks the given binding
* see ?bindingIsLocked
*
* @throw no_such_binding if there is no such binding in this environment


```
bool bindingIsLocked(const std::string& name) const{
```
* @param name name of a potential binding
*
* @return true if the binding is locked in this environment
* see ?bindingIsLocked
*
* @throw no_such_binding if there is no such binding in this environment

```
bool bindingIsActive(const std::string& name) const {
```
* @param name name of a binding
*
* @return true if the binding is active in this environment
* see ?bindingIsActive
*
* @throw no_such_binding if there is no such binding in this environment
 

```
bool is_user_database() const {
```
* Indicates if this is a user defined database.

```
Environment_Impl parent() const
```
* The parent environment of this environment

```
Environment_Impl new_child(bool hashed) {
```
* creates a new environment whose this is the parent



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



## 関数

```
Environment new_env(int size = 29)
```

```
Environment new_env(SEXP parent, int size = 29)
```

