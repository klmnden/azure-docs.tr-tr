---
title: Azure yığın telemetri | Microsoft Docs
description: PowerShell kullanarak Azure yığın telemetri ayarlarının nasıl yapılandırılacağı açıklanmaktadır.
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
ms.date: 06/07/2018
ms.author: jeffgilb
ms.reviewer: comartin
ms.openlocfilehash: ed3f09f942bdaa803ae8024d5c02230173190107
ms.sourcegitcommit: 50f82f7682447245bebb229494591eb822a62038
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35248206"
---
# <a name="azure-stack-telemetry"></a>Azure yığın telemetri

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Azure yığın telemetri sistem verileri bağlı kullanıcı deneyimi aracılığıyla Microsoft'a otomatik olarak yükler. Microsoft ekipleri müşteri deneyimleri geliştirmek için Azure yığın telemetri toplayan verileri kullanın. Bu veriler, güvenlik, sistem durumu, kalite ve performans analizi için de kullanılır.

Bir Azure yığın işleç için telemetri Kurumsal dağıtımlar değerli Öngörüler sağlayabilir ve Azure yığın gelecek sürümlerinde şekli yardımcı olan bir sesli verir.

> [!NOTE]
> Azure kullanım bilgileri için fatura Azure'a iletmek için yığın da yapılandırabilirsiniz. Bu ödeme olarak,-kullanımlı faturalama seçen çok düğümlü Azure yığın müşteriler için gereklidir. Kullanım raporlama telemetrisinden bağımsız olarak denetlenir ve kapasite modelini seçin çok düğümlü müşteriler veya Azure yığın Geliştirme Seti kullanıcılar gerekli değildir. Bu senaryolarda, kullanım raporlama kapatılabilir [kayıt komut dosyası kullanarak](https://docs.microsoft.com/azure/azure-stack/azure-stack-usage-reporting).

Azure yığın telemetri kullanan Windows Server 2016 bağlı kullanıcı deneyimi ve Telemetri bileşeni, temel [olay izleme için Windows (ETW)](https://msdn.microsoft.com/library/dn904632(v=vs.85).aspx) toplamak ve olayları ve verilerini depolamak için TraceLogging teknolojisi. Azure yığın bileşenleri, olaylar ve ortak işletim sistemi olay günlüğü ve izleme API'leri kullanılarak toplanan verileri yayımlamak için aynı teknolojiyi kullanır. Bu Azure yığın bileşenleri örnekleri bu sağlayıcıları içerir: ağ kaynağı, depolama kaynak, kaynak izleme ve güncelleştirme kaynağı. Bağlı kullanıcı deneyimi ve Telemetri bileşeni, SSL kullanarak verileri şifreler ve Microsoft Veri Yönetimi Hizmeti HTTPS üzerinden veri iletmek için sertifika sabitleme kullanır.

> [!IMPORTANT]
> Telemetri veri akışını etkinleştirmek için bağlantı noktası 443 (HTTPS), ağınızdaki açık olması gerekir. Microsoft Veri Yönetimi Hizmeti bağlı kullanıcı deneyimi ve Telemetri bileşeni bağlandığı https://v10.vortex-win.data.microsoft.com. Bağlı kullanıcı deneyimi ve Telemetri bileşeni de bağlanır https://settings-win.data.microsoft.com yapılandırma bilgilerini karşıdan yüklemek için.

## <a name="privacy-considerations"></a>Gizlilik konuları

ETW hizmeti geri korunan bulut depolama alanına telemetri verilerini yönlendirir. En az ayrıcalık prensibi erişim telemetri verilerini size rehberlik eder. Yalnızca Microsoft personeli geçerli iş gereksinimlerine telemetri verileri için erişim verilir. Microsoft olmayan paylaşmak kişisel müşteri verilerini üçüncü taraflarla dışında Müşteri'nin tedbirli veya açıklanan sınırlı amacıyla [Microsoft gizlilik bildirimi](https://privacy.microsoft.com/PrivacyStatement). OEM'ler ve iş ortakları ile paylaşılan iş raporları toplanmış, anonim verileri içerir. Veri kararları paylaşımı gizlilik, yasal ve veri yönetimi Paydaşlar dahil olmak üzere bir iç Microsoft ekibi tarafından yapılır.

Microsoft, içinde bulunduğu ve bilgi minimization yöntemler. Gereklidir ve onun için depolamak yalnızca sürece bir hizmet sağlamak için gereken ya da analiz bilgileri toplamak çalışmalarımızı. Azure yığın sistem ve Azure hizmetleri nasıl çalışır durumda olduğundan hakkında bilgilerin çoğunu altı ay içinde silinir. Özetlenen veya daha uzun bir süre boyunca toplanan veriler korunur.

Müşteri bilgileri, güvenlik ve gizlilik olduğunu önemli anlayın.  Microsoft, müşteri gizliliği ve Azure yığınında Müşteri verilerinin korumasını Düşünceli ve kapsamlı bir yaklaşım alır. BT yöneticileri herhangi bir zamanda özellikleri ve gizlilik ayarlarını özelleştirmek için denetimler sahiptir. Saydamlık ve güven müşterilerimize taahhütler temizleyin:

- Biz toplamak veri türleri hakkında müşterilerle açık çalışıyoruz.
- Biz Kurumsal müşteriler denetiminde put — kendi gizlilik ayarlarını özelleştirebilirsiniz.
- Biz müşteri gizlilik ve güvenlik ilk yerleştirin.
- Telemetri verileri kullanılma hakkında saydam çalışıyoruz.
- Telemetri verileri müşteri deneyimleri geliştirmek için kullanırız.

Microsoft, kredi kartı numaraları, kullanıcı adları ve parolalar, e-posta adresleri veya benzer hassas bilgiler gibi hassas verileri toplamak istediğiniz değil. Hassas bilgilerin yanlışlıkla alındı karar verirseniz, sileriz.

## <a name="examples-of-how-microsoft-uses-the-telemetry-data"></a>Microsoft telemetri verilerini nasıl kullandığı örnekleri

Telemetri hızlı bir şekilde tanımlamak ve müşteri dağıtımları ve yapılandırmalara kritik güvenilirlik sorunlarını gidermek için yardımcı olacak önemli bir rol oynar. Insights telemetri verileri, hizmetleri veya donanım yapılandırmaları ile sorunları tanımlamaya yardımcı olabilir. Bu veri ekosistemine, müşteriler ve sürücü iyileştirmeleri almak için Microsoft'un özelliği tümleşik Azure yığın çözümleri kalitesini çubuğu başlatır.

Telemetri daha iyi müşteriler bileşenleri nasıl dağıtıldığına anlamak, özelliklerini kullanmak ve kendi iş hedeflerinize ulaşmak için hizmetleri kullanmak için Microsoft da yardımcı olur. Bu Öngörüler müşteri deneyimleri ve iş yükleri doğrudan etkileyebilir alanlarda mühendislik Yatırımlar öncelik yardımcı olur.

Bazı örnekler kapsayıcıları, depolama ve Azure yığın rolleriyle ilişkili olan ağ yapılandırmaları müşteri kullanımını içerir. Ayrıca sürücü geliştirmeleri ve Azure yığın yönetim ve çözümlerini izleme Intelligence ınsights'a kullanırız. Bu geliştirmeler, müşterilerin sorunları tanılamak kolaylaştırır ve daha az destek yaparak tasarruf Microsoft'a çağırır.

## <a name="manage-telemetry-collection"></a>Telemetri koleksiyonunu yönetme

Kuruluşunuzdaki telemetri için önerilen kapatma yok. Ancak, bazı senaryolarda bu gerekli olabilir.

Bu senaryolarda, Microsoft Azure yığın dağıtmadan önce kayıt defteri ayarlarını kullanarak ya da Azure yığın dağıttıktan sonra Telemetri uç noktalarını kullanarak gönderilen telemetri düzeyini yapılandırabilirsiniz.

### <a name="telemetry-levels-and-data-collection"></a>Telemetri düzeyleri ve veri toplama

Telemetri ayarları değiştirmeden önce telemetri düzeyleri ve hangi verileri her düzeyde toplanan anlamanız gerekir.

Telemetri ayarlarını toplu ve aşağıdaki gibi kategorilere ayrılmış dört düzeyleri (0-3) halinde gruplandırılır:

**0 (güvenlik)**</br>
Yalnızca güvenlik verileri. İşletim sistemi güvenli tutmak için gerekli olan bilgileri. Bu, bağlı kullanıcı deneyimi ve Telemetri bileşen ayarları ve Windows Defender hakkında bilgileri içerir. Azure yığınına özel telemetri yok, bu düzeyde yayınlanır.

**1 (Temel)**</br>
Güvenlik veri ve temel sağlık ve kalite veri. Temel aygıt bilgileri de dahil olmak üzere: kalitesi ile ilgili veriler, uygulama uyumluluğu, uygulama kullanımı verilerini, verileri ve **güvenlik** düzeyi. Telemetri düzeyinizi temel etkinleştirir Azure yığın telemetri için ayarlama. Bu düzeyde toplanan verilerin içerir:

- *Temel aygıt bilgilerini* bilinmesini türleri ve yerel ve sanal Windows Server 2016 örneklerinin ekosistemindeki yapılandırmaları hakkında sağlar. Buna aşağıdakiler dahildir:

  - OEM ve model gibi makine öznitelikleri.
  - Ağ bağdaştırıcısı sayısını ve bunların hızını gibi ağ öznitelikleri.
  - İşlemci ve bellek öznitelikleri, çekirdek sayısı ve yüklü bellek miktarı gibi.
  - Sürücü, sürücü ve disk boyutu türü gibi depolama öznitelikleri.

- *Telemetri işlevselliği*, karşıya yükleme zamanı yüzdesini karşıya yüklenen olayları, bırakılan olayları ve en son verileri de dahil olmak üzere.
- *Kalite ilgili bilgiler* Microsoft Azure yığın nasıl gerçekleştirmekte'nın temel bir anlayış geliştirmenize yardımcı olur. Örneğin, belirli donanım yapılandırmasını kritik uyarı sayısı.
- *Uyumluluk verileri* yardımcı bilinmesini hakkında kaynak sağlayıcıları yüklenen bir sistem ve bir sanal makine sağlayın. Bu, olası uyumluluk sorunlarına tanımlar.

**2 (Gelişmiş)**</br>
Dahil olmak üzere ek Öngörüler: işletim sistemi ve Azure yığın hizmetlerinin nasıl kullanıldığı, bu hizmetleri nasıl gerçekleştirmek, Gelişmiş güvenilirlik verileri ve verileri **güvenlik** ve **temel** düzeyleri.

> [!NOTE]
> Varsayılan telemetri ayar budur.

**3 (tam)**</br>
Tüm verileri tanımlamak ve sorunları gidermeye yardımcı olmak gerekli ek verileri **güvenlik**, **temel**, ve **geliştirilmiş** düzeyleri.

> [!IMPORTANT]
> Bu telemetri düzeyleri yalnızca Microsoft Azure yığın bileşenleri için geçerlidir. Microsoft olmayan yazılım bileşenleri ve Azure yığın donanım ortaklardan donanım yaşam döngüsü konakta çalışan hizmetleri bu telemetri düzeyleri dışında bulut Hizmetleri ile iletişim kurabilir. Telemetri ilkelerini ve nasıl kabul veya geri çevirmek anlamak için Azure yığın donanım çözüm sağlayıcınız ile çalışması gerekir.

Windows ve Azure yığın telemetriyi kapatma SQL telemetri de devre dışı bırakır. Windows Server telemetri ayarlarını etkilerini hakkında daha fazla bilgi için bkz: [Windows Telemetri teknik](https://aka.ms/winservtelemetry).

### <a name="asdk-set-the-telemetry-level-in-the-windows-registry"></a>ASDK: Windows Kayıt Defteri'nde telemetri düzeyi

Azure yığın dağıtmadan önce fiziksel ana bilgisayara elle telemetri düzeyini ayarlamak için Windows Kayıt Defteri Düzenleyicisi'ni kullanabilirsiniz. Bir yönetim ilkesi, Grup İlkesi gibi zaten varsa bu kayıt defteri ayarını geçersiz kılar.

Azure yığın Geliştirme Seti konakta dağıtma önce CloudBuilder.vhdx önyükleme ve yükseltilmiş bir PowerShell penceresinde aşağıdaki betiği çalıştırın:

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

### <a name="asdk-and-multi-node-enable-or-disable-telemetry-after-deployment"></a>ASDK ve çok düğümlü: etkinleştirmek veya dağıtımdan sonra telemetri devre dışı bırakma

Etkinleştirmek veya dağıtımdan sonra telemetri devre dışı bırakmak için erişim için ayrıcalıklı uç noktası (ERCS sanal makinelerin kullanıma CESARETLENDİRİCİ) olması gerekir.

1. Etkinleştirmek için: `Set-Telemetry -Enable`
2. Devre dışı bırakmak için: `Set-Telemetry -Disable`

PARAMETRE ayrıntıları:
> . PARAMETRE Enable - Aç telemetri verileri karşıya yükleme</br>
> . PARAMETRE devre dışı bırak - devre dışı bırak telemetri verileri karşıya yükleme  

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

**Telemetri devre dışı bırakmak için komut dosyası:**

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

- [Azure yığın Geliştirme Seti dağıtım paketini karşıdan yükle](https://azure.microsoft.com/overview/azure-stack/try/?v=try)

- [Azure yığın Geliştirme Seti dağıtma](azure-stack-run-powershell-script.md)
