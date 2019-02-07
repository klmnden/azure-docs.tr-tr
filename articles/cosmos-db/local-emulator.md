---
title: Azure Cosmos DB öykünücüsü ile yerel olarak geliştirme
description: Azure Cosmos DB Öykünücüsü’nü kullanarak, Azure aboneliği oluşturmadan uygulamanızı ücretsiz olarak yerel ortamda geliştirip test edebilirsiniz.
ms.service: cosmos-db
ms.topic: tutorial
ms.date: 04/20/2018
author: deborahc
ms.author: dech
ms.openlocfilehash: cbdc57489eb7ebd50e3ce7e2b4e0e4081aef8e27
ms.sourcegitcommit: 415742227ba5c3b089f7909aa16e0d8d5418f7fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/06/2019
ms.locfileid: "55770393"
---
# <a name="use-the-azure-cosmos-db-emulator-for-local-development-and-testing"></a>Yerel geliştirme ve test için Azure Cosmos DB Öykünücüsünü kullanma

|**İkili dosyaları**|[indirme MSI](https://aka.ms/cosmosdb-emulator)|| **Docker**|[Docker Hub](https://hub.docker.com/r/microsoft/azure-cosmosdb-emulator/)|| **Docker kaynak** | [GitHub](https://github.com/Azure/azure-cosmos-db-emulator-docker)|

Azure Cosmos DB Öykünücüsü, geliştirme amaçlı olarak Azure Cosmos DB hizmetine öykünen yerel bir ortam sağlar. Azure Cosmos DB Öykünücüsü’nü kullanarak Azure aboneliği oluşturmadan veya masraf yapmadan uygulamanızı yerel ortamda geliştirip test edebilirsiniz. Uygulamanızın Azure Cosmos DB Öykünücüsü’ndeki performansından memnun olduğunuzda bulut üzerinde Azure Cosmos DB hesabı kullanmaya başlayabilirsiniz.

Şu anda öykünücü veri Gezgini'nde yalnızca tam olarak istemcileri için API SQL API'sini ve Azure Cosmos DB'nin MongoDB için destekler. İstemcileri tablo, grafik ve Cassandra API için tam olarak desteklenmiyor.

Bu makale aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Öykünücüyü yükleme
> * İsteklerin kimliğini doğrulama
> * Öykünücüde Veri Gezginini kullanma
> * SSL sertifikalarını dışarı aktarma
> * Öykünücüyü komut satırından çağırma
> * Öykünücüyü Docker for Windows üzerinde çalıştırma
> * İzleme dosyalarını toplama
> * Sorun giderme

## <a name="how-the-emulator-works"></a>Öykünücü nasıl çalışır?

Azure Cosmos DB Öykünücüsü, Azure Cosmos DB hizmetinin aslına çok uygun bir öykünmesini sağlar. JSON belgeleri oluşturma ve sorgulama, koleksiyonlar sağlama ve ölçeklendirme ve saklı yordamları ve tetikleyicileri yürütme desteği de dahil olmak üzere, Azure Cosmos DB ile aynı işlevleri destekler. Azure Cosmos DB öykünücüsünü kullanarak uygulamalar geliştirebilir ve test edebilir, sonra Azure Cosmos DB için bağlantı uç noktası üzerinde tek bir yapılandırma değişikliği yaparak bunları Azure'da küresel ölçekte dağıtabilirsiniz.

Azure Cosmos DB hizmetinin öykünmesi aslına sadık olsa da, öykünücünün uygulaması hizmetten farklıdır. Örneğin öykünücü, kalıcılık için yerel dosya sistemi gibi standart işletim sistemi bileşenlerini ve bağlantı için HTTPS protokolü yığınını kullanır. Genel çoğaltma, okuma/yazma için tek basamaklı milisaniyelik gecikme süresi ve ayarlanabilir tutarlılık düzeyleri gibi Azure altyapısına dayalı olan bazı işlevler kullanılamaz.

## <a name="differences-between-the-emulator-and-the-service"></a>Öykünücü ile hizmet arasındaki farklar
Azure Cosmos DB Öykünücüsü, yerel geliştirici iş istasyonunda çalıştırılan öykünmüş bir ortam sağladığından, öykünücü ile buluttaki bir Azure Cosmos DB hesabı arasında bazı işlev farkları vardır:

* Şu anda veri Gezgini'nde öykünücü MongoDB için SQL API'sini ve Azure Cosmos DB API için istemcileri destekler. İstemcileri tablo, grafik ve Cassandra API için henüz desteklenmiyor.
* Azure Cosmos DB Öykünücüsü yalnızca tek bir sabit hesabı ve iyi bilinen bir ana anahtarı destekler. Azure Cosmos DB Öykünücüsü’nde anahtar yeniden oluşturma mümkün değildir.
* Azure Cosmos DB Öykünücüsü, ölçeklenebilir bir hizmet değildir ve çok sayıda koleksiyonu desteklemez.
* Azure Cosmos DB Öykünücüsü, farklı [Azure Cosmos DB tutarlılık düzeyinin](consistency-levels.md) benzetimini yapmaz.
* Azure Cosmos DB Öykünücüsü, [çok bölgeli çoğaltma](distribute-data-globally.md) benzetimini yapmaz.
* Azure Cosmos DB Öykünücüsü, Azure Cosmos DB hizmetinde kullanılabilir olan hizmet kotası geçersiz kılmalarını (örneğin, belge boyutu sınırları, artan bölümlenmiş koleksiyon depolama alanı) desteklemez.
* Azure Cosmos DB Öykünücüsü kopyanızın, Azure Cosmos DB hizmetindeki en son değişikliklerle güncelleştirilmemiş olabileceğinden, uygulamanızın üretim aktarım hızı (RU) gereksinimlerini doğru şekilde tahmin etmek için [Azure Cosmos DB kapasite planlayıcısını](https://www.documentdb.com/capacityplanner) kullanmalısınız.

## <a name="system-requirements"></a>Sistem gereksinimleri
Azure Cosmos DB Öykünücüsü aşağıdaki donanım ve yazılım gereksinimlerine sahiptir:

* Yazılım gereksinimleri
  * Windows Server 2012 R2, Windows Server 2016 veya Windows 10
*   Minimum Donanım gereksinimleri
  * 2-GB RAM
  * 10 GB kullanılabilir sabit disk alanı

## <a name="installation"></a>Yükleme
[Microsoft İndirme Merkezi](https://aka.ms/cosmosdb-emulator)’nden Azure Cosmos DB Öykünücüsü’nü indirip yükleyebilir veya Docker for Windows’da öykünücüyü çalıştırabilirsiniz. Öykünücüyü Docker for Windows'da kullanmaya ilişkin yönergeler için bkz. [Docker üzerinde çalıştırma](#running-on-docker).

> [!NOTE]
> Azure Cosmos DB Öykünücüsü’nü yüklemek, yapılandırmak ve çalıştırmak için bilgisayarda yönetici ayrıcalıklarına sahip olmanız gerekir.

## <a name="running-on-windows"></a>Windows’da çalıştırma

Azure Cosmos DB Öykünücüsü’nü başlatmak için Başlat düğmesini seçin veya Windows tuşuna basın. **Azure Cosmos DB Öykünücüsü** yazmaya başlayın ve uygulama listesinden öykünücüyü seçin.

![Başlat düğmesini seçin veya Windows tuşuna basın, **Azure Cosmos DB Öykünücüsü** yazmaya başlayın ve uygulama listesinden öykünücüyü seçin.](./media/local-emulator/database-local-emulator-start.png)

Öykünücü çalıştırıldığında, Windows görev çubuğu bildirim alanında bir simge görürsünüz. ![Azure Cosmos DB yerel öykünücü görev çubuğu bildirimi](./media/local-emulator/database-local-emulator-taskbar.png)

Azure Cosmos DB Öykünücüsü varsayılan olarak 8081 numaralı bağlantı noktasında dinleme işlemi yapan yerel makinede ("localhost") çalıştırılır.

Azure Cosmos DB Öykünücüsü varsayılan olarak `C:\Program Files\Azure Cosmos DB Emulator` dizinine yüklenir. Komut satırından da öykünücüyü başlatabilir ve durdurabilirsiniz. Daha fazla bilgi için bkz. [komut satırı aracı başvurusu](#command-line).

## <a name="start-data-explorer"></a>Veri Gezgini’ni Başlat

Azure Cosmos DB Öykünücüsü başlatıldığında otomatik olarak tarayıcınızda Azure Cosmos DB Veri Gezgini'ni açar. Adres, [https://localhost:8081/_explorer/index.html](https://localhost:8081/_explorer/index.html) olarak görüntülenir. Gezgini kapatırsanız ve daha sonra yeniden açmak isterseniz, tarayıcınızda URL’yi açabilir veya aşağıda gösterildiği gibi Windows Tepsisindeki Azure Cosmos DB Öykücüsü’nden gezgini başlatabilirsiniz.

![Azure Cosmos DB yerel öykünücüsü veri gezgini başlatıcısı](./media/local-emulator/database-local-emulator-data-explorer-launcher.png)

## <a name="checking-for-updates"></a>Güncelleştirmeleri denetleme
Veri Gezgini, indirilebilir yeni bir güncelleştirme olup olmadığını belirtir.

> [!NOTE]
> Azure Cosmos DB Öykünücüsü’nün bir sürümünde oluşturulan verilerin farklı bir sürüm kullanılırken erişilebilir olması garanti edilmez. Verilerinizin uzun vadeli kalıcı olması gerekiyorsa, verileri Azure Cosmos DB Öykünücüsü’nde değil, bir Azure Cosmos DB hesabında depolamanız önerilir.

## <a name="authenticating-requests"></a>İsteklerin kimliğini doğrulama
Bulutta Azure Cosmos DB ile olduğu gibi, Azure Cosmos DB Öykünücüsü’ne karşı yaptığınız her isteğin kimliği doğrulanmalıdır. Azure Cosmos DB Öykünücüsü, ana anahtar kimlik doğrulaması için iyi bilinen bir kimlik doğrulaması anahtarını ve tek bir sabit hesabı destekler. Azure Cosmos DB Öykünücüsü ile kullanılmasına izin verilen kimlik bilgileri yalnızca bu hesap ve anahtardır. Bunlar:

    Account name: localhost:<port>
    Account key: C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==

> [!NOTE]
> Azure Cosmos DB Öykünücüsü tarafından desteklenen yalnızca öykünücü ile kullanılmak üzere tasarlanmıştır. Üretim Azure Cosmos DB hesabınızı ve anahtarınızı Azure Cosmos DB Öykünücüsü ile kullanamazsınız.

> [!NOTE]
> Öykünücüyü /Key seçeneğiyle başlattıysanız, "C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==" yerine oluşturulan anahtarı kullanın

Tıpkı Azure Cosmos DB hizmeti gibi Azure Cosmos DB Öykünücüsü de yalnızca SSL aracılığıyla güvenli iletişimi destekler.

## <a name="running-on-a-local-network"></a>Yerel ağ üzerinde çalışma

Yerel bir ağ üzerinde öykünücüyü çalıştırabilirsiniz. Ağ erişimini etkinleştirmek için, [komut satırında](#command-line-syntax) /Key=key_string veya /KeyFile=file_name belirtmenizi de gerektiren /AllowNetworkAccess seçeneğini belirtin. Rastgele anahtar yenileme siparişi ile dosya oluşturmak için /GenKeyFile=file_name kullanabilirsiniz. Daha sonra bunu /KeyFile=file_name veya /Key=contents_of_file öğesine geçirebilirsiniz.

İlk kez ağ erişimini etkinleştirmek için kullanıcı, öykünücüyü kapatmalı ve öykünücünün veri dizinini (C:\Users\user_name\AppData\Local\CosmosDBEmulator) silmelidir.

## <a name="developing-with-the-emulator"></a>Öykünücü ile geliştirme
Azure Cosmos DB Öykünücüsü masaüstünüzde çalışmaya başladıktan sonra, Öykünücü ile etkileşim kurmak için desteklenen [Azure Cosmos DB SDK](sql-api-sdk-dotnet.md)'larından veya [Azure Cosmos DB REST API](/rest/api/cosmos-db/)'lerinden birini kullanabilirsiniz. Azure Cosmos DB öykünücüsü'nü, Mongo DB API ve görünüm SQL API veya Cosmos DB koleksiyonları oluşturmak ve herhangi bir kod yazmadan belgeleri düzenlemesine olanak tanıyan yerleşik bir Veri Gezgini ayrıca içerir.

    // Connect to the Azure Cosmos DB Emulator running locally
    DocumentClient client = new DocumentClient(
        new Uri("https://localhost:8081"),
        "C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==");

Kullanıyorsanız [Azure Cosmos DB MongoDB için protokol desteği kablo](mongodb-introduction.md), aşağıdaki bağlantı dizesi kullanın:

    mongodb://localhost:C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==@localhost:10255/admin?ssl=true

Azure Cosmos DB Öykünücüsü’ne bağlanmak için [Azure DocumentDB Studio](https://github.com/mingaliu/DocumentDBStudio) gibi mevcut araçları kullanabilirsiniz. [Azure Cosmos DB Veri Geçişi Aracı](https://github.com/azure/azure-documentdb-datamigrationtool)’nı kullanarak Azure Cosmos DB Öykünücüsü ile Azure Cosmos DB hizmeti arasında verileri de geçirebilirsiniz.

> [!NOTE]
> Öykünücüyü /Key seçeneğiyle başlattıysanız, "C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==" yerine oluşturulan anahtarı kullanın

Azure Cosmos DB Öykünücüsü'nü kullanırken varsayılan olarak en çok tek bölümlü 25 koleksiyon veya bölümlenmiş 1 koleksiyon oluşturabilirsiniz. Bu değeri değiştirme hakkında daha fazla bilgi için bkz. [PartitionCount değerini ayarlama](#set-partitioncount).

## <a name="export-the-ssl-certificate"></a>SSL sertifikasını dışarı aktarma

.NET dilleri ve çalışma zamanı, Azure Cosmos DB yerel öykünücüsüne güvenli şekilde bağlanmak için Windows Sertifika Deposunu kullanır. Diğer dillerin kendi sertifikaları yönetme ve kullanma yöntemi vardır. Java kendi [sertifika deposunu](https://docs.oracle.com/cd/E19830-01/819-4712/ablqw/index.html) kullanırken Python ise [yuva sarmalayıcılarını](https://docs.python.org/2/library/ssl.html) kullanır.

Windows Sertifika Deposu ile tümleştirilmeyen çalışma zamanları ve dillerle kullanmak üzere bir sertifika edinmek için Windows Sertifika Yöneticisi’ni kullanarak bunu dışarı aktarmanız gerekir. certlm.msc öğesini çalıştırarak bunu başlatabilir veya [Azure Cosmos DB Öykünücüsü Sertifikalarını dışarı aktarma](./local-emulator-export-ssl-certificates.md) bölümündeki adım adım yönergeleri izleyebilirsiniz. Sertifika yöneticisi çalıştırıldıktan sonra aşağıda gösterildiği gibi Kişisel Sertifikaları açın ve sertifikayı BASE-64 kodlu X.509 (.cer) dosyası olarak "DocumentDBEmulatorCertificate" kolay adıyla dışarı aktarın.

![Azure Cosmos DB yerel öykünücüsü SSL sertifikası](./media/local-emulator/database-local-emulator-ssl_certificate.png)

X.509 sertifikası, [Java CA Sertifika Deposuna Sertifika Ekleme](https://docs.microsoft.com/azure/java-add-certificate-ca-store) bölümündeki yönergeler izlenerek Java sertifika deposuna içeri aktarılabilir. Sertifikayı sertifika deposuna içeri aktarıldıktan sonra istemciler için SQL ve Azure Cosmos DB'nin MongoDB API'si için Azure Cosmos DB öykünücüsü'nü bağlanmak mümkün olacaktır.

Python ve Node.js SDK’larından öykünücüye bağlanırken SSL doğrulaması devre dışı bırakılır.

## <a id="command-line"></a>Komut satırı aracı başvurusu
Yükleme konumundan, komut satırını kullanarak öykünücüyü başlatıp durdurabilir, seçenekleri yapılandırabilir ve başka işlemler gerçekleştirebilirsiniz.

### <a name="command-line-syntax"></a>Komut satırı sözdizimi

    CosmosDB.Emulator.exe [/Shutdown] [/DataPath] [/Port] [/MongoPort] [/DirectPorts] [/Key] [/EnableRateLimiting] [/DisableRateLimiting] [/NoUI] [/NoExplorer] [/?]

Seçenek listesini görüntülemek için komut satırına `CosmosDB.Emulator.exe /?` yazın.

|**Seçenek** | **Açıklama** | **Komut**| **Bağımsız Değişkenler**|
|---|---|---|---|
|[Bağımsız değişken yok] | Varsayılan ayarlarla Azure Cosmos DB Öykünücüsünü başlatır. |CosmosDB.Emulator.exe| |
|[Yardım] |Desteklenen komut satırı bağımsız değişkenleri listesini görüntüler.|CosmosDB.Emulator.exe /? | |
| GetStatus |Azure Cosmos DB Öykünücüsü’nün durumunu alır. Durum, çıkış kodu tarafından belirtilir: 1 = başlangıç, 2 çalışan, 3 = = durduruldu. Negatif çıkış kodu, bir hata oluştuğunu gösterir. Başka bir çıktı üretilmez. | CosmosDB.Emulator.exe /GetStatus| |
| Kapat| Azure Cosmos DB Öykünücüsü’nü kapatır.| CosmosDB.Emulator.exe /Shutdown | |
|DataPath | Veri dosyalarının depolanacağı yolu belirtir. Varsayılan: %LocalAppdata%\CosmosDBEmulator. | CosmosDB.Emulator.exe /DataPath=\<datapath\> | \<DataPath\>: Erişilebilir bir yol |
|Bağlantı noktası | Öykünücü için kullanılacak bağlantı noktası numarasını belirtir. Varsayılan: 8081. |CosmosDB.Emulator.exe /Port=\<port\> | \<Bağlantı noktası\>: Tek bir bağlantı noktası numarası |
| MongoPort | MongoDB uyumluluk API’si için kullanılacak bağlantı noktası numarasını belirtir. Varsayılan: 10255. |CosmosDB.Emulator.exe /MongoPort = \<mongoport\>|\<mongoport\>: Tek bir bağlantı noktası numarası|
| DirectPorts |Doğrudan bağlantı için kullanılacak bağlantı noktalarını belirtir. Varsayılan değerler: 10251,10252,10253,10254. | CosmosDB.Emulator.exe /DirectPorts:\<directports\> | \<directports\>: 4 bağlantı noktalarının virgülle ayrılmış listesi |
| Anahtar |Öykünücü için yetkilendirme anahtarı. Anahtar, 64 bayt vektörün base 64 kodlaması olmalıdır. | CosmosDB.Emulator.exe /Key:\<key\> | \<Anahtar\>: Anahtarı bir 64-bayt vektörü base-64 kodlama olmalıdır|
| EnableRateLimiting | İstek oranını sınırlama davranışının etkinleştirildiğini belirtir. |CosmosDB.Emulator.exe /EnableRateLimiting | |
| DisableRateLimiting |İstek oranını sınırlama davranışının devre dışı bırakıldığını belirtir. |CosmosDB.Emulator.exe /DisableRateLimiting | |
| NoUI | Öykünücü kullanıcı arabirimini gösterme. | CosmosDB.Emulator.exe /NoUI | |
| NoExplorer | Başlangıçta veri gezginini gösterme. |CosmosDB.Emulator.exe /NoExplorer | | 
| PartitionCount | Maksimum bölümlenmiş koleksiyon sayısını belirtir. Daha fazla bilgi için bkz. [Koleksiyon sayısını değiştirme](#set-partitioncount). | CosmosDB.Emulator.exe /PartitionCount=\<partitioncount\> | \<bölüm sayısı\>: İzin verilen tek bölüm koleksiyonlarını maksimum sayısı. Varsayılan: 25. Maksimum izin verilen: 250.|
| DefaultPartitionCount| Bölümlenen bir koleksiyon için varsayılan bölüm sayısını belirtir. | CosmosDB.Emulator.exe /DefaultPartitionCount=\<defaultpartitioncount\> | \<defaultpartitioncount\> Varsayılan: 25.|
| AllowNetworkAccess | Bir ağ üzerinden öykünücüye erişilmesini sağlar. Ağ erişimini etkinleştirmek için /Key=\<key_string\> veya /KeyFile=\<file_name\> öğesini de geçirmeniz gerekir. | CosmosDB.Emulator.exe AllowNetworkAccess /Key =\<key_string\> veya CosmosDB.Emulator.exe /AllowNetworkAccess/keyfile =\<file_name\>| |
| NoFirewall | /AllowNetworkAccess kullanıldığında güvenlik duvarı kurallarını ayarlama. |CosmosDB.Emulator.exe /NoFirewall | |
| GenKeyFile | Yeni bir yetkilendirme anahtarı oluşturun ve belirtilen dosyaya kaydedin. Oluşturulan anahtar, /Key veya /KeyFile seçenekleri ile kullanılabilir. | CosmosDB.Emulator.exe /GenKeyFile =\<anahtar dosyasının yolu\> | |
| Tutarlılık | Hesap için varsayılan tutarlılık düzeyini ayarlayın. | CosmosDB.Emulator.exe /Consistency=\<consistency\> | \<Tutarlılık\>: Değer şunlardan biri olmalıdır [tutarlılık düzeyleri](consistency-levels.md): Oturum, güçlü ve nihai, veya BoundedStaleness. Varsayılan değer: Oturum. |
| ? | Yardım iletisini gösterin.| | |

## <a id="set-partitioncount"></a>Koleksiyon sayısını değiştirme

Azure Cosmos DB Öykünücüsü'nü kullanırken varsayılan olarak en çok tek bölümlü 25 koleksiyon veya bölümlenmiş 1 koleksiyon oluşturabilirsiniz. **PartitionCount** değerini değiştirerek en fazla 250 tekli bölüm koleksiyonu veya 10 bölümlenmiş koleksiyon ya da 250 tekli bölümü aşmayan iki tanesinin birleşimini (burada bir bölümlenmiş koleksiyon = 25 tekli bölüm koleksiyonu) oluşturabilirsiniz.

Geçerli bölüm sayısı aşıldıktan sonra bir koleksiyon oluşturmaya çalışırsanız öykünücü aşağıdaki iletiyle bir ServiceUnavailable istisnası oluşturur.

    Sorry, we are currently experiencing high demand in this region,
    and cannot fulfill your request at this time. We work continuously
    to bring more and more capacity online, and encourage you to try again.
    Please do not hesitate to email askcosmosdb@microsoft.com at any time or
    for any reason. ActivityId: 29da65cc-fba1-45f9-b82c-bf01d78a1f91

Azure Cosmos DB Öykünücüsü’nün kullanımına sunulan koleksiyon sayısını değiştirmek için aşağıdakileri yapın:

1. Sistem tepsisindeki **Azure Cosmos DB Öykünücüsü** simgesine sağ tıklayıp **Verileri Sıfırla…** seçeneğine tıklayarak tüm yerel Azure Cosmos DB Öykünücüsü verilerini silin.
2. Şu klasördeki tüm öykünücü verilerini silin: C:\Users\user_name\AppData\Local\CosmosDBEmulator.
3. Sistem tepsisindeki **Azure Cosmos DB Öykünücüsü** simgesine sağ tıklayıp **Çıkış**’a tıklayarak tüm açık örneklerden çıkın. Tüm örneklerin çıkması bir dakika sürebilir.
4. [Azure Cosmos DB Öykünücüsü](https://aka.ms/cosmosdb-emulator)’nün en son sürümünü yükleyin.
5. 250 veya daha düşük bir değer ayarlayarak PartitionCount bayrağı ile öykünücüyü başlatın. Örneğin: `C:\Program Files\Azure Cosmos DB Emulator> CosmosDB.Emulator.exe /PartitionCount=100`.

## <a name="controlling-the-emulator"></a>Öykünücüyü denetleme

Öykünücü hizmeti başlatma, durdurma, kaldırma ve hizmetin durumunu alma için bir PowerShell modülüyle birlikte gelir. Bunu kullanmak için:

```powershell
Import-Module "$env:ProgramFiles\Azure Cosmos DB Emulator\PSModules\Microsoft.Azure.CosmosDB.Emulator"
```

veya `PSModulesPath` yolunuza `PSModules` dizinini yerleştirin ve şunun gibi içeri aktarın:

```powershell
$env:PSModulesPath += "$env:ProgramFiles\Azure Cosmos DB Emulator\PSModules"
Import-Module Microsoft.Azure.CosmosDB.Emulator
```

Aşağıda, PowerShell’den öykünücüyü denetlemeye ilişkin komutların özeti verilmiştir:

### `Get-CosmosDbEmulatorStatus`

#### <a name="syntax"></a>Sözdizimi

`Get-CosmosDbEmulatorStatus`

#### <a name="remarks"></a>Açıklamalar

Bu ServiceControllerStatus değerlerden birini döndürür: ServiceControllerStatus.StartPending, ServiceControllerStatus.Running veya ServiceControllerStatus.Stopped.

### `Start-CosmosDbEmulator`

#### <a name="syntax"></a>Sözdizimi

`Start-CosmosDbEmulator [-DataPath <string>] [-DefaultPartitionCount <uint16>] [-DirectPort <uint16[]>] [-MongoPort <uint16>] [-NoUI] [-NoWait] [-PartitionCount <uint16>] [-Port <uint16>] [<CommonParameters>]`

#### <a name="remarks"></a>Açıklamalar

Öykünücüyü başlatır. Varsayılan olarak komut, öykünücünün istekleri kabul etmeye hazır olmasını bekler. Cmdlet’in öykünücüyü başlattığı anda geri dönmesini istiyorsanız -NoWait seçeneğini kullanın.

### `Stop-CosmosDbEmulator`

#### <a name="syntax"></a>Sözdizimi

 `Stop-CosmosDbEmulator [-NoWait]`

#### <a name="remarks"></a>Açıklamalar

Öykünücüyü durdurur. Varsayılan olarak bu komut, öykünücünün tamamen kapanmasını bekler. Cmdlet’in öykünücü kapanmaya başladığı anda geri dönmesini istiyorsanız -NoWait seçeneğini kullanın.

### `Uninstall-CosmosDbEmulator`

#### <a name="syntax"></a>Sözdizimi

`Uninstall-CosmosDbEmulator [-RemoveData]`

#### <a name="remarks"></a>Açıklamalar

Öykünücüyü kaldırır ve isteğe bağlı olarak $env:LOCALAPPDATA\CosmosDbEmulator içeriklerinin tamamını kaldırır.
Cmdlet, öykünücüyü kaldırmadan önce öykünücünün tamamen durdurulduğundan emin olur.

## <a name="running-on-docker"></a>Docker’da çalıştırma

Azure Cosmos DB Öykünücüsü, Docker for Windows üzerinde çalıştırılabilir. Öykünücü Docker for Oracle Linux üzerinde çalışmaz.

[Docker for Windows](https://www.docker.com/docker-windows) yüklendikten sonra, araç çubuğundaki Docker simgesine sağ tıklayıp **Windows kapsayıcılarına geç** seçeneğini belirleyerek Windows kapsayıcılarına geçin.

Daha sonra sık kullandığınız kabuktan aşağıdaki komutu çalıştırarak Docker Hub'dan Öykünücü görüntüsünü çekin.

```
docker pull microsoft/azure-cosmosdb-emulator
```
Görüntüyü başlatmak için aşağıdaki komutları çalıştırın.

Komut satırından:
```cmd
md %LOCALAPPDATA%\CosmosDBEmulatorCert 2>null
docker run -v %LOCALAPPDATA%\CosmosDBEmulatorCert:C:\CosmosDB.Emulator\CosmosDBEmulatorCert -P -t -i -m 2GB microsoft/azure-cosmosdb-emulator
```

PowerShell’den:
```powershell
md $env:LOCALAPPDATA\CosmosDBEmulatorCert 2>null
docker run -v $env:LOCALAPPDATA\CosmosDBEmulatorCert:C:\CosmosDB.Emulator\CosmosDBEmulatorCert -P -t -i -m 2GB microsoft/azure-cosmosdb-emulator
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
cd %LOCALAPPDATA%\CosmosDBEmulatorCert
powershell .\importcert.ps1
```

PowerShell’den:
```powershell
cd $env:LOCALAPPDATA\CosmosDBEmulatorCert
.\importcert.ps1
```

Öykünücü başlatıldıktan sonra etkileşimli kabuğu kapatmak öykünücünün kapsayıcısını kapatır.

Veri Gezgini’ni açmak için tarayıcınızda aşağıdaki URL’ye gidin. Yukarıda gösterilen yanıt iletisinde öykünücü uç noktası sağlanır.

    https://<emulator endpoint provided in response>/_explorer/index.html


## <a name="troubleshooting"></a>Sorun giderme

Azure Cosmos DB Öykünücüsü ile karşılaştığınız sorunları gidermeye yardımcı olması için aşağıdaki ipuçlarını kullanın:

- Öykünücünün yeni bir sürümünü yüklediyseniz ve hatalarla karşılaşıyorsanız, verilerinizi sıfırladığınızdan emin olun. Sistem tepsisindeki Azure Cosmos DB Öykünücüsü simgesine sağ tıklayıp Verileri Sıfırla... seçeneğine tıklayarak verilerinizi sıfırlayabilirsiniz. Bu da hataları düzeltmezse, uygulamayı kaldırıp yeniden yükleyebilirsiniz. Yönergeler için bkz. [Yerel öykünücüden kaldırma](#uninstall).

- Azure Cosmos DB Öykünücüsü kilitlenirse, C:\Users\kullanici_adi\AppData\Local\CrashDumps klasöründen döküm dosyalarını toplayın, sıkıştırın ve bir e-postaya ekleyip [askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com) adresine gönderin.

- CosmosDB.StartupEntryPoint.exe dosyasında kilitlenmelerle karşılaşırsanız bir yönetici komut isteminden şu komutu çalıştırın: `lodctr /R`

- Bir bağlantı sorunu yaşarsanız [izleme dosyalarını toplayın](#trace-files), sıkıştırın ve bir e-postaya ekleyip [askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com) adresine gönderin.

- **Hizmet Kullanılamıyor** iletisi alırsanız öykünücü, ağ yığınını başlatamıyor olabilir. Pulse secure istemcisinin veya Juniper networks istemcisinin yüklü olup olmadığını denetleyin; bunların ağ filtresi sürücüleri soruna yol açıyor olabilir. Genellikle üçüncü taraf ağ filtresi sürücüleri kaldırıldığında sorun düzeltilir.

- Öykünücü çalışırken bilgisayarınız uyku moduna geçer veya herhangi bir işletim sistemi güncelleştirmesi çalıştırırsa, bir **Service is currently unavailable** (Hizmet şu anda kullanılamıyor) iletisi alabilirsiniz. Windows bildirim tepsisinde görünen simgeye sağ tıklayarak öykünücüyü sıfırlayın ve **Reset Data** (Verileri Sıfırla) seçeneğini belirleyin.

### <a id="trace-files"></a>İzleme dosyalarını toplama

Hata ayıklama izlemelerini toplamak için bir yönetici komut isteminden aşağıdaki komutları çalıştırın:

1. `cd /d "%ProgramFiles%\Azure Cosmos DB Emulator"`
2. `CosmosDB.Emulator.exe /shutdown`. Programın kapatıldığından emin olmak için sistem tepsisine bakın; bu bir dakika sürebilir. Azure Cosmos DB Öykünücüsü kullanıcı arabiriminde **Çıkış** düğmesine de tıklayabilirsiniz.
3. `CosmosDB.Emulator.exe /starttraces`
4. `CosmosDB.Emulator.exe`
5. Sorunu yeniden oluşturun. Veri Gezgini çalışmıyorsa yalnızca hatayı yakalamak için tarayıcının birkaç saniye boyunca açılmasını beklemeniz gerekir.
5. `CosmosDB.Emulator.exe /stoptraces`
6. `%ProgramFiles%\Azure Cosmos DB Emulator` konumuna gidin ve docdbemulator_000001.etl dosyasını bulun.
7. Hata ayıklama için .etl dosyasını yeniden üretme adımlarıyla birlikte [askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com) adresine gönderin.

### <a id="uninstall"></a>Yerel öykünücüyü kaldırma

1. Sistem tepsisindeki Azure Cosmos DB Öykünücüsü simgesine sağ tıklayıp Çıkış'a tıklayarak yerel öykünücünün tüm açık örneklerinden çıkın. Tüm örneklerin çıkması bir dakika sürebilir.
2. Windows arama kutusuna **Uygulamalar ve özellikler** yazın ve **Uygulamalar ve özellikler (Sistem ayarları)** sonucuna tıklayın.
3. Uygulamalar listesinde **Azure Cosmos DB Öykünücüsü**’ne gidip bunu seçin, **Kaldır**’a tıklayın, daha sonra onaylayıp yeniden **Kaldır**’a tıklayın.
4. Uygulama kaldırıldığında `C:\Users\<user>\AppData\Local\CosmosDBEmulator` klasörüne gidip klasörü silin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide aşağıdakileri yaptınız:

> [!div class="checklist"]
> * Yerel Öykünücüyü yüklediniz
> * Docker for Windows üzerinde Öykünücüyü çalıştırdınız
> * İsteklerin kimliğini doğruladınız
> * Öykünücüde Veri Gezgini’ni kullandınız
> * SSL sertifikalarını dışarı aktardınız
> * Komut satırından Öykünücüyü çağırdınız
> * İzleme dosyalarını topladınız

Bu öğreticide ücretsiz yerel geliştirme için yerel öykünücünün nasıl kullanılacağını öğrendiniz. Artık sonraki öğreticiye devam edebilir ve öykünücü SSL sertifikalarının nasıl dışarı aktarılacağını öğrenebilirsiniz.

> [!div class="nextstepaction"]
> [Azure Cosmos DB Öykünücüsü sertifikalarını dışarı aktarma](local-emulator-export-ssl-certificates.md)
