---
title: Azure Active Directory silinen bir Office 365 grubunda geri | Microsoft Docs
description: "Silinen bir grupta, görünüm geri yüklenebilen grupları ve permamnently delete Azure Active Directory'deki bir gruba geri yükleme"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: cc5f232a-1e77-45c2-b28b-1fcb4621725c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: curtand
ms.reviewer: kairaz.contractor
ms.custom: it-pro
ms.openlocfilehash: 5d06cee492e3360bcaf8c7663c97d0c8ed3e243f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="restore-a-deleted-office-365-group-in-azure-active-directory"></a>Azure Active Directory silinen bir Office 365 grubunda geri yükleme

Azure Active Directory'de (Azure AD) bir Office 365 grubu sildiğinizde, silinen grubun 30 gün boyunca silme tarihinden tutulan ancak görünür. Bu, böylelikle grup ve içeriği gerekirse geri yüklenebilir. Bu işlevsellik yalnızca Office 365 grupları için Azure AD'de sınırlıdır. Güvenlik grupları ve dağıtım grupları için kullanılabilir değil.

> [!NOTE] 
> Kullanmayan `Remove-MsolGroup` olduğundan grup kalıcı olarak temizler. Her zaman kullanmak `Remove-AzureADMSGroup` bir O365 grubunu silmek için. 

Bir gruba geri yüklemek için gerekli izinleri aşağıdakilerden biri olabilir:

Rol  | İzinler 
--------- | ---------
Şirket Yöneticisi, iş ortağı Tier2 destek ve Intune hizmet yöneticileri | Silinen tüm Office 365 grup geri yükleyebilirsiniz 
Kullanıcı hesabı yönetici ve iş ortağı Tier1 desteği | Silinen herhangi bir Office 365 grup şirket yöneticisi rolüne atanan hariç geri yükleyebilirsiniz 
Kullanıcı | Bunlar ait silinen tüm Office 365 grup geri yükleyebilirsiniz 


## <a name="view-the-deleted-office-365-groups-that-are-available-to-restore"></a>Geri yüklemek kullanılabilir silinen Office 365 grupları görüntüleyin
Aşağıdaki cmdlet, bir ya da ilgilendiğiniz olanları değil henüz kalıcı olarak temizlendiğinden emin doğrulamak için silinen gruplarını görüntülemek için kullanılabilir. Bu cmdlet'ler parçası olan [Azure AD PowerShell Modülü](https://www.powershellgallery.com/packages/AzureAD/). Bu modül hakkında daha fazla bilgi bulunabilir [Azure Active Directory PowerShell sürüm 2](/powershell/azure/install-adv2?view=azureadps-2.0) makalesi.

1.  Kiracınızda geri yüklemek hala kullanılabilir tüm silinen Office 365 gruplarını görüntülemek için aşağıdaki cmdlet'i çalıştırın.
  ```
  Get-AzureADMSDeletedGroup
  ```

2.  Alternatif olarak, belirli bir grup objectID bildiğiniz (ve 1. adımda cmdlet'inden alma varsa), belirli silinen grubun değil henüz kalıcı olarak temizlendi olduğunu doğrulamak için aşağıdaki cmdlet'i çalıştırın.
  ```
  Get-AzureADMSDeletedGroup –Id <objectId>
  ```



## <a name="how-to-restore-your-deleted-office-365-group"></a>Silinen Office 365 grubunuzun geri yükleme
Grup geri yüklemek kullanılabilir olduğunu doğruladıktan sonra aşağıdaki adımlardan birini silinen grubuyla geri yükleyin. Belgeler, SP siteleri veya diğer kalıcı nesne grubu içeriyorsa, tam olarak bir grup ve içeriği geri 24 saate kadar sürebilir.

1.  Grup ve içeriğini geri yüklemek için aşağıdaki cmdlet'i çalıştırın.
  
  ```
  Restore-AzureADMSDeletedDirectoryObject –Id <objectId>
  ``` 

Alternatif olarak, silinen grubun kalıcı olarak kaldırmak için aşağıdaki cmdlet'i çalıştırılabilir.
  ```
  Remove-AzureADMSDeletedDirectoryObject –Id <objectId>
  ```

## <a name="how-do-you-know-this-worked"></a>Bu çalışılan nasıl bilebilirsiniz?
Bir Office 365 Grup başarıyla geri döndürdük doğrulamak için çalıştırın `Get-AzureADGroup –ObjectId <objectId>` grubu ile ilgili bilgileri görüntülemek için cmdlet. Geri yüklemeden sonra isteği tamamlandı:
- Grup Exchange sol gezinti çubuğunda görünür
- Grup planlama Planlayıcı görünür
- Tüm Sharepoint siteleri ve tüm içerikleri kullanılabilir olacak
- Tüm Exchange uç noktaları ve Office 365 grupları destekleyen diğer Office 365 iş yükü grubu erişilebilir


## <a name="next-steps"></a>Sonraki adımlar
Bu makaleler Azure Active Directory gruplarına ek bilgiler sağlar.

* [Var olan grupları bakın](active-directory-groups-view-azure-portal.md)
* [Bir grubu ayarlarını yönetme](active-directory-groups-settings-azure-portal.md)
* [Bir grubun üyelerini yönetmek](active-directory-groups-members-azure-portal.md)
* [Bir grubun üyeliğini yönetme](active-directory-groups-membership-azure-portal.md)
* [Bir gruptaki kullanıcılar için dinamik kurallarını yönet](active-directory-groups-dynamic-membership-azure-portal.md)
