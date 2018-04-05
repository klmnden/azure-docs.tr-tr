---
title: Sanal makineleri oluşturmak için Azure DevTest Labs formüller yönetme | Microsoft Docs
description: Güncelleştirme ve Azure DevTest Labs formüller kaldırma hakkında bilgi edinin
services: devtest-lab,virtual-machines
documentationcenter: na
author: craigcaseyMSFT
manager: douge
editor: ''
ms.assetid: 841dd95a-657f-4d80-ba26-59a9b5104fe4
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/07/2017
ms.author: v-craic
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 330f6ae246697d54c6bce0690346652b5f2e2dd0
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="manage-azure-devtest-labs-formulas"></a>Azure DevTest Labs formüller yönetme

[!INCLUDE [devtest-lab-formula-definition](../../includes/devtest-lab-formula-definition.md)]

Bu makalede temel (özel görüntü, Market görüntüsü veya başka bir formül) veya mevcut bir VM'yi bir formül oluşturulacağını gösterir. Bu makalede ayrıca, varolan formüller yönetme aracılığıyla size yol gösterir.

## <a name="create-a-formula"></a>Formül oluşturma
DevTest Labs kimseyle *kullanıcılar* izinleridir formül temel olarak kullanarak sanal makineleri oluşturabilirsiniz. Formülleri oluşturmanın iki yolu vardır: 

* Formül tüm özelliklerini tanımlamak bir taban - kullanın.
* Bir var olan Laboratuvar VM - mevcut bir VM'yi ayarlarınızı temel alan bir formül oluşturmak istediğinizde kullanın.

Kullanıcılar ve izinler ekleme hakkında daha fazla bilgi için bkz: [Azure DevTest Labs'de sahipleri ve kullanıcılar ekleme](./devtest-lab-add-devtest-user.md).

### <a name="create-a-formula-from-a-base"></a>Formül bir taban oluşturma
Aşağıdaki adımlar bir formül özel bir görüntü, Market görüntüsü ya da başka bir formül oluşturma işleminde size kılavuzluk eder.

1. [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.

2. Seçin **tüm hizmetleri**ve ardından **DevTest Labs** listeden.

3. İstenen Laboratuvar labs listesinden seçin.  

4. Laboratuvar 's dikey penceresinde, seçin **formüller (yeniden kullanılabilir tabanları)**.
   
    ![Formül menüsü](./media/devtest-lab-create-formulas/lab-settings-formulas.png)

5. Üzerinde **formüller** dikey penceresinde, select **+ Ekle**.
   
    ![Formül ekleme](./media/devtest-lab-create-formulas/add-formula.png)

6. Üzerinde **bir temel seçin** dikey penceresinde, seçin istediğiniz için temel (özel görüntü, Market görüntüsünü veya formül) formülü oluşturun.
   
    ![Temel listesi](./media/devtest-lab-create-formulas/base-list.png)

7. Üzerinde **formül oluşturma** dikey penceresinde, aşağıdaki değerleri belirtin:
   
    * **Formül adı** -formül için bir ad girin. Bir VM oluştururken bu değeri temel görüntü listesinde görüntülenir. Ad, yazdığınız ve geçerli değil, bir ileti geçerli bir ad gereksinimlerini gösterir doğrulanır.
    * **Açıklama** -formül için anlamlı bir açıklama girin. Bir VM oluştururken bu değeri formülün bağlam menüsünden kullanılabilir.
    * **Kullanıcı adı** -yönetici ayrıcalıkları verilmiş bir kullanıcı adı girin.
    * **Parola** - girin - veya aşağı açılır listeden seçin - belirtilen kullanıcı için kullanmak istediğiniz gizliliği (parola) ile ilişkili olan bir değer. Gizli anahtarları hakkında daha fazla bilgi için bkz: [Azure DevTest Labs: Kişisel gizli depolama](https://azure.microsoft.com/updates/azure-devtest-labs-keep-your-secrets-safe-and-easy-to-use-with-the-new-personal-secret-store/).
    * **Sanal makine disk türü** - iki HDD (sabit disk sürücüsü) belirtin veya bu temel görüntü kullanılarak sağlanan sanal makineler için hangi depolama disk türünü belirtmek için (katı hal sürücüsü) SSD izin verilir.
    * ** Sanal makine boyutu ** - işlemci çekirdeği, RAM boyutu ve sabit sürücü boyutu oluşturmak için VM belirtin önceden tanımlanmış öğeleri birini seçin. 
    * **Yapıları** - açmak için Select **yapıları eklemek** dikey penceresinde, seçin ve temel görüntü eklemek istediğiniz yapıları yapılandırın. Yapılar hakkında daha fazla bilgi için bkz: [Azure DevTest Labs sanal makineniz için özel yapılar oluşturma](devtest-lab-artifact-author.md).
    * **Gelişmiş ayarlar** - açmak için Select **Gelişmiş** aşağıdaki ayarları yapılandırdığınız dikey penceresinde:
        * **Sanal ağ** -istenen sanal ağ belirtin.
        * **Alt ağ** -istenen alt ağ belirtin.    
        * **IP adresi yapılandırması** -genel, özel veya paylaşılan IP adreslerini isteyip istemediğinizi belirtin. Paylaşılan IP adresleri hakkında daha fazla bilgi için bkz: [anlayın paylaşılan Azure DevTest Labs IP adresleri](./devtest-lab-shared-ip.md).
        * **Bu makineyi claimable hale** -bir makine "claimable" yapmadan anlamına gelir, sahipliği oluşturma sırasında atanması değil. Bunun yerine Laboratuvar kullanıcılara ("talep") Laboratuvar 's dikey penceresinde makine sahipliğini mümkün olacaktır.     
    * **Görüntü** -Bu alan, seçili temel görüntü adı önceki dikey penceresinde görüntüler. 
     
       ![Formül oluşturma](./media/devtest-lab-create-formulas/create-formula.png)

8. Seçin **oluşturma** formül oluşturmak için.

9. Formül oluşturduğunuzda listede görüntüler **formüller** dikey.

### <a name="create-a-formula-from-a-vm"></a>Bir sanal makineden bir formül oluşturma
Aşağıdaki adımları var olan bir VM temelinde bir formül oluşturma işleminde size kılavuzluk eder. 

> [!NOTE]
> Bir sanal makineden bir formül oluşturmak için VM 30 Mart 2016'dan sonra oluşturulmuş olması gerekir. 
> 
> 

1. [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
2. Seçin **tüm hizmetleri**ve ardından **DevTest Labs** listeden.
3. İstenen Laboratuvar labs listesinden seçin.  
4. Laboratuvar 's üzerinde **genel bakış** dikey penceresinde, istediğiniz formül oluşturmak VM seçin.
   
    ![Labs VM'ler](./media/devtest-lab-create-formulas/my-vms.png)
5. VM'ın dikey penceresinde, seçin **formül (yeniden kullanılabilir temel) oluşturma**.
   
    ![Formül oluşturma](./media/devtest-lab-create-formulas/create-formula-menu.png)
6. Üzerinde **formül oluşturma** dikey penceresinde girin bir **adı** ve **açıklama** için yeni formül.
   
    ![Formül dikey penceresi oluşturma](./media/devtest-lab-create-formulas/create-formula-blade.png)
7. Seçin **Tamam** formül oluşturmak için.

## <a name="modify-a-formula"></a>Bir formülü değiştirme
Bir formülü değiştirmek için aşağıdaki adımları izleyin:

1. [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
2. Seçin **tüm hizmetleri**ve ardından **DevTest Labs** listeden.
3. İstenen Laboratuvar labs listesinden seçin.  
4. Laboratuvar 's dikey penceresinde, seçin **formüller (yeniden kullanılabilir tabanları)**.
   
    ![Formül menüsü](./media/devtest-lab-manage-formulas/lab-settings-formulas.png)
5. Üzerinde **Laboratuvar formüller** dikey penceresinde değiştirmek istediğiniz formülü seçin.
6. Üzerinde **güncelleştirme formülü** dikey penceresinde, istenen düzenlemeleri yapın ve seçin **güncelleştirme**.

## <a name="delete-a-formula"></a>Formül silme
Formül silmek için aşağıdaki adımları izleyin:

1. [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
2. Seçin **tüm hizmetleri**ve ardından **DevTest Labs** listeden.
3. İstenen Laboratuvar labs listesinden seçin.  
4. Laboratuvar üzerinde **ayarları** dikey penceresinde, select **formüller**.
   
    ![Formül menüsü](./media/devtest-lab-manage-formulas/lab-settings-formulas.png)
5. Üzerinde **Laboratuvar formüller** dikey penceresinde, silmek istediğiniz formülü sağındaki üç nokta seçin.
   
    ![Formül menüsü](./media/devtest-lab-manage-formulas/lab-formulas-blade.png)
6. Formülün bağlam menüsünde seçin **silmek**.
   
    ![Formül bağlam menüsü](./media/devtest-lab-manage-formulas/formula-delete-context-menu.png)
7. Seçin **Evet** silme onayı iletişim.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>İlgili blog gönderileri
* [Özel resimler veya formüller?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)

## <a name="next-steps"></a>Sonraki adımlar
Bir VM oluştururken kullanmak için bir formül oluşturduktan sonra sıradaki adım olacak [Laboratuvarınızı için bir VM eklemek](devtest-lab-add-vm.md).

