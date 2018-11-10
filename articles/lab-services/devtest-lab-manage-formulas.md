---
title: Sanal makineler oluşturmak için Azure DevTest labs'deki formülleri yönetme | Microsoft Docs
description: Azure DevTest Labs formülleri kaldırır ve güncelleştirme hakkında bilgi edinin
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: 841dd95a-657f-4d80-ba26-59a9b5104fe4
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: spelluru
ms.openlocfilehash: 47699925f057aab25fe6f7c1c7d0b0620e7e4dbe
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51228003"
---
# <a name="manage-azure-devtest-labs-formulas"></a>Azure DevTest Labs formülleri yönetme

[!INCLUDE [devtest-lab-formula-definition](../../includes/devtest-lab-formula-definition.md)]

Bu makalede bir formül temel (özel görüntü, Market görüntüsü veya başka bir formül) veya varolan bir VM'yi oluşturma işlemini göstermektedir. Bu makalede ayrıca, mevcut formülleri yönetme aracılığıyla size yol gösterir.

## <a name="create-a-formula"></a>Formül oluşturma
DevTest Labs kimseyle *kullanıcılar* izinlerdir formülde bir temel olarak kullanan VM'ler oluşturabilirsiniz. Formülleri oluşturmanın iki yolu vardır: 

* Formülün tüm özelliklerini tanımlamak bir temel - kullanın.
* Var olan bir laboratuvarda VM - bağlı olarak var olan bir sanal makinenin ayarlarını bir formül oluşturmak istediğinizde kullanın.

Kullanıcıları ve izinleri ekleme hakkında daha fazla bilgi için bkz. [Azure DevTest Labs'de sahibini ve kullanıcıları ekleme](./devtest-lab-add-devtest-user.md).

### <a name="create-a-formula-from-a-base"></a>Bir temel formül oluşturma
Aşağıdaki adımlar özel bir görüntü, Market görüntüsü ya da başka bir formülü bir formül oluşturma işleminde size kılavuzluk eder.

1. [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.

2. Seçin **tüm hizmetleri**ve ardından **DevTest Labs** listeden.

3. İstenen Laboratuvar labs listesinden seçin.  

4. Laboratuvar dikey penceresinde seçin **formülleri (yeniden kullanılabilir tabanları)**.
   
    ![Formül menüsü](./media/devtest-lab-create-formulas/lab-settings-formulas.png)

5. Üzerinde **formülleri** dikey penceresinde **+ Ekle**.
   
    ![Bir formül ekleyin](./media/devtest-lab-create-formulas/add-formula.png)

6. Üzerinde **temel seçin** dikey penceresinde istediğiniz için temel (özel görüntü, Market görüntü veya formül) seçin, formül oluşturma.
   
    ![Temel listesi](./media/devtest-lab-create-formulas/base-list.png)

7. Üzerinde **formül oluşturma** dikey penceresinde aşağıdaki değerleri belirtin:
   
    * **Formül adı** -formülünüz için bir ad girin. Bir VM oluşturduğunuzda, bu değeri temel görüntüleri listesinde görüntülenir. Adı, türü ve geçerli değil, bir ileti geçerli bir adı gereksinimlerini gösterir doğrulanır.
    * **Açıklama** -formülünüz için anlamlı bir açıklama girin. Bir VM oluşturduğunuzda, bu değer formülün bağlam menüsünden kullanılabilir.
    * **Kullanıcı adı** -yönetici ayrıcalıkları verilmiş bir kullanıcı adı girin.
    * **Parola** - girin - veya açılan listeden seçin - belirtilen kullanıcı için kullanmak istediğiniz gizli dizisini (parola) ile ilişkili olan bir değer. Bir anahtar kasasındaki gizli dizileri kaydetme ve Laboratuvar kaynaklarını oluştururken kullanma hakkında bilgi edinmek için [gizli dizileri Azure Key Vault'ta Store](devtest-lab-store-secrets-in-key-vault.md).
    * **Sanal makine diski türünü** - ya da HDD (sabit disk sürücüsü) belirtin veya bu temel görüntü kullanılarak sağlanan sanal makineler için hangi depolama disk türüne belirtmek için SSD (katı hal sürücüsü) izin verilir.
    * ** Sanal makine boyutu ** - işlemci çekirdeği ve RAM boyutu oluşturmak için sanal sabit sürücü boyutu belirlediğiniz önceden tanımlanmış öğeleri birini seçin. 
    * **Yapıtları** - açmak için seçin **yapıt Ekle** dikey penceresinde, seçin ve temel görüntüye eklemek istediğiniz yapıtları yapılandırın. Yapılar hakkında daha fazla bilgi için bkz: [Azure DevTest Labs'teki sanal makineniz için özel yapıtlar oluşturma](devtest-lab-artifact-author.md).
    * **Gelişmiş ayarlar** - açmak için seçin **Gelişmiş** dikey penceresinde aşağıdaki ayarları yapılandırdığınız:
        * **Sanal ağ** -istenen sanal ağ belirtin.
        * **Alt ağ** -istenen alt ağ belirtin.    
        * **IP adresi yapılandırması** -genel, özel veya paylaşılan IP adreslerini isteyip istemediğinizi belirtin. Paylaşılan IP adresleri hakkında daha fazla bilgi için bkz. [anlayın paylaşılan IP adresleri Azure DevTest labs'deki](./devtest-lab-shared-ip.md).
        * **Bu makineyi talep edilebilir hale** -bir makine "talep edilebilir" yapma anlamına gelir, sahipliği oluşturma sırasında atanması değil. Bunun yerine Laboratuvar kullanıcıları ("talep") makine Laboratuvar dikey penceresinde sahipliğini mümkün olacaktır.     
    * **Görüntü** -Bu alan, önceki dikey pencerede seçtiğiniz temel görüntüyü adını görüntüler. 
     
       ![Formül oluşturma](./media/devtest-lab-create-formulas/create-formula.png)

8. Seçin **Oluştur** formül oluşturma.

9. Formülü oluşturduğunuzda listede görüntüler **formülleri** dikey penceresi.

### <a name="create-a-formula-from-a-vm"></a>VM'den bir formül oluşturma
Aşağıdaki adımları varolan bir VM'yi temel alan bir formül oluşturma işleminde size kılavuzluk eder. 

> [!NOTE]
> Bir sanal makineden bir formül oluşturmak için VM 30 Mart 2016'dan sonra oluşturulmuş gerekir. 
> 
> 

1. [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
2. Seçin **tüm hizmetleri**ve ardından **DevTest Labs** listeden.
3. İstenen Laboratuvar labs listesinden seçin.  
4. Laboratuvar'ın **genel bakış** dikey penceresinde, istediğiniz bir formül oluşturmak sanal Makineyi seçin.
   
    ![Labs sanal makineleri](./media/devtest-lab-create-formulas/my-vms.png)
5. Sanal makinenin dikey penceresinde seçin **(yeniden kullanılabilir temel) formülü oluşturursunuz**.
   
    ![Formül oluşturma](./media/devtest-lab-create-formulas/create-formula-menu.png)
6. Üzerinde **formül oluşturma** dikey penceresinde girin bir **adı** ve **açıklama** için yeni formül.
   
    ![Formül dikey penceresi oluşturma](./media/devtest-lab-create-formulas/create-formula-blade.png)
7. Seçin **Tamam** formül oluşturma.

## <a name="modify-a-formula"></a>Bir formülü değiştirme
Bir formül değiştirmek için aşağıdaki adımları izleyin:

1. [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
2. Seçin **tüm hizmetleri**ve ardından **DevTest Labs** listeden.
3. İstenen Laboratuvar labs listesinden seçin.  
4. Laboratuvar dikey penceresinde seçin **formülleri (yeniden kullanılabilir tabanları)**.
   
    ![Formül menüsü](./media/devtest-lab-manage-formulas/lab-settings-formulas.png)
5. Üzerinde **Laboratuvar formülleri** dikey penceresinde değiştirmek istediğiniz formülü seçin.
6. Üzerinde **güncelleştirme formül** dikey penceresinde istediğiniz düzenlemeleri yapıp seçin **güncelleştirme**.

## <a name="delete-a-formula"></a>Bir formül Sil
Bir formülü silmek için aşağıdaki adımları izleyin:

1. [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
2. Seçin **tüm hizmetleri**ve ardından **DevTest Labs** listeden.
3. İstenen Laboratuvar labs listesinden seçin.  
4. Laboratuvar **ayarları** dikey penceresinde **formülleri**.
   
    ![Formül menüsü](./media/devtest-lab-manage-formulas/lab-settings-formulas.png)
5. Üzerinde **Laboratuvar formülleri** dikey penceresinde silmek istediğiniz formül sağındaki üç nokta simgesini seçin.
   
    ![Formül menüsü](./media/devtest-lab-manage-formulas/lab-formulas-blade.png)
6. Formülün bağlam menüsünde **Sil**.
   
    ![Formül bağlam menüsü](./media/devtest-lab-manage-formulas/formula-delete-context-menu.png)
7. Seçin **Evet** silme onay iletişim kutusunda.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>İlgili blog gönderileri
* [Özel görüntü veya formül?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)

## <a name="next-steps"></a>Sonraki adımlar
Bir VM oluştururken kullanmak için bir formül oluşturduktan sonra sonraki adım olarak [bir VM, laboratuvara ekleme](devtest-lab-add-vm.md).

