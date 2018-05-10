---
title: Azure DevTest Labs laboratuvarda ilk VM oluşturma | Microsoft Docs
description: Azure DevTest Labs laboratuvarda içinde ilk sanal makinenizi oluşturmayı öğrenin
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: fbc5a438-6e02-4952-b654-b8fa7322ae5f
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: spelluru
ms.openlocfilehash: 0240bbdf2055ac44f8836adfaad3bf6c44893b77
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="create-your-first-vm-in-a-lab-in-azure-devtest-labs"></a>Azure DevTest Labs laboratuvarda ilk VM oluşturma

Başlangıçta DevTest Labs erişmek ve ilk VM oluşturmak istediğiniz zaman, büyük olasılıkla bunu önceden yüklenmiş kullanarak yapacağınız [temel Market görüntüsü](devtest-lab-configure-marketplace-images.md). Daha sonra aynı zamanda aralarından seçim yapabileceğiniz mümkün olur bir [özel görüntü ve bir formül](devtest-lab-add-vm.md) daha fazla sanal makineleri oluştururken. 

Bu öğreticide, bir laboratuar ortamında DevTest Labs ilk VM eklemek için Azure portalını kullanarak aracılığıyla açıklanmaktadır.

## <a name="steps-to-add-your-first-vm-to-a-lab-in-azure-devtest-labs"></a>Azure DevTest Labs laboratuvarda ilk VM eklemek için adımları
1. [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
1. Seçin **tüm hizmetleri**ve ardından **DevTest Labs** listeden.
1. VM oluşturmak istediğiniz Laboratuvar labs listesinden seçin.  
1. Laboratuvar 's üzerinde **genel bakış** dikey penceresinde, select **+ Ekle**.  

    ![VM düğme ekleme](./media/devtest-lab-add-vm/devtestlab-home-blade-add-vm.png)

1. Üzerinde **bir temel seçin** bir Market görüntü VM için dikey penceresinde, seçin.
1. Üzerinde **sanal makine** dikey penceresinde, yeni sanal makine için bir ad girin **sanal makine adı** metin kutusu.

    ![Laboratuvar VM dikey penceresi](./media/devtest-lab-add-vm/devtestlab-lab-add-first-vm.png)

1. Girin bir **kullanıcı adı** sanal makinede yönetici ayrıcalıkları verilmiş.  
1. Etiketli metin alanına bir parola girin **bir değer yazın**.
1. **Sanal makine disk türü** hangi depolama disk türü laboratuvara sanal makineler için izin verilen belirler.
1. Seçin **sanal makine boyutu** ve işlemci çekirdeği, RAM boyutu ve sabit sürücü boyutu oluşturmak için VM belirtin önceden tanımlanmış öğelerden birini seçin.
1. Seçin **yapıları** - yapıları - listesinden seçin ve temel görüntü eklemek istediğiniz yapıları yapılandırın.
    **Not:** DevTest Labs yeni veya yapıları yapılandırma başvurmak [bir VM'ye var artifact ekleyin](./devtest-lab-add-vm.md#add-an-existing-artifact-to-a-vm) bölümünde ve ardından bitirdikten sonra buraya dönün.
1. Seçin **oluşturma** Laboratuvar için belirtilen VM eklemek için.

   Laboratuvar dikey durumunu VM oluşturma: ilk olarak görüntüler **oluşturma**, ardından olarak **çalıştıran** VM başlatıldıktan sonra.

## <a name="next-steps"></a>Sonraki adımlar
* VM oluşturulduktan sonra seçerek VM'ye bağlanabilir **Bağlan** VM dikey.
* Kullanıma [bir VM'yi laboratuvara ekleme](devtest-lab-add-vm.md) sonraki VM'ler laboratuvarınızda ekleme hakkında daha ayrıntılı bilgi.
* Araştır [DevTest Labs Azure Resource Manager hızlı başlangıç Şablon Galerisi](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates).
