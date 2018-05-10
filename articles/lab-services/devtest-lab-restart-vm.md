---
title: Azure DevTest Labs laboratuvarda VM yeniden | Microsoft Docs
description: Azure DevTest Labs'de bir sanal makine yeniden öğrenin
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: 8460f09e-482f-48ba-a57a-c95fe8afa001
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: spelluru
ms.openlocfilehash: 24a17ce09bee1097b0418ad4e20990d359b3e084
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="restart-a-vm-in-a-lab-in-azure-devtest-labs"></a>Bir VM Azure DevTest Labs laboratuvarda yeniden başlatın
Hızla ve kolayca bir sanal makine DevTest Labs'de bu makaledeki adımları izleyerek yeniden başlatabilirsiniz. Bir VM yeniden başlatmadan önce aşağıdakileri dikkate alın:

- VM yeniden başlatma özelliğin etkinleştirilmesi için çalışıyor olmalıdır.
- Yeniden başlatma gerçekleştirdiğinizde çalışan VM için bir kullanıcı bağlıysa, yedekleme başladıktan sonra bunlar için VM yeniden bağlanmanız gerekir.
- VM yeniden başlattığınızda bir yapı uygulanıyorsa, yapıyı uygulanmamış bir uyarı alırsınız. 

    ![Yapıları uygulanırken yeniden başlatılırken uyarı](./media/devtest-lab-restart-vm/devtest-lab-restart-vm-apply-artifacts.png)


   > [!NOTE]
   > VM uygulanırken bir yapı durdurulursa, sorunu çözmek için olası bir yol yeniden başlatma VM özelliğini kullanabilirsiniz.
   >
   >

## <a name="steps-to-restart-a-vm-in-a-lab-in-azure-devtest-labs"></a>Azure DevTest Labs laboratuvarda VM yeniden başlatmanız adımları
1. [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
1. Seçin **tüm hizmetleri**ve ardından **DevTest Labs** listeden.
1. Yeniden başlatmak istediğiniz VM içerir Laboratuvar labs listesinden seçin.  
1. Sol panelinde seçin **My sanal makineleri**. 
1. Sanal makineleri listesinden çalışan bir VM seçin.
1. VM yönetim bölmenin en üstünde seçin **yeniden**.  

    ![VM düğmesini yeniden başlatın](./media/devtest-lab-restart-vm/devtest-lab-restart-vm.png)

1. Seçerek yeniden başlatma durumunu izleme **bildirimleri** simgesine tıklayın pencerenin sağ.

    ![VM yeniden başlatma durumunu görüntüleme](./media/devtest-lab-restart-vm/devtest-lab-restart-notification.png)

Çalışan bir VM üç nokta (...) listesinde seçerek de yeniden başlatabilirsiniz **My sanal makineleri**.

![Üç nokta VM yeniden başlatma](./media/devtest-lab-restart-vm/devtest-lab-restart-elipses.png)

## <a name="next-steps"></a>Sonraki adımlar
* Hizmet yeniden başlatıldıktan sonra seçerek VM'ye bağlanabilirsiniz **Bağlan** üzerinde kendi yönetim bölmesinde.
* Araştır [DevTest Labs Azure Resource Manager hızlı başlangıç Şablon Galerisi](https://github.com/Azure/azure-devtestlab/tree/master/Samples)
