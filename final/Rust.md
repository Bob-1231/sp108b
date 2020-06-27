# Rust程式語言
參考主要來源為：   
> - <a href="https://kaisery.gitbooks.io/trpl-zh-cn/content/">Rust程序設計語言（第二版& 2018 edition）</a>
> - <a href="https://askeing.github.io/rust-book/README.html">Rust程式語言 正體中文版</a>
## 簡介
Rust是由Mozilla主導開發的通用、編譯型程式語言。設計準則為「安全、並行、實用」，支援函數式、並行式、程序式以及物件導向的編程風格。--<a href="https://zh.wikipedia.org/wiki/Rust">Rust維基百科</a>   
在開始使用之前，我先在<a href="https://kaisery.gitbooks.io/trpl-zh-cn/content/ch01-01-installation.html">這裡</a>按照教學安裝好Rust，安裝過程到開始編譯是沒遇到問題，如果有安裝好卻無法編譯，可以參考<a href="https://github.com/JesusDick/sp108b/blob/master/%E6%9C%9F%E6%9C%AB%E5%A0%B1%E5%91%8A/FinalTest.md">這篇的前置作業</a>，或許能解決。
## 試試Hello World
每次學習新程式，第一步永遠要先學會執行Hello World，執行之前先創建一個放置Rust代碼的資料夾，在PowerShell裡面輸入
<pre>
mkdir project       //創建名叫project的資料夾
cd project          //切換到project資料夾
mkdir hello_world   //在project資料夾裡面創建hello_world資料夾
cd hello_world      //切換到hello_world資料夾
</pre>
再來新增源文件，Rust源文件的副檔名必須以 **.rs** 為結尾。再來看看程式碼：
<pre>
fn main() {
    println!("Hello World");
}
</pre>
第一次看到這段程式的時候其實有些不習慣，因為沒看過fn main，只有看過int main或是def main等，不過fn應該是function的意思。還有println!指令，上網查了才知道print+ln意思是執行完換行的意思，沒換行就是只有print。至於！的部分，
>println!調用了一個Rust宏（macro）。如果是調用函數，則應輸入println（沒有!）。當看到符號!的時候，就意味著調用的是宏而不是普通函數。

文中只有先簡述了一下！的意思，要到後面章節才會詳細介紹，所以現在也還不太懂。
### 編譯和執行
從文中介紹看起來可以用rustc或是cargo進行編譯和執行，以下嘗試對這兩者做比較。如果安裝Rust時是用官方安裝包，則也自帶了Cargo，檢查有沒有Cargo可以在PowerShell輸入 "**cargo --version**"，如果看到版本號，代表有Cargo。   
![version](cargo_version.png)

不過要使用cargo，也要先創建一個cargo的目錄，回到存放代碼的目錄(project)，執行：
<pre>
cargo new hello_cargo       //創建名叫hello_cargo的資料夾
cd hello_cargo              //切換到hello_cargo資料夾
</pre>
執行完會產生其中一個文件叫*Cargo.toml*，裡面內容簡單來說好像是Cargo編譯程式所需的配置，所以去測試了沒有該文件的地方執行Cargo，發現沒有它就無法使用Cargo指令。
#### 編譯
>rustc 源檔名   
cargo build

兩者都是編譯完建立可執行檔。rustc會把執行檔就放在原資料夾裡面；Cargo是把執行檔放在/target/debug/hello_cargo.exe
#### 執行
>./源檔名.exe   
./target/debug/hello_cargo.exe

rustc因為沒變更資料夾，所以直接./執行；Cargo因為特別放在其它資料夾，所以還要輸入路徑。   
rustc的編譯和執行從文中看起來就這樣了，但Cargo還有其它指令是需要知道的。

---
### Cargo
**cargo check** 指令可以在打完一段程式後，隨時檢查代碼是否可以編譯，如果程式沒問題的話就會印出Finished，有錯誤就會印出error並會提示錯誤在哪。這個指令就是單純檢查錯誤，不會產生執行檔，所以應該也節省了一點空間。   
![check](cargo_check.jpg)

**cargo run** 指令可以在打完程式後，編譯完馬上執行，可以省去cargo build還有./target/debug/hello_cargo.exe的時間，而且這指令也會跟cargo check一樣，有錯誤會顯示出來，應該可以說是涵蓋檢查、編譯、執行的動作。如果程式有更動，這指令也會自動重新編譯出一個執行檔，且Cargo只重新編譯有做修改的部份，可以增加編譯速度。   
![run](cargo_run.jpg)

---
## Guess Number
接下來是嘗試跟者文中範例做程式碼練習。
### 產生亂數
***
先產生要猜的數字，猜數字當然不是自己出數字自己猜，而是要系統變出亂數自己猜，因此需要引入套件，而官方有提供 **rand** 套件來產生隨機數值，要加入rand套件要先回到 *Cargo.toml* 中，接著在[dependencies]底下加入 **rand = "0.3.0"**
![rand](rand.jpg)

而版本號根據文中也有其它表示方式代表不同意思：
>上面的版號實際上可以寫成 ^0.3.0，代表「所有跟 0.3.0 相容的版本」。 如果我們想要確實的使用 0.3.0，我們可以寫成 rand="=0.3.0"（請注意兩個等號）。 而當我們想要使用最新版，我們可以使用 *。 

現在就可以把套件加入程式碼了，因為Rust對於套件有定義了一個新的名稱 **crate(板條箱)**，若要在程式碼中使用套件，需在程式碼的最上方使用 **extern crate** 指令。
<pre>
extern crate rand;
</pre>
接下來要產生隨機變數，先設定一個變數名稱，然後把要隨機產生的數字指定給它
<pre>
fn main() {
    let answer = rand::thread_rng().gen_range(1, 101);
}
</pre>
***
雖然看起來跟任何程式語言一樣，但文中有提到Rust程式語言的變數綁定還暗藏玄機：
> <pre>let foo = bar</pre>舉例來說，它預設是 不可變的（immutable）。 這也是為何我們的範例使用 mut：它使綁定變成可變的（mutable），而不再是不可變的。 let 不會從左邊取得賦值（assignment）的名字，實際上他使用「模式」（pattern）。 我們在後面會使用模式。 對於我們的簡單情況，它已經夠用了：
> <pre>let foo = 5;       // immutable.
> let mut bar = 5;   // mutable</pre>
這意思應該是說變數foo沒有使用mut，所以不能進行修改；而變數bar有使用mut，所以程式在執行的時候可以被改成其它值。   
***
現在來看看這段程式碼
<pre>
let answer = rand::thread_rng().gen_range(1, 101);
</pre>
變數 answer 是用來儲存要猜的那個數字，答案是不需要被更動的，所以不需要加上mut。   
<pre>
rand::thread_rng()
</pre>
文中講到「可以透過 **rand::** 前綴詞來使用任何 rand crate 內的東西。」所以 :: 應該就是說從前者的套件來使用後者的項目，也就是從 rand套件裡面使用thread_rng()的東西。上網查了一下 thread_rng()簡單來說是隨機數字產生器：
> Retrieve the lazily-initialized thread-local random number generator, seeded by the system. --<a href="https://docs.rs/rand/0.7.3/rand/fn.thread_rng.html">Function rand::thread_rng</a>

接下來看這個
<pre>
.gen_range(1, 101);
</pre>
看上去第一個想到的是generate range 1~101，從1到101之間產生數字。但文中提到：
>結果包含了下限，但不包含上限，所以我們需要傳遞 1 和 101 去取得 1 到 100 範圍內的數字。

所以並不包含101在內。這讓我想到之前學C取亂數的時候，取1到100要使用 1 + ( rand() % 100 ) 要加1才是取1到100，不加1的話是取0到99。   
文中還有提到說：
> 我們即將使用一個方法（method），這個方法需要 Rng 在有效範圍（scope）中才能運作。

意思應該是說，要產生範圍，需要再加入額外的指令才能產生，所以要再加入
<pre>
use rand::Rng;
</pre>
現在產生完範圍亂數了，執行看看能不能正常產出1到100之間的數。
<pre>
extern crate rand;

use rand::Rng;

fn main() {
    let answer = rand::thread_rng().gen_range(1, 101);

    print!("Number is {}", answer);
}
</pre>
執行結果如下：   
![answer](answer.png)

執行3次都產出1到100之間的數字，所以應該沒問題。
### 輸入數字
***
接下來要讓使用者輸入要猜的數字了，因此需要使用輸入輸出的功能，Rust裡面 **std::io** 這個模組提供了輸入與輸出的功能，從名字上來看也是非常直白，大概是 input 跟 output。所以先把程式碼放入
<pre>
use std::io;
</pre>
再來要宣告一個變數，用來儲存使用者輸入的數字
<pre>
let mut guess = String::new();
</pre>
用 mut 大概是因為如果猜錯的話就需要再重複輸入一次，就會變更到變數值，所以用 mut 達成可更動變數。至於 String::new()，文中的解釋是這樣子：
>String 是一個字串型別，由標準函式庫提供。 一個 字串 是一個以 UTF-8 編碼的可變長度的文字。   
而 ::new() 語法使用 :: 是因為它是一個特定型別的「關聯函式」（associated function）。 也就是說，它被關聯到 String 本身，而非特定的某個 String 的實體（instance）。 一些語言稱之為「靜態方法」（static method）。   
此函式被稱為 new()，因為它建立一個新的、空的 String。 你可以在其他許多型別找到 new() 函式，因為它是建立某一些型別新值的通用名稱。

其實看得有點模模糊糊，我自己理解為讓 String 變空，使用者輸入東西到 String 裡面，再指定給 guess 變數。
### 取得數字
***
輸入完數字後要讓系統偵測輸入內容，所以還需要再輸入取得數字的程式碼。(覺得C比較方便一些，因為C只要輸入 scanf_s("%d", &guess) 就可以輸入並取得數字了)