---
title: "Hive tabloları Hive sorguları ile keşfedin | Microsoft Docs"
description: "Hive sorgularını kullanarak Hive tablolarındaki verileri keşfedin."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 0d46cea5-2b4c-4384-9bfa-fa20f6f75148
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: dc5cbbf8db46607179e8b0e8657462afac21f7da
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="explore-data-in-hive-tables-with-hive-queries"></a>Hive sorguları ile Hive tablosundaki verileri keşfedin
Bu belgede bir Hdınsight Hadoop kümesindeki Hive tablolarındaki verileri keşfetmek için kullanılan örnek Hive komut dosyaları sağlar.

Aşağıdaki **menü** araçları çeşitli depolama ortamlarından verileri araştırmak için nasıl kullanılacağını açıklayan konulara bağlantılar.

[!INCLUDE [cap-explore-data-selector](../../../includes/cap-explore-data-selector.md)]

## <a name="prerequisites"></a>Ön koşullar
Bu makalede, sahip olduğunuz varsayılmaktadır:

* Bir Azure depolama hesabı oluşturuldu. Yönergeler gerekiyorsa bkz [bir Azure depolama hesabı oluşturma](../../storage/common/storage-create-storage-account.md#create-a-storage-account)
* Özelleştirilmiş bir Hadoop kümesine Hdınsight hizmetiyle sağlandı. Yönergeler gerekiyorsa bkz [Advanced Analytics için Azure Hdınsight Hadoop kümeleri özelleştirme](customize-hadoop-cluster.md).
* Azure Hdınsight Hadoop kümeleri Hive tabloları için verileri karşıya yüklendi. Sahip değil,'ndaki yönergeleri izleyin. [Hive tabloları oluşturma ve yük verileri](move-hive-tables.md) verileri ilk Hive tablolara yüklemek için.
* Küme uzak erişim etkin. Yönergeler gerekiyorsa bkz [Hadoop küme baş düğümü erişim](customize-hadoop-cluster.md#headnode).
* Hive sorguları göndermek yönergeler gerekiyorsa bkz [nasıl Hive sorguları göndermek için](move-hive-tables.md#submit)

## <a name="example-hive-query-scripts-for-data-exploration"></a>Veri keşfi için örnek Hive sorgusu komut dosyaları
1. Bölüm başına gözlemleri sayısını alın`SELECT <partitionfieldname>, count(*) from <databasename>.<tablename> group by <partitionfieldname>;`
2. Gün başına gözlemleri sayısını alın`SELECT to_date(<date_columnname>), count(*) from <databasename>.<tablename> group by to_date(<date_columnname>);`
3. Kategorik bir sütunda düzeylerini Al  
    `SELECT  distinct <column_name> from <databasename>.<tablename>`
4. İki kategorik sütun arada düzey sayısını Al`SELECT <column_a>, <column_b>, count(*) from <databasename>.<tablename> group by <column_a>, <column_b>`
5. Sayısal sütunlar için dağıtım Al  
    `SELECT <column_name>, count(*) from <databasename>.<tablename> group by <column_name>`
6. İki tablo birleştirme kayıtları Ayıkla
   
        SELECT
            a.<common_columnname1> as <new_name1>,
            a.<common_columnname2> as <new_name2>,
            a.<a_column_name1> as <new_name3>,
            a.<a_column_name2> as <new_name4>,
            b.<b_column_name1> as <new_name5>,
            b.<b_column_name2> as <new_name6>
        FROM
            (
            SELECT <common_columnname1>,
                <common_columnname2>,
                <a_column_name1>,
                <a_column_name2>,
            FROM <databasename>.<tablename1>
            ) a
            join
            (
            SELECT <common_columnname1>,
                <common_columnname2>,
                <b_column_name1>,
                <b_column_name2>,
            FROM <databasename>.<tablename2>
            ) b
            ON a.<common_columnname1>=b.<common_columnname1> and a.<common_columnname2>=b.<common_columnname2>

## <a name="additional-query-scripts-for-taxi-trip-data-scenarios"></a>Ücreti seyahat veri senaryoları için ek sorgu komut dosyaları
Özel sorgular örnekleri [NYC ücreti seyahat veri](http://chriswhong.com/open-data/foil_nyc_taxi/) senaryoları burada da sunulmaktadır [GitHub deposunu](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts). Bu sorgular zaten belirtilen veri şeması varsa ve çalıştırmak için gönderilmesi hazırsınız.

