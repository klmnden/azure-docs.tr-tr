---
title: Azure yığın SQL kaynak sağlayıcısı güncelleştirme | Microsoft Docs
description: Azure yığın SQL kaynak sağlayıcısı nasıl güncelleştirebileceğinizi öğrenin.
services: azure-stack
documentationCenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/11/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: ac5073d1abc32b7598a869750f9c5a801559e9e6
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36264086"
---
# <a name="update-the-sql-resource-provider"></a>SQL kaynak sağlayıcısını güncelle

*Uygulandığı öğe: Azure yığın tümleşik sistemler.*

Yeni bir derleme Azure yığın güncelleştirildiğinde yeni bir SQL kaynak sağlayıcısı yayımlanan. Varolan bağdaştırıcısı çalışmaya devam eder, ancak en son sürüme mümkün olan en kısa sürede güncelleştirme öneririz.

>[!IMPORTANT]
>Bunlar serbest sırayla güncelleştirmeleri yüklemeniz gerekir. Sürümleri atlayamazsınız. Sürümleri listesinde başvurmak [kaynak sağlayıcı önkoşulları dağıtma](.\azure-stack-sql-resource-provider-deploy.md#prerequisites).

## <a name="overview"></a>Genel Bakış

Kaynak sağlayıcısı güncelleştirmek için *UpdateSQLProvider.ps1* komut dosyası. Bu komut dosyasını yeni SQL kaynak sağlayıcısı yükleme ile dahil edilir. Güncelleştirme işlemi için kullanılan işlem benzer [kaynak sağlayıcısı dağıtmak](.\azure-stack-sql-resource-provider-deploy.md). Güncelleştirme betiğini DeploySqlProvider.ps1 komut dosyası olarak aynı bağımsız değişkenlere kullanır ve sertifika bilgilerini sağlamanız gerekir.

### <a name="update-script-processes"></a>Güncelleştirme komut dosyası işlemleri

*UpdateSQLProvider.ps1* komut dosyası en son kaynak sağlayıcısı kodu ile yeni bir sanal makine (VM) oluşturur.

>[!NOTE]
>Market Yönetimi'nden en son Windows Server 2016 Core görüntüyü indirmeyi öneririz. Bir güncelleştirme yüklemeniz gerekiyorsa, yerleştirebilirsiniz bir **tek** yerel bağımlılık yolu MSU paketinde. Bu konumda birden fazla MSU dosyası ise, komut dosyası başarısız olur.

Sonra *UpdateSQLProvider.ps1* komut dosyasını yeni bir VM oluşturur, komut dosyasını aşağıdaki ayarları eski sağlayıcıdan VM geçirir:

* Veritabanı bilgileri
* barındırma sunucusu bilgileri
* gerekli DNS kaydı

### <a name="update-script-powershell-example"></a>Komut dosyası PowerShell örnek güncelleştir

Düzenle ve yükseltilmiş bir PowerShell ISE aşağıdaki betiği çalıştırın. Ortamınız için gerektiği gibi parola ve hesap bilgilerini değiştirmeyi unutmayın.

> [!NOTE]
> Bu güncelleştirme işlemi yalnızca Azure tümleşik yığını sistemleri için geçerlidir.

```powershell
# Install the AzureRM.Bootstrapper module and set the profile.
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2017-03-09-profile

# Use the NetBIOS name for the Azure Stack domain. On the Azure Stack SDK, the default is AzureStack but this might have been changed at installation.
$domain = "AzureStack"

# For integrated systems, use the IP address of one of the ERCS virtual machines.
$privilegedEndpoint = "AzS-ERCS01"

# Point to the directory where the resource provider installation files were extracted.
$tempDir = 'C:\TEMP\SQLRP'

# The service administrator account (this can be Azure AD or AD FS).
$serviceAdmin = "admin@mydomain.onmicrosoft.com"
$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass)

# Set the credentials for the new resource provider VM.
$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("sqlrpadmin", $vmLocalAdminPass)

# Add the cloudadmin credential required for privileged endpoint access.
$CloudAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$CloudAdminCreds = New-Object System.Management.Automation.PSCredential ("$domain\cloudadmin", $CloudAdminPass)

# Change the following as appropriate.
$PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force

# Change directory to the folder where you extracted the installation files.
# Then adjust the endpoints.
. $tempDir\UpdateSQLProvider.ps1 -AzCredential $AdminCreds `
  -VMLocalCredential $vmLocalAdminCreds `
  -CloudAdminCredential $cloudAdminCreds `
  -PrivilegedEndpoint $privilegedEndpoint `
  -DefaultSSLCertificatePassword $PfxPass `
  -DependencyFilesLocalPath $tempDir\cert `

 ```

## <a name="updatesqlproviderps1-parameters"></a>UpdateSQLProvider.ps1 parametreleri

Komut dosyasını çalıştırdığınızda, komut satırından aşağıdaki parametreleri belirtebilirsiniz. Bunu yapmazsanız veya hiçbir parametre doğrulaması başarısız olursa, gerekli parametreler sağlamanız istenir.

| Parametre adı | Açıklama | Açıklama veya varsayılan değer |
| --- | --- | --- |
| **CloudAdminCredential** | Bulut Yöneticisi, ayrıcalıklı endpoint erişmek için gerekli kimlik bilgileri. | _Gerekli_ |
| **AzCredential** | Azure yığın hizmet yönetici hesabı için kimlik bilgileri. Azure yığın dağıtmak için kullanılan kimlik bilgilerini kullanın. | _Gerekli_ |
| **VMLocalCredential** | SQL kaynak sağlayıcısının VM yerel yönetici hesabı için kimlik bilgileri. | _Gerekli_ |
| **PrivilegedEndpoint** | IP adresi veya ayrıcalıklı uç noktanın DNS adı. |  _Gerekli_ |
| **DependencyFilesLocalPath** | Ayrıca, bu dizinde sertifika .pfx dosyanızı konulmalıdır. | _Tek düğüm, ancak çok düğümlü için zorunlu için isteğe bağlı_ |
| **DefaultSSLCertificatePassword** | .Pfx sertifika parolası. | _Gerekli_ |
| **MaxRetryCount** | Bir hata olduğunda her işlemini yeniden denemek istiyor sayısı.| 2 |
| **RetryDuration** |Saniye cinsinden yeniden denemeler arasındaki zaman aşımı aralığı. | 120 |
| **Kaldırma** | İlişkili tüm kaynakları ve kaynak sağlayıcısını kaldırır. | Hayır |
| **DebugMode** | Otomatik temizleme hatasında engeller. | Hayır |

## <a name="next-steps"></a>Sonraki adımlar

[SQL kaynak sağlayıcısı koru](azure-stack-sql-resource-provider-maintain.md)
