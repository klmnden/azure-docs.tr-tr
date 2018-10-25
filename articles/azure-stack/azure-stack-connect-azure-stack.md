---
title: Azure Stack'e bağlanma | Microsoft Docs
description: Azure Stack'e bağlanma konusunda bilgi edinin.
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
ms.date: 09/28/2018
ms.author: mabrigg
ms.openlocfilehash: 1cdf013325afe4b217f5f56043e06f60a4933419
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2018
ms.locfileid: "49993179"
---
# <a name="connect-to-azure-stack-development-kit"></a>Azure Stack Geliştirme Seti için Bağlan

*İçin geçerlidir: Azure Stack Geliştirme Seti*

Kaynakları yönetmek için Azure Stack Geliştirme Seti için önce bağlanmanız gerekir. Bu makalede, biz geliştirme setine bağlamak için atmanız gereken adımları açıklar. Aşağıdaki bağlantı seçeneklerden birini kullanabilirsiniz:

* [Uzak Masaüstü Bağlantısı](#connect-with-remote-desktop). Uzak Masaüstü bağlantısı kullanarak bağlandığınızda, tek bir kullanıcı için Geliştirme Seti hızlı bir şekilde bağlanabilirsiniz.
* [Sanal özel ağ (VPN)](#connect-with-vpn). Bir VPN kullanarak bağlandığınızda, birden çok kullanıcı eşzamanlı olarak Azure Stack altyapısının dışında istemcilerden bağlanabilirsiniz. Bir VPN bağlantısı bazı kurulum gerektirir.

<a name="connect-to-azure-stack-with-remote-desktop"></a>
##  <a name="connect-to-azure-stack-by-using-remote-desktop-connection"></a>Uzak Masaüstü bağlantısını kullanarak Azure Stack'e bağlanma

Tek bir eş zamanlı kullanıcı işleci portalında veya Kullanıcı Portalı Uzak Masaüstü bağlantısı üzerinden kaynakları yönetebilir.

1. Uzak Masaüstü Bağlantısı'nı açın ve geliştirme setine bağlanın. Kullanıcı adı girin **AzureStack\AzureStackAdmin**. Azure yığını ayarlandığında belirtilen işleç parolayı kullanın.  

2. Geliştirme Seti bilgisayarda Sunucu Yöneticisi'ni açın. Seçin **yerel sunucu**temizleyin **Internet Explorer Artırılmış Güvenlik** onay kutusunu işaretleyin ve ardından Sunucu Yöneticisi'ni kapatın.

3. Açmak için [kullanıcı portalı](azure-stack-key-features.md#portal)Git https://portal.local.azurestack.external/. Kullanıcı kimlik bilgilerini kullanarak oturum açın. Azure Stack açmak için [işleci portalı](azure-stack-key-features.md#portal)Git https://adminportal.local.azurestack.external/. Yükleme sırasında belirttiğiniz Azure Active Directory (Azure AD) kimlik bilgilerini kullanarak oturum açın.

<a name="connect-to-azure-stack-with-vpn"></a>
## <a name="connect-to-azure-stack-by-using-vpn"></a>Azure Stack VPN kullanarak bağlanma

Bir Azure Stack geliştirme Seti'ni VPN bağlantısı bölünmüş tünel oluşturabilir. Azure Stack operatörü portal, kullanıcı portalı ve Azure Stack kaynaklarınızı yönetmek için Visual Studio ve PowerShell gibi yerel olarak yüklenen araçlar erişmek için bir VPN bağlantısı'nı kullanabilirsiniz. VPN bağlantısı, Azure AD'de desteklenir ve Active Directory Federasyon Hizmetleri (AD FS) dağıtımları. VPN bağlantıları için birden çok istemci aynı zamanda Azure Stack'e bağlanma mümkün hale getirir.

> [!NOTE]
> Azure Stack altyapısının Vm'leri bağlantısı bir VPN bağlantısı sağlamaz.

### <a name="prerequisites"></a>Önkoşullar

1. Yükleme [Azure Stack ile uyumlu Azure PowerShell](azure-stack-powershell-install.md) yerel bilgisayarınızda.  
2. İndirme [Azure Stack ile çalışması için gereken araçları](azure-stack-powershell-download.md).

### <a name="set-up-vpn-connectivity"></a>VPN bağlantısı ayarlama

Geliştirme Seti için bir VPN bağlantısı oluşturmak için Windows PowerShell, yerel Windows tabanlı bilgisayarda yönetici olarak açın. Daha sonra (IP adresi ve parola değerlerini, ortamınız için güncelleştirme) aşağıdaki betiği çalıştırın:

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

Kurulum başarılı olursa **azurestack** VPN bağlantıları listesinde görünür.

![Ağ bağlantıları](media/azure-stack-connect-azure-stack/image3.png)  

### <a name="connect-to-azure-stack"></a>Azure Stack'e Bağlanma

Aşağıdaki yöntemlerden birini kullanarak Azure Stack örneğine bağlanın:  

* Kullanım `Connect-AzsVpn ` komutu:
    
  ```PowerShell
  Connect-AzsVpn `
    -Password $Password
  ```

  İstendiğinde, Azure Stack ana güven ve sertifika yükleme **AzureStackCertificateAuthority** yerel bilgisayarınızın sertifika deposunda. (İstemi PowerShell penceresi tarafından gizlenmiş olabilir.)

* Yerel bilgisayarınızda seçin **ağ ayarlarını** > **VPN** > **azurestack** > **bağlanma**. Oturum açma isteminde kullanıcı adını girin (**AzureStack\AzureStackAdmin**) ve parolanızı.

### <a name="test-vpn-connectivity"></a>VPN bağlantısını test etme

Portal bağlantısını sınamak için bir web tarayıcısı açın ve ardından ya da Kullanıcı Portalı'na gidin (https://portal.local.azurestack.external/) ya da işleç portal (https://adminportal.local.azurestack.external/). Oturum açın ve kaynakları oluşturun.  

## <a name="next-steps"></a>Sonraki adımlar

[Sanal makineler, Azure Stack kullanıcıları için kullanılabilir yap](azure-stack-tutorial-tenant-vm.md)
