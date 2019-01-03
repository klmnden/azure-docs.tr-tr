---
title: Azure Stream Analytics işleri için uyarıları izleme işlevini ayarlama
description: Bu makalede, izleme ve Azure Stream Analytics işleri için uyarılar ayarlamak için Azure portalını kullanmayı açıklar.
services: stream-analytics
author: jseb225
ms.author: jeanb
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/07/2018
ms.custom: seodec18
ms.openlocfilehash: 727747d84d0db32c73fc1a200bcea7e5c149d24b
ms.sourcegitcommit: b767a6a118bca386ac6de93ea38f1cc457bb3e4e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2018
ms.locfileid: "53554920"
---
# <a name="set-up-alerts-for-azure-stream-analytics-jobs"></a>Azure Stream Analytics işleri için uyarıları ayarlama
Bir ölçüm belirlediğiniz bir koşulu ulaştığında bir uyarı tetiklemek için uyarılar ayarlayabilirsiniz. Örneğin, aşağıdaki gibi bir koşul için bir uyarı ayarlayabilirsiniz:

`If there are zero input events in the last 5 minutes, send email notification to sa-admin@example.com`

Kuralları, portal üzerinden ölçümler üzerinde ayarlanabilir veya yapılandırılabilir [program aracılığıyla](https://code.msdn.microsoft.com/windowsazure/Receive-Email-Notifications-199e2c9a) işlem günlükleri veriler üzerinde.

## <a name="set-up-alerts-in-the-azure-portal"></a>Azure portalında uyarıları ayarlama
1. Stream Analytics işi için bir uyarı oluşturmak istediğiniz Azure portalında açın. 

2. İçinde **iş** dikey penceresinde tıklayın **izleme** bölümü.  

3. İçinde **ölçüm** dikey penceresinde tıklayın **uyarısı Ekle** komutu.

      ![Azure portalında Stream Analytics uyarıları Kurulumu](./media/stream-analytics-set-up-alerts/06-stream-analytics-set-up-alerts.png)  

4. Bir ad ve açıklama girin.

5. Seçicileri altında bir uyarı gönderilecektir koşulu tanımlamak için kullanın.

6. Uyarıyı nereye gideceğine hakkında bilgi sağlar.

      ![Azure akış analizi işi için uyarı ayarlama](./media/stream-analytics-set-up-alerts/stream-analytics-add-alert.png)  

Azure portalında uyarıları yapılandırma hakkında daha fazla ayrıntı için [uyarı bildirimleri alma](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).  


## <a name="get-help"></a>Yardım alın
Daha fazla yardım için [Azure Stream Analytics forumumuzu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics) deneyin.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Stream analytics'e giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-get-started.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)

