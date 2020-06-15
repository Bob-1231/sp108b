# Rust程式語言
參考主要來源為：<a href="https://kaisery.gitbooks.io/trpl-zh-cn/content/" target=_blank>Rust 程序設計語言（第二版& 2018 edition）</a>
## 簡介
Rust是由Mozilla主導開發的通用、編譯型程式語言。設計準則為「安全、並行、實用」，支援函數式、並行式、程序式以及物件導向的編程風格。   
在開始使用之前，我先在<a href="https://kaisery.gitbooks.io/trpl-zh-cn/content/ch01-01-installation.html" target=_blank>這裡</a>按照教學安裝好Rust，安裝過程到開始編譯是沒遇到問題，如果有安裝好卻無法編譯，可以參考<a href="https://github.com/JesusDick/sp108b/blob/master/%E6%9C%9F%E6%9C%AB%E5%A0%B1%E5%91%8A/EndingTest.md" target=_blank>這篇的前置作業</a>，或許能解決。
## 試試Hello World
每次學習新程式，第一步永遠要先學會執行Hello World，執行之前先創建一個放置Rust代碼的資料夾，在PowerShell裡面輸入
<pre>
mkdir project       //創建名叫project的資料夾
cd project          //移動到project資料夾
mkdir hello_world   //在project資料夾創建hello_world資料夾
cd hello_world      //移動到hello_world資料夾
</pre>
再來新增源文件，Rust源文件的副檔名必須以 .rs 為結尾。程式碼如下：
<pre>
fn main() {
    println!("Hello World");
}
</pre>
第一次看到這段程式的時候其實有些不習慣，因為沒看過fn main，只有看過int main或是def main，還有println!指令，上網查了才知道print+ln意思是執行完換行的意思，沒換行就是只有print。