---
title: Azure Automation hesabında bir izleyici görev oluşturma
description: Bir klasörde oluşturulan yeni dosyalarda izlemek için Azure Automation hesabındaki bir izleyici görev oluşturmayı öğrenin.
services: automation
ms.service: automation
ms.component: process-automation
author: eamonoreilly
ms.author: eamono
ms.topic: article
ms.date: 03/19/2017
ms.openlocfilehash: 2ae5fe2f2cd01eb9a0d7105c34c1dc216479720c
ms.sourcegitcommit: d28bba5fd49049ec7492e88f2519d7f42184e3a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2018
---
# <a name="create-an-azure-automation-watcher-tasks-to-track-file-changes-on-a-local-machine"></a>Bir Azure Otomasyonu izleyicisini yerel makinede dosya değişiklikleri izlemek için görevler oluşturma

Azure Otomasyonu İzleyici görevleri için olayları izlemenize ve eylemleri tetiklemek için kullanır. Bu öğretici, bir dizine yeni bir dosya eklendiğinde izlemek için bir izleyici görev oluşturmada size yol gösterir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir izleyici runbook'u İçeri Aktar
> * Bir Otomasyon değişkeni oluşturma
> * Bir eylem runbook oluşturma
> * Bir izleyici görev oluşturma
> * Tetikleyici bir İzleyicisi
> * Çıktıyı inceleyin.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gereklidir:

* Azure aboneliği. Henüz bir aboneliğiniz yoksa [MSDN abone avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) ya da [ücretsiz hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) için kaydolabilirsiniz.
* [Otomasyon hesabı](automation-offering-get-started.md) İzleyici ve eylem runbook'ları ve İzleyici görev tutmak için.
* A [karma runbook çalışanı](automation-hybrid-runbook-worker.md) İzleyici görevin çalıştığı.

## <a name="import-a-watcher-runbook"></a>Bir izleyici runbook'u İçeri Aktar

Bu öğretici olarak adlandırılan bir izleyici runbook kullanır **izleme NewFile** yeni dosyaları bir dizinde aramak için. İzleyici runbook bir klasördeki dosyaları için son bilinen yazma süresi alır ve bu Filigran yeni olan herhangi bir dosya bakar. Bu adımda, bu runbook Otomasyon hesabınızda içeri aktarın.

1. Automation hesabınızı açın ve tıklayın **Runbook'lar** sayfası.
1. Tıklayın **Gözat galeri** düğmesi.
1. Arama "İzleyici runbook için" select **bir dizindeki yeni dosyaları arar İzleyici runbook** seçip **alma**.
  ![UI Otomasyon runbook'tan alma](media/automation-watchers-tutorial/importsourcewatcher.png)
1. Bir ad ve açıklama ve runbook vermek **Tamam** runbook Otomasyon hesabınızda içeri aktarmak için.
1. Seçin **Düzenle** ve ardından **Yayımla**. Zaman istemde seçin **Evet** runbook'u yayımlamak için.

## <a name="create-an-automation-variable"></a>Bir Otomasyon değişkeni oluşturma

Bir [Otomasyon değişkeni](automation-variables.md) önceki runbook okur ve depolayan her dosyasından zaman damgaları depolamak için kullanılır. 

1. Seçin **değişkenleri** altında **paylaşılan kaynakları** seçip **+ değişken Ekle**.
1. "Watch-NewFileTimestamp" için bir ad girin
1. DateTime türünü seçin.
1. Tıklayın **oluşturma** düğmesi. Bu Otomasyon değişkeni oluşturur.

## <a name="create-an-action-runbook"></a>Bir eylem runbook oluşturma

Bir eylem runbook bir izleyici görevde bir izleyici runbook'tan geçirilen verileri hareket için kullanılır. Bu adımda, içeri aktarma güncelleştir "İşlem-NewFile" adlı bir ön tanımlı eylem runbook.

1. Otomasyon hesabınıza gidin ve seçin **Runbook'lar** altında **işlem OTOMASYONU** kategorisi.
1. Tıklayın **Gözat galeri** düğmesi.
1. "İçin İzleyici eylem" arayın ve seçin **İzleyici runbook tarafından tetiklenen olayları işler İzleyici eylem** seçip **alma**.
  ![Kullanıcı Arabiriminden eylem runbook'u İçeri Aktar](media/automation-watchers-tutorial/importsourceaction.png)
1. Bir ad ve açıklama ve runbook vermek **Tamam** runbook Otomasyon hesabınızda içeri aktarmak için.
1. Seçin **Düzenle** ve ardından **Yayımla**. Zaman istemde seçin **Evet** runbook'u yayımlamak için.

## <a name="create-a-watcher-task"></a>Bir izleyici görev oluşturma

İzleyici görev iki bölüm içerir. İzleyici ve eylem. İzleyici görev içinde tanımlanan bir aralıkta İzleyici çalıştırır. İzleyici runbook verileri, eylem runbook geçirilir. Bu adımda, önceki adımlarda tanımlanan İzleyici ve eylem runbook'lar başvuran İzleyici görev yapılandırın.

1. Otomasyon hesabınıza gidin ve seçin **İzleyici görevleri** altında **işlem OTOMASYONU** kategorisi.
1. İzleyici Görevler sayfası seçin ve tıklatın **+ İzleyici görev ekleme** düğmesi.
1. "WatchMyFolder" adı olarak girin.

1. Seçin **yapılandırma İzleyicisi** seçip **izleme NewFile** runbook.

1. Aşağıdaki değerleri parametrelerini girin:

   * **FOLDERPATH** -yeni dosyalar oluşturulduğu karma çalışanı üzerinde bir klasör. d:\examplefiles
   * **UZANTI** -tüm dosya uzantılarını işlemek için boş bırakın.
   * **RECURSE** -bu değer varsayılan olarak bırakın.
   * **ÇALIŞMA ayarları** -karma çalışanı seçin.

1. Tamam'ı tıklatın ve ardından İzleyici sayfasına dönün.
1. Seçin **eylemi yapılandırın** ve "İşlem-NewFile" runbook'u seçin.
1. Aşağıdaki değerleri parametrelerini girin:

   *    **EVENTDATA** -boş bırakın. Verileri varsayılan olarak, İzleyici runbook'tan olarak geçirilir.  
   *    **Çalışma ayarları** -Azure bırakın Bu runbook Otomasyon hizmetinde çalışan.

1. Tıklatın **Tamam**ve ardından İzleyici sayfasına dönün.
1. Tıklatın **Tamam** İzleyici görevi oluşturmak için.

![İzleyici UI eyleminden yapılandırın](media/automation-watchers-tutorial/watchertaskcreation.png)

## <a name="trigger-a-watcher"></a>Tetikleyici bir İzleyicisi

İzleyici test beklendiği gibi çalıştığından, bir test dosyası oluşturmanız gerekir.

Karma çalışanı içine uzaktan. Açık **PowerShell** ve klasöründe bir test dosyası oluşturun.
  
   ```PowerShell-interactive
   New-Item -Name ExampleFile1.txt
   ```

Aşağıdaki örnek, beklenen çıkış gösterir.

```
    Directory: D:\examplefiles


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----       12/11/2017   9:05 PM              0 ExampleFile1.txt
```

## <a name="inspect-the-output"></a>Çıktıyı inceleyin.

1. Otomasyon hesabınıza gidin ve seçin **İzleyici görevleri** altında **işlem OTOMASYONU** kategorisi.
1. "WatchMyFolder" İzleyici görevini seçin.
1. Tıklayın **görüntülemek İzleyici akışları** altında **akışları** İzleyici yeni dosya bulundu ve eylem runbook'u başlatan görmek için.
1. Eylem runbook işleri görmek için tıklayın **görüntülemek İzleyici eylem işleri**. Her bir iş olabilir Seçili iş ayrıntılarını görüntüleyin.

   ![UI eylem işlerden İzleyicisi](media/automation-watchers-tutorial/WatcherActionJobs.png)

Aşağıdaki örnekte yeni dosya bulunduğunda beklenen çıktı görülebilir:

```
Message is Process new file...



Passed in data is @{FileName=D:\examplefiles\ExampleFile1.txt; Length=0}
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Bir izleyici runbook'u İçeri Aktar
> * Bir Otomasyon değişkeni oluşturma
> * Bir eylem runbook oluşturma
> * Bir izleyici görev oluşturma
> * Tetikleyici bir İzleyicisi
> * Çıktıyı inceleyin.

Kendi runbook yazma hakkında daha fazla bilgi için bu bağlantıyı izleyin.

> [!div class="nextstepaction"]
> [İlk PowerShell runbook'um](automation-first-runbook-textual-powershell.md).
