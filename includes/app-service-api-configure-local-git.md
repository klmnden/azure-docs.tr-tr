[az webapp source-control config-local-git](/cli/azure/appservice/web/source-control#config-local-git) komutuyla yerel Git dağıtımını API uygulamasında gerçekleşecek şekilde yapılandırın.   

App Service, web uygulamasına içerik dağıtmak için FTP, yerel Git, GitHub, Visual Studio Team Services ve Bitbucket gibi çeşitli yolları destekler. Bu hızlı başlangıç için yerel Git’i kullanarak dağıtım gerçekleştirirsiniz. Bu, yerel bir depodan Azure’daki bir depoya gönderim yapmak için bir Git komutunu kullanarak dağıtım yaptığınız anlamına gelir.  

Kodu gönderirken kullanacağınız hesap düzeyinde dağıtım kimlik bilgilerini ayarlamak için aşağıdaki betiği kullanın ve kullanıcı adı ve parola kısımları için kendi değerlerinizi girdiğinizden emin olun.   

```azurecli-interactive
az webapp deployment user set --user-name <desired user name> --password <desired password>
```
