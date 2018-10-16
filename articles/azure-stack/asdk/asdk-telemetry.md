---
title: Azure Stack telemetrisini | Microsoft Docs
description: PowerShell kullanarak Azure Stack telemetrisini ayarlarının nasıl yapılandırılacağı açıklanır.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/15/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 0e8138f66c9284531b9610c9bc2996974e2075ad
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49339518"
---
# <a name="azure-stack-telemetry"></a>Azure Stack telemetri

Azure Stack sistemi verileri veya telemetri, bağlı kullanıcı deneyimi üzerinden Microsoft'a otomatik olarak yüklenir. Azure Stack telemetrisini toplanan veriler, öncelikle bizim müşteri deneyimlerini geliştirmek üzere Microsoft teams'e ve güvenlik, sistem durumu, kalitesini ve performansını analiz için kullanılır.

Azure Stack operatör olarak telemetri Kurumsal dağıtımlar değerli Öngörüler sağlayabilir ve Azure yığını'nın gelecek sürümlerini şekli yardımcı olan bir sesli sağlar.

> [!NOTE]
> Azure Stack, ayrıca faturalandırması azure'a iletme kullanım bilgileri için yapılandırılabilir. Bu,-,-Kullandıkça Ödeme seçen çok düğümlü Azure Stack müşterileri için gereklidir. Kullanım raporlama alınan telemetri bağımsız olarak denetlenir ve kapasite modeli istemeyen çok düğümlü müşterilere veya Azure Stack geliştirme Seti'ni kullanıcılar gerekli değildir. Bu senaryolar için kullanım raporlama kapatılabilir [kayıt betiği kullanarak](https://docs.microsoft.com/azure/azure-stack/azure-stack-usage-reporting).

Azure Stack telemetri kullanan Windows Server 2016 bağlı kullanıcı deneyimi ve Telemetri bileşeni, temel [olay izleme için Windows (ETW)](https://msdn.microsoft.com/library/dn904632(v=vs.85).aspx) izleme günlüğü teknoloji toplamak ve telemetri olayları ve verileri depolamak için. Azure yığını bileşenlerini, olayları ve olay günlüğünü genel işletim sistemi ve API'leri izleme kullanarak topladığınız verileri yayımlamak için aynı günlük teknolojisini kullanın. Ağ kaynak sağlayıcısı, depolama kaynak sağlayıcısı, kaynak sağlayıcısı izleme ve güncelleştirme kaynak sağlayıcısı Azure Stack bileşenlerin örnekleridir. Bağlı kullanıcı deneyimi ve Telemetri bileşeni, SSL kullanarak verileri şifreler ve telemetri verilerini HTTPS üzerinden Microsoft Veri Yönetimi hizmetine iletmek için sertifika sabitleme kullanır.

> [!NOTE]
> Telemetri veri akışı desteklemek için bağlantı noktası 443 (HTTPS), ağınızdaki açık olması gerekir. Microsoft Veri Yönetimi hizmetine bağlı kullanıcı deneyimi ve Telemetri bileşen bağlandığı https://v10.vortex-win.data.microsoft.com. Bağlı kullanıcı deneyimi ve Telemetri bileşeni de bağlandığı https://settings-win.data.microsoft.com yapılandırma bilgilerini indirilemedi.

## <a name="privacy-considerations"></a>Gizlilik konuları
ETW hizmeti, geri korumalı bulut depolama alanına telemetri verilerini yönlendirir. Telemetri verilerini en az ayrıcalıklı kılavuzları erişim ilkesi. Yalnızca Microsoft personeli ile geçerli bir iş gereksinimi telemetri verilerini erişmesine izin verilir. Microsoft paylaşamaz kişisel veriler, müşterilerimizin üçüncü taraflarla dışında müşterinin takdirine bağlı olarak veya açıklanan sınırlı amacıyla [Azure Stack gizlilik bildirimi](https://privacy.microsoft.com/PrivacyStatement). İş rapor OEM'ler ve toplanmış, anonim telemetri bilgilerini içeren iş ortakları ile paylaşır. Veri paylaşımı kararlar, gizlilik, hukuk ve veri yönetim proje katılımcıları da dahil olmak üzere bir iç Microsoft ekibi tarafından yapılır.

Microsoft olarak düşündüğü ve bilgi küçültme yöntemler. Yalnızca ihtiyacımız ve onu yalnızca en çok bir hizmet sağlamak için gerekli olan olarak ya da analiz depolarız bilgileri toplamak için elimizden. Azure Stack sistemi ve Azure hizmetleri nasıl çalıştığını ilgili bilgilerin çoğunu altı ay içinde silinir. Özetlenen veya toplu veriler için daha uzun bir süre tutulur.

Müşterilerimizin bilgilerin güvenliği ve gizlilik olduğunu önemli biliyoruz. Müşteri gizliliği ve Azure Stack ile müşteri verilerinin korunması için biz Düşünceli ve kapsamlı bir yaklaşım yönlendirdik. BT yöneticileri, herhangi bir zamanda özellikleri ve gizlilik ayarlarını özelleştirmek için denetimleri içerir. Saydamlık ve güven bağlılığımız açıktır:
- Biz toplamak veri türleri hakkında daha fazla müşterilerle açık duyuyoruz.
- Biz kurumsal müşterilere denetimine koyabilir; kullanıcılar, kendi gizlilik ayarlarını özelleştirebilirsiniz.
- Size müşteri gizliliği ve güvenliği başa koyun.
- Telemetri nasıl kullanılır hakkında saydam duyuyoruz.
- Telemetri müşteri deneyimlerini geliştirmek için kullanırız.

Microsoft, kredi kartı numaraları, kullanıcı adları ve parolalar, e-posta adresleri veya diğer benzer şekilde hassas bilgiler gibi hassas bilgileri toplamak istememektedir. Hassas bilgilerin yanlışlıkla alındı karar verirseniz, sileceğiz.

## <a name="examples-of-how-microsoft-uses-the-telemetry-data"></a>Microsoft, telemetri verilerini kullanma örnekleri
Telemetri, hızlı bir şekilde tanımlamak ve müşterilerin dağıtımları ve yapılandırmalar kritik güvenilirlik sorunları düzeltmek yardımcı olacak önemli bir rol oynar. Biz toplayan telemetri verilerini Öngörüler Hizmetleri veya donanım yapılandırmaları ile sorunları hızlıca tanımlamanıza yardımcı olur. Microsoft'un müşterileri ve ekosistemi sürücü artışlarını bu veri alma olanağı, tümleşik Azure Stack Çözümlerimiz kalitesi için çıtayı yardımcı olur.

Telemetri ayrıca Microsoft daha iyi müşteriler bileşenlerini nasıl dağıtacağınızı anlamanıza, özelliklerini kullanmak ve iş hedeflerine ulaşması için kullanmanıza yardımcı olur. Bu verilerden öngörü alma yardımcı olur, müşterilerin deneyimlerini ve iş yüklerini doğrudan etkileyebilir alanlarda mühendislik yatırımlarına öncelik tanımamıza.

Müşteri kullanımını kapsayıcıları, depolama ve Azure Stack rolleri ile ilişkilendirilmiş ağ yapılandırmaları buna örnek verilebilir. Sürücü geliştirmeleri ve bazı bizim yönetim ve izleme çözümleri zekasından öngörüleri de kullanırız. Bu, müşterilere kalite sorunları tanılamanıza yardımcı olur ve daha az destek yaparak tasarruf Microsoft'a çağırır.

## <a name="manage-telemetry-collection"></a>Telemetri toplama yönetme
Telemetri geliştirilmiş ürün işlevselliği ve kararlılık sürücüleri veri sağladığından kuruluşunuzda telemetriyi kapatma önermiyoruz. Biz ancak bazı senaryolarda bu gerekebileceğini aklınızda tanır.

Bu gibi durumlarda, kayıt defteri ayarları dağıtım öncesi veya dağıtım sonrası Telemetri uç noktaları kullanarak tarafından Microsoft'a gönderilen telemetri düzeyini yapılandırabilirsiniz.

### <a name="set-telemetry-level-in-the-windows-registry"></a>Windows kayıt defterinde telemetri düzeyini ayarlayın
Windows Kayıt Defteri Düzenleyicisi'ni el ile Azure Stack dağıtmadan önce fiziksel ana bilgisayarda telemetri düzeyini ayarlamak için kullanılır. Yönetim ilkesi, Grup İlkesi gibi zaten varsa şu kayıt defteri ayarını geçersiz kılar.

Azure Stack Geliştirme Seti konakta dağıtma önce CloudBuilder.vhdx önyükleme ve yükseltilmiş bir PowerShell penceresinde aşağıdaki betiği çalıştırın:

```powershell
### Get current AllowTelmetry value on DVM Host
(Get-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\DataCollection" `
-Name AllowTelemetry).AllowTelemetry
### Set & Get updated AllowTelemetry value for ASDK-Host
Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\DataCollection" `
-Name "AllowTelemetry" -Value '0' # Set this value to 0,1,2,or3.  
(Get-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\DataCollection" `
-Name AllowTelemetry).AllowTelemetry
```

Toplu ve dört düzeyleri (0-3) halinde kategorilere ayrılmış telemetri düzeyleri şunlardır:

**0 (güvenlik)**. Yalnızca güvenlik verileri. İşletim sistemi tutmaya yardımcı olmak için gereken bilgileri güvenli, bağlı kullanıcı deneyimi ve Telemetri bileşen ayarları ve Windows Defender hakkında bilgileri de dahil olmak üzere. Azure Stack özel telemetri yok, bu düzeyde yayınlanır.

**1 (Temel)**. Güvenlik verilerinin ve temel sistem durumu ve kalitesi veri. Temel cihaz bilgileri dahil olmak üzere: kalite ilgili veriler, uygulama uyumluluğu, uygulama kullanımı verilerini ve güvenlik düzeyi verileri. Telemetri düzeyinizi temel etkinleştirir Azure Stack telemetri ayarlanıyor. Bu düzeyde toplanan verileri içerir:

- **Temel cihaz bilgilerini** yardımcı olarak ekosistemine yerel ve sanallaştırılmış Windows Server 2016 örneklerin yapılandırmaları ve türleri hakkında bir anlayış sağlamak da dahil olmak üzere:
  - OEM gibi makine özniteliklerini modeli
  - Ağ sayısı ve hızı ağ bağdaştırıcıları gibi öznitelikleri
  - İşlemci ve bellek öznitelikleri, çekirdek sayısı gibi bellek boyutu
  - Sürücüleri, türü ve boyut sayısı gibi depolama öznitelikleri.
- **Telemetri işlevselliği**de dahil olmak üzere karşıya yüklenen olayları, bırakılan olayları ve son yüzdesi zaman karşıya yükleyin.
- **Kalite ilgili bilgiler** Microsoft, Azure Stack performansıyla ilgili temel bilgilere geliştirmenize yardımcı olur. Belirli bir donanım yapılandırmasını kritik uyarı sayısı buna bir örnektir.
- **Uyumluluk verileri**, yardımcı olan kaynak sağlayıcıları sistemi ve sanal makine üzerinde yüklü olan ve olası uyumluluk sorunları tanımlar olduğu hakkında bir anlayış sağlayın.

**2 (Gelişmiş)**. Dahil olmak üzere ek Öngörüler: işletim sistemi ve diğer Azure Stack hizmetlerinin nasıl kullanıldığı, gerçekleştirdikleri nasıl, Gelişmiş güvenilirlik verileri ve hem temel hem de güvenlik düzeyleri verileri.

**3 (tam)**. Tüm belirlemek ve sorunları gidermeye yardımcı olmak gereken verileri, artı verilerden **güvenlik**, **temel**, ve **geliştirilmiş** düzeyleri.

> [!NOTE]
> Varsayılan telemetri Düzey 2 (Gelişmiş) değerdir.

Windows ve Azure Stack telemetriyi kapatma SQL telemetri devre dışı bırakır. Windows Server telemetri ayarlarını etkileri hakkında ek bilgi için başvuru [Windows Telemetri teknik incelemesi](https://aka.ms/winservtelemetry).

> [!IMPORTANT]
> Bu telemetri düzeyleri, yalnızca Microsoft Azure Stack bileşenleri için geçerlidir. Microsoft olmayan yazılım bileşenlerini ve Azure Stack donanım iş ortaklarından donanım yaşam döngüsü konakta çalışan hizmetleri bu telemetri düzeyleri dışında bulut Hizmetleri ile iletişim kurabilir. Telemetri ilkesini ve nasıl kabul et veya geri çevirmek anlamak için Azure Stack donanım çözümü sağlayıcınız ile çalışması gerekir.

### <a name="enable-or-disable-telemetry-after-deployment"></a>Etkinleştirmek veya dağıtımdan sonra telemetri devre dışı bırak

Etkinleştirmek veya dağıtımdan sonra telemetri devre dışı bırakmak için erişim için ayrıcalıklı uç noktasına (ERCS Vm'lerde ifşa eden CESARETLENDİRİCİ) olması gerekir.
1.  Etkinleştirmek için: `Set-Telemetry -Enable`
2.  Devre dışı bırakmak için: `Set-Telemetry -Disable`

PARAMETRE Ayrıntısı:
> . PARAMETRE Enable - açma telemetri verilerini karşıya yükleme

> . PARAMETRE devre dışı bırak - telemetri verilerini karşıya yükleme devre dışı bırak  

**Telemetri etkinleştirmek için komut dosyası:**
```powershell
$ip = "<IP ADDRESS OF THE PEP VM>" # You can also use the machine name instead of IP here.
$pwd= ConvertTo-SecureString "<CLOUD ADMIN PASSWORD>" -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential ("<DOMAIN NAME>\CloudAdmin", $pwd)
$psSession = New-PSSession -ComputerName $ip -ConfigurationName PrivilegedEndpoint -Credential $cred
Invoke-Command -Session $psSession {Set-Telemetry -Enable}
if($psSession)
{
    Remove-PSSession $psSession
}
```

**Telemetri devre dışı bırakmak için betiği:**
```powershell
$ip = "<IP ADDRESS OF THE PEP VM>" # You can also use the machine name instead of IP here.
$pwd= ConvertTo-SecureString "<CLOUD ADMIN PASSWORD>" -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential ("<DOMAIN NAME>\CloudAdmin", $pwd)
$psSession = New-PSSession -ComputerName $ip -ConfigurationName PrivilegedEndpoint -Credential $cred
Invoke-Command -Session $psSession {Set-Telemetry -Disable}
if($psSession)
{
    Remove-PSSession $psSession
}
```

## <a name="next-steps"></a>Sonraki adımlar
[Başlatma ve durdurma ASDK](asdk-start-stop.md)
