---
title: Uyarı örneklerini yönetme
description: Azure uyarı örneklerinde yönetme
author: anantr
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: anantr
ms.subservice: alerts
ms.openlocfilehash: cb93f38c05156d7ab5acb89ffff810949583e507
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60551881"
---
# <a name="manage-alert-instances"></a>Uyarı örneklerini yönetme
İle [uyarı deneyimi birleşik](https://aka.ms/azure-alerts-overview) Azure İzleyici'de artık tüm, farklı uyarı türleri tek bir cam bölmeyle içinde birden çok abonelik kapsayan azure'daki görebilirsiniz. Bu makalede, uyarı örneklerinin nasıl görüntüleyebileceğiniz aracılığıyla gösterilmektedir ve derinlemesine sorun giderme için belirli uyarı örneklerini bulmak için portalı.

1. Uyarılar sayfasına gelirsiniz başlamanın üç yolu vardır.

   + İçinde [portalı](https://portal.azure.com/)seçin **İzleyici** ve izleme bölümü altında - **uyarılar**.  
     ![İzleme](media/alerts-managing-alert-instances/monitoring-alerts-managing-alert-instances-toc.jpg)
  
   + Belirli bir bağlamında uyarılardan gidebilirsiniz **kaynak**. Bir kaynak açıldıktan sonra izleme bölümüne, içindekiler tablosu gezinmek ve seçin **uyarılar**, bu belirli kaynak ile ilgili uyarılar için önceden filtrelenen giriş sayfası ile.
   
     ![İzleme](media/alerts-managing-alert-instances/alert-resource.JPG)
    
   + Belirli bir bağlamında uyarılardan gidebilirsiniz **kaynak grubu**. Bir kaynak grubu açıldıktan sonra izleme bölümüne, içindekiler tablosu gezinmek ve seçin **uyarılar**, giriş sayfası, belirli bir kaynak grubu ile ilgili uyarılar için önceden filtrelenen ile.    
   
     ![İzleme](media/alerts-managing-alert-instances/alert-rg.JPG)

1. Üzerinde ulaşırsınız **uyarılar özeti** sayfasında, Azure genelinde tüm uyarı örnekleri genel bir bakış sağlar. Özet görünümü seçerek değiştirebilirsiniz **birden çok abonelik** (en fazla 5) veya genelinde filtreleme **kaynak grupları**, belirli **kaynakları**, veya **zaman aralıkları**. Toplam uyarı ya da herhangi bir önem derecesi bantları uyarıları için liste görünümüne gitmek için tıklayın.     
   ![Uyarılar özeti](media/alerts-managing-alert-instances/alerts-summary.jpg)
 
1. Üzerinde ulaşırsınız **tüm uyarıları** sayfasında göreceğiniz uyarı tüm örnekleri kullanıma listelenen Azure. Bir uyarı bildirimine portala gelen, hakkında uyarı örneğinin daraltmak kullanılabilir filtreleri kullanabilirsiniz. (**Not**: herhangi bir önem derecesi bantları tıklayarak sayfaya geliyorsa, geldiğinde listesi için önem derecesi önceden filtre uygulanmış olacaktır). Önceki sayfada kullanılabilir filtreleri dışında şimdi de izleme hizmeti (örneğin, ölçümler için Platform), (tetiklenen veya çözülmüş) izleme koşulu, önem derecesi, uyarı durumu (yeni/onaylanır/kapalı) veya akıllı grubu kimliğini temel alarak filtreleyebilirsiniz

   ![Tüm Uyarılar](media/alerts-managing-alert-instances/all-alerts.jpg)

   > [!NOTE]
   >  Herhangi bir önem derecesi bantları tıklayarak sayfaya geliyorsa, bu sayfada geldiğinde listesi için önem derecesi önceden filtre uygulanmış olacaktır.
 
1. Uyarı herhangi bir örneğe tıklayarak açılır **uyarı ayrıntıları** sayfasında, ilgili uyarı örneğinin bilgilerine yakından için izin verme.   
   ![Uyarı ayrıntıları](media/alerts-managing-alert-instances/alert-details.jpg)  

