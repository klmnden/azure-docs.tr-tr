---
title: Bir VM, Azure DevTest labs'deki bir laboratuvara ekleme | Microsoft Docs
description: Bir sanal makine, Azure DevTest labs'deki bir laboratuvara ekleme hakkında bilgi edinin
services: devtest-lab,virtual-machines
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.assetid: ''
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/25/2019
ms.author: spelluru
ms.openlocfilehash: be5ff2c59878cc966e73d89c18343b0a6ea3d89c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60311625"
---
# <a name="add-a-vm-to-a-lab-in-azure-devtest-labs"></a>Azure DevTest labs'deki bir laboratuvara VM ekleme
Zaten varsa [ilk VM'nizi oluşturulan](tutorial-create-custom-lab.md#add-a-vm-to-the-lab), büyük olasılıkla bunu önceden yüklü yaptığınız [Market görüntüsü](devtest-lab-configure-marketplace-images.md). Şimdi, laboratuvarınız için sonraki VM'ler eklemek istiyorsanız, ayrıca seçebileceğiniz bir *temel* ya da diğer bir deyişle bir [özel görüntü](devtest-lab-create-template.md) veya [formül](devtest-lab-manage-formulas.md). Bu öğreticide, Azure portalını kullanarak bir VM için DevTest labs'deki bir laboratuvara ekleme yoluyla açıklanmaktadır.

Bu makalede ayrıca laboratuvarınızda bir VM yapıtları yönetme işlemini gösterir.

## <a name="steps-to-add-a-vm-to-a-lab-in-azure-devtest-labs"></a>Bir VM için Azure DevTest labs'deki bir laboratuvara ekleme adımları
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

        ![Bir temel seçin](./media/tutorial-create-custom-lab/new-virtual-machine.png)

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

        ![Bir temel seçin](./media/tutorial-create-custom-lab/new-vm-advanced-settings.png)
1. Seçin **Oluştur** belirtilen VM'yi laboratuvara ekleme için.

   Laboratuvar sayfanın durumunu VM oluşturma - ilk olarak görüntüler **oluşturma**, ardından olarak **çalıştıran** VM başlatıldıktan sonra.

    ![Sanal makine oluşturma durumu](./media/tutorial-create-custom-lab/vm-creation-status.png)

## <a name="add-an-existing-artifact-to-a-vm"></a>Var olan bir yapıyı bir VM'ye ekleme
Bir VM oluştururken, mevcut yapıt ekleyebilirsiniz. Her laboratuar oluşturduğunuz ve kendi Yapıt deposuna eklendi yapıtları yanı sıra ortak DevTest Labs Yapıt deposu yapıları içerir.

* Azure DevTest Labs *yapıtları* belirtmenizi sağlar *eylemleri* VM hazırlandığında Windows PowerShell komut dosyaları çalıştırmak, Bash komutları çalıştırma ve yazılım yükleme gibi gerçekleştirilir.
* Yapıt *parametreleri* belirli senaryonuz için yapıt özelleştirmenize olanak tanır.

Yapıtlar oluşturmak için bkz nasıl keşfetmek için [DevTest Labs ile kullanmak için kendi yapıtları Yazar öğrenin](devtest-lab-artifact-author.md).

1. [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
1. Seçin **tüm hizmetleri**ve ardından **DevTest Labs** listeden.
1. Çalışmak istediğiniz sanal Makineyi içeren Laboratuvar labs listesinden seçin.
1. Seçin **sanal makinelerim**.
1. İstediğiniz VM'yi seçin.
1. Seçin **yapıtları yönetme**.
1. Seçin **yapıları uygulamak**.
1. Üzerinde **yapıları uygulamak** bölmesinde istediğiniz sanal Makineye eklemek için yapıt seçin.
1. Üzerinde **yapıt ekleme** bölmesinde gerekli parametre değerlerini ve ihtiyaç duyduğunuz herhangi bir isteğe bağlı parametreler girin.
1. Seçin **Ekle** yapıt ekleme ve dönmek için **yapıları uygulamak** bölmesi.
1. Sanal Makineniz için gerektiği şekilde yapıtları eklemeye devam edebilirsiniz.
1. Yapıtları ekledikten sonra [yapıtlar çalıştırılma sırasını değiştirmek](#change-the-order-in-which-artifacts-are-run). Aynı zamanda geri gidebilirsiniz [görüntülemek veya değiştirmek yapı](#view-or-modify-an-artifact).
1. Ekleme yapıtları işiniz bittiğinde seçin **Uygula**

## <a name="change-the-order-in-which-artifacts-are-run"></a>Yapıtları çalıştırdığınız sırasını değiştirme
Varsayılan olarak, yapıları eylemleri VM'ye eklenen sırayla yürütülür.
Aşağıdaki adımlar, yapılar çalıştığı sırasını değiştirme göstermektedir.

1. Üst kısmındaki **yapıları uygulamak** bölmesinde, sanal Makineye eklenmiş olan yapıların sayısını gösteren bağlantıyı seçin.

    ![VM'ye eklenen yapıların sayısı](./media/devtest-lab-add-vm-with-artifacts/devtestlab-add-artifacts-blade-selected-artifacts.png)
1. Üzerinde **seçili yapıtları** bölmesinde sürükleyip yapıtlar istenen sıralar. **Not:** Yapıt sürükleyerek sorun yaşıyorsanız, yapıt sol taraftan sürükleyerek emin olun.
1. Tamamladığınızda **Tamam**’ı seçin.

## <a name="view-or-modify-an-artifact"></a>Görüntüleme veya bir yapıt değiştirme
Aşağıdaki adımlar, görüntülemek veya bir yapı parametrelerini değiştirme göstermektedir:

1. Üst kısmındaki **yapıları uygulamak** bölmesinde, sanal Makineye eklenmiş olan yapıların sayısını gösteren bağlantıyı seçin.

    ![VM'ye eklenen yapıların sayısı](./media/devtest-lab-add-vm-with-artifacts/devtestlab-add-artifacts-blade-selected-artifacts.png)
1. Üzerinde **seçili yapıtları** bölmesinde, görüntülemek veya düzenlemek istediğiniz yapıyı seçin.
1. Üzerinde **yapıt ekleme** bölmesinde, tüm gereken değişiklikler ve seçin **Tamam** kapatmak için **yapıt ekleme** bölmesi.
1. Seçin **Tamam** kapatmak için **seçili yapıtları** bölmesi.

## <a name="save-azure-resource-manager-template"></a>Azure Resource Manager şablonunu Kaydet
Bir Azure Resource Manager şablonu, tekrarlanabilir bir dağıtımı tanımlamanın bildirim temelli bir yöntemini sağlar.
Aşağıdaki adımlarda, Azure Resource Manager şablonu kaydetmek için oluşturulan VM'i açıklanmaktadır.
Kaydedildikten sonra Azure Resource Manager şablonu için kullanabileceğiniz [Azure PowerShell ile yeni VM'ler dağıtma](../azure-resource-manager/resource-group-overview.md#template-deployment).

1. Üzerinde **sanal makine** bölmesinde **görünümü Azure Resource Manager şablonu**.
2. Üzerinde **görünümü Azure Resource Manager şablonu** bölmesinde şablonu metni seçin.
3. Seçili metni panoya kopyalayın.
4. Seçin **Tamam** kapatmak için **Azure Resource Manager şablonu görüntüleme bölmesinde**.
5. Bir metin düzenleyicisi açın.
6. Panodaki şablonu metni yapıştırın.
7. Daha sonra kullanmak için dosyayı kaydedin.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Sonraki adımlar
* VM oluşturulduktan sonra seçerek VM'ye bağlanabilir **Connect** sanal makinenin bölmesinde.
* Bilgi edinmek için nasıl [DevTest Labs sanal makinenizin özel yapıtlar oluşturma](devtest-lab-artifact-author.md).
* Keşfedin [DevTest Labs Azure Resource Manager hızlı başlangıç Şablon Galerisi](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/QuickStartTemplates).
