---
title: Oluşturma ve Azure DevTest Labs laboratuvarda claimable Vm'lerde yönetme | Microsoft Docs
description: Azure DevTest Labs laboratuvarda claimable bir sanal makineye eklemeyi öğrenin
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
ms.openlocfilehash: 669dfab75f34a0d1f997dc34f600402d3c10669b
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="create-and-manage-claimable-vms-in-azure-devtest-labs"></a>Oluşturma ve Azure DevTest Labs claimable Vm'lerde yönetme
Benzer şekilde nasıl laboratuvara claimable VM ekleme, [standart VM eklemek](devtest-lab-add-vm.md) – bir *temel* ya da başka bir deyişle bir [özel görüntü](devtest-lab-create-template.md), [formülü](devtest-lab-manage-formulas.md), veya [Market görüntüsü](devtest-lab-configure-marketplace-images.md). Bu öğretici DevTest Labs laboratuvarda claimable VM eklemek için Azure portalını kullanarak size yol gösterir ve bir kullanıcı talebi ve VM unclaim için aşağıdaki işlemleri gösterir.

## <a name="steps-to-add-a-claimable-vm-to-a-lab-in-azure-devtest-labs"></a>Azure DevTest Labs laboratuvarda claimable VM eklemek için adımları
1. [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
1. Seçin **tüm hizmetleri**ve ardından **DevTest Labs** listeden.
1. Labs listesinden claimable VM oluşturmak istediğiniz Laboratuvar seçin.  
1. Laboratuvar 's üzerinde **genel bakış** bölmesinde, **+ Ekle**.  

    ![VM düğme ekleme](./media/devtest-lab-add-vm/devtestlab-home-blade-add-vm.png)

1. Üzerinde **bir temel seçin** alanı, temel bir VM için seçin.
1. İçinde **sanal makine** bölmesinde, yeni sanal makine için bir ad girin **sanal makine adı** metin kutusu.

    ![Laboratuvar VM bölmesi](./media/devtest-lab-add-vm/devtestlab-lab-vm-blade.png)

1. Girin bir **kullanıcı adı** sanal makinede yönetici ayrıcalıkları verilmiş.  
1. Depolanmış bir parola kullanmak istiyorsanız, [gizli deposu](https://azure.microsoft.com/updates/azure-devtest-labs-keep-your-secrets-safe-and-easy-to-use-with-the-new-personal-secret-store)seçin **kaydedilmiş gizliliği kullanın**ve parolanızı (parola) karşılık gelen bir anahtar değeri belirtin. Aksi halde, bir parola etiketli metin alanına girin **bir değer yazın**.
1. **Sanal makine disk türü** hangi depolama disk türü laboratuvara sanal makineler için izin verilen belirler.
1. Seçin **sanal makine boyutu** ve işlemci çekirdeği, RAM boyutu ve sabit sürücü boyutu oluşturmak için VM belirtin önceden tanımlanmış öğelerden birini seçin.
1. Seçin **yapıları** yapılarının listesinden seçin ve temel görüntü eklemek istediğiniz yapıları yapılandırın. DevTest Labs yeni veya yapıları yapılandırma başvurmak [bir VM'ye var artifact ekleyin](devtest-lab-add-vm.md#add-an-existing-artifact-to-a-vm) bölümünde ve ardından bitirdikten sonra buraya dönün.
1. Seçin **Gelişmiş ayarları** VM ağ seçeneklerini ve sona erme seçeneklerini yapılandırmak için. Altında **talep seçenekleri**, seçin **Evet** makine claimable yapma.

  ![VM claimable yapmak seçin.](./media/devtest-lab-add-vm/devtestlab-claim-VM-option.png)

1. Görüntülemek veya Azure Resource Manager şablonu kopyalamak istediğiniz oluştuysa, [Kaydet Azure Resource Manager şablonu](devtest-lab-add-vm.md#save-azure-resource-manager-template) bölümünde ve bittiğinde buraya dönün.
1. Seçin **oluşturma** Laboratuvar için belirtilen VM eklemek için.

   VM oluşturma durumu görüntülenir, ilk olarak **oluşturma**, ardından olarak **çalıştıran** VM başlatıldıktan sonra.

> [!NOTE]
> Laboratuvar VM'ler aracılığıyla dağıtırsanız [Azure Resource Manager şablonları](devtest-lab-create-environment-from-arm.md), ayarlayarak claimable VM'ler oluşturabilirsiniz **allowClaim** özelliğinin özellikleri bölümündeki true.
>
>

## <a name="using-a-claimable-vm"></a>Claimable VM kullanma

Bir kullanıcı, aşağıdaki adımlardan birini yaparak "Claimable sanal makineler" listesinden herhangi bir VM talep:

* "Claimable sanal makineleri" Laboratuvar 's "Genel bakış" bölmesinin listesinden listesinde VM'ler birine sağ tıklayın ve seçin **talep makine**.

 ![Belirli bir claimable VM'yi isteyin.](./media/devtest-lab-add-vm/devtestlab-claim-VM.png)


* "Genel bakış" bölmenin en üstünde seçin **herhangi bir talep**. Rastgele bir sanal makine claimable VM'ler listesinden atanır.

 ![Tüm claimable VM isteyin.](./media/devtest-lab-add-vm/devtestlab-claim-any.png)


Bir kullanıcı bir VM talep sonra kendi "Benim sanal makineler" listesine taşınır ve artık herhangi bir kullanıcı tarafından claimable değil.

## <a name="unclaim-a-vm"></a>Bir VM unclaim

Bir kullanıcı bir talep edilen VM kullanarak tamamlandı ve başka birine hale istediği zaman aşağıdaki adımlardan birini yaparak bunlar talep edilen VM claimable sanal makinelerin listesini döndürebilirsiniz:

- "Benim sanal makineler" listesinden listesinde – VM'ler birini sağ tıklayın veya üç nokta (...) – seçin ve seçin **Unclaim**.

  ![VM VM listesine unclaim.](./media/devtest-lab-add-vm/devtestlab-unclaim-VM2.png)

- "Benim sanal makineler" listesinde Yönetimi bölmesini açın ve ardından seçmek için bir VM seçin **Unclaim** üst menü çubuğundan.

  ![VM VM'ın yönetim bölmesinde unclaim.](./media/devtest-lab-add-vm/devtestlab-unclaim-VM.png)

Bir kullanıcı bir VM unclaims, bunlar artık bu belirli Laboratuvar VM için herhangi bir iznine sahip.

### <a name="transferring-the-data-disk"></a>Veri diski aktarma
Claimable VM varsa, bunu ve bir kullanıcının bağlı bir veri diski unclaims, veri diski VM'ye kalır. Ne zaman başka bir kullanıcı daha sonra VM, bu yeni kullanıcı verileri diskin yanı sıra VM Talep veren talep.

Bu, "veri diski aktarma" olarak bilinir. Veri diski daha sonra yeni kullanıcının listesinde kullanılabilir **veri disklerim** bunları yönetmek.

![Veri diskleri unclaim.](./media/devtest-lab-add-vm/devtestlab-unclaim-datadisks.png)



## <a name="next-steps"></a>Sonraki adımlar
* Bir kez oluşturulduktan sonra seçerek VM'ye bağlanabilir **Bağlan** kendi yönetim bölmesinde.
* Araştır [DevTest Labs Azure Resource Manager hızlı başlangıç Şablon Galerisi](https://github.com/Azure/azure-devtestlab/tree/master/Samples).
