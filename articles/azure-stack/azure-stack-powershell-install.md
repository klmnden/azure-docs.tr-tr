---
title: "PowerShell için Azure yığın yükleme | Microsoft Docs"
description: "PowerShell için Azure yığınına yüklemeyi öğrenin."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 0CDD8B9B-EC32-4453-918A-D0A606C7BC10
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/13/2017
ms.author: mabrigg
ms.openlocfilehash: b44129400e878e9032623e4d0962153d50303660
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="install-powershell-for-azure-stack"></a>PowerShell için Azure yığın yükle  

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Azure yığın uyumlu Azure PowerShell modülleri Azure yığın ile çalışmak için gereklidir. Bu kılavuzda, biz, PowerShell için Azure yığınına yüklemek için gereken adımlarda size yol. Bu makalede Azure yığın Geliştirme Setinden veya Windows tabanlı bir dış istemci VPN üzerinden bağlandığı sırada açıklanan adımları kullanabilirsiniz.

Bu makalede Azure yığını için PowerShell yüklemek için yönergeleri açıklanmıştır. Ancak, hızlı bir şekilde yüklemek ve PowerShell yapılandırmak istiyorsanız, sağlanan komut dosyasını kullanabilirsiniz [PowerShell ile başlamak ve çalıştırmak](azure-stack-powershell-configure-quickstart.md) konu. 

> [!NOTE]
> Aşağıdaki adımları PowerShell 5.0 gerektirir. Sürümünüzü denetlemek için $PSVersionTable.PSVersion çalıştırın ve "Ana" sürümüne karşılaştırın.

Azure yığını için PowerShell komutlarını PowerShell Galerisi aracılığıyla yüklenir. VPN üzerinden bağlı ve aşağıdaki komutu çalıştırırsanız regiser PSGallery deposu için Geliştirme Seti ya da Windows tabanlı bir dış istemci yükseltilmiş bir PowerShell oturumu açın:

```powershell
Set-PSRepository `
  -Name "PSGallery" `
  -InstallationPolicy Trusted
```

## <a name="uninstall-existing-versions-of-powershell"></a>PowerShell varolan sürümlerini kaldırın

Gerekli sürümü yüklemeden önce tüm mevcut Azure PowerShell modülleri kaldırmak emin olun. Bunları, aşağıdaki iki yöntemden birini kullanarak kaldırabilirsiniz:

* VPN bağlantısı kuracak planlıyorsanız, varolan PowerShell modülleri kaldırmak için Geliştirme Seti ya da Windows tabanlı bir dış istemci oturum açın. Tüm etkin PowerShell oturumları kapatın ve aşağıdaki komutu çalıştırın: 

   ```powershell
   Get-Module -ListAvailable | where-Object {$_.Name -like “Azure*”} | Uninstall-Module
   ```

* VPN bağlantısı kuracak planlıyorsanız, Geliştirme Seti ya da Windows tabanlı bir dış istemci oturum açın. "Azure" ile başlayan tüm klasörleri silin `C:\Program Files (x86)\WindowsPowerShell\Modules` ve `C:\Users\AzureStackAdmin\Documents\WindowsPowerShell\Modules` klasörler. Bu klasörlerin silinmesi, tüm mevcut PowerShell modülleri "AzureStackAdmin" ve "Genel" kullanıcı kapsamlarından kaldırır. 

Aşağıdaki bölümlerde Azure yığını için PowerShell yüklemek için gereken adımlar açıklanmaktadır. PowerShell Azure yığında içinde çalıştırılır bağlı, kısmen bağlı veya bağlantısı kesilmiş bir senaryoda yüklenebilir. 

## <a name="install-powershell-in-a-connected-scenario-with-internet-connectivity"></a>PowerShell bağlı bir senaryoda (internet bağlantısı ile) yükleyin.

Azure yığın uyumlu AzureRM modülleri API sürümü profilleri yüklenir. Azure yığın gerektirir **2017-03-09-profili** AzureRM.Bootstrapper modülü yükleyerek kullanılabilir API sürümü profili. API sürümü profilleri ve onlar tarafından sağlanan cmdlet'leri hakkında bilgi edinmek için bkz [API sürümü profillerini yönetmek](azure-stack-version-profiles.md). AzureRM modülleri yanı sıra Azure yığın özgü PowerShell modülleri de yüklemeniz gerekir. Bu modüller, geliştirme iş istasyonunda yüklemek için aşağıdaki PowerShell betiğini çalıştırın:

> [!IMPORTANT]
> AzureRM 1.2.11 PowerShell modülü sürümü yeni değişiklikler ile ilgili bir listesi bulunur. 1.2.10 yükseltme sürümü, geçiş Guide'a bakın [https://aka.ms/azspowershellmigration](https://aka.ms/azspowershellmigration).

  ```powershell
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

Yükleme işlemini onaylamak için aşağıdaki komutu çalıştırın:

  ```powershell
  Get-Module `
    -ListAvailable | where-Object {$_.Name -like “Azure*”}
  ```
  Yükleme başarılı olursa, AzureRM ve AzureStack modülleri çıktısında görüntülenir.

## <a name="install-powershell-in-a-disconnected-or-a-partially-connected-scenario-with-limited-internet-connectivity"></a>PowerShell bir bağlantısı kesilmiş veya kısmen bağlı bir senaryo (sınırlı internet bağlantısı ile) yükleyin

Bağlantısı kesilmiş bir senaryoda, PowerShell modülleri Internet bağlantısına sahip bir makine için ilk indirme ve yükleme için Azure yığın Geliştirme Seti için Aktarım gerekir.

> [!IMPORTANT]
> AzureRM 1.2.11 PowerShell modülü sürümü yeni değişiklikler ile ilgili bir listesi bulunur. 1.2.10 yükseltme sürümü, geçiş Guide'a bakın [https://aka.ms/azspowershellmigration](https://aka.ms/azspowershellmigration).

1. Burada Internet bağlantısına sahip ve aşağıdaki komut dosyasını indirme AzureRM ve AzureStack paketleri için yerel bilgisayarınıza kullanın bir bilgisayarda oturum açın:

   ```powershell
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

2. İndirilen paketler üzerinden bir USB aygıtı kopyalayın.

3. Geliştirme Seti için oturum açın ve paketleri USB aygıtından Geliştirme Seti üzerinde bir konuma kopyalayın. 

4. Şimdi bu konumu varsayılan deposu olarak kaydetmek ve bu depodan AzureRM ve AzureStack modüllerini yükleyin:

   ```powershell
   $SourceLocation = "<Location on the development kit that contains the PowerShell packages>"
   $RepoName = "MyNuGetSource"

   Register-PSRepository `
     -Name $RepoName `
     -SourceLocation $SourceLocation `
     -InstallationPolicy Trusted

   ```powershell
   Install-Module AzureRM `
     -Repository $RepoName

   Install-Module AzureStack `
     -Repository $RepoName 
   ```

## <a name="next-steps"></a>Sonraki adımlar

* [Azure yığın araçları Github'dan yükleyin](azure-stack-powershell-download.md)
* [Azure yığın kullanıcının PowerShell ortamını yapılandırma](user/azure-stack-powershell-configure-user.md)  
* [Azure yığın işlecin PowerShell ortamını yapılandırma](azure-stack-powershell-configure-admin.md) 
* [Azure yığınında API sürümü profillerini yönet](azure-stack-version-profiles.md)  
