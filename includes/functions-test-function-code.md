## <a name="test"></a>İşlevi test etme

Dağıtılan işlevi bir Mac veya Linux bilgisayarda ya da Windows üzerinde Bash kullanarak test etmek için cURL kullanın. `<app_name>` yer tutucusunu işlev uygulamanızın adıyla değiştirerek aşağıdaki cURL komutunu yürütün. `&name=<yourname>` sorgu dizesini URL’ye ekleyin.

```bash
curl https://<app_name>.azurewebsites.net/api/HttpTrigger?name=<yourname>
```  

![Tarayıcıda gösterilen işlev yanıtı.](./media/functions-test-function-code/functions-azure-cli-function-test-curl.png)  

Komut satırınızda cURL yoksa, web tarayıcınızın adres çubuğuna aynı URL'yi girin. Burada da `<app_name>` yer tutucusunu işlev uygulamanızın adıyla değiştirin ve `&name=<yourname>` sorgu dizesini URL’ye ekleyip isteği yürütün.

    https://<app_name>.azurewebsites.net/api/HttpTrigger?name=<yourname>
   
![Tarayıcıda gösterilen işlev yanıtı.](./media/functions-test-function-code/functions-azure-cli-function-test-browser.png)  
