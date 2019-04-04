---
title: Azure Stack SQL kaynak sağlayıcısı güncelleştiriliyor | Microsoft Docs
description: Azure Stack SQL kaynak sağlayıcısı nasıl güncelleştirme hakkında bilgi edinin.
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
ms.date: 03/29/2019
ms.author: jeffgilb
ms.reviewer: jiahan
ms.lastreviewed: 03/11/2019
ms.openlocfilehash: 5238c60493820fe6d784049da9862b4347e563c4
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58650918"
---
# <a name="update-the-sql-resource-provider"></a>SQL kaynak sağlayıcısını güncelle

*Uygulama hedefi: Azure Stack tümleşik sistemleri.*

Yeni bir SQL kaynak sağlayıcısı, Azure Stack için yeni bir derleme güncelleştirildiğinde yayımlanan. Mevcut kaynak sağlayıcısını çalışmaya devam eder, ancak için en son sürüme mümkün olan en kısa sürede güncelleştirilmesi öneririz. 

SQL kaynak sağlayıcısı sürüm 1.1.33.0 sürümünden başlayarak, güncelleştirmeleri birikmeli özelliktedir ve yayımlanmış olan yüklü olması gerekmez; Başlangıç 1.1.24.0 sürümünden veya üzeri olduğu sürece. SQL kaynak sağlayıcısı 1.1.24.0 sürümünü çalıştırıyorsanız, örneğin, ardından 1.1.33.0 sürümüne veya daha sonra ilk sürümünü 1.1.30.0 yüklemeye gerek olmadan yükseltebilirsiniz. Kullanılabilir kaynak sağlayıcısı sürümleri ve üzerinde desteklenir, Azure Stack sürümünü gözden geçirmek için sürümleri listesinde bakın [kaynak sağlayıcı önkoşulları dağıtma](./azure-stack-sql-resource-provider-deploy.md#prerequisites).

Kaynak Sağlayıcısı'nı güncelleştirmek için *UpdateSQLProvider.ps1* betiği. Bu betik yeni SQL kaynak Sağlayıcısı'nin karşıdan yüklemesiyle dahil edilir. Güncelleştirme işlemi için kullanılan işleme benzer [kaynak sağlayıcısı dağıtma](./azure-stack-sql-resource-provider-deploy.md). Güncelleştirme betiğini DeploySqlProvider.ps1 komut dosyası olarak aynı bağımsız değişkenleri kullanır ve sertifika bilgilerini sağlamanız gerekir.

 > [!IMPORTANT]
 > Kaynak Sağlayıcısı'nı yükseltmeden önce yeni işlevler, düzeltmeler ve dağıtımınızı etkileyebilecek bilinen sorunlar hakkında bilgi edinmek için sürüm notlarını gözden geçirin.

## <a name="update-script-processes"></a>Güncelleştirme betik işlemleri

*UpdateSQLProvider.ps1* betik en son kaynak sağlayıcı kodu ile yeni bir sanal makine (VM) oluşturur.

> [!NOTE]
> En son Windows Server 2016 Core görüntüyü Market Yönetimi'nden indirmenizi öneririz. Bir güncelleştirme yüklemeniz gerekiyorsa, yerleştirebilirsiniz bir **tek** MSU paketinde bağımlılık yerel yolu. Bu konumda birden fazla MSU dosyası yoksa, betiği başarısız olur.

Sonra *UpdateSQLProvider.ps1* betik yeni bir VM oluşturur, betik aşağıdaki ayarları eski sağlayıcısından VM geçirir:

* veritabanı bilgileri
* barındırma sunucusu bilgileri
* gerekli DNS kaydı

## <a name="update-script-parameters"></a>Betik parametreleri güncelleştirin

Programını çalıştırdığınızda, komut satırından aşağıdaki parametreleri belirtebilirsiniz **UpdateSQLProvider.ps1** PowerShell Betiği. Yoksa veya herhangi bir parametre doğrulaması başarısız olursa, gerekli parametreleri sağlamanız istenir.

| Parametre adı | Açıklama | Açıklama veya varsayılan değeri |
| --- | --- | --- |
| **CloudAdminCredential** | Ayrıcalıklı uç nokta erişmek için gerekli bulut Yöneticisi kimlik bilgileri. | _Gerekli_ |
| **AzCredential** | Azure Stack hizmet yönetici hesabının kimlik bilgileri. Azure Stack dağıtmak için kullanılan kimlik bilgilerini kullanın. | _Gerekli_ |
| **VMLocalCredential** | SQL kaynak sağlayıcısı VM yerel yönetici hesabı için kimlik bilgileri. | _Gerekli_ |
| **PrivilegedEndpoint** | Ayrıcalıklı uç noktasının DNS adı veya IP adresi. |  _Gerekli_ |
| **AzureEnvironment** | Azure Stack dağıtmak için kullanılan hizmet yönetici hesabının Azure ortamı. Yalnızca Azure AD dağıtımları için gereklidir. Desteklenen ortam adları **AzureCloud**, **AzureUSGovernment**, veya bir Azure AD, Çin kullanıyorsanız **AzureChinaCloud**. | AzureCloud |
| **DependencyFilesLocalPath** | Ayrıca, sertifika .pfx dosyasını bu dizinde yerleştirmeniz gerekir. | _Çok düğümlü zorunlu ancak tek bir düğüm için isteğe bağlı_ |
| **DefaultSSLCertificatePassword** | .Pfx sertifika için parola. | _Gerekli_ |
| **MaxRetryCount** | Her işlem bir hata olursa yeniden denemek istiyor sayısı.| 2 |
| **RetryDuration** |Saniye cinsinden yeniden denemeler arasındaki zaman aşımı aralığı. | 120 |
| **Kaldırma** | Kaynak sağlayıcısı ile ilişkili tüm kaynakları kaldırır. | Hayır |
| **DebugMode** | Otomatik temizleme başarısız engeller. | Hayır |

## <a name="update-script-powershell-example"></a>PowerShell örnek betiği güncelleştir
Kullanımının bir örneği verilmiştir *UpdateSQLProvider.ps1* yükseltilmiş bir PowerShell konsolundan çalıştırarak betiği. Değişken bilgileri ve parolaları gerektiği şekilde değiştirdiğinizden emin olun:  

> [!NOTE]
> Bu güncelleştirme işlemi, yalnızca Azure Stack tümleşik sistemleri için geçerlidir.

```powershell
# Install the AzureRM.Bootstrapper module, set the profile and install the AzureStack module
# Note that this might not be the most currently available version of Azure Stack PowerShell.
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2018-03-01-hybrid -Force
Install-Module -Name AzureStack -RequiredVersion 1.6.0

# Use the NetBIOS name for the Azure Stack domain. On the Azure Stack SDK, the default is AzureStack but this might have been changed at installation.
$domain = "AzureStack"

# For integrated systems, use the IP address of one of the ERCS virtual machines.
$privilegedEndpoint = "AzS-ERCS01"

# Provide the Azure environment used for deploying Azure Stack. Required only for Azure AD deployments. Supported values for the <environment name> parameter are AzureCloud, AzureChinaCloud or AzureUSGovernment depending which Azure subscription you are using. 
$AzureEnvironment = "<EnvironmentName>"

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
  -AzureEnvironment $AzureEnvironment `
  -DefaultSSLCertificatePassword $PfxPass `
  -DependencyFilesLocalPath $tempDir\cert 

 ```

## <a name="next-steps"></a>Sonraki adımlar

[SQL kaynak sağlayıcısı koru](azure-stack-sql-resource-provider-maintain.md)
