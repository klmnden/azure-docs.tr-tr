---
title: Azure'da SQL Server sanal makinesi verilerini keşfedin | Microsoft Docs
description: Bir SQL Server VM'si, azure'da depolanan verileri araştırmak nasıl.
services: machine-learning
documentationcenter: ''
author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: ccbb3085-af9e-4ec2-9df2-15dcab261d05
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: deguhath
ms.openlocfilehash: 546d75172b9e6dbd77d63c36e5b8cebd0835a582
ms.sourcegitcommit: 96527c150e33a1d630836e72561a5f7d529521b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/09/2018
ms.locfileid: "51345615"
---
# <a name="explore-data-in-sql-server-virtual-machine-on-azure"></a>Azure üzerindeki SQL Server Sanal Makinesi verilerini keşfetme

Bu makalede, bir SQL Server VM'si, azure'da depolanan verileri araştırmak nasıl ele alınmaktadır. Bu SQL kullanarak veri denetimi veya Python gibi bir programlama dili kullanılarak yapılabilir.

Bu görev bir adımdır [Team Data Science Process](overview.md).

> [!NOTE]
> Bu belgede örnek SQL deyimlerinde veri SQL Server'da olduğunu varsayalım. Aksi takdirde, verilerinizi SQL Server'a taşıma hakkında bilgi edinmek için bulut veri bilimi işlemi eşlemesi bakın.
> 
> 

## <a name="sql-dataexploration"></a>SQL betikleri ile SQL veri keşfedin
SQL Server veri depolarında keşfetmek için kullanılan SQL betiklerini birkaç örnek aşağıda verilmiştir.

1. Gün başına gözlemler sayısını alın
   
    `SELECT CONVERT(date, <date_columnname>) as date, count(*) as c from <tablename> group by CONVERT(date, <date_columnname>)` 
2. Kategorik bir sütundaki düzeylerini Al
   
    `select  distinct <column_name> from <databasename>`
3. Kategorik iki sütunu birlikte düzey sayısını Al 
   
    `select <column_a>, <column_b>,count(*) from <tablename> group by <column_a>, <column_b>`
4. Sayısal sütunlara dağıtımı Al
   
    `select <column_name>, count(*) from <tablename> group by <column_name>`

> [!NOTE]
> Pratik bir örnek için kullandığınız [NYC taksi dataset](http://www.andresmh.com/nyctaxitrips/) ve başlıklı IPNB [Ipython Notebook ve SQL Server'ı kullanarak NYC veri denetimi](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) uçtan uca bir kılavuz için.
> 
> 

## <a name="python"></a>Python ile SQL veri keşfedin
Verileri araştırmak ve verileri SQL Server olduğunda özellikler oluşturmak için Python'ı kullanarak benzer açıklandığı gibi Python kullanarak Azure blob veri işleme için [işlem Azure Blob veri, veri bilimi ortamınızdaki](data-blob.md). Veriler veritabanından pandas DataFrame yüklenmesi gerektiğini ve daha sonra işlenebilir. Biz veritabanına bağlanma ve veri çerçevesi'Bu bölümde veri yükleme işleminin belgeleyin.

Aşağıdaki bağlantı dizesi biçimi python'dan pyodbc (Değiştir servername, dbname, kullanıcı adı ve parola, belirli değerleri içeren) kullanarak bir SQL Server veritabanına bağlanmak için kullanılabilir:

    #Set up the SQL Azure connection
    import pyodbc    
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

[Pandas Kitaplığı](http://pandas.pydata.org/) Python'da Python programlama için veri işleme için zengin bir veri yapıları ve verileri analiz araçları sağlar. Aşağıdaki kod, sonuçları bir SQL Server veritabanından bir Pandas veri çerçevesine döndürülen okur:

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <cloumnname2>... from <tablename>''', conn)

Konusunda anlatıldığı gibi Pandas DataFrame ile çalışabilir artık [işlem Azure Blob veri, veri bilimi ortamınızdaki](data-blob.md).

## <a name="the-team-data-science-process-in-action-example"></a>Team Data Science Process eylem örnekte
Cortana Analytics süreci genel bir veri kümesini kullanarak uçtan uca kılavuz örneği için bkz: [Team Data Science Process'in çalışması: SQL Server'ı kullanarak](sql-walkthrough.md).

