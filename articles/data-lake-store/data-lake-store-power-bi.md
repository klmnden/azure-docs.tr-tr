---
title: Power BI'ı kullanarak Azure Data Lake depolama Gen1 verileri çözümleme | Microsoft Docs
description: Azure Data Lake depolama Gen1 depolanan verileri analiz etmek için Power BI'ı kullanma
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.assetid: 57d19d27-e135-49d9-a7ea-46c48ef4e3bd
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: d8717b8f365e692b5f27bf8a04d65c5147b8f31b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65603201"
---
# <a name="analyze-data-in-azure-data-lake-storage-gen1-by-using-power-bi"></a>Power BI'ı kullanarak Azure Data Lake depolama Gen1 verileri çözümleme
Bu makalede çözümlemek ve Azure Data Lake depolama Gen1 depolanan verileri görselleştirmek için Power BI Desktop kullanmayı öğreneceksiniz.

## <a name="prerequisites"></a>Önkoşullar
Bu öğreticiye başlamadan önce aşağıdakilere sahip olmanız gerekir:

* **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).
* **Bir Data Lake depolama Gen1 hesabı**. Konumundaki yönergeleri [Azure Data Lake depolama Gen1 ile çalışmaya başlama Azure portalını kullanarak](data-lake-store-get-started-portal.md). Bu makalede adlı bir Data Lake depolama Gen1 hesabı zaten oluşturmuş olduğunuzu varsayar **myadlsg1**ve bir örnek veri dosyası (**Drivers.txt**) ona. Bu örnek dosya sitesinden indirilebilir [Azure Data Lake Git deposu](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/Drivers.txt).
* **Power BI Desktop**. Buradan indirebileceğiniz [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=45331). 

## <a name="create-a-report-in-power-bi-desktop"></a>Power BI Desktop'ta rapor oluşturma
1. Power BI Desktop, bilgisayarınızda başlatın.
2. Gelen **giriş** Şerit ye **Veri Al**ve ardından daha fazla. İçinde **Veri Al** iletişim kutusu, tıklayın **Azure**, tıklayın **Azure Data Lake Store**ve ardından **Connect**.
   
    ![Bağlanmak için Data Lake depolama Gen1](./media/data-lake-store-power-bi/get-data-lake-store-account.png "bağlanmak için Data Lake depolama Gen1")
3. Devam etmek için bir geliştirme aşamasında olan Bağlayıcısı hakkında bir iletişim kutusu görürseniz, iyileştirilmiş.
4. İçinde **Azure Data Lake Store** iletişim kutusu, Data Lake depolama Gen1 hesabınıza URL sağlayın ve ardından **Tamam**.
   
    ![Data Lake depolama Gen1 URL'sini](./media/data-lake-store-power-bi/get-data-lake-store-account-url.png "Data Lake depolama Gen1 URL'si")
5. Sonraki iletişim kutusunda tıklayın **oturum** Data Lake depolama Gen1 hesabında oturum açabilir. Kuruluşunuzun oturum açma sayfasına yönlendirilirsiniz. Hesapta oturum için istemleri izleyin.
   
    ![Data Lake depolama Gen1 oturum](./media/data-lake-store-power-bi/get-data-lake-store-account-signin.png "Data Lake depolama Gen1'nda oturum açın")
6. Başarıyla oturum açtıktan sonra tıklayın **Connect**.
   
    ![Bağlanmak için Data Lake depolama Gen1](./media/data-lake-store-power-bi/get-data-lake-store-account-connect.png "bağlanmak için Data Lake depolama Gen1")
7. Sonraki iletişim kutusunda, Data Lake depolama Gen1 hesabınıza yüklediğiniz dosyayı gösterir. Bilgileri doğrulayın ve ardından **yük**.
   
    ![Data Lake depolama Gen1 veri yükleme](./media/data-lake-store-power-bi/get-data-lake-store-account-load.png "Data Lake depolama Gen1 veri yükleme")
8. Verileri başarıyla Power BI'a yüklendikten sonra aşağıdaki alanlarda göreceğiniz **alanları** sekmesi.
   
    ![İçeri aktarılan alanları](./media/data-lake-store-power-bi/imported-fields.png "alınan alanları")
   
    Ancak, Görselleştirme ve veri çözümleme için aşağıdaki alanlar kullanılabilir olması için veri tercih ediyoruz
   
    ![Alanları istenen](./media/data-lake-store-power-bi/desired-fields.png "alanları istenen")
   
    Sonraki adımlarda, istenen biçimde içeri aktarılan verileri dönüştürmek için sorguyu güncelleştireceğiz.
9. Gelen **giriş** Şerit ye **sorguları Düzenle**.
   
    ![Sorguları Düzenle](./media/data-lake-store-power-bi/edit-queries.png "sorguları Düzenle")
10. Sorgu Düzenleyicisi'nde, altında **içerik** sütun tıklayın **ikili**.
    
    ![Sorguları Düzenle](./media/data-lake-store-power-bi/convert-query1.png "sorguları Düzenle")
11. Temsil eden bir dosya simgesi görürsünüz **Drivers.txt** karşıya yüklenen dosya. Dosyaya sağ tıklayın ve **CSV**.    
    
    ![Sorguları Düzenle](./media/data-lake-store-power-bi/convert-query2.png "sorguları Düzenle")
12. Aşağıda gösterildiği gibi bir çıktı görmeniz gerekir. Veri görselleştirmeleri oluşturmak için kullanabileceğiniz bir biçimde kullanıma sunuldu.
    
    ![Sorguları Düzenle](./media/data-lake-store-power-bi/convert-query3.png "sorguları Düzenle")
13. Gelen **giriş** Şerit ye **Kapat ve Uygula**ve ardından **Kapat ve Uygula**.
    
    ![Sorguları Düzenle](./media/data-lake-store-power-bi/load-edited-query.png "sorguları Düzenle")
14. Sorgu güncelleştirildikten sonra **alanları** sekmesi, görselleştirme için kullanılabilir yeni alanları gösterilir.
    
    ![Alanlar](./media/data-lake-store-power-bi/updated-query-fields.png "alanlar")
15. Bize her şehir için belirli bir ülke/bölge sürücüleri temsil etmek için bir pasta grafik oluşturun. Bunu yapmak için aşağıdaki seçimleri yapın.
    
    1. Görsel öğeler sekmesinde, pasta grafiği simgesini tıklayın.
       
        ![Pasta grafiği oluşturma](./media/data-lake-store-power-bi/create-pie-chart.png "pasta grafiği oluşturma")
    2. Kullanılacak kullanacağız sütunlar **sütun 4** (Şehir adı) ve **sütun 7** (ülke/bölge adı). Bu sütunları sürükleyin **alanları** için sekmesinde **görselleştirmeler** sekmesine aşağıda gösterildiği gibi.
       
        ![Görselleştirmeler oluşturma](./media/data-lake-store-power-bi/create-visualizations.png "görselleştirmeler oluşturma")
    3. Pasta grafiği, aşağıda gösterilene benzer artık benzemelidir.
       
        ![Pasta grafiği](./media/data-lake-store-power-bi/pie-chart.png "görselleştirmeler oluşturma")
16. Sayfa düzeyi filtreleri belirli bir ülke/bölge seçerek artık sürücüler seçilen ülke/bölge, her şehirde sayısını görebilirsiniz. Örneğin, altında **görselleştirmeler** sekmesindeki **sayfa düzeyi filtreleri**seçin **Brezilya**.
    
    ![Ülke seçin](./media/data-lake-store-power-bi/select-country.png "bir ülke/bölge seçin")
17. Pasta grafiği, Brezilya şehirlerin sürücüleri görüntülemek için otomatik olarak güncelleştirilir.
    
    ![Sürücüleri bir ülkede](./media/data-lake-store-power-bi/driver-per-country.png "ülke/bölge başına sürücüleri")
18. Gelen **dosya** menüsünde tıklatın **Kaydet** görselleştirmeyi Power BI Desktop dosyası olarak kaydetmek için.

## <a name="publish-report-to-power-bi-service"></a>Raporu Power BI hizmetinde yayımlayın
Power BI Desktop'ta görselleştirmeler oluşturduktan sonra bunu diğer kullanıcılarla Power BI hizmetinde yayımlayarak paylaşabilirsiniz. Bunu yapmak yönergeler için bkz: [Power BI Desktop'tan yayımlama](https://powerbi.microsoft.com/documentation/powerbi-desktop-upload-desktop-files/).

## <a name="see-also"></a>Ayrıca bkz.
* [Data Lake depolama Gen1 Data Lake Analytics'i kullanarak verileri analiz etme](../data-lake-analytics/data-lake-analytics-get-started-portal.md)

