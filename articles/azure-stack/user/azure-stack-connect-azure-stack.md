---
title: Azure Stack'e bağlanma | Microsoft Docs
description: Azure Stack bağlanma hakkında bilgi edinin
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 3cebbfa6-819a-41e3-9f1b-14ca0a2aaba3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/14/2019
ms.author: mabrigg
ms.lastreviewed: 01/14/2019
ms.openlocfilehash: 9b7a5aef76ec72b0de76fb4517ffda02808eab9d
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58882506"
---
# <a name="connect-to-azure-stack"></a>Azure Stack'e Bağlanma

Kaynakları yönetmek için Azure Stack Geliştirme Seti için bağlanmanız gerekir. Bu makalede ayrıntıları adımları geliştirme setine bağlanmak için gereklidir. Aşağıdaki bağlantı seçeneklerinden birini kullanabilirsiniz:

* Uzak Masaüstü: hızlı bir şekilde geliştirme Seti'nden bağlanın, tek bir eş zamanlı kullanıcı sağlar.
* Sanal özel ağ (VPN): birden çok eş zamanlı kullanıcıların (yapılandırma gerektirir) Azure Stack altyapısının dışında istemcilerden bağlanmasına izin verir.

## <a name="connect-to-azure-stack-with-remote-desktop"></a>Uzak Masaüstü ile Azure stack'e bağlanma
Uzak Masaüstü Bağlantısı ile tek bir eş zamanlı kullanıcı kaynaklarını yönetmek için portalı ile çalışabilirsiniz.

1. Uzak Masaüstü Bağlantısı'nı açın ve geliştirme setine bağlanın. Girin **AzureStack\AzureStackAdmin** kullanıcı adı ve Azure Stack Kurulum sırasında belirttiğiniz yönetici parolasını olarak.  

2. Geliştirme Seti bilgisayardan açık Sunucu Yöneticisi'ni tıklatın **yerel sunucu**, Internet Explorer Artırılmış Güvenlik açmak ve Sunucu Yöneticisi'ni kapatın.

3. Portalını açmak için gidin (https://portal.local.azurestack.external/) kullanıcı kimlik bilgilerini kullanarak oturum açın.


## <a name="connect-to-azure-stack-with-vpn"></a>VPN ile Azure stack'e bağlanma

Bölünmüş tünel bir Azure Stack geliştirme Seti'ni sanal özel ağ (VPN) bağlantısı kurabilirsiniz. VPN bağlantısı üzerinden, Kullanıcı Portalı Yöneticisi portala erişebilir ve Azure Stack kaynaklarınızı yönetmek için Visual Studio ve PowerShell gibi araçlarla yerel olarak yüklü. VPN bağlantısı hem de Azure Active Directory (aad) içinde desteklenir ve Active Directory Federasyon Services(AD FS) tabanlı dağıtımlar. VPN bağlantıları, birden çok istemci aynı zamanda Azure Stack'e bağlanma etkinleştirin. 

> [!NOTE] 
> Bu VPN bağlantısının bağlantı Azure Stack altyapısının VM'ler için sağlamaz. 

### <a name="prerequisites"></a>Önkoşullar

* Yükleme [uyumlu Azure PowerShell'i Azure Stack](azure-stack-powershell-install.md) yerel bilgisayarınızda.  
* İndirme [Azure Stack ile çalışması için gereken araçları](azure-stack-powershell-download.md). 

### <a name="configure-vpn-connectivity"></a>VPN bağlantısı yapılandırma

Geliştirme Seti için bir VPN bağlantısı oluşturmak için yerel Windows tabanlı bilgisayarda yükseltilmiş bir PowerShell oturumu açın ve aşağıdaki komut dosyası (IP adresi ve parola değerlerini ortamınızın güncelleştirdiğinizden emin olun) çalıştırın:

```powershell 
# Configure winrm if it's not already configured
winrm quickconfig  

Set-ExecutionPolicy RemoteSigned

# Import the Connect module
Import-Module .\Connect\AzureStack.Connect.psm1 

# Add the development kit computer’s host IP address & certificate authority (CA) to the list of trusted hosts. Make sure to update the IP address and password values for your environment. 

$hostIP = "<Azure Stack host IP address>"

$Password = ConvertTo-SecureString `
  "<Administrator password provided when deploying Azure Stack>" `
  -AsPlainText `
  -Force

Set-Item wsman:\localhost\Client\TrustedHosts `
  -Value $hostIP `
  -Concatenate

# Create a VPN connection entry for the local user
Add-AzsVpnConnection `
  -ServerAddress $hostIP `
  -Password $Password

```

Kurulum başarılı olursa görmelisiniz **azurestack** VPN bağlantıları listesine.

![Ağ bağlantıları](media/azure-stack-connect-azure-stack/image3.png)  

### <a name="connect-to-azure-stack"></a>Azure Stack'e Bağlanma

Azure Stack aşağıdaki iki yöntemden birini kullanarak bağlanın:  

* Kullanarak `Connect-AzsVpn` komutu: 
    
  ```powershell
  Connect-AzsVpn `
    -Password $Password
  ```

  İstendiğinde, Azure Stack ana güven ve sertifika yükleme **AzureStackCertificateAuthority** üzerine yerel bilgisayar sertifika deposu. (PowerShell oturum penceresi istemi görünebilir). 

* Yerel bilgisayarınızın açın **ağ ayarlarını** > **VPN** > tıklatın **azurestack** > **bağlanma**. Oturum açma isteminde (AzureStack\AzureStackAdmin) kullanıcı adı ve parola girin.

### <a name="test-the-vpn-connectivity"></a>VPN bağlantısını test etme

Portal bağlantısını test etmek için bir Internet tarayıcısı açın ve Kullanıcı Portalı'na gidin (https://portal.local.azurestack.external/), oturum açın ve kaynakları oluşturun.  

## <a name="next-steps"></a>Sonraki adımlar



