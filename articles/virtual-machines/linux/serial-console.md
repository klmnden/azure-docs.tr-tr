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
ms.date: 08/07/2018
ms.author: harijay
ms.openlocfilehash: 20bd2d61671d89a5c2a13525ea119595cf0b7c93
ms.sourcegitcommit: 8ebcecb837bbfb989728e4667d74e42f7a3a9352
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2018
ms.locfileid: "42062136"
---
# <a name="virtual-machine-serial-console-preview"></a>Sanal makinenin seri konsol (Önizleme) 


Azure'da sanal makine seri konsolu, Linux ve Windows sanal makineler için metin tabanlı bir konsol erişim sağlar. Bu seri bağlantı COM1 için sanal makinenin seri bağlantı noktasıdır ve sanal makineye erişim sağlar ve sanal makinenin ağa ilgili olmayan / işletim sistemi durumu. Seri konsol erişimi için bir sanal makine şu anda yalnızca Azure portalı üzerinden yapılır ve yalnızca VM katkıda bulunanı olan kullanıcılar için veya sanal makineye erişimi yukarıda izin. 

Windows Vm'leri için seri konsol belgeleri [Buraya](../windows/serial-console.md).

> [!Note] 
> Kullanım koşullarını kabul etmiş olursunuz şartıyla önizlemeleri için kullanılabilir hale getirilir. Daha fazla bilgi için bkz. [Microsoft Azure ek kullanım koşulları Microsoft Azure önizlemeleri için.] (https://azure.microsoft.com/support/legal/preview-supplemental-terms/) Bu hizmeti şu anda aşamasındadır **genel Önizleme** ve genel Azure bölgelerine seri konsoluna erişimi sanal makineler için kullanılabilir. Bu noktada seri konsol kullanılabilir Azure kamu, Azure Almanya ve Azure Çin bulut değil.


## <a name="prerequisites"></a>Önkoşullar 

* Kaynak Yönetimi dağıtım modeline kullanıyor olmanız gerekir. Klasik dağıtımlar desteklenmez. 
* Sanal makinesinin olmalıdır [önyükleme tanılaması](boot-diagnostics.md) etkin 
* Seri konsol kullanarak hesabı olmalıdır [katkıda bulunan rolü](../../role-based-access-control/built-in-roles.md) VM için ve [önyükleme tanılaması](boot-diagnostics.md) depolama hesabı. 
* Linux dağıtımları için özel ayarları için bkz: [seri konsoluna erişmek için Linux](#access-serial-console-for-linux)


## <a name="open-the-serial-console"></a>Seri Konsolu
Sanal makineler için seri konsol üzerinden erişilebilir, yalnızca [Azure portalında](https://portal.azure.com). Portal aracılığıyla sanal makineler için seri konsoluna erişmek için adımları aşağıda verilmiştir 

  1. Azure portalını açın
  2. Sol taraftaki menüde, sanal makineleri seçin.
  3. VM listesinde tıklayın. VM için genel bakış sayfası açılır.
  4. Destek + sorun giderme bölümüne aşağı kaydırın ve seri konsol (Önizleme) seçeneğine tıklayın. Seri konsolu ile yeni bir bölme açılır ve bağlantıyı başlatın.

![](../media/virtual-machines-serial-console/virtual-machine-linux-serial-console-connect.gif)


> [!NOTE] 
> Seri konsol yapılandırılmış bir parolayla yerel bir kullanıcı gerektirir. Şu anda yalnızca bir SSH ortak anahtarı ile yapılandırılan VM'ler seri konsoluna erişimi yoktur. Parola ile yerel bir kullanıcı oluşturmak için kullanın [VM erişimi uzantısı](https://docs.microsoft.com/azure/virtual-machines/linux/using-vmaccess-extension) (Ayrıca "parolayı Sıfırla" tıklayarak Portalı'nda kullanılabilir) ve bir parolayla yerel bir kullanıcı oluşturun.

## <a name="disable-serial-console"></a>Seri konsol devre dışı bırak
Varsayılan olarak, seri konsol erişimi tüm VM'ler için Etkin Abonelikler var. Abonelik düzeyinde veya VM düzeyi seri konsol devre dışı bırakabilir.

### <a name="subscription-level-disable"></a>Abonelik düzeyinde devre dışı bırak
Seri konsol devre dışı bırakılabilir bir tüm abonelik tarafından için [devre dışı konsol REST API çağrısı](https://aka.ms/disableserialconsoleapi). API belgeleri sayfasında "Try It" kullanılabilen İşlevler, devre dışı bırakın ve bir abonelik için seri konsol etkinleştirmek için kullanabilir. Girin, `subscriptionId`, "varsayılan" `default` alan ve Çalıştır'a tıklayın. Azure CLI komutları henüz kullanıma sunulmamıştır ve daha sonraki bir tarihte ulaşır. [REST API çağrısı burada deneyin](https://aka.ms/disableserialconsoleapi).

![](../media/virtual-machines-serial-console/virtual-machine-serial-console-rest-api-try-it.png)

Alternatif olarak, Cloud Shell'de (bash komutları gösterilen) aşağıdaki komutları kümesini kullanabilir. devre dışı bırakmak için etkinleştirme ve devre dışı bırakılmış bir aboneliği için seri konsol durumunu görüntüleyin. 

* Bir abonelik için seri konsol devre dışı durumunu almak için:
    ```
    $ export ACCESSTOKEN=($(az account get-access-token --output=json | jq .accessToken | tr -d '"')) 

    $ export SUBSCRIPTION_ID=$(az account show --output=json | jq .id -r)

    $ curl "https://management.azure.com/subscriptions/$SUBSCRIPTION_ID/providers/Microsoft.SerialConsole/consoleServices/default?api-version=2018-05-01" -H "Authorization: Bearer $ACCESSTOKEN" -H "Content-Type: application/json" -H "Accept: application/json" -s | jq .properties
    ```
* Seri konsol bir abonelik için devre dışı bırakmak için:
    ```
    $ export ACCESSTOKEN=($(az account get-access-token --output=json | jq .accessToken | tr -d '"')) 

    $ export SUBSCRIPTION_ID=$(az account show --output=json | jq .id -r)

    $ curl -X POST "https://management.azure.com/subscriptions/$SUBSCRIPTION_ID/providers/Microsoft.SerialConsole/consoleServices/default/disableConsole?api-version=2018-05-01" -H "Authorization: Bearer $ACCESSTOKEN" -H "Content-Type: application/json" -H "Accept: application/json" -s -H "Content-Length: 0"
    ```
* Seri konsol bir abonelik için etkinleştirmek için:
    ```
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

### <a name="disable-feature"></a>Özelliği devre dışı
Seri konsol işlevlerini belirli sanal makineler için bu sanal makinenin önyükleme tanılama ayarı devre dışı bırakılarak devre dışı bırakılabilir.

## <a name="common-scenarios-for-accessing-serial-console"></a>Seri konsoluna erişmek için genel senaryolar 
Senaryo          | Seri konsol eylemleri                |  İşletim sistemi uygulanabilirliği 
:------------------|:-----------------------------------------|:------------------
FSTAB dosyası bozuk | `Enter` devam etmek ve bir metin düzenleyici kullanarak fstab dosyasını düzeltin anahtarı. Bunun tek kullanıcı modunda olması gerekebilir. Bkz: [fstab sorunlarını gidermeye yönelik](https://support.microsoft.com/help/3206699/azure-linux-vm-cannot-start-because-of-fstab-errors) ve [GRUB ve tek kullanıcı modu erişmek için seri konsol kullanarak](serial-console-grub-single-user-mode.md) kullanmaya başlamak için. | Linux 
Yanlış güvenlik duvarı kuralları | Seri konsola erişin, iptables veya Windows Güvenlik duvarı kuralları Düzelt. | Linux/Windows 
Dosya Sistemi Bozulması/işaretleyin | Seri konsol erişmek ve dosya sistemi kurtarın. | Linux/Windows 
SSH/RDP yapılandırma sorunları | Seri Konsol erişim ve ayarları değiştirin. | Linux/Windows 
Sistem ağ kilitleme| Seri konsol sistemini yönetmek için portal aracılığıyla erişim. | Linux/Windows 
Önyükleme yükleyicisi ile etkileşim kurma | Erişim GRUB/BCD seri Konsolu aracılığıyla. Git [GRUB ve tek kullanıcı modu erişmek için seri konsol kullanarak](serial-console-grub-single-user-mode.md) kullanmaya başlamak için. | Linux/Windows 

## <a name="access-serial-console-for-linux"></a>Linux için seri konsol erişimi
Okuma ve seri bağlantı noktasına konsol iletileri yazma, düzgün bir şekilde seri konsol için sırada, konuk işletim sistemi yapılandırılması gerekir. Çoğu [desteklenen Azure Linux dağıtımı](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) varsayılan olarak yapılandırılmış seri Konsolu. Yalnızca Azure portalında seri konsol bölümüne tıklayarak konsoluna erişim sağlar. 

Distro      | Seri konsol erişimi
:-----------|:---------------------
Red Hat Enterprise Linux    | Red Hat Enterprise Linux görüntüleri Azure'da sunulan, varsayılan olarak etkin konsol erişimi. 
CentOS      | Azure'da centOS görüntülerinden, varsayılan olarak etkin konsol erişimi. 
Ubuntu      | Azure'da ubuntu görüntülerinde varsayılan olarak etkin konsol erişebilir.
CoreOS      | Azure'da CoreOS görüntülerinden, varsayılan olarak etkin konsol erişimi.
SUSE        | Azure'da sunulan yeni SLES görüntüleri, varsayılan olarak etkin konsol erişebilir. Azure'da SLES'ın daha eski sürümleri (10 veya aşağıdaki) kullanıyorsanız uygulayın [KB makalesi](https://www.novell.com/support/kb/doc.php?id=3456486) seri konsol etkinleştirmek için. 
Oracle Linux        | Azure'da Oracle Linux görüntüleri, varsayılan olarak etkin konsol erişebilir.
Özel Linux görüntüleri     | Seri konsol özel Linux VM görüntünüz için etkinleştirmek üzere bir terminal ttyS0 üzerinde çalıştırılacak /etc/inittab konsol erişimi etkinleştirin. Bu inittab dosyasına eklemek için bir örnek aşağıda verilmiştir: `S0:12345:respawn:/sbin/agetty -L 115200 console vt102`. Düzgün bir şekilde özel görüntü oluşturma hakkında daha fazla bilgi için bkz. [azure'da bir Linux VHD'si oluşturup yükleme](https://aka.ms/createuploadvhd).

## <a name="errors"></a>Hatalar
Doğası gereği geçici hataların çoğu ve adresleri bunlar genellikle seri konsol bağlantısı yeniden deneniyor. Aşağıdaki tabloda, hataları ve risk azaltma listesini gösterir 

Hata                            |   Risk azaltma 
:---------------------------------|:--------------------------------------------|
Önyükleme tanılama ayarları alınamadı. '<VMNAME>'. Seri konsol kullanmak için bu VM için o önyükleme tanılaması etkin emin olun. | Sanal makine olduğundan emin olun [önyükleme tanılaması](boot-diagnostics.md) etkin. 
Durdurulan serbest bırakılmış durumda vm'dir. VM'yi başlatın ve seri konsol bağlantısı yeniden deneyin. | Sanal makinenin seri konsol erişmek için başlatılmış durumda olması gerekir
Bu sanal makine seri konsolu kullanmak için gerekli izinlere sahip değil. En az olduğundan emin olun VM katkıda bulunan rol izinleri.| Seri konsol erişimi erişmek için belirli bir izni gerektirir. Bkz: [erişim gereksinimlerini](#prerequisites) için Ayrıntılar
Önyükleme tanılaması depolama hesabı için kaynak grubu belirlenemiyor '<STORAGEACCOUNTNAME>'. Bu VM için önyükleme tanılaması etkin ve bu depolama hesabına erişiminiz olduğunu doğrulayın. | Seri konsol erişimi erişmek için belirli bir izni gerektirir. Bkz: [erişim gereksinimlerini](#prerequisites) için Ayrıntılar
Web yuvası kapalı veya açılamadı. | Beyaz listeye gerekebilir `*.console.azure.com`. Daha ayrıntılı ancak uzun yaklaşımdır beyaz listeye [Microsoft Azure veri merkezi IP aralıkları](https://www.microsoft.com/en-us/download/details.aspx?id=41653), nispeten düzenli olarak değiştiği.
## <a name="known-issues"></a>Bilinen sorunlar 
Hala Önizleme aşamaları seri konsol erişimi için de bizim yaptığımız gibi bazı bilinen sorunlar çalışıyoruz. Bu olası geçici çözümler listesi aşağıdadır:

Sorun                           |   Risk azaltma 
:---------------------------------|:--------------------------------------------|
Sanal makine ölçek kümesi örneği seri konsolu ile seçeneği yoktur |  Önizleme sırasında sanal makine ölçek kümesi örneklerine ait seri konsoluna erişimi desteklenmiyor.
Ulaşmaktan bağlantı başlık satırında bir günlük göstermez sonra girin | [Ulaşmaktan girin hiçbir şey yapılmaz](https://github.com/Microsoft/azserialconsole/blob/master/Known_Issues/Hitting_enter_does_nothing.md)
Bu sanal makinenin önyükleme tanılaması depolama hesabı erişirken 'Yasak' yanıt karşılaşıldı. | Bu önyükleme tanılama hesabı bir güvenlik duvarı bulunmadığından emin olun. İşleve seri konsol için bir erişilebilir önyükleme tanılaması depolama hesabı gereklidir.


## <a name="frequently-asked-questions"></a>Sık sorulan sorular 
**SORU. Nasıl geri bildirim gönderebilir miyim?**

A. Giderek bir sorun geri bildirim sağlamak https://aka.ms/serialconsolefeedback. Alternatif olarak daha az (tercih edilen) gönderme aracılığıyla geri bildirim azserialhelp@microsoft.com veya sanal makine kategorisi http://feedback.azure.com

**SORU. Burada bir destek talebi belirtebileceği seri konsoluna erişmek gönderemiyorum?**

A. Bu önizleme özelliği Azure Önizleme koşulları kapsamındadır. Desteği, en iyi yukarıda belirtilen kanallar aracılığıyla işlenir. 

## <a name="next-steps"></a>Sonraki adımlar
* Seri Konsolu [önyükleme GRUB ve tek kullanıcı moduna gir](serial-console-grub-single-user-mode.md)
* Seri Konsolu [NMI ve SysRq çağrıları](serial-console-nmi-sysrq.md)
* Seri konsol için de kullanılabilir olan [Windows](../windows/serial-console.md) VM'ler
* Daha fazla bilgi edinin [önyükleme tanılaması](boot-diagnostics.md)