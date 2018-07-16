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
ms.date: 07/13/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: 4e894eaee6bb151b480204905d0a98324f5c353b
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39049604"
---
# <a name="update-the-mysql-resource-provider"></a>MySQL kaynak sağlayıcısını güncelle 

*İçin geçerlidir: Azure Stack tümleşik sistemleri.*

Azure Stack yapılar güncelleştirildiğinde yeni bir SQL kaynak sağlayıcısı bağdaştırıcısını serbest bırakılması. Mevcut bağdaştırıcısı çalışmaya devam ederken için en son sürüme mümkün olan en kısa sürede güncelleştirilmesi önerilir. 

>[!IMPORTANT]
>Güncelleştirmeleri yayımlandıktan sırayla yüklemeniz gerekir. Sürümleri atlayamazsınız. Sürümleri listesinde başvurmak [kaynak sağlayıcı önkoşulları dağıtma](.\azure-stack-mysql-resource-provider-deploy.md#prerequisites).

## <a name="update-the-mysql-resource-provider-adapter-integrated-systems-only"></a>MySQL kaynak sağlayıcısı bağdaştırıcısını (yalnızca tümleşik sistemleri) güncelleştirme
Azure Stack yapılar güncelleştirildiğinde yeni bir SQL kaynak sağlayıcısı bağdaştırıcısını serbest bırakılması. Mevcut bağdaştırıcısı çalışmaya devam ederken için en son sürüme mümkün olan en kısa sürede güncelleştirilmesi önerilir.  
 
Kullandığınız kaynak Sağlayıcısı'nı güncelleştirmek için **UpdateMySQLProvider.ps1** betiği. İşlem bölümünde anlatıldığı gibi bir kaynak sağlayıcısını yüklemek için kullanılan işlem benzer [kaynak sağlayıcısı dağıtma](#deploy-the-resource-provider) bu makalenin. Betik kaynak sağlayıcısının indirmeye dahil edilir. 

**UpdateMySQLProvider.ps1** betik en son kaynak sağlayıcı kodu ile yeni bir VM oluşturur ve ayarlar eski sanal makineden yeni sanal Makineye geçirir. Gerekli DNS kaydı ve veritabanı ve sunucu bilgileri barındırma geçişi ayarlarını içerir. 

>[!NOTE]
>En son Windows Server 2016 Core görüntüyü Market Yönetimi'nden indirmenizi öneririz. Bir güncelleştirme yüklemeniz gerekiyorsa, yerleştirebilirsiniz bir **tek** MSU paketinde bağımlılık yerel yolu. Bu konumda birden fazla MSU dosyası yoksa, betiği başarısız olur.

Betik DeployMySqlProvider.ps1 betik için tanımlanan aynı bağımsız değişkenleri kullanımını gerektirir. Sertifikayı buradan de sağlar.  

Aşağıdaki örneğidir *UpdateMySQLProvider.ps1* PowerShell İstemi'nden çalıştırabileceğiniz bir betik. Hesap bilgileri ve gerektiği gibi parolalar değiştirdiğinizden emin olun:  

> [!NOTE] 
> Güncelleştirme işlemi, yalnızca tümleşik sistemleri için geçerlidir. 

```powershell 
# Install the AzureRM.Bootstrapper module and set the profile. 
Install-Module -Name AzureRm.BootStrapper -Force 
Use-AzureRmProfile -Profile 2017-03-09-profile 

# Use the NetBIOS name for the Azure Stack domain. On the Azure Stack SDK, the default is AzureStack but could have been changed at install time. 
$domain = "AzureStack" 

# For integrated systems, use the IP address of one of the ERCS virtual machines 
$privilegedEndpoint = "AzS-ERCS01" 

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
-DefaultSSLCertificatePassword $PfxPass ` 
-DependencyFilesLocalPath $tempDir\cert ` 
-AcceptLicense 
``` 
 
### <a name="updatemysqlproviderps1-parameters"></a>UpdateMySQLProvider.ps1 parametreleri 
Bu parametreleri komut satırında belirtebilirsiniz. Bunu yapmazsanız veya herhangi bir parametre doğrulaması başarısız olursa, gerekli parametreleri sağlamak için istenir. 

| Parametre Adı | Açıklama | Açıklama veya varsayılan değeri | 
| --- | --- | --- | 
| **CloudAdminCredential** | Ayrıcalıklı uç nokta erişmek için gerekli bulut Yöneticisi kimlik bilgileri. | _Gerekli_ | 
| **AzCredential** | Azure Stack hizmet yönetici hesabının kimlik bilgileri. Azure Stack dağıtmak için kullanılan kimlik bilgilerini kullanın. | _Gerekli_ | 
| **VMLocalCredential** |SQL kaynak sağlayıcısı VM yerel yönetici hesabı için kimlik bilgileri. | _Gerekli_ | 
| **PrivilegedEndpoint** | Ayrıcalıklı uç noktasının DNS adı veya IP adresi. |  _Gerekli_ | 
| **DependencyFilesLocalPath** | Sertifika .pfx dosyanızı bu dizinde yerleştirilmelidir. | _İsteğe bağlı_ (_zorunlu_ çok düğümlü için) | 
| **DefaultSSLCertificatePassword** | .Pfx sertifika için parola. | _Gerekli_ | 
| **MaxRetryCount** | Her işlem bir hata olursa yeniden denemek istiyor sayısı.| 2 | 
| **RetryDuration** | Saniye cinsinden yeniden denemeler arasındaki zaman aşımı aralığı. | 120 | 
| **Kaldırma** | Kaynak sağlayıcısı ile ilişkili tüm kaynakları (aşağıdaki notlara bakın) kaldırın. | Hayır | 
| **DebugMode** | Otomatik temizleme başarısız engeller. | Hayır | 
| **AcceptLicense** | GPL lisansı kabul etmek için istemi atlar.  (http://www.gnu.org/licenses/old-licenses/gpl-2.0.html) | | 
 

## <a name="next-steps"></a>Sonraki adımlar
[MySQL kaynak sağlayıcısı koru](azure-stack-mysql-resource-provider-maintain.md)
