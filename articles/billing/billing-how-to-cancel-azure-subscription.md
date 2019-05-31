---
title: Azure aboneliğinizi iptal | Microsoft Docs
description: Ücretsiz deneme aboneliği gibi Azure aboneliğinizi iptal etmeyi açıklar
author: bandersmsft
manager: amberb
tags: billing
ms.assetid: 3051d6b0-179f-4e3a-bda4-3fee7135eac5
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 5/30/2019
ms.author: banders
ms.openlocfilehash: 235d93de56289bb2daaa661ee9806732da3b6d6a
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66417600"
---
# <a name="cancel-your-subscription-for-azure"></a>Azure aboneliğiniz iptal et

Bir Azure aboneliğine [Hesap Yöneticisi](billing-subscription-transfer.md#whoisaa) bir Azure aboneliği iptal edebilirsiniz. Azure Abonelik Yöneticisi ayrıca [başka bir kullanıcı bir abonelik Yöneticisi olarak atamak](billing-add-change-azure-subscription-administrator.md#assign-a-user-as-an-administrator-of-a-subscription) böylece bir aboneliği iptal edebilir. Abonelik iptal edildikten sonra Azure hizmet ve kaynaklara erişiminizi sona erer.

Aboneliğinizi iptal etmeden önce:

* Verilerinizi yedekleyin. Örneğin, verileri Azure depolama veya SQL depoluyorsanız bir kopyasını indirin. Bir sanal makine görüntüsü, yerel olarak kaydedin.
* Hizmetlerinizi kapatın. Git [Yönetim Portalı'nda kaynaklar sayfası](https://ms.portal.azure.com/?flight=1#blade/HubsExtension/Resources/resourceType/Microsoft.Resources%2Fresources), ve **Durdur** tüm sanal makineleri, uygulamaları veya diğer hizmetleri çalıştırma.
* Verilerinizi geçirme göz önünde bulundurun. Bkz: [kaynakları yeni kaynak grubuna veya aboneliğe taşıma](../azure-resource-manager/resource-group-move-resources.md).
* Tüm kaynaklar ve tüm kaynak grupları silmeniz gerekir. Bir aboneliği iptal edebilir önce bunların silinmesi gereklidir. Tek tek her kaynak grubunun silinmesi gerekir. Kaynak grubu silme işlemi sırasında kaynak grubu adını yazarak, silmeyi onaylamanız gerekir.

Ücretli bir Azure destek planı iptal ederseniz abonelik dönemi için rest yine de faturalandırılır. Daha fazla bilgi için [Azure destek planları](https://azure.microsoft.com/support/plans/).

## <a name="cancel-subscription-using-the-azure-portal"></a>Azure portalını kullanarak bir aboneliği iptal et

1. Aboneliğinizden seçin [Azure portalındaki abonelikler sayfasından](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
2. İptal etmek istediğiniz aboneliği seçin.
3. Seçin **genel bakış**ve ardından **aboneliği iptal et**.

    ![İptal düğmesini gösteren ekran görüntüsü](./media/billing-how-to-cancel-azure-subscription/cancel_ibiza.png)
3. Komut istemlerini izleyin ve iptal tamamlayabilirsiniz.

## <a name="what-happens-after-i-cancel-my-subscription"></a>Aboneliğimi iptal sonra ne olur?

Faturalandırma, hemen iptal sonra durdurulur. Ancak, portalda göstermek iptal 10 dakikaya kadar sürebilir.

Bundan sonra hizmetlerinizi devre dışı bırakılır. Bu, sanal makinelerinizi paylaştırılmamış, geçici IP adreslerini serbest ve depolama salt okunur anlamına gelir.

Bir faturalandırma döneminde ortasında iptal ederseniz, süresi sona erdikten sonra tipik fatura tarihinde son fatura göndereceğiz.

Biz, verilerinize erişim sağlaması gereken veya fikrinizi değiştirmeniz durumunda kalıcı olarak silinmeden önce 90 gün bekleyin. Biz, veri koruma için ücret almayız. Daha fazla bilgi için bkz. [Microsoft Trust Center - verilerinizi nasıl yönetilir](https://go.microsoft.com/fwLink/p/?LinkID=822930&clcid=0x409).

## <a name="reactivate-subscription"></a>Aboneliği yeniden etkinleştirme

Kullandıkça Öde aboneliğinizi yanlışlıkla iptal ederseniz yapabilecekleriniz [hesapları Merkezi'nde yeniden](billing-subscription-become-disable.md).

Aboneliğinizi Kullandıkça Öde değilse, aboneliğinizi yeniden etkinleştirmek için İptal 90 gün içinde destek başvurun.

## <a name="need-help-contact-us"></a>Yardım mı gerekiyor? Bizimle iletişim kurun.

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://go.microsoft.com/fwlink/?linkid=2083458).
