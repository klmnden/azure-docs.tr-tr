---
title: Azure Log Analytics veri Power BI'a aktarma | Microsoft Docs
description: Power BI bir zengin görselleştirmeleri ve raporlar için farklı veri kümelerini analizini sağlar bir Microsoft bulut tabanlı İş analizi hizmetidir.  Bu makalede yapılandırma ve Log Analytics verilerini Power BI'a aktarmak ve otomatik olarak yenilenecek şekilde yapılandırın.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 83edc411-6886-4de1-aadd-33982147b9c3
ms.service: log-analytics
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/01/219
ms.author: bwren
ms.openlocfilehash: 2db6ddf57802f6fcf38cfc3ad7094ed94eaca3d8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65234192"
---
# <a name="import-azure-monitor-log-data-into-power-bi"></a>Azure İzleyici günlük verilerini Power BI'a aktarma


[Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/) bir bulut tabanlı İş analizi hizmeti Microsoft'un zengin görselleştirmeleri ve raporlar için farklı veri kümelerini analizini sağlar.  Bu nedenle farklı kaynaklardan verileri birleştirme ve raporları Web'de ve mobil cihazlarda paylaşma gibi özellikleri yararlanabilirsiniz Azure izleyici günlüğü sorgusunun sonuçları bir Power BI veri kümesine içeri aktarabilirsiniz.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="overview"></a>Genel Bakış
Verileri içe aktarmak için bir [Log Analytics çalışma alanı](manage-access.md) Power bı'a Azure İzleyici'de oluşturduğunuz göre Power bı'da bir veri kümesine bir [günlük sorgusu](../log-query/log-query-overview.md) Azure İzleyici'de.  Sorgu, veri kümesi her yenilendiğinde çalıştırılır.  Ardından, veri kümesindeki verileri kullanan Power BI raporları da oluşturabilirsiniz.  Power BI'da veri kümesini oluşturmak için sorgunuzu Log analytics'ten dışarı [Power Query (M) dil](https://msdn.microsoft.com/library/mt807488.aspx).  Power bı'da bir veri kümesi olarak yayımlayın ve bu Power BI Desktop'ta bir sorgu oluşturmak için kullanın.  Bu işlemin ayrıntıları aşağıda verilmiştir.

![Log Analytics'e Power BI](media/powerbi/overview.png)

## <a name="export-query"></a>Sorgu dışarı aktarma
Oluşturarak başlayın bir [günlük sorgusu](../log-query/log-query-overview.md) Power BI veri kümesine doldurmak için istediğiniz verileri döndürür.  Sorguları dışarı aktar [Power Query (M) dil](https://msdn.microsoft.com/library/mt807488.aspx) Power BI Desktop tarafından kullanılabilir.

1. [Log Analytics'te günlük sorgusu oluşturma](../log-query/get-started-portal.md) kümeniz için verileri ayıklamak için.
2. Seçin **dışarı** > **Power BI sorgu (M)** .  Bu sorgu adlı bir metin dosyasına dışarı aktarır **PowerBIQuery.txt**. 

    ![Günlük araması dışarı aktarma](media/powerbi/export-analytics.png)

3. Metin dosyasını açın ve içeriğini kopyalayın.

## <a name="import-query-into-power-bi-desktop"></a>Power BI Desktop'a alma sorgusu
Power BI Desktop, veri kümeleri ve Power BI'da yayımlanan raporlar oluşturmanıza olanak tanıyan bir masaüstü uygulamasıdır.  Azure İzleyici'den dışarı aktarılan güç sorgu dilini kullanarak bir sorgu oluşturmak için de kullanabilirsiniz. 

1. Yükleme [Power BI Desktop](https://powerbi.microsoft.com/desktop/) yoksa zaten var ve ardından uygulamayı açın.
2. Seçin **Veri Al** > **boş sorgu** yeni bir sorgu açın.  Ardından **Gelişmiş Düzenleyici** ve sorguya aktarılan dosyanın içeriğini yapıştırın. **Bitti**’ye tıklayın.

    ![Power BI Desktop sorgu](media/powerbi/desktop-new-query.png)

5. Sorguyu çalıştırır, ve sonuçları görüntülenir.  Azure'a bağlanmak kimlik bilgileri istenebilir.  
6. Sorgu için açıklayıcı bir ad yazın.  Varsayılan değer **Sorgu1**. Tıklayın **Kapat ve Uygula** veri kümesi, rapora eklemek için.

    ![Power BI Desktop adı](media/powerbi/desktop-results.png)



## <a name="publish-to-power-bi"></a>Power BI'da yayımlama
Power BI'da yayımladığınızda, bir veri kümesi ve bir rapor oluşturulur.  Ardından bu Power BI Desktop'ta bir rapor oluşturursanız, verilerinizle yayımlanır.  Aksi durumda, boş bir rapor oluşturulur.  Power BI raporu veya veri kümesini temel alan yeni bir tane oluşturun.

1. Verilerinizi temel alan bir rapor oluşturun.  Kullanım [Power BI Desktop belgesine](https://docs.microsoft.com/power-bi/desktop-report-view) ile bilgi sahibi değilseniz.  
1. Power BI için göndermeye hazır olduğunuzda, tıklayın **Yayımla**.  
1. İstendiğinde, Power BI hesabınızda bir hedef seçin.  Belirli bir hedefe aklınızda yoksa kullanması **çalışma Alanım**.

    ![Power BI Desktop yayımlama](media/powerbi/desktop-publish.png)

1. Yayımlama tamamlandığında tıklayın **Power BI'da açın** Power BI, yeni veri kümesi ile açın.


### <a name="configure-scheduled-refresh"></a>Zamanlanmış yenileme yapılandırma
Power BI'da oluşturulan veri kümesini Power BI Desktop uygulamasında daha önce gördüğünüz aynı verilere sahip olur.  Düzenli aralıklarla sorguyu yeniden çalıştırın ve Azure İzleyicisi'nden en son verilerle doldurmak için veri kümesini yenilemek gerekir.  

1. Yüklediğiniz seçin ve raporu çalışma alanına tıklayın **veri kümeleri** menüsü. 
1. Yeni, veri kümesinin yanındaki bağlam menüsünü seçip **ayarları**. 
1. Altında **veri kaynağı kimlik bilgileri** bir ileti kimlik bilgileri geçersiz olduğundan emin olmanız gerekir.  Verileri yenilediğinde kullanmak henüz veri kümesi için kimlik bilgilerini sağlamıyordu olmasıdır.  
1. Tıklayın **kimlik bilgilerini Düzenle** ve Azure İzleyici'de Log Analytics çalışma alanına erişimi olan kimlik bilgilerini belirtin. İki öğeli kimlik doğrulaması gerekiyorsa, seçin **OAuth2** için **kimlik doğrulama yöntemi** kimlik bilgilerinizle oturum açmanız istendiğinde.

    ![Power BI zamanlama](media/powerbi/powerbi-schedule.png)

5. Altında **zamanlanmış yenileme** seçeneğini etkinleştirmek **verilerinizi güncel tutun**.  İsteğe bağlı olarak değiştirebileceğiniz **yenileme sıklığını** ve yenileme çalıştırılacak bir veya daha fazla belirli saatler.

    ![Power BI Yenile](media/powerbi/powerbi-schedule-refresh.png)



## <a name="next-steps"></a>Sonraki adımlar
* Hakkında bilgi edinin [günlük aramaları](../log-query/log-query-overview.md) Power BI'a aktarılabilir sorguları oluşturmak için.
* Daha fazla bilgi edinin [Power BI](https://powerbi.microsoft.com) Azure İzleyici günlük dışarı aktarmaya temel görselleştirmeler oluşturun.