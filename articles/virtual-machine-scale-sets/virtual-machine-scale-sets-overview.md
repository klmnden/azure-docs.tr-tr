---
title: "Sanal Makine Ölçek Kümelerine Genel Bakış | Microsoft Belgeleri"
description: "Sanal Makine Ölçek Kümeleri hakkında daha fazla bilgi edinin"
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
ms.date: 1/25/2017
ms.author: guybo
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 54673cbc69e418bdbe7cb5791dfb5d20e7ebcb9f
ms.openlocfilehash: 2294587fd3f978a3f8383112ece329b47307db7a
ms.lasthandoff: 02/28/2017


---
# <a name="what-are-virtual-machine-scale-sets-in-azure"></a>Azure'daki Sanal Makine Ölçek Kümeleri nedir?
Sanal makine ölçek kümeleri, özdeş VM’lerden oluşan bir sanal makine kümesini dağıtıp yönetmek için kullanabileceğiniz bir Azure İşlem kaynağıdır. Tüm VM’lerin aynı şekilde yapılandırıldığı VM ölçek kümeleri, gerçek otomatik ölçeklendirmeyi destekleyecek şekilde tasarlanmıştır (VM’lerin önceden sağlanması gerekmez) ve böylece büyük işlem, büyük veri ve kapsayıcılı iş yüklerini hedefleyen büyük ölçekli hizmetler oluşturmayı kolay hale getirir.

İşlem kaynaklarının ölçeğini artırmaya veya azaltmaya gerek duyan uygulamalar için ölçeklendirme işlemleri, arıza ve güncelleştirme etki alanlarında örtülü olarak dengelenir. VM ölçek kümelerine giriş için bkz. [Azure blog duyuruları](https://azure.microsoft.com/blog/azure-virtual-machine-scale-sets-ga/).

VM ölçek kümeleri hakkında daha fazla bilgi için şu videolara göz atın:

* [Mark Russinovich, Azure Ölçek Kümeleri hakkında konuşuyor](https://channel9.msdn.com/Blogs/Regular-IT-Guy/Mark-Russinovich-Talks-Azure-Scale-Sets/)  
* [Guy Bowerman ile Sanal Makine Ölçek Kümeleri](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-191-Virtual-Machine-Scale-Sets-with-Guy-Bowerman)

## <a name="creating-and-managing-vm-scale-sets"></a>VM ölçek kümeleri oluşturma ve yönetme
[Azure portalında](https://portal.azure.com) *yeni* öğesini seçip arama çubuğuna "ölçek" yazarak bir VM Ölçek Kümesi oluşturabilirsiniz. Sonuçlar arasında "Sanal makine ölçek kümesi" seçeneğini görürsünüz. Buradan gerekli alanları doldurarak ölçek kümenizi özelleştirip dağıtabilirsiniz. Portalda CPU kullanımına göre temel otomatik ölçeklendirme kurallarını ayarlamaya yönelik seçenekler de mevcuttur.

Sanal makine ölçek kümeleri, tıpkı tek Azure Resource Manager VM’lerinde olduğu gibi JSON şablonları ve [REST API’leri](https://msdn.microsoft.com/library/mt589023.aspx) kullanılarak da tanımlanıp dağıtılabilir. Bu nedenle, tüm standart Azure Resource Manager dağıtım yöntemleri kullanılabilir. Şablonlar hakkında daha fazla bilgi için bkz. [Azure Resource Manager şablonları yazma](../azure-resource-manager/resource-group-authoring-templates.md).

VM ölçek kümelerine ilişkin bir dizi örnek, [buradaki](https://github.com/Azure/azure-quickstart-templates) Azure Hızlı Başlangıç şablonları GitHub deposunda bulunabilir. (başlığında *vmss* olan şablonları arayın)

Bu şablonların ayrıntı sayfalarında, portal dağıtım özelliğine bağlanan bir düğme görürsünüz. VM ölçek kümesini dağıtmak için düğmeye tıklayın ve ardından portalda gerekli olan tüm parametreleri doldurun. Bir kaynağın büyük harfleri veya büyük/küçük harflerin karışımını destekleyip desteklemediğinden emin değilseniz, parametre değerlerinde küçük harf ve sayı kullanılması her zaman daha güvenlidir. Burada ayrıca bir VM ölçek kümesi şablonunun kullanışlı bir video açıklaması verilmiştir:

[VM Ölçek Kümesi Şablon Açıklaması](https://channel9.msdn.com/Blogs/Azure/VM-Scale-Set-Template-Dissection/player)

## <a name="scaling-a-vm-scale-set-out-and-in"></a>VM ölçeğini artırma veya azaltma
Bir VM ölçek kümesindeki sanal makinelerin sayısını artırmak ya da azaltmak için, *capacity* özelliğini değiştirmeniz ve şablonu yeniden dağıtmanız yeterlidir. Bu kolaylık, Azure otomatik ölçeklendirme tarafından desteklenmeyen özel ölçek olayları tanımlamak isterseniz kendi özel ölçeklendirme katmanınızı yazmayı kolay hale getirir.

Kapasiteyi değiştirmek için bir şablonu yeniden dağıtıyorsanız, yalnızca güncel kapasiteli 'SKU' özellik paketini içeren çok daha küçük bir şablon tanımlayabilirsiniz. Bunun bir örneği [burada](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) gösterilmiştir.

Otomatik olarak ölçeklendirilen bir ölçek kümesi oluşturan adımları uygulamak için bkz. [Sanal Makine Ölçek Kümesinde Makineleri Otomatik Olarak Ölçeklendirme](virtual-machine-scale-sets-windows-autoscale.md)

## <a name="monitoring-your-vm-scale-set"></a>VM ölçek kümenizi izleme
[Azure portalında](https://portal.azure.com) ölçek kümeleri listelenir ve kümedeki VM’lerin yanı sıra kaynak kullanım grafiğiyle temel özellikler ve işlemler gösterilir. Daha fazla ayrıntı için [Azure Resource Explorer](https://resources.azure.com)’ı kullanarak VM ölçek kümelerini görüntüleyebilirsiniz. VM ölçek kümeleri Microsoft.Compute altındaki bir kaynaktır; bu nedenle, bu siteden aşağıdaki bağlantıları genişleterek bunları görebilirsiniz:

**Abonelikler -> aboneliğiniz -> resourceGroups -> sağlayıcılar -> Microsoft.Compute -> virtualMachineScaleSets -> VM ölçek kümeniz -> vs.**

## <a name="vm-scale-set-scenarios"></a>VM ölçek kümesi senaryoları
Bu bölümde tipik VM ölçek kümesi senaryolarından bazıları listelenmektedir. Daha yüksek düzeydeki bazı Azure hizmetleri (Batch, Service Fabric, Azure Container Service gibi) bu senaryoları kullanır.

* **RDP / SSH’den VM ölçek kümesi örneklerine** - Bir Sanal ağ ve ölçek kümesi içinde genel IP adresleri atanmamış tek VM’ler içinde bir sanal makine ölçek kümesi oluşturulur. Genellikle işlem kılavuzunuzda durum bilgisi olmayan tüm kaynaklara ayrı genel IP adresleri ayırmanın harcama ve yönetim yüklerini istemediğinizden ve bu VM’lere, sanal ağınız içindeki yük dengeleyiciler veya tek başına sanal makineler gibi genel IP adreslerine sahip olan diğer kaynaklardan kolayca bağlanabileceğinizden, bunun yapılması yararlıdır.
* **NAT kurallarını kullanarak VM’lere bağlanma** - Bir genel IP adresi oluşturabilir, bunu bir yük dengeleyiciye bağlayabilir ve IP adresi üzerindeki bağlantı noktalarını VM ölçek kümesindeki bir VM’nin bağlantı noktasına eşleyen gelen NAT havuzu tanımlayabilirsiniz. Örneğin:
  
  | Kaynak | Kaynak Bağlantı Noktası | Hedef | Hedef Bağlantı Noktası |
  | --- | --- | --- | --- |
  |  Genel IP |Bağlantı Noktası 50000 |vmss\_0 |Bağlantı noktası 22 |
  |  Genel IP |Bağlantı noktası 50001 |vmss\_1 |Bağlantı noktası 22 |
  |  Genel IP |Bağlantı noktası 50002 |vmss\_2 |Bağlantı noktası 22 |
  
   Tek bir genel IP kullanarak bir ölçek kümesindeki her VM ile SSH bağlantısını etkinleştirmek üzere NAT kuralları kullanan bir VM ölçeği oluşturma örneği burada verilmiştir: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat)
  
   RDP ve Windows ile aynı işlemi yapmanın örneği ise burada verilmiştir: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-nat)
* **"Sıçrama kutusu" kullanarak VM’lere bağlanma** - Aynı sanal ağ için bir VM ölçeği ve tek başına VM oluşturursanız, tek başına VM ve VM ölçek kümesinin VM’leri, Sanal Ağ/Alt Ağ tarafından tanımlanan iç IP adreslerini kullanarak birbirine bağlanabilir. Genel bir IP adresi oluşturup tek başına VM’ye atarsanız, tek başına VM’ye RDP veya SSH uygulayabilir ve ardından bu makineden VM ölçek kümesi örneklerine bağlanabilirsiniz. Bu noktada, basit VM ölçek kümesinin, varsayılan yapılandırmasında genel IP adresine sahip basit bir tek başına VM’den doğal olarak daha güvenli olduğunu fark edebilirsiniz.
  
   Örneğin, bu şablon bağımsız bir VM’de basit bir ölçek kümesi dağıtır: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-jumpbox](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-jumpbox)
* **VM ölçek kümesi örneklerine yük devretme** - "Hepsini bir kez deneme" yaklaşımı kullanarak bir VM işlem kümesine çalışma iletmek istiyorsanız, uygun katman&4; yük dengeleme kurallarıyla bir Azure yük dengeleyici yapılandırabilirsiniz. Belirtilen bir protokol, aralık ve istek yolu ile bağlantı noktalarına ping göndererek, uygulamanızın çalıştığını doğrulamak için araştırmalar tanımlayabilirsiniz. Azure [Application Gateway](https://azure.microsoft.com/services/application-gateway/) ayrıca katman&7; ve daha karmaşık yük dengeleme senaryolarıyla birlikte ölçek kümelerini destekler.
  
   Bu örnekte Apache web sunucuları çalıştıran bir VM Ölçek Kümesi oluşturulur ve her VM’ye gelen yükü dengelemek için yük dengeleyici kullanılır: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-ubuntu-web-ssl](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-ubuntu-web-ssl) (Microsoft.Network/loadBalancers kaynak türüne ve virtualMachineScaleSet içindeki networkProfile ve extensionProfile öğesine bakın)
* **Bir VM ölçek kümesini PaaS küme yöneticisi içinde işlem kümesi olarak dağıtma** - VM ölçek kümeleri bazı durumlarda yeni nesil çalışan rolü olarak tanımlanır. Bu geçerli bir açıklamadır, ancak PaaS v1 Çalışan rolü özellikleriyle ölçek kümesi özelliklerini karıştırma riskini taşır. VM ölçek kümeleri, platformdan/çalışma zamanından bağımsız, özelleştirilebilir ve Azure Resource Manager IaaS hizmeti ile tümleştirilen genelleştirilmiş bir işlem kaynağı sağlamaları bakımından gerçek bir "çalışan rolü" veya çalışan kaynağı sağlar.
  
   PaaS v1 çalışan rolü, platform/çalışma zamanı desteği bakımından sınırlı olmasına karşın (yalnızca Windows platform görüntüleri), ayrıca VIP takas gibi hizmetler, yapılandırılabilir yükseltme ayarları, VM ölçek kümelerinde *henüz* kullanılamayan veya Service Fabric gibi diğer yüksek PaaS hizmetleri tarafından sunulacak olan çalışma zamanı/uygulama dağıtımına özgü ayarlar içerir. Bunu aklınızda bulundurarak, VM ölçek kümelerine PaaS destekleyen bir altyapı olarak bakabilirsiniz. Ör. Service Fabric gibi PaaS çözümleri veya Mesos gibi küme yöneticileri, VM ölçek kümelerinin üzerine ölçeklenebilir bir işlem katmanı olarak oluşturulabilir.
  
   Bu yaklaşıma örnek olarak Azure Container Service, kapsayıcı düzenleyici ile ölçek kümesi tabanlı bir küme dağıtır: [https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos).

## <a name="vm-scale-set-performance-and-scale-guidance"></a>VM ölçek kümesi performansı ve ölçek Kılavuzu
* Yönetilmeyen disklerle ölçek kümeleri kullanırken, depolama hesabı başına en fazla 20 VM planlayın (*overprovision* özelliği "false" olarak ayarlandığında bu sayı 40’a kadar çıkabilir). Yönetilen diskler kullanıldığında, depolama hesabına başına uygulanan bu sınırlar geçerli değildir.
* Yönetilmeyen diskleri olan ölçek kümeleri kullanırken, depolama hesabı adlarının ilk harflerini mümkün olduğunca yaymaya dikkat edin. [Azure Hızlı Başlangıç şablonlarındaki](https://github.com/Azure/azure-quickstart-templates/) VMSS şablonları bunun nasıl yapılacağına ilişkin örnekler vermektedir.
* Yönetilmeyen disklerle özel VM’ler kullanıyorsanız, tek bir depolama hesabında bir VM ölçek kümesi için en fazla 40 VM planlayabilirsiniz. VM ölçek kümesi dağıtımına başlayabilmeniz için görüntüyü depolama hesabına önceden kopyalamanız gerekir. Yönetilen disklerle kullanılan ölçek kümeleri, en fazla 100 adet özel görüntü sanal makinesi destekler. Daha fazla bilgi için SSS bölümüne bakın.
* Sanal ağ başına en fazla 4096 VM planlayabilirsiniz.
* Oluşturabileceğiniz VM sayısı, dağıtmakta olduğunuz bölgedeki çekirdek kotasıyla sınırlıdır. Şu anda bulut hizmetleri veya IaaS v1 ile kullanmak üzere yüksek çekirdek sınırına sahip olsanız bile, İşlem kota sınırınızı yükseltmek için Müşteri Desteği ile iletişim kurmanız gerekebilir. Kotanızı sorgulamak için şu Azure CLI komutunu: `azure vm list-usage` ve şu PowerShell komutunu: `Get-AzureRmVMUsage` çalıştırabilirsiniz (PowerShell 1.0’dan önceki bir sürümü kullanıyorsanız `Get-AzureVMUsage` kullanın).

## <a name="vm-scale-set-frequently-asked-questions"></a>VM ölçek kümeleri hakkında sık sorulan sorular
**S.** Bir VM ölçek kümesinde kaç tane VM’niz olabilir?

**C.** Yönetilen diskler kullanıldığında ölçek kümeniz, platform görüntülerini temel alan 0 - 1.000 sanal makineye veya özel görüntüleri temel alan 0 - 100 sanal makineye sahip olabilir. Yönetilmeyen disklerle kullanılan özel görüntüler tek bir depolama hesabıyla sınırlı olduğundan, kendi depolama hesaplarınızı tanımladığınız yönetilmeyen disklerde platform görüntüleri 100 VM ile, özel görüntüler ise en fazla 40 VM (*overprovision* özelliği "false" olarak ayarlanmışsa, varsayılan olarak 20) ile sınırlandırılmıştır.

**S.** VM ölçek kümelerinde Veri Diskleri destekleniyor mu?

**C.** Evet. Yönetilen depolama ile tanımlanmış bir ölçek kümesi, kümedeki tüm sanal makineler için geçerli olacak bağlı veri sürücüsü yapılandırmasını tanımlayabilir. Yönetilen depolama ile tanımlanmamış ölçek kümelerinde bağlı veri sürücüsü bulunmaz. Veri depolamayla ilgili diğer seçenekler şunlardır:

* Azure dosyaları (paylaşılan SMB sürücüleri)
* İşletim sistemi sürücüsü
* Geçici sürücü (yerel, Azure depolama tarafından yedeklenmez)
* Azure veri hizmeti (ör. Azure tabloları, Azure blob’ları)
* Dış veri hizmeti (ör. uzak DB)

**S.** Hangi Azure bölgeleri VM ölçek kümelerini destekler?

**C.** Tüm bölgeler VM ölçek kümelerini destekler.

**S.** Özel bir görüntü kullanarak nasıl VM ölçek kümesi oluşturulur?

**C.** vhdContainers özelliğini boş bırakın (veya atlayın) ve görüntü özelliğinin uri’sini belirtin. Örneğin: [201-vmss-windows-customimage].(https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-customimage)


**S.** VM ölçek kümemin kapasitesini 20’den 15’e düşürürsem hangi VM’ler kaldırılacak?

**C.** Sanal makineler, ölçek kümesinden yükseltme etki alanları ve hata etki alanları arasında eşit olacak şekilde kaldırılır. En yüksek kimlik numarasına sahip VM’ler ilk önce kaldırılır.

**S.** O halde kapasiteyi 15’ten 18’e yükseltirsem ne olur?

**C.** Kapasiteyi 18’e artırırsanız 3 yeni VM oluşturulur. Her defasında VM örnek kimliği önceki en yüksek değerden artırılır (ör. 20, 21, 22). VM’ler FD ve UD’ler arasında dengelenir.

**S.** Bir VM ölçek kümesinde birden fazla uzantı kullanırken bir yürütme sırası uygulamayı zorunlu kılabilir miyim?

**C.** Doğrudan olmasa da, customScript uzantısı için komut dosyanız başka bir uzantının tamamlanmasını bekleyebilir ([örneğin, uzantı günlüğü izlenerek](https://github.com/Azure/azure-quickstart-templates/blob/master/201-vmss-lapstack-autoscale/install_lap.sh)). Uzantı sıralama hakkında ek yönergeler şu blog gönderisinde bulunabilir: [Azure VM Ölçek Kümelerinde Uzantı Sıralama](https://msftstack.wordpress.com/2016/05/12/extension-sequencing-in-azure-vm-scale-sets/).

**S.** VM ölçek kümeleri Azure kullanılabilirlik kümeleri ile birlikte çalışır mı?

**C.** Evet. Bir VM ölçek kümesi, 5 FD ve 5 UD’ye sahip örtülü bir kullanılabilirlik kümesidir. VirtualMachineProfile altında herhangi bir yapılandırma yapılması gerekmez. 100’den fazla sanal makineden oluşan VM ölçek kümeleri, birden fazla kullanılabilirlik kümesine eşdeğer olan birden fazla 'yerleştirme grubuna' yayılır. Bir sanal makine kullanılabilirlik kümesi, sanal makine ölçek kümesiyle aynı sanal ağda bulunabilir. Genellikle kullanılabilirlik kümesinde benzersiz yapılandırma gerektiren denetim düğümünü sanal makinelere, veri düğümlerini ise ölçek kümesine yerleştirmek, yaygın bir yapılandırmadır.

