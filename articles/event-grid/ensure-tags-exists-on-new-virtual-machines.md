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
ms.openlocfilehash: 8b698659ed91782b80dbefbfea02aa036c09210d
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2017
---
# <a name="integrate-azure-automation-with-event-grid-and-microsoft-teams"></a>Azure Otomasyonu olay kılavuz ve Microsoft ekipleri ile tümleştirme

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir olay kılavuz örnek runbook içeri aktarın.
> * İsteğe bağlı bir Microsoft Teams Web kancası oluşturun.
> * Runbook için bir Web kancası oluşturun.
> * Bir olay kılavuz abonelik oluşturun.
> * Runbook'u tetikleyen bir VM oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için bir [Azure Automation hesabını](../automation/automation-offering-get-started.md) Azure olay kılavuz abonelikten tetiklenen runbook'u tutacak için gereklidir.

## <a name="import-an-event-grid-sample-runbook"></a>Bir olay kılavuz örnek runbook'u İçeri Aktar
1. Seçin **Automation hesapları**seçip **Runbook'lar** sayfası.

2. Seçin **Gözat galeri** düğmesi.

    ![Runbook listeden kullanıcı Arabirimi](media/ensure-tags-exists-on-new-virtual-machines/event-grid-runbook-list.png)

3. Arama **olay kılavuz**ve runbook Otomasyon dikkate alın.

    ![Galeri runbook'u İçeri Aktar](media/ensure-tags-exists-on-new-virtual-machines/gallery-event-grid.png)

4. Seçin **Düzenle** runbook kaynağı görüntülemek ve seçmek için **Yayımla** düğmesi.

    ![UI runbook'tan yayımlama](media/ensure-tags-exists-on-new-virtual-machines/publish-runbook.png)

## <a name="create-an-optional-microsoft-teams-webhook"></a>İsteğe bağlı bir Microsoft Teams Web kancası oluşturma
1. Microsoft Teams seçin **diğer seçenekler** sonraki kanal adı ve ardından **Bağlayıcılar**.

    ![Microsoft Teams bağlantıları](media/ensure-tags-exists-on-new-virtual-machines/teams-webhook.png)

2. Bağlayıcılar listesini kaydırın **gelen Web kancası**seçip **Ekle**.

    ![Microsoft Teams Web kancası bağlantı](media/ensure-tags-exists-on-new-virtual-machines/select-teams-webhook.png)

3. Girin **AzureAutomationIntegration** adı ve select **oluşturma**.

    ![Microsoft Teams Web kancası](media/ensure-tags-exists-on-new-virtual-machines/configure-teams-webhook.png)

4. Web kancası panoya kopyalayın ve kaydedin. Web kancası URL'si, Microsoft Teams bilgileri göndermek için kullanılır.

5. Seçin **Bitti** Web kancası kaydetmek için.

## <a name="create-a-webhook-for-the-runbook"></a>Runbook için bir Web kancası oluşturma
1. Gözcü VMWrite runbook'u açın.

2. Seçin **Kancalarını**seçip **ekleme Web kancası** düğmesi.

    ![Web kancası oluştur](media/ensure-tags-exists-on-new-virtual-machines/add-webhook.png)

3. Girin **WatchVMEventGrid** adı. URL'yi panoya kopyalayın ve dosyayı kaydedin.

    ![Web kancası adı yapılandırma](media/ensure-tags-exists-on-new-virtual-machines/configure-webhook-name.png)

4. Seçin **parametreler ve çalıştırma ayarları**ve Microsoft Teams Web kancası URL'si girin. Bırakın **WEBHOOKDATA** boş.

    ![Web kancası parametreleri yapılandırın](media/ensure-tags-exists-on-new-virtual-machines/configure-webhook-parameters.png)

5. Seçin **Tamam** Otomasyon runbook Web kancası oluşturulamadı.


## <a name="create-an-event-grid-subscription"></a>Bir olay kılavuz Abonelik Oluştur
1. Üzerinde **Otomasyon hesabı** genel bakış sayfasında, **olay kılavuz**.

    ![Olay kılavuz listesi](media/ensure-tags-exists-on-new-virtual-machines/event-grid-list.png)

2. Seçin **olay aboneliği** düğmesi.

3. Abonelik aşağıdaki bilgilerle yapılandırın:

    *   Girin **AzureAutomation** adı. 
    *   İçinde **konu türü**seçin **Azure abonelikleri**.
    *   Clear **abone olmak için tüm olay türleri** onay kutusu.
    *   İçinde **olay türleri**seçin **kaynak yazma başarı**.
    *   İçinde **tam uç nokta URL'si**, izleme VMWrite runbook için Web kancası URL'si girin.
    *   İçinde **önek filtre**, abonelik girin ve yeni VM'ler için aramak istediğiniz kaynak grubu oluşturduk. /Subscriptions/124aa551-849d-46e4-a6dc-0bc4895422aB/resourcegroups/ContosoResourceGroup/providers/Microsoft.Compute/virtualMachines gibi görünmelidir

    ![Olay kılavuz listesi](media/ensure-tags-exists-on-new-virtual-machines/configure-event-grid-subscription.png)

4. Seçin **oluşturma** olay kılavuz abonelik kaydetmek için.

## <a name="create-a-vm-that-triggers-the-runbook"></a>Runbook'u tetikleyen bir VM oluşturma
1. Kılavuz abonelik önek olay filtresi belirttiğiniz kaynak grubunda yeni bir VM oluşturun.

2. Gözcü VMWrite runbook çağrılmalıdır ve yeni bir etiket VM'ye eklenir.

    ![VM etiketi](media/ensure-tags-exists-on-new-virtual-machines/vm-tag.png)

3. Yeni bir ileti Microsoft Teams kanala gönderilir.

    ![Microsoft Teams bildirim](media/ensure-tags-exists-on-new-virtual-machines/teams-vm-message.png)

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, olay kılavuz ve Otomasyon arasındaki tümleştirme ayarlayın. Şunları öğrendiniz:

> [!div class="checklist"]
> * Bir olay kılavuz örnek runbook içeri aktarın.
> * İsteğe bağlı bir Microsoft Teams Web kancası oluşturun.
> * Runbook için bir Web kancası oluşturun.
> * Bir olay kılavuz abonelik oluşturun.
> * Runbook'u tetikleyen bir VM oluşturun.

> [!div class="nextstepaction"]
> [Oluşturma ve rota olay kılavuzuna özel olaylar](../event-grid/custom-event-quickstart.md)
