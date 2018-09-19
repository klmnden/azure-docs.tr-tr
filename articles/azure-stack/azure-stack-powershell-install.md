---
title: Azure Stack için PowerShell'i yükleme | Microsoft Docs
description: Azure Stack için PowerShell yüklemeyi öğrenin.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: article
ms.date: 09/18/2018
ms.author: sethm
ms.reviewer: thoroet
ms.openlocfilehash: c87b7f18ff5bf94bf842fa7a7e31cad4c7f47dfe
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46128051"
---
# <a name="install-powershell-for-azure-stack"></a>Azure Stack için PowerShell'i yükleme

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Bulut ile çalışmak için Azure Stack uyumlu PowerShell modülleri yüklemeniz gerekir. Uyumluluk adlı bir özellik üzerinden etkin *API profillerini*.

API profillerini Azure ve Azure Stack arasında sürümü farkları yönetmek için bir yol sağlar. Bir API Sürüm profili belirli API sürümleri ile Azure Resource Manager PowerShell modüllerini kümesidir. Her bulut platformu desteklenen API sürümü profillerini kümesi vardır. Örneğin, Azure Stack gibi belirli tarihli profil sürümü destekleyen **2018-03-01-karma**, ve Azure'ı destekleyen **son** API Sürüm profili. Belirtilen profiliyle Azure Resource Manager PowerShell modülleri, bir profil yükleme sırasında yüklenir.  

Internet uyumlu PowerShell modülleri bağlı, kısmen bağlantılı veya bağlantısız senaryoları Azure Stack yükleyebilirsiniz. Bu makale, bu senaryolar için Azure Stack için PowerShell yüklemeye yönelik ayrıntılı yönergeleri size.

## <a name="1-verify-your-prerequisites"></a>1. Önkoşulların doğrulayın

Azure Stack ve PowerShell ile çalışmaya başlamadan önce aşağıdaki önkoşullara sahip olmalıdır:

- **PowerShell sürüm 5.0**  
Sürümünüzü denetlemek için çalıştırma **$PSVersionTable.PSVersion** ve karşılaştırma **ana** sürümü. PowerShell 5.0 yoksa izleyin [Windows PowerShell'i yükleme](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell?view=powershell-6#upgrading-existing-windows-powershell).

  > [!Note]  
  > PowerShell 5.0, Windows makine gerektirir.

- **Yükseltilmiş bir komut istemi PowerShell çalıştırın**  
  PowerShell'i yönetici ayrıcalıklarıyla çalıştırmanız gerekir.

- **PowerShell Galerisi erişim**  
  Erişmeniz [PowerShell Galerisi](https://www.powershellgallery.com). Galeri, PowerShell içeriği için merkezi depodur. **PowerShellGet** modülü bulma, yükleme, güncelleştirme, modüller, DSC kaynakları, rol işlevleri ve PowerShell Galerisi'nde ve diğer özel betikler gibi PowerShell yapıtları yayımlama için cmdlet'leri içerir Depo. Bağlantısı kesilmiş bir senaryoda PowerShell kullanıyorsanız, Internet'e bir bağlantı olan bir makineden kaynakları almak ve bağlantısı kesilmiş makinenize erişilebilir bir konumda depolanması gerekir.


## <a name="2-validate-the-powershell-gallery-accessibility"></a>2. PowerShell Galerisi erişilebilirlik doğrulama

Bir depo PSGallery kayıtlıysa doğrulayın.

> [!Note]  
> Bu adım, Internet erişimi gerektirir. 

Yükseltilmiş bir PowerShell istemi açın ve aşağıdaki cmdlet'leri çalıştırın:

````PowerShell  
Import-Module -Name PowerShellGet -ErrorAction Stop
Import-Module -Name PackageManagement -ErrorAction Stop
Get-PSRepository -Name "PSGallery"
````

Depo kayıtlı değilse, yükseltilmiş bir PowerShell oturumu açın ve aşağıdaki komutu çalıştırın:

```PowerShell
Register-PsRepository -Default
Set-PSRepository -Name "PSGallery" -InstallationPolicy Trusted
```

## <a name="3-uninstall-existing-versions-of-the-azure-stack-powershell-modules"></a>3. Azure Stack PowerShell modüllerinin mevcut sürümlerini kaldırma

Gerekli sürümü yüklemeden önce önceden yüklenmiş tüm Azure Stack AzureRM PowerShell modülleri kaldırmak emin olun. Bunları, aşağıdaki iki yöntemden birini kullanarak kaldırabilirsiniz:

1. Mevcut AzureRM PowerShell modülleri kaldırmak için tüm etkin PowerShell oturumlarını kapatmak ve aşağıdaki cmdlet'leri çalıştırın:

  ````PowerShell  
    Uninstall-Module AzureRM.AzureStackAdmin -Force
    Uninstall-Module AzureRM.AzureStackStorage -Force
    Uninstall-Module -Name AzureStack -Force
    Get-Module Azs.* -ListAvailable | Uninstall-Module -Force
  ````

2. İle başlayan tüm klasörleri Sil `Azure` gelen `C:\Program Files\WindowsPowerShell\Modules` ve `C:\Users\{yourusername}\Documents\WindowsPowerShell\Modules` klasörleri. Bu klasörleri silmek, tüm mevcut PowerShell modüllerini kaldırır.

## <a name="4-connected-install-powershell-for-azure-stack-with-internet-connectivity"></a>4. Bağlı: Internet bağlantısı ile Azure Stack için yükleme PowerShell

Azure Stack gerektirir **2018-03-01-karma** Azure Stack sürümü 1808 için API Sürüm profili. Profili yüklenerek elde edilen **AzureRM.Bootstrapper** modülü. Ayrıca, AzureRM modülleri için de Azure Stack özel PowerShell modüllerine yüklemeniz gerekir. Azure Stack PowerShell modüllerini gerektirir ve API Sürüm profili olduğunuz Azure Stack sürümüne bağlıdır çalışıyor.

Geliştirme iş istasyonunuzda bu modülleri yüklemek için aşağıdaki PowerShell betiğini çalıştırın:

  - Azure Stack 1808 veya üzeri.

    ```PowerShell  
    # Install the AzureRM.Bootstrapper module. Select Yes when prompted to install NuGet 
    Install-Module -Name AzureRm.BootStrapper

    # Install and import the API Version Profile required by Azure Stack into the current PowerShell session.
    Use-AzureRmProfile -Profile 2018-03-01-hybrid -Force

    Install-Module -Name AzureStack -RequiredVersion 1.5.0 -Force
    ```

> [!Note]  
> Azure Powershell'den yükseltmek **2017-03-09-profile** için **2018-03-01-karma**, lütfen [Geçiş Kılavuzu](https://github.com/bganapa/azure-powershell/blob/migration-guide/documentation/migration-guides/Stack/migration-guide.2.3.0.md).

  - Azure Stack 1807 veya önceki bir sürümü.

    ```PowerShell  
    Install-Module -Name AzureRm.BootStrapper
    Use-AzureRmProfile -Profile 2017-03-09-profile -Force
    Install-Module -Name AzureStack -RequiredVersion 1.4.0 -Force
    ```

  - Azure Stack 1804 veya önceki bir sürümü.

    ```PowerShell  
    Install-Module -Name AzureRm.BootStrapper
    Use-AzureRmProfile -Profile 2017-03-09-profile -Force
    Install-Module -Name AzureStack -RequiredVersion 1.2.11 -Force
    ```

Aşağıdaki komutu çalıştırarak yüklemeyi doğrulayın:

```PowerShell  
Get-Module "Azure*" -ListAvailable
Get-Module "Azs*" -ListAvailable
```

Yükleme başarılı olursa, AzureRM ve AzureStack modülleri çıktısında görüntülenir.

## <a name="5-disconnected-install-powershell-without-an-internet-connection"></a>5. Bağlantısı kesildi: Internet bağlantısı olmadan yükleme PowerShell

Bağlantısı kesilmiş bir senaryoda, PowerShell modülleri Internet bağlantısı olan bir makine için ilk indirme ve yükleme Azure Stack Geliştirme Seti için Aktarım gerekir.

Internet bağlantısına sahip bir bilgisayarda oturum açın ve Azure Stack sürümünüze bağlı olarak Azure Resource Manager ve AzureStack paketleri indirmek için aşağıdaki komut kullanın:

  - Azure Stack 1808 veya üzeri.

    ````PowerShell  
    Import-Module -Name PowerShellGet -ErrorAction Stop
    Import-Module -Name PackageManagement -ErrorAction Stop

      $Path = "<Path that is used to save the packages>"
      Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureRM -Path $Path -Force -RequiredVersion 2.3.0
      Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureStack -Path $Path -Force -RequiredVersion 1.5.0
    ````

  - Azure Stack 1807 veya önceki bir sürümü.

    > [!Note]  
    1.2.11 yükseltme sürümünü görmek [Geçiş Kılavuzu](https://aka.ms/azspowershellmigration).

    ````PowerShell  
    Import-Module -Name PowerShellGet -ErrorAction Stop
    Import-Module -Name PackageManagement -ErrorAction Stop

    $Path = "<Path that is used to save the packages>"
    Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureRM -Path $Path -Force -RequiredVersion 1.2.11
    Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureStack -Path $Path -Force -RequiredVersion 1.4.0
    ````

  - Azure Stack 1804 veya önceki bir sürümü.

    ````PowerShell  
    Import-Module -Name PowerShellGet -ErrorAction Stop
    Import-Module -Name PackageManagement -ErrorAction Stop

    $Path = "<Path that is used to save the packages>"
    Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureRM -Path $Path -Force -RequiredVersion 1.2.11
    Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureStack -Path $Path -Force -RequiredVersion 1.3.0
    ````

2. İndirilen paketler bir USB cihazına kopyalayın.

3. İş istasyonunda oturum açabilir ve paketleri USB cihazından iş istasyonundaki bir konuma kopyalayın.

4. Artık bu konum varsayılan depo Kaydet ve bu depodan AzureRM ve AzureStack modüllerini yükleyin:

   ```PowerShell
   #requires -Version 5
   #requires -RunAsAdministrator
   #requires -Module PowerShellGet
   #requires -Module PackageManagement

   $SourceLocation = "<Location on the development kit that contains the PowerShell packages>"
   $RepoName = "MyNuGetSource"

   Register-PSRepository -Name $RepoName -SourceLocation $SourceLocation  -InstallationPolicy Trusted

   Install-Module AzureRM -Repository $RepoName

   Install-Module AzureStack -Repository $RepoName 
   ```

## <a name="6-configure-powershell-to-use-a-proxy-server"></a>6. PowerShell bir proxy sunucusu kullanacak şekilde yapılandırma

Internet'e bir proxy sunucusu gerektiren senaryolar önce mevcut proxy sunucusu kullanacak şekilde yapılandırmanız gerekir:

1. Yükseltilmiş bir PowerShell istemi açın.
2. Aşağıdaki komutları çalıştırın:

   ```PowerShell  
   #To use Windows credentials for proxy authentication
   [System.Net.WebRequest]::DefaultWebProxy.Credentials = [System.Net.CredentialCache]::DefaultCredentials

   #Alternatively, to prompt for separate credentials that can be used for #proxy authentication
   [System.Net.WebRequest]::DefaultWebProxy.Credentials = Get-Credential
   ```

## <a name="next-steps"></a>Sonraki adımlar

 - [Github'dan Azure Stack araçları indirin](azure-stack-powershell-download.md)
 - [Azure Stack kullanıcının PowerShell ortamını yapılandırma](user/azure-stack-powershell-configure-user.md)  
 - [Azure Stack işlecin PowerShell ortamını yapılandırma](azure-stack-powershell-configure-admin.md) 
 - [Azure stack'teki API sürümü profillerini yönetme](user/azure-stack-version-profiles.md)  
