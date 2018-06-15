---
title: Azure Stream Analytics işleri için uyarıları izleme işlevini ayarlama
description: Bu makalede, izleme ve Azure akış analizi işleri için uyarıları ayarlamak için Azure Portalı'nı kullanmayı açıklar.
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/26/2017
ms.openlocfilehash: fb69f1b52e5b17dd6794c896969c29003a929c9b
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
ms.locfileid: "31412119"
---
# <a name="tutorial-set-up-alerts-for-azure-stream-analytics-jobs"></a>Öğretici: Azure akış analizi işleri için uyarıları ayarlama
Ölçüm belirttiğiniz bir koşul ulaştığında bir uyarı tetiklemek için uyarıları ayarlayın. Örneğin, aşağıdaki gibi bir koşul için bir uyarı ayarlayabilirsiniz:

`If there are zero input events in the last 5 minutes, send email notification to sa-admin@example.com`

Kuralları portal üzerinden ölçümleri üzerinde ayarlanabilir veya yapılandırılabilir [program aracılığıyla](https://code.msdn.microsoft.com/windowsazure/Receive-Email-Notifications-199e2c9a) işlem günlükleri veriler üzerinde.

## <a name="set-up-alerts-in-the-azure-portal"></a>Azure portalında uyarıları ayarlama
1. Azure portalında için bir uyarı oluşturmak istediğiniz Stream Analytics işi açın. 

2. İçinde **iş** dikey penceresinde tıklatın **izleme** bölümü.  

3. İçinde **ölçüm** dikey penceresinde tıklatın **uyarı Ekle** komutu.

      ![Azure portal Kurulumu](./media/stream-analytics-set-up-alerts/06-stream-analytics-set-up-alerts.png)  

4. Bir ad ve açıklama girin.

5. Seçiciler, altında uyarı gönderilir koşulu tanımlamak için kullanın.

6. Uyarı nereye hakkında bilgi sağlar.

      ![Bir Azure akış analizi işi için bir uyarı ayarlama](./media/stream-analytics-set-up-alerts/stream-analytics-add-alert.png)  

Azure portalında uyarılar yapılandırma hakkında daha ayrıntılı bilgi için bkz: [uyarı bildirimleri alma](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).  


## <a name="get-help"></a>Yardım alın
Daha fazla yardım için [Azure Stream Analytics forumumuzu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics) deneyin.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Stream Analytics'e giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-get-started.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)

