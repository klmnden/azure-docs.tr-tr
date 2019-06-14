---
title: Azure DevTest Labs'de sahibini ve kullanıcıları ekleme | Microsoft Docs
description: Azure portal veya PowerShell kullanarak Azure DevTest Labs sahibi ve kullanıcıları ekleme
services: devtest-lab,virtual-machines
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.assetid: 4f51d9a5-2702-45f0-a2d5-a3635b58c416
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2018
ms.author: spelluru
ms.openlocfilehash: a9426c20ae23fd3dad4cdba25590ff2eac271896
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60311429"
---
# <a name="add-owners-and-users-in-azure-devtest-labs"></a>Azure DevTest Labs'de sahibini ve kullanıcıları ekleme
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/How-to-set-security-in-your-DevTest-Lab/player]
> 
> 

Azure DevTest labs'deki erişim tarafından denetlenir [Azure rol tabanlı Access Control (RBAC)](../role-based-access-control/overview.md). RBAC kullanarak, ekibiniz içinde görevleri ayırabilirsiniz *rolleri* vermek burada yalnızca kullanıcıların işlerini yapmak için gerekli erişim miktarını. Üç bu RBAC rolleri olan *sahibi*, *DevTest Labs kullanıcısı*, ve *katkıda bulunan*. Bu makalede, her üç ana RBAC rollerini hangi eylemlerin yapılabileceği öğrenin. Burada, Laboratuvar - portalından hem bir PowerShell Betiği aracılığıyla kullanıcıları eklemek ve abonelik düzeyinde kullanıcı ekleme öğrenin.

## <a name="actions-that-can-be-performed-in-each-role"></a>Her rolde gerçekleştirilebilecek eylemleri
Bir kullanıcı atayabilir üç ana rolü vardır:

* Sahip
* DevTest Labs kullanıcısı
* Katılımcı

Aşağıdaki tabloda, bu rollerin her birini kullanıcılar tarafından gerçekleştirilen eylemleri gösterir:

| **Bu roldeki kullanıcılar eylemler gerçekleştirebilir** | **DevTest Labs kullanıcısı** | **Sahip** | **Katılımcı** |
| --- | --- | --- | --- |
| **Laboratuvar görevleri** | | | |
| Kullanıcılar bir laboratuvara ekleme |Hayır |Evet |Hayır |
| Maliyet ayarlarını güncelleştirme |Hayır |Evet |Evet |
| **VM temel görevleri** | | | |
| Ekleme ve özel görüntüleri kaldırma |Hayır |Evet |Evet |
| Ekleme, güncelleştirme ve formülleri silin |Evet |Evet |Evet |
| Beyaz liste Azure Market görüntüleri |Hayır |Evet |Evet |
| **VM görevleri** | | | |
| VM oluşturma |Evet |Evet |Evet |
| Başlatma, durdurma ve Vm'leri Sil |Yalnızca kullanıcı tarafından oluşturulan sanal makineler |Evet |Evet |
| Güncelleştirme VM ilkeleri |Hayır |Evet |Evet |
| VM'ler/deposundan veri diskleri ekleme/kaldırma |Yalnızca kullanıcı tarafından oluşturulan sanal makineler |Evet |Evet |
| **Yapı görevleri** | | | |
| Yapıt deposu ekleyip |Hayır |Evet |Evet |
| Yapıtları Uygula |Evet |Evet |Evet |

> [!NOTE]
> Bir kullanıcı bir VM oluşturur, bu kullanıcı için otomatik olarak atanır **sahibi** oluşturulan VM rolü.
> 
> 

## <a name="add-an-owner-or-user-at-the-lab-level"></a>Laboratuvar düzeyinde sahibi veya kullanıcı ekleyin
Azure portal aracılığıyla Laboratuvar düzeyinde sahibini ve kullanıcıları eklenebilir. Kullanıcı, geçerli bir dış kullanıcıyla olabilir [Microsoft hesabı (MSA)](devtest-lab-faq.md#what-is-a-microsoft-account).
Aşağıdaki adımlar bir sahibi veya kullanıcı Azure DevTest labs'deki bir laboratuvara ekleme işleminde size kılavuzluk eder:

1. [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
2. Seçin **tüm hizmetleri**ve ardından **DevTest Labs** listeden.
3. İstenen Laboratuvar labs listesinden seçin.
4. Laboratuvar dikey penceresinde seçin **yapılandırması ve ilkelerini**. 
5. Üzerinde **yapılandırması ve ilkelerini** sayfasında **erişim denetimi (IAM)** sol taraftaki menüden. 
6. Seçin **rol ataması Ekle** bir role kullanıcı eklemek için araç çubuğunda.
1. İçinde **izinleri eklemek** penceresinde aşağıdaki eylemleri gerçekleştirin: 
    1. Bir rol seçin (örneğin: DevTest Labs kullanıcısı). Bölüm [her rolde gerçekleştirilebilecek eylemleri](#actions-that-can-be-performed-in-each-role) sahibi, DevTest kullanıcı ve katılımcı rollerdeki kullanıcılar tarafından gerçekleştirilen çeşitli eylemleri listeler.
    2. Role eklenecek kullanıcıyı seçin. 
    3. **Kaydet**’i seçin. 
11. İçin döndüğünüzde **kullanıcılar** dikey penceresinde kullanıcı eklendi.  

## <a name="add-an-external-user-to-a-lab-using-powershell"></a>PowerShell kullanarak Laboratuvar için bir dış kullanıcı ekleme

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Azure portalında kullanıcılar eklemenin yanı sıra, laboratuvarınız için bir PowerShell betiğini kullanarak bir dış kullanıcı ekleyebilirsiniz. Aşağıdaki örnekte, parametre değerlerini altındaki değiştirme **değiştirmek için değerleri** açıklaması.
Alabileceğiniz `subscriptionId`, `labResourceGroup`, ve `labName` Azure portalında Laboratuvar dikey penceresinden değerleri.

> [!NOTE]
> Örnek betik, belirtilen kullanıcı Active Directory'ye konuk olarak eklendi ve bu durumda değilse oynatılamaz varsayar. Bir laboratuvar için Active Directory'de bir kullanıcı eklemek için kullanıcı bölümünde gösterildiği gibi bir role atamak için Azure portalını kullanın [sahibi veya kullanıcı Laboratuvar düzeyinde ekleme](#add-an-owner-or-user-at-the-lab-level).   
> 
> 

    # Add an external user in DevTest Labs user role to a lab
    # Ensure that guest users can be added to the Azure Active directory:
    # https://azure.microsoft.com/documentation/articles/active-directory-create-users/#set-guest-user-access-policies

    # Values to change
    $subscriptionId = "<Enter Azure subscription ID here>"
    $labResourceGroup = "<Enter lab's resource name here>"
    $labName = "<Enter lab name here>"
    $userDisplayName = "<Enter user's display name here>"

    # Log into your Azure account
    Connect-AzAccount

    # Select the Azure subscription that contains the lab. 
    # This step is optional if you have only one subscription.
    Select-AzSubscription -SubscriptionId $subscriptionId

    # Retrieve the user object
    $adObject = Get-AzADUser -SearchString $userDisplayName

    # Create the role assignment. 
    $labId = ('subscriptions/' + $subscriptionId + '/resourceGroups/' + $labResourceGroup + '/providers/Microsoft.DevTestLab/labs/' + $labName)
    New-AzRoleAssignment -ObjectId $adObject.Id -RoleDefinitionName 'DevTest Labs User' -Scope $labId

## <a name="add-an-owner-or-user-at-the-subscription-level"></a>Abonelik düzeyinde sahibi veya kullanıcı ekleme
Azure izinleri Azure alt kapsamda için üst kapsamlardan yayılır. Bu nedenle, laboratuvarlar içeren bir Azure aboneliği sahiplerine otomatik olarak bu laboratuvarlar sahipleri altındadır. Vm'leri ve Laboratuvar kullanıcıları ve Azure DevTest Labs hizmeti tarafından oluşturulan diğer kaynaklar da sahip. 

Bir laboratuvar Laboratuvar dikey penceresi aracılığıyla ek sahipleri ekleyebileceğiniz [Azure portalında](https://go.microsoft.com/fwlink/p/?LinkID=525040). Ancak eklenen sahibinin yönetim kapsamını daha fazla abonelik sahibinin kapsamı dar olabilir. Örneğin, eklenen sahipleri tam abonelikte DevTest Labs hizmeti tarafından oluşturulan kaynakların bazıları için erişiminiz yok. 

Bir Azure aboneliğine sahip eklemek için aşağıdaki adımları izleyin:

1. [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
2. Seçin **tüm hizmetleri**ve ardından **abonelikleri** listeden.
3. İstediğiniz aboneliği seçin.
4. Seçin **erişim** simgesi. 
   
    ![Erişim kullanıcıları](./media/devtest-lab-add-devtest-user/access-users.png)
5. Üzerinde **kullanıcılar** dikey penceresinde **Ekle**.
   
    ![Kullanıcı ekle](./media/devtest-lab-add-devtest-user/devtest-users-blade.png)
6. Üzerinde **bir rol seçin** dikey penceresinde seçin **sahibi**.
7. Üzerinde **kullanıcı ekleme** dikey penceresinde e-posta adresi veya sahip olarak eklemek istediğiniz kullanıcının adını girin. Kullanıcı bulunamazsa, sorunu açıklayan bir hata iletisi alırsınız. Kullanıcı bulunamazsa, bu kullanıcı altında listelenir **kullanıcı** metin kutusu.
8. Bulunan kullanıcı adı seçin.
9. Seçin **seçin**.
10. Seçin **Tamam** kapatmak için **erişim Ekle** dikey penceresi.
11. İçin döndüğünüzde **kullanıcılar** dikey penceresinde kullanıcı, sahip olarak eklendi. Bu kullanıcı artık bu abonelik altında oluşturulmuş tüm Laboratuvar sahibi olduğu ve bu nedenle sahibi görevlerini gerçekleştirebilir. 

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

