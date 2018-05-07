---
title: SQL Server sanal makine veri azure'de | Microsoft Docs
description: Azure üzerinde bir SQL Server VM depolanan verileri araştırmak nasıl.
services: machine-learning
documentationcenter: ''
author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: ccbb3085-af9e-4ec2-9df2-15dcab261d05
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: deguhath
ms.openlocfilehash: 68c8386917599afcf2819ff97453de77dea90215
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
# <a name="explore-data-in-sql-server-virtual-machine-on-azure"></a>Azure üzerindeki SQL Server Sanal Makinesi verilerini keşfetme
Bu belge, Azure üzerinde bir SQL Server VM depolanan verileri araştırmak alınmaktadır. Bu SQL kullanarak veri wrangling veya Python gibi bir programlama dili kullanılarak yapılabilir.

Aşağıdaki **menü** araçları çeşitli depolama ortamlarından verileri araştırmak için nasıl kullanılacağını açıklayan konulara bağlantılar. Bu görev Cortana analitik işlem (CAP) bir adımdır.

[!INCLUDE [cap-explore-data-selector](../../../includes/cap-explore-data-selector.md)]

> [!NOTE]
> Bu belgede örnek SQL deyimlerinde veri SQL Server'da olduğunu varsayın. Öyle değilse, SQL Server'a verilerinizi taşıma hakkında bilgi edinmek için bulut veri bilimi işlem eşlemesi bakın.
> 
> 

## <a name="sql-dataexploration"></a>SQL komut dosyaları ile SQL veri keşfedin
Burada, SQL Server veri depolarında keşfetmek için kullanılabilecek birkaç örnek SQL komut dosyaları bulunmaktadır.

1. Gün başına gözlemleri sayısını alın
   
    `SELECT CONVERT(date, <date_columnname>) as date, count(*) as c from <tablename> group by CONVERT(date, <date_columnname>)` 
2. Kategorik bir sütunda düzeylerini Al
   
    `select  distinct <column_name> from <databasename>`
3. İki kategorik sütun arada düzey sayısını Al 
   
    `select <column_a>, <column_b>,count(*) from <tablename> group by <column_a>, <column_b>`
4. Sayısal sütunlar için dağıtım Al
   
    `select <column_name>, count(*) from <tablename> group by <column_name>`

> [!NOTE]
> Pratik bir örnek için kullandığınız [NYC ücreti dataset](http://www.andresmh.com/nyctaxitrips/) ve başlıklı IPNB başvuruda [IPython not defteri ve SQL Server kullanarak NYC veri wrangling](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) bir uçtan uca gözden geçirme.
> 
> 

## <a name="python"></a>Python ile SQL veri keşfedin
Verileri araştırmak ve verileri SQL Server olduğunda özellikleri oluşturmak için Python kullanarak benzer açıklandığı gibi Python kullanarak Azure blob veri işleme için [veri bilimi ortamınızdaki işlem Azure Blob veri](data-blob.md). Veriler veritabanından pandas DataFrame yüklenmesi gerektiğini ve ardından daha fazla işlenebilir. Biz, veritabanına bağlanmak ve bu bölümdeki DataFrame içine veri yükleme işleminde belge.

Aşağıdaki bağlantı dizesi biçimi Python pyodbc (Değiştir servername, dbname, kullanıcı adı ve parola, belirli değerleri içeren) kullanarak bir SQL Server veritabanına bağlanmak için kullanılabilir:

    #Set up the SQL Azure connection
    import pyodbc    
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

[Pandas Kitaplığı](http://pandas.pydata.org/) Python içinde Python programlama için veri işleme için zengin bir veri yapılarını ve veri çözümleme araçları sağlar. Aşağıdaki kod, bir SQL Server veritabanından Pandas veri çerçeveye sonuçları döndüren okur:

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <cloumnname2>... from <tablename>''', conn)

Konusunda anlatıldığı gibi Pandas DataFrame ile çalışabilirsiniz artık [veri bilimi ortamınızdaki işlem Azure Blob veri](data-blob.md).

## <a name="the-team-data-science-process-in-action-example"></a>Eylem örnekte takım veri bilimi işlemi
Cortana Analytics ortak bir veri kümesini kullanarak işlem uçtan uca kılavuz örneği için bkz: [eylem takım veri bilimi işlem: SQL Server'ı kullanarak](sql-walkthrough.md).

