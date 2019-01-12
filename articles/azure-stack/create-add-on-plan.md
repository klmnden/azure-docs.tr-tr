---
title: Bu makalede, Azure Stack'te teklifleri ve planları güncelleştirme konusunda bilgi edinin | Microsoft Docs
description: Bu makalede, görüntüleyin ve mevcut Azure Stack'te teklifleri ve planları değiştirin açıklar.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.custom: mvc
ms.date: 01/09/2019
ms.author: sethm
ms.reviewer: unknown
ms.openlocfilehash: c6b86a2cebccfde27841dca64cf24ca4f1be0f7b
ms.sourcegitcommit: f4b78e2c9962d3139a910a4d222d02cda1474440
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2019
ms.locfileid: "54247597"
---
# <a name="azure-stack-add-on-plans"></a>Azure Stack eklenti planları

Azure Stack operatör değiştirmek için eklenti planları oluşturmak bir [temel plan](azure-stack-create-plan.md) ek hizmetleri sunmak ya da genişletmek istediğinizde *bilgisayar*, *depolama*, veya *ağ* temel plan ilk teklif ötesinde kotalar. Eklenti planı, temel plan değiştirebilir ve kullanıcıların abone olmak için seçebileceği isteğe bağlı uzantılar.

Her şey tek bir plandaki birleştirme ne zaman uygun olduğu durumlar vardır. Bazen planlamak ve ardından eklenti planı'nı kullanarak ek hizmetler sunan bir temele sahip isteyebilirsiniz. Örneğin, eklenti planı kabul tüm PaaS Hizmetleri ile temel bir plan kapsamında Iaas Hizmetleri sunmaya karar verebilirsiniz.

Eklenti planı'nı kullanmak için başka bir nedenle, kullanıcıların kendi kaynak kullanımını dikkatli olmanızı yardımcı olmaktır. Bunu yapmak için (bağlı olarak gereken hizmetleri) görece küçük kotalar içeren temel bir plan ile başlatabilir. Kullanıcılara kapasiteyi ulaşmanızı gibi daha sonra kullanıcılar, bunlar kendi atanan planına dayanarak kaynakların ayrılması tükettiniz uyarı. Burada, kullanıcıların ek kaynaklar sağlayan bir eklenti planı seçebilirsiniz.

> [!NOTE]
> Eklenti planı kota genişletmek için kullanmak istemediğinizde, seçebilirsiniz [kota yapılandırmasını Düzenle](azure-stack-quota-types.md#edit-a-quota).

Kullanıcı mevcut bir teklif aboneliğe bir eklenti planı eklediğinde, ek kaynaklar için bir saat görünmesini kadar sürebilir.

## <a name="create-an-add-on-plan"></a>Eklenti planı oluşturma

Eklenti planları, var olan bir teklif değiştirerek oluşturulur:

1. Azure Stack Yönetici portalı'na bir bulutun Yöneticisi olarak oturum açın.
2. İçin kullanılan aynı adımları izleyerek [yeni bir temel plan oluşturma](azure-stack-create-plan.md) değil daha önce sunulan hizmetleri sunan yeni bir plan oluşturmak için. Bu örnekte, Key Vault (**Microsoft.KeyVault**) Hizmetleri yeni plana dahil edilir.
3. Yönetici portalında **sunar** ve ardından bir eklenti planı ile güncelleştirilmesi teklifini seçin.

   ![Eklenti planı oluşturma](media/create-add-on-plan/1.PNG)

4. Teklif özelliklerinin alt kısma kaydırın ve **eklenti planları**. **Ekle**'ye tıklayın.

    ![Eklenti planı oluşturma](media/create-add-on-plan/2.PNG)

5. Planı eklemek için seçin. Bu örnekte, plan çağrılır **anahtar kasası planı**. Plan seçtikten sonra **seçin** plan teklife eklenecek. Plan teklife başarıyla eklendiğini bir bildirim almanız gerekir.

    ![Eklenti planı oluşturma](media/create-add-on-plan/3.PNG)

6. Yeni eklenti planı listelendiğini doğrulamak için teklife dahil edilen eklenti planları listesini gözden geçirin.

    ![Eklenti planı oluşturma](media/create-add-on-plan/4.PNG)

## <a name="next-steps"></a>Sonraki adımlar

* [Teklif oluşturma](azure-stack-create-offer.md)