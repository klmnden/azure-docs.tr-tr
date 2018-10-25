---
title: Azure Stack'e bağlanma | Microsoft Docs
description: İçin ASDK bağlanmayı öğreneceksiniz.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: 3cebbfa6-819a-41e3-9f1b-14ca0a2aaba3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2018
ms.author: jeffgilb
ms.openlocfilehash: 55be312046f5cdea2c1481ed435b5859ab2c2540
ms.sourcegitcommit: f6050791e910c22bd3c749c6d0f09b1ba8fccf0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50026919"
---
# <a name="connect-to-the-azure-stack-development-kit"></a>Azure Stack Geliştirme Seti bağlanma

Kaynakları yönetmek için Azure Stack geliştirme Seti'ni (ASDK) için önce bağlanmanız gerekir. Bu makalede, biz için ASDK bağlamak için atmanız gereken adımları açıklar. Aşağıdaki bağlantı seçeneklerden birini kullanabilirsiniz:

* [Uzak Masaüstü Bağlantısı](#connect-with-remote-desktop). Uzak Masaüstü bağlantısı kullanarak bağlandığınızda, tek bir kullanıcı için Geliştirme Seti hızlı bir şekilde bağlanabilirsiniz.
* [Sanal özel ağ (VPN)](#connect-with-vpn). Bir VPN kullanarak bağlandığınızda, birden çok kullanıcı eşzamanlı olarak Azure Stack altyapısının dışında istemcilerden bağlanabilirsiniz. Bir VPN bağlantısı bazı kurulum gerektirir.

<a name="connect-to-azure-stack-with-remote-desktop"></a>
##  <a name="connect-to-azure-stack-by-using-remote-desktop-connection"></a>Uzak Masaüstü bağlantısını kullanarak Azure Stack'e bağlanma

Tek bir eş zamanlı kullanıcı işleci portalında veya Kullanıcı Portalı Uzak Masaüstü bağlantısı üzerinden kaynakları yönetebilir.

1. Uzak Masaüstü Bağlantısı (mstc.exe) açın ve Geliştirme Seti ana bilgisayarı olarak bağlanma **AzureStack\AzureStackAdmin** ASDK Kurulum sırasında belirtilen parolayla.  

2. Geliştirme Seti ana bilgisayarda Sunucu Yöneticisi'ni (ServerManager.exe) açın. Seçin **yerel sunucu**, devre dışı **IE Artırılmış Güvenlik Yapılandırması**, Sunucu Yöneticisi'ni kapatın.

3. Yönetim portalında oturum açın **AzureStack\CloudAdmin** veya diğer Azure Stack operatörü kimlik bilgilerini kullanın. ASDK yönetim portal adresi [ https://adminportal.local.azurestack.external ](https://adminportal.local.azurestack.external).

4. Kullanıcı portalında oturum açın **AzureStack\CloudAdmin** veya diğer Azure Stack kullanıcı kimlik bilgilerini kullanın. ASDK Kullanıcı Portalı adresi [ https://portal.local.azurestack.external ](https://portal.local.azurestack.external).

> [!NOTE]
> Ne zaman hangi hesabın kullanılacağını hakkında daha fazla bilgi için bkz: [ASDK yönetim temel bilgileri](asdk-admin-basics.md#what-account-should-i-use).

<a name="connect-to-azure-stack-with-vpn"></a>
## <a name="connect-to-azure-stack-by-using-vpn"></a>Azure Stack VPN kullanarak bağlanma

Azure Stack portalı ve Visual Studio ve PowerShell gibi yerel olarak yüklenen araçlar erişmek için bir ASDK VPN bağlantısı bölünmüş tünel oluşturabilir. VPN bağlantıları kullanarak, birden çok kullanıcı aynı zamanda ASDK tarafından barındırılan Azure Stack kaynaklara bağlanabilir.

VPN bağlantısı desteklenen hem Azure AD için ve Active Directory Federasyon Hizmetleri (AD FS) dağıtımları.

> [!NOTE]
> Azure Stack altyapısının Vm'leri bağlantısı bir VPN bağlantısı sağlamaz.

### <a name="prerequisites"></a>Önkoşullar
ASDK bir VPN bağlantısı kurmadan önce aşağıdaki önkoşulları karşıladığınızdan emin olun.

- Yükleme [Azure Stack ile uyumlu Azure PowerShell](asdk-post-deploy.md#install-azure-stack-powershell) yerel bilgisayarınızda.  
- İndirme [Azure Stack ile çalışması için gereken araçları](asdk-post-deploy.md#download-the-azure-stack-tools).

### <a name="set-up-vpn-connectivity"></a>VPN bağlantısı ayarlama

ASDK bir VPN bağlantısı oluşturmak için yerel Windows tabanlı bilgisayarda yönetici olarak PowerShell'i açın. Daha sonra (IP adresi ve parola değerlerini, ortamınız için güncelleştirme) aşağıdaki betiği çalıştırın:

```PowerShell
# Change directories to the default Azure Stack tools directory
cd C:\AzureStack-Tools-master

# Configure Windows Remote Management (WinRM), if it's not already configured.
winrm quickconfig  

Set-ExecutionPolicy RemoteSigned

# Import the Connect module.
Import-Module .\Connect\AzureStack.Connect.psm1

# Add the development kit host computer’s IP address as the ASDK certificate authority (CA) to the list of trusted hosts. Make sure you update the IP address and password values for your environment.

$hostIP = "<Azure Stack host IP address>"

$Password = ConvertTo-SecureString `
  "<operator's password provided when deploying Azure Stack>" `
  -AsPlainText `
  -Force

Set-Item wsman:\localhost\Client\TrustedHosts `
  -Value $hostIP `
  -Concatenate

# Create a VPN connection entry for the local user.
Add-AzsVpnConnection `
  -ServerAddress $hostIP `
  -Password $Password

```

Kurulum başarılı olursa **azurestack** VPN bağlantıları listesinde görünür.

![Ağ bağlantıları](media/asdk-connect/image3.png)  

### <a name="connect-to-azure-stack"></a>Azure Stack'e Bağlanma

Aşağıdaki yöntemlerden birini kullanarak Azure Stack örneğine bağlanın:  

* Kullanım `Connect-AzsVpn ` komutu:
    
  ```PowerShell
  Connect-AzsVpn `
    -Password $Password
  ```

  İstendiğinde, Azure Stack ana güven ve sertifika yükleme **AzureStackCertificateAuthority** yerel bilgisayarınızın sertifika deposunda. 
  
  > [!IMPORTANT]
  > İstendiğinde PowerShell penceresi tarafından gizlenmiş olabilir.

* Yerel bilgisayarınızda seçin **ağ ayarlarını** > **VPN** > **azurestack** > **bağlanma**. Oturum açma isteminde kullanıcı adını girin (**AzureStack\AzureStackAdmin**) ve parolanızı.

### <a name="test-vpn-connectivity"></a>VPN bağlantısını test etme

Portal bağlantısını sınamak için bir web tarayıcısı açın ve ardından ya da Kullanıcı Portalı'na gidin (https://portal.local.azurestack.external/) veya Yönetim Portalı (https://adminportal.local.azurestack.external/). Oturum açın ve kaynakları oluşturun.  

## <a name="next-steps"></a>Sonraki adımlar

[Sorun giderme](asdk-troubleshooting.md)
