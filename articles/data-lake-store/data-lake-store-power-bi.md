---
title: "Power BI'ı kullanarak Data Lake Store'da verilerin çözümleme | Microsoft Docs"
description: "Azure Data Lake Store içinde depolanan verileri çözümlemek için Power BI'ı kullanın"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 57d19d27-e135-49d9-a7ea-46c48ef4e3bd
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/28/2017
ms.author: nitinme
ms.openlocfilehash: 0c77ccd29e3e22005c3339ed4e89dccfed725fa0
ms.sourcegitcommit: 651a6fa44431814a42407ef0df49ca0159db5b02
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2017
---
# <a name="analyze-data-in-data-lake-store-by-using-power-bi"></a>Power BI'ı kullanarak Data Lake Store'da verilerin çözümleme
Bu makalede, Power BI Desktop çözümlemek ve Azure Data Lake Store içinde depolanan verileri görselleştirmek için nasıl kullanılacağını öğreneceksiniz.

## <a name="prerequisites"></a>Ön koşullar
Bu öğreticiye başlamadan önce aşağıdakilere sahip olmanız gerekir:

* **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).
* **Azure Data Lake Store hesabı**. [Azure Portal'ı kullanarak Azure Data Lake Store ile çalışmaya başlama](data-lake-store-get-started-portal.md) bölümündeki yönergeleri uygulayın. Bu makalede adlı bir Data Lake Store hesabı zaten oluşturduğunuzu varsayar **mybidatalakestore**ve bir örnek veri dosyasını karşıya (**Drivers.txt**) kendisine. Bu örnek dosya Merkezi'nden edinilebilir [Azure Data Lake Git deposu](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/Drivers.txt).
* **Power BI Desktop**. Buradan indirebilirsiniz [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=45331). 

## <a name="create-a-report-in-power-bi-desktop"></a>Power BI Desktop'ta rapor oluşturma
1. Power BI Desktop bilgisayarınızda başlatın.
2. Gelen **giriş** Şerit, tıklatın **Veri Al**ve daha fazlasını'ı tıklatın. İçinde **Veri Al** iletişim kutusu, tıklatın **Azure**, tıklatın **Azure Data Lake Store**ve ardından **Bağlan**.
   
    ![Data Lake Store'a bağlama](./media/data-lake-store-power-bi/get-data-lake-store-account.png "Data Lake Store'a bağlama")
3. Bir geliştirme aşamasında olan Bağlayıcısı hakkında bir iletişim kutusu görürseniz, devam etmek için kabul et.
4. İçinde **Microsoft Azure Data Lake Store** iletişim kutusu, Data Lake Store hesabınızın URL'sini girin ve ardından **Tamam**.
   
    ![Data Lake Store için URL](./media/data-lake-store-power-bi/get-data-lake-store-account-url.png "Data Lake Store için URL")
5. Sonraki iletişim kutusunda tıklatın **oturum** Data Lake Store dikkate imzalamak için. Kuruluşunuzun oturum açma sayfasına yönlendirilirsiniz. Hesaba imzalamak için istemleri izleyin.
   
    ![Data Lake Store içine oturum](./media/data-lake-store-power-bi/get-data-lake-store-account-signin.png "oturum Data Lake Store açın")
6. Başarıyla oturum açtıktan sonra tıklayın **Bağlan**.
   
    ![Data Lake Store'a bağlama](./media/data-lake-store-power-bi/get-data-lake-store-account-connect.png "Data Lake Store'a bağlama")
7. Sonraki iletişim kutusunda, Data Lake Store hesabınıza karşıya dosya gösterir. Bilgileri doğrulayın ve ardından **yük**.
   
    ![Data Lake Deposu'ndan veri veri yükleme](./media/data-lake-store-power-bi/get-data-lake-store-account-load.png "veri Data Lake Deposu'ndan veri yükleme")
8. Verileri Power BI başarıyla yüklendikten sonra aşağıdaki alanlarda görürsünüz **alanları** sekmesi.
   
    ![Alınan alanları](./media/data-lake-store-power-bi/imported-fields.png "içe alanlar")
   
    Ancak, görselleştirin ve verileri çözümlemek için biz verilerin aşağıdaki alanlar kullanılabilir tercih
   
    ![Alanları istenen](./media/data-lake-store-power-bi/desired-fields.png "alanları istenen")
   
    İçeri aktarılan verileri istediğiniz biçimde dönüştürmek üzere sorgu sonraki adımlarda güncelleştireceğiz.
9. Gelen **giriş** Şerit, tıklatın **Düzenle sorguları**.
   
    ![Sorguları düzenleme](./media/data-lake-store-power-bi/edit-queries.png "sorgularını düzenleme")
10. Sorgu Düzenleyicisi'nde, altında **içerik** sütun tıklatın **ikili**.
    
    ![Sorguları düzenleme](./media/data-lake-store-power-bi/convert-query1.png "sorgularını düzenleme")
11. Temsil eden bir dosya simgesini görürsünüz **Drivers.txt** karşıya dosya. Dosyayı sağ tıklayın ve **CSV**.    
    
    ![Sorguları düzenleme](./media/data-lake-store-power-bi/convert-query2.png "sorgularını düzenleme")
12. Aşağıda gösterildiği gibi bir çıktı görmeniz gerekir. Verilerinizi görsel öğeleri oluşturmak için kullanabileceğiniz bir biçimde kullanıma sunulmuştur.
    
    ![Sorguları düzenleme](./media/data-lake-store-power-bi/convert-query3.png "sorgularını düzenleme")
13. Gelen **giriş** Şerit, tıklatın **kapatın ve geçerli**ve ardından **kapatın ve geçerli**.
    
    ![Sorguları düzenleme](./media/data-lake-store-power-bi/load-edited-query.png "sorgularını düzenleme")
14. Sorgu güncelleştirildikten sonra **alanları** sekmesini görselleştirme için kullanılabilir yeni alanları gösterir.
    
    ![Alanları güncelleştirilmiş](./media/data-lake-store-power-bi/updated-query-fields.png "alanları güncelleştirildi")
15. Bize her şehir için belirli bir ülke sürücüleri temsil etmek için bir pasta grafik oluşturun. Bunu yapmak için aşağıdaki seçimleri yapın.
    
    1. Görselleştirmeleri sekmesinden pasta grafiği için simgeyi tıklatın.
       
        ![Pasta grafiği oluşturmak](./media/data-lake-store-power-bi/create-pie-chart.png "pasta grafik oluşturma")
    2. Biz kullanacaksanız sütunlar **sütun 4** (şehrin adı) ve **sütun 7** (ülke adı). Bu sütunlardan sürükleyin **alanları** için sekme **görselleştirmeleri** sekmesinde aşağıda gösterildiği gibi.
       
        ![Görselleştirmelerinin](./media/data-lake-store-power-bi/create-visualizations.png "görselleştirmeler oluşturun")
    3. Pasta grafik şimdi aşağıda gösterilene benzer benzemelidir.
       
        ![Pasta grafik](./media/data-lake-store-power-bi/pie-chart.png "görselleştirmeler oluşturun")
16. Belirli bir ülkeye sayfa düzeyi filtreleri seçerek, her şehir seçilen ülkenin sürücüleri sayısını şimdi görebilirsiniz. Örneğin, altında **görselleştirmeleri** sekmesinde, altında **sayfa düzeyi filtreleri**seçin **Brezilya**.
    
    ![Bir ülke seçin](./media/data-lake-store-power-bi/select-country.png "bir ülke seçin")
17. Pasta grafik Brezilya şehirlerde sürücüleri görüntülemek için otomatik olarak güncelleştirilir.
    
    ![Sürücüleri bir ülkede](./media/data-lake-store-power-bi/driver-per-country.png "ülke başına sürücüleri")
18. Gelen **dosya** menüsünde tıklatın **kaydetmek** görselleştirme Power BI Desktop dosyası olarak kaydetmek için.

## <a name="publish-report-to-power-bi-service"></a>Rapor Power BI hizmetinde yayımlama
Power BI Desktop'ta görselleştirmeleri oluşturduktan sonra bunu başkalarıyla Power BI hizmetinde yayımlayarak paylaşabilirsiniz. Bunun hakkında daha fazla yönerge için bkz: [Power BI masaüstünden Yayımla](https://powerbi.microsoft.com/documentation/powerbi-desktop-upload-desktop-files/).

## <a name="see-also"></a>Ayrıca bkz.
* [Data Lake Analytics kullanarak Data Lake Store içinde verileri analiz etme](../data-lake-analytics/data-lake-analytics-get-started-portal.md)

