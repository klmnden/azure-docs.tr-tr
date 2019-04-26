---
title: SQL ve Python - Team Data Science Process kullanarak SQL Server özelliklerini oluşturma
description: SQL ve Python - Team Data Science Process parçası kullanarak azure'da bir SQL Server VM'si depolanan verilerin özelliklerini oluşturur.
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 11/21/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 2d01b74e7db275f4b2e3933415bbae40911b114b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60399304"
---
# <a name="create-features-for-data-in-sql-server-using-sql-and-python"></a>SQL ve Python kullanarak SQL Server’daki verilerin özelliklerini oluşturma
Bu belge, daha verimli bir şekilde verilerden bilgi algoritmaları yardımcı olan bir SQL Server VM'si, azure'da depolanan verilerin özelliklerini oluşturma adımları anlatılmaktadır. Bu görevi gerçekleştirmek için SQL veya Python gibi bir programlama dili kullanabilirsiniz. Her iki yaklaşım burada gösterilmiştir.

Bu görev bir adımdır [Team Data Science işlem (TDSP)](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/).

> [!NOTE]
> Pratik bir örnek için başvurabilirsiniz [NYC taksi dataset](https://www.andresmh.com/nyctaxitrips/) ve başlıklı IPNB [Ipython Notebook ve SQL Server'ı kullanarak NYC veri denetimi](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) uçtan uca bir kılavuz için.
> 
> 

## <a name="prerequisites"></a>Önkoşullar
Bu makalede, olduğunu varsayar:

* Bir Azure depolama hesabı oluşturuldu. Yönergelere ihtiyacınız varsa bkz [bir Azure depolama hesabı oluşturma](../../storage/common/storage-quickstart-create-account.md)
* Verilerinizi SQL Server'da depolanan. Yüklemediyseniz, bkz. [veri taşıma için bir Azure SQL veritabanı için Azure Machine Learning](move-sql-azure.md) var. veri taşıma konusunda yönergeler için.

## <a name="sql-featuregen"></a>SQL ile özellik oluşturma
Bu bölümde, biz özellikleri SQL kullanarak oluşturma yolları açıklanmaktadır:  

1. [Özellik nesil sayısı tabanlı](#sql-countfeature)
2. [Gruplama özellik oluşturma](#sql-binningfeature)
3. [Tek bir sütundan özellikleri alınıyor](#sql-featurerollout)

> [!NOTE]
> Ek özellikler oluşturduktan sonra bunları mevcut tabloya sütun olarak ekleyin veya özgün tabloyla birleştirilen birincil anahtar ve ek özellikler ile yeni bir tablo oluşturabilirsiniz.
> 
> 

### <a name="sql-countfeature"></a>Temel sayısı özelliği oluşturma
Bu belge sayısı özellikleri oluşturmanın iki yolunu gösterir. İlk yöntem koşullu toplamı ve ikinci yöntem 'where' yan tümcesi kullanır. Bunlar daha sonra (birincil anahtar sütunlarını kullanarak) özgün tablo ile özgün veri yanı sıra sayısı özelliğiniz için katılabilir.

    select <column_name1>,<column_name2>,<column_name3>, COUNT(*) as Count_Features from <tablename> group by <column_name1>,<column_name2>,<column_name3>

    select <column_name1>,<column_name2> , sum(1) as Count_Features from <tablename>
    where <column_name3> = '<some_value>' group by <column_name1>,<column_name2>

### <a name="sql-binningfeature"></a>Gruplama özellik oluşturma
Aşağıdaki örnek, bunun yerine bir özelliği olarak kullanılabilir bir sayısal sütun göre gruplama (5 depo kullanarak) binned özellikleri oluşturma adımları anlatılmaktadır:

    `SELECT <column_name>, NTILE(5) OVER (ORDER BY <column_name>) AS BinNumber from <tablename>`


### <a name="sql-featurerollout"></a>Tek bir sütundan özellikleri alınıyor
Bu bölümde ek özellikler oluşturmak için bir tablodaki tek bir sütun kullanıma sunma gösterilmektedir. Örnek özellikleri oluşturmak çalıştığınız tablosunda bir enlem veya boylam sütunu olduğunu varsayar.

Enlem/boylam konumu veri kısa öncü İşte (stackoverflow kaynak var `https://gis.stackexchange.com/questions/8650/how-to-measure-the-accuracy-of-latitude-and-longitude`). Özellikler alanı oluşturmadan önce ilgili konum verileri anlamak için yararlı işlemlerden bazıları aşağıda verilmiştir:

* Oturum, biz Kuzey olup veya Güney, Doğu veya Batı dünya üzerindeki gösterir.
* Sıfır olmayan bir yüz basamağı belirtir boylam, enlem değil kullanılıyor.
* Onlarca basamaklı bir konuma yaklaşık 1.000 kilometre sağlar. Bu, hangi Kıta veya üzerinde duyuyoruz Okyanusu hakkında yararlı bilgiler verir.
* Birimleri basamak (bir ondalık derece) 111 kilometre (60 Deniz mili, yaklaşık 69 mil) bir konum sağlar. Bu, kabaca, hangi büyük eyalet veya ülke duyuyoruz gösterir.
* En fazla 11.1 km ilk ondalık yerdir: komşu büyük Şehir'dan büyük bir şehir konumunu ayırt edebilir.
* En fazla 1.1 km ikinci ondalık yerdir: sonraki bir village ayırabilirsiniz.
* Üçüncü ondalık en fazla 110 m: büyük Tarım alan veya Kurumsal kampüs tanımlayabilirsiniz yerdir.
* Dördüncü ondalık en fazla 11 m: olan bir paket tanımanıza yerdir. Tipik doğruluğunu düzeltilemeyen bir GPS birim için hiçbir engelleme ile karşılaştırılabilir.
* Beşinci ondalık en fazla 1.1 m: ağaçları birbirinden ayıran yerdir. Bu düzey ticari GPS birimleri ile tutarlılık yalnızca fark düzeltme ile gerçekleştirilebilir.
* Altıncı ondalık en fazla 0.11 m: Bu yapıları ortamlarını, tasarlamak için ayrıntılı olarak yerleştirmek için yollar oluşturma kullanabileceğiniz yerdir. Hareketleri glaciers rivers ve izlemek için birden fazla yeterince iyi olmalıdır. Bu, GPS, differentially düzeltilmiş GPS gibi painstaking ölçülerle yararlanarak gerçekleştirilebilir.

Konum bilgileri, bölge, konum ve Şehir bilgilerini ayrılarak özellikleri tespit olabilir. Bing Haritalar API'si kullanılabilir gibi bir REST uç noktası bir kez de çağırabilirsiniz Not `https://msdn.microsoft.com/library/ff701710.aspx` bölge/bölge bilgileri alınacak.

    select
        <location_columnname>
        ,round(<location_columnname>,0) as l1        
        ,l2=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 1 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),1,1) else '0' end     
        ,l3=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 2 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),2,1) else '0' end     
        ,l4=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 3 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),3,1) else '0' end     
        ,l5=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 4 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),4,1) else '0' end     
        ,l6=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 5 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),5,1) else '0' end     
        ,l7=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 6 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),6,1) else '0' end     
    from <tablename>

Bu konum tabanlı özellikleri, daha fazla ek sayısı özellikleri daha önce açıklandığı gibi oluşturmak için kullanılabilir.

> [!TIP]
> İstediğiniz dilde kullanarak kayıtları program aracılığıyla ekleyebilirsiniz. Veri yazma verimliliğini artırmak için öbekler halinde eklemek gerekebilir. [İşte bir örnek pyodbc kullanarak bunu nasıl](https://code.google.com/p/pypyodbc/wiki/A_HelloWorld_sample_to_access_mssql_with_python).
> Veritabanını kullanarak veri eklemek için başka bir alternatiftir [BCP yardımcı programı](https://msdn.microsoft.com/library/ms162802.aspx)
> 
> 

### <a name="sql-aml"></a>Azure Machine Learning ile bağlanma
Yeni oluşturulan özellik bir sütun olarak var olan bir tabloya eklenebilir veya yeni bir tablo içinde saklanan ve machine learning için özgün tablo ile birleştirilmiş. Özellikleri oluşturulan ya da zaten oluşturduysanız, kullanılarak erişilen [verileri içeri aktarma](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) aşağıda gösterildiği gibi Azure ML modülünde:

![Azure ML okuyucular](./media/sql-server-virtual-machine/reader_db_featurizedinput.png)

## <a name="python"></a>Python gibi bir programlama dilini kullanma
Veriler SQL Server'da olduğunda özellikler oluşturmak için Python kullanarak Python kullanarak Azure blob veri işleme için benzerdir. Karşılaştırma için bkz: [işlem Azure Blob veri, veri bilimi ortamınızdaki](data-blob.md). Verileri veritabanından daha fazla işlemek için bir pandas veri çerçevesine yükleyin. Veritabanına bağlanma ve veri çerçevesine veri yükleme işlemi, bu bölümde belgelenmiştir.

Aşağıdaki bağlantı dizesi biçimi python'dan pyodbc (Değiştir servername, dbname, kullanıcı adı ve parola, belirli değerleri içeren) kullanarak bir SQL Server veritabanına bağlanmak için kullanılabilir:

    #Set up the SQL Azure connection
    import pyodbc
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

[Pandas Kitaplığı](https://pandas.pydata.org/) Python'da Python programlama için veri işleme için zengin bir veri yapıları ve verileri analiz araçları sağlar. Aşağıdaki kod, sonuçları bir SQL Server veritabanından bir Pandas veri çerçevesine döndürülen okur:

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <columnname2>... from <tablename>''', conn)

Pandas veri çerçevesi ile konularda ele gibi çalışabilirsiniz artık [Panda kullanarak Azure blob depolama verilerinin özelliklerini oluşturma](create-features-blob.md).

