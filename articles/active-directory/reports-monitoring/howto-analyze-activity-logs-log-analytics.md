---
title: Azure İzleyici günlüklerini kullanarak Azure Active Directory etkinlik günlüklerini çözümleme | Microsoft Docs
description: Azure İzleyici günlüklerini kullanarak Azure Active Directory etkinlik günlükleri analiz etmeyi öğrenin
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 4535ae65-8591-41ba-9a7d-b7f00c574426
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 04/18/2019
ms.author: markvi
ms.reviewer: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4145ea2162cdd56deb8bbdcfa81d5a90a2ed9382
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2019
ms.locfileid: "67611565"
---
# <a name="analyze-azure-ad-activity-logs-with-azure-monitor-logs"></a>Azure AD çözümleme Azure İzleyici günlükleri ile etkinlik günlükleri

Çalıştırdıktan sonra [Azure AD tümleştirme etkinlik günlükleri ile Azure İzleyici günlüklerine](howto-integrate-activity-logs-with-log-analytics.md), ortamınız hakkında Öngörüler elde etmek için Azure İzleyici günlüklerine gücünü kullanın. Ayrıca yükleyebilirsiniz [oturum Analiz görünümleri için Azure AD etkinlik günlükleri](howto-install-use-log-analytics-views.md) ortamınıza erişmek için Denetim ve oturum açma olaylarını önceden oluşturulmuş raporları.

Bu makalede, Azure analiz etmeyi öğrenin AD etkinlik günlüklerini Log Analytics çalışma alanınızda. 

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="prerequisites"></a>Önkoşullar 

Örneği takip etmek için ihtiyacınız vardır:

* Azure aboneliğinizdeki bir Log Analytics çalışma. Bilgi edinmek için nasıl [Log Analytics çalışma alanı oluşturma](https://docs.microsoft.com/azure/log-analytics/log-analytics-quick-create-workspace).
* İlk olarak, adımları tamamlamak [rota Azure AD etkinlik günlüklerini Log Analytics çalışma alanınıza](howto-integrate-activity-logs-with-log-analytics.md).

## <a name="navigate-to-the-log-analytics-workspace"></a>Log Analytics çalışma alanına gidin

1. [Azure Portal](https://portal.azure.com) oturum açın. 

2. Seçin **Azure Active Directory**ve ardından **günlükleri** gelen **izleme** bölümü, Log Analytics çalışma alanını açın. Çalışma alanı bir varsayılan sorgu ile açılır.

    ![Varsayılan sorgu](./media/howto-analyze-activity-logs-log-analytics/defaultquery.png)


## <a name="view-the-schema-for-azure-ad-activity-logs"></a>Azure AD etkinlik şemasını görüntülemek günlükleri

Günlükleri depoya **AuditLogs** ve **SigninLogs** çalışma alanında Tablo. Bu tablo şemasını görüntülemek için:

1. Önceki bölümde varsayılan sorgu görünümünde seçin **şema** ve çalışma alanını genişletin. 

2. Genişletin **günlük yönetimi** bölümünde ve ya da genişletin **AuditLogs** veya **SignInLogs** günlük şemasını görüntülemek için.
    ![Denetim günlükleri](./media/howto-analyze-activity-logs-log-analytics/auditlogschema.png) ![Signın günlükleri](./media/howto-analyze-activity-logs-log-analytics/signinlogschema.png)

## <a name="query-the-azure-ad-activity-logs"></a>Sorgu Azure AD etkinlik günlükleri

Çalışma alanınızda günlükleri sahip olduğunuza göre artık sorguları bunlara karşı çalıştırabilirsiniz. Örneğin, son bir hafta içinde kullanılan en iyi uygulamaları almak için varsayılan sorguyu seçin ve aşağıdaki ile değiştirin **çalıştırın**

```
SigninLogs 
| where CreatedDateTime >= ago(7d)
| summarize signInCount = count() by AppDisplayName 
| sort by signInCount desc 
```

Geçen haftaya göre üst denetim olaylarını almak için aşağıdaki sorguyu kullanın:

```
AuditLogs 
| where TimeGenerated >= ago(7d)
| summarize auditCount = count() by OperationName 
| sort by auditCount desc 
```
## <a name="alert-on-azure-ad-activity-log-data"></a>Azure AD etkinlik günlüğü verileri temelinde uyar

Sorgunuzda uyarıları da ayarlayabilirsiniz. Örneğin, 10'dan fazla olduğunda bir uyarı yapılandırmak için uygulamaları son bir hafta içinde olarak kullanılmıştır:

1. Çalışma alanından seçin **uyarı ayarlama** açmak için **oluşturma kuralı** sayfası.

    ![Uyarı ayarlama](./media/howto-analyze-activity-logs-log-analytics/setalert.png)

2. Varsayılan seçin **Uyarı ölçütleri** uyarı ve güncelleştirme oluşturulan **eşiği** 10 varsayılan ölçümündeki.

    ![Uyarı ölçütleri](./media/howto-analyze-activity-logs-log-analytics/alertcriteria.png)

3. Bir ad ve uyarı için bir açıklama girin ve önem derecesini seçin. Bizim örneğimizde, biz ayarlayabilirsiniz **bilgilendirici**.

4. Seçin **eylem grubu** , uyarılmak sinyali oluştuğunda. Web kancaları, Azure işlevleri veya mantıksal uygulamalar'ı kullanarak eyleme otomatikleştirmek veya e-posta veya SMS mesajı aracılığıyla ekibinizi bilgilendirin seçebilirsiniz. Daha fazla bilgi edinin [grupları Azure portalında uyarı oluşturma ve yönetme](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-action-groups).

5. Uyarı yapılandırıldıktan sonra seçin **Oluştur uyarı** etkinleştirin. 

## <a name="install-and-use-pre-built-views-for-azure-ad-activity-logs"></a>Yükleyip önceden oluşturulmuş görünümler için Azure AD etkinlik günlükleri

Azure AD etkinlik için önceden oluşturulmuş bir log analytics görünümleri de indirebilirsiniz günlükleri. Görünümler, Denetim ve oturum açma olayları ile ilgili yaygın senaryolar için ilgili çeşitli raporlar sağlar. Ayrıca tüm raporların, sağlanan verilerin önceki bölümde açıklanan adımları kullanarak uyarabilir.

* **Azure AD hesabı olayları sağlama**: Bu görünümde sağlanan yeni kullanıcı sayısı gibi sağlama etkinliği denetim ile ilgili raporlar gösterilir ve sağlama hataları, kullanıcı sayısı güncelleştirildi ve güncelleştirme hataları ve kullanıcılar da sağlanan ve karşılık gelen hataları sayısı.    
* **Oturum açma olayları**: Bu görünümde, oturum açma etkinliği, uygulama, kullanıcı, cihaz yanı sıra oturum açma sayısı zamanla izleme Özet görünümünü tarafından oturum açma işlemleri gibi izleme ile ilgili en ilgili raporlar gösterilir.
* **Kullanıcı onayı gerçekleştirme**: Bu görünüm, gibi tüm onay tabanlı uygulamalar için uygulama tarafından oturum açma yanı sıra kullanıcı, oturum açma işlemleri tarafından izin verilen kullanıcılar tarafından izin verir. kullanıcı onayı için ilgili raporları gösterir. 

Bilgi nasıl [yüklemek ve log analytics görünümleri kullanmak için Azure AD etkinlik günlüklerini](howto-install-use-log-analytics-views.md). 


## <a name="next-steps"></a>Sonraki adımlar

* [Azure İzleyici günlüklerine sorguları kullanmaya başlama](https://docs.microsoft.com/azure/log-analytics/query-language/get-started-queries)
* [Azure portalında uyarı gruplarını oluşturma ve yönetme](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-action-groups)
* [Yükleme ve Azure Active Directory için log analytics görünümleri kullanma](howto-install-use-log-analytics-views.md)
