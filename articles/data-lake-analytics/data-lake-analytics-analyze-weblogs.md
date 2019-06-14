---
title: Azure Data Lake Analytics'i kullanarak Web sitesi günlüklerini çözümleme
description: Data Lake Analytics'i kullanarak Web sitesi günlüklerini analiz etmeyi öğrenin.
services: data-lake-analytics
author: saveenr
ms.author: saveenr
ms.reviewer: jasonwhowell
ms.assetid: 3a196735-d0d9-4deb-ba68-c4b3f3be8403
ms.service: data-lake-analytics
ms.topic: conceptual
ms.date: 12/05/2016
ms.openlocfilehash: 83742a4f82fb4d67fd258ff0d242847eab634c78
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60334123"
---
# <a name="analyze-website-logs-using-azure-data-lake-analytics"></a>Azure Data Lake Analytics'i kullanarak Web sitesi günlüklerini çözümleme
Web sitesini ziyaret ederek açmaya çalıştığınızda hatalarla karşılaşırsanız hangi başvuran bitmiştir özellikle bulmaya Data Lake Analytics'i kullanarak Web sitesi günlüklerini analiz etmeyi öğrenin.

## <a name="prerequisites"></a>Önkoşullar
* **Visual Studio 2015 veya Visual Studio 2013'ün**.
* **[Visual Studio için Data Lake Araçları](https://aka.ms/adltoolsvs)** .

    Visual Studio için Data Lake araçları yüklendikten sonra göreceğiniz bir **Data Lake** öğesi **Araçları** Visual Studio menüsünde:

    ![U-SQL Visual Studio menü](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-menu.png)
* **Data Lake Analytics ve Visual Studio için Data Lake araçları temel bilgiye**. Başlamak için bkz:

  * [Visual Studio için Data Lake Araçları'nı kullanarak U-SQL betiği geliştirme](data-lake-analytics-data-lake-tools-get-started.md).
* **Bir Data Lake Analytics hesabı.**  Bkz: [bir Azure Data Lake Analytics hesabı oluşturma](data-lake-analytics-get-started-portal.md).
* **Örnek verileri yükleyin.** Data Lake Analytics hesabı, Azure Portalı'nda açın ve tıklayın **örnek betikler** sol taraftaki menüde, ardından **örnek verileri Kopyala**. 

## <a name="connect-to-azure"></a>Azure'a Bağlanma
Derleme ve test herhangi bir U-SQL betikleri önce Azure için önce bağlanmanız gerekir.

**Data Lake Analytics'e bağlanmak için**

1. Visual Studio'yu açın.
2. Tıklayın **Data Lake > Seçenekler ve ayarlar**.
3. Tıklayın **oturum**, veya **Kullanıcı Değiştir** birisi oturum açtığını ve yönergeleri izleyin.
4. Tıklayın **Tamam** seçenekler ve Ayarlar iletişim kutusunu kapatmak için.

**Data Lake Analytics hesaplarınızın göz atmak için**

1. Visual Studio'da açın **Sunucu Gezgini** press tarafından **CTRL + ALT + S**.
2. **Sunucu Gezgini**'nden, **Azure** seçeneğini ve ardından **Data Lake Analytics** seçeneğini genişletin. Varsa Data Lake Analytics hesaplarınızın listesini görürsünüz. Studio'dan Data Lake Analytics hesabı oluşturamazsınız. Hesap oluşturmak için bkz. [Azure Portal'ı kullanarak Azure Data Lake Analytics ile çalışmaya başlama](data-lake-analytics-get-started-portal.md) veya [Azure PowerShell'i kullanarak Azure Data Lake Analytics ile çalışmaya başlama](data-lake-analytics-get-started-powershell.md).

## <a name="develop-u-sql-application"></a>U-SQL uygulaması geliştirme
Çoğunlukla bir U-SQL betiği bir U-SQL uygulamasıdır. U-SQL hakkında daha fazla bilgi için bkz: [U-SQL ile çalışmaya başlama](data-lake-analytics-u-sql-get-started.md).

Ayrıca kullanıcı tanımlı işleçler uygulamaya ekleyebilirsiniz.  Daha fazla bilgi için [geliştirme U-SQL kullanıcı tanımlı işleçler Data Lake Analytics işleri için](data-lake-analytics-u-sql-develop-user-defined-operators.md).

**Data Lake Analytics işi oluşturma ve gönderme**

1. Tıklayın **Dosya > Yeni > Proje**.
2. U-SQL projesi türünü seçin.

    ![yeni U-SQL Visual Studio projesi](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-new-project.png)
3. **Tamam**'ı tıklatın. Visual studio Script.usql dosyasıyla bir çözüm oluşturur.
4. Script.usql dosyasına aşağıdaki betiği girin:

        // Create a database for easy reuse, so you don't need to read from a file every time.
        CREATE DATABASE IF NOT EXISTS SampleDBTutorials;

        // Create a Table valued function. TVF ensures that your jobs fetch data from the weblog file with the correct schema.
        DROP FUNCTION IF EXISTS SampleDBTutorials.dbo.WeblogsView;
        CREATE FUNCTION SampleDBTutorials.dbo.WeblogsView()
        RETURNS @result TABLE
        (
            s_date DateTime,
            s_time string,
            s_sitename string,
            cs_method string,
            cs_uristem string,
            cs_uriquery string,
            s_port int,
            cs_username string,
            c_ip string,
            cs_useragent string,
            cs_cookie string,
            cs_referer string,
            cs_host string,
            sc_status int,
            sc_substatus int,
            sc_win32status int,
            sc_bytes int,
            cs_bytes int,
            s_timetaken int
        )
        AS
        BEGIN

            @result = EXTRACT
                s_date DateTime,
                s_time string,
                s_sitename string,
                cs_method string,
                cs_uristem string,
                cs_uriquery string,
                s_port int,
                cs_username string,
                c_ip string,
                cs_useragent string,
                cs_cookie string,
                cs_referer string,
                cs_host string,
                sc_status int,
                sc_substatus int,
                sc_win32status int,
                sc_bytes int,
                cs_bytes int,
                s_timetaken int
            FROM @"/Samples/Data/WebLog.log"
            USING Extractors.Text(delimiter:' ');
            RETURN;
        END;

        // Create a table for storing referrers and status
        DROP TABLE IF EXISTS SampleDBTutorials.dbo.ReferrersPerDay;
        @weblog = SampleDBTutorials.dbo.WeblogsView();
        CREATE TABLE SampleDBTutorials.dbo.ReferrersPerDay
        (
            INDEX idx1
            CLUSTERED(Year ASC)
            DISTRIBUTED BY HASH(Year)
        ) AS

        SELECT s_date.Year AS Year,
            s_date.Month AS Month,
            s_date.Day AS Day,
            cs_referer,
            sc_status,
            COUNT(DISTINCT c_ip) AS cnt
        FROM @weblog
        GROUP BY s_date,
                cs_referer,
                sc_status;

    U-SQL anlamak için bkz: [Data Lake Analytics U-SQL dili ile çalışmaya başlama](data-lake-analytics-u-sql-get-started.md).    
5. Yeni bir U-SQL betiği projenize ekleyin ve şunu girin:

        // Query the referrers that ran into errors
        @content =
            SELECT *
            FROM SampleDBTutorials.dbo.ReferrersPerDay
            WHERE sc_status >=400 AND sc_status < 500;

        OUTPUT @content
        TO @"/Samples/Outputs/UnsuccessfulResponses.log"
        USING Outputters.Tsv();
6. İlk U-SQL betiği ve yanındaki geri geçiş **Gönder** düğmesi, Analytics hesabınızı belirtin.
7. **Çözüm Gezgini**'nden, **Script.usql** öğesine sağ tıklayın ve ardından **Betik Oluştur**'a tıklayın. Çıkış bölmesinde sonuçları doğrulayın.
8. **Çözüm Gezgini**'nden, **Script.usql** öğesine sağ tıklayın ve ardından **Betiği Gönder**'e tıklayın.
9. Doğrulama **Analytics hesabı** işini çalıştırın ve ardından istediğiniz sertifikadır **Gönder**. Gönderim tamamlandığında, gönderme işleminin sonuçları ve iş bağlantısı Visual Studio için Data Lake Araçları içinde sunulur.
10. İş başarıyla tamamlanana kadar bekleyin.  İş başarısız olduysa, büyük olasılıkla kaynak dosyası eksik.  Lütfen bu öğreticinin önkoşul bölümüne bakın. Ek sorun giderme bilgileri için bkz. [İzleyici ve Azure Data Lake Analytics işlerini sorun giderme](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md).

    İş tamamlandığında, aşağıdaki ekranı göreceksiniz:

    ![Data lake Analytics'i Web günlüklerini Web sitesi günlüklerini çözümleme](./media/data-lake-analytics-analyze-weblogs/data-lake-analytics-analyze-weblogs-job-completed.png)
11. Şimdi için 7 - 10 adımları tekrarlayarak **Script1.usql**.

**İş çıkışını görmek için**

1. **Sunucu Gezgini**'nden, **Azure** seçeneğini, **Data Lake Analytics** seçeneğini, Data Lake Analytics hesabınızı ve **Depolama Hesapları** seçeneğini genişletin, varsayılan Data Lake Store hesabına sağ tıklayın ve ardından **Explorer**'a tıklayın.
2. Çift **örnekleri** klasörünü açın ve ardından çift **çıkışları**.
3. Çift **UnsuccessfulResponses.log**.
4. Ayrıca, çıkışa doğrudan gitmek için graf görünümünü işin içinde çıkış dosyasını çift tıklatabilirsiniz.

## <a name="see-also"></a>Ayrıca bkz.
Farklı araçlar kullanarak Data Lake Analytics ile çalışmaya başlamak için bkz.

* [Azure Portal'ı kullanarak Data Lake Analytics ile çalışmaya başlama](data-lake-analytics-get-started-portal.md)
* [Azure PowerShell'i kullanarak Data Lake Analytics ile çalışmaya başlama](data-lake-analytics-get-started-powershell.md)
* [.NET SDK'yı kullanarak Data Lake Analytics ile çalışmaya başlama](data-lake-analytics-get-started-net-sdk.md)
