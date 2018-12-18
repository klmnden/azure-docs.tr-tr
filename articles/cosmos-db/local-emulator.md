---
title: Azure Cosmos DB öykünücüsü ile yerel olarak geliştirme
description: Azure Cosmos DB Öykünücüsü’nü kullanarak, Azure aboneliği oluşturmadan uygulamanızı ücretsiz olarak yerel ortamda geliştirip test edebilirsiniz.
services: cosmos-db
keywords: Azure Cosmos DB Öykünücüsü
author: David-Noble-at-work
ms.service: cosmos-db
ms.topic: tutorial
ms.date: 04/20/2018
ms.author: danoble
ms.openlocfilehash: 334396b99609ea52085e36ee2740583e0957c3a4
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/17/2018
ms.locfileid: "53549587"
---
# <a name="use-the-azure-cosmos-db-emulator-for-local-development-and-testing"></a>Yerel geliştirme ve test için Azure Cosmos DB Öykünücüsünü kullanma

<table>
<tr>
  <td><strong>İkililer</strong></td>
  <td>[MSI’yi İndir](https://aka.ms/cosmosdb-emulator)</td>
</tr>
<tr>
  <td><strong>Docker</strong></td>
  <td>[Docker Hub](https://hub.docker.com/r/microsoft/azure-cosmosdb-emulator/)</td>
</tr>
<tr>
  <td><strong>Docker kaynağı</strong></td>
  <td>[GitHub](https://github.com/Azure/azure-cosmos-db-emulator-docker)</td>
</tr>
</table>

Azure Cosmos DB Öykünücüsü, geliştirme amaçlı olarak Azure Cosmos DB hizmetine öykünen yerel bir ortam sağlar. Azure Cosmos DB Öykünücüsü’nü kullanarak Azure aboneliği oluşturmadan veya masraf yapmadan uygulamanızı yerel ortamda geliştirip test edebilirsiniz. Uygulamanızın Azure Cosmos DB Öykünücüsü’ndeki performansından memnun olduğunuzda bulut üzerinde Azure Cosmos DB hesabı kullanmaya başlayabilirsiniz.

Şu anda öykünücüdeki Veri Gezgini yalnızca SQL API koleksiyonlarını ve MongoDB koleksiyonlarını tam olarak destekler. Tablo, Graph ve Cassandra kapsayıcıları tam olarak desteklenmez.

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

* Şu anda öykünücüdeki Veri Gezgini yalnızca SQL API koleksiyonlarını ve MongoDB koleksiyonlarını destekler. Tablo, Graph ve Cassandra API’leri henüz desteklenmemektedir.
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
Azure Cosmos DB Öykünücüsü masaüstünüzde çalışmaya başladıktan sonra, Öykünücü ile etkileşim kurmak için desteklenen [Azure Cosmos DB SDK](sql-api-sdk-dotnet.md)'larından veya [Azure Cosmos DB REST API](/rest/api/cosmos-db/)'lerinden birini kullanabilirsiniz. Azure Cosmos DB öykünücüsü'nü de Azure Cosmos DB API SQL, MongoDB ve görünümü için koleksiyonlar oluşturma ve herhangi bir kod yazmadan belgeleri düzenlemesine olanak tanıyan yerleşik bir Veri Gezgini içerir.

    // Connect to the Azure Cosmos DB Emulator running locally
    DocumentClient client = new DocumentClient(
        new Uri("https://localhost:8081"),
        "C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==");

[MongoDB için Azure Cosmos DB protokol desteği](mongodb-introduction.md) kullanıyorsanız aşağıdaki bağlantı dizesini kullanın:

    mongodb://localhost:C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==@localhost:10255/admin?ssl=true

Azure Cosmos DB Öykünücüsü’ne bağlanmak için [Azure DocumentDB Studio](https://github.com/mingaliu/DocumentDBStudio) gibi mevcut araçları kullanabilirsiniz. [Azure Cosmos DB Veri Geçişi Aracı](https://github.com/azure/azure-documentdb-datamigrationtool)’nı kullanarak Azure Cosmos DB Öykünücüsü ile Azure Cosmos DB hizmeti arasında verileri de geçirebilirsiniz.

> [!NOTE]
> Öykünücüyü /Key seçeneğiyle başlattıysanız, "C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==" yerine oluşturulan anahtarı kullanın

Azure Cosmos DB Öykünücüsü'nü kullanırken varsayılan olarak en çok tek bölümlü 25 koleksiyon veya bölümlenmiş 1 koleksiyon oluşturabilirsiniz. Bu değeri değiştirme hakkında daha fazla bilgi için bkz. [PartitionCount değerini ayarlama](#set-partitioncount).

## <a name="export-the-ssl-certificate"></a>SSL sertifikasını dışarı aktarma

.NET dilleri ve çalışma zamanı, Azure Cosmos DB yerel öykünücüsüne güvenli şekilde bağlanmak için Windows Sertifika Deposunu kullanır. Diğer dillerin kendi sertifikaları yönetme ve kullanma yöntemi vardır. Java kendi [sertifika deposunu](https://docs.oracle.com/cd/E19830-01/819-4712/ablqw/index.html) kullanırken Python ise [yuva sarmalayıcılarını](https://docs.python.org/2/library/ssl.html) kullanır.

Windows Sertifika Deposu ile tümleştirilmeyen çalışma zamanları ve dillerle kullanmak üzere bir sertifika edinmek için Windows Sertifika Yöneticisi’ni kullanarak bunu dışarı aktarmanız gerekir. certlm.msc öğesini çalıştırarak bunu başlatabilir veya [Azure Cosmos DB Öykünücüsü Sertifikalarını dışarı aktarma](./local-emulator-export-ssl-certificates.md) bölümündeki adım adım yönergeleri izleyebilirsiniz. Sertifika yöneticisi çalıştırıldıktan sonra aşağıda gösterildiği gibi Kişisel Sertifikaları açın ve sertifikayı BASE-64 kodlu X.509 (.cer) dosyası olarak "DocumentDBEmulatorCertificate" kolay adıyla dışarı aktarın.

![Azure Cosmos DB yerel öykünücüsü SSL sertifikası](./media/local-emulator/database-local-emulator-ssl_certificate.png)

X.509 sertifikası, [Java CA Sertifika Deposuna Sertifika Ekleme](https://docs.microsoft.com/azure/java-add-certificate-ca-store) bölümündeki yönergeler izlenerek Java sertifika deposuna içeri aktarılabilir. Sertifika, sertifika deposuna içeri aktarıldıktan sonra Java ve MongoDB uygulamaları, Azure Cosmos DB Öykünücüsü’ne bağlanabilir.

Python ve Node.js SDK’larından öykünücüye bağlanırken SSL doğrulaması devre dışı bırakılır.

## <a id="command-line"></a>Komut satırı aracı başvurusu
Yükleme konumundan, komut satırını kullanarak öykünücüyü başlatıp durdurabilir, seçenekleri yapılandırabilir ve başka işlemler gerçekleştirebilirsiniz.

### <a name="command-line-syntax"></a>Komut satırı sözdizimi

    CosmosDB.Emulator.exe [/Shutdown] [/DataPath] [/Port] [/MongoPort] [/DirectPorts] [/Key] [/EnableRateLimiting] [/DisableRateLimiting] [/NoUI] [/NoExplorer] [/?]

Seçenek listesini görüntülemek için komut satırına `CosmosDB.Emulator.exe /?` yazın.

<table>
<tr>
  <td><strong>Seçenek</strong></td>
  <td><strong>Açıklama</strong></td>
  <td><strong>Komut</strong></td>
  <td><strong>Bağımsız Değişkenler</strong></td>
</tr>
<tr>
  <td>[Bağımsız değişken yok]</td>
  <td>Varsayılan ayarlarla Azure Cosmos DB Öykünücüsünü başlatır.</td>
  <td>CosmosDB.Emulator.exe</td>
  <td></td>
</tr>
<tr>
  <td>[Yardım]</td>
  <td>Desteklenen komut satırı bağımsız değişkenleri listesini görüntüler.</td>
  <td>CosmosDB.Emulator.exe /?</td>
  <td></td>
</tr>
<tr>
  <td>GetStatus</td>
  <td>Azure Cosmos DB Öykünücüsü’nün durumunu alır. Durum, çıkış kodu tarafından belirtilir: 1 = başlangıç, 2 çalışan, 3 = = durduruldu. Negatif çıkış kodu, bir hata oluştuğunu gösterir. Başka bir çıktı üretilmez.</td>
  <td>CosmosDB.Emulator.exe /GetStatus</td>
  <td></td>
<tr>
  <td>Kapat</td>
  <td>Azure Cosmos DB Öykünücüsü’nü kapatır.</td>
  <td>CosmosDB.Emulator.exe /Shutdown</td>
  <td></td>
</tr>
<tr>
  <td>DataPath</td>
  <td>Veri dosyalarının depolanacağı yolu belirtir. Varsayılan: %LocalAppdata%\CosmosDBEmulator.</td>
  <td>CosmosDB.Emulator.exe /DataPath=&lt;datapath&gt;</td>
  <td>&lt;DataPath&gt;: Erişilebilir bir yol</td>
</tr>
<tr>
  <td>Bağlantı noktası</td>
  <td>Öykünücü için kullanılacak bağlantı noktası numarasını belirtir. Varsayılan: 8081.</td>
  <td>CosmosDB.Emulator.exe /Port=&lt;port&gt;</td>
  <td>&lt;Bağlantı noktası&gt;: Tek bir bağlantı noktası numarası</td>
</tr>
<tr>
  <td>MongoPort</td>
  <td>MongoDB uyumluluk API’si için kullanılacak bağlantı noktası numarasını belirtir. Varsayılan: 10255.</td>
  <td>CosmosDB.Emulator.exe /MongoPort=&lt;mongoport&gt;</td>
  <td>&lt;mongoport&gt;: Tek bir bağlantı noktası numarası</td>
</tr>
<tr>
  <td>DirectPorts</td>
  <td>Doğrudan bağlantı için kullanılacak bağlantı noktalarını belirtir. Varsayılan değerler: 10251,10252,10253,10254.</td>
  <td>CosmosDB.Emulator.exe /DirectPorts:&lt;directports&gt;</td>
  <td>&lt;directports&gt;: 4 bağlantı noktalarının virgülle ayrılmış listesi</td>
</tr>
<tr>
  <td>Anahtar</td>
  <td>Öykünücü için yetkilendirme anahtarı. Anahtar, 64 bayt vektörün base 64 kodlaması olmalıdır.</td>
  <td>CosmosDB.Emulator.exe /Key:&lt;key&gt;</td>
  <td>&lt;Anahtar&gt;: Anahtarı bir 64-bayt vektörü base-64 kodlama olmalıdır</td>
</tr>
<tr>
  <td>EnableRateLimiting</td>
  <td>İstek oranını sınırlama davranışının etkinleştirildiğini belirtir.</td>
  <td>CosmosDB.Emulator.exe /EnableRateLimiting</td>
  <td></td>
</tr>
<tr>
  <td>DisableRateLimiting</td>
  <td>İstek oranını sınırlama davranışının devre dışı bırakıldığını belirtir.</td>
  <td>CosmosDB.Emulator.exe /DisableRateLimiting</td>
  <td></td>
</tr>
<tr>
  <td>NoUI</td>
  <td>Öykünücü kullanıcı arabirimini gösterme.</td>
  <td>CosmosDB.Emulator.exe /NoUI</td>
  <td></td>
</tr>
<tr>
  <td>NoExplorer</td>
  <td>Başlangıçta veri gezginini gösterme.</td>
  <td>CosmosDB.Emulator.exe /NoExplorer</td>
  <td></td>
</tr>
<tr>
  <td>PartitionCount</td>
  <td>Maksimum bölümlenmiş koleksiyon sayısını belirtir. Daha fazla bilgi için bkz. [Koleksiyon sayısını değiştirme](#set-partitioncount).</td>
  <td>CosmosDB.Emulator.exe /PartitionCount=&lt;partitioncount&gt;</td>
  <td>&lt;bölüm sayısı&gt;: İzin verilen tek bölüm koleksiyonlarını maksimum sayısı. Varsayılan: 25. Maksimum izin verilen: 250.</td>
</tr>
<tr>
  <td>DefaultPartitionCount</td>
  <td>Bölümlenen bir koleksiyon için varsayılan bölüm sayısını belirtir.</td>
  <td>CosmosDB.Emulator.exe /DefaultPartitionCount=&lt;defaultpartitioncount&gt;</td>
  <td>&lt;defaultpartitioncount&gt; Varsayılan: 25.</td>
</tr>
<tr>
  <td>AllowNetworkAccess</td>
  <td>Bir ağ üzerinden öykünücüye erişilmesini sağlar. Ağ erişimini etkinleştirmek için /Key=&lt;key_string&gt; veya /KeyFile=&lt;file_name&gt; öğesini de geçirmeniz gerekir.</td>
  <td>CosmosDB.Emulator.exe /AllowNetworkAccess /Key=&lt;key_string&gt;<br><br>or<br><br>CosmosDB.Emulator.exe /AllowNetworkAccess /KeyFile=&lt;file_name&gt;</td>
  <td></td>
</tr>
<tr>
  <td>NoFirewall</td>
  <td>/AllowNetworkAccess kullanıldığında güvenlik duvarı kurallarını ayarlama.</td>
  <td>CosmosDB.Emulator.exe /NoFirewall</td>
  <td></td>
</tr>
<tr>
  <td>GenKeyFile</td>
  <td>Yeni bir yetkilendirme anahtarı oluşturun ve belirtilen dosyaya kaydedin. Oluşturulan anahtar, /Key veya /KeyFile seçenekleri ile kullanılabilir.</td>
  <td>CosmosDB.Emulator.exe /GenKeyFile =&lt;anahtar dosyasının yolu&gt;</td>
  <td></td>
</tr>
<tr>
  <td>Tutarlılık</td>
  <td>Hesap için varsayılan tutarlılık düzeyini ayarlayın.</td>
  <td>CosmosDB.Emulator.exe /Consistency=&lt;consistency&gt;</td>
  <td>&lt;Tutarlılık&gt;: Değer şunlardan biri olmalıdır [tutarlılık düzeyleri](consistency-levels.md): Oturum, güçlü ve nihai, veya BoundedStaleness. Varsayılan değer: Oturum.</td>
</tr>
<tr>
  <td>?</td>
  <td>Yardım iletisini gösterin.</td>
  <td></td>
  <td></td>
</tr>
</table>

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
