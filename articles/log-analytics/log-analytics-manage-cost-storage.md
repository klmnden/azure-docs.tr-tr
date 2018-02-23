---
title: "Azure günlük analizi cinsinden veri tutma maliyetini yönetme | Microsoft Docs"
description: "Azure portalında günlük analizi çalışma alanınız için fiyatlandırma planı ve veri bekletme ilkesi değiştirmeyi öğrenin."
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/27/2017
ms.author: magoedte
ms.custom: mvc
ms.openlocfilehash: 5143abdde715424a41a53bb661db342acf817e0c
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="manage-cost-of-data-retention-with-your-log-analytics-workspace"></a>Günlük analizi çalışma alanınız ile veri saklama maliyetini yönetme
Günlük analizi için seçtiğiniz plana bağlı olarak kaydolurken bağlı kaynaklar tarafından oluşturulan verilerin çalışma alanınızda ne kadar süreyle depolanır bir sınır yoktur.  Bu makalede, saat ve bu sınıra yapılandırma dönemlerini için bu verileri koruma için maliyetleri etkileyebilir dikkat edilecek noktalar vurgular.   

Günlük analizi büyük miktarlarda veri kaynakları şirket içinden tüketebildiğinden, Bulut ve karma ortamlar, bir süre için o veri depolamanın maliyeti aşağıdaki faktörlere bağlı olarak önemli olabilir:

* Sistemler, altyapı bileşenlerini, bulut kaynakları, gelen topluyorsunuz vb. sayısı
* İleti kuyrukları, günlükleri, olay, güvenlik ile ilgili veriler veya performans ölçümleri gibi kaynak tarafından oluşturulan veri türü
* Bu kaynaklar tarafından oluşturulan veri hacmi 
* Etkin yönetimi çözümü sayısı, veri kaynağı ve toplama sıklığı

> [!NOTE]
> Ne kadar veri topladığı tahmini sağladığından her çözümünü içeren belgelere bakın.   

Kullanıyorsanız *serbest* planı, veriler yedi gün bekletme için sınırlı.  İçin *tek başına* veya *Ödendi* katmanı, toplanan verileri kullanılabilir son 31 gün.  

Kullanırken *serbest* planlama, izin verilen tutarlar tutarlı bir şekilde aşması fark ederseniz, bu sınırı aşan veri toplamak için ücretli bir plana çalışma alanınızı değiştirebilirsiniz. 

> [!NOTE]
> Ücretli katmanı için uzun bir bekletme dönemi seçmek seçerseniz ücretleri uygulanır. Herhangi bir zamanda ve fiyatlandırma hakkında daha fazla bilgi için plan türünü değiştirmek, bkz: [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/log-analytics/). 

## <a name="change-the-data-retention-period"></a>Veri saklama süresi değiştirme 

1. [Azure portal](http://portal.azure.com) oturum açın. 
2. Tıklatın **tüm hizmetleri**. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Log Analytics**’i seçin.
3. Günlük analizi abonelikleri bölmesinde listeden değiştirmek için çalışma alanınızı seçin.
4. Çalışma sayfasında, tıklatın **bekletme** sol bölmesinden.
5. Çalışma alanı bekletme bölmesinde artırın veya gün sayısını azaltın ve ardından kaydırıcıyı **kaydetmek**.  Kullanıyorsanız *ücretsiz* katmanı, veri saklama süresi değiştirmek mümkün olmayacak ve bu ayarı denetlemek için ücretli katmanına yükseltme yapmanız gerekir.<br><br> ![Çalışma alanı veri saklama ayarını değiştirme](media/log-analytics-manage-cost/manage-cost-change-retention.png)

## <a name="next-steps"></a>Sonraki adımlar  

Hangi kaynakları ve kullanım ve maliyet yönetmenize yardımcı olmak için gönderilen veri farklı türdeki gönderiyorsunuz ne kadar veri toplanır belirlemek için bkz: [günlük analizi veri kullanımını çözümleme](log-analytics-usage.md)