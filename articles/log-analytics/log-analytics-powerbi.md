---
title: Power BI'da Azure günlük analizi veri içeri aktarma | Microsoft Docs
description: Power BI bir zengin Görselleştirmelerini ve raporları farklı veri kümelerinin analize sağlayan bir Microsoft bulut tabanlı iş analiz hizmetidir.  Bu makalede nasıl yapılandırılacağını açıklar günlük analizi veri Power BI'a aktarın ve otomatik olarak yenilemek için yapılandırın.
services: log-analytics
documentationcenter: ''
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 83edc411-6886-4de1-aadd-33982147b9c3
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/19/2018
ms.author: bwren
ms.openlocfilehash: 6d7f8f89f90223dc5dd186a63b3912a13910cb34
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="import-azure-log-analytics-data-into-power-bi"></a>Azure günlük analizi veri Power BI'a aktarın


[Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/) bir bulut tabanlı İş analizi hizmeti Microsoft'tan farklı veri kümelerinin analize zengin Görselleştirmelerini ve raporlar sunar.  Farklı kaynaklardan veri combing ve raporları web ve mobil cihazlarda paylaşımı kendi özellikleri da yararlanabilir için günlük analizi günlük arama sonuçlarını bir Power BI veri kümesini içeri aktarabilirsiniz.

## <a name="overview"></a>Genel Bakış
Günlük analizi çalışma alanından Power BI'a verileri almak için günlük analizi günlük arama sorgusu göre Power bı'da bir veri kümesi oluşturun.  Sorgu, veri kümesi her yenilendiğinde çalıştırılır.  Daha sonra verileri veri kümesini kullanarak Power BI raporları oluşturabilir.  Power BI'da veri kümesi oluşturmak için sorgunuz için günlük analizi verilecek [Power Query (M) dil](https://msdn.microsoft.com/library/mt807488.aspx).  Power BI bir veri kümesi olarak yayımlamayı ve bu Power BI Desktop'ta bir sorgu oluşturmak için kullanın.  Bu işlem ayrıntılarını, aşağıda açıklanmıştır.

![Power BI için günlük analizi](media/log-analytics-powerbi/overview.png)

## <a name="export-query"></a>Sorgu verme
Başlangıç oluşturarak bir [günlük arama](log-analytics-log-search-new.md) , verileri, Power BI DataSet'i istediğiniz günlük analizi döndürür.  Bu sorgu daha sonra dışarı [Power Query (M) dil](https://msdn.microsoft.com/library/mt807488.aspx) Power BI Desktop tarafından kullanılabilir.

1. Günlük arama, veri kümesi için veri ayıklamak için günlük analizi oluşturun.
2. Günlük arama portalını kullanıyorsanız, tıklatın **Power BI**.  Analytics portalını kullanıyorsanız seçin **verme** > **Power BI sorgu (M)**.  Bu seçeneklerin ikisi de sorgu adlı bir metin dosyasına dışarı aktarma **PowerBIQuery.txt**. 

    ![Verme günlüğü arama](media/log-analytics-powerbi/export-logsearch.png) ![Verme günlüğü arama](media/log-analytics-powerbi/export-analytics.png)

3. Metin dosyasını açın ve içeriğini kopyalayın.

## <a name="import-query-into-power-bi-desktop"></a>Power BI Desktop'a sorgu alma
Power BI Desktop, veri kümeleri ve Power BI yayımlanabilmesi için raporlar oluşturmanıza olanak tanıyan bir masaüstü uygulamasıdır.  Günlük analizi dışarı Power Query dilini kullanarak bir sorgu oluşturmak için de kullanabilirsiniz. 

1. Yükleme [Power BI Desktop](https://powerbi.microsoft.com/desktop/) zaten sahip yoksa ve uygulamayı açın.
2. Seçin **Veri Al** > **boş sorgu** yeni bir sorgu açın.  Ardından **Gelişmiş Düzenleyici** ve dışarı aktarılan dosya içeriğini Query'ye yapıştırın. **Bitti**’ye tıklayın.

    ![Power BI Desktop sorgu](media/log-analytics-powerbi/desktop-new-query.png)

5. Sorgu çalıştığında, ve sonuçları görüntülenir.  Azure'a bağlanmak kimlik bilgileri istenebilir.  
6. Sorgu için açıklayıcı bir ad yazın.  Varsayılan değer **Sorgu1**. Tıklatın **kapatın ve geçerli** veri kümesi için bir rapor eklemek için.

    ![Power BI Desktop adı](media/log-analytics-powerbi/desktop-results.png)



## <a name="publish-to-power-bi"></a>Power BI yayımlama
Power BI yayımladığınızda, bir veri kümesi ve bir rapor oluşturulur.  Ardından bu Power BI Desktop'ta bir rapor oluşturursanız, verilerinizle yayımlanır.  Aksi durumda, boş bir rapor oluşturulur.  Power BI raporu değiştirebilir veya veri kümesine bağlı yeni bir tane oluşturun.

8. Verilerinizi temel bir rapor oluşturun.  Kullanım [Power BI Desktop belgelerine](https://docs.microsoft.com/power-bi/desktop-report-view) , kendisiyle bilmiyorsanız.  Power BI göndermeye hazır olduğunuzda, tıklatın **Yayımla**.  İstendiğinde, Power BI hesabınızda bir hedef seçin.  Aklınızda belirli bir hedefe sahip değilseniz kullanmak **çalışma Alanım**.

    ![Power BI Desktop yayımlama](media/log-analytics-powerbi/desktop-publish.png)

3. Yayımlama tamamlandığında tıklayın **Power BI'da Aç** Power BI, yeni veri kümesi ile açın.


### <a name="configure-scheduled-refresh"></a>Zamanlanan yenileme yapılandırın
Power BI'da oluşturulan veri kümesi, Power BI Desktop'ta daha önce gördüğünüzle aynı verilere sahip olur.  Düzenli aralıklarla sorguyu yeniden çalıştırın ve günlük analizi en son verilerle doldurmak için veri kümesi yenilemeniz gerekir.  

1. Burada, yüklediğiniz seçin ve raporu çalışma alanı'ı tıklatın **veri kümeleri** menüsü. Yeni Veri kümenizi yanındaki bağlam menüsü seçip **ayarları**. Altında **veri kaynağı kimlik bilgileri** kimlik bilgileri geçersiz olduğunu bildiren bir mesaj olması gerekir.  Kimlik verilerini yenilendiğinde kullanmak henüz veri kümesi için sağlanan henüz olmasıdır.  Tıklatın **kimlik bilgilerini Düzenle** ve günlük analizi erişim kimlik bilgilerini belirtin.

    ![Power BI zamanlama](media/log-analytics-powerbi/powerbi-schedule.png)

5. Altında **zamanlanan yenileme** seçeneğini etkinleştirmek **verilerinizi güncel tutun**.  İsteğe bağlı olarak değiştirebileceğiniz **yenileme sıklığı** ve yenileme çalıştırmak için bir veya daha fazla belirli saatler.

    ![Power BI yenileme](media/log-analytics-powerbi/powerbi-schedule-refresh.png)



## <a name="next-steps"></a>Sonraki adımlar
* Hakkında bilgi edinin [oturum aramaları](log-analytics-log-searches.md) Power BI aktarılabilir sorguları oluşturmak için.
* Daha fazla bilgi edinmek [Power BI](http://powerbi.microsoft.com) günlük analizi dışarı üzerinde temel görsel öğeleri oluşturmak için.
