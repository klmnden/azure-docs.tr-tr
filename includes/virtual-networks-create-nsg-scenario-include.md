## <a name="scenario"></a>Senaryo
Nsg'ler oluşturmak nasıl daha iyi anlamak için bu belge aşağıdaki senaryoyu kullanır.

![VNet senaryosu](./media/virtual-networks-create-nsg-scenario-include/figure1.png)

Bu senaryoda, her alt ağ için bir NSG oluşturacak **TestVNet** aşağıda açıklandığı sanal ağ: 

* **NSG ön uç**. NSG uygulanacak ön uç *ön uç* alt ağı ve iki kural içerir:    
  * **RDP kural**. Bu kural için RDP trafiğine izin verir *ön uç* alt ağ.
  * **Web kuralı**. Bu kural için HTTP trafiğine olanak tanıyacak *ön uç* alt ağ.
* **NSG arka uç**. NSG uygulanacak arka uç *arka uç* alt ağı ve iki kural içerir:    
  * **SQL kural**. Bu kural yalnızca SQL trafiğe izin veren *ön uç* alt ağ.
  * **Web kuralı**. Bu kural, tüm Internet trafiği bağlı engellediği *arka uç* alt ağ.

Bu kurallar birleşimi burada arka uç alt yalnızca SQL için ön uç alt ağından gelen trafik alabilir ve ön uç alt Internet ile iletişim kurmak ve alma hiçbir Internet erişimi DMZ benzeri senaryosu, oluşturma yalnızca gelen HTTP isteklerini için.

