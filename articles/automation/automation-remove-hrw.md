---
title: "Azure Otomasyon karma Runbook çalışanları kaldırma | Microsoft Docs"
description: "Bu makalede, dağıtılan Azure Otomasyon karma Runbook runbook'lar yerel veri merkezinde veya Bulut ortamınızdaki bilgisayarlarda çalışmasına izin veren çalışanları yönetme hakkında bilgi sağlar."
services: automation
documentationcenter: 
author: georgewallace
manager: carmonm
editor: tysonn
ms.assetid: 
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/07/2017
ms.author: magoedte
ms.openlocfilehash: 602b868073da6bd1f64099c0f344c9b492abaff0
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="remove-azure-automation-hybrid-runbook-workers"></a>Azure Otomasyonu Karma Runbook Çalışanlarını kaldırma

Azure Otomasyon karma Runbook çalışanı özelliği runbook'ları doğrudan rolünü barındıran bilgisayarda ve bu yerel kaynakları yönetmek için ortamında kaynaklara karşı çalıştırmanıza olanak sağlar. Bu makalede bir karma çalışanı bir şirket içi bilgisayardan kaldırmak için adımları alır.

## <a name="removing-hybrid-runbook-worker"></a>Karma Runbook çalışanı kaldırma

Bir veya daha fazla karma Runbook çalışanları bir gruptan kaldırdığınızda veya grup gereksinimlerinize bağlı olarak kaldırabilirsiniz.  Bir karma Runbook çalışanı bir şirket içi bilgisayardan kaldırmak için aşağıdaki adımları gerçekleştirin.

1. Azure Portal'da, Automation hesabınızı gidin.  
2. Gelen **ayarları** dikey penceresinde, select **anahtarları** ve alan değerlerini Not **URL** ve **birincil erişim anahtarını**.  Bu bilgiler sonraki adımda gerekir.
3. Yönetici modunda bir PowerShell oturumu açın ve şu komutu çalıştırın `Remove-HybridRunbookWorker -url <URL> -key <PrimaryAccessKey>`.  Kullanım **-Verbose** geçiş kaldırma işleminin ayrıntılı günlüğü için.

> [!NOTE]
> Bu Microsoft İzleme Aracısı bilgisayardan, yalnızca işlevsellik ve karma Runbook çalışanı rolü yapılandırmasını kaldırmaz.  

## <a name="remove-hybrid-worker-groups"></a>Karma çalışan grupları kaldırın

Bir grubu kaldırmak için önce bir karma Runbook çalışanı daha önce gösterilen yordamı kullanarak grubunun bir üyesi olan her bilgisayarda kaldırmanız gerekir ve ardından grubunu kaldırmak için aşağıdaki adımları gerçekleştirin.  

1. Azure Portal'da Automation hesabını açın.
1. Altında **işlem Otomasyonu** seçin **karma çalışan grupları**. Silmek istediğiniz grubu seçin.  Belirli grup seçtikten sonra **karma çalışanı grubu** özellikleri dikey penceresi görüntülenir.<br> ![Karma Runbook çalışan grubu dikey penceresi](media/automation-hybrid-runbook-worker/automation-hybrid-runbook-worker-group-properties.png)   
2. Özellikleri dikey penceresinde seçili grubun tıklatın **silmek**.  Bu eylemi onaylamanızı isteyen bir ileti görüntülenir seçin **Evet** devam etmek istediğinizden eminseniz.<br> ![Grubu Sil onay iletişim kutusu](media/automation-hybrid-runbook-worker/automation-hybrid-runbook-worker-confirm-delete.png)<br> Bu işlemin tamamlanması birkaç saniye alabilir ve ilerleme durumunu menüdeki **Bildirimler**’in altından izleyebilirsiniz. 

## <a name="next-steps"></a>Sonraki Adımlar

* Gözden geçirme [runbook'lar bir karma Runbook çalışanı üzerinde çalışacak](automation-hrw-run-runbooks.md) runbook'larınızın, şirket içi veri merkezi ya da diğer bulut ortamı süreçlerini otomatikleştirmek için yapılandırma hakkında bilgi edinmek için.
* Windows karma Runbook çalışanları yükleme hakkında daha fazla bilgi için bkz: [Azure Automation Windows karma Runbook çalışanı](automation-windows-hrw-install.md).
* Linux karma Runbook çalışanları yükleme hakkında daha fazla bilgi için bkz: [Azure Otomasyon Linux karma Runbook çalışanı](automation-linux-hrw-install.md).