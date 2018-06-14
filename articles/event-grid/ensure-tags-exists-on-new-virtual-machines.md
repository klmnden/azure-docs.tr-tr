---
title: Olay kılavuzla Azure Automation tümleştirme | Microsoft Docs
description: Yeni VM oluşturulduğunda otomatik olarak bir etiket eklemeyi öğrenin ve Microsoft Teams bir bildirim gönderebilirsiniz.
keywords: Otomasyon runbook, takımları, olay kılavuz, sanal makine, VM
services: automation
documentationcenter: ''
author: eamonoreilly
manager: ''
editor: ''
ms.service: automation
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/06/2017
ms.author: eamono
ms.openlocfilehash: 9a4d6ecf19fc96a9c7b92cf246effbf3948fb478
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2017
ms.locfileid: "26349079"
---
# <a name="integrate-azure-automation-with-event-grid-and-microsoft-teams"></a>Azure Otomasyonu olay kılavuz ve Microsoft ekipleri ile tümleştirme

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir olay kılavuz örnek runbook içeri aktarın.
> * İsteğe bağlı bir Microsoft Teams Web kancası oluşturun.
> * Runbook için bir Web kancası oluşturun.
> * Bir olay kılavuz abonelik oluşturun.
> * Runbook'u tetikleyen bir VM oluşturun.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için bir [Azure Automation hesabını](../automation/automation-offering-get-started.md) Azure olay kılavuz abonelikten tetiklenen runbook'u tutacak için gereklidir.

## <a name="import-an-event-grid-sample-runbook"></a>Bir olay kılavuz örnek runbook'u İçeri Aktar
1. Otomasyon hesabınızı seçin ve Seç **Runbook'lar** sayfası.

   ![Runbook'ları seçin](./media/ensure-tags-exists-on-new-virtual-machines/select-runbooks.png)

2. Seçin **Gözat galeri** düğmesi.

3. Arama **olay kılavuz**seçip **olay kılavuz Azure Automation'da tümleştirme**. 

    ![Galeri runbook'u İçeri Aktar](media/ensure-tags-exists-on-new-virtual-machines/gallery-event-grid.png)

4. Seçin **alma** ve adlandırın **izleme VMWrite**.

5. İçe sonra seçin **Düzenle** runbook kaynağı görüntülemek için. **Yayımla** düğmesini seçin.

## <a name="create-an-optional-microsoft-teams-webhook"></a>İsteğe bağlı bir Microsoft Teams Web kancası oluşturma
1. Microsoft Teams seçin **diğer seçenekler** sonraki kanal adı ve ardından **Bağlayıcılar**.

    ![Microsoft Teams bağlantıları](media/ensure-tags-exists-on-new-virtual-machines/teams-webhook.png)

2. Bağlayıcılar listesini kaydırın **gelen Web kancası**seçip **Ekle**.

3. Girin **AzureAutomationIntegration** adı ve select **oluşturma**.

4. Web kancası panoya kopyalayın ve kaydedin. Web kancası URL'si, Microsoft Teams bilgileri göndermek için kullanılır.

5. Seçin **Bitti** Web kancası kaydetmek için.

## <a name="create-a-webhook-for-the-runbook"></a>Runbook için bir Web kancası oluşturma
1. Gözcü VMWrite runbook'u açın.

2. Seçin **Kancalarını**seçip **ekleme Web kancası** düğmesi.

3. Girin **WatchVMEventGrid** adı. URL'yi panoya kopyalayın ve dosyayı kaydedin.

    ![Web kancası adı yapılandırma](media/ensure-tags-exists-on-new-virtual-machines/copy-url.png)

4. Seçin **parametreleri yapılandırmak ve çalıştırma ayarlarını**ve Microsoft Teams Web kancası URL'si girin **CHANNELURL**. Bırakın **WEBHOOKDATA** boş.

    ![Web kancası parametreleri yapılandırın](media/ensure-tags-exists-on-new-virtual-machines/configure-webhook-parameters.png)

5. Seçin **Tamam** Otomasyon runbook Web kancası oluşturulamadı.


## <a name="create-an-event-grid-subscription"></a>Bir olay kılavuz Abonelik Oluştur
1. Üzerinde **Otomasyon hesabı** genel bakış sayfasında, **olay kılavuz**.

    ![Olay kılavuz seçin](media/ensure-tags-exists-on-new-virtual-machines/select-event-grid.png)

2. Seçin **+ olay aboneliği** düğmesi.

3. Abonelik aşağıdaki bilgilerle yapılandırın:

    *   Girin **AzureAutomation** adı.
    *   İçinde **konu türü**seçin **Azure abonelikleri**.
    *   Clear **abone olmak için tüm olay türleri** onay kutusu.
    *   İçinde **olay türleri**seçin **kaynak yazma başarı**.
    *   İçinde **abone Endpoint**, izleme VMWrite runbook için Web kancası URL'si girin.
    *   İçinde **önek filtre**, abonelik girin ve yeni VM'ler için aramak istediğiniz kaynak grubu oluşturduk. Gibi görünmelidir:`/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.Compute/virtualMachines`

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
