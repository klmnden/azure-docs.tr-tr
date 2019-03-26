---
title: Azure İzleyici günlüklerine (Önizleme) Azure Active Directory günlükleri Stream | Microsoft Docs
description: Azure Active Directory günlükleri, Azure İzleyici günlüklerine (Önizleme) ile tümleştirmeyi öğrenin
services: active-directory
documentationcenter: ''
author: MarkusVi
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
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 056a529101ffc39170ea057832bcd50b283505be
ms.sourcegitcommit: 70550d278cda4355adffe9c66d920919448b0c34
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58436278"
---
# <a name="integrate-azure-ad-logs-with-azure-monitor-logs-preview"></a>Azure İzleyici günlüklerine (Önizleme) ile Azure AD günlükleri tümleştirme

Azure İzleyici günlüklerine sayesinde belirli olayları bulmak, eğilimleri ve çeşitli veri kaynakları arasında bağıntı gerçekleştirmek için veri. Azure ad ile tümleştirme, Azure İzleyici günlüklerine etkinlik günlükleri artık gibi görevleri gerçekleştirebilirsiniz:

 * Azure Güvenlik Merkezi tarafından yayımlanan güvenlik günlükleri, Azure AD oturum açma günlükleri karşılaştırın

 * Azure Application ınsights'ı uygulama performans verilerini ilişkilendirerek, uygulamanızın oturum açma sayfasında performans sorunlarını giderin.  

Ignite oturumunda aşağıdaki video Azure AD'ye günlükler pratik kullanıcı senaryoları için Azure İzleyici günlüklerine kullanmanın avantajları gösterir.

> [!VIDEO https://www.youtube.com/embed/MP5IaCTwkQg?start=1894]

Bu makalede, Azure İzleyici ile Azure Active Directory (Azure AD) günlükleri tümleştirme öğrenin.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="supported-reports"></a>Desteklenen raporlar

Daha fazla analiz için Azure İzleyici günlüklerine denetim etkinlik günlüklerini ve oturum açma etkinlik günlüklerini yönlendirebilirsiniz. 

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

## <a name="send-logs-to-azure-monitor-logs"></a>Azure İzleyici günlüklerine günlükleri gönderme

1. [Azure Portal](https://portal.azure.com) oturum açın. 

2. Seçin **Azure Active Directory** > **tanılama ayarları** -> **tanılama ayarı ekleme**. Belirleyebilirsiniz **dışa aktarma ayarları** gelen **denetim günlüklerini** veya **oturum açma işlemleri** tanılama ayarları yapılandırma sayfasına ulaşmak için sayfa.  
    
3. İçinde **tanılama ayarları** menüsünde **Log Analytics çalışma alanına gönderme** onay kutusunu işaretleyin ve ardından **yapılandırma**.

4. Günlükleri göndermek istediğiniz Log Analytics çalışma alanını seçin veya sağlanan iletişim kutusunda yeni bir çalışma alanı oluşturun.  

5. Aşağıdakilerden birini veya ikisini birden yapın:
    * Log Analytics çalışma alanı için denetim günlüklerini göndermek için seçin **AuditLogs** onay kutusu. 
    * Log Analytics çalışma alanına oturum açma günlükleri göndermek için seçin **SignInLogs** onay kutusu.

6. Ayarları kaydetmek için **Kaydet**’i seçin.

    ![Tanılama ayarları](./media/howto-integrate-activity-logs-with-log-analytics/Configure.png)

7. Yaklaşık 15 dakika sonra olayları Log Analytics çalışma alanınıza aktarılır doğrulayın.

## <a name="next-steps"></a>Sonraki Adımlar

* [Azure AD çözümleme Azure İzleyici günlükleri ile etkinlik günlükleri](howto-analyze-activity-logs-log-analytics.md)
* [Yükleme ve Azure Active Directory için log analytics görünümleri kullanma](howto-install-use-log-analytics-views.md)
