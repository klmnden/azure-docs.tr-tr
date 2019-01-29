---
title: Azure Active Directory günlükleri Azure İzleyici (Önizleme) kullanarak Log analytics'e Stream | Microsoft Docs
description: Azure İzleyicisi'ni (Önizleme) kullanarak Azure Active Directory günlükleri Log Analytics ile tümleştirmeyi öğrenin
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: daveba
editor: ''
ms.assetid: 2c3db9a8-50fa-475a-97d8-f31082af6593
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: ad01784f3a808be4a27c97934b7ce4e83de8cdc2
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55168490"
---
# <a name="integrate-azure-ad-logs-with-log-analytics-using-azure-monitor-preview"></a>Azure İzleyicisi'ni (Önizleme) kullanarak Log Analytics ile Azure AD günlükleri tümleştirme

Log Analytics, çeşitli veri kaynakları arasında bağıntı gerçekleştirmek belirli olayları bulmak ve eğilimleri analiz için veri sağlar. Azure ad ile tümleştirme, Log Analytics'te etkinlik günlükleri artık gibi görevleri gerçekleştirebilirsiniz:

 * Azure Güvenlik Merkezi tarafından yayımlanan güvenlik günlükleri, Azure AD oturum açma günlükleri karşılaştırın

 * Azure Application ınsights'ı uygulama performans verilerini ilişkilendirerek, uygulamanızın oturum açma sayfasında performans sorunlarını giderin.  

Ignite oturumunda aşağıdaki video Azure AD'ye günlükler pratik kullanıcı senaryoları için Log Analytics kullanmanın avantajları gösterir.

> [!VIDEO https://www.youtube.com/embed/MP5IaCTwkQg?start=1894]

Bu makalede, Azure Active Directory (Azure AD) günlüklerini Azure İzleyicisi'ni kullanarak Log Analytics ile tümleştirme konusunda bilgi edinin.

## <a name="supported-reports"></a>Desteklenen raporlar

Etkinlik günlükleri denetim ve oturum açma etkinlik günlüklerini Log Analytics'e daha fazla analiz için yönlendirebilir. 

* **Denetim günlükleri**: [Denetim günlükleri Etkinlik Raporu](concept-audit-logs.md) kiracınızda gerçekleştirilen her görevin geçmişine erişmenizi sağlar.
* **Oturum açma günlükleri**: İle [oturum açma etkinliği raporunu](concept-sign-ins.md), Denetim günlüklerinde bildirilen görevleri gerçekleştiren belirleyebilirsiniz.

> [!NOTE]
> B2C ile ilgili denetim ve oturum açma işlemleri etkinlik günlükleri şu an için desteklenmemektedir.
>

## <a name="prerequisites"></a>Önkoşullar 

Bu özelliği kullanmak için şunlara ihtiyacınız vardır:

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz denemeye kaydolabilirsiniz](https://azure.microsoft.com/free/).
* Azure AD kiracısı.
* Azure AD kiracısında *genel yönetici* veya *güvenlik yöneticisi* olan bir kullanıcı.
* Azure aboneliğinizdeki bir Log Analytics çalışma. Bilgi edinmek için nasıl [Log Analytics çalışma alanı oluşturma](https://docs.microsoft.com/azure/log-analytics/log-analytics-quick-create-workspace).

## <a name="send-logs-to-log-analytics"></a>Günlükleri Log Analytics’e gönderme

1. [Azure Portal](https://portal.azure.com) oturum açın. 

2. Seçin **Azure Active Directory** > **tanılama ayarları** -> **tanılama ayarı ekleme**. Belirleyebilirsiniz **dışa aktarma ayarları** gelen **denetim günlüklerini** veya **oturum açma işlemleri** tanılama ayarları yapılandırma sayfasına ulaşmak için sayfa.  
    
3. İçinde **tanılama ayarları** menüsünde **Log Analytics'e gönderme** onay kutusunu işaretleyin ve ardından **yapılandırma**.

4. Günlükleri göndermek istediğiniz Log Analytics çalışma alanını seçin veya sağlanan iletişim kutusunda yeni bir çalışma alanı oluşturun.  

5. Aşağıdakilerden birini veya ikisini birden yapın:
    * Log Analytics çalışma alanı için denetim günlüklerini göndermek için seçin **AuditLogs** onay kutusu. 
    * Log Analytics çalışma alanına oturum açma günlükleri göndermek için seçin **SignInLogs** onay kutusu.

6. Ayarları kaydetmek için **Kaydet**’i seçin.

    ![Tanılama ayarları](./media/howto-integrate-activity-logs-with-log-analytics/Configure.png)

7. Yaklaşık 15 dakika sonra olayları Log Analytics çalışma alanınıza aktarılır doğrulayın.

## <a name="next-steps"></a>Sonraki Adımlar

* [Analiz Azure AD etkinlik günlüklerini Log Analytics'te](howto-analyze-activity-logs-log-analytics.md)
* [Yükleme ve Azure Active Directory için Log Analytics görünümleri kullanma](howto-install-use-log-analytics-views.md)
