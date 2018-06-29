---
title: 'Azure Active Directory etki alanı Hizmetleri: Grup yönetilen hizmet hesabı oluşturma | Microsoft Docs'
description: Azure Active Directory etki alanı Hizmetleri yönetilen etki alanlarını yönetme
services: active-directory-ds
documentationcenter: ''
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: e6faeddd-ef9e-4e23-84d6-c9b3f7d16567
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/26/2018
ms.author: maheshu
ms.openlocfilehash: 4b4ce876760d024170020ae3faf618c7e9e76773
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37036350"
---
# <a name="create-a-group-managed-service-account-gmsa-on-an-azure-ad-domain-services-managed-domain"></a>Bir Azure AD etki alanı Hizmetleri yönetilen etki alanında Grup yönetilen hizmet hesabı (gMSA) oluşturma
Bu makale bir Azure AD etki alanı Hizmetleri yönetilen etki alanında yönetilen hizmet hesapları oluşturulacağını gösterir.

## <a name="managed-service-accounts"></a>Yönetilen hizmet hesapları
Bir tek başına yönetilen hizmet hesabı (sMSA) parolasını otomatik olarak yönetilen bir yönetilen etki alanı hesabıdır. Hizmet asıl adı (SPN) yönetimini basitleştirir ve temsilci ile yönetimin diğer yöneticilere etkinleştirir. Bu tür bir yönetilen hizmet hesabı (MSA), Windows Server 2008 R2 ve Windows 7'de sunulmuştur.

Grup yönetilen hizmet hesabı (gMSA), birçok sunucusu etki alanı için aynı avantaj sağlar. Bir sunucu grubunda barındırılan bir hizmetin tüm örneklerine ait çalışmak için karşılıklı kimlik doğrulama protokolleri için aynı hizmet asıl adı kullanmanız gerekir. Bir gMSA hizmet sorumlusu olarak kullanıldığında, Windows işletim sistemi üzerinde yönetici kalmak yerine hesabın parolası yönetir.

**Daha fazla bilgi:**
- [Grup yönetilen hizmet hesaplarına genel bakış](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)
- [Grup yönetilen hizmet hesapları ile çalışmaya başlama](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/getting-started-with-group-managed-service-accounts)


## <a name="using-service-accounts-in-azure-ad-domain-services"></a>Hizmet hesapları Azure AD Etki Alanı Hizmetleri'ni kullanarak
Azure AD etki alanı Hizmetleri yönetilen etki alanları kilitlenir ve Microsoft tarafından yönetilir. Hizmet hesaplarını Azure AD etki alanı Hizmetleri ile kullanırken birkaç önemli noktalar vardır.

### <a name="create-service-accounts-within-custom-organizational-units-ou-on-the-managed-domain"></a>Yönetilen etki alanına özel kuruluş birimlerine (OU) içindeki hizmet hesaplarını oluşturma
İçinde yerleşik 'AADDC kullanıcılar' veya 'AADDC bilgisayarlar' kuruluş birimi bir hizmet hesabı oluşturulamıyor. [Özel bir OU oluşturmanız](active-directory-ds-admin-guide-create-ou.md) , yönetilen etki alanınızda ve bu özel OU içinde hizmet hesapları oluşturun.

### <a name="the-key-distribution-services-kds-root-key-is-already-pre-created"></a>Anahtar Dağıtım Hizmetleri (KDS) kök anahtarı önceden önceden oluşturulmuş
Anahtar Dağıtım Hizmetleri (KDS) kök anahtarı bir Azure AD etki alanı Hizmetleri yönetilen etki alanında önceden oluşturulur. Bir KDS kök oluşturmanız gerekmez anahtarı ve bu nedenle ya da yapmak için izne sahip değilsiniz. Yönetilen etki alanında ya da KDS kök anahtarını görüntüleyemezsiniz.

## <a name="sample---create-a-gmsa-using-powershell"></a>Örnek - PowerShell kullanarak bir gMSA oluşturmak
Aşağıdaki örnek PowerShell kullanarak özel bir OU oluşturulacağını gösterir. Kullanarak daha sonra bu OU içinde bir gMSA oluşturabilirsiniz ```-Path``` OU belirtmek için parametre.

```powershell
# Create a new custom OU on the managed domain
New-ADOrganizationalUnit -Name "MyNewOU" -Path "DC=CONTOSO100,DC=COM"

# Create a service account 'WebFarmSvc' within the custom OU.
New-ADServiceAccount -Name WebFarmSvc  `
-DNSHostName ` WebFarmSvc.contoso100.com  `
-Path "OU=MYNEWOU,DC=CONTOSO100,DC=com"  `
-KerberosEncryptionType AES128, AES256  ` -ManagedPasswordIntervalInDays 30  `
-ServicePrincipalNames http/WebFarmSvc.contoso100.com/contoso100.com, `
http/WebFarmSvc.contoso100.com/contoso100,  `
http/WebFarmSvc/contoso100.com, http/WebFarmSvc/contoso100  `
-PrincipalsAllowedToRetrieveManagedPassword CONTOSO-SERVER$
```

**PowerShell cmdlet belgeleri:**
- [ADOrganizationalUnit yeni cmdlet'i](https://docs.microsoft.com/powershell/module/addsadministration/new-adorganizationalunit)
- [Yeni-ADServiceAccount cmdlet'ini](https://docs.microsoft.com/powershell/module/addsadministration/New-ADServiceAccount)


## <a name="next-steps"></a>Sonraki adımlar
- [Yönetilen bir etki alanına özel bir OU oluştur](active-directory-ds-admin-guide-create-ou.md)
- [Grup yönetilen hizmet hesaplarına genel bakış](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)
- [Grup yönetilen hizmet hesapları ile çalışmaya başlama](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/getting-started-with-group-managed-service-accounts)
