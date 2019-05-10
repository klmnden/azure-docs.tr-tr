---
title: Azure Veri Gezgini ve Spark kümeleri arasında verileri taşımak için Apache Spark için Azure Veri Gezgini Bağlayıcısı'nı kullanın.
description: Bu konu Azure Veri Gezgini ve Apache Spark kümeleri arasında verileri taşımak nasıl gösterir.
author: orspod
ms.author: orspodek
ms.reviewer: michazag
ms.service: data-explorer
ms.topic: conceptual
ms.date: 4/29/2019
ms.openlocfilehash: 854e29b67b6e24c583a98b5851bf17551cfcbf61
ms.sourcegitcommit: 4891f404c1816ebd247467a12d7789b9a38cee7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65441357"
---
# <a name="azure-data-explorer-connector-for-apache-spark-preview"></a>Azure Veri Gezgini Bağlayıcısı için Apache Spark (Önizleme)

[Apache Spark](https://spark.apache.org/) bir birleşik analiz için büyük ölçekli veri işleme altyapısıdır. Azure Veri Gezgini, büyük hacimli verileri gerçek zamanlı çözümleme için hızlı, tam olarak yönetilen bir veri analiz hizmetidir. 

Spark için Azure Veri Gezgini Bağlayıcısı, veri kaynağı ve veri havuzu özelliklerinin her ikisi de kullanmak için Azure Veri Gezgini ve Spark kümeleri arasında verileri taşımak için uygular. Azure Veri Gezgini ve Apache Spark'ı kullanarak, machine learning (ML), Ayıkla-Dönüştür-yükle (ETL) ve Log Analytics gibi senaryoları verilerle hedefleyen hızlı ve ölçeklendirilebilir uygulamalar oluşturabilirsiniz. Azure Veri Gezgini yazmak, batch ve akış modunda gerçekleştirilebilir.
Azure Veri Gezgini okumaya sütun ayıklama ve Azure veri Gezgini'nde verileri filtreleyerek, aktarılan veri hacmini azaltır koşul itme destekler.

Azure Veri Gezgini Spark Bağlayıcısı bir [açık kaynaklı proje](https://github.com/Azure/azure-kusto-spark) herhangi bir Spark kümesi üzerinde çalıştırabilirsiniz.

> [!NOTE]
> Bazı örneklere bakın, ancak bir [Azure Databricks](https://docs.azuredatabricks.net/) Spark kümesi, Azure Veri Gezgini Spark Bağlayıcısı, Databricks veya diğer bir Spark dağıtımını doğrudan bağımlılıkları almaz.

## <a name="prerequisites"></a>Önkoşullar

* [Bir Azure Veri Gezgini kümesi ile veritabanı oluşturma](/azure/data-explorer/create-cluster-database-portal) 
* Spark kümesi oluşturma
* Azure Veri Gezgini Bağlayıcısı kitaplığı ve listelenen kitaplıkları yükleme [bağımlılıkları](https://github.com/Azure/azure-kusto-spark#dependencies) aşağıdakiler dahil olmak üzere [Kusto Java SDK'sı](/azure/kusto/api/java/kusto-java-client-library) kitaplıkları:
    * [Kusto Data Client](https://mvnrepository.com/artifact/com.microsoft.azure.kusto/kusto-data)
    * [Kusto istemci alma](https://mvnrepository.com/artifact/com.microsoft.azure.kusto/kusto-ingest)
* Kitaplıkları için önceden oluşturulmuş [Spark 2.4, Scala 2.11](https://github.com/Azure/azure-kusto-spark/releases)

## <a name="how-to-build-the-spark-connector"></a>Spark Bağlayıcısı oluşturma

Spark Bağlayıcısı, gelen derlenebilir [kaynakları](https://github.com/Azure/azure-kusto-spark) aşağıda ayrıntılı olarak.

> [!NOTE]
> Bu adım isteğe bağlıdır. Önceden oluşturulmuş kitaplıkları kullanıyorsanız Git [Spark Küme kurulumu](#spark-cluster-setup).

### <a name="build-prerequisites"></a>Yapı önkoşulları

* Java 1.8 SDK'sı
* [Maven 3.x](https://maven.apache.org/download.cgi) yüklü
* Apache Spark sürümü 2.4.0 veya üzeri

> [!TIP]
> 2.3.x sürümlerinde de desteklenir, ancak pom.xml bağımlılık bazı değişiklikler gerektirebilir.

Scala/Java uygulamalarını Maven projesi tanımlarını kullanarak bağlamak için şu yapı uygulamanızla (en son sürümü değişebilir):

```Maven
   <dependency>
     <groupId>com.microsoft.azure</groupId>
     <artifactId>spark-kusto-connector</artifactId>
     <version>1.0.0-Beta-02</version>
   </dependency>
```

### <a name="build-commands"></a>Yapı komutları

Jar oluşturmak ve tüm testleri çalıştırmak için:

```
mvn clean package
```

Jar oluşturmak için tüm testleri çalıştırmak ve yerel Maven deponuza jar yükleyin:

```
mvn clean install
```

Daha fazla bilgi için [Bağlayıcısı kullanımı](https://github.com/Azure/azure-kusto-spark#usage).

## <a name="spark-cluster-setup"></a>Spark kümesi Kurulumu

> [!NOTE]
> Aşağıdaki adımları gerçekleştirirken en son Azure Veri Gezgini Spark Bağlayıcısı sürümü kullanmak için önerilir:

1. Aşağıdaki Spark küme ayarlarını, Spark 2.4 ve Scala 2.11 kullanarak Azure Databricks kümesinde göre ayarlayın: 

    ![Databricks küme ayarları](media/spark-connector/databricks-cluster.png)

1. Azure Veri Gezgini Bağlayıcısı kitaplığı içeri aktarın:

    ![Azure Veri Gezgini kitaplığı içeri aktarma](media/spark-connector/db-create-library.png)

1. Ek Bağımlılıklar ekleyin:

    ![Bağımlılıkları ekleyin](media/spark-connector/db-dependencies.png)

    > [!TIP]
    > Her Spark sürümü için doğru java sürümü bulunamadı [burada](https://github.com/Azure/azure-kusto-spark#dependencies).

1. Tüm gerekli kitaplıkların yüklü olduğunu doğrulayın:

    ![Yüklü kitaplıkları doğrulayın](media/spark-connector/db-libraries-view.png)

## <a name="authentication"></a>Kimlik Doğrulaması

Azure Veri Gezgini Spark Bağlayıcısı sağlar, Azure Active Directory (Azure AD) kullanarak kimlik doğrulaması bir [Azure AD uygulaması](#azure-ad-application-authentication), [Azure AD erişim belirteci](https://github.com/Azure/azure-kusto-spark/blob/dev/docs/Authentication.md#direct-authentication-with-access-token), [cihaz kimlik doğrulaması ](https://github.com/Azure/azure-kusto-spark/blob/dev/docs/Authentication.md#device-authentication) (için üretim dışı senaryolar için) veya [Azure anahtar kasası](https://github.com/Azure/azure-kusto-spark/blob/dev/docs/Authentication.md#key-vault). Kullanıcı, azure keyvault paketini yükleyin ve Key Vault kaynağına erişmek için uygulama kimlik bilgilerini sağlayın.

### <a name="azure-ad-application-authentication"></a>Azure AD uygulama kimlik doğrulaması

En basit ve yaygın kimlik doğrulama yöntemi. Bu yöntem, Azure Veri Gezgini Spark Bağlayıcısı kullanımı önerilir.

|Özellikler  |Açıklama  |
|---------|---------|
|**KUSTO_AAD_CLIENT_ID**     |   Azure AD uygulama (istemci) tanımlayıcısı.      |
|**KUSTO_AAD_AUTHORITY_ID**     |  Azure AD kimlik doğrulaması yetkilisi. Azure AD dizini (Kiracı) kimliği.        |
|**KUSTO_AAD_CLIENT_PASSWORD**    |    İstemcisi için Azure AD uygulama anahtarı.     |

### <a name="azure-data-explorer-privileges"></a>Azure Veri Gezgini ayrıcalıkları

Bir Azure Veri Gezgini kümesinde aşağıdaki ayrıcalıklara sahip olmanız gerekir:

* (Veri kaynağı) okumak için Azure AD uygulaması olmalıdır *Görüntüleyicisi* ayrıcalıkları hedef veritabanında veya *yönetici* hedef tablodaki ayrıcalıkları.
* Azure AD uygulama olmalıdır (veri havuzu) yazmak için *çıkışlara* hedef veritabanı ayrıcalıkları. Ayrıca olmalıdır *kullanıcı* ayrıcalıkları hedef veritabanında yeni tablolar oluşturmak için. Hedef Tablo zaten varsa, *yönetici* ayrıcalıkları hedef tablodaki yapılandırılabilir.
 
Azure Veri Gezgini asıl rolleri hakkında daha fazla bilgi için bkz. [rol tabanlı yetkilendirme](/azure/kusto/management/access-control/role-based-authorization). Güvenlik rolleri yönetmek için bkz: [güvenlik rolleri Yönetim](/azure/kusto/management/security-roles).

## <a name="spark-sink-writing-to-azure-data-explorer"></a>Spark havuzu: Azure veri Gezgini'ne yazma

1. Havuz parametreleri ayarlayın:

     ```scala
    val KustoSparkTestAppId = dbutils.secrets.get(scope = "KustoDemos", key = "KustoSparkTestAppId")
    val KustoSparkTestAppKey = dbutils.secrets.get(scope = "KustoDemos", key = "KustoSparkTestAppKey")
 
    val appId = KustoSparkTestAppId
    val appKey = KustoSparkTestAppKey
    val authorityId = "72f988bf-86f1-41af-91ab-2d7cd011db47"
    val cluster = "Sparktest.eastus2"
    val database = "TestDb"
    val table = "StringAndIntTable"
    ```

1. Spark DataFrame Azure Veri Gezgini kümeye toplu yazma:

    ```scala
    df.write
      .format("com.microsoft.kusto.spark.datasource")
      .option(KustoOptions.KUSTO_CLUSTER, cluster)
      .option(KustoOptions.KUSTO_DATABASE, database)
      .option(KustoOptions.KUSTO_TABLE, table)
      .option(KustoOptions.KUSTO_AAD_CLIENT_ID, appId)
      .option(KustoOptions.KUSTO_AAD_CLIENT_PASSWORD, appKey) 
      .option(KustoOptions.KUSTO_AAD_AUTHORITY_ID, authorityId)
      .save()
    ```

1. Akış veri yazma:

    ```scala    
    import org.apache.spark.sql.streaming.Trigger
    import java.util.concurrent.TimeUnit
    
    // Set up a checkpoint and disable codeGen. Set up a checkpoint and disable codeGen as a workaround for an known issue 
    spark.conf.set("spark.sql.streaming.checkpointLocation", "/FileStore/temp/checkpoint")
    spark.conf.set("spark.sql.codegen.wholeStage","false")
    
    // Write to a Kusto table fro streaming source
    val kustoQ = csvDf
          .writeStream
          .format("com.microsoft.kusto.spark.datasink.KustoSinkProvider")
          .options(Map(
            KustoOptions.KUSTO_CLUSTER -> cluster,
            KustoOptions.KUSTO_TABLE -> table,
            KustoOptions.KUSTO_DATABASE -> database,
            KustoOptions.KUSTO_AAD_CLIENT_ID -> appId,
            KustoOptions.KUSTO_AAD_CLIENT_PASSWORD -> appKey,
            KustoOptions.KUSTO_AAD_AUTHORITY_ID -> authorityId))
          .trigger(Trigger.Once)
    
    kustoQ.start().awaitTermination(TimeUnit.MINUTES.toMillis(8))
    ```

## <a name="spark-source-reading-from-azure-data-explorer"></a>Spark kaynağı: Azure veri Gezgini'nde okuma

1. Küçük miktarlarda veri okuma sırasında veri sorgusu tanımlayın:

    ```scala
    val conf: Map[String, String] = Map(
          KustoOptions.KUSTO_AAD_CLIENT_ID -> appId,
          KustoOptions.KUSTO_AAD_CLIENT_PASSWORD -> appKey,
          KustoOptions.KUSTO_QUERY -> s"$table | where (ColB % 1000 == 0) | distinct ColA"      
        )
    
    // Simplified syntax flavor
    import org.apache.spark.sql._
    import com.microsoft.kusto.spark.sql.extension.SparkExtension._
    import org.apache.spark.SparkConf
    
    val df = spark.read.kusto(cluster, database, "", conf)
    display(df)
    ```

1. Geçici bir blob depolama, büyük miktarlarda veri okuma sırasında sağlanmalıdır. Depolama kapsayıcısı SAS anahtarı veya depolama hesabı adı ve hesap anahtarını kapsayıcı adı sağlayın. Bu adım yalnızca, Spark Bağlayıcısı geçerli Önizleme sürümü için gerekli.

    ```scala
    // Use either container/account-key/account name, or container SaS
    val container = dbutils.secrets.get(scope = "KustoDemos", key = "blobContainer")
    val storageAccountKey = dbutils.secrets.get(scope = "KustoDemos", key = "blobStorageAccountKey")
    val storageAccountName = dbutils.secrets.get(scope = "KustoDemos", key = "blobStorageAccountName")
    // val storageSas = dbutils.secrets.get(scope = "KustoDemos", key = "blobStorageSasUrl")
    ```

    Yukarıdaki örnekte, biz bağlayıcı arabirimini kullanarak Key Vault erişim yok. Alternatif olarak, Databricks gizli dizileri kullanma daha kolay bir yöntem kullanın.

1. Azure veri Gezgini'nde okuyun:

    ```scala
    val df2 = spark.read.kusto(cluster, database, "ReallyBigTable", conf3)
    
    val dfFiltered = df2
      .where(df2.col("ColA").startsWith("row-2"))
      .filter("ColB > 12")
      .filter("ColB <= 21")
      .select("ColA")
    
    display(dfFiltered)
    ```
