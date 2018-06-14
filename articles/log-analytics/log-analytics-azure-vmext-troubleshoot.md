---
title: Azure Log Analytics VM uzantısı sorunlarını giderme | Microsoft Docs
description: Belirtiler, nedenleri ve Çözümlemesi en yaygın sorunları için günlük analizi VM uzantısı ile Windows ve Linux Azure VM'ler için açıklanmaktadır.
services: log-analytics
documentationcenter: ''
author: MGoedtel
manager: carmonm
editor: tysonn
ms.assetid: ''
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/08/2018
ms.author: magoedte
ms.openlocfilehash: d1e70d8f9fb929e3877c88fd4c1169a0c76ac2a6
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
ms.locfileid: "29394995"
---
# <a name="troubleshooting-the-log-analytics-vm-extension"></a>Günlük analizi VM uzantısı sorunlarını giderme
Bu makalede Windows ve Linux Microsoft Azure üzerinde çalışan sanal makineler için günlük analizi VM uzantısı ile karşılaşabilirsiniz ve bunları gidermek için olası çözümleri önerir hatalarında sorun giderme yardım sağlar.

Uzantı durumunu doğrulamak için Azure Portal'da aşağıdaki adımları gerçekleştirin.

1. [Azure portal](http://portal.azure.com) oturum açın.
2. Azure portalında tıklatın **tüm hizmetleri**. Kaynak listesinde yazın **sanal makineleri**. Yazmaya başladığınızda liste, girişinize göre filtrelenir. Seçin **sanal makineleri**.
3. Sanal makineler listesi, bulun ve seçin.
3. Sanal makineye tıklayın **uzantıları**.
4. Listeden, günlük analizi uzantısı veya etkin olup olmadığını denetleyin.  Linux için aracı olarak listelenen **OMSAgentforLinux** ve Windows için aracı olarak listeleniyor **MicrosoftMonitoringAgent**.

   ![VM uzantısı görünümü](./media/log-analytics-azure-vmext-troubleshoot/log-analytics-vmview-extensions.png)

4. Uzantısı ayrıntılarını görüntülemek için tıklatın. 

   ![VM uzantısı ayrıntıları](./media/log-analytics-azure-vmext-troubleshoot/log-analytics-vmview-extensiondetails.png)

## <a name="troubleshooting-azure-windows-vm-extension"></a>Sorun giderme Azure Windows VM uzantısı

Varsa *Microsoft İzleme Aracısı* VM uzantısı yükleme değil veya raporlama, sorunu gidermek için aşağıdaki adımları gerçekleştirebilirsiniz.

1. Azure VM aracısı yüklü olup olmadığını denetleyin ve doğru adımları kullanarak çalışma [KB 2965986](https://support.microsoft.com/kb/2965986#mt1).
   * VM Aracısı günlük dosyasını gözden geçirebilirsiniz `C:\WindowsAzure\logs\WaAppAgent.log`
   * Günlük mevcut değilse, VM aracısı yüklü değil.
   * [Azure VM Aracısı yükleme](log-analytics-quick-collect-azurevm.md#enable-the-log-analytics-vm-extension)
2. Microsoft Monitoring Agent uzantısı sinyal görev aşağıdaki adımları kullanarak çalışıyor onaylayın:
   * Sanal makinede oturum açın
   * Görev Zamanlayıcısı'nı açın ve Bul `update_azureoperationalinsight_agent_heartbeat` görevi
   * Görev etkin ve bir dakikada çalıştığını onaylayın
   * Sinyal günlük dosyası iade etme `C:\WindowsAzure\Logs\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\heartbeat.log`
3. Microsoft İzleme Aracısı VM uzantısı günlük dosyalarını inceleyin `C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent`
4. Sanal makine PowerShell betikleri çalıştırabilirsiniz emin olun
5. C:\Windows\temp izinlerini değiştirmezsiniz emin olun
6. Sanal makine üzerinde yükseltilmiş bir PowerShell penceresinde aşağıdakileri yazarak Microsoft Monitoring Agent durumunu görüntüleme `  (New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg').GetCloudWorkspaces() | Format-List`
7. Microsoft Monitoring Agent Kurulum günlük dosyalarını inceleyin `C:\Windows\System32\config\systemprofile\AppData\Local\SCOM\Logs`

Daha fazla bilgi için bkz: [Windows uzantıları sorun giderme](../virtual-machines/windows/extensions-oms.md).

## <a name="troubleshooting-linux-vm-extension"></a>Linux VM uzantısı sorunlarını giderme
Varsa *Linux için OMS aracısının* VM uzantısı yükleme değil veya raporlama, sorunu gidermek için aşağıdaki adımları gerçekleştirebilirsiniz.

1. Uzantı durumu ise *bilinmeyen* Azure VM aracısı yüklü olup olmadığını denetleyin ve doğru VM aracı günlük dosyasını gözden geçirerek çalışma `/var/log/waagent.log`
   * Günlük mevcut değilse, VM aracısı yüklü değil.
   * [Linux VM'ler üzerinde Azure VM Aracısı yükleme](log-analytics-quick-collect-azurevm.md#enable-the-log-analytics-vm-extension)
2. Linux VM uzantısı günlük dosyaları için sağlıksız diğer durumlar için OMS Aracısı'nı gözden `/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/extension.log` ve `/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/CommandExecution.log`
3. Uzantı durumu iyi değil, ancak veri değil yükleniyor Linux günlük dosyalarında OMS Aracısı gözden geçirin. `/var/opt/microsoft/omsagent/log/omsagent.log`

Daha fazla bilgi için bkz: [Linux uzantı sorunlarını giderme](../virtual-machines/linux/extensions-oms.md).

## <a name="next-steps"></a>Sonraki adımlar

Azure dışında bilgisayarlarda barındırılan Linux için OMS Aracısı ile ilgili ek sorun giderme yönergeleri için bkz: [Azure günlük analizi Linux Aracısı sorun giderme](log-analytics-agent-linux-support.md).  
