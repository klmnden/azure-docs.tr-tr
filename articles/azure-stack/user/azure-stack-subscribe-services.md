---
title: Bu öğreticide, bir Azure Stack teklife abone olmayı öğrenin | Microsoft Docs
description: Bu öğreticide Azure Stack Hizmetleri için yeni bir abonelik oluşturmak ve test teklifini test sanal makinesi oluşturarak gösterilmektedir.
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
ms.topic: tutorial
ms.custom: mvc
ms.date: 09/05/2018
ms.author: brenduns
ms.reviewer: ''
ms.openlocfilehash: 0e2fa9b01d27d68c1eab9097a20b6e350ba47f99
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/06/2018
ms.locfileid: "44028821"
---
# <a name="tutorial-create-and-test-a-subscription"></a>Öğretici: oluşturma ve test aboneliği
Bu öğreticide bir teklif içeren bir aboneliği oluşturun ve ardından test gösterilmektedir. Test için bir bulutun Yöneticisi olarak Azure Stack kullanıcı portalında oturum açın, teklife abone olun ve sonra bir sanal makine oluşturun.

> [!TIP]
> Daha fazla Gelişmiş değerlendirme için bir deneyim, yapabilecekleriniz [belirli bir kullanıcı için bir abonelik oluşturmak](https://docs.microsoft.com/azure/azure-stack/azure-stack-subscribe-plan-provision-vm#create-a-subscription-as-a-cloud-operator) ve ardından Kullanıcı Portalı'nda, bir kullanıcı olarak oturum açın. 

Bu öğreticide bir Azure Stack teklife abone olma gösterilmektedir.

Ne öğreneceksiniz:

> [!div class="checklist"]
> * Bir teklife abone olma 
> * Test teklifini

## <a name="subscribe-to-an-offer"></a>Bir teklife abone olma
Bir kullanıcı olarak bir teklife abone olmak için Azure Stack operatör tarafından sunulan Hizmetleri bulmak için Azure Stack kullanıcı portalında oturum açmanız gerekir.

1. Kullanıcı portalında oturum açın ve tıklayın **bir abonelik edinmeniz**.

   ![Abonelik edinin](media/azure-stack-subscribe-services/get-subscription.png)

2. **Görünen Ad** alanına aboneliğiniz için bir ad yazın. ' A tıklayarak **teklif** kullanılabilir teklif birini seçmek için **bir teklif seçin** bölümüne ve ardından **Oluştur**.

   ![Teklif oluşturma](media/azure-stack-subscribe-services/create-subscription.png)

   > [!TIP]
   > Şimdi aboneliğinizi kullanmaya başlamak için Kullanıcı Portalı yenilemeniz gerekir.

3. Oluşturduğunuz aboneliği görüntülemek için tıklayın **tüm hizmetleri**.  Ardından, altında **genel** kategorisi seçin **abonelikleri**ve sonra yeni aboneliğinizi seçin. Bir teklife abone olduktan sonra yeni hizmetler, yeni aboneliği bir parçası olarak dahil edilmiştir, görmek için portalı yenileyin. Bu örnekte, **sanal makineler** eklendi.

   ![Aboneliği görüntüle](media/azure-stack-subscribe-services/view-subscription.png)


## <a name="test-the-offer"></a>Test teklifini
Kullanıcı portalında oturum açtığınızda, yeni abonelik özelliklerini kullanarak bir sanal makine sağlama tarafından teklif test edebilirsiniz. 

> [!NOTE]
> Bu test, bir Windows Server 2016 Datacenter VM için Azure Stack marketini ilk eklendiğini gerektirir. 

1. Kullanıcı portalında oturum açın.

2. Kullanıcı Portalı'nda tıklatın **sanal makineler** > **Ekle** > **Windows Server 2016 Datacenter**ve ardından **oluştur** .

3. İçinde **Temelleri** bölümüne şunu yazın bir **adı**, **kullanıcı adı**, ve **parola**, seçin bir **abonelik**, oluşturma bir **kaynak grubu** (veya varolan bir tanesini seçin) ve ardından **Tamam**.

4. İçinde **bir boyut seçin** bölümünde **standart A1**ve ardından **seçin**.  

5. Ayarlar dikey penceresinde, Varsayılanları kabul edin ve **Tamam**.

6. İçinde **özeti** bölümünde **Tamam** sanal makine oluşturmak için.  

7. Yeni sanal makinenize görmek için tıklayın **sanal makineler**, sonra yeni sanal makine için arama yapın ve onun adına tıklayın.

    ![Tüm kaynaklar](media/azure-stack-subscribe-services/view-vm.png)

> [!NOTE]
> Sanal makine dağıtımını tamamlanması birkaç dakika sürer.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide öğrendiklerinizi:

> [!div class="checklist"]
> * Bir teklife abone olma 
> * Test teklifini


> [!div class="nextstepaction"]
> [Topluluk şablonundan bir VM oluşturma](azure-stack-create-vm-template.md)