---
title: Azure Stack 1903 güncelleştirmesi | Microsoft Docs
description: Yenilikler dahil olmak üzere, Azure Stack tümleşik sistemleri 1903 güncelleştirmesi hakkında bilinen sorunlar ve güncelleştirmeyi yüklemek nereye öğrenin.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/05/2019
ms.author: sethm
ms.reviewer: adepue
ms.lastreviewed: 04/05/2019
ms.openlocfilehash: 218af82d2385632e7e7a0e77060c5deb758d1e83
ms.sourcegitcommit: b4ad15a9ffcfd07351836ffedf9692a3b5d0ac86
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2019
ms.locfileid: "59057062"
---
# <a name="azure-stack-1903-update"></a>Azure Stack 1903 güncelleştirme

*Şunlara uygulanır Azure Stack tümleşik sistemleri*

Bu makalede 1903 güncelleştirme paketinin içeriğini açıklar. Güncelleştirme geliştirmeleri ve düzeltmeleri bu sürümü, Azure Stack için yeni özellikler içerir. Bu makalede ayrıca bu sürümdeki bilinen sorunlara açıklar ve güncelleştirmeyi indirmek için bir bağlantı içerir. Bilinen sorunlar doğrudan güncelleştirme işlemiyle ilgili sorunları ve yapı (yükleme sonrası) ile ayrılır.

> [!IMPORTANT]  
> Yalnızca Azure Stack tümleşik sistemleri için bu güncelleştirme paketidir. Bu güncelleştirme paketi için Azure Stack geliştirme Seti'ni geçerli değildir.

## <a name="build-reference"></a>Yapı Başvurusu

Azure Stack 1903 güncelleştirmenin yapı numarasıdır **1.1903.0.35**.

> [!IMPORTANT]
> 1903 yükü ASDK yayın içermez.

## <a name="hotfixes"></a>Düzeltmeler

Azure Stack düzeltmeleri düzenli olarak serbest bırakır. Yüklediğinizden emin olun [en son Azure Stack düzeltme](#azure-stack-hotfixes) Azure Stack için 1903 güncelleştirmeden önce 1902 için.

Azure Stack düzeltmeleri yalnızca Azure Stack tümleşik sistemleri için geçerlidir; üzerinde ASDK düzeltmelerini çalışmayın.

> [!TIP]  
> Aşağıdaki abone olmak *RSS* veya *Atom* Azure Stack düzeltmelerle birlikte kalmasını sağlamak için akışları:
> - [RSS](https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/rss)
> - [Atom](https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/atom)

### <a name="azure-stack-hotfixes"></a>Azure Stack düzeltmeler

- **1902**: [KB 4481548 – Azure Stack düzeltme 1.1902.2.73](https://support.microsoft.com/help/4494719)

## <a name="improvements"></a>Geliştirmeleri

- 1903 güncelleştirme yükü temel işletim sistemini barındıran Azure Stack içermeyen bileşenleri Azure Stack için bir güncelleştirme içerir. Bu, Kapsamınızın belirli güncelleştirmeler sağlar. Sonuç olarak, beklenen bu alır 1903 güncelleştirmesinin tamamlanması daha az zaman (yaklaşık 16 saat, ancak tam süreleri değişebilir). Çalışma zamanı bu azalma 1903 güncelleştirmesi için özeldir ve sonraki güncelleştirmeler işletim sistemi farklı çalışma zamanlarını olduğunu belirtmek için güncelleştirmeleri içerebilir. Gelecekteki güncelleştirmelerin güncelleştirmenin tamamlanması dahil yüküne bağlı olarak beklenen süresini üzerinde benzer yönergeler sağlar.

- Ağlarla ilgili değişiklikler önleyen bir hata düzeltildi **boşta kalma zaman aşımı (dakika)** değerini bir **genel IP adresi** etki alanından. Yaptığınız değişiklikleri bağımsız olarak, 4 dakika için varsayılan değer, böylece daha önce bu değer değişiklikler, yoksayıldı. Bu ayar, etkin tutma iletileri göndermek için istemcileri kullanmadan bir TCP bağlantı tutmak için kaç dakika açın denetler. Bu hata yalnızca örnek etkilenen Not düzeyi genel IP'ler, bir yük dengeleyiciye atanan değil genel IP'ler.

- Otomatik Düzeltme yaygın sorunların kesintisiz güncelleştirmelerin uygulanması dahil olmak üzere güncelleştirme altyapısı güvenilirlik geliştirmeleri.

- Algılama ve düzeltme düşük disk alanı koşulu iyileştirmeleri.

### <a name="secret-management"></a>Gizli dizi Yönetimi

- Azure Stack artık döndürme dış gizli döndürme için sertifikaları tarafından kullanılan kök sertifikanın destekler. Daha fazla bilgi için [bu makaleye bakın](azure-stack-rotate-secrets.md).

- 1903 iç gizli döndürme yürütmek için gereken süreyi azaltmak gizli döndürme için performans geliştirmeleri içerir.

## <a name="prerequisites"></a>Önkoşullar

> [!IMPORTANT]
> Yükleme [en son Azure Stack düzeltme](#azure-stack-hotfixes) 1903 için güncelleştirmeden önce 1902 (varsa) için.

- En son sürümünü kullandığınızdan emin olun [Azure Stack kapasite Planlayıcısı](https://aka.ms/azstackcapacityplanner) iş yükünüz planlama ve boyutlandırma yapmak için. En son sürüm hata düzeltmeleri içerir ve yayımlanan yeni özellikler sağlayan her Azure Stack güncelleştirme.

- Bu güncelleştirme yüklemesi başlamadan önce çalıştırması [Test AzureStack](azure-stack-diagnostic-test.md) bulunan tüm çalışma sorunlarını çözün ve Azure Stack durumunu doğrulamak için aşağıdaki parametreyi ile tüm uyarılar ve hatalar dahil olmak üzere. Ayrıca etkin Uyarıları gözden geçirin ve eylemi gerektiren tüm çözümleyin:

    ```PowerShell
   Test-AzureStack -Group UpdateReadiness  
    ```

- Azure Stack System Center Operations Manager tarafından yönetildiğinde güncelleştirdiğinizden emin olun [Microsoft Azure Stack için Yönetim Paketi](https://www.microsoft.com/download/details.aspx?id=55184) sürümüne 1.0.3.11 1903 uygulamadan önce.

- Azure Stack güncelleştirmesi için paket biçimi değiştiğinde **.bin/.exe/.xml** için **.zip/.xml** 1902 sürümünden başlayarak. Bağlı Azure Stack ölçek birimleri ile müşteriler **güncelleştirme kullanılabilir** portalında iletisi. Bağlı olmayan müşterileri, artık yalnızca indirin ve karşılık gelen .xml ile .zip dosyasını içeri aktarın.

<!-- ## New features -->

<!-- ## Fixed issues -->

<!-- ## Common vulnerabilities and exposures -->

## <a name="known-issues-with-the-update-process"></a>Güncelleştirme işlemi ile ilgili bilinen sorunlar

- Çalıştırdığınızda [Test AzureStack](azure-stack-diagnostic-test.md), temel kart yönetim denetleyicisi (BMC) bir uyarı iletisi görüntülenir. Bu uyarıyı güvenle yok sayabilirsiniz.

- <!-- 2468613 - IS --> Bu güncelleştirme yüklemesi sırasında başlık uyarılarla görebileceğiniz **hatası – şablon FaultType UserAccounts.New için eksik.** Bu uyarılar güvenle yok sayabilirsiniz. Bu güncelleştirme yüklemesi tamamlandıktan sonra uyarıları otomatik olarak kapanır.

## <a name="post-update-steps"></a>Güncelleştirme sonrası adımlar

- Bu güncelleştirme yüklendikten sonra geçerli düzeltmeleri yükleyin. Daha fazla bilgi için [düzeltmeleri](#hotfixes), hem de bizim [hizmet İlkesi](azure-stack-servicing-policy.md).  

- Rest şifreleme anahtarları, veri almak ve bunları Azure Stack dağıtımınıza dışında güvenli bir şekilde depolayın. İzleyin [anahtarlarını almak yönergeler](azure-stack-security-bitlocker.md).

## <a name="known-issues-post-installation"></a>Bilinen sorunlar (yükleme sonrası)

Bu derleme sürümü için yükleme sonrası bilinen sorunlar verilmiştir.

### <a name="portal"></a>Portal

<!-- 2930820 - IS ASDK --> 
- "Docker için" araması yaparsanız yönetici ve kullanıcı portalı yanlış öğesi döndürülür. Azure Stack'te kullanılamıyor. Bunu oluşturmayı denerseniz, bir hata göstergesi içeren bir dikey pencere görüntülenir. 

<!-- 2931230 – IS  ASDK --> 
- Kullanıcı aboneliği plan kaldırdığınızda bile, bir kullanıcı abonelikte eklenti planı eklendiği planları silinemiyor. Eklenti planı başvuru abonelikleri de silinene kadar plan kalır. 

<!-- TBD - IS ASDK --> 
- 1804 sürümü ile sunulan iki Yönetim abonelik türlerini kullanılmamalıdır. Abonelik türleridir **abonelik ölçümü**, ve **tüketim abonelik**. Bu abonelik türlerini 1804 sürümünden başlayarak yeni Azure Stack ortamlarında görülebilir ancak henüz kullanıma sunulmamıştır. Kullanmaya devam etmelidir **varsayılan sağlayıcı** abonelik türü.

<!-- 3557860 - IS ASDK --> 
- Kullanıcı abonelikleri sonuçlarında yalnız bırakılmış kaynakları siliniyor. Geçici bir çözüm olarak kullanıcı kaynaklar veya kaynak grubunun tamamını silin ve sonra kullanıcı abonelikleri silin.

<!-- 1663805 - IS ASDK --> 
- Azure Stack portalı kullanarak aboneliğinize izinleri görüntüleyemezsiniz. Geçici çözüm olarak, [izinleri doğrulamak için PowerShell](/powershell/module/azs.subscriptions.admin/get-azssubscriptionplan).

<!-- Daniel 3/28 -->
- Bir depolama hesabındaki bir bloba gidin ve açmaya çalıştığınızda, Kullanıcı Portalı'nda **erişim ilkesi** Gezinti ağacından sonraki pencereyi yüklenemiyordur.

<!-- Daniel 3/28 -->
- Kullanıcı portalında bir blobu kullanarak yüklemeye çalıştığınızda **OAuth(preview)** seçeneği, görev bir hata iletisiyle başarısız olur. Bu sorunu geçici olarak çözmek için kullanarak blobu karşıya yükleme **SAS** seçeneği.

- Azure Stack portallarda oturum oturum açıldığında, genel Azure portalı hakkındaki bildirimleri görebilirsiniz. Şu anda Azure Stack'e uygulamayın gibi bu bildirimleri güvenle yoksayabilirsiniz (örneğin, "- 1 yeni güncelleştirme aşağıdaki güncelleştirmeleri kullanıma sunuldu: Azure portal Nisan 2019 güncelleştirmesi").

<!-- ### Health and monitoring -->

### <a name="compute"></a>İşlem

- Yeni bir Windows sanal makine (VM) oluştururken, aşağıdaki hata görüntülenebilir:

   `'Failed to start virtual machine 'vm-name'. Error: Failed to update serial output settings for VM 'vm-name'`

   Bir VM'de önyükleme tanılamalarını etkinleştirir, ancak önyükleme tanılama depolama hesabınızı silerseniz bu hata oluşur. Bu sorunu çözmek için önceden kullanılmış şekilde aynı ada sahip depolama hesabını yeniden oluşturun.

<!-- 2967447 - IS, ASDK, to be fixed in 1902 -->
- Dağıtım için bir seçenek olarak, 7.2 CentOS tabanlı sanal makine ölçek kümesi oluşturma deneyimi sağlar. Bu görüntüyü Azure Stack üzerinde kullanılabilir olmadığından, dağıtımınız için başka bir işletim sistemi veya markette dağıtımdan önce indirdiğiniz başka bir CentOS görüntüsü belirten bir Azure Resource Manager şablonu kullanma işleci.  

<!-- TBD - IS ASDK --> 
- Güncelleştirme 1903 uyguladıktan sonra yönetilen disklere sahip VM'ler dağıtırken aşağıdaki sorunlarla karşılaşabilirsiniz:

   - Yönetilen disklerle bir VM dağıtma 1808 güncelleştirmeden önce Abonelik oluşturulurken bir iç hata iletisi ile başarısız olabilir. Hatayı gidermek için her abonelik için şu adımları izleyin:
      1. Kiracı Portalı'nda Git **abonelikleri** ve aboneliği bulunamıyor. Seçin **kaynak sağlayıcıları**, ardından **Microsoft.Compute**ve ardından **yeniden kaydettirin**.
      2. Aynı abonelik altında Git **erişim denetimi (IAM)**, doğrulayın **Azure Stack – yönetilen Disk** listelenir.
   - Bir konuk dizin ile ilişkili bir abonelik içindeki Vm'leri dağıtma, çok kiracılı bir ortam yapılandırdıysanız, bir iç hata iletisi ile başarısız olabilir. Hatayı gidermek için aşağıdaki adımları izleyin. [bu makalede](azure-stack-enable-multitenancy.md#registering-azure-stack-with-the-guest-directory) her Konuk dizinlerinizi yeniden yapılandırmak için.

- Bir Ubuntu 18.04 etkinleştirilmiş SSH yetkilendirme ile oluşturulan VM, oturum açmak için SSH anahtarları kullanmak izin vermez. Geçici bir çözüm olarak VM erişimi Linux uzantısı için SSH anahtarları sağladıktan sonra uygulamak için kullanmak veya parola tabanlı kimlik doğrulaması kullanın.

- Azure Stack artık 2.2.20 sürümden daha yüksek bir Windows Azure Linux aracıları desteklemektedir. Bu destek 1901 ve 1902 düzeltmenin bir parçası olan ve müşterilerin Azure ve Azure Stack arasında tutarlı linux görüntüleri tutmak olanak tanır.


- Bir donanım yaşam döngüsü ana bilgisayar (HLH) yoksa: Grup İlkesi ayarlamak zorunda 1902 derlemeden önce **Bilgisayar Yapılandırması\Windows Ayarları\Güvenlik Ayarları\Yerel İlkeler\Güvenlik Seçenekleri** için **Gönder NTLM'yi – NTLMv2 oturum güvenliği anlaşması kullanırsanız**. 1902 yapıdan beri olarak bırakmalısınız **tanımlanmamış** veya ayarlayın **yalnızca Gönder NTLMv2 yanıtı** (varsayılan değer olmayan). Aksi halde, bir PowerShell uzak oturumu oluşturmanız mümkün olmayacaktır ve göreceğiniz bir **erişim reddedildi** hata:

   ```shell
   PS C:\Users\Administrator> $session = New-PSSession -ComputerName x.x.x.x -ConfigurationName PrivilegedEndpoint  -Credential $cred
   New-PSSession : [x.x.x.x] Connecting to remote server x.x.x.x failed with the following error message : Access is denied. For more information, see the 
   about_Remote_Troubleshooting Help topic.
   At line:1 char:12
   + $session = New-PSSession -ComputerName x.x.x.x -ConfigurationNa ...
   +            ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
      + CategoryInfo          : OpenError: (System.Manageme....RemoteRunspace:RemoteRunspace) [New-PSSession], PSRemotingTransportException
      + FullyQualifiedErrorId : AccessDenied,PSSessionOpenFailed
   ```

### <a name="networking"></a>Ağ  

<!-- 3239127 - IS, ASDK -->
- Azure Stack portalında bir VM örneğine iliştirilmiş bir ağ bağdaştırıcısına bağlı bir IP yapılandırması için statik bir IP adresi değiştirdiğinizde bildiren bir uyarı iletisi görürsünüz 

    `The virtual machine associated with this network interface will be restarted to utilize the new private IP address...`

    Bu iletiyi güvenle yoksayabilirsiniz; sanal makine örneği yeniden başlatma olsa bile IP adresi değişir.

<!-- 3632798 - IS, ASDK -->
- Portalda, bir gelen güvenlik kuralı ekleyin ve seçerseniz, **hizmet etiketi** çeşitli seçenekler görüntülenir, kaynak olarak **kaynak etiketi** Azure Stack için kullanılabilir değil bir listesi. Azure Stack'te geçerli yalnızca seçenekleri aşağıdaki gibidir:

  - **Internet**
  - **VirtualNetwork**
  - **AzureLoadBalancer**
  
  Diğer seçenekler olarak Azure Stack'te kaynak etiketleri desteklenmez. Benzer şekilde, bir giden güvenlik kuralı ekleyin ve seçin, **hizmet etiketi** hedef aynı seçeneklerinin listesi olarak **kaynak etiketi** görüntülenir. Aynı yalnızca geçerli seçenekler şunlardır **kaynak etiketi**önceki listede açıklandığı gibi.

- Azure Stack'te aynı şekilde olarak genel Azure ağ güvenlik grupları (Nsg'ler) çalışmaz. Azure'da birden çok bağlantı noktası üzerinde bir NSG kuralı ayarlayabilirsiniz (kullanarak portal, PowerShell ve Resource Manager şablonlarını). Azure Stack'te ancak Şirket portalı aracılığıyla bir NSG kuralı birden çok bağlantı noktası ayarlanamıyor. Bu sorunu çözmek için bu ek kuralları ayarlamak için bir Resource Manager şablonu veya PowerShell kullanın.

<!-- 3203799 - IS, ASDK -->
- Azure Stack, 4'ten fazla ağ arabirimlerini (NIC'ler) bir sanal makine örneğine Bugün, örnek boyutu ne olursa olsun bağlanmasını desteklemez.

<!-- ### SQL and MySQL-->

### <a name="app-service"></a>App Service

<!-- 2352906 - IS ASDK --> 
- İlk Azure işlevinizi aboneliği oluşturmadan önce depolama kaynak sağlayıcısını kaydetmeniz gerekir.

<!-- ### Usage -->

 
<!-- #### Identity -->
<!-- #### Marketplace -->

## <a name="download-the-update"></a>Güncelleştirmeyi indirin

Azure Stack 1903 güncelleştirme paketinden indirebileceğiniz [burada](https://aka.ms/azurestackupdatedownload). 

Yalnızca bağlı senaryolarda Azure Stack dağıtımları güvenli bir uç nokta düzenli aralıklarla denetleyin ve bulut için bir güncelleştirme varsa, otomatik olarak bilgilendirme. Daha fazla bilgi için [Azure Stack için güncelleştirmeleri yönetme](azure-stack-updates.md#using-the-update-tile-to-manage-updates).

## <a name="next-steps"></a>Sonraki adımlar

- Azure Stack'te güncelleştirme yönetimi genel bakış için bkz. [Azure Stack genel bakış güncelleştirmeleri yönetme](azure-stack-updates.md).  
- Azure Stack güncelleştirmeleriyle uygulama hakkında daha fazla bilgi için bkz. [güncelleştirmelerini Azure Stack'te](azure-stack-apply-updates.md).
- Azure Stack tümleşik sistemleri ve desteklenen bir duruma sisteminizi tutmak için yapmanız gerekenlere bakım ilkeyi gözden geçirmek için bkz: [Azure Stack hizmet İlkesi](azure-stack-servicing-policy.md).  
- Ayrıcalıklı uç noktasına (CESARETLENDİRİCİ) izlemek ve güncelleştirmelerini sürdürmek üzere kullanmak için bkz [izleme ayrıcalıklı uç noktayı kullanarak Azure stack'teki güncelleştirmeleri](azure-stack-monitor-update.md).  
