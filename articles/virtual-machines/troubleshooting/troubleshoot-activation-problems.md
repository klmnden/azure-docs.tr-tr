---
title: Azure'da Windows sanal makine etkinleştirme sorunlarını giderme | Microsoft Docs
description: Azure'da Windows sanal makine etkinleştirme sorunlarını düzeltmek için sorun giderme adımları sağlar.
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: genlin
manager: willchen
editor: ''
tags: top-support-issue, azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: troubleshooting
ms.date: 11/15/2018
ms.author: genli
ms.openlocfilehash: bc058cb3f27545b9e4ad8ef1062ca4d2fa4c9fa8
ms.sourcegitcommit: 1289f956f897786090166982a8b66f708c9deea1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2019
ms.locfileid: "67155151"
---
# <a name="troubleshoot-azure-windows-virtual-machine-activation-problems"></a>Azure Windows sanal makine etkinleştirme sorunlarını giderme

Özel görüntüden oluşturulan Azure Windows sanal makinesi (VM) etkinleştirirken sorun varsa, sorunu gidermek için bu belgede sağlanan bilgileri kullanabilirsiniz. 

## <a name="understanding-azure-kms-endpoints-for-windows-product-activation-of-azure-virtual-machines"></a>Windows ürün etkinleştirme, Azure sanal makineler için Azure KMS uç anlama

KMS etkinleştirme VM'nin bulunduğu bulut bölgesi bağlı olarak farklı uç noktalar Azure'ı kullanır. Bu sorun giderme kılavuzu kullanırken, bölgeniz için geçerli uygun KMS uç noktayı kullanın.

* Azure genel bulut bölgeleri: kms.core.windows.net:1688
* Azure Çin 21Vianet Ulusal bulut bölgeleri: kms.core.chinacloudapi.cn:1688
* Azure Almanya Ulusal bulut bölgeleri: kms.core.cloudapi.de:1688
* Azure ABD Devleti Ulusal bulut bölgeleri: kms.core.usgovcloudapi.net:1688

## <a name="symptom"></a>Belirti

Azure Windows VM etkinleştirmeyi denediğinizde bir hatayla karşılaştıysanız aşağıdaki örneğe benzer:

**Hata: 0xC004F074 yazılım LicensingService bilgisayarın etkinleştirilemediğini bildirdi. Hiçbir anahtar ManagementService (KMS) bağlantı kurulamadı. Lütfen ek bilgi için uygulama olay günlüğüne bakın.**

## <a name="cause"></a>Nedeni

Genellikle, Azure sanal makine etkinleştirme sorunlarını uygun KMS istemci kurulum anahtarı kullanarak Windows VM yapılandırılmamış veya Windows VM (kms.core.windows.net, bağlantı noktası 1688) Azure KMS hizmetine bir bağlantı sorunu varsa oluşur. 

## <a name="solution"></a>Çözüm

>[!NOTE]
>Siteden siteye VPN kullanıyorsanız ve zorlamalı tünel, bkz: [Azure'daki özel yollar KMS etkinleştirme'yle etkinleştirmek için zorlamalı tünel](https://blogs.msdn.com/b/mast/archive/2015/05/20/use-azure-custom-routes-to-enable-kms-activation-with-forced-tunneling.aspx). 
>
>ExpressRoute kullanıyorsanız ve sahip olduğunuz bir varsayılan rota yayımlanan, bkz: [ExpressRoute üzerinden etkinleştirmek Azure VM başarısız olabilir](https://blogs.msdn.com/b/mast/archive/2015/12/01/azure-vm-may-fail-to-activate-over-expressroute.aspx).

### <a name="step-1-configure-the-appropriate-kms-client-setup-key"></a>1\. adım yapılandırma uygun KMS istemci kurulum anahtarı

Özel görüntüden oluşturulan VM için sanal makine için uygun KMS istemci kurulum anahtarı yapılandırmanız gerekir.

1. Çalıştırma **slmgr.vbs/dlv komutunu** yükseltilmiş bir komut isteminde. Çıktıda açıklama değerini denetleyin ve ardından, perakende (Perakende kanal) veya (VOLUME_KMSCLIENT) toplu lisans medyasından oluşturulduğunu belirleyin:
  

    ```
    cscript c:\windows\system32\slmgr.vbs /dlv
    ```

2. **slmgr.vbs /dlv** alanında RETAIL kanalı gösteriliyorsa, [KMS istemci ayarı anahtarını](https://technet.microsoft.com/library/jj612867%28v=ws.11%29.aspx?f=255&MSPPError=-2147217396) kullanılmakta olan Windows Server sürümüne göre ayarlamak için aşağıdaki komutları çalıştırın ve etkinleştirmeyi yeniden denemeye zorlayın: 

    ```
    cscript c:\windows\system32\slmgr.vbs /ipk <KMS client setup key>

    cscript c:\windows\system32\slmgr.vbs /ato
     ```

    Örneğin, Windows Server 2016 Datacenter için aşağıdaki komutu çalıştırırsınız:

    ```
    cscript c:\windows\system32\slmgr.vbs /ipk CB7KF-BWN84-R7R2Y-793K2-8XDDG
    ```

### <a name="step-2-verify-the-connectivity-between-the-vm-and-azure-kms-service"></a>Adım 2 VM ve Azure KMS hizmeti arasındaki bağlantıyı doğrulama

1. İndirin ve ayıklayın [PSping](http:/technet.microsoft.com/sysinternals/jj729731.aspx) etkinleştirmez VM'deki yerel bir klasöre aracı. 

2. Başlat'a gidin, Windows PowerShell'i temel arama, Windows PowerShell sağ tıklayın ve ardından yönetici olarak çalıştır'ı seçin.

3. VM’nin doğru Azure KMS sunucusunu kullanacak şekilde yapılandırıldığından emin olun. Bunu yapmak için aşağıdaki komutu çalıştırın:
  

    ```powershell
    Invoke-Expression "$env:windir\system32\cscript.exe $env:windir\system32\slmgr.vbs /skms kms.core.windows.net:1688"
    ```

    Komut şu değeri döndürmelidir: Anahtar Yönetimi Hizmeti makine adı için kms.core.windows.net:1688 başarıyla ayarlandı.

4. KMS sunucusunda bağlantınız Psping kullanarak doğrulayın. Pstools.zip dosyasını ayıkladığınız klasöre geçin ve sonra aşağıdaki komutu çalıştırın:
  

    ```
    \psping.exe kms.core.windows.net:1688
    ```

  
   Çıktının sondan ikinci satırında şunu gördüğünüzden emin olun: Gönderilen = 4, alınan = 4, kayıp = 0 (% 0 kaybı olan).

   Kayıp 0 (sıfır)'dan büyükse, VM KMS sunucusu bağlantısı yok. Bu durumda, bir sanal ağda VM ise ve özel bir DNS sunucusu belirttiği, DNS sunucusunun emin olmanız gerekir kms.core.windows.net çözebilirsiniz. Veya kms.core.windows.net gideren bir DNS sunucusunu değiştirin.

   Sanal ağdan tüm DNS sunucularına kaldırırsanız, VM'lerin Azure'nın iç DNS hizmeti kullandığına dikkat edin. Bu hizmet kms.core.windows.net çözümleyebilir.
  
Ayrıca Konuk Güvenlik Duvarı'nı etkinleştirme girişimlerini engelleyen bir şekilde yapılandırılmamış doğrulayın.

1. Başarılı bağlantıyı kms.core.windows.net doğruladıktan sonra yükseltilmiş bir Windows PowerShell isteminde aşağıdaki komutu çalıştırın. Bu komut, etkinleştirmeyi birden çok kez dener.

    ```powershell
    1..12 | ForEach-Object { Invoke-Expression “$env:windir\system32\cscript.exe $env:windir\system32\slmgr.vbs /ato” ; start-sleep 5 }
    ```

Başarılı bir etkinleştirme aşağıdakine benzer bilgileri döndürür:

**Windows(R), Serverdatacentercore edition (12345678-1234-1234-1234-12345678) etkinleştiriliyor... Ürün başarıyla etkinleştirildi.**

## <a name="faq"></a>SSS 

### <a name="i-created-the-windows-server-2016-from-azure-marketplace-do-i-need-to-configure-kms-key-for-activating-the-windows-server-2016"></a>Azure Market'te Windows Server 2016 oluşturdum. Windows Server 2016'yı etkinleştirme için KMS anahtarı yapılandırmanız gerekiyor mu? 

 
Hayır. Azure Marketi görüntüde zaten yapılandırılmış uygun KMS istemci kurulum anahtarı vardır. 

### <a name="does-windows-activation-work-the-same-way-regardless-if-the-vm-is-using-azure-hybrid-use-benefit-hub-or-not"></a>VM veya Azure karma kullanım Avantajı'nı (HUB) kullanıyorsa, Windows etkinleştirme bakılmaksızın aynı şekilde çalışır? 

 
Evet. 
 

### <a name="what-happens-if-windows-activation-period-expires"></a>Windows etkinleştirme süresi dolarsa ne olur? 

 
Yetkisiz kullanım süresi doldu ve Windows hala etkin olduğunda, Windows Server 2008 R2 ve sonraki Windows sürümlerinde etkinleştirme hakkında ilave bildirimler gösterilir. Masaüstü duvar kağıdını siyah kalır ve Windows Update, güvenlik ve yalnızca kritik güncelleştirmeler, ancak isteğe bağlı değil güncelleştirmeleri yükler. Alt kısmındaki bildirimler bölümüne bakın [lisans koşulları](https://technet.microsoft.com/library/ff793403.aspx) sayfası.   

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun.

Hala yardıma ihtiyacınız varsa [desteğe](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorununuzun hızlıca çözülebilmesi için.
