---
title: "Posta dağıtım yapılandırmaları için Azure yığın Geliştirme Seti'nı (ASDK) | Microsoft Docs"
description: "Azure yığın Geliştirme Seti (ASDK) yükledikten sonra yapmak için önerilen yapılandırma değişiklikleri açıklar."
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/16/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 2183576e87aa2fb31f8be8f676a5aee7d52f68df
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/17/2018
---
# <a name="post-asdk-installation-configuration-tasks"></a>ASDK yükleme sonrası yapılandırma görevleri
Sonra [ASDK yükleme](asdk-install.md), birkaç önerilen yükleme sonrası yapılandırma değişiklikleri yapılan vardır. 

## <a name="install-azure-stack-powershell"></a>Azure Stack PowerShell’i yükleme 
Azure yığın uyumlu Azure PowerShell modülleri Azure yığın ile çalışmak için gereklidir.

Azure yığını için PowerShell komutlarını PowerShell Galerisi aracılığıyla yüklenir. PSGallery depo kaydetmek için yükseltilmiş bir PowerShell oturumu açın ve aşağıdaki komutu çalıştırın:

``` Powershell
Set-PSRepository `
  -Name "PSGallery" `
  -InstallationPolicy Trusted
```

 Azure yığın uyumlu AzureRM modülleri API sürümü profilleri yüklenir. Azure yığın AzureRM.Bootstrapper modülü yükleyerek kullanılabilir 2017-03-09-profili API sürümü profili gerektirir. 
 
 Azure yığın PowerShell ile veya ASDK ana bilgisayara internet bağlantısı olmadan yükleyebilirsiniz:

- **Internet bağlantısı olan** ASDK ana bilgisayardan. Bu modüller, Geliştirme Seti yüklemesine yüklemek için aşağıdaki PowerShell betiğini çalıştırın:

  ``` PowerShell
  # Install the AzureRM.Bootstrapper module. Select Yes when prompted to install NuGet 
  Install-Module `
    -Name AzureRm.BootStrapper

  # Install and import the API Version Profile required by Azure Stack into the current PowerShell session.
  Use-AzureRmProfile `
    -Profile 2017-03-09-profile -Force

  Install-Module `
    -Name AzureStack `
    -RequiredVersion 1.2.11
  ```
  Yükleme başarılı olursa, AzureRM ve AzureStack modülleri çıktısında görüntülenir.

- **İnternet bağlantısı olmadan** ASDK ana bilgisayardan. Bağlantısı kesilmiş bir senaryoda, aşağıdaki PowerShell komutlarını kullanarak Internet bağlantısına sahip bir makine için ilk PowerShell modülleri indirmeniz gerekir:

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
    -RequiredVersion 1.2.11
  ```
  Ardından, indirilen paketler ASDK bilgisayara kopyalayın ve konumu varsayılan deposu olarak kaydetmek ve bu depodan AzureRM ve AzureStack modüllerini yükleyin:

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

## <a name="download-the-azure-stack-tools"></a>Azure yığın araçları yükleyin
[AzureStack Araçları](https://github.com/Azure/AzureStack-Tools) yönetme ve dağıtma kaynakları Azure yığınına için PowerShell modülleri barındıran bir GitHub depo. Bu araçları elde etmek için GitHub deposunu kopyalayın veya AzureStack Araçlar klasörünü aşağıdaki komut dosyasını çalıştırarak yükleyebilirsiniz:

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
ASDK dağıtımınız başarılı olduğundan emin olmak için aşağıdaki adımları izleyerek Test AzureStack cmdlet'i kullanabilirsiniz:

1. AzureStack\CloudAdmin ASDK ana bilgisayarda oturum açın.
2. PowerShell'i yönetici olarak (PowerShell ISE değil) açın.
3. Çalıştırın: `Enter-PSSession -ComputerName AzS-ERCS01 -ConfigurationName PrivilegedEndpoint`
4. Çalıştırın: `Test-AzureStack`

Sınamaların tamamlanması birkaç dakika sürebilir. Yükleme başarılı olduysa, çıktı şöyle görünür:

![test-azurestack](media/asdk-post-deploy/test-azurestack.png)

Hatası varsa, Yardım almak için sorun giderme adımlarını izleyin.

## <a name="activate-the-administrator-and-tenant-portals"></a>Yönetici ve Kiracı portalı etkinleştir
Azure AD kullanan dağıtımlar sonra Azure yığın yönetici ve Kiracı portalı etkinleştirmeniz gerekir. Bu etkinleştirme, Azure yığın portalı ve Azure Resource Manager (onay sayfasında listelenen) doğru izinler tüm kullanıcılar için dizin vermenizi izin.

- Yönetici portalı için gidin https://adminportal.local.azurestack.external/guest/signup, bilgileri okuyun ve ardından **kabul**. Kabul ettikten sonra aynı zamanda dizin Kiracı yönetici olmayan hizmet yöneticileri ekleyebilirsiniz.

- Kiracı portalı için gidin https://portal.local.azurestack.external/guest/signup, bilgileri okuyun ve ardından **kabul**. Kabul ettikten sonra dizindeki kullanıcıları Kiracı portalında oturum açabilir. 

> [!NOTE] 
> Portalları etkin değil, yalnızca dizin yönetici oturum açabilir ve portalları kullanın. Başka bir kullanıcı oturum açtığında bunların yönetici izinleri diğer kullanıcılara vermemiş bildiren bir hata görürsünüz. Yönetici yerel olarak Azure yığın kayıtlı dizinine ait değil, Azure yığın dizin etkinleştirme URL'sine eklenmesi gerekir. Örneğin, Azure yığın fabrikam.onmicrosoft.com ve yönetici kullanıcı için kayıtlı ise admin@contoso.com, gitmek https://portal.local.azurestack.external/guest/signup/fabrikam.onmicrosoft.com Portalı'nı etkinleştirmek için. 

## <a name="reset-the-password-expiration-policy"></a>Parola süre sonu ilkesi Sıfırla 
ASDK dağıttıktan sonra Değerlendirme dönemi sona ermeden önce Geliştirme Seti ana bilgisayar için parola süresi sona ermiyor emin olmak için aşağıdaki adımları izleyin.

### <a name="to-change-the-password-expiration-policy-from-powershell"></a>Parola süre sonu ilkesi Powershell'den değiştirmek için:
Yükseltilmiş bir Powershell konsolundan komutu çalıştırın:

```powershell
Set-ADDefaultDomainPasswordPolicy -MaxPasswordAge 180.00:00:00 -Identity azurestack.local
```

### <a name="to-change-the-password-expiration-policy-manually"></a>Parola süre sonu ilkesi el ile değiştirmek için:
1. Geliştirme Seti konakta açmak **Grup İlkesi Yönetimi** (GPMC. MMC) ve gidin **Grup İlkesi Yönetimi** – **orman: azurestack.local** – **etki alanları** – **azurestack.local**.
2. Sağ **varsayılan etki alanı ilkesi** tıklatıp **Düzenle**.
3. Grup İlkesi Yönetimi Düzenleyicisi'nde gidin **Bilgisayar Yapılandırması** – **ilkeleri** – **Windows ayarları** – **güvenlik ayarları**– **Hesap ilkeleri** – **parola ilkesi**.
4. Sağ bölmede, çift **en uzun parola geçerlilik süresi**.
5. İçinde **en uzun parola geçerlilik süresi özellikleri** iletişim kutusu, değişiklik **parola içinde dolacak** değeri **180**ve ardından **Tamam**.

![Grup İlkesi Yönetim Konsolu](media/asdk-post-deploy/gpmc.png)


## <a name="next-steps"></a>Sonraki adımlar
[Azure ile ASDK kaydedin](asdk-register.md)
