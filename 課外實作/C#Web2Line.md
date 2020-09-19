# Asp.Net與Line Notify連動

```c#
//連動 Line Notify

HttpWebRequest request = HttpWebRequest.Create("https://notify-api.line.me/api/notify") as HttpWebRequest;
request.Method = "POST";
request.ContentType = "application/x-www-form-urlencoded";
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