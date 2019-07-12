---
title: Azure Geçişi Hakkında | Microsoft Azure
description: Azure Geçişi hizmetine genel bir bakış sağlar.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: overview
ms.date: 07/11/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 083eaaa5be35d515a477ce6286f226b267569e1e
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67840356"
---
# <a name="about-azure-migrate"></a>Azure Geçişi Hakkında

Bu makalede, Azure geçişi, hızlı bir genel bakış sağlar.

Azure geçişi, Azure'a geçirmek için yardımcı olur. Azure geçişi, Azure'da şirket içi altyapı, uygulama ve veri bulma ve değerlendirme izlemek için merkezi bir hub sağlar. Hub'ı Azure Araçları, değerlendirme ve geçiş, ek olarak üçüncü taraf bağımsız yazılım satıcısı (ISV) teklifleri sağlar. Şu olanakları sunar:

- **Birleşik geçiş platform**: Çalıştır başlatmak için tek bir portal kullanmak ve Azure'a geçişi yolculuğunuzda izleyebilirsiniz.
- **Çeşitli araçlar**: Azure geçişi, yerel araçlar sağlar ve diğer Azure hizmetleriyle yanı sıra ISV araçlarla tümleşir. Kuruluş gereksinimlerinize göre doğru değerlendirme ve Geçiş Araçları'nı seçin. 
- **Azure geçişi Server değerlendirmesi**: Değerlendirmek için Server değerlendirmesi aracını VMware Vm'leri ve Hyper-V Vm'lerini Azure'a geçiş için şirket içi.
- **Azure geçişi sunucusu geçişi**: Şirket içi VMware Vm'lerinden, Hyper-V sanal makineleri, bulut Vm'leri ve fiziksel sunucuları Azure'a geçirmek için Server Geçiş aracını kullanın.
- **Azure geçişi, veritabanı değerlendirmesini**: Şirket içi veritabanlarının Azure'a geçiş için değerlendirin.
- **Azure geçişi, veritabanı geçişi**: Şirket içi veritabanlarının Azure'a geçirin.


## <a name="azure-migrate-versions"></a>Azure geçişi sürümleri

Azure geçişi hizmeti iki sürümü vardır:

- **Geçerli sürümü**: Bu sürüm, Azure geçişi projeleri oluşturma, şirket içi makineleri bulma ve değerlendirme ve geçiş düzenlemek için kullanın. [Daha fazla bilgi edinin](whats-new.md) bu sürümdeki yenilikler hakkında bilgi.
- **Önceki sürüm**: (Yalnızca değerlendirme şirket içi VMware vm'lerinin destekleniyordu) Azure Geçişi'nın önceki sürümünü kullanıyorsanız, artık geçerli sürümü kullanmanız gerekir. Artık, önceki sürümünü kullanarak Azure geçişi projeleri oluşturmak veya yeni bir bulma gerçekleştirin. Bununla birlikte, mevcut projeleri erişmeye devam edebilirsiniz. Bunu Azure portalında > **tüm hizmetleri**, arama **Azure geçişi**. Azure geçişi Panoda eski Azure geçişi projesine erişebilmek için bir bildirim ve bir bağlantı yoktur.

## <a name="isv-integration"></a>ISV tümleştirme

Yerel Azure araçlara ek olarak, Azure geçişi, ISV tekliflerin bir sayı ile tümleştirilir. Aracın gerekir ve bir Azure geçişi projesine eklemek tanımla Azure ve ISV araçları arasında geçiş yolculuğunuza Azure geçişi projesi içinde merkezi olarak izleyebilirsiniz.

**ISV** | **Özelliği**
--- | ---
[Cloudamize](https://www.cloudamize.com/platform) | Değerlendirme
[Cihaz 42](https://docs.device42.com/) | Değerlendirme
[Turbonomic](https://learn.turbonomic.com/azure-migrate-portal-free-trial) | Değerlendirme
[UnifyCloud](https://www.cloudatlasinc.com/cloudrecon/) | Değerlendirme
[Corent Technology](https://www.corenttech.com/AzureMigrate/) | Değerlendirmek ve geçişi
[Carbonite](https://www.carbonite.com/globalassets/files/datasheets/carb-migrate4azure-microsoft-ds.pdf) | Geçiş

### <a name="selecting-an-isv-tool"></a>Bir ISV aracı seçme

Azure geçişi projesinde bir ISV aracı ekledikten sonra aracı tarafından bir lisans almak veya ISV ilkesine uygun olarak ücretsiz bir deneme için kaydolma kullanmaya başlayın. Her Aracı'nda Azure geçişi için bağlanmak için bir seçenek yoktur. Aracı yönergeleri ve belgeleri, Azure geçişi ile araç bağlanmak için izleyin.

## <a name="azure-migrate-server-assessment"></a>Azure geçişi Server değerlendirmesi

Azure geçişi Server değerlendirmesi bulur ve şirket içi VMware Vm'leri ve Hyper-V Vm'lerini Azure'a geçiş için değerlendirir. Bunu, aşağıdaki belirlemenize yardımcı olur:

- **Azure hazırlık:** Şirket içi makinelerin Azure'a geçiş için hazır olup olmadığını değerlendirin.
- **Azure boyutlandırma:** Geçişten sonra Azure Vm'lerinin boyutunu tahmin.
- **Azure maliyet tahmini:** Şirket içi sunucuları Azure'da çalıştırmaya yönelik maliyet tahminleri.
- **Bağımlılık görselleştirmesi:** Sunucular arası bağımlılıklar ve bağımlı sunucuları Azure'a taşımak için en iyi yolu belirleyin. 

Server değerlendirmesi, şirket içinde dağıtın ve Server değerlendirmesi ile kaydetmek için basit bir gereç kullanır.

- Gerecin şirket içi makineleri bulur, Server değerlendirmesi için bağlanır ve meta verileri ve performans ile ilgili veriler Azure geçişi için sürekli olarak gönderir.
- Bulma aracısız dayanır. Hiçbir şey bulunan Vm'lerine yüklü olması gerekir.
- Makineleri bulunduktan sonra bunları genellikle birlikte geçirmek istediğiniz sanal makinelerin oluşan gruplar halinde toplayın.
- Bir grup için değerlendirme oluşturursunuz. Geçiş stratejinizi şekil için değerlendirmeyi, ardından çözümleyebilirsiniz.

## <a name="azure-migrate-server-migration"></a>Azure geçişi sunucusu geçişi

Azure sanal makineleri geçirmek için VMware Vm'leri, Hyper-V sanal makinelerini, fiziksel sunucuları diğer sanal makineler ve genel şirket içinde azure geçirme sunucusu geçişine yardımcı olur bulut. Sonra bunları değerlendirme veya değerlendirme olmadan makineleri geçirebilirsiniz. 

## <a name="azure-migrate-database-assessment"></a>Azure geçişi, veritabanı değerlendirmesi

Azure geçişi ile Data Migration Yardımcısı'nı (Azure SQL DB, Azure SQL yönetilen örneği veya Azure SQL Server çalıştıran sanal makineleri geçiş için şirket içi SQL Server veritabanlarını değerlendirmek için DMA) tümleştirir. DMA, geçiş için olası engelleme sorunları hakkında bilgi sağlar. Bu, geçişten sonra yararlanabilir ve veritabanı geçişi için sağdaki yol tanımlamanıza yardımcı olur yeni özelliklerin yanı sıra, desteklenmeyen özellikleri tanımlar. [Daha fazla bilgi edinin](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017).


## <a name="azure-migrate-database-migration"></a>Azure geçişi, veritabanı geçişi

Azure geçişi ile Azure veritabanı geçiş hizmeti (şirket içi veritabanlarının Azure'a geçirmek için DMS), tümleştirir. DMS, şirket içi veritabanları SQL, Azure SQL veritabanı ve Azure SQL yönetilen örnekler çalışan Azure Vm'lerine geçirmek için kullanın. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/dms/dms-overview).

## <a name="web-app-assessment-and-migration"></a>Web uygulamasını değerlendirme ve geçiş

Azure geçişi hub'dan değerlendirmek ve şirket içi web uygulamaları Azure'a geçirme.

- **Web apps çevrimiçi değerlendirmek**: Şirket içi Web siteleri geçiş için Azure App Service'ı değerlendirmek için Azure App Service Migration Yardımcısı'nı kullanın.
- **Web uygulamaları geçirme**: .NET ve PHP web uygulamaları, Azure App Service Migration Yardımcısı'nı kullanarak Azure'a geçirin.

[Daha fazla bilgi edinin.](https://appmigration.microsoft.com/)

## <a name="offline-data-migration"></a>Çevrimdışı veri geçişi

Data Box çevrimdışı ailesinin büyük miktarda veriyi Azure'a taşımak için kullanabilirsiniz. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/databox/)

## <a name="next-steps"></a>Sonraki adımlar

- Değerlendirmek için öğreticilerimize deneyin [VMware Vm'lerini](tutorial-assess-vmware.md) ve [Hyper-V Vm'lerini](tutorial-assess-hyper-v.md).
- Azure Geçişi fiyatlandırması hakkında [daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/azure-migrate/).
- Azure Geçişi hakkında [sık sorulan soruları gözden geçirin](resources-faq.md).
