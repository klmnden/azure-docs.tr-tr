---
title: Geliştirme ve test için Azure depolama öykünücüsü kullanma | Microsoft Docs
description: Azure depolama öykünücüsü, geliştirme ve test, Azure depolama uygulamaları için ücretsiz yerel geliştirme ortamı sağlar. İstekleri nasıl yetkilendirildiği, uygulamanızı öykünücüde bağlanma ve komut satırı aracının nasıl kullanılacağını öğrenin.
services: storage
author: mhopkins-msft
ms.service: storage
ms.topic: article
ms.date: 08/10/2018
ms.author: mhopkins
ms.reviewer: seguler
ms.subservice: common
ms.openlocfilehash: 5f55228c80142b2a21af585cb04d16f148460af0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65149091"
---
# <a name="use-the-azure-storage-emulator-for-development-and-testing"></a>Geliştirme ve test için Azure depolama öykünücüsü kullanma

Microsoft Azure storage öykünücüsü geliştirme amacıyla Azure Blob, kuyruk ve Tablo Hizmetleri öykünen yerel bir ortam sağlar. Depolama öykünücüsü kullanarak bir Azure aboneliği oluşturmadan veya masraf yapmadan uygulamanızı yerel olarak depolama hizmetleri karşı sınayabilirsiniz. Uygulamanızı öykünücüde nasıl çalıştığı ile memnun kaldığınızda, bulutta bir Azure depolama hesabı kullanmaya başlayabilirsiniz.

## <a name="get-the-storage-emulator"></a>Depolama öykünücüsü Al
Depolama öykünücüsü olarak kullanılabilir parçası [Microsoft Azure SDK'sı](https://azure.microsoft.com/downloads/). Depolama öykünücüsü kullanarak da yükleyebilirsiniz [tek başına yükleyici](https://go.microsoft.com/fwlink/?linkid=717179&clcid=0x409) (doğrudan indirme). Depolama öykünücüsü'nü yüklemek için bilgisayarınızda yönetici ayrıcalıkları olmalıdır.

Depolama öykünücüsü şu anda yalnızca Windows üzerinde çalışır. Bunlar için Linux için bir depolama öykünücüsü dikkate alarak, bir seçenek saklanır, açık kaynak depolama öykünücüsü topluluktur [Azurite](https://github.com/azure/azurite).

> [!NOTE]
> Depolama öykünücüsü bir sürümünde oluşturulan veriler farklı bir sürümünü kullanırken erişilebilir olması garanti edilmez. Uzun süreli verilerinizin kalıcı hale getirilmesi gerekiyorsa, bu verileri Azure depolama hesabınız yerine depolama öykünücüsü'nü depolamak önerilir.
> 
> Depolama öykünücüsü OData kitaplıkları belirli sürümlerinde bağlıdır. Diğer sürümleriyle birlikte depolama öykünücüsü tarafından kullanılan OData DLL'leri değiştirilmesi desteklenmez ve beklenmeyen davranışlara neden olabilir. Ancak, herhangi bir depolama hizmeti tarafından desteklenen OData sürümü öykünücüye istekleri göndermek için kullanılabilir.

## <a name="how-the-storage-emulator-works"></a>Depolama öykünücüsü nasıl çalışır?
Depolama öykünücüsü, Azure depolama hizmetleri benzetmek için yerel dosya sistemi ve yerel bir Microsoft SQL Server örneği'ni kullanır. Varsayılan olarak, depolama öykünücüsü, Microsoft SQL Server 2012 Express LocalDB içinde bir veritabanı kullanır. Depolama öykünücüsü LocalDB örneğini yerine yerel bir SQL Server örneğini erişmek için yapılandırmayı seçebilirsiniz. Daha fazla bilgi için [başlangıç ve depolama öykünücüsü başlatma](#start-and-initialize-the-storage-emulator) bu makalenin devamındaki bölümüne.

Depolama öykünücüsü, SQL Server veya Windows kimlik doğrulaması kullanarak LocalDB bağlanır.

Depolama öykünücüsü ile Azure depolama hizmetleri arasında bazı farklılıklar mevcut. Bu farklılıklar hakkında daha fazla bilgi için bkz. [depolama öykünücüsü ve Azure depolama arasındaki farklar](#differences-between-the-storage-emulator-and-azure-storage) bu makalenin devamındaki bölümüne.

## <a name="start-and-initialize-the-storage-emulator"></a>Başlat ve depolama öykünücüsü başlatma

Azure depolama öykünücüsü'nü başlatmak için:
1. Seçin **Başlat** düğme veya basın **Windows** anahtarı.
2. Yazmaya başlayın `Azure Storage Emulator`.
3. Öykünücü görüntülenen uygulamalar listesinden seçin.

Depolama öykünücüsü başlatıldığında, bir komut istemi penceresi görüntülenir. Bu konsol penceresi başlatın ve depolama öykünücüsü'nü durdurun, verileri temizlemek, durumunu Al ve öykünücü başlatmak için kullanabilirsiniz. Daha fazla bilgi için [depolama öykünücüsü komut satırı aracını referans](#storage-emulator-command-line-tool-reference) bu makalenin devamındaki bölümü.

Öykünücü çalıştırıldığında, Windows görev çubuğu bildirim alanında bir simge görürsünüz.

Depolama öykünücüsü komut istemi penceresini kapattığınızda, depolama öykünücüsü çalışmaya devam eder. Depolama öykünücüsü konsol penceresi yeniden getirmek için depolama öykünücüsünü başlatma gibi yukarıdaki adımları izleyin.

Depolama öykünücüsü, çalıştırdığınız ilk kez yerel depolama ortamı için başlatılır. Başlatma işlemi LocalDB içinde bir veritabanı oluşturur ve her bir yerel depolama hizmeti için HTTP bağlantı noktası ayırır.

Varsayılan olarak yüklü depolama öykünücüsü `C:\Program Files (x86)\Microsoft SDKs\Azure\Storage Emulator`.

> [!TIP]
> Kullanabileceğiniz [Microsoft Azure Depolama Gezgini](https://storageexplorer.com) yerel depolama öykünücüsü kaynak ile çalışmak için. Yüklü ve depolama öykünücüsü başlatıldı sonra "(Geliştirme)" için "Depolama hesapları altında" Depolama Gezgini'ni kaynakları ağacında arayın.
>

### <a name="initialize-the-storage-emulator-to-use-a-different-sql-database"></a>Farklı bir SQL veritabanını kullanmak üzere depolama öykünücüsünü başlatma

Depolama öykünücüsü varsayılan LocalDB örnekten başka bir SQL veritabanı örneğine işaret edecek şekilde başlatmak için depolama öykünücüsü komut satırı aracını kullanabilirsiniz:

1. Bölümünde anlatıldığı gibi depolama öykünücüsü konsol penceresi açıyor [başlangıç ve depolama öykünücüsü başlatma](#start-and-initialize-the-storage-emulator) bölümü.
1. Konsol penceresinde aşağıdaki komutu yazın. burada `<SQLServerInstance>` SQL Server örneğinin adıdır. Localdb'yi kullanmak üzere belirtin `(localdb)\MSSQLLocalDb` SQL Server örneği olarak.

   `AzureStorageEmulator.exe init /server <SQLServerInstance>`

   Varsayılan SQL Server örneğini kullanacak şekilde öykünücü yönlendirir aşağıdaki komutu kullanabilirsiniz:

   `AzureStorageEmulator.exe init /server .`

   Veya, varsayılan LocalDB örneğini veritabanına yeniden başlatır aşağıdaki komutu kullanabilirsiniz:

   `AzureStorageEmulator.exe init /forceCreate`

Bu komutlar hakkında daha fazla bilgi için bkz. [depolama öykünücüsü komut satırı aracını referans](#storage-emulator-command-line-tool-reference).

> [!TIP]
> Kullanabileceğiniz [Microsoft SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms) LocalDB yükleme dahil olmak üzere, SQL Server örneklerini yönetmek için (SSMS). SMSS içinde **sunucuya Bağlan** iletişim kutusunda belirtin `(localdb)\MSSQLLocalDb` içinde **sunucu adı:** alanı LocalDB örneğine bağlanın.

## <a name="authenticating-requests-against-the-storage-emulator"></a>Depolama öykünücüsü karşı kimlik doğrulama istekleri
Yüklü ve depolama öykünücüsü başlatıldı sonra kodunuzu test edebilirsiniz. Bir anonim istek olmadığı sürece bulutta Azure depolama gibi storage öykünücüsüne karşı yaptığınız her istek, yetkili olması gerekir. Paylaşılan anahtar kimlik doğrulaması kullanarak depolama öykünücüsü karşı veya paylaşılan erişim imzası (SAS) ile isteklerini yetki verebilir.

### <a name="authorize-with-shared-key-credentials"></a>Paylaşılan anahtar kimlik bilgileriyle yetkilendirme
[!INCLUDE [storage-emulator-connection-string-include](../../../includes/storage-emulator-connection-string-include.md)]

Bağlantı dizeleri hakkında daha fazla bilgi için bkz. [yapılandırma Azure Storage bağlantı dizelerini](../storage-configure-connection-string.md).

### <a name="authorize-with-a-shared-access-signature"></a>Paylaşılan erişim imzası ile yetkilendirme

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Xamarin kitaplığı gibi bazı Azure depolama istemci kitaplıkları, yalnızca paylaşılan erişim imzası (SAS) belirteci ile kimlik doğrulamasını destekler. Bir aracı gibi kullanarak SAS belirteci oluşturabilirsiniz [Depolama Gezgini](https://storageexplorer.com/) veya paylaşılan anahtar kimlik doğrulamasını destekleyen başka bir uygulama.

Ayrıca, Azure PowerShell kullanarak bir SAS belirteci oluşturabilirsiniz. Aşağıdaki örnek, bir blob kapsayıcısı için tam izinlere sahip bir SAS belirteci oluşturur:

1. Henüz yapmadıysanız yükleme Azure PowerShell (Azure PowerShell cmdlet'leri önerilen en son sürümünü kullanarak). Yükleme yönergeleri için bkz. [yüklemek ve Azure PowerShell yapılandırma](/powershell/azure/install-Az-ps).
2. Azure PowerShell'i açın ve değiştirerek aşağıdaki komutları çalıştırın `CONTAINER_NAME` seçtiğiniz bir ada sahip:

```powershell
$context = New-AzStorageContext -Local

New-AzStorageContainer CONTAINER_NAME -Permission Off -Context $context

$now = Get-Date

New-AzStorageContainerSASToken -Name CONTAINER_NAME -Permission rwdl -ExpiryTime $now.AddDays(1.0) -Context $context -FullUri
```

Yeni bir kapsayıcı elde edilen paylaşılan erişim imzası URI'si aşağıdakine benzer olmalıdır:

```
http://127.0.0.1:10000/devstoreaccount1/sascontainer?sv=2012-02-12&se=2015-07-08T00%3A12%3A08Z&sr=c&sp=wl&sig=t%2BbzU9%2B7ry4okULN9S0wst%2F8MCUhTjrHyV9rDNLSe8g%3Dsss
```

Bu örnek ile oluşturulmuş paylaşılan erişim imzası bir gün boyunca geçerlidir. İmza kapsayıcı içindeki blobları tam (okuma, yazma, silme, liste) erişim verir.

Paylaşılan erişim imzaları ile ilgili daha fazla bilgi için bkz. [kullanarak paylaşılan erişim imzaları (SAS) Azure Depolama'daki](../storage-dotnet-shared-access-signature-part-1.md).

## <a name="addressing-resources-in-the-storage-emulator"></a>Depolama öykünücüsü kaynaklarını adreslemek
Depolama öykünücüsü için hizmet uç noktaları Azure depolama hesabınız olanlardan farklıdır. Fark, yerel bilgisayarın etki alanı adı çözümlemesi, depolama öykünücüsü uç noktaları yerel adresler olmasını gerektiren gerçekleştirmez olmasıdır.

Azure depolama hesabınız kaynak adresi için aşağıdaki düzenini kullanın. Hesap adının URI ana bilgisayar adının bir parçasıdır ve çalışılmaktadır Kaynak URI yolu bir parçasıdır:

`<http|https>://<account-name>.<service-name>.core.windows.net/<resource-path>`

Örneğin, aşağıdaki URI, bir Azure depolama hesabındaki bir blob için geçerli bir adresi şöyledir:

`https://myaccount.blob.core.windows.net/mycontainer/myblob.txt`

Ancak, depolama öykünücüsü, yerel bilgisayarın etki alanı adı çözümlemesi, gerçekleştirmez hesap adı çünkü URI yolu ana bilgisayar adı yerine bir parçası. Depolama öykünücüsü bir kaynak için aşağıdaki URI biçimi kullanın:

`http://<local-machine-address>:<port>/<account-name>/<resource-path>`

Örneğin, bir blob depolama öykünücüsünde erişmek için şu adresi kullanılabilir:

`http://127.0.0.1:10000/myaccount/mycontainer/myblob.txt`

Depolama öykünücüsü için hizmet uç noktaları şunlardır:

* BLOB hizmeti: `http://127.0.0.1:10000/<account-name>/<resource-path>`
* Kuyruk hizmeti: `http://127.0.0.1:10001/<account-name>/<resource-path>`
* Tablo hizmeti: `http://127.0.0.1:10002/<account-name>/<resource-path>`

### <a name="addressing-the-account-secondary-with-ra-grs"></a>İkincil RA-GRS hesabı adresleme
Depolama öykünücüsü 3.1 sürümünden başlayarak, okuma erişimli coğrafi olarak yedekli çoğaltma (RA-GRS) destekler. İçin depolama kaynaklarını hem bulutta hem de yerel öykünücüsü, ikincil konumdaki erişebileceğiniz göre ekleme - hesap adının ikincil. Örneğin, bir blob depolama öykünücüsü'nde salt okunur ikincil kullanarak erişmek için şu adresi kullanılabilir:

`http://127.0.0.1:10000/myaccount-secondary/mycontainer/myblob.txt`

> [!NOTE]
> İkincil depolama öykünücüsü ile programlı erişim için sürüm 3.2 veya sonraki sürümlerinde .NET için depolama istemcisi kitaplığını kullanırsınız. Bkz: [.NET için Microsoft Azure depolama istemci Kitaplığı](https://msdn.microsoft.com/library/azure/dn261237.aspx) Ayrıntılar için.
>
>

## <a name="storage-emulator-command-line-tool-reference"></a>Depolama öykünücüsü komut satırı araçları başvurusu
Depolama öykünücüsü başlattığınızda sürüm 3.0 başlayarak, bir konsol penceresi görüntülenir. Komut satırında başlatmak ve öykünücü yanı sıra durumu için sorgu durdurun ve diğer işlemleri gerçekleştirmek için konsol penceresinde kullanın.

> [!NOTE]
> Microsoft Azure işlem öykünücüsünü varsa, depolama öykünücüsü başlattığınızda sistem tepsisi simgesi görünür. Depolama öykünücüsü'nü durdurmak ve başlatmak için grafik bir yol sağlayan bir menüyü görüntülemek için simgeye sağ tıklayın.
>
>

### <a name="command-line-syntax"></a>Komut satırı sözdizimi
`AzureStorageEmulator.exe [start] [stop] [status] [clear] [init] [help]`

### <a name="options"></a>Seçenekler
Seçenek listesini görüntülemek için komut satırına `/help` yazın.

| Seçenek | Açıklama | Komut | Bağımsız Değişkenler |
| --- | --- | --- | --- |
| **Start** |Depolama öykünücüsü'kurmak başlatır. |`AzureStorageEmulator.exe start [-inprocess]` |*-InProcess*: Yeni bir işlem oluşturmak yerine geçerli işlemdeki öykünücüyü başlatın. |
| **Durdur** |Depolama öykünücüsü durdurur. |`AzureStorageEmulator.exe stop` | |
| **Durumu** |Depolama öykünücüsü durumunu yazdırır. |`AzureStorageEmulator.exe status` | |
| **Temizle** |Komut satırında belirtilen tüm hizmetleri verileri temizler. |`AzureStorageEmulator.exe clear [blob] [table] [queue] [all]` |*BLOB*: Blob verileri temizler. <br/>*Kuyruk*: Kuyruk verileri temizler. <br/>*Tablo*: Temizler, veri tablosu. <br/>*Tüm*: Tüm hizmetlerdeki tüm verileri temizler. |
| **Init** |Öykünücünün kurulumunu için tek seferlik başlatma gerçekleştirir. |<code>AzureStorageEmulator.exe init [-server serverName] [-sqlinstance instanceName] [-forcecreate&#124;-skipcreate] [-reserveports&#124;-unreserveports] [-inprocess]</code> |*-server Sunucuadı\örnekadı*: SQL örneğini barındıran sunucuyu belirtir. <br/>*-sqlınstance InstanceName*: Varsayılan sunucu örneğinde kullanılacak SQL örneğinin adını belirtir. <br/>*-forcecreate*: Zaten mevcut olsa bile, SQL veritabanı oluşturulmasını zorlar. <br/>*-skipcreate*: SQL veritabanı oluşturma atlanıyor. Bu,-forcecreate öncelik kazanır.<br/>*-reserveports*: Hizmetleri ile ilişkili HTTP bağlantı noktalarını ayırma dener.<br/>*-unreserveports*: Hizmetleri ile ilişkili HTTP bağlantı noktaları için ayırmaları kaldırmayı dener. Bu,-reserveports öncelik kazanır.<br/>*-InProcess*: Yeni bir işlem UNICODE yerine geçerli işlemdeki başlatma gerçekleştirir. Geçerli işlem, bağlantı noktası ayırmaları değişiyorsa yükseltilmiş izinlerle başlatılmalıdır. |

## <a name="differences-between-the-storage-emulator-and-azure-storage"></a>Depolama öykünücüsü ve Azure depolama arasındaki farklar
Depolama öykünücüsü yerel bir SQL örneğinde çalışan benzetilmiş bir ortam olduğundan, işlevsel farklılıklar öykünücü ve Azure depolama hesabı arasında bulutta vardır:

* Depolama öykünücüsü yalnızca tek bir sabit hesap ve iyi bilinen bir kimlik doğrulama anahtarı'nı destekler.
* Depolama öykünücüsü ölçeklenebilir depolama hizmeti ve çok sayıda eşzamanlı istemciler desteklemez.
* Bölümünde anlatıldığı gibi [depolama öykünücüsü kaynaklarını adreslemek](#addressing-resources-in-the-storage-emulator), kaynakları ele farklı bir Azure depolama hesabı ve depolama öykünücüsü. Etki alanı adı çözümlemesi kullanılabilir olmadığından bu farktır bulutta ancak yerel bilgisayarda değil.
* 3\.1 sürümünden başlayarak depolama öykünücüsü hesabı okuma erişimli coğrafi olarak yedekli çoğaltma (RA-GRS) destekler. Öykünücü RA-GRS etkin tüm hesapları sahip ve birincil ve ikincil çoğaltmalar arasında hiçbir zaman herhangi bir gecikme yoktur. Blob hizmeti istatistikleri alın, sıra hizmet istatistikleri alın ve tablo hizmet istatistikleri alma işlemleri ikincil hesabı desteklenir ve her zaman değerini döndürür `LastSyncTime` temel alınan SQL veritabanına göre geçerli saat olarak yanıt öğesi.
* Şu anda SMB protokolü hizmet uç noktaları ve dosya hizmeti depolama öykünücüsünde desteklenmez.
* Depolama Hizmetleri öykünücüsü tarafından henüz desteklenmeyen bir sürümünü kullanıyorsanız, depolama öykünücüsü VersionNotSupportedByEmulator hatası (HTTP durum kodu 400 - bozuk istek) döndürür.

### <a name="differences-for-blob-storage"></a>Blob Depolama için farkları
Blob Depolama öykünücüsünde kıyasla aşağıdaki farklılıklar geçerlidir:

* Depolama öykünücüsü yalnızca destekler blob 2 GB'ye kadar boyutları.
* Azure Depolama'daki bir blob adı uzunluğu en fazla 1024 karakter olduğu sürece depolama öykünücüsünde bir blob adı uzunluğu en fazla 256 karakter var.
* Hizmette hata döndürmesi artımlı kopya, kopyalanacak üzerine BLOB anlık görüntüleri sağlar.
* Fark alma sayfası aralıkları artımlı kopya blob'u kullanarak kopyalanan anlık görüntüler arasında çalışmaz.
* Kira kimliği istekte belirtilmemiş olsa bile bir Blob Put işlemi, depolama öykünücüsü ile etkin bir kiralama var olan bir bloba karşı başarılı olabilir.
* Ekleme blobu işlemleri öykünücüsü tarafından desteklenmez. Ek blob üzerinde bir işlem deneniyor FeatureNotSupportedByEmulator hatası (HTTP durum kodu 400 - bozuk istek) döndürür.

### <a name="differences-for-table-storage"></a>Tablo depolama için farkları
Tablo depolama öykünücüsünde kıyasla aşağıdaki farklılıklar geçerlidir:

* Depolama öykünücüsü tablo hizmetinde tarih özellikleri, yalnızca SQL Server 2005'in (bunlar 1 Ocak 1753 üzeri olması gerekir) tarafından desteklenen aralıkta destekler. Tüm tarihler 1 Ocak 1753 önce bu değere dönüştürülür. Tarih duyarlılığı tarihler 1 kesin olduğu anlamına gelir, SQL Server 2005 duyarlığını sınırlıdır/saniyenin 300th.
* Depolama öykünücüsü, 512 bayttan az her bölüm anahtarını ve satır anahtarı özellik değerlerini destekler. Ayrıca, hesap adı, tablo adını ve anahtar özelliği adlarının birlikte toplam boyutu 900 baytı aşamaz.
* Depolama öykünücüsü tablosundaki bir satır toplam boyutu 1 MB'tan küçük bir süre için sınırlıdır.
* Depolama öykünücüsü'nde türü özellikleri veri `Edm.Guid` veya `Edm.Binary` destekleyebilir `Equal (eq)` ve `NotEqual (ne)` sorgu Karşılaştırma işleçleri filtre dizeleri.

### <a name="differences-for-queue-storage"></a>Kuyruk depolama için farkları
Kuyruk depolama öykünücüsünde özgü hiçbir fark yoktur.

## <a name="storage-emulator-release-notes"></a>Depolama öykünücüsü sürüm notları

### <a name="version-57"></a>Sürüm 5.7
Günlüğe kaydetme etkinleştirilmişse, kilitlenmeye neden bir hata düzeltildi.

### <a name="version-56"></a>5\.6 sürümü
* Depolama öykünücüsü artık sürümü 2018-03-28 depolama hizmeti Blob, kuyruk ve tablo Hizmeti uç noktaları üzerinde destekler.

### <a name="version-55"></a>5\.5 sürümü
* Depolama öykünücüsü artık sürüm 2017-11-09 depolama hizmeti Blob, kuyruk ve tablo Hizmeti uç noktaları üzerinde destekler.
* Blob için destek eklenmiştir **oluşturulan** blobun oluşturulma zamanı döndüren özellik.

### <a name="version-54"></a>5\.4 sürümü
Yükleme kararlılığını geliştirmek üzere öykünücüyü artık yükleme sırasında bağlantı noktalarını ayırma dener. Bağlantı noktası ayırmaları isterseniz kullanın *- reserveports* seçeneği **init** bunları belirtmek için komutu.

### <a name="version-53"></a>Sürüm 5.3
Depolama öykünücüsü sürüm 2017-07-29 depolama hizmeti Blob, kuyruk ve tablo Hizmeti uç noktaları üzerinde artık desteklemektedir.

### <a name="version-52"></a>5\.2 sürümü
* Depolama öykünücüsü artık sürüm 2017-04-17 depolama hizmeti Blob, kuyruk ve tablo Hizmeti uç noktaları üzerinde destekler.
* Burada tablo özellik değerlerini düzgün şekilde kodlanmamış hata düzeltildi.

### <a name="version-51"></a>Sürüm 5.1
Depolama öykünücüsü burada döndürdü düzeltildi `DataServiceVersion` hizmet nerede oluştuğunu olmayan bazı yanıt üst bilgisi.

### <a name="version-50"></a>Sürüm 5.0
* Depolama öykünücüsü yükleyici artık için mevcut MSSQL denetler ve .NET Framework yükler.
* Depolama öykünücüsü yükleyici artık, yükleme bir parçası olarak veritabanı oluşturur. Veritabanı başlangıç bir parçası olarak gerekirse yine de oluşturulacak.
* Veritabanı oluşturma artık yükseltme gerektirir.
* Bağlantı noktası ayırmaları artık başlatma için gereklidir.
* Aşağıdaki seçeneklere ekler `init`: `-reserveports` (yükseltme gerekir), `-unreserveports` (yükseltme gerekir), `-skipcreate`.
* Sistem tepsisi simgesi depolama öykünücüsü kullanıcı Arabirimi seçeneği artık komut satırı arabirimi başlatır. Eski GUI artık kullanılamıyor.
* Bazı DLL'lerin kaldırılmış veya yeniden adlandırılamaz.

### <a name="version-46"></a>4\.6 sürümü
* Depolama öykünücüsü artık sürüm 2016-05-31 depolama hizmeti Blob, kuyruk ve tablo Hizmeti uç noktaları üzerinde destekler.

### <a name="version-45"></a>Sürüm 4.5
* Başlatma ve yükleme depolama öykünücüsünü yedekleme veritabanı adlandırıldığında başarısız olmasına neden olan hata düzeltildi.

### <a name="version-44"></a>Sürüm 4.4
* Depolama öykünücüsü artık sürümü 2015-12-11 depolama hizmeti Blob, kuyruk ve tablo Hizmeti uç noktaları üzerinde destekler.
* Depolama öykünücüsü'nın çöp toplama, blob verilerini artık çok sayıda BLOB ile ilgilenirken daha verimli olur.
* Kapsayıcı ACL biraz daha farklı depolama hizmeti bunu nasıl yaptığını doğrulanacak XML neden olan hata düzeltildi.
* Bazen maksimum ve minimum bildirilmesini DateTime değerleri yanlış saat diliminde neden olan hata düzeltildi.

### <a name="version-43"></a>Sürümü 4.3
* Depolama öykünücüsü artık sürümü 2015-07-08 depolama hizmeti Blob, kuyruk ve tablo Hizmeti uç noktaları üzerinde destekler.

### <a name="version-42"></a>4\.2 sürümü
* Depolama öykünücüsü artık sürümü 2015-04-05 depolama hizmeti Blob, kuyruk ve tablo Hizmeti uç noktaları üzerinde destekler.

### <a name="version-41"></a>Sürüm 4.1
* Depolama öykünücüsü artık sürümü 2015-02-21 depolama hizmeti Blob, kuyruk ve tablo Hizmeti uç noktaları, şirket dışında yeni ekleme blobu özelliklerini destekler.
* Depolama Hizmetleri öykünücüsü tarafından henüz desteklenmeyen bir sürümünü kullanıyorsanız, öykünücü anlamlı bir hata mesajı döndürür. Öykünücüsünün en son sürümü kullanmanızı öneririz. VersionNotSupportedByEmulator hatası (HTTP durum kodu 400 - bozuk istek) karşılaşırsanız lütfen depolama öykünücüsünün en son sürümü indirin.
* Burada görüntülerle eşzamanlı birleştirme işlemler sırasında yanlış olmasını yarış durumu nedeniyle tablo varlık veri düzeltildi.

### <a name="version-40"></a>Sürüm 4.0
* Depolama öykünücüsü yürütülebilir adlandırıldı *AzureStorageEmulator.exe*.

### <a name="version-32"></a>Sürüm 3.2
* Depolama öykünücüsü, şimdi Blob, kuyruk ve tablo Hizmeti uç noktaları üzerinde depolama hizmetleri 2014-02-14 sürümünü destekler. Dosya Hizmeti uç noktaları, şu anda depolama öykünücüsünde desteklenmez. Bkz: [Azure Storage Hizmetleri için sürüm oluşturma](/rest/api/storageservices/Versioning-for-the-Azure-Storage-Services) sürümü 2014-02-14 hakkındaki ayrıntılar için.

### <a name="version-31"></a>Sürüm 3.1
* Okuma erişimli coğrafi olarak yedekli depolama (RA-GRS) depolama öykünücüsünde artık desteklenmektedir. Blob hizmeti istatistikleri alın, sıra hizmet istatistikleri alın ve tablo hizmeti istatistikleri API'leri alma ikincil hesap için desteklenir ve her zaman temel alınan SQL veritabanına göre geçerli saati olarak LastSyncTime yanıt öğenin değerini döndürür. İkincil depolama öykünücüsü ile programlı erişim için sürüm 3.2 veya sonraki sürümlerinde .NET için depolama istemcisi kitaplığını kullanırsınız. Microsoft Azure depolama istemci kitaplığı .NET başvurusu için Ayrıntılar için bkz.

### <a name="version-30"></a>Sürüm 3.0
* Azure depolama öykünücüsü artık aynı pakette işlem öykünücüsü olarak sevk edilir.
* Depolama öykünücüsü grafik kullanıcı arabirimi ile değiştiriliyor kodlanabilir bir komut satırı arabirimi kullanım dışıdır. Depolama öykünücüsü komut satırı aracını referans komut satırı arabirimi hakkında daha fazla bilgi için bkz. Grafik arabirim sürüm 3.0 var olmaya devam edecek, ancak işlem öykünücüsü sistem tepsisindeki simgeye sağ tıklatıp depolama öykünücüsü kullanıcı arabirimini Göster'i seçerek yüklendiğinde yalnızca erişilebilir.
* Azure depolama hizmeti sürüm 2013-08-15 artık tam olarak desteklenir. (Daha önce bu sürümü yalnızca sürüm 2.2.1 depolama öykünücüsü tarafından desteklenen Önizleme.)

## <a name="next-steps"></a>Sonraki adımlar

* Platformlar arası, topluluk tarafından tutulan açık kaynak depolama öykünücüsü değerlendirmek [Azurite](https://github.com/arafato/azurite). 
* [.NET kullanarak azure depolama örnekleri](../storage-samples-dotnet.md) Uygulamanızı geliştirirken kullanabileceğiniz birkaç kod örneklerinin bağlantılarını içerir.
* Kullanabileceğiniz [Microsoft Azure Depolama Gezgini](https://storageexplorer.com) bulut depolama hesabı ve depolama öykünücüsünde kaynaklarla çalışmak için.
