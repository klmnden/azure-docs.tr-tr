---
title: Azure DevTest Labs laboratuvarda VM ekleme | Microsoft Docs
description: Azure DevTest Labs laboratuvarda için bir sanal makineye eklemeyi öğrenin
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
ms.date: 04/17/2018
ms.author: spelluru
ms.openlocfilehash: 5f953cd6f33e5d46098566740efbf83a5fd80799
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="add-a-vm-to-a-lab-in-azure-devtest-labs"></a>Azure DevTest Labs laboratuvarda VM ekleme
Zaten varsa [ilk VM oluşturulan](devtest-lab-create-first-vm.md), büyük olasılıkla bunu bir önceden yüklenmiş yaptığınız [Market görüntüsü](devtest-lab-configure-marketplace-images.md). Şimdi, laboratuvarınız için sonraki VM'ler eklemek isterseniz, ayrıca seçebileceğiniz bir *temel* ya da başka bir deyişle bir [özel görüntü](devtest-lab-create-template.md) veya [formülü](devtest-lab-manage-formulas.md). Bu öğreticide, bir VM DevTest Labs laboratuvarda eklemek için Azure portalını kullanarak aracılığıyla açıklanmaktadır.

Bu makalede ayrıca laboratuvarınızda bir VM için yapıları yönetme gösterilmektedir.

## <a name="steps-to-add-a-vm-to-a-lab-in-azure-devtest-labs"></a>Azure DevTest Labs laboratuvarda VM eklemek için adımları
1. [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
1. Seçin **tüm hizmetleri**ve ardından **DevTest Labs** listeden.
1. VM oluşturmak istediğiniz Laboratuvar labs listesinden seçin.  
1. Laboratuvar 's üzerinde **genel bakış** bölmesinde, **+ Ekle**.  

    ![VM düğme ekleme](./media/devtest-lab-add-vm/devtestlab-home-blade-add-vm.png)

1. Üzerinde **bir temel seçin** bölmesinde, VM için temel bir seçin.
1. Üzerinde **sanal makine** bölmesinde, yeni sanal makine için bir ad girin **sanal makine adı** metin kutusu.

    ![Laboratuvar VM bölmesi](./media/devtest-lab-add-vm/devtestlab-lab-vm-blade.png)

1. Girin bir **kullanıcı adı** sanal makinede yönetici ayrıcalıkları verilmiş.  
1. Depolanmış bir parola kullanmak istiyorsanız, [gizli deposu](https://azure.microsoft.com/updates/azure-devtest-labs-keep-your-secrets-safe-and-easy-to-use-with-the-new-personal-secret-store)seçin **kaydedilmiş gizliliği kullanın**ve parolanızı (parola) karşılık gelen bir anahtar değeri belirtin. Aksi halde, bir parola etiketli metin alanına girin **bir değer yazın**.
1. **Sanal makine disk türü** hangi depolama disk türü laboratuvara sanal makineler için izin verilen belirler.
1. Seçin **sanal makine boyutu** ve işlemci çekirdeği, RAM boyutu ve sabit sürücü boyutu oluşturmak için VM belirtin önceden tanımlanmış öğelerden birini seçin.
1. Seçin **yapıları** - yapıları - listesinden seçin ve temel görüntü eklemek istediğiniz yapıları yapılandırın.
    **Not:** DevTest Labs yeni veya yapıları yapılandırma başvurmak [bir VM'ye var artifact ekleyin](#add-an-existing-artifact-to-a-vm) bölümünde ve ardından bitirdikten sonra buraya dönün.
1. Seçin **Gelişmiş ayarları** VM ağ seçeneklerini ve sona erme seçeneklerini yapılandırmak için. 

   Bir süre sonu seçeneğini ayarlamak için üzerinde otomatik olarak VM silinir bir tarih belirtmek için takvim simgesini seçin.  Varsayılan olarak, VM asla sona erecek. 
1. Görüntülemek veya Azure Resource Manager şablonu kopyalamak istediğiniz oluştuysa, [Kaydet Azure Resource Manager şablonu](#save-azure-resource-manager-template) bölümünde ve bittiğinde buraya dönün.
1. Seçin **oluşturma** Laboratuvar için belirtilen VM eklemek için.
1. Laboratuvar bölmesinde durumunu VM oluşturma: ilk olarak görüntüler **oluşturma**, ardından olarak **çalıştıran** VM başlatıldıktan sonra.

> [!NOTE]
> [Claimable VM eklemek](devtest-lab-add-claimable-vm.md) , böylece laboratuvara herhangi bir kullanıcı tarafından kullanılabilir VM claimable nasıl yapılacağını gösterir.
>
>

## <a name="add-an-existing-artifact-to-a-vm"></a>Bir VM'ye var artifact ekleyin
Bir VM oluştururken, varolan yapıları ekleyebilirsiniz. Her Laboratuvar ortak DevTest Labs Yapıt deposu yanı sıra oluşturulan ve kendi Yapıt deposu eklenen yapıları yapılardan içerir.

* Azure DevTest Labs *yapıları* belirlemenize olanak *Eylemler* VM Windows PowerShell komut dosyası çalıştırarak, Bash komutlarını çalıştırarak ve yazılım yükleme gibi sağlandığında gerçekleştirilir.
* Yapı *parametreleri* belirli senaryonuz için yapıyı özelleştirmenize olanak tanır

Yapıları oluşturma, makaleye bakın bulmak için [DevTest Labs ile kullanılmak üzere kendi yapıları Yazar öğrenin](devtest-lab-artifact-author.md).

1. [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
1. Seçin **tüm hizmetleri**ve ardından **DevTest Labs** listeden.
1. Çalışmak istediğiniz VM içeren Laboratuvar labs listesinden seçin.  
1. Seçin **My sanal makineleri**.
1. İstenen VM'yi seçin.
1. Seçin **yapıları yönetme**. 
1. Seçin **yapıları uygulamak**.
1. Üzerinde **yapıları uygulamak** bölmesinde, VM'ye eklemek istediğiniz yapıyı seçin.
1. Üzerinde **ekleyin yapıt** bölmesinde gerekli parametre değerlerini ve gereken isteğe bağlı parametreleri girin.  
1. Seçin **Ekle** yapıyı ekleyin ve dönmek için **yapıları uygulamak** bölmesi.
1. Yapılar, VM için gerektiği şekilde ekleme devam edin.
1. Yapıtları ekledikten sonra [yapıları çalıştırılacağı sırasını değiştirmek](#change-the-order-in-which-artifacts-are-run). Ayrıca geri gidebilirsiniz [görüntülemek veya bir yapı değiştirmek](#view-or-modify-an-artifact).
1. Ekleme yapıları bittiğinde, seçin **Uygula**

## <a name="change-the-order-in-which-artifacts-are-run"></a>Yapıları çalıştırdığınız sırasını değiştirme
Varsayılan olarak, Eylemler yapılarının VM'ye eklenmiş sırada yürütülür. Aşağıdaki adımları yapıları çalıştığı sırayı değiştirmek nasıl gösterilmektedir.

1. Üstündeki **yapıları uygulamak** bölmesinde, VM için eklenene yapıları sayısını gösteren bağlantısını seçin.
   
    ![VM eklenen yapıları sayısı](./media/devtest-lab-add-vm-with-artifacts/devtestlab-add-artifacts-blade-selected-artifacts.png)
1. Üzerinde **seçili yapıları** bölmesinde, sürükleyip yapıları istenen sıralamaya. **Not:** yapıyı sürükleyerek sorun yaşıyorsanız, yapıyı sol taraftan sürükleyerek emin olun. 
1. Tamamladığınızda **Tamam**’ı seçin.  

## <a name="view-or-modify-an-artifact"></a>Görüntüleme veya bir yapı değiştirme
Aşağıdaki adımları görüntülemek veya bir yapı parametreleri değiştirmek nasıl gösterilmektedir:

1. Üstündeki **yapıları uygulamak** bölmesinde, VM için eklenene yapıları sayısını gösteren bağlantısını seçin.
   
    ![VM eklenen yapıları sayısı](./media/devtest-lab-add-vm-with-artifacts/devtestlab-add-artifacts-blade-selected-artifacts.png)
1. Üzerinde **seçili yapıları** bölmesinde, görüntülemek veya düzenlemek istediğiniz yapıyı seçin.  
1. Üzerinde **ekleyin yapıt** bölmesi, tüm değişikliklerin gerekli ve seçin yapma **Tamam** kapatmak için **ekleyin yapıt** bölmesi.
1. Seçin **Tamam** kapatmak için **seçili yapıları** bölmesi.

## <a name="save-azure-resource-manager-template"></a>Azure Resource Manager şablonunu Kaydet
Bir Azure Resource Manager şablonu tekrarlanabilir bir dağıtımı tanımlamanın bildirim temelli bir yolunu sağlar. Aşağıdaki adımlar, oluşturulan VM için Azure Resource Manager şablonu kaydetmek açıklanmaktadır.
Kaydedildikten sonra Azure Resource Manager şablonu kullanabilirsiniz [Azure PowerShell ile yeni VM'ler dağıtmak](../azure-resource-manager/resource-group-overview.md#template-deployment).

1. Üzerinde **sanal makine** bölmesinde, **ARM şablonu görüntüleme**.
2. Üzerinde **görünüm Azure Resource Manager şablonu** bölmesinde, şablon metni seçin.
3. Seçili metni panoya kopyalayın.
4. Seçin **Tamam** kapatmak için **Azure Resource Manager şablonu görüntüleme bölmesinde**.
5. Bir metin düzenleyicisinde açın.
6. Panodaki şablonu metni yapıştırın.
7. Daha sonra kullanmak için dosyayı kaydedin.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Sonraki adımlar
* VM oluşturulduktan sonra seçerek VM'ye bağlanabilir **Bağlan** VM bölmesi.
* Bilgi edinmek için nasıl [DevTest Labs VM için özel yapılar oluşturma](devtest-lab-artifact-author.md).
* Araştır [DevTest Labs Azure Resource Manager hızlı başlangıç Şablon Galerisi](https://github.com/Azure/azure-devtestlab/tree/master/Samples).
