---
title: Ücretsiz Azure hizmet kullanımı izleyin
description: Azure portalı ve kullanım CSV dosyasında ücretsiz hizmet kullanımı denetlemek öğrenin.
author: amberbhargava
manager: amberb
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/01/2019
ms.author: banders
ms.openlocfilehash: 3543bed7f699fd149ca7f2a6f61e9eb5aad5f1a3
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67491415"
---
# <a name="check-free-service-usage-included-with-your-azure-free-account"></a>Azure ücretsiz hesabınıza dahil olan ücretsiz hizmet kullanımını denetleyin

Hizmetleri limitlerin sürece, ücretsiz bir Azure ücretsiz hesabıyla dahil Hizmetleri için ücret ödemezsiniz. Sınırları içinde kalmasını sağlamak için izlemek ve ücretsiz hizmet kullanımı izlemek için Azure portalı veya kullanım dosyanızı kullanabilirsiniz.

## <a name="check-usage-in-the-azure-portal"></a>Azure portalında kullanımını denetleyin

1.  [Azure Portal](https://portal.azure.com) oturum açın.

2.  Sol gezinti bölmesinde seçin **tüm hizmetleri**.

3.  **Abonelikler**'i seçin.

4.  Hesap ücretsiz RMS'ye kaydolurken oluşturduğunuz aboneliği seçin.

    ![Tüm abonelikleri gösteren ekran görüntüsü](./media/billing-check-usage-of-free-services/select-free-account-subscription.png)

5.  Genel Bakış bölümünde, aboneliğiniz hakkında önemli bilgiler gösterir. Örneğin, abonelik kimliği, Teklif türü ve abonelik adı. Ücretsiz hesap kredinizin süresi dolduğunda da bilgi bulabilirsiniz.

    ![Abonelik önemli bilgiler gösteren ekran görüntüsü](./media/billing-check-usage-of-free-services/subscription-essential-information.png)

6.  Bul, güncel ve tahmini maliyet hakkında bilgilerin detayına gidin. Hizmet kullanımı ile ücretsiz hesabınızı ve ücretsiz hizmetlerin sınırlarını aşan kullanımlar bulunmayan maliyetini içerir.

    ![Abonelik maliyet bilgilerini gösteren ekran görüntüsü](./media/billing-check-usage-of-free-services/subscription-cost-information.png)

7.  Genel Bakış bölümünde son bölümü ücretsiz hizmet kullanımı gösteren bir tablo vardır.

    ![Ücretsiz hizmetlerin kullanımını gösteren ekran görüntüsü](./media/billing-check-usage-of-free-services/subscription-usage-free-services.png)

    Tabloda aşağıdaki sütunları içerir:

* **Ölçüm adı:** Tüketilen ölçüm için ölçü birimini belirtir. Hizmet ölçüm eşleme hakkında bilgi edinmek için bkz. [ücretsiz hizmet ölçer eşlemesini anlama](billing-understand-free-service-meter-mapping.md).
* **Kullanım/sınırı:** Geçerli aya ait kullanım ve ölçüm için sınırı. Durum çubuğunda bu bilgiler de bulabilirsiniz.
* **Durum:** Ölçer kullanım durumu. Kullanım deseni temel alınarak, aşağıdaki sayısının yasalar birine sahip olabilir:
  * **Kullanımda değil:** Ölçüm kullanmadıysanız veya kullanım ölçümü için fatura sistemiyle ulaşıldı edilmemiş.
  * **Üzerinde aşıldı \<tarih >:** Üzerinde ölçüm sınırını aştınız \<tarih >.
  * **Aşma olasılığı düşük:** Ölçüm sınırını aşmayacak gibi.
  * **Üzerinde aşıyor \<tarih >:** Üzerinde ölçüm sınırını aşacak \<tarih >.

## <a name="check-usage-with-the-usage-file"></a>Kullanım dosyasıyla kullanımını denetleyin

Kullanım dosyanızı Azure aboneliğinize ilişkin ayrıntılı bilgiler sağlar. Aylık ve günlük kullanım dosyanızı Azure hesap Merkezi'nden indirebilirsiniz. Kullanım dosyasını indirin ve gerekli erişim anlama hakkında bilgi edinmek için [alma faturayı ve kullanım](billing-download-azure-invoice-daily-usage-date.md). Kullanım dosyasındaki sütunlar hakkında bilgi edinmek için bkz. [kullanımınızla ilgili koşulları anlama](billing-understand-your-usage.md).

Ücretsiz ve Ücretli Hizmetleri için kullanım bilgilerini kullanım dosyası vardır. Ücretsiz hizmet ölçümleri haritamın **ücretsiz** ölçüm adının sonuna eklenir. Ücretsiz ölçümleri, dosyayı Excel'de açın ve filtre bulmak için **ölçüm kategorisi sütun** metin hücreler için **- ücretsiz** (kullanım metin filtreleri &rarr; içerir Filtresi).


![Ücretsiz hizmetlerin kullanımını gösteren ekran görüntüsü](./media/billing-check-usage-of-free-services/free-services-usage-csv.png)

## <a name="need-help-contact-us"></a>Yardım mı gerekiyor? Bizimle iletişim kurun.

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar
- [Aboneliğinizi yükseltin](billing-upgrade-azure-subscription.md)
