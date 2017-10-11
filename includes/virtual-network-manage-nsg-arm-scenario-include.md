## <a name="sample-scenario"></a>Örnek senaryo
Nsg'ler yönetmek nasıl daha iyi anlamak için bu makalede aşağıdaki senaryoyu kullanır.

![VNet senaryosu](./media/virtual-networks-create-nsg-scenario-include/figure1.png)

Bu senaryoda, her alt ağ için bir NSG oluşturacak **TestVNet** aşağıda açıklandığı sanal ağ: 

* **NSG ön uç**. NSG uygulanacak ön uç *ön uç* alt ağı ve iki kural içerir:    
  * **RDP kural**. Bu kural için RDP trafiğine izin verir *ön uç* alt ağ.
  * **Web kuralı**. Bu kural için HTTP trafiğine olanak tanıyacak *ön uç* alt ağ.
* **NSG arka uç**. NSG uygulanacak arka uç *arka uç* alt ağı ve iki kural içerir:    
  * **SQL kural**. Bu kural yalnızca SQL trafiğe izin veren *ön uç* alt ağ.
  * **Web kuralı**. Bu kural, tüm Internet trafiği bağlı engellediği *arka uç* alt ağ.

Bu kurallar birleşimi burada arka uç alt ağ ön uç alt ağından gelen trafiği SQL trafik için yalnızca alabilir ve ön uç alt Internet ile iletişim kurmak ve alma hiçbir Internet erişimi DMZ benzeri senaryosu, oluşturma yalnızca gelen HTTP isteklerini için.

Yukarıda açıklanan senaryoyu dağıtmak için izlemeniz [bu bağlantıyı](http://github.com/telmosampaio/azure-templates/tree/master/201-IaaS-WebFrontEnd-SQLBackEnd-NSG), tıklatın **Azure'a Dağıt**, gerekirse varsayılan parametre değerlerini değiştirin ve Portalı'ndaki yönergeleri izleyin. Aşağıdaki örnek yönergelerinde şablonu bir kaynak grubu adları dağıtmak için kullanılan **RG NSG**. 

