---
title: Azure güncelleştirme yönetimi Aracısı denetimi sonuçları anlama
description: Güncelleştirme yönetimi Aracısı ile ilgili sorunları giderme hakkında bilgi edinin.
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 10/25/2018
ms.topic: conceptual
ms.service: automation
ms.component: update-management
manager: carmonm
ms.openlocfilehash: 20323afe79ad3de1e3dfccd4752c4f7e28d22266
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50095380"
---
# <a name="understand-the-agent-check-results-in-update-management"></a>Güncelleştirme yönetimi Aracısı denetimi sonuçları anlama

Azure olmayan makinenize göstermiyorsa birçok nedeni olabilir **hazır** güncelleştirme yönetimi. Ortamında güncelleştirme yönetimi, arka plandaki sorunu belirlemek için bir karma çalışanı aracı durumunu kontrol edebilirsiniz. Bu makalede, Azure portalından ve çevrimdışı senaryolarda sorun gidericisini çalıştırmak anlatılmaktadır.

## <a name="start-the-troubleshooter"></a>Sorun Gidericisi

Tıklayarak **sorun giderme** altında bağlantı **Güncelleştirme Aracısı hazırlığı** sütun Portalı'nda, başlatma **Güncelleştirme Aracısı sorunlarını giderme** sayfası. Bu sayfa, aracı ve bu makalede bir bağlantı sorunları gidermeye yardımcı olması için sorunları gösterir.

![VM Listesi Sayfası](../media/update-agent-issues/vm-list.png)

> [!NOTE]
> Denetimler VM'nin çalışıyor olması gerekir. VM çalışmıyorsa, bir düğme ile sunulan **VM'yi başlatın**.

Üzerinde **Güncelleştirme Aracısı sorunlarını giderme** sayfasında **çalıştırma denetler**giderici başlatmak için. Sorun giderici kullanan [komutu çalıştırın](../../virtual-machines/windows/run-command.md) aracısı olan bağımlılıkları doğrulamak için makine üzerinde bir komut dosyasını çalıştırmak için. Sorun giderici tamamlandığında, denetimleri sonucunu döndürür.

![Sayfa sorunlarını giderme](../media/update-agent-issues/troubleshoot-page.png)

Tamamlandığında, sonuçları penceresinde döndürülür. [Bölümleri işaretleyin](#pre-requisistes-checks) hakkında her denetim aramak bilgi sağlar.

![Windows Update Aracısı sayfa denetler.](../media/update-agent-issues/update-agent-checks.png)

## <a name="prerequisite-checks"></a>Önkoşul denetimleri

### <a name="operating-system"></a>İşletim sistemi

İşletim sistemi denetimi, karma Runbook çalışanı aşağıdaki işletim sistemlerinden birini çalışıp çalışmadığını doğrular:

|İşletim sistemi  |Notlar  |
|---------|---------|
|Windows Server 2008, Windows Server 2008 R2 RTM    | Destekler, yalnızca değerlendirme güncelleştirin.         |
|Windows Server 2008 R2 SP1 ve üzeri     |.NET framework 4.5.1 veya üzeri gereklidir. ([İndirme .NET Framework](/dotnet/framework/install/guide-for-developers))<br/> Windows PowerShell 4.0 veya üzeri gereklidir. ([WMF 4.0 indirme](https://www.microsoft.com/download/details.aspx?id=40855))<br/> Windows PowerShell 5.1, daha fazla güvenilirlik için önerilir.  ([İndirme WMF 5.1](https://www.microsoft.com/download/details.aspx?id=54616))        |
|CentOS 6 (x86/x64) ve 7 (x64)      | Linux aracılarının bir güncelleştirme havuzuna erişimi olmalıdır. Sınıflandırma tabanlı düzeltme eki uygulama, CentOS, kullanıma hazır yok güvenlik verileri döndürmek için 'ı yum' gerektirir.         |
|Red Hat Enterprise 6 (x86/x64) ve 7 (x64)     | Linux aracılarının bir güncelleştirme havuzuna erişimi olmalıdır.        |
|SUSE Linux Enterprise Server 11 (x86/x64) ve 12 (x64)     | Linux aracılarının bir güncelleştirme havuzuna erişimi olmalıdır.        |
|Ubuntu 14.04 LTS, 16.04 LTS ve 18.04 LTS (x86/x64)      |Linux aracılarının bir güncelleştirme havuzuna erişimi olmalıdır.         |

### <a name="net-451"></a>.NET 4.5.1

.NET framework onay sistemde en az olup olmadığını doğrular [.NET Framework 4.5.1](https://www.microsoft.com/download/details.aspx?id=30653) mevcut.

### <a name="wmf-51"></a>WMF 5.1

WMF onay sistemde Windows Management Framework'ün gerekli sürümü olup olmadığını doğrular. [Windows Management Framework 4.0](https://www.microsoft.com/download/details.aspx?id=40855) desteklenen en düşük sürümü. Yüklemeniz önerilir [Windows Management Framework 5.1](https://www.microsoft.com/download/details.aspx?id=54616) karma Runbook Worker'ın daha fazla güvenilirlik için.

### <a name="tls-12"></a>TLS 1.2

Bu onay, kullanıcılarınızın gönderdiğiniz iletişimleri şifrelemek için TLS 1.2 kullanıyorsanız belirler. TLS 1.0, platform tarafından artık desteklenmemektedir ve istemcileri güncelleştirme yönetimi ile iletişim kurmak için TLS 1.2 kullanmak önerilir.

## <a name="connectivity-checks"></a>Bağlantısı denetimleri

### <a name="registration-endpoint"></a>Kayıt uç noktası

Bu onay, aracıyı düzgün bir şekilde Aracısı ile iletişim kurup kuramadığını belirler.

Proxy ve güvenlik duvarı yapılandırmaları karma Runbook çalışanı aracı kayıt uç noktası ile iletişim kurmasına izin vermeniz gerekir. Adresleri ve bağlantı noktalarını açmak için bir listesi için bkz. [ağ karma çalışanları için planlama](../automation-hybrid-runbook-worker.md#network-planning)

### <a name="operations-endpoint"></a>İşlemleri uç noktası

Bu onay, aracıyı düzgün bir şekilde iş çalışma zamanı veri hizmeti ile iletişim kurup kuramadığını belirler.

Proxy ve güvenlik duvarı yapılandırmaları karma Runbook çalışanı aracı işi çalışma zamanı veri hizmetiyle iletişim kurmasına izin vermeniz gerekir. Adresleri ve bağlantı noktalarını açmak için bir listesi için bkz. [ağ karma çalışanları için planlama](../automation-hybrid-runbook-worker.md#network-planning)

## <a name="vm-service-health-checks"></a>VM hizmet sistem durumu denetimleri

### <a name="monitoring-agent-service-status"></a>İzleme Aracısı hizmeti durumu

Bu onay belirler Microsoft Monitoring Agent `HealthService` makinede çalışıyor.

Hizmet sorunlarını giderme hakkında daha fazla bilgi için bkz: [Microsoft Monitoring Agent çalışmıyor](hybrid-runbook-worker.md#mma-not-running).

Microsoft Monitoring Agent'ı yeniden yüklemek için bkz: [yüklemek ve Microsoft Monitoring Agent'ı yapılandırma](/log-analytics/log-analytics-concept-hybrid.md#install-and-configure-agent)

### <a name="monitoring-agent-service-events"></a>İzleme Aracısı hizmeti olayları

Bu onay yapılmadığını herhangi belirler `4502` Operations Manager'da olayları son 24 saat içindeki makinede oturum açın.

Bu olay hakkında daha fazla bilgi için bkz: [sorun giderme kılavuzu](hybrid-runbook-worker.md#event-4502) bu olay için.

## <a name="access-permissions-checks"></a>Erişim izinleri denetimleri

### <a name="machinekeys-folder-access"></a>MachineKeys klasör erişimi

Crypto klasörüne erişim denetimi yerel sistem hesabı için erişim olup olmadığını belirler `C:\ProgramData\Microsoft\Crypto\RSA`

## <a name="troubleshoot-offline"></a>Çevrimdışı sorunlarını giderme

Yerel olarak bir betik çalıştırarak, bir karma Runbook çalışanı üzerinde çevrimdışı sorun gidericisini kullanabilirsiniz. Betik [sorun giderme WindowsUpdateAgentRegistration](https://www.powershellgallery.com/packages/Troubleshoot-WindowsUpdateAgentRegistration) PowerShell galerisinde bulunabilir. Bu betiğin çıktının bir örneği aşağıdaki örnekte gösterilmiştir:

```output
RuleId                      : OperatingSystemCheck
RuleGroupId                 : prerequisites
RuleName                    : Operating System
RuleGroupName               : Prerequisite Checks
RuleDescription             : The Windows Operating system must be version 6.1.7601 (Windows Server 2008 R2 SP1) or higher
CheckResult                 : Passed
CheckResultMessage          : Operating System version is supported
CheckResultMessageId        : OperatingSystemCheck.Passed
CheckResultMessageArguments : {}

RuleId                      : DotNetFrameworkInstalledCheck
RuleGroupId                 : prerequisites
RuleName                    : .Net Framework 4.5+
RuleGroupName               : Prerequisite Checks
RuleDescription             : .NET Framework version 4.5 or higher is required
CheckResult                 : Passed
CheckResultMessage          : .NET Framework version 4.5+ is found
CheckResultMessageId        : DotNetFrameworkInstalledCheck.Passed
CheckResultMessageArguments : {}

RuleId                      : WindowsManagementFrameworkInstalledCheck
RuleGroupId                 : prerequisites
RuleName                    : WMF 5.1
RuleGroupName               : Prerequisite Checks
RuleDescription             : Windows Management Framework version 4.0 or higher is required (version 5.1 or higher is preferable)
CheckResult                 : Passed
CheckResultMessage          : Detected Windows Management Framework version: 5.1.17763.1
CheckResultMessageId        : WindowsManagementFrameworkInstalledCheck.Passed
CheckResultMessageArguments : {5.1.17763.1}

RuleId                      : AutomationAgentServiceConnectivityCheck1
RuleGroupId                 : connectivity
RuleName                    : Registration endpoint
RuleGroupName               : connectivity
RuleDescription             : 
CheckResult                 : Failed
CheckResultMessage          : Unable to find Workspace registration information in registry
CheckResultMessageId        : AutomationAgentServiceConnectivityCheck1.Failed.NoRegistrationFound
CheckResultMessageArguments : {}

RuleId                      : AutomationJobRuntimeDataServiceConnectivityCheck
RuleGroupId                 : connectivity
RuleName                    : Operations endpoint
RuleGroupName               : connectivity
RuleDescription             : Proxy and firewall configuration must allow Automation Hybrid Worker agent to communicate with eus2-jobruntimedata-prod-su1.azure-automation.net
CheckResult                 : Passed
CheckResultMessage          : TCP Test for eus2-jobruntimedata-prod-su1.azure-automation.net (port 443) succeeded
CheckResultMessageId        : AutomationJobRuntimeDataServiceConnectivityCheck.Passed
CheckResultMessageArguments : {eus2-jobruntimedata-prod-su1.azure-automation.net}

RuleId                      : MonitoringAgentServiceRunningCheck
RuleGroupId                 : servicehealth
RuleName                    : Monitoring Agent service status
RuleGroupName               : VM Service Health Checks
RuleDescription             : HealthService must be running on the machine
CheckResult                 : Failed
CheckResultMessage          : Microsoft Monitoring Agent service (HealthService) is not running
CheckResultMessageId        : MonitoringAgentServiceRunningCheck.Failed
CheckResultMessageArguments : {Microsoft Monitoring Agent, HealthService}

RuleId                      : MonitoringAgentServiceEventsCheck
RuleGroupId                 : servicehealth
RuleName                    : Monitoring Agent service events
RuleGroupName               : VM Service Health Checks
RuleDescription             : Event Log must not have event 4502 logged in the past 24 hours
CheckResult                 : Failed
CheckResultMessage          : Microsoft Monitoring Agent service Event Log (Operations Manager) does not exist on the machine
CheckResultMessageId        : MonitoringAgentServiceEventsCheck.Failed.NoLog
CheckResultMessageArguments : {Microsoft Monitoring Agent, Operations Manager, 4502}

RuleId                      : CryptoRsaMachineKeysFolderAccessCheck
RuleGroupId                 : permissions
RuleName                    : Crypto RSA MachineKeys Folder Access
RuleGroupName               : Access Permission Checks
RuleDescription             : SYSTEM account must have WRITE and MODIFY access to 'C:\\ProgramData\\Microsoft\\Crypto\\RSA\\MachineKeys'
CheckResult                 : Passed
CheckResultMessage          : Have permissions to access C:\\ProgramData\\Microsoft\\Crypto\\RSA\\MachineKeys
CheckResultMessageId        : CryptoRsaMachineKeysFolderAccessCheck.Passed
CheckResultMessageArguments : {C:\\ProgramData\\Microsoft\\Crypto\\RSA\\MachineKeys}

RuleId                      : TlsVersionCheck
RuleGroupId                 : prerequisites
RuleName                    : TLS 1.2
RuleGroupName               : Prerequisite Checks
RuleDescription             : Client and Server connections must support TLS 1.2
CheckResult                 : Passed
CheckResultMessage          : TLS 1.2 is enabled by default on the Operating System.
CheckResultMessageId        : TlsVersionCheck.Passed.EnabledByDefault
CheckResultMessageArguments : {}
```

## <a name="next-steps"></a>Sonraki adımlar

Karma Runbook çalışanlarıyla ek sorunları gidermek için bkz: [sorun giderme - karma Runbook çalışanları](hybrid-runbook-worker.md)
