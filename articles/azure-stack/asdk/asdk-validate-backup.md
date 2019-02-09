---
title: ASDK kullanarak bir Azure Stack yedekleme doğrulama | Microsoft Docs
description: ASDK kullanarak bir Azure Stack tümleşik sistemleri yedeklemeyi doğrulama yapma.
services: azure-stack
author: jeffgilb
manager: femila
cloud: azure-stack
ms.service: azure-stack
ms.topic: article
ms.date: 02/06/2018
ms.author: jeffgilb
ms.reviewer: hectorl
ms.lastreviewed: 09/05/2018
ms.openlocfilehash: 02ecb3cdec9ddb07bf48dfe77d1ed5fbf07975e0
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55965333"
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



### <a name="prereqs"></a>Bulut kurtarma önkoşulları
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

### <a name="use-the-installer-to-deploy-the-asdk-in-recovery-mode"></a>Yükleyici, kurtarma modunda ASDK dağıtım yapma
Bu bölümdeki adımları indirme ve çalıştırma tarafından sağlanan bir grafik kullanıcı arabirimi (GUI) kullanarak ASDK dağıtmayı Göster **asdk installer.ps1** PowerShell Betiği.

> [!NOTE]
> Azure Stack geliştirme Seti'ni yükleyicisi kullanıcı arabirimi, WCF ve PowerShell temel alan bir açık kaynaklı komut dosyasıdır.

1. Ana bilgisayar CloudBuilder.vhdx görüntünün başarıyla başlatıldıktan sonra yönetici kimlik bilgilerini kullanarak oturum belirtilen, [Geliştirme Seti ana bilgisayar hazırlanmış](asdk-prepare-host.md) ASDK yükleme. Bu Geliştirme Seti ana bilgisayar yerel yönetici kimlik bilgileriyle aynı olmalıdır.
2. Yükseltilmiş bir PowerShell konsolu açın ve çalıştırın  **&lt;sürücü harfi > \AzureStack_Installer\asdk-installer.ps1** PowerShell Betiği. Betik, artık C:\ CloudBuilder.vhdx görüntüde değerinden farklı bir sürücüde olabilir. Tıklayın **kurtarmak**.

    ![ASDK yükleyicisi betiği](media/asdk-validate-backup/1.PNG) 

3. ASDK ana bilgisayarda kimlik sağlayıcısı ve kimlik bilgileri sayfasında, Azure AD directory bilgilerinizi (isteğe bağlı) ve yerel yönetici parolasını girin. **İleri**’ye tıklayın.

    ![Kimlik ve kimlik bilgileri sayfası](media/asdk-validate-backup/2.PNG) 

4. ' A tıklayın ve ASDK ana bilgisayar tarafından kullanılacak ağ bağdaştırıcısı seçin **sonraki**. Diğer tüm ağ arabirimleri ASDK yükleme sırasında devre dışı bırakılır. 

    ![Ağ bağdaştırıcısı arabirimi](media/asdk-validate-backup/3.PNG) 

5. Ağ yapılandırma sayfasında, geçerli saat sunucusu ve DNS ileticisi IP adresleri sağlayın. **İleri**’ye tıklayın.

    ![Ağ yapılandırma sayfası](media/asdk-validate-backup/4.PNG) 

6. Ağ arabirim kartının özellikleri doğrulandıktan sonra tıklayın **sonraki**. 

    ![Ağ kartı ayarlarını doğrulama](media/asdk-validate-backup/5.PNG) 

7. Daha önce açıklanan gerekli bilgileri sağlayın [Önkoşullar bölümüne](#prereqs) Yedekleme Ayarları sayfası ve kullanıcı adı ve parola paylaşımına erişmek için kullanılacak. Tıklayın **sonraki**: 

   ![Yedekleme Ayarları sayfası](media/asdk-validate-backup/6.PNG) 

8. Özet sayfasında ASDK dağıtmak için kullanılacak dağıtım betiği gözden geçirin. Tıklayın **Dağıt** dağıtımına başlamak için. 

    ![Özet sayfası](media/asdk-validate-backup/7.PNG) 


### <a name="use-powershell-to-deploy-the-asdk-in-recovery-mode"></a>Kurtarma modunda ASDK dağıtmak için PowerShell kullanma
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

