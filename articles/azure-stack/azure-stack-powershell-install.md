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
ms.date: 02/08/2019
ms.author: mabrigg
ms.reviewer: thoroet
ms.lastreviewed: 02/08/2019
ms.openlocfilehash: 7e631281405b173405f28c134432e870c757b3da
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58648411"
---
# <a name="install-powershell-for-azure-stack"></a>Azure Stack için PowerShell'i yükleme

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Bulut ile çalışmak için Azure Stack uyumlu PowerShell modülleri yüklemeniz gerekir. Uyumluluk adlı bir özellik üzerinden etkin *API profillerini*.

API profillerini Azure ve Azure Stack arasında sürümü farkları yönetmek için bir yol sağlar. Bir API Sürüm profili belirli API sürümleri ile Azure Resource Manager PowerShell modüllerini kümesidir. Her bulut platformu desteklenen API sürümü profillerini kümesi vardır. Örneğin, Azure Stack gibi bir özel profil sürümünü destekler **2018-03-01-karma**. Belirtilen profiliyle Azure Resource Manager PowerShell modülleri, bir profil yükleme sırasında yüklenir.

Internet uyumlu PowerShell modülleri bağlı, kısmen bağlantılı veya bağlantısız senaryoları Azure Stack yükleyebilirsiniz. Bu makale, bu senaryolar için Azure Stack için PowerShell yüklemeye yönelik ayrıntılı yönergeleri size.

## <a name="1-verify-your-prerequisites"></a>1. Önkoşulların doğrulayın

Azure Stack ve PowerShell ile çalışmaya başlamadan önce aşağıdaki önkoşullara sahip olmalıdır:

- **PowerShell sürüm 5.0** sürümünüzü denetlemek için çalıştırma **$PSVersionTable.PSVersion** ve karşılaştırma **ana** sürümü. PowerShell 5.0 yoksa izleyin [Windows PowerShell'i yükleme](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell?view=powershell-6#upgrading-existing-windows-powershell).

  > [!Note]
  > PowerShell 5.0, Windows makine gerektirir.

- **Yükseltilmiş bir komut istemi PowerShell çalıştırın**.
  PowerShell'i yönetici ayrıcalıklarıyla çalıştırmanız gerekir.

- **PowerShell Galerisi erişim** erişmeniz [PowerShell Galerisi](https://www.powershellgallery.com). Galeri, PowerShell içeriği için merkezi depodur. **PowerShellGet** modülü bulma, yükleme, güncelleştirme, modüller, DSC kaynakları, rol işlevleri ve PowerShell Galerisi'nde ve diğer özel betikler gibi PowerShell yapıtları yayımlama için cmdlet'leri içerir Depo. Bağlantısı kesilmiş bir senaryoda PowerShell kullanıyorsanız, Internet'e bir bağlantı olan bir makineden kaynakları almak ve bağlantısı kesilmiş makinenize erişilebilir bir konumda depolanması gerekir.

## <a name="2-validate-the-powershell-gallery-accessibility"></a>2. PowerShell Galerisi erişilebilirlik doğrulama

Bir depo PSGallery kayıtlıysa doğrulayın.

> [!Note]  
> Bu adım, Internet erişimi gerektirir.

Yükseltilmiş bir PowerShell istemi açın ve aşağıdaki cmdlet'leri çalıştırın:

```powershell
Import-Module -Name PowerShellGet -ErrorAction Stop
Import-Module -Name PackageManagement -ErrorAction Stop
Get-PSRepository -Name "PSGallery"
```

Depo kayıtlı değilse, yükseltilmiş bir PowerShell oturumu açın ve aşağıdaki komutu çalıştırın:

```powershell
Register-PsRepository -Default
Set-PSRepository -Name "PSGallery" -InstallationPolicy Trusted
```

## <a name="3-uninstall-existing-versions-of-the-azure-stack-powershell-modules"></a>3. Azure Stack PowerShell modüllerinin mevcut sürümlerini kaldırma

Gerekli sürümü yüklemeden önce önceden yüklenmiş tüm Azure Stack AzureRM PowerShell modülleri kaldırmak emin olun. Bunları, aşağıdaki iki yöntemden birini kullanarak kaldırabilirsiniz:

1. Mevcut AzureRM PowerShell modülleri kaldırmak için tüm etkin PowerShell oturumlarını kapatmak ve aşağıdaki cmdlet'leri çalıştırın:

    ```powershell
    Get-Module -Name Azs.* -ListAvailable | Uninstall-Module -Force -Verbose
    Get-Module -Name Azure* -ListAvailable | Uninstall-Module -Force -Verbose
    ```

    Bir hata ulaşırsanız 'modülü zaten kullanımda olduğu gibi', lütfen modülleri kullanan ve yukarıdaki komut dosyasını yeniden PowerShell oturumları kapatın.

2. İle başlayan tüm klasörleri Sil `Azure` veya `Azs.` gelen `C:\Program Files\WindowsPowerShell\Modules` ve `C:\Users\{yourusername}\Documents\WindowsPowerShell\Modules` klasörleri. Bu klasörleri silmek, tüm mevcut PowerShell modüllerini kaldırır.

## <a name="4-connected-install-powershell-for-azure-stack-with-internet-connectivity"></a>4. Bağlı: PowerShell'i Azure Stack için Internet bağlantısına sahip yükleyin.

Azure Stack gerektirir **2018-03-01-karma** Azure Stack 1808 veya sonraki bir sürümü için API Sürüm profili. Profili yüklenerek elde edilen **AzureRM.BootStrapper** modülü. Ayrıca, AzureRM modülleri için de Azure Stack özel PowerShell modüllerine yüklemeniz gerekir. Azure Stack PowerShell modüllerini gerektirir ve API Sürüm profili olduğunuz Azure Stack sürümüne bağlıdır çalışıyor.

Kurulum, üç adım vardır:

1. Azure Stack sürümünüze bağlı olarak Azure Stack PowerShell'i yükleme
2. Ek depolama alanı özelliklerini etkinleştirme
3. PowerShell yüklenmesini onaylayın

### <a name="install-azure-stack-powershell"></a>Azure Stack PowerShell’i yükleme

Geliştirme iş istasyonunuzda bu modülleri yüklemek için aşağıdaki PowerShell betiğini çalıştırın:

- Azure Stack 1901 veya sonraki bir sürümü:

    ```powershell
    # Install and import the API Version Profile required by Azure Stack into the current PowerShell session.

    Install-Module AzureRM -RequiredVersion 2.4.0
    Install-Module -Name AzureStack -RequiredVersion 1.7.1
    ```

    > [!Note]  
    > Azure Stack modülü sürüm 1.7.1 bir önemli değişiklik yayınıdır. Azure yığını geçirme 1.6.0 Lütfen başvurmak için [Geçiş Kılavuzu](https://aka.ms/azspshmigration171).
    > AzureRm modülü sürümü 2.4.0, Remove-AzureRmStorageAccount cmdlet'ini için bir değişiklik ile birlikte gelir. Bu cmdlet bekliyor - Force parametresini onaysız depolama hesabını kaldırmak için belirtilmelidir.

- Azure Stack 1811:

    ```powershell
    # Install the AzureRM.BootStrapper module. Select Yes when prompted to install NuGet
    Install-Module -Name AzureRM.BootStrapper

    # Install and import the API Version Profile required by Azure Stack into the current PowerShell session.
    Use-AzureRmProfile -Profile 2018-03-01-hybrid -Force

    Install-Module -Name AzureStack -RequiredVersion 1.6.0
    ```

- Azure Stack 1810 veya önceki bir sürümü:

    ```powershell
    # Install the AzureRM.BootStrapper module. Select Yes when prompted to install NuGet
    Install-Module -Name AzureRM.BootStrapper

    # Install and import the API Version Profile required by Azure Stack into the current PowerShell session.
    Use-AzureRmProfile -Profile 2018-03-01-hybrid -Force

    Install-Module -Name AzureStack -RequiredVersion 1.5.0
    ```

> [!Note]  
> Azure Powershell'den yükseltmek **2017-03-09-profile** için **2018-03-01-karma**, lütfen [Geçiş Kılavuzu](https://github.com/azure/azure-powershell/blob/AzureRM/documentation/migration-guides/Stack/migration-guide.2.3.0.md).

### <a name="enable-additional-storage-features"></a>Ek depolama alanı özelliklerini etkinleştirme

İndirme (bağlı bölümde bahsedilen) ek depolama özelliklerini kullanmak ve de aşağıdaki kullanarak paketleri yüklemeniz sağlamak için.

```powershell
# Install the Azure.Storage module version 4.5.0
Install-Module -Name Azure.Storage -RequiredVersion 4.5.0 -Force -AllowClobber

# Install the AzureRm.Storage module version 5.0.4
Install-Module -Name AzureRM.Storage -RequiredVersion 5.0.4 -Force -AllowClobber

# Remove incompatible storage module installed by AzureRM.Storage
Uninstall-Module Azure.Storage -RequiredVersion 4.6.1 -Force

# Load the modules explicitly specifying the versions
Import-Module -Name Azure.Storage -RequiredVersion 4.5.0
Import-Module -Name AzureRM.Storage -RequiredVersion 5.0.4
```

### <a name="confirm-the-installation-of-powershell"></a>PowerShell yüklenmesini onaylayın

Aşağıdaki komutu çalıştırarak yüklemeyi doğrulayın:

```powershell
Get-Module -Name "Azure*" -ListAvailable
Get-Module -Name "Azs*" -ListAvailable
```

Yükleme başarılı olursa, AzureRM ve AzureStack modülleri çıktısında görüntülenir.

## <a name="5-disconnected-install-powershell-without-an-internet-connection"></a>5. Bağlantısı kesildi: Internet bağlantısı olmadan PowerShell'i yükleme

Bağlantısı kesilmiş bir senaryoda, PowerShell modülleri Internet bağlantısı olan bir makine için ilk indirme ve yükleme Azure Stack Geliştirme Seti için Aktarım gerekir.

Internet bağlantısına sahip bir bilgisayarda oturum açın ve Azure Stack sürümünüze bağlı olarak Azure Resource Manager ve AzureStack paketleri indirmek için aşağıdaki betikleri kullanın.

Yükleme dört adım vardır:

1. Bağlı bir makine için Azure Stack PowerShell'i yükleme
2. Ek depolama alanı özelliklerini etkinleştirme
3. Bağlantısı kesilmiş iş istasyonunuzu PowerShell paketler taşıma
4. PowerShell yüklenmesini onaylayın

### <a name="install-azure-stack-powershell"></a>Azure Stack PowerShell’i yükleme

- Azure Stack 1901 veya üzeri.

    ```powershell
    Import-Module -Name PowerShellGet -ErrorAction Stop
    Import-Module -Name PackageManagement -ErrorAction Stop

    $Path = "<Path that is used to save the packages>"
    Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureRM -Path $Path -Force -RequiredVersion 2.4.0
    Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureStack -Path $Path -Force -RequiredVersion 1.7.1
    ```

    > [!Note]  
    > Azure Stack modülü sürüm 1.7.1 bölünmesi farklıdır. AzureStack geçiş 1.6.0 Lütfen başvurmak için [Geçiş Kılavuzu](https://github.com/Azure/azure-powershell/tree/AzureRM/documentation/migration-guides/Stack).

  - Azure Stack 1811 veya önceki bir sürümü.

    ```powershell
    Import-Module -Name PowerShellGet -ErrorAction Stop
    Import-Module -Name PackageManagement -ErrorAction Stop

    $Path = "<Path that is used to save the packages>"
    Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureRM -Path $Path -Force -RequiredVersion 2.3.0
    Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureStack -Path $Path -Force -RequiredVersion 1.6.0
    ```

  - Azure Stack 1809 veya önceki bir sürümü.

    ```powershell
    Import-Module -Name PowerShellGet -ErrorAction Stop
    Import-Module -Name PackageManagement -ErrorAction Stop

    $Path = "<Path that is used to save the packages>"
    Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureRM -Path $Path -Force -RequiredVersion 2.3.0
    Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureStack -Path $Path -Force -RequiredVersion 1.5.0
    ```

    > [!NOTE]
    > Internet bağlantısı olmadan makinelerde telemetri veri koleksiyonunu devre dışı bırakmak için aşağıdaki cmdlet'i yürütme öneririz. Telemetri veri koleksiyonunu devre dışı bırakmadan cmdlet'lerinin performans düşüşü karşılaşabilirsiniz. Bu yalnızca internet bağlantısı olmayan makineler için geçerlidir
    > ```powershell
    > Disable-AzureRmDataCollection
    > ```

### <a name="enable-additional-storage-features"></a>Ek depolama alanı özelliklerini etkinleştirme

İndirme (bağlı bölümde bahsedilen) ek depolama özelliklerini kullanmak ve de aşağıdaki kullanarak paketleri yüklemeniz sağlamak için.

```powershell
$Path = "<Path that is used to save the packages>"
Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name Azure.Storage -Path $Path -Force -RequiredVersion 4.5.0
Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureRm.Storage -Path $Path -Force -RequiredVersion 5.0.4
```

### <a name="add-your-packages-to-your-workstation"></a>Paketlerinizi istasyonunuza Ekle

1. İndirilen paketler bir USB cihazına kopyalayın.

2. Bağlantısı kesilmiş iş istasyonunda oturum açabilir ve paketleri USB cihazından iş istasyonundaki bir konuma kopyalayın.

3. Artık bu konum varsayılan depo Kaydet ve bu depodan AzureRM ve AzureStack modüllerini yükleyin:

   ```powershell
   #requires -Version 5
   #requires -RunAsAdministrator
   #requires -Module PowerShellGet
   #requires -Module PackageManagement

   $SourceLocation = "<Location on the development kit that contains the PowerShell packages>"
   $RepoName = "MyNuGetSource"

   Register-PSRepository -Name $RepoName -SourceLocation $SourceLocation  -InstallationPolicy Trusted

   Install-Module -Name AzureRM -Repository $RepoName

   Install-Module -Name AzureStack -Repository $RepoName
   ```

### <a name="confirm-the-installation-of-powershell"></a>PowerShell yüklenmesini onaylayın

Aşağıdaki komutu çalıştırarak yüklemeyi doğrulayın:

```powershell
Get-Module -Name "Azure*" -ListAvailable
Get-Module -Name "Azs*" -ListAvailable
```

## <a name="6-configure-powershell-to-use-a-proxy-server"></a>6. PowerShell bir proxy sunucusu kullanacak şekilde yapılandırma

Internet'e bir proxy sunucusu gerektiren senaryolar önce mevcut proxy sunucusu kullanacak şekilde yapılandırmanız gerekir:

1. Yükseltilmiş bir PowerShell istemi açın.
2. Aşağıdaki komutları çalıştırın:

   ```powershell
   #To use Windows credentials for proxy authentication
   [System.Net.WebRequest]::DefaultWebProxy.Credentials = [System.Net.CredentialCache]::DefaultCredentials

   #Alternatively, to prompt for separate credentials that can be used for #proxy authentication
   [System.Net.WebRequest]::DefaultWebProxy.Credentials = Get-Credential
   ```

## <a name="next-steps"></a>Sonraki adımlar

- [Github'dan Azure Stack araçları indirin](azure-stack-powershell-download.md)
- [Azure Stack kullanıcının PowerShell ortamını yapılandırma](user/azure-stack-powershell-configure-user.md)
- [Azure Stack işlecin PowerShell ortamını yapılandırma](azure-stack-powershell-configure-admin.md)
- [Azure Stack’de API sürümü profillerini yönetme](user/azure-stack-version-profiles.md)
