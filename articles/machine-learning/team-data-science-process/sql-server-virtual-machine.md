---
title: Bir SQL Server sanal makinesinde - Team Data Science Process verilerini keşfedin
description: Verileri araştırmak ve özellikleri azure'da bir SQL Server sanal makinesi oluşturma
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/23/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 6adc5dfa740d440e78bf2f276447c4585503d7c0
ms.sourcegitcommit: 6ea7f0a6e9add35547c77eef26f34d2504796565
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65606476"
---
# <a name="heading"></a>Azure'da SQL Server sanal makinesi verilerini işleme
Bu belge verileri araştırmak ve bir SQL Server VM'si, azure'da depolanan verilerin özelliklerini oluşturma konusunu kapsar. Bu SQL kullanarak veri denetimi veya Python gibi bir programlama dili kullanılarak yapılabilir.

> [!NOTE]
> Bu belgede örnek SQL deyimlerinde veri SQL Server'da olduğunu varsayalım. Aksi takdirde, verilerinizi SQL Server'a taşıma hakkında bilgi edinmek için bulut veri bilimi işlemi eşlemesi bakın.
> 
> 

## <a name="SQL"></a>SQL kullanma
Biz, SQL kullanarak bu bölümdeki aşağıdaki veri wrangling görevler açıklanmıştır:

1. [Veri keşfi](#sql-dataexploration)
2. [Özellik oluşturma](#sql-featuregen)

### <a name="sql-dataexploration"></a>Veri keşfi
SQL Server veri depolarında keşfetmek için kullanılan SQL betiklerini birkaç örnek aşağıda verilmiştir.

> [!NOTE]
> Pratik bir örnek için kullandığınız [NYC taksi dataset](https://www.andresmh.com/nyctaxitrips/) ve başlıklı IPNB [Ipython Notebook ve SQL Server'ı kullanarak NYC veri denetimi](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) uçtan uca bir kılavuz için.
> 
> 

1. Gün başına gözlemler sayısını alın
   
    `SELECT CONVERT(date, <date_columnname>) as date, count(*) as c from <tablename> group by CONVERT(date, <date_columnname>)` 
2. Kategorik bir sütundaki düzeylerini Al
   
    `select  distinct <column_name> from <databasename>`
3. Kategorik iki sütunu birlikte düzey sayısını Al 
   
    `select <column_a>, <column_b>,count(*) from <tablename> group by <column_a>, <column_b>`
4. Sayısal sütunlara dağıtımı Al
   
    `select <column_name>, count(*) from <tablename> group by <column_name>`

### <a name="sql-featuregen"></a>Özellik oluşturma
Bu bölümde, biz özellikleri SQL kullanarak oluşturma yolları açıklanmaktadır:  

1. [Özellik nesil sayısı tabanlı](#sql-countfeature)
2. [Gruplama özellik oluşturma](#sql-binningfeature)
3. [Tek bir sütundan özellikleri alınıyor](#sql-featurerollout)

> [!NOTE]
> Ek özellikler oluşturduktan sonra bunları mevcut tabloya sütun olarak ekleyin veya özgün tabloyla birleştirilen birincil anahtar ve ek özellikler ile yeni bir tablo oluşturabilirsiniz. 
> 
> 

### <a name="sql-countfeature"></a>Özellik nesil sayısı tabanlı
Aşağıdaki örnekler sayısı özellikleri oluşturmanın iki yolu gösterir. İlk yöntem koşullu toplamı ve ikinci yöntem 'where' yan tümcesi kullanır. Bunlar daha sonra (birincil anahtar sütunlarını kullanarak) özgün tablo ile özgün veri yanı sıra sayısı özelliğiniz için katılabilir.

    select <column_name1>,<column_name2>,<column_name3>, COUNT(*) as Count_Features from <tablename> group by <column_name1>,<column_name2>,<column_name3> 

    select <column_name1>,<column_name2> , sum(1) as Count_Features from <tablename> 
    where <column_name3> = '<some_value>' group by <column_name1>,<column_name2> 

### <a name="sql-binningfeature"></a>Gruplama özellik oluşturma
Aşağıdaki örnek, bunun yerine bir özelliği olarak kullanılabilir bir sayısal sütun göre gruplama (beş depo kullanarak) binned özellikleri oluşturma adımları anlatılmaktadır:

    `SELECT <column_name>, NTILE(5) OVER (ORDER BY <column_name>) AS BinNumber from <tablename>`


### <a name="sql-featurerollout"></a>Tek bir sütundan özellikleri alınıyor
Bu bölümde ek özellikler oluşturmak için bir tablodaki tek bir sütun kullanıma sunma gösterilmektedir. Örnek özellikleri oluşturmak çalıştığınız tablosunda bir enlem veya boylam sütunu olduğunu varsayar.

Enlem/boylam konumu veri kısa öncü İşte (stackoverflow kaynak var [enlem ve boylam doğruluğunu ölçmek nasıl?](https://gis.stackexchange.com/questions/8650/how-to-measure-the-accuracy-of-latitude-and-longitude)). Konum alanı featurizing önce anlamak kullanışlıdır:

* Oturum bize biz Kuzey olup veya Güney, Doğu veya Batı dünyayı gösterir.
* Sıfır olmayan bir yüz basamağın yuvarlanacağını belirtir bize boylam, enlem değil kullandığımız!
* Onlarca basamaklı bir konuma yaklaşık 1.000 kilometre sağlar. Bize ne Kıta veya üzerinde duyuyoruz Okyanusu hakkında yararlı bilgiler sağlıyor.
* Birimleri basamak (bir ondalık derece) 111 kilometre (60 Deniz mili, yaklaşık 69 mil) bir konum sağlar. Bunu bize kabaca hangi büyük eyalet veya ülke/bölge duyuyoruz söyleyebilirsiniz.
* En fazla 11.1 km ilk ondalık yerdir: komşu büyük Şehir'dan büyük bir şehir konumunu ayırt edebilir.
* En fazla 1.1 km ikinci ondalık yerdir: sonraki bir village ayırabilirsiniz.
* Üçüncü ondalık en fazla 110 m: büyük Tarım alan veya Kurumsal kampüs tanımlayabilirsiniz yerdir.
* Dördüncü ondalık en fazla 11 m: olan bir paket tanımanıza yerdir. Tipik doğruluğunu düzeltilemeyen bir GPS birim için hiçbir engelleme ile karşılaştırılabilir.
* Beşinci ondalık en fazla 1.1 m: ağaçları birbirinden ayıran yerdir. Bu düzey ticari GPS birimleri ile tutarlılık yalnızca fark düzeltme ile gerçekleştirilebilir.
* Altıncı ondalık en fazla 0.11 m: Bu yapıları ortamlarını, tasarlamak için ayrıntılı olarak yerleştirmek için yollar oluşturma kullanabileceğiniz yerdir. Hareketleri glaciers rivers ve izlemek için birden fazla yeterince iyi olmalıdır. Bu, GPS, differentially düzeltilmiş GPS gibi painstaking ölçülerle yararlanarak gerçekleştirilebilir.

Konum bilgileri özellikleri tespit gibi bölge, konum ve Şehir bilgilerini ayrılması olabilir. Bing Haritalar API'si kullanılabilir gibi bir REST uç noktası de çağırabilirsiniz Not [noktası tarafından bir konum bulun](https://msdn.microsoft.com/library/ff701710.aspx) bölge/bölge bilgileri alınacak.

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
> İstediğiniz dilde kullanarak kayıtları program aracılığıyla ekleyebilirsiniz. Veri yazma verimliliğini artırmak için öbekler halinde eklemek gerekebilir (pyodbc kullanarak bunu ilişkin bir örnek için bkz: [SQLServer python ile erişmek için bir HelloWorld örnek](https://code.google.com/p/pypyodbc/wiki/A_HelloWorld_sample_to_access_mssql_with_python)). Veritabanını kullanarak veri eklemek için başka bir alternatiftir [BCP yardımcı programının](https://msdn.microsoft.com/library/ms162802.aspx).
> 
> 

### <a name="sql-aml"></a>Azure Machine Learning ile bağlanma
Yeni oluşturulan özellik bir sütun olarak var olan bir tabloya eklenebilir veya yeni bir tablo içinde saklanan ve machine learning için özgün tablo ile birleştirilmiş. Özellikleri oluşturulan ya da zaten oluşturduysanız, kullanılarak erişilen [verileri içeri aktarma] [ import-data] aşağıda gösterildiği gibi Azure Machine Learning modülünde:

![azureml okuyucular][1] 

## <a name="python"></a>Python gibi bir programlama dilini kullanma
Verileri araştırmak ve verileri SQL Server olduğunda özellikler oluşturmak için Python'ı kullanarak benzer açıklandığı gibi Python kullanarak Azure blob veri işlemeye [işlem Azure Blob veri, veri bilimi ortamınızdaki](data-blob.md). Veriler veritabanından pandas veri çerçeveye yüklenmesi gerektiğini ve daha sonra işlenebilir. Biz veritabanına bağlanma ve bu bölümde veri çerçevesi veri yükleme işleminin belgeleyin.

Aşağıdaki bağlantı dizesi biçimi python'dan pyodbc (Değiştir servername, dbname, kullanıcı adı ve parola, belirli değerleri içeren) kullanarak bir SQL Server veritabanına bağlanmak için kullanılabilir:

    #Set up the SQL Azure connection
    import pyodbc    
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

[Pandas Kitaplığı](https://pandas.pydata.org/) Python'da Python programlama için veri işleme için zengin bir veri yapıları ve verileri analiz araçları sağlar. Aşağıdaki kod, sonuçları bir SQL Server veritabanından bir Pandas veri çerçevesine döndürülen okur:

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <columnname2>... from <tablename>''', conn)

Makalede de anlatılan Pandas veri çerçevesi ile çalışabilir artık [işlem Azure Blob veri, veri bilimi ortamınızdaki](data-blob.md).

## <a name="azure-data-science-in-action-example"></a>Eylem örnekte Azure veri bilimi
Azure Data Science Process genel bir veri kümesini kullanarak uçtan uca kılavuz örneği için bkz: [Azure veri bilimi işlemi yapılıyor](sql-walkthrough.md).

[1]: ./media/sql-server-virtual-machine/reader_db_featurizedinput.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/

