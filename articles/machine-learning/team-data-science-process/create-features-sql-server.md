---
title: "SQL ve Python kullanarak SQL Server verileri için özellikler oluşturmak | Microsoft Docs"
description: "SQL Azure işlem verileri"
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: 
ms.assetid: bf1f4a6c-7711-4456-beb7-35fdccd46a44
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev;fashah;garye
ms.openlocfilehash: 06c165d25361694cf660f391b3d221ad1d63e95d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-features-for-data-in-sql-server-using-sql-and-python"></a>SQL ve Python kullanarak SQL Server’daki verilerin özelliklerini oluşturma
Bu belgede daha verimli bir şekilde verilerden öğrenmeyi algoritmaları Yardım Azure üzerinde bir SQL Server VM depolanan veriler için özellikleri oluşturmak nasıl gösterir. Bu SQL kullanarak veya Python, her ikisi de aşağıda gösterildiği gibi bir programlama dili kullanılarak yapılabilir.

[!INCLUDE [cap-create-features-data-selector](../../../includes/cap-create-features-selector.md)]

Bu **menü** özellikleri veriler için çeşitli ortamlar oluşturmak nasıl açıklayan konulara bağlantılar. Bu görev bir adımdır [takım veri bilimi işlem (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

> [!NOTE]
> Pratik bir örnek için başvurabilirsiniz [NYC ücreti dataset](http://www.andresmh.com/nyctaxitrips/) ve başlıklı IPNB başvuruda [IPython not defteri ve SQL Server kullanarak NYC veri wrangling](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) bir uçtan uca gözden geçirme.
> 
> 

## <a name="prerequisites"></a>Ön koşullar
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

### <a name="sql-countfeature"></a>Özellik oluşturma sayısına dayalı
Bu belge sayısı özellikleri oluşturmanın iki yolu gösterir. İlk yöntem Koşullu Toplama ve ikinci yöntem 'where' yan tümcesi kullanır. Bunlar ardından (birincil anahtar sütunlarını kullanarak) özgün tabloyla özgün verilerin yanı sıra sayısı özelliğiniz katılabilir.

    select <column_name1>,<column_name2>,<column_name3>, COUNT(*) as Count_Features from <tablename> group by <column_name1>,<column_name2>,<column_name3>

    select <column_name1>,<column_name2> , sum(1) as Count_Features from <tablename>
    where <column_name3> = '<some_value>' group by <column_name1>,<column_name2>

### <a name="sql-binningfeature"></a>Binning özelliği oluşturma
Aşağıdaki örnek, bir özellik olarak bunun yerine kullanılabilir sayısal bir sütun (5 depo kullanarak) binning binned özellikleri oluşturmak nasıl gösterir:

    `SELECT <column_name>, NTILE(5) OVER (ORDER BY <column_name>) AS BinNumber from <tablename>`


### <a name="sql-featurerollout"></a>Tek bir sütundan özellikleri alınıyor
Bu bölümde, nasıl tek bir sütun ek özellikler oluşturmak için bir tabloda üretimini gösterilmektedir. Örnek özellikleri Oluştur çalıştığınız tabloda enlem veya boylam bir sütun olduğunu varsayar.

Enlem/boylam konumu verileri kısa öncü İşte (stackoverflow kaynak var `http://gis.stackexchange.com/questions/8650/how-to-measure-the-accuracy-of-latitude-and-longitude`). Bu konum alanı önce featurizing anlamak kullanışlıdır:

* Oturum açma bize olup olmadığını biz Kuzey Güney, Doğu ya veya Batı üzerinde dünya söyler.
* Sıfır olmayan bir yüzlerce basamağın yuvarlanacağını belirtir bize biz boylam değil enlem kullanmakta olduğunuz!
* On basamak yaklaşık 1.000 kilometre için bir konum sağlar. Bize ne kıtada veya üzerinde duyuyoruz Okyanusu hakkında yararlı bilgiler sağlar.
* Birimleri basamak (bir ondalık derece) 111 kilometre (60 Deniz mili, yaklaşık 69 mil) bir konum sağlar. Bu kabaca hangi büyük durumunun veya biz bulunan ülke bize.
* En fazla 11.1 km ilk ondalık yerdir: komşu büyük Şehir'dan büyük bir şehir konumunu ayırt edebilirsiniz.
* İkinci ondalık en fazla 1.1 km yerdir: sonraki bir village ayırabilirsiniz.
* Üçüncü ondalık en fazla 110 m: büyük agricultural alan veya Kurumsal kampüs tanımlayabilirsiniz yerdir.
* Dördüncü ondalık 11 m: kara paket tanımlayabilirsiniz yerdir. Hiçbir girişim ile karşılaştırılabilir bir düzeltilemeyen GPS birimi tipik doğruluğu için.
* En fazla 1.1 m: beşinci ondalık basamak olduğu ağaçlar birbirinden ayırt etmek. Bu düzey ticari GPS birimleri ile doğruluğu yalnızca fark düzeltmeyle elde edilebilir.
* Altıncı ondalık en fazla 0.11 m: Bu yapıları Windows'un, tasarlamak için ayrıntılı yerleştirmek için yollar oluşturma kullanabilirsiniz yerdir. Glaciers ve rivers hareketlerini izlemek için birden çok iyi yeterli olmalıdır. Bu differentially düzeltilmiş GPS gibi GPS ile painstaking ölçüleri gerçekleştirerek elde edilebilir.

Konum bilgileri featurized gibi bölge, konum ve Şehir bilgilerini ayırma olabilir. Bir kez Bing Haritalar API'si adresinde gibi bir REST uç noktası da çağırabilir Not `https://msdn.microsoft.com/library/ff701710.aspx` bölge/bölge bilgileri alınacak.

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

Yukarıdaki göre konumuna özellikleri daha fazla ek sayısı özellikleri daha önce açıklandığı gibi oluşturmak için kullanılabilir.

> [!TIP]
> Program aracılığıyla dilinizi tercih kullanarak kayıtları ekleyebilirsiniz. Veri yazma verimliliğini artırmak için yığınlar halinde eklemek gerekebilir [pyodbc burada kullanarak bunu nasıl örnek denetleyin](https://code.google.com/p/pypyodbc/wiki/A_HelloWorld_sample_to_access_mssql_with_python).
> Verileri kullanarak veritabanı eklemek için başka bir alternatiftir [BCP yardımcı programı](https://msdn.microsoft.com/library/ms162802.aspx)
> 
> 

### <a name="sql-aml"></a>Azure Machine Learning ile bağlanma
Yeni oluşturulan özellik bir sütun olarak var olan bir tabloya eklenebilir veya yeni bir tabloda depolanır ve machine learning için özgün tabloyla katılmış. Özellikler oluşturulan ya da zaten oluşturduysanız, kullanılarak erişilir [veri içeri aktarma](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) aşağıda gösterildiği gibi Azure ML modülünde:

![azureml okuyucuları](./media/sql-server-virtual-machine/reader_db_featurizedinput.png)

## <a name="python"></a>Python gibi programlama dilini kullanma
Veriler SQL Server'da olduğunda özellikleri oluşturmak için Python kullanarak benzer açıklandığı gibi Python kullanarak Azure blob veri işleme için [, veri bilimi ortamında işlem Azure Blob verileri](data-blob.md). Veriler veritabanından pandas veri çerçeveye yüklenmesi gerektiğini ve ardından daha fazla işlenebilir. Biz, veritabanına bağlanmak ve bu bölümdeki verileri çerçevesine veri yükleme işleminde belge.

Aşağıdaki bağlantı dizesi biçimi Python pyodbc (Değiştir servername, dbname, kullanıcı adı ve parola, belirli değerleri içeren) kullanarak bir SQL Server veritabanına bağlanmak için kullanılabilir:

    #Set up the SQL Azure connection
    import pyodbc
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

[Pandas Kitaplığı](http://pandas.pydata.org/) Python içinde Python programlama için veri işleme için zengin bir veri yapılarını ve veri çözümleme araçları sağlar. Aşağıdaki kod, bir SQL Server veritabanından Pandas veri çerçeveye sonuçları döndüren okur:

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <cloumnname2>... from <tablename>''', conn)

Konular, kapsanan olarak Pandas veri çerçevesi ile çalışabilir artık [Panda kullanarak Azure blob depolama verileri için özellikler oluşturmak](create-features-blob.md).

