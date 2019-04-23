---
title: Windows için Azure sanal makine seri Konsolu | Microsoft Docs
description: Azure Windows sanal makineler için çift yönlü seri konsol.
services: virtual-machines-windows
documentationcenter: ''
author: harijay
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 10/31/2018
ms.author: harijay
ms.openlocfilehash: e50243c15b5b783976374bc8b8861a0245ce1b05
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59996382"
---
# <a name="virtual-machine-serial-console-for-windows"></a>Windows için sanal makine seri Konsolu

Azure portalında sanal makine (VM) seri konsol, Windows sanal makineler için metin tabanlı bir konsol erişim sağlar. Bu seri bağlantı COM1, bu, sanal makinenin ağ veya işletim sisteminin durumunu bağımsız erişim sağlayan sanal makinenin seri bağlantı noktasına bağlanır. Bir sanal makinenin seri konsol erişimi, yalnızca Azure portalı kullanılarak yapılabilir. Sanal makineye sanal makine Katılımcısı veya daha yüksek bir erişim rolü olan kullanıcılar için izin verilir.

Linux VM'ler için seri konsol belgeleri için bkz [Linux için sanal makinenin seri konsol](serial-console-linux.md).

> [!NOTE]
> Sanal makineler için seri konsol genel Azure bölgelerinde genel kullanıma sunulmuştur. Henüz Azure kamu veya Azure China Bulutları kullanılabilir değil.


## <a name="prerequisites"></a>Önkoşullar

* Seri konsol eriştiğiniz VM kaynak yönetimi dağıtım modeline kullanmanız gerekir. Klasik dağıtımlar desteklenmez.

* Hangi erişim seri konsol VM olmalıdır [önyükleme tanılaması](boot-diagnostics.md) etkin.

    ![Önyükleme tanılama ayarları](../media/virtual-machines-serial-console/virtual-machine-serial-console-diagnostics-settings.png)

* Seri konsol kullanarak bir hesabınızın olması gerekir [sanal makine Katılımcısı rolü](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor) VM için ve [önyükleme tanılaması](boot-diagnostics.md) depolama hesabı.

* Sanal Makinenin seri konsol eriştiğiniz parola tabanlı bir hesabı olması gerekir. İle bir tane oluşturabilirsiniz [parolayı Sıfırla](https://docs.microsoft.com/azure/virtual-machines/extensions/vmaccess#reset-password) işlevi VM erişimi uzantısı. Seçin **parolayı Sıfırla** gelen **destek + sorun giderme** bölümü.


## <a name="get-started-with-the-serial-console"></a>Seri konsol ile çalışmaya başlama
Seri konsol sanal makineler için yalnızca Azure Portalı aracılığıyla erişilebilir:

  1. [Azure portalı](https://portal.azure.com) açın.
  1. Sol menüden **sanal makineler**.
  1. VM listesinde seçin. VM için genel bakış sayfası açılır.
  1. Ekranı aşağı kaydırarak **destek + sorun giderme** seçin ve bölüm **seri konsol**. Seri konsolu ile yeni bir bölme açılır ve bağlantısını başlatır.

## <a name="enable-serial-console-functionality"></a>Seri konsol işlevselliğini etkinleştirin

> [!NOTE]
> Seri konsol, herhangi bir şey görmediğinizden, sanal makinenizde, önyükleme tanılaması etkin olduğundan emin olun.

### <a name="enable-the-serial-console-in-custom-or-older-images"></a>Seri konsol özel veya eski resimlerdeki etkinleştir
Azure'da yeni Windows Server görüntülerini sahip [Özel Yönetim Konsolu](https://technet.microsoft.com/library/cc787940(v=ws.10).aspx) (SAC) varsayılan olarak etkin. SAC Windows server sürümlerinde desteklenir, ancak (örneğin, Windows 10, Windows 8 veya Windows 7) istemci sürümlerinde kullanılamaz.

(Şubat 2018 tarihinden önce oluşturulan) daha eski Windows Server görüntüleri için otomatik olarak seri konsol üzerinden Azure portalının komutu çalıştır özelliğini etkinleştirebilirsiniz. Azure portalında **komutu Çalıştır**, adlandırılmış komutu seçin **EnableEM** listeden.

![Komut listesini çalıştırın.](./media/virtual-machines-serial-console/virtual-machine-windows-serial-console-runcommand.png)

Alternatif olarak, Şubat 2018 tarihinden önce oluşturulan Windows sanal makineleri için seri konsoluna el ile etkinleştirmek için aşağıdaki adımları izleyin:

1. Uzak Masaüstü'nü kullanarak Windows sanal makinenize bağlanın
1. Bir yönetim komut isteminden aşağıdaki komutları çalıştırın:
    - `bcdedit /ems {current} on`
    - `bcdedit /emssettings EMSPORT:1 EMSBAUDRATE:115200`
1. SAC Konsolu etkinleştirilmesi için sistemi yeniden başlatın.

    ![SAC Konsolu](./media/virtual-machines-serial-console/virtual-machine-windows-serial-console-connect.gif)

Gerekirse, SAC çevrimdışı de etkinleştirilebilir:

1. Var olan VM'ye veri diski olarak yapılandırılmış SAC istediğiniz windows disk ekleyin.

1. Bir yönetim komut isteminden aşağıdaki komutları çalıştırın:
   - `bcdedit /store <mountedvolume>\boot\bcd /ems {default} on`
   - `bcdedit /store <mountedvolume>\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200`

#### <a name="how-do-i-know-if-sac-is-enabled"></a>SAC etkin olup olmadığını nasıl anlarım?

Varsa [SAC](https://technet.microsoft.com/library/cc787940(v=ws.10).aspx) değil etkin seri konsol SAC istemi görüntülemez. Bazı durumlarda, VM sistem durumu bilgileri görüntülenir ve diğer durumlarda boş olur. Şubat 2018 tarihinden önce oluşturulan bir Windows Server görüntüsü kullanıyorsanız, SAC büyük olasılıkla etkin olmayacaktır.

### <a name="enable-the-windows-boot-menu-in-the-serial-console"></a>Seri konsol içinde Windows önyükleme menüsünü etkinleştir

Windows önyükleme yükleyicisi istemleri seri konsolunda görüntülenecek etkinleştirmeniz gerekirse, aşağıdaki ek seçenekler önyükleme yapılandırma verilerinizi ekleyebilirsiniz. Daha fazla bilgi için [bcdedit](https://docs.microsoft.com/windows-hardware/drivers/devtest/bcdedit--set).

1. Windows sanal makinenize Uzak Masaüstü'nü kullanarak bağlanın.

1. Bir yönetim komut isteminden aşağıdaki komutları çalıştırın:
   - `bcdedit /set {bootmgr} displaybootmenu yes`
   - `bcdedit /set {bootmgr} timeout 10`
   - `bcdedit /set {bootmgr} bootems yes`

1. Önyükleme menüsünün etkinleştirilmesi sistemi yeniden başlatın

> [!NOTE]
> Önyükleme Yöneticisi menüsünü görüntülenecek kümesi zaman aşımı, işletim sistemi önyükleme süresini etkiler. 10 saniyelik zaman aşımı değeri çok kısa veya uzun olduğunu düşünüyorsanız, farklı bir değere ayarlayın.

## <a name="use-serial-console"></a>Seri Konsolu

### <a name="use-cmd-or-powershell-in-serial-console"></a>CMD veya PowerShell seri konsolunu kullanın.

1. Seri konsoluna bağlanın. Başarıyla bağlanmanız durumunda, istemidir **SAC >**:

    ![SAC için Bağlan](./media/virtual-machines-serial-console/virtual-machine-windows-serial-console-connect-sac.png)

1.  Girin `cmd` CMD örneğine sahip bir kanal oluşturmak için.

1.  Girin `ch -si 1` CMD örneği kanala geçin.

1.  Tuşuna **Enter**, yönetim izinleriyle oturum açma kimlik bilgilerini girin.

1.  Geçerli kimlik bilgilerini girdikten sonra CMD örneği açılır.

1.  Bir PowerShell örneği başlatmak için girin `PowerShell` CMD örneği ve ENTER tuşuna **Enter**.

    ![PowerShell örneği açın](./media/virtual-machines-serial-console/virtual-machine-windows-serial-console-powershell.png)

### <a name="use-the-serial-console-for-nmi-calls"></a>Seri konsol NMI çağrıları için kullanın.
Maskelenemez olmayan bir kesinti (NMI) bir sanal makinede yazılım yoksay olmaz bir sinyal oluşturmak için tasarlanmıştır. Tarihsel olarak, NMIs belirli yanıt süreleri gerektiren sistemleri donanım sorunları izlemek için kullanılır. Bugün, programcılar ve sistem yöneticileri bir mekanizma NMI hata ayıklama veya yanıt vermeyen sistemleri gidermek için genellikle kullanın.

Seri konsol, komut çubuğunda klavye simgesini kullanarak bir Azure sanal makinesi için bir NMI göndermek için kullanılabilir. NMI aldıktan sonra sanal makine yapılandırması sistemin nasıl yanıt vereceğini denetler. Windows için kilitlenme yapılandırılabilir ve bir bellek dökümü dosyasına bir NMI alırken oluşturun.

![NMI Gönder](../media/virtual-machines-serial-console/virtual-machine-windows-serial-console-nmi.png) <br>

Bir NMI aldığında bir kilitlenme bilgi döküm dosyası oluşturmak için Windows yapılandırma hakkında daha fazla bilgi için bkz. [bir NMI kilitlenme bilgi döküm dosyası oluşturmak nasıl](https://support.microsoft.com/help/927069/how-to-generate-a-complete-crash-dump-file-or-a-kernel-crash-dump-file).

### <a name="use-function-keys-in-serial-console"></a>Seri konsol içinde işlev tuşlarını kullanın
İşlev tuşları, Windows vm'lerinde seri Konsolu kullanımı için etkinleştirilir. Seri konsol açılır F8 kolayca Gelişmiş Önyükleme ayarlar menüsünü girme kolaylık sunar, ancak seri konsol diğer tüm işlev tuşlarını ile uyumludur. Basmanız gerekebilir **Fn** + **F1** (ya da F2, F3, vb.) bilgisayarı bağlı olarak klavyenizde seri konsolundan kullanıyorsanız.

### <a name="use-wsl-in-serial-console"></a>Seri konsol WSL kullanımda
Seri konsol içinde kullanmak için Windows Server 2019 çalıştırıyorsanız WSL etkinleştirmek olası veya sonraki bir sürümü Ayrıca, bu nedenle Linux (WSL) için Windows alt sistemi veya üzeri, Windows Server 2019 için etkinleştirildi. Bu, Linux komutlarını konusunda de kullanıcılar için yararlı olabilir. Windows Server için WSL etkinleştirmek yönergeler için bkz: [Yükleme Kılavuzu](https://docs.microsoft.com/windows/wsl/install-on-server).

### <a name="restart-your-windows-vm-within-serial-console"></a>Seri konsol içinde Windows VM'nizi yeniden başlatın
VM'NİZİN seri konsol içinde gezinme için güç düğmesi ve "VM yeniden Başlat" ı tıklatarak başlatabilirsiniz. Bu VM yeniden başlatır ve yeniden başlatma ile ilgili Azure portalında bir bildirim görürsünüz.

Seri konsol deneyimi çıkmadan sanal makinenizin önyükleme menüye erişmek için burada isteyebileceğiniz durumlarda kullanışlıdır.

![Windows seri konsol yeniden başlatma](./media/virtual-machines-serial-console/virtual-machine-serial-console-restart-button-windows.gif)

## <a name="disable-serial-console"></a>Seri konsol devre dışı bırak
Varsayılan olarak, seri konsol erişimi tüm VM'ler için Etkin Abonelikler var. Seri konsol veya abonelik düzeyinde hem de VM düzeyinde devre dışı bırakabilirsiniz.

> [!NOTE]
> Etkinleştirmek veya seri konsol bir abonelik için devre dışı bırakmak için abonelik için yazma izinleri olmalıdır. Bu izinleri içerir ancak için yönetici veya sahip rollerinin sınırlı değildir. Özel roller ayrıca yazma izinlerine sahip olabilir.

### <a name="subscription-level-disable"></a>Abonelik düzeyinde devre dışı bırak
Seri konsol tüm bir abonelik için devre dışı bırakılabilir [devre dışı konsol REST API çağrısı](/rest/api/serialconsole/console/disableconsole). Kullanabileceğiniz **deneyin** işlevi devre dışı bırakın ve bir abonelik için seri konsol etkinleştirmek için bu API belgeleri sayfasında kullanılabilir. Abonelik Kimliğinizi girin **Subscriptionıd**, "varsayılan" girin **varsayılan**ve ardından **çalıştırma**. Azure CLI komutları henüz kullanılamamaktadır.

![REST API'yi deneyin](../media/virtual-machines-serial-console/virtual-machine-serial-console-rest-api-try-it.png)

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
> Bu, bağlantısı kesilen bir kullanıcı günlüğe kaydedilmeyecek olduğunu anlamına gelir. Bir oturum kapatma sonrasında bağlantıyı kes (SIGHUP veya benzer bir mekanizma kullanarak) zorunlu tutmak için hala yol haritasında bulunan yeteneğidir. Windows için SAC içinde etkin otomatik bir zaman aşımı yoktur; Linux için terminal zaman aşımı ayarını yapılandırabilirsiniz.

## <a name="accessibility"></a>Erişilebilirlik
Erişilebilirlik bir anahtar Azure seri konsol biridir. Bu amaçla, biz seri konsol görsel ve işitme engelli yanı için fare kullanmanız mümkün olmayabilir kişiler erişilebilir olduğunu geçtiğinden emin olduk.

### <a name="keyboard-navigation"></a>Klavye ile gezinme
Kullanım **sekmesini** anahtar klavyenizde seri konsol arabirimi Azure portalından gidebilirsiniz. Konumunuz ekranda vurgulanır. Seri konsol penceresinin odağı bırakmak için basın **Ctrl**+**F6** klavyenizde.

### <a name="use-the-serial-console-with-a-screen-reader"></a>Seri konsol ekran okuyucuyla kullanma
Seri konsol ekran okuyucu desteği yerleşik olarak sahiptir. Açık bir ekran okuyucu ile geçici olarak gezinmek sesli ekran okuyucu tarafından okunacak şu anda seçili düğme için alternatif metin izin verir.

## <a name="common-scenarios-for-accessing-the-serial-console"></a>Seri konsoluna erişmek için genel senaryolar

Senaryo          | Seri konsol eylemleri
:------------------|:-----------------------------------------
Yanlış güvenlik duvarı kuralları | Seri konsol ve düzeltme Windows Güvenlik duvarı kuralları erişin.
Dosya Sistemi Bozulması/işaretleyin | Seri konsol erişmek ve dosya sistemi kurtarın.
RDP yapılandırma sorunları | Seri konsola erişin, ayarları değiştirin. Daha fazla bilgi için [RDP belgeleri](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/remote-desktop-allow-access).
Sistem ağ kilitleme | Seri konsol sistemini yönetmek için Azure portalından erişim. İçinde listelenen bazı ağ komutları [Windows komutları: CMD ve PowerShell](serial-console-cmd-ps-commands.md).
Önyükleme yükleyicisi ile etkileşim kurma | Seri Konsolu aracılığıyla BCD erişim. Bilgi için [seri konsolunda Windows önyükleme menüsünü etkinleştir](#enable-the-windows-boot-menu-in-the-serial-console).


## <a name="errors"></a>Hatalar
Geçici hataların çoğu olduğundan, bağlantınızı yeniden deneniyor genellikle bunları düzeltebilir. Aşağıdaki tabloda, hataları ve risk azaltma işlemleri listesini gösterir.

Hata                            |   Risk azaltma
:---------------------------------|:--------------------------------------------|
Önyükleme tanılama ayarları alınamadı  *&lt;VMNAME&gt;*. Seri konsol kullanmak için bu VM için o önyükleme tanılaması etkin emin olun. | Sanal makine olduğundan emin olun [önyükleme tanılaması](boot-diagnostics.md) etkin.
Durdurulan serbest bırakılmış durumda vm'dir. VM'yi başlatın ve seri konsol bağlantısı yeniden deneyin. | Sanal makinenin seri konsol erişmek için başlatılmış durumda olması gerekir
Bu sanal makine seri konsolu kullanmak için gerekli izinlere sahip değil. En az olduğundan emin olun sanal makine Katılımcısı rolü izinleri.| Seri konsol erişimi belirli izinler gerektirir. Daha fazla bilgi için [önkoşulları](#prerequisites).
Önyükleme tanılaması depolama hesabı için kaynak grubu belirlenemiyor  *&lt;STORAGEACCOUNTNAME&gt;*. Bu VM için önyükleme tanılaması etkin ve bu depolama hesabına erişiminiz olduğunu doğrulayın. | Seri konsol erişimi belirli izinler gerektirir. Daha fazla bilgi için [önkoşulları](#prerequisites).
Bu sanal makinenin önyükleme tanılaması depolama hesabı erişirken "Yasak" yanıt karşılaşıldı. | Bu önyükleme tanılama hesabı bir güvenlik duvarı bulunmadığından emin olun. İşleve seri konsol için bir erişilebilir önyükleme tanılaması depolama hesabı gereklidir.
Web yuvası kapalı veya açılamadı. | Beyaz listeye gerekebilir `*.console.azure.com`. Daha ayrıntılı ancak uzun yaklaşımdır beyaz listeye [Microsoft Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653), nispeten düzenli olarak değiştiği.
Yalnızca sistem durumu bilgileri, bir Windows VM'ye bağlanırken gösterilir.| Özel Yönetim Konsolu, Windows görüntüsü için etkin değil, bu hata meydana gelir. Bkz: [eski veya özel görüntüleri seri konsolundan etkinleştirme](#enable-the-serial-console-in-custom-or-older-images) SAC, Windows VM'de el ile etkinleştirme hakkında yönergeler için. Daha fazla bilgi için [Windows durum sinyallerini](https://github.com/Microsoft/azserialconsole/blob/master/Known_Issues/Windows_Health_Info.md).

## <a name="known-issues"></a>Bilinen sorunlar
Seri konsol ile ilgili bazı sorunlar farkında duyuyoruz. Bu sorunlar ve risk azaltma için adımlar listesi aşağıda verilmiştir.

Sorun                             |   Risk azaltma
:---------------------------------|:--------------------------------------------|
Tuşuna basarak **Enter** sonra bağlantı başlığı görüntülenecek bir oturum açma istemine neden olmaz. | Daha fazla bilgi için [Hitting girin hiçbir şey yapmaz](https://github.com/Microsoft/azserialconsole/blob/master/Known_Issues/Hitting_enter_does_nothing.md). Özel VM, sağlamlaştırılmış gereç veya Windows düzgün bir şekilde seri bağlantı noktasına bağlanmak başarısız olmasına neden olan önyükleme yapılandırması çalıştırıyorsanız, bu hata oluşabilir. Windows 10 istemci VM çalıştırıyorsanız, yalnızca Windows Server Vm'lerinin EMS etkin olacak şekilde yapılandırılmış olduğundan, bu hata ayrıca ortaya çıkar.
Çekirdek hata ayıklamasını etkin olduğunda SAC komut istemi türü oluşturulamıyor. | VM ve çalıştırmak için RDP `bcdedit /debug {current} off` yükseltilmiş bir komut isteminden. RDP gerçekleştiremezsiniz, bunun yerine başka bir Azure VM için işletim sistemi diski ve çalıştırarak veri diski olarak bağlı durumdayken değiştirmek `bcdedit /store <drive letter of data disk>:\boot\bcd /debug <identifier> off`, ardından diski geri değiştirme.
Özgün içerik yinelenen bir karakter varsa PowerShell içinde SAC sonuçları üçüncü bir karakter yapıştırma. | Geçici bir çözüm için çalıştırma `Remove-Module PSReadLine` geçerli oturumun PSReadLine modülünü kaldırmak için. Bu eylem silmez veya modül kaldırın.
Bazı klavye girişleri garip SAC çıkış üretmesi (örneğin, **[A**, **[3 ~**). | [VT100](https://aka.ms/vtsequences) kaçış dizileri SAC istemi tarafından desteklenmez.
Uzun dizeler yapıştırma çalışmaz. | Seri konsol seri bağlantı noktası bant genişliği aşırı yüklemesini önlemek için 2048 karakter terminale içine yapıştırdığınız dize uzunluğunu kısıtlar.
Seri konsol ile bir depolama hesabı güvenlik duvarı çalışmıyor. | Seri konsol tasarıma göre depolama hesabı önyükleme tanılama depolama hesabı etkin güvenlik duvarları ile çalışmaz.


## <a name="frequently-asked-questions"></a>Sık sorulan sorular

**SORU. Nasıl geri bildirim gönderebilir miyim?**

A. Bir GitHub sorunu en oluşturarak geri bildirim sağlamak https://aka.ms/serialconsolefeedback. Alternatif olarak (daha az tercih edilir), aracılığıyla geri bildirim gönderebilirsiniz azserialhelp@microsoft.com veya sanal makine kategorisi https://feedback.azure.com.

**SORU. Seri konsol kopyala/yapıştır destekliyor mu?**

A. Evet. Kullanım **Ctrl**+**Shift**+**C** ve **Ctrl**+**Shift** + **V** kopyalayıp terminale yapıştırabilirsiniz.

**SORU. Kimler etkinleştirebilir veya seri konsol için Aboneliğim devre dışı?**

A. Etkinleştirmek veya aboneliği genelinde düzeyinde seri konsol devre dışı bırakmak için abonelik için yazma izinleri olmalıdır. Yönetici veya sahip rollerinin yazmak için izne sahip roller içerir. Özel roller ayrıca yazma izinlerine sahip olabilir.

**SORU. Sanal Makinem için seri konsol erişebilecek mi?**

A. Sanal makine Katılımcısı rolü olmalıdır veya sanal makinenin seri konsol erişimi için bir VM için daha yüksek.

**SORU. Seri konsolumda herhangi bir şey görüntülenmiyor ne yapmalıyım?**

A. Görüntünüzü seri konsol erişimi için büyük olasılıkla yanlış yapılandırılmış. Görüntünüzü seri konsol etkinleştirmek için yapılandırma hakkında daha fazla bilgi için bkz: [eski veya özel görüntüleri seri konsolundan etkinleştirme](#enable-the-serial-console-in-custom-or-older-images).

**SORU. Seri konsol sanal makine ölçek kümeleri için kullanılabilir mi?**

A. Seri konsol erişimi için sanal makine ölçek kümesi örneklerine şu anda desteklenmiyor.

## <a name="next-steps"></a>Sonraki adımlar
* CMD ve PowerShell komutları, Windows SAC içinde kullanabileceğiniz kapsamlı bir kılavuzu için bkz: [Windows komutları: CMD ve PowerShell](serial-console-cmd-ps-commands.md).
* Seri konsol için de kullanılabilir olan [Linux](serial-console-linux.md) VM'ler.
* Daha fazla bilgi edinin [önyükleme tanılaması](boot-diagnostics.md).
