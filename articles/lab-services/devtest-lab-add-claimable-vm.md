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
ms.date: 01/25/2019
ms.author: spelluru
ms.openlocfilehash: fdffa3862f45b99c2c3f2ed41934e09247808ca7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60311829"
---
# <a name="create-and-manage-claimable-vms-in-azure-devtest-labs"></a>Azure DevTest labs'deki talep edilebilir VM'ler oluşturma ve yönetme
Benzer şekilde nasıl bir laboratuvara talep edilebilir VM ekleme, [standart VM ekleme](devtest-lab-add-vm.md) – bir *temel* ya da diğer bir deyişle bir [özel görüntü](devtest-lab-create-template.md), [formül](devtest-lab-manage-formulas.md) , veya [Market görüntüsü](devtest-lab-configure-marketplace-images.md). Bu öğreticide, Azure portalını kullanarak bir talep edilebilir VM DevTest labs'deki bir laboratuvara ekleme adımları gösterilmektedir ve bir kullanıcı talebi ve VM etmesini takip işlemlerini gösterir.

## <a name="steps-to-add-a-claimable-vm-to-a-lab-in-azure-devtest-labs"></a>Talep edilebilir VM için Azure DevTest labs'deki bir laboratuvara ekleme adımları
1. [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
1. Seçin **tüm hizmetleri**ve ardından **DevTest Labs** içinde **DEVOPS** bölümü. Seçerseniz * (yıldız yanındaki) **DevTest Labs** içinde **DEVOPS** bölümü. Bu eylem ekler **DevTest Labs** sol gezinti menüsüne böylece sonraki kolayca erişebilir. Ardından, seçebileceğiniz **DevTest Labs** sol gezinti menüsünde.

    ![Tüm hizmetleri - DevTest Labs seçin](./media/devtest-lab-create-lab/all-services-select.png)
1. VM'yi oluşturmak istediğiniz Laboratuvar labs listesinden seçin.
2. Laboratuvar'ın **genel bakış** sayfasında **+ Ekle**.

    ![VM düğmesi ekleme](./media/devtest-lab-add-vm/devtestlab-home-blade-add-vm.png)
1. Üzerinde **temel seçin** sayfasında, sanal makine için bir Market görüntüsü seçin.
1. Üzerinde **temel ayarları** sekmesinde **sanal makine** sayfasında, aşağıdaki eylemleri gerçekleştirin:
    1. İçinde VM için bir ad girin **sanal makine adı** metin kutusu. Metin kutusu sizin için otomatik olarak oluşturulan benzersiz bir ad ile önceden doldurulmuş olur. Ad içinde benzersiz bir 3 basamaklı sayı e-posta adresinizi kullanıcı adına karşılık gelir. Bu özellik, bir makine adı düşünün ve her bir makine oluşturma işleminde yazmak için zaman kazandırır. İstediğiniz tercih ettiğiniz bir ad ile otomatik olarak doldurulan bu alan geçersiz kılabilirsiniz. VM için otomatik olarak doldurulan adı geçersiz kılmak için bir ad girin. **sanal makine adı** metin kutusu.
    2. Girin bir **kullanıcı adı** sanal makinede yönetici ayrıcalıkları verildi. **Kullanıcı adı** makine otomatik olarak oluşturulan benzersiz bir ad ile önceden doldurulmuş için. Adı, kullanıcı adı, e-posta adresinizi içinde karşılık gelir. Bu özellik, yeni bir makine her oluşturduğunuzda, kullanıcı adına karar size zaman kazandırır. Yeniden istediğiniz bu otomatik olarak doldurulan alan bir kullanıcı adı, tercih ettiğiniz ile geçersiz kılabilirsiniz. Kullanıcı adı için otomatik olarak doldurulan değeri geçersiz kılmak için bir değer girin. **kullanıcı adı** metin kutusu. Bu kullanıcıya verilir **yönetici** sanal makinede ayrıcalıkları.
    3. Laboratuar ortamında ilk sanal makine oluşturuyorsanız, girin bir **parola** kullanıcı. Azure key vault'ta lab ile ilişkili varsayılan parola olarak bu parolayı seçilecek **varsayılan Parolayı Kaydet**. Varsayılan parola adlı anahtar Kasası'nda kaydedilir: **VmPassword**. Laboratuvarda, sonraki Vm'leri oluşturmaya çalıştığınızda **VmPassword** otomatik olarak seçilir **parola**. Geçersiz kılma değeri için temizleyin **kaydedilmiş bir gizli diziyi kullanın** onay kutusunu işaretleyin ve bir parola girin.

        Ayrıca gizli anahtar Kasası'nda ilk kaydedin ve laboratuar ortamında bir VM oluşturulurken kullanın. Daha fazla bilgi için [bir anahtar kasasındaki gizli dizileri Store](devtest-lab-store-secrets-in-key-vault.md). Anahtar Kasası'nda depolanan parola kullanmak için **kaydedilmiş bir gizli diziyi kullanın**ve, gizli dizisini (parola) karşılık gelen bir anahtar değeri belirtin.
    4. İçinde **daha fazla seçenek** bölümünden **değiştirme boyutu**. İşlemci çekirdeği ve RAM boyutu oluşturmak için sanal sabit sürücü boyutu belirtin önceden tanımlanmış öğelerden birini seçin.
    5. Seçin **ekleme veya kaldırma Yapıtları**. Seçin ve temel görüntüye eklemek istediğiniz yapıtları yapılandırın.
    **Not:** DevTest Labs kullanarak yeni veya yapıtları, yapılandırma başvurmak [var olan bir yapıyı bir VM'ye ekleme](./devtest-lab-add-vm.md#add-an-existing-artifact-to-a-vm) bölümüne ve ardından tamamladığınızda buraya dönün.
2. Geçiş **Gelişmiş ayarlar** üst kısmındaki sekme ve aşağıdaki eylemleri gerçekleştirin:
    1. VM, sanal ağ seçin **değiştirme VNet**.
    2. Alt ağ seçin **değiştirmek alt**.
    3. Sanal makinenin IP adresi olup olmadığını belirtin **genel, özel veya paylaşılan**.
    4. Otomatik olarak VM'yi silmek için belirtin **sona erme tarihi ve saati**.
    5. VM bir laboratuvar kullanıcı tarafından talep edilebilir hale getirmek için seçin **Evet** için **bu makineyi talep edilebilir hale** seçeneği.
    6. Sayısını **VM örneklerini** Laboratuvar kullanıcılarınız için kullanılabilir hale getirmek istediğiniz.
3. Seçin **Oluştur** belirtilen VM'yi laboratuvara ekleme için.

   Laboratuvar sayfanın durumunu VM oluşturma - ilk olarak görüntüler **oluşturma**, ardından olarak **çalıştıran** VM başlatıldıktan sonra.

> [!NOTE]
> Laboratuvar Vm'leri aracılığıyla dağıtırsanız [Azure Resource Manager şablonları](devtest-lab-create-environment-from-arm.md), ayarlayarak talep edilebilir VM'ler oluşturabilirsiniz **allowClaim** özelliği true olarak özellikler bölümü.


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
* Keşfedin [DevTest Labs Azure Resource Manager hızlı başlangıç Şablon Galerisi](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/QuickStartTemplates).
