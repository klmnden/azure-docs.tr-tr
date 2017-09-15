### <a name="noconnection"></a>Yerel ağ geçidinin IP adresi ön eklerini değiştirmek için - ağ geçidi bağlantısı yok

Ağ geçidi bağlantınız yoksa ve IP adresi önekleri eklemek veya kaldırmak istiyorsanız, yerel ağ geçidini oluşturmak için kullandığınız [az network local-gateway create](https://docs.microsoft.com/cli/azure/network/local-gateway#az_network_local_gateway_create) komutunu kullanın. VPN cihazının ağ geçidi IP adresini güncelleştirmek için de bu komutu kullanabilirsiniz. Geçerli ayarların üzerine yazmak için yerel ağ geçidinizin mevcut adını kullanın. Farklı bir ad kullanırsanız mevcut olanın üzerine yazmak yerine yeni bir yerel ağ geçidi oluşturursunuz.

Her değişiklik yaptığınızda, yalnızca değiştirmek istediğiniz ön ekler değil ön ek listesinin tamamı belirtilmelidir. Yalnızca kalmasını istediğiniz ön ekleri belirtin. Bu durumda, söz konusu ön ekler 10.0.0.0/24 ve 20.0.0.0/24’tür.

```azurecli
az network local-gateway create --gateway-ip-address 23.99.221.164 --name Site2 --connection-name TestRG1 --local-address-prefixes 10.0.0.0/24 20.0.0.0/24
```

### <a name="withconnection"></a>Yerel ağ geçidinin IP adresi ön eklerini değiştirmek için - ağ geçidi bağlantısı var

Bir ağ geçidi bağlantınız varsa ve IP adresi önekleri eklemek veya kaldırmak istiyorsanız önekleri [az network local-gateway update](https://docs.microsoft.com/cli/azure/network/local-gateway#az_network_local_gateway_update) komutunu kullanarak güncelleştirebilirsiniz. Bunun sonucunda, VPN bağlantınızda kesinti oluşur. IP adresi öneklerini değiştirirken, VPN ağ geçidini silmeniz gerekmez.

Her değişiklik yaptığınızda, yalnızca değiştirmek istediğiniz ön ekler değil ön ek listesinin tamamı belirtilmelidir. Bu örnekte, 10.0.0.0/24 ve 20.0.0.0/24 zaten mevcuttur. 30.0.0.0/24 ve 40.0.0.0/24 öneklerini ekliyor ve güncelleştirirken 4 öneki de belirtiyoruz.

```azurecli
az network local-gateway update --local-address-prefixes 10.0.0.0/24 20.0.0.0/24 30.0.0.0/24 40.0.0.0/24 --name VNet1toSite2 --connection-name TestRG1
```