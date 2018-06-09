---
title: Bu makalede, Azure yığın sunar ve planları güncelleştirme konusunda bilgi edinin | Microsoft Docs
description: Bu makalede, görüntüleme ve var olan Azure yığın sunar ve planları değiştirme açıklar.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.custom: mvc
ms.date: 06/07/2018
ms.author: brenduns
ms.reviewer: ''
ms.openlocfilehash: a84148a3ac31d51ff30cebffab00e5fec8fdaa87
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35238505"
---
# <a name="azure-stack-add-on-plans"></a>Azure yığın eklenti planları
Bir Azure yığın operatör olarak abone olmak, kullanıcılarınızın geçerli kotalar ve istenen hizmetleri içeren planları oluşturun. Bunlar [ *temel planları* ](azure-stack-create-plan.md) kullanıcılara sunulması için temel hizmetleri içerir ve yalnızca bir temel plan Teklif başına olabilir. Teklifiniz değiştirmeniz gerekiyorsa, kullanabileceğiniz *eklenti planları* , bilgisayar, depolama, genişletilecek planı değiştirmeye izin vermek veya ağ kotaları başlangıçta temel planla birlikte sunulmuyor. 

Her şey tek bir planda birleştirme bazı durumlarda en iyi olmakla birlikte, plan ve eklenti planlarınızı kullanarak ek hizmetler sunan bir taban olmasını isteyebilirsiniz. Örneğin, eklenti planları kabul tüm PaaS Hizmetleri ile temel bir plan bir parçası olarak Iaas Hizmetleri sunmak karar. Planları, Azure yığın ortamınızdaki kaynakların kullanımını denetlemek için de kullanılabilir. Örneğin, kullanıcılarınızın kendi kaynak kullanımını dikkatli olmanızı istiyorsanız (bağlı olarak gereken hizmetleri) görece küçük temel plan olabilir ve kullanıcıların kapasite ulaşırken Bunlar zaten kaynakları ayırma tükettiği olduğunu uyarılmak kendi atanmış planına dayanarak. Burada, kullanıcıların ek kaynaklar için kullanılabilen eklenti plan seçebilirsiniz. 

> [!NOTE]
> Bir kullanıcı için mevcut bir teklif aboneliğe bir eklenti planı eklediğinde, ek kaynaklar için bir saatten görünmesi kadar sürebilir. 

## <a name="create-an-add-on-plan"></a>Bir eklenti planı oluşturma
Eklenti planları, mevcut bir teklif değiştirerek oluşturulur:

1. Azure yığın Yönetici portalı'na bir bulut yönetici olarak oturum açın.
2. İçin kullanılan aynı adımları [yeni temel plan oluşturma](azure-stack-create-plan.md) değil önceden sunulan hizmetler sunan yeni bir plan oluşturmak için. Bu örnekte, anahtar kasası (Microsoft.KeyVault) Hizmetleri yeni plana dahil edilir.
3. Yönetici portalı'nda tıklatın **sunar** ve bir eklenti planla güncelleştirilmesi teklif seçin.

   ![](media/create-add-on-plan/1.PNG)

4.  Teklif özellikleri alt kısmına kaydırın ve seçin **eklenti planları**. **Ekle**'ye tıklayın.
   
    ![](media/create-add-on-plan/2.PNG)

5. Eklemek istediğiniz planı seçin. Bu örnekte, plan adlı **anahtar kasası planı**ve ardından **seçin** teklife plana eklemek için. Plan için Teklif başarıyla eklendiğini bir bildirim almanız gerekir.
   
    ![](media/create-add-on-plan/3.PNG)

6. Yeni bir eklenti planlama doğrulamak için teklifle dahil planları listelenen eklenti listesini gözden geçirin.
   
    ![](media/create-add-on-plan/4.PNG)

## <a name="next-steps"></a>Sonraki adımlar
[Teklif oluşturma](azure-stack-create-offer.md)