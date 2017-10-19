---
title: "Visual Studio için Data Lake Araçları'nı kullanarak U-SQL betikleri geliştirme | Microsoft Docs"
description: "Visual Studio için Data Lake Araçları'nı nasıl yükleyeceğinizi ve U-SQL betiklerini nasıl geliştirip test edeceğinizi öğrenin."
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: jhubbard
editor: cgronlun
ms.assetid: ad8a6992-02c7-47d4-a108-62fc5a0777a3
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/28/2017
ms.author: saveenr, yanacai
ms.openlocfilehash: a48ce209bf3d5b7e5060acf2850144df5418828d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="develop-u-sql-scripts-by-using-data-lake-tools-for-visual-studio"></a>Visual Studio için Data Lake Araçları'nı kullanarak U-SQL betikleri geliştirme
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]


Azure Data Lake Analytics hesapları oluşturmak, [U-SQL](data-lake-analytics-u-sql-get-started.md) içinde işler tanımlamak ve Data Lake Analytics hizmetine iş göndermek için Visual Studio’nun nasıl kullanılacağını öğrenin. Data Lake Analytics hakkında daha fazla bilgi için bkz. [Azure Data Lake Analytics'e genel bakış](data-lake-analytics-overview.md).


## <a name="prerequisites"></a>Ön koşullar

* **Visual Studio**: Express dışında tüm sürümler desteklenir.
    * Visual Studio 2017
    * Visual Studio 2015
    * Visual Studio 2013
* **.NET için Microsoft Azure SDK** 2.7.1 sürümü veya sonraki sürümleri.  [Web platformu yükleyicisini](http://www.microsoft.com/web/downloads/platform.aspx) kullanarak yükleyin.
* **Data Lake Analytics** hesabı. Hesap oluşturmak için bkz. [Azure portalı kullanarak Azure Data Lake Analytics ile çalışmaya başlama](data-lake-analytics-get-started-portal.md).

## <a name="install-azure-data-lake-tools-for-visual-studio"></a>Visual Studio için Azure Data Lake Araçları’nı yükleme 

[İndirme Merkezi'nden](http://aka.ms/adltoolsvs) Visual Studio için Azure Data Lake Araçları’nı indirip yükleyin. Yükleme işleminden sonra şunları kontrol edin:
* **Sunucu Gezgini** > **Azure** düğümü, **Data Lake Analytics** düğümü içerir. 
* **Araçlar** menüsünde **Data Lake** öğesi vardır.

## <a name="connect-to-an-azure-data-lake-analytics-account"></a>Azure Data Lake Analytics hesabına bağlanma

1. Visual Studio'yu açın.
2. **Görünüm** > **Sunucu Gezgini**’ni seçerek Sunucu Gezgini’ni açın.
3. **Azure**’a sağ tıklayın. Ardından **Microsoft Azure Aboneliğine Bağlan**’ı seçin ve yönergeleri uygulayın.
4. Sunucu Gezgini'nde **Azure** > **Data Lake Analytics**’i seçin. Data Lake Analytics hesaplarınızın listesini görürsünüz.


## <a name="write-your-first-u-sql-script"></a>İlk U-SQL betiğinizi yazma

Aşağıda basit bir U-SQL betiği gösterilmiştir. Küçük bir veri kümesini tanımlar ve bu veri kümesini `/data.csv` adlı bir dosya olarak varsayılan Data Lake Store’a yazar.

```
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS 
              D( customer, amount );
OUTPUT @a
    TO "/data.csv"
    USING Outputters.Csv();
```

### <a name="submit-a-data-lake-analytics-job"></a>Data Lake Analytics işi gönderme

1. **Dosya** > **Yeni** > **Proje**’yi seçin.

2. **U-SQL Projesi** türünü seçin ve **Tamam**’a tıklayın. Visual Studio, **Script.usql** dosyasıyla bir çözüm oluşturur.

3. Önceki betiği **Script.usql** penceresine yapıştırın.

4. **Script.usql** penceresinin sol üst köşesinde Data Lake Analytics hesabını belirtin.

    ![U-SQL Visual Studio projesini gönderme](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-submit-job.png)

5. **Script.usql** penceresinin sol üst köşesinde **Gönder**’i seçin.
6. **Analytics Hesabı**’nı doğrulayın ve ardından **Gönder**’i seçin. Gönderim tamamlandıktan sonra, gönderme işleminin sonuçları Visual Studio için Data Lake Araçları Sonuçları içinde sunulur.

    ![U-SQL Visual Studio projesini gönderme](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-submit-job-advanced.png)
7. En son iş durumunu görmek ve ekranı yenilemek için **Yenile**’ye tıklayın. İş başarılı olduğunda **İş Grafiği**, **Meta Veri İşlemleri**, **Durum Geçmişi** ve **Tanılama**’yı gösterir:

    ![U-SQL Visual Studio Data Lake Analytics iş performans grafiği](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-performance-graph.png)

   * **İş Özeti**, işin özetini gösterir.   
   * **İş Ayrıntıları**, iş hakkında betik, kaynaklar ve köşeler gibi daha özel bilgiler gösterir.
   * **İş Grafiği**, işin ilerleme durumunu görselleştirir.
   * **Meta Veri İşlemleri**, U-SQL kataloğunda yapılan tüm işlemleri gösterir.
   * **Veri** tüm girdileri ve çıktıları gösterir.
   * **Tanılama**, iş yürütme ve performans iyileştirme için gelişmiş bir analiz sağlar.

### <a name="to-check-job-state"></a>İş durumu denetlemek için

1. Sunucu Gezgini'nde **Azure** > **Data Lake Analytics**’i seçin. 
2. Data Lake Analytics hesap adını genişletin.
3. **İşler**’e çift tıklayın.
4. Daha önce gönderdiğiniz işi seçin.

### <a name="to-see-the-output-of-a-job"></a>Bir işin çıktısını görmek için

1. Sunucu Gezgini’nde gönderdiğiniz işe gidin.
2. **Veri** sekmesine tıklayın.
3. **İş Çıktıları** sekmesinde `"/data.csv"` dosyasını seçin.

## <a name="next-steps"></a>Sonraki adımlar

* [Test etmek ve hata ayıklamak için kendi iş istasyonunuzda U-SQL betiklerini çalıştırma](data-lake-analytics-data-lake-tools-local-run.md)
* [Visual Studio Code için Azure Data Lake Araçları'nı kullanarak U-SQL işlerindeki C# kodunda hata ayıklama](data-lake-tools-for-vscode-local-run-and-debug.md)
* [Visual Studio Code için Azure Data Lake Araçları’nı kullanma](data-lake-analytics-data-lake-tools-for-vscode.md)
