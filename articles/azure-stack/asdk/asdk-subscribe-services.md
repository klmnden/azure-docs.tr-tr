---
title: "Bu öğreticide, Azure yığın teklife abone olmak öğrenin | Microsoft Docs"
description: "Bu öğretici Azure yığın Hizmetleri için yeni bir abonelik oluşturmak ve test sanal makinesi oluşturarak teklif test gösterilmektedir."
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
ms.openlocfilehash: d2adda5613b9a10bc3314045b9d68b0f3196f19a
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/17/2018
---
# <a name="tutorial-create-and-test-a-subscription"></a>Öğretici: oluşturmak ve bir abonelik test etme
Bu öğretici bir teklif içeren bir abonelik oluşturun ve onu test gösterilmektedir. Test için oturum [kullanıcı portalı](https://portal.local.azurestack.external) bulut yönetici olarak teklife abone olmak ve bir sanal makine oluşturun.

> [!TIP]
> Daha fazla Gelişmiş değerlendirme için bir deneyim, şunları yapabilirsiniz [belirli bir kullanıcı için bir abonelik oluştur](https://docs.microsoft.com/azure/azure-stack/azure-stack-subscribe-plan-provision-vm#create-a-subscription-as-a-cloud-operator) ve ardından Kullanıcı Portalı'nda bu kullanıcı olarak oturum açın. 

Bu öğreticide oluşturduğunuz teklif abone olmak gösterilmiştir [önceki Öğreticisi](asdk-offer-services.md).

Ne şunları öğreneceksiniz:

> [!div class="checklist"]
> * Bir teklife abone olma 
> * Teklif test

## <a name="subscribe-to-an-offer"></a>Bir teklife abone olma
Bir kullanıcı olarak bir teklif abone olmak için Azure yığın operatör tarafından sunulan Hizmetleri bulmak için Azure yığın kullanıcı portalı oturum açma gerekir.

1. Oturum [kullanıcı portalı](https://portal.local.azurestack.external) tıklatıp **bir abonelik edinmeniz**.

   ![Abonelik edinin](media/asdk-subscribe-services/get-subscription.png)

2. **Görünen Ad** alanına aboneliğiniz için bir ad yazın. Ardından **teklif** kullanılabilir teklifleri birini seçmek için **bir teklif seçin** bölümünde ve ardından **oluşturma**.

   ![Teklif oluşturma](media/asdk-subscribe-services/create-subscription.png)

   > [!TIP]
   > Kullanıcı Portalı aboneliğinizi kullanmaya başlamak için şimdi yenilemeniz gerekir.

3. Oluşturduğunuz abonelik görüntülemek için **daha fazla hizmet**, tıklatın **abonelikleri**, yeni aboneliğinizi'ye tıklayın. Teklife abone olduktan sonra yeni hizmetleri yeni abonelik bir parçası olarak dahil durumunda olduğunu görmek için portal yenileyin. Bu örnekte, **sanal makineleri** eklendi.

   ![Görünüm abonelik](media/asdk-subscribe-services/view-subscription.png)


## <a name="test-the-offer"></a>Teklif test
İçin oturum açtığınızda [kullanıcı portalı](https://portal.local.azurestack.external), yeni abonelik özelliklerini kullanarak bir sanal makine sağlama tarafından teklif test edebilirsiniz. 

> [!NOTE]
> Bu test Azure yığın marketi'ndeki eklendi Windows Server 2016 Datacenter VM görüntü gerektiren bir [önceki Öğreticisi](asdk-marketplace-item.md). 

1. Oturum [kullanıcı portalı](https://portal.local.azurestack.external).

2. Kullanıcı Portalı'nda tıklatın **sanal makineleri** > **Ekle** > **Windows Server 2016 Datacenter**ve ardından **oluştur** .

3. İçinde **Temelleri** bölümüne şunu yazın bir **adı**, **kullanıcı adı**, ve **parola**, seçin bir **abonelik**, oluşturma bir **kaynak grubu** (veya varolan bir tanesini seçin) ve ardından **Tamam**.

4. İçinde **bir boyutu seçin** 'yi tıklatın **A1 standart**ve ardından **seçin**.  

5. Ayarlar dikey penceresinde Varsayılanları kabul edin ve **Tamam**.

6. İçinde **Özet** 'yi tıklatın **Tamam** sanal makine oluşturulamıyor.  

7. Yeni bir sanal makine görmek için tıklatın **sanal makineleri**, yeni sanal makine için arama yapın ve adına tıklayın.

    ![Tüm kaynaklar](media/asdk-subscribe-services/view-vm.png)

> [!NOTE]
> Sanal makine dağıtımı tamamlanması birkaç dakika sürer.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide öğrendiklerinizi:

> [!div class="checklist"]
> * Bir teklife abone olma 
> * Teklif test

Bir şablonu kullanarak bir VM oluşturma konusunda bilgi almak için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [Bir şablondan bir sanal makine oluşturun](asdk-create-vm-template.md)