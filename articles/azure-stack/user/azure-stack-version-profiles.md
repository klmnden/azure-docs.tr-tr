---
title: "Azure yığınında API sürümü profillerini yönetme | Microsoft Docs"
description: "Azure yığınında API sürümü profilleri hakkında bilgi edinin"
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 6B749785-DCF5-4AD8-B808-982E7C6BBA0E
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/21/2017
ms.author: mabrigg
ms.openlocfilehash: d86a54ea9e165269131eb961df7f74703f0ec6ff
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="manage-api-version-profiles-in-azure-stack"></a>Azure yığınında API sürümü profillerini yönet

Azure uygulama hizmeti sürüm profillerinin API özelliğini sürüm farklarını Azure ve Azure yığın yönetmek için bir yol sağlar. Bir API sürümü profili belirli API sürümleri ile AzureRM PowerShell modülleri kümesidir. Her bulut platformu desteklenen API sürümü profilleri kümesi vardır. Örneğin, Azure yığın belirli, tarihli profil sürümü gibi destekleyen **2017-03-09-profili**, ve Azure destekler *son* API sürümü profili. Bir profili yüklediğinizde, belirtilen profiliyle AzureRM PowerShell modülleri yüklenir.

## <a name="install-the-powershell-module-required-to-use-api-version-profiles"></a>API sürümü profillerini kullanmak için gerekli PowerShell modülünü yükleyin

**AzureRM.Bootstrapper** PowerShell Galerisi aracılığıyla kullanılabilir modül API sürümü profilleri ile çalışmak için gerekli PowerShell cmdlet'leri sağlar. Yüklemek için aşağıdaki cmdlet'i kullanın **AzureRM.Bootstrapper** Modülü:

```PowerShell
Install-Module -Name AzureRm.BootStrapper
```
**AzureRM.Bootstrapper** modülüdür Önizleme, Ayrıntılar ve işlevsellik değiştirilebilir; bu nedenle. PowerShell Galerisi'nden bu modül en son sürümünü yükleyip karşıdan yüklemek için aşağıdaki cmdlet'i çalıştırın:

```PowerShell
Update-Module -Name "AzureRm.BootStrapper"
```

## <a name="install-a-profile"></a>Profil yükleyin

Kullanım **yükleme AzureRmProfile** cmdlet'iyle **2017-03-09-profili** Azure yığını tarafından gerekli AzureRM modüllerini yüklemek için API sürümü profili. 

>[!NOTE]
>Bu API sürümü profiliyle Azure yığın bulut yönetici modülleri yüklü değil. Yönetici modülleri ayrı olarak belirtildiği gibi adım 3'te yüklenmelidir [Azure yığını için PowerShell yükleme](azure-stack-powershell-install.md) makalesi.

```PowerShell 
Install-AzureRMProfile -Profile 2017-03-09-profile
```
## <a name="install-and-import-modules-in-a-profile"></a>Yükleme ve modülleri profilde alın

Kullanım **kullanım AzureRmProfile** yüklemek ve bir API sürümü profille ilişkilendirilmiş modülleri içeri aktarmak için cmdlet. Bir PowerShell oturumunda tek bir API sürümü profilini içeri aktarabilirsiniz. Farklı bir API sürümü profil almak için yeni bir PowerShell oturum açmanız gerekir. **Kullanım AzureRMProfile** cmdlet'i aşağıdaki görevleri çalıştırır:  
1. Belirtilen API sürümü profiliyle ilişkili PowerShell modülleri geçerli kapsamda yüklü olup olmadığını görmek için denetler.  
2. İndirilir ve modülleri zaten yüklü değilse yüklenir.   
3. Modüller geçerli PowerShell oturumuna aktarır. 

```PowerShell
# Installs and imports the specified API version profile into the current PowerShell session.
Use-AzureRmProfile -Profile 2017-03-09-profile -Scope CurrentUser

# Installs and imports the specified API version profile into the current PowerShell session without any prompts.
Use-AzureRmProfile -Profile 2017-03-09-profile -Scope CurrentUser -Force
```

Yüklemek ve seçili AzureRM modülleri bir API sürümü profilden içeri aktarmak için çalıştırması **kullanım AzureRMProfile** cmdlet'iyle *Modülü* parametre:

```PowerShell
# Installs and imports the Compute, Storage, and Network modules from the specified API version profile into your current PowerShell session.
Use-AzureRmProfile -Profile 2017-03-09-profile -Module AzureRM.Compute, AzureRM.Storage, AzureRM.Network
```

## <a name="get-the-installed-profiles"></a>Yüklü profiller Al

Kullanım **Get-AzureRmProfile** kullanılabilir API sürümü profiller listesini almak için cmdlet: 

```PowerShell
# Lists all API version profiles provided by the AzureRM.BootStrapper module.
Get-AzureRmProfile -ListAvailable 

# Lists the API version profiles that are installed on your machine.
Get-AzureRmProfile
```
## <a name="update-profiles"></a>Güncelleştirme profilleri

Kullanım **güncelleştirme AzureRmProfile** cmdlet'ini bir API sürümü profili modülleri PowerShell galerisinde kullanılabilir modüllerin en son sürüme güncelleştirin. Çalıştırmanızı öneririz **güncelleştirme AzureRmProfile** cmdlet modülleri içeri aktardığınızda çakışmaları önlemek için yeni bir PowerShell oturumunda. **Güncelleştirme AzureRmProfile** cmdlet'i aşağıdaki görevleri çalıştırır:

1. Geçerli kapsam için belirtilen API sürümü profilinde modülleri en son sürümlerini yüklü olup olmadığını görmek için denetler.  
2. Zaten yüklü değilse modüllerini yüklemenizi ister.  
3. Yükler ve güncelleştirilmiş modülleri geçerli PowerShell oturumuna aktarır.  

```PowerShell
Update-AzureRmProfile -Profile 2017-03-09-profile
```

Kullanılabilir en son sürüme güncelleştirmeden önce modülleri daha önce yüklenmiş sürümlerinin kaldırmak için kullanın **güncelleştirme AzureRmProfile** cmdlet'i ile birlikte *- RemovePreviousVersions* Parametre:

```PowerShell 
Update-AzureRmProfile -Profile 2017-03-09-profile -RemovePreviousVersions
```

Bu cmdlet'i aşağıdaki görevleri çalıştırır:  

1. Geçerli kapsam için belirtilen API sürümü profilinde modülleri en son sürümlerini yüklü olup olmadığını görmek için denetler.  
2. Modülleri eski sürümleri geçerli API sürümü profili ve geçerli PowerShell oturumunda kaldırır.  
3. Modülleri en son sürümünü yüklemenizi ister.  
4. Yükler ve güncelleştirilmiş modülleri geçerli PowerShell oturumuna aktarır.  
 
## <a name="uninstall-profiles"></a>Profilleri kaldırma

Kullanım **kaldırma AzureRmProfile** cmdlet'i belirtilen API sürümü profilini kaldırmak için:

```PowerShell 
Uninstall-AzureRmProfile -Profile 2017-03-09-profile
```

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Stack için PowerShell yükleme](azure-stack-powershell-install.md)
* [Azure yığın kullanıcının PowerShell ortamını yapılandırma](azure-stack-powershell-configure-user.md)  
