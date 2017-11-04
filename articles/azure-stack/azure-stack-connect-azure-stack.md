---
title: "Azure yığınına bağlanma | Microsoft Docs"
description: "Azure yığınına bağlamayı öğrenin."
services: azure-stack
documentationcenter: 
author: SnehaGunda
manager: byronr
editor: 
ms.assetid: 3cebbfa6-819a-41e3-9f1b-14ca0a2aaba3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/25/2017
ms.author: sngun
ms.openlocfilehash: ba2359739b1d9cd265879227a499c94f93d8e4c6
ms.sourcegitcommit: 6acb46cfc07f8fade42aff1e3f1c578aa9150c73
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2017
---
# <a name="connect-to-azure-stack"></a>Azure Stack'e Bağlanma

*Uygulandığı öğe: Azure yığın Geliştirme Seti*

Kaynakları yönetmek için Azure yığın Geliştirme Seti için önce bağlanmanız gerekir. Bu makalede, Geliştirme Seti bağlanmak için attığınız adımlar açıklanmaktadır. Aşağıdaki bağlantı seçeneklerden birini kullanabilirsiniz:

* [Uzak Masaüstü Bağlantısı](#connect-with-remote-desktop). Uzak Masaüstü bağlantısı kullanarak bağlandığında, tek bir kullanıcı Hızlı geliştirme Seti'nden bağlanabilir.
* [Sanal özel ağ (VPN)](#connect-with-vpn). VPN kullanarak bağlandığında, birden çok kullanıcı aynı anda (Kurulum gerektirir) Azure yığın altyapı dışında istemcilerden bağlanabilir.

<a name="connect-to-azure-stack-with-remote-desktop"></a>
##  <a name="connect-to-azure-stack-by-using-remote-desktop-connection"></a>Uzak Masaüstü bağlantısı kullanarak Azure yığınına bağlanma
Tek bir eş zamanlı kullanıcı işleci Portalı'nı veya Kullanıcı Portalı Uzak Masaüstü bağlantısı üzerinden kaynakları yönetebilir.

1. Uzak Masaüstü Bağlantısı'nı açın ve Geliştirme Seti bağlanın. Kullanıcı adı için girin **AzureStack\AzureStackAdmin**. Azure yığınına ayarlandığında belirtilen işleci parola kullanın.  

2. Geliştirme Seti bilgisayarda Sunucu Yöneticisi'ni açın. Seçin **yerel sunucu**, Temizle **Internet Explorer Artırılmış Güvenlik** onay kutusunu işaretleyin ve ardından Sunucu Yöneticisi'ni kapatın.

3. Açmak için [kullanıcı portalı](azure-stack-key-features.md#portal)https://portal.local.azurestack.external/ gidin. Kullanıcı kimlik bilgilerini kullanarak oturum açın. Azure yığın açmak için [işleci portal](azure-stack-key-features.md#portal)https://adminportal.local.azurestack.external/ gidin. Yükleme sırasında belirtilen Azure Active Directory (Azure AD) kimlik bilgilerini kullanarak oturum açın.

<a name="connect-to-azure-stack-with-vpn"></a>
## <a name="connect-to-azure-stack-by-using-vpn"></a>Azure yığınına VPN kullanarak bağlanan

Bölünmüş tünel için bir Azure yığın Geliştirme Seti VPN bağlantısı kurabilirsiniz. Azure yığın işleci portalı, kullanıcı portalı ve Azure yığın kaynakları yönetmek için Visual Studio ve PowerShell gibi yerel olarak yüklenen araçlar erişmek için bir VPN bağlantısı kullanın. VPN bağlantısı, Azure AD hem de Active Directory Federasyon Hizmetleri (AD FS) dağıtımlar desteklenir. VPN bağlantıları aynı anda Azure yığınına bağlanmak birden çok istemci mümkün kılar. 

> [!NOTE] 
> Bir VPN bağlantısı Azure yığın altyapı VM'ler bağlantı sağlamaz. 

### <a name="prerequisites"></a>Ön koşullar

1. Yükleme [Azure yığın uyumlu Azure PowerShell](azure-stack-powershell-install.md) , yerel bilgisayarınızda.  
2. Karşıdan [Azure yığın ile çalışmak için gereken araçları](azure-stack-powershell-download.md). 

### <a name="set-up-vpn-connectivity"></a>VPN bağlantısı

Geliştirme Seti için bir VPN bağlantısı oluşturmak için Windows PowerShell'i yerel bilgisayarınızdaki Windows tabanlı bir yönetici olarak açın. Ardından, aşağıdaki komut dosyası (IP adresi ve parola değerlerini, ortamınız için güncelleştirme) çalıştırın:

```PowerShell 
# Configure Windows Remote Management (WinRM), if it's not already configured.
winrm quickconfig  

Set-ExecutionPolicy RemoteSigned

# Import the Connect module.
Import-Module .\Connect\AzureStack.Connect.psm1 

# Add the development kit computer’s host IP address and certificate authority (CA) to the list of trusted hosts. Make sure you update the IP address and password values for your environment. 

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

Kurulum başarılı olursa, **azurestack** VPN bağlantıları listesinde görüntülenir.

![Ağ bağlantıları](media/azure-stack-connect-azure-stack/image3.png)  

### <a name="connect-to-azure-stack"></a>Azure Stack'e Bağlanma

Aşağıdaki yöntemlerden birini kullanarak Azure yığın örneğine bağlan:  

* Kullanım `Connect-AzsVpn ` komutu: 
    
  ```PowerShell
  Connect-AzsVpn `
    -Password $Password
  ```

  İstendiğinde, Azure yığın konak güven ve sertifika yükleme **AzureStackCertificateAuthority** yerel bilgisayar sertifika deposunda. (İstemi PowerShell penceresi tarafından gizlenmiş olabilir.) 

* Yerel bilgisayarınızda seçin **ağ ayarlarını** > **VPN** > **azurestack** > **Bağlan**. Oturum açma isteminde kullanıcı adını girin (**AzureStack\AzureStackAdmin**) ve parolanızı.

### <a name="test-vpn-connectivity"></a>VPN bağlantısını test etme

Portal bağlantıyı sınamak için bir web tarayıcısı açın ve kullanıcı portalı (https://portal.local.azurestack.external/) ya da işleç portal (https://adminportal.local.azurestack.external/) gidin. Oturum açabilir ve kaynakları oluşturun.  

## <a name="next-steps"></a>Sonraki adımlar

[Sanal makineler Azure yığın kullanıcılarınızın kullanımına sunun](azure-stack-tutorial-tenant-vm.md)

