---
title: Bir Azure Linux VM'de betikleri çalıştırma
description: Bu konuda bir sanal makine içinde betikleri çalıştırma işlemi açıklanır
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 05/02/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: 3881a6d0ee1139fa481908f09b6e96efda24a5cd
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60542552"
---
# <a name="run-scripts-in-your-linux-vm"></a>Linux VM'nizi betikleri çalıştırma

Görevleri otomatikleştirmenize ve sorunlarını gidermek için VM ile komutları çalıştırmanız gerekebilir. Aşağıdaki makalede betikleri ve komutları sanal makinelerinizin içinde çalıştırmak kullanılabilir olan özelliklerin kısa bir genel bakış sağlar.

## <a name="custom-script-extension"></a>Özel Betik Uzantısı

[Özel betik uzantısı](../extensions/custom-script-linux.md) post dağıtım yapılandırması ve yazılım yüklemesi için öncelikli olarak kullanılır.

* İndirin ve Azure sanal makineler'de betikleri çalıştırın.
* Azure Resource Manager şablonları, Azure CLI, REST API, PowerShell veya Azure portalı kullanılarak çalıştırılabilir.
* Komut dosyalarını bir veri kaynağıyla Azure depolama veya GitHub konumlarından indirilebilir ya da PC'nizde Azure portalından çalıştırdığınızda iletilebilir.
* Windows makinelerinde PowerShell betiğini çalıştırın ve Bash betiği Linux makinelerinde.
* Dağıtım sonrası yapılandırma, yazılım yükleme ve diğer yapılandırma veya yönetim görevleri için kullanışlıdır.

## <a name="run-command"></a>Komutu çalıştırın

[Komutu Çalıştır](run-command.md) özellik sanal makine ve uygulama yönetimi ve betikleri kullanarak sorun giderme sağlar ve hatta makine Konuk güvenlik duvarı RDP veya SSH bağlantı noktası yoksa, örneğin ulaşılabilir olmadığında kullanılabilir açın.

* Betikleri Azure sanal makinelerde çalıştırır.
* Kullanılarak çalıştırılabilir [Azure portalında](run-command.md), [REST API](/rest/api/compute/virtual%20machines%20run%20commands/runcommand), [Azure CLI](/cli/azure/vm/run-command?view=azure-cli-latest#az-vm-run-command-invoke), veya [PowerShell](https://docs.microsoft.com/powershell/module/az.compute/invoke-azvmruncommand)
* Hızlı bir şekilde bir betik ve görünüm çıkış çalıştırın ve Azure portalında gerektiği şekilde yineleyin.
* Doğrudan komut dosyası yazılabilir veya yerleşik betiklerinden birini çalıştırabilirsiniz.
* Windows makinelerinde PowerShell betiğini çalıştırın ve Bash betiği Linux makinelerinde.
* Sanal makine ve uygulama yönetimi ve sanal makineler'de ulaşılamıyor betikleri çalıştırmak için kullanışlıdır.

## <a name="hybrid-runbook-worker"></a>Karma Runbook Çalışanı

[Karma Runbook çalışanı](../../automation/automation-hybrid-runbook-worker.md) kullanıcının özel betiklerle, Otomasyon hesabında depolanan genel makine, uygulama ve ortam yönetimi sağlar.

* Azure ve Azure olmayan makineler betikleri çalıştırabilir.
* Azure portalı, Azure CLI, REST API, PowerShell, Web kancası kullanarak çalıştırılabilir.
* Betikleri depolanan ve yönetilen bir Otomasyon hesabı.
* PowerShell, PowerShell iş akışı, Python veya grafik runbook'ları çalıştırma
* Betik çalıştırma zamanı sınırlama yoktur.
* Birden çok komut dosyaları eşzamanlı olarak çalışabilir.
* Tam betik çıktısı döndürdü ve depolanır.
* İş Geçmişi 90 gün boyunca kullanılabilir.
* Betikleri yerel sistem veya kullanıcı tarafından sağlanan kimlik bilgileriyle çalıştırabilirsiniz.
* Gerektirir [el ile yükleme](../../automation/automation-windows-hrw-install.md)

## <a name="serial-console"></a>Seri konsol

[Seri konsol](serial-console.md) VM'ye bağlı bir klavye olması için benzer bir VM'ye doğrudan erişim sağlar.

* Azure sanal makineler'de komutlar çalıştırın.
* Azure portalında makineye metin tabanlı bir konsol kullanarak çalıştırılabilir.
* Makine bir yerel kullanıcı hesabı ile oturum açın.
* Makinenin ağ veya işletim sistemi durumuna bakılmaksızın sanal makineye erişim gerektiğinde yararlıdır.

## <a name="next-steps"></a>Sonraki adımlar

Betikleri ve komutları sanal makinelerinizin içinde çalıştırmak kullanılabilen farklı özellikler hakkında daha fazla bilgi edinin.

* [Özel Betik uzantısı](../extensions/custom-script-linux.md)
* [Run Command](run-command.md)
* [Karma Runbook çalışanı](../../automation/automation-hybrid-runbook-worker.md)
* [Seri konsol](serial-console.md)