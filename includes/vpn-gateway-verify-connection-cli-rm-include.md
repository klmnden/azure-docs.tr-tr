Aşağıdaki CLI komutunu kullanarak bağlantınızın başarılı olup olmadığını doğrulayabilirsiniz. Değerleri, kendi değerlerinizle eşleşecek şekilde yapılandırın. Örnekte, -n oluşturduğunuz ve test etmek istediğiniz bağlantının adıdır.

```azurecli
az network vpn-connection show -n VNet1toSite2 -g TestRG1
```

Bağlantı henüz kurulma aşamasındayken, bağlantı durumu 'Bağlanıyor' olarak gösterilir. Bağlantı kurulduktan sonra, durum "Bağlandı" olarak değişir; giriş ve çıkış baytlarını görebilirsiniz.