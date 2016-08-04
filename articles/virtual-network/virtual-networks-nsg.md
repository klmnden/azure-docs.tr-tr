<properties 
   pageTitle="Ağ Güvenlik Grubu (NSG) nedir?"
   description="Ağ Güvenlik Gruplarını (NSG'ler) kullanarak Azure'daki dağıtılmış güvenlik duvarı hakkında, ayrıca sanal ağlarınızdaki (VNet'ler) trafiği yalıtmak ve denetlemek için NSG'lerin nasıl kullanılacağı konusunda bilgi edinin."
   services="virtual-network"
   documentationCenter="na"
   authors="telmosampaio"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/11/2016"
   ms.author="telmos" />

# Ağ Güvenlik Grubu (NSG) nedir?

Ağ güvenlik grubu (NSG), bir Virtual Network üzerindeki VM örneklerinize ağ trafiğinin gitmesine izin veren veya reddeden Erişim Denetimi Listesi (ACL) kurallarının bir listesini içerir. NSG'ler alt ağlarla veya bu alt ağların içindeki tekil VM örnekleriyle ilişkili olabilir. Bir NSG bir alt ağ ile ilişkili olduğunda, ACL kuralları bu alt ağdaki tüm VM örnekleri için geçerli olur. Ayrıca, bir NSG doğrudan tekil bir VM ile ilişkilendirildiği zaman bu VM'ye giden trafik daha da kısıtlanabilir.

## NSG kaynağı

NSG'ler aşağıdaki özellikleri içerir.

|Özellik|Açıklama|Kısıtlamalar|Dikkat edilmesi gerekenler|
|---|---|---|---|
|Ad|NSG'nin adı|Bölge içinde benzersiz olmalıdır<br/>Harf, sayı, alt çizgi, nokta ve kısa çizgi içerebilir<br/>Bir harf veya sayı ile başlamalıdır<br/>Bir harf, sayı veya alt çizgi ile bitmelidir<br/>En fazla 80 karakter uzunluğunda olabilir|Birden fazla NSG oluşturmanız gerekebileceği için NSG'lerinizin işlevini tanımlamanızı kolaylaştıran bir adlandırma kuralınızın bulunduğundan emin olun|
|Bölge|NSG'nin barındırıldığı Azure bölgesi|NSG'ler yalnızca oluşturuldukları bölge içindeki kaynaklara uygulanabilir|Bir bölgede kaç adet NSG'ye sahip olabileceğinizi anlamak için aşağıdaki [sınırlar](#Limits) bölümüne bakın|
|Kaynak grubu|NSG'nin ait olduğu kaynak grubu|Bir NSG bir kaynak grubuna ait olsa da, kaynağın NSG'nin ait olduğu Azure bölgesinin bir parçası olması koşuluyla, NSG herhangi bir kaynak grubuyla ilişkilendirilebilir|Kaynak grupları, birden fazla kaynak grubunun birlikte bir dağıtım birimi olarak yönetilmesi için kullanılır<br/>NSG'yi ilişkili olduğu kaynaklarla gruplandırmayı değerlendirebilirsiniz|
|Kurallar|Hangi trafiğe izin verildiğini veya reddedildiğini tanımlayan kurallar||Aşağıdaki [NSG kuralları](#Nsg-rules) bölümüne bakın| 

>[AZURE.NOTE] Uç nokta tabanlı ACL'ler ve ağ güvenlik grupları, aynı VM örneğinde desteklenmez. Bir NSG'yi kullanmak istiyorsanız ve bir uç nokta ACL'si zaten kullanılıyorsa öncelikle uç nokta ACL'sini kaldırın. Bunun nasıl yapılacağı hakkında bilgi için bkz. [Uç Noktalar için Erişim Denetimi Listelerini (ACL'ler) PowerShell kullanarak yönetme](virtual-networks-acl-powershell.md).

### NSG kuralları

NSG kuralları aşağıdaki özellikleri içerir.

|Özellik|Açıklama|Kısıtlamalar|Dikkat edilmesi gerekenler|
|---|---|---|---|
|**Ad**|Kuralın adı|Bölge içinde benzersiz olmalıdır<br/>Harf, sayı, alt çizgi, nokta ve kısa çizgi içerebilir<br/>Bir harf veya sayı ile başlamalıdır<br/>Bir harf, sayı veya alt çizgi ile bitmelidir<br/>En fazla 80 karakter uzunluğunda olabilir|Bir NSG içinde çeşitli kurallara sahip olabilirsiniz, bu nedenle kuralınızın işlevini tanımlayan bir adlandırma kuralını uyguladığınızdan emin olun|
|**Protokol**|Kural ile eşleştirilecek protokol|TCP, UDP veya \*|Bir protokol olarak \* kullanmak ICMP'yi (yalnızca Doğu-Batı trafiği), aynı zamanda UDP'yi ve TCP'yi içerir ve ihtiyacınız olan kuralların sayısını azaltabilir<br/>Bununla birlikte \* kullanmak çok geniş bir yaklaşım olabilir, bu nedenle yalnızca gerçekten gerekli olduğu zaman kullandığınızdan emin olun|
|**Kaynak bağlantı noktası aralığı**|Kural ile eşleştirilecek kaynak bağlantı noktası|1'den 65535'e kadar olan tek bağlantı noktası, bağlantı noktası aralığı (yani 1-65635) veya \* (tüm bağlantı noktaları için)|Kaynak bağlantı noktaları kısa ömürlü olabilir. İstemci programınız belirli bir bağlantı noktasını kullanmadığı sürece, lütfen çoğu durum için "*" kullanın.<br/>Birden çok kurala ihtiyaç duyulmasını önlemek için mümkün olduğunca bağlantı noktası aralıklarını kullanmaya çalışın<br/>Birden çok bağlantı noktası veya bağlantı noktası aralığı virgülle birleştirilemez
|**Hedef bağlantı noktası aralığı**|Kural ile eşleştirilecek hedef bağlantı noktası aralığı|1'den 65535'e kadar olan tek bağlantı noktası, bağlantı noktası aralığı (yani 1-65535) veya \* (tüm bağlantı noktaları için)|Birden çok kurala ihtiyaç duyulmasını önlemek için mümkün olduğunca bağlantı noktası aralıklarını kullanmaya çalışın<br/>Birden çok bağlantı noktası veya bağlantı noktası aralığı virgülle birleştirilemez
|**Kaynak adres ön eki**|Kural ile eşleştirilecek kaynak adres ön eki veya etiketi|Tek IP adresi (örneğin, 10.10.10.10), IP alt ağı (örneğin, 192.168.1.0/24), [varsayılan etiket](#Default-Tags) veya * (tüm adresler için)|Kuralların sayısını azaltmak için aralıklar, varsayılan etiketler ve * kullanmayı değerlendirin|
|**Hedef adres ön eki**|Kural ile eşleştirilecek hedef adres ön eki veya etiketi|tek IP adresi (örneğin, 10.10.10.10), IP alt ağı (örneğin, 192.168.1.0/24), [varsayılan etiket](#Default-Tags) veya * (tüm adresler için)|Kuralların sayısını azaltmak için aralıklar, varsayılan etiketler ve * kullanmayı değerlendirin|
|**Yön**|Kural için eşleştirilecek trafik yönü|gelen veya giden|Gelen veya giden kuralları, yöne bağlı olarak ayrı ayrı işlenir|
|**Öncelik**|Kurallar öncelik sırasına göre denetlenir, bir kural uygulandığı zaman başka hiçbir kural eşleştirme için test edilmez|100 ile 4096 arasında bir sayı|Var olan kuralların arasına gelecek yeni kurallara alan bırakmak amacıyla, her kural için öncelikleri 100'lü adımlarla atlayarak kuralları oluşturmayı değerlendirin|
|**Erişim**|Kuralın eşleşmesi durumunda uygulanacak erişim türü|izin ver veya reddet|Bir paket için izin verme kuralı bulunmazsa paketin bırakılacağını göz önünde bulundurun|

NSG'ler iki kural kümesi içerir: gelen ve giden. Bir kurala ait öncelik her küme içinde benzersiz olmalıdır. 

![NSG kuralının işlenmesi](./media/virtual-network-nsg-overview/figure3.png) 

Yukarıdaki şekilde NSG kurallarının nasıl işlendiği gösterilmektedir.

### Varsayılan Etiketler

Varsayılan etiketler, bir IP adresi kategorisini belirtmek için sistem tarafından sağlanan tanımlayıcılardır. Herhangi bir kuralın **kaynak adres ön eki** ve **hedef adres ön eki** özelliklerinde varsayılan etiketleri kullanabilirsiniz. Kullanabileceğiniz üç varsayılan etiket vardır.

- **VIRTUAL_NETWORK:** Bu varsayılan etiket tüm ağ adresi alanınızı belirtir. Sanal ağ adresi alanını (Azure'da tanımlanan CIDR aralıkları), bağlı olan tüm şirket içi adres alanlarını ve bağlı Azure sanal ağlarını (yerel ağlar) içerir.

- **AZURE_LOADBALANCER:** Bu varsayılan etiket Azure'ın Altyapı yük dengeleyicisini belirtir. Bu, Azure'ın sistem durumu araştırmalarının kaynağı olan bir Azure veri merkezi IP'sine çevrilir.

- **INTERNET:** Bu varsayılan etiket, sanal ağın dışında olan ve genel İnternet ile ulaşılabilen IP adresi alanını belirtir. [Azure'a ait genel IP alanı](https://www.microsoft.com/download/details.aspx?id=41653) da bu aralığa dahildir.

### Varsayılan Kurallar

Tüm NSG'ler bir varsayılan kurallar kümesini içerir. Varsayılan kurallar silinemez ancak en düşük önceliğe atanmış oldukları için sizin oluşturduğunuz kurallar tarafından geçersiz kılınabilirler. 

Aşağıdaki varsayılan kurallarda da gösterildiği üzere, kaynağı bir sanal ağ olan ve bir sanal ağda biten trafiğe hem Gelen hem de Giden yönlerde izin verilir. Giden yön için İnternet bağlantısına izin verilse de Gelen yönde varsayılan olarak engellenir. VM'lerinizin ve rol örneklerinin durumunun Azure'ın yük dengeleyicisi tarafından araştırılmasına izin veren bir varsayılan kural bulunur. Yük dengelenmiş bir küme kullanmıyorsanız bu kuralı geçersiz kılabilirsiniz.

**Gelen varsayılan kuralları**

| Ad                              | Öncelik | Kaynak IP          | Kaynak Bağlantı Noktası | Hedef IP  | Hedef Bağlantı Noktası | Protokol | Erişim |
|-----------------------------------|----------|--------------------|-------------|-----------------|------------------|----------|--------|
| SANAL AĞA GELENE İZİN VER                | 65000    | VIRTUAL_NETWORK    | *           | VIRTUAL_NETWORK | *                | *        | İZİN VER  |
| AZURE YÜK DENGELEYİCİYE GELENE İZİN VER | 65001    | AZURE_LOADBALANCER | *           | *               | *                | *        | İZİN VER  |
| GELENLERİN TÜMÜNÜ REDDET                  | 65500    | *                  | *           | *               | *                | *        | REDDET   |

**Giden varsayılan kuralları**

| Ad                    | Öncelik | Kaynak IP       | Kaynak Bağlantı Noktası | Hedef IP  | Hedef Bağlantı Noktası | Protokol | Erişim |
|-------------------------|----------|-----------------|-------------|-----------------|------------------|----------|--------|
| SANAL AĞDAN GİDENE İZİN VER     | 65000    | VIRTUAL_NETWORK | *           | VIRTUAL_NETWORK | *                | *        | İZİN VER  |
| İNTERNETTEN GİDENE İZİN VER | 65001    | *               | *           | INTERNET        | *                | *        | İZİN VER  |
| GİDENLERİN TÜMÜNÜ REDDET       | 65500    | *               | *           | *               | *                | *        | REDDET   |

## NSG'leri ilişkilendirme

Bir NSG'yi kullandığınız dağıtım modeline bağlı olarak VM'lerle, NIC'lerle ve alt ağlara ilişkilendirebilirsiniz.

[AZURE.INCLUDE [learn-about-deployment-models-both-include.md](../../includes/learn-about-deployment-models-both-include.md)]
 
- **Bir NSG'yi bir VM ile ilişkilendirme (yalnızca klasik dağıtımlar).** Bir NSG'yi bir VM ile ilişkilendirdiğinizde, NSG'deki ağ erişim kuralları VM'ye gelen ve giden tüm trafiğe uygulanır. 

- **Bir NSG'yi bir NIC ile ilişkilendirme (Yalnızca Resource Manager dağıtımları).** Bir NSG'yi bir NIC ile ilişkilendirdiğinizde, NSG'deki ağ erişim kuralları yalnızca bu NIC'ye uygulanır. Yani birden çok NIC'nin bulunduğu bir VM'de, bir NSG tek bir NIC'ye uygulandığı zaman diğer NIC'lere giden trafiği etkilemez. 

- **Bir NSG'yi bir alt ağ ile ilişkilendirme (tüm dağıtımlar)**. Bir NSG'yi bir alt ağ ile ilişkilendirdiğinizde, NSG'deki ağ erişim kuralları alt ağdaki tüm IaaS ve PaaS kaynaklarına uygulanır. 

Farklı NSG'leri bir VM ile (veya dağıtım modeline bağlı olarak NIC ile) ve NIC'nin veya VM'nin bağlı olduğu ağ ile ilişkilendirebilirsiniz. Bu durumda, her NSG'deki öncelik temel alınarak, tüm ağ erişim kuralları aşağıdaki sırayla trafiğe uygulanır:

- **Gelen trafik**
    1. NSG alt ağa uygulanır. 
    
           If subnet NSG has a matching rule to deny traffic, packet will be dropped here.
    2. NSG, NIC'ye (Resource Manager) veya VM'ye (klasik) uygulanır. 
       
           If VM\NIC NSG has a matching rule to deny traffic, packet will be dropped at VM\NIC, although subnet NSG has a matching rule to allow traffic.
- **Giden trafik**
    1. NSG, NIC'ye (Resource Manager) veya VM'ye (klasik) uygulanır. 
      
           If VM\NIC NSG has a matching rule to deny traffic, packet will be dropped here.
    2. NSG alt ağa uygulanır.
       
           If subnet NSG has a matching rule to deny traffic, packet will be dropped here, although VM\NIC NSG has a matching rule to allow traffic.

![NSG ACL'leri](./media/virtual-network-nsg-overview/figure2.png)

>[AZURE.NOTE] Tek bir NSG'yi yalnızca bir alt ağ, VM veya NIC ile ilişkilendirebilseniz de aynı NSG'yi istediğiniz kadar fazla kaynak ile ilişkilendirebilirsiniz.

## Uygulama
Aşağıda listelenen farklı araçları kullanarak klasik veya Resource Manager dağıtım modellerinde NSG'leri uygulayabilirsiniz.

|Dağıtım aracı|Klasik|Resource Manager|
|---|---|---|
|Klasik portal|![Hayır][red]|![Hayır][red]|
|Azure portalı|![Evet][green]|<a href="https://azure.microsoft.com/documentation/articles/virtual-networks-create-nsg-arm-pportal">![Evet][green]</a>|
|PowerShell|<a href="https://azure.microsoft.com/documentation/articles/virtual-networks-create-nsg-classic-ps">![Evet][green]</a>|<a href="https://azure.microsoft.com/documentation/articles/virtual-networks-create-nsg-arm-ps">![Evet][green]</a>|
|Azure CLI|<a href="https://azure.microsoft.com/documentation/articles/virtual-networks-create-nsg-classic-cli">![Evet][green]</a>|<a href="https://azure.microsoft.com/documentation/articles/virtual-networks-create-nsg-arm-cli">![Evet][green]</a>|
|ARM şablonu|![Hayır][red]|<a href="https://azure.microsoft.com/documentation/articles/virtual-networks-create-nsg-arm-template">![Evet][green]</a>|

|**Anahtar**|![Evet][green] Destekleniyor. Makale için tıklayın.|![Hayır][red] Desteklenmiyor.|
|---|---|---|

## Planlama

NSG'leri uygulamadan önce aşağıdaki soruları yanıtlamanız gerekir:   

1. Hangi tür kaynaklara gelen veya giden trafiği filtrelemek istiyorsunuz (aynı VM'de bulunan NIC'ler, VM'ler veya aynı alt ağa bağlı olan bulut hizmetleri ya da uygulama hizmeti ortamları gibi diğer kaynaklar, farklı alt ağlara bağlı olan kaynaklar arasındaki veri trafiği)?

2. Gelen/giden trafiği filtrelemek istediğiniz kaynaklar var olan sanal ağlardaki alt ağlara mı bağlı yoksa yeni sanal ağlara veya alt ağlara mı bağlanacak?
 
Azure'da ağ güvenliği planlaması konusunda daha fazla bilgi için [bulut hizmetleri ve ağ güvenliği için en iyi uygulamaları](../best-practices-network-security.md) okuyun. 

## Tasarım konusunda dikkat edilmesi gerekenler

[Planlama](#Planning) bölümündeki soruların yanıtlarını öğrendiğiniz zaman, NSG'lerinizi tanımlamadan önce aşağıdakileri gözden geçirin.

### Sınırlar

NSG'lerinizi tasarlarken aşağıdaki sınırları göz önünde bulundurmanız gerekir.

|**Açıklama**|**Varsayılan Sınır**|**Etkileri**|
|---|---|---|
|Bir alt ağ, VM veya NIC ile ilişkilendirebileceğiniz NSG sayısı|1|Bu, NSG'leri birleştiremeyeceğiniz anlamına gelir. Belirli bir kaynak kümesi için gerekli olan tüm kuralların tek bir NSG'de bulunduğundan emin olun.|
|Her abonelikteki bölge başına NSG'ler|100|Varsayılan olarak, Azure portalında oluşturduğunuz her VM için yeni bir NSG oluşturulur. Bu varsayılan davranışa izin verirseniz NSG'leriniz hızla tükenecektir. Tasarımınız sırasında bu sınırı göz önünde bulundurmayı unutmayın ve gerekli olursa kaynaklarınızı birden fazla bölgeye veya aboneliğe ayırın. |
|NSG başına NSG kuralları|200|Bu sınırı geçmediğinizden emin olmak için geniş bir IP ve bağlantı noktası aralığı kullanın. |

>[AZURE.IMPORTANT] Çözümünüzü tasarlamadan önce [Azure'daki ağ hizmetleri ile ilgili sınırların](../azure-subscription-service-limits.md#networking-limits) tümünü görüntülediğinizden emin olun. Bazı sınırlar bir destek bileti açılarak artırılabilir.

### Sanal ağ ve alt ağ tasarımı

NSG'ler alt ağlara uygulanabildiğinden kaynaklarınızı alt ağa göre gruplayıp NSG'leri alt ağlara uygulayarak NSG sayısını en aza indirebilirsiniz.  NSG'leri alt ağlara uygulamaya karar verirseniz var olan sanal ağlarınızın ve alt ağlarınızın NSG'ler göz önüne alınmadan tanımlanmış olduğunu fark edebilirsiniz. NSG tasarımınızı desteklemek için yeni sanal ağlar ve alt ağlar tanımlamanız gerekebilir. Ayrıca yeni kaynaklarınızı yeni alt ağlarınıza dağıtmanız gerekir. Bu işlemlerden sonra var olan kaynaklarınızı yeni alt ağlara taşımak için bir geçiş stratejisi tanımlayabilirsiniz. 

### Özel kurallar

Aşağıda listelenen özel kuralları dikkate almanız gerekir. Bu kurallar tarafından izin verilen trafiği engellemediğinizden emin olun, aksi halde altyapınız temel Azure hizmetleri ile iletişim kuramayacaktır.

- **Ana Bilgisayar Düğümünün Sanal IP'si:** DHCP, DNS ve Sistem durumunu izleme gibi temel altyapı hizmetleri, 168.63.129.16 numaralı sanallaştırılmış ana bilgisayar IP adresi yoluyla sağlanır. Bu genel IP adresi Microsoft'a aittir ve tüm bölgelerde bu amaç için kullanılan tek sanallaştırılmış IP adresi olarak kullanılacaktır. Bu IP adresi, sanal makineyi barındıran sunucu makinesinin (ana bilgisayar düğümü) fiziksel IP adresiyle eşleşir. Ana bilgisayar düğümü, yük dengeleyici durum araştırması ve makine durumu araştırması için araştırma kaynağı, DNS özyinelemeli çözümleyici ve DHCP geçişi olarak görev yapar. Bu IP adresi ile iletişim kurulması bir saldırı olarak değerlendirilmemelidir.

- **Lisanslama (Anahtar Yönetimi Hizmeti):** Sanal makinelerde çalışan Windows görüntülerinin lisanslanması gerekir. Bunun için lisans isteği sorgularını işleyen Anahtar Yönetimi Hizmeti ana bilgisayar sunucularına bir lisans isteği gönderilir. Bu her zaman için 1688 numaralı giden bağlantı noktasında olur.

### ICMP trafiği

Geçerli NSG kuralları yalnızca *TCP* veya *UDP* protokollerine izin verir. *ICMP* için belirli bir etiket bulunmaz. Ancak bir Virtual Network üzerindeki ICMP trafiğine, sanal ağ içindeki herhangi bir bağlantı noktasından ve protokolden gelen/giden trafiğe izin veren Gelen sanal ağ kuralı (Varsayılan kural 65000 gelen şeklindedir) yoluyla varsayılan olarak izin verilir.

### Alt ağlar

- İş yükünüzün gerektirdiği katmanların sayısını göz önünde bulundurun. Her katman bir alt ağ kullanılarak yalıtılabilir, bunun için alt ağa bir NSG uygulanır. 
- Bir VPN ağ geçidi veya ExpressRoute bağlantı hattı için bir alt ağ uygulamanız gerekiyorsa bu alt ağa bir NSG **UYGULAMADIĞINIZDAN** emin olun. Aksi halde sanal ağlar arası veya şirket içi ve dışı bağlantılar çalışmaz.
- Bir sanal gereç uygulamanız gerekiyorsa Kullanıcı Tanımlı Yollarınızın (UDR'ler) düzgün çalışabilmesi için sanal gereci kendi alt ağına dağıttığınızdan emin olun. Bu alt ağa gelen ve giden trafiği filtrelemek için alt ağ düzeyinde bir NSG uygulayabilirsiniz. [Trafik akışını denetleme ve sanal gereçler kullanma](virtual-networks-udr-overview.md) hakkında daha fazla bilgi edinin.

### Yük dengeleyiciler

- Her iş yükünüzün kullandığı her yük dengeleyici için yük dengeleme ve NAT kurallarını değerlendirin. Bu kurallar NIC'ler (Resource Manager dağıtımları) veya VM'ler/rol örnekleri (klasik dağıtımlar) içeren bir arka uç havuzuna bağlıdır. Yalnızca yük dengeleyicilerde uygulanan kurallar yoluyla eşlenen trafiğe izin vermek üzere, her arka uç havuzu için bir NSG oluşturmayı değerlendirin. Bu işlem, yük dengeleyiciden geçmeden doğrudan arka uç havuzuna gelen trafiğin de filtrelenmesini garanti eder.
- Klasik dağıtımlarda, bir yük dengeleyicideki bağlantı noktalarını VM'lerinizdeki veya rol örneklerinizdeki bağlantı noktalarına eşleyen uç noktalar oluşturursunuz. Bir Resource Manager dağıtımında, genel kullanıma yönelik bireysel yük dengeleyicinizi de oluşturabilirsiniz. NSG'leri kullanarak yük dengeleyicideki bir arka uç havuzunun parçası olan VM'lere ve rol örneklerine giden trafiği kısıtlıyorsanız gelen trafik için hedef bağlantı noktasının yük dengeleyici tarafından kullanıma sunulan bağlantı noktası değil, VM'deki veya rol örneğindeki gerçek bağlantı noktası olduğunu unutmayın. Ayrıca VM'ye gelen bağlantıya ait kaynak bağlantı noktasının ve adresinin yük dengeleyici tarafından kullanıma sunulan bağlantı noktası ve adresi değil, İnternet'teki uzak bilgisayar üzerindeki bir bağlantı noktası ve adresi olduğunu unutmayın.
- Genel kullanıma yönelik yük dengeleyicilere benzer şekilde, bir iç yük dengeleyici (ILB) yoluyla gelen trafiği filtrelemek için NSG'ler oluşturduğunuz zaman, uygulanan kaynak bağlantı noktasının ve adres aralığının yük dengeleyici tarafından değil, aramanın kaynaklandığı bilgisayar tarafından sağlandığını anlamanız gerekir. Aynı zamanda hedef bağlantı noktası ve adres aralığı yük dengeleyici ile değil, trafiği alan bilgisayar ile ilişkilidir.

### Diğer

- Uç nokta tabanlı ACL'ler ve NSG'ler, aynı VM örneğinde desteklenmez. Bir NSG'yi kullanmak istiyorsanız ve bir uç nokta ACL'si zaten kullanılıyorsa öncelikle uç nokta ACL'sini kaldırın. Bunun nasıl yapılacağı hakkında bilgi için bkz. [Uç nokta ACL'lerini yönetme](virtual-networks-acl-powershell.md).
- Resource Manager dağıtım modelinde, birden çok NIC içeren VM'ler için, bir NIC ile ilişkilendirilmiş bir NSG kullanarak NIC ile yönetimi (uzaktan erişim) etkinleştirebilir ve böylelikle trafiği ayırabilirsiniz.
- Yük dengeleyicilerin kullanımına benzer şekilde, diğer sanal ağlardan gelen trafiği filtrelerken sanal ağları bağlayan ağ geçidini değil, uzak bilgisayarın kaynak adres aralığını kullanmanız gerekir.
- Çoğu Azure hizmeti Azure Virtual Network hizmetlerine bağlanamaz ve bu nedenle bunlara gelen ve giden trafik NSG'ler ile filtrelenemez.  Kullandığınız hizmetlerin sanal ağlara bağlanıp bağlanamayacaklarını belirlemek için bu hizmetlerin belgelerini okuyun.

## Örnek dağıtımı

Bu makalede sağlanan bilgilerin uygulanmasını göstermek amacıyla, iki katmanlı bir iş yükü çözümü için ağ trafiğini filtrelemek üzere aşağıdaki gereksinimleri kullanarak NSG'ler tanımlayacağız:

1. Ön uç (Windows web sunucuları) ve arka uç (SQL veritabanı sunucuları) arasındaki trafiğin ayrılması.
2. Trafiği 80 numaralı bağlantı noktasındaki tüm web sunucularındaki yük dengeleyiciye ileten yük dengeleme kuralları.
3. Yük dengeleyicideki 50001 numaralı bağlantı noktasına gelen trafiği ön uçtaki yalnızca bir VM'nin 3389 numaralı bağlantı noktasına ileten NAT kuralları.
4. 1 numaralı gereksinim dışında, İnternet'ten ön uç veya arka uç VM'lerine erişim olmaması.
5. Ön uçtan veya arka uçtan İnternet'e erişim olmaması.
6. Ön uç alt ağının kendisinden gelen trafik için ön uçtaki tüm web sunucularının 3389 numaralı bağlantı noktasına erişim.
7. Yalnızca ön uç alt ağından arka uçtaki tüm SQL Server VM'lerinin 3389 numaralı bağlantı noktasına erişim.
8. Yalnızca ön uç alt ağından arka uçtaki tüm SQL Server VM'lerinin 1433 numaralı bağlantı noktasına erişim.
9. Arka uç VM'lerindeki farklı NIC'lerde yönetim trafiğinin (3389 numaralı bağlantı noktası) ve veritabanı trafiğinin (1433) ayrılması.

![NSG'ler](./media/virtual-network-nsg-overview/figure1.png)

Yukarıdaki diyagramda görüldüğü gibi, *Web1* ile *Web2* VM'leri *FrontEnd* alt ağına ve *DB1* ile *DB2* VM'leri *BackEnd* alt ağına bağlanır.  Her iki alt ağ da *TestVNet* sanal ağının parçasıdır. Tüm kaynaklar *Batı ABD* Azure bölgesine atanmıştır.

Yukarıdaki 1-6 gereksinimlerinin tamamı (3 dışında) alt ağ alanlarına sınırlandırılmıştır. Her NSG için gereken kuralların sayısını en aza indirmek ve var olan VM'ler ile aynı türdeki iş yüklerini çalıştıran alt ağlara ilave VM'ler eklemeyi kolaylaştırmak için aşağıdaki alt ağ düzeyindeki NSG'leri uygulayabiliriz.

### FrontEnd alt ağı için NSG

**Gelen kuralları**

|Kural|Erişim|Öncelik|Kaynak adres aralığı|Kaynak bağlantı noktası|Hedef adres aralığı|Hedef bağlantı noktası|Protokol|
|---|---|---|---|---|---|---|---|
|HTTP'ye izin ver|İzin Ver|100|INTERNET|\*|\*|80|TCP|
|FrontEnd'den gelen RDP'ye izin ver|İzin Ver|200|192.168.1.0/24|\*|\*|3389|TCP|
|İnternet'ten gelen her şeyi reddet|Reddet|300|INTERNET|\*|\*|\*|TCP|

**Giden kuralları**

|Kural|Erişim|Öncelik|Kaynak adres aralığı|Kaynak bağlantı noktası|Hedef adres aralığı|Hedef bağlantı noktası|Protokol|
|---|---|---|---|---|---|---|---|
|İnternet'i reddet|Reddet|100|\*|\*|INTERNET|\*|\*|

### BackEnd alt ağı için NSG

**Gelen kuralları**

|Kural|Erişim|Öncelik|Kaynak adres aralığı|Kaynak bağlantı noktası|Hedef adres aralığı|Hedef bağlantı noktası|Protokol|
|---|---|---|---|---|---|---|---|
|İnternet'i reddet|Reddet|100|INTERNET|\*|\*|\*|\*|

**Giden kuralları**

|Kural|Erişim|Öncelik|Kaynak adres aralığı|Kaynak bağlantı noktası|Hedef adres aralığı|Hedef bağlantı noktası|Protokol|
|---|---|---|---|---|---|---|---|
|İnternet'i reddet|Reddet|100|\*|\*|INTERNET|\*|\*|

### İnternet'ten gelen RDP'ye ilişkin FrontEnd'deki tekil VM (NIC) için NSG

**Gelen kuralları**

|Kural|Erişim|Öncelik|Kaynak adres aralığı|Kaynak bağlantı noktası|Hedef adres aralığı|Hedef bağlantı noktası|Protokol|
|---|---|---|---|---|---|---|---|
|İnternet'ten gelen RDP'ye izin ver|İzin Ver|100|INTERNET|*|\*|3389|TCP|

>[AZURE.NOTE] Bu kuralın kaynak adres aralığı olarak yük dengeleyici VIP'sinin değil **Internet**'in kullanıldığına dikkat edin, kaynak bağlantı noktası için 500001 değil, **\*** kullanılır. NAT kuralları/yük dengeleme kuralları ile NSG kurallarını birbirine karıştırmayın. NSG kuralları her zaman için trafiğin orijinal kaynağı ve son hedefi ile ilgilidir, ikisi arasındaki yük dengeleyicisiyle **DEĞİL**. 

### BackEnd'deki NIC'lerin yönetimi için NSG

**Gelen kuralları**

|Kural|Erişim|Öncelik|Kaynak adres aralığı|Kaynak bağlantı noktası|Hedef adres aralığı|Hedef bağlantı noktası|Protokol|
|---|---|---|---|---|---|---|---|
|ön uçtan gelen RDP'ye izin ver|İzin Ver|100|192.168.1.0/24|*|\*|3389|TCP|

### Arka uçtaki veritabanı erişimi NIC'leri için NSG

**Gelen kuralları**

|Kural|Erişim|Öncelik|Kaynak adres aralığı|Kaynak bağlantı noktası|Hedef adres aralığı|Hedef bağlantı noktası|Protokol|
|---|---|---|---|---|---|---|---|
|ön uçtan gelen SQL'ye izin ver|İzin Ver|100|192.168.1.0/24|*|\*|1433|TCP|

Yukarıdaki NSG'lerden bazılarının tekil NIC'lerle ilişkilendirilmesi gerektiği için bu senaryoyu bir Resource Manager dağıtımı olarak dağıtmanız gerekir. Kuralların uygulanma gereksinimlerine bağlı olarak alt ağ ve NIC düzeyi için nasıl birleştirildiklerine dikkat edin. 

## Sonraki adımlar

- [Klasik dağıtım modelinde NSG'leri dağıtın](virtual-networks-create-nsg-classic-ps.md).
- [Resource Manager'da NSG'leri dağıtın](virtual-networks-create-nsg-arm-pportal.md).
- [NSG günlüklerini yönetin](virtual-network-nsg-manage-log.md).

[yeşil]: ./media/virtual-network-nsg-overview/green.png
[sarı]: ./media/virtual-network-nsg-overview/yellow.png
[kırmızı]: ./media/virtual-network-nsg-overview/red.png



<!----HONumber=Jun16_HO2-->


