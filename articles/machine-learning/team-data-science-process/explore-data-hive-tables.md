---
title: Hive sorguları ile Hive tablosundaki verileri keşfedin | Microsoft Docs
description: Hive sorgularını kullanarak Hive tablosundaki verileri keşfedin.
services: machine-learning
documentationcenter: ''
author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: 0d46cea5-2b4c-4384-9bfa-fa20f6f75148
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: deguhath
ms.openlocfilehash: 79d40617ae4f9cd83d04cad213e5d8fd76b03876
ms.sourcegitcommit: 8ebcecb837bbfb989728e4667d74e42f7a3a9352
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2018
ms.locfileid: "42061754"
---
# <a name="explore-data-in-hive-tables-with-hive-queries"></a>Hive sorguları ile Hive tablosundaki verileri keşfedin
Bu belge, bir HDInsight Hadoop kümesindeki Hive tablolarındaki verileri araştırmak için kullanılan örnek Hive betiklerini sağlar.

Aşağıdaki **menü** Araçlar çeşitli depolama ortamlarından verileri araştırmak için nasıl kullanılacağını açıklayan konulara bağlantılar.

[!INCLUDE [cap-explore-data-selector](../../../includes/cap-explore-data-selector.md)]

## <a name="prerequisites"></a>Önkoşullar
Bu makalede, olduğunu varsayar:

* Bir Azure depolama hesabı oluşturuldu. Yönergelere ihtiyacınız varsa bkz [bir Azure depolama hesabı oluşturma](../../storage/common/storage-quickstart-create-account.md)
* HDInsight hizmeti ile özelleştirilmiş bir Hadoop kümesi hazırlandı. Yönergelere ihtiyacınız varsa bkz [Gelişmiş analiz için Azure HDInsight Hadoop kümelerini özelleştirin](customize-hadoop-cluster.md).
* Azure HDInsight Hadoop kümeleri Hive tablolarında için verileri karşıya yüklendi. Sahip değil,'ndaki yönergeleri izleyin. [Hive tabloları oluşturma ve yük verileri](move-hive-tables.md) Hive tablolarına veri önce yüklenecek.
* Kümeye uzaktan erişim etkin. Yönergelere ihtiyacınız varsa bkz [Hadoop küme baş düğümüne erişmek](customize-hadoop-cluster.md).
* Hive sorguları göndermek yönergeler gerekiyorsa bkz [nasıl Hive sorguları göndermek için](move-hive-tables.md#submit)

## <a name="example-hive-query-scripts-for-data-exploration"></a>Veri keşfi için örnek Hive sorgu betikleri
1. Bölüm başına gözlemler sayısını alın  `SELECT <partitionfieldname>, count(*) from <databasename>.<tablename> group by <partitionfieldname>;`
2. Gün başına gözlemler sayısını alın  `SELECT to_date(<date_columnname>), count(*) from <databasename>.<tablename> group by to_date(<date_columnname>);`
3. Kategorik bir sütundaki düzeylerini Al  
    `SELECT  distinct <column_name> from <databasename>.<tablename>`
4. Kategorik iki sütunu birlikte düzey sayısını Al  `SELECT <column_a>, <column_b>, count(*) from <databasename>.<tablename> group by <column_a>, <column_b>`
5. Sayısal sütunlara dağıtımı Al  
    `SELECT <column_name>, count(*) from <databasename>.<tablename> group by <column_name>`
6. İki tablo katılmasını kayıtları Ayıkla
   
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

## <a name="additional-query-scripts-for-taxi-trip-data-scenarios"></a>Taksi seyahat veri senaryoları için ek sorgu betikleri
Özel sorgular örnekleri [NYC taksi seyahat verilerini](http://chriswhong.com/open-data/foil_nyc_taxi/) senaryoları burada da sunulmaktadır [GitHub deposu](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts). Bu sorgular zaten belirtilen veri şemasına sahip ve çalıştırmak için gönderilmeye hazır.

