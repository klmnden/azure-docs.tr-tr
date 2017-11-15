---
title: "Örnek veri Azure üzerinde SQL Server'daki | Microsoft Docs"
description: "Azure SQL Server'da örnek veri"
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgeonlun
editor: cgronlun
ms.assetid: 33c030d4-5cca-4cc9-99d7-2bd13a3926af
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2017
ms.author: fashah;garye;bradsev
ms.openlocfilehash: fd669f3951b1f7f05932634f039a04e02993399f
ms.sourcegitcommit: 659cc0ace5d3b996e7e8608cfa4991dcac3ea129
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/13/2017
---
# <a name="heading"></a>Azure üzerinde SQL Server örnek veri
Bu makalede Azure SQL sunucusunda depolanan verileri örnek SQL veya Python programlama dili kullanılarak gösterilmektedir. Ayrıca, bir dosyaya kaydetmeyi, bir Azure blob karşıya yükleme ve Azure Machine Learning Studio'ya okuma Azure Machine Learning örneklenen verileri taşımak nasıl gösterir.

Python örnekleme kullandığı [pyodbc](https://code.google.com/p/pyodbc/) Azure SQL sunucusuna bağlanmak için ODBC kitaplığı ve [Pandas](http://pandas.pydata.org/) örnekleme yapmak için kitaplık.

> [!NOTE]
> Örnek SQL kod bu belgedeki verileri Azure SQL Server'da olduğunu varsayar. Değilse, başvurmak [veri taşıma SQL Server için Azure üzerinde](move-sql-server-virtual-machine.md) makale verilerinizi Azure üzerinde SQL Server'a taşıma konusunda yönergeler için.
> 
> 

Aşağıdaki **menü** çeşitli depolama ortamlarından veri örneği anlatmaktadır makalelerinin bağlantıları. 

[!INCLUDE [cap-sample-data-selector](../../../includes/cap-sample-data-selector.md)]

**Neden verilerinizi örnek?**
Analiz etmek için planlama dataset büyükse, genellikle aşağı örnek için daha küçük, ancak temsili ve daha kolay yönetilebilir bir boyutunu azaltmak için veri için iyi bir fikir değil. Bu, veri anlama, keşfi ve özellik Mühendisliği kolaylaştırır. Kendi rolünde [takım veri bilimi işlem (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) hızlı prototipi oluşturulurken veri işleme işlevleri ve makine öğrenimi modellerinin oluşturulmasına olanak sağlamasıdır.

Bir adımda bu örnekleme görevdir [takım veri bilimi işlem (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

## <a name="SQL"></a>SQL kullanarak
Bu bölümde SQL kullanarak basit rastgele örnekleme veri karşı veritabanında gerçekleştirmek için çeşitli yöntemler açıklanmaktadır. Veri boyutu ve onun dağıtım dayalı bir yöntem seçin.

Aşağıdaki iki öğeyi nasıl kullanılacağını gösteren `newid` örnekleme gerçekleştirmek için SQL Server'daki. Seçtiğiniz yöntem nasıl rastgele olması için örnek istediğiniz bağlıdır (Aşağıdaki örnek kodda pk_id olduğu varsayılır otomatik olarak oluşturulan bir birincil anahtar).

1. Daha az kesin rasgele örnek
   
        select  * from <table_name> where <primary_key> in 
        (select top 10 percent <primary_key> from <table_name> order by newid())
2. Daha fazla rasgele örnek 
   
        SELECT * FROM <table_name>
        WHERE 0.1 >= CAST(CHECKSUM(NEWID(), <primary_key>) & 0x7fffffff AS float)/ CAST (0x7fffffff AS int)

Tablesample yanısıra veri örnekleme için kullanılabilir. (Veri sihirbazın farklı sayfalarında değil bağıntılı varsayılarak), veri boyutu büyükse ve makul bir sürede tamamlamak bir sorgu için bu daha iyi bir yaklaşım olabilir.

    SELECT *
    FROM <table_name> 
    TABLESAMPLE (10 PERCENT)

> [!NOTE]
> Keşfetmek ve yeni bir tabloda depolayarak örneklenen bu verilerden Özellikleri Oluştur
> 
> 

### <a name="sql-aml"></a>Azure Machine Learning ile bağlanma
Yukarıdaki örnek sorgular Azure Machine Learning ile doğrudan kullanabilirsiniz [veri içeri aktarma] [ import-data] aşağı-kolay bir şekilde veri örneği ve bir Azure Machine Learning deneme hale getirmek için modülü. Örneklenen verileri okumak için okuyucu modülü kullanarak bir ekran görüntüsü aşağıda gösterilmiştir:

![Okuyucu sql][1]

## <a name="python"></a>Python programlama dili kullanma
Bu bölüm kullanarak gösterir [pyodbc Kitaplığı](https://code.google.com/p/pyodbc/) Python bir SQL server veritabanında bir ODBC bağlantı kurulacak. Veritabanı bağlantı dizesi şu şekildedir: (servername, dbname, kullanıcı adı ve parola yapılandırmanızı değiştirin):

    #Set up the SQL Azure connection
    import pyodbc    
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

[Pandas](http://pandas.pydata.org/) Python kitaplıkta Python programlama için veri işleme için zengin bir veri yapılarını ve veri çözümleme araçları sağlar. Aşağıdaki kodu % 0,1 örnek verileri Azure SQL veritabanındaki bir tablo Pandas verisine okur:

    import pandas as pd

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select column1, cloumn2... from <table_name> tablesample (0.1 percent)''', conn)

Şimdi, örneklenen verileri Pandas veri çerçevesinde çalışabilirsiniz. 

### <a name="python-aml"></a>Azure Machine Learning ile bağlanma
Aşağıdaki örnek kod, aşağı örneklenen verileri bir dosyaya kaydedin ve bir Azure blobuna yüklemek için kullanabilirsiniz. Blob verileri doğrudan bir Azure Machine Learning deneme okunabilir kullanarak [veri içeri aktarma] [ import-data] modülü. Adımlar aşağıdaki gibidir: 

1. Pandas veri çerçevesi yerel bir dosyaya yazma
   
        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)
2. Yerel dosya Azure blobuna yükle
   
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
3. Azure Machine Learning kullanarak Azure blob üzerinden veri okuma [veri içeri aktarma] [ import-data] aşağıdaki ekran Al gösterildiği gibi Modülü:

![Okuyucu blob][2]

## <a name="the-team-data-science-process-in-action-example"></a>Eylem örnekte takım veri bilimi işlemi
Bir ortak bir veri kümesini kullanarak takım veri bilimi işlemi örneği kılavuz için bkz [takım veri bilimi işlemi sürüyor: SQL Server'ı kullanarak](sql-walkthrough.md).

[1]: ./media/sample-sql-server-virtual-machine/reader_database.png
[2]: ./media/sample-sql-server-virtual-machine/reader_blob.png

[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
