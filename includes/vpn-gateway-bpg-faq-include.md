### BGP tüm Azure VPN Gateway SKU'larında destekleniyor mu?
Hayır, BGP Azure **Standart**’ta ve **HighPerformance** VPN Gateway’lerinde desteklenir. **Temel** SKU DESTEKLENMEZ.

### Azure İlke Temelli VPN ağ geçitleriyle BGP kullanabilir miyim?
Hayır, BGP yalnızca Rota Temelli VPN Gateway’lerinde desteklenir.

### Özel ASN’ler (Otonom Sistem Numaralarını) kullanabilir miyim?
Evet, kendi ortak ASN’lerinizi veya özel ASN’lerinizi hem şirket içi ağlarda, hem de Azure sanal ağlarda kullanabilirsiniz.

#### Azure tarafından ayrılan bir ASN var mı?
Evet, aşağıdaki ASN’ler iç ve dış eşlemeler için Azure tarafından ayrılmıştır:

* Ortak ASN’ler: 8075, 8076, 12076
* Özel ASN’ler: 65515, 65517, 65518, 65519, 65520

Bu ASN’leri Azure VPN ağ geçitlerine bağlanırken şirket içi VPN cihazlarınız için belirtemezsiniz.

### Aynı ASN’yi hem şirket içi VPN ağlarında, hem de Azure VNet'lerde kullanabilir miyim?
Hayır, BGP’yle bunlara birlikte bağlanıyorsanız, şirket içi ağlar ve Azure VNet'ler arasında farklı ASN’ler atamanız gerekir. BGP ister şirket içi ve dışı karışık bağlantı için etkin olsun, ister olmasın, Azure VPN Gateway’lerinde atanmış varsayılan 65515 ASN’si vardır. VPN Gateway oluştururken farklı ASN atayarak bu varsayılan değeri geçersiz kılabilir veya ağ geçidini oluşturduktan sonra ASN’yi değiştirebilirsiniz. Şirket içi ASN’lerinizi ilgili Azure Yerel Ağ Geçitlerine atamanız gerekir.

### Azure VPN Gateway’ler bana hangi adres öneklerini tanıtacak?
Azure VPN Gateway şirket içi BGP cihazlarınıza şu rotaları tanıtacaktır:

* VNet adres önekleriniz
* Azure VPN Gateway’e bağlı her Yerel Ağ Geçidi için adres önekleri
* Azure VPN Gateway’e bağlı diğer BGP eşdeğer oturumlarından öğrenilen rotalar; **varsayılan rota ve herhangi bir VNet önekiyle çakışan rotalar dışında**.

#### Varsayılan yolu (0.0.0.0/0) Azure VPN ağ geçitlerine tanıtabilir miyim?
Şu anda değil.

#### Tam ön eklerimi Sanal Ağ ön eklerim olarak tanıtabilir miyim?
Hayır, aynı ön eklerin Sanal Ağ adresi ön eklerinden biri olarak tanıtılması Azure platformu tarafından engellenir veya filtrelenir. Ancak, Sanal Ağınızda bulunanların üst kümesi olan bir ön ek tanıtabilirsiniz. Örneğin, Sanal Ağınız 10.10.0.0/16 adres aralığını kullanabilir ve siz de 10.0.0.0/8 aralığını tanıtabilirsiniz.

### VNet - VNet bağlantılarımla BGP kullanabilir miyim?
Evet, BGP’yi hem şirket içi bağlantılar için, hem de VNet - VNet bağlantıları için kullanabilirsiniz.

### Azure VPN Gateway’lerim için BGP’yi BGP olmayan bağlantılarla karıştırabilir miyim?
Evet, aynı Azure VPN Gateway için BGP ve BGP olmayan bağlantıları karıştırabilirsiniz.

### Azure VPN Gateway BGP transit rotasını destekliyor mu?
Evet, BGP transit rotası desteklense de, özel durum olarak Azure VPN Gateway’lerinin varsayılan rotaları diğer BGP eşdeğerlerine **TANITMAMALARI** geçerlidir. Transit rotayı birden fazla Azure VPN Gateway’de etkinleştirmek için tüm ara VNet - VNet bağlantılarında BGP’yi etkinleştirmelisiniz.

### Azure VPN Gateway ve şirket içi ağım arasında birden fazla tünelim olabilir mi?
Evet, bir Azure VPN Gateway ve şirket içi ağınız arasında birden fazla S2S tüneli kurabilirsiniz. Bu tünellerin tümünün Azure VPN Gateway’lerinize yönelik tünellerin toplam sayısına karşılık hesaplanacağını lütfen unutmayın. Örneğin, Azure VPN Gateway’iniz ve şirket içi ağınız arasında iki yedekli tüneliniz varsa, Azure VPN Gateway’inizin toplam kotasından (Standard için 10, HighPerformance için de 30) 2 tünel tüketeceklerdir.

### BGP bulunan iki Azure VNet arasında birden çok tünelim olabilir mi?
Hayır, bir sanal ağ çifti arasında yedekli tünel desteklenmez.

### BGP’yi ExpressRoute/S2S VPN birlikte varolma yapılandırmasında S2S VPN için kullanabilir miyim?
Şu anda değil.

### Azure VPN Gateway BGP Eşdeğer IP’si için hangi adresi kullanıyor?
Azure VPN Gateway sanal ağ için ayrılan GatewaySubnet aralığından tek bir IP adresi ayırır. Varsayılan olarak, aralığın sondan ikinci adresidir. Örneğin, GatewaySubnet’iniz 10.12.255.0/27 olursa, 10.12.255.0 ve 10.12.255.31 arasında sıralanıyorsa, o zaman Azure VPN Gateway’deki BGP Eşdeğer IP adresi 10.12.255.30 olacaktır. Azure VPN Gateway bilgilerini listelediğinizde bu bilgileri bulabilirsiniz.

### VPN cihazımdaki BGP Eşdeğer IP adreslerinin gereksinimleri nelerdir?
Şirket içi BGP eşdeğer adresinizin, VPN cihazınızın genel IP adresiyle aynı olmaması **GEREKİR**. BGP Eşdeğer IP’si için VPN cihazında farklı bir IP adresi kullanın. Bu aygıttaki geri döngü arabirimine atanmış bir adres olabilir. Bu adresi, konumu temsil eden ilgili Yerel Ağ Geçidi’nde belirtin.

### BGP kullandığımda Yerel Ağ Geçidi için adres önekim olarak ne belirtmeliyim?
Azure Yerel Ağ Geçidi, şirket içi ağ için başlangıç adresi öneklerini belirtir. BGP ile, BGP Eşdeğer IP adresinizin konak önekini (/32 önek) bu şirket içi ağın adres alanı olarak ayırmalısınız. BGP Eşdeğer IP’niz 10.52.255.254 olursa, bu şirket içi ağda temsil edilen Yerel Ağ Geçidine ait "10.52.255.254/32" olarak belirtmelisiniz. Azure VPN Gateway’in S2S VPN tüneli aracılığıyla BGP oturumunu başlatmasını sağlamak içindir.

### BGP eşdeğer oturumu için şirket içi VPN cihazıma ne eklemeliyim?
VPN cihazınızdaki Azure BGP Eşdeğer IP adresinin, IPsec S2S VPN tüneline yönlenmiş konak rotasını eklemelisiniz. Örneğin, Azure VPN Eşdeğer IP’si "10.12.255.30" olursa, "10.12.255.30" için VPN cihazınızdaki eşleşen IPsec tüneli arabiriminin sonraki durak arabirimine sahip bir konak rotası eklemelisiniz.

<!--HONumber=Sep16_HO3-->


