---
title: Azure AD’de silinmiş bir Office 365 grubunu geri yükleme | Microsoft Docs
description: Azure Active Directory’de silinmiş bir grubu geri yükleme, geri yüklenebilir grupları görüntüleme ve bir grubu kalıcı olarak silme
services: active-directory
documentationcenter: ''
author: eross-msft
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: quickstart
ms.date: 08/28/2017
ms.author: lizross
ms.reviewer: krbain
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 01d521ac384ef0264e62e3af2e106099f369231c
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56206166"
---
# <a name="restore-a-deleted-office-365-group-in-azure-active-directory"></a>Azure Active Directory’de silinmiş bir Office 365 grubunu geri yükleme

Azure Active Directory’de (Azure AD) bir Office 365 grubunu sildiğinizde, silinen grup görüntülenmez ancak silindiği tarihten itibaren 30 gün boyunca tutulur. Bu davranış sayesinde, gerekirse grup ve içeriği geri yüklenebilir. Bu işlev yalnızca Azure AD’deki Office 365 gruplarına özgüdür. Güvenlik grupları ve dağıtım grupları için kullanılamaz.

> [!NOTE]
> `Remove-MsolGroup` kullanmayın çünkü bu komut grubu kalıcı olarak siler. O365 gruplarını silmek için her zaman `Remove-AzureADMSGroup` kullanın.

Grubu geri yüklemek için gerekli izinler aşağıdakilerden herhangi biri olabilir:

Rol | İzinler
--------- | ---------
Şirket Yöneticisi, Partner Tier2 Desteği ve InTune Hizmet Yöneticileri | Silinen herhangi bir Office 365 grubunu geri yükleyebilir
Kullanıcı Hesabı Yöneticisi ve Partner Tier1 desteği | Şirket Yöneticisi rolüne atanmış Office 365 grupları dışında silinmiş tüm Office 365 gruplarını geri yükleyebilir
Kullanıcı | Sahip olduğu tüm silinmiş Office 365 gruplarını geri yükleyebilir


## <a name="view-the-deleted-office-365-groups-that-are-available-to-restore"></a>Geri yüklenebilir tüm silinmiş Office 365 gruplarını görüntüleme

İlgilendiğiniz bir veya birden çok silinmiş grubun henüz kalıcı olarak temizlenmediğini doğrulamak için, aşağıdaki cmdlet'ler kullanılarak silinmiş gruplar görüntülenebilir. Bu cmdlet’ler [Azure AD PowerShell modülünün](https://www.powershellgallery.com/packages/AzureAD/) parçasıdır. Bu modülle ilgili daha fazla bilgi [Azure Active Directory PowerShell Sürüm 2](/powershell/azure/install-adv2?view=azureadps-2.0) makalesinde bulunabilir.

1.  Kiracınızda yer alan tüm geri yüklenmeye uygun, silinmiş Office 365 gruplarını görüntülemek için bu cmdlet’i çalıştırın.
   
  ```
  Get-AzureADMSDeletedGroup
  ```

2.  Alternatif olarak, belirli bir grubun objectID değerini biliyorsanız (bunu 1. adımdaki cmdlet ile alabilirsiniz), silinmiş belirli bir grubun henüz kalıcı olarak silinmediğini doğrulamak için aşağıdaki cmdlet’i çalıştırın.
  
  ```
  Get-AzureADMSDeletedGroup –Id <objectId>
  ```

## <a name="how-to-restore-your-deleted-office-365-group"></a>Silinmiş Office 365 grubunuzu geri yükleme
Grubun hala geri yüklemeye uygun olduğunu doğruladıktan sonra aşağıdaki adımlardan birini kullanarak silinmiş grubu geri yükleyin. Grup belgeler, SP siteleri veya diğer kalıcı nesneler içeriyorsa, grubu ve tüm içeriğini tümüyle geri yüklemek 24 saat kadar sürebilir.

1.  Grubu ve içeriğini geri yüklemek için aşağıdaki cmdlet’i çalıştırın.
 
 ```
 Restore-AzureADMSDeletedDirectoryObject –Id <objectId>
 ``` 

Alternatif olarak, silinmiş grubu kalıcı olarak silmek için aşağıdaki cmdlet kullanılabilir.
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
