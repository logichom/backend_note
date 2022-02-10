# Aglio
#### Aglio

0.基礎知識

> Aglio是一種API Buleprint的Render，而API Buleprint是一種markdown，主要的用途是撰寫 API文件。用 markdown語法寫一個apib檔的API文件，再用Aglio的指令把它編譯成一個html檔，白話一點就是：撰寫一個md檔，然後輸出一個漂亮的API文件

1.先確認本機已安裝nodejs

```shell=
node -v
```

2.安裝nodejs

```shell=
#直接下載
https://nodejs.org/en/

#終端機
curl "https://nodejs.org/dist/latest/node-${VERSION:-$(wget -qO- https://nodejs.org/dist/latest/ | sed -nE 's|.*>node-(.*)\.pkg</a>.*|\1|p')}.pkg" > "$HOME/Downloads/node-latest.pkg" && sudo installer -store -pkg "$HOME/Downloads/node-latest.pkg" -target "/"

#homebrew
brew update
brew install node
```

3.安裝aglio

```shell
#會跑一段時間
npm install -g aglio
#有錯誤的話
#sudo npm install -g aglio --unsafe-perm
#加 --unsafe-perm 参数，这样就不会切换到 nobody 上，运行时是哪个用户就是哪个用户，即使是 root
#homebrew也有錯誤訊息產生但不影響後續指令操作
```

4.製作API文件範例(撰寫apib/md檔)

> 參考文件：https://apiblueprint.org/documentation/tutorial.html

5.將aglio檔案輸出成HTML

> 使用上有兩種做法，一種是透過 aglio 的 command line 指令產生網頁，而另一種是當成 node.js 的 library 使用，在 javascript 中產生

```shell=
#Default theme
aglio -i input.apib -o output.html
aglio -i input.md -o output.html #來源也可以是md檔

#Built-in color scheme
aglio --theme-variables slate -i index.apib -o index.html
aglio --theme-variables slate -i index.apib --server #看即時修改的結果

#aglio -h 可以看到 aglio 相關的全部指令，常用參數：
#-i, --input Input file
#-o, --output Output file
#-s, --server Start a local live preview server
```

6.DEMO

7.與其他API文件生成工具優缺點比較

|         | 優點                    | 缺點               |
| ------- | ----------------------- | ------------------ |
| Aglio   | 自訂性高且支援 Markdown | 不可以直接測試 API |
| Swagger | 可以直接測試 API        | 自訂性較低         |

8.遇到問題

- Parameters與Attributes 差異？ (參考備註)
- Response內Attributes跟Body同時存在，結果只顯示Attributes？ (參考備註)
- Response內Attributes如何做巢狀描述？ (參考範例)
- Attributes參數的範例無法給空值？

9.備註

+ Parameters: URI parameters should describe the URI using a list of Parameters.

+ Attributes: Another way of describing examples and the structure of your request and response content is by using [MSON](https://github.com/apiaryio/mson#readme).

+ URI Parameters: 用在URI的參數描述，使用`+ Parameters`做`list`的第一層，通常是用來描述URL上的變數

+ Response內Attributes跟Body同時存在時，若都有給範例只會顯示Attributes內容！

+ Attributes參數沒給時就算有設定在Body，範例還是會顯示"Hello World!" ！

+ Attributes不能單獨存在，要包在Request裡面！

+ 缺字範例值用`包起來`
