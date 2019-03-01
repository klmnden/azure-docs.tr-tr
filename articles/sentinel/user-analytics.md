---
title: Azure Önizleme Gözcü kullanıcı analytics'te kullanıcılarla araştırmak | Microsoft Docs
description: Azure Gözcü kullanıcı analytics'te kullanıcılarla araştırma hakkında bilgi edinin.
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: 5b06aef0-16be-4ca0-83e4-a33795a82353
ms.service: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2019
ms.author: rkarlin
ms.openlocfilehash: b9944764345f97c561f3f82f9cce9d37e204f134
ms.sourcegitcommit: f7f4b83996640d6fa35aea889dbf9073ba4422f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2019
ms.locfileid: "56992981"
---
# <a name="investigate-users-with-user-analytics"></a>Kullanıcı analiz kullanıcılarla araştırın

> [!IMPORTANT]
> Azure Sentinel şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Kullanıcılar, yakından takip etmek istediğiniz varlıklardır. Kullanıcılarınız hakkında Öngörüler elde etmenize yardımcı olmak için Azure Gözcü sorunsuz bir şekilde Azure Gelişmiş tehdit koruması ile tümleştirilir. Bu tümleştirme kullanıcı davranışı çözümleyebilir ve hangi kullanıcıların kendi uyarıları ve şüpheli etkinlik desenlerini Gözcü Azure ve Microsoft 365 arasında göre ilk olarak araştırmanız gereken önceliklendirmenize imkan sağlar.

Azure Sentinel kullanıcı verileriniz ile Azure Active Directory içindeki tüm kullanıcılar için kapsamlı kullanıcı analizi ve risk analizi etkinleştirmek için Microsoft 365 analytics'ten zenginleştirir. 

## <a name="how-it-works"></a>Nasıl çalışır?

1. Temel güvenlik analiz kuralları, bir eşleşme olduğunda algılandığında, Gözcü Azure, Azure ATP uyarılar gönderir.
1. Azure ATP hangi kullanıcı varlıkları uyarılarla ilgili denetler ve kullanıcılar için araştırma öncelik hesaplar.
3. Bu analiz kurallarınızı verilerle Azure Gözcü için zenginleştirilmiş sonra azure ATP sonra kullanıcıların puanı yeniden hesaplar.


## <a name="prerequisites"></a>Önkoşullar

- Azure Gelişmiş tehdit koruması lisans 
- Özelliği etkinleştirmek için Azure Gözcü yüklü Kiracı genel yönetici izinleri gerekir.

> [!NOTE]
> - Her Azure ATP Kiracı için bir Azure Gözcü örneği bağlanabilirsiniz.
> - Kullanıcı analizi şu anda yalnızca Azure Active Directory Kullanıcıları için kullanılabilir. 

## <a name="enable-user-analytics"></a>Kullanıcı analizi etkinleştir

1. Kullanıcı analytics'te Azure Gözcü, portal Git etkinleştirmek için **kullanıcı analizi** sayfasında ve tıklayın **etkinleştirme**. Bu bilgiler Azure ATP çalışma alanıyla ilgili gönderir.

1. Ardından, sizi Azure ATP götürür. Altında **güvenlik uzantıları** içinde **Microsoft Azure Gözcü** sekmesinde **+ artı** ekleyin ve ardından çalışma alanını seçin. 
1. **Bağlan**'a tıklayın.

## <a name="investigate-a-user"></a>Bir kullanıcı araştırın

1. Azure Gözcü menüsünde altında **kullanıcı analizi**, araştırma önceliklerine göre kullanıcıların listesini gözden geçirin. Puan, Gözcü Azure uyarıları ve Microsoft 365 uyarıları temel alır.

2. Araştırmak istediğiniz bir kullanıcıyı arayabilir. Kullanıcının kullanıcı sayfasına ulaşmak için tıklayın. Zaman çizelgesinde üzerinden kullanıcı etkinlikleri ve Uyarıları gözden geçirin. Microsoft 365 ve Azure Gözcü gerçekleştirilen tüm etkinlikler görebilirsiniz. Kullanıcı sayfası, kullanıcıların bir servis talebi araştırıp bulma tarafından da ulaşabilirsiniz.
      
    ![Kullanıcılar](./media/user-analytics/user-investigation.png)

 
3. Bunun iyi çalışması için düzgün bir şekilde ayarlamak zorunda [özel uyarı kuralı](tutorial-detect-threats.md) için doğru kullanıcı tanımlayıcıları eşlemek için **hesabı** varlık.

    - Windows olayları için harita **hesabı** için **SID**
    - (Bu, Azure etkinlik dahil olmak üzere birçok günlüklerinde bulunabilir) azure AD veri veya Office 365 verilerine, eşleme **hesabı** özelliğini **GUID**, veya **kullanıcı asıl adı**. 

   ![Sorgu](./media/user-analytics/query-window.png)



Örneğin: 
> [!NOTE]
> Azure etkinlik etkinlik günlüğünde UPN arayan varlık içerir.

1. Bu sorgu oluşturma kaynakları ve dağıtım etkinlikleri Azure etkinlik günlüğünde anormal bir sayının arar.

        AzureActivity      
        | where TimeGenerated >= startofday(ago(7d))        
        | where OperationName == "Create or Update Virtual Machine" or OperationName == "Create Deployment"        
        | where ActivityStatus == "Succeeded"        
        | make-series dResourceCount=dcount(ResourceId)  default=0 on EventSubmissionTimestamp in range(startofday(ago(7d)), now(), 1d) by Caller        
        | extend (RSquare,Slope,Variance,RVariance,Interception,LineFit)=series_fit_line(dResourceCount)        
        | where Slope > 0.2        
        | join kind=leftsemi (        
        // Last day's activity is anomalous        
        AzureActivity        
        | where TimeGenerated >= startofday(ago(1d))        
        | where OperationName == "Create or Update Virtual Machine" or OperationName == "Create Deployment"        
        | where ActivityStatus == "Succeeded"        
        | make-series dResourceCount=dcount(ResourceId)  default=0 on EventSubmissionTimestamp in range(startofday(ago(1d)), now(), 1d) by Caller        
        | extend (RSquare,Slope,Variance,RVariance,Interception,LineFit)=series_fit_line(dResourceCount)        
        | where Slope >0.2        
        ) on Caller        
        | extend AccountCustomEntity = Caller
 
2. Özel uyarı kuralı harita **hesabı** özelliğini **çağıran**.
   
   ![Sorgu](./media/user-analytics/query-window.png)

3. Kullanıcının kullanıcı araştırma penceresinde araştırın. Kullanıcılar araştırma hakkında daha fazla öneri için bkz. [Öğreticisi: Bir kullanıcı araştırmak](https://docs.microsoft.com/azure-advanced-threat-protection/investigate-a-user).



## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, Azure Gözcü için tehdit bilgileri sağlayıcınıza bağlanmak öğrendiniz. Azure Gözcü hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın.

- Gözcü Azure ile çalışmaya başlamak için Microsoft Azure aboneliği gerekir. Aboneliğiniz yoksa [ücretsiz deneme sürümü](https://azure.microsoft.com/free/) için kaydolabilirsiniz.
- Bilgi nasıl [ekleme verilerinizi Azure Gözcü](quickstart-onboard.md), ve [görünürlük almak, veri ve olası tehditleri](quickstart-get-visibility.md).
