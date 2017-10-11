---
title: "Office 365 grupları sona erme Azure Active Directory'de Önizleme | Microsoft Docs"
description: "Office 365 grupları Azure Active Directory'de (Önizleme) için sona erme kurma"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: curtand
ms.reviewer: kairaz.contractor
ms.custom: it-pro
ms.openlocfilehash: 8a43df84fd050d7b4bd8d937b8c55e744cb805d3
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2017
---
# <a name="configure-office-365-groups-expiration-preview"></a>Office 365 grupları süre sonu (Önizleme) yapılandırma

Seçtiğiniz herhangi bir Office 365 grup için sona erme ayarlayarak artık Office 365 grupları yaşam döngüsü yönetebilirsiniz. Bu süre sonu ayarladıktan sonra bu grupları sahipleri hala grupları gerekiyorsa bunları gruplara yenileme istenir. Değil yenilendiğinden herhangi bir Office 365 grubu silinir. Silindi herhangi bir Office 365 grubu 30 gün içinde grup sahiplerine veya yönetici tarafından geri yüklenebilir.  


> [!NOTE]
> Süre sonu yalnızca Office 365 grupları için ayarlayabilirsiniz.
>
> O365 grupları için sona erme ayarlamak için bir Azure AD Premium lisansı atanmış gerekiyor
>   - Kiracı için sona erme ayarları yapılandıran yönetici
>   - Bu ayar için seçili grupların tüm üyeleri

## <a name="set-office-365-groups-expiration"></a>Office 365 grupları sona erme ayarlayın

1. Açık [Azure AD Yönetim Merkezi](https://aad.portal.azure.com) Azure AD kiracınızda genel yönetici olan bir hesapla.

2. Açık Azure AD, select **kullanıcılar ve gruplar**.

3. Seçin **grup ayarları** ve ardından **sona erme** süre sonu ayarlarını açın.
  
  ![Sona erme dikey penceresi](./media/active-directory-groups-lifecycle-azure-portal/expiration-settings.png)

4. Üzerinde **sona erme** dikey penceresinde aşağıdakileri yapabilirsiniz:

  * Grup ömrü gün olarak ayarlayın. Hazır değerlerden ya da (31 gün veya daha uzun olmalıdır) özel bir değer birini seçebilirsiniz. 
  * Bir grup sahibi olduğunda burada yenileme ve süre sonu bildirimleri gönderilmesi gereken bir e-posta adresi belirtin. 
  * Office 365 grupları sona seçin. Süre sonu için etkinleştirebilirsiniz **tüm** Office 365 grupları, Office 365 grupları kümelerini seçebilirsiniz veya seçtiğiniz **hiçbiri** süre sonu tüm gruplar için devre dışı bırakmak için.
  * Seçerek bittiğinde ayarlarınızı kaydetmek **kaydetmek**.

Süre sonu için PowerShell aracılığıyla Office 365 grupları yapılandırmak yönergeler için karşıdan yüklemek ve Microsoft PowerShell modülünü yüklemek, bkz: [Azure Active Directory V2 PowerShell modülü - genel Önizleme sürümü 2.0.0.137](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.137).

Bunun gibi e-posta bildirimleri 30 gün, 15 gün ve 1 gün grubunun süre sonundan önce Office 365 grup sahiplerine gönderilir.

![Sona erme e-posta bildirimi](./media/active-directory-groups-lifecycle-azure-portal/expiration-notification.png)

Grup sahibi sonra seçebilirsiniz **yenileme grup** Office 365 yenilemek için grubu ya da seçebilirsiniz **Grup Git** üyeleri ve grubu ile ilgili diğer ayrıntıları görmek için.

Bir grup süresi dolduğunda, Grup bir gün sonra sona erme tarihini silinir. Bunun gibi bir e-posta bildirimi geçerlilik süresi ve bunların Office 365 grubunun sonraki silinmesi hakkında bildiren Office 365 grup sahiplerine gönderilir.

![Grup silme e-posta bildirimi](./media/active-directory-groups-lifecycle-azure-portal/deletion-notification.png)

Grup seçerek geri yüklenebilir **geri yükleme grup** veya [geri yükleme silinmiş bir Office 365 Grup Azure Active Directory'de] bölümünde açıklanan PowerShell cmdlet'lerini kullanarak (https://docs.microsoft.com/azure/active-directory/ Active-Directory-Groups-Restore-Azure-Portal).
    
Geri yüklemekte grubu belgeleri, SharePoint siteleri veya diğer kalıcı nesne içeriyorsa, Grup ve içeriği tam olarak geri 24 saate kadar sürebilir.

> [!NOTE]
> * Sona erme ayarları dağıtırken, sona erme penceresinden daha eski olan bazı grupları olabilir. Bu gruplar olması hemen silinmez, ancak 30 güne kadar sona erme ayarlayın. Bir gün içinde ilk yenileme bildirim e-posta gönderilir. Örneğin, grubu A 400 gün önce oluşturulmuş ve sona erme aralığını 180 gün olarak ayarlanır. Sona erme tarihi geçerli olduğunda, gruba sahibi, yeniler sürece silinmeden önce 30 gün sahiptir.
> * Dinamik bir grup silinir ve geri, yeni bir grup olarak görülen ve kuralına göre yeniden doldurulur. Bu işlem, 24 saate kadar sürebilir.

## <a name="next-steps"></a>Sonraki adımlar
Bu makaleler Azure AD grupları hakkında ek bilgiler sağlar.

* [Var olan grupları bakın](active-directory-groups-view-azure-portal.md)
* [Bir grubu ayarlarını yönetme](active-directory-groups-settings-azure-portal.md)
* [Bir grubun üyelerini yönetmek](active-directory-groups-members-azure-portal.md)
* [Bir grubun üyeliğini yönetme](active-directory-groups-membership-azure-portal.md)
* [Bir gruptaki kullanıcılar için dinamik kurallarını yönet](active-directory-groups-dynamic-membership-azure-portal.md)
