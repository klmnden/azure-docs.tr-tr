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
ms.author: banders
ms.openlocfilehash: 4e940a12cd57ef136cfd9ead298f9afcd2d6ad1f
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60617830"
---
# <a name="check-usage-of-free-services-included-with-your-azure-free-account"></a>Azure ücretsiz hesabınıza dahil olan ücretsiz hizmetlerin kullanımını denetleme 

Bu hizmetlerin limitlerin sürece, ücretsiz olarak dahil edilen ücretsiz Azure hesabı ile Hizmetleri için ücret ödemezsiniz. Sınırsız kalmasına ya da Azure portalı veya kullanım dosyanız ücretsiz hizmetlerin kullanımını izlemeye yarayan kullanabilirsiniz. 

## <a name="check-usage-on-the-azure-portal"></a>Azure portalında kullanımını denetleyin

1.  [Azure Portal](https://portal.azure.com) oturum açın.

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

    Tabloda aşağıdaki sütunları içerir:

* **Ölçüm adı:** Tüketilen ölçüm için ölçü birimini belirtir. Hizmet ölçüm eşleme hakkında bilgi edinmek için bkz. [ücretsiz hizmet ölçer eşlemesini anlama](billing-understand-free-service-meter-mapping.md).
* **Kullanım/sınırı:** Geçerli aya ait kullanım ve ölçüm için sınırı. Durum çubuğunda bu bilgiler de bulabilirsiniz.
* **Durum:** Ölçer kullanım durumu. Kullanım deseni temel alınarak, bu sayısının yasalar birine sahip olabilir.
  * **Kullanımda değil:** Ölçüm kullanmadıysanız veya kullanım ölçümü için fatura sistemiyle ulaşıldı edilmemiş.
  * **Üzerinde aşıldı \<tarih >:** Üzerinde ölçüm sınırını aştınız \<tarih >.
  * **Aşma olasılığı düşük:** Ölçüm sınırını aşmayacak gibi.
  * **Üzerinde aşıyor \<tarih >:** Üzerinde ölçüm sınırını aşacak \<tarih >.

## <a name="check-usage-through-the-usage-file"></a>Kullanım dosyası aracılığıyla kullanımını denetleyin

Kullanım dosyanızı Azure aboneliğiniz için ayrıntılı bilgi sağlar. Azure hesap Merkezi'nden, aylık ve günlük kullanım dosyası indirebilirsiniz. Kullanım dosyasını indirin ve gerekli erişim anlama hakkında bilgi edinmek için [alma faturayı ve kullanım](billing-download-azure-invoice-daily-usage-date.md). Kullanım dosyasındaki sütunlar hakkında bilgi edinmek için bkz. [kullanımınızla ilgili koşulları anlama](billing-understand-your-usage.md).

Ücretsiz ve Ücretli Hizmetleri için kullanım bilgilerini kullanım dosyası vardır. Ücretsiz hizmet ölçümleri haritamın **ücretsiz** ölçüm adının sonuna eklenir. Ücretsiz ölçümleri, dosyayı Excel'de açın ve filtre bulmak için **ölçüm kategorisi sütun** metin hücreler için **- ücretsiz** (kullanım metin filtreleri &rarr; içerir Filtresi) &nbsp;

![Ücretsiz hizmetlerin kullanımını gösteren ekran görüntüsü](./media/billing-check-usage-of-free-services/free-services-usage-csv.png)

## <a name="need-help-contact-us"></a>Yardım mı gerekiyor? Bizimle iletişim kurun.

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://go.microsoft.com/fwlink/?linkid=2083458).