---
title: İşlem günlüklerini kullanarak BizTalk Hizmetleri sorunlarını giderme | Microsoft Docs
description: İşlem günlüklerini kullanarak BizTalk Hizmetleri sorunlarını giderin. MABS, WABS
services: biztalk-services
documentationcenter: ''
author: MandiOhlinger
manager: anneta
editor: ''
ms.assetid: 1081a9c6-58cc-4a76-8ac8-11e5e7a6ea27
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: e58a62761284e0671c0083d41f5dde4c13b32fe2
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/18/2019
ms.locfileid: "58108265"
---
# <a name="biztalk-services-troubleshoot-using-operation-logs"></a>BizTalk Services: İşlem günlüklerini kullanarak sorun giderme

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]
> 
> [!INCLUDE [Use APIs to manage MABS](../../includes/biztalk-services-retirement-azure-classic-portal.md)]

## <a name="what-are-the-operation-logs"></a>İşlem günlükleri nelerdir
İşlem günlükleri, BizTalk hizmetleri de dahil olmak üzere Azure hizmetleriniz üzerinde gerçekleştirilen işlemlerin sayısına geçmiş günlükleri görüntülemenize olanak tanıyan bir Yönetim Hizmetleri özelliğidir. Bu, BizTalk hizmeti aboneliğinizi 180 gün öncesine varan yönetim işlemlerini ilgili geçmiş verileri görüntülemesine olanak sağlar.

> [!NOTE]
> Bu özellik yalnızca günlükleri gibi hizmet başlatıldığı, BizTalk Hizmetleri, yönetim işlemleri için desteklenen en fazla vb. yakalar. Bu işlemler kullanılarak izlenir [BizTalk hizmeti REST API'leri](https://msdn.microsoft.com/library/azure/dn232347.aspx). Yönetim Hizmetleri kullanılarak izlenir işlemleri tam bir listesi için bkz. [Operations izlenen kullanarak Azure Yönetim Hizmetleri](#bizops).<br/><br/>
> Bu, BizTalk hizmeti çalışma zamanı (örneğin, iletiyi. Köprüsü ve benzeri tarafından işlenen) ilgili etkinlikleri için günlüklere yakalamaz. Bu günlükleri görüntülemek için izleme görünümünden BizTalk Hizmetleri portalını kullanın. Daha fazla bilgi için [izleme iletileri](https://msdn.microsoft.com/library/azure/hh949805.aspx).
> 
> 

## <a name="view-biztalk-services-operation-logs"></a>BizTalk Services işlem günlükleri görüntüle
1. Portalında **Yönetim Hizmetleri**ve ardından **işlem günlükleri** sekmesi.
2. Abonelik, tarih aralığı, hizmet türü (örneğin, BizTalk Hizmetleri), hizmet adı veya (başarılı, başarısız) işlemin durumu gibi farklı parametreler göre günlükleri filtreleyebilirsiniz.
3. Filtrelenmiş listeyi görüntülemek için onay işaretini seçin. Aşağıdaki görüntüde testbiztalkservice için ilgili etkinlikleri gösterir: ![İşlem günlüklerini görüntüle][ViewLogs] 
4. Belirli bir işlemi hakkında daha fazla görüntülemek için satırı seçin ve tıklayın **ayrıntıları** altındaki görev çubuğunda.

## <a name="bizops"></a>Azure Yönetim Hizmetleri ile izlenen işlemler
Aşağıdaki tabloda, Azure Yönetim Hizmetleri kullanılarak izlenir işlemleri listelenmektedir:

| İşlem Adı | Görev |
| --- | --- |
| CreateBizTalkService |Yeni BizTalk hizmeti oluşturma işlemi |
| DeleteBizTalkService |BizTalk hizmeti silme işlemi |
| RestartBizTalkService |BizTalk hizmetini yeniden başlatma işlemi |
| StartBizTalkService |BizTalk hizmeti başlatmak için işlemi |
| StopBizTalkService |BizTalk hizmeti durdurmak için işlemi |
| DisableBizTalkService |BizTalk hizmeti devre dışı bırakma işlemi |
| EnableBizTalkService |BizTalk hizmeti etkinleştirme işlemi |
| BackupBizTalkService |BizTalk hizmeti yedekleme işlemi |
| RestoreBizTalkService |BizTalk hizmeti belirtilen yedekten geri yükleme işlemi |
| SuspendBizTalkService |BizTalk hizmeti askıya alma işlemi |
| ResumeBizTalkService |BizTalk hizmeti sürdürmek için işlemi |
| ScaleBizTalkService |BizTalk hizmeti yukarı veya aşağı ölçeklendirme işlemi |
| ConfigUpdateBizTalkService |BizTalk hizmeti yapılandırmasını güncelleştirmek için işlemi |
| ServiceUpdateBizTalkService |Yükseltme veya düşürme farklı bir sürüme BizTalk hizmeti için işlem |
| PurgeBackupBizTalkService |BizTalk hizmeti saklama süresi dışında yedeklerini temizlemeye işlemi |

## <a name="see-also"></a>Ayrıca Bkz.
* [BizTalk hizmeti yedekleme](https://go.microsoft.com/fwlink/p/?LinkID=325584)
* [BizTalk hizmeti yedekten geri yükleyin.](https://go.microsoft.com/fwlink/p/?LinkID=325582)
* [BizTalk Services: Geliştirici, temel, standart ve Premium sürümler grafiği](https://go.microsoft.com/fwlink/p/?LinkID=302279)
* [BizTalk Services: Sağlama](https://go.microsoft.com/fwlink/p/?LinkID=302280)
* [BizTalk Services: Durum grafiğini hazırlama](https://go.microsoft.com/fwlink/p/?LinkID=329870)
* [BizTalk Services: Pano, İzleyici ve ölçek sekmeleri](https://go.microsoft.com/fwlink/p/?LinkID=302281)
* [BizTalk Services: Azaltma](https://go.microsoft.com/fwlink/p/?LinkID=302282)
* [BizTalk Services: Verenin adı ve verenin anahtarı](https://go.microsoft.com/fwlink/p/?LinkID=303941)
* [Azure BizTalk Services SDK'sını Kullanmaya Nasıl Başlarım](https://go.microsoft.com/fwlink/p/?LinkID=302335)

[ViewLogs]: ./media/biztalk-troubleshoot-using-ops-logs/Operation-Logs.png

