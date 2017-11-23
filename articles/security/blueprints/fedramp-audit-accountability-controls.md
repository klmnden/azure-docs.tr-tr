---
title: "FedRAMP Azure şeması Otomasyonu - denetim ve Sorumluluk"
description: "Denetim ve Sorumluluk FedRAMP için-Web uygulamaları"
services: security
documentationcenter: na
author: jomolesk
manager: mbaldwin
editor: tomsh
ms.assetid: c5858e07-ca74-4526-b31f-e6b4e55bb594
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: jomolesk
ms.openlocfilehash: 83ef9cbb7652bf128d7758237a8e6fbeed6c6565
ms.sourcegitcommit: 8aa014454fc7947f1ed54d380c63423500123b4a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/23/2017
---
# <a name="audit-and-accountability-au"></a>Denetim ve Sorumluluk (AU)

> [!NOTE]
> Bu denetimler NIST ve ABD tarafından tanımlanır Ticaret Bakanlığı NIST özel yayını 800-53 düzeltme 4 bir parçası olarak. NIST 800 53 düzeltme 4 yordamları ve yönergeler her denetim için test etme hakkında bilgi için lütfen bakın.

## <a name="nist-800-53-control-au-1"></a>NIST 800 53 denetim AU-1

#### <a name="audit-and-accountability-policy-and-procedures"></a>Denetim ve Sorumluluk İlkesi ve yordamları

**AU-1** kuruluş geliştirir, belgeler ve için disseminates [atama: kuruluş tarafından tanımlanan personel ya da roller] adresleri amacı, kapsam, roller, sorumlulukları, yönetim taahhüt bir denetim ve Sorumluluk İlkesi Kurumsal varlıklar ve uyumluluk arasında koordinasyon; ve denetim ve Sorumluluk ilkeyi ve ilgili denetim ve Sorumluluk denetimleri uyarlamasını kolaylaştırmak için yordamlar; gözden geçirir ve geçerli denetim ve Sorumluluk İlkesi güncelleştirmeleri [atama: kuruluş tarafından tanımlanan sıklığı]; ve denetim ve Sorumluluk yordamları [atama: kuruluş tarafından tanımlanan sıklığı].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri'nin kuruluş düzeyi denetim ve Sorumluluk ilke ve yordamlar bu denetim gidermek yeterli olabilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-au-2a"></a>NIST 800 53 denetim AU-2.a

#### <a name="audit-events"></a>Denetim olayları

**AU 2.a** kuruluş bilgileri sistem aşağıdaki olaylar denetimini kapasitesine sahip olduğunu belirler: [atama: kuruluş tanımlanan denetlenebilir olaylar].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması için denetim özelliğini Azure İzleyici ve OMS günlük analizi hizmeti tarafından sağlanır. Azure İzleyicisi dağıtılan kaynaklarla ilgili etkinliği hakkında ayrıntılı denetim günlüklerini sağlar. Bunlar ve işletim sistemi düzeyinde günlükleri günlük analizi tarafından toplanır ve OMS deposunda saklanır. Günlük analizi bu çözümü tarafından dağıtılmış kaynaklar genelinde karşılık gelen denetim verilerini ve müşteri tarafından dağıtılan web uygulamasına genişletilebilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-au-2b"></a>NIST 800 53 denetim AU-2.b

#### <a name="audit-events"></a>Denetim olayları

**AU 2.b** kuruluşun güvenlik denetim işlevi karşılıklı destek geliştirmek ve denetlenebilir olayları seçimi Kılavuzu yardımcı olmak için Denetim ile ilgili bilgiler gerektiren diğer kuruluş varlıklarıyla düzenler.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri denetlenebilir olayları belirler kurulmuş bir kuruluş düzeyi işlemi kullanır. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-au-2c"></a>NIST 800 53 denetim AU-2.c

#### <a name="audit-events"></a>Denetim olayları

**AU 2.c** neden denetlenebilir olayları sonra--olgu araştırmalar güvenlik olayların desteklemek yeterli sayılan için bir stratejinin kuruluş sağlar.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması tarafından denetlenen olayları, olaylar meydana geldiğinde belirlemek yeterli bilgi, olay kaynağı, olay ve güvenlik olaylarını incelenmesi destekleyen diğer ayrıntılı bilgilerden sonucunu içerir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-au-2d"></a>NIST 800 53 denetim AU-2.d

#### <a name="audit-events"></a>Denetim olayları

**AU 2.d** aşağıdaki olaylar bilgi sisteminde denetleneceğini kuruluş belirler: [atama: sıklığını birlikte Denetlenen olayları (AU 2 A'da tanımlanan denetlenebilir olaylar alt) kuruluş tanımlı (veya tanımlanan her olay için situation requiring) denetim].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması tarafından denetlenen olayları Azure etkinlik günlükleri dağıtılan kaynakları, işletim sistemi düzeyinde günlükleri, Active Directory günlükleri için bu denetlenen vardır ve SQL Server günlüğe kaydeder. Müşteriler, iş gereksinimlerini karşılamak için denetlenecek ek olaylar seçebilirsiniz. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-au-2-3"></a>NIST 800 53 denetim AU-2 (3)

#### <a name="audit-events--reviews-and-updates"></a>Denetim olayları | Gözden geçirme ve güncelleştirmeler

**AU-2 (3)** kuruluş gözden geçirir ve denetlenen olayları güncelleştirir [atama: kuruluş tarafından tanımlanan sıklığı].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri kurulmuş bir kuruluş düzeyi düzenli gözden kullanır ve denetlenen olayları tanımlı kümesi için işlem güncelleştirme olabilirsiniz. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-au-3"></a>NIST 800 53 denetim AU-3

#### <a name="content-of-audit-records"></a>Denetim kayıtlarının içeriği

**AU 3** denetim kayıtlarının ne tür olay oluştu olay oluştuğunda olayın gerçekleştiği kurar bilgileri içeren, olay kaynağı, olay sonucunu ve herhangi bir kimlik bilgileri sistem oluşturur Kişiler veya olay ile ilgili konular.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması yerleşik denetim yeteneklerine Azure, Windows Server ve SQL Server kullanır. Bu çözümleri yakalama denetim kayıtlarının bu denetim gereksinimlerini karşılamak için yeterli ayrıntılarla denetim. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-au-3-1"></a>NIST 800 53 denetim AU-3 (1)

#### <a name="content-of-audit-records--additional-audit-information"></a>Denetim kayıtlarının içerik | Ek denetim bilgileri

**AU-3 (1)** bilgileri sistem aşağıdaki ek bilgileri içeren denetim kayıtlarının oluşturur: [atama: kuruluş tarafından tanımlanan ek, daha ayrıntılı bilgi].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Azure etkinlik günlüğü olaylarını denetim bilgilerini 20'den fazla türleri için alanları içeren ayrıntılı bir şema kullanın. Etkinlik günlüğü ek olarak, bu Azure şeması Windows günlükleri, Linux günlükleri, Azure tanılama günlüklerini ve müşteri günlükleri de dahil olmak üzere veri kaynakları, farklı bir kümesini destekler OMS günlük analizi çözümde dağıtır.  |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-au-3-2"></a>NIST 800 53 denetim AU-3 (2)

#### <a name="content-of-audit-records--centralized-management-of-planned-audit-record-content"></a>Denetim kayıtlarının içerik | Planlanan denetim kayıt içeriği için Merkezi Yönetim

**AU-3 (2)** bilgileri Sistem Merkezi Yönetim ve yapılandırma denetim kaydı tarafından oluşturulan Yakalanacak içeriğin sağlar [atama: kuruluş tarafından tanımlanan bilgileri sistem bileşenleri].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması tarafından dağıtılan tüm sanal makineleri dağıtılan Active Directory etki alanına eklenir. Etki alanına katılmış tüm sanal makinelerin işletim sistemi düzeyinde denetim sistem yapılandırmasını merkezi olarak yönetmek üzere yapılandırılmış bir Grup İlkesi uygulayın. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-au-4"></a>NIST 800 53 denetim AU-4

#### <a name="audit-storage-capacity"></a>Denetim depolama kapasitesi

**AU 4** denetim kaydı depolama kapasitesi ile uyumlu olarak kuruluş ayırır [atama: kuruluş tanımlı denetim kaydı depolama gereksinimleri].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması denetim kayıtlarının bir yıl boyunca korumak için yeterli depolama kapasitesi ayırır. Tüm denetim kayıtlarının bir yıllık bekletme için yapılandırılmış olan günlük analizi tarafından toplanır. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-au-5a"></a>NIST 800 53 denetim AU-5.a

#### <a name="response-to-audit-processing-failures"></a>İşleme hataları denetlemek için yanıt

**AU 5.a** bilgileri sistem uyarıları [atama: kuruluş tarafından tanımlanan personel ya da roller] bir denetim işleme hatası durumunda.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Azure İzleyici ve günlük analizi için hizmet durumunu Azure durumu Web sitesi ve hizmet sistem durumu dikey Azure portalında kullanılabilir. Uyarıları işleme hataları denetim diğer tür bildirim sağlamak için günlük analizi yapılandırılabilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-au-5b"></a>NIST 800 53 denetim AU-5.b

#### <a name="response-to-audit-processing-failures"></a>İşleme hataları denetlemek için yanıt

**AU 5.b** bilgi sistemi aşağıdaki ek eylemleri gerçekleştirir: [atama: kuruluş tarafından tanımlanan Eylemler gerçekleştirilmesine (örn., bilgi sistemi Kapat, en eski denetim kayıtlarının üzerine yazma, Denetim kayıtlarının oluşturma işlemini)].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması tarafından dağıtılan kaynaklar tarafından oluşturulan tüm denetim kayıtlarının günlük analizi tarafından toplanan ve bir yıl boyunca tutulur. Bu denetim kaydı depolama için depolama ayırma dinamik olarak ayrılan yeterli kapasitesi sağlama kullanılabilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-au-5-1"></a>NIST 800 53 denetim AU-5 (1)

#### <a name="response-to-audit-processing-failures--audit-storage-capacity"></a>İşleme hataları denetlemek için yanıt | Denetim depolama kapasitesi

**AU-5 (1)** bir uyarı bilgi sistem sağlar [atama: kuruluş tarafından tanımlanan personel, rolleri ve/veya konumları] içinde [atama: kuruluş tanımlı süre] [ulaştığında ayrılmış denetim kaydı depolama birimi Atama: kuruluş tarafından tanımlanan yüzdeyi] deposu en fazla denetim kaydı depolama kapasitesi.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması tarafından dağıtılan kaynaklar tarafından oluşturulan tüm denetim kayıtlarının günlük analizi tarafından toplanan ve bir yıl boyunca tutulur. Bu denetim kaydı depolama için depolama ayırma dinamik olarak ayrılan yeterli kapasitesi sağlama kullanılabilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-au-5-2"></a>NIST 800 53 denetim AU-5 (2)

#### <a name="response-to-audit-processing-failures--real-time-alerts"></a>İşleme hataları denetlemek için yanıt | Gerçek zamanlı uyarılar

**AU-5 (2)** bir uyarı bilgi sistem sağlar [atama: kuruluş tarafından tanımlanan gerçek zamanlı dönemi] için [atama: kuruluş tarafından tanımlanan personel, rolleri ve/veya konumları] aşağıdaki denetim hatası olaylarının ortaya çıktığında: [atama: Kuruluşunuz tarafından tanımlanan denetim hatası olaylarının] gerçek zamanlı uyarılar gerektirme.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Azure için hizmet durumunu hizmet sistem durumu dikey Azure portalında kullanılabilir. Uyarıları işleme hataları denetim diğer tür bildirim sağlamak için günlük analizi yapılandırılabilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-au-6a"></a>NIST 800 53 denetim AU-6.a

#### <a name="audit-review-analysis-and-reporting"></a>Denetim gözden geçirme, analiz ve Raporlama

**AU 6.a** kuruluş gözden geçirir ve bilgi Sistem Denetim kayıtlarını çözümler [atama: kuruluş tarafından tanımlanan sıklığı] göstergeleri için [atama: kuruluş tarafından tanımlanan uygun veya alışılmadık etkinliği].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, gözden geçirme ve müşteri tarafından dağıtılan kaynaklarına (uygulamalar, işletim sistemleri, veritabanları ve yazılım dahil etmek için) Denetim kayıtlarını çözümleme sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-au-6b"></a>NIST 800 53 denetim AU-6.b

#### <a name="audit-review-analysis-and-reporting"></a>Denetim gözden geçirme, analiz ve Raporlama

**AU 6.b** kuruluş için bulgularını raporları [atama: kuruluş tarafından tanımlanan personel ya da roller].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, müşteri tarafından dağıtılan kaynakları (AU-06.a tanımlanan) uygun veya alışılmadık etkinliği bulgularını raporlama için sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-au-6-1"></a>NIST 800 53 denetim AU-6 (1)

#### <a name="audit-review-analysis-and-reporting--process-integration"></a>Denetim gözden geçirme, analiz ve Raporlama | İşlem tümleştirme

**AU-6 (1)** kuruluş denetim gözden geçirme, analiz ve araştırma ve yanıt kuşkulu etkinlikler için kuruluş işlemleri desteklemek için işlemleri raporlama tümleştirmek için otomatik mekanizmaları uygular.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, bir kuruluş düzeyi Merkezi Denetim gözden geçirme, analiz ve raporlama özelliği bağlı. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-au-6-3"></a>NIST 800 53 denetim AU-6 (3)

#### <a name="audit-review-analysis-and-reporting--correlate-audit-repositories"></a>Denetim gözden geçirme, analiz ve Raporlama | Denetim depoları ilişkilendirmek

**AU-6 (3)** kuruluş analiz eder ve karşılık gelen kayıtları arasında farklı depoları kuruluş genelinde situational tanıma kazanmak için denetleme.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması kuruluş genelinde situational tanıma destekleyici denetim verilerini dağıtılan kaynaklar arasında merkezileştirmek için OMS içinde günlük analizi çözüm uygular. Daha fazla günlük analizi diğer sistemlerle tümleştirmek için müşteriler may seçerseniz. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-au-6-4"></a>NIST 800 53 denetim AU-6 (4)

#### <a name="audit-review-analysis-and-reporting--central-review-and-analysis"></a>Denetim gözden geçirme, analiz ve Raporlama | Merkezi gözden geçirme ve çözümleme

**AU-6 (4)** bilgileri Sistem Merkezi olarak gözden geçirin ve sistem içinde birden çok bileşen denetim kayıtları çözümlemek için yeteneği sağlar.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması günlük analizi çözüm dağıtılan kaynaklar, Destek Merkezi gözden geçirme, çözümleme ve raporlama arasında denetim verilerini merkezileştirmek için OMS içinde uygular. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-au-6-5"></a>NIST 800 53 denetim AU-6 (5)

#### <a name="audit-review-analysis-and-reporting--integration--scanning-and-monitoring-capabilities"></a>Denetim gözden geçirme, analiz ve Raporlama | Tümleştirme / tarama ve izleme kapasiteleri

**AU-6 (5)** kuruluş analiz denetim kayıtlarının analizini ile tümleşir [seçimi (bir veya daha fazla): Tarama bilgi, performans verileri; izleme bilgilerini; bilgileri sistem güvenlik açığı [Atama: kuruluş tarafından tanımlanan veri/bilgileri toplanan diğer kaynaklardan]] Daha fazla uygun veya alışılmadık etkinliği tanımlamak yeteneğini geliştirmek için.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması OMS güvenlik ve denetim çözümü dağıtır. Bu çözüm güvenlik tutumunu kapsamlı bir görünümünü sağlar. Güvenlik ve Denetim Panosu kullanılabilir veri taban çizgisi ve düzeltme eki değerlendirmesi güvenlik açığı verileri ve günlük verilerini tümleştirme dağıtılan OMS çözümleri kullanılarak dağıtılan kaynakların güvenlik durumu üst düzey bir anlayış sağlar. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-au-6-6"></a>NIST 800 53 denetim AU-6 (6)

#### <a name="audit-review-analysis-and-reporting--correlation-with-physical-monitoring"></a>Denetim gözden geçirme, analiz ve Raporlama | Fiziksel izleme ile birlikte bağıntı

**AU-6 (6)** kuruluş denetim kayıtlarının bilgilerinden daha fazla şüpheli, uygunsuz, olağan dışı veya kötü niyetli etkinlik tanımlamak yeteneğini geliştirmek için fiziksel erişimi izleme alınan bilgilerle karşılık gelen.

**Sorumlulukları:**`Azure Only`

|||
|---|---|
| **Müşteri** | Müşteriler fiziksel tüm sistem kaynakları Azure veri merkezlerinde erişiminiz yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure'nın SIM takım ve var olup olmadığını tüm ilişkili mantıksal ihlal ya da şüpheli davranış fiziksel olaylar algılandığında belirlemek için Denetim kayıtlarıyla karşılık gelen ilgili fiziksel izleme verilerini kullanır. |


 ### <a name="nist-800-53-control-au-6-7"></a>NIST 800 53 denetim AU-6 (7)

#### <a name="audit-review-analysis-and-reporting--permitted-actions"></a>Denetim gözden geçirme, analiz ve Raporlama | İzin verilen eylemleri

**AU-6 (7)** kuruluş her biri için izin verilen eylemleri belirtir [seçimi (bir veya daha fazla): bilgi sistem işlemi, rol; kullanıcı] gözden geçirme ile ilişkilendirilmiş, analiz, raporlama denetim ve bilgi.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması tarafından dağıtılan Windows sanal makineleri bir kullanıcı saygı denetim bilgilerle gerçekleştirebileceğiniz eylemleri kısıtlamak işletim sistemi düzeyinde izinler uygulayın. Azure içinde kullanıcılar ya da kullanıcı grupları (örn., sahibi, katkıda bulunan, okuyucu veya özel bir rol) rollere göre herhangi bir kaynağa kullanılabilir eylemleri kısıtlamak için atanabilir veya günlük analizi dahil olmak üzere çözümleri, dağıtılan.  |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-au-6-10"></a>NIST 800 53 denetim AU-6 (10)

#### <a name="audit-review-analysis-and-reporting--audit-level-adjustment"></a>Denetim gözden geçirme, analiz ve Raporlama | Denetim düzeyi ayarlama

**AU-6 (10)** kuruluş denetim gözden geçirme, analiz ve risk yasaları zorlama bilgileri, yönetim bilgilerini veya diğer güvenilir kaynakları göre bir değişiklik olduğunda bilgileri sistem içinde raporlama düzeyini ayarlar bilgi.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri denetim gözden geçirme, analiz ve Raporlama (uygulamalar, işletim sistemleri, veritabanları ve yazılım dahil olmak üzere) için kaynaklar müşteri tarafından dağıtılan düzeyini ayarlamak için sorumlu olduğu risk yasalar tarafından sağlanan bilgilere dayanarak bir değişiklik olduğunda zorlama, Intelligence veya diğer güvenilir kaynakları. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-au-7a"></a>NIST 800 53 denetim AU-7.a

#### <a name="audit-reduction-and-report-generation"></a>Denetim azaltma ve rapor oluşturma

**AU 7.a** bir denetim azaltma ve isteğe bağlı destekleyen rapor oluşturma yeteneği denetim gözden geçirme, analiz ve raporlama gereksinimlerini ve sonra--olgu araştırmalar güvenlik olayların bilgi sistemi sağlar.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması günlük analizi çözüm içinde OMS uygular. Günlük analizi, merkezi bir depoya yönetilen kaynaklarından veri toplayarak OMS için izleme hizmetleri sağlar. Toplanan veriler uyarı, analiz ve dışarı aktarma için kullanılabilir hale gelir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-au-7b"></a>NIST 800 53 denetim AU-7.b

#### <a name="audit-reduction-and-report-generation"></a>Denetim azaltma ve rapor oluşturma

**AU 7.b** bilgi sistemi, özgün içerik veya zaman Denetim kayıtlarını sıralama değiştirmez bir denetim azaltma ve rapor oluşturma yeteneği sağlar.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması günlük analizi çözüm içinde OMS uygular. Günlük analizi, merkezi bir depoya yönetilen kaynaklarından veri toplayarak OMS için izleme hizmetleri sağlar. İçerik ve Denetim kayıtlarını zaman sıralama günlük analizi tarafından toplanan zaman değiştirilmiş değil. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-au-7-1"></a>NIST 800 53 denetim AU-7 (1)

#### <a name="audit-reduction-and-report-generation--automatic-processing"></a>Denetim azaltma ve rapor oluşturma | Otomatik işleme

**AU-7 (1)** bilgileri Sistem Denetim kayıtlarını göre ilgi olayları işleme yeteneği sağlar [atama: denetim kaydı içinde kuruluşunuz tarafından tanımlanan denetim alanları].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması günlük analizi çözüm içinde OMS uygular. Günlük analizi, merkezi bir depoya yönetilen kaynaklarından veri toplayarak OMS için izleme hizmetleri sağlar. Toplanan veriler uyarı, analiz ve dışarı aktarma için kullanılabilir hale gelir. Log Analytics, depoda saklanan verilerin ayıklanması için güçlü bir sorgu dili içerir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-au-8a"></a>NIST 800 53 denetim AU-8.a

#### <a name="time-stamps"></a>Zaman damgaları

**AU 8.a** denetim kayıtlarının zaman damgalarını üretmek için iç sistem saatleri bilgi sistemi kullanır.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması tarafından dağıtılan kaynak iç sistem saatlerinin zaman damgalarını denetim kaydı oluşturmak için kullanın. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-au-8b"></a>NIST 800 53 denetim AU-8.b

#### <a name="time-stamps"></a>Zaman damgaları

**AU 8.b** bilgi sistemi Eşgüdümlü Evrensel Saat (UTC) veya (GMT) Greenwich saati eşlenmiş denetim kayıtlarının zaman damgalarını kaydeder ve karşılayan [atama: zaman ölçümü kuruluşunuz tarafından tanımlanan kesinliği].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması tarafından dağıtılan kaynak iç sistem saatlerinin zaman damgalarını denetim kaydı oluşturmak için kullanın. Zaman damgaları UTC olarak kaydedilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-au-8-1a"></a>NIST 800 53 denetim AU-8 (1) bir

#### <a name="time-stamps--synchronization-with-authoritative-time-source"></a>Zaman damgaları | Yetkili zaman kaynağı ile eşitleme

**AU-8 (1) bir** bilgileri sistem iç bilgileri sistem saatlerinin karşılaştırır [atama: kuruluş tarafından tanımlanan sıklığı] ile [atama: kuruluş tarafından tanımlanan yetkili zaman kaynağı].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması tarafından dağıtılan kaynak iç sistem saatlerinin zaman damgalarını denetim kaydı oluşturmak için kullanın. İç sistem saatleri yapılandırılmış olan bir yetkili saat kaynağı ile eşitleme. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-au-8-1b"></a>NIST 800 53 denetim AU-8 (1) .b

#### <a name="time-stamps--synchronization-with-authoritative-time-source"></a>Zaman damgaları | Yetkili zaman kaynağı ile eşitleme

**AU-8 (1) .b** zaman farkı daha büyük olduğunda bilgi sistemi yetkili saat kaynağına iç sistem saatleri eşitler [atama: kuruluş tanımlı süre].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması tarafından dağıtılan kaynak iç sistem saatlerinin zaman damgalarını denetim kaydı oluşturmak için kullanın. İç sistem saatleri yapılandırılmış olan bir yetkili saat kaynağı ile eşitleme. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-au-9"></a>NIST 800 53 denetim AU-9

#### <a name="protection-of-audit-information"></a>Denetim bilgilerin korunması

**AU 9** yetkisiz erişim, değiştirilmesi ve silinmesini bilgi sistemi denetim bilgileri ve denetim araçları korur.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Mantıksal erişim denetimleri denetim bilgileri ve bu Azure şeması içinde araçları yetkisiz erişim, değiştirilmesi ve silinmesini korumak için kullanılır. Azure Active Directory rol tabanlı Grup üyeliklerini kullanma onaylanan mantıksal erişimini zorunlu kılar. Denetim bilgilerini görüntülemek ve denetleme araçları kullanmak için özelliği bu izni gerektiren kullanıcılar sınırlı olabilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-au-9-2"></a>NIST 800 53 denetim AU-9 (2)

#### <a name="protection-of-audit-information--audit-backup-on-separate-physical-systems--components"></a>Denetim bilgilerini koruma | Denetim ayrı fiziksel sistemleri üzerinde yedekleme / bileşenleri

**AU-9 (2)** bilgi sistemi denetim kayıtlarının yedekler [atama: kuruluş tarafından tanımlanan sıklığı] fiziksel olarak farklı sisteminize veya sistem bileşeni sistemin veya bileşenin Denetlenmekte daha.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması günlük analizi hizmeti OMS uygular. Dağıtılan VM'ler ve Azure tanılama depolama hesapları günlük analizi için bağlı kaynaklar ve bunların kaynaktan ayrı olarak korunur. Verileri, içinde OMS tarafından yakın gerçek zamanlı olarak toplanır. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-au-9-3"></a>NIST 800 53 denetim AU-9 (3)

#### <a name="protection-of-audit-information--cryptographic-protection"></a>Denetim bilgilerini koruma | Şifreleme koruma

**AU-9 (3)** bilgi sistemi denetim bilgileri ve denetim araçları bütünlüğünü korumak için şifreleme mekanizmaları uygular.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması günlük analizi hizmeti OMS uygular. Günlük analizi gelen verilerin güvenilir bir kaynaktan sertifikalar ve Azure kimlik doğrulaması ile veri bütünlüğünü doğrulayarak olmasını sağlar. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-au-9-4"></a>NIST 800 53 denetim AU-9 (4)

#### <a name="protection-of-audit-information--access-by-subset-of-privileged-users"></a>Denetim bilgilerini koruma | Ayrıcalıklı kullanıcı alt kümesine göre erişim

**AU-9 (4)** kuruluş denetim işlevselliği yalnızca yönetim erişimini yetkilendirir [atama: kuruluş tarafından tanımlanan alt ayrıcalıklı kullanıcıların].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Mantıksal erişim denetimleri denetim bilgileri ve bu Azure şeması içinde araçları yetkisiz erişim, değiştirilmesi ve silinmesini korumak için kullanılır. Azure Active Directory rol tabanlı Grup üyeliklerini kullanma onaylanan mantıksal erişimini zorunlu kılar. Denetim bilgilerini görüntülemek ve denetleme araçları kullanmak için özelliği bu izni gerektiren kullanıcılar sınırlı olabilir.
 |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-au-10"></a>NIST 800 53 denetim AU-10

#### <a name="non-repudiation"></a>Red olmayan

**AU 10** bir kişi (veya bir kişinin adına işlem hareket karşı) yanlışlıkla gerçekleştirilen reddetme bilgi sistemi korur [atama: kuruluş tarafından tanımlanan Eylemler inkar tarafından ele alınacak].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması için denetim özelliğini Azure İzleyici ve OMS günlük analizi hizmeti tarafından sağlanır. Azure İzleyicisi dağıtılan kaynaklarla ilgili etkinliği hakkında ayrıntılı denetim günlüklerini sağlar. Bunlar ve işletim sistemi düzeyinde günlükleri günlük analizi tarafından toplanır ve OMS deposunda saklanır. Bu günlükler, bilgileri sistem olayları ayrıntılı kayıtlar içeriyordu ve inkar karşı korunmasına yardımcı olabilir. Ayrıca, verileri günlüğe kaydetmek için erişim unauthored değişiklik ya da günlük verilerinin silinmesini önlemek için rol tabanlı erişim denetimini kullanarak sınırlıdır. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-au-11"></a>NIST 800 53 denetim AU-11

#### <a name="audit-record-retention"></a>Denetim kaydı saklama

**AU 11** kuruluş için Denetim kayıtlarının korur [atama: kuruluş tanımlı süre kayıtları bekletme ilkesi ile tutarlı] güvenlik olaylarına sonrasında--olgu araştırmalar için destek sağlar ve Mevzuat karşılamak için ve kuruluş bilgilerini bekletme gereksinimleri.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması günlük analizi hizmeti OMS uygular. Günlük analizi, merkezi bir depoya yönetilen kaynaklarından veri toplayarak OMS için izleme hizmetleri sağlar. Toplandığında, günlük analizi yapılandırması başına bir yıl için veriler korunur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-au-12a"></a>NIST 800 53 denetim AU-12.a

#### <a name="audit-generation"></a>Denetim oluşturma

**AU 12.a** bilgi sistemi AU-2'de tanımlanan denetlenebilir olaylar denetim kaydı oluşturma yeteneği sağlar bir. konumundaki [atama: kuruluş tarafından tanımlanan bilgileri sistem bileşenleri].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması tarafından denetlenen olayları Azure etkinlik günlükleri dağıtılan kaynakları, işletim sistemi düzeyinde günlükleri, Active Directory günlükleri için bu denetlenen vardır ve SQL Server günlüğe kaydeder. Müşteriler, iş gereksinimlerini karşılamak için denetlenecek ek olaylar seçebilirsiniz. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-au-12b"></a>NIST 800 53 denetim AU-12.b

#### <a name="audit-generation"></a>Denetim oluşturma

**AU 12.b** bilgi sistemi sağlar [atama: kuruluş tarafından tanımlanan personel ya da roller] denetlenebilir olayları belirli bilgileri sistem bileşenleri tarafından denetleneceğini seçin.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | İşlevler denetlenecek erişim, rol tabanlı erişim denetimini Azure içinde ve sanal makinede işletim sistemi düzeyinde kullanarak sınırlıdır. Bu Azure şeması tarafından dağıtılan kaynaklar tarafından denetlenmesi için seçilen olayları yapılandırmasını uygun rol tabanlı yetkilendirme sahip kullanıcılar tarafından yapılandırılabilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-au-12c"></a>NIST 800 53 denetim AU-12.c

#### <a name="audit-generation"></a>Denetim oluşturma

**AU 12.c** bilgi sistemi AU-2.d tanımlanan olaylar için denetim kaydı oluşturur. AU-3'te tanımlanan içerikle.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması tarafından denetlenen olayları Azure etkinlik günlükleri dağıtılan kaynakları, işletim sistemi düzeyinde günlükleri, Active Directory günlükleri için bu denetlenen vardır ve SQL Server günlüğe kaydeder. Müşteriler, iş gereksinimlerini karşılamak için denetlenecek ek olaylar seçebilirsiniz. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-au-12-1"></a>NIST 800 53 denetim AU-12 (1)

#### <a name="audit-generation--system-wide--time-correlated-audit-trail"></a>Denetim oluşturma | Sistem genelinde / saat bağıntılı denetim izi

**AU-12 (1)** denetim kayıtlarından bilgi sistemi derler [atama: kuruluş tarafından tanımlanan bilgileri sistem bileşenleri] zaman-bağıntılı için içinde bir sistem genelinde (mantıksal veya fiziksel) denetim izi içine [atama: Kuruluş tanımlı dayanıklılık düzeyini denetim izi tek tek kayıtların zaman damgaları arasındaki ilişki için].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması günlük analizi hizmeti OMS uygular. Günlük analizi, merkezi bir depoya yönetilen kaynaklarından veri toplayarak OMS için izleme hizmetleri sağlar. Denetim kaydı zaman damgaları değiştirilmediğini, bu nedenle denetim izi zaman bağıntılı. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-au-12-3"></a>NIST 800 53 denetim AU-12 (3)

#### <a name="audit-generation--changes-by-authorized-individuals"></a>Denetim oluşturma | Yetkili kişiler tarafından yapılan değişiklikler

**AU-12 (3)** bilgi sistemi yeteneği sağlar [atama: kuruluş tarafından tanımlanan kişiler veya rolleri] üzerinde gerçekleştirilecek denetim değiştirmek için [atama: kuruluş tarafından tanımlanan bilgileri sistem bileşenleri] dayalı [üzerinde Atama: kuruluş tarafından tanımlanan seçilebilir olay ölçütleri] içinde [atama: kuruluş tanımlı bir zaman eşikleri].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | İşlevler denetlenecek erişim, rol tabanlı erişim denetimini Azure içinde ve sanal makinede işletim sistemi düzeyinde kullanarak sınırlıdır. Bu Azure şeması tarafından dağıtılan kaynaklar tarafından denetlenmesi için seçilen olayları yapılandırmasını uygun rol tabanlı yetkilendirme sahip kullanıcılar tarafından yapılandırılabilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |
