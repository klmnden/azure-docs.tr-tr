---
title: Hizmet durumu bildirimlerine nedir | Microsoft Docs
description: "Hizmet durumu bildirimlerine Microsoft Azure tarafından iletileri yayımlamak hizmet durumunu görüntülemek sağlar."
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: ancav
ms.openlocfilehash: d85281c02b792921f12cc62e6d60bef3e7c13b3f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="service-health-notifications"></a>Hizmet durumu bildirimlerine
## <a name="overview"></a>Genel Bakış

Bu makalede, Azure portalını kullanarak hizmet durumu bildirimlerine görüntüleme gösterilmektedir.

Hizmet durumu bildirimlerine aboneliğinizi kaynaklarınıza etkileyen Azure ekibi tarafından yayımlanan hizmet sistem durumu iletilerini görüntülemek sağlar. Bu bildirimler, bir alt sınıfı etkinlik günlüğü olaylardır ve etkinlik günlüğü dikey penceresinde de bulunabilir. Hizmet durumu bildirimlerine bilgilendirici veya sınıf bağlı olarak tıklatılabilir olabilir.

Hizmet durumu bildirimlerine beş sınıfları şunlardır:  

- **Gerekli eylem:** zaman zaman size hesabınızla ilgili durum olağan dışı bir şey fark edebilirsiniz. Bu sorunu gidermek için sizinle birlikte çalışma gerekebilir. Yapılacak gerekir ya da eylemlerin ayrıntılı bir bildirim göndereceğiz veya ile Azure mühendislik veya destek personeline başvurun hakkında ayrıntılar.  
- **Yardımlı kurtarma:** bir olay oluştu ve mühendisleri onaylanıp etkisi hala yaşıyor. Mühendislik doğrudan hizmetlerinizi geri getirmek için sizinle çalışma gerekecektir.  
- **Olay:** olay etkileyen bir hizmet şu anda bir veya daha fazla aboneliğiniz kaynaklarında etkileyen.  
- **Bakım:** , bir veya daha fazla aboneliğiniz kaynaklarınıza etkileyebilecek bir planlı bakım etkinliği bildiren bir bildirim budur.  
- **Bilgi:** biz, bildirimler, gönderdiğinizde için saati kaynak kullanımınızı geliştirmenize yardımcı olabilecek potansiyel iyileştirmeler hakkında sizinle iletişim kurun.  
- **Güvenlik:** Acil güvenlik ilgili Azure üzerinde çalışan, solution(s) ilgili bilgiler.

Her hizmet sistem durumu bildirimi kapsam ve kaynaklarınıza etkisi hakkında ayrıntılı yürütecek. Ayrıntılar içerir:

Özellik adı | Açıklama
-------- | -----------
Kanalları | Aşağıdaki değerlerden biridir: "Yönetici", "İşlem"
correlationId | Genellikle bir GUID dize biçiminde değil. Olaylar, ile ait aynı uber eylemi genellikle aynı correlationıd değeri paylaşın.
eventDataId | Bir olayın benzersiz tanımlayıcı
EventName | Olay başlığıdır
düzeyi | Olay düzeyi. Aşağıdaki değerlerden birini: "Kritik", "Error"Uyarı",", "Bilgi" ve "Ayrıntılı"
resourceProviderName | Etkilenen kaynak için kaynak sağlayıcısının adı
Kaynak türü| Etkilenen kaynağın kaynak türü
alt durum | Genellikle HTTP durum kodu karşılık gelen REST çağrısı, ancak bu ortak değerleri gibi bir alt durum açıklayan diğer dizeleri de içerir: Tamam (HTTP durum kodu: 200), oluşturulan (HTTP durum kodu: 201), kabul edilen (HTTP durum kodu: 202), Hayır içeriği (HTTP durum kodu: 204), hatalı istek (HTTP durum kodu: 400), bulunamadı (HTTP durum kodu: 404), çakışma (HTTP durum kodu : 409), iç sunucu hatası (HTTP durum kodu: 500), hizmet kullanılamıyor (HTTP durum kodu: 503), ağ geçidi zaman aşımı (HTTP durum kodu: 504).
eventTimestamp | Olay işleme olay karşılık gelen isteği Azure hizmeti tarafından oluşturulan zaman damgası.
submissionTimestamp |   Olay sorgulama için kullanılabilir duruma zaman damgası.
subscriptionId | Bu olayın günlüğe yazıldığı Azure aboneliği
durum | İşlemin durumunu açıklayan dize. Bazı genel değerler şunlardır:, ilerleme, başarılı, başarısız, etkin, çözümlenmiş başlatıldı.
operationName | İşlemin adı.
category | "ServiceHealth"
resourceId | Etkilenen kaynağının kaynak kimliği.
Properties.Title | Bu iletişim için yerelleştirilmiş başlık. İngilizce varsayılan dildir.
Properties.Communication | HTML biçimlendirmesi iletişimi yerelleştirilmiş ayrıntıları. İngilizce varsayılandır.
Properties.incidentType | Olası değerler: AssistedRecovery, ActionRequired, bilgi, olay, bakım, güvenlik
Properties.trackingId | Bu olay ile ilişkili olay tanımlar. Bir olaya ilgili olayları ilişkilendirmek için bunu kullanın.
Properties.impactedServices | Hizmetlerin ve olaydan etkilenen bölgeler açıklar bir kaçış karakterli JSON blobu. Her biri bir ServiceName ve her biri bir RegionName sahip ImpactedRegions listesini sahip hizmetlerin listesini.
Properties.defaultLanguageTitle | İngilizce dilinde iletişim
Properties.defaultLanguageContent | Html biçimlendirmesi veya düz metin olarak İngilizce dilinde iletişim
Properties.Stage | AssistedRecovery, ActionRequired, bilgi, olay, güvenlik için olası değerler: etkin, olan çözümlendi. Bakım için oldukları: etkin, planlanmış, devam ediyor, iptal edildi, Rescheduled, çözümlenmiş, tamamlandı
Properties.communicationId | Bu olay iletişimi ilişkilidir.


## <a name="viewing-your-service-health-notifications-in-the-azure-portal"></a>Azure portalında, hizmet durumu bildirimlerine görüntüleme
1.  İçinde [portal](https://portal.azure.com), gitmek **İzleyici** hizmeti

    ![İzleme](./media/monitoring-service-notifications/home-monitor.png)
2.  Tıklatın **İzleyici** İzleyici dikey penceresini açmak için seçeneği. Bu dikey pencere tüm izleme ayarlarınızı ve verilerinizi tek bir birleştirilmiş görünümde gösterir. İlk olarak **Etkinlik günlüğü** bölümü açılır.

3.  Şimdi tıklayın **hizmet bildirimleri** bölümü

    ![İzleme](./media/monitoring-service-notifications/service-health-summary.png)
4.  Daha fazla ayrıntı görüntülemek için çizgi öğelerini birini tıklatın

5. Tıklayın **+ etkinlik günlüğü uyarı Ekle** bu tür gelecekteki hizmet bildirimleri için bildirim emin olmak için bildirimleri almak için işlemi. Uyarıları hizmeti bildirimlerinin yapılandırma hakkında daha fazla bilgi için [burayı tıklatın](monitoring-activity-log-alerts-on-service-notifications.md)

## <a name="next-steps"></a>Sonraki Adımlar:
Alma [uyarı bildirimleri hizmeti sistem durumu bildirimi her](monitoring-activity-log-alerts-on-service-notifications.md) nakledilir  
Daha fazla bilgi edinmek [etkinlik günlüğü uyarıları](monitoring-activity-log-alerts.md)
