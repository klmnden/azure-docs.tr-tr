---
title: SQL veritabanları Azure yığında kullanarak | Microsoft Docs
description: SQL veritabanları Azure yığını ve hızlı adımlar SQL Server Kaynak sağlayıcısı bağdaştırıcısı dağıtmak için bir hizmet olarak nasıl dağıtabileceğini öğrenin.
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
ms.date: 05/01/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: 9154f509f9019c28515970869678aa6633d16163
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/04/2018
---
# <a name="update-the-sql-resource-provider-adapter"></a>SQL kaynak sağlayıcısı bağdaştırıcısını güncelleştir
Azure yığın derlemeleri güncelleştirildiğinde yeni bir SQL kaynak sağlayıcısı bağdaştırıcısı yayımlanan. Varolan bağdaştırıcısı çalışmaya devam ederken, en son sürüme mümkün olan en kısa sürede güncelleştirilmesi önerilir. Güncelleştirmeleri sırayla yüklü olmalıdır: sürümleri atlayamazsınız (sürümleri listesinde bkz [kaynak sağlayıcı önkoşulları dağıtma](.\azure-stack-sql-resource-provider-deploy.md#prerequisites)).

Kullandığınız kaynak sağlayıcısı güncelleştirmek için *UpdateSQLProvider.ps1* komut dosyası. İşlem bölümünde açıklandığı gibi bir kaynak Sağlayıcısı'nı yüklemek için kullanılan işlem benzer [kaynak sağlayıcısı dağıtmak](.\azure-stack-sql-resource-provider-deploy.md) makalesi. Betik kaynak sağlayıcısı yükleme ile dahil edilir.

*UpdateSQLProvider.ps1* komut dosyası en son kaynak sağlayıcısı kodu ile yeni bir VM oluşturur ve yeni VM'ye eski sanal makineden ayarları geçirir. Geçiş ayarları veritabanı ve barındırma sunucusu bilgilerini içerir ve gerekli DNS kaydı.

Komut dosyası için DeploySqlProvider.ps1 komut açıklanan aynı bağımsız değişkenlere kullanılmasını gerektirir. Sertifika burada da sağlar. 

Market Yönetimi'nden en son Windows Server 2016 Core görüntüyü indirmeyi öneririz. Bir güncelleştirme yüklemeniz gerekiyorsa, tek bir yerleştirebilirsiniz. Yerel bağımlılık yolundaki MSU paketi. Birden fazla ise. MSU dosyası bulundu, komut dosyası başarısız olur.

Aşağıdaki örneği verilmiştir *UpdateSQLProvider.ps1* PowerShell isteminden çalıştırıp komut dosyası. Hesap bilgileri ve gerektiğinde parolaları değiştirdiğinizden emin olun: 

> [!NOTE]
> Güncelleştirme işlemi yalnızca tümleşik sistemleri için geçerlidir.

```powershell
# Install the AzureRM.Bootstrapper module, set the profile, and install the AzureRM and AzureStack modules.
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2017-03-09-profile
Install-Module -Name AzureStack -RequiredVersion 1.2.11 -Force

# Use the NetBIOS name for the Azure Stack domain. On the Azure Stack SDK, the default is AzureStack but could have been changed at install time.
$domain = "AzureStack"

# For integrated systems, use the IP address of one of the ERCS virtual machines
$privilegedEndpoint = "AzS-ERCS01"

# Point to the directory where the resource provider installation files were extracted.
$tempDir = 'C:\TEMP\SQLRP'

# The service admin account (can be Azure AD or AD FS).
$serviceAdmin = "admin@mydomain.onmicrosoft.com"
$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass)

# Set credentials for the new Resource Provider VM.
$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("sqlrpadmin", $vmLocalAdminPass)

# And the cloudadmin credential required for privileged endpoint access.
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
  -DependencyFilesLocalPath $tempDir\cert
 ```

## <a name="updatesqlproviderps1-parameters"></a>UpdateSQLProvider.ps1 parametreleri
Komut satırında bu parametreleri belirtebilirsiniz. Bunu yapmazsanız veya hiçbir parametre doğrulaması başarısız olursa, gerekli parametreler sağlamanız istenir.

| Parametre adı | Açıklama | Açıklama veya varsayılan değer |
| --- | --- | --- |
| **CloudAdminCredential** | Bulut Yöneticisi, ayrıcalıklı endpoint erişmek için gerekli kimlik bilgileri. | _Gerekli_ |
| **AzCredential** | Azure yığın hizmet yönetici hesabı için kimlik bilgileri. Azure yığın dağıtmak için kullanılan kimlik bilgilerini kullanın. | _Gerekli_ |
| **VMLocalCredential** | SQL kaynak sağlayıcısının VM yerel yönetici hesabı için kimlik bilgileri. | _Gerekli_ |
| **PrivilegedEndpoint** | IP adresi veya ayrıcalıklı uç noktanın DNS adı. |  _Gerekli_ |
| **DependencyFilesLocalPath** | Sertifika .pfx dosyasının bu dizinde yerleştirilmelidir. | _İsteğe bağlı_ (_zorunlu_ çok düğümlü için) |
| **DefaultSSLCertificatePassword** | .Pfx sertifika parolası. | _Gerekli_ |
| **MaxRetryCount** | Bir hata olduğunda her işlemini yeniden denemek istiyor sayısı.| 2 |
| **RetryDuration** |Saniye cinsinden yeniden denemeler arasındaki zaman aşımı aralığı. | 120 |
| **Kaldırma** | (Aşağıdaki notlara bakın) ilişkili tüm kaynakları ve kaynak sağlayıcısını kaldırır. | Hayır |
| **DebugMode** | Otomatik temizleme hatasında engeller. | Hayır |


## <a name="next-steps"></a>Sonraki adımlar

[SQL kaynak sağlayıcısı koru](azure-stack-sql-resource-provider-maintain.md)
