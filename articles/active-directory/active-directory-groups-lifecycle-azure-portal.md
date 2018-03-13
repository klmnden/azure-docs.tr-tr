---
title: "Süre sonu için Office 365 grupları Azure Active Directory'de | Microsoft Docs"
description: "Süre sonu için Office 365 grupları Azure Active Directory'de kurma"
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
ms.date: 03/09/2018
ms.author: curtand
ms.reviewer: kairaz.contractor
ms.custom: it-pro
ms.openlocfilehash: 95593eaacd73316ab527ffda8f977fbf0eb15558
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="configure-the-expiration-policy-for-office-365-groups"></a>Office 365 grupları için süre sonu ilkesi yapılandırma

Bunlar için süre sonu ilkesi ayarlayarak artık Office 365 grupları yaşam döngüsü yönetebilirsiniz. Yalnızca Office 365 grupları Azure Active Directory'de (Azure AD) için süre sonu ilkesi ayarlayabilirsiniz. 

Süresi dolmak üzere bir grup ayarladıktan sonra:
-   Süre sonu yaklaştığında Grup yenilemek için grubun sahiplerini bildirilir
-   Değil yenilendiğinden herhangi bir grubu silindi
-   Silinen herhangi bir Office 365 grubu 30 gün içinde grup sahiplerine veya yönetici tarafından geri yüklenebilir

> [!NOTE]
> Yapılandırma ve Office 365 grupları için süre sonu ilkesi kullanarak sona erme ilkenin geçerli olduğu tüm grupların tüm üyeleri için Azure AD Premium lisansı taraftan olmasını gerektirir.

Azure AD PowerShell cmdlet'leri yükleyip konusunda daha fazla bilgi için bkz: [Azure Active Directory PowerShell grafik 2.0.0.137 için](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.137).

## <a name="roles-and-permissions"></a>Rolleri ve izinleri
Aşağıdaki yapılandırabilir ve sona erme Azure AD'de Office 365 gruplarında kullanmak rolleridir.

Rol | İzinler
-------- | --------
Genel yönetici veya kullanıcı hesabının yönetici | Oluşturma, okuma, güncelleştirme veya silme Office 365 grupları süre sonu ilkesi ayarları<br>Herhangi bir Office 365 Grup yenileyebilirsiniz
Kullanıcı | Oldukları bir Office 365 Grup yenileyebilirsiniz<br>Oldukları bir Office 365 grup geri yükleyebilirsiniz<br>Süre sonu ilkesi ayarlarını okuyabilirsiniz

Silinen bir grupta geri yüklemek için izinler hakkında daha fazla bilgi için bkz: [Azure Active Directory silinen bir Office 365 grubunda geri](active-directory-groups-restore-azure-portal.md).

## <a name="set-group-expiration"></a>Set grup süre sonu

1. Açık [Azure AD Yönetim Merkezi](https://aad.portal.azure.com) Azure AD kiracınızda genel yönetici olan bir hesapla.

2. Seçin **grupları**seçeneğini belirleyip **sona erme** süre sonu ayarlarını açın.
  
  ![Sona erme dikey penceresi](./media/active-directory-groups-lifecycle-azure-portal/expiration-settings.png)

4. Üzerinde **sona erme** dikey penceresinde aşağıdakileri yapabilirsiniz:

  * Grup ömrü gün olarak ayarlayın. Hazır değerlerden ya da (31 gün veya daha uzun olmalıdır) özel bir değer birini seçebilirsiniz. 
  * Bir grup sahibi olduğunda burada yenileme ve süre sonu bildirimleri gönderilmesi gereken bir e-posta adresi belirtin. 
  * Office 365 grupları sona seçin. Süre sonu için etkinleştirebilirsiniz **tüm** Office 365 grupları seçebilirsiniz yalnızca etkinleştirmek **seçili** Office 365 grupları veya seçin **hiçbiri** süre sonu tüm gruplar için devre dışı bırakmak için .
  * Seçerek bittiğinde ayarlarınızı kaydetmek **kaydetmek**.


Bunun gibi e-posta bildirimleri 30 gün, 15 gün ve 1 gün grubunun süre sonundan önce Office 365 grup sahiplerine gönderilir.

![Sona erme e-posta bildirimi](./media/active-directory-groups-lifecycle-azure-portal/expiration-notification.png)

Gelen **yenileme grup** bildirim e-posta, Grup sahipleri için erişim panelinde erişim t hegroup Ayrıntıları sayfasında doğrudan. Burada, kullanıcıların zaman sona ereceği zaman, son, yenilendi, açıklamasını ve gruba yenileme özelliği gibi Grup hakkında daha fazla bilgi alabilirsiniz. Grup sahibi rahat içerik ve etkinlik kendi grubunda görüntüleyebilmesi için Grup Ayrıntılar sayfası artık aynı zamanda Office 365 Grup kaynaklarına bağlantılar içerir.

Bir grup süresi dolduğunda, Grup bir gün sonra sona erme tarihini silinir. Bunun gibi bir e-posta bildirimi geçerlilik süresi ve bunların Office 365 grubunun sonraki silinmesi hakkında bildiren Office 365 grup sahiplerine gönderilir.

![Grup silme e-posta bildirimi](./media/active-directory-groups-lifecycle-azure-portal/deletion-notification.png)

Grup kendi silme işlemi 30 gün içinde seçerek geri yüklenebilir **geri yükleme grup** ya da açıklandığı gibi PowerShell cmdlet'lerini kullanarak [Azure Active Directory silinen bir Office 365 grubunda geri](active-directory-groups-restore-azure-portal.md).
    
Geri yüklemekte grubu belgeleri, SharePoint siteleri veya diğer kalıcı nesne içeriyorsa, Grup ve içeriği tam olarak geri 24 saate kadar sürebilir.

> [!NOTE]
> * Sona erme ilk ayarladığınızda, sona erme aralığından daha eski olan tüm grupları dolmasına 30 güne ayarlanır. Bir gün içinde ilk yenileme bildirim e-posta gönderilir. 
>   Örneğin, grubu A 400 gün önce oluşturulmuş ve sona erme aralığını 180 gün olarak ayarlanır. Sona erme tarihi geçerli olduğunda, gruba sahibi, yeniler sürece silinmeden önce 30 gün sahiptir.
> * Şu anda yalnızca bir süre sonu ilkesi, bir kiracı Office 365 gruplarında için yapılandırılabilir.
> * Dinamik bir grup silinir ve geri, yeni bir grup olarak görülen ve kuralına göre yeniden doldurulur. Bu işlem, 24 saate kadar sürebilir.

## <a name="how-office-365-group-expiration-works-with-a-mailbox-on-legal-hold"></a>Office 365 Grup sona erme bir posta kutusuyla yasal tutmada nasıl çalışır
Bir grup süresi dolar ve silme işleminden sonra 30 gün sonra silinir grubun verilerini uygulamalardan Planlayıcısı, siteleri, ister veya takımlar kalıcı olarak silinir, ancak yasal tutmada Grup posta kutusu korunur ve kalıcı olarak silinmez. Yönetici, veri getirecek posta kutusunu geri yüklemek için Exchange cmdlet'lerini kullanabilirsiniz. 

## <a name="how-office-365-group-expiration-works-with-retention-policy"></a>Office 365 Grup sona erme bekletme ilkesi ile nasıl çalışır?
Bekletme İlkesi güvenlik ve Uyumluluk Merkezi yöntem kullanılarak yapılandırılır. Bir grup süresi dolar ve silinir, Office 365 grupları için bir bekletme ilkesi ayarladıysanız, Grup kutunuzdaki Grup görüşmeler ve Grup sitesindeki dosyaları bekletme kapsayıcısında belirli sayıda gün bekletme tanımlanan korunur ilke. Kullanıcılar grubu veya içeriği süresi dolduktan sonra göremezsiniz, ancak e-bulma üzerinden site ve posta kutusu verileri kurtarabilir.

## <a name="powershell-examples"></a>PowerShell örnekleri
Kiracınızda Office 365 grupları için sona erme ayarları yapılandırmak için PowerShell cmdlet'leri nasıl kullanabileceğiniz örnekler aşağıdadır:

1. PowerShell v2.0 Önizleme Modülü (2.0.0.137) yükleyin ve PowerShell komut isteminde oturum açın:
  ````
  Install-Module -Name AzureADPreview
  connect-azuread 
  ````
2. Yeni AzureADMSGroupLifecyclePolicy sona erme ayarları yapılandırın: Bu cmdlet, Kiracı 365 gün için tüm Office 365 grupları için yaşam süresi ayarlar. Office 365 için yenileme bildirimleri grupları sahiplerine gönderilir olmadan 'emailaddress@contoso.com'
  
  ````
  New-AzureADMSGroupLifecyclePolicy -GroupLifetimeInDays 365 -ManagedGroupTypes All -AlternateNotificationEmails emailaddress@contoso.com
  ````
3. Get-AzureADMSGroupLifecyclePolicy mevcut ilkeyi almak: Bu cmdlet, yapılandırılmış geçerli Office 365 Grup sona erme ayarları alır. Bu örnekte, görebilirsiniz:
  * İlke kimliği 
  * Kiracıdaki tüm Office 365 grupları için yaşam süresi 365 gün olarak ayarlanır
  * Office 365 için yenileme bildirimleri grupları sahiplerine gönderilir olmadan 'emailaddress@contoso.com.'
  
  ````
  Get-AzureADMSGroupLifecyclePolicy
  
  ID                                    GroupLifetimeInDays ManagedGroupTypes AlternateNotificationEmails
  --                                    ------------------- ----------------- ---------------------------
  26fcc232-d1c3-4375-b68d-15c296f1f077  365                 All               emailaddress@contoso.com
  ```` 
   
4. Varolan ilke kümesi AzureADMSGroupLifecyclePolicy güncelleştirin: Bu cmdlet, mevcut bir ilkenin güncelleştirmek için kullanılır. Aşağıdaki örnekte mevcut ilkedeki Grup ömrü 365 gün 180 gün olarak değiştirilir. 
  
  ````
  Set-AzureADMSGroupLifecyclePolicy -Id “26fcc232-d1c3-4375-b68d-15c296f1f077”   -GroupLifetimeInDays 180 -AlternateNotificationEmails "emailaddress@contoso.com"
  ````
  
5. Belirli Grup İlkesi Ekle AzureADMSLifecyclePolicyGroup ekleyin: Bu cmdlet bir grup yaşam döngüsü ilkesi ekler. Örneğin: 
  
  ````
  Add-AzureADMSLifecyclePolicyGroup -Id “26fcc232-d1c3-4375-b68d-15c296f1f077” -groupId "cffd97bd-6b91-4c4e-b553-6918a320211c"
  ````
  
6. Varolan ilke Kaldır AzureADMSGroupLifecyclePolicy kaldırın: Bu cmdlet Office 365 Grup sona erme ayarları siler, ancak ilke Kimliği gereklidir. Bu süre sonu Office 365 grupları için devre dışı bırakır. 
  
  ````
  Remove-AzureADMSGroupLifecyclePolicy -Id “26fcc232-d1c3-4375-b68d-15c296f1f077”
  ````
  
Aşağıdaki cmdlet'leri daha ayrıntılı olarak ilkesini yapılandırmak için kullanılabilir. Daha fazla bilgi için bkz: [PowerShell belgelerine](https://docs.microsoft.com/en-us/powershell/module/azuread/?view=azureadps-2.0-preview&branch=master#groups).

* Get-AzureADMSGroupLifecyclePolicy
* New-AzureADMSGroupLifecyclePolicy
* Get-AzureADMSGroupLifecyclePolicy
* Set-AzureADMSGroupLifecyclePolicy
* Remove-AzureADMSGroupLifecyclePolicy
* Add-AzureADMSLifecyclePolicyGroup
* Remove-AzureADMSLifecyclePolicyGroup
* Reset-AzureADMSLifeCycleGroup   
* Get-AzureADMSLifecyclePolicyGroup

## <a name="next-steps"></a>Sonraki adımlar
Bu makaleler Azure AD grupları hakkında ek bilgiler sağlar.

* [Var olan grupları bakın](active-directory-groups-view-azure-portal.md)
* [Bir grubu ayarlarını yönetme](active-directory-groups-settings-azure-portal.md)
* [Bir grubun üyelerini yönetmek](active-directory-groups-members-azure-portal.md)
* [Bir grubun üyeliğini yönetme](active-directory-groups-membership-azure-portal.md)
* [Bir gruptaki kullanıcılar için dinamik kurallarını yönet](active-directory-groups-dynamic-membership-azure-portal.md)
