---
title: Azure Stack geliştirme Seti'ni (ASDK) yükleme | Microsoft Docs
description: Azure Stack geliştirme Seti'ni (ASDK) yüklemeyi açıklar.
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
ms.date: 09/10/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.lastreviewed: 09/10/2018
ms.openlocfilehash: 20c96d1e25e0aef86d09c37ed919e61ec6058c6d
ms.sourcegitcommit: 947b331c4d03f79adcb45f74d275ac160c4a2e83
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2019
ms.locfileid: "55747450"
---
# <a name="install-the-azure-stack-development-kit-asdk"></a>Azure Stack geliştirme Seti'ni (ASDK) yükleyin
Sonra [ASDK ana bilgisayar hazırlama](asdk-prepare-host.md), bu makalede aşağıdaki adımları kullanarak CloudBuilder.vhdx görüntüye ASDK dağıtılabilir.

## <a name="install-the-asdk"></a>ASDK yükleyin
Bu makaledeki adımlarda, indirme ve çalıştırma tarafından sağlanan bir grafik kullanıcı arabirimi (GUI) kullanarak ASDK dağıtmayı Göster **asdk installer.ps1** PowerShell Betiği.

> [!NOTE]
> Azure Stack geliştirme Seti'ni yükleyicisi kullanıcı arabirimi, WCF ve PowerShell temel alan bir açık kaynaklı komut dosyasıdır.


1. Ana bilgisayar CloudBuilder.vhdx görüntünün başarıyla başlatıldıktan sonra yönetici kimlik bilgilerini kullanarak oturum belirtilen, [Geliştirme Seti ana bilgisayar hazırlanmış](asdk-prepare-host.md) ASDK yükleme. Bu Geliştirme Seti ana bilgisayar yerel yönetici kimlik bilgileriyle aynı olmalıdır.
2. Yükseltilmiş bir PowerShell konsolu açın ve çalıştırın  **&lt;sürücü harfi > \AzureStack_Installer\asdk-installer.ps1** (Bu artık C:\ CloudBuilder.vhdx görüntüde değerinden farklı bir sürücüde olabilir) komut dosyası. **Yükle**'ye tıklayın.

    ![](media/asdk-install/1.PNG) 

3. Kimlik sağlayıcısındaki **türü** açılan kutusunda **Azure bulut** veya **AD FS**. Altında **yerel yönetici parolasını** yazın (geçerli yapılandırılmış yerel yönetici parolasını eşleşmelidir) bir yerel yönetici parolasını **parola** kutusuna ve ardından  **Sonraki**.
    - **Azure bulut**: Azure Active Directory (Azure AD) kimlik sağlayıcısı olarak yapılandırır. Bu seçeneği kullanmak için bir Azure AD alanının tam adı bir internet bağlantısı gerekir. dizin Kiracı biçiminde *domainname*. onmicrosoft.com veya Azure AD'yi özel etki alanı adını ve belirtilen için genel yönetici kimlik doğrulandı Dizin. Dağıtımdan sonra Azure Active Directory genel yönetici izni gerekli değildir. Ancak, bazı işlemler, genel yönetici kimlik bilgileri gerektirebilir. Örneğin, bir kaynak sağlayıcısı yükleyicisi betiği veya izin verilecek gerektiren yeni bir özelliktir. Geçici olarak hesap genel yönetici izinleri yeniden geri veya sahiplerinden biri olan ayrı bir genel yönetici hesabı kullanın *varsayılan sağlayıcı aboneliği*.
    - **AD FS**: Varsayılan damga dizin hizmeti, kimlik sağlayıcısı olarak kullanılır. Oturum açmak için varsayılan hesap azurestackadmin@azurestack.local, ve kullanılacak kurulumunun bir parçası sağlanan bir paroladır.

    ![](media/asdk-install/2.PNG) 
    
    > [!NOTE]
    > Kimlik sağlayıcısı olarak, AD FS kullanarak, bağlantısı kesilmiş bir Azure Stack ortam kullanmak istediğiniz bile en iyi sonuçlar için internet'e bağlıyken ASDK yüklemek en iyisidir. Böylece, Geliştirme Seti yükleme işlemine dahil Windows Server 2016 Değerlendirme sürümü, dağıtım sırasında etkinleştirilebilir.
4. Geliştirme Seti kullanın ve ardından bir ağ bağdaştırıcısını seçin **sonraki**.

    ![](media/asdk-install/3.PNG)

5. DHCP veya statik ağ yapılandırması BGPNAT01 sanal makine seçin.
    > [!TIP]
    > Azure Stack için NAT ve VPN özellikleri sağlayan sınır yönlendiricisi BGPNAT01 vm'dir.

    - **DHCP** (varsayılan): Sanal makine ağ IP yapılandırması DHCP sunucusundan alır.
    - **Statik**: Yalnızca DHCP İnternet'e erişmek Azure Stack için geçerli bir IP adresi atayamadığı durumlarda bu seçeneği kullanın. **Statik bir IP adresi, CIDR biçiminde (örneğin, 10.0.0.5/24) alt ağ maskesi uzunluğu kullanılarak belirtilmelidir.**.
    - Geçerli bir türü **zaman sunucu IP'si** adresi. Başka bir gerekli alan Geliştirme Seti tarafından kullanılacak saat sunucusu belirler. Bu parametre, geçerli saat sunucusu IP adresi olarak sağlanmalıdır. Sunucu adları desteklenmez.

      > [!TIP]
      > Bir saat sunucusu IP adresini bulmak için ziyaret [ntppool.org](https://www.ntppool.org/) veya time.windows.com ping işlemi. 

    - **İsteğe bağlı olarak**, aşağıdaki değerleri ayarlayın:
        - **VLAN KİMLİĞİ**: VLAN kimliğini ayarlar Bu seçeneği yalnızca AzS-BGPNAT01 ve konak fiziksel ağ (ve İnternet'e) erişmek için VLAN Kimliğini yapılandırması gereken durumlarda kullanın. 
        - **DNS ileticisi**: Bir DNS sunucusu, Azure Stack dağıtımının bir parçası oluşturulur. Damga dışında adlarını çözümlemek için çözüm içindeki bilgisayarları izin vermek için mevcut altyapı DNS sunucunuzu sağlar. Damga DNS sunucusu bu sunucusuna Bilinmeyen ad çözümleme isteklerini iletir.

    ![](media/asdk-install/4.PNG)

6. Üzerinde **ağ arabirim kartının özellikleri doğrulanıyor** sayfasında, bir ilerleme çubuğu görürsünüz. Doğrulama tamamlandığında tıklayın **sonraki**.

    ![](media/asdk-install/5.PNG)

9. Üzerinde **özeti** sayfasında **Dağıt** Geliştirme Seti ana bilgisayarda ASDK yüklemeyi başlatmak için.

    ![](media/asdk-install/6.PNG)

    > [!TIP]
    > Burada geliştirme Seti'ni yüklemek için kullanılan PowerShell Kurulum komutları kopyalayabilirsiniz. İçin ihtiyacınız varsa yararlıdır [ASDK PowerShell kullanarak ana bilgisayarda yeniden](asdk-deploy-powershell.md).

10. Bir Azure AD dağıtım gerçekleştiriyorsanız, Kurulum başladıktan sonra birkaç dakika sonra Azure AD genel yönetici hesabı kimlik bilgilerinizi girmeniz istenir.

    ![](media/asdk-install/7.PNG)

11. Dağıtım işlemi sırasında hangi zaman ana bilgisayar otomatik olarak bir kez yeniden birkaç saat sürer. Dağıtımın ilerleme durumunu izlemek istiyorsanız, Geliştirme Seti konak yeniden başlatıldıktan sonra azurestack\AzureStackAdmin oturum açın. Dağıtım başarılı olduktan sonra PowerShell konsolunu görüntüler: **TAMAMLAYIN: Eylem 'Dağıtımı'**. 
    > [!IMPORTANT]
    > Makine etki alanına katıldıktan sonra yerel yönetici oturum açarsanız, dağıtımın ilerleme durumunu göremezsiniz. Dağıtım yeniden değil, bunun yerine azurestack\AzureStackAdmin çalıştığını doğrulamak oturum açın.

    ![](media/asdk-install/8.PNG)

Tebrikler, ASDK başarıyla yüklediğiniz!

Dağıtım için herhangi bir nedenle başarısız olursa, yapabilecekleriniz [yeniden](asdk-redeploy.md) sıfırdan ya da kullanım aşağıdaki PowerShell, aynı yükseltilmiş PowerShell penceresinden, son başarılı adımdan dağıtımı yeniden komutları:

  ```powershell
  cd C:\CloudDeployment\Setup
  .\InstallAzureStackPOC.ps1 -Rerun
  ```

## <a name="next-steps"></a>Sonraki adımlar
[Dağıtım sonrası yapılandırma](asdk-post-deploy.md)
