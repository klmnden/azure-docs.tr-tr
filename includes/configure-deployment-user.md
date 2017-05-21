## <a name="configure-a-deployment-user"></a>Dağıtım kullanıcısı yapılandırma  

FTP ve yerel Git için dağıtım kimliğini doğrulamak amacıyla sunucu üzerinde bir dağıtım kullanıcısı yapılandırılmış olmalıdır.

> [!NOTE]
> Bir Web Uygulamasına FTP ve Yerel Git dağıtımı için dağıtım kullanıcısı gereklidir.
> `username` ve `password` hesap düzeyindedir. Bunlar Azure Aboneliği kimlik bilgilerinizden farklıdır.
>

Dağıtım kimlik bilgilerinizi oluşturmak için [az appservice web deployment user set](/cli/azure/appservice/web/deployment/user#set) komutunu çalıştırın.

```azurecli
az appservice web deployment user set --user-name <username> --password <password>
```

Kullanıcı adının benzersiz, parolanın da güçlü olması gerekir. ` 'Conflict'. Details: 409` hatası alırsanız kullanıcı adını değiştirin. ` 'Bad Request'. Details: 400` hatası alırsanız daha güçlü bir parola kullanın.

Bu dağıtım kullanıcısını bir kez oluşturduktan sonra tüm Azure dağıtımlarınız için kullanabilirsiniz.

Uygulamayı dağıtırken ihtiyaç duyacağınız için kullanıcı adını ve parolayı bir yere kaydedin.