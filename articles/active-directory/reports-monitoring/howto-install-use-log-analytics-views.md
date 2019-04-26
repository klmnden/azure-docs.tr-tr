---
title: Yükleme ve Azure Active Directory için log analytics görünümleri kullanma | Microsoft Docs
description: Yükleme ve Azure Active Directory için log analytics görünümleri kullanma hakkında bilgi edinin
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 2290de3c-2858-4da0-b4ca-a00107702e26
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 04/18/2019
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 92a5d9539d254eb7d52e3277691c9d8ff5a41821
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60286817"
---
# <a name="install-and-use-the-log-analytics-views-for-azure-active-directory"></a>Yükleme ve Azure Active Directory için log analytics görünümleri kullanma

Azure AD kiracınızda Azure AD etkinlik günlüklerini arama ve analiz Azure Active Directory log analytics görünümleri yardımcı olur. Azure AD etkinlik günlükleri şunlardır:

* Denetim günlükleri: [Denetim günlükleri Etkinlik Raporu](concept-audit-logs.md) kiracınızda gerçekleştirilen her görevin geçmişine erişmenizi sağlar.
* Oturum açma günlükleri: İle [oturum açma etkinliği raporunu](concept-sign-ins.md), Denetim günlüklerinde bildirilen görevleri gerçekleştiren belirleyebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Log analytics görünümleri kullanmak için gerekir:

* Azure aboneliğinizdeki bir Log Analytics çalışma. Bilgi edinmek için nasıl [Log Analytics çalışma alanı oluşturma](https://docs.microsoft.com/azure/log-analytics/log-analytics-quick-create-workspace).
* İlk olarak, adımları tamamlamak [rota Azure AD etkinlik günlüklerini Log Analytics çalışma alanınıza](howto-integrate-activity-logs-with-log-analytics.md).
* Görünümleri indirme [GitHub deposu](https://aka.ms/AADLogAnalyticsviews) yerel bilgisayarınıza.

## <a name="install-the-log-analytics-views"></a>Log analytics görünümleri yükleyin

1. Log Analytics çalışma alanınıza gidin. Bunu yapmak için önce gidin [Azure portalında](https://portal.azure.com) seçip **tüm hizmetleri**. Tür **Log Analytics** seçin ve metin kutusu **Log Analytics çalışma alanları**. Önkoşulların bir parçası, etkinlik günlükleri yönlendirilen çalışma alanı seçin.
2. Seçin **Görünüm Tasarımcısı**seçin **alma** seçip **Dosya Seç** görünümleri kullanarak yerel bilgisayarınızdan içeri aktarmak için.
3. Önkoşullar ve da seçin, indirilen görünümleri seçin **Kaydet** içeri aktarma kaydetmek için. Bunu yapmak **Azure AD hesabı sağlama olayları** görünümü ve **oturum açma olayları** görünümü.

## <a name="use-the-views"></a>Görünümleri kullanma

1. Log Analytics çalışma alanınıza gidin. Bunu yapmak için önce gidin [Azure portalında](https://portal.azure.com) seçip **tüm hizmetleri**. Tür **Log Analytics** seçin ve metin kutusu **Log Analytics çalışma alanları**. Önkoşulların bir parçası, etkinlik günlükleri yönlendirilen çalışma alanı seçin.

2. Çalışma alanında olduğunuzda seçin **çalışma özeti**. Aşağıdaki üç görünüm görmeniz gerekir:

    * **Azure AD hesabı olayları sağlama**: Bu görünümde sağlanan yeni kullanıcı sayısı gibi sağlama etkinliği denetim ile ilgili raporlar gösterilir ve sağlama hataları, kullanıcı sayısı güncelleştirildi ve güncelleştirme hataları ve kullanıcılar da sağlanan ve karşılık gelen hataları sayısı.    
    * **Oturum açma olayları**: Bu görünümde, oturum açma etkinliği, uygulama, kullanıcı, cihaz yanı sıra oturum açma sayısı zamanla izleme Özet görünümünü tarafından oturum açma işlemleri gibi izleme ile ilgili en ilgili raporlar gösterilir.

3. Bu görünümler için bireysel raporlar hemen birini seçin. Ayrıca tüm rapor parametreleri uyarılar ayarlayabilirsiniz. Var olan her zaman bir oturum açma hatası örnek için bir uyarı ayarlayalım. Bunu yapmak için önce seçin **oturum açma olayları** görüntülenecek **zaman içinde oturum açma hataları** rapor ve ardından **Analytics** gerçek ile sorguyu Ayrıntılar sayfasını açmak için Raporun. 

    ![Ayrıntılar](./media/howto-install-use-log-analytics-views/details.png)


4. Seçin **ayarlamak uyarı**ve ardından **her özel günlük arama &lt;tanımsız mantık&gt;**  altında **Uyarı ölçütleri** bölümü. Bir oturum açma hatası olduğunda uyar istiyoruz, ayarlanması **eşiği** varsayılan uyarı mantığının **1** seçip **Bitti**. 

    ![Sinyal mantığını yapılandırma](./media/howto-install-use-log-analytics-views/configure-signal-logic.png)

5. Bir ad ve uyarı için bir açıklama girin ve önem derecesini ayarlayın **uyarı**.

    ![Kural oluşturma](./media/howto-install-use-log-analytics-views/create-rule.png)

6. Uyarı eylem grubu seçin. Genel olarak, bu e-posta veya SMS mesajı bildirmek istediğiniz takım olabilir veya Web kancaları, runbook'ları, İşlevler, logic apps veya harici ITSM çözümleriyle kullanarak otomatik bir görev olabilir. Bilgi edinmek için nasıl [Azure portalında Eylem grupları oluşturma ve yönetme](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-action-groups).

7. Seçin **uyarı kuralı oluştur** uyarı oluşturmak için. Artık var. her zaman bir oturum açma hatası uyarılırsınız.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure İzleyici günlükleri ile etkinlik günlüklerini analiz etme](howto-analyze-activity-logs-log-analytics.md)
* [Azure portalında Azure İzleyici günlüklerine kullanmaya başlama](https://docs.microsoft.com/azure/log-analytics/query-language/get-started-analytics-portal)