---
title: "Azure üzerinde bir SQL Server sanal makine keşfedin | Microsoft Docs"
description: "Verileri araştırmak ve özellikleri Azure üzerinde bir SQL Server sanal makine oluştur"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: 
ms.assetid: 3949fb2c-ffab-49fb-908d-27d5e42f743b
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: garye;bradsev
ms.openlocfilehash: 6a97e1afb761191874b7a54b1951cb6ef9c4b07e
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/03/2018
---
# <a name="heading"></a>SQL Server sanal makinede Azure üzerinde işlem verileri
Bu belge, verileri araştırmak ve Azure üzerinde bir SQL Server VM depolanan veri için özellikleri oluşturmak alınmaktadır. Bu SQL kullanarak veri wrangling veya Python gibi bir programlama dili kullanılarak yapılabilir.

> [!NOTE]
> Bu belgede örnek SQL deyimlerinde veri SQL Server'da olduğunu varsayın. Öyle değilse, SQL Server'a verilerinizi taşıma hakkında bilgi edinmek için bulut veri bilimi işlem eşlemesi bakın.
> 
> 

## <a name="SQL"></a>SQL kullanarak
Biz SQL kullanarak bu bölümdeki aşağıdaki veri wrangling görevleri açıklar:

1. [Veri keşfi](#sql-dataexploration)
2. [Özellik oluşturma](#sql-featuregen)

### <a name="sql-dataexploration"></a>Veri keşfi
Burada, SQL Server veri depolarında keşfetmek için kullanılabilecek birkaç örnek SQL komut dosyaları bulunmaktadır.

> [!NOTE]
> Pratik bir örnek için kullandığınız [NYC ücreti dataset](http://www.andresmh.com/nyctaxitrips/) ve başlıklı IPNB başvuruda [IPython not defteri ve SQL Server kullanarak NYC veri wrangling](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) bir uçtan uca gözden geçirme.
> 
> 

1. Gün başına gözlemleri sayısını alın
   
    `SELECT CONVERT(date, <date_columnname>) as date, count(*) as c from <tablename> group by CONVERT(date, <date_columnname>)` 
2. Kategorik bir sütunda düzeylerini Al
   
    `select  distinct <column_name> from <databasename>`
3. İki kategorik sütun arada düzey sayısını Al 
   
    `select <column_a>, <column_b>,count(*) from <tablename> group by <column_a>, <column_b>`
4. Sayısal sütunlar için dağıtım Al
   
    `select <column_name>, count(*) from <tablename> group by <column_name>`

### <a name="sql-featuregen"></a>Özellik oluşturma
Bu bölümde, SQL kullanarak özellik oluşturmanın yolları açıklanmaktadır:  

1. [Özellik oluşturma sayısına dayalı](#sql-countfeature)
2. [Binning özelliği oluşturma](#sql-binningfeature)
3. [Tek bir sütundan özellikleri alınıyor](#sql-featurerollout)

> [!NOTE]
> Ek özellikler oluşturmak sonra bunları varolan tablonun sütun olarak ekleyin veya özgün tabloyla katılabilir birincil anahtar ve ek özellikler ile yeni bir tablo oluşturun. 
> 
> 

### <a name="sql-countfeature"></a>Özellik oluşturma sayısına dayalı
Aşağıdaki örnekler sayısı özellikleri oluşturmanın iki yolu gösterir. İlk yöntem Koşullu Toplama ve ikinci yöntem 'where' yan tümcesi kullanır. Bunlar ardından (birincil anahtar sütunlarını kullanarak) özgün tabloyla özgün verilerin yanı sıra sayısı özelliğiniz katılabilir.

    select <column_name1>,<column_name2>,<column_name3>, COUNT(*) as Count_Features from <tablename> group by <column_name1>,<column_name2>,<column_name3> 

    select <column_name1>,<column_name2> , sum(1) as Count_Features from <tablename> 
    where <column_name3> = '<some_value>' group by <column_name1>,<column_name2> 

### <a name="sql-binningfeature"></a>Binning özelliği oluşturma
Aşağıdaki örnek, bir özellik olarak bunun yerine kullanılabilir sayısal bir sütun (beş depo kullanarak) binning binned özellikleri oluşturmak nasıl gösterir:

    `SELECT <column_name>, NTILE(5) OVER (ORDER BY <column_name>) AS BinNumber from <tablename>`


### <a name="sql-featurerollout"></a>Tek bir sütundan özellikleri alınıyor
Bu bölümde nasıl ek özellikler oluşturmak için bir tablo tek bir sütunda çıkışı alınacağı gösterilmektedir. Örnek özellikleri Oluştur çalıştığınız tabloda enlem veya boylam bir sütun olduğunu varsayar.

Enlem/boylam konumu verileri kısa öncü İşte (stackoverflow kaynak var [enlem ve boylam doğruluğunu ölçme?](http://gis.stackexchange.com/questions/8650/how-to-measure-the-accuracy-of-latitude-and-longitude)). Bu konum alanı önce featurizing anlamak kullanışlıdır:

* Oturum açma bize olup olmadığını biz Kuzey Güney, Doğu ya veya Batı üzerinde dünya söyler.
* Sıfır olmayan bir yüzlerce basamağın yuvarlanacağını belirtir bize boylam değil enlem kullanıyorsanız!
* On basamak yaklaşık 1.000 kilometre için bir konum sağlar. Bize ne kıtada veya üzerinde duyuyoruz Okyanusu hakkında yararlı bilgiler sağlar.
* Birimleri basamak (bir ondalık derece) 111 kilometre (60 Deniz mili, yaklaşık 69 mil) bir konum sağlar. Bu kabaca hangi büyük durumunun veya biz bulunan ülke bize.
* En fazla 11.1 km ilk ondalık yerdir: komşu büyük Şehir'dan büyük bir şehir konumunu ayırt edebilirsiniz.
* İkinci ondalık en fazla 1.1 km yerdir: sonraki bir village ayırabilirsiniz.
* Üçüncü ondalık en fazla 110 m: büyük agricultural alan veya Kurumsal kampüs tanımlayabilirsiniz yerdir.
* Dördüncü ondalık 11 m: kara paket tanımlayabilirsiniz yerdir. Hiçbir girişim ile karşılaştırılabilir bir düzeltilemeyen GPS birimi tipik doğruluğu için.
* Beşinci ondalık en fazla 1.1 m: ağaçları birbirinden ayırt yerdir. Bu düzey ticari GPS birimleri ile doğruluğu yalnızca fark düzeltmeyle elde edilebilir.
* Altıncı ondalık en fazla 0.11 m: Bu yapıları Windows'un, tasarlamak için ayrıntılı yerleştirmek için yollar oluşturma kullanabilirsiniz yerdir. Glaciers ve rivers hareketlerini izlemek için birden çok iyi yeterli olmalıdır. Bu differentially düzeltilmiş GPS gibi GPS ile painstaking ölçüleri gerçekleştirerek elde edilebilir.

Konum bilgileri featurized bölge, konum ve Şehir bilgilerini ayırma aşağıdaki gibi olabilir. Bing Haritalar API'si adresinde gibi bir REST uç noktası da çağırabilirsiniz Not [noktası tarafından bir konum bulun](https://msdn.microsoft.com/library/ff701710.aspx) bölge/bölge bilgileri alınacak.

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

Bu konum temelli özellikleri, daha önce açıklandığı gibi ek sayısı özellikleri oluşturmak için daha fazla kullanılabilir. 

> [!TIP]
> Program aracılığıyla dilinizi tercih kullanarak kayıtları ekleyebilirsiniz. Veri yazma verimliliğini artırmak için yığınlar halinde eklemek gerekebilir (pyodbc kullanarak bunu nasıl bir örnek için bkz: [SQLServer python ile erişmek için A HelloWorld örnek](https://code.google.com/p/pypyodbc/wiki/A_HelloWorld_sample_to_access_mssql_with_python)). Verileri kullanarak veritabanı eklemek için başka bir alternatiftir [BCP yardımcı programı](https://msdn.microsoft.com/library/ms162802.aspx).
> 
> 

### <a name="sql-aml"></a>Azure Machine Learning ile bağlanma
Yeni oluşturulan özellik bir sütun olarak var olan bir tabloya eklenebilir veya yeni bir tabloda depolanır ve machine learning için özgün tabloyla katılmış. Özellikler oluşturulan ya da zaten oluşturduysanız, kullanılarak erişilir [veri içeri aktarma] [ import-data] aşağıda gösterildiği gibi Azure Machine Learning modülünde:

![azureml okuyucuları][1] 

## <a name="python"></a>Python gibi programlama dilini kullanma
Verileri araştırmak ve verileri SQL Server olduğunda özellikleri oluşturmak için Python kullanarak benzer açıklandığı gibi Python kullanarak Azure blob veri işleme için [veri bilimi ortamınızdaki işlem Azure Blob veri](data-blob.md). Veriler veritabanından pandas veri çerçeveye yüklenmesi gerektiğini ve ardından daha fazla işlenebilir. Biz, veritabanına bağlanmak ve bu bölümdeki verileri çerçevesine veri yükleme işleminde belge.

Aşağıdaki bağlantı dizesi biçimi Python pyodbc (Değiştir servername, dbname, kullanıcı adı ve parola, belirli değerleri içeren) kullanarak bir SQL Server veritabanına bağlanmak için kullanılabilir:

    #Set up the SQL Azure connection
    import pyodbc    
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

[Pandas Kitaplığı](http://pandas.pydata.org/) Python içinde Python programlama için veri işleme için zengin bir veri yapılarını ve veri çözümleme araçları sağlar. Aşağıdaki kod, bir SQL Server veritabanından Pandas veri çerçeveye sonuçları döndüren okur:

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <cloumnname2>... from <tablename>''', conn)

Makalede ele alınan olarak Pandas veri çerçevesi ile çalışabilir artık [veri bilimi ortamınızdaki işlem Azure Blob veri](data-blob.md).

## <a name="azure-data-science-in-action-example"></a>Azure veri bilimi eylem örnekte
Azure veri bilimi ortak bir veri kümesini kullanarak işlem uçtan uca kılavuz örneği için bkz: [Azure veri bilimi eylem işleminde](sql-walkthrough.md).

[1]: ./media/sql-server-virtual-machine/reader_db_featurizedinput.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/

