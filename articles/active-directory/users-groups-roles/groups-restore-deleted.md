---
title: Silinen bir Office 365 grubu - Azure AD geri yükleme | Microsoft Docs
description: Silinen bir grubu geri yükleme, geri yüklenebilen gruplarını görüntülemek ve Azure Active Directory'de bir grup kalıcı olarak sil
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: quickstart
ms.date: 03/18/2019
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: 55d08ddef46c4c78452fcdbc839219b624d55c04
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58666418"
---
# <a name="restore-a-deleted-office-365-group-in-azure-active-directory"></a>Azure Active Directory’de silinmiş bir Office 365 grubunu geri yükleme

Azure Active Directory’de (Azure AD) bir Office 365 grubunu sildiğinizde, silinen grup görüntülenmez ancak silindiği tarihten itibaren 30 gün boyunca tutulur. Bu davranış sayesinde, gerekirse grup ve içeriği geri yüklenebilir. Bu işlev yalnızca Azure AD’deki Office 365 gruplarına özgüdür. Güvenlik grupları ve dağıtım grupları için kullanılamaz. 30 günlük grubu geri yükleme süresi özelleştirilebilir olmadığını unutmayın.

> [!NOTE]
> `Remove-MsolGroup` kullanmayın çünkü bu komut grubu kalıcı olarak siler. Her zaman `Remove-AzureADMSGroup` bir Office 365 grubu silinemedi.

Grubu geri yüklemek için gerekli izinler aşağıdakilerden herhangi biri olabilir:

Rol | İzinler
--------- | ---------
Genel yönetici, Partner Tier2 desteği ve Intune Yöneticisi | Silinen herhangi bir Office 365 grubunu geri yükleyebilir
Kullanıcı Yöneticisi ve Partner Tier1 desteği | Silinen herhangi bir Office 365 grubu şirket yöneticisi rolüne atanan bu grupları hariç geri yükleyebilirsiniz
Kullanıcı | Oldukları herhangi silinen bir Office 365 grubunu geri yükleyebilirsiniz

## <a name="view-and-manage-the-deleted-office-365-groups-that-are-available-to-restore"></a>Görüntüleme ve geri yüklemek kullanılabilir olan silinen bir Office 365 grupları yönetme

1. Oturum [Azure AD yönetim merkezini](https://aad.portal.azure.com) kullanıcı yönetici hesabıyla.

2. Seçin **grupları**, ardından **grupları silinmiş** , geri yüklenecek silinmiş gruplarını görüntülemek için.

    ![geri yüklemek kullanılabilir grupları görüntüleyin](media/groups-lifecycle/deleted-groups3.png)

3. Üzerinde **grupları silinmiş** dikey penceresinde şunları yapabilirsiniz:

   - Silinen grubun ve içeriği seçerek geri **grubu geri yükleme**.
   - Kalıcı olarak silinen grubun seçerek kaldırmak **kalıcı olarak silmek**. Bir grubu kalıcı olarak kaldırmak için yönetici olmanız gerekir.

## <a name="view-the-deleted-office-365-groups-that-are-available-to-restore-using-powershell"></a>Powershell kullanarak geri yüklemek kullanılabilir olan silinen bir Office 365 grupları görüntüleme
İlgilendiğiniz bir veya birden çok silinmiş grubun henüz kalıcı olarak temizlenmediğini doğrulamak için, aşağıdaki cmdlet'ler kullanılarak silinmiş gruplar görüntülenebilir. Bu cmdlet’ler [Azure AD PowerShell modülünün](https://www.powershellgallery.com/packages/AzureAD/) parçasıdır. Bu modülle ilgili daha fazla bilgi [Azure Active Directory PowerShell Sürüm 2](/powershell/azure/install-adv2?view=azureadps-2.0) makalesinde bulunabilir.

1.  Kiracınızda yer alan tüm geri yüklenmeye uygun, silinmiş Office 365 gruplarını görüntülemek için bu cmdlet’i çalıştırın.
   
    ```
    Get-AzureADMSDeletedGroup
    ```

2.  Alternatif olarak, belirli bir grubun objectID değerini biliyorsanız (bunu 1. adımdaki cmdlet ile alabilirsiniz), silinmiş belirli bir grubun henüz kalıcı olarak silinmediğini doğrulamak için aşağıdaki cmdlet’i çalıştırın.

    ```
    Get-AzureADMSDeletedGroup –Id <objectId>
    ```

## <a name="how-to-restore-your-deleted-office-365-group-using-powershell"></a>PowerShell kullanarak silinen, Office 365 grubu geri yükleme
Grubun hala geri yüklemeye uygun olduğunu doğruladıktan sonra aşağıdaki adımlardan birini kullanarak silinmiş grubu geri yükleyin. Grup belgeler, SP siteleri veya diğer kalıcı nesneler içeriyorsa, grubu ve tüm içeriğini tümüyle geri yüklemek 24 saat kadar sürebilir.

1. Grubu ve içeriğini geri yüklemek için aşağıdaki cmdlet’i çalıştırın.
 
   ```
    Restore-AzureADMSDeletedDirectoryObject –Id <objectId>
    ``` 

2. Alternatif olarak, silinmiş grubu kalıcı olarak silmek için aşağıdaki cmdlet kullanılabilir.
    
    ```
    Remove-AzureADMSDeletedDirectoryObject –Id <objectId>
    ```

## <a name="how-do-you-know-this-worked"></a>İşe yaradığını nasıl anlarsınız?

Office 365 grubunu başarıyla geri yüklediğinizi doğrulamak için, grup hakkındaki bilgileri görüntüleyen `Get-AzureADGroup –ObjectId <objectId>` cmdlet’ini çalıştırın. Geri yükleme isteği tamamlandıktan sonra:

- Grup, Exchange’de Sol gezinti çubuğunda görünür
- Grubun planı Planner’da gösterilir
- Herhangi bir SharePoint sitesine ve tüm içerikleri kullanıma sunulacaktır
- Gruba, herhangi bir Exchange uç noktasından ve Office 365 gruplarını destekleyen diğer Office 365 iş yüklerinden erişilebilir

## <a name="next-steps"></a>Sonraki adımlar
Bu makalelerde Azure Active Directory gruplarıyla ilgili ek bilgi sağlanmıştır.

* [Var olan grupları görme](../fundamentals/active-directory-groups-view-azure-portal.md)
* [Bir grubun ayarlarını yönetme](../fundamentals/active-directory-groups-settings-azure-portal.md)
* [Bir grubun üyelerini yönetme](../fundamentals/active-directory-groups-members-azure-portal.md)
* [Bir grubun üyeliklerini yönetme](../fundamentals/active-directory-groups-membership-azure-portal.md)
* [Bir gruptaki kullanıcılar için dinamik kuralları yönetme](groups-dynamic-membership.md)
