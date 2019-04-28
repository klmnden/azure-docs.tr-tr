---
title: Makineleri ve Azure Güvenlik Merkezi'nde uygulamalarınızı koruma | Microsoft Docs
description: Bu belgede ele yardımcı olacak öneriler Güvenlik Merkezi'nde sanal makinelerinizi ve bilgisayarlar ve web uygulamaları ve App Service ortamları koruma.
services: security-center
documentationcenter: na
author: monhaber
manager: barbkess
editor: ''
ms.assetid: 47fa1f76-683d-4230-b4ed-d123fef9a3e8
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/20/2019
ms.author: monhaber
ms.openlocfilehash: fa664952f3eb7d6f9e611fb87a9e484e97f388a2
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62121824"
---
# <a name="protecting-your-machines-and-applications-in-azure-security-center"></a>Makineleri ve Azure Güvenlik Merkezi'nde uygulamalarınızı koruma
Azure Güvenlik Merkezi, Azure kaynakları, Azure dışı sunucuların ve sanal makinelerin güvenlik durumunu analiz eder. Güvenlik Merkezi olası güvenlik açıklarını belirlediğinde, gerekli denetimlerin yapılandırılması işlemi boyunca size rehberlik öneriler oluşturur. Öneriler, Azure kaynak türleri için geçerlidir: sanal makineleri (VM'ler) ve bilgisayarlar, uygulamalar, ağ, SQL ve kimlik ve erişim.

Bu makalede makineleri ve uygulamaları için uygulama önerileri ele alır.

## <a name="monitoring-security-health"></a>Güvenlik durumunu izleme
Kaynaklarınızın güvenlik durumunu izleyebilirsiniz **Güvenlik Merkezi-genel bakış** Pano. **Kaynakları** bölüm, her kaynak türü için tanımlanan sorunların sayısını ve güvenlik durumunu sağlar.

Seçerek, tüm sorunların bir listesini görüntüleyebilirsiniz **önerileri**. Önerileri uygulama hakkında daha fazla bilgi için bkz. [Azure Güvenlik Merkezi'nde güvenlik önerilerini uygulama](security-center-recommendations.md).

Tam listesi işlem ve App services önerileri için bkz: [önerileri](security-center-virtual-machine-recommendations.md).

Devam etmek için seçin **işlem ve uygulamalar** altında **kaynakları** veya Güvenlik Merkezi ana menüsünde.
![Güvenlik Merkezi Panosu](./media/security-center-virtual-machine-recommendations/overview.png)

## <a name="monitor-compute-and-app-services"></a>İşlem ve uygulama hizmetlerini izleme
Altında **işlem ve uygulamalar**, aşağıdaki sekmeleri vardır:

- **Genel Bakış**: Güvenlik Merkezi tarafından belirlenen izleme ve öneriler.
- **Sanal makineler ve bilgisayarlar**: Sanal makinelerinizin, bilgisayarlarınızın ve her birinin geçerli güvenlik durumunun listesi.
- **Cloud Services**: Güvenlik Merkezi tarafından izlenen web ve çalışan rollerinizin listesi.
- **Uygulama Hizmetleri**: App service ortamları ve her birinin geçerli güvenlik durumunu listesi.
- **Kapsayıcılar (Önizleme)**: Iaas Linux makineleri ve Docker yapılandırmalarına güvenlik değerlendirmesini üzerinde barındırılan kapsayıcıları listesi.
- **İşlem kaynakları (Önizleme)**: Service Fabric kümeleri ile Event hubs gibi işlem kaynaklarınız için önerilerin bir listesi.

Devam etmek için seçin **işlem ve uygulamalar** altında **kaynak güvenlik sağlığı**.

![İşlem](./media/security-center-virtual-machine-recommendations/compute.png)

Her sekmede birden fazla bölüm olabilir ve her bölümde, sorunu çözmek üzere önerilen adımlarla ilgili daha fazla ayrıntı görmek için ayrı ayrı seçenekler belirleyebilirsiniz.

### İzlenmeyen VM'ler ve bilgisayarlar <a name="unmonitored-vms-and-computers"></a>
Microsoft Monitoring Agent uzantısını makine çalışmıyorsa bir VM'yi veya bilgisayarı Güvenlik Merkezi tarafından izlenmez. Bir yerel aracı zaten yüklü bir makineye sahip olabilir, örneğin OMS aracısı veya System Center Operations Manager aracısının doğrudan. Bu aracıları makinelerle olarak tanımlandığında bu aracılar Güvenlik Merkezi'nde tam olarak desteklenmediğinden izlenmeyen. Güvenlik Merkezi’nin tüm özelliklerinden tam olarak faydalanmak için, Microsoft Monitoring Agent uzantısı gereklidir.

İzlenmeyen VM'de veya bilgisayarda zaten yüklü olan yerel aracıya ek olarak, uzantı yükleyebilirsiniz. İki aracıyı da aynı çalışma alanına bağlayarak aynı şekilde yapılandırın. Bu, Güvenlik Merkezi’nin Microsoft Monitoring Agent uzantısıyla etkileşim kurup veri toplamasını sağlar. Microsoft Monitoring Agent uzantısını nasıl yükleyeceğiniz hakkında yönergeler için bkz. [VM uzantısını etkinleştir](../azure-monitor/learn/quick-collect-azurevm.md).

Güvenlik Merkezi’nin otomatik hazırlama için başlatılan VM’leri ve bilgisayarları başarılı bir şekilde izleyememe nedenleri hakkında daha fazla bilgi edinmek için bkz. [Aracı durumu sorunlarını izleme](security-center-troubleshooting-guide.md#mon-agent).

### <a name="recommendations"></a>Öneriler
Bu bölümde, her VM ve bilgisayar, web ve çalışan rolleri, Azure App Service Web Apps ve Güvenlik Merkezi'nin izlediği Azure App Service ortamı için önerilerin kümesine sahiptir. İlk sütunda öneriler listelenmiştir. İkinci sütunda, bu önerinin etkilediği kaynakların toplam sayısı gösterilmektedir. Üçüncü sütunda, sorunun önem derecesini gösterir.

Her önerinin gerçekleştirebileceğiniz bir eylemler kümesi bulunur seçtikten sonra. Örneğin, **eksik sistem güncelleştirmeleri**, Vm'leri ve yamaları eksik olan bilgisayarların sayısını ve eksik güncelleştirmenin önem görünür.

**Sistem güncelleştirmelerini uygulayın** grafik biçiminde, bir Windows ve Linux için kritik güncelleştirmelerin özeti vardır. İkinci bölümde, aşağıdaki bilgileri içeren bir tablo vardır:

- **AD**: Eksik güncelleştirmenin adıdır.
- **VM VM'ler ve BİLGİSAYARLARIN**: VM'ler ve bu güncelleştirmenin eksik olduğu bilgisayarların toplam sayısı.
- **GÜNCELLEŞTİRME ÖNEM DERECESİ**: Belirli bir önerinin önem açıklanmaktadır:

    - **Kritik**: Bir güvenlik açığı, anlamlı bir kaynakta (uygulama, sanal makine veya ağ güvenlik grubu) var ve ilgilenilmesi gerekiyor.
    - **Önemli**: Bir işlemin tamamlanması veya bir güvenlik açığını ortadan kaldırmak için kritik olmayan veya ek adımlar gerekir.
    - **Orta**: Bir güvenlik açığının ele alınması gerekiyor ancak hemen ilgilenilmesi gerekmiyor. (Varsayılan olarak, düşük öneriler sunulmaz ancak bunları görüntülemek istiyorsanız düşük öneriler filtresini kullanabilirsiniz.)


- **DURUM**: Önerinin geçerli durumu:

    - **Açık**: Öneri henüz ele alınmadı.
    - **Devam eden**: Öneri şu anda bu kaynaklara uygulanıyor ve herhangi bir işlem yapmanıza gerek yoktur.
    - **Çözümlenen**: Öneri zaten tamamlandı. (Sorun çözüldüğünde girdi soluklaşır).

Öneri ayrıntılarını görüntülemek için listeden eksik güncelleştirmenin adına tıklayın.


> [!NOTE]
> Buradaki güvenlik önerileri altında aynıdır **önerileri** Döşe. Bkz: [Azure Güvenlik Merkezi'nde güvenlik önerilerini uygulama](security-center-recommendations.md) önerileri çözümleme hakkında daha fazla bilgi için.
>
>

### <a name="vms-and-computers"></a>VM'ler ve bilgisayarlar
VM'ler ve bilgisayarlar bölümü, tüm VM ve bilgisayar önerilerini genel bir bakış sağlar. Her sütunda bir dizi öneri sunulur.

![VM ve bilgisayar önerileri](./media/security-center-virtual-machine-recommendations/vm-computers.png)

Simgeler bu listede gösterilen dört tür vardır:

![Azure olmayan bilgisayar](./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon1.png) Azure olmayan bilgisayar.

![Azure Resource Manager VM](./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon2.png) Azure Resource Manager sanal makinesi.

![Azure Klasik VM](./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon3.png) Azure Klasik VM.


![Vm'leri çalışma alanından tanımlanır](./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon4.png) VM'ler yalnızca görüntülenen abonelik parçası olan çalışma alanından tanımlanır. Bu söz konusu raporun diğer aboneliklere ait Vm'leri çalışma alanında, bu abonelik ve doğrudan Operations Manager Aracısı ile yüklenmiş olan Vm'leri içerir ve kaynak kimliği olmayan

Her önerinin altında görüntülenen simge VM ile dikkat ve önerinin türünü gerektiren bilgisayar hızlıca tanımlamanıza yardımcı olur. Göre listesi aramak için filtreleri kullanabilirsiniz **kaynak türü** ve **önem derecesi**.

Her VM için güvenlik önerilerini incelemek için VM'ye tıklayın.
Burada VM'nin veya bilgisayar için güvenlik ayrıntılarını görürsünüz. En altta, sorunlar için önerilen eylemleri ve sorunların önem derecesini görebilirsiniz.
![Bulut hizmetleri](./media/security-center-virtual-machine-recommendations/recommendation-list.png)

### <a name="cloud-services"></a>Bulut hizmetleri
İşletim sistemi sürümü güncel olmadığında, bulut Hizmetleri için bir öneri oluşturulur.

![Bulut hizmetleri](./media/security-center-virtual-machine-recommendations/security-center-monitoring-fig1-new006-2017.png)

(Bu durum önceki örnek için geçerli değildir) bir öneri sahip olduğu bir senaryoda, işletim sistemi sürümünü güncelleştirmek için Önerideki adımları izlemeniz gerekir. Bir güncelleştirme mevcut olduğunda uyarı alırsınız (sorunun önem derecesine bağlı olarak kırmızı veya turuncu). Seçtiğinizde, bu uyarı webrole1 (Windows Server çalıştırır web uygulamanızı IIS'e otomatik olarak) veya WorkerRole1 (Windows Server çalıştırır web uygulamanızı IIS'e otomatik olarak), satır bu öneriye ilişkin daha fazla ayrıntı görürsünüz.

Bu öneriyle ilgili daha kesin bir açıklama görmek için **AÇIKLAMA** sütunu altındaki **İşletim sistemi sürümünü güncelleştir**’e tıklayın.



![İşletim sistemi sürümünü güncelleştirme](./media/security-center-virtual-machine-recommendations/security-center-monitoring-fig8-new4.png)

### <a name="app-services"></a>Uygulama hizmetleri
App Service bilgileri görüntülemek için aboneliğinize App Service etkinleştirmeniz gerekir. Bu özellik etkinleştirme hakkında yönergeler için bkz: [Azure Güvenlik Merkezi ile App Service'ı koruma](security-center-app-services.md).
[!NOTE]
> App Service'ı izleme önizlemede ve Güvenlik Merkezi'nin standart katmanında yalnızca kullanılabilir.


Altında **uygulama hizmetleri**, App service ortamları listesini bulmak ve Güvenlik Merkezi değerlendirmesini temel alan durum özeti gerçekleştirilir.

![Uygulama hizmetleri](./media/security-center-virtual-machine-recommendations/app-services.png)

Üç tür bu listede gösterilen simge vardır:

![App services ortamı](./media/security-center-virtual-machine-recommendations/ase.png) App services ortamı.

![Web uygulaması](./media/security-center-virtual-machine-recommendations/web-app.png) Web uygulaması.

![İşlev uygulaması](./media/security-center-virtual-machine-recommendations/function-app.png) İşlev uygulaması.

1. Bir web uygulaması'nı seçin. Özet görünümü ile üç sekme açar:

   - **Öneriler**: Değerlendirme başarısız oldu Güvenlik Merkezi tarafından gerçekleştirilen göre.
   - **Değerlendirmeler geçirilen**: Güvenlik Merkezi tarafından geçirilen gerçekleştirdiği değerlendirmeler listesi.
   - **Kullanılamayan iç değerlendirmeler**: bir hata veya öneri nedeniyle çalıştırılamadı değerlendirmelerinin listesini belirli bir App service için uygun değil

   Altında **önerileri** Seçili web uygulaması için önerilerin bir listesi ve önerilerin önem derecesi.

   ![App Services önerileri](./media/security-center-virtual-machine-recommendations/app-services-rec.png)

2. Öneri açıklaması ve iyi durumda olmayan kaynaklar ve iyi durumdaki kaynaklar Taranmayan kaynaklar listesini görmek için bir öneri seçin.

   - Altında **değerlendirmeleri geçirilen** sütun geçilen iç değerlendirmeler listesi verilmiştir.  Bu değerlendirmeler önemini her zaman büyük/küçük harf yeşildir.

   - Geçirilen bir değerlendirme, listenin değerlendirme açıklaması için sağlıksız ve iyi durumda kaynakların bir listesini ve Taranmayan kaynaklar listesini seçin. İyi durumda olmayan kaynaklar için bir sekme yoktur ancak değerlendirmede başarılı olduğundan bu liste her zaman boştur.

     ![App Service düzeltme](./media/security-center-virtual-machine-recommendations/app-service-remediation.png)

## <a name="virtual-machine-scale-sets"></a>Sanal makine ölçek kümeleri
Güvenlik Merkezi, Ölçek kümesine sahiptir ve bu ölçek kümeleri Microsoft Monitoring Agent yüklemenizi önerir olup olmadığını otomatik olarak bulur. 

Microsoft Monitoring Agent'ı yüklemek için: 

1. Bir öneri seçin **sanal makine ölçek kümesi üzerinde izleme Aracısı yükleme**. İzlenmeyen ölçek kümeleri listesini al
2. Sağlıksız bir ölçek kümesi seçin. Mevcut bir doldurulmuş çalışma kullanarak izleme aracısını yüklemek için yönergeleri izleyin veya yeni bir tane oluşturun. Çalışma alanı ayarladığınızdan emin olun [fiyatlandırma katmanı](security-center-pricing.md) ayarlı değil ise.

   ![MMS yükleyin](./media/security-center-virtual-machine-recommendations/install-mms.png)

Ayarlamak istiyorsanız Yeni ölçeği otomatik olarak Microsoft Monitoring Agent'ı yüklemek için ayarlar:
1. Azure İlkesi'ne gidin ve tıklayın **tanımları**.
2. İlke arama **dağıtma Log Analytics aracısını Windows sanal makine ölçek kümeleri için** ve tıklayın.
3. **Ata**'ya tıklayın.
4. Ayarlama **kapsam** ve **Log Analytics çalışma alanı** tıklatıp **atama**.

Ölçek kümeleri, Azure İlkesi'nde Microsoft Monitoring Agent'ı yüklemek için tüm mevcut ayarlamak istiyorsanız gidin **düzeltme** ve mevcut ilkenin var olan ölçek kümeleri için geçerlidir.


## <a name="compute-and-app-recommendations"></a>İşlem ve uygulama önerileri
|Kaynak türü|Güvenlik puanı|Öneri|Açıklama|
|----|----|----|----|
|App Service|20|Web uygulaması yalnızca HTTPS üzerinden erişilebilir olmalıdır|Web uygulamalarının erişim yalnızca HTTPS üzerinden sınırlayın.|
|App Service|20|İşlev uygulaması yalnızca HTTPS üzerinden erişilebilir olmalıdır|İşlev uygulamaları, erişim yalnızca HTTPS üzerinden sınırlayın.|
|App Service|5|App Service'te tanılama günlüklerini etkinleştirme|Günlükleri etkinleştirmek ve bunları bir yıla kadar korur. Bu, etkinlik kayıtlarını araştırma amacıyla bir güvenlik olayı ortaya veya ağınızın tehlikeye yeniden oluşturmanıza olanak sağlar. |
|App Service|10|Uzaktan hata ayıklama için Web uygulaması kapalı olmaları|Artık bunu kullanmanız gerekiyorsa, Web uygulamaları için hata ayıklama devre dışı bırakın. Uzaktan hata ayıklama, gelen bağlantı noktası üzerinde bir işlev uygulaması açılmasını gerektirir.|
|App Service|10|Uzaktan hata ayıklama işlev uygulaması için kapatılmalıdır|Artık bunu kullanmanız gerekiyorsa, işlev uygulaması için hata ayıklama devre dışı bırakın. Uzaktan hata ayıklama, gelen bağlantı noktası üzerinde bir işlev uygulaması açılmasını gerektirir.|
|App Service|10|Web uygulaması için IP kısıtlamalarını yapılandırma|Uygulamanıza erişmek için izin verilen IP adreslerinin bir listesini tanımlar. IP kısıtlamaları kullanımı, bir web uygulaması genel saldırılara karşı korur.|
|App Service|10|Tüm izin verme ('* ') uygulamanıza erişmek için kaynaklar| WEBSITE_LOAD_CERTIFICATES parametre kümesi izin verme "". Parametresini ayarlayarak '' tüm sertifikalar, web uygulamaları kişisel sertifika deposuna yüklendiğini anlamına gelir. Site çalışma zamanında tüm sertifikalara erişim gerektiğini olası olmadığından bu en düşük öncelik ilkesini kötüye neden olabilir.|
|App Service|20|CORS her kaynağın Web uygulamalarınıza erişmek izin vermemelidir|Web uygulamanızla etkileşim kurmak yalnızca gerekli etki alanı sağlar. Kaynak kaynak paylaşımı (CORS) tüm etki alanları web uygulamanıza erişmek izin vermemelisiniz.|
|App Service|20|CORS her kaynağın işlev uygulamanıza erişmek izin vermemelidir| İşlev uygulamanızla etkileşim kurmak yalnızca gerekli etki alanı sağlar. Kaynak kaynak paylaşımı (CORS) tüm etki alanlarının işlev uygulamanıza erişmek izin vermemelisiniz.|
|İşlem kaynakları (toplu)|1|Batch hesabında ölçüm uyarı kuralları yapılandırma|Batch hesabında ölçüm uyarı kuralları yapılandırın ve havuzu Silme Tamamlandı olayları ve havuz silme başlangıç olayları ölçümlerini etkinleştir|
|İşlem kaynakları (service fabric)|10|Service fabric'te istemci kimlik doğrulaması için Azure Active Directory kullanın|İstemci kimlik doğrulaması yalnızca Azure Active Directory aracılığıyla Service Fabric'te gerçekleştirin.|
|İşlem kaynakları (Otomasyon hesabı)|5| Otomasyon hesabının şifrelemeyi etkinleştir|Otomasyon hesabı değişken varlıkları şifrelenmesi, hassas verileri depolarken etkinleştirin.|
|İşlem kaynakları (yük dengeleyici)|5|Yük dengeleyicide tanılama günlüklerini etkinleştirme|Günlükleri etkinleştirmek ve bunları bir yıla kadar korur. Bu, etkinlik kayıtlarını araştırma amacıyla bir güvenlik olayı ortaya veya ağınızın tehlikeye yeniden oluşturmanıza olanak sağlar. |
|İşlem kaynakları (arama)|5|Arama hizmetinde tanılama günlüklerini etkinleştirme|Günlükleri etkinleştirmek ve bunları bir yıla kadar korur. Bu, etkinlik kayıtlarını araştırma amacıyla bir güvenlik olayı ortaya veya ağınızın tehlikeye yeniden oluşturmanıza olanak sağlar. |
|İşlem kaynakları (hizmet veri yolu)|5|Hizmet veri yolu tanılama günlüklerini etkinleştirme|Günlükleri etkinleştirmek ve bunları bir yıla kadar korur. Bu, etkinlik kayıtlarını araştırma amacıyla bir güvenlik olayı ortaya veya ağınızın tehlikeye yeniden oluşturmanıza olanak sağlar. |
|İşlem kaynakları (akış analizi)|5|Azure Stream analytics'te tanılama günlüklerini etkinleştirme|Günlükleri etkinleştirmek ve bunları bir yıla kadar korur. Bu, etkinlik kayıtlarını araştırma amacıyla bir güvenlik olayı ortaya veya ağınızın tehlikeye yeniden oluşturmanıza olanak sağlar. |
|İşlem kaynakları (service fabric)|5|Service fabric'te tanılama günlüklerini etkinleştirme|Günlükleri etkinleştirmek ve bunları bir yıla kadar korur. Bu, etkinlik kayıtlarını araştırma amacıyla bir güvenlik olayı ortaya veya ağınızın tehlikeye yeniden oluşturmanıza olanak sağlar. |
|İşlem kaynakları (toplu)|5|Batch hesapları, tanılama günlüklerini etkinleştirme|Günlükleri etkinleştirmek ve bunları bir yıla kadar korur. Bu, etkinlik kayıtlarını araştırma amacıyla bir güvenlik olayı ortaya veya ağınızın tehlikeye yeniden oluşturmanıza olanak sağlar. |
|İşlem kaynakları (olay hub'ı)|5|Olay Hub'ındaki tanılama günlüklerini etkinleştirme|Günlükleri etkinleştirmek ve bunları bir yıla kadar korur. Bu, etkinlik kayıtlarını araştırma amacıyla bir güvenlik olayı ortaya veya ağınızın tehlikeye yeniden oluşturmanıza olanak sağlar. |
|İşlem kaynakları (mantıksal uygulamalar)|5|Logic apps'teki tanılama günlüklerini etkinleştirme|Günlükleri etkinleştirmek ve bunları bir yıla kadar korur. Bu, etkinlik kayıtlarını araştırma amacıyla bir güvenlik olayı ortaya veya ağınızın tehlikeye yeniden oluşturmanıza olanak sağlar. |
|İşlem kaynakları (service fabric)|15|Service fabric'te EncryptAndSign ClusterProtectionLevel özelliğini ayarlayın|Service Fabric, düğümden düğüme iletişim için bir birincil küme sertifikası kullanarak koruma (None, oturum ve EncryptAndSign) üç düzeyleri sağlar.  Tüm düğümler için iletileri şifrelenir ve dijital olarak imzalanmış emin olmak için koruma düzeyini ayarlayın. |
|İşlem kaynakları (hizmet veri yolu)|1|Service Bus ad alanındaki tüm yetkilendirme kurallarını RootManageSharedAccessKey dışında Kaldır |Hizmet veri yolu istemcileri tüm kuyrukları ve konuları ad alanında erişim sağlayan bir ad alanı düzeyinde erişim ilkesi kullanmamanız gerekir. En az ayrıcalık ile hizalamak için güvenlik modeli oluşturmanız gerektiğini erişim ilkelerini kuyruklar ve konular erişebilmesi için yalnızca belirli bir varlık için varlık düzeyinde.|
|İşlem kaynakları (olay hub'ı)|1|RootManageSharedAccessKey dışındaki tüm yetkilendirme kurallarını olay hub'ı ad alanından kaldırın |Olay hub'ı istemcilerin tüm kuyrukları ve konuları ad alanında erişim sağlayan bir ad alanı düzeyinde erişim ilkesi kullanmamanız gerekir. En az ayrıcalık ile hizalamak için güvenlik modeli oluşturmanız gerektiğini erişim ilkelerini kuyruklar ve konular erişebilmesi için yalnızca belirli bir varlık için varlık düzeyinde.|
|İşlem kaynakları (olay hub'ı)|5|Olay hub'ı varlıkta yetkilendirme kuralları tanımlayın|Yetkilendirme kuralları en az ayrıcalıklı erişim vermek için olay hub'ı varlık üzerinde denetim.|
|Makine|50|Makinelerinize izleme aracısı yükleyin|Veri toplama, tarama, temel tarama ve her makineye bir uç nokta koruma güncelleştirmeleri etkinleştirmek için izleme aracısını yükleyin.|
|Makine|50|Otomatik sağlama ve Abonelikleriniz için veri toplamayı etkinleştir |Otomatik sağlama ve veri toplamayı etkinleştirmek için aboneliklerinizi tarama, temel tarama ve aboneliklerinize eklenen her makineye bir uç nokta koruma güncelleştirmeleri makineler için veri toplamayı etkinleştirin.|
|Makine|40|Makinelerinizin izleme aracısı sistem durumu sorunlarını çözün|Tam Güvenlik Merkezi koruma için sorun giderme Kılavuzu'ndaki yönergeleri takip ederek makinelerinizdeki izleme aracı sorunlarını çözün.| 
|Makine|40|Makinelerinizin uç nokta koruması sistem durumu sorunlarını çözün|Tüm Güvenlik Merkezi koruma için sorun giderme Kılavuzu'ndaki yönergeleri takip ederek makinelerinizi izleme aracı sorunları çözün.|
|Makine|40|Makinelerinizde eksik tarama verileri sorunlarını giderin|Sanal makineler ve bilgisayarlar üzerinde eksik tarama verileri sorunlarını giderin. Tarama, temel tarama ve uç nokta koruma çözümü tarama eksik güvenlik değerlendirmeleri gibi eksik, makineleri sonuçları üzerinde eksik tarama verileri güncelleştirin.|
|Makine|40|Makinelerinize sistem güncelleştirmeleri yükleyin|Eksik sistem güvenliği ve güvenli Windows ve Linux sanal makineleri ve bilgisayarlar için kritik güncelleştirmeleri yükleyin
|Makine|15|Web uygulaması güvenlik duvarı ekleme| Web uygulamalarınızın güvenliğini sağlamak için bir web uygulaması Güvenlik Duvarı (WAF) çözümünü dağıtın. |
|Makine|40|Bulut hizmeti rolleriniz için işletim sistemi sürümünü güncelleştirin|Bulut hizmeti rolleriniz için işletim sistemi sürümünü, işletim sistemi ailenizdeki en yeni sürüm ile güncelleştirin.|
|Makine|35|Makinelerinizin güvenlik yapılandırmasındaki güvenlik açıklarını düzeltin|Güvenlik Yapılandırması saldırılarına karşı korunacak makinelerinizde güvenlik açıklarını düzeltin. |
|Makine|35|Kapsayıcılarınızı güvenlik yapılandırması, güvenlik açıklarını düzeltin|Docker yüklü makineleri korumak için bunların güvenlik yapılandırmasındaki güvenlik açıklarını düzeltin.|
|Makine|25|Uyarlamalı Uygulama Denetimlerini etkinleştirin|Uygulamalar, Azure'da yer alan Vm'leriniz üzerinde çalışabilen uygulama denetime etkinleştirin. Bu, sanal makinelerinizi kötü amaçlı yazılımlara karşı sağlamlaştırma yardımcı olur. Güvenlik Merkezi, her sanal makinede çalışan uygulamaların çözümlemek için machine learning kullanır ve bu bilgileri kullanarak kuralları yardımcı olur, uygulamanızı sağlar. Bu özellik yapılandırma işlemi basitleştirir ve izin verme kurallarını uygulama koruma.|
|Makine|20|Makinelerinize uç nokta koruma çözümü yükleyin|Tehditleri ve güvenlik açıklarından korumak için sanal makineler, bir uç nokta koruma çözümüne yükleyin.|
|Makine|20|Sistem güncelleştirmelerini uygulamak için makinelerinizi yeniden başlatın|Sistem güncelleştirmelerini uygulamak ve makinenizi güvenlik açıklarına karşı korumak için makinelerinizi yeniden başlatın.|
|Makine|15|Sanal makinelerinizde disk şifrelemesi Uygula|Windows ve Linux sanal makineleri için Azure Disk şifrelemesi hem kullanılarak sanal makine disklerinizi şifreleyin. Azure Disk şifrelemesi (ADE) sektörde standart BitLocker özelliğini Windows ve Linux korumak ve verilerinizi koruyun ve kurumsal güvenlik ve uyumluluk karşılamanıza yardımcı olmak için işletim sistemi ve veri disk şifreleme sağlamak için DM-Crypt özelliğini kullanır. Taahhüt müşteri Azure anahtar kasası. Ne zaman, uyumluluk ve güvenlik gereksinimleri, kullanım Azure disk şifrelemesi kısa ömürlü (yerel olarak bağlı geçici) disk şifreleme gibi şifreleme anahtarlarınızı kullanarak uca verileri şifrelemek gerektirir. Alternatif olarak, Microsoft tarafından yönetilen anahtarları azure'da şifreleme anahtarları nerede Azure depolama hizmeti şifrelemesi kullanarak varsayılan olarak varsayılan olarak, yönetilen diskler bekleme durumundayken şifrelenir. Bu, uyumluluk ve güvenlik gereksinimlerini karşılıyorsa, gereksinimlerinizi karşılamak için varsayılan yönetilen disk şifrelemesi yararlanabilirsiniz.|
|Makine|30|Sanal makinelerinize bir güvenlik açığı değerlendirme çözümü yükleyin|Sanal makinelerinize bir güvenlik açığı değerlendirme çözümü yükleyin|
|Makine|15|Web uygulaması güvenlik duvarı ekleme| Web uygulamalarınızın güvenliğini sağlamak için bir web uygulaması Güvenlik Duvarı (WAF) çözümünü dağıtın. |
|Makine|30|Bir güvenlik açığı değerlendirme çözümü kullanarak güvenlik açıklarını düzeltin|Kendisi için bir güvenlik açığı değerlendirme 3 taraf çözümü dağıtılan sanal makinelerin sürekli olarak uygulama ve işletim sistemi güvenlik açıklarını karşı incelenen. Tür güvenlik açıklarına bulunduğunda, bunlar öneri bir parçası olarak daha fazla bilgi için kullanılabilir.|
|Makine|30|Sanal makinelerinize bir güvenlik açığı değerlendirme çözümü yükleyin|Sanal makinelerinize bir güvenlik açığı değerlendirme çözümü yükleyin|
|Makine|1|Yeni Azure Resource Manager kaynaklarına sanal makineleri geçirme|Azure Resource Manager sanal makineleriniz için gibi güvenlik geliştirmeleri sağlamak için kullanın: yönetilen kimlik, anahtar kasası için erişim gizli dizileri, daha güçlü erişim denetimi (RBAC), daha iyi denetim, Resource Manager tabanlı bir dağıtım ve yönetim erişim Azure AD tabanlı kimlik doğrulaması ve etiketleri için destek ve daha kolay güvenlik yönetimi için kaynak grupları. |
|Makine|30|Bir güvenlik açığı değerlendirme çözümü kullanarak güvenlik açıklarını düzeltin|Kendisi için bir güvenlik açığı değerlendirme 3 taraf çözümü dağıtılan sanal makinelerin sürekli olarak uygulama ve işletim sistemi güvenlik açıklarını karşı incelenen. Tür güvenlik açıklarına bulunduğunda, bunlar öneri bir parçası olarak daha fazla bilgi için kullanılabilir.|
|Sanal makine ölçek kümesi |4|Sanal Makine Ölçek Kümelerinde tanılama günlüklerini etkinleştirme|Günlükleri etkinleştirmek ve bunlar için bir yıla kadar Beklet. Bu, araştırma amacıyla etkinlik kayıtlarını yeniden oluşturmanıza olanak sağlar. Bu bir güvenlik olayı ortaya veya ağınızın tehlikeye yararlı olur.|
|Sanal makine ölçek kümesi|35|Sanal makine ölçek kümelerinizin güvenlik yapılandırmasındaki güvenlik açıklarını düzeltme|Sanal makine ölçek kümelerinizi saldırılardan korumak için güvenlik yapılandırmasındaki güvenlik açıklarını düzeltin. |
|Sanal makine ölçek kümesi|5|Sanal makine ölçek kümelerinde uç nokta koruması sistem durumu hatalarını düzeltme|Sanal makine ölçek kümelerinizi tehdit ve güvenlik açıklarından korumak için uç nokta koruma sistem durumu hatalarını düzeltin. |
|Sanal makine ölçek kümesi|10|Sanal makine ölçek kümelerine uç nokta koruma çözümü yükleme|Tehditleri ve güvenlik açıklarından korumak için sanal makine ölçek kümeleri, bir uç nokta koruma çözümüne yükleyin. |
|Sanal makine ölçek kümesi|40|Sanal makine ölçek kümelerine sistem güncelleştirmelerini yükleme|Windows ve Linux sanal makine ölçek kümelerinizi korumak için eksik sistem güvenliği güncelleştirmelerini ve diğer kritik güncelleştirmeleri yükleyin. |
 





## <a name="next-steps"></a>Sonraki adımlar
Diğer Azure kaynak türü için geçerli öneriler hakkında daha fazla bilgi için aşağıdakilere bakın:


* [Sanal makineler için Azure Güvenlik Merkezi'ni anlama önerileri](security-center-virtual-machine-recommendations.md)
* [Kimlik ve erişim Azure Güvenlik Merkezi'nde izleme](security-center-identity-access.md)
* [Azure Güvenlik Merkezi'nde ağınızı koruma](security-center-network-recommendations.md)
* [Azure Güvenlik Merkezi'nde Azure SQL hizmetinizi koruma](security-center-sql-service-recommendations.md)

Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](tutorial-security-policy.md) -- Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md) -- Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) -- Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.

