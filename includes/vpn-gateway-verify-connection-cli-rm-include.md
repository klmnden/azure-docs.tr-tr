[az network vpn-connection show](/cli/azure/network/vpn-connection#show) komutunu kullanarak bağlantınızın başarılı olup olmadığını doğrulayabilirsiniz. Değerleri, kendi değerlerinizle eşleşecek şekilde yapılandırın. Örnekte --name, oluşturduğunuz ve test etmek istediğiniz bağlantının adıdır.

```azurecli
az network vpn-connection show --name VNet1toSite2 --resource-group TestRG1
```

Bağlantı henüz kurulma aşamasındayken, bağlantı durumu 'Bağlanıyor' olarak gösterilir. Bağlantı kurulduktan sonra, durum "Bağlandı" olarak değişir; giriş ve çıkış baytlarını görebilirsiniz.