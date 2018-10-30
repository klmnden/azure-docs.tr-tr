---
title: Azure sanal makine seri Konsolu | Microsoft Docs
description: Azure sanal makineleri için çift yönlü seri konsol.
services: virtual-machines-linux
documentationcenter: ''
author: harijay
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 09/11/2018
ms.author: harijay
ms.openlocfilehash: 57abb01d70929144a8457a04ebc0caf9aecaa61c
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50212412"
---
# <a name="virtual-machine-serial-console"></a>Sanal makine seri Konsolu


Azure'da sanal makine seri konsolu, Linux sanal makineleri için metin tabanlı bir konsol erişim sağlar. Bu seri bağlantı, COM1, bir sanal makinenin ağ veya işletim sistemi durumu bağımsız olan sanal makineye erişim sağlayarak sanal makinenin seri bağlantı noktası sağlamaktır. Bir sanal makine şu anda için seri konsoluna erişim yalnızca Azure Portalı aracılığıyla yapılması ve VM katkıda bulunanı olan kullanıcılar için veya sanal makineye erişimi yukarıda izin verilmiyor. 

Windows Vm'leri için seri konsol belgeleri [Buraya](../windows/serial-console.md).

> [!Note] 
> Sanal makineler için seri konsol genel Azure bölgelerinde genel kullanıma sunulmuştur. Bu noktada seri konsol henüz Azure kamu veya Azure China Bulutları içinde kullanılamaz.


## <a name="prerequisites"></a>Önkoşullar 

* Kaynak Yönetimi dağıtım modeline kullanıyor olmanız gerekir. Klasik dağıtımlar desteklenmez. 
* Sanal makineniz olmalıdır [önyükleme tanılaması](boot-diagnostics.md) etkin - aşağıdaki ekran görüntüsüne bakın.

    ![](./media/virtual-machines-serial-console/virtual-machine-serial-console-diagnostics-settings.png)

* Seri konsol kullanarak bir Azure hesabınızın olması gerekir [katkıda bulunan rolü](../../role-based-access-control/built-in-roles.md) VM için ve [önyükleme tanılaması](boot-diagnostics.md) depolama hesabı. 
* Seri konsol indirmesindeki olduğunuz sanal makine de parola tabanlı bir hesabı olmalıdır. İle bir tane oluşturabilirsiniz [parolayı Sıfırla](https://docs.microsoft.com/azure/virtual-machines/extensions/vmaccess#reset-password) VM erişimi uzantısı - işlevselliğini aşağıdaki ekran görüntüsüne bakın.

    ![](./media/virtual-machines-serial-console/virtual-machine-serial-console-reset-password.png)

* Linux dağıtımları için özel ayarları için bkz: [seri Konsolu Linux distro kullanılabilirlik](#serial-console-linux-distro-availability)



## <a name="get-started-with-serial-console"></a>Seri konsol ile çalışmaya başlama
Sanal makineler için seri konsol üzerinden erişilebilir, yalnızca [Azure portalında](https://portal.azure.com). Karşıladığınızdan emin olun [önkoşulları](#prerequisites) yukarıda. Portal aracılığıyla sanal makineler için seri konsoluna erişmek için adımlar aşağıdadır:

  1. Azure portalını açın
  1. (Bu, sanal Makinenizin parola kimlik doğrulaması kullanan bir kullanıcının varsa atlayın) Kullanıcı adı/parola kimlik doğrulaması ile kullanıcı dikey penceresinde "Parolayı Sıfırla" tıklayarak ekleme
  1. Sol taraftaki menüde, sanal makineleri seçin.
  1. VM listesinde tıklayın. VM için genel bakış sayfası açılır.
  1. Destek + sorun giderme bölümüne aşağı kaydırın ve "Seri konsol" seçeneğine tıklayın. Seri konsolu ile yeni bir bölme açılır ve bağlantıyı başlatın.

![](./media/virtual-machines-serial-console/virtual-machine-linux-serial-console-connect.gif)

### 

> [!NOTE] 
> Seri konsol yapılandırılmış bir parolayla yerel bir kullanıcı gerektirir. Şu anda yalnızca bir SSH ortak anahtarı ile yapılandırılan VM'ler için seri konsol oturum açmak mümkün olmayacaktır. Parola ile yerel bir kullanıcı oluşturmak için kullanın [VM erişimi uzantısı](https://docs.microsoft.com/azure/virtual-machines/linux/using-vmaccess-extension), "parola sıfırlama portalı içinde" tıklayarak Portalı'nda ve bir parolayla yerel bir kullanıcı oluşturun.
> Hesabınızda tarafından yönetici parolasını sıfırlayabilir [tek kullanıcı moduna bırakmak GRUB'ı kullanarak](./serial-console-grub-single-user-mode.md).

## <a name="serial-console-linux-distro-availability"></a>Seri konsol Linux distro kullanılabilirlik
Okuma ve seri bağlantı noktasına konsol iletileri yazma, düzgün bir şekilde seri konsol için sırada, konuk işletim sistemi yapılandırılması gerekir. Çoğu [desteklenen Azure Linux dağıtımı](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) varsayılan olarak yapılandırılmış seri Konsolu. Yalnızca Azure portalında seri konsol bölümüne tıklayarak konsoluna erişim sağlar. 

Distro      | Seri konsol erişimi
:-----------|:---------------------
Red Hat Enterprise Linux    | Red Hat Enterprise Linux görüntüleri Azure'da sunulan, varsayılan olarak etkin konsol erişimi. 
CentOS      | Azure'da centOS görüntülerinden, varsayılan olarak etkin konsol erişimi. 
Ubuntu      | Azure'da ubuntu görüntülerinde varsayılan olarak etkin konsol erişebilir.
CoreOS      | Azure'da CoreOS görüntülerinden, varsayılan olarak etkin konsol erişimi.
SUSE        | Azure'da sunulan yeni SLES görüntüleri, varsayılan olarak etkin konsol erişebilir. Azure'da SLES'ın daha eski sürümleri (10 veya aşağıdaki) kullanıyorsanız uygulayın [KB makalesi](https://www.novell.com/support/kb/doc.php?id=3456486) seri konsol etkinleştirmek için. 
Oracle Linux        | Azure'da Oracle Linux görüntüleri, varsayılan olarak etkin konsol erişebilir.
Özel Linux görüntüleri     | Seri konsol özel Linux VM görüntünüz için etkinleştirmek için konsol erişimi etkinleştirmek `/etc/inittab` bir terminal çalıştırılacak `ttyS0`. Bu inittab dosyasına eklemek için bir örnek aşağıda verilmiştir: `S0:12345:respawn:/sbin/agetty -L 115200 console vt102`. Düzgün bir şekilde özel görüntü oluşturma hakkında daha fazla bilgi için bkz. [azure'da bir Linux VHD'si oluşturup yükleme](https://aka.ms/createuploadvhd). Özel bir çekirdek oluşturuyorsanız etkinleştirme dikkate alınması gereken bazı çekirdek bayraklar `CONFIG_SERIAL_8250=y` ve `CONFIG_MAGIC_SYSRQ_SERIAL=y`. Yapılandırma dosyası, genellikle daha fazla araştırma için /boot/ altında bulunur.

## <a name="common-scenarios-for-accessing-serial-console"></a>Seri konsoluna erişmek için genel senaryolar 
Senaryo          | Seri konsol eylemleri                
:------------------|:-----------------------------------------
FSTAB dosyası bozuk | `Enter` devam etmek ve bir metin düzenleyici kullanarak fstab dosyasını düzeltin anahtarı. Bunun tek kullanıcı modunda olması gerekebilir. Bkz: [fstab sorunlarını gidermeye yönelik](https://support.microsoft.com/help/3206699/azure-linux-vm-cannot-start-because-of-fstab-errors) ve [GRUB ve tek kullanıcı modu erişmek için seri konsol kullanarak](serial-console-grub-single-user-mode.md) kullanmaya başlamak için.
Yanlış güvenlik duvarı kuralları | Seri konsola erişin, iptables düzeltin. 
Dosya Sistemi Bozulması/işaretleyin | Seri konsol erişmek ve dosya sistemi kurtarın. 
SSH/RDP yapılandırma sorunları | Seri Konsol erişim ve ayarları değiştirin. 
Sistem ağ kilitleme| Seri konsol sistemini yönetmek için portal aracılığıyla erişim. 
Önyükleme yükleyicisi ile etkileşim kurma | Seri konsol üzerinden GRUB erişim. Git [GRUB ve tek kullanıcı modu erişmek için seri konsol kullanarak](serial-console-grub-single-user-mode.md) kullanmaya başlamak için. 

## <a name="disable-serial-console"></a>Seri konsol devre dışı bırak
Varsayılan olarak, seri konsol erişimi tüm VM'ler için Etkin Abonelikler var. Abonelik düzeyinde veya VM düzeyi seri konsol devre dışı bırakabilir.

> [!Note] 
> Etkinleştirmek veya devre dışı seri konsol için bir abonelik için abonelik için yazma izinleri olmalıdır. Bu, içerir, ancak için yönetici veya sahip rollerinin sınırlı değildir. Özel roller ayrıca yazma izinlerine sahip.

### <a name="subscription-level-disable"></a>Abonelik düzeyinde devre dışı bırak
Seri konsol devre dışı bırakılabilir bir tüm abonelik tarafından için [devre dışı konsol REST API çağrısı](https://aka.ms/disableserialconsoleapi). API belgeleri sayfasında "Try It" kullanılabilen İşlevler, devre dışı bırakın ve bir abonelik için seri konsol etkinleştirmek için kullanabilir. Girin, `subscriptionId`, "varsayılan" `default` alan ve Çalıştır'a tıklayın. Azure CLI komutları henüz kullanıma sunulmamıştır ve daha sonraki bir tarihte ulaşır. [REST API çağrısı burada deneyin](https://aka.ms/disableserialconsoleapi).

![](./media/virtual-machines-serial-console/virtual-machine-serial-console-rest-api-try-it.png)

Alternatif olarak, Cloud Shell'de (bash komutları gösterilen) aşağıdaki komutları kümesini kullanabilir. devre dışı bırakmak için etkinleştirme ve devre dışı bırakılmış bir aboneliği için seri konsol durumunu görüntüleyin. 

* Bir abonelik için seri konsol devre dışı durumunu almak için:
    ```azurecli-interactive
    $ export ACCESSTOKEN=($(az account get-access-token --output=json | jq .accessToken | tr -d '"')) 

    $ export SUBSCRIPTION_ID=$(az account show --output=json | jq .id -r)

    $ curl "https://management.azure.com/subscriptions/$SUBSCRIPTION_ID/providers/Microsoft.SerialConsole/consoleServices/default?api-version=2018-05-01" -H "Authorization: Bearer $ACCESSTOKEN" -H "Content-Type: application/json" -H "Accept: application/json" -s | jq .properties
    ```
* Seri konsol bir abonelik için devre dışı bırakmak için:
    ```azurecli-interactive
    $ export ACCESSTOKEN=($(az account get-access-token --output=json | jq .accessToken | tr -d '"')) 

    $ export SUBSCRIPTION_ID=$(az account show --output=json | jq .id -r)

    $ curl -X POST "https://management.azure.com/subscriptions/$SUBSCRIPTION_ID/providers/Microsoft.SerialConsole/consoleServices/default/disableConsole?api-version=2018-05-01" -H "Authorization: Bearer $ACCESSTOKEN" -H "Content-Type: application/json" -H "Accept: application/json" -s -H "Content-Length: 0"
    ```
* Seri konsol bir abonelik için etkinleştirmek için:
    ```azurecli-interactive
    $ export ACCESSTOKEN=($(az account get-access-token --output=json | jq .accessToken | tr -d '"')) 

    $ export SUBSCRIPTION_ID=$(az account show --output=json | jq .id -r)

    $ curl -X POST "https://management.azure.com/subscriptions/$SUBSCRIPTION_ID/providers/Microsoft.SerialConsole/consoleServices/default/enableConsole?api-version=2018-05-01" -H "Authorization: Bearer $ACCESSTOKEN" -H "Content-Type: application/json" -H "Accept: application/json" -s -H "Content-Length: 0"
    ```

### <a name="vm-level-disable"></a>VM düzeyinde devre dışı bırak
Seri konsol belirli sanal makineler için bu sanal makinenin önyükleme tanılama ayarı devre dışı bırakılarak devre dışı bırakılabilir. Azure portalından önyükleme tanılaması yalnızca kapatmak ve sanal Makinenin seri konsol devre dışı bırakılır.

## <a name="serial-console-security"></a>Seri Konsolu güvenliği 

### <a name="access-security"></a>Erişimi güvenliği 
Seri konsol erişimi olan kullanıcılarla sınırlıdır [VM katkıda bulunanlar](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor) veya sanal makineye erişimi. AAD kiracınızın çok faktörlü kimlik doğrulaması gerektiren sonra aracılığıyla erişimini olduğu gibi seri konsoluna erişimi MFA ayrıca gerekir [Azure portalında](https://portal.azure.com).

### <a name="channel-security"></a>Kanalı güvenliği
İleri ve geri gönderilen tüm veriler kablo şifrelenir.

### <a name="audit-logs"></a>Denetim günlükleri
Seri konsola tüm erişim şu anda oturum [önyükleme tanılaması](https://docs.microsoft.com/azure/virtual-machines/linux/boot-diagnostics) sanal makinenin günlükleri. Bu günlükler için erişim sahibi ve Azure sanal makine yöneticisi tarafından denetlenen.  

>[!CAUTION] 
Konsolun içinden çalıştırma komutları içeriyorsa ya da parolalar, gizli dizileri, kullanıcı adlarını veya diğer bir form herhangi biri kişisel bilgilerin (PII) çıktı Konsolu için erişim parolası, oturumunuz açıkken, bu sanal makine önyükleme tanılaması için yazılır Tüm diğer görünen metni, uygulanması seri Konsolu scrollback işlevselliğinin bir parçası olarak birlikte günlüğe kaydeder. Bu günlükler döngüsel ve gizli dizileri ve/veya PII içerebilir, her şey için SSH konsolunu kullanmanın en iyi yöntemin izlenmesi önerilir ancak tanılama depolama hesabı için Okuma izinleri olan tek kişilere, onlara yönelik erişimi sahiptir. 

### <a name="concurrent-usage"></a>Eşzamanlı kullanım
Seri konsola bir kullanıcı bağlandığından ve başka bir kullanıcı, aynı sanal makineye erişimi başarıyla istekleri, ilk kullanıcı bağlantısı kesilir ve duran ve fiziksel konsol ve yeni bir bırakarak ilk kullanıcı benzer şekilde ikinci kullanıcı bağlı Kullanıcı oturan.

>[!CAUTION] 
Başka bir deyişle, bağlantısı kesilse kullanıcı kapatılacak değil! Bağlantıyı Kes (aracılığıyla SIGHUP veya benzer bir mekanizma) bağlı bir oturum kapatma zorunlu tutma becerisi hala Yol Haritası ' dir. Windows için otomatik bir zaman aşımı SAC içinde etkin ancak Linux için terminal zaman aşımı ayarını yapılandırabilirsiniz. Bunu yapmak için yalnızca ekleyin `export TMOUT=600` .bash_profile veya kullanıcı profili, 10 dakika sonra oturumu zaman aşımına konsolunda oturum.

## <a name="accessibility"></a>Erişilebilirlik
Erişilebilirlik bir anahtar Azure seri konsol biridir. Bu amaçla, seri konsol fare kullanmanız mümkün olmayabilir kişilerin yanı sıra visual ve işitme zorluğu yaşayan kişiler erişilebilir olduğunu belirlediniz.

### <a name="keyboard-navigation"></a>Klavye ile gezinme
Kullanım `tab` With Azure Portal'ın seri konsol arabirimi geçici olarak gezinmek için klavyenizdeki anahtar. Konumunuz ekranda vurgulanır. Seri konsol dikey pencerenin odağı bırakmak için basın `Ctrl + F6` klavyenizde.

### <a name="use-serial-console-with-a-screen-reader"></a>Seri konsol ekran okuyucuyla kullanma
Seri konsol ekran okuyucu desteği yerleşik olarak bulunur. Açık bir ekran okuyucu ile geçici olarak gezinmek sesli ekran okuyucu tarafından okunacak şu anda seçili düğme için alternatif metin izin verir.

## <a name="errors"></a>Hatalar
Doğası gereği geçici hataların çoğu ve adresleri bunlar genellikle seri konsol bağlantısı yeniden deneniyor. Aşağıdaki tabloda, hataları ve risk azaltma işlemleri listesini gösterir

Hata                            |   Risk azaltma 
:---------------------------------|:--------------------------------------------|
Önyükleme tanılama ayarları alınamadı. '<VMNAME>'. Seri konsol kullanmak için bu VM için o önyükleme tanılaması etkin emin olun. | Sanal makine olduğundan emin olun [önyükleme tanılaması](boot-diagnostics.md) etkin. 
Durdurulan serbest bırakılmış durumda vm'dir. VM'yi başlatın ve seri konsol bağlantısı yeniden deneyin. | Sanal makinenin seri konsol erişmek için başlatılmış durumda olması gerekir
Bu sanal makine seri konsolu kullanmak için gerekli izinlere sahip değil. En az olduğundan emin olun VM katkıda bulunan rol izinleri.| Seri konsol erişimi erişmek için belirli bir izni gerektirir. Bkz: [erişim gereksinimlerini](#prerequisites) için Ayrıntılar
Önyükleme tanılaması depolama hesabı için kaynak grubu belirlenemiyor '<STORAGEACCOUNTNAME>'. Bu VM için önyükleme tanılaması etkin ve bu depolama hesabına erişiminiz olduğunu doğrulayın. | Seri konsol erişimi erişmek için belirli bir izni gerektirir. Bkz: [erişim gereksinimlerini](#prerequisites) için Ayrıntılar
Web yuvası kapalı veya açılamadı. | Beyaz listeye gerekebilir `*.console.azure.com`. Daha ayrıntılı ancak uzun yaklaşımdır beyaz listeye [Microsoft Azure veri merkezi IP aralıkları](https://www.microsoft.com/en-us/download/details.aspx?id=41653), nispeten düzenli olarak değiştiği.
Bu sanal makinenin önyükleme tanılaması depolama hesabı erişirken 'Yasak' yanıt karşılaşıldı. | Bu önyükleme tanılama hesabı bir güvenlik duvarı bulunmadığından emin olun. İşleve seri konsol için bir erişilebilir önyükleme tanılaması depolama hesabı gereklidir.

## <a name="known-issues"></a>Bilinen sorunlar 
Seri konsol ile ilgili bazı sorunlar farkında duyuyoruz. Bu sorunlar ve risk azaltma için adımlar listesi aşağıda verilmiştir.

Sorun                           |   Risk azaltma 
:---------------------------------|:--------------------------------------------|
Ulaşmaktan bağlantı başlık satırında bir günlük göstermez sonra girin | Lütfen şu sayfaya bakın: [Hitting girin hiçbir şey yapmaz](https://github.com/Microsoft/azserialconsole/blob/master/Known_Issues/Hitting_enter_does_nothing.md). Özel VM, sağlamlaştırılmış gereç veya Linux düzgün bir şekilde seri bağlantı noktasına bağlanmak başarısız olmasına neden olan GRUB yapılandırma çalıştırıyorsanız, bu durum oluşabilir.
Yalnızca seri konsol metin (genellikle bir metin düzenleyicisi kullanarak sonra) ekran boyutu bir kısmını alır | Seri konsol penceresi boyutu hakkında anlaşması desteklemez ([RFC 1073](https://www.ietf.org/rfc/rfc1073.txt)) SIGWINCH sinyali yok olacak anlamına gönderilen ekran boyutu güncelleştirilecek ve VM terminalinizi 's boyutu olanağıyla olacaktır. İnstaling xterm ya da 'boyutlandırma' komutu veren bazı diğer benzer bir yardımcı programını öneririz. 'Yeniden boyutlandırma' çalıştıran bu düzeltir.
Çok uzun dizeler yapıştırma çalışmıyor | Seri konsol 2048 karakter terminale içine yapıştırdığınız dize uzunluğunu kısıtlar. Bu seri bağlantı noktası bant genişliği aşırı yüklenilmesini önlemek içindir.


## <a name="frequently-asked-questions"></a>Sık sorulan sorular 
**SORU. Nasıl geri bildirim gönderebilir miyim?**

A. Giderek bir sorun geri bildirim sağlamak https://aka.ms/serialconsolefeedback. Alternatif olarak aracılığıyla geri bildirim gönder (daha az tercih edilir), azserialhelp@microsoft.com veya sanal makine kategorisi http://feedback.azure.com

**SORU. Seri konsol kopyala/yapıştır destekliyor mu?**

A. Evet, bunu yapar. Kopyalama ve yapıştırma terminale Ctrl + Shift + C ve Ctrl + Shift + V kullanın.

**SORU. Seri konsol yerine bir SSH bağlantısı kullanabilir miyim?**

A. Bu teknik olarak mümkün görünebilir, ancak seri konsol öncelikle bir SSH aracılığıyla bağlantı mümkün olduğu durumlarda sorun giderme aracı olarak kullanılmak üzere tasarlanmıştır. Seri konsol bir SSH yerine iki nedenden dolayı kullanılmasından öneririz:

1. Seri konsol SSH aynı miktarda bant genişliği yok - daha fazla GUI yoğun etkileşimleri seri konsolunda zor olacaktır, böylece yalnızca metin bağlantı değildir.
1. Seri konsol erişimi şu anda yalnızca kullanıcı adı ve parola tarafından aşamasındadır. SSH anahtarları kullanıcı adı/parola birleşimleri çok daha güvenli olduğundan oturum açma güvenlik açısından SSH seri konsol öneririz.

**SORU. Kimler etkinleştirebilir veya seri konsol için Aboneliğim devre dışı?**

A. Etkinleştirmek veya devre dışı bir aboneliği genelinde düzeyinde seri konsol için abonelik yazma izinleri olmalıdır. Yazmak için izne sahip roller içerir ancak için yönetici veya sahip rollerinin sınırlı değildir. Özel roller ayrıca yazma izinlerine sahip.

**SORU. Sanal Makinem için seri konsol erişebilecek mi?**

A. Katkıda bulunan düzeyinde erişime sahip olmalıdır veya üzeri bir VM sanal makinenin seri konsol erişebilmek için. 

**SORU. Seri konsolumda herhangi bir şey ne yapmalıyım gösterilmiyor?**

A. Görüntünüzü seri konsol erişimi için büyük olasılıkla yanlış yapılandırılmış. Bkz: [seri Konsolu Linux distro kullanılabilirlik](#serial-console-linux-distro-availability) görüntünüzü seri konsol etkinleştirmek için yapılandırma hakkında ayrıntılar için.

**SORU. Seri konsol sanal makine ölçek kümeleri için kullanılabilir mi?**

A. Seri konsol erişimi için sanal makine ölçek kümesi örneklerine şu anda desteklenmiyor.

**SORU. My VM'yi yalnızca SSH anahtar kimlik kullanarak ayarlarım, hala seri konsoluna bağlanmak için sanal Makinem için kullanabilir miyim?** Yanıt: Evet. Tüm yapmanız gereken ayarlanmış şekilde bir kullanıcı adı/parola birleşimini SSH anahtarları, seri konsol gerektirmez. Portalda "Parolayı Sıfırla" dikey penceresini kullanarak ve seri konsol açmak için bu kimlik bilgilerini kullanarak bunu yapabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
* Seri Konsolu [önyükleme GRUB ve tek kullanıcı moduna gir](serial-console-grub-single-user-mode.md)
* Seri Konsolu [NMI ve SysRq çağrıları](serial-console-nmi-sysrq.md)
* Seri konsola kullanmayı öğrenin [GRUB çeşitli dağıtım paketlerini etkinleştir](https://blogs.msdn.microsoft.com/linuxonazure/2018/10/23/why-proactively-ensuring-you-have-access-to-grub-and-sysrq-in-your-linux-vm-could-save-you-lots-of-down-time/)
* Seri konsol için de kullanılabilir olan [Windows](../windows/serial-console.md) VM'ler
* Daha fazla bilgi edinin [önyükleme tanılaması](boot-diagnostics.md)