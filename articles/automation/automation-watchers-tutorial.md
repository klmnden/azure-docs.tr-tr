---
title: "Azure Automation hesabında bir izleyici görev oluşturma | Microsoft Docs"
description: "Bir klasörde oluşturulan yeni dosyalarda izlemek için Azure Automation hesabındaki bir izleyici görev oluşturmayı öğrenin."
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
ms.date: 01/15/2017
ms.author: eamono
ms.openlocfilehash: 7cd6bebcaa1ed263b9854f7307cf22fba006748e
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/18/2017
---
# <a name="azure-automation-watcher-tasks-enable-you-to-respond-to-events-happening-in-your-local-datacenter"></a>Azure Otomasyonu İzleyici görevleri, yerel veri merkezinizde gerçekleştiği olaylara yanıt olanak sağlar

Bu öğreticide, yeni bir izleyici görevi nasıl oluşturacağınızı öğrenin:

> [!div class="checklist"]
> * Bir dizindeki yeni dosyaları arar bir izleyici runbook oluşturun.
> * Bir dosya İzleyicisi tarafından işlenen en son ne zaman tutmak için bir Otomasyon değişkeni oluşturun.
> * İzleyici runbook yeni bir dosya bulduğunda çağrılan bir eylem runbook oluşturun.
> * Eylem runbook ve İzleyici runbook seçer bir izleyici görev oluşturun.
> * Bir izleyici bir dizine yeni bir dosya ekleyerek tetikler.
> * Yeni dosyasını bilgileri gösterir eylem runbook çıkışı inceleyin.  

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gereklidir.
+ Azure aboneliği. Henüz bir aboneliğiniz yoksa [MSDN abone avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) ya da [ücretsiz hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) için kaydolabilirsiniz.
+ [Otomasyon hesabı](automation-offering-get-started.md) İzleyici ve eylem runbook'ları ve İzleyici görev tutmak için.
+ A [karma runbook çalışanı](automation-hybrid-runbook-worker.md) İzleyici görevin çalıştığı.

## <a name="create-a-watcher-runbook-that-looks-for-new-files"></a>Yeni dosya arayan bir izleyici runbook oluşturma
1.  Automation hesabını açın ve runbook'ları sayfasında'yi tıklatın.
2.  "Gözat Galerisi" düğmesine tıklayın.
![Runbook listeden kullanıcı Arabirimi](media/automation-watchers-tutorial/WatcherTasksRunbookList.png)
3.  "Watch-NewFile" için arama ve runbook Otomasyon dikkate alın.
![UI runbook'tan yayımlama](media/automation-watchers-tutorial/Watch-NewFileRunbook.png)
4.  Runbook kaynağı görüntüleyip "Yayımla" düğmesine tıklayın "Düzenle"'i tıklatın.

## <a name="create-an-automation-variable-to-keep-the-last-time-a-file-was-processed-by-the-watcher"></a>Bir dosya İzleyicisi tarafından işlenen en son ne zaman tutmak için bir Otomasyon değişkeni oluşturma
1.  Paylaşılan kaynaklar altında değişkenleri sayfasını açın ve "Ekle değişkeni" tıklatın ![listesinde değişkenleri, kullanıcı Arabirimi](media/automation-watchers-tutorial/WatcherVariableList.png)
2.  "Watch-NewFileTimestamp" için bir ad girin
3.  DateTime olarak türü seçin ve ardından "Oluştur" düğmesine tıklayın.
![Kullanıcı Arabiriminden Filigran değişkeni oluşturma](media/automation-watchers-tutorial/WatcherWatermarkVariable.png)

## <a name="create-an-action-runbook-that-is-called-when-the-watcher-runbook-finds-a-new-file"></a>İzleyici runbook yeni bir dosya bulduğunda çağrılan bir eylem runbook oluşturma
1.  "İşlem OTOMASYONU" kategorisi altında Runbook'lar sayfasında'i tıklatın.
2.  "Gözat Galerisi" düğmesine tıklayın.
3.  "İşlem-NewFile" için arama ve runbook Otomasyon dikkate alın.
4.  Runbook kaynağı görüntüleyip "Yayımla" düğmesine tıklayın "Düzenle"'i tıklatın.
![UI gelen işlem İzleyicisi](media/automation-watchers-tutorial/Watch-ProcessNewFile.png)


## <a name="create-a-watcher-task-that-selects-the-watcher-runbook-and-action-runbook"></a>Eylem runbook ve İzleyici runbook seçer bir izleyici görev oluşturma
1.  İzleyici görevleri sayfasını açın ve "bir izleyici Görev Ekle" düğmesini tıklatın.
![İzleyici listeden kullanıcı Arabirimi](media/automation-watchers-tutorial/WatchersList.png)
2.  "Yeni dosya adı olarak izleme" girin.
3.  "Yapılandırma İzleyicisi" ve "Watch-NewFile" runbook'u seçin.
![UI gelen İzleyici yapılandırma](media/automation-watchers-tutorial/ConfigureWatcher.png)
4.  Aşağıdaki değerleri parametrelerini girin:
    *   FOLDERPATH. Yeni dosyalar oluşturulduğu karma çalışanı üzerinde bir klasör
    *   UZANTI. Tüm dosya uzantılarını işlemek için boş bırakın.
    *   RECURSE. Varsayılan adı bırakın.
    *   ÇALIŞMA AYARLARI. Karma çalışanı seçin.
5.  Tamam'ı tıklatın ve ardından İzleyici sayfasına dönün.
6.  "Yapılandırma eylem" ve "İşlem-NewFile" runbook'u seçin.
![İzleyici UI eyleminden yapılandırın](media/automation-watchers-tutorial/ConfigureAction.png)
7.  Aşağıdaki değerleri parametrelerini girin:
    *   EVENTDATA. Boş bırakın. Verileri varsayılan olarak, İzleyici runbook'tan olarak geçirilir.
    *   Çalışma ayarları. Azure bırakın Bu runbook Otomasyon hizmetinde çalışan.
8.  Tamam'ı tıklatın ve ardından İzleyici sayfasına dönün.
9.  İzleyici görevi oluşturmak için Tamam'ı tıklatın.

## <a name="trigger-a-watcher-by-adding-a-new-file-to-a-directory"></a>Tetikleyici bir dizine yeni bir dosya ekleyerek bir İzleyicisi
1.  Karma çalışanı içine uzaktan
2.  İzleyici görev tarafından izlenen klasörü için yeni bir metin dosyası ekleyin.

## <a name="inspect-the-output-from-the-action-runbook-that-shows-information-on-the-new-file"></a>Yeni dosyasını bilgileri gösterir eylem runbook çıkışı inceleyin.
1.  İzleyici görev "İzleme yeni dosyalar" için tıklayın
2.  ' I tıklatın, "Görünüm İzleyici akışların" İzleyici yeni dosya bulundu ve eylem runbook'u başlatan görmek için.
3.  Tıklayın "görünümü İzleyici eylem işlere" eylemi runbook işi görmek için.
![UI eylem işlerden İzleyicisi](media/automation-watchers-tutorial/WatcherActionJobs.png)


## <a name="next-steps"></a>Sonraki adımlar:

Daha fazla bilgi için bkz: [ilk PowerShell runbook'um](automation-first-runbook-textual-powershell.md).








