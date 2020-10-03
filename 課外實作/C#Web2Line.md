# Asp.Net與Line Notify連動

## 製作動機

這是我在實習的時候，本來公司要我寫一個表單可以給使用者回報損壞訊息，然後把相關資料存進資料表裡面，這時候我就想到之前上課時有將錯誤訊息發送至Line Notify，那網頁應該也可以做到，而且我覺得直接發送到Line比維護人員去資料庫裡看更快，所以我就做了這個功能。

## 製作方法

首先要先去[Line Notify](https://notify-bot.line.me/zh_TW/)申請個人權杖，然後以Post的方式請求`https://notify-api.line.me/api/notify`，而需要在請求裡加上一些資料，詳細請求如下表所示

- 詳細請求

   Type|Key|Value
   -|-|-
   Header|Authorization|Bearer 個人權杖
   Body|Message|訊息

   注意事項:Bearer與權杖中間需要有一個空格
## 實作畫面

![](img/C%23Web2Line/1.png)

## 程式碼

```C#
//連動 Line Notify

HttpWebRequest request = HttpWebRequest.Create("https://notify-api.line.me/api/notify") as HttpWebRequest;
request.Method = "POST";
request.ContentType = "application/x-www-form-urlencoded";

// %0D%0A是\r\n
string postdata = "message=%0D%0A姓名:" + TextBox1.Text + ",%0D%0A電話號碼:" + TextBox2.Text + ",%0D%0A站台:" + DropDownList1.SelectedValue + ",%0D%0A車號:" + TextBox3.Text + ",%0D%0A內容:" + TextBox4.Text; 

byte[] data = Encoding.UTF8.GetBytes(postdata);
string token = "權杖請輸入於此";
request.Headers.Set("Authorization", "Bearer " + token);
request.ContentLength = data.Length;
using (Stream st = request.GetRequestStream())
{
   st.Write(data, 0, data.Length);
}

string responseStr = "";
using (WebResponse response = request.GetResponse())
{
   using (StreamReader sr = new StreamReader(response.GetResponseStream(), Encoding.UTF8))
   {
       responseStr = sr.ReadToEnd();
   }//end using  
}
```

---

**參考資料:**

- [Asp.net連動Line notify教學](https://youyouyou.pixnet.net/blog/post/118168077-line-bot-notify-%E8%81%8A%E5%A4%A9%E6%A9%9F%E5%99%A8%E4%BA%BA-%E4%BD%BF%E7%94%A8-asp.net-vb.net)