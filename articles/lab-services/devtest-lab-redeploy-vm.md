---
title: Azure DevTest labs'deki bir laboratuvara içinde bir VM'yi yeniden dağıtma | Microsoft Docs
description: Azure DevTest Labs sanal makineler'de (diğer bir Azure düğümünden taşıma) yeniden dağıtma hakkında bilgi edinin.
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
ms.date: 06/11/2018
ms.author: spelluru
ms.openlocfilehash: 4500fcfa6fbfb346a6e5c7fd045ba0046a901b91
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60561633"
---
# <a name="redeploy-a-vm-in-a-lab-in-azure-devtest-labs"></a>Azure DevTest labs'deki bir laboratuvara içinde bir VM'yi yeniden dağıtma
Bir sanal makineye (VM) bir Uzak Masaüstü bağlantısı aracılığıyla bir laboratuvarda bağlanamıyorsanız, VM'yi yeniden dağıtma ve yeniden bağlanmayı deneyin. DevTest Labs VM yeniden dağıtırken, VM, Azure altyapısı içinde yeni bir düğüm için çalıştığı düğümünden taşır. Daha sonra tüm yapılandırma seçenekleri ve ilişkili kaynakları korurken VM'yi başlatır. Bu özellik, Uzak Masaüstü Bağlantısı ya da uygulama erişimi için Windows tabanlı Vm'leri Laboratuvardaki sorun giderme için harcanan süre kaydeder. 

## <a name="steps-to-redeploy-a-vm-in-a-lab"></a>Bir VM'yi yeniden dağıtma adımları 
Azure DevTest labs'deki bir laboratuvara VM'nin yeniden dağıtmak için aşağıdaki adımları uygulayın: 

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Seçin **tüm hizmetleri**ve ardından **DevTest Labs** listeden.
3. Yeniden dağıtmak istediğiniz VM içeren laboratuar labs listesinden seçin.  
4. Sol bölmede bulunan seçin **My sanal makineler**. 
5. VM listesinden bir VM'yi seçin.
6. VM'niz için sanal makine sayfasında seçin **yeniden** altında **OPERATIONS** soldaki menüde.

    ![Yeniden dağıtım](media/devtest-lab-redeploy-vm/redeploy.png)
7. Sayfadaki bilgileri okuyun ve seçin **yeniden** düğmesi. 9. Yeniden dağıtma işlemi durumunu **bildirimleri** penceresi.

    ![Durum yeniden dağıtma](media/devtest-lab-redeploy-vm/redeploy-status.png)

## <a name="next-steps"></a>Sonraki adımlar
Azure DevTest labs'deki bir VM'yi yeniden boyutlandırma hakkında bilgi edinin [VM'yi yeniden boyutlandırma](devtest-lab-resize-vm.md).


