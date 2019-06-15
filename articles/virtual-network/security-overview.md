---
title: Azure güvenlik gruplarına genel bakış
titlesuffix: Azure Virtual Network
description: Ağ ve uygulama güvenlik grupları hakkında bilgi edinin. Güvenlik grupları Azure kaynakları arasındaki ağ trafiğini filtrelemenize yardımcı olur.
services: virtual-network
documentationcenter: na
author: malopMSFT
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/26/2018
ms.author: malop;kumud
ms.openlocfilehash: ee976f163bdb00511e2a8f85906aa59aaebbfa47
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67056547"
---
# <a name="security-groups"></a>Güvenlik grupları
<a name="network-security-groups"></a>

Ağ güvenlik grupları ile bir Azure [sanal ağı](virtual-networks-overview.md) içindeki Azure kaynaklarına gelen ve bu kaynaklardan giden ağ trafiğini filtreleyebilirsiniz. Ağ güvenlik grupları, farklı Azure kaynaklarına gelen ya da bu kaynaklardan dışarı giden ağ trafiğine izin veren veya bu trafiği reddeden [güvenlik kuralları](#security-rules) içerir. Sanal ağ dağıtımı ve ağ güvenlik grubu ataması için uygun olan Azure kaynakları hakkında bilgi için bkz. [Azure hizmetleri için sanal ağ tümleştirmesi](virtual-network-for-azure-services.md). Her kural için kaynak, hedef, bağlantı noktası ve protokol belirtebilirsiniz.

Bu makalede, ağ güvenlik gruplarını daha etkili bir şekilde kullanmanıza yardımcı olmak için gereken kavramlar açıklanır. Daha önce bir ağ güvenlik grubu oluşturmadıysanız deneyim edinmek için hızlı [öğreticiyi](tutorial-filter-network-traffic.md) tamamlayabilirsiniz. Ağ güvenlik grupları ve yönetimi hakkında bilginiz varsa bkz. [Ağ güvenlik gruplarını yönetme](manage-network-security-group.md). İletişim sorunları yaşıyorsanız ve ağ güvenlik gruplarıyla ilgili sorunları gidermeniz gerekiyorsa bkz. [Sanal makine ağ trafiği filtresi sorunlarını tanılama](diagnose-network-traffic-filter-problem.md). [Ağ güvenlik grubu akış günlüklerini](../network-watcher/network-watcher-nsg-flow-logging-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) etkinleştirerek bir ağ güvenlik grubu ile ilişkilendirilmiş kaynaklara gelen ve bu kaynaklardan giden [ağ trafiğini analiz edebilirsiniz](../network-watcher/traffic-analytics.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="security-rules"></a>Güvenlik kuralları

Bir ağ güvenlik grubunda Azure abonelik [limitleri](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) dahilinde sıfır veya istenen sayıda kural bulunabilir. Her bir kural aşağıdaki özellikleri belirtir:

|Özellik  |Açıklama  |
|---------|---------|
|Ad|Ağ güvenlik grubu içinde benzersiz bir ad.|
|Öncelik | 100 ile 4096 arasında bir rakam. Kurallar öncelik sırasına göre işleme alınır ve düşük rakamlı kurallar daha yüksek önceliğe sahip olduğundan yüksek rakamlı kurallardan önce uygulanır. Trafik bir kuralla eşleştiğinde işlem durur. Bunun sonucunda yüksek önceliğe sahip olan kurallarla aynı özniteliklere sahip olan önceliği daha düşük olan (yüksek rakamlı) kurallar işleme alınmaz.|
|Kaynak veya hedef| Herhangi bir IP adresi, sınıfsız etki alanları arası yönlendirme (CIDR) bloğu (10.0.0.0/24 gibi), [hizmet etiketi](#service-tags) veya [uygulama güvenlik grubu](#application-security-groups). Bir Azure kaynağı için adres belirtirken kaynağa atanmış olan özel IP adresini belirtmeniz gerekir. Ağ güvenlik grupları, Azure gelen trafik için genel IP adresini özel IP adresine çevirdikten sonra ve giden trafik için özel IP adresini genel IP adreslerine çevirmeden önce işleme alınır. Azure [IP adresleri](virtual-network-ip-addresses-overview-arm.md) hakkında daha fazla bilgi edinin. Aralık, hizmet etiketi veya uygulama güvenlik grubu belirterek daha az sayıda güvenlik kuralı oluşturabilirsiniz. Bir kuralda birden fazla IP adresi veya aralığı belirtme özelliği (birden fazla hizmet etiketi veya uygulama grubu belirtemezsiniz) [genişletilmiş güvenlik kuralı](#augmented-security-rules) olarak adlandırılır. Genişletilmiş güvenlik kuralları yalnızca Resource Manager dağıtım modeliyle oluşturulmuş olan ağ güvenlik gruplarında oluşturulabilir. Klasik dağıtım modeliyle oluşturulmuş olan ağ güvenlik gruplarında birden fazla IP adresi ve IP adresi aralığı belirtemezsiniz. [Azure dağıtım modelleri](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) hakkında daha fazla bilgi edinin.|
|Protocol     | TCP, UDP veya (ancak bunlarla sınırlı değil) içeren herhangi, TCP, UDP ve ICMP. Tek başına ICMP'yi belirtemezsiniz. ICMP gerekiyorsa Tümü seçeneğini kullanın. |
|Direction| Kuralın gelen veya giden trafiğe uygulanma seçeneği.|
|Bağlantı noktası aralığı     |Tek bir bağlantı noktası veya aralık belirtebilirsiniz. Örneğin 80 veya 10000-10005 değerini kullanabilirsiniz. Aralık belirterek oluşturmanız gereken güvenlik kuralı sayısını azaltabilirsiniz. Genişletilmiş güvenlik kuralları yalnızca Resource Manager dağıtım modeliyle oluşturulmuş olan ağ güvenlik gruplarında oluşturulabilir. Klasik dağıtım modeliyle oluşturulmuş olan ağ güvenlik gruplarında aynı güvenlik kuralı içinde birden fazla bağlantı noktası ve bağlantı noktası aralığı belirtemezsiniz.   |
|Eylem     | İzin ver veya reddet        |

Ağ güvenlik grubu güvenlik kuralları, trafiğe izin verilmesi veya trafiğin reddedilmesi için 5 tanımlama grubu bilgisi (kaynak, kaynak bağlantı noktası, hedef, hedef bağlantı noktası ve protokol) ile önceliğe göre değerlendirilir. Var olan bağlantılar için bir akış kaydı oluşturulur. Akış kaydının bağlantı durumuna göre iletişime izin verilir veya iletişim reddedilir. Akış kaydı bir ağ güvenlik grubunun durum bilgisine sahip olmasını sağlar. Örneğin 80 numaralı bağlantı noktasından tüm adreslere doğru giden bir güvenlik kuralı belirtirseniz giden trafiğe yanıt olarak bir gelen güvenlik kuralı belirtmeniz gerekli değildir. Yalnızca iletişimin dışarıdan başlatılması halinde bir gelen güvenlik kuralı belirtmeniz gerekir. Bunun tersi de geçerlidir. Gelen trafiğe bir bağlantı noktası üzerinden izin verilmesi halinde bağlantı noktasından geçen trafiğe yanıt olarak bir giden güvenlik belirtmeniz gerekli değildir.
Akışı etkinleştiren bir güvenlik kuralını kaldırdığınızda mevcut bağlantılar kesintiye uğramayabilir. Bağlantılar durdurulduğunda trafik akışları kesintiye uğrar ve en azından birkaç dakika boyunca hiçbir yönde trafik akışı gerçekleşmez.

Bir ağ güvenlik grubu içinde sınırlı sayıda güvenlik kuralı oluşturabilirsiniz. Ayrıntılar için [Azure limitleri](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) makalesini inceleyin.

## <a name="augmented-security-rules"></a>Genişletilmiş güvenlik kuralları

Genişletilmiş güvenlik kuralları, sanal ağlar için güvenlik tanımını daha basit hale getirerek daha az sayıda kuralla daha geniş çaplı ve karmaşık ağ güvenlik ilkesi tanımlamanızı sağlar. Birden fazla bağlantı noktası ile birden fazla açık IP adresini ve aralığını bir araya getirerek tek ve anlaşılması kolay bir güvenlik kuralı oluşturabilirsiniz. Genişletilmiş kuralları bir kuralın kaynak, hedef ve bağlantı noktası alanlarında kullanabilirsiniz. Güvenlik kuralı tanımınızın bakımını kolaylaştırmak için genişletilmiş güvenlik kurallarını [hizmet etiketleri](#service-tags) veya [uygulama güvenlik gruplarıyla](#application-security-groups) bir arada kullanabilirsiniz. Adresleri, aralıkları ve bir kuralda belirttiğiniz bağlantı noktaları sayısının bir sınırı yoktur. Ayrıntılar için [Azure limitleri](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) makalesini inceleyin.

## <a name="service-tags"></a>Hizmet etiketleri

 Hizmet etiketi, güvenlik kuralı oluşturma sırasındaki karmaşıklığı en aza indirmeye yardımcı olmak için bir IP adresi ön eki grubunu temsil eder. Kendi hizmet etiketinizi oluşturamaz veya bir etiket içinde yer alacak IP adreslerini belirleyemezsiniz. Hizmet etiketine dahil olan adres ön ekleri Microsoft tarafından yönetilir ve hizmet etiketi adresler değiştikçe otomatik olarak güncelleştirilir. Hizmet etiketlerini güvenlik kuralı oluştururken belirli IP adreslerinin yerine kullanabilirsiniz. 
 
 Azure [Genel](https://www.microsoft.com/download/details.aspx?id=56519), [US Government](https://www.microsoft.com/download/details.aspx?id=57063), [Çin](https://www.microsoft.com/download/details.aspx?id=57062) ve [Almanya](https://www.microsoft.com/download/details.aspx?id=57064) bulutları için haftalık yayınlardaki hizmet etiketlerini ve ön ek ayrıntıları içeren listeleri indirip şirket içi güvenlik duvarıyla tümleştirebilirsiniz.

 Güvenlik kuralı tanımlarken aşağıdaki hizmet etiketlerinden faydalanabilirsiniz. Farklı [Azure dağıtım modellerindeki](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) etiket adları arasında küçük farklar vardır.

* **VirtualNetwork** (Resource Manager) (**vırtual_network** Klasik için): Sanal ağ adres alanını (sanal ağ için tanımlanmış olan tüm CIDR aralıkları), bu etiketi içeren bağlı olan tüm şirket içi adres alanlarını ve [eşlenmiş](virtual-network-peering-overview.md) sanal ağlara veya bağlı olan sanal ağı bir [sanal ağ geçidi](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ve adres önekleri kullanılan [kullanıcı tanımlı yollar](virtual-networks-udr-overview.md).
* **AzureLoadBalancer** (Resource Manager) (**AZURE_LOADBALANCER** Klasik için): Bu etiket Azure'nın altyapı yük dengeleyicisini belirtir. Bu etiket, Azure’ın sistem durumu araştırmalarının kaynağı olan [Ana bilgisayar sanal IP adresine](security-overview.md#azure-platform-considerations) (168.63.129.16) çevrilir. Azure yük dengeleyiciyi kullanmıyorsanız bu kuralı geçersiz kılabilirsiniz.
* **Internet** (Resource Manager) (**INTERNET** Klasik için): Bu etiket, sanal ağın dışında olan ve genel Internet ile ulaşılabilen IP adresi alanını belirtir. Adres aralığı [Azure'a ait genel IP adresi alanını](https://www.microsoft.com/download/details.aspx?id=41653) içerir.
* **AzureCloud** (yalnızca Resource Manager): Bu etiket, tüm dahil olmak üzere Azure için IP adresi alanını belirtir [veri merkezi genel IP adresleri](https://www.microsoft.com/download/details.aspx?id=41653). *AzureCloud* değerini belirtirseniz Azure genel IP adreslerine giden trafiğe izin verilir veya trafik reddedilir. Yalnızca belirli bir [bölgede](https://azure.microsoft.com/regions) AzureCloud erişimine izin vermek istiyorsanız bölgeyi belirtebilirsiniz. Örneğin yalnızca Doğu ABD bölgesindeki Azure AzureCloud hizmetine erişim izni vermek istiyorsanız *AzureCloud.EastUS* hizmet etiketini kullanabilirsiniz. 
* **AzureTrafficManager** (yalnızca Resource Manager): Bu etiket Azure Traffic Manager araştırma IP adresleri için IP adresi alanını belirtir. Traffic Manager yoklama IP adresleri hakkında daha fazla bilgi için [Azure Traffic Manager hakkında SSS](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs) sayfasına göz atın. 
* **Depolama** (yalnızca Resource Manager): Bu etiket Azure Storage hizmeti için IP adresi alanını belirtir. *Depolama* değerini belirtirseniz depolama alanına gelen trafiğe izin verilir veya trafik reddedilir. Yalnızca belirli bir [bölgedeki](https://azure.microsoft.com/regions) depolama alanına erişime izin vermek istiyorsanız bölgeyi belirtebilirsiniz. Örneğin yalnızca Doğu ABD bölgesindeki Azure Depolama hizmetine erişim izni vermek istiyorsanız *Storage.EastUS* hizmet etiketini kullanabilirsiniz. Bu etiket hizmetin belirli örneklerini değil yalnızca hizmetin kendisini temsil eder. Örneğin etiket belirli bir Azure Depolama hesabını değil Azure Depolama hizmetini temsil eder. 
* **SQL** (yalnızca Resource Manager): Bu etiket Azure SQL veritabanı, MySQL için Azure veritabanı, PostgreSQL için Azure veritabanı adres ön eklerini belirtir ve Azure SQL veri ambarı Hizmetleri. *Sql* değerini belirtirseniz Sql’e gelen trafiğe izin verilir veya trafik reddedilir. Yalnızca belirli bir [bölgedeki](https://azure.microsoft.com/regions) Sql’e erişime izin vermek istiyorsanız bölgeyi belirtebilirsiniz. Örneğin yalnızca Doğu ABD bölgesindeki Azure SQL Veritabanı hizmetine erişim izni vermek istiyorsanız *Sql.EastUS* hizmet etiketini kullanabilirsiniz. Bu etiket hizmetin belirli örneklerini değil yalnızca hizmetin kendisini temsil eder. Örneğin etiket belirli bir SQL veritabanını veya sunucusunu değil Azure SQL Veritabanı hizmetini temsil eder. 
* **AzureCosmosDB** (yalnızca Resource Manager): Bu etiket, Azure Cosmos veritabanı hizmetin adres ön eklerini belirtir. *AzureCosmosDB* değerini belirtirseniz AzureCosmosDB’ye gelen trafiğe izin verilir veya trafik reddedilir. Yalnızca belirli bir [bölgede](https://azure.microsoft.com/regions) AzureCosmosDB’ye erişime izin vermek istiyorsanız bölgeyi şu biçimde belirtebilirsiniz: AzureCosmosDB.[bölge adı]. 
* **AzureKeyVault** (yalnızca Resource Manager): Bu etiket Azure KeyVault hizmetin adres ön eklerini belirtir. *AzureKeyVault* değerini belirtirseniz AzureKeyVault’a gelen trafiğe izin verilir veya trafik reddedilir. Yalnızca belirli bir [bölgede](https://azure.microsoft.com/regions) AzureKeyVault’a erişime izin vermek istiyorsanız bölgeyi şu biçimde belirtebilirsiniz: AzureKeyVault.[bölge adı]. 
* **EventHub** (yalnızca Resource Manager): Bu etiket, Azure Event Hubs'a hizmetin adres ön eklerini belirtir. *EventHub* değerini belirtirseniz EventHub’a gelen trafiğe izin verilir veya trafik reddedilir. Yalnızca belirli bir [bölgede](https://azure.microsoft.com/regions) EventHub’a erişime izin vermek istiyorsanız bölgeyi şu biçimde belirtebilirsiniz: EventHub.[bölge adı]. 
* **ServiceBus** (yalnızca Resource Manager): Bu etiket, Premium Hizmet katmanını kullanarak Azure Service Bus hizmetin adres ön eklerini belirtir. *ServiceBus* değerini belirtirseniz ServiceBus'a gelen trafiğe izin verilir veya trafik reddedilir. Yalnızca belirli bir [bölgede](https://azure.microsoft.com/regions) ServiceBus’a erişime izin vermek istiyorsanız bölgeyi şu biçimde belirtebilirsiniz: ServiceBus.[bölge adı]. 
* **MicrosoftContainerRegistry** (yalnızca Resource Manager): Bu etiket, Microsoft kapsayıcı kayıt defteri hizmetin adres ön eklerini belirtir. *MicrosoftContainerRegistry* değerini belirtirseniz MicrosoftContainerRegistry'ye gelen trafiğe izin verilir veya trafik reddedilir. Yalnızca belirli bir [bölgede](https://azure.microsoft.com/regions) MicrosoftContainerRegistry’ye erişime izin vermek istiyorsanız bölgeyi şu biçimde belirtebilirsiniz: MicrosoftContainerRegistry.[bölge adı]. 
* **AzureContainerRegistry** (yalnızca Resource Manager): Bu etiket, Azure Container Registry hizmetin adres ön eklerini belirtir. *AzureContainerRegistry* değerini belirtirseniz AzureContainerRegistry'ye gelen trafiğe izin verilir veya trafik reddedilir. Yalnızca belirli bir [bölgede](https://azure.microsoft.com/regions) AzureContainerRegistry’ye erişime izin vermek istiyorsanız bölgeyi şu biçimde belirtebilirsiniz: AzureContainerRegistry.[bölge adı]. 
* **AppService** (yalnızca Resource Manager): Bu etiket Azure AppService hizmetin adres ön eklerini belirtir. *AppService* değerini belirtirseniz AppService'e gelen trafiğe izin verilir veya trafik reddedilir. Yalnızca belirli bir [bölgede](https://azure.microsoft.com/regions) AppService’e erişime izin vermek istiyorsanız bölgeyi şu biçimde belirtebilirsiniz: AppService.[bölge adı]. 
* **AppServiceManagement** (yalnızca Resource Manager): Bu etiket Azure AppService yönetim hizmetin adres ön eklerini belirtir. *AppServiceManagement* değerini belirtirseniz AppServiceManagement'a gelen trafiğe izin verilir veya trafik reddedilir. 
* **ApiManagement** (yalnızca Resource Manager): Bu etiket Azure API Management hizmetinin adres ön eklerini belirtir. Belirtirseniz *ApiManagement* değeri için trafiğe izin verilir veya trafik ApiManagement yönetim arabiriminden reddedilir.  
* **AzureConnectors** (yalnızca Resource Manager): Bu etiket Azure bağlayıcılar hizmetin adres ön eklerini belirtir. *AzureConnectors* değerini belirtirseniz AzureConnectors’a gelen trafiğe izin verilir veya trafik reddedilir. Yalnızca belirli bir [bölgede](https://azure.microsoft.com/regions) AzureConnectors’a erişime izin vermek istiyorsanız bölgeyi şu biçimde belirtebilirsiniz: AzureConnectors.[bölge adı]. 
* **GatewayManager** (yalnızca Resource Manager): Bu etiket Azure Ağ Geçidi Yöneticisi hizmetin adres ön eklerini belirtir. *GatewayManager* değerini belirtirseniz GatewayManager’a gelen trafiğe izin verilir veya trafik reddedilir.  
* **AzureDataLake** (yalnızca Resource Manager): Bu etiket, Azure Data Lake hizmetin adres ön eklerini belirtir. *AzureDataLake* değerini belirtirseniz AzureDataLake’e gelen trafiğe izin verilir veya trafik reddedilir. 
* **AzureActiveDirectory** (yalnızca Resource Manager): Bu etiket AzureActiveDirectory hizmetin adres ön eklerini belirtir. *AzureActiveDirectory* değerini belirtirseniz AzureActiveDirectory’ye gelen trafiğe izin verilir veya trafik reddedilir.  
* **AzureMonitor** (yalnızca Resource Manager): Bu etiket AzureMonitor hizmetin adres ön eklerini belirtir. Belirtirseniz *AzureMonitor* değeri için trafiğe izin veya trafik için AzureMonitor reddedilir. 
* **ServiceFabric** (yalnızca Resource Manager): Bu etiket ServiceFabric hizmetin adres ön eklerini belirtir. Belirtirseniz *ServiceFabric* değeri için trafiğe izin veya trafik için ServiceFabric reddedilir. 
* **AzureMachineLearning** (yalnızca Resource Manager): Bu etiket AzureMachineLearning hizmetin adres ön eklerini belirtir. Belirtirseniz *AzureMachineLearning* değeri için trafiğe izin veya trafik için AzureMachineLearning reddedilir. 
* **BatchNodeManagement** (yalnızca Resource Manager): Bu etiket Azure BatchNodeManagement hizmetin adres ön eklerini belirtir. Belirtirseniz *BatchNodeManagement* değeri için trafiğe izin veya trafik Batch hizmetinden işlem düğümlerine reddedilir.
* **AzureBackup**(yalnızca Resource Manager): Bu etiket AzureBackup hizmetin adres ön eklerini belirtir. AzureBackup değeri belirtirseniz, trafiğe izin verilir veya trafik reddedilir AzureBackup için.

> [!NOTE]
> Hizmet etiketleri Azure hizmetlerinin adres ön eklerini kullanılan özel buluttan gösterir. 

> [!NOTE]
> Azure Depolama veya Azure SQL Veritabanı gibi bir hizmet için bir [sanal ağ hizmet uç noktası](virtual-network-service-endpoints-overview.md) uygularsanız Azure, sanal ağ alt ağına hizmet için bir [rota](virtual-networks-udr-overview.md#optional-default-routes) ekler. Rotada adres ön ekleri ilgili hizmet etiketiyle aynı adres ön ekleri veya CIDR aralıklarıdır.

## <a name="default-security-rules"></a>Varsayılan güvenlik kuralları

Azure, oluşturduğunuz tüm ağ güvenlik gruplarına aşağıdaki varsayılan kuralları ekler:

### <a name="inbound"></a>Gelen

#### <a name="allowvnetinbound"></a>AllowVNetInBound

|Öncelik|source|Kaynak bağlantı noktaları|Hedef|Hedef bağlantı noktaları|Protocol|Access|
|---|---|---|---|---|---|---|
|65000|VirtualNetwork|0-65535|VirtualNetwork|0-65535|Tümü|İzin Ver|

#### <a name="allowazureloadbalancerinbound"></a>AllowAzureLoadBalancerInBound

|Öncelik|source|Kaynak bağlantı noktaları|Hedef|Hedef bağlantı noktaları|Protocol|Access|
|---|---|---|---|---|---|---|
|65001|AzureLoadBalancer|0-65535|0.0.0.0/0|0-65535|Tümü|İzin Ver|

#### <a name="denyallinbound"></a>DenyAllInbound

|Öncelik|source|Kaynak bağlantı noktaları|Hedef|Hedef bağlantı noktaları|Protocol|Access|
|---|---|---|---|---|---|---|
|65500|0.0.0.0/0|0-65535|0.0.0.0/0|0-65535|Tümü|Reddet|

### <a name="outbound"></a>Giden

#### <a name="allowvnetoutbound"></a>AllowVnetOutBound

|Öncelik|source|Kaynak bağlantı noktaları| Hedef | Hedef bağlantı noktaları | Protocol | Access |
|---|---|---|---|---|---|---|
| 65000 | VirtualNetwork | 0-65535 | VirtualNetwork | 0-65535 | Tümü | İzin Ver |

#### <a name="allowinternetoutbound"></a>AllowInternetOutBound

|Öncelik|source|Kaynak bağlantı noktaları| Hedef | Hedef bağlantı noktaları | Protocol | Access |
|---|---|---|---|---|---|---|
| 65001 | 0.0.0.0/0 | 0-65535 | Internet | 0-65535 | Tümü | İzin Ver |

#### <a name="denyalloutbound"></a>DenyAllOutBound

|Öncelik|source|Kaynak bağlantı noktaları| Hedef | Hedef bağlantı noktaları | Protocol | Access |
|---|---|---|---|---|---|---|
| 65500 | 0.0.0.0/0 | 0-65535 | 0.0.0.0/0 | 0-65535 | Tümü | Reddet |

**Kaynak** ve **Hedef** sütunlarında *VirtualNetwork*, *AzureLoadBalancer* ve *Internet*, için IP adresi yerine [hizmet etiketi](#service-tags) belirtilir. Protokol sütunundaki **Tümü** seçeneği TCP, UDP ve ICMP'yi kapsar. Bir kural oluştururken TCP, UDP veya Tümü seçeneğini belirleyebilirsiniz ancak yalnızca ICMP'yi belirtemezsiniz. Bu nedenle kuralınız ICMP gerektiriyorsa protokol için *Tümü* seçeneğini belirleyin. **Kaynak** ve **Hedef** sütunlarında yer alan *0.0.0.0/0* ifadesi tüm adresleri temsil eder. Powershell kullanabilir veya istemciler gibi Azure portalı, Azure CLI'yı * ya da bu ifade için.
 
Varsayılan kuralları kaldıramazsınız ancak daha yüksek önceliğe sahip kurallar oluşturarak onları geçersiz kılabilirsiniz.

## <a name="application-security-groups"></a>Uygulama güvenliği grupları

Uygulama güvenlik grupları ağ güvenliğini uygulamanın yapısının doğal bir uzantısı olarak yapılandırmanıza imkan vererek sanal makineleri gruplamanızı ve ağ güvenlik ilkelerini bu gruplara göre tanımlamanızı sağlar. Açık IP adreslerinin bakımını el ile yapmanıza gerek kalmadan güvenlik ilkesini farklı ölçeklerde yeniden kullanabilirsiniz. Platform açık IP adreslerinin ve birden fazla kural kümesinin karmaşık süreçlerini üstlenerek iş mantığınıza odaklanmanızı sağlar. Uygulama güvenlik gruplarını daha iyi anlamak için aşağıdaki örneği inceleyin:

![Uygulama güvenliği grupları](./media/security-groups/application-security-groups.png)

Yukarıdaki resimde *NIC1* ve *NIC2*, *AsgWeb* uygulama güvenlik grubunun üyeleridir. *NIC3*, *AsgLogic* uygulama güvenlik grubunun üyesidir. *NIC4*, *AsgDb* uygulama güvenlik grubunun üyesidir. Bu örnekteki tüm ağ arabirimleri tek bir uygulama güvenlik grubuna üye olsa da bir ağ arabirimi [Azure sınırları](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) dahilinde birden fazla uygulama güvenlik grubuna üye olabilir. Ağ arabirimlerinin hiçbiri bir ağ güvenlik grubuyla ilişkilendirilmemiştir. *NSG1*, iki alt ağ ile de ilişkilendirilmiştir ve aşağıdaki kuralları içerir:

### <a name="allow-http-inbound-internet"></a>Allow-HTTP-Inbound-Internet

Bu kural, internetten Web sunucularına gelen trafiğe izin vermek için kullanılır. İnternetten gelen trafik, [DenyAllInbound](#denyallinbound) varsayılan güvenlik grubu tarafından reddedildiğinden *AsgLogic* veya *AsgDb* uygulama güvenlik grupları için ek kurala ihtiyaç duyulmaz.

|Öncelik|source|Kaynak bağlantı noktaları| Hedef | Hedef bağlantı noktaları | Protocol | Access |
|---|---|---|---|---|---|---|
| 100 | Internet | * | AsgWeb | 80 | TCP | İzin Ver |

### <a name="deny-database-all"></a>Deny-Database-All

[AllowVNetInBound](#allowvnetinbound) varsayılan güvenlik kuralı aynı sanal ağ içinde bulunan kaynaklar arasındaki tüm iletişime izin verdiğinden, tüm kaynaklardan gelen trafiği reddetmek için bu kurala ihtiyaç duyulur.

|Öncelik|source|Kaynak bağlantı noktaları| Hedef | Hedef bağlantı noktaları | Protocol | Access |
|---|---|---|---|---|---|---|
| 120 | * | * | AsgDb | 1433 | Tümü | Reddet |

### <a name="allow-database-businesslogic"></a>Allow-Database-BusinessLogic

Bu kural *AsgLogic* uygulama güvenlik grubundan *AsgDb* uygulama güvenlik grubuna gelen trafiğe izin verir. Bu kuralın önceliği, *Deny-Database-All* kuralının önceliğinden daha yüksektir. Sonuç olarak bu kural, *Deny-Database-All* kuralından önce işlenir ve böylece *AsgLogic* uygulama güvenlik grubundan gelen trafiğe izin veriler ve diğer tüm trafik engellenir.

|Öncelik|source|Kaynak bağlantı noktaları| Hedef | Hedef bağlantı noktaları | Protocol | Access |
|---|---|---|---|---|---|---|
| 110 | AsgLogic | * | AsgDb | 1433 | TCP | İzin Ver |

Bir uygulama güvenlik grubunu kaynak veya hedef olarak belirten kurallar yalnızca uygulama güvenlik grubuna üye olan ağ arabirimlerine uygulanır. Ağ arabirimi bir uygulama güvenlik grubuna üye değilse, ağ güvenlik grubu alt ağ ile ilişkilendirilmiş olsa dahi kural ağ arabirimine uygulanmaz.

Uygulama güvenlik grupları aşağıdaki sınırlamalara sahiptir:

-   Bir abonelik içinde bulunabilecek uygulama güvenlik grubu sayısı sınırlıdır ve uygulama güvenlik gruplarıyla ilgili başka sınırlar da vardır. Ayrıntılar için [Azure limitleri](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) makalesini inceleyin.
- Bir uygulama güvenlik grubunu bir güvenlik kuralında kaynak ve hedef olarak belirtebilirsiniz. Kaynak veya hedefte birden çok uygulama güvenlik grubu belirtemezsiniz.
- Bir uygulama güvenlik grubuna atanan tüm ağ arabirimleri, uygulama güvenlik grubuna atanmış ilk ağ arabirimiyle aynı sanal ağda olmalıdır. Örneğin, ilk ağ arabirimi *VNet1* adlı sanal ağdaki *AsgWeb* adlı bir uygulama güvenlik grubuna atanmışsa *ASGWeb*’e atanan sonraki tüm ağ arabirimleri *VNet1*’de olmalıdır. Bir uygulama güvenlik grubuna farklı ağlarda bulunan ağ arabirimlerini ekleyemezsiniz.
- Uygulama güvenlik grubunu bir güvenlik kuralında kaynak ve hedef olarak belirtirseniz iki uygulama güvenlik grubundaki ağ arabirimlerinin de aynı sanal ağda bulunması gerekir. Örneğin *AsgLogic* üzerinde *VNet1* içinde bulunan ağ arabirimleri, *AsgDb* üzerinde de *VNet2* içinde bulunan ağ arabirimleri varsa, bir kural içinde *AsgLogic* grubunu kaynak olarak ve *AsgDb* grubunu da hedef olarak belirleyemezsiniz. Hem kaynak hem de hedef uygulama güvenlik gruplarının tüm ağ arabirimlerinin aynı sanal ağ içinde bulunması gerekir.

> [!TIP]
> İhtiyacınız olan güvenlik kuralı sayısını ve kural değiştirme gereksinimini en aza indirmek için, gereken uygulama güvenlik gruplarını planlarken ve kuralları oluştururken tek IP adreslerini veya IP adresi aralıklarını kullanmak yerine hizmet etiketlerini ya da uygulama güvenlik gruplarını kullanın.

## <a name="how-traffic-is-evaluated"></a>Trafik nasıl değerlendirilir?

Birden fazla Azure hizmetinde bulunan kaynakları bir Azure sanal ağına dağıtabilirsiniz. Hizmetlerin tam listesi için bkz. [Sanal ağa dağıtılabilecek hizmetler](virtual-network-for-azure-services.md#services-that-can-be-deployed-into-a-virtual-network). Bir sanal makine içindeki her bir sanal ağ [alt ağına](virtual-network-manage-subnet.md#change-subnet-settings) ve [ağ arabirimine](virtual-network-network-interface.md#associate-or-dissociate-a-network-security-group) sıfır veya bir ağ güvenlik grubu atayabilirsiniz. Bir ağ güvenlik grubunu istediğiniz sayıda alt ağ ve ağ arabirimi ile ilişkilendirebilirsiniz.

Aşağıdaki resimde, ağ güvenlik gruplarının 80 numaralı TCP bağlantı noktası üzerinden gelen ve giden internet trafiğine izin vermek için dağıtılabilecek ağ güvenlik grupları için farklı senaryolar gösterilmektedir:

![NSG-processing](./media/security-groups/nsg-interaction.png)

Azure'ın ağ güvenlik grupları için gelen ve giden kuralları nasıl işlediğini anlamak için yukarıdaki resmi ve aşağıdaki metni inceleyin:

### <a name="inbound-traffic"></a>Gelen trafik

Azure, gelen trafik için ilk olarak varsa bir alt ağ ile ilişkilendirilmiş ağ güvenlik grubu içindeki kuralları ve ardından varsa ağ arabirimi ile ilişkilendirilmiş ağ güvenlik grubundaki kuralları işler.

- **VM1**: Güvenlik kuralları içinde *NSG1* işlenir, ilişkili olduğundan *Subnet1* ve *VM1* bulunduğu *Subnet1*. Gelen trafik için 80 numaralı bağlantı noktasına izin veren bir kural oluşturmadığınız sürece trafik [DenyAllInbound](#denyallinbound) varsayılan güvenlik kuralı tarafından reddedilir ve *NSG2*, ağ arabirimi ile ilişkilendirilmiş olduğundan *NSG2* tarafından değerlendirilmez. *NSG1*, 80 numaralı bağlantı noktasına izin veren bir güvenlik kuralına sahipse trafik *NSG2* tarafından işlenir. 80 numaralı bağlantı noktasından gelen trafiğin sanal makineye iletilmesine izin vermek için hem *NSG1* hem de *NSG2* içinde 80 numaralı bağlantı noktası üzerinden gelen internet bağlantılarına izin veren bir kural olması gerekir.
- **VM2**: Kurallarda *NSG1* çünkü işlenen *VM2* de erişilebilir *Subnet1*. *VM2* ağ arabirimi ile ilişkilendirilmiş bir ağ güvenlik grubu olmadığından *NSG1* üzerinden izin verilen tüm trafiği alır veya *NSG1* tarafından reddedilen tüm trafiği reddeder. Ağ güvenlik grubu bir alt ağ ile ilişkilendirilmiş olduğunda bu alt ağ içindeki tüm kaynaklar için trafiğe izin verilir veya trafik reddedilir.
- **VM3**: Olduğundan, hiçbir ağ güvenlik grubu için ilişkili *Subnet2*, trafiğin bir alt ağa izin verilen ve tarafından işlenen *NSG2*, çünkü *NSG2* ağa ilişkilendirilir arabirimi iliştirilmiş *VM3*.
- **VM4**: Trafiğin izin verilen *VM4,* bir ağ güvenlik grubu için ilişkili olmadığından *Subnet3*, ya da sanal makinede ağ arabirimi. Ağ güvenlik grubu ile ilişkilendirilmiş olmayan alt ağ ve ağ arabirimleri üzerinden gelen tüm ağ trafiğine izin verilir.

### <a name="outbound-traffic"></a>Giden trafik

Azure, giden trafik için ilk olarak varsa bir ağ arabirimi ile ilişkilendirilmiş ağ güvenlik grubu içindeki kuralları ve ardından varsa alt ağ ile ilişkilendirilmiş ağ güvenlik grubundaki kuralları işler.

- **VM1**: Güvenlik kuralları içinde *NSG2* işlenir. 80 numaralı bağlantı noktası üzerinden internete giden trafiği reddeden bir güvenlik kuralı oluşturmadığınız sürece *NSG1* ve *NSG2* içindeki [AllowInternetOutbound](#allowinternetoutbound) varsayılan güvenlik kuralı trafiğe izin verir. *NSG2* içinde 80 numaralı bağlantı noktasından giden trafiği reddeden bir güvenlik kuralı varsa trafik reddedilir ve *NSG1* tarafından değerlendirilmez. Sanal makinenin 80 numaralı bağlantı noktasından giden trafiği reddetmek için ağ güvenlik gruplarından birinde veya her ikisinde 80 numaralı bağlantı noktasından internete giden trafiği reddeden bir kural olması gerekir.
- **VM2**: Bağlı ağ arabiriminin olduğundan tüm trafiği alt ağına ağ arabirimi üzerinden gönderilen *VM2* ilişkili ağ güvenlik grubu yok. *NSG1* içindeki kurallar işlenir.
- **VM3**: Varsa *NSG2* bir güvenlik kuralı sahip, 80 numaralı bağlantı noktasını reddeder, trafik reddedilir. *NSG2* içinde 80 numaralı bağlantı noktasından giden trafiğe izin veren bir güvenlik kuralı varsa *Subnet2* ile ilişkilendirilmiş bir ağ güvenlik grubu bulunmadığından 80 numaralı bağlantı noktasından internete giden trafiğe izin verilir.
- **VM4**: Tüm ağ trafiğine izin verilir *VM4,* bir ağ güvenlik grubu sanal makineye veya çok bağlı ağ arabirimi ile ilişkili olmadığından *Subnet3*.

Bir ağ arabirimi için [geçerli güvenlik kurallarını](virtual-network-network-interface.md#view-effective-security-rules) görüntüleyerek bir ağ arabirimine uygulanmış olan toplu kuralları kolayca görüntüleyebilirsiniz. Azure Ağ İzleyicisi'ndeki [IP akışı doğrulama](../network-watcher/diagnose-vm-network-traffic-filtering-problem.md?toc=%2fazure%2fvirtual-network%2ftoc.json) özelliğini kullanarak da bir ağ arabirimine gelen veya dışarı giden iletişime izin verilip verilmediğini belirleyebilirsiniz. IP akışı doğrulama, iletişime izin verme veya reddetme durumunu ve trafiğe izin veren veya onu reddeden ağ güvenlik kuralının hangisi olduğunu belirler.

> [!NOTE]
> Ağ güvenlik grupları, alt ağlara veya sanal makineleri ve dağıtılan Klasik dağıtım modelinde bulut Hizmetleri ve alt ağlar ya da Resource Manager dağıtım modelindeki ağ arabirimlerinin ilişkilendirilir. Azure dağıtım modelleri hakkında daha fazla bilgi edinmek için bkz. [Azure dağıtım modellerini kavrama](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

> [!TIP]
> Belirli bir nedeniniz yoksa bir ağ güvenlik grubunu bir alt ağ veya ağ arabirimi ile ilişkilendirmenizi ancak ikisiyle birden ilişkilendirmemenizi öneririz. Bir alt ağ ile ilişkilendirilmiş olan ağ güvenlik grubundaki kurallar bir ağ arabirimi ile ilişkilendirilmiş olan ağ güvenlik grubundaki kurallarla çakışabileceğinden çözmeniz gereken beklenmeyen iletişim sorunlarıyla karşılaşabilirsiniz.

## <a name="azure-platform-considerations"></a>Azure platformunda dikkat edilmesi gerekenler

- **Ana bilgisayar düğümünün sanal IP**: DHCP, DNS, IMDS ve sistem durumu izleme gibi temel altyapı hizmetleri, 168.63.129.16 ve 169.254.169.254 numaralı sanallaştırılmış ana bilgisayar IP adresleri sağlanır. Bu IP adresleri Microsoft'a aittir ve tüm bölgelerde bu amaç için kullanılan tek sanallaştırılmış IP adresleridir.
- **Lisanslama (anahtar yönetimi hizmeti)** : Sanal makinelerde çalışan Windows görüntülerinin lisanslanması gerekir. Lisanslama için, lisans isteği sorgularını işleyen Anahtar Yönetimi Hizmeti ana bilgisayar sunucularına bir lisans isteği gönderilir. İstek, bağlantı noktası 1688 üzerinden gönderilir. [default route 0.0.0.0/0](virtual-networks-udr-overview.md#default-route) yapılandırması kullanan dağıtmalar için bu platform kuralı devre dışı bırakılır.
- **Yük dengelenmiş havuzlardaki sanal makineler'de**: Uygulanan kaynak bağlantı noktası ve adres aralığı yük dengeleyiciye değil kaynak bilgisayardan var. Hedef bağlantı noktası ve adres aralığı, yük dengeleyici değil, hedef bilgisayar içindir.
- **Azure hizmeti örnekleri**: HDInsight, uygulama servisi ortamları ve sanal makine ölçek kümeleri gibi çeşitli Azure hizmetlerini örnekleri, sanal ağınızın alt ağlarında dağıtılır. Sanal ağlara dağıtabileceğiniz hizmetlerin tam listesi için bkz. [Azure hizmetleri için sanal ağ](virtual-network-for-azure-services.md#services-that-can-be-deployed-into-a-virtual-network). Bir ağ güvenlik grubunu kaynağın dağıtılmış olduğu alt ağa uygulamadan önce hizmetlerle ilgili olan bağlantı noktası gereksinimlerini öğrendiğinizden emin olun. Gerekli bağlantı noktalarını reddetmeniz halinde hizmet düzgün çalışmaz.
- **Giden e-posta gönderme**: Microsoft Azure sanal makinelerden e-posta göndermek için kimliği doğrulanmış SMTP geçiş hizmetlerini (normalde TCP bağlantı noktası 587 üzerinden bağlanır, ama diğer de bağlı) kullanan önerir. SMTP geçiş hizmetleri, üçüncü taraf e-posta sağlayıcılarının iletileri reddetme olasılığını en aza indirmek için gönderen saygınlığı konusunda uzmanlaşmıştır. Bu tür SMTP geçiş hizmetleri Exchange Online Protection ve SendGrid'i içerir ancak bunlarla sınırlı değildir. Abonelik türünüz ne olursa olsun, Azure'da SMTP geçiş hizmetlerinin kullanımına hiçbir kısıtlama getirilmez. 

  Azure aboneliğinizi 15 Kasım 2017'den önce oluşturduysanız, SMTP geçiş hizmetlerini kullanabileceğiniz gibi, doğrudan TCP bağlantı noktası 25 üzerinden de e-posta gönderebilirsiniz. Aboneliğinizi 15 Kasım 2017'den sonra oluşturduysanız, doğrudan bağlantı noktası 25 üzerinden e-posta gönderemeyebilirsiniz. Bağlantı noktası 25 üzerinden giden iletişimin davranışı, sahip olduğunuz aboneliğe bağlıdır:

     - **Kurumsal Anlaşma**: Giden bağlantı noktası 25 iletişimi izin verilir. Azure platformundan hiçbir kısıtlama uygulanmadan, giden e-postaları doğrudan sanal makinelerden dış e-posta sağlayıcılarına gönderebilirsiniz. 
     - **Kullandıkça Öde:** Tüm kaynaklardan giden bağlantı noktası 25 iletişimi engellenir. Sanal makineden doğrudan dış e-posta sağlayıcılarına (kimliği doğrulanmış SMTP geçişi kullanmadan) e-posta göndermeniz gerekirse, kısıtlamanın kaldırılması için istekte bulunabilirsiniz. İstekler gözden geçirilip Microsoft'un takdirine bağlı olarak onaylanır ve yalnızca dolandırıcılık önleme denetimleri yapıldıktan sonra kabul edilir. İstekte bulunmak için, sorun türü *Teknik*, *Sanal Ağ Bağlantısı*, *E-posta gönderilemiyor (SMTP/Bağlantı Noktası 25)* olan bir destek olayı açın. Destek olayınıza, aboneliğinizin neden kimliği doğrulanmış SMTP geçişi üzerinden değil de doğrudan posta sağlayıcılarına e-posta göndermesi gerektiği konusundaki ayrıntıları da ekleyin. Aboneliğiniz muaf tutulursa, yalnızca muafiyet tarihinden sonra oluşturulmuş sanal makineler bağlantı noktası 25 üzerinden giden iletişimi kurabilir.
     - **MSDN, Azure Pass, Open, Education, BizSpark ve ücretsiz deneme Azure'da**: Tüm kaynaklardan giden bağlantı noktası 25 iletişimi engellenir. Kısıtlamayı kaldırmaya yönelik istekte bulunulamaz çünkü istekler kabul edilmez. Sanal makinenizden e-posta göndermeniz gerekirse, SMTP geçiş hizmetini kullanmanız gerekir.
     - **Bulut hizmeti sağlayıcısı**: Bir bulut hizmeti sağlayıcısı aracılığıyla Azure kaynaklarını kullanan müşteriler, bulut hizmeti sağlayıcısı ile bir destek talebi oluşturun ve güvenli bir SMTP geçiş kullandıysanız isteği Sağlayıcı adına, bir engellemeyi kaldırma çalışması oluşturma.

  Azure bağlantı noktası 25 üzerinde e-posta göndermenize izin verirse, Microsoft e-posta sağlayıcılarının sanal makinenizden gelen e-postayı kabul edeceğini garanti edemez. Belirli bir sağlayıcı sanal makinenizden gelen postayı reddederse, tüm ileti teslimi veya istenmeyen posta filtreleme sorunlarını çözmek için doğrudan sağlayıcıyla çalışmanız veya kimliği doğrulanmış SMTP geçiş hizmeti kullanın.

## <a name="next-steps"></a>Sonraki adımlar

* [Ağ güvenlik grubu oluşturmayı](tutorial-filter-network-traffic.md) öğrenin.
