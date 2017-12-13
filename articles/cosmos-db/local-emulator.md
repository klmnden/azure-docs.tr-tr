---
title: "Yerel olarak Azure Cosmos DB öykünücü ile geliştirme | Microsoft Docs"
description: "Azure Cosmos DB öykünücüsü kullanarak, geliştirmek ve uygulamanızı yerel olarak ücretsiz, Azure aboneliği oluşturmadan test edin."
services: cosmos-db
documentationcenter: 
keywords: "Azure Cosmos DB öykünücüsü"
author: arramac
manager: jhubbard
editor: 
ms.assetid: 90b379a6-426b-4915-9635-822f1a138656
ms.service: cosmos-db
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/18/2017
ms.author: arramac
ms.openlocfilehash: 69736670068479ce90cc346a163fe27b340cdb0a
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="use-the-azure-cosmos-db-emulator-for-local-development-and-testing"></a>Yerel geliştirme ve sınama için Azure Cosmos DB öykünücüsünü kullanma

<table>
<tr>
  <td><strong>İkili dosyalar</strong></td>
  <td>[MSI indirin](https://aka.ms/cosmosdb-emulator)</td>
</tr>
<tr>
  <td><strong>Docker</strong></td>
  <td>[Docker hub'a](https://hub.docker.com/r/microsoft/azure-documentdb-emulator/)</td>
</tr>
<tr>
  <td><strong>Docker kaynak</strong></td>
  <td>[Github](https://github.com/azure/azure-documentdb-emulator-docker)</td>
</tr>
</table>
  
Azure Cosmos DB öykünücüsü geliştirme amacıyla Azure Cosmos DB hizmet öykünen yerel bir ortam sağlar. Azure Cosmos DB öykünücüsü kullanarak, geliştirmek ve bir Azure aboneliği oluşturmak veya herhangi bir maliyet olmadan uygulamanızı yerel olarak test etmek. Uygulamanızı Azure Cosmos DB öykünücüsünde nasıl çalıştığını ile memnun kaldığınızda, bulutta bir Azure Cosmos DB hesabı kullanmaya geçiş yapabilirsiniz.

Bu makalede aşağıdaki görevleri içerir: 

> [!div class="checklist"]
> * Öykünücü yükleme
> * Kimlik doğrulama istekleri
> * Öykünücüde Veri Gezgini'ni kullanma
> * SSL sertifikaları verme
> * Öykünücü komut satırından çağırma
> * Windows için Docker öykünücü çalışan
> * İzleme dosyaları toplama
> * Sorun giderme

Burada Kirill Gavrylyuk Azure Cosmos DB öykünücü ile çalışmaya başlama gösterilmektedir aşağıdaki videoyu izleyerek çalışmaya başlamanızı öneririz. Video öykünücüsü DocumentDB öykünücüsü olarak başvuruyor ancak aracı yüklendi unutmayın video basmaya itibaren Azure Cosmos DB öykünücüsü yeniden adlandırıldı. Video içindeki tüm bilgileri için Azure Cosmos DB öykünücüsü hala doğru olur. 

> [!VIDEO https://channel9.msdn.com/Events/Connect/2016/192/player]
> 
> 

## <a name="how-the-emulator-works"></a>Öykünücü nasıl çalışır?
Azure Cosmos DB öykünücüsü yüksek doğruluk öykünmesi Azure Cosmos DB hizmet sağlar. Azure Cosmos JSON belgelerini sorgulamak için destek dahil olmak üzere sağlama DB olarak aynı işlevselliği destekler ve koleksiyonları ölçekleme ve yürütme yordamları ve Tetikleyicileri depolanır. Geliştirmek ve Azure Cosmos DB öykünücüsü kullanarak uygulamaları test ve bunları Azure'a yalnızca tek bir yapılandırma için Azure Cosmos DB bağlantı uç noktasına değişikliği yaparak genel ölçekte dağıtma.

Yüksek kaliteli yerel öykünme gerçek Azure Cosmos DB hizmetinin oluşturduğumuz olsa da, Azure Cosmos DB öykünücüsü hizmet farklı uygulamasıdır. Örneğin, Azure Cosmos DB öykünücüsü Kalıcılık ve HTTPS protokol yığını bağlantısı için yerel dosya sistemi gibi standart işletim sistemi bileşenlerini kullanır. Bu genel çoğaltma, okuma/yazma ve ince ayarlanabilir tutarlılık düzeyleri için tek basamaklı milisaniyelik gecikme süresi yok gibi Azure Cosmos DB öykünücüsü kullanılabilir Azure altyapı dayanan bazı işlevler anlamına gelir.

> [!NOTE]
> Şu anda öykünücü veri Explorer'da yalnızca SQL API koleksiyonları ve MongoDB koleksiyonları oluşturmayı destekler. Öykünücü veri Explorer'da oluşturulmasını tablolar ve grafikler şu anda desteklemiyor. 

## <a name="differences-between-the-emulator-and-the-service"></a>Öykünücü ve hizmet arasındaki farklar 
Azure Cosmos DB öykünücüsü yerel geliştirici istasyonunda çalıştıran benzetilmiş bir ortam sağladığından, bazı arasındaki işlevsel farklılıklar öykünücüsü ve bir Azure Cosmos DB hesap bulutta vardır:

* Azure Cosmos DB öykünücüsü yalnızca tek bir sabit hesap ve bilinen bir ana anahtar destekler.  Anahtarını yeniden üretme Azure Cosmos DB öykünücüsünde mümkün değildir.
* Azure Cosmos DB öykünücüsü ölçeklenebilir hizmet değil ve çok sayıda koleksiyonları desteklemez.
* Azure Cosmos DB öykünücüsü farklı benzetimini değil [Azure Cosmos DB tutarlılık düzeylerini](consistency-levels.md).
* Azure Cosmos DB öykünücüsü benzetimi yapılamadı [bölgeli çoğaltma](distribute-data-globally.md).
* Azure Cosmos DB öykünücüsü Azure Cosmos DB hizmetinde (örneğin belge boyutu sınırları, artan bölümlenmiş koleksiyonu depolama alanı) bulunan hizmet kota geçersiz kılmaları desteklemez.
* Azure Cosmos DB öykünücüsü kopyanızı Azure Cosmos DB hizmetiyle en son değişikliklerle güncel olmayabilir gibi lütfen [Azure Cosmos DB kapasite Planlayıcısı](https://www.documentdb.com/capacityplanner) doğru şekilde üretim verimlilik (RUs) gereksinimlerini tahmin etmek için uygulama.

## <a name="system-requirements"></a>Sistem gereksinimleri
Azure Cosmos DB öykünücüsü donanım ve yazılım gereksinimleri şunlardır:

* Yazılım gereksinimleri
  * Windows Server 2012 R2, Windows Server 2016 veya Windows 10
*   En düşük donanım gereksinimleri
  * 2 GB RAM
  * 10 GB kullanılabilir sabit disk alanı

## <a name="installation"></a>Yükleme
Azure Cosmos DB Öykünücüsünden yükleyip [Microsoft Download Center](https://aka.ms/cosmosdb-emulator) veya Windows için Docker öykünücü çalıştırabilirsiniz. Windows için Docker öykünücü kullanma ile ilgili yönergeler için bkz: [Docker üzerinde çalışan](#running-on-docker). 

> [!NOTE]
> Yüklemek, yapılandırmak ve Azure Cosmos DB öykünücüsü çalıştırmak için bilgisayarda yönetici ayrıcalıkları olmalıdır.

## <a name="running-on-windows"></a>Windows üzerinde çalışan

Azure Cosmos DB öykünücüsü başlatmak için Başlat düğmesine basın veya Windows tuşuna basın. Yazmaya başlayın **Azure Cosmos DB öykünücüsü**ve uygulamaları listesinden öykünücü seçin. 

![Başlat düğmesine basın veya Windows tuşuna basın, yazmaya başlayın ** Azure Cosmos DB öykünücüsü ** ve uygulamalar listesinden öykünücü seçin](./media/local-emulator/database-local-emulator-start.png)

Öykünücü çalıştırırken, Windows görev çubuğundaki bildirim alanında bir simge görürsünüz. ![Azure Cosmos DB yerel öykünücüsü görev çubuğu bildirim](./media/local-emulator/database-local-emulator-taskbar.png)

Varsayılan olarak Azure Cosmos DB öykünücüsü 8081 numaralı bağlantı noktasını dinlemeye yerel makine ("localhost") çalışır.

Varsayılan olarak Azure Cosmos DB öykünücü yüklendikten `C:\Program Files\Azure Cosmos DB Emulator` dizin. Ayrıca, başlatma ve komut satırından öykünücüsü durdurma. Bkz: [komut satırı aracını referans](#command-line) daha fazla bilgi için.

## <a name="start-data-explorer"></a>Veri Gezgini'ni başlatın

Azure Cosmos DB öykünücüsü başlattığında tarayıcınızda otomatik olarak Azure Cosmos DB Veri Gezgini'ni açar. Adres olarak görünür [https://localhost:8081/_explorer/index.html](https://localhost:8081/_explorer/index.html). Explorer'ı kapatın ve daha sonra yeniden açmak istiyor musunuz URL'nin tarayıcınızda açın veya aşağıda gösterildiği gibi Azure Cosmos DB öykücüsünden Windows Tepsi simgesi başlatın.

![Azure Cosmos DB yerel öykünücüsü Veri Gezgini Başlatıcısı](./media/local-emulator/database-local-emulator-data-explorer-launcher.png)

## <a name="checking-for-updates"></a>Güncelleştirmeleri denetleme
Veri Gezgini indirme için kullanılabilir yeni bir güncelleştirme olup olmadığını gösterir. 

> [!NOTE]
> Azure Cosmos DB öykünücüsü bir sürümünde oluşturulan veri farklı bir sürümünü kullanırken erişilebilir olması garanti edilmez. Verileriniz için uzun vadeli kalıcı olması gerekiyorsa, bu verileri Azure Cosmos DB hesabı yerine Azure Cosmos DB öykünücüsü depolamanız önerilir. 

## <a name="authenticating-requests"></a>Kimlik doğrulama istekleri
Gibi bulutta Azure Cosmos DB ile Azure Cosmos DB öykünücüsüne karşı yaptığınız her isteğin kimliğinin doğrulanması gerekir. Azure Cosmos DB öykünücüsü ana anahtar kimlik doğrulaması için tek bir sabit hesap ve bilinen bir kimlik doğrulama anahtarı destekler. Bu hesabı ve anahtarı Azure Cosmos DB öykünücü ile kullanmak için izin verilen tek kimlik bilgileridir. Bunlar:

    Account name: localhost:<port>
    Account key: C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==

> [!NOTE]
> Azure Cosmos DB öykünücüsü tarafından desteklenen ana anahtar yalnızca öykünücü ile kullanılmak üzere tasarlanmıştır. Üretim Azure Cosmos DB hesabı ve anahtarı Azure Cosmos DB öykünücü ile kullanamazsınız. 

> [!NOTE] 
> Öykünücü /Key seçeneğiyle başlattıysanız yerine oluşturulan anahtarı kullanın "C2y6yDjf5/R, ob0N8A7Cgv30VRDJIWEHLM + 4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw =="

Ayrıca, yalnızca Azure Cosmos DB hizmet olarak Azure Cosmos DB öykünücüsü yalnızca SSL aracılığıyla güvenli iletişimi destekler.

## <a name="running-on-a-local-network"></a>Bir yerel ağ üzerinde çalışıyor

Yerel bir ağda öykünücü çalıştırabilirsiniz. Ağ erişimini etkinleştirmek için /AllowNetworkAccess tasarrufunda belirtin [komut satırı](#command-line-syntax), gerektiren /Key belirttiğiniz = key_string veya/keyfile dosya_adı =. /GenKeyFile kullanabileceğiniz önceden rastgele bir anahtar ile bir dosya oluşturmak için dosya_adı =.  Geçirebilirsiniz sonra için/keyfile dosya_adı veya /Key = contents_of_file =.

İlk kez ağ erişimini etkinleştirmek için kullanıcı kapatma öykünücü gerekir ve öykünücüsü'nın veri dizini (C:\Users\user_name\AppData\Local\CosmosDBEmulator) silin.

## <a name="developing-with-the-emulator"></a>Öykünücü ile geliştirme
Masaüstünde çalışan Azure Cosmos DB öykünücüsü olduktan sonra desteklenen herhangi biri kullanabilirsiniz [Azure Cosmos DB SDK](documentdb-sdk-dotnet.md) veya [Azure Cosmos DB REST API](/rest/api/documentdb/) öykünücü ile etkileşim kurmak için. Azure Cosmos DB öykünücüsü ayrıca SQL ve MongoDB API'ları ve görünüm için koleksiyonları oluşturun ve hiçbir kod yazmadan belgeleri düzenlemesine olanak tanır yerleşik bir Veri Gezgini içerir.   

    // Connect to the Azure Cosmos DB Emulator running locally
    DocumentClient client = new DocumentClient(
        new Uri("https://localhost:8081"), 
        "C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==");

Kullanıyorsanız, [MongoDB için protokol desteği Azure Cosmos DB](mongodb-introduction.md), Lütfen bağlantı dizesi olarak şunu kullanın:

    mongodb://localhost:C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==@localhost:10255/admin?ssl=true

Var olan araçlarla kullanabilirsiniz [Azure DocumentDB Studio](https://github.com/mingaliu/DocumentDBStudio) Azure Cosmos DB öykünücüsü bağlanmak için. Ayrıca Azure Cosmos DB öykünücüsü ve Azure Cosmos DB hizmetini kullanarak arasında verileri geçirebilirsiniz [Azure Cosmos DB veri geçiş aracı](https://github.com/azure/azure-documentdb-datamigrationtool).

> [!NOTE] 
> Öykünücü /Key seçeneğiyle başlattıysanız yerine oluşturulan anahtarı kullanın "C2y6yDjf5/R, ob0N8A7Cgv30VRDJIWEHLM + 4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw =="

Varsayılan olarak Azure Cosmos DB öykünücüsü kullanarak 25 tek bölüm koleksiyonları veya 1 bölümlenmiş koleksiyonu kadar oluşturabilirsiniz. Bu değer değiştirme hakkında daha fazla bilgi için bkz: [PartitionCount değeri ayarı](#set-partitioncount).

## <a name="export-the-ssl-certificate"></a>SSL sertifikasını dışarı aktarma

.NET dilleri ve çalışma zamanı Azure Cosmos DB yerel öykünücüsü güvenli bir şekilde bağlanmak için Windows sertifika deposunu kullan. Diğer diller yönetme ve sertifikaları kullanarak kendi yöntemi vardır. Java kullanan kendi [sertifika deposu](https://docs.oracle.com/cd/E19830-01/819-4712/ablqw/index.html) Python kullanırken [yuva sarmalayıcıları](https://docs.python.org/2/library/ssl.html).

Diller ve Windows sertifika deposu ile tümleştirmek değil çalışma zamanları ile kullanmak üzere bir sertifika almak için Windows Sertifika Yöneticisi'ni kullanarak vermeniz gerekir. Başlatılsın certlm.msc çalıştırarak ya da adım adım yönergeleri izleyin [Azure Cosmos DB öykünücüsü sertifikaları verme](./local-emulator-export-ssl-certificates.md). Sertifika Yöneticisi'ni çalışmaya başladıktan sonra aşağıda gösterildiği gibi kişisel Sertifikalar'ı açmak ve bir BASE-64 kodlanmış X.509 (.cer) dosyası olarak kolay adı "DocumentDBEmulatorCertificate" Sertifika verme.

![Azure Cosmos DB yerel öykünücüsü SSL sertifikası](./media/local-emulator/database-local-emulator-ssl_certificate.png)

X.509 Sertifikası'ndaki yönergeleri izleyerek Java sertifika deposuna alınabilir [Java CA sertifikalarını depolamak için bir sertifika ekleme](https://docs.microsoft.com/azure/java-add-certificate-ca-store). Sertifikayı sertifika deposuna içeri aktarıldığında, Java ve MongoDB uygulamalar Azure Cosmos DB öykünücüsü bağlanabiliyor olur.

Öykünücü Python ve Node.js SDK'ları bağlanırken SSL doğrulama devre dışı bırakılır.

## <a id="command-line"></a>Komut satırı aracı başvurusu
Yükleme konumundan, komut satırında başlatmak ve öykünücü durdurmak, seçenekleri yapılandırın ve diğer işlemleri gerçekleştirmek için kullanabilirsiniz.

### <a name="command-line-syntax"></a>Komut satırı sözdizimi

    CosmosDB.Emulator.exe [/Shutdown] [/DataPath] [/Port] [/MongoPort] [/DirectPorts] [/Key] [/EnableRateLimiting] [/DisableRateLimiting] [/NoUI] [/NoExplorer] [/?]

Seçeneklerinin listesini görüntülemek için şunu yazın `CosmosDB.Emulator.exe /?` komut isteminde.

<table>
<tr>
  <td><strong>Seçeneği</strong></td>
  <td><strong>Açıklama</strong></td>
  <td><strong>Komutu</strong></td>
  <td><strong>Bağımsız değişkenler</strong></td>
</tr>
<tr>
  <td>[Bağımsız değişkenler]</td>
  <td>Varsayılan ayarlarla Azure Cosmos DB öykünücüsü başlatır.</td>
  <td>CosmosDB.Emulator.exe</td>
  <td></td>
</tr>
<tr>
  <td>[Help]</td>
  <td>Desteklenen komut satırı bağımsız değişkenleri listesini görüntüler.</td>
  <td>CosmosDB.Emulator.exe /?</td>
  <td></td>
</tr>
<tr>
  <td>Kapat</td>
  <td>Azure Cosmos DB öykünücüsü kapatır.</td>
  <td>CosmosDB.Emulator.exe Shutdown</td>
  <td></td>
</tr>
<tr>
  <td>DataPath</td>
  <td>Veri dosyalarını depolamak yolu belirtir. % LocalAppdata%\CosmosDBEmulator varsayılandır.</td>
  <td>CosmosDB.Emulator.exe /DataPath =&lt;datapath&gt;</td>
  <td>&lt;DataPath&gt;: erişilebilir yolu</td>
</tr>
<tr>
  <td>Bağlantı noktası</td>
  <td>Öykünücü için kullanılacak bağlantı noktası numarasını belirtir.  8081 varsayılandır.</td>
  <td>CosmosDB.Emulator.exe Port =&lt;bağlantı noktası&gt;</td>
  <td>&lt;bağlantı noktası&gt;: tek bir bağlantı noktası numarası</td>
</tr>
<tr>
  <td>MongoPort</td>
  <td>API MongoDB uyumluluk için kullanılacak bağlantı noktası numarasını belirtir. Varsayılandır 10255 değerini bulur.</td>
  <td>CosmosDB.Emulator.exe /MongoPort =&lt;mongoport&gt;</td>
  <td>&lt;mongoport&gt;: tek bir bağlantı noktası numarası</td>
</tr>
<tr>
  <td>DirectPorts</td>
  <td>Doğrudan bağlantı için kullanılacak bağlantı noktalarını belirtir. Varsayılan 10251,10252,10253,10254 olur.</td>
  <td>CosmosDB.Emulator.exe /DirectPorts:&lt;directports&gt;</td>
  <td>&lt;directports&gt;: 4 bağlantı noktalarının virgülle ayrılmış listesi</td>
</tr>
<tr>
  <td>Anahtar</td>
  <td>Öykünücü için yetkilendirme anahtar. Anahtarı bir 64 baytlık vektör 64 tabanlı kodlama olması gerekir.</td>
  <td>CosmosDB.Emulator.exe /Key:&lt;anahtarı&gt;</td>
  <td>&lt;anahtar&gt;: anahtarı bir 64 baytlık vektör 64 tabanlı kodlama olması gerekir</td>
</tr>
<tr>
  <td>EnableRateLimiting</td>
  <td>Davranış sınırlama istek hızı etkinleştirilip etkinleştirilmeyeceğini belirtir.</td>
  <td>CosmosDB.Emulator.exe /EnableRateLimiting</td>
  <td></td>
</tr>
<tr>
  <td>DisableRateLimiting</td>
  <td>Davranış sınırlama istek hızı devre dışı belirtir.</td>
  <td>CosmosDB.Emulator.exe /DisableRateLimiting</td>
  <td></td>
</tr>
<tr>
  <td>Nouı</td>
  <td>Öykünücü kullanıcı arabirimi gösterme.</td>
  <td>CosmosDB.Emulator.exe/nouı</td>
  <td></td>
</tr>
<tr>
  <td>NoExplorer</td>
  <td>Belge Gezgini başlatma sırasında gösterme.</td>
  <td>CosmosDB.Emulator.exe /NoExplorer</td>
  <td></td>
</tr>
<tr>
  <td>bölüm sayısı</td>
  <td>Bölümlenmiş koleksiyonlar en fazla sayısını belirtir. Bkz: [koleksiyonları sayısını değiştirme](#set-partitioncount) daha fazla bilgi için.</td>
  <td>CosmosDB.Emulator.exe /PartitionCount =&lt;bölüm sayısı&gt;</td>
  <td>&lt;bölüm sayısı&gt;: maksimum sayısı, izin verilen tek bölüm koleksiyonları. Varsayılan 25'tir. İzin verilen en fazla 250'dir.</td>
</tr>
<tr>
  <td>DefaultPartitionCount</td>
  <td>Bölümler bölümlendirilmiş bir koleksiyon için varsayılan sayısını belirtir.</td>
  <td>CosmosDB.Emulator.exe /DefaultPartitionCount =&lt;defaultpartitioncount&gt;</td>
  <td>&lt;defaultpartitioncount&gt; 25 varsayılandır.</td>
</tr>
<tr>
  <td>AllowNetworkAccess</td>
  <td>Bir ağ üzerinden öykünücüsü erişim sağlar. /Key geçmesi gereken =&lt;key_string&gt; veya/keyfile =&lt;dosya_adı&gt; ağ erişimini etkinleştirmek için.</td>
  <td>CosmosDB.Emulator.exe AllowNetworkAccess /Key =&lt;key_string&gt;<br><br>or<br><br>CosmosDB.Emulator.exe /AllowNetworkAccess/keyfile =&lt;dosya_adı&gt;</td>
  <td></td>
</tr>
<tr>
  <td>NoFirewall</td>
  <td>/AllowNetworkAccess kullanıldığında, güvenlik duvarı kurallarını ayarlama.</td>
  <td>CosmosDB.Emulator.exe /NoFirewall</td>
  <td></td>
</tr>
<tr>
  <td>GenKeyFile</td>
  <td>Yeni bir yetkilendirme anahtar oluşturmak ve belirtilen dosyaya kaydedin. Oluşturulan anahtarı /Key veya/keyfile seçenekleriyle kullanılabilir.</td>
  <td>CosmosDB.Emulator.exe /GenKeyFile =&lt;anahtar dosyasının yolu&gt;</td>
  <td></td>
</tr>
<tr>
  <td>Tutarlılık</td>
  <td>Hesap için varsayılan tutarlılık düzeyini ayarlayın.</td>
  <td>CosmosDB.Emulator.exe /Consistency =&lt;tutarlılık&gt;</td>
  <td>&lt;Tutarlılık&gt;: değeri şunlardan biri olmalıdır [tutarlılık düzeylerini](consistency-levels.md): oturum, güçlü, Eventual veya BoundedStaleness.  Varsayılan değer oturumdur.</td>
</tr>
<tr>
  <td>?</td>
  <td>Yardım iletisi göster.</td>
  <td></td>
  <td></td>
</tr>
</table>

## <a id="set-partitioncount"></a>Koleksiyonları sayısını değiştirme

Varsayılan olarak, en fazla 25 tek bölüm koleksiyonları veya Azure Cosmos DB öykünücüsü kullanarak 1 bölümlenmiş koleksiyon oluşturabilirsiniz. Değiştirerek **PartitionCount** değer 250 tek bölüm koleksiyonları veya 10 bölümlenmiş koleksiyonlar veya herhangi bir bileşimini 250 tek bölüm aşmayan iki kadar oluşturabilirsiniz (burada 1 bölümlenmiş koleksiyonu 25 = tek bölümlü bir koleksiyon).

Geçerli bölüm sayısı aşıldı sonra bir koleksiyon oluşturmayı denerseniz, öykünücü aşağıdaki iletiyle ServiceUnavailable bir özel durum oluşturur.

    Sorry, we are currently experiencing high demand in this region, 
    and cannot fulfill your request at this time. We work continuously 
    to bring more and more capacity online, and encourage you to try again. 
    Please do not hesitate to email docdbswat@microsoft.com at any time or 
    for any reason. ActivityId: 29da65cc-fba1-45f9-b82c-bf01d78a1f91

Azure Cosmos DB öykünücüsü koleksiyonları kullanılabilir sayısını değiştirmek için aşağıdakileri yapın:

1. Sağ tıklayarak tüm yerel Azure Cosmos DB öykünücüsü verileri Sil **Azure Cosmos DB öykünücüsü** sistem tepsisi tıklatıp, ardından simgesine **veri Sıfırla...** .
2. Bu klasörde C:\Users\user_name\AppData\Local\CosmosDBEmulator tüm öykünücüsü verileri silin.
3. Tüm açık örnekleri sağ tıklayarak çıkmak **Azure Cosmos DB öykünücüsü** sistem tepsisi tıklatıp, ardından simgesine **çıkış**. Çıkmak tüm örnekleri için bir dakika sürebilir.
4. En son sürümünü yüklemek [Azure Cosmos DB öykünücüsü](https://aka.ms/cosmosdb-emulator).
5. Bir değer ayarlanarak PartitionCount bayrağı öykünücü başlatma < 250 =. Örneğin: `C:\Program Files\Azure CosmosDB Emulator>CosmosDB.Emulator.exe /PartitionCount=100`.

## <a name="running-on-docker"></a>Docker üzerinde çalışıyor

Azure Cosmos DB öykünücüsü Windows için Docker çalıştırabilirsiniz. Öykünücü, Oracle Linux için Docker üzerinde çalışmaz.

Bulduktan sonra [Windows için Docker](https://www.docker.com/docker-windows) yüklü Windows kapsayıcıları için araç çubuğunda Docker simgesine sağ tıklayıp seçerek geçiş **geçiş Windows kapsayıcılara**.

Ardından, öykünücü görüntünüzün, sık kullanılan kabuğundan aşağıdaki komutu çalıştırarak Docker hub'dan çeker.

```     
docker pull microsoft/azure-cosmosdb-emulator 
```
Görüntü başlatmak için aşağıdaki komutları çalıştırın.

Komut satırından:
```cmd 
md %LOCALAPPDATA%\CosmosDBEmulatorCert 2>null
docker run -v %LOCALAPPDATA%\CosmosDBEmulatorCert:c:\CosmosDBEmulator\CosmosDBEmulatorCert -P -t -i -m 2GB microsoft/azure-cosmosdb-emulator 
```

Powershell'den:
```powershell
md $env:LOCALAPPDATA\CosmosDBEmulatorCert 2>null
docker run -v $env:LOCALAPPDATA\CosmosDBEmulatorCert:c:\CosmosDBEmulator\CosmosDBEmulatorCert -P -t -i -m 2GB microsoft/azure-cosmosdb-emulator 
```

Yanıt aşağıdakine benzer:

```
Starting Emulator
Emulator Endpoint: https://172.20.229.193:8081/
Master Key: C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==
Exporting SSL Certificate
You can import the SSL certificate from an administrator command prompt on the host by running:
cd /d %LOCALAPPDATA%\CosmosDBEmulatorCert
powershell .\importcert.ps1
--------------------------------------------------------------------------------------------------
Starting interactive shell
``` 

Artık uç noktası ve ana anahtarından içinde yanıt, istemci kullanın ve ana bilgisayara SSL sertifikasını içeri. SSL sertifikasını içeri aktarmak için bir yönetici komut isteminde aşağıdakileri yapın:

```
cd %LOCALAPPDATA%\CosmosDBEmulatorCert
powershell .\importcert.ps1
```

Öykünücü silindikten sonra etkileşimli Kabuk kapatma öykünücüsü'nın kapsayıcı kapatma işlemi başlatıldı.

Veri Gezgini'ni açmak için tarayıcınızı aşağıdaki URL'ye gidin. Öykünücü uç noktası, yukarıda gösterilen Yanıt iletisindeki sağlanır.

    https://<emulator endpoint provided in response>/_explorer/index.html


## <a name="troubleshooting"></a>Sorun giderme

Azure Cosmos DB öykünücü ile karşılaştığınız sorunları gidermeye yardımcı olması için aşağıdaki ipuçlarını kullanın:

- Öykünücü yeni bir sürümünü yüklediyseniz ve hataları yaşıyor verilerinizi sıfırlama emin olun. Sistem tepsisindeki Azure Cosmos DB öykünücü simgesine sağ tıklayıp sıfırlama verileri tıklatarak verilerinizi sıfırlayabilirsiniz... Hataları çözmezse kaldırın ve uygulamayı yeniden yükleyin. Bkz: [yerel öykünücüsü kaldırma](#uninstall) yönergeler için.

- Azure Cosmos DB öykünücüsü çökerse c:\Users\user_name\AppData\Local\CrashDumps klasöründen döküm dosyaları toplamak, sıkıştırmak ve e-posta ekleme [ askcosmosdb@microsoft.com ](mailto:askcosmosdb@microsoft.com).

- Kilitlenme karşılaşırsanız CosmosDB.StartupEntryPoint.exe içinde bir yönetici komut isteminden aşağıdaki komutu çalıştırın:`lodctr /R` 

- Bir bağlantı sorunu yaşarsanız [toplamak izleme dosyaları](#trace-files)sıkıştırmak ve e-posta ekleme [ askcosmosdb@microsoft.com ](mailto:askcosmosdb@microsoft.com).

- Alırsanız, bir **Hizmet kullanılamıyor** ileti öykünücü başarısız ağ yığınını başlatılamadı. Ağ filtre sürücülerini sorunu neden olabileceğinden Pulse güvenli istemci veya Juniper ağları istemcisi yüklü olup olmadığını denetleyin. Kaldırma işlemi üçüncü taraf ağ filtre sürücüleri genellikle sorunu giderir.

### <a id="trace-files"></a>İzleme dosyaları Topla

Hata ayıklama izlemeleri toplamak için bir yönetici komut isteminden aşağıdaki komutları çalıştırın:

1. `cd /d "%ProgramFiles%\Azure Cosmos DB Emulator"`
2. `CosmosDB.Emulator.exe /shutdown`. Gözcü program emin olmak için sistem tepsisi kapatıldı, bir dakika sürebilir. Aynı zamanda yalnızca tıklatabilirsiniz **çıkış** Azure Cosmos DB öykünücüsü kullanıcı arabiriminde.
3. `CosmosDB.Emulator.exe /starttraces`
4. `CosmosDB.Emulator.exe`
5. Sorunu yeniden oluşturun. Veri Gezgini çalışmıyorsa, yalnızca birkaç saniye hata catch açmak için tarayıcı beklemeniz gerekir.
5. `CosmosDB.Emulator.exe /stoptraces`
6. Gidin `%ProgramFiles%\Azure Cosmos DB Emulator` ve docdbemulator_000001.etl dosyasını bulun.
7. .Etl dosyası ile birlikte yeniden oluşturma adımları için gönderme [ askcosmosdb@microsoft.com ](mailto:askcosmosdb@microsoft.com) hata ayıklama için.

### <a id="uninstall"></a>Yerel öykünücüsü kaldırma

1. Tüm açık örnekleri yerel öykünücüsü sistem tepsisindeki Azure Cosmos DB öykünücü simgesine sağ tıklayıp sonra Çıkış'ı tıklatarak çıkın. Çıkmak tüm örnekleri için bir dakika sürebilir.
2. Windows Arama kutusuna yazın **uygulamalar ve Özellikler** ve tıklayın **uygulamalar ve Özellikler (sistem ayarlarını)** sonucu.
3. Uygulamalar listesinde kaydırın **Azure Cosmos DB öykünücüsü**, onu seçin, **kaldırma**, ardından onaylayın ve tıklatın **kaldırma** yeniden.
4. Uygulama kaldırıldığında C:\Users gidin\<kullanıcı > \AppData\Local\CosmosDBEmulator ve klasörü silin. 

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, aşağıdakileri yaptığınızdan:

> [!div class="checklist"]
> * Yerel öykünücüsü yüklü
> * Rand Docker Windows öykünücüsünde
> * Kimliği doğrulanmış istekler
> * Veri Gezgini öykünücüsünde kullanılır
> * Dışarı aktarılan SSL sertifikaları
> * Komut satırından öykünücü çağrılır
> * Toplanan izleme dosyaları

Bu öğreticide, ücretsiz yerel geliştirme için yerel öykünücüsü kullanmayı öğrendiniz. Şimdi, sonraki öğretici devam ve öykünücüsü SSL sertifikaları vermek öğrenin. 

> [!div class="nextstepaction"]
> [Azure Cosmos DB öykünücüsü sertifikaları verme](local-emulator-export-ssl-certificates.md)
