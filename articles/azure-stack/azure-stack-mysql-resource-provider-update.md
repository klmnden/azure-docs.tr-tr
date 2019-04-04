---
title: Azure Stack MySQL kaynak sağlayıcısı güncelleştiriliyor | Microsoft Docs
description: Azure Stack MySQL kaynak sağlayıcısı nasıl güncelleştirme hakkında bilgi edinin.
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
ms.lastreviewed: 01/11/2019
ms.openlocfilehash: 976c05449704b0ecbc48ee5bd4799f300fcac0aa
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58651694"
---
# <a name="update-the-mysql-resource-provider"></a>MySQL kaynak sağlayıcısını güncelle 

*Uygulama hedefi: Azure Stack tümleşik sistemleri.*

Azure Stack oluşturduğunda kaynak sağlayıcısı bağdaştırıcısını serbest yeni bir MySQL güncelleştirilir. Mevcut bağdaştırıcısı çalışmaya devam ederken için en son sürüme mümkün olan en kısa sürede güncelleştirilmesi önerilir. 

MySQL kaynak sağlayıcısı sürüm 1.1.33.0 sürümünden başlayarak, güncelleştirmeleri birikmeli özelliktedir ve yayımlanmış olan yüklü olması gerekmez; Başlangıç 1.1.24.0 sürümünden veya üzeri olduğu sürece. MySQL kaynak sağlayıcısı 1.1.24.0 sürümünü çalıştırıyorsanız, örneğin, ardından 1.1.33.0 sürümüne veya daha sonra ilk sürümünü 1.1.30.0 yüklemeye gerek olmadan yükseltebilirsiniz. Kullanılabilir kaynak sağlayıcısı sürümleri ve üzerinde desteklenir, Azure Stack sürümünü gözden geçirmek için sürümleri listesinde bakın [kaynak sağlayıcı önkoşulları dağıtma](./azure-stack-mysql-resource-provider-deploy.md#prerequisites).

Kullandığınız kaynak Sağlayıcısı'nı güncelleştirmek için **UpdateMySQLProvider.ps1** betiği. İşlem, bir kaynak sağlayıcısı dağıtma bu makalenin kaynak sağlayıcı bölümüne açıklandığı yüklemek için kullanılan işleme benzer. Betik kaynak sağlayıcısının indirmeye dahil edilir. 

 > [!IMPORTANT]
 > Kaynak Sağlayıcısı'nı yükseltmeden önce yeni işlevler, düzeltmeler ve dağıtımınızı etkileyebilecek bilinen sorunlar hakkında bilgi edinmek için sürüm notlarını gözden geçirin.

## <a name="update-script-processes"></a>Güncelleştirme betik işlemleri

**UpdateMySQLProvider.ps1** betik en son kaynak sağlayıcı kodu ile yeni bir VM oluşturur ve ayarlar eski sanal makineden yeni sanal Makineye geçirir. Gerekli DNS kaydı ve veritabanı ve sunucu bilgileri barındırma geçişi ayarlarını içerir. 

>[!NOTE]
>En son Windows Server 2016 Core görüntüyü Market Yönetimi'nden indirmenizi öneririz. Bir güncelleştirme yüklemeniz gerekiyorsa, yerleştirebilirsiniz bir **tek** MSU paketinde bağımlılık yerel yolu. Bu konumda birden fazla MSU dosyası yoksa, betiği başarısız olur.

Betik DeployMySqlProvider.ps1 betik için tanımlanan aynı bağımsız değişkenleri kullanımını gerektirir. Sertifikayı buradan de sağlar.  


## <a name="update-script-parameters"></a>Betik parametreleri güncelleştirin 
Programını çalıştırdığınızda, komut satırından aşağıdaki parametreleri belirtebilirsiniz **UpdateMySQLProvider.ps1** PowerShell Betiği. Yoksa veya herhangi bir parametre doğrulaması başarısız olursa, gerekli parametreleri sağlamanız istenir. 

| Parametre Adı | Açıklama | Açıklama veya varsayılan değeri | 
| --- | --- | --- | 
| **CloudAdminCredential** | Ayrıcalıklı uç nokta erişmek için gerekli bulut Yöneticisi kimlik bilgileri. | _Gerekli_ | 
| **AzCredential** | Azure Stack hizmet yönetici hesabının kimlik bilgileri. Azure Stack dağıtmak için kullanılan kimlik bilgilerini kullanın. | _Gerekli_ | 
| **VMLocalCredential** |SQL kaynak sağlayıcısı VM yerel yönetici hesabı için kimlik bilgileri. | _Gerekli_ | 
| **PrivilegedEndpoint** | Ayrıcalıklı uç noktasının DNS adı veya IP adresi. |  _Gerekli_ | 
| **AzureEnvironment** | Azure Stack dağıtmak için kullanılan hizmet yönetici hesabının Azure ortamı. Yalnızca Azure AD dağıtımları için gereklidir. Desteklenen ortam adları **AzureCloud**, **AzureUSGovernment**, veya bir Azure AD, Çin kullanıyorsanız **AzureChinaCloud**. | AzureCloud |
| **DependencyFilesLocalPath** | Sertifika .pfx dosyanızı bu dizinde yerleştirilmelidir. | _İsteğe bağlı_ (_zorunlu_ çok düğümlü için) | 
| **DefaultSSLCertificatePassword** | .Pfx sertifika için parola. | _Gerekli_ | 
| **MaxRetryCount** | Her işlem bir hata olursa yeniden denemek istiyor sayısı.| 2 | 
| **RetryDuration** | Saniye cinsinden yeniden denemeler arasındaki zaman aşımı aralığı. | 120 | 
| **Kaldırma** | Kaynak sağlayıcısı ile ilişkili tüm kaynakları (aşağıdaki notlara bakın) kaldırın. | Hayır | 
| **DebugMode** | Otomatik temizleme başarısız engeller. | Hayır | 
| **AcceptLicense** | GPL lisansı kabul etmek için istemi atlar.  (https://www.gnu.org/licenses/old-licenses/gpl-2.0.html) | | 

## <a name="update-script-example"></a>Güncelleştirme betiği örneği
Kullanımının bir örneği verilmiştir *UpdateMySQLProvider.ps1* yükseltilmiş bir PowerShell konsolundan çalıştırarak betiği. Değişken bilgileri ve parolaları gerektiği şekilde değiştirdiğinizden emin olun:  

> [!NOTE] 
> Güncelleştirme işlemi, yalnızca tümleşik sistemleri için geçerlidir. 

```powershell 
# Install the AzureRM.Bootstrapper module, set the profile and install the AzureStack module
# Note that this might not be the most currently available version of Azure Stack PowerShell.
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2018-03-01-hybrid -Force
Install-Module -Name AzureStack -RequiredVersion 1.6.0

# Use the NetBIOS name for the Azure Stack domain. On the Azure Stack SDK, the default is AzureStack but could have been changed at install time. 
$domain = "AzureStack" 

# For integrated systems, use the IP address of one of the ERCS virtual machines 
$privilegedEndpoint = "AzS-ERCS01" 

# Provide the Azure environment used for deploying Azure Stack. Required only for Azure AD deployments. Supported environment names are AzureCloud, AzureUSGovernment, or AzureChinaCloud. 
$AzureEnvironment = "<EnvironmentName>"

# Point to the directory where the resource provider installation files were extracted. 
$tempDir = 'C:\TEMP\MYSQLRP' 

# The service admin account (can be Azure Active Directory or Active Directory Federation Services). 
$serviceAdmin = "admin@mydomain.onmicrosoft.com" 
$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force 
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass) 
 
# Set credentials for the new resource provider VM. 
$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force 
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("sqlrpadmin", $vmLocalAdminPass) 
 
# And the cloudadmin credential required for privileged endpoint access. 
$CloudAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force 
$CloudAdminCreds = New-Object System.Management.Automation.PSCredential ("$domain\cloudadmin", $CloudAdminPass) 

# Change the following as appropriate. 
$PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force 
 
# Change directory to the folder where you extracted the installation files. 
# Then adjust the endpoints. 
$tempDir\UpdateMySQLProvider.ps1 -AzCredential $AdminCreds ` 
-VMLocalCredential $vmLocalAdminCreds ` 
-CloudAdminCredential $cloudAdminCreds ` 
-PrivilegedEndpoint $privilegedEndpoint ` 
-AzureEnvironment $AzureEnvironment `
-DefaultSSLCertificatePassword $PfxPass ` 
-DependencyFilesLocalPath $tempDir\cert ` 
-AcceptLicense 
```  

## <a name="next-steps"></a>Sonraki adımlar
[MySQL kaynak sağlayıcısı koru](azure-stack-mysql-resource-provider-maintain.md)
