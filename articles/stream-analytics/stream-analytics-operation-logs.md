---
title: "İşlemi ile hata ayıklama ve hizmet günlüklerini akış analizi | Microsoft Docs"
description: "Nasıl kullanılır Stream Analytics işlem günlükleri"
keywords: "Hizmet Günlükleri"
services: stream-analytics
documentationcenter: 
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: a2ed9676-f0bd-4398-87c8-a592779ac728
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: 2ee0871752dc2a3da345339fb826340d44ae48d7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="debug-stream-analytics-jobs-using-service-and-operation-logs"></a>Stream Analytics işleri hizmet ve işlem günlüklerini kullanarak hata ayıklama
Tüm Azure hizmetlerini işletimsel günlük iletilerini için kullanıcılar için yönetim işlemleriyle ilgili kayıt ayrıntılarını sağlayın. Azure akış analizi, bu bilgiler hata ayıklama işi durumunu, işin ilerleme durumunu görüntüleme gibi amacıyla kullanılabilir ve gelen zaman içinde bir işin ilerleme durumunu izlemek için hata iletileri çıkış işleme için başlatın.

## <a name="find-operation-logs-in-the-azure-portal"></a>Azure portalında işlem günlüklerini bulma
İşlem günlükleri iki yolla erişilebilir:  

* Stream Analytics işi Panosu  
* Klasik Azure portalındaki yönetim Hizmetleri'nde  

## <a name="dashboard-of-the-stream-analytics-job"></a>Stream Analytics işi Panosu
Akış analizi işi karşılık gelen günlüklerini bağlantı iş Pano sekmesinde görüntülenir. Bu bağlantıya tıklayın, belirli bir iş son günlükleri gösterir şekilde filtreler ayarlar.

  ![Yönetim Hizmetleri günlükleri seçin](./media/stream-analytics-operation-logs/01-stream-analytics-operation-logs.png)  

## <a name="management-services"></a>Yönetim Hizmetleri
Akış analizi ve klasik Azure portalındaki diğer hizmetler için el ile işlem günlükleri gezinmek için:

1. Tıklayın **Yönetim Hizmetleri** içinde [Klasik Azure portalı](https://manage.windowsazure.com).
2. Seçin **Stream Analytics** için **türü** ve işin adını **hizmet adı**.  
   
   ![Akış analizi seçin](./media/stream-analytics-operation-logs/02-stream-analytics-operation-logs.png)  

## <a name="find-audit-logs-in-the-azure-portal"></a>Azure portalında denetim günlüklerini bulma
Azure portalında Stream Analytics işiniz için işlem günlüklerini bulmak için tıklatın **Gözat** ve ardından **denetim günlüklerini**.

  ![Azure portal Stream Analytics seçin](./media/stream-analytics-operation-logs/06-stream-analytics-operation-logs.png)  

Bu, tüm kaynaklar için son yedi gün olaylarından aboneliğinizde gösteren bir görüntü açar.  Tıklayarak belirt tür ya da zaman çerçevesi olayları görmek için filtreleyebilirsiniz **filtre** komutu.

  ![Azure portal Stream Analytics seçin](./media/stream-analytics-operation-logs/07-stream-analytics-operation-logs.png)  

## <a name="get-log-details"></a>Günlüğü ayrıntılarının alınması
Zaman aralığı ve işinizi günlükleri görüntülemek için durum göre filtre uygulayabilirsiniz.

Azure portalında tıklayın **ayrıntıları** seçili olay hakkında daha fazla ayrıntı görüntülemek için pencerenin altındaki düğmesi. 

  ![Ayrıntılarını seçin](./media/stream-analytics-operation-logs/03-stream-analytics-operation-logs.png)  

Azure portalında bir günlük girişi içindeki ayrıntılı olayları görmek için tıklayın.

  ![Azure portal ayrıntılarını seçin](./media/stream-analytics-operation-logs/08-stream-analytics-operation-logs.png)  

Buradan, açtığınız **ayrıntı** görüntülemek olayda tıklayarak.

  ![Azure portal ayrıntılarını seçin](./media/stream-analytics-operation-logs/09-stream-analytics-operation-logs.png)  

## <a name="debug-a-failed-job"></a>Başarısız bir işi hata ayıklama
Azure portalında arama simgesine ve 'failed' türü üzerinde'ı tıklatın. Bu, hataları olan tüm günlükleri sonucunu verir. 

  ![Başarısız bir işi hata ayıklama](./media/stream-analytics-operation-logs/04-stream-analytics-operation-logs.png)  

Azure portalında görüntülemek için ileti düzeyine göre filtreleyebilirsiniz **kritik** olaylar.

  ![Azure portal hata ayıklama](./media/stream-analytics-operation-logs/10-stream-analytics-operation-logs.png)  

Hataları herhangi birini seçin ve tıklayın **ayrıntıları** hata hakkında daha fazla bilgi için.  Bazı hata iletileri de sorunu en aza hakkında bilgi sağlar. 

  ![İşlem ayrıntıları](./media/stream-analytics-operation-logs/05-stream-analytics-operation-logs.png)  

İle iletişime geçmeniz durumunda [Destek](https://azure.microsoft.com/support/options/) veya takımı bilgilerini [MSDN Forumu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics), Lütfen işlem ayrıntıları özellikle dikkat **bağıntı kimliği**. 

## <a name="get-help"></a>Yardım alın
Daha fazla yardım için [Azure Stream Analytics forumumuzu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) deneyin.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Stream Analytics'e giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)

