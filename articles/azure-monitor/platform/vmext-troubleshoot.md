---
title: Azure Log Analytics VM uzantısını Azure İzleyicisi'nde sorun giderme | Microsoft Docs
description: Belirtiler, nedenleri ve Log Analytics VM uzantısı ile çözüm en sık karşılaşılan sorunlara yönelik Windows ve Linux Azure Vm'leri için açıklar.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: ''
ms.service: log-analytics
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/08/2018
ms.author: magoedte
ms.openlocfilehash: 17a52cf17c62c930dc3922ded48d659f56016688
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58370508"
---
# <a name="troubleshooting-the-log-analytics-vm-extension-in-azure-monitor"></a>Log Analytics VM uzantısını Azure İzleyicisi'nde sorun giderme
Bu makalede, Windows ve Linux'ta Microsoft Azure üzerinde çalışan sanal makineler için Log Analytics VM uzantısı ile karşılaşabilirsiniz ve bunların çözülmesine yönelik olası çözümler önerir hatalarını giderme hakkında Yardım sağlar.

Uzantı durumu doğrulamak için Azure portalında aşağıdaki adımları gerçekleştirin.

1. [Azure portal](https://portal.azure.com) oturum açın.
2. Azure portalında **Tüm hizmetler**’e tıklayın. Kaynak listesinde yazın **sanal makineler**. Yazmaya başladığınızda liste, girişinize göre filtrelenir. Seçin **sanal makineler**.
3. Sanal makineler listesinde, bulun ve seçin.
3. Sanal makineye tıklayın **uzantıları**.
4. Listeden Log Analytics uzantısı etkin'in etkin olup olmadığını denetleyin.  Linux için aracı olarak listelenip listelenmediğini **OMSAgentforLinux** ve Windows için aracı olarak listeleniyor **MicrosoftMonitoringAgent**.

   ![VM uzantısı görüntüle](./media/vmext-troubleshoot/log-analytics-vmview-extensions.png)

4. Ayrıntılarını görüntülemek için uzantısına tıklayın. 

   ![VM uzantısı ayrıntıları](./media/vmext-troubleshoot/log-analytics-vmview-extensiondetails.png)

## <a name="troubleshooting-azure-windows-vm-extension"></a>Azure Windows VM uzantısı sorunlarını giderme

Varsa *Microsoft Monitoring Agent* VM uzantısı yükleme değil veya raporlama, sorunu gidermek için aşağıdaki adımları gerçekleştirebilirsiniz.

1. Azure VM aracısı yüklü olup olmadığını denetleyin ve doğru adımları kullanarak çalışma [KB 2965986](https://support.microsoft.com/kb/2965986#mt1).
   * Ayrıca, VM aracı günlük dosyasını gözden geçirebilirsiniz `C:\WindowsAzure\logs\WaAppAgent.log`
   * Günlük yoksa, VM aracısı yüklü değil.
   * [Azure VM aracısını yükleyin](../../azure-monitor/learn/quick-collect-azurevm.md#enable-the-log-analytics-vm-extension)
2. Aşağıdaki adımları kullanarak Microsoft Monitoring Agent uzantısı sinyal görevin çalıştığı onaylayın:
   * Sanal makinede oturum açın
   * Görev Zamanlayıcısı'nı açın ve bulun `update_azureoperationalinsight_agent_heartbeat` görevi
   * Görev etkin ve dakikada bir çalışan onaylayın
   * Sinyal logfile iade et `C:\WindowsAzure\Logs\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\heartbeat.log`
3. Microsoft İzleme Aracısı VM uzantısını günlük dosyalarını gözden geçirin `C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent`
4. Sanal makine PowerShell betiklerini çalıştırabilirsiniz emin olun.
5. C:\Windows\temp izinlerini değiştirilmemiştir emin olun.
6. Sanal makinede yükseltilmiş bir PowerShell penceresinde aşağıdakileri yazarak Microsoft Monitoring Agent durumunu görüntüleme `  (New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg').GetCloudWorkspaces() | Format-List`
7. Microsoft Monitoring Agent Kurulum günlük dosyalarını gözden geçirin `C:\Windows\System32\config\systemprofile\AppData\Local\SCOM\Logs`

Daha fazla bilgi için [Windows uzantı sorunlarını giderme](../../virtual-machines/extensions/oms-windows.md).

## <a name="troubleshooting-linux-vm-extension"></a>Linux VM uzantı sorunlarını giderme
[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)] 
Varsa *Linux için Log Analytics aracısını* VM uzantısı yükleme değil veya raporlama, sorunu gidermek için aşağıdaki adımları gerçekleştirebilirsiniz.

1. Uzantı durumunun ise *bilinmeyen* VM aracı günlük dosyasını gözden geçirerek doğru şekilde çalıştığına ve Azure VM aracısı yüklü olup olmadığını denetleyin `/var/log/waagent.log`
   * Günlük yoksa, VM aracısı yüklü değil.
   * [Linux Vm'lerinde Azure VM aracısını yükleyin](../../azure-monitor/learn/quick-collect-azurevm.md#enable-the-log-analytics-vm-extension)
2. Linux VM uzantısı günlük dosyaları için iyi durumda olmayan diğer durumlar için Log Analytics aracısını gözden `/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/extension.log` ve `/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/CommandExecution.log`
3. Uzantı durumunun sağlıklı olduğundan, ancak veri değil karşıya yükleniyor Linux günlük dosyalarında için Log Analytics aracısını gözden geçirin. `/var/opt/microsoft/omsagent/log/omsagent.log`

Daha fazla bilgi için [Linux uzantı sorunlarını giderme](../../virtual-machines/extensions/oms-linux.md).

## <a name="next-steps"></a>Sonraki adımlar

Azure'un dışındaki bilgisayarlarda barındırılan Linux için Log Analytics aracısını ilgili ek sorun giderme kılavuzu için bkz. [Azure Log Analytics Linux Aracısı sorunlarını giderme](agent-linux-troubleshoot.md).  
