---
title: Güncelleştirme yönetimi sorunlarını giderme
description: Güncelleştirme yönetimi ile ilgili sorunları giderme hakkında bilgi edinin
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 10/17/2018
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: 41883fd677d276f8f26721fdccc3ded020c3278b
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2018
ms.locfileid: "49405240"
---
# <a name="troubleshooting-issues-with-update-management"></a>Güncelleştirme yönetimi ile ilgili sorunları giderme

Bu makalede, güncelleştirme yönetimi kullanırken çalışabilir genelinde sorunları çözmek için çözümleri açıklanmaktadır.

## <a name="general"></a>Genel

### <a name="components-enabled-not-working"></a>Senaryo: 'Güncelleştirme Yönetim' çözümünün bileşenleri etkinleştirildi ve bu sanal makine artık yapılandırılıyor

#### <a name="issue"></a>Sorun

15 dakika sonra ekleme bir sanal makinede aşağıdaki iletiyi görmeye devam:

```
The components for the 'Update Management' solution have been enabled, and now this virtual machine is being configured. Please be patient, as this can sometimes take up to 15 minutes.
```

#### <a name="cause"></a>Nedeni

Bu hata, aşağıdaki nedenlerden kaynaklanabilir:

1. Otomasyon hesabını geri iletişimi engelleniyor.
2. Eklenen VM'de Sysprep kullanılarak hazırlanmış Microsoft izleme aracısının yüklü olmadığı bir kopyalanan makineden gelmiş olabilir.

#### <a name="resolution"></a>Çözüm

1. Ziyaret [ağ planlaması](../automation-hybrid-runbook-worker.md#network-planning) hakkında adresler ve bağlantı noktaları çalışacak şekilde güncelleştirme yönetimi için izin verilmesi gereken öğrenin.
2. Kopyalanan bir görüntü sysprep görüntüsü kullanarak ve olaydan sonra MMA aracısını yükleme durumunda.

## <a name="windows"></a>Windows

Ekleme çözüm bir sanal makine üzerinde çalışırken sorunlarla karşılaşırsanız, kontrol **Operations Manager** altında olay günlüğüne **uygulama ve hizmet günlükleri** olan olaylar için yerel makinede Olay Kimliği **4502** ve içeren olay iletisi **sorun**.

Aşağıdaki bölümde her biri için belirli hata iletileri ve olası çözümü vurgulanmıştır. Diğer ekleme için sorunlara bakın, [çözüm ekleme sorunlarını giderme](onboarding.md).

### <a name="machine-already-registered"></a>Senaryo: Makine için farklı bir hesap zaten kayıtlı

#### <a name="issue"></a>Sorun

Aşağıdaki hata iletisini alıyorsunuz:

```
Unable to Register Machine for Patch Management, Registration Failed with Exception System.InvalidOperationException: {"Message":"Machine is already registered to a different account."}
```

#### <a name="cause"></a>Nedeni

Güncelleştirme yönetimi için başka bir çalışma alanına eklenen makine zaten var.

#### <a name="resolution"></a>Çözüm

Eski yapıtları temizleyin, makine tarafından gerçekleştir [karma runbook grubunu silerek](../automation-hybrid-runbook-worker.md#remove-a-hybrid-worker-group) ve yeniden deneyin.

### <a name="machine-unable-to-communicate"></a>Senaryo: Makine hizmetiyle iletişim kuramıyor

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

Bir ara sunucu, ağ geçidi veya güvenlik duvarı ağ iletişimi engelliyor olabilir.

#### <a name="resolution"></a>Çözüm

Ağınızın gözden geçirin ve uygun bağlantı noktalarını ve adresi izin verildiğinden emin olun. Bkz: [ağ gereksinimleri](../automation-hybrid-runbook-worker.md#network-planning), bağlantı noktaları ve güncelleştirme yönetimi ve karma Runbook çalışanları tarafından gerekli adresleri listesi.

### <a name="unable-to-create-selfsigned-cert"></a>Senaryo: Otomatik olarak imzalanan sertifika oluşturulamadı

#### <a name="issue"></a>Sorun

Aşağıdaki hata iletilerinden birini alabilirsiniz:

```
Unable to Register Machine for Patch Management, Registration Failed with Exception AgentService.HybridRegistration. PowerShell.Certificates.CertificateCreationException: Failed to create a self-signed certificate. ---> System.UnauthorizedAccessException: Access is denied.
```

#### <a name="cause"></a>Nedeni

Karma Runbook çalışanı otomatik olarak imzalanan bir sertifika oluşturmak mümkün değildi

#### <a name="resolution"></a>Çözüm

Sistem hesabı, klasöre okuma erişimi olduğunu doğrulayın **C:\ProgramData\Microsoft\Crypto\RSA** ve yeniden deneyin.

### <a name="hresult"></a>Senaryo: Makine, değerlendirilmeyen ve bir HResult özel durum gösterir şekilde gösterir.

#### <a name="issue"></a>Sorun

Gösterme makineler sahip **değerlendirilmeyen** altında **Uyumluluk**, ve bunun altındaki bir özel durum iletisi görürsünüz.

#### <a name="cause"></a>Nedeni

Windows update makinede doğru yapılandırılmamış.

#### <a name="resolution"></a>Çözüm

Tüm özel durum iletisi görmek için kırmızı renkte gösterilen özel durumu çift tıklayın. Aşağıdaki tabloda olası çözümleri veya gerçekleştirilecek eylemleri gözden geçirin:

|Özel durum  |Çözüm veya eylem  |
|---------|---------|
|`Exception from HRESULT: 0x……C`     | İlgili hata kodunu arama [Windows güncelleştirme hatası kodu listesi](https://support.microsoft.com/help/938205/windows-update-error-code-list) özel durumun nedeni hakkında ek ayrıntıları bulmak için.        |
|`0x8024402C` veya `0x8024401C`     | Bu hatalar, ağ bağlantısı sorunları var. Makinenizde güncelleştirme yönetimi için uygun ağ bağlantısı olduğundan emin olun. Bölümüne [ağ planlaması](../automation-update-management.md#ports) bağlantı noktaları ve gerekli olan adresleri listesi.        |
|`0x8024402C`     | Bir WSUS sunucusu kullanıyorsanız, kayıt defteri değerlerini emin `WUServer` ve `WUStatusServer` kayıt defteri anahtarı altında `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate` doğru WSUS sunucusu vardır.        |
|`The service cannot be started, either because it is disabled or because it has no enabled devices associated with it. (Exception from HRESULT: 0x80070422)`     | Windows Güncelleştirme Hizmeti (wuauserv) çalıştığından ve devre dışı emin olun.        |
|Herhangi bir genel durum     | Bir arama olası çözümler için internet ve yerel BT desteği ile çalışır.         |

## <a name="linux"></a>Linux

### <a name="scenario-update-run-fails-to-start"></a>Senaryo: başlatmak güncelleştirme çalışması başarısız.

#### <a name="issue"></a>Sorun

Bir Linux makinesinde başlatmak için bir güncelleştirme çalıştırmaları başarısız.

#### <a name="cause"></a>Nedeni

Linux karma çalışanı sağlam değil.

#### <a name="resolution"></a>Çözüm

Aşağıdaki günlük dosyası bir kopyasını alın ve sorun giderme amacıyla Koru:

```
/var/opt/microsoft/omsagent/run/automationworker/worker.log
```

### <a name="scenario-update-run-starts-but-encounters-errors"></a>Senaryo: güncelleştirme çalışması başlar ancak hatalarla karşılaştığında

#### <a name="issue"></a>Sorun

Bir güncelleştirme çalışması başlatmaz, bunun çalıştırma sırasında hatalarla karşılaştığında.

#### <a name="cause"></a>Nedeni

Olası nedenler aşağıdakilerden biri olabilir:

* Paket Yöneticisi sağlam değil
* Bulut tabanlı düzeltme eki uygulama ile belirli paketleri etkileyebilir
* Diğer nedenler

#### <a name="resolution"></a>Çözüm

Linux üzerinde sorunsuz başlatıldıktan sonra Çalıştır güncelleştirilmesi sırasında hatalar olması durumunda, etkilenen makinenin çalıştırmada çıktı iş kontrol edin. Araştırma ve üzerinde işlem gerçekleştir makinenizin Paket Yöneticisi'nden belirli hata iletileri bulabilirsiniz. Güncelleştirme yönetimi, Paket Yöneticisi başarılı güncelleştirme dağıtımları için iyi durumda olmasını gerektirir.

Bazı durumlarda, güncelleştirme bir güncelleştirme dağıtımı tamamlanmasını engelliyor yönetimi ile paket güncelleştirmelerini engelleyebilir. Gördüğünüz, gelecek güncelleştirmelerden çalıştırmalardan bu paketleri Dışla veya bunları el ile yüklemeniz gerekir kendiniz.

Bir düzeltme eki uygulama sorunu çözemezseniz, aşağıdaki günlük dosyası bir kopyasını oluşturun ve bunu korumak **önce** sorun giderme amacıyla sonraki güncelleştirme dağıtımına başlatır:

```
/var/opt/microsoft/omsagent/run/automationworker/omsupdatemgmt.log
```

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediniz veya sorununuzu çözmenize yüklenemiyor, daha fazla destek için aşağıdaki kanalları birini ziyaret edin:

* [Azure Forumları](https://azure.microsoft.com/support/forums/) aracılığıyla Azure uzmanlarından yanıtlar alın
* [@AzureSupport](https://twitter.com/azuresupport) hesabı ile bağlantı kurun. Bu resmi Microsoft Azure hesabı, müşteri deneyimini geliştirmek için Azure topluluğunu doğru kaynaklara ulaştırır: yanıtlar, destek ve uzmanlar.
* Daha fazla yardıma ihtiyacınız varsa, bir Azure destek olayına dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) seçip **alma desteği**.