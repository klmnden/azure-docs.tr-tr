---
title: Contoso-ölçek azure'a geçiş | Microsoft Docs
description: Contoso ölçeklendirilmiş azure'a nasıl işlediğini öğrenin.
services: azure-migrate
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 10/08/2018
ms.author: raynew
ms.openlocfilehash: 9253051d907a811ffedad3a714112c9b25543a35
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60667456"
---
# <a name="contoso---scale-a-migration-to-azure"></a>Contoso - ölçek azure'a geçiş

Bu makalede, Contoso uygun ölçekte azure'da bir geçiş yapar. Bunlar nasıl planlayıp 3000'den fazla iş yükleri, 8000 veritabanları ve 10. 000'den sanal makineleri geçirmek göz önünde bulundurun.


Bu makalede şirket içi kaynaklarını Contoso adlı kurgusal şirketin Microsoft Azure bulutuna nasıl geçirdiğini belge makaleler serisinin biridir. Seri arka plan içerir ve planlama bilgilerini ve bir geçiş altyapısını kurma nasıl çalışılacağını dağıtım senaryoları geçiş için şirket içi kaynaklara uygunluğunu değerlendirmek ve farklı türde geçiş çalıştırın. Senaryoları, karmaşık hale gelmesi. Zaman içinde makaleler serinin ekleyeceğiz.


**Makale** | **Ayrıntılar** | **Durum**
--- | --- | ---
[1. makale: Genel bakış](contoso-migration-overview.md) | Makale serisi, Contoso'nun geçiş stratejisi ve dizisinde kullanılan örnek uygulamalar genel bakış. | Kullanılabilir
[2. makale: Bir Azure altyapısı dağıtma](contoso-migration-infrastructure.md) | Contoso şirket içi altyapısını ve Azure altyapısını geçiş için hazırlar. Altyapıyı, serideki tüm geçiş makaleleri için kullanılır. | Kullanılabilir.
[3. makale: Şirket içi kaynaklarınızı Azure'a geçiş için değerlendirme](contoso-migration-assessment.md)  | Contoso, Vmware'de çalıştırılan şirket içi SmartHotel360 uygulamasının bir değerlendirme çalışır. Contoso Azure geçişi hizmeti ve veri geçiş Yardımcısı'nı kullanarak uygulama SQL Server veritabanı kullanarak uygulama Vm'leri değerlendirir. | Kullanılabilir
[4. makale: Bir Azure VM ve SQL veritabanı yönetilen örneği bir uygulamada barındırma](contoso-migration-rehost-vm-sql-managed-instance.md) | Contoso, Azure'a lift-and-shift ile taşıma geçiş için kendi şirket içi SmartHotel360 uygulaması çalışır. Contoso geçirir uygulama ön uç VM kullanarak [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview). Contoso geçirir uygulama veritabanını kullanarak bir Azure SQL veritabanı yönetilen örneği [Azure veritabanı geçiş hizmeti](https://docs.microsoft.com/azure/dms/dms-overview). | Kullanılabilir   
[Makale 5: Bir uygulamayı Azure vm'lerinde yeniden barındırma](contoso-migration-rehost-vm.md) | Contoso, SmartHotel360 uygulama sanal makinelerini Azure Site Recovery hizmetini kullanarak sanal makineleri geçirir. | Kullanılabilir
[Makale 6: Azure sanal makinelerinde ve SQL Server AlwaysOn Kullanılabilirlik grubuna bir uygulamayı barındırma](contoso-migration-rehost-vm-sql-ag.md) | Contoso uygulaması app sanal makineleri ve veritabanı geçiş hizmeti uygulama veritabanı AlwaysOn Kullanılabilirlik grubu tarafından korunan bir SQL Server kümesine geçirmek için geçirmek için Site RECOVERY'yi kullanarak geçirir. | Kullanılabilir
[Makale 7: Azure vm'lerinde Linux uygulaması barındırma](contoso-migration-rehost-linux-vm.md) | Contoso Azure vm'lerine, Site Recovery hizmetini kullanarak kendi Linux osTicket uygulamasının lift-and-shift ile taşıma geçiş tamamlanır. | Kullanılabilir
[Makale 8: MySQL için Azure sanal makineler ve Azure veritabanı üzerinde bir Linux uygulaması barındırma](contoso-migration-rehost-linux-vm-mysql.md) | Contoso, Linux osTicket uygulaması, Site Recovery kullanarak Azure Vm'lerine geçirir. Bu uygulama veritabanı için Azure veritabanı için MySQL MySQL Workbench kullanarak geçirir. | Kullanılabilir
[Makale 9: Bir Azure web uygulaması ve Azure SQL veritabanı içinde bir uygulamayı yeniden düzenleme](contoso-migration-refactor-web-app-sql.md) | Contoso, SmartHotel360 uygulama için bir Azure web uygulaması geçirir ve uygulama veritabanı için veritabanı geçiş Yardımcısı'nı kullanarak bir Azure SQL Server örneği geçirir. | Kullanılabilir    
[Makale 10: MySQL için bir Azure web uygulaması ve Azure veritabanı'nda bir Linux uygulama yeniden düzenleyin](contoso-migration-refactor-linux-app-service-mysql.md) | Contoso, Linux osTicket uygulaması birden çok siteye bir Azure web uygulamasında geçirir. Web uygulaması için sürekli teslimi GitHub ile tümleşiktir. Örnek MySQL için Azure veritabanı uygulama veritabanı geçirir. | Kullanılabilir
[11. makale: Azure DevOps Hizmetleri Team Foundation Server'da yeniden düzenleyin](contoso-migration-tfs-vsts.md) | Contoso, Azure DevOps Hizmetleri azure'da, şirket içi Team Foundation Server dağıtımı geçirir. | Kullanılabilir
[Makale 12: Bir uygulamayı Azure kapsayıcıları ve Azure SQL veritabanı yeniden oluşturma](contoso-migration-rearchitect-container-sql.md) | Contoso, SmartHotel uygulamayı Azure'a geçirir. Ardından, Azure Service Fabric ve Azure SQL veritabanı ile uygulama veritabanı çalıştıran bir Windows kapsayıcısı olarak app web katmanından rearchitects. | Kullanılabilir    
[Makale 13: Uygulamanızı Azure'a yeniden oluşturun](contoso-migration-rebuild.md) | Contoso Azure özellikleri ve Hizmetleri, Azure App Service, Azure Kubernetes Service (AKS), Azure işlevleri, Azure Bilişsel hizmetler ve Azure Cosmos DB dahil olmak üzere çeşitli kullanarak kendi SmartHotel uygulaması oluşturur. | Kullanılabilir 
Makale 14: Azure'a geçiş ölçeklendirin | Geçiş birleşimleri denedikten sonra Contoso Azure tam geçişi ölçeklendirilebilecek şekilde hazırlar. | Bu makalede

## <a name="business-drivers"></a>İş sürücüleri

BT yönetim takımı, bu geçişle elde etmek istedikleri anlamak için iş ortaklarıyla yakından çalıştı:

- **Adres büyütmeye**: Contoso şirket içi sistemler ve altyapınıza baskısı neden artıyor.
- **Verimliliği artırmak**: Contoso gereksiz yordamları kaldırın ve geliştiriciler ve kullanıcılar için işlemleri daha verimli hale getirin gerekiyor. Hızlı olmasını iş ihtiyaçlarınıza BT ve değil atık zaman ve para, bu nedenle müşteri gereksinimlerine daha hızlı teslim.
- **Çevikliği artırın**: Contoso BT işletme ihtiyaçlarını daha duyarlı olmamız gerekir. Market'te genel ekonomi de başarılı etkinleştirmek için değişiklikleri daha hızlı tepki vermek mümkün olması gerekir. Ulaşın veya bir iş engelleyici hale gerekmez.
- **Ölçek**: İş başarıyla büyüdükçe, Contoso BT Ekibi aynı yükselmeye mümkün sistemlerini sağlamanız gerekir.
- **Maliyet modelleri geliştirmek**: Contoso BT bütçenizi sermaye gereksinimlerini azaltmak istemektedir.  Contoso, ölçeklendirme ve pahalı donanımlar gereksinimini azaltmak için bulut yeteneklerini kullanmanıza ister.
- **Düşük maliyetler lisanslama**: Bulut maliyetlerini en aza indirmek contoso istiyor.


## <a name="migration-goals"></a>Geçiş hedefleri

Contoso bulut takım hedeflerini bu geçiş için aşağı sabitlenmiş. Bu hedefleri, en iyi geçiş yöntemini belirlemek için kullanıldı.

**Gereksinimler** | **Ayrıntılar**
--- | --- 
**Azure'a kolayca taşıyın** | Contoso, uygulamaları ve VM'ler için Azure mümkün olan en kısa sürede taşımaya başlamak istiyor.
**Tam envanterini derleme** | Contoso kuruluşundaki tam stok durumuna tüm uygulamaları, veritabanları ve VM'ler istiyor.
**Değerlendirmek ve uygulamaları sınıflandırma** | Contoso tamamen buluttan yararlanarak istiyor. Varsayılan olarak, tüm hizmetleri PaaS çalışacağını Contoso varsayar. Iaas, PaaS burada uygun olmayan kullanılır. 
**Eğitim ve DevOps için taşıma** | Contoso bir DevOps modeline taşımak istiyor. Contoso Azure ile DevOps eğitimi sağlar ve takımların yeniden düzenlenebileceği gerektiğinde. 


Hedefleri ve gereksinimleri sabitleme sonra Contoso BT kaplama inceler ve geçiş işlemini tanımlar.

## <a name="current-deployment"></a>Geçerli dağıtım

Planlama ve ayarlama sonra bir [Azure altyapı](contoso-migration-infrastructure.md) ve farklı kavram kanıtı (POC) geçiş birleşimleri olarak yukarıdaki tabloda açıklandığı gibi denemeye, Contoso tam bir geçiş Azure'da uygun ölçekte süreçlerle hazırdır. Contoso geçmek istediği aşağıda verilmiştir.

**Öğesi** | **Birim** | **Ayrıntılar**
--- | --- | ---
**İş yükleri** | 3. 000'den fazla uygulama | Uygulamalar, VM'ler üzerinde çalıştırın.<br/><br/>  Windows, SQL tabanlı ve OSS LAMP uygulamalardır.
**Veritabanları** | Yaklaşık 8,500 | SQL Server, MySQL, PostgreSQL veritabanları içerir.
**VM’ler** | 10. 000'den fazla | Vm'leri, VMware ana bilgisayarlarda çalışır ve vCenter sunucuları tarafından yönetilir.


## <a name="migration-process"></a>Geçiş işlemi

Contoso sabitlenmiş göre iş sürücüleri ve geçiş amaçları aşağı, geçiş işlemi için dört başlı bir yaklaşım belirler:

- **1. Aşama-değerlendirme**: Geçerli varlıklarını bulmak ve olup, Azure'a geçiş için uygun kullanıma şekil.
- **2. Aşama-geçiş**: Varlıkları Azure'a taşıyın. Nasıl uygulamaları taşınabilecek Azure nesnelere uygulamasının üzerinde bağlıdır ve elde etmek istedikleri.
- **3. Aşama-en iyi duruma getirme**: Kaynakları Azure'a taşıdıktan sonra geliştirmek ve en yüksek performans ve verimlilik için kolaylaştırmak Contoso gerekir.
- **Aşama 4-güvenliğini sağlama ve yönetme**: Olan her şeyi yerinde, Contoso artık Azure güvenliği ve Yönetimi kaynakları ve Hizmetleri yöneten, güvenli ve azure'da, bulut uygulamalarını izlemek için kullanır.


Bu aşama kuruluş genelinde seri değildir. Her parça Contoso'nun geçiş projesi, değerlendirme ve geçiş işlemi farklı bir aşamada olacaktır. En iyi duruma getirme, güvenlik ve yönetim zaman içinde sürekli olacaktır.


## <a name="phase-1-assess"></a>1. Aşama: Değerlendirme

Contoso, bulma ve şirket içi uygulamalarınızı, verilerinizi ve altyapınızı değerlendiriliyor kapatma işlemini başlatıyor. Contoso ne yapacağını aşağıda verilmiştir:

- Contoso uygulamaları bulmak gereken, uygulamalar arasında bağımlılıklar eşler ve geçiş sırası ve öncelik karar verin.
- Contoso değerlendirir gibi uygulamalar ve kaynaklar kapsamlı Envanterine oluşturacaksınız. Yeni bir envanter yanı sıra, Contoso kullanın ve var olan yapılandırma yönetimi veritabanı (CMDB) ve hizmet Kataloğu güncelleştirin.
    - CMDB Contoso uygulamalar için teknik yapılandırmaları tutar.
    - İlişkili iş ortakları ve hizmet düzeyi sözleşmeleri (SLA'lar) dahil olmak üzere uygulamaların ve işletimsel ayrıntıları hizmet Kataloğu belgeleri

### <a name="discover-apps"></a>Uygulamaları keşfedin

Contoso binlerce uygulama sayıda sunucu üzerinde çalışır. CMDB ve hizmet Kataloğu ek olarak, Contoso Keşif ve değerlendirme araçları gerekir. 

- Araçlar, Değerlendirme verileri geçiş işlemine besleyebilecek bir mekanizma sağlamanız gerekir.
- Değerlendirme araçları, Contoso'nun fiziksel ve sanal kaynakları envanterini akıllı yapı yardımcı olan veri sağlamanız gerekir. Veri profili bilgilerini ve performans ölçümlerini içermelidir.
- Bulma işlemi tamamlandığında, Contoso tam stok durumuna varlıklarınızı ve ilişkili meta verileri olmalıdır. Bu geçiş planı tanımlamak için kullanılır.

### <a name="identify-classifications"></a>Sınıflandırmaları tanımlayın

Contoso Envanter varlıkları sınıflandırmak için bazı genel kategorileri tanımlar. Bu sınıflandırmalar Contoso'nun kararların geçiş için kritik öneme sahiptir. Sınıflandırması listesi geçiş öncelikleri kurmak ve karmaşık sorunları belirlemenize yardımcı olur.

**Kategori** | **Atanan değer** | **Ayrıntılar**
--- | --- | ---
İş grubu | İş grubu adları listesi | Hangi grubu için Envanter öğesini sorumlu mi?
POC adayı | Y/N | Uygulama POC veya erken benimseyen olarak buluta geçiş için kullanılabilir mi?
Teknik Borç | Hiçbiri/bazı/ciddi | Envanter öğesini çalıştıran veya desteği ürün, platform veya işletim sistemini kullanarak?
Güvenlik Duvarı uygulamaları | Y/N | Uygulamanın trafiği dışında/Internet ile iletişim mu?  Bir güvenlik duvarı ile tümleştirilir?
Güvenlik sorunları | Y/N | Güvenlik bilinen sorunlar vardır uygulamasıyla?  Uygulama, şifrelenmemiş verileri veya güncel olmayan platformlar kullanıyor mu?


### <a name="discover-app-dependencies"></a>Uygulama bağımlılıklarını keşfedin

Contoso değerlendirme işleminin bir parçası olarak, uygulamaların çalıştığı tanımlamak ve uygulama sunucuları arasında bağlantılar ve bağımlılıklar ekleyeceğimi gerekir. Contoso adımları ortamında eşler.

1. İlk adım, tek tek uygulamalar, ağ konumlarını ve gruplar için sunucular ve makineler nasıl eşleştiği Contoso bulur.
2. Bu bilgilerle Contoso açıkça birkaç bağımlılıkları olan ve hızlı geçiş için uygun olan uygulamaları tanımlayabilirsiniz.
3. Contoso, daha karmaşık bağımlılıklar ve uygulama sunucuları arasında iletişim belirlemenize yardımcı olmak için eşlemesi'ni kullanabilirsiniz. Contoso sonra mantıksal uygulamaları göstermek için bu sunucu gruplandırma ve bu gruplara göre bir geçiş stratejisi planlama.


Tamamlanan eşleme ile tüm uygulama bileşenleri tanımlanır ve geçiş planını oluştururken belirlenmiştir Contoso emin olabilirsiniz. 

![Bağımlılık eşlemesi](./media/contoso-migration-scale/dependency-map.png)


### <a name="evaluate-apps"></a>Uygulamaları değerlendirin

Keşif ve değerlendirme işleminin son adımı, hizmet Kataloğu her uygulamayı geçirmek nasıl ekleyeceğimi değerlendirme ve eşleştirme sonuçları Contoso değerlendirebilirsiniz. 

Bu değerlendirme işlemi yakalamak için bunlar birkaç ek sınıflandırmaları envantere ekleyin.

**Kategori** | **Atanan değer** | **Ayrıntılar**
--- | --- | ---
İş grubu | İş grubu adları listesi | Hangi grubu için Envanter öğesini sorumlu mi?
POC adayı | Y/N | Uygulama POC veya erken benimseyen olarak buluta geçiş için kullanılabilir mi?
Teknik Borç | Hiçbiri/bazı/ciddi | Envanter öğesini çalıştıran veya desteği ürün, platform veya işletim sistemini kullanarak?
Güvenlik Duvarı uygulamaları | Y/N | Uygulamanın trafiği dışında/Internet ile iletişim mu?  Bir güvenlik duvarı ile tümleştirilir?
Güvenlik sorunları | Y/N | Güvenlik bilinen sorunlar vardır uygulamasıyla?  Uygulama, şifrelenmemiş verileri veya güncel olmayan platformlar kullanıyor mu?
Geçiş stratejisi | Rehost/düzenleme/yeniden oluşturma/yeniden oluşturma | Ne tür bir geçiş için uygulamanın gerekiyor? Uygulamayı Azure'da nasıl dağıtılır? [Daha fazla bilgi edinin](contoso-migration-overview.md#migration-strategies).
Teknik karmaşıklığı | 1-5 | Geçiş nasıl karmaşık mı? Bu değer, Contoso DevOps ve ilgili iş ortakları tarafından tanımlanmalıdır.
İş birbirleri ile olan önem | 1-5 | İş için uygulamanın ne kadar önemlidir? Örneğin, kuruluş kullanılan kritik bir uygulama bir puan beş atanabilir ancak küçük bir çalışma grubu uygulaması, bir puan atanabilir. Bu puanı geçiş öncelik düzeyi etkiler.
Geçiş önceliği | 1/2/3 | Hangi geçiş öncelikli uygulama için?
Geçiş risk  | 1-5 | Uygulamayı geçirmek için risk düzeyi nedir? Bu değer üzerine Contoso DevOps ve ilgili iş ortakları tarafından kabul edilmelidir.




### <a name="figure-out-costs"></a>Maliyetleri Şekil

Maliyetleri ve Azure geçiş olası tasarruf out şekil için Contoso kullanabilirsiniz [toplam maliyeti, sahipliği (TCO) hesaplayıcı](https://azure.microsoft.com/pricing/tco/calculator/) hesaplamak ve Azure için TCO'yu karşılaştırılabilir şirket içi dağıtımına karşılaştırmak için.

### <a name="identify-assessment-tools"></a>Değerlendirme araçları tanımlayın

Contoso hangi aracı bulma, değerlendirme ve stok oluşturmaya karar verir. Contoso Azure Araçları ve Hizmetleri, yerel uygulama araçları ve komut dosyaları ve iş ortağı araçları bir karışımını tanımlar. Özellikle, Contoso nasıl Azure geçişi ölçekli olarak değerlendirmek için kullanılan ilgileniyor.


#### <a name="azure-migrate"></a>Azure Geçişi


Azure geçişi hizmeti, Azure'a geçiş için hazırlık şirket içi VMware Vm'lerini keşfedin ve değerlendirin yardımcı olur. İşte Azure geçişi şunları yapar:

1. Keşfedin: Şirket içi VMware Vm'lerini keşfedin.
    - Azure geçişi, birden fazla vCenter sunucuları keşiften (seri olarak) destekler ve bulmalar ayrı Azure geçişi projelerinde çalıştırabilirsiniz.
    - Azure geçişi, VMware geçişi Toplayıcısı'nı çalıştıran bir VM'de bir yöntemle bulma işlemini gerçekleştirir. Aynı Toplayıcı, farklı vCenter sunucuları üzerinde Vm'leri bulmak ve farklı projelere verileri gönderin.
1. Hazır olma durumu değerlendirin: Şirket içi makineleri Azure'da çalıştırılmaya uygun olup olmadığını değerlendirin. Değerlendirme içerir:
    - Boyut önerileri: Şirket içi sanal makinelerin performans geçmişi temel alarak Azure Vm'leri için boyut önerileri alın.
    - Tahmini aylık Maliyet: Şirket içi makineleri Azure'da çalıştırmanın tahmini maliyetleri alın.
2. Bağımlılıkları tanımlayın:  Değerlendirme ve geçiş için en uygun makine grupları oluşturmak için şirket içi makinelerin bağımlılıklarını görselleştirin.


![Azure Geçişi](./media/contoso-migration-scale/azure-migrate.png)


##### <a name="migrate-at-scale"></a>Uygun ölçekte geçirme

Azure Geçişi'ı doğru kullanma Contoso gereksinimlerini Bu geçişin ölçek sağlar. 

- Contoso Azure geçişi ile bir uygulama tarafından değerlendirme yapın. Bu, Azure geçişi güncel veriler Azure portalında döndürdüğünü sağlar.
- Contoso yöneticileri okuma hakkında [Azure geçişi, uygun ölçekte dağıtma](how-to-scale-assessment.md)
- Contoso aşağıdaki tabloda özetlenen Azure geçişi sınırları notlar.


**Eylem** | **Sınırı**
--- | ---
Azure geçişi projesi oluşturun | 1500 sanal makine
Bulma | 1500 sanal makine
Değerlendirme | 1500 sanal makine

Contoso Azure geçişi aşağıdaki gibi kullanır:

- Vcenter'da Contoso Vm'leri klasörler halinde organize eder. Bu kendileri için değerlendirme Vm'leri belirli bir klasöre karşı çalışan odaklanmasını kolaylaştıracak.
- Azure geçişi, makineler arasındaki bağımlılıkları değerlendirmek için Azure hizmet eşlemesi kullanır. Bu aracıları denetlenmesini Vm'lerde yüklü olmasını gerektirir. 
    - Contoso, gerekli Windows veya Linux aracıları yüklemek için otomatik betikler kullanır.
    - Komut dosyası tarafından Contoso yükleme VM'ler için bir vCenter klasördeki gönderebilirsiniz.


#### <a name="database-tools"></a>Veritabanı araçları

Azure geçişi ek olarak, Contoso özellikle veritabanı değerlendirmesi için araçları kullanarak odaklanır. Araçlar [veritabanı geçiş Yardımcısı'nı](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017) geçiş için SQL Server veritabanlarını değerlendirmenize yardımcı olur.

Data Migration Yardımcısı (DMA), Contoso, şirket içi veritabanları, Azure SQL veritabanı, bir Azure Iaas VM ve Azure SQL yönetilen örneği üzerinde çalışan SQL Server gibi çeşitli Azure veritabanı çözümleri ile uyumlu olup olmadığını belirlemeye yardımcı olabilir. 

DMS yanı sıra, Contoso bulmak için kullandıkları başka bir betikleri ve SQL Server veritabanlarını belgeleme var. Bu, GitHub deposunda yer alır.

#### <a name="partner-tools"></a>İş ortağı araçları

Contoso, Azure'a geçiş için şirket içi ortamda değerlendirmesine yardımcı olabilecek diğer iş ortağı araçları vardır. [Daha fazla bilgi edinin](https://azure.microsoft.com/migration/partners/) Azure geçiş iş ortakları hakkında.  

## <a name="phase-2-migrate"></a>2. Aşama: Geçiş

Kendi değerlendirmesi ile tam Contoso kendi uygulamaları, verilerinizi ve altyapınızı Azure'a taşımak için araçları ihtiyaçlarınızı belirlemek için. 




### <a name="migration-strategies"></a>Geçiş stratejileri

Contoso düşünebilirsiniz dört geniş geçiş stratejileri vardır. 

**Stratejisi** | **Ayrıntılar** | **Kullanım**
--- | --- | ---
**Yeniden barındırma**  | Genellikle için "kaldırma ve kaydırma" geçiş adlandırılan, bu bir kod içermeyen azure'a geçirme mevcut uygulamaları için hızlı seçenektir.<br/><br/> Bir uygulama olarak geçirilen-riskleri veya kod değişiklikleri ile ilişkili maliyetleri olmadan bulutun avantajlarından olduğu. | Contoso hiçbir kod değişikliği gerektiren daha az stratejik uygulamaları yeniden barındırın.
**Yeniden düzenleme** |  "Yeniden paketleme olarak" da, bu strateji en az bir uygulama kodu gerektirir veya uygulama Azure PaaS bağlanma ve daha iyi bulut özelliklerinden yararlanan yapılandırma değişiklikleri gerekir. | Contoso, stratejik uygulamaların aynı temel işlevlerini korur, ancak Azure App Services gibi bir Azure platformu üzerinde çalışmasını taşıma yeniden düzenleyebilirsiniz.<br/><br/> Bu, en düşük kod değişiklikleri gerektirir.<br/><br/> Öte yandan, bu Microsoft tarafından yönetilen olmaz olduğundan VM platform korumak Contoso gerekir.
**Yeniden oluşturma** | Bu strateji, değiştirir veya uygulama mimarisi bulut özellikleri ve ölçek için en iyi duruma getirmek için temel bir uygulama kodu genişletir.<br/><br/> Bu, bir uygulamayı dayanıklı, yüksek oranda ölçeklenebilen, bağımsız bir şekilde dağıtılabilen bir mimari olarak modernizes.<br/><br/> Azure Hizmetleri, işlemi hızlandırmak, uygulamaları güvenle ölçeklendirin ve uygulamaları kolayca yönetin.
**Yeniden oluşturma** | Bu strateji, yerel bulut teknolojilerini kullanarak sıfırdan bir uygulama oluşturur.<br/><br/> Hizmet (PaaS) olarak Azure platformu, bulutta eksiksiz bir geliştirme ve dağıtım ortamı sağlar. Bazı gider ve yazılım lisansları karmaşıklığını ortadan kaldırır ve temel alınan bir uygulamanın altyapı, ara yazılım ve diğer kaynakları gereksinimini ortadan kaldırır. | Contoso baştan kritik apps, sunucusuz bir bilgisayar veya mikro hizmetler gibi bulut teknolojilerinin yararlanmak için yazabilirsiniz.<br/><br/> Contoso uygulama ve Hizmetleri geliştiren, yönetmek ve diğer her şey Azure yönetir.


Veri Ayrıca, özellikle de Contoso sahip veritabanları birimle dikkate alınmalıdır. Contoso'nun varsayılan yaklaşım bulut özelliklerinden tam olarak yararlanmak için Azure SQL veritabanı gibi PaaS hizmetlerine kullanmaktır. Veritabanları için bir PaaS hizmeti taşıyarak, Contoso yalnızca veri korumak temel platform Microsoft'a bırakarak sahip olur.

### <a name="evaluate-migration-tools"></a>Geçiş Araçları değerlendir

Contoso öncelikli olarak kullandığınız Azure hizmetlerini ve araçlarını birkaç geçiş için:

- [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview): Olağanüstü durum kurtarma hizmetini düzenler ve şirket içi Vm'leri Azure'a geçirir.
- [Azure veritabanı geçiş hizmeti](https://docs.microsoft.com/azure/dms/dms-overview): SQL Server, MySQL ve Oracle gibi şirket içi veritabanlarının Azure'a geçirir.


#### <a name="azure-site-recovery"></a>Azure Site Recovery

Azure Site Recovery için olağanüstü durum kurtarma ve gelen azure'daki ve şirket içi sitelerden azure'a geçiş işlemlerini Birincil Azure hizmetidir.

1. Site Recovery sağlar, Azure, şirket içi siteleriniz çoğaltma işlemlerini yönetir.
2. Ne zaman çoğaltma ayarlanır ve çalışan, makineler yerine Azure'a geçiş işlemini tamamlama çalışılabilir şirket.

Contoso zaten [bir POC tamamlandı](contoso-migration-rehost-vm.md) Site Recovery buluta geçirmek için bunları nasıl yardımcı olabileceğini öğrenin.

##### <a name="using-site-recovery-at-scale"></a>Site Recovery kullanarak uygun ölçekte

Contoso planları, birden çok lift-and-shift ile taşıma geçişler çalıştırma. Bu çalıştığından emin olmak için Site Recovery yaklaşık 100 sanal makineleri geçiremediğiniz aynı anda çoğaltma. Bunun nasıl çalışacağını şekil için önerilen Site Recovery geçiş için kapasite planlaması gerçekleştirmek Contoso gerekir.

- Contoso trafiği hacimleri hakkında bilgi toplamak gerekir. Özellikle:
    - Sanal makineleri çoğaltmak için istediği için değişiklik oranına belirlemek contoso gerekir.
    - Contoso, ayrıca ağ bağlantısı şirket içi siteden Azure'a dikkate almanız gerekir.
- Kapasite ve birim gereksinimleri yanıt olarak, Contoso, kurtarma noktası hedefi (RPO) karşılayacak şekilde gerekli VM'ler için günlük veri değişikliği hızınıza göre yeterli bant genişliğini ayırmanız gerekir.
- Son olarak, kaç tane sunucuya dağıtımı için gerekli olan Site Recovery bileşenlerinin çalışması için gerekli olan çıkış bulmanız gerekir.

###### <a name="gather-on-premises-information"></a>Şirket içi bilgileri toplayın
Contoso kullanabileceğiniz [Site Recovery dağıtım Planlayıcısı](https://docs.microsoft.com/azure/site-recovery/site-recovery-deployment-planner) bu adımları tamamlamak için aracı:

- Contoso, üretim ortamında bir etkisi olmadan, uzaktan sanal makinelerin profilini için Aracı'nı kullanabilirsiniz. Bu, çoğaltma ve yük devretme için bant genişliği ve depolama gereksinimlerini belirlemenize yardımcı olur.
- Contoso, tüm Site Recovery bileşenlerini şirket yüklemeden aracı çalıştırabilirsiniz.
- Aracı uyumlu ve uyumsuz VM'ler, VM başına disk sayısı hakkında bilgi toplar ve disk başına veri değişim sıklığı. Ayrıca ağ bant genişliği gereksinimlerini ve başarılı çoğaltma ve yük devretme için gereken Azure altyapısını da tanımlar.
- Site Recovery yapılandırma sunucusunun en düşük gereksinimlerini karşılayan bir Windows Server makinelerinde ardından Planlayıcısı aracını çalıştıran emin olmak contoso gerekir. Yapılandırma sunucusu şirket içi VMware Vm'lerini çoğaltmak için gerekli olan bir Site Recovery makinesidir.


###### <a name="identify-site-recovery-requirements"></a>Site Recovery gereksinimlerini tanımlama

Çoğaltılan VM'ler ek olarak, Site Recovery, VMware geçiş için çeşitli bileşenler gerektirir.

**Bileşen** | **Ayrıntılar**
--- | ---
**Yapılandırma sunucusu** | Genellikle bir VMware VM OVF şablonunu kullanarak ayarlayabilirsiniz.<br/><br/> Yapılandırma sunucusu bileşeni, şirket içi ile Azure arasındaki iletişimi düzenler ve veri çoğaltma işlemlerini yönetir.
**İşlem sunucusu** | Varsayılan olarak yapılandırma sunucusuna yüklenir.<br/><br/> İşlem sunucusu bileşenini çoğaltma verilerini alıp; Bu, önbelleğe alma, sıkıştırma ve şifreleme ile iyileştirir; ve Azure depolamaya gönderir.<br/><br/> İşlem sunucusu, çoğaltmak istediğiniz Vm'lere de Azure Site Recovery Mobility hizmeti yükler ve şirket içi makineleri otomatik olarak bulunmasını gerçekleştirir.<br/><br/> Ölçeklendirilmiş dağıtımları gerekli ek, büyük çoğaltma trafiği hacimlerini idare etmek için tek başına işlem sunucuları.
**Mobility hizmeti** | Geçirilecek her VMware VM'nin Site Recovery Mobility Hizmeti Aracısı yüklenir.  

Contoso kapasite konularına göre bu bileşenleri dağıtmanın nasıl ekleyeceğimi gerekir.

**Bileşen** | **Kapasite gereksinimleri**
--- | ---
**Maksimum günlük değişim hızı** | Bir tek işlem sunucusu günlük işleyebilir değişiklik hızı 2 TB'a kadar artırın. Bir sanal makine yalnızca bir işlem sunucusu kullanabildiğinden, çoğaltılmış bir VM için desteklenen en fazla günlük veri değişim hızı 2 TB'tır.
**En fazla aktarım hızı** | Bir standart Azure depolama hesabı en fazla saniye başına 20.000 istekleri işleyebilir ve giriş/çıkış işlemi (IOPS) saniyede arasında çoğaltılan VM bu sınır içinde olmalıdır. Örneğin, bir sanal makine 5 diskler içeriyor ve 120 IOPS (8 K boyutu) VM'deki her disk oluşturur, ardından bu azure'da 500 IOPS sınırı disk başına olacaktır.<br/><br/> Gerekli depolama hesabı sayısı 20. 000'ile ayrılmış toplam kaynak makine IOPS, eşit olduğunu unutmayın. Çoğaltılmış bir makineden yalnızca azure'da tek bir depolama hesabına ait olabilir.
**Yapılandırma sunucusu** | Contoso'nun tahminine 100 = 200 çoğaltmak dayanarak birlikte, VM'ler ve [yapılandırma sunucusu boyutlandırma gereksinimleri](../site-recovery/site-recovery-plan-capacity-vmware.md#size-recommendations-for-the-configuration-server-and-inbuilt-process-server), Contoso tahmin gibidir gereksinimlerini configuration server makinesi:<br/><br/> CPU: 16 Vcpu (2 yuva * @ 2.5 GHz 8 çekirdek)<br/><br/> Bellek: 32 GB<br/><br/> Önbellek diski: 1 TB<br/><br/> Veri değişiklik oranı: 1 TB ile 2 TB.<br/><br/> Gereksinimleri boyutlandırma ek olarak, yapılandırma sunucusuna geçirilecek vm'lerle LAN kesimi ve aynı ağ üzerinde en uygun şekilde yer olduğundan emin olmak Contoso gerekir.
**İşlem sunucusu** | Contoso 100 200 Vm'lerini çoğaltma özelliği sayesinde tek başına adanmış işlem sunucusu dağıtır:<br/><br/> CPU: 16 Vcpu (2 yuva * @ 2.5 GHz 8 çekirdek)<br/><br/> Bellek: 32 GB<br/><br/> Önbellek diski: 1 TB<br/><br/> Veri değişiklik oranı: 1 TB ile 2 TB.<br/><br/> İşlem sunucusu sabit çalışma olacaktır ve bu nedenle bir ESXi konağındaki disk g/ç ve ağ trafiğini çoğaltma için gereken CPU işleyebilir bulunması gerekir. Contoso, bu amaç için adanmış bir ana bilgisayar dikkate alacaktır. 
**Ağ** | Contoso, geçerli siteden siteye VPN altyapısı gözden geçirdi ve Azure ExpressRoute uygulamaya karar verdi. Bu daha düşük gecikme süresi ve Contoso'nun birincil Doğu ABD 2 Azure bölgesini bant genişliğini iyileştirmek için kritik bir uygulamasıdır.<br/><br/> **İzleme**: Contoso veri akışının işlem sunucusundan dikkatle izlemeniz gerekir. Veri Contoso dikkate alınır ağ bant genişliği aşırı varsa [işlem sunucusu bant genişliği azaltma](../site-recovery/site-recovery-plan-capacity-vmware.md#control-network-bandwidth).
**Azure depolama alanı** | Geçiş için doğru tür ve sayıda hedef Azure depolama hesabı, Contoso tanımlamanız gerekir.  Site Recovery, sanal makine verilerini Azure depolama alanına çoğaltır.<br/><br/> Site Recovery, standart veya premium (SSD) depolama hesaplarına çoğaltabilir.<br/><br/> Depolama hakkında karar vermek üzere Contoso gözden geçirmelisiniz [depolama sınırları](../virtual-machines/windows/disks-types.md)ve beklenen büyüme ve artan kullanım zaman içinde faktörü. Hız ve geçişlerin öncelik verildiğinde, Contoso premium SSD kullanmaya karar verdi<br/><br/>

Contoso, Azure'da dağıtılan tüm VM'ler için yönetilen diskleri kullanmayı karar vermiştir.  Gerekli IOPS disk standart HDD, SSD standart veya Premium (SSD) olup olmayacağını belirler.<br/><br/>

#### <a name="data-migration-service"></a>Veri geçiş hizmeti

Azure veritabanı geçiş hizmeti (DMS), birden çok veritabanı kaynağını sorunsuz geçiş için en düşük kapalı kalma süresi ile Azure veri platformu sağlayan tam olarak yönetilen bir hizmettir.

- DMS, mevcut araçları ve Hizmetleri işlevselliğinin tümleştirir. Data Migration Yardımcısı (DMA), veritabanı uyumluluğu ile ilgili öneriler ve gerekli değişiklikleri belirlemenize değerlendirme raporları oluşturmak için kullanır.
- DMS bir basit ve kendi kendine yol gösteren bir geçiş işlemi geçiş işleminden önce olası sorunları gidermek yardımcı olacak akıllı değerlendirmesi ile kullanır.
- DMS, uygun ölçekte birden çok kaynaktan hedefe Azure veritabanı geçiş yapabilirsiniz.
- DMS, SQL Server 2005'ten SQL Server 2017 için destek sağlar.

DMS, yalnızca Microsoft veritabanı geçiş aracı değildir. Alma bir [karşılaştırma araçları ve Hizmetleri](https://blogs.msdn.microsoft.com/datamigration/2017/10/13/differentiating-microsofts-database-migration-tools-and-services/).

###### <a name="using-dms-at-scale"></a>DMS kullanarak uygun ölçekte

Contoso, SQL Server'dan geçirilirken DMS kullanır.  

- DMS sağlanırken Contoso doğru boyutta ve veri geçişi için performansı iyileştirmek için ayarlanmış olduğunu sağlaması gerekir. Bu nedenle paralelleştirme ve daha hızlı veri aktarımı için birden çok Vcpu yararlanmak hizmet verme contoso "iş açısından kritik katman 4 sanal çekirdek ile" seçeneğini seçin.

    ![DMS ölçeklendirme](./media/contoso-migration-scale/dms.png)

- Contoso için başka bir ölçeklendirme Taktik geçici olarak Azure SQL veya MySQL veritabanı hedef örneği Premium katmanda SKU için ölçeği veri geçişi sırasında dir. Bu veritabanı veri aktarımı etkinlikleri düşük düzeyli SKU'ları kullanırken etkileyebilecek azaltma en aza indirir.



##### <a name="using-other-tools"></a>Diğer araçları kullanma

DMS yanı sıra, Contoso diğer araçları ve Hizmetleri VM bilgilerini tanımlamak için kullanabilirsiniz.

- El ile migrations ile yardımcı olacak betikler sahiptirler. Bunlar, GitHub deposunda kullanılabilir.
- Bir dizi [iş ortağı Araçları](https://azure.microsoft.com/migration/partners/) geçiş için de kullanılabilir.


## <a name="phase-3-optimize"></a>3. Aşama: İyileştirme

Contoso kaynakları Azure'a taşır. sonra performansını ve maliyet yönetimi araçlarıyla yatırım Getirisini en üst düzeye çıkarmak için kolaylaştırmak gerekir. Azure Kullandıkça Öde hizmet verildiğinde, sistemleri nasıl performans gösterdiğini anlamak ve doğru boyutlarda emin olmak için Contoso için önemlidir.


### <a name="azure-cost-management"></a>Azure maliyet Yönetimi

Contoso, kendi bulut yatırımınızdan en iyi hale getirmek için ücretsiz Azure maliyet yönetimi aracı özelliğinden yararlanır.

- Bir Microsoft yan kuruluşu olan Cloudyn tarafından oluşturulan lisanslı Bu çözüm, saydamlık ve doğru bir biçimde harcama Bulutu yönetme Contoso olanak tanır.  İzleme, ayırma ve bulut maliyetlerinizi kırpma için araçlar sağlar.
- Azure maliyet yönetimi, maliyet ayırma, Ücret hesaplama ve yansıtma yardımcı olmak için basit bir Pano raporları sağlar.
- Maliyet yönetimi, Contoso sonra yönetmek ve ayarlamak isteyeceğiniz az kullanılan kaynakları belirleyerek bulut en iyi duruma getirebilirsiniz.
- [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/cost-management/overview) Azure maliyet yönetimi hakkında.

    
![Maliyet yönetimi](./media/contoso-migration-scale/cost-management.png)  
    
 
### <a name="native-tools"></a>Yerel Araçlar

Contoso betikleri, kullanılmamış kaynakları bulmak için de kullanır.

- Büyük geçişler sırasında genellikle arta kalan ücreti uygulanır, ancak hiçbir değer sağlamak için şirket sanal sabit diskleri (VHD'ler) gibi veri parçalarını vardır. Betikleri GitHub deposunda bulunur.
- Contoso, Microsoft tarafından yapılan iş yararlanarak BT departmanı kullanıcının ve Azure kaynak iyileştirme (ARO) araç seti uygulamayı düşünün.
- Contoso, önceden yapılandırılmış bir runbook'ları ve tabloları ile Azure Otomasyonu hesabı aboneliğine dağıtın ve paradan tasarruf etmeye başlayın. Bir zamanlama etkin veya yeni kaynaklar en iyi duruma getirilmesi dahil olmak üzere, oluşturduktan sonra azure kaynak en iyi duruma getirme, bir abonelikte otomatik olarak gerçekleşir.
- Bu, maliyetleri azaltmak için merkezi olmayan Otomasyon özellikleri sağlar. Şu özellikler mevcuttur:
    - Düşük CPU üzerinde alan otomatik erteleme Azure VM'ler.
    - Azure Vm'leri, uykuda ve unsnooze zamanlayın.
    - Azure Vm'leri, uykuda veya artan ve azalan sırada Azure etiketleri kullanarak unsnooze zamanlayın.
    - Kaynak grupları üzerine toplu silme işlemi.
- Bu ARO setinde başlama [GitHub deposunu](https://github.com/Azure/azure-quickstart-templates/tree/master/azure-resource-optimization-toolkit).

### <a name="partner-tools"></a>İş ortağı araçları

İş ortağı araçları gibi [Hanu](https://hanu.com/insight/) ve [Scalr]( https://www.scalr.com/cost-optimization/) yararlanılabilir.


## <a name="phase-4-secure--manage"></a>4. Aşama: Güvenliğini sağlama ve yönetme

Bu aşamada, Contoso yöneten, güvenli ve azure'daki bulut uygulamaları izlemek için Azure güvenlik ve yönetim kaynakları kullanır. Bu kaynakları Azure portalında kullanılabilir ürünler kullanırken güvenli ve iyi yönetilen bir ortamda çalışacak yardımcı olur. Contoso, geçiş sırasında bu hizmetleri kullanmaya başlar ve Azure hibrit desteğiyle hibrit bulut genelinde çoğu için tutarlı bir deneyim kullanarak devam eder.


### <a name="security"></a>Güvenlik
Contoso, hibrit bulut iş yüklerinde Birleşik güvenlik yönetimi ve Gelişmiş tehdit koruması için Azure Güvenlik Merkezi'ndeki güvenirsiniz.

- Güvenlik Merkezi, tam görünürlük ve denetime, azure'daki bulut uygulamaları güvenliğini sağlar.
- Hızlı bir şekilde contoso algılayın ve tehditlere yanıt eylem yararlanın ve Uyarlamalı tehdit koruması sağlayarak güvenlik riskleri azaltmak.

[Daha fazla bilgi edinin](https://azure.microsoft.com/services/security-center/) Güvenlik Merkezi hakkında. 


### <a name="monitoring"></a>İzleme

Contoso görünürlük durumunu ve performansını yeni geçirilen uygulamalar, altyapı ve artık Azure çalışan veri gerekir. Contoso izleme araçları Azure İzleyici, Log Analytics çalışma alanı ve Application Insights gibi yerleşik Azure bulut özelliğinden yararlanır.
 
- Bu araçları kullanarak Contoso kolayca kaynaktan veri toplayın ve zengin içgörüler elde edebilirsiniz. Örneğin, Contoso sanal makineleri, uygulamaları ve birden çok sanal makine arasında ağ bağımlılıklarını görüntüleme CPU disk ve bellek kullanımını ölçer ve uygulama performansını izleme.
- Contoso, eylem ve hizmet çözümleriyle tümleştirmek için bu bulut izleme araçları kullanır.
- [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview) Azure izleme hakkında.

### <a name="bcdr"></a>BCDR 

Contoso Azure kaynakları için bir iş sürekliliği ve olağanüstü durum kurtarma (BCDR) stratejisine gerekir.

- Azure sağlar [yerleşik BCDR özellikleriyle](https://docs.microsoft.com/azure/architecture/resiliency/disaster-recovery-azure-applications) güvenli veri ve uygulamaları/hizmetlerinizi çalışır durumda tutmak için. 
- Yerleşik özelliklerine ek olarak, Contoso hatalardan kurtarma, yüksek maliyetli iş kesintilerinden kaçının, uyumluluk hedeflerinizi karşılayın ve verileri fidye yazılımlarına ve insan hatalarına karşı koruma emin olmak ister. Bunu yapmak için
    - Contoso Azure yedekleme için yedekleme Azure kaynaklarınızın maliyet açısından verimli bir çözüm olarak dağıtır. Yerleşik olduğundan bulut yedekleme birkaç basit adımda Contoso ayarlayabilirsiniz.
    - Contoso olağanüstü durum kurtarma ayarlama, Azure Vm'leri için çoğaltma, yük devretme ve yeniden çalışma, belirten Azure bölgeleri arasında Azure Site Recovery kullanarak ayarlayın.  Bu Azure Vm'lerinde çalışan uygulamaları birincil bölgede bir kesinti oluşursa, Contoso'nun seçme, ikincil bir bölgede kullanılabilir kalmasını sağlar. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-quickstart).

 
## <a name="conclusion"></a>Sonuç

Bu makalede, bir Azure geçişi uygun ölçekte Contoso planlanan.  Bunlar, geçiş işlemi dört aşamaya bölünür. Aracılığıyla değerlendirme ve geçiş, iyileştirme için güvenlik ve geçiş sonra Yönetim tamamlayın. Çoğunlukla, tam bir işlem olarak bir geçiş projesi planlamak için ancak sistemler bir kuruluştaki sınıflandırmalar ve iş için anlamlı sayıları kümeleri parçalamak tarafından geçirmek için önemlidir. Veriler değerlendiriliyor ve Sınıflandırmalar, uygulama tarafından ve proje daha küçük geçişleri güvenli ve hızlı bir şekilde çalışabilen bir dizi bölünmüştür.  Bu küçük geçişleri toplamını büyük başarılı bir geçiş azure'a hızlıca dönüştürür.
