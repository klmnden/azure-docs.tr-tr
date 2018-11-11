---
title: Örnek verileri Azure üzerinde SQL Server'da | Microsoft Docs
description: Azure'da SQL Server verileri örneklendirme
services: machine-learning
documentationcenter: ''
author: deguhath
manager: cgeonlun
editor: cgronlun
ms.assetid: 33c030d4-5cca-4cc9-99d7-2bd13a3926af
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2017
ms.author: deguhath
ms.openlocfilehash: 7852a0fc548980227723c9f6a259c63367159201
ms.sourcegitcommit: 96527c150e33a1d630836e72561a5f7d529521b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/09/2018
ms.locfileid: "51346248"
---
# <a name="heading"></a>Azure'da SQL Server verileri örneklendirme

Bu makalede, SQL veya Python programlama dili kullanarak Azure SQL Server'da depolanan verileri örnek kullanmayı gösterir. Ayrıca bir dosyaya kaydetme, bir Azure blobuna yüklemeyi ve ardından Azure Machine Learning Studio'ya okuma tarafından Azure Machine Learning içine örneklenen verileri taşıma işlemini de gösterir.

Python örnekleme kullandığı [pyodbc](https://code.google.com/p/pyodbc/) Azure üzerindeki SQL Server'a bağlanmak için ODBC kitaplığı ve [Pandas](http://pandas.pydata.org/) örnekleme yapmak için kitaplık.

> [!NOTE]
> Örnek SQL kodunu bu belgedeki verileri azure'da bir SQL Server'da olduğunu varsayar. Yüklü değilse, başvurmak [veri taşıma SQL Server için Azure üzerinde](move-sql-server-virtual-machine.md) makale için Azure üzerinde SQL Server'a veri taşıma konusunda yönergeler.
> 
> 

**Neden verilerinizi örnek?**
Veri kümesini analiz etmek için planlama büyükse, genellikle aşağı örnek veriler için daha küçük ancak temsilcisi ve daha kolay yönetilebilir bir boyutunu azaltmak için iyi bir fikir olduğunu. Bu, özellik Mühendisliği veri anlama ve keşfetme kolaylaştırır. Kendi rolünde [Team Data Science işlem (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) hızlı prototip oluşturma veri işleme işlevleri ve makine öğrenimi modellerini etkinleştirmektir.

Bir adımda bu örnekleme görevdir [Team Data Science işlem (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

## <a name="SQL"></a>SQL kullanma
Bu bölümde veritabanındaki verilere yönelik basit rastgele örnekleme gerçekleştirmek için SQL kullanarak çeşitli yöntemler açıklanmaktadır. Veri boyutu ve onun dağıtım göre bir yöntem seçin.

Aşağıdaki iki öğeyi nasıl kullanabileceğinizi gösteren `newid` örnekleme gerçekleştirmek için SQL Server. Seçtiğiniz yöntem nasıl rastgele örnek olması için istediğinize bağlıdır (Aşağıdaki örnek kodda pk_id olduğu varsayılır otomatik olarak oluşturulan bir birincil anahtar).

1. Daha az katı rastgele örnek
   
        select  * from <table_name> where <primary_key> in 
        (select top 10 percent <primary_key> from <table_name> order by newid())
2. Daha fazla rastgele örnek 
   
        SELECT * FROM <table_name>
        WHERE 0.1 >= CAST(CHECKSUM(NEWID(), <primary_key>) & 0x7fffffff AS float)/ CAST (0x7fffffff AS int)

Tablesample verileri de örnekleme için kullanılabilir. Bu (farklı sayfalarında veri değil bağıntılı varsayılarak), veri boyutu büyükse ve sorgu makul bir süre içinde tamamlanması için daha iyi bir yaklaşım olabilir.

    SELECT *
    FROM <table_name> 
    TABLESAMPLE (10 PERCENT)

> [!NOTE]
> Keşfedin ve yeni bir tablo depolayarak örneklenen bu verilerden Özellikleri Oluştur
> 
> 

### <a name="sql-aml"></a>Azure Machine Learning ile bağlanma
Azure Machine Learning'de doğrudan Yukarıdaki örnek sorguları kullanabilirsiniz [verileri içeri aktarma] [ import-data] modülü aşağı örnek verileri hareket halindeyken ve bir Azure Machine Learning denemesine getirin. Örneklenen verileri okumak için okuyucu modülü kullanarak bir ekran görüntüsü aşağıda gösterilmiştir:

![Okuyucu sql][1]

## <a name="python"></a>Python programlama dili kullanma
Bu bölümde kullanmayı gösterir [pyodbc Kitaplığı](https://code.google.com/p/pyodbc/) Python bir SQL server veritabanında bir ODBC bağlantı kurmak için. Veritabanı bağlantı dizesi şu şekildedir: (servername, dbname, kullanıcı adı ve parola yapılandırmanızı değiştirin):

    #Set up the SQL Azure connection
    import pyodbc    
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

[Pandas](http://pandas.pydata.org/) Python kitaplıkta Python programlama için veri işleme için zengin bir veri yapıları ve verileri analiz araçları sağlar. Aşağıdaki kod Pandas verisine %0,1 örnek verileri Azure SQL veritabanındaki bir tablodan okur:

    import pandas as pd

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select column1, cloumn2... from <table_name> tablesample (0.1 percent)''', conn)

Artık, örneklenen verileri Pandas veri çerçevesi ile çalışabilirsiniz. 

### <a name="python-aml"></a>Azure Machine Learning ile bağlanma
Aşağıdaki örnek kod, alt örneklenen verileri bir dosyaya kaydedin ve uygulamayı bir Azure blobuna yüklemek için kullanabilirsiniz. Blob verileri doğrudan Azure Machine Learning denemesine okunabilir kullanarak [verileri içeri aktarma] [ import-data] modülü. Adımlar aşağıdaki gibidir: 

1. Pandas veri çerçevesi yerel bir dosyaya yazma
   
        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)
2. Azure blobuna yerel dosya karşıya yükleme
   
        from azure.storage import BlobService
        import tables
   
        STORAGEACCOUNTNAME= <storage_account_name>
        LOCALFILENAME= <local_file_name>
        STORAGEACCOUNTKEY= <storage_account_key>
        CONTAINERNAME= <container_name>
        BLOBNAME= <blob_name>
   
        output_blob_service=BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)    
        localfileprocessed = os.path.join(os.getcwd(),LOCALFILENAME) #assuming file is in current working directory
   
        try:
   
        #perform upload
        output_blob_service.put_block_blob_from_path(CONTAINERNAME,BLOBNAME,localfileprocessed)
   
        except:            
            print ("Something went wrong with uploading blob:"+BLOBNAME)
3. Azure Machine Learning kullanarak Azure BLOB'dan veri okuma [verileri içeri aktarma] [ import-data] aşağıdaki ekran tutamaçları gösterildiği Modülü:

![Okuyucu blob][2]

## <a name="the-team-data-science-process-in-action-example"></a>Team Data Science Process eylem örnekte
Adım adım kılavuz kullanarak genel bir veri kümesi Team Data Science Process örneği görmek [Team Data Science Process'in çalışması: SQL Server'ı kullanarak](sql-walkthrough.md).

[1]: ./media/sample-sql-server-virtual-machine/reader_database.png
[2]: ./media/sample-sql-server-virtual-machine/reader_blob.png

[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
