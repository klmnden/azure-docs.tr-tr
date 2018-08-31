---
title: Çok sayıda uygulamaya bir Azure anahtar kasası erişim izni verme | Microsoft Docs
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
ms.devlang: na
ms.topic: article
ms.date: 12/01/2016
ms.author: ambapat
ms.openlocfilehash: 14e6e8bb723eb236f8fb315454b8697a3bd947ef
ms.sourcegitcommit: f94f84b870035140722e70cab29562e7990d35a3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2018
ms.locfileid: "43286436"
---
# <a name="grant-permission-to-many-applications-to-access-a-key-vault"></a>Çok sayıda uygulamaya bir anahtar kasasına erişim izni verme

## <a name="q-i-have-several-applications-that-need-to-access-a-key-vault-how-can-i-give-these-applications-up-to-1024-access-to-key-vault"></a>S: bir anahtar kasasına erişmek için gereken çeşitli uygulamalar varsa, nasıl miyim bu uygulamaları (en fazla 1024) Key Vault'a erişmesini?

Anahtar kasası erişim denetimi İlkesi, en fazla 1024 girişleri destekler. Ancak, bir Azure Active Directory güvenlik grubu oluşturabilirsiniz. Tüm ilişkili hizmet sorumlularını bu sistem güvenlik grubuna ekleyin ve bu güvenlik grubu için Key Vault erişim hakkı.

Ön koşullar şunlardır:
* [Azure Active Directory V2 PowerShell modülünü yükleme](https://www.powershellgallery.com/packages/AzureAD).
* [Azure PowerShell'i yükleme](/powershell/azure/overview).
* Aşağıdaki komutları çalıştırmanız grupları Azure Active Directory kiracısı oluşturma/düzenleme izni gerekir. İzinleriniz yoksa, Azure Active Directory yöneticinize başvurmanız gerekebilir.

Artık PowerShell'de aşağıdaki komutları çalıştırın.

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

Farklı bir uygulama grubu için izin kümesi vermek gerekirse, bu tür uygulamalar için ayrı bir Azure Active Directory güvenlik grubu oluşturun.

## <a name="next-steps"></a>Sonraki adımlar

Kullanma hakkında daha fazla bilgi edinin [anahtar kasanızın güvenliğini sağlama](key-vault-secure-your-key-vault.md).
