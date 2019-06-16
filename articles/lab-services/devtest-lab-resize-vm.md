---
title: Azure DevTest labs'deki bir laboratuvara, VM'yi yeniden boyutlandırma | Microsoft Docs
description: Azure DevTest labs'deki bir sanal makine yeniden boyutlandırma hakkında bilgi edinin
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
ms.openlocfilehash: a0bc618a9c0a02aae884d8be359df6bdbf4c0d2a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60868103"
---
# <a name="resize-a-vm-in-a-lab-in-azure-devtest-labs"></a>Azure DevTest labs'deki bir laboratuvara, VM'yi yeniden boyutlandırma
Azure sanal makineleri önemli özellikleri, CPU, ağ veya disk performans ihtiyaçlarınıza göre sanal makine (VM) değiştirmenizi sağlayan biridir. Azure DevTest Labs bu özellik VM'ler için bir laboratuar ortamında artık desteklemektedir. Yeniden boyutlandırma özelliği için izin verilen VM boyutları Laboratuvardaki Laboratuvar ilkesine uyar. Diğer bir deyişle, yalnızca verilen boyutlar laboratuvarda bir VM boyutunu değiştirebilirsiniz. 


## <a name="steps-to-resize-a-vm-in-a-lab"></a>Bir VM'yi yeniden boyutlandırma için adımları 
Azure DevTest labs'deki bir laboratuvara VM'nin yeniden boyutlandırmak için aşağıdaki adımları uygulayın: 

> [!NOTE]
> VM'ye Uzak Masaüstü oturumu (RDP) aracılığıyla birbirine bağlandıysa çalışmanızı kaydedin ve yeniden boyutlandırmadan önce VM'den kesin.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Seçin **tüm hizmetleri**ve ardından **DevTest Labs** listeden.
3. Yeniden boyutlandırmak istediğiniz VM içeren laboratuar labs listesinden seçin.  
4. Sol bölmede bulunan seçin **My sanal makineler**. 
5. VM listesinden bir VM'yi seçin.
6. Seçin **Durdur** VM çalışıyorsa araç çubuğundaki. İşlemin durumunu **bildirimleri** penceresi. VM durduruldu ve ardından Kapat tamamlanana kadar bekleyin **bildirimleri** penceresi. 

    ![VM’yi durdurma](media/devtest-lab-resize-vm/stop-vm.png)
1. VM'niz için sanal makine sayfasında seçin **boyutu** altında **ayarları** soldaki menüde.

    ![Boyutu menüsü](media/devtest-lab-resize-vm/size-menu.png)
1. İçinde **bir boyut seçin** penceresinde, Gözat ve sanal Makineniz için bir boyut seçin ve tıklayın **seçin**.     
1. Yeniden boyutlandırma işleminin durumunu **bildirimleri** penceresi.

    ![Durum yeniden boyutlandırma](media/devtest-lab-resize-vm/resize-status.png)
10. Sonra bir yeniden boyutlandırma işlemi, yakın başarılı **bildirimleri** penceresi. 
11. Seçin **genel bakış** seçin ve soldaki menüden **yeniden** araç çubuğundaki VM'yi yeniden başlatın. 

## <a name="next-steps"></a>Sonraki adımlar
Azure sanal makineler tarafından desteklenen yeniden boyutlandırma özelliği hakkında ayrıntılı bilgi için bkz. [yeniden boyutlandırma, sanal makineler](https://azure.microsoft.com/blog/resize-virtual-machines/).


