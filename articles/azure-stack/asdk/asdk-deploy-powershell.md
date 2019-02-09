---
title: Azure Stack - PowerShell dağıtma | Microsoft Docs
description: Bu makalede, PowerShell kullanarak komut satırından ASDK yükleyin.
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
ms.custom: ''
ms.date: 02/08/2019
ms.author: jeffgilb
ms.reviewer: misainat
ms.lastreviewed: 02/08/2019
ms.openlocfilehash: 0fb3e9cd193e570a965d6bbd3e16c86dc39de350
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2019
ms.locfileid: "55984282"
---
# <a name="deploy-the-asdk-from-the-command-line"></a>ASDK komut satırından dağıtma
ASDK değerlendirmek ve Azure Stack özelliklerini ve hizmetler için dağıtabileceğiniz bir test ve geliştirme ortamıdır. Bu alınacağı ayarlandıktan ve çalışmaya, ortam donanım hazırlama ve bazı komut dosyaları (Bu işlem birkaç saat sürebilir) çalıştırmanız gerekir. Bundan sonra yönetici ve kullanıcı portalı için Azure Stack kullanmaya başlamak için oturum açabilir.

## <a name="prerequisites"></a>Önkoşullar 
Geliştirme Seti ana bilgisayar hazırlayın. Donanım, yazılım ve ağ planlayın. Geliştirme Seti (Geliştirme Seti ana bilgisayarı) barındıran bilgisayarın donanım, yazılım ve ağ gereksinimlerini karşılaması gerekir. Ayrıca, Azure Active Directory (Azure AD) veya Active Directory Federasyon Hizmetleri (AD FS) kullanma arasında seçmeniz gerekir. Yükleme işlemi sorunsuz çalışır böylece dağıtımınıza başlamadan önce bu önkoşullarıyla uyumlu olduğundan emin olun. 

ASDK dağıtmadan önce planlanan Geliştirme Seti ana bilgisayarınızın donanım, işletim sistemi, hesap ve ağ yapılandırmaları ASDK yüklemeye yönelik en düşük gereksinimleri karşıladığından emin olun.

**[ASDK dağıtım planlama konuları gözden geçirin](asdk-deploy-considerations.md)**

> [!TIP]
> Kullanabileceğiniz [Azure Stack dağıtım gereksinimleri kontrol aracı](https://gallery.technet.microsoft.com/Deployment-Checker-for-50e0f51b) donanımınızın tüm gereksinimleri karşıladığından emin doğrulamak için işletim sistemini yükledikten sonra.

## <a name="download-and-extract-the-deployment-package"></a>İndirin ve ayıklayın dağıtım paketi
Geliştirme Seti ana bilgisayarınız ASDK yüklemeye yönelik temel gereksinimleri karşıladığından emin olduktan sonra sonraki indirmeyi ve ayıklamayı ASDK dağıtım paketi adımdır. Dağıtım paketi önyüklenebilir bir işletim sistemi ve Azure Stack yükleme dosyalarını içeren bir sanal sabit sürücü Cloudbuilder.vhdx dosyasını içerir.

Dağıtım paketi Geliştirme Seti ana bilgisayara veya başka bir bilgisayara yükleyebilirsiniz. Başka bir bilgisayar kullanarak Geliştirme Seti ana bilgisayar için donanım gereksinimlerini azaltmaya yardımcı olabilmemiz için ayıklanan dağıtım dosyaları 60 GB boş disk alanı yararlanın.

**[İndirin ve Azure Stack geliştirme Seti'ni (ASDK) ayıklayın](asdk-download.md)**

## <a name="prepare-the-development-kit-host-computer"></a>Geliştirme Seti ana bilgisayarını hazırlayın
Ana bilgisayarda ASDK yükleyebilmek için önce ortamı hazırlanmalıdır ve sistem VHD'den önyükleme için yapılandırılmış. Bu adımdan sonra Geliştirme Seti konak Cloudbuilder.vhdx (önyüklenebilir bir işletim sistemi ve Azure Stack yükleme dosyalarını içeren bir sanal sabit sürücü) önyüklenir.

CloudBuilder.vhdx önyükleme ASDK ana bilgisayarı yapılandırmak için PowerShell kullanın. Bu komutlar ASDK ana bilgisayar önyükleme indirilen ve ayıklanan Azure Stack sanal sabit disk (CloudBuilder.vhdx) yapılandırın. Bu adımları tamamladıktan sonra ASDK ana bilgisayar yeniden başlatılır.

CloudBuilder.vhdx önyükleme ASDK ana bilgisayarı yapılandırmak için:

  1. Bir komut istemini yönetici olarak başlatın.
  2. `bcdedit /copy {current} /d "Azure Stack"` öğesini çalıştırın
  3. Gerekli dahil olmak üzere döndürülen, Kopyala (CTRL + C) CLSID değeri {}' s. Bu değer, {CLSID} adlandırılır ve kalan adımları (CTRL + V veya sağ tıklama) yapıştırılmasına gerekir.
  4. `bcdedit /set {CLSID} device vhd=[C:]\CloudBuilder.vhdx` öğesini çalıştırın 
  5. `bcdedit /set {CLSID} osdevice vhd=[C:]\CloudBuilder.vhdx`'i çalıştırın. 
  6. `bcdedit /set {CLSID} detecthal on`'i çalıştırın. 
  7. `bcdedit /default {CLSID}` öğesini çalıştırın
  8. Önyükleme ayarları doğrulamak için çalıştırın `bcdedit`. 
  9. Dosyayı C:\ sürücüsüne (C:\CloudBuilder.vhdx) kök dizinine taşındı ve Geliştirme Seti ana bilgisayar yeniden başlatılır CloudBuilder.vhdx emin olun. ASDK ana bilgisayar yeniden başlatıldığında ASDK dağıtımına başlamak için CloudBuilder.vhdx sanal makine sabit sürücüden önyükleme yapmanız gerekir. 

> [!IMPORTANT]
> Yeniden başlatmadan önce doğrudan fiziksel veya Geliştirme Seti ana bilgisayar KVM erişimine sahip olun. VM ilk kez başlatıldığında, Windows Server Kurulumu tamamlamak için ister. Geliştirme Seti ana bilgisayara oturum açmak için kullandığınız aynı yönetici kimlik bilgilerini sağlayın. 

### <a name="prepare-the-development-kit-host-using-powershell"></a>PowerShell kullanarak Geliştirme Seti konak hazırlama 
Geliştirme Seti sonra ana bilgisayarı başarıyla CloudBuilder.vhdx görüntünün Geliştirme Seti ana bilgisayara oturum açmak için kullanılan (ve Windows Server sonlandırılıyor bir parçası olarak sağlanan aynı yerel yönetici kimlik bilgileriyle oturum önyüklenir Ana bilgisayar VHD'den önyüklendiğinde Kurulumu). 

> [!NOTE]
> İsteğe bağlı olarak da yapılandırabilirsiniz [Azure Stack telemetri ayarlarını](asdk-telemetry.md#set-telemetry-level-in-the-windows-registry) *önce* ASDK yükleme.

Yükseltilmiş bir PowerShell konsolu açın ve Geliştirme Seti konaktaki ASDK dağıtmak için bu bölümdeki komutları çalıştırın.

> [!IMPORTANT] 
> ASDK yükleme için ağ tam olarak bir ağ arabirim kartı (NIC) destekler. Birden çok NIC varsa, yalnızca bir etkin (ve diğerlerinin tümü devre dışıdır) dağıtım betiğini çalıştırmadan önce emin olun.

Azure AD ile Azure Stack'te dağıtabilir veya Windows Server AD FS kimlik sağlayıcısı. Azure Stack, kaynak sağlayıcıları ve diğer uygulamalar her ikisi de aynı şekilde çalışır.

> [!TIP]
> (İsteğe bağlı parametreler InstallAzureStackPOC.ps1 ve örneklere bakın) herhangi bir kurulum parametre sağlamazsanız, gerekli parametreler için istenir.

### <a name="deploy-azure-stack-using-azure-ad"></a>Azure Stack, Azure AD kullanarak dağıtma 
Azure Stack dağıtmayı **kimlik sağlayıcısı olarak Azure AD'yi kullanarak**, doğrudan veya saydam bir ara sunucu üzerinden internet bağlantısı olması gerekir. 

Azure AD kullanarak Geliştirme Seti dağıtmak için aşağıdaki PowerShell komutlarını çalıştırın:

  ```powershell
  cd C:\CloudDeployment\Setup     
  $adminpass = Get-Credential Administrator     
  .\InstallAzureStackPOC.ps1 -AdminPassword $adminpass.Password
  ```

Birkaç dakika içinde ASDK yükleme için Azure AD kimlik bilgileri istenir. Azure AD kiracınız için genel yönetici kimlik bilgilerini sağlamanız gerekir. 

Dağıtımdan sonra Azure Active Directory genel yönetici izni gerekli değildir. Ancak, bazı işlemler, genel yönetici kimlik bilgileri gerektirebilir. Örneğin, bir kaynak sağlayıcısı yükleyicisi betiği veya izin verilecek gerektiren yeni bir özelliktir. Geçici olarak hesap genel yönetici izinleri yeniden geri veya sahiplerinden biri olan ayrı bir genel yönetici hesabı kullanın *varsayılan sağlayıcı aboneliği*.

### <a name="deploy-azure-stack-using-ad-fs"></a>AD FS kullanarak Azure Stack dağıtma 
Geliştirme Seti dağıtmak için **kimlik sağlayıcısı olarak AD FS kullanarak**, (yeterlidir - UseADFS parametre eklemek için) aşağıdaki PowerShell komutlarını çalıştırın: 

  ```powershell
  cd C:\CloudDeployment\Setup     
  $adminpass = Get-Credential Administrator 
  .\InstallAzureStackPOC.ps1 -AdminPassword $adminpass.Password -UseADFS
  ```

AD FS dağıtımında, varsayılan damga dizin hizmeti, kimlik sağlayıcısı olarak kullanılır. Oturum açmak için varsayılan hesap azurestackadmin@azurestack.local, ve PowerShell Kurulum komutları bir parçası olarak sağlanan için parola ayarlanır.

Dağıtım işlemi, hangi sırada sistem otomatik olarak bir kez yeniden başlatılır, birkaç saat sürebilir. Dağıtım başarılı olduktan sonra PowerShell konsolunu görüntüler: **TAMAMLAYIN: Eylem 'Dağıtımı'**. Dağıtım başarısız olursa, betiği kullanarak yeniden deneyebilirsiniz yeniden çalıştırma parametresi. Veya [ASDK yeniden](asdk-redeploy.md) sıfırdan.

> [!IMPORTANT]
> ASDK konak yeniden başlatıldıktan sonra dağıtımın ilerleme durumunu izlemek istiyorsanız, AzureStack\AzureStackAdmin oturum açmalısınız. Ana bilgisayar yeniden (ve azurestack.local etki alanına katılmış sonra) yerel bir yönetici olarak oturum açarsanız, dağıtımın ilerleme durumunu göremezsiniz. Dağıtım yeniden değil, bunun yerine azurestack çalıştığını doğrulamak oturum açın.


#### <a name="azure-ad-deployment-script-examples"></a>Azure AD dağıtım betik örnekleri
Tüm komut dosyası Azure AD dağıtımı. Bazı isteğe bağlı parametreler içeren açıklamalı birkaç örnek aşağıda verilmiştir.

Azure AD kimlik bilgilerinizi yalnızca ile ilişkili değilse **bir** Azure AD dizini:
```powershell
cd C:\CloudDeployment\Setup 
$adminpass = Get-Credential Administrator 
$aadcred = Get-Credential "<Azure AD global administrator account name>" 
.\InstallAzureStackPOC.ps1 -AdminPassword $adminpass.Password -InfraAzureDirectoryTenantAdminCredential $aadcred -TimeServer 52.168.138.145 #Example time server IP address.
```

Azure AD kimlik bilgilerinizi ilişkilendirilen **birden büyük** Azure AD dizini:
```powershell
cd C:\CloudDeployment\Setup 
$adminpass = Get-Credential Administrator 
$aadcred = Get-Credential "<Azure AD global administrator account name>" #Example: user@AADDirName.onmicrosoft.com 
.\InstallAzureStackPOC.ps1 -AdminPassword $adminpass.Password -InfraAzureDirectoryTenantAdminCredential $aadcred -InfraAzureDirectoryTenantName "<Azure AD directory in the form of domainname.onmicrosoft.com or an Azure AD verified custom domain name>" -TimeServer 52.168.138.145 #Example time server IP address.
```

Ortamınız DHCP etkin olmaması durumunda aşağıdaki ek parametreler (sağlanan örnek kullanım) yukarıdaki seçeneklerden birini eklemeniz gerekir: 

```powershell
.\InstallAzureStackPOC.ps1 -AdminPassword $adminpass.Password -InfraAzureDirectoryTenantAdminCredential $aadcred -TimeServer 10.222.112.26
```

### <a name="asdk-installazurestackpocps1-optional-parameters"></a>ASDK InstallAzureStackPOC.ps1 isteğe bağlı parametreler
|Parametre|Gerekli/isteğe bağlı|Açıklama|
|-----|-----|-----|
|AdminPassword|Gerekli|Geliştirme Seti dağıtımının bir parçası oluşturulan tüm sanal makinelerde yerel yönetici hesabı ve diğer tüm kullanıcı hesaplarını ayarlar. Bu parola, ana bilgisayardaki geçerli yerel yönetici parolasını eşleşmesi gerekir.|
|InfraAzureDirectoryTenantName|Gerekli|Kiracı dizinini ayarlar. AAD hesabının birden çok dizini Yönetme iznine sahip olduğu belirli bir dizini belirtmek için bu parametreyi kullanın. Tam adı biçiminde bir AAD Directory Kiracısı. onmicrosoft.com veya Azure AD'yi özel etki alanı adı doğrulandı.|
|Zaman sunucusunu|Gerekli|Belirli bir saat sunucusu belirtmek için bu parametreyi kullanın. Bu parametre, geçerli saat sunucusu IP adresi olarak sağlanmalıdır. Sunucu adları desteklenmez.|
|InfraAzureDirectoryTenantAdminCredential|İsteğe bağlı|Azure Active Directory kullanıcı adını ve parolasını ayarlar. Bu Azure kimlik bilgileri, bir kuruluş kimliği olmalıdır|
|InfraAzureEnvironment|İsteğe bağlı|Azure ile bu Azure Stack dağıtım kaydetmek istediğiniz ortamı seçin. Genel Azure, Azure - Çin'de, Azure - US Government seçenekleri içerir.|
|DNSForwarder|İsteğe bağlı|Bir DNS sunucusu, Azure Stack dağıtımının bir parçası oluşturulur. Damga dışında adlarını çözümlemek için çözüm içindeki bilgisayarları izin vermek için mevcut altyapı DNS sunucunuzu sağlar. Damga DNS sunucusu bu sunucusuna Bilinmeyen ad çözümleme isteklerini iletir.|
|Yeniden çalıştır|İsteğe bağlı|Dağıtım yeniden çalıştırmak için bu bayrağı kullanın. Önceki tüm giriş kullanılır. Çeşitli benzersiz değerler olduğundan ve oluşturulan dağıtım için kullanılan, daha önce sağlanan verileri yeniden girmeden desteklenmiyor.|


## <a name="perform-post-deployment-configurations"></a>Dağıtım sonrası yapılandırmaları gerçekleştirin
ASDK yükledikten sonra var. birkaç önerilen yükleme sonrası denetimleri ve yapılması gereken yapılandırma değişiklikleri Yüklemenizi doğrulamak için test AzureStack cmdlet'ini kullanarak başarıyla yüklendi ve Azure Stack PowerShell ve GitHub araçlarını yükleyin. 

Ayrıca, değerlendirme süresi sona ermeden önce Geliştirme Seti konak için parola süresi sona ermiyor emin olmak için parola süresi dolma ilkesini sıfırlamalısınız.

> [!NOTE]
> İsteğe bağlı olarak da yapılandırabilirsiniz [Azure Stack telemetri ayarlarını](asdk-telemetry.md#enable-or-disable-telemetry-after-deployment) *sonra* ASDK yükleme.

**[Sonrası ASDK dağıtım görevleri](asdk-post-deploy.md)**

## <a name="register-with-azure"></a>Azure ile kaydedin
Olan Azure Stack Azure ile kaydetmelisiniz [Azure Market öğelerini indirme](asdk-marketplace-item.md) Azure Stack için.

**[Azure Stack Azure ile kaydedin](asdk-register.md)**

## <a name="next-steps"></a>Sonraki adımlar
Tebrikler! Bu adımları tamamladıktan sonra hem bir geliştirme seti ortamını gerekir [yönetici](https://adminportal.local.azurestack.external) ve [kullanıcı](https://portal.local.azurestack.external) portalları. 

[ASDK yükleme sonrası yapılandırma görevleri](asdk-post-deploy.md)

