---
title: "Azure Sanal Makine Ölçek Kümelerine Genel Bakış | Microsoft Docs"
description: "Azure Sanal Makine Ölçek Kümeleri hakkında daha fazla bilgi edinin"
services: virtual-machine-scale-sets
documentationcenter: 
author: gbowerman
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/10/2017
ms.author: guybo
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 97acd09d223e59fbf4109bc8a20a25a2ed8ea366
ms.openlocfilehash: 2071debe24ef2705b2ee05ce1210a813234cf277
ms.lasthandoff: 03/10/2017


---
# <a name="what-are-virtual-machine-scale-sets-in-azure"></a>Azure’daki sanal makine ölçek kümeleri nedir?
Sanal makine ölçek kümeleri, özdeş VM’lerden oluşan bir sanal makine kümesini dağıtıp yönetmek için kullanabileceğiniz bir Azure İşlem kaynağıdır. Tüm VM’lerin aynı şekilde yapılandırıldığı ölçek kümeleri, gerçek otomatik ölçeklendirmeyi destekleyecek şekilde tasarlanmıştır (VM’lerin önceden sağlanması gerekmez) ve böylece büyük işlem, büyük veri ve kapsayıcılı iş yüklerini hedefleyen büyük ölçekli hizmetler oluşturmayı kolay hale getirir.

İşlem kaynaklarının ölçeğini artırmaya veya azaltmaya gerek duyan uygulamalar için ölçeklendirme işlemleri, arıza ve güncelleştirme etki alanlarında örtülü olarak dengelenir. Ölçek kümelerine giriş için bkz. [Azure blog duyuruları](https://azure.microsoft.com/blog/azure-virtual-machine-scale-sets-ga/).

Ölçek kümeleri hakkında daha fazla bilgi için şu videoları izleyin:

* [Mark Russinovich, Azure ölçek kümeleri hakkında konuşuyor](https://channel9.msdn.com/Blogs/Regular-IT-Guy/Mark-Russinovich-Talks-Azure-Scale-Sets/)  
* [Guy Bowerman ile Sanal Makine Ölçek Kümeleri](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-191-Virtual-Machine-Scale-Sets-with-Guy-Bowerman)

## <a name="creating-and-managing-scale-sets"></a>Ölçek kümeleri oluşturma ve yönetme
[Azure portalında](https://portal.azure.com) *yeni* öğesini seçip arama çubuğuna "ölçek" yazarak bir ölçek kümesi oluşturabilirsiniz. Sonuçlar arasında "Sanal makine ölçek kümesi" seçeneği listelenir. Buradan gerekli alanları doldurarak ölçek kümenizi özelleştirip dağıtabilirsiniz. Portalda CPU kullanımına göre temel otomatik ölçeklendirme kurallarını ayarlamaya yönelik seçenekler de mevcuttur.

Ölçek kümeleri, tıpkı tek Azure Resource Manager VM’lerinde olduğu gibi JSON şablonları ve [REST API’leri](https://msdn.microsoft.com/library/mt589023.aspx) kullanılarak da tanımlanıp dağıtılabilir. Bu nedenle, tüm standart Azure Resource Manager dağıtım yöntemleri kullanılabilir. Şablonlar hakkında daha fazla bilgi için bkz. [Azure Resource Manager şablonları yazma](../azure-resource-manager/resource-group-authoring-templates.md).

Sanal makine ölçek kümelerine ilişkin bir dizi örnek, [buradaki](https://github.com/Azure/azure-quickstart-templates) Azure Hızlı Başlangıç şablonları GitHub deposunda bulunabilir (başlığında *vmss* içeren şablonları arayın).

Bu şablonların ayrıntı sayfalarında, portal dağıtım özelliğine bağlanan bir düğme bulunur. Ölçek kümesini dağıtmak için düğmeye tıklayın ve ardından portalda gerekli olan tüm parametreleri doldurun. Bir kaynağın büyük harfleri veya büyük/küçük harflerin karışımını destekleyip desteklemediğinden emin değilseniz, parametre değerlerinde küçük harf ve sayı kullanılması her zaman daha güvenlidir. Burada ayrıca bir ölçek kümesi şablonunun kullanışlı bir video açıklaması verilmiştir:

[VM Ölçek Kümesi Şablon Açıklaması](https://channel9.msdn.com/Blogs/Azure/VM-Scale-Set-Template-Dissection/player)

## <a name="scaling-a-scale-set-out-and-in"></a>Ölçek kümesinin ölçeğini artırma veya azaltma
Azure portalında bir ölçek kümesinin kapasitesini _Ayarlar_ altında _Ölçeklendirme_ bölümüne tıklayarak değiştirebilirsiniz. 

Ölçek kümesi kapasitesini komut satırında değiştirmek için, [Azure CLI](https://github.com/Azure/azure-cli) bir _scale_ komutu sağlar. Örneğin, bir ölçek kümesini 10 VM kapasitesine ayarlamak için:

```bash
az vmss scale -g resourcegroupname -n scalesetname --new-capacity 10 
```

PowerShell kullanarak bir ölçek kümesindeki VM sayısını ayarlamak için, _Update-AzureRmVmss_ komutunu kullanın:

```PowerShell
$vmss = Get-AzureRmVmss -ResourceGroupName resourcegroupname -VMScaleSetName scalesetname  
$vmss.Sku.Capacity = 10
Update-AzureRmVmss -ResourceGroupName resourcegroupname -Name scalesetname -VirtualMachineScaleSet $vmss
```

Azure Resource Manager şablonu kullanarak bir ölçek kümesindeki sanal makinelerin sayısını artırmak ya da azaltmak için, *capacity* özelliğini değiştirin ve şablonu yeniden dağıtın. Bu kolaylık, ölçek kümelerini Azure otomatik ölçeklendirme ile tümleştirmeyi veya Azure otomatik ölçeklendirme tarafından desteklenmeyen özel ölçek olayları tanımlamanız gerekirse kendi özel ölçeklendirme katmanınızı yazmayı kolay hale getirir. 

Kapasiteyi değiştirmek için bir Azure Resource Manager şablonunu yeniden dağıtıyorsanız, yalnızca güncel kapasiteli 'SKU' özellik paketini içeren çok daha küçük bir şablon tanımlayabilirsiniz. Bir örnek [burada](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) gösterilmiştir.

## <a name="autoscale"></a>Otomatik Ölçeklendirme

Bir ölçek kümesi Azure portalında oluşturulduğunda isteğe bağlı olarak otomatik ölçeklendirme ayarlarıyla yapılandırılarak VM sayısının ortalama CPU kullanımına göre artırılması veya azaltılması sağlanabilir. [Azure hızlı başlangıç şablonlarındaki](https://github.com/Azure/azure-quickstart-templates) birçok ölçek kümesi şablonu otomatik ölçeklendirme ayarlarını tanımlar. Mevcut bir ölçek kümesine de otomatik ölçeklendirme ayarları ekleyebilirsiniz. Örneğin, burada bir ölçek kümesine CPU tabanlı otomatik ölçeklendirme eklemek için bir Azure PowerShell betiği verilmiştir:

```PowerShell

$subid = "yoursubscriptionid"
$rgname = "yourresourcegroup"
$vmssname = "yourscalesetname"
$location = "yourlocation" # e.g. southcentralus

$rule1 = New-AzureRmAutoscaleRule -MetricName "Percentage CPU" -MetricResourceId /subscriptions/$subid/resourceGroups/$rgname/providers/Microsoft.Compute/virtualMachineScaleSets/$vmssname -Operator GreaterThan -MetricStatistic Average -Threshold 60 -TimeGrain 00:01:00 -TimeWindow 00:05:00 -ScaleActionCooldown 00:05:00 -ScaleActionDirection Increase -ScaleActionValue 1
$rule2 = New-AzureRmAutoscaleRule -MetricName "Percentage CPU" -MetricResourceId /subscriptions/$subid/resourceGroups/$rgname/providers/Microsoft.Compute/virtualMachineScaleSets/$vmssname -Operator LessThan -MetricStatistic Average -Threshold 30 -TimeGrain 00:01:00 -TimeWindow 00:05:00 -ScaleActionCooldown 00:05:00 -ScaleActionDirection Decrease -ScaleActionValue 1
$profile1 = New-AzureRmAutoscaleProfile -DefaultCapacity 2 -MaximumCapacity 10 -MinimumCapacity 2 -Rules $rule1,$rule2 -Name "autoprofile1"
Add-AzureRmAutoscaleSetting -Location $location -Name "autosetting1" -ResourceGroup $rgname -TargetResourceId /subscriptions/$subid/resourceGroups/$rgname/providers/Microsoft.Compute/virtualMachineScaleSets/$vmssname -AutoscaleProfiles $profile1
```

 Ölçeklendirme için geçerli ölçümlerin bir listesini _Microsoft.Compute/virtualMachineScaleSets_ başlığı altında [Azure İzleyici ile desteklenen ölçümler](../monitoring-and-diagnostics/monitoring-supported-metrics.md) içinde bulabilirsiniz. Zamanlama tabanlı ölçeklendirme ve uyarı sistemleri ile tümleştirme için web kancalarını kullanma gibi daha gelişmiş otomatik ölçeklendirme seçenekleri de sunulur.

## <a name="monitoring-your-scale-set"></a>Ölçek kümenizi izleme
[Azure portalı](https://portal.azure.com) ölçek kümelerini listeler ve özelliklerini gösterir. Portal ayrıca, hem ölçek kümelerinde hem de bir ölçek kümesindeki her VM’de ayrı ayrı gerçekleştirilebilen yönetim işlemlerini de destekler. Portal ayrıca özelleştirilebilir bir kaynak kullanımı grafiği sağlar. Bir Azure kaynağının temel aldığı JSON tanımını görmeniz veya düzenlemeniz gerekiyorsa, [Azure Kaynak Gezgini](https://resources.azure.com)’ni de kullanabilirsiniz. Ölçek kümeleri Microsoft.Compute Azure Kaynak Sağlayıcısı altındaki bir kaynaktır; bu nedenle, bu siteden aşağıdaki bağlantıları genişleterek bunları görebilirsiniz:

**Abonelikler -> aboneliğiniz -> resourceGroups -> sağlayıcılar -> Microsoft.Compute -> virtualMachineScaleSets -> ölçek kümeniz -> vs.**

## <a name="scale-set-scenarios"></a>Ölçek kümesi senaryoları
Bu bölümde tipik ölçek kümesi senaryolarından bazıları listelenmektedir. Daha yüksek düzeydeki bazı Azure hizmetleri (Batch, Service Fabric, Azure Container Service gibi) bu senaryoları kullanır.

* **RDP / SSH’den ölçek kümesi örneklerine** - Bir sanal ağ ve ölçek kümesi içinde genel IP adresleri atanmamış tek VM’ler içinde bir ölçek kümesi oluşturulur. Bu ilke, işlem kılavuzunuzdaki tüm düğümlere ayrı genel IP adresleri atama maliyeti ve yönetim yükünü ortadan kaldırır. Bu VM’lere sanal ağınızdaki genel IP adresi atanabilen diğer kaynaklardan (örneğin, yük dengeleyiciler ve tek başına sanal makineler) bağlanabilirsiniz.
* **NAT kurallarını kullanarak VM’lere bağlanma** - Bir genel IP adresi oluşturabilir, bunu bir yük dengeleyiciye bağlayabilir ve IP adresi üzerindeki bağlantı noktalarını ölçek kümesindeki bir VM’in bağlantı noktasına eşleyen gelen NAT havuzu tanımlayabilirsiniz. Örneğin:
  
  | Kaynak | Kaynak Bağlantı Noktası | Hedef | Hedef Bağlantı Noktası |
  | --- | --- | --- | --- |
  |  Genel IP |Bağlantı Noktası 50000 |vmss\_0 |Bağlantı noktası 22 |
  |  Genel IP |Bağlantı noktası 50001 |vmss\_1 |Bağlantı noktası 22 |
  |  Genel IP |Bağlantı noktası 50002 |vmss\_2 |Bağlantı noktası 22 |
  
   Bu örnekte tek bir genel IP adresi kullanarak bir ölçek kümesindeki her VM ile bir SSH bağlantısı etkinleştirmek üzere NAT kuralları tanımlanmıştır: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat)
  
   RDP ve Windows ile aynı işlemi yapmanın örneği ise burada verilmiştir: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-nat)
* **"Sıçrama kutusu" kullanarak VM’lere bağlanma** - Aynı sanal ağ için bir ölçek kümesi ve tek başına VM oluşturursanız, tek başına VM ve ölçek kümesinin VM’leri, Sanal Ağ/Alt Ağ tarafından tanımlanan iç IP adreslerini kullanarak birbirine bağlanabilir. Genel bir IP adresi oluşturup tek başına VM’e atarsanız, tek başına VM’e RDP veya SSH uygulayabilir ve ardından bu makineden ölçek kümesi örneklerine bağlanabilirsiniz. Bu noktada, basit ölçek kümesinin, varsayılan yapılandırmasında genel IP adresine sahip basit bir tek başına VM’den doğal olarak daha güvenli olduğunu fark edebilirsiniz.
  
   Örneğin, bu şablon bağımsız bir VM’de basit bir ölçek kümesi dağıtır: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-jumpbox](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-jumpbox)
* **Ölçek kümesi örneklerine yük devretme** - "Hepsini bir kez deneme" yaklaşımı kullanarak bir VM işlem kümesine çalışma iletmek istiyorsanız, uygun katman&4; yük dengeleme kurallarıyla bir Azure yük dengeleyici yapılandırabilirsiniz. Belirtilen bir protokol, aralık ve istek yolu ile bağlantı noktalarına ping göndererek, uygulamanızın çalıştığını doğrulamak için araştırmalar tanımlayabilirsiniz. Azure [Application Gateway](https://azure.microsoft.com/services/application-gateway/) ayrıca katman&7; ve daha karmaşık yük dengeleme senaryolarıyla birlikte ölçek kümelerini destekler.
  
   Bu örnekte Apache web sunucuları çalıştıran bir ölçek kümesi oluşturulur ve her VM’e gelen yükü dengelemek için yük dengeleyici kullanılır: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-ubuntu-web-ssl](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-ubuntu-web-ssl) (Microsoft.Network/loadBalancers kaynak türüne ve virtualMachineScaleSet içindeki networkProfile ve extensionProfile öğesine bakın)

   Bu örnekte bir Application Gateway kullanılmaktadır. Linux:  [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-ubuntu-app-gateway](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-ubuntu-app-gateway). Windows: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-app-gateway](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-app-gateway) 

* **Bir ölçek kümesini PaaS küme yöneticisi içinde işlem kümesi olarak dağıtma** - Ölçek kümeleri bazı durumlarda yeni nesil çalışan rolü olarak tanımlanır. Bu geçerli bir açıklamadır, ancak Azure Cloud Services özellikleriyle ölçek kümesi özelliklerini karıştırma riskini taşır. Ölçek kümeleri, platformdan/çalışma zamanından bağımsız, özelleştirilebilir ve Azure Resource Manager IaaS hizmeti ile tümleştirilen genelleştirilmiş bir işlem kaynağı sağlamaları bakımından gerçek bir "çalışan rolü" veya çalışan kaynağıdır.
  
   Cloud Services çalışan rolü, platform/çalışma zamanı desteği bakımından sınırlı olmasına karşın (yalnızca Windows platform görüntüleri), ayrıca VIP takas gibi hizmetler, yapılandırılabilir yükseltme ayarları, ölçek kümelerinde *henüz* kullanılamayan veya Service Fabric gibi diğer yüksek düzey PaaS hizmetleri tarafından sunulan çalışma zamanı/uygulama dağıtımına özgü ayarlar içerir. Ölçek kümelerini PaaS desteğine sahip bir altyapı olarak düşünebilirsiniz. [Azure Service Fabric](https://azure.microsoft.com/services/service-fabric/) gibi PaaS çözümleri bu altyapıyı kullanır.
  
   Bu yaklaşıma örnek olarak [Azure Container Service](https://azure.microsoft.com/services/container-service/), kapsayıcı düzenleyici ile ölçek kümesi tabanlı bir küme dağıtır: [https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos).

## <a name="scale-set-performance-and-scale-guidance"></a>Ölçek kümesi performansı ve ölçek kılavuzu
* Ölçek kümeleri, bir ölçek kümesinde 1000 adede kadar VM’yi destekler. Kendi özel VM görüntülerinizi oluşturur ve karşıya yüklerseniz, sınır 100’dür. Büyük ölçek kümeleri kullanma hakkında konular için bkz. [Büyük sanal makine ölçek kümeleri ile çalışma](virtual-machine-scale-sets-placement-groups.md).
* Ölçek kümelerini kullanmak için Azure depolama hesaplarını önceden oluşturmanız gerekmez. Ölçek kümeleri Azure Yönetilen Diskler’i destekleyerek depolama hesabı başına disk sayısı ile ilgili performans endişelerini giderir. Daha fazla bilgi için bkz. [Azure sanal makine ölçek kümeleri ve yönetilen diskler](virtual-machine-scale-sets-managed-disks.md).
* Daha hızlı, daha öngörülebilir VM sağlama zamanları ve gelişmiş G/Ç performansı için Standart depolama yerine Azure Premium depolama kullanmayı düşünün.
* Oluşturabileceğiniz VM sayısı, dağıtmakta olduğunuz bölgedeki çekirdek kotasıyla sınırlıdır. Şu anda Azure bulut hizmetleri ile kullanmak üzere yüksek çekirdek sınırına sahip olsanız bile, İşlem kota sınırınızı yükseltmek için Müşteri Desteği ile iletişim kurmanız gerekebilir. Kotanızı sorgulamak için şu Azure CLI komutunu: `azure vm list-usage` veya şu PowerShell komutunu: `Get-AzureRmVMUsage` çalıştırın (PowerShell 1.0’dan önceki bir sürümü kullanıyorsanız `Get-AzureVMUsage` kullanın).

## <a name="scale-set-frequently-asked-questions"></a>Ölçek kümeleri hakkında sık sorulan sorular
**S.** Bir ölçek kümesinde kaç tane VM’niz olabilir?

**C.** Ölçek kümeleri, platform görüntülerini temel alan 0 - 1.000 sanal makineye veya özel görüntüleri temel alan 0 - 100 sanal makineye sahip olabilir. 

**S.** Ölçek kümelerinde veri diskleri destekleniyor mu?

**C.** Evet. Bir ölçek kümesi, kümedeki tüm sanal makineler için geçerli olan bağlı veri sürücüsü yapılandırmasını tanımlayabilir. Daha fazla bilgi için bkz. (Azure ölçek kümeleri ve ekli veri diskleri)[virtual-machine-scale-sets-attached-disks.md]. Veri depolamayla ilgili diğer seçenekler şunlardır:

* Azure dosyaları (paylaşılan SMB sürücüleri)
* İşletim sistemi sürücüsü
* Geçici sürücü (yerel, Azure depolama tarafından yedeklenmez)
* Azure veri hizmeti (örneğin Azure tabloları, Azure blobları)
* Dış veri hizmeti (örneğin, uzak veritabanı)

**S.** Hangi Azure bölgeleri ölçek kümelerini destekler?

**C.** Tüm bölgeler ölçek kümelerini destekler.

**S.** Özel bir görüntü kullanarak nasıl ölçek kümesi oluşturulur?

**C.** Özel görüntü VHD’nizi temel alan bir Yönetilen Disk oluşturun ve ölçek kümesi şablonunuzda başvurun. Bir örnek şurada verilmiştir: [https://github.com/chagarw/MDPP/tree/master/101-vmss-custom-os](https://github.com/chagarw/MDPP/tree/master/101-vmss-custom-os).

**S.** Ölçek kümemin kapasitesini 20’den 15’e düşürürsem hangi VM’ler kaldırılır?

**C.** Sanal makineler, ölçek kümesinden yükseltme etki alanları ve hata etki alanları arasında eşit olacak şekilde kaldırılır. En yüksek kimlik numarasına sahip VM’ler ilk önce kaldırılır.

**S.** O halde kapasiteyi 15’ten 18’e yükseltirsem ne olur?

**C.** Kapasiteyi 18’e artırırsanız 3 yeni VM oluşturulur. Her defasında VM örnek kimliği önceki en yüksek değerden artırılır (örneğin 20, 21, 22). VM’ler FD ve UD’ler arasında dengelenir.

**S.** Bir ölçek kümesinde birden fazla uzantı kullanırken bir yürütme sırası uygulamayı zorunlu kılabilir miyim?

**C.** Doğrudan olmasa da, customScript uzantısı için komut dosyanız başka bir uzantının tamamlanmasını bekleyebilir ([örneğin, uzantı günlüğü izlenerek](https://github.com/Azure/azure-quickstart-templates/blob/master/201-vmss-lapstack-autoscale/install_lap.sh)). Uzantı sıralama hakkında ek yönergeler şu blog gönderisinde bulunabilir: [Azure VM Ölçek Kümelerinde Uzantı Sıralama](https://msftstack.wordpress.com/2016/05/12/extension-sequencing-in-azure-vm-scale-sets/).

**S.** Ölçek kümeleri Azure kullanılabilirlik kümeleri ile birlikte çalışır mı?

**C.** Evet. Bir ölçek kümesi, 5 FD ve 5 UD’ye sahip örtülü bir kullanılabilirlik kümesidir. 100’den fazla sanal makineden oluşan ölçek kümeleri, birden fazla kullanılabilirlik kümesine eşdeğer olan birden fazla 'yerleştirme grubuna' yayılır. Yerleştirme grupları hakkında daha fazla bilgi için bkz. [Büyük sanal makine ölçek kümeleri ile çalışma](virtual-machine-scale-sets-placement-groups.md). Bir sanal makine kullanılabilirlik kümesi, sanal makine ölçek kümesiyle aynı sanal ağda bulunabilir. Genellikle bir kullanılabilirlik kümesinde benzersiz yapılandırma gerektiren denetim düğümünü sanal makinelere, veri düğümlerini ise ölçek kümesine yerleştirmek, yaygın bir yapılandırmadır.

Ölçek kümeleri hakkında sık sorulan sorular [Azure Sanal Makine Ölçek Kümeleri hakkında SSS](virtual-machine-scale-sets-faq.md) bölümünde bulunabilir.

