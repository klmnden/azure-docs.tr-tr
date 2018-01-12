---
title: Azure hizmet durumu bildirimlerine nelerdir? | Microsoft Docs
description: "Hizmet durumu bildirimlerine, Microsoft Azure tarafından yayınlanan hizmet sistem durumu iletileri görüntülemenize olanak sağlar."
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
ms.openlocfilehash: 4a95e9882515e6a2861292829a44847e11f39063
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2018
---
# <a name="view-service-health-notifications-by-using-the-azure-portal"></a>Azure portalını kullanarak hizmet durumu bildirimlerine görüntüleme

Hizmet durumu bildirimlerine Azure tarafından yayımlanan ve aboneliğinizi altındaki kaynakları hakkında bilgi içerir. Bu bildirimler bir alt sınıfı etkinlik günlüğü olaylardır ve ayrıca etkinlik günlüğünde bulunabilir. Hizmet durumu bildirimlerine bilgilendirici veya tıklatılabilir sınıfı bağlı olabilir.

Hizmet durumu bildirimlerine çeşitli sınıfları şunlardır:  

- **Eylem gerekli:** Azure olağan dışı bir şey hesabınızdaki gerçekleşir ve bu sorunu gidermek için sizinle birlikte çalışma fark. Azure uygulamanız gereken eylemlerin veya Azure mühendislik veya Desteğe başvurma ya da ayrıntılı bir bildirim gönderir.  
- **Kurtarma destekli:** bir olay oluştu ve mühendisleri onaylanıp etkisi hala yaşıyor. Azure mühendislik doğrudan hizmetleriniz için tam sistem geri yükleme için sizinle çalışma gerekiyor.  
- **Olay:** hizmet etkiler bir olay şu anda bir veya daha fazla aboneliğiniz kaynaklarında etkileyen.  
- **Bakım:** bir veya daha fazla aboneliğiniz kaynaklarınıza etkileyebilecek bir planlı bakım etkinliği.  
- **Bilgi:** olası en iyi duruma getirme yardımcı olabilecek geliştirmek, kaynak kullanımı. 
- **Güvenlik:** Azure üzerinde çalışan çözümlerinizi Acil güvenlik ilgili bilgiler.

Her hizmet sistem durumu bildirimi kapsam ve kaynaklarınıza etkisi hakkında ayrıntılar içerir. Ayrıntıları içerir:

Özellik adı | Açıklama
-------- | -----------
kanallar | Aşağıdaki değerlerden birini: **yönetici** veya **işlemi**.
correlationId | Genellikle bir GUID dize biçiminde. Aynı işlemi genellikle ait olayları aynı correlationıd değeri paylaşır.
eventDataId | Bir olay benzersiz tanımlayıcısı.
EventName | Bir olay başlığı.
düzey | Bir olay düzeyi. Aşağıdaki değerlerden birini: **kritik**, **hata**, **uyarı** veya **bilgilendirici**.
resourceProviderName | Etkilenen kaynak için kaynak sağlayıcı adı.
Kaynak türü| Etkilenen kaynağın kaynak türü.
alt durum | Genellikle HTTP durum kodu karşılık gelen REST çağrısı, ancak alt açıklayan diğer dizeleri de içerir. Örneğin: Tamam (HTTP durum kodu: 200), oluşturulan (HTTP durum kodu: 201), kabul edilen (HTTP durum kodu: 202), Hayır içeriği (HTTP durum kodu: 204), hatalı istek (HTTP durum kodu: 400), bulunamadı (HTTP durum kodu: 404), çakışma (HTTP durum kodu: 409), iç sunucu Hata (HTTP durum kodu: 500), hizmet kullanılamıyor (HTTP durum kodu: 503) ve ağ geçidi zaman aşımı (HTTP durum kodu: 504).
eventTimestamp | Olay işleme olaya karşılık gelen isteği Azure hizmeti tarafından oluşturulan zaman damgası.
submissionTimestamp | Olay sorgulama için kullanılabilir duruma zaman damgası.
subscriptionId | Bu olayın günlüğe yazıldığı Azure aboneliği.
durum | İşlemin durumunu açıklayan dize. Bazı genel değerler şunlardır: **başlatıldı**, **sürüyor**, **başarılı**, **başarısız**, **etkin**, ve **Çözülmüş**.
operationName | İşlemin adı.
category | Bu özellik her zaman olduğu **ServiceHealth**.
resourceId | Etkilenen kaynağının kaynak kimliği.
Properties.Title | Bu iletişim için yerelleştirilmiş başlık. İngilizce varsayılandır.
Properties.Communication | HTML biçimlendirmesi iletişimi yerelleştirilmiş ayrıntıları. İngilizce varsayılandır.
Properties.incidentType | Aşağıdaki değerlerden birini: **AssistedRecovery**, **ActionRequired**, **bilgi**, **olay**,  **Bakım**, veya **güvenlik**.
Properties.trackingId | Bu olay ilişkili olduğu olay. Bir olaya ilgili olayları ilişkilendirmek için bunu kullanın.
Properties.impactedServices | Hizmetlerin ve olaydan etkilenen bölgeler açıklar kaçış karakterli bir JSON blobu. Özelliği Hizmetleri, her biri listesini içeren bir **ServiceName**ve her biri etkilenen bölgelerin bir listesi bir **RegionName**.
Properties.defaultLanguageTitle | İngilizce dilinde iletişim.
Properties.defaultLanguageContent | HTML biçimlendirmesi veya düz metin olarak İngilizce dilinde iletişim.
Properties.Stage | Olası değerler için **AssistedRecovery**, **ActionRequired**, **bilgi**, **olay**, ve **güvenlik**  olan **etkin** veya **çözümlenmiş**. İçin **Bakım**, bunlar: **etkin**, **planlanan**, **devam ediyor**, **iptal edildi**, **Yeniden**, **çözülmüş**, veya **tam**.
Properties.communicationId | Bu olay ilişkilendirildiği iletişim.


## <a name="view-your-service-health-notifications-in-the-azure-portal"></a>Hizmet durumu bildirimlerine Azure portalında görüntüleyin
1.  İçinde [Azure portal](https://portal.azure.com)seçin **İzleyici**.

    ![Seçili İzleyicisi ile ekran görüntüsü, Azure portal menüsü](./media/monitoring-service-notifications/home-monitor.png)

    Azure İzleyicisi, izleme ayarları ve verileri birlikte bir birleştirilmiş görünüme getirir. İlk olarak **Etkinlik günlüğü** bölümü açılır.

3.  Seçin **uyarıları**.

    ![Seçili uyarıları ile İzleyici ekran etkinlik günlüğü](./media/monitoring-service-notifications/service-health-summary.png)
4. Seçin **+ etkinlik günlüğü uyarı Ekle**ve gelecekteki hizmet bildirimleri için bildirim emin olmak için bir alarm ayarlayın. Daha fazla bilgi için bkz: [etkinlik günlüğü uyarı hizmeti bildirimlerinin oluşturma](monitoring-activity-log-alerts-on-service-notifications.md).

## <a name="next-steps"></a>Sonraki adımlar
Alma [uyarı bildirimleri hizmeti sistem durumu bildirimi her](monitoring-activity-log-alerts-on-service-notifications.md) nakledilir.  
Daha fazla bilgi edinmek [etkinlik günlüğü uyarıları](monitoring-activity-log-alerts.md).
