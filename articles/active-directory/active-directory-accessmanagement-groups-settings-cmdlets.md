---
title: PowerShell kullanarak Azure Active Directory'de Grup ayarlarını yapılandırma | Microsoft Docs
description: Azure Active Directory cmdlet'lerini kullanarak gruplar için ayarları yönetmek hakkında
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 02/20/2018
ms.author: curtand
ms.reviewer: kairaz.contractor
ms.custom: it-pro;
ms.openlocfilehash: 636de232e38a7d940a5f20a1c9d37971942fae34
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="azure-active-directory-cmdlets-for-configuring-group-settings"></a>Grup ayarlarını yapılandırmak için Azure Active Directory cmdlet'leri
Bu makale Azure Active Directory (Azure AD) PowerShell cmdlet'lerini kullanarak oluşturma ve güncelleştirme gruplarına yönelik yönergeleri içerir. Bu içerik yalnızca (Birleşik grupları denir) Office 365 grupları için geçerlidir. 

> [!IMPORTANT]
> Bazı ayarları bir Azure Active Directory Premium P1 lisansı gerektirir. Daha fazla bilgi için bkz: [şablonu ayarlarını](#template-settings) tablo.

Yönetici olmayan kullanıcıların oluşturmasını engellemek hakkında daha fazla bilgi için *güvenlik* gruplar `Set-MsolCompanySettings -UsersPermissionToCreateGroupsEnabled $False` açıklandığı gibi [Set-MSOLCompanySettings](https://docs.microsoft.com/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0). 

Office 365 grupları ayarları, bir ayar nesnesi ve SettingsTemplate nesnesi kullanılarak yapılandırılır. Başlangıçta, dizininize varsayılan ayarlarla yapılandırıldığından dizininizde, tüm ayarları nesnelerini görmüyorum. Varsayılan ayarları değiştirmek için ayarları şablon kullanarak yeni bir ayarları nesnesi oluşturmanız gerekir. Ayarları şablonları Microsoft tarafından tanımlanır. Birkaç farklı ayarlar şablonu vardır. Dizininiz için Office 365 Grup ayarlarını yapılandırmak için "Group.Unified" adlı şablonu kullanın. Tek bir grup Office 365 Grup ayarlarını yapılandırmak için "Group.Unified.Guest" adlı şablonunu kullanın. Bu şablon, bir Office 365 Grup Konuk erişimi yönetmek için kullanılır. 

Cmdlet'leri Azure Active Directory PowerShell V2 modülünde bir parçasıdır. Yönergeler için karşıdan yükle ve modülü bilgisayarınıza yüklemek, makaleyi bkz [Azure Active Directory PowerShell sürüm 2](https://docs.microsoft.com/powershell/azuread/). Modülünden sürüm 2 sürümü yükleyebilirsiniz [PowerShell Galerisi](https://www.powershellgallery.com/packages/AzureAD/).

## <a name="retrieve-a-specific-settings-value"></a>Belirli ayarları değeri Al
Almak istediğiniz ayar adını biliyorsanız, kullanabileceğiniz aşağıdaki geçerli ayarları değerini almak için cmdlet'i. Bu örnekte, biz "UsageGuidelinesUrl." adlı bir ayarının değeri alınırken Dizin ayarlarını ve adları hakkında daha fazla aşağı bu makalede daha fazla bilgi edinebilirsiniz.

```powershell
(Get-AzureADDirectorySetting).Values | Where-Object -Property Name -Value UsageGuidelinesUrl -EQ
```

## <a name="create-settings-at-the-directory-level"></a>Dizin düzeyinde ayarları oluştur
Bu adımları ayarları dizin düzeyinde dizindeki tüm Office 365 grupları için uygulanacak oluşturun.

1. DirectorySettings Cmdlet'lerde, kullanmak istediğiniz SettingsTemplate kimliği belirtmelisiniz. Bu kimliği bilmiyorsanız bu cmdlet'i tüm ayarları şablonları listesini döndürür:
  
  ```
  PS C:> Get-AzureADDirectorySettingTemplate
  ```
  Bu cmdlet çağrısı kullanılabilir tüm şablonları döndürür:
  
  ```
  Id                                   DisplayName         Description
  --                                   -----------         -----------
  62375ab9-6b52-47ed-826b-58e47e0e304b Group.Unified       ...
  08d542b9-071f-4e16-94b0-74abb372e3d9 Group.Unified.Guest Settings for a specific Office 365 group
  16933506-8a8d-4f0d-ad58-e1db05a5b929 Company.BuiltIn     Setting templates define the different settings that can be used for the associ...
  4bc7f740-180e-4586-adb6-38b2e9024e6b Application...
  898f1161-d651-43d1-805c-3b0b388a9fc2 Custom Policy       Settings ...
  5cf42378-d67d-4f36-ba46-e8b86229381d Password Rule       Settings ...
  ```
2. Kullanım Kılavuzu URL eklemek için ilk kullanım kılavuzu URL değeri tanımlayan SettingsTemplate nesnesi almanız gereken; diğer bir deyişle, Group.Unified şablonu:
  
  ```
  $Template = Get-AzureADDirectorySettingTemplate -Id 62375ab9-6b52-47ed-826b-58e47e0e304b
  ```
3. Ardından, bu şablona dayalı yeni bir ayar nesnesi oluşturun:
  
  ```
  $Setting = $template.CreateDirectorySetting()
  ```  
4. Ardından Kullanım Kılavuzu değeri güncelleştirin:
  
  ```
  $setting["UsageGuidelinesUrl"] = "<https://guideline.com>"
  ```  
5. Son olarak, ayarlarını uygulayın:
  
  ```
  New-AzureADDirectorySetting -DirectorySetting $setting
  ```

Başarıyla tamamlandığında, yeni ayarları nesnesinin kimliği cmdlet döndürür:
  ```
  Id                                   DisplayName TemplateId                           Values
  --                                   ----------- ----------                           ------
  c391b57d-5783-4c53-9236-cefb5c6ef323             62375ab9-6b52-47ed-826b-58e47e0e304b {class SettingValue {...
  ```
## <a name="template-settings"></a>Şablon ayarları
Group.Unified SettingsTemplate tanımlanan ayarlar aşağıda verilmiştir. Aksi belirtilmediği sürece, bu özellikler bir Azure Active Directory Premium P1 lisansı gerektirir. 

| **Ayar** | **Açıklama** |
| --- | --- |
|  <ul><li>EnableGroupCreation<li>Tür: Boolean<li>Varsayılan: True |Office 365 grup oluşturma dizinde yönetici olmayan kullanıcılar tarafından izin verilip verilmediğini belirten bayrak. Bu ayar, bir Azure Active Directory Premium P1 lisansı gerektirmez.|
|  <ul><li>GroupCreationAllowedGroupId<li>Türü: Dize<li>Varsayılan: "" |Kendisi için üyeleri verilir Office 365 grupları oluşturmak için güvenlik grubunun GUID bile EnableGroupCreation == false. |
|  <ul><li>UsageGuidelinesUrl<li>Türü: Dize<li>Varsayılan: "" |Grup kullanım yönergeleri için bir bağlantı. |
|  <ul><li>ClassificationDescriptions<li>Türü: Dize<li>Varsayılan: "" | Sınıflandırma açıklamaları virgülle ayrılmış listesi. |
|  <ul><li>DefaultClassification<li>Türü: Dize<li>Varsayılan: "" | Hiçbiri belirtilmemişse varsayılan sınıflandırması bir grup için kullanılacak sınıflandırması.|
|  <ul><li>PrefixSuffixNamingRequirement<li>Türü: Dize<li>Varsayılan: "" | Office 365 grupları için yapılandırılmış adlandırma kuralı tanımlayan en fazla 64 karakter dizisi. Daha fazla bilgi için bkz: [Office 365 grupları (Önizleme) için bir adlandırma politikasını](groups-naming-policy.md). |
| <ul><li>CustomBlockedWordsList<li>Türü: Dize<li>Varsayılan: "" | Kullanıcılar grup adlarını veya diğer adlar kullanmak için izin verilmez tümcecikleri virgülle ayrılmış dizesi. Daha fazla bilgi için bkz: [Office 365 grupları (Önizleme) için bir adlandırma politikasını](groups-naming-policy.md). |
| <ul><li>EnableMSStandardBlockedWords<li>Tür: Boolean<li>Varsayılan: "False" | Kullanmayın
|  <ul><li>AllowGuestsToBeGroupOwner<li>Tür: Boolean<li>Varsayılan: False | Konuk kullanıcı gruplarının sahibi olması olup olmadığını gösteren bir Boole değeri. |
|  <ul><li>AllowGuestsToAccessGroups<li>Tür: Boolean<li>Varsayılan: True | Konuk kullanıcı Office 365 grupları içeriğe erişmeye sahip olup olmadığını gösteren bir Boole değeri.  Bu ayar, bir Azure Active Directory Premium P1 lisansı gerektirmez.|
|  <ul><li>GuestUsageGuidelinesUrl<li>Türü: Dize<li>Varsayılan: "" | Konuk kullanım yönergeleri bağlantı URL'si. |
|  <ul><li>AllowToAddGuests<li>Tür: Boolean<li>Varsayılan: True | Boole konuklar bu dizine ekleme izni olup olmadığını belirten bir.|
|  <ul><li>ClassificationList<li>Türü: Dize<li>Varsayılan: "" |Office 365 grupları uygulanabilir geçerli sınıflandırma değerleri virgülle ayrılmış listesi. |

## <a name="read-settings-at-the-directory-level"></a>Dizin düzeyinde ayarlarını okuma
Bu adımları dizindeki tüm Office grupları için geçerli olan ayarları dizin düzeyinde okuyun.

1. Tüm mevcut directory ayarları okuyun:
  ```
  Get-AzureADDirectorySetting -All $True
  ```
  Bu cmdlet, tüm dizin ayarları listesini döndürür:
  ```
  Id                                   DisplayName   TemplateId                           Values
  --                                   -----------   ----------                           ------
  c391b57d-5783-4c53-9236-cefb5c6ef323 Group.Unified 62375ab9-6b52-47ed-826b-58e47e0e304b {class SettingValue {...
  ```

2. Belirli bir grup için tüm ayarların okuyun:
  ```
  Get-AzureADObjectSetting -TargetObjectId ab6a3887-776a-4db7-9da4-ea2b0d63c504 -TargetType Groups
  ```

3. Tüm dizin ayarlarının değerleri ayarları kimliği GUID kullanarak bir özel dizin ayarları nesnesinin okuyun:
  ```
  (Get-AzureADDirectorySetting -Id c391b57d-5783-4c53-9236-cefb5c6ef323).values
  ```
  Bu cmdlet, belirli bu grup için bu ayarları nesnesinde adları ve değerleri döndürür:
  ```
  Name                          Value
  ----                          -----
  ClassificationDescriptions
  DefaultClassification
  PrefixSuffixNamingRequirement
  CustomBlockedWordsList        
  AllowGuestsToBeGroupOwner     False 
  AllowGuestsToAccessGroups     True
  GuestUsageGuidelinesUrl
  GroupCreationAllowedGroupId
  AllowToAddGuests              True
  UsageGuidelinesUrl            <https://guideline.com>
  ClassificationList
  EnableGroupCreation           True
  ```

## <a name="update-settings-for-a-specific-group"></a>Belirli bir grup için güncelleştirme ayarları

1. "Groups.Unified.Guest" adlı ayarları şablonu için arama
  ```
  Get-AzureADDirectorySettingTemplate
  
  Id                                   DisplayName            Description
  --                                   -----------            -----------
  62375ab9-6b52-47ed-826b-58e47e0e304b Group.Unified          ...
  08d542b9-071f-4e16-94b0-74abb372e3d9 Group.Unified.Guest    Settings for a specific Office 365 group
  4bc7f740-180e-4586-adb6-38b2e9024e6b Application            ...
  898f1161-d651-43d1-805c-3b0b388a9fc2 Custom Policy Settings ...
  5cf42378-d67d-4f36-ba46-e8b86229381d Password Rule Settings ...
  ```
2. Şablon nesnesinin Groups.Unified.Guest şablonunun Al:
  ```
  $Template = Get-AzureADDirectorySettingTemplate -Id 08d542b9-071f-4e16-94b0-74abb372e3d9
  ```
3. Yeni bir ayar nesnesi şablonu oluşturun:
  ```
  $Setting = $Template.CreateDirectorySetting()
  ```

4. Gerekli değere ayarı:
  ```
  $Setting["AllowToAddGuests"]=$False
  ```
5. Gerekli grup için yeni ayar dizini oluşturun:
  ```
  New-AzureADObjectSetting -TargetType Groups -TargetObjectId ab6a3887-776a-4db7-9da4-ea2b0d63c504 -DirectorySetting $Setting
  
  Id                                   DisplayName TemplateId                           Values
  --                                   ----------- ----------                           ------
  25651479-a26e-4181-afce-ce24111b2cb5             08d542b9-071f-4e16-94b0-74abb372e3d9 {class SettingValue {...
  ```

## <a name="update-settings-at-the-directory-level"></a>Dizin düzeyinde ayarlarını güncelleştirme

Bu adımları dizindeki tüm Office 365 grupları için geçerli dizin düzeyinde ayarlarını güncelleştirin. Bu örnekler, dizininizde bir ayar nesnesi bulunmakta varsayar.

1. Var olan ayarları nesnesini Bul:
  ```
  Get-AzureADDirectorySetting | Where-object -Property Displayname -Value "Group.Unified" -EQ
  
  Id                                   DisplayName   TemplateId                           Values
  --                                   -----------   ----------                           ------
  c391b57d-5783-4c53-9236-cefb5c6ef323 Group.Unified 62375ab9-6b52-47ed-826b-58e47e0e304b {class SettingValue {...
  
  $setting = Get-AzureADDirectorySetting –Id c391b57d-5783-4c53-9236-cefb5c6ef323
  ```
2. Değeri güncelleştirin:
  
  ```
  $Setting["AllowToAddGuests"] = "false"
  ```
3. Ayar güncelleştirin:
  
  ```
  Set-AzureADDirectorySetting -Id c391b57d-5783-4c53-9236-cefb5c6ef323 -DirectorySetting $Setting
  ```

## <a name="remove-settings-at-the-directory-level"></a>Dizin düzeyinde ayarlarını Kaldır
Bu adım dizindeki tüm Office grupları için geçerli olan ayarları dizin düzeyinde kaldırır.
  ```
  Remove-AzureADDirectorySetting –Id c391b57d-5783-4c53-9236-cefb5c6ef323c
  ```

## <a name="cmdlet-syntax-reference"></a>Cmdlet'in söz dizimi başvurusu
Daha fazla Azure Active Directory PowerShell belgeleri bulabilirsiniz [Azure Active Directory cmdlet'leri](/powershell/azure/install-adv2?view=azureadps-2.0).

## <a name="additional-reading"></a>Ek kaynaklar

* [Azure Active Directory grupları ile kaynaklara erişimi yönetme](active-directory-manage-groups.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md)
