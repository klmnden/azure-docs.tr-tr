---
title: "Azure Sanal Makine Ölçek Kümelerine Genel Bakış | Microsoft Docs"
description: "Azure sanal makine ölçek kümeleri hakkında daha fazla bilgi edinin"
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
ms.date: 09/01/2017
ms.author: guybo
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5a786e9baa275e029343571bdb9a6480334f5cf3
ms.sourcegitcommit: c7215d71e1cdeab731dd923a9b6b6643cee6eb04
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/17/2017
---
# <a name="what-are-virtual-machine-scale-sets-in-azure"></a>Azure’daki sanal makine ölçek kümeleri nedir?
Sanal makine ölçek kümeleri, özdeş VM’lerden oluşan bir sanal makine kümesini dağıtıp yönetmek için kullanabileceğiniz bir Azure işlem kaynağıdır. Tüm sanal makinelerin aynı şekilde yapılandırıldığı ölçek kümeleri, gerçek otomatik ölçeklendirmeyi destekleyecek şekilde tasarlanmıştır ve sanal makinelerin önceden hazırlanması gerekmez. Bu nedenle büyük işlem, büyük veri ve kapsayıcı iş yüklerini hedefleyen büyük ölçekli hizmetler oluşturmayı kolaylaştırır.

İşlem kaynaklarının ölçeğini artırmaya veya azaltmaya gerek duyan uygulamalar için ölçeklendirme işlemleri, arıza ve güncelleştirme etki alanlarında örtülü olarak dengelenir. Ölçek kümelerine daha ayrıntılı bir giriş için bkz. [Azure blog duyuruları](https://azure.microsoft.com/blog/azure-virtual-machine-scale-sets-ga/).

Ölçek kümeleri hakkında daha fazla bilgi için şu videoları izleyin:

* [Mark Russinovich, Azure ölçek kümeleri hakkında konuşuyor](https://channel9.msdn.com/Blogs/Regular-IT-Guy/Mark-Russinovich-Talks-Azure-Scale-Sets/)  
* [Guy Bowerman ile Sanal Makine Ölçek Kümeleri](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-191-Virtual-Machine-Scale-Sets-with-Guy-Bowerman)

## <a name="creating-and-managing-scale-sets"></a>Ölçek kümeleri oluşturma ve yönetme
[Azure portalında](https://portal.azure.com) **yeni** öğesini seçip arama çubuğuna **ölçek** yazarak bir ölçek kümesi oluşturabilirsiniz. Sonuçlar arasında **Sanal makine ölçek kümesi** seçeneği listelenir. Buradan gerekli alanları doldurarak ölçek kümenizi özelleştirip dağıtabilirsiniz. Portalda CPU kullanımına göre temel otomatik ölçeklendirme kurallarını ayarlamaya yönelik seçenekler de mevcuttur. Ölçek kümenizi yönetmek için Azure portalını, [Azure PowerShell cmdlet'lerini](virtual-machine-scale-sets-windows-manage.md) veya Azure CLI 2.0'ı kullanabilirsiniz.

Ölçek kümeleri bir [kullanılabilirlik alanına](../availability-zones/az-overview.md) dağıtılabilir.

> [!NOTE]
> Şu anda sanal makine ölçek kümeleri yalnızca tek bir kullanılabilirlik alanına dağıtmayı desteklemektedir. İleride birden fazla alana dağıtım desteği eklenecektir.

Tıpkı tek Azure Resource Manager VM’lerinde olduğu gibi JSON şablonları ve [REST API’leri](https://msdn.microsoft.com/library/mt589023.aspx) kullanarak da ölçek kümeleri tanımlayıp dağıtabilirsiniz. Bu nedenle, tüm standart Azure Resource Manager dağıtım yöntemlerini kullanabilirsiniz. Şablonlar hakkında daha fazla bilgi için bkz. [Azure Resource Manager şablonları yazma](../azure-resource-manager/resource-group-authoring-templates.md).

Sanal makine ölçek kümelerine ilişkin örnek şablon kümesini [Azure Hızlı Başlangıç şablonları GitHub deposunda](https://github.com/Azure/azure-quickstart-templates) bulabilirsiniz. (Başlığında **vmss** olan şablonları arayın.)

Hızlı Başlangıç şablon örneklerinde, her bir şablona yönelik Benioku belgesinde bulunan "Azure'a dağıtın" düğmesi, portal dağıtım özelliğine bağlantı sağlar. Ölçek kümesini dağıtmak için düğmeye tıklayın ve ardından portalda gerekli olan tüm parametreleri doldurun. 


## <a name="autoscale"></a>Otomatik Ölçeklendirme
Uygulama performansının tutarlı olması için ölçek kümenizdeki VM örneği sayısını otomatik olarak artırabilir ve azaltabilirsiniz. Otomatik ölçeklendirme özelliği, zaman içinde değişen müşteri talebini izlemek ve buna göre ayarlama yapmak için gerekli yönetim maliyetini azaltır. Kuralları performans ölçümleri, uygulama yanıtı veya sabit bir plana göre tanımlarsınız ve ölçek kümeniz ihtiyaçlara göre otomatik olarak ölçeklendirilir.

Temel otomatik ölçeklendirme kuralları için CPU kullanımı veya disk G/Ç gibi konak tabanlı performans ölçümlerini kullanabilirsiniz. Bu konak tabanlı ölçümler ek aracı veya uzantı yükleyip yapılandırmaya gerek olmadan ilk günden itibaren kullanılabilir. Konak tabanlı ölçümleri kullanan otomatik ölçeklendirme kuralları aşağıdaki araçlarla oluşturulabilir:

- [Azure portal](virtual-machine-scale-sets-autoscale-portal.md)
- [Azure PowerShell](virtual-machine-scale-sets-autoscale-powershell.md)
- [Azure CLI 2.0](virtual-machine-scale-sets-autoscale-cli.md)

Daha ayrıntılı performans ölçümleri kullanmak için ölçek kümenizdeki VM örneklerine Azure tanılama uzantısını yükleyebilir ve yapılandırabilirsiniz. Azure tanılama uzantısı her VM örneğinden bellek tüketimi gibi ek performans ölçümlerini toplamanızı sağlar. Bu performans ölçümleri bir Azure depolama hesabına aktarılır ve bu verileri kullanan otomatik ölçeklendirme kuralları oluşturursunuz. Daha fazla bilgi için Azure tanılama uzantısını [Linux VM](../virtual-machines/linux/diagnostic-extension.md) veya [Windows VM](../virtual-machines/windows/ps-extensions-diagnostics.md) için etkinleştirme makalelerini inceleyebilirsiniz.

Doğrudan uygulama performansını izlemek için uygulamanıza App Insights için küçük bir izleme paketi yükleyebilir ve yapılandırabilirsiniz. Bu işlemin ardından uygulama yanıt süresi veya oturum sayısı için ayrıntılı performans ölçümleri uygulamanızdan geri aktarılabilir. Daha sonra uygulama düzeyinde performans için tanımlanan eşiklere göre otomatik ölçeklendirme kuralları oluşturabilirsiniz. App Insights hakkında daha fazla bilgi için bkz. [Application Insights nedir?](../application-insights/app-insights-overview.md).


## <a name="manually-scaling-a-scale-set-out-and-in"></a>Ölçek kümesinin ölçeğini el ile artırma veya azaltma
Azure portalında bir ölçek kümesinin kapasitesini **Ayarlar** altında **Ölçeklendirme** bölümüne tıklayarak el ile değiştirebilirsiniz. 

Ölçek kümesi kapasitesini komut satırında değiştirmek için, [Azure CLI’da](https://github.com/Azure/azure-cli) **scale** komutunu kullanın. Örneğin, bir ölçek kümesini 10 VM kapasitesine ayarlamak için şu komutu kullanın:

```bash
az vmss scale -g resourcegroupname -n scalesetname --new-capacity 10 
```

PowerShell kullanarak bir ölçek kümesindeki VM sayısını ayarlamak için, **Update-AzureRmVmss** komutunu kullanın:

```PowerShell
$vmss = Get-AzureRmVmss -ResourceGroupName resourcegroupname -VMScaleSetName scalesetname  
$vmss.Sku.Capacity = 10
Update-AzureRmVmss -ResourceGroupName resourcegroupname -Name scalesetname -VirtualMachineScaleSet $vmss
```

Azure Resource Manager şablonu kullanarak bir ölçek kümesindeki sanal makinelerin sayısını artırmak ya da azaltmak için, **capacity** özelliğini değiştirin ve şablonu yeniden dağıtın. Bu kolaylık, ölçek kümelerini Azure Otomatik Ölçeklendirme ile tümleştirmeyi veya Azure Otomatik Ölçeklendirme tarafından desteklenmeyen özel ölçek olayları tanımlamanız gerekirse kendi özel ölçeklendirme katmanınızı yazmayı kolay hale getirir. 

Kapasiteyi değiştirmek için bir Azure Resource Manager şablonunu yeniden dağıtıyorsanız, yalnızca güncel kapasiteli **SKU** özellik paketini içeren çok daha küçük bir şablon tanımlayabilirsiniz. [Bir örneği aşağıda verilmiştir](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing).


## <a name="monitoring-your-scale-set"></a>Ölçek kümenizi izleme
[Azure portalı](https://portal.azure.com) ölçek kümelerini listeler ve özelliklerini gösterir. Portal ayrıca yönetim işlemlerini destekler. Yönetim işlemlerini hem ölçek kümeleri hem de bir ölçek kümesindeki tek VM’ler üzerinde gerçekleştirebilirsiniz. Portal ayrıca özelleştirilebilir bir kaynak kullanımı grafiği sağlar. 

Bir Azure kaynağının temel aldığı JSON tanımını görmeniz veya düzenlemeniz gerekiyorsa, [Azure Kaynak Gezgini](https://resources.azure.com)’ni de kullanabilirsiniz. Ölçek kümeleri, Microsoft.Compute Azure kaynak sağlayıcısı altında bir kaynaktır. Bu siteden, aşağıdaki bağlantıları genişleterek bunları görebilirsiniz:

**Abonelikler** > **aboneliğiniz** > **resourceGroups** > **sağlayıcılar** > **Microsoft.Compute** > **virtualMachineScaleSets** > **ölçek kümeniz** > vb.

## <a name="scale-set-scenarios"></a>Ölçek kümesi senaryoları
Bu bölümde tipik ölçek kümesi senaryolarından bazıları listelenmektedir. Daha yüksek düzeydeki bazı Azure hizmetleri (Batch, Service Fabric ve Container Service gibi) bu senaryoları kullanır.

* **RDP veya SSH kullanarak ölçek kümesi örneklerine bağlanma**: Sanal ağ içinde bir ölçek kümesi oluşturulur ve ölçek kümesindeki ayrı VM'ler için genel IP adresleri varsayılan olarak ayrılmaz. Bu ilke, işlem kılavuzunuzdaki tüm düğümlere ayrı genel IP adresleri atama maliyeti ve yönetim yükünü ortadan kaldırır. Ölçek kümesi VM'lerine yönelik doğrudan harici bağlantıya ihtiyacınız varsa bir ölçek kümesini, yeni VM'lere otomatik olarak genel IP adresi atayacak şekilde yapılandırabilirsiniz. Alternatif olarak, sanal ağınızda bulunan ve genel IP adresi atanabilen diğer VM'lere (örneğin, yük dengeleyiciler ve tek başına sanal makineler) diğer kaynaklardan bağlanabilirsiniz. 
* **NAT kurallarını kullanarak VM’lere bağlanma** - Bir genel IP adresi oluşturabilir, bunu bir yük dengeleyiciye bağlayabilir ve bir gelen NAT havuzu tanımlayabilirsiniz. Bu eylemler, IP adresindeki bağlantı noktalarını ölçek kümesindeki VM’nin bir bağlantı noktasına eşler. Örneğin:
  
  | Kaynak | Kaynak bağlantı noktası | Hedef | Hedef bağlantı noktası |
  | --- | --- | --- | --- |
  |  Genel IP |Bağlantı Noktası 50000 |vmss\_0 |Bağlantı noktası 22 |
  |  Genel IP |Bağlantı noktası 50001 |vmss\_1 |Bağlantı noktası 22 |
  |  Genel IP |Bağlantı noktası 50002 |vmss\_2 |Bağlantı noktası 22 |
  
   [Bu örnekte](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat), bir ölçek kümesindeki her VM ile SSH bağlantısını etkinleştirmek üzere tek bir genel IP adresi kullanılarak NAT kuralları tanımlanır.
  
   [Bu örnek](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-nat), RDP ve Windows ile de aynı işlemi yapar.
* **"Sıçrama kutusu" kullanarak VM’lere bağlanma** - Aynı sanal ağ da bir ölçek kümesi ve tek başına VM oluşturursanız, tek başına VM ve ölçek kümesinin VM’leri, sanal ağ veya alt ağ tarafından tanımlanan iç IP adreslerini kullanarak birbirine bağlanabilir. Genel bir IP adresi oluşturup tek başına VM’ye atarsanız, tek başına VM’ye bağlanmak için RDP veya SSH kullanabilirsiniz. Daha sonra bu makineyi ölçek kümesi örneklerinize bağlayabilirsiniz. Bu noktada, basit ölçek kümesinin, varsayılan yapılandırmasında genel IP adresine sahip basit bir tek başına VM’den doğal olarak daha güvenli olduğunu fark edebilirsiniz.
  
   Örneğin, [bu şablon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-jumpbox) bir tek başına VM ile basit bir ölçek kümesini dağıtır. 
* **Ölçek kümesi örneklerine yük devretme**: Hepsini bir kez deneme yaklaşımı kullanarak bir VM işlem kümesine çalışma iletmek istiyorsanız, uygun katman 4 yük dengeleme kurallarıyla bir Azure yük dengeleyici yapılandırabilirsiniz. Belirtilen bir protokol, aralık ve istek yolu ile bağlantı noktalarına ping göndererek, uygulamanızın çalıştığını doğrulamak için araştırmalar tanımlayabilirsiniz. [Azure Application Gateway](https://azure.microsoft.com/services/application-gateway/) ayrıca katman 7 ve daha karmaşık yük dengeleme senaryolarıyla birlikte ölçek kümelerini destekler.
  
   [Bu örnek](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-ubuntu-web-ssl), Apache web sunucuları çalıştıran bir ölçek kümesi oluşturur ve her VM’nin aldığı yükü dengelemek için bir yük dengeleyici kullanır. (Microsoft.Network/loadBalancers kaynak türüne ve virtualMachineScaleSet içindeki networkProfile ve extensionProfile öğesine bakın.)

   [Bu Linux örneği](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-ubuntu-app-gateway) ile [bu Windows örneği](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-app-gateway), Application Gateway kullanır.  

* **Bir ölçek kümesini PaaS küme yöneticisi içinde işlem kümesi olarak dağıtma**: Ölçek kümeleri bazı durumlarda yeni nesil çalışan rolü olarak tanımlanır. Bu geçerli bir açıklamadır, ancak Azure Cloud Services özellikleriyle ölçek kümesi özelliklerini karıştırma riskini taşır. Bir anlamda, ölçek kümeleri gerçek bir çalışan rolü veya çalışan kaynağı sağlar. Bunlar platform/çalışma zamanından bağımsız, özelleştirilebilir ve Azure Resource Manager IaaS ile tümleşen genelleştirilmiş işlem kaynaklarıdır.
  
   Bir Cloud Services çalışan rolü, platform/çalışma zamanı desteği bakımından sınırlıdır (yalnızca Windows platform görüntüleri). Ancak, aynı zamanda VIP değiştirme, yapılandırılabilir yükseltme ayarları ve çalışma zamanı/uygulama dağıtımına özel ayarlar gibi hizmetleri içerir. Bu hizmetler ölçek kümelerinde *henüz* kullanılamamaktadır veya Azure Service Fabric gibi diğer üst düzey PaaS hizmetleri tarafından sunulmaktadır. Ölçek kümelerini PaaS desteğine sahip bir altyapı olarak düşünebilirsiniz. [Service Fabric](https://azure.microsoft.com/services/service-fabric/) gibi PaaS çözümleri bu altyapıyı kullanır.
  
   Bu yaklaşımın [bu örneğinde](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos) [Azure Container Service](https://azure.microsoft.com/services/container-service/), bir kapsayıcı düzenleyicisi ile ölçek kümelerini temel alan bir küme dağıtır.

## <a name="scale-set-performance-and-scale-guidance"></a>Ölçek kümesi performansı ve ölçek kılavuzu
* Bir ölçek kümesi en çok 1.000 VM’yi destekler. Kendi özel VM görüntülerinizi oluşturur ve karşıya yüklerseniz, sınır 300’dür. Büyük ölçek kümeleri kullanma hakkında konular için bkz. [Büyük sanal makine ölçek kümeleri ile çalışma](virtual-machine-scale-sets-placement-groups.md).
* Ölçek kümelerini kullanmak için Azure depolama hesaplarını önceden oluşturmanız gerekmez. Ölçek kümeleri Azure yönetilen diskleri destekleyerek depolama hesabı başına disk sayısı ile ilgili performans endişelerini giderir. Daha fazla bilgi için bkz. [Azure sanal makine ölçek kümeleri ve yönetilen diskler](virtual-machine-scale-sets-managed-disks.md).
* Daha hızlı, daha öngörülebilir VM sağlama zamanları ve gelişmiş G/Ç performansı için Azure Depolama yerine Azure Premium Depolama kullanmayı düşünün.
* Dağıtım yaptığınız bölgedeki vCPU kotası, oluşturabileceğiniz VM sayısını sınırlar. Şu anda Azure Cloud Services ile kullanmak üzere yüksek vCPU sınırına sahip olsanız bile, işlem kota sınırınızı yükseltmek için Müşteri Desteği ile iletişim kurmanız gerekebilir. Kotanızı sorgulamak için bu Azure CLI komutunu çalıştırın: `azure vm list-usage`. Veya bu PowerShell komutunu çalıştırın: `Get-AzureRmVMUsage`.

## <a name="frequently-asked-questions-for-scale-sets"></a>Ölçek kümeleri için sık sorulan sorular
**S.** Bir ölçek kümesinde kaç tane sanal makinem olabilir?

**C.** Ölçek kümeleri, platform görüntülerini temel alan 0 ila 1.000 VM’ye veya özel görüntüleri temel alan 0 ila 300 VM’ye sahip olabilir. 

**S.** Ölçek kümelerinde veri diskleri destekleniyor mu?

**C.** Evet. Bir ölçek kümesi, kümedeki tüm sanal makineler için geçerli olan bağlı veri diski yapılandırmasını tanımlayabilir. Daha fazla bilgi için bkz. [Azure ölçek kümeleri ve bağlı veri diskleri](virtual-machine-scale-sets-attached-disks.md). Veri depolamayla ilgili diğer seçenekler şunlardır:

* Azure dosyaları (paylaşılan SMB sürücüleri)
* İşletim sistemi sürücüsü
* Geçici sürücü (yerel, Azure Depolama tarafından yedeklenmez)
* Azure veri hizmeti (örneğin Azure tabloları, Azure blobları)
* Dış veri hizmeti (örneğin, uzak veritabanı)

**S.** Hangi Azure bölgeleri ölçek kümelerini destekler?

**C.** Tüm bölgeler ölçek kümelerini destekler.

**S.** Özel bir görüntü kullanarak nasıl ölçek kümesi oluşturabilirim?

**C.** Özel görüntü VHD’nizi temel alan bir yönetilen disk oluşturun ve ölçek kümesi şablonunuzda başvurun. [Bir örneği aşağıda verilmiştir](https://github.com/chagarw/MDPP/tree/master/101-vmss-custom-os).

**S.** Ölçek kümemin kapasitesini 20’den 15’e düşürürsem hangi VM’ler kaldırılır?

**C.** Sanal makineler, ölçek kümesinden güncelleştirme etki alanları ve hata etki alanları arasında eşit olacak şekilde kaldırılır. En yüksek kimlik numarasına sahip VM’ler ilk önce kaldırılır.

**S.** Kapasiteyi 15’ten 18’e yükseltirsem ne olur?

**C.** Kapasiteyi 18’e artırırsanız 3 yeni VM oluşturulur. Her defasında VM örnek kimliği önceki en yüksek değerden artırılır (örneğin 20, 21, 22). VM’ler hata etki alanlarında ve güncelleştirme etki alanlarında dengelenir.

**S.** Bir ölçek kümesinde birden fazla uzantı kullanırken bir yürütme sırası uygulamayı zorunlu kılabilir miyim?

**C.** Doğrudan olmasa da customScript uzantısı için betiğiniz başka bir uzantının tamamlanmasını bekleyebilir. Uzantı sıralama hakkında ek yönergeleri şu blog gönderisinde bulabilirsiniz: [Azure VM Ölçek Kümelerinde Uzantı Sıralama](https://msftstack.wordpress.com/2016/05/12/extension-sequencing-in-azure-vm-scale-sets/).

**S.** Ölçek kümeleri Azure kullanılabilirlik kümeleri ile birlikte çalışır mı?

**C.** Evet. Bir ölçek kümesi, 5 hata etki alanı ve 5 güncelleştirme etki alanına sahip örtülü bir kullanılabilirlik kümesidir. 100’den fazla sanal makineden oluşan ölçek kümeleri, birden fazla kullanılabilirlik kümesine eşdeğer olan birden fazla *yerleştirme grubuna* yayılır. Yerleştirme grupları hakkında daha fazla bilgi için bkz. [Büyük sanal makine ölçek kümeleri ile çalışma](virtual-machine-scale-sets-placement-groups.md). Bir sanal makine kullanılabilirlik kümesi, sanal makine ölçek kümesiyle aynı sanal ağda bulunabilir. Genellikle bir kullanılabilirlik kümesinde benzersiz yapılandırma gerektiren denetim düğümünü sanal makinelere, veri düğümlerini ise ölçek kümesine yerleştirmek, yaygın bir yapılandırmadır.

Ölçek kümeleriyle ilgili soruların diğer yanıtlarını [Azure sanal makine ölçek kümeleri hakkında SSS](virtual-machine-scale-sets-faq.md) bölümünde bulabilirsiniz.
