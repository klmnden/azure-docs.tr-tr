---
title: Güncelleştirme yönetimi hatalarıyla ilgili sorunları giderme
description: Güncelleştirme yönetimi ile ilgili sorunları giderme hakkında bilgi edinin
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 06/19/2018
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: b77d1210ff48a4bd30834fcbad64173bf77b1290
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37064101"
---
# <a name="troubleshooting-issues-with-update-management"></a>Güncelleştirme yönetimi ile ilgili sorunları giderme

Bu makalede, güncelleştirme yönetimi kullanırken karşılaşabileceğiniz sorunları gidermek için çözümleri anlatılmaktadır.

## <a name="windows"></a>Windows

Giriş çözüm bir sanal makine üzerinde çalışırken sorunlarla karşılaşırsanız, denetleme **Operations Manager** altındaki olay günlüğü **uygulama ve hizmet günlükleri** olan olaylar için yerel makinede Olay Kimliği **4502** ve olay iletisi içeren **Microsoft.EnterpriseManagement.HealthService.AzureAutomation.HybridAgent**.

Aşağıdaki bölüm, özel hata iletileri ve olası bir çözüm her biri için vurgular. Diğer hazırlanması için sorunları bakın, [çözüm ekleme sorun giderme](onboarding.md).

### <a name="machine-already-registered"></a>Senaryo: Makine için farklı bir hesap zaten kayıtlı

#### <a name="issue"></a>Sorun

Aşağıdaki hata iletisini alıyorsunuz:

```error
Unable to Register Machine for Patch Management, Registration Failed with Exception System.InvalidOperationException: {"Message":"Machine is already registered to a different account."}
```

#### <a name="cause"></a>Nedeni

Güncelleştirme yönetimi için başka bir çalışma sayede makine zaten var.

#### <a name="resolution"></a>Çözüm

Eski yapılarının tarafından makinede temizlenmesini [karma runbook grubu silme](../automation-hybrid-runbook-worker.md#remove-a-hybrid-worker-group) ve yeniden deneyin.

### <a name="machine-unable-to-communicate"></a>Senaryo: Hizmetiyle iletişim kurmak makine alamıyor

#### <a name="issue"></a>Sorun

Aşağıdaki hata iletilerinden birini alabilirsiniz:

```
Unable to Register Machine for Patch Management, Registration Failed with Exception System.Net.Http.HttpRequestException: An error occurred while sending the request. ---> System.Net.WebException: The underlying connection was closed: An unexpected error occurred on a receive. ---> System.ComponentModel.Win32Exception: The client and server can't communicate, because they do not possess a common algorithm
```

```
Unable to Register Machine for Patch Management, Registration Failed with Exception Newtonsoft.Json.JsonReaderException: Error parsing positive infinity value.
```

```
The certificate presented by the service <wsid>.oms.opinsights.azure.com was not issued by a certificate authority used for Microsoft services. Contact your network administrator to see if they are running a proxy that intercepts TLS/SSL communication.
```

#### <a name="cause"></a>Nedeni

Bir proxy, ağ geçidi veya Güvenlik Duvarı'nı ağ iletişimi engelliyor olabilir.

#### <a name="resolution"></a>Çözüm

Ağ gözden geçirin ve uygun bağlantı noktalarının ve adresleri izin verildiğinden emin olun. Bkz: [ağ gereksinimleri](../automation-hybrid-runbook-worker.md#network-planning), bağlantı noktaları ve güncelleştirme yönetimi ve karma Runbook çalışanları tarafından gerekli adresleri listesi.

### <a name="unable-to-create-selfsigned-cert"></a>Senaryo: Otomatik olarak imzalanan sertifika oluşturulamadı

#### <a name="issue"></a>Sorun

Aşağıdaki hata iletilerinden birini alabilirsiniz:

```
Unable to Register Machine for Patch Management, Registration Failed with Exception AgentService.HybridRegistration. PowerShell.Certificates.CertificateCreationException: Failed to create a self-signed certificate. ---> System.UnauthorizedAccessException: Access is denied.
```

#### <a name="cause"></a>Nedeni

Karma Runbook çalışanı otomatik olarak imzalanan sertifika oluşturmak mümkün değildi

#### <a name="resolution"></a>Çözüm

Sistem hesabı klasöre okuma erişimi olduğunu doğrulayın **C:\ProgramData\Microsoft\Crypto\RSA** ve yeniden deneyin.

## <a name="linux"></a>Linux

### <a name="scenario-update-run-fails-to-start"></a>Senaryo: başlatmak güncelleştirme çalışması başarısız

#### <a name="issue"></a>Sorun

Bir Linux makinesinde başlatmak için bir güncelleştirme çalışmaları başarısız.

#### <a name="cause"></a>Nedeni

Linux karma çalışanı sağlam değil.

#### <a name="resolution"></a>Çözüm

Aşağıdaki günlük dosyası bir kopyasını alın ve sorun giderme amacıyla Koru:

```
/var/opt/microsoft/omsagent/run/automationworker/worker.log
```

### <a name="scenario-update-run-starts-but-encounters-errors"></a>Senaryo: güncelleştirme çalışması başlatır, ancak hatayla karşılaştığında

#### <a name="issue"></a>Sorun

Güncelleştirme çalışması başlatır, ancak çalışma sırasında hatayla karşılaştığında.

#### <a name="cause"></a>Nedeni

Olası nedenleri şunlardan biri olabilir:

* Paket Yöneticisi sağlam değil
* Bulut tabanlı düzeltme eki uygulama ile belirli paketleri etkileyebilir
* Diğer nedenler

#### <a name="resolution"></a>Çözüm

Linux üzerinde başarıyla başlatıldıktan sonra Çalıştır'ün güncelleştirilmesi sırasında hatalar meydana gelirse, etkilenen makinenin çalıştırmada çıktısı işini denetleyin. Araştırma ve eylem gerçekleştiren makinenizin Paket Yöneticisi'nden özel hata iletileri bulabilirsiniz. Güncelleştirme yönetimi, Paket Yöneticisi'ni başarılı güncelleştirme dağıtımları için sağlıklı olmasını gerektirir.

Bazı durumlarda, güncelleştirme bir güncelleştirme dağıtımı tamamlanmasını engelleyen yönetimiyle paket güncelleştirmesi etkileyebilir. Görürseniz, bu paketleri gelecekteki güncelleştirme dosyadan hariç veya bunları el ile yüklemeniz gerekir kendiniz.

Bir düzeltme eki uygulama sorunu çözemezseniz, aşağıdaki günlük dosyasının bir kopyası ve onu korumak **önce** sorun giderme amacıyla sonraki güncelleştirme dağıtımı başlatır:

```
/var/opt/microsoft/omsagent/run/automationworker/omsupdatemgmt.log
```

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmemeniz veya sorunu çözmek oluşturulamıyor, daha fazla destek için aşağıdaki kanallar birini ziyaret edin:

* [Azure Forumları](https://azure.microsoft.com/support/forums/) aracılığıyla Azure uzmanlarından yanıtlar alın
* [@AzureSupport](https://twitter.com/azuresupport) hesabı ile bağlantı kurun. Bu resmi Microsoft Azure hesabı, müşteri deneyimini geliştirmek için Azure topluluğunu doğru kaynaklara ulaştırır: yanıtlar, destek ve uzmanlar.
* Daha fazla yardıma gereksinim duyarsanız, Azure destek olay dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) seçip **alma desteği**.