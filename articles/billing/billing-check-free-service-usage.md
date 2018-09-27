---
title: Ücretsiz Azure Hizmetleri kullanımı İzleyicisi ve izleme | Microsoft Docs
description: Ücretsiz hizmetlerin kullanımını denetleme hakkında bilgi alın. Azure portalı ve kullanım csv kullanın.
services: ''
documentationcenter: ''
author: amberbhargava
manager: amberb
editor: ''
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/25/2017
ms.author: cwatson
ms.openlocfilehash: df049a87763f3aae8da2db153f876b88ed39b988
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2018
ms.locfileid: "47390247"
---
# <a name="check-usage-of-free-services-included-with-your-azure-free-account"></a>Azure ücretsiz hesabınıza dahil olan ücretsiz hizmetlerin kullanımını denetleme 

Bu hizmetlerin limitlerin sürece ücretsiz olarak dahil edilen ücretsiz Azure hesabı ile Hizmetleri için ücretlendirilmez. Sınırsız kalmasına ya da Azure portalı veya kullanım dosyanız ücretsiz hizmetlerin kullanımını izlemeye yarayan kullanabilirsiniz. 

## <a name="check-usage-on-the-azure-portal"></a>Azure portalında kullanımını denetleyin

1.  [Azure Portal]( http://portal.azure.com)’da oturum açın.

2.  Sol gezinti alanından seçin **tüm hizmetleri**.

3.  **Abonelikler**'i seçin.

4.  Hesap ücretsiz RMS'ye kaydolurken oluşturduğunuz aboneliği seçin.

    ![Tüm abonelikleri gösteren ekran görüntüsü](./media/billing-check-usage-of-free-services/select-free-account-subscription.png)

5.  Genel Bakış bölümünde, abonelik kimliği gibi aboneliğinizle ilgili temel bilgileri gösterir, türü ve abonelik adı sunar. Ayrıca, ücretsiz hesap kredinizi sonra sona erer hakkında daha fazla bilgi bulabilirsiniz.

    ![Abonelik önemli bilgiler gösteren ekran görüntüsü](./media/billing-check-usage-of-free-services/subscription-essential-information.png)

6.  Bulma bilgileri, güncel ve tahmini maliyeti aşağı kaydırın. Ücretsiz hesabınıza dahil olmayan Hizmetleri kullanımını ve ücretsiz hizmetlerin sınırlarını aşan kullanımlar maliyetini içerir. 

    ![Abonelik maliyet bilgilerini gösteren ekran görüntüsü](./media/billing-check-usage-of-free-services/subscription-cost-information.png)

7.  Genel Bakış bölümünde son bölümü ücretsiz hizmetlerin kullanımını üzerinde bir tablo vardır. 

    ![Ücretsiz hizmetlerin kullanımını gösteren ekran görüntüsü](./media/billing-check-usage-of-free-services/subscription-usage-free-services.png)

    Tablo şu sütunları içerir:

* **Ölçüm adı:** tüketilen ölçüm için ölçü birimini belirtir. Hizmet ölçüm eşleme hakkında bilgi edinmek için bkz. [ücretsiz hizmet ölçer eşlemesini anlama](billing-understand-free-service-meter-mapping.md). 
* **Kullanım/sınırı:** geçerli aya ait kullanım ve ölçüm için sınırı. Durum çubuğunda bu bilgiler de bulabilirsiniz.
* **Durum:** ölçer kullanım durumu. Kullanım deseni temel alınarak, bu sayısının yasalar birine sahip olabilir.
  * **Kullanılmayan:** ölçüm kullanılmayan veya kullanım ölçümü için fatura sistemiyle ulaşılmamış.
  * **Üzerinde aşıldı \<tarih >:** üzerinde ölçüm sınırını aştınız \<tarih >.
  * **Aşan kurmasının pek olası:** ölçüm sınırını aşmayacak gibi.
  * **Üzerinde aşıyor \<tarih >:** üzerinde ölçüm sınırını aşacak \<tarih >.


## <a name="check-usage-through-the-usage-file"></a>Kullanım dosyası aracılığıyla kullanımını denetleyin

Kullanım dosyanızı Azure aboneliğiniz için ayrıntılı bilgi sağlar. Azure hesap Merkezi'nden, aylık ve günlük kullanım dosyası indirebilirsiniz. Kullanım dosyasını indirin ve gerekli erişim anlama hakkında bilgi edinmek için [alma faturayı ve kullanım](billing-download-azure-invoice-daily-usage-date.md). Kullanım dosyasındaki sütunlar hakkında bilgi edinmek için bkz. [kullanımınızla ilgili koşulları anlama](billing-understand-your-usage.md). 

Kullanım dosyası, ücretsiz ve Ücretli Hizmetleri için kullanım bilgilerini içerir. Ücretsiz hizmet ölçümleri haritamın **ücretsiz** ölçüm adının sonuna eklenir. Ücretsiz ölçümleri, dosyayı Excel'de açın ve filtre bulmak için **ölçüm kategorisi sütun** metin içeren hücrelerin **- ücretsiz** (kullanım metin filtreleri &rarr; içerir Filtresi) &nbsp;

![Ücretsiz hizmetlerin kullanımını gösteren ekran görüntüsü](./media/billing-check-usage-of-free-services/free-services-usage-csv.png)


## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun

Yardıma ihtiyacınız varsa [desteğe](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorununuzun hızlıca çözülebilmesi için.
