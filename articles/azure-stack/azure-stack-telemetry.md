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
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/19/2019
ms.author: jeffgilb
ms.reviewer: comartin
ms.lastreviewed: 10/15/2018
ms.openlocfilehash: 8d1e13b5bcda174c979205537f4c484aa94dce10
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/20/2019
ms.locfileid: "56428831"
---
# <a name="azure-stack-telemetry"></a>Azure Stack telemetri

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure Stack telemetrisini sistem verilerini bağlı kullanıcı deneyimi üzerinden Microsoft'a otomatik olarak yükler. Microsoft teams, müşteri deneyimlerini geliştirmek için Azure Stack telemetri toplayan verileri kullanın. Bu veriler, güvenlik, sistem durumu, kalite ve performans analizi için de kullanılır.

Azure Stack operatör için telemetri Kurumsal dağıtımlar değerli Öngörüler sağlayabilir ve Azure yığını'nın gelecek sürümlerini şekli yardımcı olan bir sesli sağlar.

> [!NOTE]
> Azure Stack, kullanım bilgilerini faturalandırması Azure'a iletmek için de yapılandırabilirsiniz. Bu,-,-Kullandıkça Ödeme seçen çok düğümlü Azure Stack müşterileri için gereklidir. Kullanım raporlama alınan telemetri bağımsız olarak denetlenir ve kapasite modeli istemeyen çok düğümlü müşterilere veya Azure Stack geliştirme Seti'ni kullanıcılar gerekli değildir. Bu senaryolar için kullanım raporlama kapatılabilir [kayıt betiği kullanarak](https://docs.microsoft.com/azure/azure-stack/azure-stack-usage-reporting).

Azure Stack telemetri kullanan Windows Server 2016 bağlı kullanıcı deneyimi ve Telemetri bileşeni, temel [olay izleme için Windows (ETW)](https://msdn.microsoft.com/library/dn904632(v=vs.85).aspx) TraceLogging teknoloji toplamak ve olayları ve verileri depolamak için. Azure Stack bileşenleri, olayları ve olay günlüğünü genel işletim sistemi ve API'leri izleme kullanarak topladığınız verileri yayımlamak için aynı teknolojiyi kullanın. Bu sağlayıcıları bu Azure Stack bileşenlerin örnekleridir: Ağ kaynağı, depolama kaynağı, kaynak izleme ve kaynak güncelleştirin. Bağlı kullanıcı deneyimi ve Telemetri bileşeni, SSL kullanarak verileri şifreler ve sertifika sabitleme Microsoft Veri Yönetimi hizmetine HTTPS üzerinden veri iletmek için kullanır.

> [!IMPORTANT]
> Telemetri veri akışı etkinleştirmek için bağlantı noktası 443 (HTTPS), ağınızdaki açık olması gerekir. Microsoft Veri Yönetimi hizmetine bağlı kullanıcı deneyimi ve Telemetri bileşen bağlandığı https://v10.vortex-win.data.microsoft.com. Bağlı kullanıcı deneyimi ve Telemetri bileşeni de bağlandığı https://settings-win.data.microsoft.com yapılandırma bilgilerini indirilemedi.

## <a name="privacy-considerations"></a>Gizlilik konuları

ETW hizmeti, geri korumalı bulut depolama alanına telemetri verilerini yönlendirir. En az ayrıcalık sorumlusu erişim telemetri verilerini gösterir. Yalnızca geçerli bir iş gereksinimi olan Microsoft personeli, telemetri verileri için erişim verilir. Microsoft olmayan paylaşabilirsiniz kişisel müşteri verilerini üçüncü taraflarla dışında müşterinin takdirine bağlı olarak veya açıklanan sınırlı amacıyla [Microsoft gizlilik bildirimi](https://privacy.microsoft.com/PrivacyStatement). OEM'ler ve iş ortakları ile paylaşılan iş raporlar, toplu, anonimleştirilmiş veriler içerir. Veri paylaşımı kararlar, gizlilik, hukuk ve veri yönetim proje katılımcıları da dahil olmak üzere bir iç Microsoft ekibi tarafından yapılır.

Microsoft olarak düşündüğü ve bilgi küçültme yöntemler. Yalnızca gerekli değil ve bunun için yalnızca olduğu sürece bir hizmet sağlamak için gereken veya analiz için Depolama bilgileri toplamak için elimizden. Azure Stack sistemi ve Azure hizmetleri nasıl çalıştığını ilgili bilgilerin çoğunu altı ay içinde silinir. Özetlenen veya daha uzun bir süre için toplanan veriler korunur.

Müşteri bilgileri, güvenlik ve gizlilik olduğunu önemli biliyoruz.  Microsoft, müşteri gizliliğini ve Azure stack'teki Müşteri verilerinin korunmasında Düşünceli ve kapsamlı bir yaklaşım alır. BT yöneticileri, herhangi bir zamanda özellikleri ve gizlilik ayarlarını özelleştirmek için denetimleri içerir. Saydamlık ve güven bağlılığımız açıktır:

- Biz toplamak veri türleri hakkında daha fazla müşterilerle açık duyuyoruz.
- Biz kurumsal müşterilere denetimine koyabilir; kullanıcılar, kendi gizlilik ayarlarını özelleştirebilirsiniz.
- Size müşteri gizliliği ve güvenliği başa koyun.
- Telemetri verileri nasıl kullanılır hakkında saydam duyuyoruz.
- Telemetri verilerinin müşteri deneyimlerini geliştirmek için kullanırız.

Microsoft, kredi kartı numaraları, kullanıcı adları ve parolalar, e-posta adresleri veya benzer hassas bilgiler gibi hassas verileri toplamak istediğiniz değil. Hassas bilgilerin yanlışlıkla alındı karar verirseniz, sileceğiz.

## <a name="examples-of-how-microsoft-uses-the-telemetry-data"></a>Microsoft, telemetri verilerini kullanma örnekleri

Telemetri hızla belirleyip müşteri dağıtımları ve yapılandırmalar kritik güvenilirlik sorunları düzeltmek için yardımcı olacak önemli bir rol oynar. Insights telemetri verileri, hizmetleri veya donanım yapılandırmaları ile ilgili sorunları belirlemenize yardımcı olabilir. Microsoft'un bu veriler müşteriler ve sürücü geliştirmeleri ekosistemine alın olanağı tümleşik Azure Stack çözümleri kalitesi çubuğu oluşturur.

Telemetri ayrıca Microsoft daha iyi müşteriler bileşenlerini nasıl dağıtacağınızı anlamanıza, özelliklerini kullanmak ve iş hedeflerine ulaşması için kullanmanıza yardımcı olur. Bu Öngörüler, müşteri deneyimlerini ve iş yüklerini doğrudan etkileyebilir alanlarda mühendislik yatırımlarına öncelik tanımamıza yardımcı olur.

Müşteri kullanımını kapsayıcıları, depolama ve Azure Stack rolleri ile ilişkilendirilmiş ağ yapılandırmaları buna örnek verilebilir. Sürücü geliştirmeleri ve Azure Stack yönetim ve izleme çözümleri zekasından öngörüleri de kullanırız. Bu geliştirmeler, sorunların tanılanması faydalanılan müşteriler kolaylaştırmak ve daha az destek yaparak tasarruf Microsoft'a çağırır.

## <a name="manage-telemetry-collection"></a>Telemetri toplama yönetme

Biz, kuruluşunuzdaki telemetri için önerilen kapatma yok. Ancak, bazı senaryolarda bu gerekli olabilir.

Bu senaryolarda, Azure Stack dağıtmadan önce kayıt defteri ayarlarını kullanarak veya Azure Stack dağıttıktan sonra Telemetri uç noktaları kullanarak Microsoft'a gönderilecek telemetri düzeyini yapılandırabilirsiniz.

### <a name="telemetry-levels-and-data-collection"></a>Telemetri düzeyleri ve veri toplama

Telemetri ayarlarını değiştirmeden önce telemetri düzeyleri ve her düzeyde hangi verilerin toplandığını anlamanız gerekir.

Telemetri ayarlarını, toplu ve aşağıdaki gibi kategorilere ayrılmış dört düzeyleri (0-3) içinde gruplandırılır:

**0 (güvenlik)**</br>
Yalnızca güvenlik verileri. İşletim sisteminin güvenliğini sağlamak için gereken bilgileri. Bu, bağlı kullanıcı deneyimi ve Telemetri bileşen ayarları ve Windows Defender hakkında veri içerir. Azure Stack'e özel telemetri yok, bu düzeyde yayınlanır.

**1 (Temel)**</br>
Güvenlik verilerinin ve temel sistem durumu ve kalitesi veri. Temel cihaz bilgileri dahil olmak üzere: kalite ilgili veriler, uygulama uyumluluğu, uygulama kullanımı verilerini, verileri ve **güvenlik** düzeyi. Telemetri düzeyinizi temel etkinleştirir Azure Stack telemetri ayarlanıyor. Bu düzeyde toplanan verileri içerir:

- *Temel cihaz bilgilerini* türleri ve yerel ve sanal Windows Server 2016 örneklerin ekosistemindeki yapılandırmalarına hakkında bir anlayış sağlar. Buna aşağıdakiler dahildir:

  - OEM ve model gibi makine öznitelikleri.
  - Ağ bağdaştırıcılarının sayısını ve hızını gibi ağ öznitelikleri.
  - İşlemci ve bellek gibi özniteliklerini çekirdek sayısı ve yüklü bellek miktarı.
  - Sürücü sayısı, türü sürücü ve disk boyutu gibi depolama öznitelikleri.

- *Telemetri işlevselliği*, karşıya yüklenen olayları, bırakılan olayları ve en son verileri yüzdesi de dahil olmak üzere karşıya yükleme zamanı.
- *Kalite ilgili bilgiler* Microsoft, Azure Stack performansıyla ilgili temel bilgilere geliştirmenize yardımcı olur. Örneğin, belirli donanım yapılandırmasını kritik uyarı sayısı.
- *Uyumluluk verileri* yardımcı ilgili kaynak sağlayıcıları yüklü bir sistem ve bir sanal makine üzerinde bir anlayış sağlayın. Bu, olası uyumluluk sorunları tanımlar.

**2 (Gelişmiş)**</br>
Dahil olmak üzere ek Öngörüler: işletim sistemi ve Azure Stack hizmetlerinin nasıl kullanıldığı, bu hizmetleri nasıl gerçekleştirmek, Gelişmiş güvenilirlik verileri ve verilerden **güvenlik** ve **temel** düzeyleri.

> [!NOTE]
> Telemetri varsayılan ayar budur.

**3 (tam)**</br>
Tüm belirlemek ve sorunları gidermeye yardımcı olmak gereken verileri, artı verilerden **güvenlik**, **temel**, ve **geliştirilmiş** düzeyleri.

> [!IMPORTANT]
> Bu telemetri düzeyleri, yalnızca Microsoft Azure Stack bileşenleri için geçerlidir. Microsoft olmayan yazılım bileşenlerini ve Azure Stack donanım iş ortaklarından donanım yaşam döngüsü konakta çalışan hizmetleri bu telemetri düzeyleri dışında bulut Hizmetleri ile iletişim kurabilir. Telemetri ilkesini ve nasıl kabul et veya geri çevirmek anlamak için Azure Stack donanım çözümü sağlayıcınız ile çalışması gerekir.

Windows ve Azure Stack telemetriyi kapatma SQL telemetri de devre dışı bırakır. Windows Server telemetri ayarlarını etkileri hakkında daha fazla bilgi için bkz. [Windows Telemetri teknik incelemesi](https://aka.ms/winservtelemetry).

### <a name="asdk-set-the-telemetry-level-in-the-windows-registry"></a>ASDK: Windows kayıt defterinde telemetri düzeyi

Azure Stack dağıtmadan önce fiziksel ana bilgisayarda el ile telemetri düzeyini ayarlamak için Windows Kayıt Defteri Düzenleyicisi'ni kullanabilirsiniz. Yönetim ilkesi, Grup İlkesi gibi zaten varsa şu kayıt defteri ayarını geçersiz kılar.

Azure Stack Geliştirme Seti konakta dağıtma önce CloudBuilder.vhdx önyükleme ve yükseltilmiş bir PowerShell penceresinde aşağıdaki betiği çalıştırın:

```powershell
### Get current AllowTelemetry value on DVM Host
(Get-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\DataCollection" `
-Name AllowTelemetry).AllowTelemetry
### Set & Get updated AllowTelemetry value for ASDK-Host
Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\DataCollection" `
-Name "AllowTelemetry" -Value '0' # Set this value to 0,1,2,or3.  
(Get-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\DataCollection" `
-Name AllowTelemetry).AllowTelemetry
```

### <a name="asdk-and-multi-node-enable-or-disable-telemetry-after-deployment"></a>ASDK ve çok düğümlü: etkinleştirin veya dağıtımdan sonra telemetri devre dışı bırak

Etkinleştirmek veya dağıtımdan sonra telemetri devre dışı bırakmak için erişim için ayrıcalıklı uç noktasına (ERCS Vm'lerde ifşa eden CESARETLENDİRİCİ) olması gerekir.

1. Etkinleştirmek için: `Set-Telemetry -Enable`
2. Devre dışı bırakmak için: `Set-Telemetry -Disable`

PARAMETRE ayrıntıları:
> . PARAMETRE Enable - açma telemetri verilerini karşıya yükleme</br>
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

[Azure Stack Azure ile kaydedin](azure-stack-registration.md)