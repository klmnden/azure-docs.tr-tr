---
title: Azure sanal makinesi seri konsol | Microsoft Docs
description: Azure sanal makineleri için çift yönlü seri Konsolu.
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
ms.date: 03/05/2018
ms.author: harijay
ms.openlocfilehash: 9c16114b4f8d335dc750b1beef1fb6204ae20e0f
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="virtual-machine-serial-console-preview"></a>Sanal makine seri konsol (Önizleme) 


Azure sanal makine seri konsolunuzdaki Linux ve Windows sanal makineler için metin tabanlı bir konsol erişim sağlar. Bu seri bağlantı COM1 seri bağlantı noktasına sanal makine ve sanal makine erişim sağlar ve sanal makinenin ağa ilgili olmayan / sistem durumu işletim. Seri konsol erişimi için bir sanal makine şu anda yalnızca Azure portal aracılığıyla yapılır ve VM katılımcı olan kullanıcılar için veya sanal makineye erişim yukarıda izin. 

> [!Note] 
> Kullanım koşullarını kabul ediyorum koşuluyla önizlemeleri için kullanılabilir hale getirilir. Daha fazla bilgi için bkz: [Microsoft Azure ek kullanım koşulları'nı Microsoft Azure önizlemeleri.] (https://azure.microsoft.com/support/legal/preview-supplemental-terms/) Bu hizmet olarak şu anda **genel Önizleme** ve seri konsoluna erişimi sanal makineler için genel Azure bölgeler için kullanılabilir. Bu noktada seri konsol kullanılabilir Azure kamu, Azure Almanya ve Azure Çin bulut değil.

 

## <a name="prerequisites"></a>Önkoşullar 

* Sanal makine olmalıdır [önyükleme tanılama](boot-diagnostics.md) etkin 
* Seri konsol kullanarak hesabı olmalıdır [katkıda bulunan rolü](../../active-directory/role-based-access-built-in-roles.md) VM için ve [önyükleme tanılama](boot-diagnostics.md) depolama hesabı. 

## <a name="open-the-serial-console"></a>Seri konsol açın
sanal makineler için seri Konsolu aracılığıyla erişilebilir durumda yalnızca [Azure portal](https://portal.azure.com). Portal üzerinden sanal makineler için seri konsoluna erişmek için adımları aşağıda verilmiştir 

  1. Azure portalını açın
  2. Soldaki menüde sanal makineleri seçin.
  3. VM listesinde tıklayın. VM için genel bakış sayfası açılır.
  4. Destek + sorun giderme bölümüne aşağı kaydırın ve seri konsol (Önizleme) seçeneğine tıklayın. Seri konsolu ile yeni bir bölmesini açın ve bağlantıyı başlatın.

![](../media/virtual-machines-serial-console/virtual-machine-windows-serial-console-connect.gif)

### <a name="disable-feature"></a>Özelliğini devre dışı bırakma
Seri konsol işlevlerini belirli VM'ler için bu VM'in önyükleme tanılama ayarını devre dışı bırakarak devre dışı bırakılabilir.

## <a name="serial-console-security"></a>Seri konsol güvenlik 

### <a name="access-security"></a>Erişimi güvenliği 
Seri konsol erişimi olan kullanıcılar için sınırlı [VM katkıda bulunanlar](../../active-directory/role-based-access-built-in-roles.md#virtual-machine-contributor) ya da sanal makineye erişim üstünde. AAD kiracınızın çok faktörlü kimlik doğrulaması gerektiren sonra uygulamaya erişim aracılığıyla olduğundan seri konsoluna erişimi MFA ayrıca gerekir [Azure portal](https://portal.azure.com).

### <a name="channel-security"></a>Kanalı güvenliği
Tüm verileri geri gönderilir ve kablo İleri şifrelenir.

### <a name="audit-logs"></a>Denetim günlükleri
Seri konsol tüm erişim şu anda oturum [önyükleme tanılama](https://docs.microsoft.com/azure/virtual-machines/linux/boot-diagnostics) sanal makinenin günlükleri. Bu günlükler erişim sahibi ve Azure sanal makine yöneticisi tarafından denetlenir.  

>[!CAUTION] 
Konsolda çalıştırma komutları içeriyorsa ya da parolaları, gizli, kullanıcı adlarını veya diğer, kişisel bilgilerin (PII) çıkış konsol için erişim parolası günlüğe kaydedilen olsa da, bu sanal makine önyükleme tanılaması için yazılır Tüm diğer görünür metin, seri konsolun scrollback işlevselliği uyarlamasını bir parçası olarak birlikte günlüğe kaydeder. Bu günlükler döngüsel ve parolaları ve/veya PII içeren herhangi bir şey için Uzak Masaüstü'nü kullanarak en iyi uygulama olarak önerilir ancak tanılama depolama hesabı için Okuma izinlerine sahip tek kişiler bunları erişimi. 

### <a name="concurrent-usage"></a>Eş zamanlı kullanım
Seri konsoluna bir kullanıcının bağlı olduğu ve başka bir kullanıcı başarıyla aynı sanal makinenin erişim isteğinde, ilk kullanıcının bağlantısı kesilecek ve ikinci kullanıcı duran ve fiziksel konsolunda ve yeni bir bırakarak ilk kullanıcı benzer bir şekilde bağlı durduğunu kullanıcı.

>[!CAUTION] 
Başka bir deyişle, bağlantısı kesilen kullanıcının oturumu değil! Bağlantıyı Kes (aracılığıyla SIGHUP veya benzer bir mekanizma) bağlı bir oturum kapatma zorlamak için hala yol haritası özelliğidir. Windows SAC içinde etkin otomatik bir zaman aşımı yoktur, ancak Linux için terminal zaman aşımı ayarını yapılandırabilirsiniz. 


## <a name="accessing-serial-console-for-windows"></a>Seri konsol için Windows erişme 
Azure daha yeni Windows Server görüntülerinde olacaktır [Özel Yönetim Konsolu](https://technet.microsoft.com/library/cc787940(v=ws.10).aspx) (SAC) varsayılan olarak etkin. SAC Windows server sürümleri desteklenir, ancak istemci sürümleri (örneğin Windows 10, Windows 8 veya Windows 7) mevcut değil. Seri konsol Feb2018 ya da alt görüntüleri kullanarak oluşturulan Windows sanal makineleri için etkinleştirmek için aşağıdaki adımları Lütfen: 

1. Windows sanal makinenizi Uzak Masaüstü aracılığıyla bağlanma
2. Bir yönetici komut isteminden aşağıdaki komutları çalıştırın 
* `bcdedit /ems {current} on`
* `bcdedit /emssettings EMSPORT:1 EMSBAUDRATE:115200`
3. Etkinleştirilecek SAC Konsolu için sistemi yeniden başlatın

![](../media/virtual-machines-serial-console/virtual-machine-windows-serial-console-connect.gif)

Gerekli SAC çevrimdışı de etkinleştirilebilir 

1. Var olan VM için bir veri diski olarak yapılandırılmış SAC istediğiniz windows disk ekleyin. 
2. Bir yönetici komut isteminden aşağıdaki komutları çalıştırın 
* `bcdedit /store <mountedvolume>\boot\bcd /ems {default} on`
* `bcdedit /store <mountedvolume>\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200`

### <a name="how-do-i-know-if-sac-is-enabled-or-not"></a>SAC veya etkin olup olmadığını nasıl anlayabilirim 

Varsa [SAC] (https://technet.microsoft.com/library/cc787940(v=ws.10).aspx) etkin seri konsol SAC istemi gösterilmez. Bazı durumlarda bir VM sistem durumu bilgisi gösterebilir veya boş.  

### <a name="enabling-boot-menu-to-show-in-the-serial-console"></a>Seri konsoldan göstermek, önyükleme menüsü etkinleştirme 

Windows önyükleme yükleyicisi için aşağıdaki ek seçenekler ekleyebilirsiniz seri konsoldan göstermek için Windows önyükleme yükleyicisi etkinleştirmeniz gerekirse ister.

1. Windows sanal makinenizi Uzak Masaüstü aracılığıyla bağlanma
2. Bir yönetici komut isteminden aşağıdaki komutları çalıştırın 
* `bcdedit /set {bootmgr} displaybootmenu yes`
* `bcdedit /set {bootmgr} timeout 5`
* `bcdedit /set {bootmgr} bootems yes`
3. Önyükleme menüsünde etkinleştirilmesi için sistemi yeniden başlatma

> [!NOTE] 
> Bu işlev noktası desteği, anahtarları etkin değil, Gelişmiş Önyükleme Seçenekleri üzerinde bcdedit/set {geçerli} onetimeadvancedoptions kullanmanız gerekiyorsa, bkz [bcdedit](https://docs.microsoft.com/windows-hardware/drivers/devtest/bcdedit--set) daha fazla ayrıntı için

## <a name="common-scenarios-for-accessing-windows-serial-console"></a>Windows seri konsoluna erişmek için genel senaryolar 
Senaryo          | Seri konsol Eylemler                
:------------------|:----------------------------------------
Yanlış güvenlik duvarı kuralları | Seri Konsolu erişebilir ve iptables ya da Windows Güvenlik duvarı kuralları Düzelt 
Dosya Sistemi Bozulması/onay | Seri konsol erişmek ve dosya sistemi SAC CMD oturum açtıktan sonra kurtarma
RDP yapılandırma sorunları | Seri konsol erişmek ve cmd kanala oturum açın. Terminal Hizmetleri durumunu denetleyin ve gerekiyorsa yeniden başlatın.
Sistem ağ kilitleme| Erişim seri konsolu ve oturum açma cmd kanal. Tarafından güvenlik duvarı durumunu denetle [netsh](https://docs.microsoft.com/windows-server/networking/technologies/netsh/netsh-contexts) komut satırı. 

## <a name="errors"></a>Hatalar
Hataların çoğu doğasını ve bu bağlantı adresi denemeden geçicidir. Aşağıdaki tablo hataları ve azaltma listesini gösterir 

Hata                            |   Risk azaltma 
:---------------------------------|:--------------------------------------------|
İçin önyükleme tanılaması ayarları alınamadı '<VMNAME>'. Seri konsol kullanmak için bu önyükleme tanılaması bu VM için etkinleştirildiğinden emin olun. | VM sahip olduğundan emin olun [önyükleme tanılama](boot-diagnostics.md) etkin. 
VM durduruldu deallocated bir durumda değil. VM başlatmak ve seri konsol bağlantısı yeniden deneyin. | Sanal makinenin seri konsoluna erişmek için başlatılmış bir durumda olması gerekir
Bu VM seri konsol kullanmak için gerekli izinlere sahip değil. En az olduğundan emin olun VM katkıda bulunan rolü izinleri.| Seri konsol erişimi erişim izni belirli gerektirir. Bkz: [erişim gereksinimleri](#prerequisites) Ayrıntılar için
Önyükleme tanılama depolama hesabı için kaynak grubu belirlenemiyor '<STORAGEACCOUNTNAME>'. Bu VM için önyükleme tanılaması etkin ve bu depolama hesabına erişimi doğrulayın. | Seri konsol erişimi erişim izni belirli gerektirir. Bkz: [erişim gereksinimleri](#prerequisites) Ayrıntılar için

## <a name="known-issues"></a>Bilinen sorunlar 
Biz yine seri konsol erişimi için Önizleme aşamalarını olduğu gibi biz bilinen bazı sorunlar çalışıyorsanız, bunların olası geçici çözümler ile listesi aşağıdadır 

Sorun                           |   Risk azaltma 
:---------------------------------|:--------------------------------------------|
Sanal makine ölçek kümesi örnek seri konsolu ile bir seçenek yoktur | Önizleme zaman seri konsoluna erişimi sanal makine ölçek kümesi örneklerinin desteklenmiyor.
Devreyi enter bağlantı başlık satırında bir günlük göstermez | [Devreyi girin, hiçbir şey yapmıyor](https://github.com/Microsoft/azserialconsole/blob/master/Known_Issues/Hitting_enter_does_nothing.md)
Yalnızca sistem durumu bilgilerini bir Windows VM bağlanırken gösterilir| [Windows sistem durumu sinyalleri](https://github.com/Microsoft/azserialconsole/blob/master/Known_Issues/Windows_Health_Info.md)

## <a name="frequently-asked-questions"></a>Sık sorulan sorular 
**Q. Geri bildirim nasıl gönderebilir miyim?**

A. Giderek bir sorun görüş https://aka.ms/serialconsolefeedback. Alternatif olarak daha az (tercih edilen) geribildirim gönderme aracılığıyla azserialhelp@microsoft.com veya sanal makine kategorisi http://feedback.azure.com 

**Q. Bir hata alıyorsunuz "Varolan konsol çakışan işletim sistemi türü istenen Linux işletim sistemi türü olan" Windows"var?**

A. Bu bu sorunu düzeltmek için bilinen bir sorundur yalnızca açık [Azure bulut Kabuk](https://docs.microsoft.com/azure/cloud-shell/overview) bash modunda ve yeniden deneyin.

**Q. I bir destek servis talebi burada dosya seri Konsolu erişebilir değilim?**

A. Bu önizleme özelliği Azure Önizleme koşulları ele alınmıştır. Bu destek en iyi yukarıda belirtilen kanallar aracılığıyla gerçekleştirilir. 

## <a name="next-steps"></a>Sonraki adımlar
* Seri konsol ayrıca kullanılabilir [Linux](../linux/serial-console.md) VM'ler
* Daha fazla bilgi edinmek [bootdiagnostics](boot-diagnostics.md)
