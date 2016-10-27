<properties
    pageTitle="Sanal Makine Ölçek Kümelerine Genel Bakış | Microsoft Azure"
    description="Sanal Makine Ölçek Kümeleri hakkında daha fazla bilgi edinin"
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gbowerman"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/13/2016"
    ms.author="guybo"/>


# <a name="virtual-machine-scale-sets-overview"></a>Sanal Makine Ölçek Kümelerine Genel Bakış

Sanal makine ölçek kümeleri, özdeş VM’lerden oluşan bir sanal makine kümesini dağıtıp yönetmek için kullanabileceğiniz bir Azure İşlem kaynağıdır. Tüm VM’lerin aynı şekilde yapılandırıldığı VM ölçek kümeleri, gerçek otomatik ölçeklendirmeyi destekleyecek şekilde tasarlanmıştır (VM’lerin önceden sağlanması gerekmez) ve böylece büyük işlem, büyük veri ve kapsayıcılı iş yüklerini hedefleyen büyük ölçekli hizmetler oluşturmayı kolay hale getirir.

İşlem kaynaklarının ölçeğini artırmaya veya azaltmaya gerek duyan uygulamalar için ölçeklendirme işlemleri, arıza ve güncelleştirme etki alanlarında örtülü olarak dengelenir. VM ölçek kümelerine giriş için bkz. [Azure blog duyuruları](https://azure.microsoft.com/blog/azure-virtual-machine-scale-sets-ga/).

VM ölçek kümeleri hakkında daha fazla bilgi için şu videolara göz atın:

 - [Mark Russinovich, Azure Ölçek Kümeleri hakkında konuşuyor](https://channel9.msdn.com/Blogs/Regular-IT-Guy/Mark-Russinovich-Talks-Azure-Scale-Sets/)  

 - [Guy Bowerman ile Sanal Makine Ölçek Kümeleri](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-191-Virtual-Machine-Scale-Sets-with-Guy-Bowerman)

## <a name="creating-and-managing-vm-scale-sets"></a>VM ölçek kümeleri oluşturma ve yönetme

[Azure portalında](https://portal.azure.com) _yeni_ öğesini seçip arama çubuğuna "ölçek" yazarak bir VM Ölçek Kümesi oluşturabilirsiniz. Sonuçlar arasında "Sanal makine ölçek kümesi" seçeneğini görürsünüz. Buradan gerekli alanları doldurarak ölçek kümenizi özelleştirip dağıtabilirsiniz. 

Sanal makine ölçek kümeleri, tıpkı tek Azure Resource Manager VM’lerinde olduğu gibi JSON şablonları ve [REST API’leri](https://msdn.microsoft.com/library/mt589023.aspx) kullanılarak da tanımlanıp dağıtılabilir. Bu nedenle, tüm standart Azure Resource Manager dağıtım yöntemleri kullanılabilir. Şablonlar hakkında daha fazla bilgi için bkz. [Azure Resource Manager şablonları yazma](../resource-group-authoring-templates.md).

VM ölçek kümelerine ilişkin bir dizi örnek, [buradaki](https://github.com/Azure/azure-quickstart-templates) Azure Hızlı Başlangıç şablonları GitHub deposunda bulunabilir. (başlığında _vmss_ olan şablonları arayın)

Bu şablonların ayrıntı sayfalarında, portal dağıtım özelliğine bağlanan bir düğme görürsünüz. VM ölçek kümesini dağıtmak için düğmeye tıklayın ve ardından portalda gerekli olan tüm parametreleri doldurun. Bir kaynağın büyük harfleri veya büyük/küçük harflerin karışımını desteklediğinden emin değilseniz, her zaman küçük harfli parametrelerin kullanılması daha güvenlidir. Burada ayrıca bir VM ölçek kümesi şablonunun kullanışlı bir video açıklaması verilmiştir:

[VM Ölçek Kümesi Şablon Açıklaması](https://channel9.msdn.com/Blogs/Windows-Azure/VM-Scale-Set-Template-Dissection/player)

## <a name="scaling-a-vm-scale-set-out-and-in"></a>VM ölçeğini artırma veya azaltma

Bir VM ölçek kümesindeki sanal makinelerin sayısını artırmak ya da azaltmak için, _capacity_ özelliğini değiştirmeniz ve şablonu yeniden dağıtmanız yeterlidir. Bu kolaylık, Azure otomatik ölçeklendirme tarafından desteklenmeyen özel ölçek olayları tanımlamak isterseniz kendi özel ölçeklendirme katmanınızı yazmayı kolay hale getirir.

Kapasiteyi değiştirmek için bir şablonu yeniden dağıtıyorsanız, yalnızca SKU ve güncelleştirilmiş kapasiteyi içeren çok daha küçük bir şablon tanımlayabilirsiniz. Bunun bir örneği [burada](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) gösterilmiştir.

Otomatik olarak ölçeklendirilen bir ölçek kümesi oluşturan adımları uygulamak için bkz. [Sanal Makine Ölçek Kümesinde Makineleri Otomatik Olarak Ölçeklendirme](virtual-machine-scale-sets-windows-autoscale.md)

## <a name="monitoring-your-vm-scale-set"></a>VM ölçek kümenizi izleme

[Azure portalı](https://portal.azure.com), ölçek kümelerini listeleyip temel özellikleri göstermesinin yanında küme içindeki VM’leri listeler. Daha fazla ayrıntı için [Azure Resource Explorer](https://resources.azure.com)’ı kullanarak VM ölçek kümelerini görüntüleyebilirsiniz. VM ölçek kümeleri Microsoft.Compute altındaki bir kaynaktır; bu nedenle, bu siteden aşağıdaki bağlantıları genişleterek bunları görebilirsiniz:

    subscriptions -> your subscription -> resourceGroups -> providers -> Microsoft.Compute -> virtualMachineScaleSets -> your VM scale set -> etc.

## <a name="vm-scale-set-scenarios"></a>VM ölçek kümesi senaryoları

Bu bölümde tipik VM ölçek kümesi senaryolarından bazıları listelenmektedir. Daha yüksek düzeydeki bazı Azure hizmetleri (Batch, Service Fabric, Azure Container Service gibi) bu senaryoları kullanır.

 - **RDP / SSH’den VM ölçek kümesi örneklerine** - Bir Sanal ağ ve ölçek kümesi içinde genel IP adresleri atanmamış tek VM’ler içinde bir sanal makine ölçek kümesi oluşturulur. Genellikle işlem kılavuzunuzda durum bilgisi olmayan tüm kaynaklara ayrı genel IP adresleri ayırmanın harcama ve yönetim yüklerini istemediğinizden ve bu VM’lere, sanal ağınız içindeki yük dengeleyiciler veya tek başına sanal makineler gibi genel IP adreslerine sahip olan diğer kaynaklardan kolayca bağlanabileceğinizden, bunun yapılması yararlıdır.

 - **NAT kurallarını kullanarak VM’lere bağlanma** - Bir genel IP adresi oluşturabilir, bunu bir yük dengeleyiciye bağlayabilir ve IP adresi üzerindeki bir bağlantı noktasını VM ölçek kümesindeki bir VM’nin bağlantı noktasına eşleyen gelen NAT kuralları tanımlayabilirsiniz. Örneğin:
 
    Kaynak | Kaynak Bağlantı Noktası | Hedef | Hedef Bağlantı Noktası
    --- | --- | --- | ---
    Genel IP | Bağlantı Noktası 50000 | vmss\_0 | Bağlantı noktası 22
    Genel IP | Bağlantı noktası 50001 | vmss\_1 | Bağlantı noktası 22
    Genel IP | Bağlantı noktası 50002 | vmss\_2 | Bağlantı noktası 22

    Tek bir genel IP kullanarak bir ölçek kümesindeki her VM ile SSH bağlantısını etkinleştirmek üzere NAT kuralları kullanan bir VM ölçeği oluşturma örneği burada verilmiştir: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat)

    RDP ve Windows ile aynı işlemi yapmanın örneği ise burada verilmiştir: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-nat)

 - **"Sıçrama kutusu" kullanarak VM’lere bağlanma** - Aynı sanal ağ için bir VM ölçeği ve tek başına VM oluşturursanız, tek başına VM ve VM ölçek kümesinin VM’leri, Sanal Ağ/Alt Ağ tarafından tanımlanan iç IP adreslerini kullanarak birbirine bağlanabilir. Genel bir IP adresi oluşturup tek başına VM’ye atarsanız, tek başına VM’ye RDP veya SSH uygulayabilir ve ardından bu makineden VM ölçek kümesi örneklerine bağlanabilirsiniz. Bu noktada, basit VM ölçek kümesinin, varsayılan yapılandırmasında genel IP adresine sahip basit bir tek başına VM’den doğal olarak daha güvenli olduğunu fark edebilirsiniz.

    [Bu yaklaşımın bir örneği olarak, bu şablon bir VM ölçek kümesini temel alan VM kümesini yöneten tek başına bir Ana VM’nin oluşturduğu basit bir Mesos kümesi oluşturur.](https://github.com/gbowerman/azure-myriad/blob/master/mesos-vmss-simple-cluster.json)

 - **VM ölçek kümesi örneklerine yük devretme** - "Hepsini bir kez deneyen" bir yaklaşım kullanarak bir VM işlem kümesine çalışma iletmek istiyorsanız, bir Azure Load Balancer’ı uygun yük dengeleme kurallarıyla yapılandırabilirsiniz. Belirtilen bir protokol, aralık ve istek yolu ile bağlantı noktalarına ping göndererek, uygulamanızın çalıştığını doğrulamak için araştırmalar tanımlayabilirsiniz. Azure [Application Gateway](https://azure.microsoft.com/services/application-gateway/) ayrıca daha karmaşık yük dengeleme senaryolarıyla birlikte ölçek kümelerini destekler.

    [IIS web sunucusu çalıştıran bir VM ölçek kümesinin oluşturulduğu ve her VM’nin aldığı yükü dengelemek üzere bir yük dengeleyicinin kullanıldığı örnek aşağıda verilmiştir. Bu örnekte ayrıca her VM’de belirli bir URL’ye ping göndermek için HTTP protokolü kullanılır.](https://github.com/gbowerman/azure-myriad/blob/master/vmss-win-iis-vnet-storage-lb.json) (Microsoft.Network/loadBalancers kaynak türüne ve virtualMachineScaleSet içindeki networkProfile ve extensionProfile öğesine bakın)

 - **Bir VM ölçek kümesini PaaS küme yöneticisi içinde işlem kümesi olarak dağıtma** - VM ölçek kümeleri bazı durumlarda yeni nesil çalışan rolü olarak tanımlanır. Bu geçerli bir açıklamadır, ancak aynı zamanda PaaS v1 Çalışan rolü özellikleriyle ölçek kümesi özelliklerini karıştırma riskini taşır. VM ölçek kümeleri, platformdan/çalışma zamanından bağımsız, özelleştirilebilir ve Azure Resource Manager IaaS hizmeti ile tümleştirilen genelleştirilmiş bir işlem kaynağı sağlamaları bakımından gerçek bir "çalışan rolü" veya çalışan kaynağı sağlar.

    PaaS v1 çalışan rolü, platform/çalışma zamanı desteği bakımından sınırlı olmasına karşın (yalnızca Windows platform görüntüleri), ayrıca VIP takas gibi hizmetler, yapılandırılabilir yükseltme ayarları, VM ölçek kümelerinde _henüz_ kullanılamayan veya Service Fabric gibi diğer yüksek PaaS hizmetleri tarafından sunulacak olan çalışma zamanı/uygulama dağıtımına özgü ayarlar içerir. Bunu aklınızda bulundurarak, VM ölçek kümelerine PaaS destekleyen bir altyapı olarak bakabilirsiniz. Ör. Service Fabric gibi PaaS çözümleri veya Mesos gibi küme yöneticileri, VM ölçek kümelerinin üzerine ölçeklenebilir bir işlem katmanı olarak oluşturulabilir.

    [Bu yaklaşımın bir örneği olarak, bu şablon bir VM ölçek kümesini temel alan VM kümesini yöneten tek başına bir Ana VM’nin oluşturduğu basit bir Mesos kümesi oluşturur.](https://github.com/gbowerman/azure-myriad/blob/master/mesos-vmss-simple-cluster.json) [Azure Container Service](https://azure.microsoft.com/blog/azure-container-service-now-and-the-future/)’in gelecekteki sürümleri, bu senaryonun VM ölçek kümelerini temel alan daha karmaşık/zorlaştırılmış sürümlerini dağıtacaktır.

## <a name="vm-scale-set-performance-and-scale-guidance"></a>VM ölçek kümesi performansı ve ölçek Kılavuzu

- Aynı anda birden fazla VM Ölçek Kümesinde en fazla 500 VM oluşturabilirsiniz.
- Depolama hesabı başına 20 VM’den fazlasını planlamayın (_overprovision_ özelliği "false" olarak ayarlanırsa 40’a kadar gidilebilir).
- Depolama hesabı adlarının ilk harflerini mümkün olduğunca yaymaya dikkat edin.  [Azure Hızlı Başlangıç şablonlarındaki](https://github.com/Azure/azure-quickstart-templates/) VMSS şablonları bunun nasıl yapılacağına ilişkin örnekler vermektedir.
- Özel VM’ler kullanıyorsanız bir VM ölçek kümesi için tek bir depolama hesabında en fazla 40 VM planlayabilirsiniz.  VM ölçek kümesi dağıtımına başlayabilmeniz için görüntüyü depolama hesabına önceden kopyalamanız gerekir. Daha fazla bilgi için SSS bölümüne bakın.
- Sanal ağ başına en fazla 4096 VM planlayabilirsiniz.
- Oluşturabileceğiniz VM sayısı, dağıtmakta olduğunuz bölgedeki çekirdek kotasıyla sınırlıdır. Şu anda bulut hizmetleri veya IaaS v1 ile kullanmak üzere yüksek çekirdek sınırına sahip olsanız bile, İşlem kota sınırınızı yükseltmek için Müşteri Desteği ile iletişim kurmanız gerekebilir. Kotanızı sorgulamak için şu Azure CLI komutunu: `azure vm list-usage` ve şu PowerShell komutunu: `Get-AzureRmVMUsage` çalıştırabilirsiniz (PowerShell 1.0’dan önceki bir sürümü kullanıyorsanız `Get-AzureVMUsage` kullanın).

## <a name="vm-scale-set-frequently-asked-questions"></a>VM ölçek kümeleri hakkında sık sorulan sorular

**S.** Bir VM ölçek kümesinde kaç tane VM’niz olabilir?

**C.** birden fazla depolama hesabı arasında dağıtılabilir platform görüntüleri kullanıyorsanız, 100. Özel görüntüler kullanıyorsanız, özel görüntüler şu anda tek bir depolama hesabıyla sınırlı olduğundan en fazla 40 sanal makineniz olabilir (_overprovision_ özelliği "false" olarak ayarlanırsa, varsayılan değer 20’dir).

**S** VM ölçek kümeleri için başka hangi kaynak limitleri mevcuttur?

**C.** 10 dakikalık bir süre boyunca her bölge için birden fazla ölçek kümesinde en fazla 500 VM oluşturabilirsiniz. Var olan [Azure Abonelik Hizmeti Limitleri/](../azure-subscription-service-limits.md) geçerlidir.

**S.** VM ölçek kümelerinde Veri Diskleri destekleniyor mu?

**C.** İlk sürümde desteklenmez. Veri depolama seçenekleriniz şunlardır:

- Azure dosyaları (paylaşılan SMB sürücüleri)

- İşletim sistemi sürücüsü

- Geçici sürücü (yerel, Azure depolama tarafından yedeklenmez)

- Azure veri hizmeti (ör. Azure tabloları, Azure blob’ları)

- Dış veri hizmeti (ör. uzak DB)

**S.** Hangi Azure bölgeleri VM ölçek kümelerini destekler?

**C.** Azure Resource Manager’ı destekleyen tüm bölgeler VM Ölçek Kümelerini de destekler.

**S.** Özel bir görüntü kullanarak nasıl VM ölçek kümesi oluşturulur?

**C.** vhdContainers özelliğini boş bırakın, örneğin:

    "storageProfile": {
        "osDisk": {
            "name": "vmssosdisk",
            "caching": "ReadOnly",
            "createOption": "FromImage",
            "image": {
                "uri": "https://mycustomimage.blob.core.windows.net/system/Microsoft.Compute/Images/mytemplates/template-osDisk.vhd"
            },
            "osType": "Windows"
        }
    },


**S.** VM ölçek kümemin kapasitesini 20’den 15’e düşürürsem hangi VM’ler kaldırılacak?

**C.** Sanal makineler, ölçek kümesinden yükseltme etki alanları ve hata etki alanları arasında eşit olacak şekilde kaldırılır. En yüksek kimlik numarasına sahip VM’ler ilk önce kaldırılır.

**S.** O halde kapasiteyi 15’ten 18’e yükseltirsem ne olur?

**C.** Kapasiteyi 18’e artırırsanız 3 yeni VM oluşturulur. Her defasında VM örnek kimliği önceki en yüksek değerden artırılır (ör. 20, 21, 22). VM’ler FD ve UD’ler arasında dengelenir.

**S.** Bir VM ölçek kümesinde birden fazla uzantı kullanırken bir yürütme sırası uygulamayı zorunlu kılabilir miyim?

**C.** Doğrudan olmasa da, customScript uzantısı için komut dosyanız başka bir uzantının tamamlanmasını bekleyebilir ([örneğin, uzantı günlüğü izlenerek](https://github.com/Azure/azure-quickstart-templates/blob/master/201-vmss-lapstack-autoscale/install_lap.sh)). Uzantı sıralama hakkında ek yönergeler şu blog gönderisinde bulunabilir: [Azure VM Ölçek Kümelerinde Uzantı Sıralama](https://msftstack.wordpress.com/2016/05/12/extension-sequencing-in-azure-vm-scale-sets/).

**S.** VM ölçek kümeleri Azure kullanılabilirlik kümeleri ile birlikte çalışır mı?

**C.** Evet. Bir VM ölçek kümesi, 5 FD ve 5 UD’ye sahip örtülü bir kullanılabilirlik kümesidir. VirtualMachineProfile altında herhangi bir yapılandırma yapılması gerekmez. Gelecekteki sürümlerde, VM ölçek kümeleri birden fazla kiracıya yayılabilir, ancak şimdilik bir ölçek kümesi tek bir kullanılabilirlik kümesidir.



<!--HONumber=Oct16_HO3-->


