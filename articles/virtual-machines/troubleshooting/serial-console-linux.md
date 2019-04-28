---
title: Linux için Azure sanal makine seri Konsolu | Microsoft Docs
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
ms.date: 10/31/2018
ms.author: harijay
ms.openlocfilehash: f407d87249c44ad3a4773b2cd8fc85ee09506ceb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60921426"
---
# <a name="virtual-machine-serial-console-for-linux"></a>Linux için sanal makine seri Konsolu

Azure portalında sanal makine (VM) seri konsol, Linux sanal makineleri için metin tabanlı bir konsol erişim sağlar. Bu seri bağlantı COM1, bu, sanal makinenin ağ veya işletim sisteminin durumunu bağımsız erişim sağlayan sanal makinenin seri bağlantı noktasına bağlanır. Bir sanal makinenin seri konsol erişimi, yalnızca Azure portalı kullanılarak yapılabilir. Sanal makineye sanal makine Katılımcısı veya daha yüksek bir erişim rolü olan kullanıcılar için izin verilir.

Windows Vm'leri için seri konsol belgeleri için bkz [Windows için sanal makinenin seri konsol](../windows/serial-console.md).

> [!NOTE]
> Sanal makineler için seri konsol genel Azure bölgelerinde genel kullanıma sunulmuştur. Henüz Azure kamu veya Azure China Bulutları kullanılabilir değil.


## <a name="prerequisites"></a>Önkoşullar

- Seri konsol eriştiğiniz VM kaynak yönetimi dağıtım modeline kullanmanız gerekir. Klasik dağıtımlar desteklenmez.

- Hangi erişim seri konsol VM olmalıdır [önyükleme tanılaması](boot-diagnostics.md) etkin.

    ![Önyükleme tanılama ayarları](./media/virtual-machines-serial-console/virtual-machine-serial-console-diagnostics-settings.png)

- Seri konsol kullanan bir hesabınızın olması gerekir [sanal makine Katılımcısı rolü](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor) VM için ve [önyükleme tanılaması](boot-diagnostics.md) depolama hesabı:

- Sanal Makinenin seri konsol eriştiğiniz parola tabanlı bir hesabı olması gerekir. İle bir tane oluşturabilirsiniz [parolayı Sıfırla](https://docs.microsoft.com/azure/virtual-machines/extensions/vmaccess#reset-password) işlevi VM erişimi uzantısı. Seçin **parolayı Sıfırla** gelen **destek + sorun giderme** bölümü.

- Linux dağıtımları için özel ayarları için bkz: [seri konsol Linux dağıtım kullanılabilirlik](#serial-console-linux-distribution-availability).



## <a name="get-started-with-the-serial-console"></a>Seri konsol ile çalışmaya başlama
Seri konsol sanal makineler için yalnızca Azure Portalı aracılığıyla erişilebilir:

  1. [Azure portalı](https://portal.azure.com) açın.

  1. Sol menüden **sanal makineler**.

  1. VM listesinde seçin. VM için genel bakış sayfası açılır.

  1. Ekranı aşağı kaydırarak **destek + sorun giderme** seçin ve bölüm **seri konsol**. Seri konsolu ile yeni bir bölme açılır ve bağlantısını başlatır.

     ![Linux seri konsol penceresi](./media/virtual-machines-serial-console/virtual-machine-linux-serial-console-connect.gif)


> [!NOTE]
> Seri konsol, yerel bir kullanıcı ile yapılandırılmış bir parola gerektirir. Yalnızca bir SSH ortak anahtarı ile yapılandırılan VM'ler seri konsola oturum açmanız mümkün olmayacaktır. Bir parola ile yerel bir kullanıcı oluşturmak için kullanın [VMAccess uzantısı](https://docs.microsoft.com/azure/virtual-machines/linux/using-vmaccess-extension), kullanılabildiği portalda seçerek **parolayı Sıfırla** Azure portalında ve bir parolayla yerel bir kullanıcı oluşturun.
> Hesabınızda tarafından yönetici parolasını sıfırlayabilir [GRUB'ı kullanarak tek kullanıcı moduna önyüklemesini](./serial-console-grub-single-user-mode.md).

## <a name="serial-console-linux-distribution-availability"></a>Seri konsol Linux dağıtım kullanılabilirlik
Seri konsol düzgün çalışması okuma ve seri bağlantı noktasına konsol iletileri yazma konuk işletim sisteminin yapılandırılması gerekir. Çoğu [destekli Azure Linux dağıtımları](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) varsayılan olarak yapılandırılmış seri Konsolu. Seçme **seri konsol** içinde **destek + sorun giderme** bölümde Azure portal'ın seri konsoluna erişim sağlar.

Dağıtım      | Seri konsol erişimi
:-----------|:---------------------
Red Hat Enterprise Linux    | Seri konsol erişimi varsayılan olarak etkindir.
CentOS      | Seri konsol erişimi varsayılan olarak etkindir.
Ubuntu      | Seri konsol erişimi varsayılan olarak etkindir.
CoreOS      | Seri konsol erişimi varsayılan olarak etkindir.
SUSE        | Azure'da sunulan yeni SLES görüntüleri, varsayılan olarak etkin seri konsol erişimi. Azure üzerinde eski sürümlerini (10 veya öncesi) SLES kullanıyorsanız, bkz. [KB makalesi](https://www.novell.com/support/kb/doc.php?id=3456486) seri konsol etkinleştirmek için.
Oracle Linux        | Seri konsol erişimi varsayılan olarak etkindir.
Özel Linux görüntüleri     | Seri konsol özel Linux VM görüntünüz için etkinleştirmek için konsol erişimi dosyasındaki etkinleştirme */etc/inittab* bir terminal çalıştırılacak `ttyS0`. Örneğin: `S0:12345:respawn:/sbin/agetty -L 115200 console vt102`. Düzgün bir şekilde özel görüntülerinizi oluşturmayla ilgili daha fazla bilgi için [azure'da bir Linux VHD'si oluşturup yükleme](https://aka.ms/createuploadvhd). Özel bir çekirdek oluşturuyorsanız, bu çekirdek bayraklar etkinleştirme göz önünde bulundurun: `CONFIG_SERIAL_8250=y` ve `CONFIG_MAGIC_SYSRQ_SERIAL=y`. Yapılandırma dosyası genellikle bulunan */boot/* yolu.

> [!NOTE]
> Seri konsol, herhangi bir şey görmediğinizden, sanal makinenizde, önyükleme tanılaması etkin olduğundan emin olun.

## <a name="common-scenarios-for-accessing-the-serial-console"></a>Seri konsoluna erişmek için genel senaryolar

Senaryo          | Seri konsol eylemleri
:------------------|:-----------------------------------------
Bozuk *FSTAB* dosyası | Tuşuna **Enter** devam etmek ve gidermek için bir metin düzenleyicisi kullanın. anahtar *FSTAB* dosya. Bunu yapmak için tek kullanıcı modunda olması gerekebilir. Daha fazla bilgi için seri konsol bölümüne bakın. [fstab sorunlarını gidermeye yönelik](https://support.microsoft.com/help/3206699/azure-linux-vm-cannot-start-because-of-fstab-errors) ve [GRUB ve tek kullanıcı modunda erişmek için kullanım seri konsol](serial-console-grub-single-user-mode.md).
Yanlış güvenlik duvarı kuralları |  İptables SSH bağlantısı engelleyecek şekilde yapılandırdıysanız, seri konsol ile sanal makinenize SSH gerek kalmadan etkileşim kurmak için kullanabilirsiniz. Daha fazla ayrıntı şu adreste bulunabilir: [iptables adam sayfa](https://linux.die.net/man/8/iptables). Benzer şekilde, siz firewalld SSH erişimi engelliyorsa, sanal Makinenin seri konsol üzerinden erişmek ve firewalld yeniden yapılandırın. Daha fazla ayrıntı bulunabilir [firewalld belgeleri](https://firewalld.org/documentation/).
Dosya Sistemi Bozulması/işaretleyin | Lütfen seri konsol bölümüne bakın [Azure Linux VM, dosya sistem hataları nedeniyle başlatılamıyor](https://support.microsoft.com/en-us/help/3213321/linux-recovery-cannot-ssh-to-linux-vm-due-to-file-system-errors-fsck) daha fazla bilgi için sorun giderme ayrıntıları seri konsol kullanarak dosya sistemleri bozuk.
SSH yapılandırma sorunları | Seri konsola erişin, ayarları değiştirin. Seri konsol bağımsız olarak sanal makinenin SSH yapılandırması kullanılabilir VM çalışacak şekilde ağ bağlantısının olmasını gerektirmez. Sorun giderme kılavuzu kullanılabilir [sorun giderme SSH bağlantıları için bir Azure Linux VM, hataları, başarısız veya reddedildi](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/troubleshoot-ssh-connection). Daha fazla ayrıntı bulabilirsiniz [ayrıntılı sorun giderme adımları için azure'da bir Linux VM'ye bağlanma sorunu SSH](./detailed-troubleshoot-ssh-connection.md)
Önyükleme yükleyicisi ile etkileşim kurma | GRUB Linux vm'nize erişmesine, VM'den seri konsol Dikey içinde yeniden başlatın. Daha fazla ayrıntı ve distro özgü bilgiler için bkz. [GRUB ve tek kullanıcı modunda erişmek için kullanım seri konsol](serial-console-grub-single-user-mode.md).

## <a name="disable-the-serial-console"></a>Seri konsol devre dışı bırak
Varsayılan olarak, seri konsol erişimi tüm VM'ler için Etkin Abonelikler var. Seri konsol abonelik düzeyinde veya VM düzeyinde devre dışı bırakabilirsiniz.

> [!NOTE]
> Etkinleştirmek veya seri konsol bir abonelik için devre dışı bırakmak için abonelik için yazma izinleri olmalıdır. Yönetici veya sahip rollerinin bu izinleri içerir. Özel roller ayrıca yazma izinlerine sahip olabilir.

### <a name="subscription-level-disable"></a>Abonelik düzeyinde devre dışı bırak
Seri konsol tüm bir abonelik için devre dışı bırakılabilir [devre dışı konsol REST API çağrısı](/rest/api/serialconsole/console/disableconsole). Kullanabileceğiniz **deneyin** işlevi devre dışı bırakın ve bir abonelik için seri konsol etkinleştirmek için bu API belgeleri sayfasında kullanılabilir. Abonelik Kimliğinizi girin **Subscriptionıd**, girin **varsayılan** için **varsayılan**ve ardından **çalıştırma**. Azure CLI komutları henüz kullanılamamaktadır.

![REST API'yi deneyin](./media/virtual-machines-serial-console/virtual-machine-serial-console-rest-api-try-it.png)

Alternatif olarak, devre dışı bırakma, etkinleştirme ve seri konsol bir abonelik için devre dışı durumunu görüntülemek için Cloud Shell'de aşağıdaki bash komutları kümesini kullanabilirsiniz:

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
Seri konsol için belirli bir VM'ye, sanal makinenin önyükleme tanılama ayarı devre dışı bırakılarak devre dışı bırakılabilir. Önyükleme tanılaması sanal Makinenin seri konsol devre dışı bırakmak için Azure portalından devre dışı bırakın.

## <a name="serial-console-security"></a>Seri konsol güvenlik

### <a name="access-security"></a>Erişimi güvenliği
Seri konsol erişimi için bir erişim rolüne sahip kullanıcılar sınırlıdır, [sanal makine Katılımcısı](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor) veya sanal makine için daha yüksek. Çok faktörlü kimlik doğrulaması (MFA), Azure Active Directory kiracısı gerektiren sonra seri konsolun erişim aracılığıyla olduğundan seri konsoluna erişimi MFA ayrıca gerekir [Azure portalında](https://portal.azure.com).

### <a name="channel-security"></a>Kanalı güvenliği
İleri ve geri gönderilen tüm veriler kablo şifrelenir.

### <a name="audit-logs"></a>Denetim günlükleri
Seri konsola tüm erişim şu anda oturum [önyükleme tanılaması](https://docs.microsoft.com/azure/virtual-machines/linux/boot-diagnostics) sanal makinenin günlükleri. Bu günlükler için erişim sahibi ve Azure sanal makine yöneticisi tarafından denetlenen.

> [!CAUTION]
> Konsolu için erişim parolası günlüğe kaydedilir. Ancak, bu konsolda Çalıştır komutları içeren veya parolaları, parola, kullanıcı adlarını veya diğer tür kişisel bilgileri (PII) çıktı, VM önyükleme tanılama günlüklerine yazılır. Bunlar tüm diğer görünen metni yanı sıra, seri konsolun kaydırma uygulamasının bir parçası olarak işlev yazılır. Bu günlükler döngüsel ve onlara yönelik erişimi yalnızca Kişiler tanılama depolama hesabı için Okuma izinlerine sahip olması. Ancak, gizli dizileri ve/veya PII içerebilir Uzak Masaüstü için herhangi bir şey kullanmanın en iyi yöntemin izlenmesi önerilir.

### <a name="concurrent-usage"></a>Eşzamanlı kullanım
Seri konsola bir kullanıcı bağlandığından ve başka bir kullanıcı, aynı sanal makineye erişimi başarıyla istekleri, ilk kullanıcı bağlantısı kesilir ve ikinci kullanıcı aynı oturuma bağlı.

> [!CAUTION]
> Bu, bağlantısı kesilen bir kullanıcı günlüğe kaydedilmeyecek olduğunu anlamına gelir. Bir oturum kapatma sonrasında bağlantıyı kes (SIGHUP veya benzer bir mekanizma kullanarak) zorunlu tutmak için hala yol haritasında bulunan yeteneğidir. Windows için özel yönetim konsolunda (SAC) etkin otomatik bir zaman aşımı yoktur; Ancak, Linux için terminal zaman aşımı ayarını yapılandırabilirsiniz. Bunu yapmak için ekleme `export TMOUT=600` içinde *.bash_profile* veya *profili* konsola oturum açmak için kullandığınız kullanıcı için dosya. Bu ayar, 10 dakika sonra oturum zaman aşımı olur.

## <a name="accessibility"></a>Erişilebilirlik
Erişilebilirlik bir anahtar Azure seri konsol biridir. Bu amaçla, biz seri konsol tam olarak erişilebilir olduğunu geçtiğinden emin olduk.

### <a name="keyboard-navigation"></a>Klavye ile gezinme
Kullanım **sekmesini** anahtar klavyenizde seri konsol arabirimi Azure portalından gidebilirsiniz. Konumunuz ekranda vurgulanır. Seri konsol penceresinin odağı bırakmak için basın **Ctrl**+**F6** klavyenizde.

### <a name="use-the-serial-console-with-a-screen-reader"></a>Seri konsol ekran okuyucuyla kullanma
Seri konsol ekran okuyucu desteği yerleşik olarak sahiptir. Açık bir ekran okuyucu ile geçici olarak gezinmek sesli ekran okuyucu tarafından okunacak şu anda seçili düğme için alternatif metin izin verir.

## <a name="errors"></a>Hatalar
Geçici hataların çoğu olduğundan, bağlantınızı yeniden deneniyor genellikle bunları düzeltebilir. Aşağıdaki tabloda, hataları ve risk azaltma işlemleri listesini gösterir.

Hata                            |   Risk azaltma
:---------------------------------|:--------------------------------------------|
Önyükleme tanılama ayarları alınamadı  *&lt;VMNAME&gt;*. Seri konsol kullanmak için bu VM için o önyükleme tanılaması etkin emin olun. | Sanal makine olduğundan emin olun [önyükleme tanılaması](boot-diagnostics.md) etkin.
Durdurulan serbest bırakılmış durumda vm'dir. VM'yi başlatın ve seri konsol bağlantısı yeniden deneyin. | Sanal Makinenin seri konsoluna erişmek için başlatılmış durumda olması gerekir.
Bu sanal makine seri konsolu ile kullanmak için gerekli izinlere sahip değil. En az olduğundan emin olun sanal makine Katılımcısı rolü izinleri.| Seri konsol erişimi belirli izinler gerektirir. Daha fazla bilgi için [önkoşulları](#prerequisites).
Önyükleme tanılaması depolama hesabı için kaynak grubu belirlenemiyor  *&lt;STORAGEACCOUNTNAME&gt;*. Bu VM için önyükleme tanılaması etkin ve bu depolama hesabına erişiminiz olduğunu doğrulayın. | Seri konsol erişimi belirli izinler gerektirir. Daha fazla bilgi için [önkoşulları](#prerequisites).
Web yuvası kapalı veya açılamadı. | Beyaz listeye gerekebilir `*.console.azure.com`. Daha ayrıntılı ancak uzun yaklaşımdır beyaz listeye [Microsoft Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653), nispeten düzenli olarak değiştiği.
Bu sanal makinenin önyükleme tanılaması depolama hesabı erişirken "Yasak" yanıt karşılaşıldı. | Bu önyükleme tanılama hesabı bir güvenlik duvarı yok emin olun. İşleve seri konsol için bir erişilebilir önyükleme tanılaması depolama hesabı gereklidir.

## <a name="known-issues"></a>Bilinen sorunlar
Seri konsol ile ilgili bazı sorunlar farkında duyuyoruz. Bu sorunlar ve risk azaltma için adımlar listesi aşağıda verilmiştir.

Sorun                           |   Risk azaltma
:---------------------------------|:--------------------------------------------|
Tuşuna basarak **Enter** sonra bağlantı başlığı görüntülenecek bir oturum açma istemine neden olmaz. | Daha fazla bilgi için [Hitting girin hiçbir şey yapmaz](https://github.com/Microsoft/azserialconsole/blob/master/Known_Issues/Hitting_enter_does_nothing.md). Özel VM, sağlamlaştırılmış gereç veya Linux düzgün bir şekilde seri bağlantı noktasına bağlanmak başarısız olmasına neden olan GRUB yapılandırma çalıştırıyorsanız, bu sorun oluşabilir.
Seri konsol metin yalnızca bir kısmını ekran boyutunu (genellikle bir metin düzenleyicisi kullanarak sonra) alır. | Seri konsol penceresi boyutu hakkında anlaşması desteklemez ([RFC 1073](https://www.ietf.org/rfc/rfc1073.txt)) SIGWINCH sinyali yok olacak anlamına gönderilen ekran boyutu güncelleştirilecek ve VM terminalinizi 's boyutu olanağıyla olacaktır. Xterm veya sunmak için benzer bir yardımcı programını yüklemek `resize` komutunu ve ardından çalıştırın `resize`.
Uzun dizeler yapıştırma çalışmaz. | Seri konsol seri bağlantı noktası bant genişliği aşırı yüklemesini önlemek için 2048 karakter terminale içine yapıştırdığınız dize uzunluğunu kısıtlar.
Seri konsol ile bir depolama hesabı güvenlik duvarı çalışmıyor. | Seri konsol tasarıma göre depolama hesabı önyükleme tanılama depolama hesabı etkin güvenlik duvarları ile çalışmaz.


## <a name="frequently-asked-questions"></a>Sık sorulan sorular

**SORU. Nasıl geri bildirim gönderebilir miyim?**

A. Bir GitHub sorunu en oluşturarak geri bildirim sağlamak https://aka.ms/serialconsolefeedback. Alternatif olarak (daha az tercih edilir), aracılığıyla geri bildirim gönderebilirsiniz azserialhelp@microsoft.com veya sanal makine kategorisi https://feedback.azure.com.

**SORU. Seri konsol kopyala/yapıştır destekliyor mu?**

A. Evet. Kullanım **Ctrl**+**Shift**+**C** ve **Ctrl**+**Shift** + **V** kopyalayıp terminale yapıştırabilirsiniz.

**SORU. Seri konsol yerine bir SSH bağlantısı kullanabilir miyim?**

A. Bu kullanım teknik olarak mümkün görünebilir, ancak seri konsol öncelikle burada SSH aracılığıyla bağlantı kurulamadığı durumlarda sorun giderme aracı olarak kullanılmak üzere tasarlanmıştır. Aşağıdaki nedenlerle bir SSH yerine seri konsol kullanılmasından öneririz:

- Seri konsol kadar bant genişliği olarak SSH yok. Daha fazla GUI yoğun etkileşimleri salt metin bir bağlantı olduğu için zordur.
- Seri konsol erişimi yalnızca bir kullanıcı adı ve parola kullanarak şu anda mümkündür. SSH anahtarları oturum açma güvenlik açısından, kullanıcı adı/parola birleşimleri daha güvenli olduğundan seri konsol SSH öneririz.

**SORU. Kimler etkinleştirebilir veya seri konsol için Aboneliğim devre dışı?**

A. Etkinleştirmek veya aboneliği genelinde düzeyinde seri konsol devre dışı bırakmak için abonelik için yazma izinleri olmalıdır. Yönetici veya sahip rollerinin yazmak için izne sahip roller içerir. Özel roller ayrıca yazma izinlerine sahip olabilir.

**SORU. Sanal Makinem için seri konsol erişebilecek mi?**

A. Sanal makine Katılımcısı rolü olmalıdır veya sanal makinenin seri konsol erişimi için bir VM için daha yüksek.

**SORU. Seri konsolumda herhangi bir şey görüntülenmiyor ne yapmalıyım?**

A. Görüntünüzü seri konsol erişimi için büyük olasılıkla yanlış yapılandırılmış. Görüntünüzü seri konsol etkinleştirmek için yapılandırma hakkında daha fazla bilgi için bkz: [seri konsol Linux dağıtım kullanılabilirlik](#serial-console-linux-distribution-availability).

**SORU. Seri konsol sanal makine ölçek kümeleri için kullanılabilir mi?**

A. Seri konsol erişimi için sanal makine ölçek kümesi örneklerine şu anda desteklenmiyor.

**SORU. My VM'yi yalnızca SSH anahtar kimlik kullanarak ayarlarım, yine de seri konsol VM'me bağlanmak için kullanabilir miyim?**

A. Evet. Seri konsol SSH anahtarları gerektirmediği için yalnızca bir kullanıcı adı/parola birleşimini ayarlayın gerekir. Bunu seçerek yapabilirsiniz **parolayı Sıfırla** Azure portalında ve seri konsola oturum açmak için bu kimlik bilgilerini kullanarak.

## <a name="next-steps"></a>Sonraki adımlar
* Seri konsola kullanın [GRUB tek kullanıcı modunda erişip](serial-console-grub-single-user-mode.md).
* İçin seri Konsolu [NMI ve SysRq çağrıları](serial-console-nmi-sysrq.md).
* Seri konsola kullanmayı öğrenin [GRUB çeşitli dağıtım paketlerini etkinleştirme](https://blogs.msdn.microsoft.com/linuxonazure/2018/10/23/why-proactively-ensuring-you-have-access-to-grub-and-sysrq-in-your-linux-vm-could-save-you-lots-of-down-time/).
* Seri konsol için de kullanılabilir olan [Windows Vm'leri](../windows/serial-console.md).
* Daha fazla bilgi edinin [önyükleme tanılaması](boot-diagnostics.md).

