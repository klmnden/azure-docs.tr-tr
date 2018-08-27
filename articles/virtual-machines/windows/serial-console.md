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
ms.date: 08/07/2018
ms.author: harijay
ms.openlocfilehash: 91c917687edbdfb49fc7a390187a860d9474623a
ms.sourcegitcommit: ebb460ed4f1331feb56052ea84509c2d5e9bd65c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/24/2018
ms.locfileid: "42918933"
---
# <a name="virtual-machine-serial-console-preview"></a>Sanal makinenin seri konsol (Önizleme) 


Azure'da sanal makine seri konsolu, Linux ve Windows sanal makineler için metin tabanlı bir konsol erişim sağlar. Bu seri bağlantı COM1 için sanal makinenin seri bağlantı noktasıdır ve sanal makineye erişim sağlar ve sanal makinenin ağa ilgili olmayan / işletim sistemi durumu. Seri konsol erişimi için bir sanal makine şu anda yalnızca Azure portalı üzerinden yapılır ve yalnızca VM katkıda bulunanı olan kullanıcılar için veya sanal makineye erişimi yukarıda izin. 

Linux VM'ler için seri konsol belgeleri [Buraya](../linux/serial-console.md).

> [!Note] 
> Kullanım koşullarını kabul etmiş olursunuz şartıyla önizlemeleri için kullanılabilir hale getirilir. Daha fazla bilgi için bkz. [Microsoft Azure ek kullanım koşulları Microsoft Azure önizlemeleri için.] (https://azure.microsoft.com/support/legal/preview-supplemental-terms/) Bu hizmeti şu anda aşamasındadır **genel Önizleme** ve genel Azure bölgelerine seri konsoluna erişimi sanal makineler için kullanılabilir. Bu noktada seri konsol kullanılabilir Azure kamu, Azure Almanya ve Azure Çin bulut değil.

 

## <a name="prerequisites"></a>Önkoşullar 

* Kaynak Yönetimi dağıtım modeline kullanıyor olmanız gerekir. Klasik dağıtımlar desteklenmez. 
* Sanal makinesinin olmalıdır [önyükleme tanılaması](boot-diagnostics.md) etkin   ![](../media/virtual-machines-serial-console/virtual-machine-serial-console-diagnostics-settings.png)
* Seri konsol kullanarak hesabı olmalıdır [katkıda bulunan rolü](../../role-based-access-control/built-in-roles.md) VM için ve [önyükleme tanılaması](boot-diagnostics.md) depolama hesabı. 

## <a name="open-the-serial-console"></a>Seri Konsolu
Sanal makineler için seri konsol üzerinden erişilebilir, yalnızca [Azure portalında](https://portal.azure.com). Portal aracılığıyla sanal makineler için seri konsoluna erişmek için adımları aşağıda verilmiştir 

  1. Azure portalını açın
  2. Sol taraftaki menüde, sanal makineleri seçin.
  3. VM listesinde tıklayın. VM için genel bakış sayfası açılır.
  4. Destek + sorun giderme bölümüne aşağı kaydırın ve seri konsol (Önizleme) seçeneğine tıklayın. Seri konsolu ile yeni bir bölme açılır ve bağlantıyı başlatın.

![](../media/virtual-machines-serial-console/virtual-machine-windows-serial-console-connect.gif)

## <a name="access-serial-console-for-windows"></a>Windows için seri konsol erişimi 
Azure'da yeni Windows Server görüntülerini olacaktır [Özel Yönetim Konsolu](https://technet.microsoft.com/library/cc787940(v=ws.10).aspx) (SAC) varsayılan olarak etkin. SAC Windows server sürümlerinde desteklenir, ancak istemci sürümleri (örneğin, Windows 10, Windows 8 veya Windows 7) kullanılamıyor. Seri konsol kullanarak oluşturulan Windows sanal makineler için etkinleştirmeyi Feb2018 veya alt görüntüleri Lütfen aşağıdaki adımları kullanın: 

1. Windows sanal makinenize Uzak Masaüstü aracılığıyla bağlanma
2. Bir yönetim komut isteminden aşağıdaki komutları çalıştırın. 
* `bcdedit /ems {current} on`
* `bcdedit /emssettings EMSPORT:1 EMSBAUDRATE:115200`
3. SAC Konsolu etkinleştirilmesi için sistemi yeniden başlatın

![](../media/virtual-machines-serial-console/virtual-machine-windows-serial-console-connect.gif)

Gerekli SAC çevrimdışı de etkinleştirilebilir ise 

1. Var olan sanal makineye veri diski olarak yapılandırılmış SAC istediğiniz windows disk ekleyin. 
2. Bir yönetim komut isteminden aşağıdaki komutları çalıştırın. 
* `bcdedit /store <mountedvolume>\boot\bcd /ems {default} on`
* `bcdedit /store <mountedvolume>\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200`

### <a name="how-do-i-know-if-sac-is-enabled-or-not"></a>SAC etkin'in etkin olup olmadığını nasıl anlarım 

Varsa [SAC] (https://technet.microsoft.com/library/cc787940(v=ws.10).aspx) etkin seri konsol SAC istemi gösterilmez. Bazı durumlarda VM sistem durumu bilgileri gösterebilir veya boş olamaz.  

### <a name="enabling-boot-menu-to-show-in-the-serial-console"></a>Seri konsol içinde göstermek, önyükleme menüsü etkinleştirme 

Windows Önyükleme Yükleyicisi'ni etkinleştirmek gerekiyorsa göstermek için seri konsolunda Windows önyükleme yükleyicisi için aşağıdaki ek seçenekleri ekleyebilirsiniz ister.

1. Windows sanal makinenize Uzak Masaüstü aracılığıyla bağlanma
2. Bir yönetici komut isteminden aşağıdaki komutları çalıştırın. 
* `bcdedit /set {bootmgr} displaybootmenu yes`
* `bcdedit /set {bootmgr} timeout 5`
* `bcdedit /set {bootmgr} bootems yes`
3. Önyükleme menüsünün etkinleştirilmesi sistemi yeniden başlatın

> [!NOTE] 
> Bu işlev için noktası Destek'teki anahtarları etkin değilse, Gelişmiş Önyükleme Seçenekleri kullandığınıza bcdedit/set {geçerli} onetimeadvancedoptions ihtiyaç duyuyorsanız bkz [bcdedit](https://docs.microsoft.com/windows-hardware/drivers/devtest/bcdedit--set) daha fazla ayrıntı için

## <a name="disable-serial-console"></a>Seri konsol devre dışı bırak
Varsayılan olarak, seri konsol erişimi tüm VM'ler için Etkin Abonelikler var. Abonelik düzeyinde veya VM düzeyi seri konsol devre dışı bırakabilir.

### <a name="subscription-level-disable"></a>Abonelik düzeyinde devre dışı bırak
Seri konsol devre dışı bırakılabilir bir tüm abonelik tarafından için [devre dışı konsol REST API çağrısı](https://aka.ms/disableserialconsoleapi). API belgeleri sayfasında "Try It" kullanılabilen İşlevler, devre dışı bırakın ve bir abonelik için seri konsol etkinleştirmek için kullanabilir. Girin, `subscriptionId`, "varsayılan" `default` alan ve Çalıştır'a tıklayın. Azure CLI komutları henüz kullanıma sunulmamıştır ve daha sonraki bir tarihte ulaşır. [REST API çağrısı burada deneyin](https://aka.ms/disableserialconsoleapi).

![](../media/virtual-machines-serial-console/virtual-machine-serial-console-rest-api-try-it.png)

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
Konsolun içinden çalıştırma komutları içeriyorsa ya da parolalar, gizli dizileri, kullanıcı adlarını veya diğer bir form herhangi biri kişisel bilgilerin (PII) çıktı Konsolu için erişim parolası, oturumunuz açıkken, bu sanal makine önyükleme tanılaması için yazılır günlükleri, tüm diğer görünen metni, seri konsolun kaydırma uygulamasının bir parçası olarak birlikte işlevselliği yedekleyin. Bu günlükler döngüsel ve, gizli dizileri ve/veya PII içerebilir Uzak Masaüstü için herhangi bir şey kullanmanın en iyi yöntemin izlenmesi önerilir ancak tanılama depolama hesabı için Okuma izinleri olan tek kişilere, onlara yönelik erişimi sahiptir. 

### <a name="concurrent-usage"></a>Eşzamanlı kullanım
Seri konsola bir kullanıcı bağlandığından ve başka bir kullanıcı, aynı sanal makineye erişimi başarıyla istekleri, ilk kullanıcı bağlantısı kesilir ve duran ve fiziksel konsol ve yeni bir bırakarak ilk kullanıcı benzer şekilde ikinci kullanıcı bağlı Kullanıcı oturan.

>[!CAUTION] 
Başka bir deyişle, bağlantısı kesilse kullanıcı kapatılacak değil! Bağlantıyı Kes (aracılığıyla SIGHUP veya benzer bir mekanizma) bağlı bir oturum kapatma zorunlu tutma becerisi hala Yol Haritası ' dir. Windows için yoktur SAC içinde etkin otomatik bir zaman aşımı ancak Linux için terminal zaman aşımı ayarını yapılandırabilirsiniz. 

## <a name="using-serial-console-for-nmi-calls-in-windows-vms"></a>Seri konsol için NMI kullanarak Windows VM çağırır
Maskelenemez olmayan bir kesinti (NMI) yazılımı bir sanal makinede değil yoksayacak bir sinyal oluşturmak için tasarlanmıştır. Tarihsel olarak, NMIs belirli yanıt süreleri gerektiren sistemleri donanım sorunları izlemek için kullanılır.  Bugün, programcılar ve sistem yöneticileri bir mekanizma NMI hata ayıklama veya askıya sistemler gidermek için genellikle kullanın.

Seri konsol, aşağıda gösterilen komut çubuğunda klavye simgesini kullanarak bir Azure sanal makine bir NMI göndermek için kullanılabilir. NMI teslim sonra sanal makine yapılandırması sistem nasıl yanıt denetler. Windows için kilitlenme yapılandırılabilir ve bir NMI alırken bir bellek dökümü oluşturun.

![](../media/virtual-machines-serial-console/virtual-machine-windows-serial-console-nmi.png) <br>

Kilitlenme bilgi dökümü bir NMI aldığında oluşturmak için Windows yapılandırma hakkında daha fazla bilgi için bkz: [tam kilitlenme bilgi döküm dosyası veya bir çekirdek kilitlenme dökümü dosyalarının Windows tabanlı bir sistemde bir NMI kullanarak oluşturma](https://support.microsoft.com/en-us/help/927069/how-to-generate-a-complete-crash-dump-file-or-a-kernel-crash-dump-file)


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

## <a name="known-issues"></a>Bilinen sorunlar 
Hala Önizleme aşamaları seri konsol erişimi için de bizim yaptığımız gibi biz bazı bilinen sorunlar ile çalışıyorsanız, olası geçici çözümleri ile bunların bir listesi aşağıda verilmiştir 

Sorun                             |   Risk azaltma 
:---------------------------------|:--------------------------------------------|
Sanal makine ölçek kümesi örneği seri konsolu ile seçeneği yoktur | Önizleme sırasında sanal makine ölçek kümesi örneklerine ait seri konsoluna erişimi desteklenmiyor.
Ulaşmaktan bağlantı başlık satırında bir günlük göstermez sonra girin | Lütfen şu sayfaya bakın: [Hitting girin hiçbir şey yapmaz](https://github.com/Microsoft/azserialconsole/blob/master/Known_Issues/Hitting_enter_does_nothing.md). Bu özel bir VM çalıştırıyorsanız, gereç sağlamlaştırılmış olması veya düzgün bir şekilde seri bağlantı noktasına bağlanmak başarısız olmasına, causers Windows yapılandırma kaz.
Yalnızca sistem durumu bilgileri, bir Windows VM'ye bağlanırken gösterilir.| Bu özel Yönetim Konsolu Windows görüntünüzü için etkinleştirilmemiş olmadığını gösterilir. Bkz: [erişim seri konsol için Windows](#access-serial-console-for-windows) SAC, Windows VM'de el ile etkinleştirme hakkında yönergeler için. Daha fazla ayrıntı şu adreste bulunabilir: [Windows durum sinyallerini](https://github.com/Microsoft/azserialconsole/blob/master/Known_Issues/Windows_Health_Info.md).
Çekirdek hata ayıklamasını etkin olduğunda SAC komut istemi türü oluşturulamıyor | VM ve çalıştırmak için RDP `bcdedit /debug {current} off` yükseltilmiş bir komut isteminden. RDP gerçekleştiremezsiniz, bunun yerine başka bir Azure VM için işletim sistemi diski ve kullanarak bir veri diski olarak bağlı durumdayken değiştirmek `bcdedit /store <drive letter of data disk>:\boot\bcd /debug <identifier> off`, ardından diski geri değiştirme.
Özgün içerik yinelenen bir karakter varsa PowerShell içinde SAC sonuçları üçüncü bir karakter yapıştırma | Geçici bir çözüm PSReadLine modülü kaldırmaktır. `Remove-Module PSReadLine` PSReadLine modülü geçerli oturumdan kaldırır.
Bazı klavye girişleri garip SAC çıkış üretmesi (örneğin `[A`, `[3~`) | [VT100](https://aka.ms/vtsequences) kaçış dizileri SAC istemi tarafından desteklenmiyor.
Bu sanal makinenin önyükleme tanılaması depolama hesabı erişirken 'Yasak' yanıt karşılaşıldı. | Bu önyükleme tanılama hesabı bir güvenlik duvarı bulunmadığından emin olun. İşleve seri konsol için bir erişilebilir önyükleme tanılaması depolama hesabı gereklidir.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular 
**SORU. Nasıl geri bildirim gönderebilir miyim?**

A. Giderek bir sorun geri bildirim sağlamak https://aka.ms/serialconsolefeedback. Alternatif olarak daha az (tercih edilen) gönderme aracılığıyla geri bildirim azserialhelp@microsoft.com veya sanal makine kategorisi http://feedback.azure.com

**SORU. Burada bir destek talebi belirtebileceği seri konsoluna erişmek gönderemiyorum?**

A. Bu önizleme özelliği Azure Önizleme koşulları kapsamındadır. Desteği, en iyi yukarıda belirtilen kanallar aracılığıyla işlenir. 

## <a name="next-steps"></a>Sonraki adımlar
* CMD ve PowerShell komutları, Windows SAC içinde kullanabileceğiniz kapsamlı bir kılavuzu için tıklayın [burada](serial-console-cmd-ps-commands.md).
* Seri konsol için de kullanılabilir olan [Linux](../linux/serial-console.md) VM'ler.
* Daha fazla bilgi edinin [önyükleme tanılaması](boot-diagnostics.md).
