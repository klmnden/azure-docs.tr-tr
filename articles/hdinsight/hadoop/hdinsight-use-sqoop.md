---
title: Azure HDInsight (Apache Hadoop) ile Apache Sqoop işleri çalıştırma
description: Bir Hadoop kümesi ile bir Azure SQL veritabanı arasında Sqoop alma ve için bir iş istasyonundan Azure PowerShell'i kullanma konusunda bilgi edinin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/12/2019
ms.openlocfilehash: 6764d8d812789c9f54fa59e10b2a3e416e583a9c
ms.sourcegitcommit: b8a8d29fdf199158d96736fbbb0c3773502a092d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/15/2019
ms.locfileid: "59565861"
---
# <a name="use-apache-sqoop-with-hadoop-in-hdinsight"></a>HDInsight, Hadoop ile Apache Sqoop'u kullanma
[!INCLUDE [sqoop-selector](../../../includes/hdinsight-selector-use-sqoop.md)]

İçeri ve dışarı aktarma HDInsight kümesi ve bir Azure SQL veritabanı arasında veri için HDInsight Apache Sqoop'u kullanma konusunda bilgi edinin.

Apache Hadoop günlüklerini ve dosyaları gibi yapılandırılmamış ve yarı yapılandırılmış verileri işlemek için doğal bir seçim olsa da olabilir ilişkisel veritabanlarının depolanan yapılandırılmış verileri işlemek için bir gereksinim.

[Apache Sqoop](https://sqoop.apache.org/docs/1.99.7/user.html) Hadoop kümeleri ve ilişkisel veritabanları arasında veri aktarmak için tasarlanmış bir araçtır. SQL Server, MySQL veya Oracle Hadoop dağıtılmış dosya sistemi (HDFS) ile Hadoop MapReduce veya Apache Hive ile verileri dönüştürün ve ardından bir RDBMS'de geri verileri dışarı aktarma gibi bir ilişkisel veritabanı yönetim sistemi (RDBMS) verilerini almak için kullanın . Bu makalede, bir SQL Server veritabanı ilişkisel veritabanınız için kullanırsınız.

> [!IMPORTANT]  
> Bu makalede, veri aktarımı gerçekleştirmek için bir test ortamını ayarlar. Bu ortam için bir veri aktarım yöntemini bölümündeki yöntemleri birinden ardından [Sqoop çalıştırma işleri](#run-sqoop-jobs)aşağıda daha.

HDInsight kümelerinde desteklenir Sqoop sürümleri için bkz: [HDInsight tarafından sağlanan küme sürümlerindeki yenilikler nelerdir?](../hdinsight-component-versioning.md)

## <a name="understand-the-scenario"></a>Senaryoyu anlama

HDInsight küme bazı örnek verilerle birlikte gelir. Aşağıdaki iki örnek kullanabilirsiniz:

* Şu konumdadır bir Apache Log4j günlük dosyasını `/example/data/sample.log`. Aşağıdaki günlüklere dosyasından ayıklanır:

```text
2012-02-03 18:35:34 SampleClass6 [INFO] everything normal for id 577725851
2012-02-03 18:35:34 SampleClass4 [FATAL] system problem at id 1991281254
2012-02-03 18:35:34 SampleClass3 [DEBUG] detail for id 1304807656
...
```

* Adlı bir Hive tablosu `hivesampletable`, başvuruda bulunan veri dosyası `/hive/warehouse/hivesampletable`. Tablo bazı mobil cihaz verilerini içerir.
  
  | Alan | Veri türü |
  | --- | --- |
  | ClientID |string |
  | querytime |string |
  | Pazar |string |
  | deviceplatform |string |
  | devicemake |string |
  | devicemodel |string |
  | durum |string |
  | Ülke |string |
  | querydwelltime |double |
  | oturum kimliği |bigint |
  | sessionpagevieworder |bigint |

Bu makalede, test Sqoop alma ve dışarı aktarmak için bu iki veri kümesi kullanın.

## <a name="create-cluster-and-sql-database"></a>Test ortamını ayarlama
Küme, SQL veritabanı ve diğer nesneleri, bir Azure Resource Manager şablonu kullanarak Azure portalı üzerinden oluşturulur. Şablon bulunabilir [Azure hızlı başlangıç şablonları](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-with-sql-database/). Resource Manager şablonu bir SQL veritabanı'na Tablo şemalarını dağıtmak için bir bacpac paketi çağırır.  Bacpac paketi bir ortak blob kapsayıcısında yer https://hditutorialdata.blob.core.windows.net/usesqoop/SqoopTutorial-2016-2-23-11-2.bacpac. Özel bir kapsayıcı bacpac dosyalarını kullanmak istiyorsanız, şablonda aşağıdaki değerleri kullanın:

```json
"storageKeyType": "Primary",
"storageKey": "<TheAzureStorageAccountKey>",
```

> [!NOTE]  
> Şablon kullanarak içeri aktarın veya Azure portalı, yalnızca bir BACPAC dosyasını Azure blob depolama alanından içeri destekler.

1. Aşağıdaki görüntüde Azure portalında Resource Manager şablonu açmak için seçin.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-with-sql-database%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-use-sqoop/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Aşağıdaki özellikleri girin:

    |Alan |Değer |
    |---|---|
    |Abonelik |Aşağı açılan listeden Azure aboneliğinizi seçin.|
    |Kaynak grubu |Aşağı açılan listeden kaynak grubunuzu seçin veya yeni bir tane oluşturun|
    |Konum |Aşağı açılan listeden bir bölge seçin.|
    |Küme Adı |Hadoop kümesi için bir ad girin. Yalnızca küçük harf kullanın.|
    |Küme Oturum Açma Kullanıcı Adı |Önceden doldurulmuş değeri tutun `admin`.|
    |Küme Oturum Açma Parolası |Parola girin.|
    |SSH kullanıcı adı |Önceden doldurulmuş değeri tutun `sshuser`.|
    |SSH parolası |Parola girin.|
    |SQL Yöneticisi oturum açma |Önceden doldurulmuş değeri tutun `sqluser`.|
    |SQL yönetici parolası |Parola girin.|
    |_artifacts konumu | Farklı bir konumda kendi bacpac dosyanızı kullanmak istediğiniz sürece varsayılan değeri kullanın.|
    |_artifacts konumu Sas belirteci |Boş bırakın.|
    |Bacpac dosyası adı |Kendi bacpac dosyanızı kullanmak istediğiniz sürece varsayılan değeri kullanın.|
    |Konum |Varsayılan değeri kullanın.|

    Azure SQL sunucusu adı olacaktır `<ClusterName>dbserver`. Veritabanı adı `<ClusterName>db`. Varsayılan depolama hesabı adı olacaktır `e6qhezrh2pdqu`.

3. Seçin **hüküm ve koşulları yukarıda belirtilen kabul ediyorum**.

4. **Satın al**'ı seçin. Dağıtımı şablon dağıtımı için dağıtım gönderme başlıklı yeni bir kutucuk görürsünüz. Kümenin ve SQL veritabanının oluşturulması yaklaşık 20 dakika sürer.

## <a name="run-sqoop-jobs"></a>Sqoop işleri çalıştırma

HDInsight, çeşitli yöntemler kullanarak Sqoop işleri çalıştırabilirsiniz. Hangi yöntemin size uygun olduğuna karar vermek için aşağıdaki tabloyu kullanın ve ardından bir kılavuz için bağlantıyı izleyin.

| **Bu** isterseniz... | ...an **etkileşimli** Kabuk | ...**toplu** işleme | ...from bu **istemci işletim sistemi** |
|:--- |:---:|:---:|:--- |:--- |
| [SSH](apache-hadoop-use-sqoop-mac-linux.md) |? |? |Linux, UNIX, Mac OS X veya Windows |
| [Hadoop için .NET SDK](apache-hadoop-use-sqoop-dotnet-sdk.md) |&nbsp; |?  |Windows (şimdilik ile) |
| [Azure PowerShell](apache-hadoop-use-sqoop-powershell.md) |&nbsp; |? |Windows |

## <a name="limitations"></a>Sınırlamalar

* Toplu Dışarı Aktar - ile Linux tabanlı HDInsight, Microsoft SQL Server veya Azure SQL veritabanı için verileri dışarı aktarmak için kullanılan Sqoop bağlayıcısının toplu ekleme şu anda desteklemiyor.
* -Kullanırken, Linux tabanlı HDInsight ile toplu işleme `-batch` ekler gerçekleştirirken geçiş, Sqoop toplu INSERT işlemler yerine birden çok ekleme yapar.

## <a name="next-steps"></a>Sonraki adımlar
Artık Sqoop kullanmayı öğrendiniz. Daha fazla bilgi için bkz:

* [Apache Hive, HDInsight ile kullanma](../hdinsight-use-hive.md)
* [Apache Pig, HDInsight ile kullanma](../hdinsight-use-pig.md)
* [HDInsight için verileri karşıya](../hdinsight-upload-data.md): HDInsight/Azure Blob depolama alanına veri yüklemek için diğer yöntemler bulun.
