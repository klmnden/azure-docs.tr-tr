---
title: Azure Stack syslog iletme
description: Azure Stack syslog iletme kullanarak izleme çözümleri ile tümleştirmeyi öğrenin
services: azure-stack
author: PatAltimore
manager: femila
ms.service: azure-stack
ms.topic: article
ms.date: 01/28/2019
ms.author: patricka
ms.reviewer: fiseraci
ms.lastreviewed: 01/28/2019
keywords: ''
ms.openlocfilehash: 3694425ac72d3b75d66d870e3746bc1738ba0138
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58481918"
---
# <a name="azure-stack-datacenter-integration---syslog-forwarding"></a>Azure Stack veri merkezi tümleştirmesi - syslog iletme

Bu makalede, Azure Stack altyapısının veri merkezinizde zaten dağıtılmış bir dış güvenlik çözümleri ile tümleştirmek için syslog kullanmayı gösterir. Örneğin, bir güvenlik bilgileri olay Yönetimi (SIEM) sistemine. Denetimler, uyarılar ve Azure Stack altyapısının tüm bileşenlerin güvenlik günlükleri syslog kanalı sunar. Güvenlik İzleme çözümleri ile tümleştirmek için Syslog iletmeyi kullanın ve/veya tüm denetimler, uyarılar ve güvenlik almak için bekletme için depolamaya kaydeder.

1809 güncelleştirmesinden itibaren Azure Stack, yapılandırıldıktan sonra Yük Common Event Format (CEF), syslog iletileri yayar bir tümleşik syslog istemcisi vardır.

Aşağıdaki diyagram, Azure Stack dış bir SIEM ile tümleştirme açıklar. Ele alınması gereken iki tümleştirme desenleri vardır: ilk (bir mavi) bir altyapı sanal makineler ve Hyper-V düğümlerinin kapsayan Azure Stack altyapısıdır. Tüm denetimleri, güvenlik günlükleri ve uyarılar Bu bileşenlerden merkezi olarak toplanan ve syslog CEF yüküyle aracılığıyla kullanıma sunulan. Bu tümleştirme deseni, bu belge sayfasında açıklanmıştır.
İkinci tümleştirme deseni turuncu renkte gösterilen olur ve temel kart yönetim denetleyicileri (Bmc'ler), donanım yaşam döngüsü ana bilgisayar (HLH), sanal makineler ve/veya izleme donanım iş ortağı çalıştırılan sanal Gereçleri ve Yönetimi kapsar yazılım ve üst raf üstü (TOR) anahtarlar. Bu bileşenler donanım iş ortağı belirli, kişi olduğundan, donanım ortak bunları dış bir SIEM ile tümleştirme hakkında bilgi için.

![Syslog iletmeyi diyagramı](media/azure-stack-integrate-security/syslog-forwarding.png)

## <a name="configuring-syslog-forwarding"></a>Syslog iletmeyi yapılandırma

Azure stack'teki syslog istemci aşağıdaki yapılandırmaları destekler:

1. **Syslog ile karşılıklı kimlik doğrulaması (istemci ve sunucu) ve TLS 1.2 şifrelemeyi, TCP üzerinden:** Bu yapılandırmada, syslog sunucusuna hem syslog istemci sertifikaları aracılığıyla birbirine kimliğini doğrulayabilirsiniz. İletileri, TLS 1.2 şifrelenmiş bir kanal üzerinden gönderilir.

2. **Syslog sunucu kimlik doğrulaması ve TLS 1.2 şifrelemeyi ile TCP üzerinden:** Bu yapılandırmada, syslog istemci sertifika aracılığıyla syslog sunucusunun kimliğini doğrulayabilirsiniz. İletileri, TLS 1.2 şifrelenmiş bir kanal üzerinden gönderilir.

3. **Syslog üzerinden TCP, şifreleme ile:** Bu yapılandırmada, syslog istemci ve syslog sunucusu kimlikleri doğrulanmaz. İletileri, TCP üzerinden açık metin olarak gönderilir.

4. **Syslog şifreleme ile UDP üzerinden:** Bu yapılandırmada, syslog istemci ve syslog sunucusu kimlikleri doğrulanmaz. İletileri UDP üzerinden açık metin olarak gönderilir.

> [!IMPORTANT]
> Microsoft kimlik doğrulama ve şifreleme kullanarak TCP kullanmak için kesinlikle önerir (yapılandırma #1 veya çok az # 2) iletilerin gizlice ve ADAM-de-adam saldırılarına karşı korumak, üretim ortamları için.

### <a name="cmdlets-to-configure-syslog-forwarding"></a>Syslog iletmeyi yapılandırma cmdlet'leri
Syslog iletmeyi yapılandırma uç noktası (CESARETLENDİRİCİ) ayrıcalıklı erişim gerektirir. Syslog iletmeyi yapılandırma CESARETLENDİRİCİ iki PowerShell cmdlet'leri eklenmiştir:


```powershell
### cmdlet to pass the syslog server information to the client and to configure the transport protocol, the encryption and the authentication between the client and the server

Set-SyslogServer [-ServerName <String>] [-ServerPort <String>] [-NoEncryption] [-SkipCertificateCheck] [-SkipCNCheck] [-UseUDP] [-Remove]

### cmdlet to configure the certificate for the syslog client to authenticate with the server

Set-SyslogClient [-pfxBinary <Byte[]>] [-CertPassword <SecureString>] [-RemoveCertificate] 
```
#### <a name="cmdlets-parameters"></a>Cmdlet parametreleri

Parametreler için *kümesi SyslogServer* cmdlet:

| Parametre | Açıklama | Type | Gerekli |
|---------|---------|---------|---------|
|*SunucuAdı* | Syslog sunucusunun FQDN veya IP adresi | String | evet|
|*ServerPort* | Bağlantı noktası numarası syslog sunucusunun dinleme yaptığı | String | evet|
|*Şifreleme yok*| Düz metin olarak Syslog iletilerini göndermek için istemci zorla | Bayrağı | hayır|
|*SkipCertificateCheck*| İlk TLS anlaşması sırasında syslog sunucusu tarafından sağlanan sertifika doğrulamasını atla | Bayrağı | hayır|
|*SkipCNCheck*| Ortak ad değeri ilk TLS anlaşması sırasında syslog sunucusu tarafından sağlanan sertifika doğrulamasını atlayın | Bayrağı | hayır|
|*UseUDP*| Syslog ile UDP taşıma protokol olarak kullanın. |Bayrağı | hayır|
|*Kaldır*| İstemciden sunucusunun yapılandırmasını kaldırın ve syslog iletmeyi Durdur| Bayrağı | hayır|

Parametreler için *kümesi SyslogClient* cmdlet:

| Parametre | Açıklama | Type |
|---------|---------| ---------|
| *pfxBinary* | İstemci tarafından kimliği olarak syslog sunucusuna göre kimlik doğrulaması için kullanılacak sertifikayı içeren pfx dosyasını  | Byte[] |
| *CertPassword* |  Pfx dosyası ile ilişkili özel anahtarı içeri aktarmak için parola | SecureString |
|*RemoveCertificate* | İstemciden sertifikayı Kaldır | Bayrağı|

### <a name="configuring-syslog-forwarding-with-tcp-mutual-authentication-and-tls-12-encryption"></a>TCP, karşılıklı kimlik doğrulaması ve TLS 1.2 şifrelemeyi Syslog iletmeyi yapılandırma

Bu yapılandırmada, Azure Stack syslog istemcisinde syslog sunucunuza iletileri TCP üzerinden TLS 1.2 şifrelemeyle iletir. İlk anlaşması sırasında istemci, sunucu geçerli ve güvenilen bir sertifika sağlar doğrular; benzer şekilde, istemci kimlik kanıtı da sunucuya bir sertifika sağlar. Bu en güvenli şekilde tam bir istemci ve sunucu kimliğini doğrulamasını sağlar ve şifreli bir kanal iletiler gönderen bir yapılandırmadır. 

> [!IMPORTANT]
> Microsoft, bu yapılandırmayı üretim ortamları için kullanmak için kesinlikle önerir. 

TCP, karşılıklı kimlik doğrulaması ve TLS 1.2 şifrelemeyi Syslog iletmeyi yapılandırma için CESARETLENDİRİCİ oturum hem de bu cmdlet'leri çalıştırın:

```powershell
# Configure the server
Set-SyslogServer -ServerName <FQDN or ip address of syslog server> -ServerPort <Port number on which the syslog server is listening on>

# Provide certificate to the client to authenticate against the server
Set-SyslogClient -pfxBinary <Byte[] of pfx file> -CertPassword <SecureString, password for accessing the pfx file>
```

İstemci sertifikası aynı kök Azure Stack dağıtımı sırasında sağlanan fazla olması gerekir. Ayrıca özel anahtar içermesi gerekir.

```powershell
##Example on how to set your syslog client with the certificate for mutual authentication.
##This example script must be run from your hardware lifecycle host or privileged access workstation.

$ErcsNodeName = "<yourPEP>"
$password = ConvertTo-SecureString -String "<your cloudAdmin account password" -AsPlainText -Force
 
$cloudAdmin = "<your cloudAdmin account name>"
$CloudAdminCred = New-Object System.Management.Automation.PSCredential ($cloudAdmin, $password)
 
$certPassword = $password
$certContent = Get-Content -Path C:\cert\<yourClientCertificate>.pfx -Encoding Byte
 
$params = @{ 
    ComputerName = $ErcsNodeName 
    Credential = $CloudAdminCred 
    ConfigurationName = "PrivilegedEndpoint" 
}

$session = New-PSSession @params
 
$params = @{ 
    Session = $session 
    ArgumentList = @($certContent, $certPassword) 
}
Write-Verbose "Invoking cmdlet to set syslog client certificate..." -Verbose 
Invoke-Command @params -ScriptBlock { 
    param($CertContent, $CertPassword) 
    Set-SyslogClient -PfxBinary $CertContent -CertPassword $CertPassword }
```

### <a name="configuring-syslog-forwarding-with-tcp-server-authentication-and-tls-12-encryption"></a>Syslog iletmeyi yapılandırma TCP ile sunucu kimlik doğrulaması ve TLS 1.2 şifrelemeyi

Bu yapılandırmada, Azure Stack syslog istemcisinde syslog sunucunuza iletileri TCP üzerinden TLS 1.2 şifrelemeyle iletir. İlk anlaşması sırasında istemci, sunucu geçerli ve güvenilen bir sertifika sağlar de doğrular. Bu yapılandırma, güvenilmeyen hedeflere ileti göndermek için istemci engeller.
Kimlik doğrulama ve şifreleme kullanarak TCP varsayılan yapılandırma ve bir üretim ortamı için Microsoft'un önerdiği güvenlik en düşük düzeyi temsil eder. 

```powershell
Set-SyslogServer -ServerName <FQDN or ip address of syslog server> -ServerPort <Port number on which the syslog server is listening on>
```

Syslog sunucunuzun Azure Stack istemci ile tümleştirme, otomatik olarak imzalanan ve/veya güvenilmeyen bir sertifika kullanarak test etmek istediğiniz durumda ilk anlaşması sırasında istemci tarafından gerçekleştirilen sunucu doğrulamasını atlamak için bu bayraklar kullanabilirsiniz.

```powershell
 #Skip validation of the Common Name value in the server certificate. Use this flag if you provide an IP address for your syslog server
 Set-SyslogServer -ServerName <FQDN or ip address of syslog server> -ServerPort <Port number on which the syslog server is listening on>
 -SkipCNCheck

 #Skip entirely the server certificate validation
 Set-SyslogServer -ServerName <FQDN or ip address of syslog server> -ServerPort <Port number on which the syslog server is listening on>
 -SkipCertificateCheck
```

> [!IMPORTANT]
> Microsoft, üretim ortamları için - SkipCertificateCheck bayrağı kullanımına karşı önerir. 

### <a name="configuring-syslog-forwarding-with-tcp-and-no-encryption"></a>TCP ve şifreleme ile Syslog iletmeyi yapılandırma

Bu yapılandırmada, Azure Stack syslog istemcisinde şifreleme ile TCP üzerinden iletileri syslog sunucunuza iletir. İstemci, sunucunun kimliğini doğrulamaz ve bu sunucuya kendi kimlik doğrulama için sağlamaz. 

```powershell
Set-SyslogServer -ServerName <FQDN or ip address of syslog server> -ServerPort <Port number on which the syslog server is listening on> -NoEncryption
```

> [!IMPORTANT]
> Microsoft, bu yapılandırmayı üretim ortamları için kullanılmamasını önerir. 


### <a name="configuring-syslog-forwarding-with-udp-and-no-encryption"></a>UDP ve şifreleme ile Syslog iletmeyi yapılandırma

Bu yapılandırmada, Azure Stack syslog istemcisinde şifreleme ile UDP üzerinden iletileri syslog sunucunuza iletir. İstemci, sunucunun kimliğini doğrulamaz ve bu sunucuya kendi kimlik doğrulama için sağlamaz. 

```powershell
Set-SyslogServer -ServerName <FQDN or ip address of syslog server> -ServerPort <Port number on which the syslog server is listening on> -UseUDP
```

Şifreleme ile UDP yapılandırmak en kolay olsa da, ADAM-de-adam saldırılarına ve iletilerin gizlice karşı bir koruma sağlamaz. 

> [!IMPORTANT]
> Microsoft, bu yapılandırmayı üretim ortamları için kullanılmamasını önerir. 


## <a name="removing-syslog-forwarding-configuration"></a>Syslog iletmeyi yapılandırma kaldırılıyor

Syslog sunucu yapılandırmasını tamamen kaldırın ve syslog iletmeyi durdurmak için:

**Syslog sunucu yapılandırması istemciden Kaldır**

```powershell  
Set-SyslogServer -Remove
```

**İstemci sertifikası istemciyi kaldırın**

```powershell  
Set-SyslogClient -RemoveCertificate
```

## <a name="verifying-the-syslog-setup"></a>Syslog Kurulum doğrulanıyor

Syslog istemci başarıyla syslog sunucunuza bağladıysanız, yakında olayları alma başlamanız gerekir. Herhangi bir olay görmüyorsanız, aşağıdaki cmdlet'leri çalıştırarak, syslog istemci yapılandırmasını doğrulayın:

**Syslog istemci sunucu yapılandırmasını doğrulayın**

```powershell  
Get-SyslogServer
```

**Syslog istemci sertifika kurulumunu doğrulama**

```powershell  
Get-SyslogClient
```

## <a name="syslog-message-schema"></a>Syslog ileti şeması

Azure Stack altyapısının syslog iletmeyi Common Event Format (CEF) biçimli iletileri gönderir.
Her syslog iletisi bu şemasını temel alan yapılandırılır: 

```Syslog
<Time> <Host> <CEF payload>
```

CEF yükü yapısına bağlıdır, ancak her bir alan için eşleme ileti türüne bağlı olarak değişir (Windows olay uyarısı oluşturulan uyarı kapalı).

```CEF
# Common Event Format schema
CEF: <Version>|<Device Vendor>|<Device Product>|<Device Version>|<Signature ID>|<Name>|<Severity>|<Extensions>
* Version: 0.0
* Device Vendor: Microsoft
* Device Product: Microsoft Azure Stack
* Device Version: 1.0
```

### <a name="cef-mapping-for-privileged-endpoint-events"></a>Ayrıcalıklı uç nokta olayları CEF eşleme

```
Prefix fields
* Signature ID: Microsoft-AzureStack-PrivilegedEndpoint: <PEP Event ID>
* Name: <PEP Task Name>
* Severity: mapped from PEP Level (details see the PEP Severity table below)
```

Ayrıcalıklı uç noktası için bir olay tablosu:

| Olay | CESARETLENDİRİCİ olay kimliği | CESARETLENDİRİCİ görev adı | Severity |
|-------|--------------| --------------|----------|
|PrivilegedEndpointAccessed|1000|PrivilegedEndpointAccessedEvent|5|
|SupportSessionTokenRequested |1001|SupportSessionTokenRequestedEvent|5|
|SupportSessionDevelopmentTokenRequested |1002|SupportSessionDevelopmentTokenRequestedEvent|5|
|SupportSessionUnlocked |1003|SupportSessionUnlockedEvent|10|
|SupportSessionFailedToUnlock |1004|SupportSessionFailedToUnlockEvent|10|
|PrivilegedEndpointClosed |1005|PrivilegedEndpointClosedEvent|5|
|NewCloudAdminUser |1006|NewCloudAdminUserEvent|10|
|RemoveCloudAdminUser |1007|RemoveCloudAdminUserEvent|10|
|SetCloudAdminUserPassword |1008|SetCloudAdminUserPasswordEvent|5|
|GetCloudAdminPasswordRecoveryToken |1009|GetCloudAdminPasswordRecoveryTokenEvent|10|
|ResetCloudAdminPassword |1010|ResetCloudAdminPasswordEvent|10|

CESARETLENDİRİCİ önem derecesi tablosu:

| Severity | Düzey | Sayısal değer |
|----------|-------| ----------------|
|0|Undefined|Değer: 0. Tüm düzeylerde günlükleri gösterir|
|10|Kritik|Değer: 1. Kritik Uyarı için günlükleri gösterir|
|8|Hata| Değer: 2. Hata günlüklerini gösterir|
|5|Uyarı|Değer: 3. Günlükleri için bir uyarı gösterir|
|2|Bilgi|Değer: 4. Bir bilgi iletisidir günlüklerini gösterir|
|0|Ayrıntılı|Değer: 5. Tüm düzeylerde günlükleri gösterir|

### <a name="cef-mapping-for-recovery-endpoint-events"></a>Kurtarma uç nokta olayları CEF eşleme

```
Prefix fields
* Signature ID: Microsoft-AzureStack-PrivilegedEndpoint: <REP Event ID>
* Name: <REP Task Name>
* Severity: mapped from REP Level (details see the REP Severity table below)
```

Tablo olayların kurtarma uç noktası için:

| Olay | Temsilcisi olay kimliği | Temsilcisi görev adı | Severity |
|-------|--------------| --------------|----------|
|RecoveryEndpointAccessed |1011|RecoveryEndpointAccessedEvent|5|
|RecoverySessionTokenRequested |1012|RecoverySessionTokenRequestedEvent |5|
|RecoverySessionDevelopmentTokenRequested |1013|RecoverySessionDevelopmentTokenRequestedEvent|5|
|RecoverySessionUnlocked |1014|RecoverySessionUnlockedEvent |10|
|RecoverySessionFailedToUnlock |1015|RecoverySessionFailedToUnlockEvent|10|
|RecoveryEndpointClosed |1016|RecoveryEndpointClosedEvent|5|

Tablo Temsilcisi önem derecesi:

| Severity | Düzey | Sayısal değer |
|----------|-------| ----------------|
|0|Undefined|Değer: 0. Tüm düzeylerde günlükleri gösterir|
|10|Kritik|Değer: 1. Kritik Uyarı için günlükleri gösterir|
|8|Hata| Değer: 2. Hata günlüklerini gösterir|
|5|Uyarı|Değer: 3. Günlükleri için bir uyarı gösterir|
|2|Bilgi|Değer: 4. Bir bilgi iletisidir günlüklerini gösterir|
|0|Ayrıntılı|Değer: 5. Tüm düzeylerde günlükleri gösterir|

### <a name="cef-mapping-for-windows-events"></a>Windows olayları CEF eşleme

```
* Signature ID: ProviderName:EventID
* Name: TaskName
* Severity: Level (for details, see the severity table below)
* Extension: Custom Extension Name (for details, see the Custom Extension table below)
```

Windows olayları için tablo önem derecesi:

| CEF önem derecesi değeri | Windows olay düzeyi | Sayısal değer |
|--------------------|---------------------| ----------------|
|0|Undefined|Değer: 0. Tüm düzeylerde günlükleri gösterir|
|10|Kritik|Değer: 1. Kritik Uyarı için günlükleri gösterir|
|8|Hata| Değer: 2. Hata günlüklerini gösterir|
|5|Uyarı|Değer: 3. Günlükleri için bir uyarı gösterir|
|2|Bilgi|Değer: 4. Bir bilgi iletisidir günlüklerini gösterir|
|0|Ayrıntılı|Değer: 5. Tüm düzeylerde günlükleri gösterir|

Azure stack'teki Windows olayları için özel uzantı tablosu:

| Özel uzantı adı | Windows olay örneği | 
|-----------------------|---------|
|MasChannel | Sistem|
|MasComputer | test.azurestack.contoso.com|
|MasCorrelationActivityID| C8F40D7C-3764-423B-A4FA-C994442238AF|
|MasCorrelationRelatedActivityID| C8F40D7C-3764-423B-A4FA-C994442238AF|
|MasEventData| Svchost!! 4132, G, 0!!! EseDiskFlushConsistency!! ESENT!! 0x800000|
|MasEventDescription| Kullanıcı için Grup İlkesi ayarları başarıyla işlendi. Değişiklik algılanmadı beri son başarılı işleme, Grup İlkesi vardı.|
|MasEventID|1501|
|MasEventRecordID|26637|
|MasExecutionProcessID | 29380|
|MasExecutionThreadID |25480|
|MasKeywords |0x8000000000000000|
|MasKeywordName |Denetim başarılı|
|MasLevel |4|
|MasOpcode |1|
|MasOpcodeName |bilgi|
|MasProviderEventSourceName ||
|MasProviderGuid |AEA1B4FA-97D1-45F2-A64C-4D69FFFD92C9|
|MasProviderName |Microsoft-Windows-Grup İlkesi|
|MasSecurityUserId |\<Windows SID\> |
|MasTask |0|
|MasTaskCategory| İşlem oluşturma|
|MasUserData|KB4093112!! 5112!! Yüklü!! 0x0!! WindowsUpdateAgent Xpath: /Event/UserData / *|
|MasVersion|0|

### <a name="cef-mapping-for-alerts-created"></a>Oluşturulan uyarılar için CEF eşleme

```
* Signature ID: Microsoft Azure Stack Alert Creation : FaultTypeId
* Name: FaultTypeId : AlertId
* Severity: Alert Severity (for details, see alerts severity table below)
* Extension: Custom Extension Name (for details, see the Custom Extension table below)
```

Uyarı önem derecesi tablosu:

| Severity | Düzey |
|----------|-------|
|0|Undefined|
|10|Kritik|
|5|Uyarı|

Azure Stack'te oluşturulan uyarılar için özel uzantı tablosu:

| Özel uzantı adı | Örnek | 
|-----------------------|---------|
|MasEventDescription|AÇIKLAMA: Bir kullanıcı hesabı \<TestUser\> oluşturulduğu \<TestDomain\>. Bu, olası bir güvenlik riski oluşturur. --DÜZELTME: Desteğe başvurun. Bu sorunu çözmek için müşteri desteği gereklidir. Kendi yardımı olmadan bu sorunu çözmek çalışmayın. Bir destek talebi açmadan önce kılavuzdan kullanarak günlük dosya toplama işlemi Başlat https://aka.ms/azurestacklogfiles |

### <a name="cef-mapping-for-alerts-closed"></a>Uyarılar için CEF eşleme kapalı

```
* Signature ID: Microsoft Azure Stack Alert Creation : FaultTypeId
* Name: FaultTypeId : AlertId
* Severity: Information
```

Aşağıdaki örnek, bir syslog iletisi CEF yüküyle gösterir:
```
2018:05:17:-23:59:28 -07:00 TestHost CEF:0.0|Microsoft|Microsoft Azure Stack|1.0|3|TITLE: User Account Created -- DESCRIPTION: A user account \<TestUser\> was created for \<TestDomain\>. It's a potential security risk. -- REMEDIATION: Please contact Support. Customer Assistance is required to resolve this issue. Do not try to resolve this issue without their assistance. Before you open a support request, start the log file collection process using the guidance from https://aka.ms/azurestacklogfiles|10
```

## <a name="next-steps"></a>Sonraki adımlar

[Hizmet İlkesi](azure-stack-servicing-policy.md)