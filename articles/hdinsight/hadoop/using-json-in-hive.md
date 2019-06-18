---
title: Azure HDInsight, Apache Hive ile JSON belgelerini işleme ve analiz edin
description: JSON belgelerini kullanın ve bunları Azure HDInsight, Apache Hive'ı kullanarak analiz etme hakkında bilgi edinin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 06/03/2019
ms.openlocfilehash: 904a6a2af4c92c374d5afe4148f50e853e5d1fb2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66479600"
---
# <a name="process-and-analyze-json-documents-by-using-apache-hive-in-azure-hdinsight"></a>Azure HDInsight, Apache Hive'ı kullanarak işleyebilir ve JSON belgeleri

Azure HDInsight, Apache Hive'ı kullanarak JavaScript nesne gösterimi (JSON) dosyalarını işleyebilir ve öğrenin. Bu makalede aşağıdaki JSON belgesini kullanır:

```json
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
    }
  ]
}
```

Dosya şu yolda bulunabilir: `wasb://processjson@hditutorialdata.blob.core.windows.net/`. HDInsight ile Azure Blob Depolama kullanma hakkında daha fazla bilgi için bkz. [kullanım HDFS uyumlu Azure Blob Depolama, HDInsight, Apache Hadoop ile](../hdinsight-hadoop-use-blob-storage.md). Kümenizin varsayılan kapsayıcıya dosyayı kopyalayabilirsiniz.

Bu öğreticide, Apache Hive konsolunu kullanabilirsiniz. Hive konsolunu açmak yönergeler için bkz: [kullanım Apache Ambari Hive görünümünü HDInsight, Apache Hadoop ile](apache-hadoop-use-hive-ambari-view.md).

## <a name="flatten-json-documents"></a>JSON belgeleri düzleştirmek
Sonraki bölümde listelenen yöntemlerden istediğinizi JSON belgesini tek bir satırı sahipliğindeki gerektirir. Bu nedenle, dize JSON belgesini düzleştirin gerekir. JSON belgenizi zaten düzleştirilmiş bu adımı atlayın ve JSON verilerini analiz etme üzerinde doğrudan bir sonraki bölüme gidin. JSON belgesini düzleştirmek için aşağıdaki betiği çalıştırın:

```sql
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
```

Ham JSON dosyası şu konumdadır `wasb://processjson@hditutorialdata.blob.core.windows.net/`. **StudentsRaw** Hive tablosu noktaları değil düzleştirilmiş ham JSON belgesi.

**StudentsOneLine** Hive tablosu altında HDInsight varsayılan dosya sistemindeki verileri depolayan **/json/Öğrenciler/** yolu.

**Ekle** deyimi doldurur **StudentOneLine** düzleştirilmiş JSON verilerini içeren tablo.

**Seçin** deyimi yalnızca bir satır döndürür.

Çıktısı şöyledir **seçin** deyimi:

![JSON belgesini düzleştirme](./media/using-json-in-hive/flatten.png)

## <a name="analyze-json-documents-in-hive"></a>Hive'da JSON belgeleri analiz edin
Hive sorguları çalıştırmak JSON belgeleri için üç farklı mekanizmaları sağlar ve kendi yazabilirsiniz:

* Get_json_object kullanıcı tanımlı işlev (UDF) kullanın.
* UDF json_tuple kullanın.
* Özel seri hale getirici/seri durumdan çıkarıcı (SerDe) kullanın.
* Python veya diğer dilleri kullanarak kendi UDF yazın. Hive ile kendi Python kodunu çalıştırma hakkında daha fazla bilgi için bkz. [Apache Hive ve Apache Pig ile Python UDF] hdınsight-python.

### <a name="use-the-getjsonobject-udf"></a>' % S ' get_json_object UDF kullanma
Hive sağlar adlı yerleşik bir UDF [get_json_object](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object) çalışma zamanı sırasında JSON sorgulama gerçekleştirebilir. Bu yöntem, iki bağımsız değişkeni--düzleştirilmiş JSON belgesini içeren ve ayrıştırılması gereken JSON alanını yöntem adı ve tablo adını alır. Bu UDF nasıl çalıştığını görmek için bir örneğe göz atalım.

Ad ve Soyadı her öğrencinin için aşağıdaki sorguyu döndürür:

```sql
SELECT
  GET_JSON_OBJECT(StudentsOneLine.json_body,'$.StudentDetails.FirstName'),
  GET_JSON_OBJECT(StudentsOneLine.json_body,'$.StudentDetails.LastName')
FROM StudentsOneLine;
```

Konsol penceresinde bu sorgu çalıştırdığınızda, çıktı şöyledir:

![get_json_object UDF](./media/using-json-in-hive/getjsonobject.png)

UDF get_json_object sınırlamaları vardır:

* Sorgudaki her bir alan sorgu reparsing gerektirdiğinden performansı etkiler.
* **Alma\_JSON_OBJECT()** bir dizinin dize gösterimini döndürür. Bu dizi bir Hive dizisine dönüştürmek için köşeli ayraç değiştirmek için normal ifadeleri kullanma sahip "[" ve "]", ve sonra da dizinin almak için bölünmüş çağırmak.

Hive wiki json_tuple kullanmanızı önerir nedeni budur.  

### <a name="use-the-jsontuple-udf"></a>' % S ' json_tuple UDF kullanma
Hive tarafından sağlanan başka bir UDF adlı [json_tuple](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-json_tuple), gerçekleştiren daha iyi [get_ json n_esne](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object). Bu yöntem, bir dizi anahtarları ve bir JSON dizesi alır ve bir işlevi kullanarak değerleri bir tanımlama grubu döndürür. Aşağıdaki sorgu, JSON belgesinden Öğrenci Kimliği ve sınıf döndürür:

```sql
SELECT q1.StudentId, q1.Grade
FROM StudentsOneLine jt
LATERAL VIEW JSON_TUPLE(jt.json_body, 'StudentId', 'Grade') q1
  AS StudentId, Grade;
```

Hive konsolunda bu betiğin çıkışı:

![json_tuple UDF](./media/using-json-in-hive/jsontuple.png)

UDF kullanan json_tuple [yana görünümü](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView) hive'da json sağlayan, söz dizimi\_özgün tablonun her satırı için UDT işlevi uygulayarak sanal bir tablo oluşturmak için demet. Yinelenen kullanımı nedeniyle çok zahmetli hale karmaşık Json'lerini **YATAY Görünüm**. Ayrıca, **JSON_TUPLE** iç içe geçmiş Json'lerini işleyemiyor.

### <a name="use-a-custom-serde"></a>Özel SerDe kullanın
SerDe, iç içe geçmiş JSON belgelerini ayrıştırmak için en iyi bir seçimdir. JSON şemasını tanımlamanıza olanak sağlar ve ardından belgelerini ayrıştırmak için bir şema kullanabilirsiniz. Yönergeler için [Microsoft Azure HDInsight ile özel bir JSON SerDe kullanmayı](https://web.archive.org/web/20190217104719/ https://blogs.msdn.microsoft.com/bigdatasupport/2014/06/18/how-to-use-a-custom-json-serde-with-microsoft-azure-hdinsight/).

## <a name="summary"></a>Özet
Conclusion, seçtiğiniz hive'da JSON işleç türü, senaryoya bağlıdır. Basit bir JSON belge varsa ve yalnızca bir alan üzerinde arama yapmak için Hive UDF get_json_object kullanmayı da tercih edebilirsiniz. Ardından, üzerinde arama yapmak için birden fazla anahtar varsa, json_tuple kullanabilirsiniz. Bir iç içe geçmiş bir belgeniz varsa, JSON SerDe kullanmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

İlgili makaleler için bkz:

* [Apache Hive ve HiveQL HDInsight, Apache Hadoop ile bir örnek Apache log4j dosyasını çözümlemek için kullanın](../hdinsight-use-hive.md)
* [Apache Hive, HDInsight'ı kullanarak uçuş gecikme verilerini çözümleme](../hdinsight-analyze-flight-delay-data-linux.md)
* [Apache Hive, HDInsight kullanarak twitter verilerini çözümleme](../hdinsight-analyze-twitter-data-linux.md)
