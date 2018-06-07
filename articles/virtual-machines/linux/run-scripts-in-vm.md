---
title: Bir Azure Linux VM'de komut dosyalarını çalıştır
description: Bu konu, bir sanal makine içinde komut dosyalarını çalıştırmak açıklar
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 05/02/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: 209b476e21a3a9ba08eb907a279c7c5f8faa1c17
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34661125"
---
# <a name="run-scripts-in-your-linux-vm"></a>Linux VM ile komut dosyalarını çalıştır

Görevleri otomatikleştirmenize ve sorunlarını gidermek için bir VM'de komutları çalıştırmanız gerekebilir. Aşağıdaki makalede komut dosyaları ve Vm'lerinizi içindeki komutları çalıştırmak kullanılabilen özellikleri kısa bir genel bakış sunulmaktadır.

## <a name="custom-script-extension"></a>Özel Betik Uzantısı

[Özel betik uzantısının](../extensions/custom-script-linux.md) post dağıtım yapılandırması ve yazılım yükleme için öncelikle kullanılır.

* İndirin ve Azure sanal makinelerinde betikleri çalıştırın.
* Azure Resource Manager şablonları, Azure CLI, REST API'si, PowerShell veya Azure portalı kullanılarak çalıştırılabilir.
* Komut dosyası Azure depolama veya GitHub indirilen veya Azure portalından çalıştırdığınızda PC'nizden sağlanan.
* Windows makinelerinizde PowerShell komut dosyasını çalıştırın ve komut dosyası Linux makinelerde Bash.
* Dağıtım yapılandırmaları, yazılım yükleme ve diğer yapılandırma veya yönetim görevleri için kullanışlıdır.

## <a name="run-command"></a>Komutu çalıştırın

[Komutu Çalıştır](run-command.md) sanal makine ve uygulama yönetimi ve komut dosyalarını kullanarak sorun giderme özelliği sağlar ve hatta makine olduğunu değil ağ olduğunda kullanılabilir bağlıdır.

* Komut dosyalarını Azure sanal makinelerinde çalıştırın.
* Kullanılarak çalıştırılabilir [Azure portal](run-command.md), [REST API](/rest/api/compute/virtual%20machines%20run%20commands/runcommand), [Azure CLI](/cli/azure/vm/run-command?view=azure-cli-latest#az-vm-run-command-invoke), veya [PowerShell](/powershell/module/azurerm.compute/invoke-azurermvmruncommand)
* Hızlı bir şekilde bir komut dosyası ve görünüm çıkış çalıştırın ve Azure portalında gerektiği kadar yineleyin.
* Komut dosyası doğrudan yazılabilir veya yerleşik komut dosyalarından birini çalıştırabilirsiniz.
* Windows makinelerinizde PowerShell komut dosyasını çalıştırın ve komut dosyası Linux makinelerde Bash.
* Sanal makine ve uygulama yönetimi ve komut dosyaları, ağa bağlı olmayan sanal makinelerde çalışan için kullanışlıdır.

## <a name="hybrid-runbook-worker"></a>Karma Runbook Çalışanı

[Karma Runbook çalışanı](../../automation/automation-hybrid-runbook-worker.md) genel makine, uygulama ve ortam yönetimi Automation hesabında depolanan kullanıcının özel komut dosyaları sağlar.

* Komut dosyaları, Azure ve Azure dışı makineleri çalışır.
* Azure portal, Azure CLI, REST API'si, PowerShell, Web kancası kullanılarak çalıştırılabilir.
* Komut dosyaları saklanır ve Automation hesabında yönetilir.
* PowerShell, PowerShell iş akışı, Python veya grafik runbook'ları çalıştırın
* Komut dosyası çalıştırma zamanı sınırlama yoktur.
* Birden fazla komut dosyası birlikte çalışabilir.
* Tam komut dosyası çıkışını döndürülen ve saklanır.
* İş Geçmişi 90 gün boyunca kullanılabilir.
* Kullanıcı tarafından sağlanan kimlik bilgileriyle veya yerel sistem olarak çalıştırabilir.
* Gerektirir [el ile yükleme](../../automation/automation-windows-hrw-install.md)

## <a name="serial-console"></a>Seri konsol

[Seri konsol](serial-console.md) VM, VM'ye bağlı bir klavye zorunda benzer doğrudan erişim sağlar.

* Azure sanal makinelerde çalıştırma komutları.
* Azure portalında makineye metin tabanlı bir konsol kullanarak çalıştırılabilir.
* Makine bir yerel kullanıcı hesabıyla oturum açın.
* Makinenin ağ veya işletim sistemi durumu bağımsız olarak sanal makineye erişim gerektiğinde kullanışlıdır.

## <a name="next-steps"></a>Sonraki adımlar

Komut dosyaları ve Vm'lerinizi içindeki komutları çalıştırmak kullanılabilen farklı özellikler hakkında daha fazla bilgi edinin.

* [Özel Betik uzantısı](../extensions/custom-script-linux.md)
* [Komutu çalıştırın](run-command.md)
* [Karma Runbook çalışanı](../../automation/automation-hybrid-runbook-worker.md)
* [Seri konsol](serial-console.md)