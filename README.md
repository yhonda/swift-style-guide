# Swiftコーディング規約

# バージョン
v1.0.0

# 目次

- [はじめに](#はじめに)
- [命名規則](#命名規則)
- [ファイルフォーマット](#ファイルフォーマット)
- [アクセス制御](#アクセス制御)
- [クラス](#クラス)
- [プロパティ](#プロパティ)
- [変数](#変数)
- [型](#型)
- [制御](#制御)
- [演算子](#演算子)
- [エラーハンドリング・オプショナル](#エラーハンドリング・オプショナル)
- [クロージャー](#クロージャー)
- [規約外とした項目](#規約外とした項目)

# はじめに

このコーディング規約は以下を方針として策定しています。
- 一貫性
- 可読性
- 冗長性の排除

# 命名規則

## クラス・構造体・列挙体・プロトコル・ジェネリクスの型パラメータ
キャメルケースとし、先頭を大文字とすること。  
(UpperCamelCase)

**理由**

API Design Guidelinesに従う。

**例**

良い例

```swift
class SomeClass<T> {
    ...
}

enum SomeEnum {
    ...
}

struct SomeStruct {
    typealias Element = String

    ...
}

protocol SomeProtocol {
    associatedtype Parameter

    ...
}
```

悪い例

```swift
class someClass<t> {
    ...
}

enum someEnum {
    ...
}

struct someStruct {
    typealias element = String

    ...
}

protocol someProtocol {
    associatedtype parameter

    ...
}
```

## 変数・メソッド・enumのcase
キャメルケースとし、先頭は小文字とすること。  
(lowerCamelCase)

**理由**

API Design Guidelinesに従う。

**例**

良い例

```swift
struct SomeStruct {

    let someNumberProperty: Int

    func someMethod() -> String {
        ...
    }
}

enum SomeEnum {

    case red
    case green
    case blue

    ...
}
```

悪い例

```swift
struct SomeStruct {

    let SomeNumberProperty: Int

    func SomeMethod() -> String {
        ...
    }
}

enum SomeEnum {

    case Red
    case Green
    case Blue

    ...
}
```

## クラス名
クラス名にプレフィックスは付けない。

**理由**

Swiftでは名前空間が存在するためクラス名は衝突せず、ただ可読性を下げてしまうため。  
また、Swift3でFoundationAPIsのNSプレフィックスは除去されるため。

**例**

良い例

```swift
class SomeClass {

    ...
}
```

悪い例

```swift
class STVSomeClass {

    ...
}
```

## 頭文字語
すべて大文字またはすべて小文字とすること。

**理由**

API Design Guidelinesに従う。

**例**

```swift
let url: NSURL = ...
let thumbnailURL: NSURL = ...

let id: String = ...
let userID: String = ...
```

## urlとdateの命名
NSURLの場合○○URLを使い、Stringであれば○○URLStringとする。  
同様にNSDateの場合○○Dateを使い、Stringであれば○○DateStringとする。

**理由**

混同しやすいため。

**例**

良い例

```swift
var thumbnailURL: URL
var imageURLString: String
var lastUpdateDate: Date
var birthdayString: String
```

悪い例

```swift
var thumbnailURL: String
var lastUpdateDate: String
```

## extensionのファイル名
extensionのみのファイル名は**UIView+○○(機能名).swift**とすること。  
加えて、extensionは機能単位でグルーピングすること。

**理由**

extensionであることを明確にするため。  
機能ごとにextensionを作り、可読性をあげるため。

**良い例**

UIView+Border.swift

```swift
extension UIView {

    @IBInspectable
    var cornerRadius: CGFloat {
        get {
            return layer.cornerRadius
        }
        set {
            layer.cornerRadius = newValue
            layer.masksToBounds = newValue > 0
        }
    }

    @IBInspectable
    var borderWidth: CGFloat {
        get {
            return layer.borderWidth
        }
        set {
            layer.borderWidth = newValue
        }
    }

    @IBInspectable
    var borderColor: UIColor? {
        get {
            return layer.borderColor.map { UIColor(CGColor: $0) }
        }
        set {
            layer.borderColor = newValue?.CGColor
        }
    }
}
```

悪い例

Border.swift

```swift
extension UIView {

    @IBInspectable
    var borderColor: UIColor? {
        get {
            return layer.borderColor.map { UIColor(CGColor: $0) }
        }
        set {
            layer.borderColor = newValue?.CGColor
        }
    }

    // ↓↓関係ない
    var hasSubview: Bool {
        return !subviews.isEmpty
    }
}
```

## その他
その他命名に関しては、[API Design Guidelines](https://swift.org/documentation/api-design-guidelines/)を参考にすること。  
以下は*Fundamentals*からの引用です。

> ・**利用の観点から、明確さ**が最大の目標である。  
> メソッドやプロパティなどのエンティティは、一度だけ定義され、何度も使われるものである。  
> これらを利用する際、明確かつ簡潔であるようにAPIを設計しなければならない。  
> APIの評価をする際は、定義を読むだけでは不十分である。  
> コンテキスト上で明確であるよう、APIを使っている例も含めて評価しなければならない。

> ・**明確さは簡潔さよりも重要である。**  
> Swiftのコードは短く書くことができるが、できるだけ少ない文字数で  
> 可能な限り最小のコードを実現することが目標ではない。  
> Swiftにおける簡潔さは  
> 強力な型システムと自然にボイラープレートコードを減らすことができる性質から生まれた、  
> 副次的なものである。

> ・すべての宣言に**ドキュメントとしてコメント**を記載すること。  
> ドキュメントを書くことで得られる洞察は設計上に大きな影響をあたえるので、  
> 端折らないこと。

# ファイルフォーマット

## ファイル内の順序
以下の順に書くこと。  
extensionを使い、関連メソッドをグルーピングするのは任意とする。

```swift
// 1.import
import UIKit

// 2.protocol
protocol ViewControllerDelegate {

}

// 3.class・struct・enum
final class ViewController: UIViewController {

    // 4.typealias
    // 5.Inner class, enum & struct

    // 6.static member
    //
    // 6-1.property
    // |
    // | 6-1-1.public
    // | |
    // | | 6-1-1-1.let
    // | | 6-1-1-2.var
    // | | 6-1-1-3.computed var
    // |
    // | 6-1-2.internal
    // | |
    // | | 6-1-2-1.let
    // | | 6-1-2-2.var
    // | | 6-1-2-3.computed var
    // |
    // | 6-1-3.private
    // | |
    // | | 6-1-3-1.let
    // | | 6-1-3-2.var
    // | | 6-1-3-3.computed var
    //
    // 6-2.method
    // |
    // | 6-2-1.public
    // |
    // | 6-2-2.internal
    // |
    // | 6-2-3.private

    // 7.instance member
    //
    // 7-1.property
    // |
    // | 7-1-1.public
    // | |
    // | | 7-1-1-1.@~
    // | | 7-1-1-2.let
    // | | 7-1-1-3.var
    // | | 7-1-1-4.computed var
    // |
    // | 7-1-2.internal
    // | |
    // | | 7-1-2-1.@~
    // | | 7-1-2-2.let
    // | | 7-1-2-3.var
    // | | 7-1-2-4.computed var
    // |
    // | 7-1-3.private
    // | |
    // | | 7-1-3-1.@~
    // | | 7-1-3-2.let
    // | | 7-1-3-3.var
    // | | 7-1-3-4.computed var
    //
    // 7-2.method
    // |
    // | 7-2-1.public
    // | |
    // | | 7-2-1-1.init
    // | | 7-2-1-2.life cycle
    // | | 7-2-1-3.@~
    // | | 7-2-1-4.others
    // |
    // | 7-2-2.internal
    // | |
    // | | 7-2-2-1.init
    // | | 7-2-2-2.life cycle
    // | | 7-2-2-3.@~
    // | | 7-2-2-4.others
    // |
    // | 7-2-3.private
    // | |
    // | | 7-2-2-1.init
    // | | 7-2-2-2.life cycle
    // | | 7-2-2-3.@~
    // | | 7-2-2-4.others
}

// 8.extension
extension ViewController: UITableViewDelegate {

}
```

**理由**

書く順序を統一することで、参照したい情報へのアクセスを効率的にする。

## 変数宣言時の修飾子の順序
以下の順に書くこと。

@~  
↓  
アクセス制御  
↓  
static, class  
↓  
dynamic, lazy, weak  
↓  
let, var

**理由**

統一のため。

## import文の順序
以下の順に書くこと。

フレームワーク  
↓  
ライブラリ

また、同系列内ではアルファベット順に書くこと。

**理由**

コンフリクトを防止するため。  
順序を揃えることで可読性を上げるため。

## 使われていないコードの削除
- 使われていないコード
- スーパークラスを呼び出すだけのコード
- Xcodeのテンプレートとして書かれたままのコード

は削除する。

**理由**

無用なコードは可読性を下げるため。

## スペース

- ブラケットの前後には半角スペースを1つおくこと。
- メソッドの戻り値の->の前後に半角スペースを1つおくこと。
- コロンの前はスペースを入れず、コロンの後ろに1つスペースをいれること。(三項演算子を除く)
- カンマの前にはスペースを入れず、カンマの後ろに1つスペースをいれること。
- 演算子の実装の際は演算子の後ろに1つスペースをおくこと。

**理由**

一貫性を持たせるため。

**例**

良い例

```swift
func something() -> Int {
    let some: Int = 0
}

func <| (lhs: Int, rhs: Int) -> Int
func <|< <A>(lhs: A, rhs: A) -> A
```

悪い例

```swift
func something()->Int{
    let some :Int = 0
}

func <|(lhs: Int , rhs: Int)->Int
func <|<<A>(lhs: A,rhs: A)->A
```

## プロトコルの実装
ひとつのextensionで実装するプロトコルは１つとする。  
また、継承とプロトコルの準拠を同時に行わないこと。

**理由**

関連メソッドをグルーピングすることで可読性を上げるため。

**例**

良い例

```swift
class ViewController: UIViewController {
    ...
}

extension ViewController: UITableViewDataSource {
    ...
}

extension ViewController: UITableViewDelegate {
    ...
}
```

悪い例

```swift
class ViewController: UIViewController, UITableViewDataSource {
    ...
}
```
```swift
extension ViewController: UITableViewDataSource, UITableViewDelegate {
    ...
}
```

## enumの場所
広く使われるものであれば独立したファイルに宣言すること。  
ある構造に特有のものであればその構造内で宣言すること。

**理由**

enumのある場所を統一するため。

## 関数の宣言
エディター上で折り返したら改行すること。  
また、改行をいれたらすべての引数の前で改行すること。

**理由**

可読性を上げるため。

## セミコロンの使用禁止
行末のセミコロンの使用を禁止する。

**理由**

必要ないため。

## 条件文の()の禁止
条件文を()で囲わないこと。

**理由**

必要ないため。

**例**

良い例

```swift
if names.isEmpty {
    ...
}
```

悪い例

```swift
if (names.isEmpty) {
    ...
}
```

## todo / fixmeの記載
未対応の機能には以下を記述すること。

```swift
// TODO: 〜
```

修正が必要な場合は以下を記述すること。

```swift
// FIXME: 〜
```

**理由**

実装漏れを防止するため。

# アクセス制御

## アクセス制御
アクセス制御は可能な限りprivateを指定すること。

**理由**

そのクラス内、メソッド内…ということが担保でき、実装変更時の影響範囲を小さくできるため。

# クラス

## final宣言
継承、オーバーライドをさせたくない場合は明示的にfinalを宣言すること。

**理由**

継承、オーバーライド不可なことを明示的に示すことで設計意図を示せるため。  
また、副次的にパフォーマンス向上にも繋がるため。

# プロパティ

## self.でのアクセス
必要な時にのみ使用すること。  
(引数と同じ名前のプロパティ、クロージャ内等)

**理由**

統一し、また冗長性を排除するため。

**例**

良い例

```swift
struct Person {

  let name: String

  init(name: String) {
      self.name = name
  }

  func printName() {
      print(name)
  }

  let callback: () -> String = {
      return self.name
  }
}
```

悪い例

```swift
struct Person {

  let name: String

  func printName() {
      print(self.name)
  }
}
```

## getクロージャの省略
算出型プロパティにおいて、ゲッターのみ定義する場合はgetのクロージャを省略すること。

**理由**

不要なため。

**例**

良い例

```swift
class Person {

    let first: String
    let family: String

    var full: String = {
        return first + family
    }
}
```

悪い例

```swift
class Person {

    let first: String
    let family: String

    var full: String = {
        get {
            return first + family
        }
    }
}
```

# 変数

## 定数・変数の宣言
var宣言を使用するのは、その値が変わり得る等、明確な理由があるときのみとする。  
それ以外の場合はlet宣言を使用する。

**理由**

より安全なコードにするため。  
意図しない値の変更を防ぐため。

letを使用することでプログラマーが値が変わらないことを確認することができる。  
逆に、varによって宣言された変数が変更されることを予期できる。

# 型

## 型推論
最大限利用すること。

**理由**

冗長性を排除し、シンプルなコードにするため。

**例**

良い例

```swift
let name = "KentaKudo"
```

悪い例

```swift
let name: String = "KentaKudo"
```

## 空配列・空辞書の初期化
空配列・空辞書の初期化の際は、型推論を利用した形で書くこと。

**理由**

統一のため。

**例**

良い例

```swift
var names = [String]()
var jsonDic = [String: AnyObject]()
```

悪い例

```swift
var names: [String] = []
var jsonDic: [String: AnyObject] = [:]
```

## 糖衣構文の使用
糖衣構文がある場合はそちらを使用すること。  
Voidに関しては、可読性を理由に例外を認める。

**理由**

統一のため。

**例**

良い例

```swift
var names: [String]
var jsonDic: [String: AnyObject]
var title: String?
var someClosure: () -> ()
```

悪い例

```swift
var names: Array<String>
var jsonDic: Dictionary<String, AnyObject>
var title: Optional<String>
var someClosure: Void -> Void
```

## Objective-Cクラスの扱い
文字列はNSString型でなく、String型を使用すること。  
また、文字列長判定にはcharacters.countを使用すること。

NSNumber、NSArray、NSDictionary型も同様に、  
Swiftネイティブな型を利用すること。

**理由**

swiftの型で統一するため。

# 制御構文

## 条件式for loop
使用禁止とする。  
for in を使うこと。

**理由**

swift3でdeprecatedとなるため。

**備考**

swift3移行後は項目を削除する。

## 早期Return
guard文を利用し、例外の場合に早めに制御を返すこと。

**理由**

guardにより制御が返ることを明示し、可読性を上げるため。  
また、ネストを減らすことで書きやすさ読みやすさを上げるため。

**例**

良い例

```swift
guard hogehoge else {
    return
}
```

悪い例

```swift
if hogehoge {

} else {
    return
}
```

# 演算子

## インクリメント、デクリメント
使用禁止とする。

**理由**

Swift3でdeprecatedとなるため。

**備考**

swift3移行後に項目を削除する。

# エラーハンドリング・オプショナル

## Implicitly Unwrapped 型
利用をIBOutletに限定する。

**理由**

nilアクセスの可能性をできるだけ下げるため。

## アンラップ処理

!(forced unwrap)を使わず、optional chaining, optional binding, nil coalescingを使用すること。  
また、optional bindingの際には、同じ変数名で変数宣言すること。

**理由**

nilアクセスの可能性をできるだけ下げるため。  
アンラップ前の変数を使ってしまうバグを防ぐため。

**例**

良い例

```swift:GoodOptionalChaining.swift
final class ViewController: UIViewController {
    override func viewDidLoad() {
        super.viewDidLoad()
        let navigationItem = navigationController?.navigationItem
    }
}
```

```swift:GoodOptionalBinding.swift
let response: AnyObject = [:]
if let json = response as? [String: AnyObject] {
    ...
}
```

```swift:GoodNilCoalescing.swift
let someNumber: Int? = 10
let otherNumber: Int = someNumber ?? 0
```

```swift:GoodBindingNaming.swift
if let some = some {
    ...
}
```

悪い例

```swift:BadOptionalChaining.swift
final class ViewController: UIViewController {
    override func viewDidLoad() {
        super.viewDidLoad()
        let navigationItem = navigationController!.navigationItem
    }
}
```

```swift:BadOptionalBinding.swift
let response: AnyObject = [:]
let json = response as! [String: AnyObject]
```

```swift:BadNilCoalescing.swift
let someNumber: Int? = 10
let otherNumber: Int = someNumber!
```

```swift:BadBindingNaming.swift
if let bindedSome = some {
    ...
}
```

## 弱参照
弱参照をする際にはunownedではなくweakを使用すること。

**理由**

nilアクセスの可能性をできるだけ下げるため。

**例**

良い例

```swift
someFunc { [weak self] in
    ...
}
```

悪い例

```swift
someFunc { [unowned self] in
    ...
}
```

## try~catch
catchで例外を処理しない場合はtry!ではなく、try?を使用する。

**理由**

nilアクセスの可能性をできるだけ下げるため。

**例**

良い例

```swift
let someInt = try? someErrorThrowMethod() ?? 0
```

悪い例

```swift
let someInt = try! someErrorThrowMethod()
```

# クロージャー

## クロージャーの表記
一行で記述する場合に限り、引数名を省略すること。  
メソッドが複数のクロージャを引数にとる場合、trailingクロージャの省略形を使用しないこと。

**理由**

複数行のクロージャ内で引数名を省略した場合に可読性が落ちるため。  
複数のクロージャを引数にするメソッドの呼び出しでtrailingクロージャの省略形を使うと、外部引数名が省略され、可読性が落ちるため。

**例**

良い例

```swift
let someClosure: String -> Int? = { Int($0) }

let someClosure: String -> Int? = { string in

    let intValue = Int(string)
    return intValue
}

UIView.animateWithDuration(10.0,
                           animations: {
                               print("animate")
                           },
                           completion: { _ in
                               print("completed")
                           })
```

悪い例

```swift
let someClosure: String -> Int? = {

    let intValue = Int($0)
    return intValue
}

UIView.animateWithDuration(10.0,
                           animations: {
                               print("animate")
                           }) { _ in
                              print("competed")
                           }
```

# 規約外とした項目

## warningを解消する
warningを可能な限り解消すること。

**却下理由**

Xcodeに関する事項であり、コーディングの規約としては不適切と判断した。

## 第一引数の明記
第一引数について、必要な場合は外部引数名を使うこと。

**却下理由**

swift3では第一引数にデフォルトで外部引数名がつくため。

**備考**

swift3に移行後は項目を削除する。

## 関数型のメソッドに関する制約
関数型の使用を制限する。

**却下理由**

プロジェクトごとに判断されるべきものであり、コーディング規約としては不適切と判断した。

## AutoLayoutに関する制約
AutoLayoutを使用する際に関する規約。

**却下理由**

各プロジェクトに裁量を任せることとし、コーディング規約の対象外とした。
