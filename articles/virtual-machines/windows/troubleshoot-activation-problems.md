---
title: "Azure'da Windows sanal makine etkinleştirme sorunlarını giderme | Microsoft Docs"
description: "Azure'da Windows sanal makine etkinleştirme sorunlarını düzeltmek için sorun giderme adımları sağlar"
services: virtual-machines-windows, azure-resource-manager
documentationcenter: 
author: genlin
manager: willchen
editor: 
tags: top-support-issue, azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 11/03/2017
ms.author: genli
ms.openlocfilehash: 4f3a388e95d3689cafcd2462e821cb361c46989a
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2018
---
# <a name="troubleshoot-azure-windows-virtual-machine-activation-problems"></a>Azure Windows sanal makine etkinleştirme sorunlarını giderme

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Özel bir görüntüden oluşturulan Azure Windows sanal makine (VM) etkinleştirirken konusunda sorun yaşıyorsanız, sorunu gidermek için bu belgede sağlanan bilgileri kullanabilirsiniz. 

## <a name="symptom"></a>Belirti

Bir Azure Windows VM etkinleştirmeye çalıştığınızda aldığınız hata iletisi aşağıdaki örneğe benzer:

**Hata: 0xC004F074 yazılım LicensingService bilgisayarın etkinleştirilemediğini bildirdi. Hiçbir anahtar ManagementService (KMS) bağlantı kurulamadı. Lütfen ek bilgi için uygulama olay günlüğüne bakın.**

## <a name="cause"></a>Nedeni
Genellikle, Azure VM etkinleştirme sorunlar uygun KMS istemci kurulum anahtarı'nı kullanarak Windows VM yapılandırılmamış veya Windows VM (kms.core.windows.net, bağlantı noktası 1668) Azure KMS hizmetine bir bağlantı sorunu olup olmadığını oluşur. 

## <a name="solution"></a>Çözüm

>[!NOTE]
>Siteden siteye VPN kullanıyorsanız ve zorlanan tünel, bkz: [KMS etkinleştirme'yle etkinleştirmek için özel yollar kullanım Azure zorlamalı tünel](http://blogs.msdn.com/b/mast/archive/2015/05/20/use-azure-custom-routes-to-enable-kms-activation-with-forced-tunneling.aspx). 
>
>ExpressRoute kullanarak ve varsa varsayılan rota yayımlanan bkz [Azure VM ExpressRoute etkinleştirmek başarısız olabilir](http://blogs.msdn.com/b/mast/archive/2015/12/01/azure-vm-may-fail-to-activate-over-expressroute.aspx).

### <a name="step-1-configure-the-appropriate-kms-client-setup-key-for-windows-server-2016-and-windows-server-2012-r2"></a>1. adım yapılandırma uygun KMS istemci kurulum anahtarını (Windows Server 2016 ve Windows Server 2012 R2)

Windows Server 2016 veya Windows Server 2012 R2 özel bir görüntüden oluşturulan VM için VM için uygun KMS istemci kurulum anahtarı yapılandırmanız gerekir.

Bu adım Windows 2012 veya Windows 2008 R2 için geçerli değildir. Yalnızca Windows Server 2016 ve Windows Server 2012 R2 tarafından desteklenen Otomasyon sanal makine etkinleştirme (AVMA) özelliği kullanır.

1. Çalıştırma **slmgr.vbs/dlv** yükseltilmiş bir komut isteminde. Çıktıda açıklama değerini denetleyin ve perakende (Perakende kanal) veya toplu (VOLUME_KMSCLIENT) lisans ortamdan oluşturulduğu olup olmadığını belirleyin:
  
    ```
    cscript c:\windows\system32\slmgr.vbs /dlv
    ```

2. Varsa **slmgr.vbs/dlv** ayarlamak için aşağıdaki komutları çalıştırın, perakende kanal gösterir [KMS istemci kurulum anahtarı](https://technet.microsoft.com/library/jj612867%28v=ws.11%29.aspx?f=255&MSPPError=-2147217396) Windows Server sürümü için kullanılıyorsa ve etkinleştirmeyi yeniden deneyin zorla: 

    ```
    cscript c:\windows\system32\slmgr.vbs /ipk <KMS client setup key>

    cscript c:\windows\system32\slmgr.vbs /ato
     ```

    Örneğin, Windows Server 2016 Datacenter için aşağıdaki komutu çalıştırın:

    ```
    cscript c:\windows\system32\slmgr.vbs /ipk CB7KF-BWN84-R7R2Y-793K2-8XDDG
    ```

### <a name="step-2-verify-the-connectivity-between-the-vm-and-azure-kms-service"></a>2. adım VM ve Azure KMS hizmeti arasındaki bağlantıyı doğrulayın

1. İndirmeyi ve ayıklamayı [Psping](http:/technet.microsoft.com/en-us/sysinternals/jj729731.aspx) değil etkinleştirme VM yerel bir klasöre aracı. 

2. Başlat'a gidin, Windows PowerShell üzerinde arama, Windows PowerShell sağ tıklayın ve ardından yönetici olarak çalıştır'ı seçin.

3. VM doğru Azure KMS sunucusunu kullanacak şekilde yapılandırıldığından emin olun. Bunu yapmak için aşağıdaki komutu çalıştırın:
  
    ```
    iex “$env:windir\system32\cscript.exe $env:windir\system32\slmgr.vbs /skms
    kms.core.windows.net:1688
    ```
    Komutun döndürmesi: anahtar yönetimi hizmeti makine adı için kms.core.windows.net:1688 başarılı bir şekilde ayarlayın.

4. KMS sunucusu bağlantısı yoksa Psping kullanarak doğrulayın. Pstools.zip indirme ayıkladığınız klasöre geçin ve ardından şu komutu çalıştırın:
  
    ```
    \psping.exe kms.core.windows.net:1688
    ```
  
  Çıktı ikinci son satırında gördüğünüz emin olun: gönderilen = 4, alınan = 4, kaybolan = 0 (%0 kayıp).

  Kayıp 0'dan (sıfır) büyük ise, VM KMS sunucusu bağlantısı yok. Bu durumda, sanal bir ağa VM ise ve özel bir DNS sunucusu belirttiği, bu DNS sunucusuna emin olun kms.core.windows.net çözebilirsiniz. Veya DNS sunucusu kms.core.windows.net çözümlenmesi değiştirin.

  Sanal ağdan tüm DNS sunucularına kaldırırsanız, sanal makineleri Azure'nın iç DNS hizmeti kullandığına dikkat edin. Bu hizmet kms.core.windows.net çözümleyebilir.
  
Ayrıca, Konuk Güvenlik Duvarı'nı etkinleştirme girişimlerini engelleyebilecek bir biçimde yapılandırılmamış doğrulayın.

5. Kms.core.windows.net başarılı bağlantı doğruladıktan sonra o yükseltilmiş Windows PowerShell komut isteminde aşağıdaki komutu çalıştırın. Bu komut etkinleştirme birden çok kez çalışır.

    ```
    1..12 | % { iex “$env:windir\system32\cscript.exe $env:windir\system32\slmgr.vbs /ato” ; start-sleep 5 }
    ```

Başarılı bir etkinleştirme aşağıdakine benzer bilgiler verir:

**Windows(R), Serverdatacentercore edition (12345678-1234-1234-1234-12345678) etkinleştiriliyor... Ürün başarıyla etkinleştirildi.**

## <a name="faq"></a>SSS 

### <a name="i-created-the-windows-server-2016-from-azure-marketplace-do-i-need-to-configure-kms-key-for-activating-the-windows-server-2016"></a>Windows Server 2016 Azure Marketi'nden oluşturdum. Windows Server 2016 etkinleştirme için KMS anahtarı yapılandırmanız gerekiyor mu? 
 
Hayır. Azure Market görüntüde zaten yapılandırılmış uygun KMS istemci kurulum anahtarı vardır. 

### <a name="does-windows-activation-work-the-same-way-regardless-if-the-vm-is-using-azure-hybrid-use-benefit-hub-or-not"></a>VM veya Azure karma kullanımı Avantajı (HUB) kullanıyorsa, Windows etkinleştirme bakılmaksızın aynı şekilde çalışır? 
 
Evet. 
 
### <a name="what-happens-if-windows-activation-period-expires"></a>Windows etkinleştirme süresi dolarsa ne olur? 
 
Yetkisiz kullanım süresi dolmuştur ve Windows hala etkin olduğunda, Windows Server 2008 R2 ve sonraki Windows sürümlerini etkinleştirme hakkında ek bildirimleri gösterir. Masaüstü duvar kağıdı siyah kalır ve Windows Update, güvenlik ve yalnızca kritik güncelleştirmeler, ancak isteğe bağlı değil güncelleştirmeleri yükler. Ekranın alt kısmındaki bildirimleri bölümüne bakın [lisans koşulları](http://technet.microsoft.com/en-us/library/ff793403.aspx) sayfası.   

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun.
Hala yardıma gereksiniminiz varsa [desteğine başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) hızla çözümlenen sorunu almak için.
