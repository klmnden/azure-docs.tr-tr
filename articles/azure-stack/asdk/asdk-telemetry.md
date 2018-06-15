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
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/17/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: bfd16901c5ce036719a1ed19e9a5b5c6ef52be93
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
ms.locfileid: "34257433"
---
# <a name="azure-stack-telemetry"></a>Azure yığın telemetri

Azure yığın sistem verileri veya telemetri, bağlı kullanıcı deneyimi aracılığıyla Microsoft'a otomatik olarak yüklenir. Azure yığın telemetrisinden toplanan veriler öncelikle bizim müşteri deneyimleri geliştirmek için Microsoft ekipleri tarafından ve güvenlik, sistem durumu, kalite ve performans analizi için kullanılır.

Bir Azure yığın işleç olarak telemetri Kurumsal dağıtımlar değerli Öngörüler sağlayabilir ve Azure yığın gelecek sürümlerinde şekli yardımcı olan bir sesli verir.

> [!NOTE]
> Azure yığını, aynı zamanda için fatura Azure iletme kullanım bilgileri için yapılandırılabilir. Bu ödeme olarak,-kullanımlı faturalama seçen çok düğümlü Azure yığın müşteriler için gereklidir. Kullanım raporlama telemetrisinden bağımsız olarak denetlenir ve kapasite modelini seçin çok düğümlü müşteriler veya Azure yığın Geliştirme Seti kullanıcılar gerekli değildir. Bu senaryolarda, kullanım raporlama kapatılabilir [kayıt komut dosyası kullanarak](https://docs.microsoft.com/azure/azure-stack/azure-stack-usage-reporting).

Azure yığın telemetri kullanan Windows Server 2016 bağlı kullanıcı deneyimi ve Telemetri bileşeni, temel [olay izleme için Windows (ETW)](https://msdn.microsoft.com/library/dn904632(v=vs.85).aspx) izleme toplamak ve telemetri olayları ve verilerini depolamak için günlüğe kaydetme teknolojisi. Azure yığın bileşenleri olayları ve ortak işletim sistemi olay günlüğü ve izleme API'leri kullanılarak toplanan verileri yayımlamak için aynı günlük teknolojisini kullanır. Ağ kaynak sağlayıcısı, depolama kaynak sağlayıcısı, kaynak sağlayıcısı izleme ve güncelleştirme kaynak sağlayıcısı Azure yığın bileşenleri örneklerindendir. Bağlı kullanıcı deneyimi ve Telemetri bileşeni, SSL kullanarak verileri şifreler ve telemetri verilerini HTTPS üzerinden Microsoft Veri Yönetimi hizmetine iletmek için sertifika sabitleme kullanır.

> [!NOTE]
> Telemetri veri akışı desteklemek için bağlantı noktası 443 (HTTPS), ağınızdaki açık olması gerekir. Microsoft Veri Yönetimi Hizmeti bağlı kullanıcı deneyimi ve Telemetri bileşeni bağlandığı https://v10.vortex-win.data.microsoft.com. Bağlı kullanıcı deneyimi ve Telemetri bileşeni de bağlanır https://settings-win.data.microsoft.com yapılandırma bilgilerini karşıdan yüklemek için.

## <a name="privacy-considerations"></a>Gizlilik konuları
ETW hizmeti geri korunan bulut depolama alanına telemetri verilerini yönlendirir. Telemetri verileri az ayrıcalıklı kılavuzları erişim ilkesi. Yalnızca Microsoft personeli geçerli iş gereksinimlerine telemetri verilere erişmesine izin verilir. Microsoft olmayan kişisel verileri paylaşmak müşterilerimizin üçüncü taraflarla dışında Müşteri'nin tedbirli veya açıklanan sınırlı amacıyla [Azure yığın gizlilik bildirimi](https://privacy.microsoft.com/PrivacyStatement). İş raporları OEM'ler ve toplanan, anonim telemetri bilgileri içerecek iş ortakları ile paylaşır. Veri kararları paylaşımı gizlilik, yasal ve veri yönetimi Paydaşlar dahil olmak üzere bir iç Microsoft ekibi tarafından yapılır.

Microsoft, içinde bulunduğu ve bilgi minimization yöntemler. Yalnızca ihtiyacımız ve onu yalnızca sürece bir hizmet sağlamak için gerekli olan gibi veya Analiz depolarız bilgileri toplamak çalışmalarımızı. Azure yığın sistem ve Azure hizmetleri nasıl çalışır durumda olduğundan hakkında bilgilerin çoğunu altı ay içinde silinir. Özetlenen veya toplanan verileri daha uzun bir süre boyunca tutulur.

Gizlilik ve güvenlik müşterilerimizin bilgilerinin olduğunu önemli anlayın. Müşteri gizliliği ve müşteri verilerini Azure yığını ile koruma biz Düşünceli ve kapsamlı bir yaklaşım gerçekleştirmişsiniz. BT yöneticileri herhangi bir zamanda özellikleri ve gizlilik ayarlarını özelleştirmek için denetimler sahiptir. Saydamlık ve güven müşterilerimize taahhütler temizleyin:
- Biz, biz toplamak veri türleri hakkında müşterilerle açıktır.
- Biz Kurumsal müşteriler denetiminde put — kendi gizlilik ayarlarını özelleştirebilirsiniz.
- Biz müşteri gizlilik ve güvenlik ilk yerleştirin.
- Telemetri kullanılma hakkında saydam duyuyoruz.
- Telemetri müşteri deneyimleri geliştirmek için kullanırız.

Microsoft, kredi kartı numaraları, kullanıcı adları ve parolalar, e-posta adresleri veya diğer benzer şekilde hassas bilgiler gibi hassas bilgileri toplamak istememektedir. Hassas bilgilerin yanlışlıkla alındı karar verirseniz, sileriz.

## <a name="examples-of-how-microsoft-uses-the-telemetry-data"></a>Microsoft telemetri verilerini nasıl kullandığı örnekleri
Telemetri bize hızla tanımlayın ve müşterilerimizin dağıtımları ve yapılandırmalara kritik güvenilirlik sorunlarını gidermeye yardımcı olacak önemli bir rol oynar. Biz toplamak telemetri verilerini Öngörüler Hizmetleri veya donanım yapılandırmaları ile sorunları hızla belirlemek yardımcı olur. Microsoft'un müşterileri ve ekosistemi sürücü artışlarını bu veri alma becerisini bizim tümleşik Azure yığın çözümleri kalitesini çubuğu Yükselt yardımcı olur.

Telemetri daha iyi müşteriler bileşenleri nasıl dağıtıldığına anlamak, özelliklerini kullanmak ve kendi iş hedeflerinize ulaşmak için hizmetleri kullanmak için Microsoft da yardımcı olur. Bu verilerden Öngörüler alma yardımcı müşterilerimizin deneyimleri ve iş yükleri doğrudan etkileyebilir alanlarda mühendislik Yatırımlar öncelik verin.

Bazı örnekler müşteri kullanımını kapsayıcıları, depolama ve Azure yığın rolleriyle ilişkili olan ağ yapılandırmaları şunlardır. Ayrıca sürücü geliştirmeleri ve yönetimi bilgileri içine bazı bizim yönetimi ve çözümlerini izleme ınsights'a kullanırız. Bu, müşterilerin Kalite sorunlarını tanılamanıza yardımcı olur ve daha az destek yaparak tasarruf Microsoft'a çağırır.

## <a name="manage-telemetry-collection"></a>Telemetri koleksiyonunu yönetme
Geliştirilmiş ürün işlevselliği ve kararlılık sürücüleri veri telemetri sağlayan gibi kuruluşunuzdaki telemetriyi açın önermiyoruz. Biz ancak, bazı senaryolarda bu gerekebileceğini aklınızda farkındayız.

Bu durumlarda, kayıt defteri ayarları dağıtım öncesi veya Telemetri uç noktaları post dağıtım kullanarak tarafından Microsoft'a gönderilen telemetri düzeyini yapılandırabilirsiniz.

### <a name="set-telemetry-level-in-the-windows-registry"></a>Windows Kayıt Defteri'nde telemetri düzeyini ayarlayın
Windows Kayıt Defteri Düzenleyicisi'ni el ile Azure yığın dağıtmadan önce fiziksel ana bilgisayarda telemetri düzeyini ayarlamak için kullanılır. Bir yönetim ilkesi, Grup İlkesi gibi zaten varsa bu kayıt defteri ayarını geçersiz kılar.

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

Toplu ve kategorilere ayrılmış (0-3) dört düzeylere telemetri düzeyleri şunlardır:

**0 (güvenlik)**. Yalnızca güvenlik verileri. İşletim sistemi tutmaya yardımcı olmak için gerekli olan bilgileri güvenli, bağlı kullanıcı deneyimi ve Telemetri bileşen ayarları, Windows Defender hakkında verileri de dahil olmak üzere. Azure yığın belirli telemetri yok, bu düzeyde yayınlanır.

**1 (Temel)**. Güvenlik veri ve temel sağlık ve kalite veri. Temel aygıt bilgileri de dahil olmak üzere: kalitesi ile ilgili veriler, uygulama uyumluluğu, uygulama kullanımı verilerini ve güvenlik düzeyini veriler. Telemetri düzeyinizi temel etkinleştirir Azure yığın telemetri için ayarlama. Bu düzeyde toplanan verilerin içerir:

- **Temel aygıt bilgilerini** yardımcı bilinmesini türleri ve yerel ve sanallaştırılmış Windows Server 2016 örneklerinin ekosistemindeki yapılandırmaları hakkında sağlamak da dahil olmak üzere:
 - OEM gibi makine öznitelikleri modeli
 - Ağ iletişimi sayısını ve hızını ağ bağdaştırıcıları gibi öznitelikleri
 - İşlemci ve bellek öznitelikleri, çekirdek sayısı gibi bellek boyutu
 - Sürücüleri, türü ve boyutunu sayısı gibi depolama öznitelikleri.
- **Telemetri işlevselliği**dahil yüzde karşıya yüklenen olayları, bırakılan olayları ve son zaman karşıya yükleyin.
- **Kalite ilgili bilgiler** Microsoft Azure yığın nasıl gerçekleştirmekte'nın temel bir anlayış geliştirmenize yardımcı olur. Bir örnek belirli donanım yapılandırmasını kritik uyarılar sayısıdır.
- ** Uyumluluk verileri yardımcı olduğu hakkında kaynak sağlayıcıları sistemi ve sanal makine üzerinde yüklü ve olası uyumluluk sorunlarına tanımlayan bir anlama sağlayın.

**(Gelişmiş) 2**. Dahil olmak üzere ek Öngörüler: işletim sistemi ve diğer Azure yığın hizmetlerinin nasıl kullanıldığı, nasıl gerçekleştirdikleri, güvenilirliğin veri ve temel ve güvenlik düzeyleri veriler.

**3 (tam)**. Tüm verileri tanımlamak ve sorunları gidermeye yardımcı olmak gerekli ek verileri **güvenlik**, **temel**, ve **geliştirilmiş** düzeyleri.

> [!NOTE]
> Varsayılan telemetri düzeyi değeri (Gelişmiş) 2'dir.

Windows ve Azure yığın telemetriyi kapatma SQL telemetri devre dışı bırakır. Windows Server telemetri ayarlarını etkilerini hakkında ek bilgi için başvuru [Windows Telemetri teknik](https://aka.ms/winservtelemetry).

> [!IMPORTANT]
> Bu telemetri düzeyleri yalnızca Microsoft Azure yığın bileşenleri için geçerlidir. Microsoft olmayan yazılım bileşenleri ve Azure yığın donanım ortaklardan donanım yaşam döngüsü konakta çalışan hizmetleri bu telemetri düzeyleri dışında bulut Hizmetleri ile iletişim kurabilir. Telemetri ilkelerini ve nasıl kabul veya geri çevirmek anlamak için Azure yığın donanım çözüm sağlayıcınız ile çalışması gerekir.

### <a name="enable-or-disable-telemetry-after-deployment"></a>Etkinleştirmek veya dağıtımdan sonra telemetri devre dışı bırakma

Etkinleştirmek veya dağıtımdan sonra telemetri devre dışı bırakmak için erişim için ayrıcalıklı uç noktası (ERCS sanal makinelerin kullanıma CESARETLENDİRİCİ) olması gerekir.
1.  Etkinleştirmek için: `Set-Telemetry -Enable`
2.  Devre dışı bırakmak için: `Set-Telemetry -Disable`

PARAMETRE Ayrıntısı:
> . PARAMETRE Enable - Aç telemetri verileri karşıya yükleme

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
[Market öğesi ekleme](asdk-marketplace-item.md)
