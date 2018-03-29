---
title: SQL ve Python kullanarak SQL Server verileri için özellikler oluşturmak | Microsoft Docs
description: SQL Azure işlem verileri
services: machine-learning
documentationcenter: ''
author: bradsev
manager: cgronlun
editor: ''
ms.assetid: bf1f4a6c-7711-4456-beb7-35fdccd46a44
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/21/2017
ms.author: bradsev
ms.openlocfilehash: 5eaa7b5b30617cabedc7ed15a8fc7b174ecc68f2
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="create-features-for-data-in-sql-server-using-sql-and-python"></a>SQL ve Python kullanarak SQL Server’daki verilerin özelliklerini oluşturma
Bu belgede daha verimli bir şekilde verilerden öğrenmeyi algoritmaları Yardım Azure üzerinde bir SQL Server VM depolanan veriler için özellikleri oluşturmak nasıl gösterir. SQL veya Python gibi bir programlama dili, bu görevi gerçekleştirmek için kullanabilirsiniz. Her iki yaklaşımın burada gösterilmiştir.

[!INCLUDE [cap-create-features-data-selector](../../../includes/cap-create-features-selector.md)]

Bu **menü** özellikleri veriler için çeşitli ortamlar oluşturmak nasıl açıklayan konulara bağlantılar. Bu görev bir adımdır [takım veri bilimi işlem (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

> [!NOTE]
> Pratik bir örnek için başvurabilirsiniz [NYC ücreti dataset](http://www.andresmh.com/nyctaxitrips/) ve başlıklı IPNB başvuruda [IPython not defteri ve SQL Server kullanarak NYC veri wrangling](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) bir uçtan uca gözden geçirme.
> 
> 

## <a name="prerequisites"></a>Önkoşullar
Bu makalede, sahip olduğunuz varsayılmaktadır:

* Bir Azure depolama hesabı oluşturuldu. Yönergeler gerekiyorsa bkz [bir Azure depolama hesabı oluşturma](../../storage/common/storage-create-storage-account.md#create-a-storage-account)
* Verilerinizi SQL Server içinde depolanır. Yüklemediyseniz, bkz: [veri taşıma bir Azure SQL veritabanına Azure Machine Learning için](move-sql-azure.md) verileri var. taşımak yönergeler için.

## <a name="sql-featuregen"></a>SQL ile özelliği oluşturma
Bu bölümde, SQL kullanarak özellik oluşturmanın yolları açıklanmaktadır:  

1. [Özellik oluşturma sayısına dayalı](#sql-countfeature)
2. [Binning özelliği oluşturma](#sql-binningfeature)
3. [Tek bir sütundan özellikleri alınıyor](#sql-featurerollout)

> [!NOTE]
> Ek özellikler oluşturmak sonra bunları varolan tablonun sütun olarak ekleyin veya özgün tabloyla katılabilir birincil anahtar ve ek özellikler ile yeni bir tablo oluşturun.
> 
> 

### <a name="sql-countfeature"></a>Temel sayısı özelliği oluşturma
Bu belge sayısı özellikleri oluşturmanın iki yolu gösterir. İlk yöntem Koşullu Toplama ve ikinci yöntem 'where' yan tümcesi kullanır. Bunlar ardından (birincil anahtar sütunlarını kullanarak) özgün tabloyla özgün verilerin yanı sıra sayısı özelliğiniz katılabilir.

    select <column_name1>,<column_name2>,<column_name3>, COUNT(*) as Count_Features from <tablename> group by <column_name1>,<column_name2>,<column_name3>

    select <column_name1>,<column_name2> , sum(1) as Count_Features from <tablename>
    where <column_name3> = '<some_value>' group by <column_name1>,<column_name2>

### <a name="sql-binningfeature"></a>Binning özelliği oluşturma
Aşağıdaki örnek, bir özellik olarak bunun yerine kullanılabilir sayısal bir sütun (5 depo kullanarak) binning binned özellikleri oluşturmak nasıl gösterir:

    `SELECT <column_name>, NTILE(5) OVER (ORDER BY <column_name>) AS BinNumber from <tablename>`


### <a name="sql-featurerollout"></a>Tek bir sütundan özellikleri alınıyor
Bu bölümde nasıl ek özellikler oluşturmak için bir tablo tek bir sütunda çıkışı alınacağı gösterilmektedir. Örnek özellikleri Oluştur çalıştığınız tabloda enlem veya boylam bir sütun olduğunu varsayar.

Enlem/boylam konumu verileri kısa öncü İşte (stackoverflow kaynak var `http://gis.stackexchange.com/questions/8650/how-to-measure-the-accuracy-of-latitude-and-longitude`). Konum verileri hakkında alanından özellikleri oluşturmadan önce anlamak için yararlı bazı şeyleri burada bulabilirsiniz:

* Oturum açma olup olmadığını biz Kuzey Güney, Doğu ya veya Batı dünya üzerindeki gösterir.
* Sıfır olmayan bir yüzlerce basamak boylam gösterir, değil enlem kullanılıyor.
* On basamak yaklaşık 1.000 kilometre için bir konum sağlar. Hangi kıtada veya üzerinde duyuyoruz Okyanusu hakkında yararlı bilgiler sağlar.
* Birimleri basamak (bir ondalık derece) 111 kilometre (60 Deniz mili, yaklaşık 69 mil) bir konum sağlar. Bu, kabaca, hangi büyük durumu veya biz bulunan ülke gösterir.
* En fazla 11.1 km ilk ondalık yerdir: komşu büyük Şehir'dan büyük bir şehir konumunu ayırt edebilirsiniz.
* İkinci ondalık en fazla 1.1 km yerdir: sonraki bir village ayırabilirsiniz.
* Üçüncü ondalık en fazla 110 m: büyük agricultural alan veya Kurumsal kampüs tanımlayabilirsiniz yerdir.
* Dördüncü ondalık 11 m: kara paket tanımlayabilirsiniz yerdir. Hiçbir girişim ile karşılaştırılabilir bir düzeltilemeyen GPS birimi tipik doğruluğu için.
* Beşinci ondalık en fazla 1.1 m: ağaçları birbirinden ayırt yerdir. Bu düzey ticari GPS birimleri ile doğruluğu yalnızca fark düzeltmeyle elde edilebilir.
* Altıncı ondalık en fazla 0.11 m: Bu yapıları Windows'un, tasarlamak için ayrıntılı yerleştirmek için yollar oluşturma kullanabilirsiniz yerdir. Glaciers ve rivers hareketlerini izlemek için birden çok iyi yeterli olmalıdır. Bu differentially düzeltilmiş GPS gibi GPS ile painstaking ölçüleri gerçekleştirerek elde edilebilir.

Konum bilgileri, bölge, konum ve Şehir bilgilerini ayırarak featurized olabilir. Bir kez Bing Haritalar API'si adresinde gibi bir REST uç noktası da çağırabilir Not `https://msdn.microsoft.com/library/ff701710.aspx` bölge/bölge bilgileri alınacak.

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

Bu konum tabanlı özellikleri daha fazla ek sayısı özellikleri daha önce açıklandığı gibi oluşturmak için kullanılabilir.

> [!TIP]
> Program aracılığıyla dilinizi tercih kullanarak kayıtları ekleyebilirsiniz. Veri yazma verimliliğini artırmak için yığınlar halinde eklemek gerekebilir. [Örneği pyodbc kullanarak bunu nasıl](https://code.google.com/p/pypyodbc/wiki/A_HelloWorld_sample_to_access_mssql_with_python).
> Verileri kullanarak veritabanı eklemek için başka bir alternatiftir [BCP yardımcı programı](https://msdn.microsoft.com/library/ms162802.aspx)
> 
> 

### <a name="sql-aml"></a>Azure Machine Learning ile bağlanma
Yeni oluşturulan özellik bir sütun olarak var olan bir tabloya eklenebilir veya yeni bir tabloda depolanır ve machine learning için özgün tabloyla katılmış. Özellikler oluşturulan ya da zaten oluşturduysanız, kullanılarak erişilir [veri içeri aktarma](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) aşağıda gösterildiği gibi Azure ML modülünde:

![Azure ML okuyucuları](./media/sql-server-virtual-machine/reader_db_featurizedinput.png)

## <a name="python"></a>Python gibi programlama dilini kullanma
Veriler SQL Server'da olduğunda özellikleri oluşturmak için Python kullanarak Python kullanarak Azure blob veri işleme için benzer. Karşılaştırma için bkz: [veri bilimi ortamınızdaki işlem Azure Blob veri](data-blob.md). Verileri veritabanından daha fazla işlem için bir pandas veri çerçevesine yükleyin. Veritabanına bağlanma ve verileri çerçeveye veri yükleme işlemi, bu bölümünde belgelenmiştir.

Aşağıdaki bağlantı dizesi biçimi Python pyodbc (Değiştir servername, dbname, kullanıcı adı ve parola, belirli değerleri içeren) kullanarak bir SQL Server veritabanına bağlanmak için kullanılabilir:

    #Set up the SQL Azure connection
    import pyodbc
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

[Pandas Kitaplığı](http://pandas.pydata.org/) Python içinde Python programlama için veri işleme için zengin bir veri yapılarını ve veri çözümleme araçları sağlar. Aşağıdaki kod, bir SQL Server veritabanından Pandas veri çerçeveye sonuçları döndüren okur:

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <cloumnname2>... from <tablename>''', conn)

Konular, kapsanan olarak Pandas veri çerçevesi ile çalışabilir artık [Panda kullanarak Azure blob depolama verileri için özellikler oluşturmak](create-features-blob.md).

