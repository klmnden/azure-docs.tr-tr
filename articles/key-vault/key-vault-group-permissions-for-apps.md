---
title: Azure anahtar kasası erişmek için birçok uygulamalara iznini | Microsoft Docs
description: Bir anahtar kasası erişmek için birçok uygulama izni öğrenin
services: key-vault
documentationcenter: ''
author: amitbapat
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 785d4e40-fb7b-485a-8cbc-d9c8c87708e6
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/01/2016
ms.author: ambapat
ms.openlocfilehash: ddeaf184138bd48d324799ddb45248b0a0ee8eeb
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="grant-permission-to-many-applications-to-access-a-key-vault"></a>Bir anahtar kasası erişmek için birçok uygulama izni verin

## <a name="q-i-have-several-over-16-applications-that-need-to-access-a-key-vault-since-key-vault-only-allows-16-access-control-entries-how-can-i-achieve-that"></a>S: birkaç sahip bir anahtar kasası erişmesi gerekir (üzerinde 16) uygulamaları. Key Vault yalnızca 16 erişim denetimi girişine izin verdiği için bunu nasıl gerçekleştirebilirim?

Anahtar kasası erişim denetimi ilkesini yalnızca 16 girişleri destekler. Ancak, bir Azure Active Directory güvenlik grubu oluşturabilirsiniz. Tüm ilişkili hizmet asıl adı bu güvenlik grubuna ekleyin ve bu anahtar kasası güvenlik grubuna erişim hakkı.

Ön koşullar şunlardır:
* [Azure Active Directory V2 PowerShell modülünü yüklemek](https://www.powershellgallery.com/packages/AzureAD).
* [Azure PowerShell'i yükleme](/powershell/azure/overview).
* Aşağıdaki komutları çalıştırmak için Azure Active Directory Kiracı gruplarında Oluştur/Düzenle izinlerine sahip olmanız gerekir. İzinleri yoksa, Azure Active Directory yöneticinize başvurmanız gerekebilir.

Şimdi PowerShell'de aşağıdaki komutları çalıştırın.

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
Set-AzureRmKeyVaultAccessPolicy –VaultName ContosoVault –ObjectId $aadGroup.ObjectId -PermissionsToKeys all –PermissionsToSecrets all –PermissionsToCertificates all 
 
# Of course you can adjust the permissions as required 
```

Farklı bir uygulama grubu için izinler kümesini vermeniz gerekiyorsa, bu tür uygulamalar için ayrı bir Azure Active Directory güvenlik grubu oluşturun.

## <a name="next-steps"></a>Sonraki adımlar

Nasıl yapılır hakkında daha fazla bilgi [anahtar kasanızı güvenli](key-vault-secure-your-key-vault.md).
