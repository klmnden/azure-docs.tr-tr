---
title: Azure yığın Geliştirme Seti (ASDK) yükleme | Microsoft Docs
description: Azure yığın Geliştirme Seti (ASDK) yüklemeyi açıklar.
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
ms.date: 03/22/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 7b8fe61731a9412c61152bc58e55deebb611d011
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
ms.locfileid: "30171205"
---
# <a name="install-the-azure-stack-development-kit-asdk"></a>Azure yığın Geliştirme Seti (ASDK) yükleyin
Sonra [ASDK ana bilgisayarı hazırlama](asdk-prepare-host.md), bu makalede aşağıdaki adımları kullanarak CloudBuilder.vhdx görüntüye ASDK dağıtılabilir.

## <a name="install-the-asdk"></a>ASDK yükleyin
Bu makaledeki adımları indirme ve çalıştırma sağlanan bir grafik kullanıcı arabirimi (GUI) kullanarak ASDK dağıtma Göster **asdk installer.ps1** PowerShell Betiği.

> [!NOTE]
> Yükleyici kullanıcı arabirimi Azure yığın Geliştirme Seti için WCF ve PowerShell dayalı bir açık kaynaklıdır komut dosyasıdır.


1. Ana bilgisayar CloudBuilder.vhdx görüntüsüne başarıyla başlatıldıktan sonra yönetici kimlik bilgilerini kullanarak oturum açın belirtilen, [Geliştirme Seti ana bilgisayar hazır](asdk-prepare-host.md) ASDK yükleme. Bu Geliştirme Seti ana bilgisayar yerel yönetici kimlik bilgileriyle aynı olmalıdır.
2. Yükseltilmiş bir PowerShell konsolunu açın ve Çalıştır  **&lt;sürücü harfi > \AzureStack_Installer\asdk-installer.ps1** (olabilen şimdi CloudBuilder.vhdx görüntüsündeki C:\'den farklı bir sürücüde) komut dosyası. **Yükle**'ye tıklayın.

    ![](media/asdk-install/1.PNG) 

3. Kimlik sağlayıcısı'nda **türü** açılan kutusunda **Azure bulut** veya **AD FS**. Altında **yerel yönetici parolası** (hangi geçerli yapılandırılmış yerel yönetici parolası eşleşmelidir) yerel yönetici parolasını yazın **parola** kutusuna ve ardından  **Sonraki**.
    - **Azure bulut**: yapılandırır Azure Active Directory (Azure AD) kimlik sağlayıcısı. Bu seçeneği kullanmak için Azure AD alanının tam adı internet bağlantısına ihtiyacınız dizin Kiracı biçiminde *domainname*. onmicrosoft.com veya Azure AD özel etki alanı adı ve genel yönetici kimlik bilgileri için belirtilen doğrulandı Dizin. 
    - **AD FS**: varsayılan damga dizin hizmeti kimlik sağlayıcısı olarak kullanılır. Oturum açmak için varsayılan hesaptır azurestackadmin@azurestack.local, ve kullanmak için kurulumunun bir parçası sağlanan bir paroladır.

    ![](media/asdk-install/2.PNG) 
    
    > [!NOTE]
    > Kimlik sağlayıcısı olarak, AD FS kullanarak bağlantısı kesilmiş bir Azure yığın ortam kullanmak istiyorsanız bile, en iyi sonuçlar için internet'e bağlı değilken ASDK yüklemek en iyisidir. Böylece, dağıtım sırasında Geliştirme Seti yüklemeye dahil Windows Server 2016 Değerlendirme sürümü etkinleştirilebilir.
4. Geliştirme Seti için kullanın ve ardından bir ağ bağdaştırıcısı seçin **sonraki**.

    ![](media/asdk-install/3.PNG)

5. DHCP veya BGPNAT01 sanal makine için statik ağ yapılandırması seçin.
    > [!TIP]
    > BGPNAT01 VM Azure yığını için NAT ve VPN özellikleri sağlayan sınır yönlendiricisi olur.

    - **DHCP** (varsayılan): sanal makine IP ağ yapılandırması DHCP sunucusundan alır.
    - **Statik**: DHCP Internet'e erişmek için Azure yığın için geçerli bir IP adresi atanamıyor, yalnızca bu seçeneği kullanın. **CIDR biçiminde (örneğin, 10.0.0.5/24) AltAğMaskesi uzunluğa sahip bir statik IP adresi belirtilmelidir**.
    - Türü geçerli bir **zaman sunucu IP** adresi. Bu alan Geliştirme Seti tarafından kullanılacak saat sunucusu ayarlar gereklidir. Bu parametre, geçerli saat sunucusu IP adresi olarak sağlanmalıdır. Sunucu adları desteklenmez.

      > [!TIP]
      > Bir saat sunucusu IP adresini bulmak için ziyaret [pool.ntp.org](http:\\pool.ntp.org) veya time.windows.com ping işlemi yapın. 

    - **İsteğe bağlı olarak**, aşağıdaki değerleri ayarlayın:
        - **VLAN kimliği**: VLAN kimliğini ayarlar Yalnızca AzS BGPNAT01 ve konak fiziksel ağ (ve Internet) erişmek için VLAN kimliği yapılandırmanız gerekir, bu seçeneği kullanın. 
        - **DNS ileticisi**: bir DNS sunucusu, Azure yığın dağıtımının bir parçası oluşturulur. Damga dışında adları çözümlemek için çözüm içinde izin vermek için mevcut altyapısını DNS sunucunuzun sağlar. Damga DNS sunucusu Bilinmeyen ad çözümleme isteklerinin bu sunucusuna iletir.

    ![](media/asdk-install/4.PNG)

6. Üzerinde **ağ arabirimi kartı özelliklerini doğrulama** sayfasında, bir ilerleme çubuğu görürsünüz. Doğrulama tamamlandığında tıklatın **sonraki**.

    ![](media/asdk-install/5.PNG)

9. Üzerinde **Özet** sayfasında, **dağıtma** Geliştirme Seti ana bilgisayara ASDK yüklemeyi başlatmak için.

    ![](media/asdk-install/6.PNG)

    > [!TIP]
    > Burada, Geliştirme Seti yüklemek için kullanılan PowerShell Kurulum komutları de kopyalayabilirsiniz. Şimdiye kadar gerekirse yararlıdır [ASDK PowerShell kullanarak ana bilgisayarda yeniden](asdk-deploy-powershell.md).

10. Bir Azure AD dağıtımı gerçekleştiriyorsanız, Kurulum başladıktan sonra birkaç dakika Azure AD genel yönetici hesabı kimlik bilgilerinizi girmeniz istenir.

    ![](media/asdk-install/7.PNG)

11. Dağıtım işlemi sırasında hangi zaman ana bilgisayarda otomatik olarak bir kez yeniden birkaç saat sürer. Dağıtımının ilerleme durumunu izlemek isterseniz, Geliştirme Seti konak yeniden başlatıldıktan sonra azurestack\AzureStackAdmin oturum açın. Dağıtım başarılı olduğunda, PowerShell konsolunda görüntüler: **tamamlandı: eylem 'Dağıtım'**. 
    > [!IMPORTANT]
    > Makine etki alanına katılan sonra yerel yönetici oturum açarsanız, dağıtımının ilerleme durumunu göremezsiniz. Dağıtımı yeniden çalıştır bulunamadı, bunun yerine azurestack\AzureStackAdmin çalıştığını doğrulamak oturum açın.

    ![](media/asdk-install/8.PNG)

Tebrikler, ASDK başarıyla yüklediniz.

Dağıtım için herhangi bir nedenle başarısız olursa şunları yapabilirsiniz [dağıtmanız](asdk-redeploy.md) baştan ya da kullanım aşağıdaki PowerShell, aynı yükseltilmiş PowerShell penceresinden son başarılı adımı dağıtımından yeniden başlatmak için komutlar:

  ```powershell
  cd C:\CloudDeployment\Setup
  .\InstallAzureStackPOC.ps1 -Rerun
  ```

## <a name="next-steps"></a>Sonraki adımlar
[Dağıtım yapılandırma sonrası](asdk-post-deploy.md)