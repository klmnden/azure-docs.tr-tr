---
title: Visual Studio'da Azure Stream Analytics işleri görüntüle
description: Bu makalede, Stream Analytics işleri Visual Studio'da görüntülemeyi açıklar.
services: stream-analytics
author: su-jie
ms.author: sujie
manager: kfile
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 07/10/2018
ms.openlocfilehash: 1c7133801eb0d95616cacf501162e6cee3da7c80
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61477933"
---
# <a name="use-visual-studio-to-view-azure-stream-analytics-jobs"></a>Azure Stream Analytics işleri görüntülemek için Visual Studio

Visual Studio için Azure Stream Analytics araçları kolaylaştırır geliştiricilerin Stream Analytics işlerini doğrudan IDE'den yönetme. Azure Stream Analytics araçları ile şunları yapabilirsiniz:
- [Yeni İş Oluştur](stream-analytics-quick-create-vs.md)
- Başlatabilir, durdurabilir ve [işleri izleme](stream-analytics-monitor-jobs-use-vs.md)
- İş sonuçlarını denetleyin
- Mevcut dışarı aktarma işleri için bir proje
- Giriş ve çıkış bağlantılarını test et
- [Sorguları yerel olarak çalıştırma](stream-analytics-vs-tools-local-run.md)

Bilgi edinmek için nasıl [Visual Studio için Azure Stream Analytics araçları yükleme](stream-analytics-tools-for-visual-studio-install.md).

## <a name="explore-the-job-view"></a>İş görünümü keşfedin

Visual Studio'dan Azure Stream Analytics işleri ile etkileşim kurmak için iş görünümü kullanabilirsiniz.

### <a name="open-the-job-view"></a>İş görünümü Aç

1. İçinde **Sunucu Gezgini**seçin **Stream Analytics işleri** seçip **Yenile**. İşinizi altında görünmelidir **Stream Analytics işleri**.

    ![Stream Analytics Sunucu Gezgini listesi](./media/stream-analytics-vs-tools/stream-analytics-tools-for-vs-list-jobs-01.png)



2. Proje düğümünü ve çift **iş görünümü** iş görünümünü açmak için düğümü.
    
   ![Genişletilmiş proje düğümü](./media/stream-analytics-vs-tools/stream-analytics-tools-for-vs-job-view-01.png)

### <a name="start-and-stop-jobs"></a>İşleri durdurmak ve başlatmak

Azure Stream Analytics işleri, Visual Studio Proje görünümü tam olarak yönetilebilir. Denetimleri başlatmak, durdurmak veya bir işi silmek için kullanın.
    
   ![Stream Analytics işi denetimleri](./media/stream-analytics-vs-tools/azure-stream-analytics-job-view-controls.png)


## <a name="check-job-results"></a>İş sonuçlarını denetleyin

Visual Studio için Stream Analytics araçları Azure Data Lake depolama ve blob depolama için şu anda çıkış Önizleme destekler. Sonucu görüntülemek için çift iş diyagramında çıkış düğümünün tıklamanız yeterlidir **iş görünümü** ve uygun kimlik bilgilerini girin.

   ![Stream Analytics işi blob çıkış](./media/stream-analytics-vs-tools/stream-analytics-blob-preview.png)


## <a name="export-jobs-to-a-project"></a>Bir proje için dışarı aktarma işleri

Var olan bir işi bir projeye aktarabilirsiniz iki yolu vardır.

1. İçinde **Sunucu Gezgini**, Stream Analytics işleri düğümünde proje düğümüne sağ tıklayın. Seçin **dışarı yeni Stream Analytics projesine**.
    
   ![Proje için dışarı aktarma işi](./media/stream-analytics-vs-tools/stream-analytics-tools-for-vs-export-job-01.png)
    
    Oluşturulan proje görünür **Çözüm Gezgini**.
    
   ![Çözüm Gezgini](./media/stream-analytics-vs-tools/stream-analytics-tools-for-vs-export-job-02.png)

2. Proje Görünümü'nde seçin **proje oluşturma**.
    
   ![Proje Görünümü'ndeki proje oluştur](./media/stream-analytics-vs-tools/stream-analytics-tools-for-vs-export-job-03.png)

## <a name="test-connections"></a>Test bağlantıları

Giriş ve çıkış bağlantıları, gelen edilebilirler **iş görünümü** bir seçenek belirleyerek **Test Bağlantısı** açılır.

   ![Test bağlantısı açılan kutusu](./media/stream-analytics-vs-tools/stream-analytics-test-connection-dropdown.png)

**Test Bağlantısı** sonuçları görüntülenir **çıkış** penceresi.

   ![Bağlantı testi sonuçları](./media/stream-analytics-vs-tools/stream-analytics-test-connection-results.png)

## <a name="next-steps"></a>Sonraki adımlar

* [İzleme ve Visual Studio kullanarak Azure Stream Analytics işlerini yönetme](stream-analytics-monitor-jobs-use-vs.md)
* [Hızlı Başlangıç: Visual Studio kullanarak Stream Analytics işi oluşturma](stream-analytics-quick-create-vs.md)
* [Öğretici: Azure Stream Analytics işi dağıtma ile CI/Azure işlem hatları kullanarak CD](stream-analytics-tools-visual-studio-cicd-vsts.md)
* [Stream Analytics araçlarıyla sürekli tümleştirme ve geliştirme](stream-analytics-tools-for-visual-studio-cicd.md)
