---
title: Azure'da Windows VM'ler için eşitleme zaman | Microsoft Docs
description: Zaman eşitleme Windows sanal makineleri için.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 09/17/2018
ms.author: cynthn
ms.openlocfilehash: 1a2e75dcffe32c6f1aeaba8646b96bbc1500ffdf
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61438219"
---
# <a name="time-sync-for-windows-vms-in-azure"></a>Azure'da Windows VM'ler için zaman eşitleme

Zaman eşitleme, güvenlik ve olay bağıntısı için önemlidir. Bazen, dağıtılmış işlemler uygulama için kullanılır. Birden çok bilgisayar sistemleri arasındaki zaman doğruluğu eşitleme ile elde edilir. Eşitleme yeniden başlatma ve bir zaman kaynağı ve zamanı getirilirken bilgisayar arasındaki ağ trafiği dahil olmak üzere birden çok şey etkilenebilir. 

Azure artık Windows Server 2016 çalıştıran altyapı tarafından desteklenir. Windows Server 2016, gelişmiş algoritmalar saat düzeltin ve yerel saat ile UTC eşitlemek için koşul için kullanılan sahiptir.  Windows Server 2016'da nasıl Vm'leri konak üzerinde doğru zaman eşitleme yöneten VMICTimeSync hizmet geliştirildi. Geliştirmeleri VM başlangıç veya VM geri yükleme ve kesme gecikmesi düzeltme Windows Saati (W32time) için sağlanan örnekleri için daha doğru başlangıç saati içerir. 


>[!NOTE]
>Windows Saat hizmeti hızlı bir genel bakış için bu göz atın [üst düzey genel bakış videosu](https://aka.ms/WS2016TimeVideo).
>
> Daha fazla bilgi için [Windows Server 2016 doğru zamanı](https://docs.microsoft.com/windows-server/networking/windows-time-service/accurate-time). 

## <a name="overview"></a>Genel Bakış

Bir bilgisayar saatini doğruluk ne kadar yakın bilgisayar saati Eşgüdümlü Evrensel Saat (UTC) zamanı standart açıktır gauged. UTC yalnızca devre dışı olarak 300 yıl cinsinden bir saniye olabilir kesin atomik saatler çok uluslu bir örnek tarafından tanımlanır. Ancak UTC doğrudan okunurken özel bir donanım gerektirir. Bunun yerine, saat sunucuları UTC'ye eşitlenir ve ölçeklenebilirlik ve sağlamlık sağlamak için diğer bilgisayarlardan erişilir. Her bilgisayar eşitleme hizmeti çalıştıran kullanmak için hangi saat sunucuları bilir ve bilgisayar saatini düzeltilmesi gerekip gerekmediğini düzenli olarak denetler ve gerekirse süreyi ayarlar. 

Azure ana Katman 1 Microsoft'a ait cihazlardan, GPS anten ile kendi zaman iç Microsoft saat sunucuları eşitlenir. Azure sanal makineler'de ya da doğru zaman geçirmek için konakta bağlıdır (*konak zaman*) VM veya VM'yi açın doğrudan saat saat sunucusu veya her ikisinin bir birleşiminde alabilirsiniz. 

Sanal makine konak etkileşim saati da etkileyebilir. Sırasında [Bakımı koruma bellek](maintenance-and-updates.md#maintenance-not-requiring-a-reboot), Vm'leri 30 saniyeye kadar duraklatıldı. Örneğin, bakım başlamadan önce VM saat 10:00:00 AM'den gösterir ve 28 saniye sürer. VM çağrıldıktan sonra sanal makinedeki saat 10:00:00 28 saniye olabilecek'da, yine de gösterebilir devre dışı. Bu, doğru VMICTimeSync hizmet konak ve istemleri değişiklikler için VM'ler üzerinde gerçekleştirilecek dengelemek için neler olduğunu izler.

VMICTimeSync hizmet örneği veya eşitleme modda çalışır ve yalnızca bir saat ileri olumlu yönde etkiler. W32time çalışıyor olmasını gerektiren örnek modunda VMICTimeSync hizmeti konak her 5 saniyede bir yoklar ve zamanı örnekleri için W32time sağlar. Her 30 saniyede yaklaşık W32time hizmeti en son zaman örneği alır ve konuğun saat etkilemek için kullanır. Bir konuk sürdürüldü veya 5 saniyeden ana bilgisayarın saati arkasındaki bir konuğun saat drifts eşitleme modunu etkinleştirir. W32time hizmeti düzgün şekilde çalıştığı durumlarda asla ikinci durumda olmaması gerekir.

Zaman eşitleme çalışmıyor olmadan, sanal makinedeki saatin hataları accumulate. Yalnızca bir VM olduğunda, iş yüküne yüksek oranda doğru zaman tutma gerektirmedikçe etkisini önemli olmayabilir. Ancak çoğu durumda, biz birden fazla varsa, birbirine bağlı işlemler ve süre tüm dağıtım tutarlı olması gerekiyor izlemek için zaman kullanan VM'ler. Zaman VM'ler arasında farklı olduğunda, aşağıdaki etkileri görebilirsiniz:

- Kimlik doğrulaması başarısız olur. Güvenlik protokolleri Kerberos ister veya sertifika bağımlı teknolojisini kullanan sistemler arasında tutarlı olan zaman. 
- Günlükleri (veya diğer verileri) zamanında kabul etmiyorsanız ne bir sistemde bulmamız çok zordur. Aynı olay, farklı zamanlarda yapmayı bağıntı zor oluştu gibi görünür.
- Saat kapalıysa, faturalandırma yanlış hesaplanması.

Zaman eşitleme en son iyileştirmelerden kullanabileceğiniz sağlayan konuk işletim sistemi olarak Windows Server 2016'yı kullanarak Windows dağıtımlar için en iyi sonuçlar elde edersiniz.

## <a name="configuration-options"></a>Yapılandırma seçenekleri

Zaman eşitleme Windows Azure'da barındırılan sanal makineleriniz için yapılandırmaya yönelik üç seçenek vardır:

- Ana saat ve time.windows.com. Bu, Azure Market görüntüleri kullanılan varsayılan yapılandırmadır.
- Konak yalnızca.
- Başka bir, dış saat sunucusu ile veya olmadan konağı zaman kullanarak kullanın.


### <a name="use-the-default"></a>Varsayılanı kullan

Varsayılan olarak, Windows işletim sistemi VM görüntüleri için iki kaynaktan eşitlemek w32time yapılandırılır: 

- NtpClient sağlayıcısı time.windows.com bilgilerini alır.
- Konak iletişim kurmak için kullanılan VMICTimeSync hizmet, VM'ler için saat ve bakım için sanal makine duraklatıldıktan sonra düzeltmeleri yapın. Azure konakları, doğru zamanı tutmak için Microsoft'a ait katman 1 cihazları kullanın.

W32Time aşağıdaki öncelik sırasına göre zaman sağlayıcısı tercih eder: katman düzeyi, kök gecikme, kök dağılımı, saat uzaklığı. Çoğu durumda, daha düşük katman Time.Windows.com rapor ettiğinden w32time time.windows.com konağa tercih edebilir. 

Etki alanına katılan makineler için etki alanının kendisini süre eşitleme hiyerarşiye ancak orman kökü hala ihtiyacı bir yere biraz ve aşağıdaki maddeler hala true tutan oluşturur.


### <a name="host-only"></a>Yalnızca konak 

Time.Windows.com genel bir NTP sunucusu olduğundan, bu kez eşitleniyor internet üzerinden trafik gönderen gerektirir, değişen paket gecikmeler zaman eşitleme kalitesini olumsuz yönde etkileyebilir. Yalnızca konak eşitlenecek geçerek Time.Windows.com kaldırma sürenizi bazen geliştirebilir eşitleme sonuçları.

Yalnızca ana bilgisayar saati eşitleme yapar varsayılan yapılandırmayı kullanarak zaman eşitleme karşılaşırsanız, algılama sorunları geçiliyor. Bu VM üzerinde zaman eşitleme artardı görmek için yalnızca ana bilgisayar eşitleme deneyin. 

### <a name="external-time-server"></a>Dış saat sunucusu

Belirli bir zaman eşitleme gereksinimleriniz varsa da ayrıca dış saat sunucuları kullanma seçeneği mevcuttur. Dış saat sunucuları olmayan Microsoft veri merkezleri ya da işleme artık saniye içinde özel bir şekilde barındırılan makinelerle süre gerekmemesi sağlayarak, test senaryoları için yararlı olabilecek belirli bir zaman sağlayabilir.

Dış sunucuları VMICTimeSync hizmeti ve bir varsayılan yapılandırmaya benzer sonuçlar sağlamak için VMICTimeProvider ile birleştirebilirsiniz. 

## <a name="check-your-configuration"></a>Yapılandırmanızı denetleyin


NtpClient zaman sağlayıcısı açık NTP sunucuları (NTP) veya etki alanı zaman eşitleme (NT5DS) kullanacak şekilde yapılandırılmışsa denetleyin.

```
w32tm /dumpreg /subkey:Parameters | findstr /i "type"
```

VM NTP kullanıyorsa, aşağıdaki çıktıyı görürsünüz:

```
Value Name                 Value Type          Value Data
Type                       REG_SZ              NTP
```

Ne zaman sunucusu görmek için NtpClient zaman sağlayıcısı, bir yükseltilmiş komut isteminde kullanır:

```
w32tm /dumpreg /subkey:Parameters | findstr /i "ntpserver"
```

Varsayılan VM kullanıyorsanız, çıktı şuna benzeyecektir:

```
NtpServer                  REG_SZ              time.windows.com,0x8
```


Ne zaman sağlayıcısı şu anda kullanıldığını görmek için.

```
w32tm /query /source
```


Gördüğünüz çıktı ve ne anlama aşağıda verilmiştir:
    
- **time.Windows.com** -varsayılan yapılandırmasında w32time zaman time.windows.com elde edersiniz. Zaman eşitleme kalite internet bağlantısı bağlıdır ve paket gecikmeleri etkilenir. Bu, varsayılan kurulumu normal çıktısı bulunmaktadır.
- **VM IC zaman eşitleme sağlayıcısı** -VM konağından zaman eşitleniyor. Bu genellikle, yalnızca ana bilgisayar saati eşitleme için katılımı veya NtpServer şu anda kullanılabilir değilse kaynaklanır. 
- *Etki alanı sunucunuz* - geçerli makine bir etki alanında olduğundan ve etki alanı zaman eşitleme hiyerarşisi tanımlar.
- *Başka bir sunucuda* -zaman başka bir sunucudan almak için açıkça w32time yapılandırıldı. Zaman eşitleme kalitesi, bu zaman server kalitesini bağlıdır.
- **Yerel CMOS saat** -saat eşitlenmemiş. W32Time bir yeniden başlatma işleminden sonra veya tüm yapılandırılan süre kaynakları mevcut olmadığı durumlarda başlatmak için yeterli zamana sahip değilse bu çıkış elde edebilirsiniz.


## <a name="opt-in-for-host-only-time-sync"></a>Yalnızca ana bilgisayar saati eşitleme katılımı

Azure sürekli eşitleme konaklarda geliştirme çalışmaları ve tüm zaman eşitleme altyapısı Microsoft'a ait veri merkezlerinde birlikte bulunmasını garanti edebilir. Zaman eşitleme time.windows.com birincil zaman kaynağı olarak kullanmayı tercih eder varsayılan kurulumu ile ilgili sorunlar varsa, katılım için yalnızca ana bilgisayar saati eşitleme için aşağıdaki komutları kullanabilirsiniz.

VMIC sağlayıcısı etkin olarak işaretleyin. 

```
reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\TimeProviders\VMICTimeProvider /v Enabled /t REG_DWORD /d 1 /f
```

NTPClient sağlayıcısı devre dışı olarak işaretleyebilirsiniz.

```
reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\TimeProviders\NtpClient /v Enabled /t REG_DWORD /d 0 /f
```

W32time hizmeti yeniden başlatın.

```
net stop w32time && net start w32time
```


## <a name="windows-server-2012-and-r2-vms"></a>Windows Server 2012 ve R2 VM'ler 

Windows Server 2012 ve Windows Server 2012 R2'in zaman eşitleme için farklı varsayılan ayarları vardır. Varsayılan olarak w32time kesin zaman düşük yüklerle hizmeti üzerinden tercih ettiği şekilde yapılandırılır. 

Windows Server 2012 ve 2012 R2 dağıtımlarını kesin zaman tercih yeni varsayılan değerleri kullanmak taşımak istiyorsanız, aşağıdaki ayarları uygulayabilirsiniz.

W32time yoklama güncelleştirin ve aralıkları, Windows Server 2016 ayarlarınızla eşleşecek şekilde güncelleştirin.

```
reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\Config /v MinPollInterval /t REG_DWORD /d 6 /f
reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\Config /v MaxPollInterval /t REG_DWORD /d 10 /f
reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\Config /v UpdateInterval /t REG_DWORD /d 100 /f
w32tm /config /update
```

İçin yeni bir yoklama aralıkları kullanabilmek w32time NtpServers kullanarak olarak işaretlenir. Sunucuları 0x1 bit bayrak maskeyle açıklamalı olan, bu mekanizma geçersiz kılarsınız ve w32time SpecialPollInterval yerine kullanırsınız. Belirtilen NTP sunucuları ya da 0x8 bayrağı veya bayrak hiç kullandığınızdan emin olun:

Hangi bayrakları için kullanılan NTP sunucularını kullanıldığını denetleyin.

```
w32tm /dumpreg /subkey:Parameters | findstr /i "ntpserver"
```

## <a name="next-steps"></a>Sonraki adımlar

Zaman eşitleme hakkında daha fazla ayrıntı için bağlantılar aşağıda verilmiştir:

- [Windows Zaman hizmeti araçları ve ayarları](https://docs.microsoft.com/windows-server/networking/windows-time-service/Windows-Time-Service-Tools-and-Settings)
- [Windows Server 2016 geliştirmeleri ](https://docs.microsoft.com/windows-server/networking/windows-time-service/windows-server-2016-improvements)
- [Windows Server 2016 için doğru zamanı](https://docs.microsoft.com/windows-server/networking/windows-time-service/accurate-time)
- [Yüksek doğruluk ortamları için Windows Saat hizmeti yapılandırmak için destek sınır](https://docs.microsoft.com/windows-server/networking/windows-time-service/support-boundary)


