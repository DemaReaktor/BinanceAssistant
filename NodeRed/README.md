# BinanceAssistant
Binance Assistant help save, open data about tokens. 
Ukrainian language is below(українська нижче)
# Content
[Node properties](#Node-properties)

[Укр](#Укр)

[Code](#Code)

[Example](#Example)

# Installs
![](https://github.com/DemaReaktor/BinanceAssistant/tree/NodeRed/NodeRed/images/installs.png)
# Nodes
Blue object is **start-up-trigger**, dark and green is **fs-ops-access**, object **table** is **table** from 
**node-red-node-ui-table**. Objects **pair**, **price** and **amount** are **text input**. **read-file** is **read file**, **write-file** is  **write file**. **update dashboard** is **ui control**, **if-finish** – **switch**, **get-price-now** - **http request**. 
Other objects are **button** or **function**.
![](https://github.com/DemaReaktor/BinanceAssistant/tree/NodeRed/NodeRed/images/nodes.png)
# Groups
Groups are two. There are **pair**, **price**, **amount** and **button_add** in first group, other are in second group.
# Node properties
There are node properties in photos.
![](https://github.com/DemaReaktor/BinanceAssistant/tree/NodeRed/NodeRed/images/start.png)

**open** has standart properties expend payload has data.txt (filename).
**pair price and amount** has same properies (pair - price - amount)

![](https://github.com/DemaReaktor/BinanceAssistant/tree/NodeRed/NodeRed/images/pair.png)
![](https://github.com/DemaReaktor/BinanceAssistant/tree/NodeRed/NodeRed/images/isFile.png)

There is path to file in **read-file** -> Filename, I have E://data.txt.
 Every column has name and property: (name - property) **pair-pair, price-price, price now-price_now, per cent-cent, amount-amount, time-time, delete-delete**.

![](https://github.com/DemaReaktor/BinanceAssistant/tree/NodeRed/NodeRed/images/table.png)
![](https://github.com/DemaReaktor/BinanceAssistant/tree/NodeRed/NodeRed/images/update.png)
![](https://github.com/DemaReaktor/BinanceAssistant/tree/NodeRed/NodeRed/images/element.png)
![](https://github.com/DemaReaktor/BinanceAssistant/tree/NodeRed/NodeRed/images/switch.png)
![](https://github.com/DemaReaktor/BinanceAssistant/tree/NodeRed/NodeRed/images/request.png)
![](https://github.com/DemaReaktor/BinanceAssistant/tree/NodeRed/NodeRed/images/write.png)

Other properies  are standart.

[back to content](#Content)
# Укр
# Пакети
![](https://github.com/DemaReaktor/BinanceAssistant/tree/NodeRed/NodeRed/images/installs.png)
# Ноди
Синім об'єктом є start-up-trigger, темнозеленим - fs-ops-access, об'єктом з назвою table є table з 
node-red-node-ui-table. Об'єкти pair, price i amount є text input. Об'єкт read-file є read file, write-file -  write file. Об'єкт update dashboard є ui control, if-finish – switch, get-price-now - http request. інші об'єкти є або функціями, або кнопками.
![](https://github.com/DemaReaktor/BinanceAssistant/tree/NodeRed/NodeRed/images/nodes.png)
# Групи
Їх лише дві. pair, price, amount i button_add у першій групі, інші в іншій
# Властивості нодів
Властивості наведені у фото
![](https://github.com/DemaReaktor/BinanceAssistant/tree/NodeRed/NodeRed/images/start.png)

У кнопці open стандартні властивості, тільки у payload має бути data.txt (назва файлу).
pair price i amount мають відповідні властивості

![](https://github.com/DemaReaktor/BinanceAssistant/tree/NodeRed/NodeRed/images/pair.png)
![](https://github.com/DemaReaktor/BinanceAssistant/tree/NodeRed/NodeRed/images/isFile.png)

У read-file у Filename має бути шлях до файла, у мене E://data.txt.
У об'єкт table добавляєм властивості як на фото. Кожна колонка містить ім'я і властивість: (name - property) pair-pair, price-price, price now-price_now, per cent-cent, amount-amount, time-time, delete-delete.

![](https://github.com/DemaReaktor/BinanceAssistant/tree/NodeRed/NodeRed/images/table.png)
![](https://github.com/DemaReaktor/BinanceAssistant/tree/NodeRed/NodeRed/images/update.png)
![](https://github.com/DemaReaktor/BinanceAssistant/tree/NodeRed/NodeRed/images/element.png)
![](https://github.com/DemaReaktor/BinanceAssistant/tree/NodeRed/NodeRed/images/switch.png)
![](https://github.com/DemaReaktor/BinanceAssistant/tree/NodeRed/NodeRed/images/request.png)
![](https://github.com/DemaReaktor/BinanceAssistant/tree/NodeRed/NodeRed/images/write.png)

Інші властивості стандартні.

[до змісту](#Content)
# Code

start
```js
var pair = new Object();
var price = new Object();
var amount = new Object();
pair.payload = "BTCUSDT";
price.payload = "1";
amount.payload = "1";
msg.payload = "data.txt";

return [msg, pair, price, amount];
```

global_property_add
```js
global.set(msg.topic, msg.payload);
```

add
```js
global.get("row").push({ "pair": global.get("pair"),
    "price": global.get("price"), "amount": 
        global.get("amount"), "time": new Date().toLocaleString(), 
        "price_now": "not update", "cent":"no data",
         "delete": "delete"});
    
return msg;
```

update table
```js
msg.payload = global.get("row");
var data = new Object();
data.payload = "";
return [data,msg];
```

get-data
```js
msg.payload = global.get("row");
return msg;
```

set-data
```js
global.set("row", JSON.parse(msg.payload));
msg.payload = global.get("row");
return msg;
```

delete
```js
if(msg.topic == "delete")
{
    var rows = global.get("row");
    rows.pop(rows[msg.row]);
    return msg;
}
```

update-function
```js
msg.payload = global.get("row");

var copy = new Set();
global.get("row").forEach(function(element) {
        copy.add(element["pair"]);
});

global.set("update", copy);
msg.payload = "start";
var time = new Object();
time.payload = new Date().toLocaleString();
return [msg, time];
```

element
```js
var first = global.get("update").values().next().value;

if (msg.payload != "start"){
    global.get("row").forEach(function(element) {
        if (element["pair"] == first){
            element["price_now"] =
                JSON.parse(msg.payload)["price"];
                element["cent"] = element["price"]/
                element["price_now"];
        }
    });
    global.get("update").delete(first);
        }

if (global.get("update").size <= 0) {
    msg.payload = "finish";
    return msg;
}

var pair = global.get("update").values().next().value;
msg.url = "https://www.binance.com/api/v3/ticker/price?symbol="
    + pair;

return msg;
```
[back to content](#Content)

# Example
![](https://github.com/DemaReaktor/BinanceAssistant/tree/NodeRed/NodeRed/images/example.png)