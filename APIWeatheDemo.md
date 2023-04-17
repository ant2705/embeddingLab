从API获取网络天气信息

要从网络上获取天气信息，首先需要一个天气API。知乎上有大量的API推荐，在这里不再赘述。

从API返回的天气信息一般都是json格式的。我们需要从json中提取出我们想要的信息。这时，大家的第一想法都是解析json。在这里，提供另外一种“偷懒”的办法：直接拆分字符串。

本次的实验平台是ESP32C3，使用Arduino IDE，实验要求获取网络天气信息。（实际上老师的意思是具体的天气如“Cloudy”"Sand""Sunny"+温度值）

首先加入必须的头文件，并将自己的API粘贴进来。

```c
#include<HTTPClient.h>
```

```c
HTTPClient http;
http.begin("https://api.seniverse.com/v3/weather/now.json?key=【替换为YOUR_SECRET_KEY】&location=beijing&language=en&unit=c");
```

以下代码可以直接从HTTP获取字符串。

```java
int httpCode = http.GET();
String weatherInfo = "";
if (httpCode > 0) {
	if (httpCode == HTTP_CODE_OK) {
        String payload = http.getString();
		Serial.println(payload);
 		// get info from payload
        // using String indexOf function
}
else{
    Serial.print("failed");
}
http.end();
```

之后拆分字符串。我们首先观察http返回的字符串。

>```
>{"results":[{"location":{"id":"WX4FBXXFKE4F","name":"Beijing","country":"CN","path":"Beijing,Beijing,China","timezone":"Asia/Shanghai","timezone_offset":"+08:00"},"now":{"text":"Cloudy","code":"4","temperature":"16"},"last_update":"2023-04-17T21:32:18+08:00"}]}
>```

我们只需要其中的now下的text和temperature部分。字符串拆分可以使用indexOf函数，其返回值的*要查找的字符串*的起始字符在原字符串payload中的索引。具体的代码不再赘述

```java
int t = payload.indexOf("要查找的字符串");
```



