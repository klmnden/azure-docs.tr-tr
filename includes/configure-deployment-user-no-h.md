[az webapp deployment user set](/cli/azure/webapp/deployment/user#set) komutuyla dağıtım kimlik bilgileri oluşturun.

Bir web uygulamasına FTP ve yerel Git dağıtımı için dağıtım kullanıcısı gereklidir. Kullanıcı adı ve parola, hesap düzeyindedir. Bunlar Azure aboneliği kimlik bilgilerinizden farklıdır.

Aşağıdaki komutta *\<user-name>* ve *\<password>* kısımlarını yeni bir kullanıcı adı ve parola ile değiştirin.

```azurecli-interactive
az webapp deployment user set --user-name <username> --password <password>
```

Kullanıcı adı benzersiz olmalıdır. Parola, şu üç öğeyi de içerecek şekilde en az sekiz karakter uzunluğunda olmalıdır: harf, rakam, sembol. ` 'Conflict'. Details: 409` hatası alırsanız kullanıcı adını değiştirin. ` 'Bad Request'. Details: 400` hatası alırsanız daha güçlü bir parola kullanın.

Bu dağıtım kullanıcısını bir kez oluşturduktan sonra tüm Azure dağıtımlarınız için kullanabilirsiniz.

> [!NOTE]
> Kullanıcı adını ve parolayı kaydedin. Daha sonra web uygulamasının dağıtımı için bunlara ihtiyacınız olacaktır.
>
>