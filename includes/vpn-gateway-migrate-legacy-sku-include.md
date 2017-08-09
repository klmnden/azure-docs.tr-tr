> [!NOTE]
> * Eski bir SKU'dan yeni bir SKU'ya geçiş yapılırken VPN ağ geçidi Genel IP adresi değişir.
> * Klasik VPN ağ geçitlerini yeni SKU’lara geçiremezsiniz. Klasik VPN ağ geçitleri yalnızca eski SKU'ları kullanabilir.
> 

Azure VPN ağ geçitlerinizi eski SKU'lar ve yeni SKU aileleri arasında yeniden boyutlandıramazsınız. Resource Manager dağıtım modelinde SKU'ların daha eski bir sürümünü kullanan VPN ağ geçitleriniz varsa yeni SKU'lara geçiş yapabilirsiniz. Geçiş yapmak için sanal ağınıza ilişkin mevcut VPN ağ geçidini silip yeni bir ağ geçidi oluşturursunuz.

Geçiş iş akışı:

1. Sanal ağ geçidine ilişkin tüm bağlantıları kesin.
2. Eski VPN ağ geçidini silin.
3. Yeni VPN ağ geçidini oluşturun.
4. Şirket içi VPN cihazlarınızdaki VPN ağ geçidi IP adresini (Siteden Siteye bağlantılar için) yenisi ile güncelleştirin.
5. Bu ağ geçidine bağlanacak tüm VNet-VNet yerel ağ geçitleri için ağ geçidi IP adresi değerini güncelleştirin.
6. Bu VPN ağ geçidi yoluyla sanal ağa bağlanan P2S istemcileri için yeni istemci VPN yapılandırma paketlerini indirin.
7. Sanal ağ geçidine ilişkin tüm bağlantıları yeniden oluşturun.