---
title: İzleyici görevi Azure Otomasyonu hesabı oluşturma
description: İzleyici görevi, bir klasörde oluşturulan yeni dosyaları izlemek üzere Azure Otomasyonu hesabı oluşturmayı öğrenin.
services: automation
ms.service: automation
ms.subservice: process-automation
author: eamonoreilly
ms.author: eamono
ms.topic: conceptual
ms.date: 10/30/2018
ms.openlocfilehash: bee414ada61e2cfcf7609b02ef1da7323a0fe0e3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61304648"
---
# <a name="create-an-azure-automation-watcher-tasks-to-track-file-changes-on-a-local-machine"></a>Bir Azure Otomasyonu izleyicisini yerel bir makinede dosya değişiklikleri izlemek için görevleri oluşturun

Azure Otomasyonu İzleyici görevleri için olayları izleyin ve PowerShell runbook'ları ile eylemleri tetiklemek için kullanır. Bu öğreticide bir dizine yeni bir dosya eklendiğinde izlemek için bir izleyici görevi oluşturma işleminde size yol gösterir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir izleyici runbook'u İçeri Aktar
> * Bir Otomasyon değişkeni oluşturma
> * Eylem runbook oluşturma
> * İzleyici görevi oluşturma
> * Tetikleyici bir izleyici
> * Çıktıyı İnceleme

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gereklidir:

* Azure aboneliği. Henüz bir aboneliğiniz yoksa [MSDN abone avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) ya da [ücretsiz hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) için kaydolabilirsiniz.
* [Otomasyon hesabı](automation-offering-get-started.md) izleyiciyi, eylem runbook'ları ve İzleyici görevi tutacak.
* A [karma runbook çalışanı](automation-hybrid-runbook-worker.md) İzleyici görevi çalıştığı.

> [!NOTE]
> İzleyici görevleri Azure Çin'de desteklenmez.

## <a name="import-a-watcher-runbook"></a>Bir izleyici runbook'u İçeri Aktar

Bu öğreticide adlı bir izleyici runbook'u **Watch NewFile** yeni dosyaları bir dizinde aramak için. Bir izleyici runbook'u bir klasördeki dosyaları için son bilinen yazma zamanını alır ve bu eşik daha yeni olan herhangi bir dosya arar. Bu adımda, bu runbook, Otomasyon hesabına aktarın.

1. Otomasyon hesabınızı açın ve tıklayarak **runbook'ları** sayfası.
2. Tıklayarak **Galeriye Gözat** düğmesi.
3. "İzleyici runbook'u", select Ara **yeni dosyaları bir dizinde bakan İzleyici runbook'u** seçip **alma**.
  ![Kullanıcı Arabiriminden Otomasyon runbook'u İçeri Aktar](media/automation-watchers-tutorial/importsourcewatcher.png)
1. Runbook'a bir ad ve açıklama ve select vermek **Tamam** runbook Otomasyon hesabınızda içeri aktarmak için.
1. Seçin **Düzenle** ve ardından **Yayımla**. Zaman seçim istenir **Evet** runbook'u yayımlayamadı.

## <a name="create-an-automation-variable"></a>Bir Otomasyon değişkeni oluşturma

Bir [automation değişkeni](automation-variables.md) önceki runbook okuyan ve depolar her bir dosyadan zaman damgaları depolamak için kullanılır.

1. Seçin **değişkenleri** altında **paylaşılan kaynakları** seçip **+ değişken Ekle**.
1. "Watch-NewFileTimestamp" adını girin
1. DateTime türünü seçin.
1. Tıklayarak **Oluştur** düğmesi. Bu Otomasyon değişkeni oluşturur.

## <a name="create-an-action-runbook"></a>Eylem runbook oluşturma

Eylem runbook bir izleyici görevi, bir izleyici runbook'tan geçirilen verileri üzerinde işlem için kullanılır. PowerShell iş akışı runbook'ları İzleyici görevleri tarafından desteklenmiyor, PowerShell runbook'ları kullanmanız gerekir. Bu adımda, içeri aktarma güncelleştirme "İşlem NewFile" adlı önceden tanımlanmış eylem runbook.

1. Otomasyon hesabınıza gidin ve seçin **runbook'ları** altında **süreç OTOMASYONU** kategorisi.
1. Tıklayarak **Galeriye Gözat** düğmesi.
1. "İzleyici eylemi için" arayıp seçin **bir izleyici runbook'u tarafından tetiklenen olayları işler İzleyici eylemi** seçip **alma**.
  ![Kullanıcı Arabiriminden eylemi runbook'u İçeri Aktar](media/automation-watchers-tutorial/importsourceaction.png)
1. Runbook'a bir ad ve açıklama ve select vermek **Tamam** runbook Otomasyon hesabınızda içeri aktarmak için.
1. Seçin **Düzenle** ve ardından **Yayımla**. Zaman seçim istenir **Evet** runbook'u yayımlayamadı.

## <a name="create-a-watcher-task"></a>İzleyici görevi oluşturma

İzleyici görevi iki bölüm içerir. İzleyiciyi, eylem. İzleyici görevi'içinde tanımlanan bir aralıkta İzleyicisi çalıştırır. İzleyici runbook'u verileri, eylem runbook geçirilir. Bu adımda, önceki adımlarda tanımlanan izleyiciyi, eylem runbook'ları başvuran İzleyici görevi yapılandırın.

1. Otomasyon hesabınıza gidin ve seçin **İzleyici görevleri** altında **süreç OTOMASYONU** kategorisi.
1. İzleyici görevleri sayfasını seçin ve tıklayın **+ İzleyici görevi Ekle** düğmesi.
1. "WatchMyFolder" adı girin.

1. Seçin **yapılandırma İzleyicisi** seçip **Watch NewFile** runbook.

1. Parametreler için aşağıdaki değerleri girin:

   * **FOLDERPATH** -karma çalışanı yeni dosya oluşturulduğu bir klasör. d:\examplefiles
   * **UZANTI** -tüm dosya uzantılarını işlemek için boş bırakın.
   * **RECURSE** -bu değeri varsayılan olarak bırakın.
   * **ÇALIŞTIRMA ayarları** -karma çalışanı'nı seçin.

1. Tamam'a tıklayın ve ardından İzleyicisi sayfasına geri dönün.
1. Seçin **eylemini yapılandırma** ve "İşlem NewFile" runbook'u seçin.
1. Parametreler için aşağıdaki değerleri girin:

   * **EVENTDATA** -boş bırakın. Veri İzleyicisi runbook'tan geçirilir.  
   * **Çalıştırma ayarları** -olarak Azure'a bırakın Bu runbook Otomasyon hizmeti çalışırken.

1. Tıklayın **Tamam**ve ardından İzleyicisi sayfasına geri dönün.
1. Tıklayın **Tamam** İzleyici görevi oluşturmak için.

![Kullanıcı arabiriminden İzleyici eylemi yapılandırın](media/automation-watchers-tutorial/watchertaskcreation.png)

## <a name="trigger-a-watcher"></a>Tetikleyici bir izleyici

İzleyici test beklendiği gibi çalıştığından, bir test dosyası oluşturmanız gerekir.

Uzak bağlantı karma çalışanı kurun. Açık **PowerShell** ve klasöründe bir sınama dosyası oluşturun.
  
```azurepowerShell-interactive
New-Item -Name ExampleFile1.txt
```

Aşağıdaki örnek, beklenen çıktıyı gösterir.

```output
    Directory: D:\examplefiles


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----       12/11/2017   9:05 PM              0 ExampleFile1.txt
```

## <a name="inspect-the-output"></a>Çıktıyı İnceleme

1. Otomasyon hesabınıza gidin ve seçin **İzleyici görevleri** altında **süreç OTOMASYONU** kategorisi.
1. İzleyici görevi "WatchMyFolder" seçin.
1. Tıklayarak **İzleyici akışlarını görüntüle** altında **akışları** İzleyicisi yeni dosyası bulundu ve eylemi runbook'u başlatan görmek için.
1. Eylem runbook işlerini görmek için tıklayın **İzleyici eylemi işlerini görüntüleme**. Her bir iş olabilir Seçili işin ayrıntılarını görüntüleyin.

   ![İzleyici eylemi işlerini kullanıcı arabiriminden](media/automation-watchers-tutorial/WatcherActionJobs.png)

Aşağıdaki örnekte yeni bir dosya bulunduğunda beklenen çıktıyı görülebilir:

```output
Message is Process new file...



Passed in data is @{FileName=D:\examplefiles\ExampleFile1.txt; Length=0}
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Bir izleyici runbook'u İçeri Aktar
> * Bir Otomasyon değişkeni oluşturma
> * Eylem runbook oluşturma
> * İzleyici görevi oluşturma
> * Tetikleyici bir izleyici
> * Çıktıyı İnceleme

Kendi runbook yazma hakkında daha fazla bilgi için bu bağlantıyı izleyin.

> [!div class="nextstepaction"]
> [İlk PowerShell runbook'um](automation-first-runbook-textual-powershell.md).

