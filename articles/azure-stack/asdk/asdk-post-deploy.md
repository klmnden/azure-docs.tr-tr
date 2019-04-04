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
ms.date: 02/15/2019
ms.author: jeffgilb
ms.reviewer: misainat
ms.lastreviewed: 10/10/2018
ms.openlocfilehash: 7699c9279138aedfdcfe63fb42e65ad102ac92c9
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58652476"
---
# <a name="post-asdk-installation-configuration-tasks"></a>ASDK yükleme sonrası yapılandırma görevleri

Sonra [Azure Stack geliştirme Seti'ni (ASDK) yükleme](asdk-install.md), ASDK ana bilgisayarda AzureStack\AzureStackAdmin oturumunuz açıkken birkaç önerilen yükleme sonrası yapılandırma değişiklikleri yapmak. 

## <a name="install-azure-stack-powershell"></a>Azure Stack PowerShell’i yükleme

Azure Stack uyumlu Azure PowerShell modülleri, Azure Stack ile çalışmak için gereklidir.

Azure Stack için PowerShell komutları PowerShell Galerisi'nde yüklenir. PSGallery depo kaydetmek için yükseltilmiş bir PowerShell oturumu açın ve aşağıdaki komutu çalıştırın:

``` Powershell
Set-PSRepository -Name "PSGallery" -InstallationPolicy Trusted
```

Azure Stack uyumlu AzureRM modülleri belirtmek için API sürümü profillerini kullanabilirsiniz.  API sürümü profillerini Azure ve Azure Stack arasında sürümü farkları yönetmek için bir yol sağlar. Bir API Sürüm profili, AzureRM PowerShell modülleri belirli API sürümleri ile kümesidir. **AzureRM.Bootstrapper** PowerShell Galerisi'nde kullanılabilir modül ile API Sürüm profillerini çalışması için gerekli olan PowerShell cmdlet'leri sağlar.

En son Azure Stack PowerShell modülü ile veya ASDK konak bilgisayara Internet bağlantısı olmadan yükleyebilirsiniz:

> [!IMPORTANT]
> Gerekli sürümü yüklemeden önce emin olun, [tüm mevcut Azure PowerShell modülleri kaldırma](../azure-stack-powershell-install.md#3-uninstall-existing-versions-of-the-azure-stack-powershell-modules).

- **İnternet bağlantısı ile** ASDK ana bilgisayar. Bu modüller, Geliştirme Seti yüklemesine yüklemek için aşağıdaki PowerShell betiğini çalıştırın:

- Azure Stack 1901 veya sonraki bir sürümü:

    ```powershell
    # Install and import the API Version Profile required by Azure Stack into the current PowerShell session.
    Install-Module AzureRM -RequiredVersion 2.4.0
    Install-Module -Name AzureStack -RequiredVersion 1.7.1
    ```

    > [!Note]  
    > Azure Stack modülü sürüm 1.7.1 bölünmesi farklıdır. Azure yığını geçirme 1.6.0 Lütfen başvurmak için [Geçiş Kılavuzu](https://aka.ms/azspshmigration171).

  - Azure Stack 1811:

    ``` PowerShell
    # Install the AzureRM.Bootstrapper module. Select Yes when prompted to install NuGet. 
    Install-Module -Name AzureRm.BootStrapper

    # Install and import the API Version Profile required by Azure Stack into the current PowerShell session.
    Use-AzureRmProfile -Profile 2018-03-01-hybrid -Force

    # Install Azure Stack Module Version 1.6.0.
    Install-Module -Name AzureStack -RequiredVersion 1.6.0
    ```

  - Azure Stack 1810 veya önceki bir sürümü:

    ``` PowerShell
    # Install the AzureRM.Bootstrapper module. Select Yes when prompted to install NuGet. 
    Install-Module -Name AzureRm.BootStrapper

    # Install and import the API Version Profile required by Azure Stack into the current PowerShell session.
    Use-AzureRmProfile -Profile 2018-03-01-hybrid -Force

    # Install Azure Stack Module Version 1.5.0.
    Install-Module -Name AzureStack -RequiredVersion 1.5.0
    ```

  Yükleme başarılı olursa, AzureRM ve AzureStack modülleri çıktısında görüntülenir.

- **İnternet bağlantısı olmadan** ASDK ana bilgisayar. Bağlantısı kesilmiş bir senaryoda, PowerShell modüllerine aşağıdaki PowerShell komutlarını kullanarak internet bağlantısı olan bir makineye indirmeniz gerekir:

  ```powershell
  $Path = "<Path that is used to save the packages>"

  Save-Package `
    -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureRM -Path $Path -Force -RequiredVersion 2.3.0
  
  Save-Package `
    -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureStack -Path $Path -Force -RequiredVersion 1.5.0
  ```

  Ardından, indirilen paketler ASDK bilgisayara kopyalayın ve varsayılan depo konumu kaydetmek ve bu depodan AzureRM ve AzureStack modüllerini yükleyin:

    ```powershell  
    $SourceLocation = "<Location on the development kit that contains the PowerShell packages>"
    $RepoName = "MyNuGetSource"

    Register-PSRepository -Name $RepoName -SourceLocation $SourceLocation -InstallationPolicy Trusted

    Install-Module AzureRM -Repository $RepoName

    Install-Module AzureStack -Repository $RepoName
    ```

## <a name="download-the-azure-stack-tools"></a>Azure Stack araçları indirin

[AzureStack Araçları](https://github.com/Azure/AzureStack-Tools) yönetme ve dağıtma kaynakları Azure Stack için PowerShell modülleri barındıran bir GitHub deposudur. Bu araçları edinmek için GitHub deposunu kopyalayın veya AzureStack Araçlar klasörüne aşağıdaki komutu çalıştırarak yükleyebilirsiniz:

  ```powershell
  # Change directory to the root directory.
  cd \

  # Enforce usage of TLSv1.2 to download the Azure Stack tools archive from GitHub
  [Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
  invoke-webrequest `
    https://github.com/Azure/AzureStack-Tools/archive/master.zip `
    -OutFile master.zip

  # Expand the downloaded files.
  expand-archive master.zip -DestinationPath . -Force

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

### <a name="to-change-the-password-expiration-policy-from-powershell"></a>Powershell'den parola süresi dolma ilkesini değiştirmek için

Yükseltilmiş bir Powershell konsolundan komutu çalıştırın:

```powershell
Set-ADDefaultDomainPasswordPolicy -MaxPasswordAge 180.00:00:00 -Identity azurestack.local
```

### <a name="to-change-the-password-expiration-policy-manually"></a>Parola süresi dolma ilkesini el ile olarak değiştirmek için

1. Geliştirme Seti konakta açın **Grup İlkesi Yönetimi** (GPMC. MMC) gidin **Grup İlkesi Yönetimi** – **orman: azurestack.local** – **etki alanları** – **azurestack.local**.
2. Sağ **varsayılan etki alanı ilkesi** tıklatıp **Düzenle**.
3. Grup İlkesi Yönetimi Düzenleyicisi'nde gidin **Bilgisayar Yapılandırması** – **ilkeleri** – **Windows ayarları** – **güvenlik ayarları**– **Hesap ilkeleri** – **parola ilkesi**.
4. Sağ bölmede **en fazla parola geçerlilik süresi**.
5. İçinde **en fazla parola geçerlilik süresi özellikleri** iletişim kutusunda, değişiklik **parola içinde sona erecek** değerini **180**ve ardından **Tamam**.

![Grup İlkesi Yönetim Konsolu](media/asdk-post-deploy/gpmc.png)

## <a name="enable-multi-tenancy"></a>Çok kiracılı modeli etkinleştirme

Azure AD kullanarak dağıtımları için şunları yapmanız [çok kiracılı modeli etkinleştirme](../azure-stack-enable-multitenancy.md#enable-multi-tenancy) ASDK yüklemenizin.

> [!NOTE]
> Yönetici veya kullanıcı hesaplarından farklı Azure Stack kaydetmek için kullanılan etki alanları, bir Azure Stack Portal'da oturum açmak için kullanıldığında, Azure Stack kaydetmek için kullanılan etki alanı adı portala eklenmesi gereken url. Örneğin, Azure Stack ile Fabrikam.onmicrosoft.com adresli kayıtlı olup olmadığını ve oturum açma kullanıcı hesabı olan admin@contoso.com, kullanıcı portalında oturum açmak için kullanılacak url şu şekilde olacaktır: https://portal.local.azurestack.external/fabrikam.onmicrosoft.com.

## <a name="next-steps"></a>Sonraki adımlar

[Azure ile ASDK kaydedin](asdk-register.md)
