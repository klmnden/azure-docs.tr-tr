---
title: Dağıtım yapılandırmaları Azure Stack geliştirme Seti'ni (ASDK) için gönderin | Microsoft Docs
description: Azure Stack geliştirme Seti'ni (ASDK) yükledikten sonra yapmak için önerilen yapılandırma değişiklikleri açıklar.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/05/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: d3bfe2c472d48a68bd818ac06874db136528b470
ms.sourcegitcommit: 3d0295a939c07bf9f0b38ebd37ac8461af8d461f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/06/2018
ms.locfileid: "43840278"
---
# <a name="post-asdk-installation-configuration-tasks"></a>ASDK yükleme sonrası yapılandırma görevleri

Sonra [Azure Stack geliştirme Seti'ni (ASDK) yükleme](asdk-install.md), bazı önerilen bir yükleme sonrası yapılandırma değişiklikleri yapmak ihtiyacınız olacak.

## <a name="install-azure-stack-powershell"></a>Azure Stack PowerShell’i yükleme

Azure Stack uyumlu Azure PowerShell modülleri, Azure Stack ile çalışmak için gereklidir.

Azure Stack için PowerShell komutları PowerShell Galerisi'nde yüklenir. PSGallery depo kaydetmek için yükseltilmiş bir PowerShell oturumu açın ve aşağıdaki komutu çalıştırın:

``` Powershell
Set-PSRepository `
  -Name "PSGallery" `
  -InstallationPolicy Trusted
```

Azure Stack uyumlu AzureRM modülleri belirtmek için API sürümü profillerini kullanabilirsiniz.  API sürümü profillerini Azure ve Azure Stack arasında sürümü farkları yönetmek için bir yol sağlar. Bir API Sürüm profili, AzureRM PowerShell modülleri belirli API sürümleri ile kümesidir. **AzureRM.Bootstrapper** PowerShell Galerisi'nde kullanılabilir modül ile API Sürüm profillerini çalışması için gerekli olan PowerShell cmdlet'leri sağlar.

En son Azure Stack PowerShell modülü ile veya ASDK konak bilgisayara Internet bağlantısı olmadan yükleyebilirsiniz:

> [!IMPORTANT]
> Gerekli sürümü yüklemeden önce emin olun, [tüm mevcut Azure PowerShell modülleri kaldırma](.\.\azure-stack-powershell-install.md#3-uninstall-existing-versions-of-the-azure-stack-powershell-modules).

- **İnternet bağlantısı ile** ASDK ana bilgisayar. Bu modüller, Geliştirme Seti yüklemesine yüklemek için aşağıdaki PowerShell betiğini çalıştırın:

  ``` PowerShell
  # Install the AzureRM.Bootstrapper module. Select Yes when prompted to install NuGet. 
  Install-Module `
    -Name AzureRm.BootStrapper

  # Install and import the API Version Profile required by Azure Stack into the current PowerShell session.
  Use-AzureRmProfile `
    -Profile 2017-03-09-profile -Force

  # Install Azure Stack Module Version 1.4.0. If running a pre-1804 version of Azure Stack, change the -RequiredVersion value to 1.2.11.
  Install-Module -Name AzureStack -RequiredVersion 1.4.0 

  ```

  Yükleme başarılı olursa, AzureRM ve AzureStack modülleri çıktısında görüntülenir.

- **İnternet bağlantısı olmadan** ASDK ana bilgisayar. Bağlantısı kesilmiş bir senaryoda, PowerShell modüllerine aşağıdaki PowerShell komutlarını kullanarak internet bağlantısı olan bir makineye indirmeniz gerekir:

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
  # Install Azure Stack Module Version 1.4.0. If running a pre-1804 version of Azure Stack, change the -RequiredVersion value to 1.2.11.  
    -RequiredVersion 1.4.0
  ```

  Ardından, indirilen paketler ASDK bilgisayara kopyalayın ve varsayılan depo konumu kaydetmek ve bu depodan AzureRM ve AzureStack modüllerini yükleyin:

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

## <a name="download-the-azure-stack-tools"></a>Azure Stack araçları indirin

[AzureStack Araçları](https://github.com/Azure/AzureStack-Tools) yönetme ve dağıtma kaynakları Azure Stack için PowerShell modülleri barındıran bir GitHub deposudur. Bu araçları edinmek için GitHub deposunu kopyalayın veya AzureStack Araçlar klasörüne aşağıdaki komutu çalıştırarak yükleyebilirsiniz:

  ```PowerShell
  # Change directory to the root directory. 
  cd \

  # Enforce usage of TLSv1.2 to download the Azure Stack tools archive from GitHub
  [Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
  invoke-webrequest `
    https://github.com/Azure/AzureStack-Tools/archive/master.zip `
    -OutFile master.zip

  # Expand the downloaded files.
  expand-archive master.zip `
    -DestinationPath . `
    -Force

  # Change to the tools directory.
  cd AzureStack-Tools-master
  ```

## <a name="validate-the-asdk-installation"></a>ASDK yüklemeyi doğrulama
ASDK dağıtımınızın başarılı olmasını sağlamak için aşağıdaki adımları izleyerek Test AzureStack cmdlet'i kullanabilirsiniz:

1. AzureStack\AzureStackAdmin ASDK ana bilgisayarda oturum açın.
2. PowerShell'i yönetici olarak (PowerShell ISE değil) açın.
3. Çalıştırın: `Enter-PSSession -ComputerName AzS-ERCS01 -ConfigurationName PrivilegedEndpoint`
4. Çalıştırın: `Test-AzureStack`

Sınamaların tamamlanması birkaç dakika sürebilir. Yükleme başarılı olduysa, çıktı şuna benzer:

![Test-azurestack](media/asdk-post-deploy/test-azurestack.png)

Bir hata oluştuğunda, Yardım almak için sorun giderme adımlarını izleyin.

## <a name="reset-the-password-expiration-policy"></a>Parola süresi dolma ilkesini Sıfırla 
ASDK dağıttıktan sonra değerlendirme süresi sona ermeden önce Geliştirme Seti konak için parola süresi sona ermiyor emin olmak için aşağıdaki adımları izleyin.

### <a name="to-change-the-password-expiration-policy-from-powershell"></a>Powershell'den parola süresi dolma ilkesini değiştirmek için:
Yükseltilmiş bir Powershell konsolundan komutu çalıştırın:

```powershell
Set-ADDefaultDomainPasswordPolicy -MaxPasswordAge 180.00:00:00 -Identity azurestack.local
```

### <a name="to-change-the-password-expiration-policy-manually"></a>Parola süresi dolma ilkesini el ile olarak değiştirmek için:
1. Geliştirme Seti konakta açın **Grup İlkesi Yönetimi** (GPMC. MMC) gidin **Grup İlkesi Yönetimi** – **orman: azurestack.local** – **etki alanları** – **azurestack.local**.
2. Sağ **varsayılan etki alanı ilkesi** tıklatıp **Düzenle**.
3. Grup İlkesi Yönetimi Düzenleyicisi'nde gidin **Bilgisayar Yapılandırması** – **ilkeleri** – **Windows ayarları** – **güvenlik ayarları**– **Hesap ilkeleri** – **parola ilkesi**.
4. Sağ bölmede **en fazla parola geçerlilik süresi**.
5. İçinde **en fazla parola geçerlilik süresi özellikleri** iletişim kutusunda, değişiklik **parola içinde sona erecek** değerini **180**ve ardından **Tamam**.

![Grup İlkesi Yönetim Konsolu](media/asdk-post-deploy/gpmc.png)


## <a name="next-steps"></a>Sonraki adımlar
[Azure ile ASDK kaydedin](asdk-register.md)
