---
title: Azure DevTest labs'deki bir laboratuvara ilk VM'nizi oluşturun | Microsoft Docs
description: Azure DevTest labs'deki bir laboratuvara içinde ilk sanal makinenizi oluşturmayı öğrenin
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
ms.openlocfilehash: 93ce9feaf52282b9477d49eaf270d6d89dca7811
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51232537"
---
# <a name="create-your-first-vm-in-a-lab-in-azure-devtest-labs"></a>Azure DevTest labs'deki bir laboratuvara ilk VM'nizi oluşturun

İlk DevTest Labs erişmek ve ilk VM'nizi oluşturmak istiyorsanız, olasılıkla önceden yüklenmiş kullanarak bunu [temel Market görüntüsü](devtest-lab-configure-marketplace-images.md). Daha sonra ayrıca arasından seçim yapmanız mümkün olacaktır bir [özel görüntü ve bir formül](devtest-lab-add-vm.md) daha fazla sanal makine oluştururken. 

Bu öğreticide Azure portalı kullanarak ilk VM'nizi DevTest labs'deki bir laboratuvara ekleme adımları gösterilmektedir.

## <a name="steps-to-add-your-first-vm-to-a-lab-in-azure-devtest-labs"></a>Azure DevTest labs'deki bir laboratuvara ilk VM'nizi eklemek için adımları
1. [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
1. Seçin **tüm hizmetleri**ve ardından **DevTest Labs** listeden.
1. VM'yi oluşturmak istediğiniz Laboratuvar labs listesinden seçin.  
1. Laboratuvar'ın **genel bakış** dikey penceresinde **+ Ekle**.  

    ![VM düğmesi ekleme](./media/devtest-lab-add-vm/devtestlab-home-blade-add-vm.png)

1. Üzerinde **temel seçin** bir Market görüntüsü için VM dikey penceresinde seçin.
1. Üzerinde **sanal makine** dikey penceresinde, yeni sanal makine için bir ad girin **sanal makine adı** metin kutusu.

    ![Laboratuvar VM dikey penceresi](./media/devtest-lab-add-vm/devtestlab-lab-add-first-vm.png)

1. Girin bir **kullanıcı adı** sanal makinede yönetici ayrıcalıkları verildi.  
1. Etiketli metin alanına bir parola girin **bir değer yazın**.
1. **Sanal makine diski türünü** Laboratuvar sanal makineler için hangi depolama disk türüne izin belirler.
1. Seçin **sanal makine boyutu** ve işlemci çekirdeği ve RAM boyutu oluşturmak için sanal sabit sürücü boyutu belirtin önceden tanımlanmış öğelerden birini seçin.
1. Seçin **Yapıtları** - yapıtları - listesinden seçin ve temel görüntüye eklemek istediğiniz yapıtları yapılandırın.
    **Not:** DevTest Labs kullanarak yeni veya yapıtları, yapılandırma başvurmak [var olan bir yapıyı bir VM'ye ekleme](./devtest-lab-add-vm.md#add-an-existing-artifact-to-a-vm) bölümüne ve ardından tamamladığınızda buraya dönün.
1. Seçin **Oluştur** belirtilen VM'yi laboratuvara ekleme için.

   Laboratuvar dikey penceresi durumunu sanal makinenin oluşturma: ilk olarak görüntüler **oluşturma**, ardından olarak **çalıştıran** VM başlatıldıktan sonra.

## <a name="next-steps"></a>Sonraki adımlar
* VM oluşturulduktan sonra seçerek VM'ye bağlanabilir **Connect** sanal makinenin dikey penceresinde.
* Kullanıma [bir VM'yi bir laboratuvara ekleme](devtest-lab-add-vm.md) laboratuvarınızda sonraki Vm'leri ekleme hakkında daha ayrıntılı bilgi için.
* Keşfedin [DevTest Labs Azure Resource Manager hızlı başlangıç Şablon Galerisi](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates).
