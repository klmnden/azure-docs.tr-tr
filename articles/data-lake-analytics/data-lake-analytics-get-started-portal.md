---
title: "Azure portalını kullanarak Azure Data Lake Analytics ile çalışmaya başlama | Microsoft Docs"
description: "Bir Data Lake Analytics hesabı oluşturmak, U-SQL&quot;yi kullanarak Data Lake Analytics işi oluşturmak ve bu işi göndermek için Azure portalının nasıl kullanılacağını öğrenin. "
services: data-lake-analytics
documentationcenter: 
author: edmacauley
manager: jhubbard
editor: cgronlun
ms.assetid: b1584d16-e0d2-4019-ad1f-f04be8c5b430
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/21/2017
ms.author: edmaca
translationtype: Human Translation
ms.sourcegitcommit: b0c27ca561567ff002bbb864846b7a3ea95d7fa3
ms.openlocfilehash: 64c5869f3e66c249fefa9af228fe1b33974cf293
ms.lasthandoff: 04/25/2017


---
# <a name="tutorial-get-started-with-azure-data-lake-analytics-using-azure-portal"></a>Öğretici: Azure portalını kullanarak Azure Data Lake Analytics ile çalışmaya başlama
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Azure Data Lake Analytics hesapları oluşturmak, [U-SQL](data-lake-analytics-u-sql-get-started.md) içinde işler tanımlamak ve Data Lake Analytics hizmetine iş göndermek için Azure portalının nasıl kullanılacağını öğrenin. Data Lake Analytics hakkında daha fazla bilgi için bkz. [Azure Data Lake Analytics'e genel bakış](data-lake-analytics-overview.md).

Bu öğreticide, bir sekmeyle ayrılmış değerler (TSV) dosyasını okuyan ve bunu virgülle ayrılmış değerler (CSV) dosyasına dönüştüren bir iş geliştireceksiniz. Öğreticiyi desteklenen diğer araçları kullanarak tamamlamak için bu bölümün üst kısmındaki sekmelere tıklayın. İlk işiniz başarılı olduktan sonra, U-SQL ile daha karmaşık veri dönüşümleri yazmaya başlayabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar
Bu öğreticiye başlamadan önce aşağıdaki öğelere sahip olmanız gerekir:

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).

## <a name="create-data-lake-analytics-account"></a>Data Lake Analytics hesabı oluşturma
Herhangi bir işi çalıştırmadan önce bir Data Lake Analytics hesabına sahip olmanız gerekir.

Her Data Lake Analytics hesabı, bir Azure Data Lake Store hesabı bağımlılığına sahiptir.  Bu hesap, varsayılan Data Lake Store hesabı olarak adlandırılır.  Data Lake Store hesabını önceden veya Data Lake Analytics hesabınızı oluşturduğunuzda oluşturabilirsiniz. Bu öğreticide, Data Lake Store hesabını Data Lake Analytics hesabıyla oluşturacaksınız.

**Data Lake Analytics hesabı oluşturma**

1. [Azure portalı](https://portal.azure.com) üzerinde oturum açın.
2. **Yeni**’ye, **Zeka + analiz**’e ve ardından **Data Lake Analytics**'e tıklayın.
3. Aşağıdaki değerleri yazın veya seçin:

    ![Azure Data Lake Analytics portalı dikey penceresi](./media/data-lake-analytics-get-started-portal/data-lake-analytics-portal-create-adla.png)

   * **Ad**: Data Lake Analytics hesabınızı adlandırın (Yalnızca küçük harf ve sayı kullanılabilir).
   * **Abonelik**: Analytics hesabı için kullanılan Azure aboneliğini seçin.
   * **Kaynak Grubu**. Var olan bir Azure Kaynak Grubu'nu seçin veya yeni bir grup oluşturun. Azure Resource Manager, uygulamanızdaki kaynaklarla bir grup olarak çalışmanıza olanak sağlar. Daha fazla bilgi için bkz. [Azure Resource Manager'a Genel Bakış](../azure-resource-manager/resource-group-overview.md).
   * **Konum**. Data Lake Analytics hesabı için bir Azure veri merkezi seçin.
   * **Data Lake Store**: *Gerekli ayarları yapılandır*’a tıklayın. Yeni bir Data Lake Store hesabı oluşturmaya yönelik yönergeyi uygulayın veya var olan bir hesabı seçin. Her Data Lake Analytics hesabı, bağımlı bir Data Lake Store hesabına sahiptir. Data Lake Analytics hesabı ve bağımlı Data Lake Store hesabı aynı Azure veri merkezinde bulunmalıdır.
4. Fiyatlandırma Katmanınızı seçme  
5. **Oluştur**’a tıklayın. Bu seçenek sizi "Azure Data Lake Analytics Dağıtılıyor" iletisini gösteren yeni bir kutucuğun görüntülendiği portal giriş sayfasına döndürür. Dağıtım işleminin bir Data Lake Analytics hesabı oluşturması birkaç dakika sürer. Hesap oluşturulduğunda, portal, hesabı yeni bir dikey pencerede açar.

Data Lake Analytics hesabı oluşturulduktan sonra, ek Data Lake Store hesapları ve Azure Storage hesapları ekleyebilirsiniz. Yönergeler için bkz. [Data Lake Analytics hesabı veri kaynaklarını yönetme](data-lake-analytics-manage-use-portal.md#manage-account-data-sources).

## <a name="prepare-source-data"></a>Kaynak verileri hazırlama
Bu öğreticide, arama günlüklerini işleyeceksiniz.  Arama günlüğü, Data Lake Store veya Azure Blob depolama alanında depolanabilir.

Azure portalı, bir arama günlüğü dosyası içeren örnek veri dosyalarını varsayılan Data Lake Store hesabına kopyalamak için bir kullanıcı arabirimi sağlar.

**Örnek veri dosyalarını kopyalama**

1. [Azure portalından](https://portal.azure.com) Data Lake Analytics hesabınızı açın.  Portalda bir hesap oluşturmak ve hesabı açmak için bkz. [Data Lake Analytics hesaplarını yönetme](data-lake-analytics-get-started-portal.md#create-data-lake-analytics-account).
2. **Temel Bileşenler** bölmesini genişletin ve ardından **Örnek betikleri keşfedin** seçeneğine tıklayın. Bu işlem sonucunda, **Örnek Betikler** adlı başka bir dikey pencere açılır.

    ![Azure Data Lake Analytics portalı örnek betik](./media/data-lake-analytics-get-started-portal/data-lake-analytics-portal-sample-scripts.png)
3. Örnek veri dosyalarını kopyalamak için **Örnek Veri Yok**’a tıklayın. İşlem tamamlandığında portal **Örnek veriler başarıyla güncelleştirildi** iletisini görüntüler.
4. Data Lake Analytics hesabı dikey penceresinden, üstteki **Veri Gezgini**'ne tıklayın.

    ![Azure Data Lake Analytics veri gezgini penceresi](./media/data-lake-analytics-get-started-portal/data-lake-analytics-data-explorer-button.png)

    Bu işlem sonucunda iki dikey pencere açılır. Bunlardan biri **Veri Gezgini**, diğeri ise varsayılan Data Lake Store hesabıdır.
5. Varsayılan Data Lake Store hesabı dikey penceresinde, klasörü genişletmek için **Örnekler** seçeneğine, klasörü genişletmek için de **Veri** seçeneğine tıklayın. Aşağıdaki dosyaları ve klasörleri göreceksiniz:

   * AmbulanceData/
   * AdsLog.tsv
   * SearchLog.tsv
   * version.txt
   * WebLog.log

     Bu öğreticide, SearchLog.tsv dosyasını kullanacaksınız.

Uygulamada, uygulamalarınızı bağlı depolama hesaplarına veri yazmak veya veri yüklemek üzere programlayacaksınız. Dosya yükleme için bkz. [Data Lake Store'a veri yükleme](data-lake-analytics-manage-use-portal.md#upload-data-to-adls) veya [Blob depolama alanına veri yükleme](data-lake-analytics-manage-use-portal.md#upload-data-to-wasb).

## <a name="create-and-submit-data-lake-analytics-jobs"></a>Data Lake Analytics işleri oluşturma ve gönderme
Veri kaynağını hazırladıktan sonra, U-SQL betiği geliştirmeye başlayabilirsiniz.  

**İş göndermek için**

1. Portaldaki Data Lake Analytics hesabı dikey penceresinden **Yeni İş**'e tıklayın.

    ![Azure Data Lake Analytics yeni iş düğmesi](./media/data-lake-analytics-get-started-portal/data-lake-analytics-new-job-button.png)

    Dikey pencereyi görmüyorsanız bkz. [Portaldan Data Lake Analytics hesabı açma](data-lake-analytics-manage-use-portal.md#access-adla-account).
2. **İş Adı**'nı ve şu U-SQL betiğini girin:

        @searchlog =
            EXTRACT UserId          int,
                    Start           DateTime,
                    Region          string,
                    Query           string,
                    Duration        int?,
                    Urls            string,
                    ClickedUrls     string
            FROM "/Samples/Data/SearchLog.tsv"
            USING Extractors.Tsv();

        OUTPUT @searchlog   
            TO "/Output/SearchLog-from-Data-Lake.csv"
        USING Outputters.Csv();

    ![Azure Data Lake Analytics U-SQL işleri oluşturma](./media/data-lake-analytics-get-started-portal/data-lake-analytics-new-job.png)

    Bu U-SQL betiği, **Extractors.Tsv()** öğesini kullanarak kaynak veri dosyasını okur ve ardından **Outputters.Csv()** öğesini kullanarak bir csv dosyası oluşturur.

    Kaynak dosyayı farklı bir konuma kopyalamadıkça bu iki yolu değiştirmeyin.  Data Lake Analytics, mevcut olmaması halinde çıkış klasörünü oluşturur.  Bu durumda biz basit, göreli yolları kullanıyoruz.  

    Varsayılan Data Lake hesaplarında depolanan dosyalar için göreli yolların kullanılması daha basittir. Mutlak yol da kullanabilirsiniz.  Örneğin:

        adl://<Data LakeStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv

    U-SQL hakkında daha fazla bilgi için bkz. [Azure Data Lake Analytics U-SQL dili ile çalışmaya başlama](data-lake-analytics-u-sql-get-started.md) ve [U-SQL dili başvurusu](http://go.microsoft.com/fwlink/?LinkId=691348).

1. Üstteki **İşi Gönder** seçeneğine tıklayın.   
2. İş durumunun **Başarılı** olarak değiştirilmesini bekleyin. İşin tamamlanmasının yaklaşık bir dakika sürdüğünü görebilirsiniz.

    İş başarısız olduysa bkz. [Data Lake Analytics işlerini izleme ve sorun giderme](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md).
3. Dikey pencerenin altındaki **Çıkış** sekmesine ve ardından **SearchLog-from-Data-Lake.csv**’ye tıklayın. Çıktı dosyasını indirebilir, yeniden adlandırabilir ve silebilirsiniz.

    ![Azure Data Lake Analytics işi çıktı dosyası özellikleri](./media/data-lake-analytics-get-started-portal/data-lake-analytics-output-file-properties.png)

## <a name="see-also"></a>Ayrıca bkz.
* Daha karmaşık bir sorgu görmek için [Azure Data Lake Analytics'i kullanarak Web sitesi günlüklerini çözümleme](data-lake-analytics-analyze-weblogs.md) makalesine bakın.
* U-SQL uygulamalarını geliştirmeye başlamak için bkz. [Visual Studio için Data Lake Araçları'nı kullanarak U-SQL betikleri geliştirme](data-lake-analytics-data-lake-tools-get-started.md).
* U-SQL öğrenmek için bkz. [Azure Data Lake Analytics U-SQL dili ile çalışmaya başlama](data-lake-analytics-u-sql-get-started.md).
* Yönetim görevleri için bkz. [Azure portalı kullanarak Azure Data Lake Analytics'i yönetme](data-lake-analytics-manage-use-portal.md).
* Data Lake Analytics'e yönelik bir genel bakış için bkz. [Azure Data Lake Analytics'e genel bakış](data-lake-analytics-overview.md).
* Aynı öğreticiyi diğer araçları kullanarak görmek için sayfanın üst kısmındaki sekme seçicilerine tıklayın.
* Tanılama bilgilerini günlüğe kaydetmek için bkz. [Azure Data Lake Analytics için tanılama günlüklerine erişme](data-lake-analytics-diagnostic-logs.md)

