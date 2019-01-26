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
ms.openlocfilehash: c72e10991e27f7d616703e635ee6e1a18122afc5
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/26/2019
ms.locfileid: "55078085"
---
# <a name="create-your-first-vm-in-a-lab-in-azure-devtest-labs"></a>Azure DevTest labs'deki bir laboratuvara ilk VM'nizi oluşturun

İlk DevTest Labs erişmek ve ilk VM'nizi oluşturmak istiyorsanız, olasılıkla önceden yüklenmiş kullanarak bunu [temel Market görüntüsü](devtest-lab-configure-marketplace-images.md). Daha sonra ayrıca arasından seçim yapmanız mümkün olacaktır bir [özel görüntü ve bir formül](devtest-lab-add-vm.md) daha fazla sanal makine oluştururken. 

Bu öğreticide Azure portalı kullanarak ilk VM'nizi DevTest labs'deki bir laboratuvara ekleme adımları gösterilmektedir.

## <a name="steps-to-add-your-first-vm-to-a-lab-in-azure-devtest-labs"></a>Azure DevTest labs'deki bir laboratuvara ilk VM'nizi eklemek için adımları
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

## <a name="next-steps"></a>Sonraki adımlar
* VM oluşturulduktan sonra seçerek VM'ye bağlanabilir **Connect** sanal makinenin sayfasında.
* Kullanıma [bir VM'yi bir laboratuvara ekleme](devtest-lab-add-vm.md) laboratuvarınızda sonraki Vm'leri ekleme hakkında daha ayrıntılı bilgi için.
* Keşfedin [DevTest Labs Azure Resource Manager hızlı başlangıç Şablon Galerisi](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates).
