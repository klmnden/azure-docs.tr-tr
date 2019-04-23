---
title: Azure DevTest labs'deki bir laboratuvara içinde bir VM'yi yeniden başlatma | Microsoft Docs
description: Azure DevTest Labs'de sanal makine yeniden başlatma hakkında bilgi edinin
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
ms.openlocfilehash: 34c08a79abf6acb5ae8582ecd0743a890d850fc8
ms.sourcegitcommit: c884e2b3746d4d5f0c5c1090e51d2056456a1317
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60149886"
---
# <a name="restart-a-vm-in-a-lab-in-azure-devtest-labs"></a>Azure DevTest labs'deki bir laboratuvara içinde bir VM'yi yeniden başlatma
Hızla ve kolayca bir sanal makine DevTest Labs'de bu makaledeki adımları izleyerek yeniden başlatabilirsiniz. Bir VM'yi yeniden başlatmadan önce aşağıdakileri dikkate alın:

- VM yeniden başlatma özelliğin etkinleştirilmesi için çalışıyor olması gerekir.
- Bunlar yeniden gerçekleştirdiğinizde kullanıcı çalışan bir VM'ye bağlı ise, yedeklemeyi başlattıktan sonra bunlar için VM yeniden bağlanmanız gerekir.
- VM yeniden başlatıldığında bir yapıt uygulanıyorsa, yapıt uygulanmayabilir bir uyarı alırsınız.

    ![Yapıtları uygulanırken yeniden başlatılırken Uyarısı](./media/devtest-lab-restart-vm/devtest-lab-restart-vm-apply-artifacts.png)


   > [!NOTE]
   > VM yapıt uygulanırken girmediğiniz, sorunu çözmek için olası bir şekilde yeniden VM özelliğini kullanabilirsiniz.
   >
   >

## <a name="steps-to-restart-a-vm-in-a-lab-in-azure-devtest-labs"></a>Azure DevTest labs'deki bir laboratuvara içinde bir VM'yi yeniden başlatma adımlarını
1. [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
1. Seçin **tüm hizmetleri**ve ardından **DevTest Labs** listeden.
1. Yeniden başlatmak istediğiniz VM içeren laboratuar labs listesinden seçin.
1. Sol bölmede bulunan seçin **My sanal makineler**.
1. Çalışan bir VM VM'ler listesinden seçin.
1. VM yönetim bölmesinin en üstünde seçin **yeniden**.

    ![VM düğmeyi yeniden başlatın](./media/devtest-lab-restart-vm/devtest-lab-restart-vm.png)

1. Seçerek yeniden başlatma durumunu izlemek **bildirimleri** simgesine sağ.

    ![VM yeniden başlatma durumunu görüntüleme](./media/devtest-lab-restart-vm/devtest-lab-restart-notification.png)

Çalışan bir VM listesinde üç nokta işaretine (...)'i seçerek de başlatabilirsiniz **My sanal makineler**.

![Üç nokta sanal Makineyi yeniden başlatın](./media/devtest-lab-restart-vm/devtest-lab-restart-elipses.png)

## <a name="next-steps"></a>Sonraki adımlar
* Hizmet yeniden başlatıldıktan sonra seçerek VM'ye bağlanabilirsiniz **Connect** üzerinde kendi Yönetimi bölmesi.
* Keşfedin [DevTest Labs Azure Resource Manager hızlı başlangıç Şablon Galerisi](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/QuickStartTemplates)
