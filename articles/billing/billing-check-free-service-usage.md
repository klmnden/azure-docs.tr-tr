---
title: "Ücretsiz hizmetlere - Azure İzleyicisi ve izleme kullanımını | Microsoft Docs"
description: "Ücretsiz Hizmetler kullanımını denetlemek öğrenin. Azure portal ve kullanım csv kullanın."
services: 
documentationcenter: 
author: amberbhargava
manager: amberb
editor: 
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/25/2017
ms.author: amberb
ms.openlocfilehash: 8bc63091dfba822f9839f61dd12c212154ba695d
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="check-usage-of-free-services-included-with-your-azure-free-account"></a>Azure ücretsiz hesabınıza eklendi Ücretsiz Hizmetler kullanımını denetleyin 

Bu hizmetler sınırları aşan sürece, ücretsiz Azure ücretsiz hesapla dahil Hizmetleri için sizden ücret istenmese. Sınırsız olarak kalmasını sağlamak için ya da Azure portal ya da kullanım dosyanızı görüntülemek ve ücretsiz hizmetler kullanımını izlemek için kullanabilirsiniz. 

## <a name="check-usage-on-the-azure-portal"></a>Azure portalındaki kullanımını denetleyin

1.  [Azure Portal]( http://portal.azure.com)’da oturum açın.

2.  Sol gezinti alanından seçin **tüm hizmetleri**.

3.  Seçin **abonelikleri**.

4.  Ücretsiz hesap kaydolurken oluşturduğunuz aboneliği seçin.

    ![Tüm abonelikleri gösteren ekran görüntüsü](./media/billing-check-usage-of-free-services/select-free-account-subscription.png)

5.  Genel Bakış bölümünde aboneliğinizi abonelik kimliği gibi ilgili temel bilgileri gösterir, türü ve abonelik adı sunar. Ayrıca, ücretsiz hesap kredi ne zaman sona hakkında daha fazla bilgi bulabilirsiniz.

    ![Abonelik önemli bilgiler gösteren ekran görüntüsü](./media/billing-check-usage-of-free-services/subscription-essential-information.png)

6.  Geçerli ve tahmini maliyet Bul bilgi gidin. Maliyet ücretsiz hesabınızla bulunmayan hizmetlerin kullanımı ve ücretsiz hizmetler sınırlarının aşılması kullanımı içerir. 

    ![Abonelik maliyet bilgileri gösteren ekran görüntüsü](./media/billing-check-usage-of-free-services/subscription-cost-information.png)

7.  Genel Bakış bölümünde son bölümü, bir tablo Ücretsiz Hizmetler kullanıma sahiptir. 

    ![Ücretsiz Hizmetler kullanımını gösteren ekran görüntüsü](./media/billing-check-usage-of-free-services/subscription-usage-free-services.png)

    Tablo şu sütunları içerir:

* **Ölçüm adı:** tüketilen ölçer ölçü tanımlar. Ölçer eşlemesi hizmeti hakkında bilgi edinmek için [ölçer eşleme için ücretsiz hizmeti anlamak](billing-understand-free-service-meter-mapping.md). 
* **Kullanım/sınırı:** geçerli ayın kullanım ve ölçüm için sınır. Bu gibi durumlarda, bu bilgiler ayrıca durum çubuğunda bulabilirsiniz.
* **Durum:** ölçer kullanım durumu. Kullanım deseni temel alınarak, bu sayısının yasalar biri olabilir.
  * **Kullanılmadığı:** ölçer kullanılmayan veya ölçüm için kullanım fatura sistemiyle ulaştı değil.
  * **Üzerinde aşıldı \<tarihi >:** üzerinde ölçer sınırını aştınız \<tarihi >.
  * **Aşan düşüktür:** ölçer sınırı aşan düşüktür.
  * **Üzerinde aşıyor \<tarihi >:** üzerinde ölçer sınırını aşan büyük olasılıkla \<tarihi >.


## <a name="check-usage-through-the-usage-file"></a>Kullanım dosyasıyla kullanımını denetleyin

Kullanım dosyanızı Azure aboneliğiniz için ayrıntılı bilgiler sağlar. Aylık ve günlük kullanım dosyasını Azure hesap Merkezi'nden indirebilirsiniz. Kullanım dosyasını indirin ve gerekli erişim anlama hakkında bilgi edinmek için [fatura almak ve kullanım](billing-download-azure-invoice-daily-usage-date.md). Kullanım dosyasındaki sütunları hakkında bilgi edinmek için [anlamak kullanımınızı koşullarınızda](billing-understand-your-usage.md). 

Kullanım dosyasını Ücretli ve ücretsiz hizmetler için kullanım bilgileri içerir. Ücretsiz hizmet ölçümler sahip olabilir **serbest** ölçer adının sonuna eklenir. Ücretsiz ölçümler, dosyayı Excel'de açın ve filtre bulmak için **ölçer kategori sütunu** metin içeren hücrelerin **- boş** (kullanımı metin filtreleri &rarr; içerir Filtresi) &nbsp;

![Ücretsiz Hizmetler kullanımını gösteren ekran görüntüsü](./media/billing-check-usage-of-free-services/free-services-usage-csv.png)


## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun

Yardıma gereksiniminiz varsa [desteğine başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) hızla çözümlenen sorunu almak için.
