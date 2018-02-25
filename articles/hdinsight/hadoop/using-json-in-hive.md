---
title: "Çözümleme ve işlem Azure hdınsight'ta Apache Hive ile JSON belgeleri | Microsoft Docs"
description: "JSON belgelerini kullanın ve apache Azure hdınsight'ta Hive kullanarak bunları analiz hakkında bilgi edinin"
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: e17794e8-faae-4264-9434-67f61ea78f13
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/20/2017
ms.author: jgao
ms.openlocfilehash: 62b21db5c52287c1d0d058cba3a433434c364777
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/24/2018
---
# <a name="process-and-analyze-json-documents-by-using-apache-hive-in-azure-hdinsight"></a>İşlem ve Azure hdınsight'ta Apache Hive kullanarak JSON belgelerini çözümleme

İşlem ve Azure hdınsight'ta Apache Hive kullanarak JavaScript nesne gösterimi (JSON) dosyaları çözümleme hakkında bilgi edinin. Bu öğretici aşağıdaki JSON belgesini kullanır:

    {
        "StudentId": "trgfg-5454-fdfdg-4346",
        "Grade": 7,
        "StudentDetails": [
            {
                "FirstName": "Peggy",
                "LastName": "Williams",
                "YearJoined": 2012
            }
        ],
        "StudentClassCollection": [
            {
                "ClassId": "89084343",
                "ClassParticipation": "Satisfied",
                "ClassParticipationRank": "High",
                "Score": 93,
                "PerformedActivity": false
            },
            {
                "ClassId": "78547522",
                "ClassParticipation": "NotSatisfied",
                "ClassParticipationRank": "None",
                "Score": 74,
                "PerformedActivity": false
            },
            {
                "ClassId": "78675563",
                "ClassParticipation": "Satisfied",
                "ClassParticipationRank": "Low",
                "Score": 83,
                "PerformedActivity": true
                    ]
    }

Dosya şu yolda bulunabilir:  **wasb://processjson@hditutorialdata.blob.core.windows.net/** . Hdınsight ile Azure Blob storage kullanma hakkında daha fazla bilgi için bkz: [hdınsight'ta Hadoop ile kullanım HDFS uyumlu Azure Blob storage](../hdinsight-hadoop-use-blob-storage.md). Kümenizin varsayılan kapsayıcı dosyayı kopyalayabilirsiniz.

Bu öğreticide, Hive konsolunu kullanın. Hive Konsolu hakkında daha fazla yönerge için bkz: [Uzak Masaüstü kullanarak hdınsight'ta Hadoop ile Hive kullanma](apache-hadoop-use-hive-remote-desktop.md).

## <a name="flatten-json-documents"></a>JSON belgeleri düzleştirmek
Sonraki bölümde listelenen yöntemleri JSON belgesi tek bir satır oluşan gerektirir. Bu nedenle, bir dizeye JSON belgesi düzleştirmek gerekir. JSON belgenizi zaten düzleştirilmiş, bu adımı atlayın ve JSON verilerini çözümleme düz sonraki bölüme gidin. JSON belgesini düzleştirmek için aşağıdaki betiği çalıştırın:

    DROP TABLE IF EXISTS StudentsRaw;
    CREATE EXTERNAL TABLE StudentsRaw (textcol string) STORED AS TEXTFILE LOCATION "wasb://processjson@hditutorialdata.blob.core.windows.net/";

    DROP TABLE IF EXISTS StudentsOneLine;
    CREATE EXTERNAL TABLE StudentsOneLine
    (
      json_body string
    )
    STORED AS TEXTFILE LOCATION '/json/students';

    INSERT OVERWRITE TABLE StudentsOneLine
    SELECT CONCAT_WS(' ',COLLECT_LIST(textcol)) AS singlelineJSON
          FROM (SELECT INPUT__FILE__NAME,BLOCK__OFFSET__INSIDE__FILE, textcol FROM StudentsRaw DISTRIBUTE BY INPUT__FILE__NAME SORT BY BLOCK__OFFSET__INSIDE__FILE) x
          GROUP BY INPUT__FILE__NAME;

    SELECT * FROM StudentsOneLine

Ham JSON dosyası şu konumdadır  **wasb://processjson@hditutorialdata.blob.core.windows.net/** . **StudentsRaw** Hive tablosu noktaları değil düzleştirilmiş ham JSON belgesi.

**StudentsOneLine** Hive tablosu altında Hdınsight varsayılan dosya sistemindeki verileri depolayan **/json/Öğrenciler/** yolu.

**Ekle** deyimi doldurur **StudentOneLine** düzleştirilmiş JSON verilerini içeren tablo.

**Seçin** deyimi yalnızca bir satır döndürür.

Çıktısını işte **seçin** deyimi:

![JSON belgesini düzleştirme][image-hdi-hivejson-flatten]

## <a name="analyze-json-documents-in-hive"></a>Hive JSON belgelerde Çözümle
Hive sorguları JSON belgelerde çalıştırmak için üç farklı mekanizmalar sağlar ya da kendi yazabilirsiniz:

* Get_json_object kullanıcı tanımlı işlev (UDF) kullanın.
* UDF json_tuple kullanın.
* Özel seri hale getirici/seri durumdan Çıkarıcının (SerDe) kullanın.
* Kendi UDF, Python veya diğer dillerini kullanarak yazın. Kendi Python kodu Hive ile çalıştırma hakkında daha fazla bilgi için bkz: [Python UDF Apache Hive veya Pig ile][hdinsight-python].

### <a name="use-the-getjsonobject-udf"></a>UDF get_json_object kullanın
Hive sağlar olarak adlandırılan yerleşik bir UDF [get_json_object](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object) çalışma zamanı sırasında JSON sorgulama gerçekleştirebilir. Bu yöntem, iki bağımsız değişken--düzleştirilmiş JSON belgesini ve ayrıştırılması gerekiyor JSON alanının sahip yöntemi adını ve tablo adı alır. Bu UDF nasıl çalıştığını görmek için bir örneğe bakalım.

Aşağıdaki sorgu, ad ve Soyadı her Öğrenci için döndürür:

    SELECT
      GET_JSON_OBJECT(StudentsOneLine.json_body,'$.StudentDetails.FirstName'),
      GET_JSON_OBJECT(StudentsOneLine.json_body,'$.StudentDetails.LastName')
    FROM StudentsOneLine;

Konsol penceresinde bu sorguyu çalıştırdığınızda çıktısı şöyledir:

![get_json_object UDF][image-hdi-hivejson-getjsonobject]

UDF get_json_object sınırlamaları vardır:

* Sorgudaki her bir alan için sorgu yeniden ayrıştırmayı gerektirdiğinden, performansı etkiler.
* **Alma\_JSON_OBJECT()** bir dizi dize gösterimini döndürür. Bu bir Hive dizisine dönüştürme için köşeli ayraç değiştirme için normal ifadeleri kullanma gerekir "[" ve "]", ve sonra da dizi almak için bölünmüş çağırın.

Hive wiki json_tuple kullanmanızı önerir nedeni budur.  

### <a name="use-the-jsontuple-udf"></a>UDF json_tuple kullanın
Hive tarafından sağlanan başka bir UDF adlı [json_tuple](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-json_tuple), gerçekleştirir daha iyi [get_ json _object](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object). Bu yöntem, bir dizi anahtarları ve bir JSON dizesinde alır ve bir işlevi kullanarak değerlerin bir tanımlama grubu döndürür. Aşağıdaki sorgu, JSON belgesinden Öğrenci Kimliğini ve sınıf döndürür:

    SELECT q1.StudentId, q1.Grade
      FROM StudentsOneLine jt
      LATERAL VIEW JSON_TUPLE(jt.json_body, 'StudentId', 'Grade') q1
        AS StudentId, Grade;

Bu komut dosyası Hive konsolunda çıktı:

![json_tuple UDF][image-hdi-hivejson-jsontuple]

UDF kullanan json_tuple [yanal Görünüm](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView) json etkinleştirir Hive sözdiziminde\_orijinal tablonun her satırı için UDT işlevi uygulayarak sanal bir tablo oluşturmak için tanımlama grubu. Yinelenen kullanımı nedeniyle kullanılması çok zor hale karmaşık JSONs **YANAL Görünüm**. Ayrıca, **JSON_TUPLE** iç içe geçmiş JSONs işleyemiyor.

### <a name="use-a-custom-serde"></a>Özel SerDe kullanın
SerDe iç içe geçmiş JSON belgelerini ayrıştırma için en iyi bir seçimdir. JSON şeması tanımlamanıza izin verir ve ardından belgeleri ayrıştırmak için şema kullanabilirsiniz. Yönergeler için bkz: [Microsoft Azure Hdınsight ile özel bir JSON SerDe kullanmayı](https://blogs.msdn.microsoft.com/bigdatasupport/2014/06/18/how-to-use-a-custom-json-serde-with-microsoft-azure-hdinsight/).

## <a name="summary"></a>Özet
Conclusion, seçtiğiniz Hive JSON işlecinde tür, senaryoya bağlıdır. Basit bir JSON belge sahip ve üzerinde aramak için yalnızca bir alan varsa, Hive UDF get_json_object kullanmayı seçebilirsiniz. Ardından üzerinde aramak için birden fazla anahtar varsa json_tuple kullanabilirsiniz. İç içe geçmiş bir belgeniz varsa, JSON SerDe kullanmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

İlgili makaleler için bkz:

* [Hive ve HiveQL hdınsight'ta Hadoop ile bir örnek Apache log4j dosyasını çözümlemek için kullanın](../hdinsight-use-hive.md)
* [Hdınsight'ta Hive kullanarak uçuş gecikme verilerini çözümleme](../hdinsight-analyze-flight-delay-data.md)
* [Twitter verilerini hdınsight'ta Hive kullanarak çözümleme](../hdinsight-analyze-twitter-data.md)

[hdinsight-python]:python-udf-hdinsight.md

[image-hdi-hivejson-flatten]: ./media/using-json-in-hive/flatten.png
[image-hdi-hivejson-getjsonobject]: ./media/using-json-in-hive/getjsonobject.png
[image-hdi-hivejson-jsontuple]: ./media/using-json-in-hive/jsontuple.png
[image-hdi-hivejson-jdk]: ./media/hdinsight-using-json-in-hive/jdk.png
[image-hdi-hivejson-maven]: ./media/hdinsight-using-json-in-hive/maven.png
[image-hdi-hivejson-serde]: ./media/hdinsight-using-json-in-hive/serde.png
[image-hdi-hivejson-addjar]: ./media/hdinsight-using-json-in-hive/addjar.png
[image-hdi-hivejson-serde_query1]: ./media/hdinsight-using-json-in-hive/serde_query1.png
[image-hdi-hivejson-serde_query2]: ./media/hdinsight-using-json-in-hive/serde_query2.png
[image-hdi-hivejson-serde_query3]: ./media/hdinsight-using-json-in-hive/serde_query3.png
[image-hdi-hivejson-serde_result]: ./media/hdinsight-using-json-in-hive/serde_result.png

