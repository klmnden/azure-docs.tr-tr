---
title: "Bu öğreticide, Azure yığın sunar ve planları güncelleştirme konusunda bilgi edinin | Microsoft Docs"
description: "Bu makalede, görüntüleme ve var olan Azure yığın sunar ve planları değiştirme açıklar."
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 03/16/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 3b5d2ab5e924f578f5838d11b0d2058aacf67dec
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/17/2018
---
# <a name="tutorial-update-offers-and-plans"></a>Öğretici: teklifleri ve planları güncelleştirme
Bir Azure yığın operatör olarak abone olmak, kullanıcılarınızın geçerli kotalar ve istenen hizmetleri içeren planları oluşturun. Bunlar *temel planları* kullanıcılara sunulması için temel hizmetleri içerir ve yalnızca bir temel plan Teklif başına olabilir. Teklifiniz değiştirmeniz gerekiyorsa, kullanabileceğiniz *eklenti planları* , bilgisayar, depolama, genişletilecek planı değiştirmeye izin vermek veya ağ kotaları başlangıçta temel planla birlikte sunulmuyor. 

Her şey tek bir planda birleştirme bazı durumlarda en iyi olmakla birlikte, plan ve eklenti planlarınızı kullanarak ek hizmetler sunan bir taban olmasını isteyebilirsiniz. Örneğin, eklenti planları kabul tüm PaaS Hizmetleri ile temel bir plan bir parçası olarak Iaas Hizmetleri sunmak karar. Planları, sınırlı ASDK ortamınızdaki kaynakların kullanımını denetlemek için de kullanılabilir. Örneğin, kullanıcılarınızın kendi kaynak kullanımını dikkatli olmanızı istiyorsanız (bağlı olarak gereken hizmetleri) görece küçük temel plan olabilir ve kullanıcıların kapasite ulaşırken Bunlar zaten kaynakları ayırma tükettiği olduğunu uyarılmak kendi atanmış planına dayanarak. Burada, kullanıcıların ek kaynaklar için kullanılabilen eklenti plan seçebilirsiniz. 

> [!NOTE]
> Bir kullanıcı için mevcut bir teklif aboneliğe bir eklenti planı eklediğinde, ek kaynaklar için bir saatten görünmesi kadar sürebilir. 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir eklenti planı oluşturma 
> * Eklenti plana abone

## <a name="create-an-add-on-plan"></a>Bir eklenti planı oluşturma
**Eklenti planları** mevcut bir teklif değiştirerek oluşturulur.

1. Oturum [Azure yığın portal](https://adminportal.local.azurestack.external) bulut Yöneticisi olarak.
2. Daha önce kullanılan aynı adımları izleyin [temel plan oluşturma](asdk-offer-services.md) değil önceden sunulan hizmetler sunan yeni bir plan oluşturmak için. Bu örnekte, anahtar kasası (Microsoft.KeyVault) Hizmetleri plana dahil edilir.
3. Yönetici portalı'nda tıklatın **sunar** ve bir eklenti planla güncelleştirilmesi teklif seçin.

   ![](media/asdk-update-offers/1.PNG)

4.  Teklif özellikleri alt kısmına kaydırın ve seçin **eklenti planları**. **Ekle**'ye tıklayın.
   
    ![](media/asdk-update-offers/2.PNG)

5. Eklemek istediğiniz planı seçin. Bu örnekte, plan adlı **anahtar kasası planı**ve ardından **seçin** teklife plana eklemek için. Plan için Teklif başarıyla eklendiğini bir bildirim almanız gerekir.
   
    ![](media/asdk-update-offers/3.PNG)

6. Yeni bir eklenti planlama doğrulamak için teklifle dahil planları listelenen eklenti listesini gözden geçirin.
   
    ![](media/asdk-update-offers/4.PNG)

## <a name="subscribe-to-the-add-on-plan"></a>Eklenti plana abone
Mevcut bir Azure yığın aboneliğe eklenti bir plan ile genişletmek için Azure yığın kullanıcı portalı oturum açma gerekir.

Azure yığın operatör tarafından sunulan ek kaynakları bulmak ve önceden abone olduğunuz teklif için bir eklenti plana eklemek için aşağıdaki adımları kullanın.

1. Oturum [kullanıcı portalı](https://portal.local.azurestack.external).
2. Eklentinin plan ile genişletmek için abonelik bulmak için tıklatın **daha fazla hizmet**, tıklatın **abonelikleri**, aboneliğinizi seçin.
   
    ![](media/asdk-update-offers/5.PNG)

3.  Abonelik genel bakış tıklatın **planı Ekle**.
   
    ![](media/asdk-update-offers/6.PNG)

4. Ekle planı dikey penceresinde aboneliği eklemek için eklenti planı seçin. Bu örnekte, **anahtar kasası planı** seçilir. Ardından, eklenti plana başarıyla alındı ve güncelleştirilmiş abonelik erişmek için portal yenilemek ihtiyacınız olan bir bildirim almanız gerekir.
   
    ![](media/asdk-update-offers/7.PNG)

5. Son olarak, eklenti plana başarıyla eklendi emin olmak için abonelik ile ilişkili eklenti planlarını gözden geçirin.
   
    ![](media/asdk-update-offers/8.PNG)


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Bir eklenti planı oluşturma 
> * Eklenti plana abone

> [!div class="nextstepaction"]
> [Bir şablondan bir sanal makine oluşturun](asdk-create-vm-template.md)

