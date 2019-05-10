---
title: Azure Otomasyonu’nu Event Grid ile tümleştirme | Microsoft Docs
description: Yeni bir VM oluşturulduğunda otomatik olarak etiket oluşturmayı ve Microsoft Teams'e bildirim göndermeyi öğrenin.
keywords: otomasyon, runbook, ekipler, event grid, sanal makine, VM
services: automation
author: eamonoreilly
manager: ''
ms.service: automation
ms.topic: tutorial
ms.workload: infrastructure-services
ms.date: 05/10/2019
ms.author: eamono
ms.openlocfilehash: 9f99ce5862850c2453e9e72241fff77fe091616f
ms.sourcegitcommit: bb85a238f7dbe1ef2b1acf1b6d368d2abdc89f10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2019
ms.locfileid: "65521422"
---
# <a name="tutorial-integrate-azure-automation-with-event-grid-and-microsoft-teams"></a>Öğretici: Azure Otomasyonu’nu Event Grid ve Microsoft Teams ile tümleştirme

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Event Grid örnek runbook'unu içeri aktarma.
> * İsteğe bağlı bir Microsoft Teams web kancası oluşturma.
> * Runbook için web kancası oluşturma.
> * Event Grid aboneliği oluşturun.
> * Runbook'u tetikleyen bir VM oluşturma.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [requires-azurerm](../../includes/requires-azurerm.md)]

Bu öğreticiyi tamamlamak için, bir [Azure Otomasyonu hesabının](../automation/automation-offering-get-started.md) Azure Event Grid aboneliğinden tetiklenen runbook'u barındırması gerekir.

* Otomasyon Hesabınıza `AzureRM.Tags` modülü yüklenmelidir. Modülleri Azure Otomasyonu'na aktarmayı öğrenmek için bkz. [Modülleri Azure Otomasyonu'na aktarma](../automation/automation-update-azure-modules.md).

## <a name="import-an-event-grid-sample-runbook"></a>Event Grid örnek runbook'unu içeri aktarma

1. Otomasyon hesabınızı seçin ve sonra da **Runbook'lar** sayfasını seçin.

   ![Runbook'ları seçme](./media/ensure-tags-exists-on-new-virtual-machines/select-runbooks.png)

2. **Galeriye gözat** düğmesini seçin.

3. **Event Grid** için arama yapın ve **Azure Otomasyonu'nu Event Grid ile tümleştirme** öğesini seçin.

    ![Runbook galerisini içeri aktarma](media/ensure-tags-exists-on-new-virtual-machines/gallery-event-grid.png)

4. **İçeri Aktar**'ı seçin ve bunu **Watch-VMWrite** olarak adlandırın.

5. İçeri aktarıldıktan sonra, runbook kaynağını görüntülemek için **Düzenle**'yi seçin. 
6. 74 kullanmak için komut satırı güncelleştiririz `Tag` yerine `Tags`.

    ```powershell
    Update-AzureRmVM -ResourceGroupName $VMResourceGroup -VM $VM -Tag $Tag | Write-Verbose
    ```
7. **Yayımla** düğmesini seçin.

## <a name="create-an-optional-microsoft-teams-webhook"></a>İsteğe bağlı bir Microsoft Teams web kancası oluşturma

1. Microsoft Teams'de, kanal adının yanındaki **Diğer Seçenekler**'i ve sonra da **Bağlayıcılar**'ı seçin.

    ![Microsoft Teams bağlantıları](media/ensure-tags-exists-on-new-virtual-machines/teams-webhook.png)

2. Bağlayıcılar listesini **Gelen Web Kancası**'na kadar kaydırın ve **Ekle**'yi seçin.

3. Ad olarak **AzureAutomationIntegration** girin ve **Oluştur**'u seçin.

4. Web kancası URL'sini panoya kopyalayın ve kaydedin. Web kancası URL'si, Microsoft Teams'e bilgi göndermek için kullanılır.

5. **Bitti**'yi seçerek web kancasını kaydedin.

## <a name="create-a-webhook-for-the-runbook"></a>Runbook için web kancası oluşturma

1. Watch-VMWrite runbook'unu açın.

2. **Web kancaları**'nı ve ardından **Web Kancası Ekle** düğmesini seçin.

3. Ad olarak **WatchVMEventGrid** girin. URL'yi panoya kopyalayın ve kaydedin.

    ![Web kancası adını yapılandırma](media/ensure-tags-exists-on-new-virtual-machines/copy-url.png)

4. **Parametreleri ve çalıştırma ayarlarını yapılandır**'ı seçin ve **CHANNELURL** olarak Microsoft Teams web kancası URL'sini girin. **WEBHOOKDATA** alanını boş bırakın.

    ![Web kancası parametrelerini yapılandırma](media/ensure-tags-exists-on-new-virtual-machines/configure-webhook-parameters.png)

5. Otomasyon runbook web kancasını oluşturmak için **Oluştur**'u seçin.

## <a name="create-an-event-grid-subscription"></a>Event Grid aboneliği oluşturma

1. **Otomasyon Hesabı** genel bakış sayfasında **Event Grid**'i seçin.

    ![Event Grid'i seçme](media/ensure-tags-exists-on-new-virtual-machines/select-event-grid.png)

2. **+ Olay Aboneliği**'ne tıklayın.

3. Aboneliği aşağıdaki bilgilerle yapılandırın:
    1. **Konu Başlığı Türü** için **Azure Abonelikleri**'ni seçin.
    2. **Tüm olay türlerine abone ol** onay kutusunun işaretini kaldırın.
    3. Ad olarak **AzureAutomation** girin.
    4. **Tanımlanan Olay Türleri** açılan menüsünde **Kaynak Yazma Başarısı** dışındaki tüm seçeneklerin işaretini kaldırın.

        > [!NOTE] 
        > Tüm Microsoft.Resources.ResourceWriteSuccess olaylar için Bu öğreticide, Azure aboneliğinizde uygulama hacmi yüksek olan çağrıları neden olabilir, böylece azure Resource Manager şu anda Create ve Update arasında ayrım yapmaz.
    1. **Uç Noktası Türü** için **Web kancası**'nı seçin.
    2. **Bir uç nokta seçin**'e tıklayın. Açılan **Web Kancası seçin** sayfasına Watch-VMWrite runbook'u için oluşturduğunuz web kancası URL'sini yapıştırın.
    3. **FİLTRELER** bölümünde, oluşturulan yeni VM'leri aramak istediğiniz aboneliği ve kaynak grubunu girin. Şu şekilde görünmelidir: `/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.Compute/virtualMachines`

4. Event Grid aboneliğini kaydetmek için **Oluştur**'u seçin.

## <a name="create-a-vm-that-triggers-the-runbook"></a>Runbook'u tetikleyen bir VM oluşturma

1. Event Grid aboneliği önek filtresinde belirttiğiniz kaynak grubunda yeni bir VM oluşturun.

2. Watch-VMWrite runbook çağrılmalı ve VM'ye yeni etiket eklenmelidir.

    ![VM etiketi](media/ensure-tags-exists-on-new-virtual-machines/vm-tag.png)

3. Microsoft Teams kanalına yeni bir ileti gönderilir.

    ![Microsoft Teams bildirimi](media/ensure-tags-exists-on-new-virtual-machines/teams-vm-message.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Event Grid ile Otomasyon arasında tümleştirme ayarladınız. Şunları öğrendiniz:

> [!div class="checklist"]
> * Event Grid örnek runbook'unu içeri aktarma.
> * İsteğe bağlı bir Microsoft Teams web kancası oluşturma.
> * Runbook için web kancası oluşturma.
> * Event Grid aboneliği oluşturun.
> * Runbook'u tetikleyen bir VM oluşturma.

> [!div class="nextstepaction"]
> [Event Grid ile özel olaylar oluşturma ve yönlendirme](../event-grid/custom-event-quickstart.md)
