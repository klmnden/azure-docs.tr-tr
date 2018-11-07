---
title: Azure DevTest labs'deki bir laboratuvara içinde talep edilebilir VM'ler oluşturma ve yönetme | Microsoft Docs
description: Talep edilebilir bir sanal makine, Azure DevTest labs'deki bir laboratuvara ekleme hakkında bilgi edinin
services: devtest-lab,virtual-machines
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.assetid: f671e66e-9630-4e30-a131-a6bad9ed9c11
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/01/2018
ms.author: spelluru
ms.openlocfilehash: 3bfb674fa66f0701a099d237f4e760453c7b6a6e
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51232136"
---
# <a name="create-and-manage-claimable-vms-in-azure-devtest-labs"></a>Azure DevTest labs'deki talep edilebilir VM'ler oluşturma ve yönetme
Benzer şekilde nasıl bir laboratuvara talep edilebilir VM ekleme, [standart VM ekleme](devtest-lab-add-vm.md) – bir *temel* ya da diğer bir deyişle bir [özel görüntü](devtest-lab-create-template.md), [formül](devtest-lab-manage-formulas.md) , veya [Market görüntüsü](devtest-lab-configure-marketplace-images.md). Bu öğreticide, Azure portalını kullanarak bir talep edilebilir VM DevTest labs'deki bir laboratuvara ekleme adımları gösterilmektedir ve bir kullanıcı talebi ve VM etmesini takip işlemlerini gösterir.

## <a name="steps-to-add-a-claimable-vm-to-a-lab-in-azure-devtest-labs"></a>Talep edilebilir VM için Azure DevTest labs'deki bir laboratuvara ekleme adımları
1. [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
1. Seçin **tüm hizmetleri**ve ardından **DevTest Labs** listeden.
1. Talep edilebilir VM oluşturmak istediğiniz Laboratuvar labs listesinden seçin.  
1. Laboratuvar'ın **genel bakış** bölmesinde **+ Ekle**.  

    ![VM düğmesi ekleme](./media/devtest-lab-add-vm/devtestlab-home-blade-add-vm.png)

1. Üzerinde **temel seçin** alanında, sanal makine için temel bir seçin.
1. İçinde **sanal makine** bölmesinde, yeni sanal makine için bir ad girin **sanal makine adı** metin kutusu.

    ![Laboratuvar VM bölmesi](./media/devtest-lab-add-vm/devtestlab-lab-vm-blade.png)

1. Girin bir **kullanıcı adı** sanal makinede yönetici ayrıcalıkları verildi.  
1. Depolanan bir parola kullanmak istiyorsanız, [gizli dizi deposu](https://azure.microsoft.com/updates/azure-devtest-labs-keep-your-secrets-safe-and-easy-to-use-with-the-new-personal-secret-store)seçin **kaydedilmiş bir gizli diziyi kullanın**ve, gizli dizisini (parola) karşılık gelen bir anahtar değeri belirtin. Aksi takdirde etiketli metin alanına bir parola girin **bir değer yazın**.
1. **Sanal makine diski türünü** Laboratuvar sanal makineler için hangi depolama disk türüne izin belirler.
1. Seçin **sanal makine boyutu** ve işlemci çekirdeği ve RAM boyutu oluşturmak için sanal sabit sürücü boyutu belirtin önceden tanımlanmış öğelerden birini seçin.
1. Seçin **Yapıtları** ve yapıtları listesinden seçin ve temel görüntüye eklemek istediğiniz yapıtları yapılandırın. DevTest Labs kullanarak yeni veya yapıtları, yapılandırma başvurmak [var olan bir yapıyı bir VM'ye ekleme](devtest-lab-add-vm.md#add-an-existing-artifact-to-a-vm) bölümüne ve ardından tamamladığınızda buraya dönün.
1. Seçin **Gelişmiş ayarlar** sanal makinenin ağ seçeneklerini ve sona erme seçeneklerini yapılandırmak için. Altında **talep seçenekleri**, seçin **Evet** makineyi talep edilebilir yapma.

  ![Talep edilebilir VM yapmak bu seçeneği seçin.](./media/devtest-lab-add-vm/devtestlab-claim-VM-option.png)

1. Görüntülemek veya Azure Resource Manager şablonu kopyalamak istiyorsanız, başvurmak [Kaydet Azure Resource Manager şablonu](devtest-lab-add-vm.md#save-azure-resource-manager-template) bölümünde ve bitirdikten sonra buraya dönün.
1. Seçin **Oluştur** belirtilen VM'yi laboratuvara ekleme için.

   Sanal makinenin oluşturma durumu görüntülenir, ilk olarak **oluşturma**, ardından olarak **çalıştıran** VM başlatıldıktan sonra.

> [!NOTE]
> Laboratuvar Vm'leri aracılığıyla dağıtırsanız [Azure Resource Manager şablonları](devtest-lab-create-environment-from-arm.md), ayarlayarak talep edilebilir VM'ler oluşturabilirsiniz **allowClaim** özelliği true olarak özellikler bölümü.
>
>

## <a name="using-a-claimable-vm"></a>Talep edilebilir VM kullanma

Bir kullanıcı, "Talep edilebilir sanal makineler" listesinden herhangi bir VM, aşağıdaki adımlardan birini yaparak alabilir:

* "Talep edilebilir sanal makineler" Laboratuvar'ın "Genel bakış" bölmesinin alt kısmındaki listesinden listedeki Vm'lerden birinin sağ tıklayın ve seçin **talep makine**.

 ![Belirli bir talep edilebilir VM isteyin.](./media/devtest-lab-add-vm/devtestlab-claim-VM.png)


* "Genel bakış" bölmesinin en üstünde **herhangi talep**. Talep edilebilir VM'ler listesinden rastgele bir sanal makineye atanır.

 ![Herhangi bir talep edilebilir VM isteyin.](./media/devtest-lab-add-vm/devtestlab-claim-any.png)


Bir sanal makine bir kullanıcı talepleri, sonra "Sanal makinelerim" kendi listesine taşınır ve artık herhangi bir kullanıcı tarafından talep edilebilir değil.

## <a name="unclaim-a-vm"></a>Bir VM talebi kaldırır

Bir kullanıcı kullanarak bir talep edilen sanal tamamlandı ve kullanılabilir başka birine hale getirmek istediği, aşağıdaki adımlardan birini yaparak bunların talep edilen VM talep edilebilir sanal makineler listesine döndürebilirsiniz:

- "Sanal makinelerim" listesinden – listedeki Vm'lerden birinin sağ tıklayın veya üç nokta işaretine (...) – seçip **Unclaim**.

  ![VM listesinde üzerinde VM talebi kaldırır.](./media/devtest-lab-add-vm/devtestlab-unclaim-VM2.png)

- "Sanal makinelerim" listesinde, kendi yönetim bölmesini açın ve ardından sanal Makineyi seçin **Unclaim** üst menü çubuğundan.

  ![Yönetim bölmesinde sanal makinenin bir VM'de talebi kaldırır.](./media/devtest-lab-add-vm/devtestlab-unclaim-VM.png)

Bir kullanıcı bir VM unclaims, bunlar artık o belirli Laboratuvar sanal makinesi için herhangi bir izni yoktur.

### <a name="transferring-the-data-disk"></a>Veri diski aktarma
Talep edilebilir VM varsa, onu ve bir kullanıcının bağlı veri diski unclaims, VM veri diski kalır. Ne zaman başka bir kullanıcı daha sonra bu yeni kullanıcı VM veri diski, hem de VM Talep veren talep.

Bu, "veri diski aktarma olarak" adı verilir. Veri diski daha sonra yeni kullanıcının listesinde kullanılabilir **veri disklerim** bunları yönetmek.

![Veri diskleri talebi kaldırır.](./media/devtest-lab-add-vm/devtestlab-unclaim-datadisks.png)



## <a name="next-steps"></a>Sonraki adımlar
* Oluşturulduktan sonra seçerek VM'ye bağlanabilir **Connect** , yönetim bölmesinde.
* Keşfedin [DevTest Labs Azure Resource Manager hızlı başlangıç Şablon Galerisi](https://github.com/Azure/azure-devtestlab/tree/master/Samples).
