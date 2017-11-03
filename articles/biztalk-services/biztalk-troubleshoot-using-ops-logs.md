---
title: "BizTalk Services işlem günlükleri kullanarak sorun giderme | Microsoft Docs"
description: "BizTalk Services işlem günlükleri kullanarak sorun giderin. MABS, WABS"
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 1081a9c6-58cc-4a76-8ac8-11e5e7a6ea27
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: c0c83361f94ffd9c30d7fcc551ff4b85ad7d6fa5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="biztalk-services-troubleshoot-using-operation-logs"></a>BizTalk Services: işlem günlükleri kullanarak sorun giderme

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

## <a name="what-are-the-operation-logs"></a>İşlem günlükleri nelerdir
İşlem günlükleri bir özelliktir Management Services, BizTalk Services de dahil olmak üzere Azure hizmetlerinizi üzerinde gerçekleştirilen işlemlerinin geçmiş günlükleri görüntülemenize izin verir Azure Klasik portalında kullanılabilir. Bu, BizTalk hizmeti aboneliğinizi 180 gün'a kadar yönetim işlemlerini ilgili geçmiş verileri görüntülemenize olanak sağlar.

> [!NOTE]
> Bu özellik yalnızca günlükleri hizmet başlatıldığı gibi yönetim işlemlerini BizTalk Services için desteklenen en fazla, vb. yakalar. Bunlar Azure Klasik portalından veya kullanarak gerçekleştirilen yedeklemiş gibi işlemleri izlenen [BizTalk hizmeti REST API'leri](http://msdn.microsoft.com/library/azure/dn232347.aspx). Yönetim Hizmetleri kullanılarak izlenen operations tam bir listesi için bkz: [Operations izlenen kullanarak Azure Yönetim Hizmetleri](#bizops).<br/><br/>
> Bu, BizTalk hizmeti çalışma zamanı (örneğin, ileti. köprüleri vb. tarafından işlenen) ilgili etkinlikleri için günlükleri yakalamaz. Bu günlükleri görüntülemek için BizTalk Services Portalı'ndan izleme görünümünü kullanın. Daha fazla bilgi için bkz: [izleme iletileri](http://msdn.microsoft.com/library/azure/hh949805.aspx).
> 
> 

## <a name="view-biztalk-services-operation-logs"></a>İşlem günlükleri BizTalk Services görünümü
1. Klasik Azure portalında seçin **Yönetim Hizmetleri**ve ardından **işlem günlükleri** sekmesi.
2. Abonelik, tarih aralığı, hizmet türüne (örneğin, BizTalk Services), hizmet adı veya (başarılı, başarısız) işleminin durumu gibi farklı parametreler göre günlükleri filtreleyebilirsiniz.
3. Filtrelenmiş listesini görüntülemek için onay işaretini seçin. Aşağıdaki resimde testbiztalkservice için ilgili etkinlikleri gösterir: ![işlem günlükleri görüntüleme][ViewLogs] 
4. Belirli bir işlemi hakkında daha fazla görüntülemek için satırı seçin ve'ı tıklatın **ayrıntıları** altındaki görev çubuğunda.

## <a name="bizops"></a>Azure Yönetim Hizmetleri'ni kullanarak izlenen işlemleri
Aşağıdaki tabloda Azure Yönetimi hizmetlerini kullanarak izlenen işlemleri listeler:

| İşlem adı | Görev |
| --- | --- |
| CreateBizTalkService |Yeni bir BizTalk hizmeti oluşturma işlemi |
| DeleteBizTalkService |BizTalk hizmeti silme işlemi |
| RestartBizTalkService |BizTalk hizmetini yeniden başlatma işlemi |
| StartBizTalkService |İşlemin bir BizTalk hizmetini başlatmak için |
| StopBizTalkService |İşlemin bir BizTalk hizmeti durdurmak için |
| DisableBizTalkService |BizTalk hizmeti devre dışı bırakma işlemi |
| EnableBizTalkService |İşlemin bir BizTalk hizmeti etkinleştirmek için |
| BackupBizTalkService |BizTalk hizmeti kurma yedekleme işlemi |
| RestoreBizTalkService |BizTalk hizmeti belirtilen yedekten geri yükleme işlemi |
| SuspendBizTalkService |BizTalk hizmeti askıya alma işlemi |
| ResumeBizTalkService |İşlemin bir BizTalk hizmeti sürdürmek için |
| ScaleBizTalkService |BizTalk hizmeti yukarı veya aşağı ölçeklendirme işlemi |
| ConfigUpdateBizTalkService |BizTalk hizmeti yapılandırmasını güncelleştirmek için güncelleştirme işlemi |
| ServiceUpdateBizTalkService |İşlemin yükseltin veya farklı bir sürüme BizTalk hizmeti düşürmek için |
| PurgeBackupBizTalkService |Saklama dönemi dışında BizTalk hizmeti yedeklerini temizlenecek işlemi |

## <a name="see-also"></a>Ayrıca Bkz.
* [BizTalk hizmeti yedekleme](http://go.microsoft.com/fwlink/p/?LinkID=325584)
* [BizTalk hizmeti yedekten geri yükleyin](http://go.microsoft.com/fwlink/p/?LinkID=325582)
* [BizTalk Services: Geliştirici, temel, standart ve Premium sürümler grafiği](http://go.microsoft.com/fwlink/p/?LinkID=302279)
* [BizTalk Services: Klasik portalı kullanarak Azure hazırlama](http://go.microsoft.com/fwlink/p/?LinkID=302280)
* [BizTalk Services: Durum Grafiğini hazırlama](http://go.microsoft.com/fwlink/p/?LinkID=329870)
* [BizTalk Services: Pano, İzleyici ve Ölçek sekmeleri](http://go.microsoft.com/fwlink/p/?LinkID=302281)
* [BizTalk Services: Azaltma](http://go.microsoft.com/fwlink/p/?LinkID=302282)
* [BizTalk Services: Verenin Adı ve Verenin Anahtarı](http://go.microsoft.com/fwlink/p/?LinkID=303941)
* [Azure BizTalk Services SDK'sını Kullanmaya Nasıl Başlarım](http://go.microsoft.com/fwlink/p/?LinkID=302335)

[ViewLogs]: ./media/biztalk-troubleshoot-using-ops-logs/Operation-Logs.png

