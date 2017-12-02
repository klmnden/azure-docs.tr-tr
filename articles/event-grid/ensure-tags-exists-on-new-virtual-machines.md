---
title: "Olay kılavuzla Azure Automation tümleştirme | Microsoft Docs"
description: "Yeni VM oluşturulduğunda otomatik olarak bir etiket eklemeyi öğrenin ve Microsoft Teams bir bildirim gönderebilirsiniz."
keywords: "Otomasyon runbook, takımları, olay kılavuz, sanal makine, VM"
services: automation
documentationcenter: 
author: eamonoreilly
manager: 
editor: 
ms.assetid: 0dd95270-761f-448e-af48-c8b1e82cd821
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/28/2017
ms.author: eamono
ms.openlocfilehash: 6798f98755ad1d70d316b074643700f7b3e25ee7
ms.sourcegitcommit: 80eb8523913fc7c5f876ab9afde506f39d17b5a1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2017
---
# <a name="integrating-azure-automation-with-event-grid-and-microsoft-teams"></a>Azure Otomasyonu olay kılavuz ve Microsoft ekipleri ile tümleştirme

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Olay kılavuz örnek runbook içeri aktarın.
> * İsteğe bağlı bir ekip Web kancası oluşturun.
> * Runbook için bir Web kancası oluşturun.
> * Bir olay kılavuz abonelik oluşturun.
> * Runbook'u tetikleyen VM oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gereklidir.
+ [Otomasyon hesabı](../automation/automation-offering-get-started.md) olay kılavuz abonelikten tetiklenen runbook'u tutacak.

## <a name="import-event-grid-sample-runbook"></a>Olay kılavuz örnek runbook'u İçeri Aktar
1.  Automation hesabını açın ve runbook'ları sayfasında'yi tıklatın.
2.  "Gözat Galerisi" düğmesine tıklayın.
![Runbook listeden kullanıcı Arabirimi](media/ensure-tags-exists-on-new-virtual-machines/event-grid-runbook-list.png)
3.  "Olay ızgara" için arama ve runbook Otomasyon dikkate alın.
![Galeri runbook'u İçeri Aktar](media/ensure-tags-exists-on-new-virtual-machines/gallery-event-grid.png)
4.  Runbook kaynağı görüntüleyip "Yayımla" düğmesine tıklayın "Düzenle"'i tıklatın.
![UI runbook'tan yayımlama](media/ensure-tags-exists-on-new-virtual-machines/publish-runbook.png)

## <a name="create-an-optional-teams-webhook"></a>İsteğe bağlı bir ekip Web kancası oluşturma
1.  Microsoft Teams diğer seçenekler (...) ve kanal adının yanındaki seçin ve bağlayıcıları seçin.
![Takımlar bağlantıları](media/ensure-tags-exists-on-new-virtual-machines/teams-webhook.png)
2.  Gelen Web kancası bağlayıcılar listesi boyunca kaydırma yapın ve Ekle'yi tıklatın.
![Takımlar Web kancası bağlantı](media/ensure-tags-exists-on-new-virtual-machines/select-teams-webhook.png)
3.  AzureAutomationIntegration için bir ad girin ve Oluştur'u tıklatın.
![Takımlar Web kancası](media/ensure-tags-exists-on-new-virtual-machines/configure-teams-webhook.png)
4.  Web kancası panoya kopyalayın ve kaydedin. Web kancası URL'si, Microsoft Teams bilgileri göndermek için kullanılır.
5.  Web kancası kaydetmek için yapılan seçin.

## <a name="create-a-webhook-for-the-runbook"></a>Runbook için bir Web kancası oluşturma
1.  Gözcü VMWrite runbook'u açın.
2.  Web kancası ve Ekle Web kancası düğmesini tıklatın ![Web kancası oluşturma](media/ensure-tags-exists-on-new-virtual-machines/add-webhook.png)
2.  "WatchVMEventGrid" için bir ad girin ve URL'yi panoya kopyalayın ve kaydedin.
![Web kancası adı yapılandırma](media/ensure-tags-exists-on-new-virtual-machines/configure-webhook-name.png)
3.  Parametreleri seçin ve Microsoft Teams Web kancası URL'si girin ve WEBHOOKDATA boş bırakın.
![Web kancası parametreleri yapılandırın](media/ensure-tags-exists-on-new-virtual-machines/configure-webhook-parameters.png)
4.  Otomasyon runbook Web kancası oluşturmak için Tamam'ı seçin.

## <a name="create-an-event-grid-subscription"></a>Bir olay kılavuz Abonelik Oluştur
1.  Automation hesabına genel bakış olay kılavuz sayfasından tıklayın.
![Olay kılavuz listesi](media/ensure-tags-exists-on-new-virtual-machines/event-grid-list.png)
2.  Yeni olay abonelik düğmesine tıklayın.
3.  Abonelik aşağıdaki bilgilerle yapılandırın:
    *   AzureAutomation adını girin. 
    *   Konu Türü'nde, Azure abonelikleri seçin.
    *   "Abone ol tüm olay türleri" seçeneğinin işaretini kaldırın
    *   Kaynak yazma başarı olay türlerini seçin.
    *   Abone uç izleme VMWrite runbook için Web kancası URL'si girin.
    *   Önek filtrede abonelik ve oluşturulan yeni VM'ler için aramak istediğiniz kaynak grubunu girin. /Subscriptions/124aa551-849d-46e4-a6dc-0bc4895422aB/resourcegroups/ContosoResourceGroup/providers/Microsoft.Compute/virtualMachines gibi görünmelidir ![olay kılavuz listesi](media/ensure-tags-exists-on-new-virtual-machines/configure-event-grid-subscription.png)
6.  Olay kılavuz aboneliği kaydetmek için "Oluştur" seçeneğini tıklatın.

## <a name="create-vm-that-triggers-runbook"></a>Runbook'u tetikleyen VM oluşturma
1.  Kılavuz abonelik önek olay filtresi belirttiğiniz kaynak grubunda yeni bir sanal makine oluşturun.
2.  Gözcü VMWrite runbook çağrılmalıdır ve yeni bir etiket VM'ye eklenir.
![VMTag](media/ensure-tags-exists-on-new-virtual-machines/vm-tag.png)
3.  Yeni bir ileti takımlar kanala gönderilir.

![Takımlar bildirim](media/ensure-tags-exists-on-new-virtual-machines/teams-vm-message.png)

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, olay kılavuz ve Otomasyon arasındaki tümleştirme ayarlayın. Şunları öğrendiniz:

> [!div class="checklist"]
> * Olay kılavuz örnek runbook içeri aktarın.
> * İsteğe bağlı bir ekip Web kancası oluşturun.
> * Runbook için bir Web kancası oluşturun.
> * Bir olay kılavuz abonelik oluşturun.
> * Runbook'u tetikleyen VM oluşturun.

> [!div class="nextstepaction"]
> [Oluşturma ve rota olay kılavuzuna özel olaylar](../event-grid/custom-event-quickstart.md)
