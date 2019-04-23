---
title: Güncelleştirme yönetimini Azure Windows Aracısı onay sonuçları anlama
description: Güncelleştirme yönetimi Aracısı ile ilgili sorunları giderme hakkında bilgi edinin.
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 04/22/2019
ms.topic: conceptual
ms.service: automation
ms.subservice: update-management
manager: carmonm
ms.openlocfilehash: 864fe70d7702680f21234a1a15c02515b19f770b
ms.sourcegitcommit: c884e2b3746d4d5f0c5c1090e51d2056456a1317
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60149623"
---
# <a name="understand-the-windows-agent-check-results-in-update-management"></a>Windows Aracısı onay sonuçları güncelleştirme yönetimini anlama

Makinenizde görüntülenmiyorsa birçok nedeni olabilir **hazır** güncelleştirme yönetimi. Ortamında güncelleştirme yönetimi, arka plandaki sorunu belirlemek için bir karma çalışanı aracı durumunu kontrol edebilirsiniz. Bu makalede Azure portalı ve Azure olmayan makineler Azure makineler için sorun giderici çalıştırın anlatılmaktadır [çevrimdışı senaryosu](#troubleshoot-offline).

Aşağıdaki listede, bir makine olabilir üç hazır olma durumlarından şunlardır:

* **Hazır** -Güncelleştirme Aracısı dağıtılır ve son 1 saatten önce görüldü.
* **Bağlantısı kesilmiş** -Güncelleştirme Aracısı dağıtıldıktan ve üzerinde 1 saat önce son kez görüldü.
* **Yapılandırılmamış** -Windows update Aracısı, bulunamadığında veya ekleme işlemi tamamlanmadı.

> [!NOTE]
> Hangi Azure portalında gösterilir ve makinenin geçerli durumu arasında bir gecikme olabilir.

## <a name="start-the-troubleshooter"></a>Sorun Gidericisi

Tıklayarak Azure makineler için **sorun giderme** altında bağlantı **Güncelleştirme Aracısı hazırlığı** portal başlatır sütununda **Güncelleştirme Aracısı sorunlarını giderme** sayfası. Azure olmayan makineler için bağlantıyı bu makaleye getirir. Bkz: [çevrimdışı yönergeleri](#troubleshoot-offline) Azure olmayan makine giderilir.

![Sanal makine yönetim listesini güncelleştirme](../media/update-agent-issues/vm-list.png)

> [!NOTE]
> Bir aracı durumunu denetlemek için VM çalıştırılması gerekir. VM çalışmıyorsa, bir **VM'yi başlatın** düğmesi görünür.

Üzerinde **Güncelleştirme Aracısı sorunlarını giderme** sayfasında **denetimlerini çalıştırmak** giderici başlatmak için. Sorun giderici kullanan [komutu Çalıştır](../../virtual-machines/windows/run-command.md) Aracısı bağımlılıkları doğrulamak için makine üzerinde bir komut dosyasını çalıştırmak için. Sorun giderici tamamlandığında denetimleri sonucunu döndürür.

![Windows Update Aracısı sayfasında ilgili sorunları giderme](../media/update-agent-issues/troubleshoot-page.png)

Hazır olduğunuzda, sonuçları sayfasında gösterilir. Denetimleri aşağıdaki bölümlerde her iade nelerin dahil olduğunu gösterir.

![Windows Update Aracısı denetimleri sorunlarını giderme](../media/update-agent-issues/update-agent-checks.png)

## <a name="prerequisite-checks"></a>Önkoşul denetimleri

### <a name="operating-system"></a>İşletim sistemi

İşletim sistemi denetimi, karma Runbook çalışanı bu işletim sistemlerinden birini çalışıp çalışmadığını doğrular:

|İşletim sistemi  |Notlar  |
|---------|---------|
|Windows Server 2008 R2 RTM, Windows Server 2008 | Destekler, yalnızca değerlendirme güncelleştirin.         |
|Windows Server 2008 R2 SP1 ve üzeri |.NET framework 4.5.1 veya üzeri gereklidir. ([.NET Framework'ü indirin](/dotnet/framework/install/guide-for-developers))<br/> Windows PowerShell 4.0 veya üzeri gereklidir. ([Windows Management Framework 4.0 indirme](https://www.microsoft.com/download/details.aspx?id=40855))<br/> Windows PowerShell 5.1, daha fazla güvenilirlik için önerilir.  ([İndirme Windows Management Framework'ün 5.1](https://www.microsoft.com/download/details.aspx?id=54616))        |

### <a name="net-451"></a>.NET 4.5.1

.NET Framework onay sistem en az sahip olduğunu doğrular [.NET Framework 4.5.1](https://www.microsoft.com/download/details.aspx?id=30653) yüklü.

### <a name="wmf-51"></a>WMF 5.1

WMF denetim sistemi Windows Management Framework (WMF) gerekli sürümü olduğunu doğrular. [Windows Management Framework 4.0](https://www.microsoft.com/download/details.aspx?id=40855) desteklenen en erken sürümü. Yüklemenizi öneririz [Windows Management Framework 5.1](https://www.microsoft.com/download/details.aspx?id=54616) karma Runbook çalışanı güvenilirliğini artırmak için.

### <a name="tls-12"></a>TLS 1.2

Bu onay, TLS 1.2, iletişimi şifrelemek için kullanmakta olduğunuz olup olmadığını belirler. TLS 1.0, platform tarafından artık desteklenmiyor. İstemcileri güncelleştirme yönetimi ile iletişim kurmak için TLS 1.2 kullanmanızı öneririz.

## <a name="connectivity-checks"></a>Bağlantısı denetimleri

### <a name="registration-endpoint"></a>Kayıt uç noktası

Bu onay, aracıyı düzgün bir şekilde Aracısı ile iletişim kurup kuramadığını belirler.

Proxy ve güvenlik duvarı yapılandırmaları karma Runbook çalışanı aracı kayıt uç noktası ile iletişim kurmasına izin vermeniz gerekir. Adresleri ve bağlantı noktalarını açmak için bir listesi için bkz. [ağ karma çalışanları için planlama](../automation-hybrid-runbook-worker.md#network-planning).

### <a name="operations-endpoint"></a>İşlemleri uç noktası

Bu onay, aracıyı düzgün bir şekilde iş çalışma zamanı veri hizmeti ile iletişim kurup kuramadığını belirler.

Proxy ve güvenlik duvarı yapılandırmaları karma Runbook çalışanı aracı işi çalışma zamanı veri hizmetiyle iletişim kurmasına izin vermeniz gerekir. Adresleri ve bağlantı noktalarını açmak için bir listesi için bkz. [ağ karma çalışanları için planlama](../automation-hybrid-runbook-worker.md#network-planning).

## <a name="vm-service-health-checks"></a>VM hizmet sistem durumu denetimleri

### <a name="monitoring-agent-service-status"></a>İzleme Aracısı hizmeti durumu

Bu onay belirler olmadığını `HealthService`, Microsoft Monitoring Agent, makinede çalışıyor.

Hizmet sorunlarını giderme hakkında daha fazla bilgi için bkz: [Microsoft Monitoring Agent çalışmıyor](hybrid-runbook-worker.md#mma-not-running).

Microsoft Monitoring Agent'ı yeniden yüklemek için bkz: [yüklemek ve Microsoft Monitoring Agent'ı yapılandırma](../../azure-monitor/learn/quick-collect-windows-computer.md#install-the-agent-for-windows).

### <a name="monitoring-agent-service-events"></a>İzleme Aracısı hizmeti olayları

Bu onay herhangi gerekmediğini belirleyen `4502` olaylar son 24 saat içindeki makine üzerinde Azure Operations Manager günlüğünde görünür.

Bu olay hakkında daha fazla bilgi için bkz: [sorun giderme kılavuzu](hybrid-runbook-worker.md#event-4502) bu olay için.

## <a name="access-permissions-checks"></a>Erişim izinleri denetimleri

### <a name="machinekeys-folder-access"></a>MachineKeys klasör erişimi

Şifreleme klasör erişim denetimi, yerel sistem hesabını C:\ProgramData\Microsoft\Crypto\RSA erişimi olup olmadığını belirler.

## <a name="troubleshoot-offline"></a>Çevrimdışı sorunlarını giderme

Betiği yerel olarak çalıştırarak karma Runbook çalışanı üzerinde çevrimdışı sorun gidericisini kullanabilirsiniz. Betik alabilirsiniz [sorun giderme WindowsUpdateAgentRegistration](https://www.powershellgallery.com/packages/Troubleshoot-WindowsUpdateAgentRegistration), PowerShell Galerisi'ndeki. Bu betiğin çıkışı aşağıdaki örnekteki gibi görünür:

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
RuleName                    : .NET Framework 4.5+
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

Karma Runbook çalışanlarıyla daha fazla sorunlarını gidermek için bkz: [sorun giderme karma Runbook çalışanları](hybrid-runbook-worker.md).

