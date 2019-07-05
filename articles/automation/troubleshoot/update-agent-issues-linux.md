---
title: Güncelleştirme yönetimini Azure Linux Aracısı onay sonuçları anlama
description: Güncelleştirme yönetimi Aracısı ile ilgili sorunları giderme hakkında bilgi edinin.
services: automation
author: bobbytreed
ms.author: robreed
ms.date: 04/22/2019
ms.topic: conceptual
ms.service: automation
ms.subservice: update-management
manager: carmonm
ms.openlocfilehash: c37d8be8862e75a6520ccefe4b9df93dd993b2a8
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67477108"
---
# <a name="understand-the-linux-agent-check-results-in-update-management"></a>Güncelleştirme yönetimi Linux Aracısı onay sonuçlarında anlama

Makinenizde görüntülenmiyorsa birçok nedeni olabilir **hazır** güncelleştirme yönetimi. Ortamında güncelleştirme yönetimi, arka plandaki sorunu belirlemek için bir karma çalışanı aracı durumunu kontrol edebilirsiniz. Bu makalede Azure portalı ve Azure olmayan makineler Azure makineler için sorun giderici çalıştırın anlatılmaktadır [çevrimdışı senaryosu](#troubleshoot-offline).

Aşağıdaki listede, bir makine olabilir üç hazır olma durumlarından şunlardır:

* **Hazır** -Güncelleştirme Aracısı dağıtılır ve son 1 saatten önce görüldü.
* **Bağlantısı kesilmiş** -Güncelleştirme Aracısı dağıtıldıktan ve üzerinde 1 saat önce son kez görüldü.
* **Yapılandırılmamış** -Windows update Aracısı, bulunamadığında veya ekleme işlemi tamamlanmadı.

> [!NOTE]
> Hangi Azure portalında gösterilir ve makinenin geçerli durumu arasında bir gecikme olabilir.

## <a name="start-the-troubleshooter"></a>Sorun Gidericisi

Tıklayarak Azure makineler için **sorun giderme** altında bağlantı **Güncelleştirme Aracısı hazırlığı** portal başlatır sütununda **Güncelleştirme Aracısı sorunlarını giderme** sayfası. Azure olmayan makineler için bağlantıyı bu makaleye getirir. Azure olmayan makine sorunlarını gidermek için çevrimdışı yönergelere bakın.

![VM Listesi Sayfası](../media/update-agent-issues-linux/vm-list.png)

> [!NOTE]
> Denetimler VM'nin çalışıyor olması gerekir. VM çalışmıyorsa, bir düğme ile sunulan **VM'yi başlatın**.

Üzerinde **Güncelleştirme Aracısı sorunlarını giderme** sayfasında **çalıştırma denetler**giderici başlatmak için. Sorun giderici kullanan [komutu çalıştırın](../../virtual-machines/linux/run-command.md) aracısı olan bağımlılıkları doğrulamak için makine üzerinde bir komut dosyasını çalıştırmak için. Sorun giderici tamamlandığında, denetimleri sonucunu döndürür.

![Sayfa sorunlarını giderme](../media/update-agent-issues-linux/troubleshoot-page.png)

Tamamlandığında, sonuçları penceresinde döndürülür. Onay bölümler, her denetimi aramak hakkında bilgi sağlar.

![Windows Update Aracısı sayfa denetler.](../media/update-agent-issues-linux/update-agent-checks.png)

## <a name="prerequisite-checks"></a>Önkoşul denetimleri

### <a name="operating-system"></a>İşletim sistemi

İşletim sistemi denetimi, karma Runbook çalışanı aşağıdaki işletim sistemlerinden birini çalışıp çalışmadığını doğrular:

|İşletim sistemi  |Notlar  |
|---------|---------|
|CentOS 6 (x86/x64) ve 7 (x64)      | Linux aracılarının bir güncelleştirme havuzuna erişimi olmalıdır. Sınıflandırma tabanlı düzeltme eki uygulama, CentOS, kullanıma hazır yoksa güvenlik verileri döndürmek için 'ı yum' gerektirir.         |
|Red Hat Enterprise 6 (x86/x64) ve 7 (x64)     | Linux aracılarının bir güncelleştirme havuzuna erişimi olmalıdır.        |
|SUSE Linux Enterprise Server 11 (x86/x64) ve 12 (x64)     | Linux aracılarının bir güncelleştirme havuzuna erişimi olmalıdır.        |
|Ubuntu 14.04 LTS, 16.04 LTS ve 18.04 LTS (x86/x64)      |Linux aracılarının bir güncelleştirme havuzuna erişimi olmalıdır.         |

## <a name="monitoring-agent-service-health-checks"></a>İzleme Aracısı hizmeti sistem durumu denetimleri

### <a name="oms-agent"></a>OMS Aracısı

Bu onay, Linux için OMS aracısı yüklü olduğunu sağlar. Nasıl yükleneceği hakkında yönergeler için bkz [Linux için aracıyı yükleme](../../azure-monitor/learn/quick-collect-linux-computer.md#install-the-agent-for-linux
).

### <a name="oms-agent-status"></a>OMS Aracısı durumu

Bu onay, Linux için OMS Aracısı çalıştığını sağlar. Aracıyı çalışmıyorsa, yeniden denemek için aşağıdaki komutu çalıştırabilirsiniz. Aracı sorunlarını giderme hakkında daha fazla bilgi için bkz. [Linux karma Runbook çalışanı sorunlarını giderme](hybrid-runbook-worker.md#linux)

```bash
sudo /opt/microsoft/omsagent/bin/service_control restart
```

### <a name="multihoming"></a>Birden çok giriş

Bu onay, aracıyı birden çok çalışma alanına raporlama olmadığını belirler. Çoklu yönlendirmeyi güncelleştirme yönetimi tarafından desteklenmiyor.

### <a name="hybrid-runbook-worker"></a>Karma Runbook Çalışanı

Bu denetim, Linux için OMS Aracısı karma Runbook çalışanı paket olup olmadığını doğrular. Bu paket çalışacak şekilde güncelleştirme yönetimi için gereklidir.

### <a name="hybrid-runbook-worker-status"></a>Karma Runbook çalışanı durumu

Bu onay karma Runbook çalışanı makinede çalıştığından emin olur. Aşağıdaki işlemler, karma Runbook çalışanı düzgün çalışıyorsa mevcut olmalıdır. Daha fazla bilgi için bkz. [Linux için Log Analytics Aracısı sorunlarını giderme](hybrid-runbook-worker.md#oms-agent-not-running).

```bash
nxautom+   8567      1  0 14:45 ?        00:00:00 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/main.py /var/opt/microsoft/omsagent/state/automationworker/oms.conf rworkspace:<workspaceId> <Linux hybrid worker version>
nxautom+   8593      1  0 14:45 ?        00:00:02 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/hybridworker.py /var/opt/microsoft/omsagent/state/automationworker/worker.conf managed rworkspace:<workspaceId> rversion:<Linux hybrid worker version>
nxautom+   8595      1  0 14:45 ?        00:00:02 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/hybridworker.py /var/opt/microsoft/omsagent/<workspaceId>/state/automationworker/diy/worker.conf managed rworkspace:<workspaceId> rversion:<Linux hybrid worker version>
```

## <a name="connectivity-checks"></a>Bağlantısı denetimleri

### <a name="general-internet-connectivity"></a>Genel internet bağlantısı

Bu onay, makinenin internet erişimi olduğundan emin olur.

### <a name="registration-endpoint"></a>Kayıt uç noktası

Bu onay, aracıyı düzgün bir şekilde Aracısı ile iletişim kurup kuramadığını belirler.

Proxy ve güvenlik duvarı yapılandırmaları karma Runbook çalışanı aracı kayıt uç noktası ile iletişim kurmasına izin vermeniz gerekir. Adresleri ve bağlantı noktalarını açmak için bir listesi için bkz. [ağ karma çalışanları için planlama](../automation-hybrid-runbook-worker.md#network-planning)

### <a name="operations-endpoint"></a>İşlemleri uç noktası

Bu onay, aracıyı düzgün bir şekilde iş çalışma zamanı veri hizmeti ile iletişim kurup kuramadığını belirler.

Proxy ve güvenlik duvarı yapılandırmaları karma Runbook çalışanı aracı işi çalışma zamanı veri hizmetiyle iletişim kurmasına izin vermeniz gerekir. Adresleri ve bağlantı noktalarını açmak için bir listesi için bkz. [ağ karma çalışanları için planlama](../automation-hybrid-runbook-worker.md#network-planning)

### <a name="log-analytics-endpoint-1"></a>Log Analytics uç noktası 1

Bu onay, makinenizin Log Analytics aracı tarafından gerekli uç noktalarına erişime sahip olduğunu doğrular.

### <a name="log-analytics-endpoint-2"></a>Log Analytics uç noktası 2

Bu onay, makinenizin Log Analytics aracı tarafından gerekli uç noktalarına erişime sahip olduğunu doğrular.

### <a name="log-analytics-endpoint-3"></a>Log Analytics uç noktası 3

Bu onay, makinenizin Log Analytics aracı tarafından gerekli uç noktalarına erişime sahip olduğunu doğrular.

## <a name="troubleshoot-offline"></a>Çevrimdışı sorunlarını giderme

Yerel olarak bir betik çalıştırarak, bir karma Runbook çalışanı üzerinde çevrimdışı sorun gidericisini kullanabilirsiniz. Python betiğini [update_mgmt_health_check.py](https://gallery.technet.microsoft.com/scriptcenter/Troubleshooting-utility-3bcbefe6) betik Merkezi'nde bulunabilir. Bu betiğin çıktının bir örneği aşağıdaki örnekte gösterilmiştir:

```output
Debug: Machine Information:   Static hostname: LinuxVM2
         Icon name: computer-vm
           Chassis: vm
        Machine ID: 00000000000000000000000000000000
           Boot ID: 00000000000000000000000000000000
    Virtualization: microsoft
  Operating System: Ubuntu 16.04.5 LTS
            Kernel: Linux 4.15.0-1025-azure
      Architecture: x86-64


Passed: Operating system version is supported

Passed: Microsoft Monitoring agent is installed

Debug: omsadmin.conf file contents:
        WORKSPACE_ID=00000000-0000-0000-0000-000000000000
        AGENT_GUID=00000000-0000-0000-0000-000000000000
        LOG_FACILITY=local0
        CERTIFICATE_UPDATE_ENDPOINT=https://00000000-0000-0000-0000-000000000000.oms.opinsights.azure.com/ConfigurationService.Svc/RenewCertificate
        URL_TLD=opinsights.azure.com
        DSC_ENDPOINT=https://scus-agentservice-prod-1.azure-automation.net/Accou            nts/00000000-0000-0000-0000-000000000000/Nodes\(AgentId='00000000-0000-0000-0000-000000000000'\)
        OMS_ENDPOINT=https://00000000-0000-0000-0000-000000000000.ods.opinsights            .azure.com/OperationalData.svc/PostJsonDataItems
        AZURE_RESOURCE_ID=/subscriptions/00000000-0000-0000-0000-000000000000/re            sourcegroups/myresourcegroup/providers/microsoft.compute/virtualmachines/linuxvm            2
        OMSCLOUD_ID=0000-0000-0000-0000-0000-0000-00
        UUID=00000000-0000-0000-0000-000000000000


Passed: Microsoft Monitoring agent is running

Passed: Machine registered with log analytics workspace:['00000000-0000-0000-0000-000000000000']

Passed: Hybrid worker package is present

Passed: Hybrid worker is running

Passed: Machine is connected to internet

Passed: TCP test for {scus-agentservice-prod-1.azure-automation.net} (port 443)             succeeded

Passed: TCP test for {eus2-jobruntimedata-prod-su1.azure-automation.net} (port 4            43) succeeded

Passed: TCP test for {00000000-0000-0000-0000-000000000000.ods.opinsights.azure.            com} (port 443) succeeded

Passed: TCP test for {00000000-0000-0000-0000-000000000000.oms.opinsights.azure.            com} (port 443) succeeded

Passed: TCP test for {ods.systemcenteradvisor.com} (port 443) succeeded

```

## <a name="next-steps"></a>Sonraki adımlar

Karma Runbook çalışanlarıyla ek sorunları gidermek için bkz: [sorun giderme - karma Runbook çalışanları](hybrid-runbook-worker.md)

