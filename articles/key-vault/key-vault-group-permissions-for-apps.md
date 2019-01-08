---
title: Çok sayıda uygulamaya bir Azure key vault - Azure anahtar kasası erişim izni verme | Microsoft Docs
description: Çok sayıda uygulamaya bir anahtar kasasına erişim izni vermeyi öğreneceksiniz
services: key-vault
documentationcenter: ''
author: amitbapat
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 785d4e40-fb7b-485a-8cbc-d9c8c87708e6
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/07/2019
ms.author: ambapat
ms.openlocfilehash: cd680f24eafe61bc73fa6eb91df4b4dfa5f5399b
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2019
ms.locfileid: "54073441"
---
# <a name="grant-several-applications-access-to-a-key-vault"></a>Bir anahtar kasasına çeşitli uygulamaları erişimi verme

Erişim denetimi İlkesi, bir anahtar kasası çeşitli uygulamalar erişim vermek için kullanılabilir. Bir erişim denetimi ilkesi en fazla 1024 uygulamaları destekler ve aşağıdaki gibi yapılandırılır:

1. Bir Azure Active Directory güvenlik grubu oluşturun. 
2. Hizmet sorumlularını güvenlik grubunun ilişkili tüm uygulamalar ekleyin.
3. Güvenlik grubu, anahtar Kasası'na erişim.

Ön koşullar şunlardır:
* [Azure Active Directory V2 PowerShell modülünü yükleme](https://www.powershellgallery.com/packages/AzureAD).
* [Azure PowerShell'i yükleme](/powershell/azure/overview).
* Aşağıdaki komutları çalıştırmanız grupları Azure Active Directory kiracısı oluşturma/düzenleme izni gerekir. İzinleriniz yoksa, Azure Active Directory yöneticinize başvurmanız gerekebilir. Bkz: [Azure Key Vault hakkında anahtarlara, parolalara ve sertifikalara](about-keys-secrets-and-certificates.md) Key Vault hakkında ayrıntılı bilgi için erişim ilkesi izinleri.

Artık PowerShell'de aşağıdaki komutları çalıştırın:

```powershell
# Connect to Azure AD 
Connect-AzureAD 
 
# Create Azure Active Directory Security Group 
$aadGroup = New-AzureADGroup -Description "Contoso App Group" -DisplayName "ContosoAppGroup" -MailEnabled 0 -MailNickName none -SecurityEnabled 1 
 
# Find and add your applications (ServicePrincipal ObjectID) as members to this group 
$spn = Get-AzureADServicePrincipal –SearchString "ContosoApp1" 
Add-AzureADGroupMember –ObjectId $aadGroup.ObjectId -RefObjectId $spn.ObjectId 
 
# You can add several members to this group, in this fashion. 
 
# Set the Key Vault ACLs 
Set-AzureRmKeyVaultAccessPolicy –VaultName ContosoVault –ObjectId $aadGroup.ObjectId `
-PermissionsToKeys decrypt,encrypt,unwrapKey,wrapKey,verify,sign,get,list,update,create,import,delete,backup,restore,recover,purge `
–PermissionsToSecrets get,list,set,delete,backup,restore,recover,purge `
–PermissionsToCertificates get,list,delete,create,import,update,managecontacts,getissuers,listissuers,setissuers,deleteissuers,manageissuers,recover,purge,backup,restore `
-PermissionsToStorage get,list,delete,set,update,regeneratekey,getsas,listsas,deletesas,setsas,recover,backup,restore,purge 
 
# Of course you can adjust the permissions as required 
```

Farklı bir uygulama grubu için izin kümesi vermek gerekirse, bu tür uygulamalar için ayrı bir Azure Active Directory güvenlik grubu oluşturun.

## <a name="next-steps"></a>Sonraki adımlar

Kullanma hakkında daha fazla bilgi edinin [anahtar kasanızın güvenliğini sağlama](key-vault-secure-your-key-vault.md).
