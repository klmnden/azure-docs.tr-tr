---
title: "Resource Manager ve klasik dağıtımı | Microsoft Docs"
description: "Resource Manager dağıtım modeli ve klasik arasındaki farklılıkları açıklar (veya Hizmet Yönetimi) dağıtım modeli."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: tomfitz
ms.openlocfilehash: 2144e3527b44e3cf508d23fedf7abb4cda595bbf
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/16/2017
---
# <a name="azure-resource-manager-vs-classic-deployment-understand-deployment-models-and-the-state-of-your-resources"></a>Azure Resource Manager ve klasik dağıtım: dağıtım modelleri ve kaynaklarınızın durumunu anlamak
Bu makalede, Azure Resource Manager ve klasik dağıtım modelleri hakkında bilgi edinin. Resource Manager ve klasik dağıtım modellerine dağıtma ve yönetme Azure çözümlerinizi iki farklı şekilde temsil eder. İki farklı API kümesi bunlarla çalışmak ve dağıtılan kaynakları önemli farklılıklar içerebilir. İki model birbirleri ile uyumlu değildir. Bu makalede bu farklar açıklanmaktadır.

Dağıtım ve kaynaklarının yönetimini basitleştirmek için Microsoft, tüm yeni kaynaklar için Kaynak Yöneticisi'ni kullanmanızı önerir. Mümkünse, Microsoft, var olan kaynakların Resource Manager aracılığıyla dağıtmanız önerir.

Resource Manager'da yeniyseniz, ilk tanımlanan terimleri gözden geçirmek isteyebileceğiniz [Azure Resource Manager'a genel bakış](resource-group-overview.md).

## <a name="history-of-the-deployment-models"></a>Dağıtım modelleri geçmişi
Azure başlangıçta yalnızca klasik dağıtım modeli sağlanan. Bu modelde, bağımsız olarak her bir kaynağın var; İlgili kaynaklar gruplamak için hiçbir yolu yoktu. Bunun yerine, el ile çözüm ya da uygulama yapılan hangi kaynaklara izlemek ve eşgüdümlü bir yaklaşım yönetmeyi unutmayın zorunda kaldı. Bir çözümü dağıtmak için portal aracılığıyla ayrı ayrı her bir kaynak oluşturmak veya tüm kaynakların doğru sırada dağıtılan bir komut dosyası oluşturmak zorunda kalındı. Bir çözümü silmek için ayrı ayrı her bir kaynağın silme gerekiyordu. Kolayca uygulanır ve ilgili kaynaklar için erişim denetimi ilkeleri güncelleştirin. Son olarak, geçerli yardımcı şartlarını etiket kaynaklarına etiketleri kaynaklarınızı izleme ve faturalama yönetme.

2014'te Azure Kaynak Yöneticisi, bir kaynak grubu kavramı eklenen kullanıma sunuldu. Bir kaynak grubu, ortak bir yaşam döngüsü paylaşmak kaynaklar için bir kapsayıcıdır. Resource Manager dağıtım modeli çeşitli avantajlar sunar:

* Dağıtmak, yönetmek ve bu işleme ayrı ayrı hizmetleri yerine, çözümünüz için tüm Hizmetleri Grup olarak izleme.
* Art arda çözümünüzü yaşam döngüsü dağıtmak ve kaynaklarınızın tutarlı bir durumda dağıtılması da size güven verir.
* Kaynak grubunuzdaki tüm kaynaklara erişim denetimini uygulayabilirsiniz ve yeni kaynaklar kaynak grubuna eklendiğinde bu ilkeleri otomatik olarak uygulanır.
* Aboneliğinizdeki tüm kaynakları mantıksal olarak düzenlemek için kaynaklarınıza etiketler ekleyebilirsiniz.
* Çözümünüz için altyapıyı tanımlamak için JavaScript nesne gösterimi (JSON) kullanabilirsiniz. JSON dosyasının bir Resource Manager şablonu olarak bilinir.
* Doğru sırayla dağıtılmalarını sağlamak için kaynaklarınız arasındaki bağımlılıkları tanımlayabilirsiniz.

Resource Manager eklendiğinde tüm kaynakları firmalarda geriye dönük varsayılan kaynak grubuna eklendi. Bir kaynak şimdi Klasik dağıtım aracılığıyla oluşturursanız, bu kaynak grubu dağıtım sırasında belirtmedi olsa bile kaynak bu hizmet için varsayılan bir kaynak grubu içinde otomatik olarak oluşturulur. Ancak, yalnızca var olan bir kaynak grubu içindeki kaynak için Resource Manager modeli dönüştürülmüş anlamına gelmez.

## <a name="understand-support-for-the-models"></a>Modelleri için destek anlama
Dikkat edilmesi gereken üç senaryo vardır:

1. Bulut Hizmetleri, Resource Manager dağıtım modeli desteklemez.
2. Sanal makineler, depolama hesapları ve sanal ağlar Resource Manager ve klasik dağıtım modellerini destekler.
3. Diğer Azure Hizmetleri Kaynak Yöneticisi'ni destekler.

Kaynak Klasik dağıtım oluşturduysanız sanal makineler, depolama hesapları ve sanal ağlar için üzerinde Klasik işlemleri aracılığıyla çalışmaya devam etmeniz gerekir. Sanal makine, depolama hesabı ya da sanal ağ Resource Manager dağıtım oluşturulmuşsa, Resource Manager işlemleri kullanarak devam etmeniz gerekir. Aboneliğinizi Resource Manager ve klasik dağıtımı oluşturulan kaynakları bir karışımını içeriyorsa bu ayrım kafa karıştırıcı elde edebilirsiniz. Kaynaklar aynı işlemleri desteklemediğinden bu kaynakların bileşimini beklenmeyen sonuçlar oluşturabilirsiniz.

Bazı durumlarda, bir Kaynak Yöneticisi komut Klasik dağıtım oluşturulan bir kaynak hakkında bilgi alabilir veya Klasik bir kaynağı başka bir kaynak grubuna taşımak gibi bir yönetim görevi gerçekleştirebilirsiniz. Ancak, bu gibi durumlarda türü Resource Manager işlemlerini destekleyen izlenim vermemelisiniz. Örneğin, Klasik dağıtım ile oluşturulmuş bir sanal makine içeren bir kaynak grubu olduğunu varsayalım. Aşağıdaki Resource Manager PowerShell komutu çalıştırırsanız:

```powershell
Get-AzureRmResource -ResourceGroupName ExampleGroup -ResourceType Microsoft.ClassicCompute/virtualMachines
```

Sanal makine döndürür:

```powershell
Name              : ExampleClassicVM
ResourceId        : /subscriptions/{guid}/resourceGroups/ExampleGroup/providers/Microsoft.ClassicCompute/virtualMachines/ExampleClassicVM
ResourceName      : ExampleClassicVM
ResourceType      : Microsoft.ClassicCompute/virtualMachines
ResourceGroupName : ExampleGroup
Location          : westus
SubscriptionId    : {guid}
```

Ancak, Resource Manager cmdlet'i **Get-AzureRmVM** yalnızca Resource Manager aracılığıyla dağıtılan sanal makineleri döndürür. Aşağıdaki komut Klasik dağıtım oluşturulan sanal makinenin döndürmez.

```powershell
Get-AzureRmVM -ResourceGroupName ExampleGroup
```

Yalnızca Kaynak Yöneticisi desteği etiketleri oluşturulan kaynakları. Klasik kaynaklar için etiketler uygulanamıyor.

## <a name="changes-for-compute-network-and-storage"></a>İşlem, ağ ve depolama için değişiklikleri
Aşağıdaki diyagramda, Resource Manager aracılığıyla dağıtılan işlem, ağ ve depolama kaynakları görüntüler.

![Resource Manager mimarisi](./media/resource-manager-deployment-model/arm_arch3.png)

Kaynaklar arasındaki aşağıdaki ilişkilerini dikkat edin:

* Tüm kaynaklar bir kaynak grubu içindeki mevcut.
* Sanal makinesi, disklerini (gerekli) blob storage'da depolamak için depolama kaynak Sağlayıcısı'nda tanımlanan belirli bir depolama hesabı bağlıdır.
* Sanal makine (gerekli) ağ kaynak sağlayıcısı ve kullanılabilirlik işlem kaynak Sağlayıcısı'nda tanımlanan (isteğe bağlı) kümesi içinde tanımlanan belirli bir NIC başvurur.
* NIC (gerekli) sanal makinenin atanan IP adresi alt ağı sanal ağın (gerekli) sanal makine için ve (isteğe bağlı) bir ağ güvenlik grubuna başvuruyor.
* Bir sanal ağ içindeki alt ağı ağ güvenlik grubu (isteğe bağlı) başvuruyor.
* Yük Dengeleyici örneği (isteğe bağlı) bir sanal makine NIC içerir ve bir yük dengeleyici genel veya özel IP adresi (isteğe bağlı) başvuran IP adresleri arka uç havuzu başvurur.

Bileşenleri ve klasik dağıtımı için ilişkilerini şunlardır:

![Klasik mimarisi](./media/resource-manager-deployment-model/arm_arch1.png)

Bir sanal makineyi barındırmak için Klasik çözüm içerir:

* Sanal makineleri (işlem) barındırmak için bir kapsayıcı olarak davranır gerekli bulut hizmeti. Sanal makineleri otomatik olarak bir ağ arabirimi kartı (NIC) ile sağlanan ve Azure tarafından bir IP adresi atanır. Ayrıca, bulut hizmeti, bir dış yük dengeleyici örneği, bir ortak IP adresi ve Windows tabanlı sanal makineler için Uzak Masaüstü ile uzaktan PowerShell trafik ve Linux tabanlı sanal makineler için güvenli Kabuk (SSH) trafiğine izin vermek için varsayılan uç noktaları içerir.
* VHD'ler işletim sistemi, geçici ve ek veri diskleri (Depolama) dahil olmak üzere bir sanal makine için depolar gerekli depolama hesabı.
* Görevi gören bir alt ağlı yapısı oluşturun ve üzerinde sanal makinenin bulunduğu alt ağ belirlemek ek bir kapsayıcı, isteğe bağlı bir sanal ağ (ağ).

İşlem, ağ ve depolama kaynak sağlayıcıları nasıl etkileşim değişiklikler aşağıdaki tabloda açıklanmaktadır:

| Öğe | Klasik | Resource Manager |
| --- | --- | --- |
| Virtual Machines için Bulut Hizmeti |Bulut Hizmeti, platformdan ve Yük Dengeleme’den Uygunluk gereken sanal makineleri barındırmak için bir kapsayıcıydı. |Bulut Hizmeti artık yeni modeli kullanarak bir Sanal Makine oluşturmak için gereken nesne değildir. |
| Sanal Ağlar |Bir sanal ağ sanal makine için isteğe bağlıdır. Dahil edilmişse, sanal ağ Resource Manager ile dağıtılamıyor. |Sanal makine Resource Manager ile dağıtılan bir sanal ağ gerektirir. |
| Depolama Hesapları |Sanal makine işletim sistemi, geçici ve ek veri disklerinin VHD'ler depolayan bir depolama hesabı gerektirir. |Sanal makinesi, disklerini blob storage'da depolamak için bir depolama hesabı gerektirir. |
| Kullanılabilirlik Kümeleri |Platformun uygunluğu Sanal Makinelerde "AvailabilitySetName" yapılandırılarak belirtilirdi. Hata etki alanlarının en yüksek sayısı 2’ydi. |Kullanılabilirlik kümesi, Microsoft.Compute Sağlayıcısı tarafından sağlanan bir kaynaktır. Yüksek kullanılabilirliğin gerektiği Sanal Makineler Kullanılabilirlik Kümesi içinde bulunmalıdır. Hata etki alanlarının en yüksek sayısı artık 3’tür. |
| Benzeşim Grupları |Benzeşim Grupları Sanal Ağlar oluşturmak için gerekliydi. Ancak, Bölgesel Sanal Aağlar girişiyle artık gerekmiyordu. |Basitleştirmek için, Benzeşim Grupları kavramı Azure Resource Manager aracılığıyla sunulan API'lerde yok. |
| Yük Dengeleme |Bulut Hizmeti oluşturulması dağıtılan Virtual Machines için örtük yük dengeleyici sağlar. |Load Balancer, Microsoft.Network sağlayıcısı tarafından sunulan bir kaynaktır. Yük dengeli olması gereken Virtual Machines’in birincil ağ arabirimi yük dengeleyiciye baş vurmalıdır. Yük Dengeleyiciler dahili ve harici olabilir. Bir yük dengeleyici örneği (isteğe bağlı) bir sanal makine NIC içerir ve bir yük dengeleyici genel veya özel IP adresi (isteğe bağlı) başvuran IP adresleri arka uç havuzu başvurur. [Daha fazla bilgi edinin.](../virtual-network/resource-groups-networking.md) |
| Sanal IP Adresi |Bulut Hizmetleri, bir bulut hizmeti için bir VM eklendiğinde varsayılan VIP'i (sanal IP adresi) alır. Sanal IP Adresi örtük yük dengeleyiciyle ilişkili adrestir. |Genel IP Adresi, Microsoft.Network sağlayıcısı tarafından sunulan bir kaynaktır. Genel IP adresi statik (ayrılmış) veya dinamik olabilir. Dinamik genel IP'ler bir yük dengeleyiciye atanabilir. Genel IP’ler Güvenlik Grupları kullanılarak güvenli hale getirilebilir. |
| Ayrılmış IP Adresi |Azure’da bir IP adresini ayırabilir ve IP Adresinin yapışkan olmasını sağlamak için bunu Bulut Hizmetiyle ilişkilendirebilirsiniz. |Genel IP adresi statik modunda oluşturulabilir ve ayrılmış bir IP adresi ile aynı özelliği sunar. |
| Genel IP Adresi (PIP) / VM |Genel IP adresleri de doğrudan bir VM'ye ilişkilendirilebilir. |Genel IP Adresi, Microsoft.Network sağlayıcısı tarafından sunulan bir kaynaktır. Genel IP adresi statik (ayrılmış) veya dinamik olabilir. |
| Uç Noktalar |Giriş Uç Noktaları, belirli bağlantı noktaları bağlantısının açık olması için bir Sanal Makinede yapılandırılmalıdır. Sanal makinelere bağlanmanın yaygın modlarında biri de giriş uç noktaları ayarlanarak yapılır. |Gelen NAT kuralları, VM’lere bağlanması için belirli bağlantı noktalarındaki uç noktaların etkinleştirilmesiyle aynı beceriyi gerçekleştirmek için Yük Dengeleyicilerde yapılandırılabilir. |
| DNS Adı |Bulut hizmeti örtük bir genel benzersiz DNS Adı almalıdır. Örneğin: `mycoffeeshop.cloudapp.net`. |DNS Adları, Genel IP Adresi kaynağında belirtilebilen isteğe bağlı parametrelerdir. Şu biçimde - FQDN'sidir `<domainlabel>.<region>.cloudapp.azure.com`. |
| Ağ Arabirimleri |Birincil ve İkincil Ağ Arabirimi ve özellikleri Sanal makinenin ağ yapılandırması olarak tanımlanmıştı. |Ağ Arabirimi, Microsoft.Network Sağlayıcısı tarafından sunulan bir kaynaktır. Ağ Arabiriminin yaşam döngüsü bir Sanal Makineye bağlı değildir. (Gerekli) sanal makinenin atanan IP adresi alt ağı sanal ağın (gerekli) sanal makine için ve (isteğe bağlı) bir ağ güvenlik grubuna başvuruyor. |

Sanal ağlar farklı dağıtım modelinden bağlanma hakkında bilgi edinmek için [Portalı'nda farklı dağıtım modelinden sanal ağlara bağlanabilir](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).

## <a name="migrate-from-classic-to-resource-manager"></a>Klasik'ten Kaynak Yöneticisi geçirme
Resource Manager dağıtım Klasik dağıtımından kaynaklarınızı geçirmeye hazır olduğunuzda bkz:

1. [Teknik ya ilişkin ayrıntılar platform desteklenen geçiş Klasik'ten Azure Kaynak Yöneticisi](../virtual-machines/windows/migration-classic-resource-manager-deep-dive.md)
2. [Iaas Klasik kaynaklardan Azure Resource Manager için desteklenen platform geçişi](../virtual-machines/windows/migration-classic-resource-manager-overview.md)
3. [Iaas kaynaklarına Klasikten Azure Resource Manager Azure PowerShell kullanarak geçirme](../virtual-machines/windows/migration-classic-resource-manager-ps.md)
4. [Iaas kaynaklarını Klasikten Azure Resource Manager'da Azure CLI kullanarak geçirme](../virtual-machines/virtual-machines-linux-cli-migration-classic-resource-manager.md)

## <a name="frequently-asked-questions"></a>Sık sorulan sorular
**Klasik dağıtım kullanılarak oluşturulan sanal bir ağa dağıtmak için Kaynak Yöneticisi'ni kullanarak bir sanal makine oluşturabilir miyim?**

Bu yapılandırma desteklenmiyor. Klasik dağıtım kullanılarak oluşturulmuş bir sanal ağ içinde bir sanal makine dağıtmak için Kaynak Yöneticisi'ni kullanamazsınız.

**Klasik dağıtım modeli kullanılarak oluşturulmuş kullanıcı görüntüsünden Kaynak Yöneticisi'ni kullanarak bir sanal makine oluşturabilir miyim?**

Bu yapılandırma desteklenmiyor. Ancak, Klasik dağıtım modeli kullanılarak oluşturulmuş bir depolama hesabından VHD dosyaları kopyalayın ve Resource Manager aracılığıyla oluşturulan yeni bir hesap ekleyin.

**Aboneliğimi ilgili kotaya etkisi nedir?**

Sanal makineler, sanal ağlar ve Azure Resource Manager aracılığıyla oluşturulan depolama hesapları kotaları diğer kota ayrıdır. Her abonelik yeni API'leri kullanarak kaynak oluşturmak için kotaları alır. Ek kotalar hakkında daha fazla bilgiyi [burada](../azure-subscription-service-limits.md) okuyabilirsiniz.

**Sanal makineler, sanal ağlar ve depolama hesaplarını Resource Manager API'leri aracılığıyla sağlamak için otomatik betiklerimi kullanmaya devam edebilirsiniz?**

Tüm otomasyon ve derlediğiniz betikleri varolan sanal makineler, Azure hizmet yönetimi modu altında oluşturulan sanal ağlar için çalışmaya devam eder. Ancak, komut dosyaları Resource Manager moduna aracılığıyla aynı kaynakların oluşturulmasında yeni şemayı kullanacak şekilde güncelleştirilmesi gerekir.

**Azure Resource Manager şablonları örneklerini nerede bulabilirim?**

Başlangıç şablonu kapsamlı bir kümesini bulunabilir [Azure Resource Manager hızlı başlangıç şablonları](https://azure.microsoft.com/documentation/templates/).

## <a name="next-steps"></a>Sonraki adımlar
* Bir sanal makine, depolama hesabı ve sanal ağ tanımlayan şablonu oluşturulmasını yol için bkz: [Resource Manager şablonu Kılavuzu](resource-manager-template-walkthrough.md).
* Bir şablonu dağıtmak için komutları görmek için bkz: [Azure Resource Manager şablonu ile bir uygulamayı dağıtmak](resource-group-template-deploy.md).

