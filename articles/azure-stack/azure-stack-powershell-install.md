---
title: Azure Stack için PowerShell'i yükleme | Microsoft Docs
description: Azure Stack için PowerShell yüklemeyi öğrenin.
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
ms.date: 07/10/2018
ms.author: mabrigg
ms.reviewer: thoroet
ms.openlocfilehash: e2785b0beeab042d4b1ad9a9eb5f545dbb58b8b9
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38487510"
---
# <a name="install-powershell-for-azure-stack"></a>Azure Stack için PowerShell'i yükleme

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure Stack uyumlu Azure PowerShell modülleri, Azure Stack ile çalışmak için gereklidir. Bu kılavuzda, Azure Stack için PowerShell yüklemek için gereken adımları inceleyeceğiz.

Bu makalede, Azure Stack için PowerShell yükleme yönergeleri ayrıntılı içerir.

> [!Note]  
> Aşağıdaki adımlar PowerShell 5.0 gerekir. Sürümünüzü denetlemek için $PSVersionTable.PSVersion çalıştırmak ve karşılaştırmak **ana** sürümü.

Azure Stack için PowerShell komutları PowerShell Galerisi'nde yüklenir. Bir depo PSGallery kayıtlıysa doğrulamak için yükseltilmiş bir PowerShell oturumu açın ve aşağıdaki komutu çalıştırın aşağıdaki yordamı kullanabilirsiniz:

```PowerShell  
Get-PSRepository -Name "PSGallery"
```

Depo kayıtlı değilse, yükseltilmiş bir PowerShell oturumu açın ve aşağıdaki komutu çalıştırın:

```PowerShell  
Set-PSRepository -Name "PSGallery" -InstallationPolicy Trusted
```
> [!Note]  
> Bu adım, Internet erişimi gerektirir. 

## <a name="uninstall-existing-versions-of-the-azure-stack-powershell-modules"></a>Azure Stack PowerShell modüllerinin mevcut sürümlerini kaldırma

Gerekli sürümü yüklemeden önce önceden yüklenmiş tüm Azure Stack AzureRM PowerShell modülleri kaldırmak emin olun. Bunları, aşağıdaki iki yöntemden birini kullanarak kaldırabilirsiniz:

 - Mevcut AzureRM PowerShell modülleri kaldırmak için tüm etkin PowerShell oturumlarını kapatmak ve aşağıdaki komutu çalıştırın:

  ```PowerShell
    Uninstall-Module AzureRM.AzureStackAdmin -Force
    Uninstall-Module AzureRM.AzureStackStorage -Force
    Uninstall-Module -Name AzureStack -Force
  ```

 - "Azure" ile başlayan tüm klasörleri silmek `C:\Program Files\WindowsPowerShell\Modules` ve `C:\Users\AzureStackAdmin\Documents\WindowsPowerShell\Modules` klasörleri. Bu klasörleri silmek, tüm mevcut PowerShell modüllerini kaldırır.

Aşağıdaki bölümlerde, Azure Stack için PowerShell'i yüklemek için gereken adımlar açıklanmaktadır. PowerShell, Azure Stack'te de işletilen bağlı, kısmen bağlı veya bağlantısı kesilmiş bir senaryoda yüklenebilir.

## <a name="install-the-azure-stack-powershell-modules-in-a-connected-scenario-with-internet-connectivity"></a>Azure Stack PowerShell modüllerine (Internet bağlantısı) bağlı bir senaryoda yüklemek

Azure Stack uyumlu AzureRM modüllerini API sürümü profillerini yüklenir. Azure Stack gerektirir **2017-03-09-profile** AzureRM.Bootstrapper modülü yüklenerek kullanılabilir API Sürüm profili. API sürümü profillerini ve onlar tarafından sağlanan cmdlet'leri hakkında bilgi edinmek için bkz [API sürümü profillerini yönetme](user/azure-stack-version-profiles.md). AzureRM modülleri ek olarak, Azure Stack özel PowerShell modüllerini de yüklemeniz gerekir. Geliştirme iş istasyonunuzda bu modülleri yüklemek için aşağıdaki PowerShell betiğini çalıştırın:

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

## <a name="install-the-azure-stack-powershell-modules-in-a-disconnected-or-a-partially-connected-scenario-with-limited-internet-connectivity"></a>Azure Stack PowerShell modülleri bir bağlantısı kesilmiş veya kısmen bağlı bir senaryo (sınırlı Internet bağlantısı ile) yükleyin

Bağlantısı kesilmiş bir senaryoda, PowerShell modülleri Internet bağlantısı olan bir makine için ilk indirme ve yükleme Azure Stack Geliştirme Seti için Aktarım gerekir.

> [!IMPORTANT]  
> Azure Stack 1.3.0 PowerShell modülü sürüm çarpıcı değişikliklerin dinamik listesi ile birlikte gelir. 1.2.11 yükseltme sürümünü görmek [Geçiş Kılavuzu](https://aka.ms/azspowershellmigration).

1. İnternet bağlantınız ve yerel bilgisayarınıza indirme AzureRM ve AzureStack paketleri aşağıdaki betiği kullanın olduğu bir bilgisayarda oturum açın:

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
  > Azure Stack ile güncelleştirme 1804 veya üstü değil çalıştırıyorsanız, değiştirme **requiredversion** parametre değerine `1.2.11`. 

2. İndirilen paketler bir USB cihazına kopyalayabilirsiniz.

3. İş istasyonunda oturum açabilir ve paketleri USB cihazından iş istasyonundaki bir konuma kopyalayın.

4. Artık bu konum varsayılan depo Kaydet ve bu depodan AzureRM ve AzureStack modül yüklemeniz gerekir:

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

## <a name="configure-powershell-to-use-a-proxy-server"></a>PowerShell bir proxy sunucusu kullanacak şekilde yapılandırma

İnternet'e bir proxy sunucusu gerektiren senaryolar önce PowerShell var olan bir proxy sunucusu kullanacak şekilde yapılandırmanız gerekir.

1. Yükseltilmiş bir PowerShell istemi açın.
2. Aşağıdaki komutları çalıştırın:

````PowerShell  
  #To use Windows credentials for proxy authentication
  [System.Net.WebRequest]::DefaultWebProxy.Credentials = [System.Net.CredentialCache]::DefaultCredentials

  #Alternatively, to prompt for separate credentials that can be used for #proxy authentication

  [System.Net.WebRequest]::DefaultWebProxy.Credentials = Get-Credential
````

## <a name="next-steps"></a>Sonraki adımlar

 - [Github'dan Azure Stack araçları indirin](azure-stack-powershell-download.md)
 - [Azure Stack kullanıcının PowerShell ortamını yapılandırma](user/azure-stack-powershell-configure-user.md)  
 - [Azure Stack işlecin PowerShell ortamını yapılandırma](azure-stack-powershell-configure-admin.md) 
 - [Azure stack'teki API sürümü profillerini yönetme](user/azure-stack-version-profiles.md)  
