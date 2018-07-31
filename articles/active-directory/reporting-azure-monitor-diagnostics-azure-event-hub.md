---
title: "Öğretici: Bir Azure olay hub'ına Azure Active Directory günlüğü akışı yapma (önizleme) | Microsoft Docs"
description: Azure Tanılama'yı kullanarak Azure Active Directory günlüklerini bir olay hub'ına göndermeyi öğrenin (önizleme)
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 045f94b3-6f12-407a-8e9c-ed13ae7b43a3
ms.service: active-directory
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: compliance-reports
ms.date: 07/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: c4f5bf1b49d7f59b2bb938a7ddc53b96c1d354ef
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39240273"
---
# <a name="tutorial-stream-azure-active-directory-logs-to-an-azure-event-hub-preview"></a>Öğretici: Bir Azure olay hub'ına Azure Active Directory günlüğü akışı yapma (önizleme)

Bu öğreticide bir Azure olay hub'ına Azure Active Directory günlüğü akışı yapma amacıyla Azure İzleyici tanılama ayarlarını yapmayı öğreneceksiniz. Bu mekanizmayı kullanarak günlüklerinizi Splunk ve QRadar gibi üçüncü taraf SIEM araçlarıyla tümleştirebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar 

Gerekenler:

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz denemeye kaydolabilirsiniz](https://azure.microsoft.com/free/).
* Azure Active Directory kiracısı
* Kiracıda genel yönetici veya güvenlik yöneticisi olan bir kullanıcı
* Azure aboneliğinizde bir Event Hubs ad alanı ve olay hub'ı. Oluşturmayı öğrenmek için [buraya bakın](https://docs.microsoft.com/azure/event-hubs/event-hubs-create.md).

## <a name="archive-logs-to-event-hub"></a>Günlükleri olay hub'ında arşivleme

1. [Azure Portal](https://portal.azure.com) oturum açın. 
2. **Azure Active Directory** -> **Etkinlik** -> **Denetim günlükleri**'ne tıklayın. 
3. **Dışarı Aktarma Ayarları**'na tıklayarak Tanılama Ayarları dikey penceresini açın. Var olan ayarları değiştirmek istiyorsanız **Ayarı düzenle**'ye, yeni bir ayar eklemek istiyorsanız **Tanılama ayarı ekle**'ye tıklayın. En fazla üç ayar kullanabilirsiniz. 
    ![Dışarı aktarma ayarları](./media/reporting-azure-monitor-diagnostics-azure-event-hub/ExportSettings.png "Dışarı aktarma ayarları")

4. **Bir olay hub'ına akış yap** onay kutusunu işaretleyip **Event Hubs/Yapılandır**'a tıklayın.
5. Günlükleri yönlendirmek istediğiniz Azure aboneliğini ve Event Hubs ad alanını seçin. Aboneliğin ve Event Hubs ad alanının günlüklerin akışının yapıldığı Active Directory kiracısı ile ilişkilendirilmiş olması gerekir. Event Hubs alanında günlüklerin gönderilmesini istediğiniz olay hub'ını da belirtebilirsiniz. Olay hub'ı belirtilmezse ad alanında **insights-logs-audit** varsayılan adıyla yeni bir olay hub'ı oluşturulur.
6. Olay hub'ı yapılandırmasını kapatmak için **Tamam**'a tıklayın.
7. Denetim günlüklerini depolama hesabına göndermek için **AuditLogs** onay kutusunu işaretleyin. 
8. Oturum açma günlüklerini depolama hesabına göndermek için **SignInLogs** onay kutusunu işaretleyin.
9. Ayarı kaydetmek için **Kaydet**’e tıklayın.
    ![Tanılama ayarları](./media/reporting-azure-monitor-diagnostics-azure-event-hub/DiagnosticSettings.png "Tanılama ayarları")

10. Yaklaşık 15 dakika sonra olayların olay hub'ınızda görünüp görünmediğini kontrol edin. Bunu yapmak için portaldan olay hub'ına gidin ve **gelen iletiler** değerinin sıfırdan büyük olduğundan emin olun. 
    ![Denetim günlükleri](./media/reporting-azure-monitor-diagnostics-azure-event-hub/InsightsLogsAudit.png "Denetim günlükleri")


## <a name="access-data-from-event-hubs"></a>Verilere Event Hubs’dan erişme

Olay hub'ında görünen verilere iki farklı şekilde erişim sağlayabilirsiniz.

* **Verileri okumak için desteklenen bir SIEM aracını yapılandırma**: Çoğu araç olay hub'ındaki verileri okumak için olay hub'ı dizesine ve Azure aboneliğinizde belirli izinlere ihtiyaç duyar. Azure İzleyici tümleştirmesine sahip araçların bir bölümü aşağıda listelenmiştir:
    1. **Splunk**: Azure AD günlüklerini Splunk ile tümleştirme hakkında daha fazla bilgi için bkz. [Azure İzleyici'yi kullanarak Azure Active Directory günlüklerini Splunk ile tümleştirme](reporting-azure-monitor-diagnostics-splunk-integration.md).
    
    2. **IBM QRadar**: Microsoft Azure DSM ve Microsoft Azure Olay Hub'ı Protokolünü [IBM destek web sitesinden](http://www.ibm.com/support) indirebilirsiniz. [Azure ile tümleştirme hakkında daha fazla bilgiye buradan](https://www.ibm.com/support/knowledgecenter/SS42VS_DSM/c_dsm_guide_microsoft_azure_overview.html?cp=SS42VS_7.3.0) ulaşabilirsiniz.
    
    3. **SumoLogic**: SumoLogic'i olay hub'ı verilerini kullanacak şekilde ayarlamak için [bu yönergeleri](https://help.sumologic.com/Send-Data/Applications-and-Other-Data-Sources/Azure-Audit/02Collect-Logs-for-Azure-Audit-from-Event-Hub) izleyin. 

* **Özel bir aracı verileri okuyacak şekilde ayarlama**: Geçerli SIEM çözümünüz henüz Azure İzleyici tanılamaları için desteklenmiyorsa Olay hub'ı API'lerini kullanarak özel aracınızı ayarlayabilirsiniz. Daha fazla bilgi için bkz. [Olay hub'ı API'leri](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph).


## <a name="next-steps"></a>Sonraki adımlar

* [Azure İzleyici Tanılamaları'nı kullanarak Azure Active Directory günlüklerini Splunk ile tümleştirme](reporting-azure-monitor-diagnostics-splunk-integration.md)
* [Azure izleyici tanılamalarındaki denetim günlükleri şemasını yorumlama](reporting-azure-monitor-diagnostics-audit-log-schema.md)
* [Azure izleyici tanılamalarındaki oturum açma günlükleri şemasını yorumlama](reporting-azure-monitor-diagnostics-sign-in-log-schema.md)