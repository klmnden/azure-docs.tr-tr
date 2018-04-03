---
title: Azure yığınında API sürümü profillerini kullanma | Microsoft Docs
description: Azure yığınında API sürümü profilleri hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: EBAEA4D2-098B-4B5A-A197-2CEA631A1882
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: mabrigg
ms.openlocfilehash: 68f4250c2a2a6bed1a1e21dc444e93cc87b6f59b
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="manage-api-version-profiles-in-azure-stack"></a>Azure yığınında API sürümü profillerini yönet

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

API sürümü profilleri sürüm farklarını Azure ve Azure yığın yönetmek için bir yöntem sunar. Bir API sürümü profili belirli API sürümleri ile AzureRM PowerShell modülleri kümesidir. Her bulut platformu desteklenen API sürümü profilleri kümesi vardır. Örneğin, Azure yığın belirli tarihli profil sürümü gibi destekleyen **2017-03-09-profili**, ve Azure destekler **son** API sürümü profili. Bir profili yüklediğinizde, belirtilen profiliyle AzureRM PowerShell modülleri yüklenir.

## <a name="install-the-powershell-module-required-to-use-api-version-profiles"></a>API sürümü profillerini kullanmak için gerekli PowerShell modülünü yükleyin

**AzureRM.Bootstrapper** PowerShell Galerisi aracılığıyla kullanılabilir modül API sürümü profilleri ile çalışmak için gerekli PowerShell cmdlet'leri sağlar. AzureRM.Bootstrapper modülünü yüklemek için aşağıdaki cmdlet'i kullanın:

```PowerShell
Install-Module -Name AzureRm.BootStrapper
```
AzureRM.Bootstrapper modülü önizlemede değil; Ayrıntılar ve işlevsellik değiştirilebilir. PowerShell Galerisi'nden bu modül en son sürümünü yükleyip karşıdan yüklemek için aşağıdaki cmdlet'i çalıştırın:

```PowerShell
Update-Module -Name "AzureRm.BootStrapper"
```

## <a name="install-a-profile"></a>Profil yükleyin

Kullanım **yükleme AzureRmProfile** cmdlet'iyle **2017-03-09-profili** Azure yığını tarafından gerekli AzureRM modüllerini yüklemek için API sürümü profili. Bu API sürümü profiliyle Azure yığın işleci modülleri yüklenmemiş ve bunlar ayrı olarak belirtildiği gibi adım 3'te yüklenmesi gerektiğini unutmayın [Azure yığını için PowerShell yükleme](azure-stack-powershell-install.md) makalesi.

```PowerShell 
Install-AzureRMProfile -Profile 2017-03-09-profile
```
## <a name="install-and-import-modules-in-a-profile"></a>Yükleme ve modülleri profilde alın

Kullanım **kullanım AzureRmProfile** yüklemek ve bir API sürümü profille ilişkilendirilmiş modülleri içeri aktarmak için cmdlet. Bir PowerShell oturumunda tek bir API sürümü profilini içeri aktarabilirsiniz. Farklı bir API sürümü profil almak için yeni bir PowerShell oturum açmanız gerekir. Kullanım AzureRMProfile cmdlet'i aşağıdaki görevleri çalıştırır:  
1. Belirtilen API sürümü profiliyle ilişkili PowerShell modülleri geçerli kapsamda yüklü olup olmadığını denetler.  
2. İndirilir ve modülleri zaten yüklü değilse yüklenir.   
3. Modüller geçerli PowerShell oturumuna aktarır. 

```PowerShell
# Installs and imports the specified API version profile into the current PowerShell session.
Use-AzureRmProfile -Profile 2017-03-09-profile -Scope CurrentUser

# Installs and imports the specified API version profile into the current PowerShell session without any prompts
Use-AzureRmProfile -Profile 2017-03-09-profile -Scope CurrentUser -Force
```

Yükleme ve seçili AzureRM modülleri bir API sürümü profilden içeri aktarmak için kullanım AzureRMProfile çalıştırın **Modülü** parametre:

```PowerShell
# Installs and imports the compute, Storage and Network modules from the specified API version profile into your current PowerShell session.
Use-AzureRmProfile -Profile 2017-03-09-profile -Module AzureRM.Compute, AzureRM.Storage, AzureRM.Network
```

## <a name="get-the-installed-profiles"></a>Yüklü profiller Al

Kullanım **Get-AzureRmProfile** kullanılabilir API sürümü profiller listesini almak için cmdlet: 

```PowerShell
# lists all API version profiles provided by the AzureRM.BootStrapper module.
Get-AzureRmProfile -ListAvailable 

# lists the API version profiles which are installed on your machine
Get-AzureRmProfile
```
## <a name="update-profiles"></a>Güncelleştirme profilleri

Kullanım **güncelleştirme AzureRmProfile** cmdlet'ini bir API sürümü profili modülleri PSGallery kullanılabilir modüllerin en son sürüme güncelleştirin. Her zaman çalıştırmak için önerilen **güncelleştirme AzureRmProfile** cmdlet modülleri içeri aktarırken çakışmaları önlemek için yeni bir PowerShell oturumunda. Güncelleştirme AzureRmProfile cmdlet'i aşağıdaki görevleri çalıştırır:

1. Geçerli kapsam için belirtilen API sürümü profilinde modülleri en son sürümlerini yüklü olup olmadığını denetler.  
2. Zaten yüklü değilse yüklemenizi ister.  
3. Yükler ve güncelleştirilmiş modülleri geçerli PowerShell oturumuna aktarır.  

```PowerShell
Update-AzureRmProfile -Profile 2017-03-09-profile
```

Kullanılabilir en son sürüme güncelleştirmeden önce modülleri daha önce yüklenmiş sürümlerinin kaldırmak için güncelleştirme AzureRmProfile cmdlet ile birlikte kullanmak **- RemovePreviousVersions** parametre:

```PowerShell 
Update-AzureRmProfile -Profile 2017-03-09-profile -RemovePreviousVersions
```

Bu cmdlet'i aşağıdaki görevleri çalıştırır:  

1. Geçerli kapsam için belirtilen API sürümü profilinde modülleri en son sürümlerini yüklü olup olmadığını denetler.  
2. Modülleri eski sürümleri geçerli API sürümü profili ve geçerli PowerShell oturumunda kaldırır.  
4. en son sürümünü yüklemenizi ister.  
5. Yükler ve güncelleştirilmiş modülleri geçerli PowerShell oturumuna aktarır.  
 
## <a name="uninstall-profiles"></a>Profilleri kaldırma

Kullanım **kaldırma AzureRmProfile** cmdlet'ini belirtilen API sürümü profilini kaldırın.

```PowerShell 
Uninstall-AzureRmProfile -Profile 2017-03-09-profile
```

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Stack için PowerShell yükleme](azure-stack-powershell-install.md)
* [Azure yığın kullanıcının PowerShell ortamını yapılandırma](user/azure-stack-powershell-configure-user.md)  
