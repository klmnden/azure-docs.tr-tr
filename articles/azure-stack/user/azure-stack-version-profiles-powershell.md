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
ms.date: 08/15/2018
ms.author: sethm
ms.reviewer: sijuman
ms.openlocfilehash: 994893eb73356fde9acc593569dc5fb1c5a0106f
ms.sourcegitcommit: 794bfae2ae34263772d1f214a5a62ac29dcec3d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2018
ms.locfileid: "44391139"
---
# <a name="use-api-version-profiles-for-powershell-in-azure-stack"></a>PowerShell'de Azure Stack için API Sürüm profillerini kullanma

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

API sürümü profillerini Azure ve Azure Stack arasında sürümü farkları yönetmek için bir yol sağlar. Bir API Sürüm profili, AzureRM PowerShell modülleri belirli API sürümleri ile kümesidir. Her bulut platformu desteklenen API sürümü profillerini kümesi vardır. Örneğin, Azure Stack gibi belirli tarihli profil sürümü destekleyen **2017-03-09-profile**, ve Azure'ı destekleyen **son** API Sürüm profili. Belirtilen profiliyle AzureRM PowerShell modülleri, bir profil yükleme sırasında yüklenir.

 

## <a name="install-the-powershell-module-required-to-use-api-version-profiles"></a>API sürümü profillerini kullanmak için gerekli PowerShell modülünü yükleme

**AzureRM.Bootstrapper** PowerShell Galerisi'nde kullanılabilir modül ile API Sürüm profillerini çalışması için gerekli olan PowerShell cmdlet'leri sağlar. AzureRM.Bootstrapper modülünü yüklemek için aşağıdaki cmdlet'i kullanın:

```PowerShell
Install-Module -Name AzureRm.BootStrapper
```

## <a name="install-a-profile"></a>Profil yükle

Kullanım **yükleme-AzureRmProfile** cmdlet'iyle **2017-03-09-profile** Azure yığını tarafından gerekli AzureRM modüllerini yüklemek için API Sürüm profili. Bu API Sürüm profili ile Azure Stack operatörü modülleri yüklü değil. Bunlar ayrı olarak belirtilen adım 3'te yüklenmelidir [Azure Stack için PowerShell yükleme](azure-stack-powershell-install.md) makalesi.

```PowerShell 
Install-AzureRMProfile -Profile 2017-03-09-profile
```
## <a name="install-and-import-modules-in-a-profile"></a>Yükleme ve bir profildeki modüllerini içeri aktarma

Kullanım **kullanın-AzureRmProfile** yüklemek ve bir API Sürüm profili ile ilişkili olan modülleri içeri aktarmak için cmdlet'ini. Bir PowerShell oturumunda tek bir API Sürüm profili içeri aktarabilirsiniz. Farklı bir API Sürüm profili içeri aktarmak için yeni bir PowerShell oturumu açmanız gerekir. Aşağıdaki görevleri kullanın-AzureRMProfile cmdlet'ini çalıştırır:  
1. PowerShell modülleri belirtilen API Sürüm profili ile ilişkili geçerli kapsamda yüklü olup olmadığını denetler.  
2. İndirir ve zaten yüklü değilse, modülleri yükler.   
3. Modüller geçerli PowerShell oturumuna aktarır. 

```PowerShell
# Installs and imports the specified API version profile into the current PowerShell session.
Use-AzureRmProfile -Profile 2017-03-09-profile -Scope CurrentUser

# Installs and imports the specified API version profile into the current PowerShell session without any prompts
Use-AzureRmProfile -Profile 2017-03-09-profile -Scope CurrentUser -Force
```

Yükleyin ve seçili AzureRM modülleri bir API sürümü profilinden içeri aktarmak için kullanın-AzureRMProfile cmdlet'ini çalıştırmak **Modülü** parametresi:

```PowerShell
# Installs and imports the compute, Storage and Network modules from the specified API version profile into your current PowerShell session.
Use-AzureRmProfile -Profile 2017-03-09-profile -Module AzureRM.Compute, AzureRM.Storage, AzureRM.Network
```

## <a name="get-the-installed-profiles"></a>Yüklü profili Al

Kullanım **Get-AzureRmProfile** kullanılabilir API sürümü profillerini listesini almak için cmdlet: 

```PowerShell
# lists all API version profiles provided by the AzureRM.BootStrapper module.
Get-AzureRmProfile -ListAvailable 

# lists the API version profiles which are installed on your machine
Get-AzureRmProfile
```
## <a name="update-profiles"></a>Profillerini güncelleştirin

Kullanım **güncelleştirme-AzureRmProfile** bir API Sürüm profili modülleri PSGallery içinde kullanılabilir olan modülleri en son sürümüne güncelleştirmek için cmdlet. Her zaman çalıştır önerilir **güncelleştirme-AzureRmProfile** modüller içeri aktarılırken çakışmalarını önlemek için yeni bir PowerShell oturumunda cmdlet'i. Update-AzureRmProfile cmdlet'i aşağıdaki görevleri çalıştırır:

1. Belirtilen API sürümü profilinde geçerli kapsam için modülleri en son sürümleri yüklü olmadığını denetler.  
2. Zaten yüklü değilse yüklemenizi ister.  
3. Yükler ve güncelleştirilmiş modülleri geçerli PowerShell oturumuna aktarır.  

```PowerShell
Update-AzureRmProfile -Profile 2017-03-09-profile
```

Daha önce yüklenen modülleri sürümleri için en son sürümüne güncelleştirmeden önce kaldırmak için Update-AzureRmProfile cmdlet'i ile birlikte kullanın. **- RemovePreviousVersions** parametresi:

```PowerShell 
Update-AzureRmProfile -Profile 2017-03-09-profile -RemovePreviousVersions
```

Bu cmdlet, aşağıdaki görevleri çalıştırır:  

1. Belirtilen API sürümü profilinde geçerli kapsam için modülleri en son sürümleri yüklü olmadığını denetler.  
2. Modüller eski sürümleri, geçerli API Sürüm profili ve geçerli PowerShell oturumunda kaldırır.  
4. en son sürümünü yüklemenizi ister.  
5. Yükler ve güncelleştirilmiş modülleri geçerli PowerShell oturumuna aktarır.  
 
## <a name="uninstall-profiles"></a>Profilleri kaldırma

Kullanım **Kaldır-AzureRmProfile** cmdlet'i, belirtilen API sürümü profilini kaldırmak için.

```PowerShell 
Uninstall-AzureRmProfile -Profile 2017-03-09-profile
```

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Stack için PowerShell yükleme](azure-stack-powershell-install.md)
* [Azure Stack kullanıcının PowerShell ortamını yapılandırma](azure-stack-powershell-configure-user.md)  
