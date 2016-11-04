---
title: Azure Güvenlik Merkezi'ne Giriş | Microsoft Docs
description: Azure Güvenlik Merkezi, önemli işlevleri ve nasıl çalıştığı hakkında bilgi edinin.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: StevenPo
editor: ''

ms.service: security-center
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/22/2016
ms.author: terrylan

---
# Azure Güvenlik Merkezi'ne Giriş
Azure Güvenlik Merkezi, önemli işlevleri ve nasıl çalıştığı hakkında bilgi edinin.

> [!NOTE]
> Bu belgedeki bilgiler Azure Güvenlik Merkezi önizleme sürümü için geçerlidir. Bu belge, örnek bir dağıtım kullanarak hizmeti tanıtır.
> 
> 

## Azure Güvenlik Merkezi nedir?
 Güvenlik Merkezi, artırılmış görünürlük aracılığıyla tehditleri engellemenize, algılamanıza ve yanıtlamanıza, ayrıca Azure kaynaklarınızın güvenliğini denetlemenize yardımcı olur. Aboneliklerinizde, tümleşik güvenlik izleme ve ilke yönetimi sağlar; normal koşullarda gözden kaçabilecek tehditleri algılamaya yardımcı olur ve güvenlik çözümlerinin geniş ekosistemiyle çalışır.

## Temel işlevler
 Güvenlik Merkezi, Azure'da yerleşik olan kullanımı kolay ve etkili tehdit önleme, saptama ve yanıtlama işlevlerini sunar. Temel işlevler şunlardır:

|  |  |
| --- | --- |
| Önleme |Azure kaynaklarınızın güvenlik durumunu izler |
| Şirketinizin güvenlik gereksinimlerine, kullandığınız uygulama türlerine ve verilerinizin duyarlılığına bağlı olarak Azure abonelikleriniz ve kaynak gruplarınız için ilkeler tanımlar | |
| Gerekli denetimleri uygulama işlemi boyunca hizmet sahiplerine rehberlik etmek için ilke temelli güvenlik önerilerini kullanır | |
| Microsoft ve ortaklarından güvenlik hizmetlerini ve gereçlerini hızlı bir şekilde dağıtır | |
| Algılama |Azure kaynaklarınızdan, ağdan ve kötü amaçlı yazılımdan koruma programları ve güvenlik duvarları gibi iş ortağı çözümlerinden güvenlik verilerini otomatik olarak çözümleyip toplar |
| Microsoft ürün ve hizmetleri, Microsoft Dijital Suçlar Birimi (DCU), Microsoft Güvenlik Yanıt Merkezi (MSRC) ve dış akışların küresel tehdit zekasından yararlanır | |
| Machine learning ve davranış analizi dahil olmak üzere gelişmiş analizler uygular | |
| Yanıtlama |Öncelikli güvenlik olayları/uyarıları sağlar |
| Saldırının kaynağı ve etkilenen kaynakları hakkında öngörüler sunar | |
| Geçerli saldırıyı durdurmaya ve gelecekteki saldırıları önlenmeye yönelik yöntemler önerir | |

## Tanıtıcı kılavuz
 Güvenlik Merkezi'ne [Azure portalından](https://azure.microsoft.com/features/azure-portal/) erişebilirsiniz. [Portalda oturum açın](https://portal.azure.com), **Gözat**'ı seçin ve ardından **Güvenlik Merkezi** seçeneğine kaydırın veya önceden portal panosuna sabitlediğiniz **Güvenlik Merkezi** kutucuğunu seçin.

![Azure portalında güvenlik kutucuğu][1]

Güvenlik Merkezi'nde güvenlik ilkelerini ayarlayabilir, güvenlik yapılandırmalarını izleyebilir ve güvenlik uyarıları görüntüleyebilirsiniz.

### Güvenlik ilkeleri
Azure aboneliklerinizin ve kaynak gruplarınızın ilkelerini, şirketinizin güvenlik gereksinimlerine göre tanımlayabilirsiniz. Bunları kullanmakta olduğunuz uygulama türlerine veya her bir abonelikteki verilerin duyarlılığına göre de uyarlayabilirsiniz. Örneğin, geliştirme veya test etme için kullanılan kaynaklar, üretim uygulamaları için kullanılanlardan farklı güvenlik gereksinimlerine sahip olabilir. Benzer şekilde, PII gibi düzenlenen veriler içeren uygulamalar, daha yüksek bir güvenlik düzeyi gerektirebilir.

> [!NOTE]
> Abonelik düzeyinde veya kaynak grubu düzeyinde bir güvenlik ilkesini değiştirmek için, aboneliğin Sahibi veya Katılımcısı olmanız gerekir.
> 
> 

**Güvenlik Merkezi** dikey penceresinde, aboneliklerinizin ve kaynak gruplarınızın bir listesi için **İlke** kutucuğunu seçin.   

![Güvenlik Merkezi dikey penceresi][2]

**Güvenlik ilkesi** dikey penceresinde ilke ayrıntılarını görüntülemek için bir aboneliği seçin.

![Güvenlik ilkesi dikey pencere aboneliği][3]

**Veri toplama** (yukarıya bakın) bir güvenlik ilkesi için veri toplamayı etkinleştirir. Etkinleştirildiğinde şunlar sağlanır:

* Güvenlik izleme ve öneriler için desteklenen tüm sanal makinelerin günlük taraması.
* Analiz ve tehdit algılama için güvenlik olaylarının toplanması.

**Her bölge için bir depolama hesabı seçme** (yukarıya bakın), sanal makinelerinizin çalıştığı her bir bölge için bu sanal makinelerden toplanan verilerin depolandığı depolama hesabını seçmenize olanak sağlar. Her bölge için bir depolama hesabı seçmezseniz sizin için oluşturulur. Toplanan veriler, güvenlik nedenleriyle diğer müşterilerin verilerinden mantıksal olarak yalıtılır.

> [!NOTE]
> Veri toplama ve her bölge için bir depolama hesabı seçme, abonelik düzeyinde yapılandırılır.
> 
> 

**Şunun için öneri göster** (yukarıya bakın) abonelikteki kaynakların güvenlik ihtiyaçlarına bağlı olarak izlemek ve önermek istediğiniz güvenlik denetimlerini seçmenize olanak sağlar.

Ardından, ilke ayrıntılarını görüntülemek için bir kaynak grubu seçin.

![Güvenlik ilkesi dikey penceresi kaynak grubu][4]

**Devralma** (yukarıya bakın) kaynak grubunu şu şekilde tanımlamanıza olanak sağlar:

* Devralınmış kaynak grubu(varsayılan) (Bu kaynak grubu için tüm güvenlik ilkelerinin abonelik düzeyinden devralındığı anlamına gelir)
* Benzersiz kaynak grubu (kaynak grubunun özel bir güvenlik ilkesine sahip olacağı anlamına gelir). **Şunun için öneri göster** bölümünde değişiklikler yapmanız gerekir.

> [!NOTE]
> Abonelik düzeyi ilkesi ile kaynak grubu düzeyi ilkesi arasında bir çakışma olması durumunda, kaynak grubu düzeyi ilkesi önceliklidir.
> 
> 

### Güvenlik önerileri
 Güvenlik Merkezi, olası güvenlik açıklarını tanımlamak için Azure kaynaklarınızın güvenlik durumunu inceler. Gerekli denetimlerin yapılandırılması işlemi boyunca bir öneri listesi size rehberlik eder. Örneklere şunlar dahildir:

* Kötü amaçlı yazılımı tanımlama ve kaldırmada yardım etmesi için kötü amaçlı yazılımdan koruma yazılımı hazırlama
* Sanal makinelere doğru trafiği denetlemek için ağ güvenlik gruplarını ve kurallarını yapılandırma
* Web uygulamalarınızı hedefleyen saldırılara karşı korumaya yardımcı olmak için web uygulaması güvenlik duvarı hazırlama
* Eksik sistem güncelleştirmelerini dağıtma
* Önerilen taban çizgileriyle eşleşmeyen işletim sistemi yapılandırmalarını ele alma

Öneriler listesi için **Öneriler**'e tıklayın. Ek bilgileri görüntülemek veya sorunu çözmek üzere eyleme geçmek için önerilere tıklayın.

![Azure Güvenlik Merkezi'nde güvenlik önerileri][5]

### Kaynak durumu
**Kaynak güvenlik durumu** kutucuğu; sanal makineler, web uygulamaları ve diğer kaynaklar dahil olmak üzere kaynak türüne göre ortamın genel güvenlik duruşunu gösterir.   

Tanımlanan olası güvenlik açıkları listesi dahil olmak üzere daha fazla bilgi görüntülemek için **Kaynak güvenlik durumu** kutucuğunda bir kaynak türü seçin. (Aşağıdaki örnekte **Sanal makineler** seçilidir.)

![Kaynak durumu kutucuğu][6]

### Güvenlik uyarıları
 Güvenlik Merkezi, Azure kaynaklarınızdan, ağdan ve kötü amaçlı yazılımdan koruma programları ve güvenlik duvarları gibi iş ortağı çözümlerinden günlük verilerini otomatik olarak toplar, çözümler ve tümleştirir. Tehditler algılandığında bir güvenlik uyarısı oluşturulur. Örneklere şunların algılanması dahildir:

* Bilinen kötü amaçlı IP adresleri ile iletişim kuran tehlikeye girmiş sanal makineler
* Windows hata raporlama kullanılarak algılanan gelişmiş kötü amaçlı yazılım
* Sanal makinelere karşı deneme yanılma saldırıları
* Tümleşik kötü amaçlı yazılımdan koruma programlarından ve güvenlik duvarlarından güvenlik uyarıları

**Güvenlik uyarıları** kutucuğuna tıkladığınızda öncelikli uyarıların bir listesi görüntülenir.

![Güvenlik uyarıları][7]

Bir uyarının seçilmesi, saldırı hakkında daha fazla bilgi ve sorunun nasıl çözüleceği hakkında öneriler gösterir.

![Güvenlik uyarısı ayrıntıları][8]

### İş ortağı çözümleri
**İş ortağı çözümleri** kutucuğu, Azure aboneliğinizle tümleşik iş ortağı çözümlerinizin sistem durumunu bir bakışta izlemenizi sağlar. Güvenlik Merkezi, çözümlerden gelen uyarıları görüntüler.

**İş ortağı çözümleri** kutucuğunu seçin. Tüm bağlı iş ortağı çözümlerinin listesini görüntüleyen bir dikey pencere açılır.

![İş ortağı çözümleri][9]

## Başlarken
Güvenlik Merkezi ile çalışmaya başlamak için bir Microsoft Azure aboneliğinizin olması gerekir. Güvenlik Merkezi, Azure aboneliğiniz ile etkinleştirilir. Bir aboneliğiniz yoksa [ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.

 Güvenlik Merkezi'ne [Azure portalından](https://azure.microsoft.com/features/azure-portal/) erişebilirsiniz. Daha fazla bilgi edinmek için bkz. [portal belgeleri](https://azure.microsoft.com/documentation/services/azure-portal/).

[Azure Güvenlik Merkezi'ni kullanmaya başlama](security-center-get-started.md), Güvenlik Merkezi'nin güvenlik izleme ve ilke yönetimi bileşenlerinde size hızla rehberlik sağlar.

## Sonraki adımlar
Bu belgede Güvenlik Merkezi, temel işlevleri ve nasıl kullanmaya başlayacağınız size tanıtıldı. Daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](security-center-policies.md) - Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik önerilerini yönetme](security-center-recommendations.md) - Önerilerin Azure kaynaklarınızı korumanıza nasıl yardım edeceği hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md) - Azure kaynaklarınızın sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md) - Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md) - İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) - Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure Güvenlik blogu](http://blogs.msdn.com/b/azuresecurity/) - En son Azure güvenlik haberlerini ve bilgilerini edinin.

<!--Image references-->
[1]: ./media/security-center-intro/security-tile.PNG
[2]: ./media/security-center-intro/security-center.png
[3]: ./media/security-center-intro/security-policy.png
[4]: ./media/security-center-intro/security-policy-blade.png
[5]: ./media/security-center-intro/recommendations.png
[6]: ./media/security-center-intro/resources-health.png
[7]: ./media/security-center-intro/security-alert.png
[8]: ./media/security-center-intro/security-alert-detail.png
[9]: ./media/security-center-intro/partner-solutions.png



<!----HONumber=Jun16_HO2-->


