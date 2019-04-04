---
title: PowerShell'de Azure Stack ile API Sürüm profillerini kullanma | Microsoft Docs
description: PowerShell'de Azure Stack ile API Sürüm profillerini kullanma hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: EBAEA4D2-098B-4B5A-A197-2CEA631A1882
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2019
ms.author: sethm
ms.reviewer: sijuman
ms.lastreviewed: 01/05/2019
ms.openlocfilehash: 6bad40b840d6bd511ad0526c47e8a43f692a5cc2
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58483586"
---
# <a name="use-api-version-profiles-for-powershell-in-azure-stack"></a>PowerShell'de Azure Stack için API Sürüm profillerini kullanma

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

API sürümü profillerini Azure ve Azure Stack arasında sürümü farkları yönetmek için bir yol sağlar. Bir API Sürüm profili, AzureRM PowerShell modülleri belirli API sürümleri ile kümesidir. Her bulut platformu desteklenen API sürümü profillerini kümesi vardır. Örneğin, Azure Stack gibi belirli tarihli profil sürümü destekleyen **2018-03-01-karma**, ve Azure'ı destekleyen **son** API Sürüm profili. Belirtilen profiliyle AzureRM PowerShell modülleri, bir profil yükleme sırasında yüklenir.

## <a name="install-the-powershell-module-required-to-use-api-version-profiles"></a>API sürümü profillerini kullanmak için gerekli PowerShell modülünü yükleme

**AzureRM.Bootstrapper** PowerShell Galerisi'nde kullanılabilir modül ile API Sürüm profillerini çalışması için gerekli olan PowerShell cmdlet'leri sağlar. Yüklemek için aşağıdaki cmdlet'i kullanın **AzureRM.Bootstrapper** Modülü:

```powershell
Install-Module -Name AzureRm.BootStrapper
```

## <a name="azure-stack-version-and-profile-versions"></a>Azure Stack sürümü ve profil sürümleri

Gerekli API profili sürümü ve Azure Stack son sürümleri için kullanılan PowerShell yönetici modülü ad aşağıdaki tabloda listelenmektedir. Bu makalede 1808 önce bir sürüm kullanıyorsanız, ad ve sürüm profili doğru bir değerle güncelleştirin.

| Sürüm No | API Sürüm profili | PS yönetim modülü bilinen ad |
| --- | --- | --- |
| 1811 veya üzeri | 2018-03-01-karma | 1.6.0 |
| 1808 veya üzeri | 2018-03-01-karma | 1.5.0 |
| 1804 veya üzeri | 2017-03-09-profile | 1.4.0 |
| 1804 öncesi sürümleri | 2017-03-09-profile | 1.2.11 |

> [!NOTE]  
> 1.2.11 yükseltme sürümünü görmek [Geçiş Kılavuzu](https://aka.ms/azpsh130migration).

## <a name="install-a-profile"></a>Profil yükle

Kullanım **yükleme-AzureRmProfile** cmdlet'iyle **2018-03-01-karma** Azure yığını tarafından gerekli AzureRM modüllerini yüklemek için API Sürüm profili. Bu API Sürüm profili ile Azure Stack operatörü modülleri yüklü değil. Bunlar ayrı olarak belirtilen adım 3'te yüklenmelidir [Azure Stack için PowerShell yükleme](../azure-stack-powershell-install.md) makalesi.

```powershell
Install-AzureRMProfile -Profile 2018-03-01-hybrid
```

## <a name="install-and-import-modules-in-a-profile"></a>Yükleme ve bir profildeki modüllerini içeri aktarma

Kullanım **kullanın-AzureRmProfile** yüklemek ve bir API Sürüm profili ile ilişkili olan modülleri içeri aktarmak için cmdlet'ini. Bir PowerShell oturumunda tek bir API Sürüm profili içeri aktarabilirsiniz. Farklı bir API Sürüm profili içeri aktarmak için yeni bir PowerShell oturumu açmanız gerekir. **Kullanın-AzureRMProfile** cmdlet'i aşağıdaki görevleri gerçekleştirir:

1. PowerShell modülleri belirtilen API Sürüm profili ile ilişkili geçerli kapsamda yüklü olup olmadığını denetler.  
2. İndirir ve zaten yüklü değilse, modülleri yükler.
3. Modüller geçerli PowerShell oturumuna aktarır.

```powershell
# Installs and imports the specified API version profile into the current PowerShell session.
Use-AzureRmProfile -Profile 2018-03-01-hybrid -Scope CurrentUser

# Installs and imports the specified API version profile into the current PowerShell session without any prompts
Use-AzureRmProfile -Profile 2018-03-01-hybrid -Scope CurrentUser -Force
```

Yükleme ve seçili AzureRM modülleri bir API sürümü profilinden içeri aktarmak için çalıştırın **kullanın-AzureRMProfile** cmdlet'iyle **Modülü** parametresi:

```powershell
# Installs and imports the compute, storage and network modules from the specified API version profile into your current PowerShell session.
Use-AzureRmProfile -Profile 2018-03-01-hybrid -Module AzureRM.Compute, AzureRM.Storage, AzureRM.Network
```

## <a name="get-the-installed-profiles"></a>Yüklü profili Al

Kullanım **Get-AzureRmProfile** kullanılabilir API sürümü profillerini listesini almak için cmdlet:

```powershell
# lists all API version profiles provided by the AzureRM.BootStrapper module.
Get-AzureRmProfile -ListAvailable

# lists the API version profiles which are installed on your machine
Get-AzureRmProfile
```

## <a name="update-profiles"></a>Profillerini güncelleştirin

Kullanım **güncelleştirme-AzureRmProfile** bir API Sürüm profili modülleri PSGallery içinde kullanılabilir olan modülleri en son sürümüne güncelleştirmek için cmdlet. Her zaman çalıştırmanız önerilir **güncelleştirme-AzureRmProfile** modüller içeri aktarılırken çakışmalarını önlemek için yeni bir PowerShell oturumunda cmdlet'i. **Güncelleştirme-AzureRmProfile** cmdlet'i aşağıdaki görevleri çalıştırır:

1. Belirtilen API sürümü profilinde geçerli kapsam için modülleri en son sürümleri yüklü olmadığını denetler.  
2. Zaten yüklü değilse yüklemenizi ister.  
3. Yükler ve güncelleştirilmiş modülleri geçerli PowerShell oturumuna aktarır.  

```powershell
Update-AzureRmProfile -Profile 2018-03-01-hybrid
```

<!-- To remove the previously installed versions of the modules before updating to the latest available version, use the Update-AzureRmProfile cmdlet along with the **-RemovePreviousVersions** parameter:

```powershell 
Update-AzureRmProfile -Profile 2018-03-01-hybrid -RemovePreviousVersions
``` -->

Bu cmdlet, aşağıdaki görevleri çalıştırır:  

1. Belirtilen API sürümü profilinde geçerli kapsam için modülleri en son sürümleri yüklü olmadığını denetler.  
2. Modüller eski sürümleri, geçerli API Sürüm profili ve geçerli PowerShell oturumunda kaldırır.  
3. en son sürümünü yüklemenizi ister.  
4. Yükler ve güncelleştirilmiş modülleri geçerli PowerShell oturumuna aktarır.  

## <a name="uninstall-profiles"></a>Profilleri kaldırma

Kullanım **Kaldır-AzureRmProfile** cmdlet'i, belirtilen API sürümü profilini kaldırmak için.

```powershell
Uninstall-AzureRmProfile -Profile  2018-03-01-hybrid
```

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Stack için PowerShell yükleme](azure-stack-powershell-install.md)
* [Azure Stack kullanıcının PowerShell ortamını yapılandırma](azure-stack-powershell-configure-user.md)  
