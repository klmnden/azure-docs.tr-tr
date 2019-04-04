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
ms.date: 02/12/2019
ms.author: jeffgilb
ms.reviewer: knithinc
ms.lastreviewed: 10/25/2018
ms.openlocfilehash: 31025582516198bdfe9da9312bae33852986a423
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58884865"
---
# <a name="connect-to-the-asdk"></a>İçin ASDK bağlanma

Kaynakları yönetmek için Azure Stack geliştirme Seti'ni (ASDK) için önce bağlanmanız gerekir. Bu makalede, biz ASDK için aşağıdaki bağlantı seçeneklerini kullanarak bağlanmak için attığınız adımlar açıklanmaktadır:

* [Uzak Masaüstü Bağlantısı (RDP)](#connect-with-rdp). Uzak Masaüstü bağlantısı kullanarak bağlandığınızda, tek bir kullanıcı için Geliştirme Seti hızlı bir şekilde bağlanabilirsiniz.
* [Sanal özel ağ (VPN)](#connect-with-vpn). Bir VPN kullanarak bağlandığınızda, birden çok kullanıcı için Azure Stack portalı Azure Stack altyapısının dışında istemcilerden eşzamanlı olarak bağlanabilir. Bir VPN bağlantısı bazı kurulum gerektirir.

<a name="connect-with-rdp"></a>
## <a name="connect-to-azure-stack-using-rdp"></a>RDP kullanarak Azure Stack'e bağlanma

Tek bir eş zamanlı kullanıcı kaynakları Azure Stack yönetim portalında veya Uzak Masaüstü bağlantısı aracılığıyla kullanıcı portalında doğrudan ASDK ana bilgisayardan yönetebilirsiniz. 

> [!TIP]
> Bu seçenek, RDP yeniden ASDK ana bilgisayarda açmış durumdayken ASDK ana bilgisayarda oluşturulan sanal makineler için oturum açarken kullandığınız da sağlar. 

1. Uzak Masaüstü Bağlantısı (mstc.exe) açın ve ASDK ana bilgisayara uzaktan oturum yetkisine sahip bir hesap kullanarak Geliştirme Seti ana bilgisayar IP adresine bağlanın. Varsayılan olarak, **AzureStack\AzureStackAdmin** ASDK ana bilgisayarına uzak öğesini izinlerine sahiptir.  

2. Geliştirme Seti ana bilgisayarda Sunucu Yöneticisi'ni (ServerManager.exe) açın. Seçin **yerel sunucu**, devre dışı **IE Artırılmış Güvenlik Yapılandırması**, Sunucu Yöneticisi'ni kapatın.

3. Yönetim portalında oturum açın **AzureStack\CloudAdmin** veya diğer Azure Stack operatörü kimlik bilgilerini kullanın. ASDK yönetim portal adresi [ https://adminportal.local.azurestack.external ](https://adminportal.local.azurestack.external).

4. Kullanıcı portalında oturum açın **AzureStack\CloudAdmin** veya diğer Azure Stack kullanıcı kimlik bilgilerini kullanın. ASDK Kullanıcı Portalı adresi [ https://portal.local.azurestack.external ](https://portal.local.azurestack.external).

> [!NOTE]
> Ne zaman hangi hesabın kullanılacağını hakkında daha fazla bilgi için bkz: [ASDK yönetim temel bilgileri](asdk-admin-basics.md#what-account-should-i-use).

<a name="connect-with-vpn"></a>
## <a name="connect-to-azure-stack-using-vpn"></a>Azure Stack VPN kullanarak bağlanma

Azure Stack portalı ve Visual Studio ve PowerShell gibi yerel olarak yüklenen araçlar erişmek için bir ASDK ana bilgisayara VPN bağlantısı bölünmüş tünel oluşturabilir. VPN bağlantıları kullanarak, birden çok kullanıcı aynı zamanda ASDK tarafından barındırılan Azure Stack kaynaklara bağlanabilir.

VPN bağlantısı desteklenen hem Azure AD için ve Active Directory Federasyon Hizmetleri (AD FS) dağıtımları.

> [!NOTE]
> Bir VPN bağlantısı *yok* Azure Stack Vm'lere bağlantı sağlamak. Azure Stack VPN bağlıyken Vm'leri rdp'ye mümkün olmayacaktır.

### <a name="prerequisites"></a>Önkoşullar
ASDK bir VPN bağlantısı kurmadan önce aşağıdaki önkoşulları karşıladığınızdan emin olun.

- Yükleme [Azure Stack ile uyumlu Azure PowerShell](asdk-post-deploy.md#install-azure-stack-powershell) yerel bilgisayarınızda.  
- İndirme [Azure Stack ile çalışması için gereken araçları](asdk-post-deploy.md#download-the-azure-stack-tools).

### <a name="set-up-vpn-connectivity"></a>VPN bağlantısı ayarlama

ASDK bir VPN bağlantısı oluşturmak için yerel Windows tabanlı bilgisayarda yönetici olarak PowerShell'i açın. Daha sonra (IP adresi ve parola değerlerini, ortamınız için güncelleştirme) aşağıdaki betiği çalıştırın:

```powershell
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

![Ağ bağlantıları](media/asdk-connect/vpn.png)  

### <a name="connect-to-azure-stack"></a>Azure Stack'e Bağlanma

  Aşağıdaki yöntemlerden birini kullanarak Azure Stack örneğine bağlanın:  

  * Kullanım `Connect-AzsVpn` komutu:
      
    ```powershell
    Connect-AzsVpn `
      -Password $Password
    ```

  * Yerel bilgisayarınızda seçin **ağ ayarlarını** > **VPN** > **azurestack** > **bağlanma**. Oturum açma isteminde kullanıcı adını girin (**AzureStack\AzureStackAdmin**) ve parolanızı.

İlk kez bağlandığınızda, size Azure Stack kök sertifikası yüklemeniz istenir **AzureStackCertificateAuthority** yerel bilgisayarınızın sertifika deposunda. Bu adım ASDK sertifika yetkilisi (CA) güvenilir konaklar listesine ekler. Tıklayın **Evet** sertifikayı yüklemek için.

![Kök sertifika](media/asdk-connect/cert.png)  
  
  > [!IMPORTANT]
  > İstendiğinde PowerShell penceresine veya diğer uygulamalar tarafından gizlenmiş olabilir.

### <a name="test-vpn-connectivity"></a>VPN bağlantısını test etme

Portal bağlantısını sınamak için bir web tarayıcısı açın ve ardından ya da Kullanıcı Portalı'na gidin (https://portal.local.azurestack.external/) veya Yönetim Portalı (https://adminportal.local.azurestack.external/). 

Kaynakları oluşturup yönetmek için uygun bir abonelik kimlik bilgilerinizle oturum açın.  

## <a name="next-steps"></a>Sonraki adımlar

[Sorun giderme](asdk-troubleshooting.md)
