---
title: PowerShell için Azure yığın yükleme | Microsoft Docs
description: PowerShell için Azure yığınına yüklemeyi öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: article
ms.date: 5/18/2018
ms.author: mabrigg
ms.reviewer: thoroet
ms.openlocfilehash: b3c09582f5135655640768bcbcbef91750827bfa
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
ms.locfileid: "34358899"
---
# <a name="install-powershell-for-azure-stack"></a>PowerShell için Azure yığın yükle

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Azure yığın uyumlu Azure PowerShell modülleri Azure yığın ile çalışmak için gereklidir. Bu kılavuzda, biz, PowerShell için Azure yığınına yüklemek için gereken adımlarda size yol.

Bu makalede Azure yığını için PowerShell yüklemek için yönergeleri açıklanmıştır.

> [!Note]
> Aşağıdaki adımları PowerShell 5.0 gerektirir. Sürümünüzü denetlemek için $PSVersionTable.PSVersion çalıştırmak ve karşılaştırmak **ana** sürümü.

Azure yığını için PowerShell komutlarını PowerShell Galerisi aracılığıyla yüklenir. PSGallery depo olarak kaydedilmişse doğrulamak için yükseltilmiş bir PowerShell oturumu açın ve aşağıdaki komutu çalıştırmak için aşağıdaki yordamı kullanın:

```PowerShell  
Get-PSRepository -Name "PSGallery"
```

Depo kayıtlı değilse, yükseltilmiş bir PowerShell oturumu açın ve aşağıdaki komutu çalıştırın:

```PowerShell  
Set-PSRepository -Name "PSGallery" -InstallationPolicy Trusted
```
> [!Note]  
> Bu adım Internet erişimi gerektirir. 

## <a name="uninstall-existing-versions-of-powershell"></a>PowerShell varolan sürümlerini kaldırın

Gerekli sürümü yüklemeden önce önceden yüklenmiş tüm Azure yığın PowerShell modülleri kaldırmak emin olun. Bunları, aşağıdaki iki yöntemden birini kullanarak kaldırabilirsiniz:

 - Varolan PowerShell modülleri kaldırmak için tüm etkin PowerShell oturumları kapatın ve aşağıdaki komutu çalıştırın:

  ```PowerShell
    Uninstall-Module AzureRM.AzureStackAdmin -Force
    Uninstall-Module AzureRM.AzureStackStorage -Force
    Uninstall-Module -Name AzureStack -Force
  ```

 - "Azure" ile başlayan tüm klasörleri silin `C:\Program Files\WindowsPowerShell\Modules` ve `C:\Users\AzureStackAdmin\Documents\WindowsPowerShell\Modules` klasörler. Bu klasörlerin silinmesi, tüm mevcut PowerShell modülleri kaldırır.

Aşağıdaki bölümlerde Azure yığını için PowerShell yüklemek için gereken adımlar açıklanmaktadır. PowerShell Azure yığında içinde çalıştırılır bağlı, kısmen bağlı veya bağlantısı kesilmiş bir senaryoda yüklenebilir.

## <a name="install-powershell-in-a-connected-scenario-with-internet-connectivity"></a>PowerShell bağlı bir senaryoda (Internet bağlantısı ile) yükleyin.

Azure yığın uyumlu AzureRM modülleri API sürümü profilleri yüklenir. Azure yığın gerektirir **2017-03-09-profili** AzureRM.Bootstrapper modülü yükleyerek kullanılabilir API sürümü profili. API sürümü profilleri ve onlar tarafından sağlanan cmdlet'leri hakkında bilgi edinmek için bkz [API sürümü profillerini yönetmek](user/azure-stack-version-profiles.md). AzureRM modülleri yanı sıra Azure yığın özgü PowerShell modülleri de yüklemeniz gerekir. Bu modüller, geliştirme iş istasyonunda yüklemek için aşağıdaki PowerShell betiğini çalıştırın:

  ```PowerShell  
# Install the AzureRM.Bootstrapper module. Select Yes when prompted to install NuGet 
Install-Module -Name AzureRm.BootStrapper 

# Install and import the API Version Profile required by Azure Stack into the current PowerShell session. 
Use-AzureRmProfile -Profile 2017-03-09-profile -Force 

# Install Module Version 1.3.0 if Azure Stack is running 1804 at a minimum 
Install-Module -Name AzureStack -RequiredVersion 1.3.0 

# Install Module Version 1.2.11 if Azure Stack is running a lower version than 1804 
Install-Module -Name AzureStack -RequiredVersion 1.2.11 
  ```

Yükleme işlemini onaylamak için aşağıdaki komutu çalıştırın:

```PowerShell  
Get-Module -ListAvailable | where-Object {$_.Name -like "Azs*"}
```

Yükleme başarılı olursa, AzureRM ve AzureStack modülleri çıktısında görüntülenir.

## <a name="install-powershell-in-a-disconnected-or-a-partially-connected-scenario-with-limited-internet-connectivity"></a>PowerShell bir bağlantısı kesilmiş veya kısmen bağlı bir senaryo (sınırlı Internet bağlantısı ile) yükleyin

Bağlantısı kesilmiş bir senaryoda, PowerShell modülleri Internet bağlantısına sahip bir makine için ilk indirme ve yükleme için Azure yığın Geliştirme Seti için Aktarım gerekir.

> [!IMPORTANT]  
> Azure yığın 1.3.0 PowerShell modülü sürümü yeni değişiklikler ile ilgili bir listesi bulunur. 1.2.11 yükseltmek için sürüm, bkz: [Geçiş Kılavuzu](https://aka.ms/azspowershellmigration).

1. Burada Internet bağlantısına sahip ve aşağıdaki komut dosyasını indirme AzureRM ve AzureStack paketleri için yerel bilgisayarınıza kullanın bir bilgisayarda oturum açın:

   ```PowerShell  
   $Path = "<Path that is used to save the packages>"

   Save-Package `
     -ProviderName NuGet `
     -Source https://www.powershellgallery.com/api/v2 `
     -Name AzureRM `
     -Path $Path `
     -Force `
     -RequiredVersion 1.2.11

   Save-Package `
     -ProviderName NuGet `
     -Source https://www.powershellgallery.com/api/v2 `
     -Name AzureStack `
     -Path $Path `
     -Force `
     -RequiredVersion 1.3.0 
   ```

  > [!Important]  
  > Azure yığın güncelleştirmeyle 1804 veya daha büyük olmayan çalıştırıyorsanız, değiştirme **requiredversion** parametre değerini `1.2.11`. 

2. İndirilen paketler üzerinden bir USB aygıtı kopyalayın.

3. İş istasyonunda oturum açabilir ve paketleri USB aygıtından iş istasyonundaki bir konuma kopyalayın.

4. Şimdi bu konumu varsayılan deposu olarak kaydetmek ve bu depodan AzureRM ve AzureStack modüllerini yükleyin:

   ```PowerShell
   $SourceLocation = "<Location on the development kit that contains the PowerShell packages>"
   $RepoName = "MyNuGetSource"

   Register-PSRepository `
     -Name $RepoName `
     -SourceLocation $SourceLocation `
     -InstallationPolicy Trusted

   Install-Module AzureRM `
     -Repository $RepoName

   Install-Module AzureStack `
     -Repository $RepoName 
   ```

## <a name="configure-powershell-to-use-a-proxy-server"></a>Bir proxy sunucu kullanmak için PowerShell yapılandırma

İnternet'e erişmesi için Ara sunucuya ihtiyaç senaryolarda, önce varolan bir proxy sunucusu kullanmak için PowerShell yapılandırmanız gerekir.

1. Yükseltilmiş bir PowerShell komut istemini açın.
2. Aşağıdaki komutları çalıştırın:

````PowerShell  
  #To use Windows credentials for proxy authentication
  [System.Net.WebRequest]::DefaultWebProxy.Credentials = [System.Net.CredentialCache]::DefaultCredentials

  #Alternatively, to prompt for separate credentials that can be used for #proxy authentication

  [System.Net.WebRequest]::DefaultWebProxy.Credentials = Get-Credential
````

## <a name="next-steps"></a>Sonraki adımlar

 - [Azure yığın araçları Github'dan yükleyin](azure-stack-powershell-download.md)
 - [Azure yığın kullanıcının PowerShell ortamını yapılandırma](user/azure-stack-powershell-configure-user.md)  
 - [Azure yığın işlecin PowerShell ortamını yapılandırma](azure-stack-powershell-configure-admin.md) 
 - [Azure yığınında API sürümü profillerini yönet](user/azure-stack-version-profiles.md)  
