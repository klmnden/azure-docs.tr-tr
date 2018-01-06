---
title: "Süre sonu için Office 365 grupları Azure Active Directory'de | Microsoft Docs"
description: "Office 365 grupları Azure Active Directory'de (Önizleme) için sona erme kurma"
services: active-directory
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2018
ms.author: curtand
ms.reviewer: kairaz.contractor
ms.custom: it-pro
ms.openlocfilehash: 6b454ed7257e8d3f91e585cee2b559c54371fb15
ms.sourcegitcommit: 1d423a8954731b0f318240f2fa0262934ff04bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/05/2018
---
# <a name="configure-expiration-for-office-365-groups-preview"></a>Süre sonu Office 365 grupları (Önizleme) için yapılandırma

Bunlar için süre sonu özellikleri ayarlayarak artık Office 365 grupları yaşam döngüsü yönetebilirsiniz. Azure Active Directory (Azure AD) süre sonu yalnızca Office 365 grupları için ayarlayabilirsiniz. Süresi dolmak üzere bir grup ayarladıktan sonra:
-   Süre sonu yaklaştığında Grup yenilemek için grubun sahiplerini bildirilir
-   Değil yenilendiğinden herhangi bir grubu silindi
-   Silinen herhangi bir Office 365 grubu 30 gün içinde grup sahiplerine veya yönetici tarafından geri yüklenebilir

> [!NOTE]
> Office 365 grupları için sona erme ayarlama eklendiği grupların tüm üyeleri için sona erme ayarları uygulanır bir Azure AD Premium lisansı gerektirir.

Azure AD PowerShell cmdlet'leri yükleyip konusunda daha fazla bilgi için bkz: [PowerShell Azure Active Directory Graph - genel Önizleme sürümü 2.0.0.137 için](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.137).

## <a name="set-group-expiration"></a>Set grup süre sonu

1. Açık [Azure AD Yönetim Merkezi](https://aad.portal.azure.com) Azure AD kiracınızda genel yönetici olan bir hesapla.

2. Açık Azure AD, select **kullanıcılar ve gruplar**.

3. Seçin **grup ayarları** ve ardından **sona erme** süre sonu ayarlarını açın.
  
  ![Sona erme dikey penceresi](./media/active-directory-groups-lifecycle-azure-portal/expiration-settings.png)

4. Üzerinde **sona erme** dikey penceresinde aşağıdakileri yapabilirsiniz:

  * Grup ömrü gün olarak ayarlayın. Hazır değerlerden ya da (31 gün veya daha uzun olmalıdır) özel bir değer birini seçebilirsiniz. 
  * Bir grup sahibi olduğunda burada yenileme ve süre sonu bildirimleri gönderilmesi gereken bir e-posta adresi belirtin. 
  * Office 365 grupları sona seçin. Süre sonu için etkinleştirebilirsiniz **tüm** Office 365 grupları, Office 365 grupları kümelerini seçebilirsiniz veya seçtiğiniz **hiçbiri** süre sonu tüm gruplar için devre dışı bırakmak için.
  * Seçerek bittiğinde ayarlarınızı kaydetmek **kaydetmek**.


Bunun gibi e-posta bildirimleri 30 gün, 15 gün ve 1 gün grubunun süre sonundan önce Office 365 grup sahiplerine gönderilir.

![Sona erme e-posta bildirimi](./media/active-directory-groups-lifecycle-azure-portal/expiration-notification.png)

Grup sahibi sonra seçebilirsiniz **yenileme grup** Office 365 yenilemek için grubu ya da seçebilirsiniz **Grup Git** üyeleri ve grubu ile ilgili diğer ayrıntıları görmek için.

Bir grup süresi dolduğunda, Grup bir gün sonra sona erme tarihini silinir. Bunun gibi bir e-posta bildirimi geçerlilik süresi ve bunların Office 365 grubunun sonraki silinmesi hakkında bildiren Office 365 grup sahiplerine gönderilir.

![Grup silme e-posta bildirimi](./media/active-directory-groups-lifecycle-azure-portal/deletion-notification.png)

Grup seçerek geri yüklenebilir **geri yükleme grup** veya [geri yükleme silinmiş bir Office 365 Grup Azure Active Directory'de] bölümünde açıklanan PowerShell cmdlet'lerini kullanarak (https://docs.microsoft.com/azure/active-directory/ Active-Directory-Groups-Restore-Azure-Portal).
    
Geri yüklemekte grubu belgeleri, SharePoint siteleri veya diğer kalıcı nesne içeriyorsa, Grup ve içeriği tam olarak geri 24 saate kadar sürebilir.

> [!NOTE]
> * Sona erme ilk ayarladığınızda, sona erme aralığından daha eski olan tüm grupları dolmasına 30 güne ayarlanır. Bir gün içinde ilk yenileme bildirim e-posta gönderilir. 
>   Örneğin, grubu A 400 gün önce oluşturulmuş ve sona erme aralığını 180 gün olarak ayarlanır. Sona erme tarihi geçerli olduğunda, gruba sahibi, yeniler sürece silinmeden önce 30 gün sahiptir.
> * Dinamik bir grup silinir ve geri, yeni bir grup olarak görülen ve kuralına göre yeniden doldurulur. Bu işlem, 24 saate kadar sürebilir.

## <a name="next-steps"></a>Sonraki adımlar
Bu makaleler Azure AD grupları hakkında ek bilgiler sağlar.

* [Var olan grupları bakın](active-directory-groups-view-azure-portal.md)
* [Bir grubu ayarlarını yönetme](active-directory-groups-settings-azure-portal.md)
* [Bir grubun üyelerini yönetmek](active-directory-groups-members-azure-portal.md)
* [Bir grubun üyeliğini yönetme](active-directory-groups-membership-azure-portal.md)
* [Bir gruptaki kullanıcılar için dinamik kurallarını yönet](active-directory-groups-dynamic-membership-azure-portal.md)
