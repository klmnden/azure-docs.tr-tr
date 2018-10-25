---
title: Azure sanal makine seri Konsolu | Microsoft Docs
description: Azure sanal makineleri için çift yönlü seri konsol.
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
ms.date: 10/22/2018
ms.author: harijay
ms.openlocfilehash: facd9be037894932e516e8294e36b6b0e55374c8
ms.sourcegitcommit: f6050791e910c22bd3c749c6d0f09b1ba8fccf0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50024437"
---
# <a name="virtual-machine-serial-console"></a>Sanal makine seri Konsolu


Azure'da sanal makine seri konsolu, Windows sanal makineler için metin tabanlı bir konsol erişim sağlar. Bu seri bağlantı, COM1, bir sanal makinenin ağ veya işletim sistemi durumu bağımsız olan sanal makineye erişim sağlayarak sanal makinenin seri bağlantı noktası sağlamaktır. Bir sanal makine şu anda için seri konsoluna erişim yalnızca Azure Portalı aracılığıyla yapılması ve VM katkıda bulunanı olan kullanıcılar için veya sanal makineye erişimi yukarıda izin verilmiyor. 

Linux VM'ler için seri konsol belgeleri [Buraya](serial-console-linux.md).

> [!NOTE] 
> Sanal makineler için seri konsol genel Azure bölgelerinde genel kullanıma sunulmuştur. Bu noktada seri konsol henüz Azure kamu veya Azure China Bulutları içinde kullanılamaz.

 

## <a name="prerequisites"></a>Önkoşullar 

* Kaynak Yönetimi dağıtım modeline kullanıyor olmanız gerekir. Klasik dağıtımlar desteklenmez. 
* Sanal makinesinin olmalıdır [önyükleme tanılaması](boot-diagnostics.md) etkin 

    ![](../media/virtual-machines-serial-console/virtual-machine-serial-console-diagnostics-settings.png)

* Seri konsol kullanarak hesabı olmalıdır [katkıda bulunan rolü](../../role-based-access-control/built-in-roles.md) VM için ve [önyükleme tanılaması](boot-diagnostics.md) depolama hesabı. 
* Sanal makinenin seri konsol erişimi de parola tabanlı bir hesabı olmalıdır. İle bir tane oluşturabilirsiniz [parolayı Sıfırla](https://docs.microsoft.com/azure/virtual-machines/extensions/vmaccess#reset-password) VM erişimi uzantısı - işlevselliğini aşağıdaki ekran görüntüsüne bakın.

    ![](../media/virtual-machines-serial-console/virtual-machine-serial-console-reset-password.png)

## <a name="get-started-with-serial-console"></a>Seri konsol ile çalışmaya başlama
Sanal makineler için seri konsol üzerinden erişilebilir, yalnızca [Azure portalında](https://portal.azure.com). Portal aracılığıyla sanal makineler için seri konsoluna erişmek için adımlar aşağıdadır.

  1. Azure portalını açın
  2. Sol taraftaki menüde, sanal makineleri seçin.
  3. VM listesinde tıklayın. VM için genel bakış sayfası açılır.
  4. Destek + sorun giderme bölümüne aşağı kaydırın ve "Seri konsol" seçeneğine tıklayın. Seri konsolu ile yeni bir bölme açılır ve bağlantıyı başlatın.

## <a name="enable-serial-console-in-custom-or-older-images"></a>Seri konsol eski veya özel görüntüleri etkinleştir
Azure'da yeni Windows Server görüntülerini olacaktır [Özel Yönetim Konsolu](https://technet.microsoft.com/library/cc787940(v=ws.10).aspx) (SAC) varsayılan olarak etkin. SAC Windows server sürümlerinde desteklenir, ancak istemci sürümleri (örneğin, Windows 10, Windows 8 veya Windows 7) kullanılamıyor. Şubat 2018 tarihinden önce oluşturulan Windows sanal makineleri için seri konsol etkinleştirmek için lütfen aşağıdaki adımları kullanın: 

1. Windows sanal makinenize Uzak Masaüstü aracılığıyla bağlanma
2. Bir yönetim komut isteminden aşağıdaki komutları çalıştırın. 
* `bcdedit /ems {current} on`
* `bcdedit /emssettings EMSPORT:1 EMSBAUDRATE:115200`
3. SAC Konsolu etkinleştirilmesi için sistemi yeniden başlatın

![](/media/virtual-machines-serial-console/virtual-machine-windows-serial-console-connect.gif)

Gerekirse, SAC çevrimdışı de etkinleştirilebilir:

1. Var olan sanal makineye veri diski olarak yapılandırılmış SAC istediğiniz windows disk ekleyin. 
2. Bir yönetim komut isteminden aşağıdaki komutları çalıştırın. 
* `bcdedit /store <mountedvolume>\boot\bcd /ems {default} on`
* `bcdedit /store <mountedvolume>\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200`

### <a name="how-do-i-know-if-sac-is-enabled"></a>SAC etkin olup olmadığını nasıl anlarım?

Varsa [SAC](https://technet.microsoft.com/library/cc787940(v=ws.10).aspx) etkin seri konsol SAC istemi gösterilmez. Bazı durumlarda, VM sistem durumu bilgileri gösterilir ve diğer durumlarda boş olacaktır. Şubat 2018 tarihinden önce oluşturulan bir Windows Server görüntüsü kullanıyorsanız büyük olasılıkla SAC etkinleştirilmeyecek.

## <a name="enable-the-windows-boot-menu-in-serial-console"></a>Seri konsol içinde Windows önyükleme menüsünü etkinleştir 

Windows Önyükleme Yükleyicisi'ni etkinleştirmek gerekiyorsa göstermek için seri konsoldan, önyükleme yapılandırma verileri için aşağıdaki ek seçenekleri ekleyebilirsiniz ister. Bkz: [bcdedit](https://docs.microsoft.com/windows-hardware/drivers/devtest/bcdedit--set) daha fazla ayrıntı için

1. Windows sanal makinenize Uzak Masaüstü aracılığıyla bağlanma
2. Bir yönetici komut isteminden aşağıdaki komutları çalıştırın. 
* `bcdedit /set {bootmgr} displaybootmenu yes`
* `bcdedit /set {bootmgr} timeout 10`
* `bcdedit /set {bootmgr} bootems yes`
3. Önyükleme menüsünün etkinleştirilmesi sistemi yeniden başlatın

> [!NOTE] 
> Önyükleme Yöneticisi menüsünün gösterilmesi ayarladığınız zaman aşımı, işletim sistemi önyükleme zamanı gelecekte etkiler. Bazı Önyükleme Yöneticisi seri konsol görünür olduğundan emin olmak için 10 ikinci zaman aşımı eklemek için kabul edilebilir olabilir, ancak diğerleri daha kısa veya uzun bir zaman aşımı isteyebilirsiniz. Zaman aşımı değeri yararlanacağınız bir değere ayarlayın.

## <a name="use-serial-console-for-nmi-calls-in-windows-vms"></a>Windows vm'lerinde NMI çağrıları için seri Konsolu
Maskelenemez olmayan bir kesinti (NMI) yazılımı bir sanal makinede değil yoksayacak bir sinyal oluşturmak için tasarlanmıştır. Tarihsel olarak, NMIs belirli yanıt süreleri gerektiren sistemleri donanım sorunları izlemek için kullanılır.  Bugün, programcılar ve sistem yöneticileri bir mekanizma NMI hata ayıklama veya askıya sistemler gidermek için genellikle kullanın.

Seri konsol, aşağıda gösterilen komut çubuğunda klavye simgesini kullanarak bir Azure sanal makine bir NMI göndermek için kullanılabilir. NMI teslim sonra sanal makine yapılandırması sistemin nasıl yanıt vereceğini denetleyin. Windows için kilitlenme yapılandırılabilir ve bir NMI alırken bir bellek dökümü oluşturun.

![](../media/virtual-machines-serial-console/virtual-machine-windows-serial-console-nmi.png) <br>

Kilitlenme bilgi dökümü bir NMI aldığında oluşturmak için Windows yapılandırma hakkında daha fazla bilgi için bkz: [tam kilitlenme bilgi döküm dosyası veya bir çekirdek kilitlenme dökümü dosyalarının Windows tabanlı bir sistemde bir NMI kullanarak oluşturma](https://support.microsoft.com/en-us/help/927069/how-to-generate-a-complete-crash-dump-file-or-a-kernel-crash-dump-file)

## <a name="open-cmd-or-powershell-in-serial-console"></a>CMD veya Powershell seri konsolundan açın

1. Seri konsoluna bağlanın. Başarılı bir şekilde seri konsoluna bağlanmak, görürsünüz **SAC >** aşağıdaki ekran görüntüsünde gösterilmektedir:

    ![SAC için Bağlan](./media/virtual-machines-serial-console/virtual-machine-windows-serial-console-connect-sac.png)

3.  Tür `cmd` CMD örneğine sahip bir kanal oluşturmak için. 
4.  Tür `ch -si 1` CMD örneği kanala geçin. 
5.  ENTER tuşuna basın ve ardından yönetici izinlerine sahip oturum açma kimlik bilgilerinizi girin.
6.  Geçerli kimlik bilgilerini girdikten sonra CMD örneği açılır.
7.  PowerShell örneği başlatmak için aşağıdakileri yazın `PowerShell` CMD örneği ve ardından Enter tuşuna basın. 

    ![PowerShell örneği açın](./media/virtual-machines-serial-console/virtual-machine-windows-serial-console-powershell.png)


## <a name="disable-serial-console"></a>Seri konsol devre dışı bırak
Varsayılan olarak, seri konsol erişimi tüm VM'ler için Etkin Abonelikler var. Abonelik düzeyinde veya VM düzeyi seri konsol devre dışı bırakabilir.

> [!NOTE]       
> Etkinleştirmek veya devre dışı seri konsol için bir abonelik için abonelik için yazma izinleri olmalıdır. Bu, içerir, ancak için yönetici veya sahip rollerinin sınırlı değildir. Özel roller ayrıca yazma izinlerine sahip.

### <a name="subscription-level-disable"></a>Abonelik düzeyinde devre dışı bırak
Seri konsol devre dışı bırakılabilir bir tüm abonelik tarafından için [devre dışı konsol REST API çağrısı](https://aka.ms/disableserialconsoleapi). API belgeleri sayfasında "Try It" kullanılabilen İşlevler, devre dışı bırakın ve bir abonelik için seri konsol etkinleştirmek için kullanabilir. Girin, `subscriptionId`, "varsayılan" `default` alan ve Çalıştır'a tıklayın. Azure CLI komutları henüz kullanıma sunulmamıştır ve daha sonraki bir tarihte ulaşır. [REST API çağrısı burada deneyin](https://aka.ms/disableserialconsoleapi).

![](../media/virtual-machines-serial-console/virtual-machine-serial-console-rest-api-try-it.png)

Alternatif olarak, Cloud Shell'de (bash komutları gösterilen) aşağıdaki komutları kümesini kullanabilir. devre dışı bırakmak için etkinleştirme ve seri konsol bir abonelik için devre dışı durumunu görüntüleyin. 

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
Konsolun içinden çalıştırma komutları içeriyorsa ya da parolalar, gizli dizileri, kullanıcı adlarını veya diğer bir form herhangi biri kişisel bilgilerin (PII) çıktı Konsolu için erişim parolası, oturumunuz açıkken, bu sanal makine önyükleme tanılaması için yazılır günlükleri, tüm diğer görünen metni, seri konsolun kaydırma uygulamasının bir parçası olarak birlikte işlevselliği yedekleyin. Bu günlükler döngüsel ve, gizli dizileri ve/veya PII içerebilir Uzak Masaüstü için herhangi bir şey kullanmanın en iyi yöntemin izlenmesi önerilir ancak tanılama depolama hesabı için Okuma izinleri olan tek kişilere, onlara yönelik erişimi sahiptir. 

### <a name="concurrent-usage"></a>Eşzamanlı kullanım
Seri konsola bir kullanıcı bağlandığından ve başka bir kullanıcı, aynı sanal makineye erişimi başarıyla istekleri, ilk kullanıcı bağlantısı kesilir ve duran ve fiziksel konsol ve yeni bir bırakarak ilk kullanıcı benzer şekilde ikinci kullanıcı bağlı Kullanıcı oturan.

>[!CAUTION] 
Başka bir deyişle, bağlantısı kesilse kullanıcı kapatılacak değil! Bağlantıyı Kes (aracılığıyla SIGHUP veya benzer bir mekanizma) bağlı bir oturum kapatma zorunlu tutma becerisi hala Yol Haritası ' dir. Windows için yoktur SAC içinde etkin otomatik bir zaman aşımı ancak Linux için terminal zaman aşımı ayarını yapılandırabilirsiniz. 

## <a name="common-scenarios-for-accessing-serial-console"></a>Seri konsoluna erişmek için genel senaryolar 
Senaryo          | Seri konsol eylemleri                
:------------------|:-----------------------------------------
Yanlış güvenlik duvarı kuralları | Seri konsol ve düzeltme Windows Güvenlik duvarı kuralları erişin. 
Dosya Sistemi Bozulması/işaretleyin | Seri konsol erişmek ve dosya sistemi kurtarın. 
RDP yapılandırma sorunları | Seri Konsol erişim ve ayarları değiştirin. Git [RDP belgeleri](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/remote-desktop-allow-access) kullanmaya başlamak için.
Sistem ağ kilitleme| Seri konsol sistemini yönetmek için portal aracılığıyla erişim. İçinde listelenen bazı ağ komutları [seri Konsolu CMD ve PowerShell belgeleri](serial-console-cmd-ps-commands.md). 
Önyükleme yükleyicisi ile etkileşim kurma | Seri konsol üzerinden BCD erişim. Git [seri konsol içinde gösterilecek etkinleştirme önyükleme menüsü](#enabling-boot-menu-to-show-in-the-serial-console) kullanmaya başlamak için. 

## <a name="accessibility"></a>Erişilebilirlik
Erişilebilirlik bir anahtar Azure seri konsol biridir. Bu amaçla, seri konsol fare kullanmanız mümkün olmayabilir kişilerin yanı sıra visual ve işitme zorluğu yaşayan kişiler erişilebilir olduğunu belirlediniz.

### <a name="keyboard-navigation"></a>Klavye ile gezinme
Kullanım `tab` With Azure Portal'ın seri konsol arabirimi geçici olarak gezinmek için klavyenizdeki anahtar. Konumunuz ekranda vurgulanır. Seri konsol dikey pencerenin odağı bırakmak için basın `Ctrl + F6` klavyenizde.

### <a name="use-serial-console-with-a-screen-reader"></a>Seri konsol ekran okuyucuyla kullanma
Seri konsol ekran okuyucu desteği yerleşik olarak bulunur. Açık bir ekran okuyucu ile geçici olarak gezinmek sesli ekran okuyucu tarafından okunacak şu anda seçili düğme için alternatif metin izin verir.

## <a name="errors"></a>Hatalar
Çoğu hataların doğası ve bağlantı adresi bunlar yeniden deneniyor geçicidir. Aşağıdaki tabloda, hataları ve risk azaltma işlemleri listesini gösterir

Hata                            |   Risk azaltma 
:---------------------------------|:--------------------------------------------|
Önyükleme tanılama ayarları alınamadı. '<VMNAME>'. Seri konsol kullanmak için bu VM için o önyükleme tanılaması etkin emin olun. | Sanal makine olduğundan emin olun [önyükleme tanılaması](boot-diagnostics.md) etkin. 
Durdurulan serbest bırakılmış durumda vm'dir. VM'yi başlatın ve seri konsol bağlantısı yeniden deneyin. | Sanal makinenin seri konsol erişmek için başlatılmış durumda olması gerekir
Bu sanal makine seri konsolu kullanmak için gerekli izinlere sahip değil. En az olduğundan emin olun VM katkıda bulunan rol izinleri.| Seri konsol erişimi erişmek için belirli bir izni gerektirir. Bkz: [erişim gereksinimlerini](#prerequisites) için Ayrıntılar
Önyükleme tanılaması depolama hesabı için kaynak grubu belirlenemiyor '<STORAGEACCOUNTNAME>'. Bu VM için önyükleme tanılaması etkin ve bu depolama hesabına erişiminiz olduğunu doğrulayın. | Seri konsol erişimi erişmek için belirli bir izni gerektirir. Bkz: [erişim gereksinimlerini](#prerequisites) için Ayrıntılar
Bu sanal makinenin önyükleme tanılaması depolama hesabı erişirken 'Yasak' yanıt karşılaşıldı. | Bu önyükleme tanılama hesabı bir güvenlik duvarı bulunmadığından emin olun. İşleve seri konsol için bir erişilebilir önyükleme tanılaması depolama hesabı gereklidir.
Web yuvası kapalı veya açılamadı. | Beyaz listeye gerekebilir `*.console.azure.com`. Daha ayrıntılı ancak uzun yaklaşımdır beyaz listeye [Microsoft Azure veri merkezi IP aralıkları](https://www.microsoft.com/en-us/download/details.aspx?id=41653), nispeten düzenli olarak değiştiği.
Yalnızca sistem durumu bilgileri, bir Windows VM'ye bağlanırken gösterilir.| Bu özel Yönetim Konsolu Windows görüntünüzü için etkinleştirilmemiş olmadığını gösterilir. Bkz: [erişim seri konsol için Windows](#access-serial-console-for-windows) SAC, Windows VM'de el ile etkinleştirme hakkında yönergeler için. Daha fazla ayrıntı şu adreste bulunabilir: [Windows durum sinyallerini](https://github.com/Microsoft/azserialconsole/blob/master/Known_Issues/Windows_Health_Info.md).

## <a name="known-issues"></a>Bilinen sorunlar 
Seri konsol ile ilgili bazı sorunlar farkında duyuyoruz. Bu sorunlar ve risk azaltma için adımlar listesi aşağıda verilmiştir.

Sorun                             |   Risk azaltma 
:---------------------------------|:--------------------------------------------|
Ulaşmaktan bağlantı başlık satırında bir günlük göstermez sonra girin | Bu sayfaya bakın: [Hitting girin hiçbir şey yapmaz](https://github.com/Microsoft/azserialconsole/blob/master/Known_Issues/Hitting_enter_does_nothing.md). Özel bir VM, sağlamlaştırılmış gereç veya önyükleme yapılandırma düzgün bir şekilde seri bağlantı noktasına bağlanmak başarısız olmasına, causers Windows çalıştırıyorsanız, bu durum oluşabilir. Windows 10 istemci VM çalıştırıyorsanız yalnızca Windows Server Vm'lerinin EMS etkin olacak şekilde yapılandırılmış olarak bu de gerçekleşir.
Çekirdek hata ayıklamasını etkin olduğunda SAC komut istemi türü oluşturulamıyor | VM ve çalıştırmak için RDP `bcdedit /debug {current} off` yükseltilmiş bir komut isteminden. RDP gerçekleştiremezsiniz, bunun yerine başka bir Azure VM için işletim sistemi diski ve kullanarak bir veri diski olarak bağlı durumdayken değiştirmek `bcdedit /store <drive letter of data disk>:\boot\bcd /debug <identifier> off`, ardından diski geri değiştirme.
Özgün içerik yinelenen bir karakter varsa PowerShell içinde SAC sonuçları üçüncü bir karakter yapıştırma | Geçmiş geçerli oturumun PSReadLine modülünü kaldırmak için bir çözüm olabilir. Çalıştırma `Remove-Module PSReadLine` - geçerli oturumu PSReadLine modülünden kaldırmak için bu silmez veya modül kaldırın.
Bazı klavye girişleri garip SAC çıkış üretmesi (örneğin `[A`, `[3~`) | [VT100](https://aka.ms/vtsequences) kaçış dizileri SAC istemi tarafından desteklenmiyor.
Çok uzun dizeler yapıştırma çalışmıyor | Seri konsol 2048 karakter terminale içine yapıştırdığınız dize uzunluğunu kısıtlar. Bu seri bağlantı noktası bant genişliği aşırı yüklenilmesini önlemek içindir.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular 

**SORU. Nasıl geri bildirim gönderebilir miyim?**

A. Giderek bir sorun geri bildirim sağlamak https://aka.ms/serialconsolefeedback. Alternatif olarak daha az (tercih edilen) gönderme aracılığıyla geri bildirim azserialhelp@microsoft.com veya sanal makine kategorisi http://feedback.azure.com

**SORU. Seri konsol kopyala/yapıştır destekliyor mu?**

A. Evet, bunu yapar. Kopyalama ve yapıştırma terminale Ctrl + Shift + C ve Ctrl + Shift + V kullanın.

**SORU. Kimler etkinleştirebilir veya seri konsol için Aboneliğim devre dışı?**

A. Etkinleştirmek veya devre dışı bir aboneliği genelinde düzeyinde seri konsol için abonelik yazma izinleri olmalıdır. Yazmak için izne sahip roller içerir ancak için yönetici veya sahip rollerinin sınırlı değildir. Özel roller ayrıca yazma izinlerine sahip.

**SORU. Sanal Makinem için seri konsol erişebilecek mi?**

A. Katkıda bulunan düzeyinde erişime sahip olmalıdır veya üzeri bir VM sanal makinenin seri konsol erişebilmek için. 

**SORU. Seri konsolumda herhangi bir şey ne yapmalıyım gösterilmiyor?**

A. Görüntünüzü seri konsol erişimi için büyük olasılıkla yanlış yapılandırılmış. Bkz: [eski veya özel görüntüleri seri konsolunda etkinleştirme](#enable-serial-console-in-custom-or-older-images) görüntünüzü seri konsol etkinleştirmek için yapılandırma hakkında ayrıntılar için.

**SORU. Seri konsol sanal makine ölçek kümeleri için kullanılabilir mi?**

A. Seri konsol erişimi için sanal makine ölçek kümesi örneklerine şu anda desteklenmiyor.

## <a name="next-steps"></a>Sonraki adımlar
* CMD ve PowerShell komutları, Windows SAC içinde kullanabileceğiniz kapsamlı bir kılavuzu için tıklayın [burada](serial-console-cmd-ps-commands.md).
* Seri konsol için de kullanılabilir olan [Linux](serial-console-linux.md) VM'ler.
* Daha fazla bilgi edinin [önyükleme tanılaması](boot-diagnostics.md).