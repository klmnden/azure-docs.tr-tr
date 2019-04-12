---
title: Azure Cosmos öykünücü ile yerel olarak geliştirme
description: Azure Cosmos öykünücüsü'nü kullanarak geliştirin ve bir Azure aboneliği oluşturmadan için yerel uygulamanızı ücretsiz test edin.
ms.service: cosmos-db
ms.topic: tutorial
ms.date: 04/20/2018
author: deborahc
ms.author: dech
ms.openlocfilehash: 3d535c71480693d0424c6697776a1ddbf37b47c5
ms.sourcegitcommit: 1a19a5845ae5d9f5752b4c905a43bf959a60eb9d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/11/2019
ms.locfileid: "59493221"
---
# <a name="use-the-azure-cosmos-emulator-for-local-development-and-testing"></a>Yerel geliştirme ve test için Azure Cosmos öykünücüsünü kullanma

Azure Cosmos öykünücüsü'nü geliştirme amacıyla Azure Cosmos DB hizmetine öykünür yerel bir ortam sağlar. Azure Cosmos öykünücüsü'nü kullanarak geliştirme ve bir Azure aboneliği oluşturmadan veya masraf yapmadan uygulamanızı yerel olarak test edin. Uygulamanızı Azure Cosmos öykünücüsü'nde nasıl çalıştığı ile memnun kaldığınızda, bulutta bir Azure Cosmos hesabı kullanmaya başlayabilirsiniz.

Azure Cosmos öykünücüsü'nü ile geliştirebilirsiniz [SQL](local-emulator.md#sql-api), [Cassandra](local-emulator.md#cassandra-api), [MongoDB](local-emulator.md#azure-cosmos-dbs-api-for-mongodb), [Gremlin](local-emulator.md#gremlin-api), ve [tablo](local-emulator.md#table-api) API hesaplarını. Ancak şu anda öykünücü Veri Gezgini görünümünde tamamen istemcileri SQL API'si için yalnızca destekler. 

## <a name="how-the-emulator-works"></a>Öykünücü nasıl çalışır?

Azure Cosmos öykünücüsü'nü bir Azure Cosmos DB hizmetinin yüksek kaliteli öykünmesi sağlar. Azure Cosmos oluşturmak ve verileri sorgulamak için destek dahil olmak üzere sağlama DB olarak aynı işlevselliği destekler ve kapsayıcıları ölçekleme ve yürütme saklı yordamlar ve tetikleyiciler. Geliştirme ve Azure Cosmos öykünücüsü'nü uygulamaları test edin ve yalnızca tek bir yapılandırma için Azure Cosmos DB bağlantı uç noktası için değişiklik yaparak global bir ölçekte Azure'a dağıtın.

Azure Cosmos DB hizmetinin öykünmesi aslına sadık olsa da, öykünücünün uygulaması hizmetten farklıdır. Örneğin öykünücü, kalıcılık için yerel dosya sistemi gibi standart işletim sistemi bileşenlerini ve bağlantı için HTTPS protokolü yığınını kullanır. Genel çoğaltma gibi Azure altyapı dayanan işlevselliği okumayı/yazmayı ve ince ayarlanabilir tutarlılık düzeyleri için Tek haneli milisaniyelik gecikme kazanılan geçerli değildir.

Kullanarak Azure Cosmos öykünücü ve Azure Cosmos DB hizmetini arasında verileri geçirebilirsiniz [Azure Cosmos DB veri geçiş aracı](https://github.com/azure/azure-documentdb-datamigrationtool).

Azure Cosmos öykünücüsü Windows Docker kapsayıcısını çalıştırın, bkz: [Docker Hub](https://hub.docker.com/r/microsoft/azure-cosmosdb-emulator/) docker çekme komutu için ve [GitHub](https://github.com/Azure/azure-cosmos-db-emulator-docker) öykünücü kaynak kodunuz için.

## <a name="differences-between-the-emulator-and-the-service"></a>Öykünücü ile hizmet arasındaki farklar

Azure Cosmos öykünücüsü'nü çalıştıran yerel geliştirici iş istasyonunda benzetilmiş bir ortam sağladığından, bazı işlevsel farklılıklar öykünücü ve Azure Cosmos hesabınız arasında bulutta vardır:

* Şu anda veri Gezgini'nde öykünücü SQL API'si için istemcileri destekler. Veri Gezgini görünümü ve işlemleri için Azure Cosmos DB API'leri MongoDB, tablo, grafik ve Cassandra API gibi tam olarak desteklenmez.
* Azure Cosmos öykünücüsü, yalnızca tek bir sabit hesap ve iyi bilinen bir ana anahtar destekler. Anahtarı yeniden üretme Azure Cosmos öykünücüsü'nde mümkün değildir, bu komut satırı seçeneğini kullanarak varsayılan anahtarı değiştirilebilir ancak.
* Azure Cosmos öykünücüsü'nü ölçeklenebilir bir hizmet değil ve çok sayıda kapsayıcı desteklemez.
* Azure Cosmos öykünücüsü'nü farklı sunmaz [Azure Cosmos DB tutarlılık düzeyleri](consistency-levels.md).
* Azure Cosmos öykünücüsü'nü teklif değil [çok bölgeli çoğaltma](distribute-data-globally.md).
* Azure Cosmos öykünücüsü'nü kopyanızın her zaman Azure Cosmos DB hizmetini en son değişikliklerle güncel olmayabilir olarak başvurmanız gerekir [Azure Cosmos DB kapasite Planlayıcısı](https://www.documentdb.com/capacityplanner) üretim doğru şekilde tahmin etmek için Uygulamanızın ihtiyaçlarını aktarım hızını (RU).
* Varsayılan olarak Azure Cosmos öykünücüsü'nü kullanırken, en fazla 25 sabit boyutlu kapsayıcıların (yalnızca Azure Cosmos DB SDK'larını kullanarak desteklenir) veya Azure Cosmos öykünücüsü'nü 5 sınırsız kapsayıcılar oluşturabilirsiniz. Bu değeri değiştirme hakkında daha fazla bilgi için bkz. [PartitionCount değerini ayarlama](#set-partitioncount).

## <a name="system-requirements"></a>Sistem gereksinimleri

Azure Cosmos öykünücüsü'nü donanım ve yazılım gereksinimleri şunlardır:

* Yazılım gereksinimleri
  * Windows Server 2012 R2, Windows Server 2016 veya Windows 10
  * 64-bit işletim sistemi
* Minimum Donanım gereksinimleri
  * 2-GB RAM
  * 10 GB kullanılabilir sabit disk alanı

## <a name="installation"></a>Yükleme

Azure Cosmos Öykünücüsünden yükleyip [Microsoft Download Center](https://aka.ms/cosmosdb-emulator) veya öykünücü için Docker Windows üzerinde çalıştırabilirsiniz. Öykünücüyü Docker for Windows'da kullanmaya ilişkin yönergeler için bkz. [Docker üzerinde çalıştırma](#running-on-docker).

> [!NOTE]
> Yükleme, yapılandırma ve Azure Cosmos öykünücüyü çalıştırmak için bilgisayarda yönetici ayrıcalıkları olmalıdır. Öykünücü, bir sertifika oluşturun/ekleyin ve ayrıca hizmetlerini çalıştırmak için güvenlik duvarı kuralları ayarlamanıza; Bu nedenle öykünücü gibi işlemleri yürütmek gereklidir.

## <a name="running-on-windows"></a>Windows’da çalıştırma

Azure Cosmos öykünücü başlatmak için Başlat düğmesine basın veya Windows tuşuna basın. Yazmaya başlayın **Azure Cosmos öykünücüsü**ve öykünücü uygulamalar listesinden seçin.

![Başlat düğmesine basın ya da Windows tuşuna basın, yazmaya başlayın ** Azure Cosmos öykünücü ** ve uygulamalar listesinden öykünücüyü seçin](./media/local-emulator/database-local-emulator-start.png)

Öykünücü çalıştırıldığında, Windows görev çubuğu bildirim alanında bir simge görürsünüz. ![Azure Cosmos DB yerel öykünücüsü görev çubuğu bildirimi](./media/local-emulator/database-local-emulator-taskbar.png)

Varsayılan olarak Azure Cosmos öykünücüsü'nü 8081 numaralı bağlantı noktasında dinleme yerel makinede ("localhost") çalışır.

Azure Cosmos öykünücü yüklü olduğu `C:\Program Files\Azure Cosmos DB Emulator` varsayılan olarak. Komut satırından da öykünücüyü başlatabilir ve durdurabilirsiniz. Daha fazla bilgi için bkz. [komut satırı aracı başvurusu](#command-line).

## <a name="start-data-explorer"></a>Veri Gezgini’ni Başlat

Azure Cosmos öykünücüsü'nü başlattığında, tarayıcınızda otomatik olarak Azure Cosmos Veri Gezgini'ni açar. Adres olarak görünür `https://localhost:8081/_explorer/index.html`. Explorer'ı kapatın ve daha sonra yeniden açmak istiyorsanız, URL'yi tarayıcınızda açın veya aşağıda gösterildiği gibi Azure Cosmos öykücüsünden Windows Tepsi simgesinde başlatın.

![Azure Cosmos yerel öykünücü Veri Gezgini Başlatıcısı](./media/local-emulator/database-local-emulator-data-explorer-launcher.png)

## <a name="checking-for-updates"></a>Güncelleştirmeleri denetleme

Veri Gezgini, indirilebilir yeni bir güncelleştirme olup olmadığını belirtir.

> [!NOTE]
> Veri birinde oluşturulan Azure Cosmos öykünücüsü'nü sürümünü (%LOCALAPPDATA%\CosmosDBEmulator ya da veri yolu isteğe bağlı ayarlar bakın) garanti edilmez farklı bir sürümünü kullanırken erişilebilir olması. Uzun süreli verilerinizin kalıcı hale getirilmesi gerekiyorsa, Azure Cosmos hesabınız yerine Azure Cosmos öykünücüsü'nü verileri saklamanız önerilir.

## <a name="authenticating-requests"></a>İsteklerin kimliğini doğrulama

Olarak bulutta, Azure Cosmos DB ile Azure Cosmos öykünücüsüne karşı yaptığınız her isteğin kimliğinin doğrulanması gerekir. Azure Cosmos öykünücüsü'nü ana anahtarı kimlik doğrulaması için tek bir sabit hesap ve iyi bilinen bir kimlik doğrulama anahtarı destekler. Bu hesabı ve anahtarı Azure Cosmos öykünücüsü ile kullanmak için izin verilen tek kimlik bilgileridir. Bunlar:

```bash
Account name: localhost:<port>
Account key: C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==
```

> [!NOTE]
> Azure Cosmos öykünücüsü tarafından desteklenen ana anahtarı yalnızca öykünücü ile kullanıma yöneliktir. Üretim Azure Cosmos DB hesabınız ve anahtarı Azure Cosmos öykünücü ile kullanamazsınız.

> [!NOTE]
> Öykünücü /Key seçeneğiyle başlattıysanız, sonra yerine oluşturulan anahtarı kullanın `C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==`. /Key seçenek hakkında daha fazla bilgi için bkz. [komut satırı aracını referans.](#command-line-syntax)

Olarak Azure Cosmos DB ile Azure Cosmos öykünücüsü'nü yalnızca SSL aracılığıyla güvenli iletişimi destekler.

## <a name="running-on-a-local-network"></a>Yerel ağ üzerinde çalışma

Yerel bir ağ üzerinde öykünücüyü çalıştırabilirsiniz. Ağ erişimi etkinleştirmek üzere belirtin `/AllowNetworkAccess` adresindeki seçeneği [komut satırı](#command-line-syntax), gerektiren belirttiğiniz `/Key=key_string` veya `/KeyFile=file_name`. Kullanabileceğiniz `/GenKeyFile=file_name` önceden rastgele bir anahtar ile bir dosya oluşturabilirsiniz. Olarak geçirebilirsiniz sonra `/KeyFile=file_name` veya `/Key=contents_of_file`.

İlk kez ağ erişimi etkinleştirmek için kullanıcı öykünücüyü kapatın ve öykünücü'nın veri dizini (% LOCALAPPDATA%\CosmosDBEmulator) silin.

## <a name="developing-with-the-emulator"></a>Öykünücü ile geliştirme

### <a name="sql-api"></a>SQL API’si

Azure Cosmos masaüstünüzde çalışan öykünücüsü'nü aldıktan sonra herhangi bir desteklenen kullanabilirsiniz [Azure Cosmos DB SDK'sı](sql-api-sdk-dotnet.md) veya [Azure Cosmos DB REST API](/rest/api/cosmos-db/) öykünücü ile etkileşim kurmak için. Azure Cosmos öykünücüsü'nü kapsayıcıları SQL API veya Cosmos DB için Mongo DB API ve görünüm oluşturma ve herhangi bir kod yazmadan öğelerini düzenle olanak tanıyan yerleşik bir Veri Gezgini ayrıca içerir.

```csharp
// Connect to the Azure Cosmos Emulator running locally
DocumentClient client = new DocumentClient(
   new Uri("https://localhost:8081"), "C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==");

```

### <a name="azure-cosmos-dbs-api-for-mongodb"></a>MongoDB için Azure Cosmos DB API'si

Kullanıyorsanız [Azure Cosmos DB'nin MongoDB API'si](mongodb-introduction.md), aşağıdaki bağlantı dizesi kullanın:

```bash
mongodb://localhost:C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==@localhost:10255/admin?ssl=true
```

### <a name="table-api"></a>Tablo API’si

Azure Cosmos masaüstünüzde çalışan öykünücüsü'nü açtıktan sonra kullanabileceğiniz [Azure Cosmos DB tablo API'si SDK'sı](table-storage-how-to-use-dotnet.md) öykünücü ile etkileşim kurmak için. Öykünücü ile bir yönetici olarak bir komut isteminden başlatmak "/ EnableTableEndpoint". Ardından tablo API'si hesabına bağlanmak için aşağıdaki kodu çalıştırın:

```csharp
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Table;
using CloudTable = Microsoft.WindowsAzure.Storage.Table.CloudTable;
using CloudTableClient = Microsoft.WindowsAzure.Storage.Table.CloudTableClient;

string connectionString = "DefaultEndpointsProtocol=http;AccountName=localhost;AccountKey=C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==;TableEndpoint=http://localhost:8902/;";

CloudStorageAccount account = CloudStorageAccount.Parse(connectionString);
CloudTableClient tableClient = account.CreateCloudTableClient();
CloudTable table = tableClient.GetTableReference("testtable");
table.CreateIfNotExists();
table.Execute(TableOperation.Insert(new DynamicTableEntity("partitionKey", "rowKey")));
```

### <a name="cassandra-api"></a>Cassandra API’si

Öykünücü ile bir yönetici komut isteminden başlatmak "/ EnableCassandraEndpoint". Alternatif olarak da ortam değişkeni ayarlayabilirsiniz `AZURE_COSMOS_EMULATOR_CASSANDRA_ENDPOINT=true`.

* [Python 2.7 yükleme](https://www.python.org/downloads/release/python-2716/)

* [Cassandra CLI/CQLSH yükleyin](http://cassandra.apache.org/download/)

* Normal komut istemi penceresinde aşağıdaki komutları çalıştırın:

  ```bash
  set Path=c:\Python27;%Path%
  cd /d C:\sdk\apache-cassandra-3.11.3\bin
  set SSL_VERSION=TLSv1_2
  set SSL_VALIDATE=false
  cqlsh localhost 10350 -u localhost -p C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw== --ssl
  ```

* CQLSH Kabuğu'nda Cassandra uç noktaya bağlanmak için aşağıdaki komutları çalıştırın:

  ```bash
  CREATE KEYSPACE MyKeySpace WITH replication = {'class':'MyClass', 'replication_factor': 1};
  DESCRIBE keyspaces;
  USE mykeyspace;
  CREATE table table1(my_id int PRIMARY KEY, my_name text, my_desc text);
  INSERT into table1 (my_id, my_name, my_desc) values( 1, 'name1', 'description 1');
  SELECT * from table1;
  EXIT
  ```

### <a name="gremlin-api"></a>Gremlin API

Öykünücü ile bir yönetici komut isteminden başlatmak "/ EnableGremlinEndpoint". Alternatif olarak da ortam değişkeni ayarlayabilirsiniz `AZURE_COSMOS_EMULATOR_GREMLIN_ENDPOINT=true`

* [Apache-tinkerpop-gremlin-konsol-3.3.4 yükleyin](http://tinkerpop.apache.org/downloads.html)

* Öykünücü'nın veri Gezgini'nde bir veritabanı "db1" ve "coll1"; bir koleksiyon oluşturma Bölüm anahtarı seçin "/ name"

* Normal komut istemi penceresinde aşağıdaki komutları çalıştırın:

  ```bash
  cd /d C:\sdk\apache-tinkerpop-gremlin-console-3.3.4-bin\apache-tinkerpop-gremlin-console-3.3.4
  
  copy /y conf\remote.yaml conf\remote-localcompute.yaml
  notepad.exe conf\remote-localcompute.yaml
    hosts: [localhost]
    port: 8901
    username: /dbs/db1/colls/coll1
    password: C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==
    connectionPool: {
    enableSsl: false}
    serializer: { className: org.apache.tinkerpop.gremlin.driver.ser.GraphSONMessageSerializerV1d0,
    config: { serializeResultToString: true  }}

  bin\gremlin.bat
  ```

* Gremlin Kabuğu'nda Gremlin uç noktaya bağlanmak için aşağıdaki komutları çalıştırın:

  ```bash
  :remote connect tinkerpop.server conf/remote-localcompute.yaml
  :remote console
  :> g.V()
  :> g.addV('person1').property(id, '1').property('name', 'somename1')
  :> g.addV('person2').property(id, '2').property('name', 'somename2')
  :> g.V()
  ```

## <a name="export-the-ssl-certificate"></a>SSL sertifikasını dışarı aktarma

.NET dilleri ve çalışma zamanı, Azure Cosmos DB yerel öykünücüsüne güvenli şekilde bağlanmak için Windows Sertifika Deposunu kullanır. Diğer dillerin kendi sertifikaları yönetme ve kullanma yöntemi vardır. Java kendi [sertifika deposunu](https://docs.oracle.com/cd/E19830-01/819-4712/ablqw/index.html) kullanırken Python ise [yuva sarmalayıcılarını](https://docs.python.org/2/library/ssl.html) kullanır.

Windows Sertifika Deposu ile tümleştirilmeyen çalışma zamanları ve dillerle kullanmak üzere bir sertifika edinmek için Windows Sertifika Yöneticisi’ni kullanarak bunu dışarı aktarmanız gerekir. Başlatın certlm.msc çalıştırarak ya da adım adım yönergeleri [Azure Cosmos öykünücü sertifikalarını dışarı aktarma](./local-emulator-export-ssl-certificates.md). Sertifika yöneticisi çalıştırıldıktan sonra aşağıda gösterildiği gibi Kişisel Sertifikaları açın ve sertifikayı BASE-64 kodlu X.509 (.cer) dosyası olarak "DocumentDBEmulatorCertificate" kolay adıyla dışarı aktarın.

![Azure Cosmos DB yerel öykünücüsü SSL sertifikası](./media/local-emulator/database-local-emulator-ssl_certificate.png)

X.509 sertifikası, [Java CA Sertifika Deposuna Sertifika Ekleme](https://docs.microsoft.com/azure/java-add-certificate-ca-store) bölümündeki yönergeler izlenerek Java sertifika deposuna içeri aktarılabilir. Sertifikayı sertifika deposuna içeri aktarıldıktan sonra istemcileri SQL ve Azure Cosmos DB'nin MongoDB API'si için Azure Cosmos öykünücüsüne Bağlan mümkün olacaktır.

Python ve Node.js SDK’larından öykünücüye bağlanırken SSL doğrulaması devre dışı bırakılır.

## <a id="command-line"></a>Komut satırı aracı başvurusu
Yükleme konumundan komut satırında başlatmak ve öykünücüsü'nü durdurun, seçenekleri yapılandırın ve diğer işlemleri gerçekleştirmek için kullanabilirsiniz.

### <a name="command-line-syntax"></a>Komut satırı sözdizimi

    CosmosDB.Emulator.exe [/Shutdown] [/DataPath] [/Port] [/MongoPort] [/DirectPorts] [/Key] [/EnableRateLimiting] [/DisableRateLimiting] [/NoUI] [/NoExplorer] [/?]

Seçenek listesini görüntülemek için komut satırına `CosmosDB.Emulator.exe /?` yazın.

|**Seçenek** | **Açıklama** | **Komut**| **Bağımsız Değişkenler**|
|---|---|---|---|
|[Bağımsız değişken yok] | Azure Cosmos öykünücüsü varsayılan ayarlarla kurmak başlatır. |CosmosDB.Emulator.exe| |
|[Yardım] |Desteklenen komut satırı bağımsız değişkenleri listesini görüntüler.|CosmosDB.Emulator.exe /? | |
| GetStatus |Azure Cosmos öykünücüsü'nü durumunu alır. Durum, çıkış kodu tarafından belirtilir: 1 = başlangıç, 2 çalışan, 3 = = durduruldu. Negatif çıkış kodu, bir hata oluştuğunu gösterir. Başka bir çıktı üretilmez. | CosmosDB.Emulator.exe /GetStatus| |
| Kapat| Azure Cosmos öykünücüsü'nü kapatır.| CosmosDB.Emulator.exe /Shutdown | |
|DataPath | Veri dosyalarının depolanacağı yolu belirtir. % LocalAppdata%\CosmosDBEmulator varsayılan değerdir. | CosmosDB.Emulator.exe /DataPath=\<datapath\> | \<DataPath\>: Erişilebilir bir yol |
|Bağlantı noktası | Öykünücü için kullanılacak bağlantı noktası numarasını belirtir. Varsayılan değer 8081'dir. |CosmosDB.Emulator.exe /Port=\<port\> | \<Bağlantı noktası\>: Tek bir bağlantı noktası numarası |
| MongoPort | MongoDB uyumluluk API’si için kullanılacak bağlantı noktası numarasını belirtir. 10255 olarak varsayılan değerdir. |CosmosDB.Emulator.exe /MongoPort = \<mongoport\>|\<mongoport\>: Tek bir bağlantı noktası numarası|
| CassandraPort | Cassandra uç noktası için kullanılacak bağlantı noktası numarasını belirtir. 10350 varsayılan değerdir. | CosmosDB.Emulator.exe /CassandraPort = \<cassandraport\> | \<cassandraport\>: Tek bir bağlantı noktası numarası |
| ComputePort | Belirtilen işlem Interop ağ geçidi hizmeti için kullanılacak bağlantı noktası numarası. Ağ geçidinin HTTP uç noktası araştırma bağlantı noktası ComputePort + 79 olarak hesaplanır. Bu nedenle, ComputePort ve ComputePort + 79 açık ve kullanılabilir olması gerekir. 8900, varsayılan değerler 8979. | CosmosDB.Emulator.exe /ComputePort = \<computeport\> | \<computeport\>: Tek bir bağlantı noktası numarası |
| EnableCassandraEndpoint | Cassandra API sağlar. | CosmosDB.Emulator.exe /EnableCassandraEndpoint | |
| EnableGremlinEndpoint | Gremlin API sağlar. | CosmosDB.Emulator.exe /EnableGremlinEndpoint | |
| GremlinPort | Gremlin uç noktası için kullanılacak bağlantı noktası numarası. 8901 varsayılan değerdir. | CosmosDB.Emulator.exe /GremlinPort=\<port\> | \<Bağlantı noktası\>: Tek bir bağlantı noktası numarası |
|TablePort | Azure tablosu uç noktası için kullanılacak bağlantı noktası numarası. 8902 varsayılan değerdir. | CosmosDB.Emulator.exe /TablePort =\<bağlantı noktası\> | \<Bağlantı noktası\>: Tek bir bağlantı noktası numarası|
| KeyFile | Yetkilendirme anahtarı belirtilen dosyadan okuma. Bir keyfile oluşturmak için /GenKeyFile seçeneğini kullanın | CosmosDB.Emulator.exe/keyfile =\<file_name\> | \<file_name\>: Dosyasının yolu |
| ResetDataPath | Yinelemeli olarak tüm dosyaları belirtilen yolda kaldırır. Bir yol belirtmezseniz, %LOCALAPPDATA%\CosmosDbEmulator için varsayılan olarak | CosmosDB.Emulator.exe /ResetDataPath[=<path>] | \<Yol\>: Dosya yolu  |
| StartTraces  |  Hata ayıklama izleme günlüklerini toplama başlatma. | CosmosDB.Emulator.exe /StartTraces | |
| StopTraces     | Hata ayıklama izleme günlükleri toplamayı durdurun. | CosmosDB.Emulator.exe /StopTraces  | |
|EnableTableEndpoint | Azure tablo API'si sağlar. | CosmosDB.Emulator.exe /EnableTableEndpoint | |
|FailOnSslCertificateNameMismatch | Sertifikanın SAN öykünücü ana bilgisayarın etki alanı adı, yerel IPv4 adresi, 'localhost' ve '127.0.0.1' içermiyorsa varsayılan olarak öykünücü otomatik olarak imzalanan SSL sertifikasını yeniden oluşturur. Bu seçenekle, öykünücü için bunun yerine başlangıçta başarısız olur. Ardından, oluşturmak ve yeni bir otomatik olarak imzalanan SSL sertifikası yüklemek için /GenCert seçeneği kullanmanız gerekir. | CosmosDB.Emulator.exe /FailOnSslCertificateNameMismatch  | |
| GenCert | Oluştur ve yeni bir otomatik olarak imzalanan SSL sertifikası yükleyin. İsteğe bağlı olarak ağ üzerinden öykünücü erişmek için ek DNS adlarını virgülle ayrılmış bir listesi dahil. | CosmosDB.Emulator.exe /GenCert [ \<ek dns adlarını virgülle ayrılmış listesi\>] | |
| DirectPorts |Doğrudan bağlantı için kullanılacak bağlantı noktalarını belirtir. Varsayılan değerler: 10251,10252,10253,10254. | CosmosDB.Emulator.exe /DirectPorts:\<directports\> | \<directports\>: 4 bağlantı noktalarının virgülle ayrılmış listesi |
| Anahtar |Öykünücü için yetkilendirme anahtarı. Anahtar, 64 bayt vektörün base 64 kodlaması olmalıdır. | CosmosDB.Emulator.exe /Key:\<key\> | \<Anahtar\>: Anahtarı bir 64-bayt vektörü base-64 kodlama olmalıdır|
| EnableRateLimiting | İstek oranını sınırlama davranışının etkinleştirildiğini belirtir. |CosmosDB.Emulator.exe /EnableRateLimiting | |
| DisableRateLimiting |İstek oranını sınırlama davranışının devre dışı bırakıldığını belirtir. |CosmosDB.Emulator.exe /DisableRateLimiting | |
| NoUI | Öykünücü kullanıcı arabirimini gösterme. | CosmosDB.Emulator.exe /NoUI | |
| NoExplorer | Başlangıçta veri gezginini gösterme. |CosmosDB.Emulator.exe /NoExplorer | | 
| PartitionCount | Bölümlenmiş kapsayıcıları maksimum sayısını belirtir. Bkz: [kapsayıcıları sayısını değiştirme](#set-partitioncount) daha fazla bilgi için. | CosmosDB.Emulator.exe /PartitionCount=\<partitioncount\> | \<bölüm sayısı\>: İzin verilen tek bölüm kapsayıcı sayısı. Varsayılan değer 25’tir. Maksimum izin verilen: 250.|
| DefaultPartitionCount| Bölümler bir bölünmüş kapsayıcı için varsayılan sayısını belirtir. | CosmosDB.Emulator.exe /DefaultPartitionCount=\<defaultpartitioncount\> | \<defaultpartitioncount\> varsayılan değer 25'dir.|
| AllowNetworkAccess | Bir ağ üzerinden öykünücüye erişilmesini sağlar. Ağ erişimini etkinleştirmek için /Key=\<key_string\> veya /KeyFile=\<file_name\> öğesini de geçirmeniz gerekir. | CosmosDB.Emulator.exe AllowNetworkAccess /Key =\<key_string\> veya CosmosDB.Emulator.exe /AllowNetworkAccess/keyfile =\<file_name\>| |
| NoFirewall | Güvenlik duvarı kuralları /AllowNetworkAccess seçeneği kullanıldığında ayarlamayın. |CosmosDB.Emulator.exe /NoFirewall | |
| GenKeyFile | Yeni bir yetkilendirme anahtarı oluşturun ve belirtilen dosyaya kaydedin. Oluşturulan anahtar, /Key veya /KeyFile seçenekleri ile kullanılabilir. | CosmosDB.Emulator.exe /GenKeyFile =\<anahtar dosyasının yolu\> | |
| Tutarlılık | Hesap için varsayılan tutarlılık düzeyini ayarlayın. | CosmosDB.Emulator.exe /Consistency=\<consistency\> | \<Tutarlılık\>: Değer şunlardan biri olmalıdır [tutarlılık düzeyleri](consistency-levels.md): Oturum, güçlü ve nihai, veya BoundedStaleness. Varsayılan değer: Oturum. |
| ? | Yardım iletisini gösterin.| | |

## <a id="set-partitioncount"></a>Kapsayıcıları sayısını değiştirin

Varsayılan olarak, en fazla 25 sabit boyutlu kapsayıcıların (yalnızca Azure Cosmos DB SDK'larını kullanarak desteklenir) veya Azure Cosmos öykünücüsü'nü 5 sınırsız kapsayıcılar oluşturabilirsiniz. Değiştirerek **bölüm sayısı** değer, en fazla 250 sabit boyutlu kapsayıcıların veya 50 sınırsız kapsayıcılar veya herhangi bir birleşimini 250 sabit boyutlu kapsayıcıların aşmayan iki oluşturabilirsiniz (burada bir sınırsız kapsayıcı = 5 sabit boyutu kapsayıcılar için). Ancak, öykünücünün kurulumunu ile 200'den fazla sabit boyutlu kapsayıcıları çalıştırmak için önerilmez. Disk g/ç işlemleri için ekler ek yükü nedeniyle, hangi API uç noktası kullanırken öngörülemeyen zaman aşımlarına neden.


Geçerli bölüm sayısı aşıldıktan sonra bir kapsayıcı oluşturma girişimi, öykünücü şu iletiyle ServiceUnavailable bir özel durum oluşturur.

"Ne yazık ki şu anda bu bölgede yüksek talep yaşayan olan ve isteğiniz şu anda yerine getiremiyor. Size daha fazla kapasite ve daha fazla çevrimiçi duruma getirmek için sürekli olarak çalışır ve yeniden denemeniz önerilir.
Lütfen e-posta çekinmeyin askcosmosdb@microsoft.com herhangi bir zamanda veya herhangi bir nedenle. Etkinlik Kimliği: 12345678-1234-1234-1234-123456789abc"

Azure Cosmos öykünücüsü'nde kullanılabilir kapsayıcı sayısını değiştirmek için aşağıdaki adımları çalıştırın:

1. Sağ tıklayarak tüm yerel Azure Cosmos öykünücüsü'nü verileri silmesi **Azure Cosmos DB öykünücüsü'nü** sistem tepsisi tıklatıp, ardından simgesine **veri Sıfırla...** .
2. Bu klasördeki tüm öykünücüsü verilerini Sil `%LOCALAPPDATA%\CosmosDBEmulator`.
3. Sistem tepsisindeki **Azure Cosmos DB Öykünücüsü** simgesine sağ tıklayıp **Çıkış**’a tıklayarak tüm açık örneklerden çıkın. Tüm örneklerin çıkması bir dakika sürebilir.
4. En son sürümünü yükleyin [Azure Cosmos öykünücüsü](https://aka.ms/cosmosdb-emulator).
5. 250 veya daha düşük bir değer ayarlayarak PartitionCount bayrağı ile öykünücüyü başlatın. Örneğin: `C:\Program Files\Azure Cosmos DB Emulator> CosmosDB.Emulator.exe /PartitionCount=100`.

## <a name="controlling-the-emulator"></a>Öykünücüyü denetleme

Öykünücü başlatma, durdurma, kaldırma ve hizmet durumunu almak için bir PowerShell modülü ile birlikte gelir. PowerShell modülü kullanmak için aşağıdaki cmdlet'i çalıştırın:

```powershell
Import-Module "$env:ProgramFiles\Azure Cosmos DB Emulator\PSModules\Microsoft.Azure.CosmosDB.Emulator"
```

yerleştirin `PSModules` dizininde, `PSModulesPath` ve aşağıdaki komutta gösterildiği gibi içeri aktarın:

```powershell
$env:PSModulesPath += "$env:ProgramFiles\Azure Cosmos DB Emulator\PSModules"
Import-Module Microsoft.Azure.CosmosDB.Emulator
```

Aşağıda, PowerShell’den öykünücüyü denetlemeye ilişkin komutların özeti verilmiştir:

### `Get-CosmosDbEmulatorStatus`

**Sözdizimi**

`Get-CosmosDbEmulatorStatus`

**Açıklamalar**

Bu ServiceControllerStatus değerlerden birini döndürür: ServiceControllerStatus.StartPending, ServiceControllerStatus.Running veya ServiceControllerStatus.Stopped.

### `Start-CosmosDbEmulator`

**Sözdizimi**

`Start-CosmosDbEmulator [-DataPath <string>] [-DefaultPartitionCount <uint16>] [-DirectPort <uint16[]>] [-MongoPort <uint16>] [-NoUI] [-NoWait] [-PartitionCount <uint16>] [-Port <uint16>] [<CommonParameters>]`

**Açıklamalar**

Öykünücüyü başlatır. Varsayılan olarak komut, öykünücünün istekleri kabul etmeye hazır olmasını bekler. Cmdlet’in öykünücüyü başlattığı anda geri dönmesini istiyorsanız -NoWait seçeneğini kullanın.

### `Stop-CosmosDbEmulator`

**Sözdizimi**

 `Stop-CosmosDbEmulator [-NoWait]`

**Açıklamalar**

Öykünücüyü durdurur. Varsayılan olarak bu komut, öykünücünün tamamen kapanmasını bekler. Cmdlet’in öykünücü kapanmaya başladığı anda geri dönmesini istiyorsanız -NoWait seçeneğini kullanın.

### `Uninstall-CosmosDbEmulator`

**Sözdizimi**

`Uninstall-CosmosDbEmulator [-RemoveData]`

**Açıklamalar**

Öykünücüyü kaldırır ve isteğe bağlı olarak $env:LOCALAPPDATA\CosmosDbEmulator içeriklerinin tamamını kaldırır.
Cmdlet, öykünücüyü kaldırmadan önce öykünücünün tamamen durdurulduğundan emin olur.

## <a name="running-on-docker"></a>Docker’da çalıştırma

Azure Cosmos öykünücüsü için Docker Windows üzerinde çalıştırılabilir. Öykünücü Docker for Oracle Linux üzerinde çalışmaz.

[Docker for Windows](https://www.docker.com/docker-windows) yüklendikten sonra, araç çubuğundaki Docker simgesine sağ tıklayıp **Windows kapsayıcılarına geç** seçeneğini belirleyerek Windows kapsayıcılarına geçin.

Daha sonra sık kullandığınız kabuktan aşağıdaki komutu çalıştırarak Docker Hub'dan Öykünücü görüntüsünü çekin.

```bash
docker pull microsoft/azure-cosmosdb-emulator
```
Görüntüyü başlatmak için aşağıdaki komutları çalıştırın.

Komut satırından:
```cmd

md %LOCALAPPDATA%\CosmosDBEmulator\bind-mount

docker run --name azure-cosmosdb-emulator --memory 2GB --mount "type=bind,source=%LOCALAPPDATA%\CosmosDBEmulator\bind-mount,destination=C:\CosmosDB.Emulator\bind-mount" --interactive --tty -p 8081:8081 -p 8900:8900 -p 8901:8901 -p 8902:8902 -p 10250:10250 -p 10251:10251 -p 10252:10252 -p 10253:10253 -p 10254:10254 -p 10255:10255 -p 10256:10256 -p 10350:10350 microsoft/azure-cosmosdb-emulator
```

PowerShell’den:
```powershell

md $env:LOCALAPPDATA\CosmosDBEmulator\bind-mount 2>null

docker run --name azure-cosmosdb-emulator --memory 2GB --mount "type=bind,source=$env:LOCALAPPDATA\CosmosDBEmulator\bind-mount,destination=C:\CosmosDB.Emulator\bind-mount" --interactive --tty -p 8081:8081 -p 8900:8900 -p 8901:8901 -p 8902:8902 -p 10250:10250 -p 10251:10251 -p 10252:10252 -p 10253:10253 -p 10254:10254 -p 10255:10255 -p 10256:10256 -p 10350:10350 microsoft/azure-cosmosdb-emulator

```

Yanıt şuna benzer:

```
Starting emulator
Emulator Endpoint: https://172.20.229.193:8081/
Master Key: C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==
Exporting SSL Certificate
You can import the SSL certificate from an administrator command prompt on the host by running:
cd /d %LOCALAPPDATA%\CosmosDBEmulatorCert
powershell .\importcert.ps1
--------------------------------------------------------------------------------------------------
Starting interactive shell
```

Şimdi istemcinizdeki yanıttan uç noktayı ve ana anahtar girişini kullanın ve SSL sertifikasını ana bilgisayarınıza içeri aktarın. SSL sertifikasını içeri aktarmak için, yönetici komut isteminden aşağıdakileri yapın:

Komut satırından:

```cmd
cd  %LOCALAPPDATA%\CosmosDBEmulator\bind-mount
powershell .\importcert.ps1
```

PowerShell’den:
```powershell
cd $env:LOCALAPPDATA\CosmosDBEmulator\bind-mount
.\importcert.ps1
```

Öykünücü başlatıldıktan sonra etkileşimli kabuğu kapatmak öykünücünün kapsayıcısını kapatır.

Veri Gezgini’ni açmak için tarayıcınızda aşağıdaki URL’ye gidin. Yukarıda gösterilen yanıt iletisinde öykünücü uç noktası sağlanır.

    https://<emulator endpoint provided in response>/_explorer/index.html


## <a name="troubleshooting"></a>Sorun giderme

Azure Cosmos öykünücü ile karşılaştığınız sorunları gidermenize yardımcı olması için aşağıdaki ipuçlarını kullanın:

- Öykünücünün yeni bir sürümünü yüklediyseniz ve hatalarla karşılaşıyorsanız, verilerinizi sıfırladığınızdan emin olun. Verilerinizi Azure Cosmos öykünücüsü sistem tepsisindeki simgeye sağ tıklayarak ve sıfırlama veri ardından sıfırlayabilirsiniz... Hataları çözmezse öykünücü ve öykünücü eski sürümlerini, kaldırabilirsiniz bulundu, "C:\Program files\Azure Cosmos DB öykünücüsü'nü" dizinini kaldırın ve öykünücü yeniden yükleyin. Yönergeler için bkz. [Yerel öykünücüden kaldırma](#uninstall).

- Azure Cosmos öykünücüsü'nü çökerse, '% LOCALAPPDATA%\CrashDumps' klasöründen döküm dosyalarını toplayın, sıkıştırmak ve e-posta ekleme [ askcosmosdb@microsoft.com ](mailto:askcosmosdb@microsoft.com).

- Kilitlenme karşılaşırsanız, `Microsoft.Azure.Cosmos.ComputeServiceStartupEntryPoint.exe`, bu performans sayaçlarını bozuk durumda olduğu bir belirti olabilir. Genellikle bir yönetici komut isteminden aşağıdaki komutu çalıştırarak sorunu giderir:

  ```cmd
  lodctr /R
   ```

- Bir bağlantı sorunu yaşarsanız [izleme dosyalarını toplayın](#trace-files), sıkıştırın ve bir e-postaya ekleyip [askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com) adresine gönderin.

- **Hizmet Kullanılamıyor** iletisi alırsanız öykünücü, ağ yığınını başlatamıyor olabilir. Pulse secure istemcisinin veya Juniper networks istemcisinin yüklü olup olmadığını denetleyin; bunların ağ filtresi sürücüleri soruna yol açıyor olabilir. Genellikle üçüncü taraf ağ filtresi sürücüleri kaldırıldığında sorun düzeltilir. Alternatif olarak, hangi öykünücü ağ iletişimi için normal Winsock geçecektir /DisableRIO ile öykünücüyü başlatın. 

- Öykünücü çalışırken bilgisayarınız uyku moduna geçer veya herhangi bir işletim sistemi güncelleştirmesi çalıştırırsa, bir **Service is currently unavailable** (Hizmet şu anda kullanılamıyor) iletisi alabilirsiniz. Öykünücü'nın verileri seçin ve windows bildirim Tepsisi üzerinde görüntülenen simgeyi sağ tıklayarak sıfırlama **sıfırlama veri**.

### <a id="trace-files"></a>İzleme dosyalarını toplama

Hata ayıklama izlemelerini toplamak için bir yönetici komut isteminden aşağıdaki komutları çalıştırın:

1. `cd /d "%ProgramFiles%\Azure Cosmos DB Emulator"`
2. `CosmosDB.Emulator.exe /shutdown`. Programın kapatıldığından emin olmak için sistem tepsisine bakın; bu bir dakika sürebilir. Ayrıca yalnızca tıklatabilirsiniz **çıkış** Azure Cosmos öykünücüsü kullanıcı arabiriminde.
3. `CosmosDB.Emulator.exe /starttraces`
4. `CosmosDB.Emulator.exe`
5. Sorunu yeniden oluşturun. Veri Gezgini çalışmıyorsa yalnızca hatayı yakalamak için tarayıcının birkaç saniye boyunca açılmasını beklemeniz gerekir.
5. `CosmosDB.Emulator.exe /stoptraces`
6. `%ProgramFiles%\Azure Cosmos DB Emulator` konumuna gidin ve docdbemulator_000001.etl dosyasını bulun.
7. Hata ayıklama için .etl dosyasını yeniden üretme adımlarıyla birlikte [askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com) adresine gönderin.

### <a id="uninstall"></a>Yerel öykünücüyü kaldırma

1. Sistem tepsisindeki Azure Cosmos öykünücü simgesine sağ tıklayıp sonra Çıkış'ı tıklatarak yerel öykünücü tüm açık örneklerini kapatın. Tüm örneklerin çıkması bir dakika sürebilir.
2. Windows arama kutusuna **Uygulamalar ve özellikler** yazın ve **Uygulamalar ve özellikler (Sistem ayarları)** sonucuna tıklayın.
3. Uygulamalar listesinde **Azure Cosmos DB Öykünücüsü**’ne gidip bunu seçin, **Kaldır**’a tıklayın, daha sonra onaylayıp yeniden **Kaldır**’a tıklayın.
4. Uygulama kaldırıldığında `%LOCALAPPDATA%\CosmosDBEmulator` klasörüne gidip klasörü silin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide ücretsiz yerel geliştirme için yerel öykünücünün nasıl kullanılacağını öğrendiniz. Artık sonraki öğreticiye devam edebilir ve öykünücü SSL sertifikalarının nasıl dışarı aktarılacağını öğrenebilirsiniz.

> [!div class="nextstepaction"]
> [Azure Cosmos öykünücü sertifikalarını dışarı aktarma](local-emulator-export-ssl-certificates.md)
