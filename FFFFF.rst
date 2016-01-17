---
tags: Markdown, StackEdit
---
>以下是我用在StackEdit編輯的Markdown文件
>
* [請點我]用StackEdit開啟，並點右上角的 <i class="icon-pencil"></i> 複製這份文件！
![編輯按鈕][]
* 我用StackEdit發佈到Blogger的網誌
	1. [用StackEdit寫Markdown，並輸出至網誌!]
	2. [Android兩個下拉重整的方法]

----------

>
>dddd
>ddd


#用StackEdit寫Markdown，並輸出至網誌!

安安！我是**StackEdit**[^stackedit]的Markdown教學。別把我刪掉囉～我可是很有用的<i class="icon-heart">

> **Note:** 如果刪掉了，可以至[這裡]重新下載

翻譯：**[Paul <i class="icon-thumbs-up"></i>]**
<!--more-->

----------

[toc]


----------


##文件
StackEdit會自動存檔，而且是存在瀏覽器，代表可以**離線編輯**！

> **Note:**

> - 在第一次開啟StackEdit後便可以離線編輯。
> - 因為文件是儲存在瀏覽器，意思是在不同瀏覽器和不同電腦是不共用存檔的。
> - 刪除瀏覽器瀏覽資料的話會同時**刪除所有本地文件！**
> - 為了保護您寶貴的資料，請將文件同步到**Google Drive**或**Dropbox**  (詳情請見 [<i class="icon-refresh"></i> 同步](#同步) )

#### <i class="icon-file"></i> 建立一個新文件

* <i class="icon-folder-open"></i>(右上角按鈕) --> <i class="icon-file"></i> **New document**

#### <i class="icon-folder-open"></i> 切換其他文件

* <i class="icon-folder-open"></i>(右上角按鈕) --> 點開後可以看到你所有的文件
* 使用<kbd>Ctrl+[</kbd>或<kbd>Ctrl+]</kbd>可以快速切換其他文件

#### <i class="icon-pencil"></i> 重新命名

* 點右上角的檔名可開始重新命名，完成編輯點<kbd>Knter</kbd>儲存
![rename image][]

#### <i class="icon-trash"></i> 刪除文件

* <i class="icon-folder-open"></i>(右上角按鈕) --> <i class="icon-trash"></i> **Delete document**
	* 或者 --> <i class="icon-layers"></i> **Manage documents**

#### <i class="icon-hdd"></i> 匯出文件

* <i class="icon-provider-stackedit"></i> (左上角按鈕 ) --> <i class="icon-hdd"></i> **Export to disk**

> 關於匯出格式請看 [<i class="icon-upload"></i> 發佈文件](#發佈文件) 


----------


##同步

StackEdit支援<i class="icon-provider-gdrive"></i> **Google Drive** 和 <i class="icon-provider-dropbox"></i> **Dropbox** 同步。

> **Note:**

> - Full access to **Google Drive** or **Dropbox** is required to be able to import any document in StackEdit. Permission restrictions can be configured in the settings.
> - Imported documents are downloaded in your browser and are not transmitted to a server.
> - If you experience problems saving your documents on Google Drive, check and optionally disable browser extensions, such as Disconnect.

#### <i class="icon-refresh"></i> 開啟文件

* <i class="icon-provider-stackedit"></i> (左上角按鈕) --> <i class="icon-refresh"></i> **Synchronize** --> **Open from...**
* 一旦開啟，文件將會自動與雲端同步。

#### <i class="icon-refresh"></i> 儲存文件

* <i class="icon-provider-stackedit"></i> (左上角按鈕) --> <i class="icon-refresh"></i> **Synchronize** --> **Save on...**
* 即使文件已和 **Google Drive** 或 **Dropbox** 同步，你仍然可以將文件儲存至其他雲端，StackEdit可以將同個文件與多個雲端同步。

#### <i class="icon-refresh"></i> 同步文件

* 一旦文件和<i class="icon-provider-gdrive"></i> **Google Drive** 或<i class="icon-provider-dropbox"></i> **Dropbox** 檔案連結，StackEdit會每三分鐘自動和雲端同步。
* 點右上方的 <i class="icon-refresh"></i> 按鈕以立即進行同步。

#### <i class="icon-refresh"></i> 管理文件同步

* <i class="icon-provider-stackedit"></i> (左上角按鈕) --> <i class="icon-refresh"></i> **Synchronize** --> **Manage synchronization** 
	* 可以取消同步

> **Note:** 如果你在 **Google Drive** 或 **Dropbox** 把文件刪除，你的StackEdit文件將無法進行同步。

----------


##發佈

可以直接在StackEdit發佈你的文件！StackEdit 目前支援 **Blogger**, **Dropbox**, **Gist**, **GitHub**, **Google Drive**, **Tumblr**, **WordPress** 或者任何 SSH server.

#### <i class="icon-upload"></i> 發佈文件

* <i class="icon-provider-stackedit"></i> (左上角按鈕) --> <i class="icon-upload"></i> **Publish** --> 選擇要進行發佈網站
* 選擇發佈格式
	* Markdown：如果要發佈的網站支援Markdown語法(例如**GitHub**)
	* HTML：把Markdown轉成HTML進行發佈(至網誌)
	* Template：擁有最完整的控制權

> **Note:** 預設的Template是一個簡單的網頁，你可以在<i class="icon-provider-stackedit"></i> --> <i class="icon-cog"></i> **Settings** --> **Advanced**設定Template。

#### <i class="icon-upload"></i> 更新已發佈的文件

進行發佈之後，StackEdit會將文件與發佈網站進行連結，以利於你進行更新。點右上角的<i class="icon-upload"></i>按鈕以進行更新。

#### <i class="icon-upload"></i> 管理發佈的文件

* <i class="icon-provider-stackedit"></i> -> <i class="icon-upload"></i> **Manage publication** --> 可移除這個文件的發佈地址

> **Note:** 移除之後，文件將永遠失去與該發佈文章的連結，必須進行重新發佈。

----------


Markdown Extra
--------------------

StackEdit 支援 **Markdown Extra**, 提供 **Markdown** 一些很讚的新語法！

> **Note:**
> 
> * [關於**Markdown**語法][2]
> * [關於**Markdown Extra**][3]

### 表格

**Markdown Extra** 的表格語法：

商品 			| 價格
----------------|------
[MacBook Pro]	| $999
[iPhone]		| $888
[Apple Watch]	| $777

***

| 靠左      | 置中 	  | 靠右   |
| :------- | :------: | ----: |
| Computer | $1600000 |  5    |
| Phone    | $12      |  12   |
| Pipe     | $1       |  234  |


### 清單

**Markdown Extra** 的清單語法：

Term 1
Term 2
:   Definition A
:   Definition B

Term 3

:   Definition C

:   Definition D

	> part of definition D


### 程式碼區塊

和GitHub的程式碼區塊一樣是用 **Highlight.js** 語法變色：

```
// Foo
var bar = 0;
```

> **Tip:** <i class="icon-provider-stackedit"></i> --> <i class="icon-cog"></i> **Settings** --> **Extensions** -->  **Markdown Extra**：可以把**Highlight.js**改成**Prettify**

> **Note:** 關於**Prettify**與**Highlight.js**

> - about **Prettify** syntax highlighting [here][5],
> - about **Highlight.js** syntax highlighting [here][6].


### Footnotes

You can create footnotes like this[^footnote].

  [^footnote]: Here is the *text* of the **footnote**.


### SmartyPants

SmartyPants converts ASCII punctuation characters into "smart" typographic punctuation HTML entities. For example:

|                  | ASCII                        | HTML              |
 ----------------- | ---------------------------- | ------------------
| Single backticks | `'Isn't this fun?'`            | 'Isn't this fun?' |
| Quotes           | `"Isn't this fun?"`            | "Isn't this fun?" |
| Dashes           | `-- is en-dash, --- is em-dash` | -- is en-dash, --- is em-dash |


### Table of contents

You can insert a table of contents using the marker `[TOC]`:

[TOC]


### MathJax

You can render *LaTeX* mathematical expressions using **MathJax**, as on [math.stackexchange.com][1]:

The *Gamma function* satisfying $\Gamma(n) = (n-1)!\quad\forall n\in\mathbb N$ is via the Euler integral

$$
\Gamma(z) = \int_0^\infty t^{z-1}e^{-t}dt\,.
$$

> **Tip:** To make sure mathematical expressions are rendered properly on your website, include **MathJax** into your template:

```
<script type="text/javascript" src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS_HTML"></script>
```

> **Note:** You can find more information about **LaTeX** mathematical expressions [here][4].


### UML diagrams

You can also render sequence diagrams like this:

```sequence
Alice->Bob: Hello Bob, how are you?
Note right of Bob: Bob thinks
Bob-->Alice: I am good thanks!
```

And flow charts like this:

```flow
st=>start: Start
e=>end
op=>operation: My Operation
cond=>condition: Yes or No?

st->op->cond
cond(yes)->e
cond(no)->op
```

> **Note:** You can find more information:

> - about **Sequence diagrams** syntax [here][7],
> - about **Flow charts** syntax [here][8].

### Support StackEdit

[![](https://cdn.monetizejs.com/resources/button-32.png)](https://monetizejs.com/authorize?client_id=ESTHdCYOi18iLhhO&summary=true)

  [^stackedit]: [StackEdit](https://stackedit.io/) is a full-featured, open-source Markdown editor based on PageDown, the Markdown library used by Stack Overflow and the other Stack Exchange sites.

<!--Links-->
  [1]: http://math.stackexchange.com/
  [2]: http://daringfireball.net/projects/markdown/syntax "Markdown"
  [3]: https://github.com/jmcmanus/pagedown-extra "Pagedown Extra"
  [4]: http://meta.math.stackexchange.com/questions/5020/mathjax-basic-tutorial-and-quick-reference
  [5]: https://code.google.com/p/google-code-prettify/
  [6]: http://highlightjs.org/
  [7]: http://bramp.github.io/js-sequence-diagrams/
  [8]: http://adrai.github.io/flowchart.js/
  [請點我]: https://stackedit.io/viewer#!provider=gist&gistId=118f02f182d6ed400f94&filename=stack-edit
  [這裡]: http://iampaul83.blogspot.tw/2015/04/stackeditmarkdown.html
  [Paul <i class="icon-thumbs-up"></i>]: https://www.facebook.com/iampaul83
  [用StackEdit寫Markdown，並輸出至網誌!]: http://iampaul83.blogspot.tw/2015/04/stackeditmarkdown.html
  [Android兩個下拉重整的方法]: http://iampaul83.blogspot.tw/2015/04/android_76.html
  [MacBook Pro]: http://www.apple.com/tw/macbook-pro/
  [iPhone]: http://www.apple.com/tw/iphone/
  [Apple Watch]: http://www.apple.com/tw/watch/
  
<!--Images-->
  [編輯按鈕]: https://lh3.googleusercontent.com/-miyeDf8IxaM/VTdb9pTUlTI/AAAAAAAALek/_OGeyvsDRt4/s0/Screen+Shot+2015-04-22+at+16.28.33.png "編輯按鈕"
  [rename image]: https://lh3.googleusercontent.com/-jh1zFWERN4s/VTdGzqY6ObI/AAAAAAAALeQ/0zB-O7H7iOM/s0/Screen+Shot+2015-04-22+at+14.58.54.png "重新命名"

