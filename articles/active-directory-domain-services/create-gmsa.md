---
title: 'Azure Active Directory etki alanı Hizmetleri: Bir grup yönetilen hizmet hesabı oluşturma | Microsoft Docs'
description: Azure Active Directory Domain Services yönetilen etki alanlarını yönetme
services: active-directory-ds
documentationcenter: ''
author: MikeStephens-MS
manager: daveba
editor: curtand
ms.assetid: e6faeddd-ef9e-4e23-84d6-c9b3f7d16567
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/13/2019
ms.author: mstephen
ms.openlocfilehash: 0f4e57359797e9c28ea62397e5c92c5fca05c69e
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66246352"
---
# <a name="create-a-group-managed-service-account-gmsa-on-an-azure-ad-domain-services-managed-domain"></a>Bir Azure AD Domain Services yönetilen etki alanında bir grup yönetilen hizmet hesabı (gMSA) oluşturma
Bu makalede bir Azure AD Domain Services yönetilen etki alanında yönetilen hizmet hesapları oluşturma işlemini gösterir.

## <a name="managed-service-accounts"></a>Yönetilen hizmet hesapları
Tek başına yönetilen hizmet hesabı (sMSA) parolasını otomatik olarak yönetilen bir yönetilen etki alanı hesabıdır. Hizmet asıl adı (SPN) yönetimini basitleştirir ve yönetimin diğer yöneticilere etkinleştirir temsilcisi. Bu tür bir yönetilen hizmet hesabı (MSA), Windows Server 2008 R2 ve Windows 7'de sunulmuştur.

Grup yönetilen hizmet hesabı (gMSA) birçok sunucuya etki alanında aynı avantajları sunar. Çalışmak için aynı hizmet sorumlusu karşılıklı kimlik doğrulama protokolleri için kullanılacak bir sunucu grubunda barındırılan bir hizmetin tüm örneklerine gerekir. Bir gmsa'yı hizmet sorumlusu olarak kullanıldığında, Windows işletim sistemi üzerinde yönetici kalmak yerine hesabın parolası yönetir.

**Daha fazla bilgi:**
- [Grup yönetilen hizmet hesaplarına genel bakış](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)
- [Grup yönetilen hizmet hesapları ile çalışmaya başlama](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/getting-started-with-group-managed-service-accounts)


## <a name="using-service-accounts-in-azure-ad-domain-services"></a>Azure AD Etki Alanı Hizmetleri'nde hizmet hesaplarını kullanma
Azure AD Domain Services yönetilen etki alanlarını kilitli ve Microsoft tarafından yönetilir. Hizmet hesapları ile Azure AD Domain Services'ı kullanırken birkaç önemli noktalar yer vardır.

### <a name="create-service-accounts-within-custom-organizational-units-ou-on-the-managed-domain"></a>Yönetilen etki alanında özel kuruluş birimlerine (OU) içinde hizmet hesapları oluşturma
Yerleşik 'AADDC Users' veya 'AADDC Computers' kuruluş birimleri parantez içinde bir hizmet hesabı oluşturulamıyor. [Özel bir OU oluşturmanız](create-ou.md) yönetilen etki alanınızda ve ardından o özel OU içinde hizmet hesapları oluşturun.

### <a name="the-key-distribution-services-kds-root-key-is-already-pre-created"></a>Anahtar Dağıtım Hizmetleri (KDS) kök anahtarı önceden oluşturmuş
Bir Azure AD Domain Services yönetilen etki alanında Anahtar Dağıtım Hizmetleri (KDS) kök anahtarı önceden oluşturulur. KDS kök oluşturmanız gerekmez anahtar ve bu nedenle ya da yapmak için ayrıcalığınız yok. Yönetilen etki alanında ya da KDS kök anahtarını görüntüleyemezsiniz.

## <a name="sample---create-a-gmsa-using-powershell"></a>Örnek - PowerShell kullanarak gmsa'yı oluşturma
Aşağıdaki örnek, PowerShell kullanarak özel bir OU oluşturma işlemini göstermektedir. Kullanarak sonra söz konusu OU içinde bir gMSA oluşturabilirsiniz ```-Path``` OU belirtmek için parametre.

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
- [ADOrganizationalUnit yeni cmdlet](https://docs.microsoft.com/powershell/module/addsadministration/new-adorganizationalunit)
- [Yeni-ADServiceAccount cmdlet'i](https://docs.microsoft.com/powershell/module/addsadministration/New-ADServiceAccount)


## <a name="next-steps"></a>Sonraki adımlar
- [Yönetilen bir etki alanında özel bir OU oluşturma](create-ou.md)
- [Grup yönetilen hizmet hesaplarına genel bakış](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)
- [Grup yönetilen hizmet hesapları ile çalışmaya başlama](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/getting-started-with-group-managed-service-accounts)
