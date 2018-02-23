---
title: "Azure DevTest Labs'de sahipleri ve kullanıcılar ekleme | Microsoft Docs"
description: "Azure portal veya PowerShell kullanarak Azure DevTest Labs içinde sahipleri ve kullanıcılar ekleme"
services: devtest-lab,virtual-machines
documentationcenter: na
author: craigcaseyMSFT
manager: douge
editor: 
ms.assetid: 4f51d9a5-2702-45f0-a2d5-a3635b58c416
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2017
ms.author: v-craic
ms.openlocfilehash: 348952626e13b9ac73ca2ec8e101bf02e416dc9b
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="add-owners-and-users-in-azure-devtest-labs"></a>Azure DevTest Labs'de sahipleri ve kullanıcılar ekleme
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/How-to-set-security-in-your-DevTest-Lab/player]
> 
> 

Azure DevTest Labs Access'te tarafından denetlenir [Azure rol tabanlı erişim denetimi (RBAC)](../active-directory/role-based-access-control-what-is.md). RBAC kullanarak, ekibiniz içinde içinde görevlerini ayırabilirsiniz *rolleri* vermek burada yalnızca kullanıcıların işlerini gerçekleştirmek için gerekli erişim miktarı. Üç RBAC rolde olan *sahibi*, *DevTest Labs kullanıcı*, ve *katkıda bulunan*. Bu makalede, hangi eylemlerin her üç ana RBAC rolü gerçekleştirilebilir öğrenin. Buradan, laboratuvara - Portalı aracılığıyla hem bir PowerShell komut dosyası aracılığıyla kullanıcı ekleme ve abonelik düzeyinde kullanıcı ekleme öğrenin.

## <a name="actions-that-can-be-performed-in-each-role"></a>Her rol gerçekleştirilen eylemler
Bir kullanıcı atayabilirsiniz üç ana rol vardır:

* Sahip
* DevTest Labs Kullanıcısı
* Katılımcı

Aşağıdaki tabloda bu rollerin her biri bulunan kullanıcılar tarafından gerçekleştirilen eylemler gösterilmektedir:

| **Bu roldeki kullanıcılar eylemleri gerçekleştirebilirsiniz** | **DevTest Labs kullanıcı** | **Sahibi** | **Katkıda bulunan** |
| --- | --- | --- | --- |
| **Laboratuvar görevleri** | | | |
| Kullanıcılar bir laboratuvara ekleme |Hayır |Evet |Hayır |
| Maliyet ayarlarını güncelleştirme |Hayır |Evet |Evet |
| **VM temel görevleri** | | | |
| Özel resimler ekleyip |Hayır |Evet |Evet |
| Ekleme, güncelleştirme ve formülleri silme |Evet |Evet |Evet |
| Beyaz liste Azure Marketi görüntüleri |Hayır |Evet |Evet |
| **VM görevleri** | | | |
| VM oluşturma |Evet |Evet |Evet |
| Başlatma, durdurma ve sanal makineleri silin |Yalnızca kullanıcı tarafından oluşturulan sanal makineleri |Evet |Evet |
| VM ilkelerini güncelleştirme |Hayır |Evet |Evet |
| Veri diskleri için/VMs Ekle/Kaldır |Yalnızca kullanıcı tarafından oluşturulan sanal makineleri |Evet |Evet |
| **Yapı görevleri** | | | |
| Ekleme ve yapı depoları kaldırma |Hayır |Evet |Evet |
| Yapıları Uygula |Evet |Evet |Evet |

> [!NOTE]
> Bir kullanıcı bir VM oluşturduğunda, o kullanıcı için otomatik olarak atanır **sahibi** oluşturulan VM rolü.
> 
> 

## <a name="add-an-owner-or-user-at-the-lab-level"></a>Laboratuvar düzeyinde sahibi veya kullanıcı ekleyin
Azure Portalı aracılığıyla Laboratuvar düzeyinde sahipleri ve kullanıcıların eklenebilir. Bu geçerli bir dış kullanıcılarla içerir [Microsoft hesabı (MSA)](devtest-lab-faq.md#what-is-a-microsoft-account).
Aşağıdaki adımlar bir laboratuar ortamında Azure DevTest Labs sahibi veya kullanıcı ekleme işleminde size kılavuzluk eder:

1. [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
2. Seçin **tüm hizmetleri**ve ardından **DevTest Labs** listeden.
3. İstenen Laboratuvar labs listesinden seçin.
4. Laboratuvar 's dikey penceresinde, seçin **yapılandırma**. 
5. Üzerinde **yapılandırma** dikey penceresinde, select **kullanıcılar**.
6. Üzerinde **kullanıcılar** dikey penceresinde, select **+ Ekle**.
   
    ![Kullanıcı ekle](./media/devtest-lab-add-devtest-user/devtest-users-blade.png)
7. Üzerinde **bir rol seçin** dikey penceresinde istediğiniz rolü seçin. Bölüm [her rolünde gerçekleştirilen eylemler](#actions-that-can-be-performed-in-each-role) sahibi, DevTest kullanıcı ve katkıda bulunan rollerdeki kullanıcılar tarafından gerçekleştirilen çeşitli eylemleri listeler.
8. Üzerinde **kullanıcıları eklemek** dikey penceresinde, e-posta adresi veya belirtilen rolde eklemek istediğiniz kullanıcının adını girin. Kullanıcı bulunamazsa, bir hata iletisi sorunu açıklar. Kullanıcı bulunursa, o kullanıcı listelenen ve seçili. 
9. Seçin **seçin**.
10. Seçin **Tamam** kapatmak için **erişim Ekle** dikey.
11. Ne zaman döndürmek için **kullanıcılar** dikey penceresinde kullanıcı eklendi.  

## <a name="add-an-external-user-to-a-lab-using-powershell"></a>PowerShell kullanarak Laboratuvar için bir dış kullanıcı ekleme
Azure portalında kullanıcı ekleme ek olarak, bir PowerShell Betiği kullanılarak Laboratuvarınızı bir dış kullanıcı ekleyebilirsiniz. Aşağıdaki örnekte, yalnızca altında parametre değerlerini değiştirmek **değiştirmek için değerleri** açıklama.
Alabilirsiniz `subscriptionId`, `labResourceGroup`, ve `labName` Azure portalında Laboratuvar dikey penceresinden değerleri.

> [!NOTE]
> Örnek komut dosyasını belirtilen kullanıcının Active Directory'ye konuk olarak eklendi ve, yoksa başarısız olur varsayar. Laboratuvara değil Active Directory'de bir kullanıcı eklemek için kullanıcının bölümünde gösterildiği gibi bir role atamak için Azure portalını kullanın [sahibi veya kullanıcı Laboratuvar düzeyinde ekleme](#add-an-owner-or-user-at-the-lab-level).   
> 
> 

    # Add an external user in DevTest Labs user role to a lab
    # Ensure that guest users can be added to the Azure Active directory:
    # https://azure.microsoft.com/en-us/documentation/articles/active-directory-create-users/#set-guest-user-access-policies

    # Values to change
    $subscriptionId = "<Enter Azure subscription ID here>"
    $labResourceGroup = "<Enter lab's resource name here>"
    $labName = "<Enter lab name here>"
    $userDisplayName = "<Enter user's display name here>"

    # Log into your Azure account
    Login-AzureRmAccount

    # Select the Azure subscription that contains the lab. 
    # This step is optional if you have only one subscription.
    Select-AzureRmSubscription -SubscriptionId $subscriptionId

    # Retrieve the user object
    $adObject = Get-AzureRmADUser -SearchString $userDisplayName

    # Create the role assignment. 
    $labId = ('subscriptions/' + $subscriptionId + '/resourceGroups/' + $labResourceGroup + '/providers/Microsoft.DevTestLab/labs/' + $labName)
    New-AzureRmRoleAssignment -ObjectId $adObject.Id -RoleDefinitionName 'DevTest Labs User' -Scope $labId

## <a name="add-an-owner-or-user-at-the-subscription-level"></a>Abonelik düzeyinde sahibi veya kullanıcı ekleyin
Azure izinleri Azure alt kapsamda için üst kapsamdan yayılır. Dolayısıyla, labs içeren Azure aboneliği sahipleri, otomatik olarak bu labs sahipleri altındadır. Ayrıca sanal makineleri ve Laboratuvar'ın kullanıcılar ve Azure DevTest Labs hizmeti tarafından oluşturulan diğer kaynaklara sahip. 

Laboratuvar Laboratuvar 's dikey penceresinde aracılığıyla ek sahipleri ekleyebilirsiniz [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040). Ancak, eklenen sahibinin yönetim abonelik sahibinin kapsamı daha dar kapsamıdır. Örneğin, eklenen sahipleri, bazı abonelikte DevTest Labs hizmeti tarafından oluşturulan kaynaklara tam erişim sahip değil. 

Bir Azure aboneliğine sahip eklemek için aşağıdaki adımları izleyin:

1. [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
2. Seçin **tüm hizmetleri**ve ardından **abonelikleri** listeden.
3. İstediğiniz aboneliği seçin.
4. Seçin **erişim** simgesi. 
   
    ![Erişim kullanıcıları](./media/devtest-lab-add-devtest-user/access-users.png)
5. Üzerinde **kullanıcılar** dikey penceresinde, select **Ekle**.
   
    ![Kullanıcı ekle](./media/devtest-lab-add-devtest-user/devtest-users-blade.png)
6. Üzerinde **bir rol seçin** dikey penceresinde, seçin **sahibi**.
7. Üzerinde **kullanıcıları eklemek** dikey penceresinde, e-posta adresi veya bir sahibi olarak eklemek istediğiniz kullanıcının adını girin. Kullanıcı bulunamazsa, sorunu açıklayan bir hata iletisi alırsınız. Kullanıcı bulunursa, o kullanıcı altında listelenen **kullanıcı** metin kutusu.
8. Bulunduğu kullanıcı adını seçin.
9. Seçin **seçin**.
10. Seçin **Tamam** kapatmak için **erişim Ekle** dikey.
11. Ne zaman döndürmek için **kullanıcılar** dikey penceresinde kullanıcı sahibi olarak eklendi. Bu kullanıcı artık bu abonelik altında oluşturulan labs sahibi ve böylece sahibi görevlerini gerçekleştirebilir. 

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

