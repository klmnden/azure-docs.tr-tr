---
title: Makineleri ve Azure Güvenlik Merkezi'nde uygulamalarınızı koruma | Microsoft Docs
description: Bu belgede ele yardımcı olacak öneriler Güvenlik Merkezi'nde sanal makinelerinizi ve bilgisayarlar ve web uygulamaları ve App Service ortamları koruma.
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: 47fa1f76-683d-4230-b4ed-d123fef9a3e8
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/28/2018
ms.author: rkarlin
ms.openlocfilehash: d1e7046ed1fc87a85a08b21a48accaaf3b5087b9
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/26/2018
ms.locfileid: "52316659"
---
# <a name="protecting-your-machines-and-applications-in-azure-security-center"></a>Makineleri ve Azure Güvenlik Merkezi'nde uygulamalarınızı koruma
Azure Güvenlik Merkezi, Azure kaynaklarınızın güvenlik durumunu analiz eder. Güvenlik Merkezi olası güvenlik açıklarını belirlediğinde, gerekli denetimlerin yapılandırılması işlemi boyunca size rehberlik öneriler oluşturur. Öneriler, Azure kaynak türleri için geçerlidir: sanal makineleri (VM'ler) ve bilgisayarlar, uygulamalar, ağ, SQL ve kimlik ve erişim.

Bu makalede makineleri ve uygulamaları için uygulama önerileri ele alır.

## <a name="monitoring-security-health"></a>Güvenlik durumunu izleme
Kaynaklarınızın güvenlik durumunu izleyebilirsiniz **Güvenlik Merkezi-genel bakış** Pano. **Kaynakları** bölüm, her kaynak türü için tanımlanan sorunların sayısını ve güvenlik durumunu sağlar.

Seçerek, tüm sorunların bir listesini görüntüleyebilirsiniz **önerileri**. Önerileri uygulama hakkında daha fazla bilgi için bkz. [Azure Güvenlik Merkezi'nde güvenlik önerilerini uygulama](security-center-recommendations.md).

Tam listesi işlem ve App services önerileri için bkz: [önerileri](security-center-virtual-machine-recommendations.md).

Devam etmek için seçin **işlem ve uygulamalar** altında **kaynakları** veya Güvenlik Merkezi ana menüsünde.
![Güvenlik Merkezi Panosu][1]

## <a name="monitor-compute-and-app-services"></a>İzleyici işlemi ve uygulama hizmetleri
**İşlem** bölümünde dört sekme vardır:

- **Genel Bakış**: Güvenlik Merkezi tarafından belirlenen izleme ve öneriler.
- **Sanal makineler ve bilgisayarlar**: Sanal makinelerinizin, bilgisayarlarınızın ve her birinin geçerli güvenlik durumunun listesi.
- **Cloud Services**: Güvenlik Merkezi tarafından izlenen web ve çalışan rollerinizin listesi.
- **Uygulama Hizmetleri (Önizleme)**: App service ortamları ve her birinin geçerli güvenlik durumunu listesi.
Devam etmek için seçin **işlem ve uygulamalar** altında **kaynakları** veya Güvenlik Merkezi ana menüsünde.

![İşlem][2]

Her sekmede birden fazla bölüm olabilir ve her bölümde, sorunu çözmek üzere önerilen adımlarla ilgili daha fazla ayrıntı görmek için ayrı ayrı seçenekler belirleyebilirsiniz.

### <a name="monitoring-recommendations"></a>İzleme önerileri
Bu bölümde, VM'ler ve bilgisayarların otomatik sağlama ve bunların geçerli durumu için başlatılan toplam sayısını gösterir. Bu örnekte şu öneri mevcuttur: **Aracı sistem durumu sorunlarını izleme**. Bu öneriyi seçin.

![Aracı sistem durumu sorunlarını izleme][3]

**Aracı sistem durumu sorunlarını izleme** açılır. Güvenlik Merkezi'nin başarılı bir şekilde izleyemediği VM'ler ve bilgisayarlar listelenir. Ayrıntılı bilgi için bir VM'yi veya bilgisayarı seçin. **İZLEME DURUMU**, Güvenlik Merkezi’nin izlemeyi neden yapamadığını belirtir. **İZLEME DURUMU** değerleri, açıklamaları ve çözüm adımlarının listesi için bkz. [Güvenlik Merkezi sorun giderme kılavuzu](security-center-troubleshooting-guide.md).

### İzlenmeyen VM'ler ve bilgisayarlar <a name="unmonitored-vms-and-computers"></a>
Microsoft Monitoring Agent uzantısını makine çalışmıyorsa bir VM'yi veya bilgisayarı Güvenlik Merkezi tarafından izlenmez. Bir yerel aracı zaten yüklü bir makineye sahip olabilir, örneğin OMS aracısı veya SCOM Aracısı doğrudan. Bu aracıları makinelerle olarak tanımlandığında bu aracılar Güvenlik Merkezi'nde tam olarak desteklenmediğinden izlenmeyen. Güvenlik Merkezi’nin tüm özelliklerinden tam olarak faydalanmak için, Microsoft Monitoring Agent uzantısı gereklidir.

İzlenmeyen VM'de veya bilgisayarda zaten yüklü olan yerel aracıya ek olarak, uzantı yükleyebilirsiniz. İki aracıyı da aynı çalışma alanına bağlayarak aynı şekilde yapılandırın. Bu, Güvenlik Merkezi’nin Microsoft Monitoring Agent uzantısıyla etkileşim kurup veri toplamasını sağlar. Microsoft Monitoring Agent uzantısını nasıl yükleyeceğiniz hakkında yönergeler için bkz. [VM uzantısını etkinleştir](../log-analytics/log-analytics-quick-collect-azurevm.md).

Güvenlik Merkezi’nin otomatik hazırlama için başlatılan VM’leri ve bilgisayarları başarılı bir şekilde izleyememe nedenleri hakkında daha fazla bilgi edinmek için bkz. [Aracı durumu sorunlarını izleme](security-center-troubleshooting-guide.md#mon-agent).

### <a name="recommendations"></a>Öneriler
Bu bölümde, her VM ve bilgisayar, web ve çalışan rolleri, Azure App Service Web Apps ve Güvenlik Merkezi'nin izlediği Azure App Service ortamı için önerilerin kümesine sahiptir. İlk sütunda öneriler listelenmiştir. İkinci sütunda, bu önerinin etkilediği kaynakların toplam sayısı gösterilmektedir. Üçüncü sütunda, aşağıdaki ekran görüntüsünde gösterildiği gibi sorunun önem derecesi belirtilmiştir.

![Öneriler][4]

Her önerinin gerçekleştirebileceğiniz bir eylemler kümesi bulunur seçtikten sonra. Örneğin, **eksik sistem güncelleştirmeleri**, Vm'leri ve yamaları eksik olan bilgisayarların sayısını ve eksik güncelleştirmenin önem görünür, aşağıdaki ekran görüntüsünde gösterildiği gibi:

![Sistem güncelleştirmelerini uygulayın][5]

**Sistem güncelleştirmelerini uygulayın** grafik biçiminde, bir Windows ve Linux için kritik güncelleştirmelerin özeti vardır. İkinci bölümde, aşağıdaki bilgileri içeren bir tablo vardır:

- **AD**: Eksik güncelleştirmenin adıdır.
- **VM’LER VE BİLGİSAYARLARIN SAYISI**: Bu güncelleştirmenin eksik olduğu VM'ler ve bilgisayarların toplam sayısı.
- **Güncelleştirme önem DERECESİ**: belirli bir önerinin önem açıklar:

    - **Kritik**: bir güvenlik açığı anlamlı bir kaynakta (uygulama, sanal makine veya ağ güvenlik grubu) var ve ilgilenilmesi gerekiyor.
    - **Önemli**: kritik olmayan veya ek adımlar, bir işlemin tamamlanması veya bir güvenlik açığını ortadan kaldırmak için gereklidir.
    - **Orta**: bir güvenlik açığının ele alınması gerekiyor ancak hemen ilgilenilmesi gerekmiyor. (Varsayılan olarak, düşük öneriler sunulmaz ancak bunları görüntülemek istiyorsanız düşük öneriler filtresini kullanabilirsiniz.)


- **DURUM**: Önerinin geçerli durumu:

    - **Açık**: Öneri henüz ele alınmadı.
    - **Devam Ediyor**: Öneri şu anda bu kaynaklara uygulanıyor; sizin bir eylem yapmanıza gerek yok.
    - **Çözüldü**: Öneri daha önce tamamlandı. (Sorun çözüldüğünde girdi soluklaşır).

Öneri ayrıntılarını görüntülemek için listeden eksik güncelleştirmenin adına tıklayın.

![Öneri ayrıntıları][6]

> [!NOTE]
> Buradaki güvenlik önerileri altında aynıdır **önerileri** Döşe. Bkz: [Azure Güvenlik Merkezi'nde güvenlik önerilerini uygulama](security-center-recommendations.md) önerileri çözümleme hakkında daha fazla bilgi için.
>
>

### <a name="vms-and-computers"></a>VM'ler ve bilgisayarlar
VM'ler ve bilgisayarlar bölümü, tüm VM ve bilgisayar önerilerini genel bir bakış sağlar. Her sütun, aşağıdaki ekran görüntüsünde gösterildiği gibi bir dizi öneriyi temsil eder:

![VM ve bilgisayar önerileri][7]

Simgeler bu listede gösterilen dört tür vardır:

![Azure olmayan bilgisayar][8] Azure olmayan bilgisayar.

![Azure Resource Manager VM][9] Azure Resource Manager sanal makinesi.

![Azure Klasik VM][10] Azure Klasik VM.

![Vm'leri çalışma alanından tanımlanır][11] VM'ler yalnızca görüntülenen abonelik parçası olan çalışma alanından tanımlanır. Buna diğer aboneliklere ait ancak bu çalışma alanına bildirimde bulunan sanal makineler, SCOM doğrudan aracısıyla yüklenen ve kaynak kimliği olmayan VM’ler dahildir.

Her önerinin altında görüntülenen simge VM ile dikkat ve önerinin türünü gerektiren bilgisayar hızlıca tanımlamanıza yardımcı olur. Göreceğiniz bu ekranda hangi seçenekleri seçmek için filtre seçeneğini de kullanabilirsiniz.

![Filtre][12]

Önceki örnekte, bir VM'nin uç nokta koruması ile ilgili kritik bir önerisi vardır. Daha fazla bilgi almak için VM'yi seçin:

![Kritik bir önerisi][13]

Burada VM'nin veya bilgisayar için güvenlik ayrıntılarını görürsünüz. En altta, sorunlar için önerilen eylemleri ve sorunların önem derecesini görebilirsiniz.

### <a name="cloud-services"></a>Bulut hizmetleri
Bulut hizmetleri için işletim sistemi sürümü güncel olmadığında aşağıdaki ekran görüntüsünde gösterildiği gibi bir öneri oluşturulur:

![Bulut hizmetleri][14]

(Bu durum önceki örnek için geçerli değildir) bir öneri sahip olduğu bir senaryoda, işletim sistemi sürümünü güncelleştirmek için Önerideki adımları izlemeniz gerekir. Bir güncelleştirme mevcut olduğunda uyarı alırsınız (sorunun önem derecesine bağlı olarak kırmızı veya turuncu). Seçtiğinizde, bu uyarı webrole1 (Windows Server çalıştırır web uygulamanızı IIS'e otomatik olarak) veya WorkerRole1 (Windows Server çalıştırır web uygulamanızı IIS'e otomatik olarak), satır gösterildiği gibi bu öneriye ilişkin daha fazla ayrıntı görürsünüz Aşağıdaki ekran görüntüsüne bakın:

![WorkerRole1][15]

Bu öneriyle ilgili daha kesin bir açıklama görmek için **AÇIKLAMA** sütunu altındaki **İşletim sistemi sürümünü güncelleştir**’e tıklayın.

![İşletim sistemi sürümünü güncelleştirme][16]

### <a name="app-services-preview"></a>Uygulama hizmetleri (Önizleme)

> [!NOTE]
> App Service'ı izleme önizlemede ve Güvenlik Merkezi'nin standart katmanında yalnızca kullanılabilir. Güvenlik Merkezi’nin fiyatlandırma katmanları hakkında daha fazla bilgi almak için bkz. [Fiyatlandırma](security-center-pricing.md).
>
>

Altında **uygulama hizmetleri**, App service ortamları listesini bulmak ve Güvenlik Merkezi değerlendirmesini temel alan durum özeti gerçekleştirilir.

![Uygulama hizmetleri][17]

Üç tür bu listede gösterilen simge vardır:

![App services ortamı][18] App services ortamı.

![Web uygulaması][19] Web uygulaması.

![İşlev uygulaması][24] İşlev uygulaması.

1. Bir web uygulaması'nı seçin. Özet görünümü ile üç sekme açar:

  - **Öneriler**: Değerlendirme başarısız oldu Güvenlik Merkezi tarafından gerçekleştirilen göre.
  - **Değerlendirmeler geçirilen**: Güvenlik Merkezi tarafından geçirilen gerçekleştirdiği değerlendirmeler listesi.
  - **Kullanılamayan iç değerlendirmeler**: bir hata veya öneri nedeniyle çalıştırılamadı değerlendirmelerinin listesini belirli bir App service için uygun değil

  Altında **önerileri** Seçili web uygulaması için önerilerin bir listesi ve önerilerin önem derecesi.

  ![Özet görünümü][20]

2. Öneri iyi durumda olmayan kaynaklar ve iyi durumdaki kaynaklar Taranmayan kaynaklar listesi ve açıklaması için bir öneri seçin.

  ![Öneri açıklaması][21]

  Altında **değerlendirmeleri geçirilen** geçilen iç değerlendirmeler listesidir.  Bu değerlendirmeler önemini her zaman büyük/küçük harf yeşildir.

  ![Geçilen iç değerlendirmeler][22]

3. Geçirilen bir değerlendirme, listenin değerlendirme açıklaması için sağlıksız ve iyi durumda kaynakların bir listesini ve Taranmayan kaynaklar listesini seçin. İyi durumda olmayan kaynaklar için bir sekme yoktur ancak değerlendirmede başarılı olduğundan bu liste her zaman boştur.

    ![İyi durumdaki kaynaklar][23]

## <a name="compute-and-app-recommendations"></a>İşlem ve uygulama önerileri
|Kaynak türü|Güvenlik puanı|Öneri|Açıklama|
|----|----|----|----|
|Makine|50|Makinelerinize izleme aracısı yükleyin|Veri toplama, tarama, temel tarama ve her makineye bir uç nokta koruma güncelleştirmeleri etkinleştirmek için izleme aracısını yükleyin.|
|Makine|50|Otomatik sağlama ve Abonelikleriniz için veri toplamayı etkinleştir |Otomatik sağlama ve veri toplamayı etkinleştirmek için aboneliklerinizi tarama, temel tarama ve aboneliklerinize eklenen her makineye bir uç nokta koruma güncelleştirmeleri makineler için veri toplamayı etkinleştirin.|
|Makine|40|Makinelerinizin izleme aracısı sistem durumu sorunlarını çözün|Tam Güvenlik Merkezi koruma için sorun giderme Kılavuzu'ndaki yönergeleri takip ederek makinelerinizdeki izleme aracı sorunlarını çözün.| 
|Makine|40|Makinelerinizin uç nokta koruması sistem durumu sorunlarını çözün|Tüm Güvenlik Merkezi koruma için sorun giderme Kılavuzu'ndaki yönergeleri takip ederek makinelerinizi izleme aracı sorunları çözün.|
|Makine|40|Makinelerinizde eksik tarama verileri sorunlarını giderin|Sanal makineler ve bilgisayarlar üzerinde eksik tarama verileri sorunlarını giderin. Tarama, temel tarama ve uç nokta koruma çözümü tarama eksik güvenlik değerlendirmeleri gibi eksik, makineleri sonuçları üzerinde eksik tarama verileri güncelleştirin.|
|Makine|40|Makinelerinize sistem güncelleştirmeleri yükleyin|Eksik sistem güvenliği ve güvenli Windows ve Linux sanal makineleri ve bilgisayarlar için kritik güncelleştirmeleri yükleyin
|Makine|40|Bulut hizmeti rolleriniz için işletim sistemi sürümünü güncelleştirin|Bulut hizmeti rolleriniz için işletim sistemi sürümünü, işletim sistemi ailenizdeki en yeni sürüm ile güncelleştirin.|
|Makine|35|Makinelerinizin güvenlik yapılandırmasındaki güvenlik açıklarını düzeltin|Güvenlik Yapılandırması saldırılarına karşı korunacak makinelerinizde güvenlik açıklarını düzeltin. |
|Makine|35|Kapsayıcılarınızı güvenlik yapılandırması, güvenlik açıklarını düzeltin|Docker yüklü makineleri korumak için bunların güvenlik yapılandırmasındaki güvenlik açıklarını düzeltin.|
|Makine|25|Uyarlamalı Uygulama Denetimlerini etkinleştirin|Uygulamalar, Azure'da yer alan Vm'leriniz üzerinde çalışabilen uygulama denetime etkinleştirin. Bu, sanal makinelerinizi kötü amaçlı yazılımlara karşı sağlamlaştırma yardımcı olur. Güvenlik Merkezi, her sanal makinede çalışan uygulamaların çözümlemek için machine learning kullanır ve bu bilgileri kullanarak kuralları yardımcı olur, uygulamanızı sağlar. Bu özellik yapılandırma işlemi basitleştirir ve izin verme kurallarını uygulama koruma.|
|Makine|20|Makinelerinize uç nokta koruma çözümü yükleyin|Tehditleri ve güvenlik açıklarından korumak için sanal makineler, bir uç nokta koruma çözümüne yükleyin.|
|Makine|20|Sistem güncelleştirmelerini uygulamak için makinelerinizi yeniden başlatın|Sistem güncelleştirmelerini uygulamak ve makinenizi güvenlik açıklarına karşı korumak için makinelerinizi yeniden başlatın.|
|App Service|20|Web uygulaması yalnızca HTTPS üzerinden erişilebilir olmalıdır|Web uygulamalarının erişim yalnızca HTTPS üzerinden sınırlayın.|
|App Service|20|İşlev uygulaması yalnızca HTTPS üzerinden erişilebilir olmalıdır|İşlev uygulamaları, erişim yalnızca HTTPS üzerinden sınırlayın.|
|Makine|15|Sanal makinelerinizde disk şifrelemesi Uygula|Windows ve Linux sanal makineleri için Azure Disk şifrelemesi hem kullanılarak sanal makine disklerinizi şifreleyin. Azure Disk şifrelemesi (ADE) sektörde standart BitLocker özelliğini Windows ve Linux korumak ve verilerinizi koruyun ve kurumsal güvenlik ve uyumluluk karşılamanıza yardımcı olmak için işletim sistemi ve veri disk şifreleme sağlamak için DM-Crypt özelliğini kullanır. Taahhüt müşteri Azure anahtar kasası. Ne zaman, uyumluluk ve güvenlik gereksinimleri, kullanım Azure disk şifrelemesi kısa ömürlü (yerel olarak bağlı geçici) disk şifreleme gibi şifreleme anahtarlarınızı kullanarak uca verileri şifrelemek gerektirir. Alternatif olarak, Microsoft tarafından yönetilen anahtarları azure'da şifreleme anahtarları nerede Azure depolama hizmeti şifrelemesi kullanarak varsayılan olarak varsayılan olarak, yönetilen diskler bekleme durumundayken şifrelenir. Bu, uyumluluk ve güvenlik gereksinimlerini karşılıyorsa, gereksinimlerinizi karşılamak için varsayılan yönetilen disk şifrelemesi yararlanabilirsiniz.|
|İşlem kaynakları (service fabric)|10|Service fabric'te istemci kimlik doğrulaması için Azure Active Directory kullanın|İstemci kimlik doğrulaması yalnızca Azure Active Directory aracılığıyla Service Fabric'te gerçekleştirin.|
|İşlem kaynakları (Otomasyon hesabı)|5| Otomasyon hesabının şifrelemeyi etkinleştir|Otomasyon hesabı değişken varlıkları şifrelenmesi, hassas verileri depolarken etkinleştirin.|
|App Service|5|App Service'te tanılama günlüklerini etkinleştirme|Günlükleri etkinleştirmek ve bunları bir yıla kadar korur. Bu, etkinlik kayıtlarını araştırma amacıyla bir güvenlik olayı ortaya veya ağınızın tehlikeye yeniden oluşturmanıza olanak sağlar. |
|İşlem kaynakları (yük dengeleyici)|5|Yük dengeleyicide tanılama günlüklerini etkinleştirme|Günlükleri etkinleştirmek ve bunları bir yıla kadar korur. Bu, etkinlik kayıtlarını araştırma amacıyla bir güvenlik olayı ortaya veya ağınızın tehlikeye yeniden oluşturmanıza olanak sağlar. |
|İşlem kaynakları (arama)|5|Arama hizmetinde tanılama günlüklerini etkinleştirme|Günlükleri etkinleştirmek ve bunları bir yıla kadar korur. Bu, etkinlik kayıtlarını araştırma amacıyla bir güvenlik olayı ortaya veya ağınızın tehlikeye yeniden oluşturmanıza olanak sağlar. |
|İşlem kaynakları (hizmet veri yolu)|5|Hizmet veri yolu tanılama günlüklerini etkinleştirme|Günlükleri etkinleştirmek ve bunları bir yıla kadar korur. Bu, etkinlik kayıtlarını araştırma amacıyla bir güvenlik olayı ortaya veya ağınızın tehlikeye yeniden oluşturmanıza olanak sağlar. |
|İşlem kaynakları (akış analizi)|5|Azure Stream analytics'te tanılama günlüklerini etkinleştirme|Günlükleri etkinleştirmek ve bunları bir yıla kadar korur. Bu, etkinlik kayıtlarını araştırma amacıyla bir güvenlik olayı ortaya veya ağınızın tehlikeye yeniden oluşturmanıza olanak sağlar. |
|İşlem kaynakları (service fabric)|5|Service fabric'te tanılama günlüklerini etkinleştirme|Günlükleri etkinleştirmek ve bunları bir yıla kadar korur. Bu, etkinlik kayıtlarını araştırma amacıyla bir güvenlik olayı ortaya veya ağınızın tehlikeye yeniden oluşturmanıza olanak sağlar. |
|İşlem kaynakları (toplu)|5|Batch hesapları, tanılama günlüklerini etkinleştirme|Günlükleri etkinleştirmek ve bunları bir yıla kadar korur. Bu, etkinlik kayıtlarını araştırma amacıyla bir güvenlik olayı ortaya veya ağınızın tehlikeye yeniden oluşturmanıza olanak sağlar. |
|İşlem kaynakları (olay hub'ı)|5|Olay Hub'ındaki tanılama günlüklerini etkinleştirme|Günlükleri etkinleştirmek ve bunları bir yıla kadar korur. Bu, etkinlik kayıtlarını araştırma amacıyla bir güvenlik olayı ortaya veya ağınızın tehlikeye yeniden oluşturmanıza olanak sağlar. |
|İşlem kaynakları (mantıksal uygulamalar)|5|Logic apps'teki tanılama günlüklerini etkinleştirme|Günlükleri etkinleştirmek ve bunları bir yıla kadar korur. Bu, etkinlik kayıtlarını araştırma amacıyla bir güvenlik olayı ortaya veya ağınızın tehlikeye yeniden oluşturmanıza olanak sağlar. |
|Makine|30|Sanal makinelerinize bir güvenlik açığı değerlendirme çözümü yükleyin|Sanal makinelerinize bir güvenlik açığı değerlendirme çözümü yükleyin|
|Makine|15|Web uygulaması güvenlik duvarı ekleme| Web uygulamalarınızın güvenliğini sağlamak için bir web uygulaması Güvenlik Duvarı (WAF) çözümünü dağıtın. |
|Makine|30|Güvenlik açıklarını - bir güvenlik açığı değerlendirme çözümü tarafından Düzelt|Kendisi için bir güvenlik açığı değerlendirme 3 taraf çözümü dağıtılan sanal makinelerin sürekli olarak uygulama ve işletim sistemi güvenlik açıklarını karşı incelenen. Tür güvenlik açıklarına bulunduğunda, bunlar öneri bir parçası olarak daha fazla bilgi için kullanılabilir.|
|İşlem kaynakları (service fabric)|15|Service fabric'te EncryptAndSign ClusterProtectionLevel özelliğini ayarlayın|Service Fabric, düğümden düğüme iletişim için bir birincil küme sertifikası kullanarak koruma (None, oturum ve EncryptAndSign) üç düzeyleri sağlar.  Tüm düğümler için iletileri şifrelenir ve dijital olarak imzalanmış emin olmak için koruma düzeyini ayarlayın. |
|App Service|10|Uzaktan hata ayıklama için Web uygulaması kapalı olmaları|Artık bunu kullanmanız gerekiyorsa, Web uygulamaları için hata ayıklama devre dışı bırakın. Uzaktan hata ayıklama, gelen bağlantı noktası üzerinde bir işlev uygulaması açılmasını gerektirir.|
|App Service|10|Uzaktan hata ayıklama işlev uygulaması için kapatılmalıdır|Artık bunu kullanmanız gerekiyorsa, işlev uygulaması için hata ayıklama devre dışı bırakın. Uzaktan hata ayıklama, gelen bağlantı noktası üzerinde bir işlev uygulaması açılmasını gerektirir.|
|App Service|10|Web uygulaması için IP kısıtlamalarını yapılandırma|Uygulamanıza erişmek için izin verilen IP adreslerinin bir listesini tanımlar. IP kısıtlamaları kullanımı, bir web uygulaması genel saldırılara karşı korur.|
|App Service|10|İşlev uygulaması için IP kısıtlamalarını yapılandırma| Uygulamanıza erişmek için izin verilen IP adreslerinin bir listesini tanımlar. IP kısıtlamaları kullanımını bir işlev uygulaması genel saldırılara karşı korur.|
|App Service|10|Tüm izin verme ('* ') uygulamanıza erişmek için kaynaklar| WEBSITE_LOAD_CERTIFICATES parametre kümesi izin verme "". Parametresini ayarlayarak '' tüm sertifikalar, web uygulamaları kişisel sertifika deposuna yüklendiğini anlamına gelir. Site çalışma zamanında tüm sertifikalara erişim gerektiğini olası olmadığından bu en düşük öncelik ilkesini kötüye neden olabilir.|
|App Service|5|Web uygulaması için Web yuvalarını devre dışı bırakılmalıdır|Web uygulamaları içinde Web yuvaları kullanımını gözden geçirin. Web yuvaları Protokolü farklı güvenlik tehdidi türlerine savunmasızdır.|
|App Service|5|İşlev uygulaması için Web yuvalarını devre dışı bırakılmalıdır|İşlev uygulamaları içinde Web yuvaları kullanımını gözden geçirin. Web yuvaları Protokolü farklı güvenlik tehdidi türlerine savunmasızdır.|
|App Service|5|Web uygulamanız için özel etki alanları kullanın|Özel etki alanları, bir web uygulaması kimlik avı gibi genel saldırılara ve DNS ile ilgili diğer saldırılara karşı korumak için kullanın.|
|App Service|5|İşlev uygulaması için özel etki alanları kullanın|Özel etki alanları, bir işlev uygulaması, kimlik avı gibi genel saldırılara ve DNS ile ilgili diğer saldırılara karşı korumak için kullanın.|
|İşlem kaynakları (toplu)|1|Batch hesabında ölçüm uyarı kuralları yapılandırma|Batch hesabında ölçüm uyarı kuralları yapılandırın ve havuzu Silme Tamamlandı olayları ve havuz silme başlangıç olayları ölçümlerini etkinleştir|
|İşlem kaynakları (hizmet veri yolu)|1|Service Bus ad alanındaki tüm yetkilendirme kurallarını RootManageSharedAccessKey dışında Kaldır |Hizmet veri yolu istemcileri tüm kuyrukları ve konuları ad alanında erişim sağlayan bir ad alanı düzeyinde erişim ilkesi kullanmamanız gerekir. En az ayrıcalık güvenlik ile hizalamak için modeli, %d oluşturma erişim ilkelerini kuyruklar ve konular erişebilmesi için yalnızca belirli bir varlık için varlık düzeyinde.|
|İşlem kaynakları (olay hub'ı)|1|RootManageSharedAccessKey dışındaki tüm yetkilendirme kurallarını olay hub'ı ad alanından kaldırın |Olay hub'ı istemcilerin tüm kuyrukları ve konuları ad alanında erişim sağlayan bir ad alanı düzeyinde erişim ilkesi kullanmamanız gerekir. En az ayrıcalık güvenlik ile hizalamak için modeli, %d oluşturma erişim ilkelerini kuyruklar ve konular erişebilmesi için yalnızca belirli bir varlık için varlık düzeyinde.|
|İşlem kaynakları (olay hub'ı)|5|Olay hub'ı varlıkta yetkilendirme kuralları tanımlayın|Yetkilendirme kuralları en az ayrıcalıklı erişim vermek için olay hub'ı varlık üzerinde denetim.|
|Makine|30|Sanal makinelerinize bir güvenlik açığı değerlendirme çözümü yükleyin|Sanal makinelerinize bir güvenlik açığı değerlendirme çözümü yükleyin|
|App Service|20|CORS her kaynağın Web uygulamalarınıza erişmek izin vermemelidir|Web uygulamanızla etkileşim kurmak yalnızca gerekli etki alanı sağlar. Kaynak kaynak paylaşımı (CORS) tüm etki alanları web uygulamanıza erişmek izin vermemelisiniz.|
|App Service|20|CORS her kaynağın işlev uygulamanıza erişmek izin vermemelidir| İşlev uygulamanızla etkileşim kurmak yalnızca gerekli etki alanı sağlar. Kaynak kaynak paylaşımı (CORS) tüm etki alanlarının işlev uygulamanıza erişmek izin vermemelisiniz.|
|Makine|15|Web uygulaması güvenlik duvarı ekleme| Web uygulamalarınızın güvenliğini sağlamak için bir web uygulaması Güvenlik Duvarı (WAF) çözümünü dağıtın. |
|App Service|10|Kullanım en son .NET Framework Web uygulaması için desteklenir.|En son .NET Framework sürümü için en son güvenlik sınıflarını kullanın. Eski sınıfları ve türleri kullanma, uygulamanızı saldırılara açık hale getirebilirsiniz.|
|App Service|10|Web uygulaması için desteklenen en son Java sürümünü kullanın|En son Java sürümünü en son güvenlik sınıflarını kullanın. Eski sınıfları ve türleri kullanma, uygulamanızı saldırılara açık hale getirebilirsiniz.|
|App Service|10|Web uygulaması için desteklenen en son PHP sürümünü kullanın|En son PHP sürümünü en son güvenlik sınıflarını kullanın. Eski sınıfları ve türleri kullanma, uygulamanızı saldırılara açık hale getirebilirsiniz.|
|App Service|10|Web uygulaması için desteklenen en son Node.js sürümünü kullanın|En son Node.js sürümü için en son güvenlik sınıflarını kullanın. Eski sınıfları ve türleri kullanma, uygulamanızı saldırılara açık hale getirebilirsiniz.|
|App Service|10|Web uygulaması için desteklenen en son Python sürümünü kullanın|En son Python sürümünü en son güvenlik sınıflarını kullanın. Eski sınıfları ve türleri kullanma, uygulamanızı saldırılara açık hale getirebilirsiniz.|
|VM'ler ve bilgisayarlar|1|Sanal makineleri geçirmek için yeni AzureRM kaynaklar|Sanal makineleriniz için yeni Azure Resource Manager v2 gibi güvenlik geliştirmeleri sağlamak için kullanın: yönetilen kimlikleri, gizli dizileri, Azure anahtar kasası için erişim daha güçlü erişim denetimi (RBAC), daha iyi denetim, ARM tabanlı bir dağıtım ve yönetim erişim AD tabanlı kimlik doğrulaması ve etiketleri için destek ve daha kolay güvenlik yönetimi için kaynak grupları. |
|Makine|30|Güvenlik açıklarını - bir güvenlik açığı değerlendirme çözümü tarafından Düzelt|Kendisi için bir güvenlik açığı değerlendirme 3 taraf çözümü dağıtılan sanal makinelerin sürekli olarak uygulama ve işletim sistemi güvenlik açıklarını karşı incelenen. Tür güvenlik açıklarına bulunduğunda, bunlar öneri bir parçası olarak daha fazla bilgi için kullanılabilir.|




## <a name="next-steps"></a>Sonraki adımlar
Diğer Azure kaynak türü için geçerli öneriler hakkında daha fazla bilgi için aşağıdakilere bakın:


* [Sanal makineler için Azure Güvenlik Merkezi'ni anlama önerileri](security-center-virtual-machine-recommendations.md)
* [Kimlik ve erişim Azure Güvenlik Merkezi'nde izleme](security-center-identity-access.md)
* [Azure Güvenlik Merkezi'nde ağınızı koruma](security-center-network-recommendations.md)
* [Azure Güvenlik Merkezi'nde Azure SQL hizmetinizi koruma](security-center-sql-service-recommendations.md)

Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](security-center-azure-policy.md) -- Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md) -- Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) -- Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.

<!--Image references-->
[1]: ./media/security-center-virtual-machine-recommendations/overview.png
[2]: ./media/security-center-virtual-machine-recommendations/compute.png
[3]: ./media/security-center-virtual-machine-recommendations/monitoring-agent-health-issues.png
[4]: ./media/security-center-virtual-machine-recommendations/compute-recommendations.png
[5]: ./media/security-center-virtual-machine-recommendations/apply-system-updates.png
[6]: ./media/security-center-virtual-machine-recommendations/missing-update-details.png
[7]: ./media/security-center-virtual-machine-recommendations/vm-computers.png
[8]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon1.png
[9]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon2.png
[10]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon3.png
[11]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon4.png
[12]: ./media/security-center-virtual-machine-recommendations/filter.png
[13]: ./media/security-center-virtual-machine-recommendations/vm-detail.png
[14]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-fig1-new006-2017.png
[15]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-fig8-new3.png
[16]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-fig8-new4.png
[17]: ./media/security-center-virtual-machine-recommendations/app-services.png
[18]: ./media/security-center-virtual-machine-recommendations/ase.png
[19]: ./media/security-center-virtual-machine-recommendations/web-app.png
[20]: ./media/security-center-virtual-machine-recommendations/recommendation.png
[21]: ./media/security-center-virtual-machine-recommendations/recommendation-desc.png
[22]: ./media/security-center-virtual-machine-recommendations/passed-assessment.png
[23]: ./media/security-center-virtual-machine-recommendations/healthy-resources.png
[24]: ./media/security-center-virtual-machine-recommendations/function-app.png
