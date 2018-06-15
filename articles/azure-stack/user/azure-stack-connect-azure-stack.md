---
title: Azure yığınına bağlanma | Microsoft Docs
description: Azure yığın bağlamayı öğrenin
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
ms.topic: get-started-article
ms.date: 08/22/2017
ms.author: mabrigg
ms.openlocfilehash: 7479202a8afabf5a84560691a2bccf849206c077
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
ms.locfileid: "26636186"
---
# <a name="connect-to-azure-stack"></a>Azure Stack'e Bağlanma

Kaynakları yönetmek için Azure yığın Geliştirme Seti bağlanmanız gerekir. Bu makale ayrıntıları adımları Geliştirme Seti bağlanmak için gerekli. Aşağıdaki bağlantı seçeneklerinden birini kullanabilirsiniz:

* [Uzak Masaüstü](#connect-with-remote-desktop): hızlı geliştirme Seti'nden bağlanmak tek bir eş zamanlı kullanıcı olanak sağlar.
* [Sanal özel ağ (VPN)](#connect-with-vpn): birden çok eşzamanlı kullanıcı bağlanmak (yapılandırma gerektirir) Azure yığın altyapı dışında istemcilerden sağlar.

## <a name="connect-to-azure-stack-with-remote-desktop"></a>Uzak Masaüstü ile Azure yığın bağlanın
Uzak Masaüstü Bağlantısı ile tek bir eş zamanlı kullanıcı kaynaklarını yönetmek için portal ile çalışabilirsiniz.

1. Uzak Masaüstü Bağlantısı'nı açın ve Geliştirme Seti bağlanın. Girin **AzureStack\AzureStackAdmin** kullanıcı adı ve Azure yığın Kurulum sırasında sağlanan yönetici parolası olarak.  

2. Geliştirme Seti bilgisayardan açık Sunucu Yöneticisi,'ı tıklatın **yerel sunucu**, Internet Explorer Artırılmış Güvenlik devre dışı bırakma ve Sunucu Yöneticisi'ni kapatın.

3. Portalı'nı açmak için (https://portal.local.azurestack.external/) gidin ve kullanıcı kimlik bilgilerini kullanarak oturum açın.


## <a name="connect-to-azure-stack-with-vpn"></a>VPN ile Azure yığın bağlanın

Bölünmüş tünel için bir Azure yığın Geliştirme Seti sanal özel ağ (VPN) bağlantısı kurabilirsiniz. VPN bağlantısı üzerinden Kullanıcı Portalı, Yönetici portalına erişebilir ve Azure yığın kaynakları yönetmek için Visual Studio ve PowerShell gibi araçlar yerel olarak yüklü. VPN bağlantısı hem de Azure Active Directory(AAD) desteklenir ve Active Directory Federasyon Services(AD FS) tabanlı dağıtımlarının. VPN bağlantıları aynı anda Azure yığınına bağlanmak birden çok istemciye etkinleştirin. 

> [!NOTE] 
> Bu VPN bağlantısının bağlantı Azure yığın altyapısına VM'ler sağlamaz. 

### <a name="prerequisites"></a>Ön koşullar

* Yükleme [Azure yığın uyumlu Azure PowerShell](azure-stack-powershell-install.md) , yerel bilgisayarınızda.  
* Karşıdan [Azure yığın ile çalışmak için gereken araçları](azure-stack-powershell-download.md). 

### <a name="configure-vpn-connectivity"></a>VPN bağlantısı yapılandırma

Geliştirme Seti için bir VPN bağlantısı oluşturmak için yerel Windows tabanlı bilgisayarınızdan yükseltilmiş bir PowerShell oturumu açın ve aşağıdaki komut dosyasını (ortamınız için IP adresi ve parola değerlerini güncelleştirmek emin olun) çalıştırın:

```PowerShell 
# Configure winrm if it's not already configured
winrm quickconfig  

Set-ExecutionPolicy RemoteSigned

# Import the Connect module
Import-Module .\Connect\AzureStack.Connect.psm1 

# Add the development kit computer’s host IP address & certificate authority (CA) to the list of trusted hosts. Make sure to update the the IP address and password values for your environment. 

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

Kurulum başarılı olursa, görmelisiniz **azurestack** VPN bağlantılarının listenizde.

![Ağ bağlantıları](media/azure-stack-connect-azure-stack/image3.png)  

### <a name="connect-to-azure-stack"></a>Azure Stack'e Bağlanma

Aşağıdaki iki yöntemden birini kullanarak Azure yığın örneğine bağlan:  

* Kullanarak `Connect-AzsVpn ` komutu: 
    
  ```PowerShell
  Connect-AzsVpn `
    -Password $Password
  ```

  İstendiğinde, Azure yığın konak güven ve sertifika yükleme **AzureStackCertificateAuthority** yerel bilgisayar sertifika deposuna üzerine. (istemi PowerShell oturumu penceresi görünebilir). 

* Yerel bilgisayarınızın açık **ağ ayarlarını** > **VPN** > tıklatın **azurestack** > **bağlanmak**. Oturum açma isteminde (AzureStack\AzureStackAdmin) kullanıcı adı ve parola girin.

### <a name="test-the-vpn-connectivity"></a>VPN bağlantısını test etme

Portal bağlantıyı sınamak için bir Internet tarayıcısı açın ve Kullanıcı Portalı'na (https://portal.local.azurestack.external/) gidin, oturum açın ve kaynakları oluşturun.  

## <a name="next-steps"></a>Sonraki adımlar



