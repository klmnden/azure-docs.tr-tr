---
title: ASDK kullanarak bir Azure Stack yedekleme doğrulama | Microsoft Docs
description: ASDK kullanarak bir Azure Stack tümleşik sistemleri yedeklemeyi doğrulama yapma.
services: azure-stack
author: jeffgilb
manager: femila
cloud: azure-stack
ms.service: azure-stack
ms.topic: article
ms.date: 09/05/2018
ms.author: jeffgilb
ms.reviewer: hectorl
ms.lastreviewed: 09/05/2018
ms.openlocfilehash: 027d4a9f93032bfdd0f4cda96df74c92b5679540
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55251580"
---
# <a name="use-the-asdk-to-validate-an-azure-stack-backup"></a>Azure Stack yedekleme doğrulamak için ASDK kullanın
Azure Stack dağıtma ve kullanıcı kaynaklarını tekliflerini, planları, kotalar ve abonelikler gibi sağlama sonra yapmanız gerekenler [Azure Stack altyapısını yedekleme etkinleştir](../azure-stack-backup-enable-backup-console.md). Zamanlama ve normal altyapı yedeklemeleri geri dönülemez bir donanım veya Hizmeti hatası olması durumunda altyapı Yönetimi veri kaybı olmadığından sağlayacaktır.

> [!TIP]
> Önerilir, [isteğe bağlı yedekleme çalıştırmak](../azure-stack-backup-back-up-azure-stack.md) en son altyapı verilerin bir kopyasını olduğundan emin olmak için bu yordamı başlamadan önce. Yedekleme başarıyla tamamlandıktan sonra yedekleme kimliği yakalamak emin olun. Bulut Kurtarma sırasında bu kimliği gerekli olacaktır. 

Azure Stack altyapısını yedekleme Azure Stack'i yeniden dağıtma sırasında geri bulutunuzu hakkında önemli verileri içerir. Bu yedeklemeler, üretim bulut etkilemeden doğrulamak için ASDK kullanabilirsiniz. 

Yedekleri ASDK doğrulanırken aşağıdaki senaryolar için desteklenir:

|Senaryo|Amaç|
|-----|-----|
|Tümleşik bir çözüm altyapı yedeklerden doğrulayın.|Kısa süreli doğrulama yedekleme verilerinde geçerlidir.|
|Uçtan uca kurtarma iş akışı öğrenin.|Tüm yedekleme doğrulamak ve geri yükleme deneyimi ASDK kullanın.|
|     |     |

Aşağıdaki senaryoyu **değil** ASDK yedekleri doğrularken desteklenir:

|Senaryo|Amaç|
|-----|-----|
|Derleme yedekleme ve geri yüklemek için ASDK oluşturun.|Yedekleme verilerini ASDK önceki bir sürümünden daha yeni bir sürüme geri yükleyin.|
|     |     |


## <a name="cloud-recovery-deployment"></a>Bulut kurtarma dağıtımı
Altyapı tarafından sunulan tümleşik sistemler dağıtımınızı yedeklerden ASDK bulut kurtarma dağıtımını yaparak doğrulanabilir. Bu tür dağıtımda, ASDK ana bilgisayara yüklendikten sonra hizmete veri yedekten geri yüklenir.



### <a name="cloud-recovery-prerequisites"></a>Bulut kurtarma önkoşulları
ASDK bulut kurtarma dağıtımı başlatmadan önce aşağıdaki bilgilere sahip olduğunuzdan emin olun:

|Önkoşul|Açıklama|
|-----|-----|
|Yedekleme paylaşım yolu.|Azure Stack altyapısını bilgilerini kurtarmak için kullanılacak en son Azure Stack yedekleme UNC dosya paylaşım yolu. Bulut kurtarma dağıtım işlemi sırasında yerel Bu paylaşımı oluşturulur.|
|Yedek şifreleme anahtarı.|Azure Stack yönetim portalını kullanarak çalıştırmak için altyapı yedeklemeyi zamanlamak için kullanılan şifreleme anahtarı.|
|Geri yüklemek için yedekleme kimliği.|Bulut Kurtarma sırasında geri yüklenecek yedek tanımlayan "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx" alfasayısal biçiminde yedekleme kimliği.|
|Zaman sunucu IP'si.|Azure Stack dağıtımı için 132.163.97.2 gibi geçerli zaman bir sunucu IP'si gerekiyor.|
|Dış sertifika parolası.|Azure yığını tarafından kullanılan dış sertifika için parola. CA yedekleme bu parolayla geri yüklenmesi gereken dış sertifikaları içerir.|
|     |     | 

## <a name="prepare-the-host-computer"></a>Ana bilgisayar hazırlama 
Normal bir ASDK dağıtım olduğu gibi yükleme için ASDK konak sistem ortamı hazırlanmalıdır. Geliştirme Seti ana bilgisayar hazırlandığınızda ASDK dağıtımına başlamak için CloudBuilder.vhdx sanal makine sabit sürücüden önyükleme yapmaz.

ASDK ana bilgisayarda yedeklenen Azure Stack aynı sürüme karşılık gelen yeni bir cloudbuilder.vhdx indirin ve yönergelerini izleyin [ASDK ana bilgisayar hazırlama](asdk-prepare-host.md).

Cloudbuilder.vhdx ana sunucu yeniden başlatıldıktan sonra bir dosya paylaşımı oluşturmak ve yedekleme verilerinizi kopyalayın. Dosya Paylaşımı Kurulumu çalıştıran hesap için erişilebilir olmalıdır; Bu örnek PowerShell komutlarını yönetici: 

```powershell
$shares = New-Item -Path "c:\" -Name "Shares" -ItemType "directory"
$azsbackupshare = New-Item -Path $shares.FullName -Name "AzSBackups" -ItemType "directory"
New-SmbShare -Path $azsbackupshare.FullName -FullAccess ($env:computername + "\Administrator")  -Name "AzSBackups"
```

Ardından, en son Azure Stack yedekleme dosyalarını yeni oluşturduğunuz paylaşıma kopyalayın. Paylaşım klasör yapısında olmalıdır: `\\<ComputerName>\AzSBackups\MASBackup\<BackupID>\`.

## <a name="deploy-the-asdk-in-cloud-recovery-mode"></a>Bulut kurtarma modunda ASDK dağıtma
**InstallAzureStackPOC.ps1** betik bulut kurtarma başlatmak için kullanılır. 

> [!IMPORTANT]
> ASDK yükleme için ağ tam olarak bir ağ arabirim kartı (NIC) destekler. Birden çok NIC varsa, yalnızca bir etkin (ve diğerlerinin tümü devre dışıdır) dağıtım betiğini çalıştırmadan önce emin olun.

Ortamınız için aşağıdaki PowerShell komutları değiştirebilir ve bunları bulut kurtarma modunda ASDK dağıtmak için çalıştırın:

```powershell
cd C:\CloudDeployment\Setup     
$adminPass = Get-Credential Administrator
$key = ConvertTo-SecureString "<Your backup encryption key>" -AsPlainText -Force ` 
$certPass = Read-Host -AsSecureString  

.\InstallAzureStackPOC.ps1 -AdminPassword $adminpass.Password -BackupStorePath ("\\" + $env:COMPUTERNAME + "\AzSBackups") `
-BackupEncryptionKeyBase64 $key -BackupStoreCredential $adminPass -BackupId "<Backup ID to restore>" `
-TimeServer "<Valid time server IP>" -ExternalCertPassword $certPass
```

## <a name="restore-infrastructure-data-from-backup"></a>Altyapı verileri yedekten geri yükleyin
Başarılı bir bulut kurtarma dağıtımdan sonra geri yükleme kullanarak tamamlamanız gereken **geri yükleme-AzureStack** cmdlet'i. 

Azure Stack operatör olarak oturum açtıktan sonra [Azure Stack PowerShell'i yükleme](asdk-post-deploy.md#install-azure-stack-powershell) ve ardından yedekleme Kimliğinizi değiştirerek `Name` parametresi, aşağıdaki komutu çalıştırın:

```powershell
Restore-AzsBackup -Name "<BackupID>"
```
Bulutta yedekleme verilerinin doğrulaması başlatmak için bu cmdlet'i çağırma ASDK kurtarılan sonra 60 dakika bekleyin.

## <a name="next-steps"></a>Sonraki adımlar
[Azure Stack kaydetme](asdk-register.md)

