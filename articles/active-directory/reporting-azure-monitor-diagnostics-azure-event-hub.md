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
ms.openlocfilehash: 50e7b7c35386bafe521078308a8809c4d3038dd8
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2018
ms.locfileid: "39501705"
---
# <a name="tutorial-stream-azure-ad-logs-to-an-azure-event-hub-preview"></a>Öğretici: Bir Azure olay hub'ına Azure AD günlüğü akışı yapma (önizleme)

Bu öğreticide bir Azure olay hub'ına Azure Active Directory (Azure AD) günlüğü akışı yapma amacıyla Azure İzleyici tanılama ayarlarını yapmayı öğreneceksiniz. Bu mekanizmayı kullanarak günlüklerinizi Splunk ve QRadar gibi üçüncü taraf Güvenlik Bilgileri ve Olay Yönetimi (SIEM) araçlarıyla tümleştirebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar 

Bu özelliği kullanmak için şunlara ihtiyacınız vardır:

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz denemeye kaydolabilirsiniz](https://azure.microsoft.com/free/).
* Azure AD kiracısı.
* Azure AD kiracısında *genel yönetici* veya *güvenlik yöneticisi* olan bir kullanıcı.
* Azure aboneliğinizde bir Event Hubs ad alanı ve bir olay hub'ı. [Olay hub'ı oluşturma](https://docs.microsoft.com/azure/event-hubs/event-hubs-create.md) hakkında bilgi edinin.

## <a name="archive-logs-to-an-event-hub"></a>Günlükleri olay hub'ında arşivleme

1. [Azure Portal](https://portal.azure.com) oturum açın. 

2. **Azure Active Directory** > **Etkinlik** > **Denetim günlükleri**'ni seçin. 

3. **Dışarı Aktarma Ayarları**'nı seçin.  
    
4. **Tanılama ayarları** bölmesinde aşağıdakilerden birini yapın:
    * Var olan ayarları değiştirmek için **Ayarı düzenleyin**'i seçin.
    * Yeni ayar eklemek için **Tanılama ayarı ekle**'yi seçin.  
      En fazla üç ayar kullanabilirsiniz.

      ![Dışarı aktarma ayarları](./media/reporting-azure-monitor-diagnostics-azure-event-hub/ExportSettings.png)

5. **Bir olay hub'ına akış yap** onay kutusunu ve ardından **Event Hubs/Yapılandır**'ı seçin.

6. Günlükleri yönlendirmek istediğiniz Azure aboneliğini ve Event Hubs ad alanını seçin.  
    Aboneliğin ve Event Hubs ad alanının günlüklerin akışının yapıldığı Azure AD kiracısı ile ilişkilendirilmiş olması gerekir. Event Hubs alanında günlüklerin gönderilmesini istediğiniz olay hub'ını da belirtebilirsiniz. Olay hub'ı belirtilmezse ad alanında **insights-logs-audit** varsayılan adıyla yeni bir olay hub'ı oluşturulur.

7. Olay hub'ı yapılandırmasını kapatmak için **Tamam**'ı seçin.

8. Aşağıdakilerden birini veya ikisini birden yapın:
    * Denetim günlüklerini depolama hesabına göndermek için **AuditLogs** onay kutusunu işaretleyin. 
    * Oturum açma günlüklerini depolama hesabına göndermek için **SignInLogs** onay kutusunu işaretleyin.

9. Ayarları kaydetmek için **Kaydet**’i seçin.

    ![Tanılama ayarları](./media/reporting-azure-monitor-diagnostics-azure-event-hub/DiagnosticSettings.png)

10. Yaklaşık 15 dakika sonra olayların olay hub'ınızda görüntülenip görüntülenmediğini kontrol edin. Bunu yapmak için portaldan olay hub'ına gidin ve **gelen iletiler** değerinin sıfırdan büyük olduğundan emin olun. 

    ![Denetim günlükleri](./media/reporting-azure-monitor-diagnostics-azure-event-hub/InsightsLogsAudit.png)

## <a name="access-data-from-your-event-hub"></a>Verilere olay hub'ınızdan erişme

Olay hub'ında görüntülenen verilere iki farklı şekilde erişebilir ve onları okuyabilirsiniz:

* **Desteklenen bir SIEM aracını yapılandırın**. Çoğu araç olay hub'ındaki verileri okumak için olay hub'ı bağlantı dizesine ve Azure aboneliğinizde belirli izinlere ihtiyaç duyar. Azure İzleyici tümleştirmesine sahip üçüncü taraf araçlarının bazıları şunlardır:
    * **Splunk**: Azure AD günlüklerini Splunk ile tümleştirme hakkında daha fazla bilgi için bkz. [Azure İzleyici'yi kullanarak Azure AD günlüklerini Splunk ile tümleştirme](reporting-azure-monitor-diagnostics-splunk-integration.md).
    
    * **IBM QRadar**: DSM ve Azure Olay Hub'ı Protokolünü [IBM destek](http://www.ibm.com/support) sayfasından indirebilirsiniz. Azure tümleştirmesi hakkında daha fazla bilgi için [IBM QRadar Security Intelligence Platform 7.3.0](https://www.ibm.com/support/knowledgecenter/SS42VS_DSM/c_dsm_guide_microsoft_azure_overview.html?cp=SS42VS_7.3.0) sitesini ziyaret edin.
    
    * **Sumo Logic**: Sumo Logic uygulamasını olay hub'ındaki verileri kullanacak şekilde ayarlamak için bkz. [Bir olay hub'ındaki Azure etkinlik günlüklerini toplama](https://help.sumologic.com/Send-Data/Applications-and-Other-Data-Sources/Azure-Audit/02Collect-Logs-for-Azure-Audit-from-Event-Hub). 

* **Özel araçları ayarlama**. Geçerli SIEM çözümünüz henüz Azure İzleyici tanılamaları için desteklenmiyorsa Event Hubs API'lerini kullanarak özel aracınızı ayarlayabilirsiniz. Daha fazla bilgi için bkz. [Bir olay hub'ındaki verileri almaya başlama](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph).


## <a name="next-steps"></a>Sonraki adımlar

* [Azure İzleyici kullanarak Azure AD günlüklerini Splunk ile tümleştirme](reporting-azure-monitor-diagnostics-splunk-integration.md)
* [Azure İzleyici denetim günlükleri şemasını yorumlama](reporting-azure-monitor-diagnostics-audit-log-schema.md)
* [Azure İzleyici oturum açma günlükleri şemasını yorumlama](reporting-azure-monitor-diagnostics-sign-in-log-schema.md)
