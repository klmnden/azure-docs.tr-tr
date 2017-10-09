---
title: "Azure ağ güvenliğine genel bakış | Microsoft Docs"
description: "Azure kaynakları arasındaki ağ trafiği akışını denetlemeye yönelik güvenlik seçenekleri hakkında bilgi edinin."
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/19/2017
ms.author: jdial
ms.translationtype: HT
ms.sourcegitcommit: 0e862492c9e17d0acb3c57a0d0abd1f77de08b6a
ms.openlocfilehash: 63a313d9035422207a1ce56f0da8b388e2747685
ms.contentlocale: tr-tr
ms.lasthandoff: 09/27/2017

---
# <a name="network-security"></a>Ağ güvenliği

Bir ağ güvenlik grubu kullanarak ağ trafiğini bir sanal ağ içindeki kaynaklarla sınırlayabilirsiniz. Bir ağ güvenlik grubunda gelen veya giden ağ trafiğine kaynak ya da hedef IP adresi, bağlantı noktası ve protokole göre izin veren veya bu trafiği reddeden güvenlik kurallarından oluşan bir liste bulunur. 

## <a name="network-security-groups"></a>Ağ güvenlik grupları

Her bir ağ arabirimiyle ilişkilendirilmiş sıfır veya bir ağ güvenlik grubu vardır. Her ağ arabirimi bir [sanal ağ](virtual-networks-overview.md) alt ağında yer alır. Bir alt ağ ile ilişkilendirilmiş sıfır veya bir ağ güvenlik grubu olabilir. 

Bir alt ağa uygulanan güvenlik kuralları, alt ağdaki tüm kaynaklara uygulanır. Alt ağda ağ arabirimlerine ek olarak HDInsight, Sanal Makine Ölçek Kümeleri ve Uygulama Servisi Ortamları gibi başka Azure hizmeti örnekleri dağıtılmış olabilir.

Ağ güvenlik gruplarının bir ağ güvenlik grubu hem bir ağ arabirimi hem de ağ arabiriminin içinde bulunduğu alt ağ ile ilişkilendirilmesi halinde uygulanma şekli aşağıda verilmiştir:

- **Gelen trafik**: Önce ağ arabiriminin içinde bulunduğu alt ağ ile ilişkilendirilmiş olan ağ güvenlik grubu değerlendirilir. Alt ağ ile ilişkilendirilmiş olan ağ güvenlik grubunun izin verdiği trafik sonrasında ağ arabirimi ile ilişkilendirilmiş olan ağ güvenlik grubu tarafından değerlendirilir. Örneğin, bir sanal makineye İnternet ve 80 numaralı bağlantı noktası üzerinden erişim sağlamaya ihtiyaç duyabilirsiniz. Bir ağ güvenlik grubunu hem ağ arabirimi hem de ağ arabiriminin içinde bulunduğu alt ağ ile ilişkilendirirseniz, alt ağ ile ilişkilendirilmiş ağ güvenlik grubunun ve ağ arabiriminin 80 numaralı bağlantı noktasına izin vermesi gerekir. 80 numaralı bağlantı noktasına yalnızca alt ağ ile ilişkilendirilmiş ağ güvenlik grubunda veya alt ağın içinde bulunduğu ağ arabiriminde izin vermeniz halinde varsayılan güvenlik kuralları nedeniyle iletişim başarısız olur. Ayrıntılar için [varsayılan güvenlik kurallarına](#default-security-rules) bakın. Bir ağ güvenlik grubunu yalnızca alt ağa veya ağ arabirimine uygulamanız ve ağ güvenlik grubunda 80 numaralı bağlantı noktasından gelen trafiğe izin veren bir kural bulunması halinde iletişim başarılı olur. 
- **Giden trafik**: Önce ağ arabirimi ile ilişkilendirilmiş olan ağ güvenlik grubu değerlendirilir. Ağ arabirimi ile ilişkilendirilmiş olan ağ güvenlik grubunun izin verdiği trafik sonrasında alt ağ ile ilişkilendirilmiş olan ağ güvenlik grubu tarafından değerlendirilir.

Her zaman ağ güvenlik gruplarının hem bir ağ arabirimine hem de alt ağa uygulandığının farkında olmayabilirsiniz. Bir ağ arabirimi için [geçerli güvenlik kurallarını](virtual-network-manage-nsg-arm-portal.md) görüntüleyerek bir ağ arabirimine uygulanmış olan toplu kuralları kolayca görüntüleyebilirsiniz. Azure Ağ İzleyicisi'ndeki [IP akışı doğrulama](../network-watcher/network-watcher-check-ip-flow-verify-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) özelliğini kullanarak da bir ağ arabirimine gelen veya dışarı giden iletişime izin verilip verilmediğini belirleyebilirsiniz. Bu araç iletişime izin verilip verilmediğini ve trafiğe izin veren veya onu reddeden ağ güvenlik kuralının hangisi olduğunu belirler.
 
> [!NOTE]
> Ağ güvenlik grupları Resource Manager dağıtım modelindeki ağ arabirimlerinin yerine klasik dağıtım modelindeki alt ağlar veya sanal makineler ve bulut hizmetleri ile ilişkilendirilir. Azure dağıtım modelleri hakkında daha fazla bilgi edinmek için bkz. [Azure dağıtım modellerini kavrama](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Bir ağ güvenlik grubunu istediğiniz sayıda ağ arabirimine ve alt ağa uygulayabilirsiniz.

## <a name="security-rules"></a>Güvenlik kuralları

Bir ağ güvenlik grubunda Azure abonelik [limitleri](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) dahilinde sıfır veya istenen sayıda kural bulunabilir. Her bir kural aşağıdaki özellikleri belirtir:

|Özellik  |Açıklama  |
|---------|---------|
|Ad|Ağ güvenlik grubu içinde benzersiz bir ad.|
|Öncelik | 100 ile 4096 arasında bir rakam. Kurallar öncelik sırasına göre işleme alınır ve düşük rakamlı kurallar daha yüksek önceliğe sahip olduğundan yüksek rakamlı kurallardan önce uygulanır. Trafik bir kuralla eşleştiğinde işlem durur. Bunun sonucunda yüksek önceliğe sahip olan kurallarla aynı özniteliklere sahip olan önceliği daha düşük olan (yüksek rakamlı) kurallar işleme alınmaz.|
|Kaynak veya hedef| Herhangi bir IP adresi, CIDR bloğu (10.0.0.0/24 gibi), hizmet etiketi veya uygulama güvenlik grubu. [Hizmet etiketleri](#service-tags) ve [uygulama güvenlik grupları](#application-security-groups) hakkında daha fazla bilgi edinin. Aralık, hizmet etiketi veya uygulama güvenlik grubu belirterek daha az sayıda güvenlik kuralı oluşturabilirsiniz. Bir kuralda birden fazla IP adresi veya aralığı belirtme özelliği (birden fazla hizmet etiketi veya uygulama grubu belirtemezsiniz) önizleme sürümündedir ve genişletilmiş güvenlik kuralı olarak adlandırılır. Genişletilmiş güvenlik kuralları yalnızca Resource Manager dağıtım modeliyle oluşturulmuş olan ağ güvenlik gruplarında oluşturulabilir. Klasik dağıtım modeliyle oluşturulmuş olan ağ güvenlik gruplarında birden fazla IP adresi ve IP adresi aralığı belirtemezsiniz.|
|Protokol     | TCP, UDP veya Tümü (TCP, UDP ve ICMP). Tek başına ICMP'yi belirtemezsiniz, bu nedenle Tümü seçeneğini kullanmanız gerekir. |
|Yön| Kuralın gelen veya giden trafiğe uygulanma seçeneği.|
|Bağlantı noktası aralığı     |Tek bir bağlantı noktası veya aralık belirtebilirsiniz. Örneğin 80 veya 10000-10005 değerini kullanabilirsiniz. Aralık belirterek oluşturmanız gereken güvenlik kuralı sayısını azaltabilirsiniz. Bir kural içinde birden fazla bağlantı noktası ve bağlantı noktası aralığı belirtme özelliği önizleme sürümündedir ve genişletilmiş güvenlik kuralı olarak adlandırılır. Genişletilmiş güvenlik kurallarını kullanmadan önce [Önizleme özellikleri](#preview-features) bölümündeki önemli bilgileri inceleyin. Genişletilmiş güvenlik kuralları yalnızca Resource Manager dağıtım modeliyle oluşturulmuş olan ağ güvenlik gruplarında oluşturulabilir. Klasik dağıtım modeliyle oluşturulmuş olan ağ güvenlik gruplarında aynı güvenlik kuralı içinde birden fazla bağlantı noktası ve bağlantı noktası aralığı belirtemezsiniz.   |
|Eylem     | İzin ver veya reddet        |

**Dikkat edilmesi gerekenler**

- **Ana bilgisayar düğümünün sanal IP'si:** DHCP, DNS ve sistem durumunu izleme gibi temel altyapı hizmetleri, 168.63.129.16 ve 169.254.169.254 numaralı sanallaştırılmış ana bilgisayar IP adresleri yoluyla sağlanır. Bu genel IP adresleri Microsoft'a aittir ve tüm bölgelerde bu amaç için kullanılan tek sanallaştırılmış IP adresleri olarak kullanılır. Bu IP adresleri, VM'yi barındıran sunucu makinesinin (ana bilgisayar düğümü) fiziksel IP adresiyle eşleşir. Ana bilgisayar düğümü, yük dengeleyici durum araştırması ve makine durumu araştırması için araştırma kaynağı, DNS özyinelemeli çözümleyici ve DHCP geçişi olarak görev yapar. Bu IP adresleri ile iletişim bir saldırı değildir. Bu IP adreslerine giden veya oradan gelen trafiği engellerseniz sanal makine düzgün çalışmayabilir.
- **Lisanslama (Anahtar Yönetimi Hizmeti)**: VM'lerde çalışan Windows görüntülerinin lisanslanması gerekir. Lisanslama için, lisans isteği sorgularını işleyen Anahtar Yönetimi Hizmeti ana bilgisayar sunucularına bir lisans isteği gönderilir. İstek, bağlantı noktası 1688 üzerinden gönderilir.
- **Yük dengelenmiş havuzlardaki sanal makineler**: Uygulanan kaynak bağlantı noktası ve adres aralığı, yük dengeleyiciye değil kaynak bilgisayara aittir. Hedef bağlantı noktası ve adres aralığı, yük dengeleyici değil, hedef bilgisayar içindir.
- **Azure hizmet örnekleri**: HDInsight, Uygulama Servisi Ortamları ve Sanal Makine Ölçek Kümeleri gibi sanal ağ alt ağlarına dağıtılmış olan farklı Azure hizmeti örnekleri. Bir ağ güvenlik grubunu kaynağın dağıtılmış olduğu alt ağa uygulamadan önce hizmetlerle ilgili olan bağlantı noktası gereksinimlerini öğrendiğinizden emin olun. Gerekli bağlantı noktalarını reddetmeniz halinde hizmet düzgün çalışmaz. 

Güvenlik kuralları durum bilgisine sahiptir. Örneğin 80 numaralı bağlantı noktasından tüm adreslere doğru giden bir güvenlik kuralı belirtirseniz giden trafiğe yanıt olarak bir gelen güvenlik kuralı belirtmeniz gerekli değildir. Yalnızca iletişimin dışarıdan başlatılması halinde bir gelen güvenlik kuralı belirtmeniz gerekir. Bunun tersi de geçerlidir. Gelen trafiğe bir bağlantı noktası üzerinden izin verilmesi halinde bağlantı noktasından geçen trafiğe yanıt olarak bir giden güvenlik belirtmeniz gerekli değildir. Güvenlik kuralı oluştururken geçerli olan limitler hakkında bilgi edinmek için bkz. [Azure limitleri](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).

## <a name="augmented-security-rules"></a>Genişletilmiş güvenlik kuralları

Genişletilmiş kurallar sanal ağlar için güvenlik tanımını daha basit hale getirerek daha az sayıda kuralla daha geniş çaplı ve karmaşık ağ güvenlik ilkesi tanımlamanızı sağlar. Birden fazla bağlantı noktasını, açık IP adresini, Hizmet etiketini ve Uygulama güvenlik gruplarını bir araya getirerek tek ve anlaşılması kolay bir güvenlik kuralı oluşturabilirsiniz. Genişletilmiş kuralları bir kuralın kaynak, hedef ve bağlantı noktası alanlarında kullanabilirsiniz. Bir kural oluştururken birden fazla açık IP adresi, CIDR aralığı ve bağlantı noktası belirtebilirsiniz. Güvenlik kuralı tanımınızın bakımını yapmayı daha kolay hale getirmek için genişletilmiş güvenlik kurallarını hizmet etiketleri veya uygulama güvenlik gruplarıyla bir arada kullanabilirsiniz. 

Genişletilmiş güvenlik kuralları önizleme sürümündedir. Genişletilmiş güvenlik kuralı oluştururken geçerli olan limitler hakkında bilgi edinmek için bkz. [Azure limitleri](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits). Önizleme sürümündeki özellikler, genel sürümdeki özelliklerle aynı kullanılabilirlik ve güvenilirlik seviyesine sahip değildir. Özellikler yalnızca şu bölgelerde kullanılabilir: Doğu ABD, Batı ABD, Batı ABD 2, Batı Orta ABD, Avustralya Doğu, Avustralya Güneydoğu ve UK Güney.

## <a name="default-security-rules"></a>Varsayılan güvenlik kuralları

Bir ağ güvenlik grubu bir alt ağ veya ağ arabirimi ile ilişkilendirilmemişse alt ağ veya ağ arabirimi için tüm gelen veya giden trafiğe izin verilir. Ağ güvenlik grubu oluşturulur oluşturulmaz Azure ağ güvenlik grubu içinde aşağıdaki kuralları oluşturur:

### <a name="inbound"></a>Gelen

#### <a name="allowvnetinbound"></a>AllowVNetInBound

|Öncelik|Kaynak|Kaynak bağlantı noktaları|Hedef|Hedef bağlantı noktaları|Protokol|Access|
|---|---|---|---|---|---|---|
|65000|VirtualNetwork|0-65535|VirtualNetwork|0-65535|Tümü|İzin Ver|

#### <a name="allowazureloadbalancerinbound"></a>AllowAzureLoadBalancerInBound

|Öncelik|Kaynak|Kaynak bağlantı noktaları|Hedef|Hedef bağlantı noktaları|Protokol|Access|
|---|---|---|---|---|---|---|
|65001|AzureLoadBalancer|0-65535|0.0.0.0/0|0-65535|Tümü|İzin Ver|

#### <a name="denyallinbound"></a>DenyAllInbound

|Öncelik|Kaynak|Kaynak bağlantı noktaları|Hedef|Hedef bağlantı noktaları|Protokol|Access|
|---|---|---|---|---|---|---|
|65500|0.0.0.0/0|0-65535|0.0.0.0/0|0-65535|Tümü|Reddet|

### <a name="outbound"></a>Giden

#### <a name="allowvnetoutbound"></a>AllowVnetOutBound

|Öncelik|Kaynak|Kaynak bağlantı noktaları| Hedef | Hedef bağlantı noktaları | Protokol | Access |
|---|---|---|---|---|---|---|
| 65000 | VirtualNetwork | 0-65535 | VirtualNetwork | 0-65535 | Tümü | İzin Ver |

#### <a name="allowinternetoutbound"></a>AllowInternetOutBound

|Öncelik|Kaynak|Kaynak bağlantı noktaları| Hedef | Hedef bağlantı noktaları | Protokol | Access |
|---|---|---|---|---|---|---|
| 65001 | 0.0.0.0/0 | 0-65535 | Internet | 0-65535 | Tümü | İzin Ver |

#### <a name="denyalloutbound"></a>DenyAllOutBound

|Öncelik|Kaynak|Kaynak bağlantı noktaları| Hedef | Hedef bağlantı noktaları | Protokol | Access |
|---|---|---|---|---|---|---|
| 65500 | 0.0.0.0/0 | 0-65535 | 0.0.0.0/0 | 0-65535 | Tümü | Reddet |

**Kaynak** ve **Hedef** sütunlarında *VirtualNetwork*, *AzureLoadBalancer* ve *Internet*, için IP adresi yerine [hizmet etiketi](#tags) belirtilir. Protokol sütunundaki **Tümü** seçeneği TCP, UDP ve ICMP'yi kapsar. Bir kural oluştururken TCP, UDP veya Tümü seçeneğini belirleyebilirsiniz ancak yalnızca ICMP'yi belirtemezsiniz. Bu nedenle kural ICMP gerektiriyorsa protokol için *Tümü* seçeneğini belirlemeniz gerekir. **Kaynak** ve **Hedef** sütunlarında yer alan *0.0.0.0/0* ifadesi tüm adresleri temsil eder.
 
Varsayılan kuralları kaldıramazsınız ancak daha yüksek önceliğe sahip kurallar oluşturarak onları geçersiz kılabilirsiniz.

## <a name="service-tags"></a>Hizmet etiketleri

 Hizmet etiketi, güvenlik kuralı oluşturma sırasındaki karmaşıklığı en aza indirmeye yardımcı olmak için bir IP adresi ön eki grubunu temsil eder. Kendi hizmet etiketinizi oluşturamaz veya bir etiket içinde yer alacak IP adreslerini belirleyemezsiniz. Hizmet etiketine dahil olan adres ön ekleri Microsoft tarafından yönetilir ve hizmet etiketi adresler değiştikçe otomatik olarak güncelleştirilir. Hizmet etiketlerini güvenlik kuralı oluştururken belirli IP adreslerinin yerine kullanabilirsiniz. Güvenlik kuralı tanımlarken aşağıdaki hizmet etiketlerinden faydalanabilirsiniz. Farklı [Azure dağıtım modellerindeki](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) etiket adları arasında küçük farklar vardır.

* **VirtualNetwork** (*Resource Manager) (klasik için* *VIRTUAL_NETWORK**): Bu etiket sanal ağ adres alanını (sanal ağ için tanımlanmış olan tüm CIDR aralıkları), bağlı olan tüm şirket içi adres alanlarını ve [eşlenmiş](virtual-network-peering-overview.md) sanal ağları veya bir [sanal ağ geçidine](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json) bağlı olan sanal ağı içerir.
* **AzureLoadBalancer** (Resource Manager) (Klasik için **AZURE_LOADBALANCER**): Bu etiket Azure altyapı infrastructure yük dengeleyicisini belirtir. Bu etiket, Azure'ın sistem durumu araştırmalarının kaynağı olan bir [Azure veri merkezi IP adresine](https://www.microsoft.com/download/details.aspx?id=41653) çevrilir. Azure yük dengeleyiciyi kullanmıyorsanız bu kuralı geçersiz kılabilirsiniz.
* **İnternet** (Resource Manager) (klasik için **INTERNET**): Bu etiket Azure genel IP adresini belirtir. Bu etikete dahil olan adresler, düzenli olarak güncelleştirilen [Azure'un sahip olduğu genel IP alanı](https://www.microsoft.com/download/details.aspx?id=41653) belgesinde listelenmiştir.
* **AzureTrafficManager** (yalnızca Resource Manager): Bu etiket Azure Traffic Manager hizmeti için IP adresi alanını belirtir.
* **Storage** (yalnızca Resource Manager): Bu etiket Azure Storage hizmeti için IP adresi alanını belirtir. *Depolama* değerini belirtirseniz depolama alanına gelen trafiğe izin verilir veya trafik reddedilir. Yalnızca belirli bir [bölgedeki](https://azure.microsoft.com/regions) depolama alanına erişime izin vermek istiyorsanız bölgeyi belirtebilirsiniz. Örneğin yalnızca Doğu ABD bölgesindeki Azure Depolama hizmetine erişim izni vermek istiyorsanız *Storage.EastUS* hizmet etiketini kullanabilirsiniz. Kullanabileceğiniz ek bölgesel hizmet etiketleri şunlardır: Storage.AustraliaEast, Storage.AustraliaSoutheast, Storage.EastUS, Storage.UKSouth, Storage.WestCentralUS, Storage.WestUS ve Storage.WestUS2. Bu etiket hizmetin belirli örneklerini değil yalnızca hizmetin kendisini temsil eder. Örneğin etiket belirli bir Azure Depolama hesabını değil Azure Depolama hizmetini temsil eder.
* **Sql** (yalnızca Resource Manager): Bu etiket Azure SQL Veritabanı ve Azure SQL Veri Ambarı hizmetlerinin adres ön eklerini belirtir. Bu hizmet etiketi için yalnızca belirli bölgeleri tercih edebilirsiniz. Örneğin yalnızca Doğu ABD bölgesindeki Azure SQL Veritabanı hizmetine erişim izni vermek istiyorsanız *Sql.EastUS* hizmet etiketini kullanabilirsiniz. Sql hizmet etiketini aynı anda tüm bölgeler için belirtemezsiniz, her bölgeyi ayrıca belirtmeniz gerekir. Kullanılabilir diğer bölgesel etiketler: Sql.AustraliaEast, Sql.AustraliaSoutheast, Sql.EastUS, Sql.UKSouth, Sql.WestCentralUS, Sql.WestUS ve Sql.WestUS2. Bu etiket hizmetin belirli örneklerini değil yalnızca hizmetin kendisini temsil eder. Örneğin etiket belirli bir Azure SQL veritabanını değil Azure SQL Veritabanı hizmetini temsil eder.

> [!WARNING]
> AzureTrafficManager, Storage ve Sql hizmet etiketleri önizleme sürümündedir. Önizleme sürümündeki özellikler, genel sürümdeki özelliklerle aynı kullanılabilirlik ve güvenilirlik seviyesine sahip değildir. Hizmet etiketleri yalnızca şu bölgelerde kullanılabilir: Doğu ABD, Batı ABD, Batı ABD 2, Batı Orta ABD, Avustralya Doğu, Avustralya Güneydoğu ve UK Güney.

> [!NOTE]
> Azure Depolama veya Azure SQL Veritabanı gibi bir hizmet için bir sanal ağ hizmet uç noktası uygularsanız Azure, sanal ağ alt ağına hizmet için bir rota ekler. Rotaya ait adres ön ekleri ilgili hizmet etiketiyle aynı adres ön ekleri veya CIDR aralıklarıdır.

## <a name="application-security-groups"></a>Uygulama güvenliği grupları

Uygulama güvenlik grupları ağ güvenliğini uygulamanın yapısının doğal bir uzantısı olarak yapılandırmanıza imkan vererek sanal makineleri gruplamanızı ve ağ güvenlik ilkelerini bu gruplara göre tanımlamanızı sağlar. Bu özellik, açık IP adreslerinin bakımını el ile yapmanıza gerek kalmadan güvenlik ilkesini farklı ölçeklerde yeniden kullanmanıza izin verir. Platform açık IP adreslerinin ve birden fazla kural kümesinin karmaşık süreçlerini üstlenerek iş mantığınıza odaklanmanızı sağlar.

Bir uygulama güvenlik grubunu bir güvenlik kuralında kaynak ve hedef olarak belirtebilirsiniz. Güvenlik ilkeniz tanımlandıktan sonra sanal makineler oluşturarak sanal makinedeki ağ arabirimlerini bir uygulama güvenlik grubuna atayabilirsiniz. İlke bir sanal makine içindeki her bir ağ arabiriminin uygulama güvenlik grubu üyeliğine göre uygulanır. Aşağıdaki örnekte bir uygulama güvenlik grubunu aboneliğinizdeki tüm web sunucuları için nasıl kullanacağınız gösterilmiştir:

1. *WebServers* adlı bir uygulama güvenlik grubu oluşturun.
2. *MyNSG* adlı bir ağ güvenlik grubu oluşturun.
3. Ağ güvenlik grubunda kaynak adres olarak *Internet* hizmet etiketini, hedef adres olarak da *WebServers* uygulama güvenlik grubunu belirterek bir gelen güvenlik kuralı oluşturun, 80 ve 443 numaralı bağlantı noktalarına izin verin.
4. Bir web sunucusu uygulaması çalıştıran bir sanal makine dağıtın. Sanal makine içindeki ağ arabirimini *WebServers* uygulama güvenlik grubuna üye olarak ekleyin. Bu işlemin ardından sanal makine için 80 ve 443 numaralı bağlantı noktalarına izin verilir. Bu bağlantı noktalarına ayrıca daha sonra oluşturacağınız ve *WebServers* uygulama güvenlik grubuna üye olarak ekleyeceğiniz web sunucuları için de izin verilir. 

Hedef olarak başka uygulama güvenlik gruplarını belirten ek kurallar oluşturursanız bu kurallar önceki örnekteki web sunucularına uygulanmaz. Bir uygulama güvenlik grubunu belirten kurallar yalnızca uygulama güvenlik grubuna üye olan ağ arabirimlerine uygulanır. Uygulama güvenlik grupları genişletilmiş güvenlik kuralları ve hizmet etiketleriyle birlikte kullanıldığında aboneliğiniz kapsamındaki ağ güvenliğini yönetmek için mümkün olan en az sayıda ağ güvenlik grubu oluşturulmasını sağlar.
 
Uygulama güvenlik grubu oluşturma ve bunları güvenlik kurallarında belirtme limitleri hakkında bilgi edinmek için bkz. [Azure limitleri](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).

Uygulama güvenlik grupları önizleme sürümündedir. Ağ güvenlik gruplarını kullanmadan önce [Uygulama güvenlik gruplarıyla ağ güvenlik grubu oluşturma](create-network-security-group-preview.md#powershell) bölümünde yer alan 1-5 arası adımları tamamlayıp kullanmak üzere kaydolmanız ve [Önizleme özellikleri](#preview-features) bölümündeki önemli bilgileri incelemeniz gerekir. Uygulama güvenlik grupları önizleme sırasında sanal ağ kapsamıyla sınırlıdır. Bir ağ güvenlik grubu üzerindeki uygulama güvenlik gruplarına çapraz başvurularla eşlenmiş sanal ağlar geçerli olmaz. 

Önizleme sürümündeki özellikler, genel sürümdeki özelliklerle aynı kullanılabilirlik ve güvenilirlik seviyesine sahip değildir. Uygulama güvenlik gruplarını kullanmadan önce kullanmak üzere kaydolmanız gerekir. Özellikler yalnızca şu bölgelerde kullanılabilir: Batı Orta ABD.

## <a name="next-steps"></a>Sonraki adımlar

* [Ağ güvenlik grubu oluşturma](virtual-networks-create-nsg-arm-pportal.md) öğreticisini tamamlayın
* [Uygulama güvenlik gruplarıyla ağ güvenlik grubu oluşturma](create-network-security-group-preview.md) öğreticisini tamamlayın

