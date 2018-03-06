---
title: "Azure Service Fabric tek başına küme dağıtım hazırlama | Microsoft Docs"
description: "Ortamı hazırlama ve üretim iş yükü işleme için amacını küme dağıtmadan önce kabul edilmesi için küme yapılandırması oluşturma ile ilgili belgeler."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 9/12/2017
ms.author: dekapur;maburlik;chackdan
ms.openlocfilehash: b1190ec5a3ff70a368b29465699f9082d2b989bf
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2017
---
<a id="preparemachines"></a>

# <a name="plan-and-prepare-your-service-fabric-standalone-cluster-deployment"></a>Planlama ve Service Fabric tek başına küme dağıtımınızı hazırlama
Kümenizi oluşturmadan önce aşağıdaki adımları gerçekleştirin.

## <a name="plan-your-cluster-infrastructure"></a>Küme altyapınızı planlama
Ne tür hataları varlığını sürdürmesi için küme istediğiniz karar vermem "sahip", makinelerde Service Fabric kümesi oluşturmak üzere olduğunuz. Örneğin, güç satırları ya da bu makinelere sağlanan Internet bağlantıları ayrı? Ayrıca, bu makinelerin fiziksel güvenlik göz önünde bulundurun. Makineler bulunduğu ve onlara erişimi gerek duyan? Bu kararlar yaptıktan sonra mantıksal olarak makineleri (sonraki adıma bakın) çeşitli hata etki alanları için eşleyebilirsiniz. Altyapı üretim kümeleri için planlama test kümeleri için daha daha karmaşıktır.

## <a name="determine-the-number-of-fault-domains-and-upgrade-domains"></a>Hata etki alanlarının sayısını belirlemek ve etki alanlarını yükseltme
A [ *hata etki alanı* (FD)](service-fabric-cluster-resource-manager-cluster-description.md) hatası fiziksel birimidir ve veri merkezlerinde fiziksel altyapı doğrudan ilişkilidir. Hata etki alanı tek hata noktası paylaşan donanım bileşenleri (bilgisayarlar, anahtarları, ağlar ve daha fazla) oluşur. Hata etki alanları ve raflarının arasında 1:1 eşleme olsa da, geniş konuşarak, her bir rafa hata etki alanı kabul edilebilir.

ClusterConfig.json içinde FDs belirttiğinizde, her FD adını seçebilirsiniz. Service Fabric hiyerarşik FDs destekler, bu nedenle altyapı topolojinizi bunlara yansıtabilirsiniz.  Örneğin, aşağıdaki FDs geçerlidir:

* "faultDomain": "fd: / raf1/Room1/MAKİNE1"
* "faultDomain": "fd: / FD1"
* "faultDomain": "fd: / Room1/raf1/PDU1/M1"

Bir *yükseltme etki alanı* (UD) olan bir mantıksal birim düğümlerinin. Service Fabric bağımsızlıklar yükseltmeleri sırasında (Uygulama yükseltme veya Küme yükseltme), bir UD tüm düğümlerde diğer UDs düğümler isteklere yanıt kullanılabilir kaldığı sürece yükseltmeyi gerçekleştirmek için alınır. Bunları bir yapmanız gereken şekilde makinelerinizi üzerinde gerçekleştirdiğiniz bellenim yükseltmeleri UDs, getirmiyor birer birer makine.

Bu kavramları hakkında düşünmek için basit FDs planlanmamış hatası ve UDs birimi planlı bakım birim olarak olarak dikkate alınması gereken yoldur.

ClusterConfig.json içinde UDs belirttiğinizde, her UD adını seçebilirsiniz. Örneğin, aşağıdaki adları geçerlidir:

* "upgradeDomain": "UD0"
* "upgradeDomain": "UD1A"
* "upgradeDomain": "DomainRed"
* "upgradeDomain": "Mavi"

FDs ve UDs hakkında daha ayrıntılı bilgi için bkz: [Service Fabric kümesi açıklayan](service-fabric-cluster-resource-manager-cluster-description.md).

Üretim bir kümede bir üretim ortamında desteklenmesi için en az üç FDs yayılacağı Bakım ve yönetim düğümü üzerinde tam denetime sahiptir, yani, güncelleştirme ve makineler değiştirme sorumludur. (Yani, Amazon Web Hizmetleri VM örnekleri) ortamlarında, makineler üzerinde tam denetime sahip olduğu değil çalıştıran kümeler için en az beş FDs kümenizdeki olmalıdır. Her FD bir veya daha fazla düğüm olabilir. Bu makine yükseltmeleri ve kendi zamanlama bağlı olarak, uygulamaların ve hizmetlerin kümelerinde çalışan intefere olabilir, güncelleştirmeleri nedeniyle oluşan sorunları önlemek için yapılır.

## <a name="determine-the-initial-cluster-size"></a>İlk küme boyutu belirleme

Genellikle, kümenizdeki düğümlerin sayısını iş gereksinimlerinize bağlı olarak, yani, kaç tane Hizmetleri ve kapsayıcıları küme üzerinde çalışacağı ve iş yüklerinizi sürdürebilmek gereken kaç kaynak belirlenir. Üretim kümeleri için en az 5 düğümleri, kümede sahip 5 FDs kapsayıcı öneririz. Ancak, düğümleriniz üzerinde tam denetime sahiptir ve üç FDs yayılabilir yukarıda açıklandığı gibi ardından üç düğüm ayrıca iş yapmanız gerekir.

Test kümelerindeki yalnızca yalnızca durum bilgisiz iş yükleri çalıştıran tek bir düğüm gerekiyor ancak test kümelerindeki durum bilgisi olan iş yükleri çalıştıran üç düğümü olmalıdır. Bu, geliştirme amaçlı birden fazla düğüm verilen makinede sağlayabilirsiniz de unutulmamalıdır. Bir üretim ortamında, ancak yalnızca bir düğüm fiziksel veya sanal makine başına Service Fabric destekler.

## <a name="prepare-the-machines-that-will-serve-as-nodes"></a>Düğümleri olarak hizmet verecektir makineler hazırlama

Kümeye eklemek istediğiniz her makine için önerilen bazı özellikleri şunlardır:

* En az 16 GB RAM
* 40 GB kullanılabilir disk alanı en az
* 4 çekirdek ya da daha fazla CPU
* Güvenli bir ağ veya tüm makineler için ağ bağlantısı
* Windows Server 2012 R2 veya Windows Server 2016
* [.NET framework 4.5.1 veya üzeri](https://www.microsoft.com/download/details.aspx?id=40773), tam yükleme
* [Windows PowerShell 3.0](https://msdn.microsoft.com/powershell/scripting/setup/installing-windows-powershell)
* [RemoteRegistry hizmeti](https://technet.microsoft.com/library/cc754820) tüm makinelerde çalışıyor olması gerekir

Dağıtma ve küme yapılandırma Küme Yöneticisi olmalıdır [yönetici ayrıcalıkları](https://social.technet.microsoft.com/wiki/contents/articles/13436.windows-server-2012-how-to-add-an-account-to-a-local-administrator-group.aspx) makinelerinin her biri. Service Fabric’i bir etki alanı denetleyicisine yükleyemezsiniz.

## <a name="download-the-service-fabric-standalone-package-for-windows-server"></a>Windows Server için Service Fabric tek başına paketini indirin
[Bağlantı - Service Fabric tek başına paketi - Windows Server karşıdan](http://go.microsoft.com/fwlink/?LinkId=730690) ve kümenin parçası olmayan bir dağıtım makinesi, veya kümeniz parçası olacak makinelerden biri paketin sıkıştırmasını açın.

## <a name="modify-cluster-configuration"></a>Küme yapılandırmasını Değiştir
Tek başına küme oluşturmak için küme belirtimi tanımlayan bir tek başına küme yapılandırma ClusterConfig.json dosyası oluşturmanız gerekir. Yapılandırma dosyasının bulunduğu şablonları hakkında temel aşağıdaki bağlantıdaki. <br>
[Tek başına küme yapılandırmaları](https://github.com/Azure-Samples/service-fabric-dotnet-standalone-cluster-configuration/tree/master/Samples)

Bu dosya bölümlerde hakkında daha fazla bilgi için bkz: [tek başına Windows kümesi için yapılandırma ayarlarını](service-fabric-cluster-manifest.md).

İndirdiğiniz paketinden ClusterConfig.json dosyalarından birini açın ve aşağıdaki ayarları değiştirin:
| **Yapılandırma ayarı** | **Açıklama** |
| --- | --- |
| **NodeTypes** |Düğüm türleri, Küme düğümlerinizi çeşitli gruplar halinde ayırmanıza olanak sağlar. Bir küme en az bir NodeType olması gerekir. Bir gruptaki tüm düğümlerde aşağıdaki ortak özelliklere sahiptir: <br> **Ad** -düğüm türü adı budur. <br>**Uç nokta bağlantı noktaları** - bu çeşitli son bu düğüm türü ile ilişkili noktalarını (bağlantı noktaları) adlandırılır. Bu bildiriminde başka bir şey ile çakışmadığından sürece istiyor ve zaten değil makine/VM üzerinde çalışan başka bir uygulama tarafından kullanılmakta olan bağlantı noktası numarası kullanabilirsiniz. <br> **Yerleşim özellikleri** -bunlar kısıtlamalarından sistem hizmetleri veya hizmetleriniz için kullandığınız bu düğüm türü için özellikleri açıklar. Bu özellikler için belirli bir düğümün ek meta veri sağlayan kullanıcı tanımlı anahtar/değer çiftleridir. Düğüm özellikleri örnekleri, bir sabit sürücü veya ekran kartı, sayısı dağılımı sabit sürücü, çekirdek ve diğer fiziksel özellikler düğümü olup olacaktır. <br> **Kapasiteleri** -düğüm kapasiteleri tanımlamak adını ve belirli bir kaynak miktarını belirli bir düğüme tüketimi için kullanılabilir olduğunu. Örneğin, bir düğümü "MemoryInMb" adlı bir ölçüm için kapasiteye sahip olduğundan ve varsayılan olarak 2048 MB kullanılabilir olduğunu tanımlayabilir. Bu kapasiteleri çalışma zamanında belirli miktarda kaynağı gerekli hizmetleri, bu gerekli tutarların kullanılabilir kaynaklarınız düğümlere yerleştirilir emin olmak için kullanılır.<br>**IsPrimary** - yalnızca bir değerle için birincil ayarlandığından emin olun birden fazla NodeType tanımlı varsa *doğru*, burada Çalıştır Sistem Hizmetleri olduğu. Diğer tüm düğüm türleri değerine ayarlanmalıdır *false* |
| **Düğümler** |Bunlar (düğüm türü, düğüm adı, IP adresi, hata etki alanı ve düğümün yükseltme etki alanı) kümesinin parçası olan düğümlerinin her biri için ayrıntılar verilmektedir. Makine IP adreslerini burada listelenecek gereksinimi oluşturulacak küme istiyorsunuz. <br> Tüm düğümler için aynı IP adresi kullanırsanız, ardından bir kutusunu küme, test amacıyla kullanabileceğiniz oluşturulur. Bir çalıştırma kümeleri üretim iş yükleri dağıtmak için kullanmayın. |

Küme yapılandırmasını ortama yapılandırılan tüm ayarları aldıktan sonra (adım 7) küme ortamında sınanabilir.

<a id="environmentsetup"></a>

## <a name="environment-setup"></a>Ortam kurulumu

Bir Küme Yöneticisi bir Service Fabric tek başına küme yapılandırdığında, ortamın aşağıdaki ölçütlerle ayarlanmış olması gerekir: <br>
1. Kümeyi oluşturan kullanıcının küme yapılandırma dosyasındaki düğümler olarak listelenen tüm makinelere yönetici düzeyi güvenlik ayrıcalıkları olmalıdır.
2. Her küme düğümü makine yanı sıra küme oluşturulduğu makine gerekir:
* Service Fabric SDK kaldırdınız
* Service Fabric çalışma zamanı modülünün kaldırıldı 
* Windows Güvenlik Duvarı hizmetini (mpssvc) etkinleştirdikten
* Uzak Kayıt Hizmeti (remoteregistry) etkinleştirdiyseniz
* Dosya Paylaşımı (SMB) etkin
* Gerekli bağlantı noktaları açıldı, küme yapılandırması bağlantı noktalarını temel alarak sahip
* Sahip Windows SMB ve uzak kayıt defteri hizmeti için açılan gerekli bağlantı noktaları: 135 ve 137, 138, 139 ve 445
* Bir diğer ağ bağlantısına sahip
3. Küme düğümü makinelerden hiçbiri bir etki alanı denetleyicisi olmalıdır.
4. Kümenin dağıtılması için güvenli bir küme ise, Önkoşullar yerleştirin ve yapılandırmalara karşı doğru şekilde yapılandırıldığından bulunan gerekli güvenlik doğrulayın.
5. Küme makinelerinde Internet erişilebilir değilse, aşağıdaki küme yapılandırmasında ayarlayın:
* Telemetri devre dışı bırak: altında *özellikleri* ayarlamak *"enableTelemetry": yanlış*
* Otomatik yapı sürümü indirme & Geçerli Küme sürümü destek sonuna yaklaşıyor bildirimleri devre dışı bırak: altında *özellikleri* ayarlamak *"fabricClusterAutoupgradeEnabled": yanlış*
* Alternatif olarak, ağ Internet erişimi beyaz listelenen etki alanları için sınırlı ise, aşağıdaki etki alanları otomatik yükseltme için gereken: go.microsoft.com download.microsoft.com

6. Uygun Service Fabric virüsten koruma Dışlamalar ayarlayın:

| **Virüsten koruma dışlanan dizinler** |
| --- |
| Program Files\Microsoft Service Fabric |
| FabricDataRoot (küme yapılandırmasından) |
| FabricLogRoot (küme yapılandırmasından) |

| **Virüsten koruma hariç tutulan işlemler** |
| --- |
| Fabric.exe |
| FabricHost.exe |
| FabricInstallerService.exe |
| FabricSetup.exe |
| FabricDeployer.exe |
| ImageBuilder.exe |
| FabricGateway.exe |
| FabricDCA.exe |
| FabricFAS.exe |
| FabricUOS.exe |
| FabricRM.exe |
| FileStoreService.exe |

## <a name="validate-environment-using-testconfiguration-script"></a>TestConfiguration komut dosyası kullanarak ortamı doğrula
Tek başına paketinde TestConfiguration.ps1 komut dosyası bulunamadı. En iyi yöntemler Çözümleyicisi yukarıdaki ölçütleri bazıları doğrulamak için kullanılır ve bir küme belirli bir ortamda dağıtılabilir olup olmadığını doğrulamak için sağlamlık denetimi olarak kullanılmalıdır. Listenin altında herhangi bir hata varsa, başvurmak [ortam Kurulumu](service-fabric-cluster-standalone-deployment-preparation.md) sorun giderme. 

Bu komut dosyası, küme yapılandırma dosyasındaki düğümler olarak listelenen tüm makineler yönetici erişimi olan herhangi bir makinede çalıştırılabilir. Bu komut dosyasını çalıştırmak makine kümesinin parçası olması gerekmez.

```powershell
PS C:\temp\Microsoft.Azure.ServiceFabric.WindowsServer> .\TestConfiguration.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json
Trace folder already exists. Traces will be written to existing trace folder: C:\temp\Microsoft.Azure.ServiceFabric.WindowsServer\DeploymentTraces
Running Best Practices Analyzer...
Best Practices Analyzer completed successfully.


LocalAdminPrivilege        : True
IsJsonValid                : True
IsCabValid                 : True
RequiredPortsOpen          : True
RemoteRegistryAvailable    : True
FirewallAvailable          : True
RpcCheckPassed             : True
NoConflictingInstallations : True
FabricInstallable          : True
Passed                     : True
```

Şu anda bu bağımsız olarak yapılması gerekir böylece bu yapılandırmayı test modül güvenlik yapılandırması doğrulamaz.  

> [!NOTE]
> Eksik veya hatalı bir durumda ise, düşündüğünüz değil için sürekli olarak iyileştirmeler Bu modülün daha sağlam hale TestConfiguration tarafından şu anda yakalanan yapıyoruz, lütfen aracılığıyla bize bizim [destek kanalları](https://docs.microsoft.com/azure/service-fabric/service-fabric-support).   
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
* [Windows Server çalıştıran tek başına kümesi oluşturma](service-fabric-cluster-creation-for-windows-server.md)
