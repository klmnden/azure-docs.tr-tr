---
title: "Geliştirme ve sınama için Azure storage öykünücüsünü kullanma | Microsoft Docs"
description: "Azure storage öykünücüsü geliştirmek ve Azure Storage uygulamalarınızı test etme için boş yerel geliştirme ortamı sağlar. İstekleri nasıl doğrulanır, uygulamanızdan öykünücüsünü bağlanma ve komut satırı aracının nasıl kullanılacağını öğrenin."
services: storage
documentationcenter: 
author: tamram
manager: timlt
editor: tysonn
ms.assetid: f480b059-df8a-4a63-b05a-7f2f5d1f5c2a
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/08/2017
ms.author: tamram
ms.openlocfilehash: 7d86d5e8547d977c07cfbb0597b74382172a8472
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="use-the-azure-storage-emulator-for-development-and-testing"></a>Geliştirme ve sınama için Azure storage öykünücüsünü kullanma

Microsoft Azure storage öykünücüsü geliştirme amacıyla Azure Blob, kuyruk ve Tablo Hizmetleri öykünen yerel bir ortam sağlar. Depolama öykünücüsü kullanarak, bir Azure aboneliği oluşturmak veya herhangi bir maliyet olmadan uygulamanızı yerel olarak depolama hizmetleri karşı sınayabilirsiniz. Uygulamanızı öykünücüde nasıl çalıştığını ile memnun kaldığınızda, bulutta bir Azure depolama hesabı kullanmaya geçiş yapabilirsiniz.

## <a name="get-the-storage-emulator"></a>Depolama öykünücüsü Al
Depolama öykünücüsü olarak kullanılabilir parçası [Microsoft Azure SDK'sı](https://azure.microsoft.com/downloads/). Depolama öykünücüsü kullanarak da yükleyebilirsiniz [tek başına yükleyici](https://go.microsoft.com/fwlink/?linkid=717179&clcid=0x409) (doğrudan indirme). Depolama öykünücüsü yüklemek için bilgisayarınızda yönetici ayrıcalıkları olmalıdır.

Depolama öykünücüsü şu anda yalnızca Windows üzerinde çalışır. Olanlar için Linux için depolama öykünücüsü dikkate alarak, bir seçenek tutulan, açık kaynaklı depolama öykünücüsü topluluktur [Azurite](https://github.com/arafato/azurite).

> [!NOTE]
> Depolama öykünücüsü bir sürümünde oluşturulan veri farklı bir sürümünü kullanırken erişilebilir olması garanti edilmez. Verileriniz için uzun vadeli kalıcı olması gerekiyorsa, bu verileri Azure depolama hesabı yerine depolama öykünücüsünü depolamak önerilir.
> <p/>
> Depolama öykünücüsü OData kitaplıklarının belirli sürümlerini bağlıdır. Diğer sürümler ile depolama öykünücüsü tarafından kullanılan OData DLL'leri değiştirme desteklenmez ve beklenmeyen davranışlara neden olabilir. Ancak, OData depolama hizmeti tarafından desteklenen herhangi bir sürümünü öykünücüsünü istekleri göndermek için kullanılabilir.
>

## <a name="how-the-storage-emulator-works"></a>Depolama öykünücüsü nasıl çalışır?
Depolama öykünücüsü Azure storage Hizmetleri benzetmek için yerel bir Microsoft SQL Server örneğini ve yerel dosya sistemi kullanır. Varsayılan olarak, depolama öykünücüsünü Microsoft SQL Server 2012 Express LocalDB içinde bir veritabanı kullanır. Yerel bir SQL Server örneği yerel veritabanı örneği yerine erişmek için depolama öykünücüsünü yapılandırmak seçebilirsiniz. Daha fazla bilgi için bkz: [başlangıç ve başlatma depolama öykünücüsünü](#start-and-initialize-the-storage-emulator) bu makalenin sonraki bölümlerinde bölümü.

Depolama öykünücüsü, SQL Server veya Windows kimlik doğrulaması kullanarak LocalDB bağlanır.

Bazı işlev depolama öykünücüsü ve Azure storage Hizmetleri farklar. Bu farklılıklar hakkında daha fazla bilgi için bkz: [depolama öykünücüsü Azure Storage arasındaki farklar](#differences-between-the-storage-emulator-and-azure-storage) bu makalenin sonraki bölümünde.

## <a name="start-and-initialize-the-storage-emulator"></a>Başlangıç ve depolama öykünücüsü başlatma
Azure storage öykünücüsü başlatmak için:
1. Seçin **Başlat** düğmesini veya tuşuna **Windows** anahtarı.
1. Yazmaya başlayın `Azure Storage Emulator`.
1. Öykünücü görüntülenen uygulamalar listesinden seçin.

Depolama öykünücüsü başladığında, bir komut istemi penceresi görüntülenir. Bu konsol penceresi başlatın ve depolama öykünücüsü durdurmak, verileri temizlemek, durumunu Al ve öykünücü başlatma için kullanabilirsiniz. Daha fazla bilgi için bkz: [depolama öykünücüsü komut satırı aracını referans](#storage-emulator-command-line-tool-reference) bu makalenin sonraki bölümlerinde bölümü.

Öykünücü çalıştırırken, Windows görev çubuğundaki bildirim alanında bir simge görürsünüz.

Depolama öykünücüsü komut istemi penceresini kapattığınızda, depolama öykünücüsünü çalışmaya devam eder. Depolama öykünücüsü konsol penceresini yeniden açın için depolama öykünücüsünü başlatma gibi yukarıdaki adımları izleyin.

İlk kez depolama öykünücüsünü çalıştırdığınızda yerel depolama ortamı için başlatılır. Başlatma işlemi yerel veritabanı bir veritabanı oluşturur ve her yerel depolama hizmet için HTTP bağlantı noktası ayırır.

Varsayılan olarak yüklü depolama öykünücüsünü `C:\Program Files (x86)\Microsoft SDKs\Azure\Storage Emulator`.

> [!TIP]
> Kullanabileceğiniz [Microsoft Azure Storage Gezgini](http://storageexplorer.com) yerel depolama öykünücüsü kaynakları ile çalışmak için. Yüklenmiş ve depolama öykünücüsü başlatılmış sonra "(Geliştirme)" için "Depolama hesapları altında" Depolama Gezgini kaynakları ağacında arayın.
>

### <a name="initialize-the-storage-emulator-to-use-a-different-sql-database"></a>Farklı bir SQL veritabanını kullanmak üzere depolama öykünücüsünü başlatma
Varsayılan yerel veritabanı örneği dışında bir SQL veritabanı örneğine işaret edecek şekilde depolama öykünücüsünü başlatmak için depolama öykünücüsü komut satırı aracını kullanın:

1. Bölümünde açıklandığı gibi depolama öykünücüsü konsol penceresi açın [başlangıç ve başlatma depolama öykünücüsünü](#start-and-initialize-the-storage-emulator) bölümü.
1. Konsol penceresinde aşağıdaki komutu yazın burada `<SQLServerInstance>` SQL Server örneğinin adıdır. Yerel veritabanı kullanmak için `(localdb)\MSSQLLocalDb` SQL Server örneği olarak.

  `AzureStorageEmulator.exe init /server <SQLServerInstance>`

  Ayrıca, varsayılan SQL Server örneği kullanmak için öykünücü yönlendirir aşağıdaki komutu kullanabilirsiniz:

  `AzureStorageEmulator.exe init /server .\\`

  Veya varsayılan yerel veritabanı örneğine veritabanı yeniden başlatır aşağıdaki komutu kullanabilirsiniz:

  `AzureStorageEmulator.exe init /forceCreate`

Bu komutlar hakkında daha fazla bilgi için bkz: [depolama öykünücüsü komut satırı aracını referans](#storage-emulator-command-line-tool-reference).

> [!TIP]
> Kullanabileceğiniz [Microsoft SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms) LocalDB yükleme de dahil olmak üzere, SQL Server örneklerini yönetmek için (SSMS). SMSS içinde **sunucuya Bağlan** iletişim kutusunda, belirtin `(localdb)\MSSQLLocalDb` içinde **sunucu adı:** alan yerel veritabanı örneğine bağlanın.

## <a name="authenticating-requests-against-the-storage-emulator"></a>Depolama öykünücüsü isteklerinde kimlik doğrulaması
Yüklenmiş ve depolama öykünücüsü başlatılmış sonra kodunuzu onu karşı test edebilirsiniz. Anonim İstek olmadığı sürece bulutta Azure depolama gibi storage öykünücüsüne karşı yaptığınız her istek, kimliğinizin doğrulanması gerekiyor. İstekleri paylaşılan anahtar kimlik doğrulaması kullanarak storage öykünücüsüne karşı veya bir paylaşılan erişim imzası (SAS) ile kimlik doğrulaması yapabilir.

### <a name="authenticate-with-shared-key-credentials"></a>Paylaşılan anahtar kimlik bilgileriyle kimlik doğrulaması
[!INCLUDE [storage-emulator-connection-string-include](../../../includes/storage-emulator-connection-string-include.md)]

Bağlantı dizeleri hakkında daha fazla bilgi için bkz: [yapılandırma Azure Storage bağlantı dizelerini](../storage-configure-connection-string.md).

### <a name="authenticate-with-a-shared-access-signature"></a>İle paylaşılan erişim imzası kimlik doğrulaması
Xamarin kitaplığı gibi bazı Azure storage istemcisi kitaplıklarını yalnızca bir paylaşılan erişim imzası (SAS) belirteci ile kimlik doğrulamasını destekler. Gibi bir araç kullanarak SAS belirteci oluşturabilirsiniz [Depolama Gezgini](http://storageexplorer.com/) veya paylaşılan anahtar kimlik doğrulamasını destekleyen başka bir uygulama.

Azure PowerShell kullanarak bir SAS belirteci de oluşturabilirsiniz. Aşağıdaki örnek, bir blob kapsayıcısı için tam izinleri olan bir SAS belirteci oluşturur:

1. Henüz yapmadıysanız yükleme Azure PowerShell (cmdlet'leri önerilen Azure PowerShell'in en son sürümünü kullanarak). Yükleme yönergeleri için bkz: [yükleyin ve Azure PowerShell yapılandırma](/powershell/azure/install-azurerm-ps).
2. Azure PowerShell'i açın ve değiştirerek aşağıdaki komutları çalıştırın `ACCOUNT_NAME` ve `ACCOUNT_KEY==` kendi kimlik bilgileriyle ve `CONTAINER_NAME` seçtiğiniz bir ada sahip:

```powershell
$context = New-AzureStorageContext -StorageAccountName "ACCOUNT_NAME" -StorageAccountKey "ACCOUNT_KEY=="

New-AzureStorageContainer CONTAINER_NAME -Permission Off -Context $context

$now = Get-Date

New-AzureStorageContainerSASToken -Name CONTAINER_NAME -Permission rwdl -ExpiryTime $now.AddDays(1.0) -Context $context -FullUri
```

Yeni kapsayıcı için sonuçta elde edilen paylaşılan erişim imzası URI benzer olmalıdır:

```
https://storageaccount.blob.core.windows.net/sascontainer?sv=2012-02-12&se=2015-07-08T00%3A12%3A08Z&sr=c&sp=wl&sig=t%2BbzU9%2B7ry4okULN9S0wst%2F8MCUhTjrHyV9rDNLSe8g%3Dsss
```

Bu örnek ile oluşturulmuş paylaşılan erişim imzası bir gün için geçerli değil. İmza BLOB kapsayıcı içinde tam (okuma, yazma, silme, liste) erişim verir.

Paylaşılan erişim imzaları ile ilgili daha fazla bilgi için bkz: [kullanarak paylaşılan erişim imzaları (SAS) Azure storage'da](../storage-dotnet-shared-access-signature-part-1.md).

## <a name="addressing-resources-in-the-storage-emulator"></a>Depolama öykünücüsü kaynaklarında adresleme
Hizmet uç noktalarına depolama öykünücüsü Azure storage hesabı olanlardan farklı. Yerel bilgisayarın etki alanı adı çözümlemesi, depolama öykünücüsü uç yerel adreslerini gerektiren gerçekleştirmez çünkü farktır.

Bir Azure depolama hesabındaki bir kaynak adresi için aşağıdaki düzenini kullanın. Hesap adı URI ana bilgisayar adı bir parçasıdır ve ele alınan kaynak URI yolu bir parçasıdır:

`<http|https>://<account-name>.<service-name>.core.windows.net/<resource-path>`

Örneğin, aşağıdaki URI bir Azure depolama hesabındaki bir blob için geçerli bir adresi şöyledir:

`https://myaccount.blob.core.windows.net/mycontainer/myblob.txt`

Ancak, depolama öykünücüsü, yerel bilgisayarın etki alanı adı çözümlemesine gerçekleştirmez hesap adı çünkü URI yolu ana bilgisayar adı yerine bir parçası. Depolama öykünücüsü bir kaynak için aşağıdaki URI biçimi kullanın:

`http://<local-machine-address>:<port>/<account-name>/<resource-path>`

Örneğin, bir blob depolama öykünücüsünü erişmek için şu adresi kullanılabilir:

`http://127.0.0.1:10000/myaccount/mycontainer/myblob.txt`

Hizmet uç noktaları için depolama öykünücüsünü şunlardır:

* BLOB hizmeti:`http://127.0.0.1:10000/<account-name>/<resource-path>`
* Kuyruk hizmeti:`http://127.0.0.1:10001/<account-name>/<resource-path>`
* Tablo hizmeti:`http://127.0.0.1:10002/<account-name>/<resource-path>`

### <a name="addressing-the-account-secondary-with-ra-grs"></a>RA-GRS ile ikincil hesap adresleme
Sürüm 3.1 ile başlayarak, depolama öykünücüsünü okuma erişimli coğrafi olarak yedekli çoğaltma (RA-GRS) destekler. Depolama kaynaklarını bulut hem de yerel öykünücüsü için ikincil konuma erişmek göre ekleme - hesap adının ikincil. Örneğin, bir blob depolama öykünücüsünde salt okunur ikincil kullanarak erişmek için şu adresi kullanılabilir:

`http://127.0.0.1:10000/myaccount-secondary/mycontainer/myblob.txt`

> [!NOTE]
> Depolama öykünücüsü ile ikincil için programlı erişim için depolama istemci kitaplığı için .NET 3.2 veya sonraki bir sürümü kullanın. Bkz: [.NET için Microsoft Azure Storage istemci Kitaplığı](https://msdn.microsoft.com/library/azure/dn261237.aspx) Ayrıntılar için.
>
>

## <a name="storage-emulator-command-line-tool-reference"></a>Depolama öykünücüsü komut satırı aracı başvurusu
Depolama öykünücüsü başlattığınızda sürüm 3. 0'dan başlayarak, bir konsol penceresi görüntülenir. Komut satırında başlatmak ve öykünücüsü yanı sıra durumu için sorguyu durdur ve diğer işlemleri gerçekleştirmek için konsol penceresinde kullanın.

> [!NOTE]
> Microsoft Azure işlem öykünücüsü yüklü varsa, depolama öykünücüsünü başlattığında sistem tepsisi simgesi görünür. Depolama öykünücüsü durdurmak ve başlatmak için grafiksel bir yöntem sağlayan bir menüsünü ortaya çıkarmak üzere simgesine sağ tıklayın.
>
>

### <a name="command-line-syntax"></a>Komut satırı sözdizimi
`AzureStorageEmulator.exe [start] [stop] [status] [clear] [init] [help]`

### <a name="options"></a>Seçenekler
Seçeneklerinin listesini görüntülemek için şunu yazın `/help` komut isteminde.

| Seçenek | Açıklama | Komut | Bağımsız Değişkenler |
| --- | --- | --- | --- |
| **Start** |Depolama öykünücüsü başlatır. |`AzureStorageEmulator.exe start [-inprocess]` |*-InProcess*: yeni bir işlem oluşturmak yerine geçerli işlem öykünücüsü başlatın. |
| **Durdur** |Depolama öykünücüsü durdurur. |`AzureStorageEmulator.exe stop` | |
| **Durumu** |Depolama öykünücüsü durumunu yazdırır. |`AzureStorageEmulator.exe status` | |
| **Temizle** |Komut satırında belirtilen tüm hizmetleri verileri temizler. |`AzureStorageEmulator.exe clear [blob] [table] [queue] [all]                                                    ` |*BLOB*: blob verileri temizler. <br/>*sıra*: sırası verileri temizler. <br/>*Tablo*: Tablo verileri temizler. <br/>*tüm*: tüm hizmetlerin tüm verileri temizler. |
| **Init** |Öykünücü ayarlamak için tek seferlik başlatma gerçekleştirir. |<code>AzureStorageEmulator.exe init [-server serverName] [-sqlinstance instanceName] [-forcecreate&#124;-skipcreate] [-reserveports&#124;-unreserveports] [-inprocess]</code> |*-Sunucu Sunucuadı\örnekadı*: SQL örneğini barındıran sunucunun belirtir. <br/>*-sqlınstance InstanceName*: varsayılan sunucu örneğinde kullanılacak SQL örneğinin adını belirtir. <br/>*-forcecreate*: zaten mevcut olsa bile SQL veritabanı oluşturma zorlar. <br/>*-skipcreate*: SQL veritabanı oluşturma atlar. Bu - forcecreate önceliklidir.<br/>*-reserveports*: servisleri ile ilişkili HTTP bağlantı noktalarını ayırma dener.<br/>*-unreserveports*: HTTP bağlantı noktaları için ayırmaları kaldırmak için deneme Hizmetleri ile ilişkilendirilmiş. Bu - reserveports önceliklidir.<br/>*-InProcess*: yeni bir işlem örnekten oluşturmak yerine geçerli işlemde başlatmayı gerçekleştirir. Mevcut işlemin yükseltilmiş izinleri olan bağlantı noktası ayırmaları değiştiriliyorsa başlatılması gerekir. |

## <a name="differences-between-the-storage-emulator-and-azure-storage"></a>Depolama öykünücüsü Azure Storage arasındaki farklar
Depolama öykünücüsü yerel bir SQL örneğinde çalışan benzetilmiş bir ortam olduğundan, arasındaki işlevsel farklılıklar öykünücüsü ve Azure storage hesabı bulutta vardır:

* Depolama öykünücüsü yalnızca tek bir sabit hesap ve bilinen bir kimlik doğrulama anahtarı destekler.
* Depolama öykünücüsü ölçeklenebilir depolama hizmeti ve çok sayıda eşzamanlı istemci desteklemez.
* Bölümünde açıklandığı gibi [depolama öykünücüsünü kaynaklarında adresleme](#addressing-resources-in-the-storage-emulator), kaynakları ele farklı depolama öykünücüsü Azure storage hesabı karşı içinde. Etki alanı adı çözümlemesine kullanılabilir olduğu için bu farktır bulutta ancak yerel bilgisayarda değil.
* Sürüm 3.1 ile başlayarak, depolama öykünücüsü hesabı okuma erişimli coğrafi olarak yedekli çoğaltma (RA-GRS) destekler. Öykünücü tüm hesaplarının RA-GRS etkin olması ve birincil ve ikincil çoğaltmaları hiçbir zaman bir gecikme yoktur. Blob hizmeti istatistikleri almak, kuyruk hizmeti istatistikleri almak ve tablo hizmeti istatistikleri alma işlemleri ikincil hesap desteklenir ve her zaman değerini döndürür `LastSyncTime` temel SQL veritabanını göre geçerli saati olarak yanıt öğesi.
* Şu anda SMB protokolü hizmet uç noktaları ve dosya hizmeti depolama öykünücüsünde desteklenmez.
* Öykünücü tarafından henüz desteklenmeyen bir depolama Hizmetleri sürümünü kullanıyorsanız, depolama öykünücüsünü VersionNotSupportedByEmulator hata (HTTP durum kodu 400 - Hatalı istek) döndürür.

### <a name="differences-for-blob-storage"></a>Blob storage için farklar
Blob depolama alanına öykünücü aşağıdaki farkları Uygula:

* Depolama öykünücüsü yalnızca destekler blob 2 GB boyutları.
* Artımlı kopya kopyalanacak üzerine BLOB'lar alınan anlık hizmette hata döndüren sağlar.
* Get sayfa aralıklarını fark artımlı kopya Blob kullanarak kopyalanan anlık görüntüler arasında çalışmaz.
* İstekte kira kimliği belirtilmemiş olsa bile bir Blob Put işlemi depolama öykünücü ile etkin bir kira var. bir blob karşı başarılı olabilir.
* İşlem öykünücüsü tarafından desteklenmeyen Blob ekleyin. Bir ek blobu üzerinde bir işlemi çalışırken FeatureNotSupportedByEmulator hata (HTTP durum kodu 400 - Hatalı istek) döndürür.

### <a name="differences-for-table-storage"></a>Table storage farkları
Tablo depolama öykünücüsü alanına aşağıdaki farkları Uygula:

* Depolama öykünücüsü tablo hizmetinde tarih özellikleri yalnızca SQL Server 2005'in (bunlar 1 Ocak 1753 sonraki olması gerekir) tarafından desteklenen aralığın destekler. Tüm tarih 1 Ocak 1753 önce bu değer ile değiştirilir. Tarihleri kesinliğini tarihleri 1 kesin olduğu anlamına gelir, SQL Server 2005'in duyarlık sınırlıdır/saniyede 300th.
* Depolama öykünücüsü değerinden 512 bayt her bölüm anahtarını ve satır anahtar özellik değerlerini destekler. Buna ek olarak, hesap adı, tablo adını ve anahtar özellik adlarının birlikte toplam boyutu 900 baytı aşamaz.
* Depolama öykünücüsü tablosunda bir satırı toplam boyutu 1 MB'tan az sınırlıdır.
* Depolama öykünücüsünde türü özellikleri veri `Edm.Guid` veya `Edm.Binary` destekleyebilir `Equal (eq)` ve `NotEqual (ne)` Karşılaştırma işleçleri sorgu dizeleri filtre.

### <a name="differences-for-queue-storage"></a>Kuyruk depolama farkları
Kuyruk depolama öykünücüsü içinde belirli bir fark yoktur.

## <a name="storage-emulator-release-notes"></a>Depolama öykünücüsü sürüm notları
### <a name="version-52"></a>5.2 sürümü
* Depolama öykünücüsü şimdi Blob, kuyruk ve tablo Hizmeti uç noktalarda depolama hizmetleri 2017-04-17 sürümünü destekler.
* Bir hata olduğu tablo özellik değerlerini düzgün şekilde kodlanmamış sabit.

### <a name="version-51"></a>Sürüm 5.1
* Depolama öykünücüsü burada döndürdü hatanın düzeltildiğini `DataServiceVersion` nerede hizmet değildi bazı yanıtları üstbilgisinde.

### <a name="version-50"></a>Sürüm 5.0
* Depolama öykünücüsü yükleyici artık için varolan MSSQL denetler ve .NET Framework yükler.
* Depolama öykünücüsü yükleyici artık yükleme bir parçası olarak bir veritabanı oluşturur. Veritabanı hala başlangıç bir parçası olarak gerekirse oluşturulacak.
* Veritabanı oluşturma artık yükseltme gerektirir.
* Bağlantı noktası ayırmaları başlatma için artık gerekli değildir.
* Aşağıdaki seçenekleri ekler `init`: `-reserveports` (yükseltme gerekir), `-unreserveports` (yükseltme gerekir), `-skipcreate`.
* Sistem tepsisi simgesi depolama öykünücüsü UI seçeneği artık komut satırı arabirimini başlatır. Eski GUI artık kullanılamıyor.
* Bazı DLL'lerin kaldırılmış veya yeniden adlandırılamaz.

### <a name="version-46"></a>Sürüm 4.6
* Depolama öykünücüsü şimdi Blob, kuyruk ve tablo Hizmeti uç noktalarda depolama hizmetleri 2016-05-31 sürümünü destekler.

### <a name="version-45"></a>Sürüm 4.5
* Başlatma ve yükleme depolama öykünücüsünü yedekleme veritabanı adlandırıldığında başarısız olmasına neden olan bir hata sabit.

### <a name="version-44"></a>Sürüm 4.4
* Depolama öykünücüsü şimdi sürümü 2015-12-11, depolama hizmetleri Blob, kuyruk ve tablo Hizmeti uç noktalarda destekler.
* Depolama öykünücüsü'nın çöp toplama blob verisi artık çok sayıda BLOB ilgilenirken daha verimli olur.
* Kapsayıcı ACL biraz daha farklı depolama hizmeti bunu nasıl yapar doğrulanacak XML neden olan bir hata sabit.
* Bazen maksimum ve minimum yanlış saat diliminde bildirilecek DateTime değerleri neden olan bir hata sabit.

### <a name="version-43"></a>Sürümü 4.3
* Depolama öykünücüsü şimdi sürümü 2015-07-08, depolama hizmetleri Blob, kuyruk ve tablo Hizmeti uç noktalarda destekler.

### <a name="version-42"></a>4.2 sürümü
* Depolama öykünücüsü şimdi Blob, kuyruk ve tablo Hizmeti uç noktalarda depolama hizmetleri 2015-04-05 sürümünü destekler.

### <a name="version-41"></a>Sürümü 4.1
* Depolama öykünücüsü şimdi sürümü 2015-02-21 Blob, kuyruk ve tablo Hizmeti uç noktaları, depolama hizmetlerinde yeni ekleme Blob özellikleri dışında destekler.
* Öykünücü tarafından henüz desteklenmeyen bir depolama Hizmetleri sürümünü kullanıyorsanız, öykünücü anlamlı bir hata mesajı döndürür. Öykünücü en son sürümünü kullanmanızı öneririz. Lütfen bir VersionNotSupportedByEmulator hatası (HTTP durum kodu 400 - Hatalı istek) karşılaşırsanız, depolama öykünücüsü en son sürümünü yükleyin.
* Bir hata; burada görüntülerle eşzamanlı birleştirme işlemleri sırasında hatalı bir yarış neden koşul tablo varlık veri sabit.

### <a name="version-40"></a>Sürüm 4.0
* Depolama öykünücüsü yürütülebilir adlandırıldı *AzureStorageEmulator.exe*.

### <a name="version-32"></a>Sürüm 3.2
* Depolama öykünücüsü şimdi Blob, kuyruk ve tablo Hizmeti uç noktalarda depolama hizmetleri 2014-02-14 sürümünü destekler. Dosya Hizmeti uç noktalarını depolama öykünücüsünde şu anda desteklenmemektedir. Bkz: [Azure Storage Hizmetleri için sürüm oluşturma](/rest/api/storageservices/Versioning-for-the-Azure-Storage-Services) sürümü 2014-02-14 hakkında ayrıntılı bilgi için.

### <a name="version-31"></a>Sürüm 3.1
* Okuma erişimli coğrafi olarak yedekli depolama (RA-GRS), depolama öykünücüsünde artık desteklenmektedir. Blob hizmeti istatistikleri almak, kuyruk hizmeti istatistikleri almak ve tablo hizmeti istatistikleri API'leri almak için ikincil hesabı desteklenir ve her zaman arka plandaki SQL veritabanı göre geçerli saati olarak LastSyncTime yanıt öğenin değerini döndürür. Depolama öykünücüsü ile ikincil için programlı erişim için depolama istemci kitaplığı için .NET 3.2 veya sonraki bir sürümü kullanın. Microsoft Azure Storage istemci kitaplığı .NET başvurusu için ayrıntılı bilgi için bkz.

### <a name="version-30"></a>Sürüm 3.0
* Azure storage Öykünücüsü işlem öykünücüsü aynı paketteki artık geliyordu.
* Depolama öykünücüsü grafik kullanıcı arabirimi lehinde kodlanabilir bir komut satırı arabirimi kullanım dışıdır. Komut satırı arabirimi hakkında daha fazla bilgi için bkz. depolama öykünücüsü komut satırı aracı başvurusu. Grafik arabirimi sürüm 3.0 var olmaya devam edecek, ancak işlem öykünücüsü sistem tepsisi simgesine sağ tıklatıp depolama öykünücüsü UI Göster'i seçerek yüklendiğinde yalnızca erişilebilir.
* Sürüm 2013-08-15 Azure depolama hizmetleri artık tam olarak desteklenmektedir. (Daha önce bu sürümü yalnızca sürüm 2.2.1 depolama öykünücüsü tarafından desteklenen Önizleme.)

## <a name="next-steps"></a>Sonraki adımlar

* Platformlar arası, topluluk tarafından tutulan açık kaynak depolama öykünücüsünü değerlendirmek [Azurite](https://github.com/arafato/azurite). 
* [.NET kullanarak azure depolama örnekleri](../storage-samples-dotnet.md) Uygulamanızı geliştirirken kullanabileceğiniz birkaç kod örnekleri bağlantılar içerir.
* Kullanabileceğiniz [Microsoft Azure Storage Gezgini](http://storageexplorer.com) , bulut depolama hesabı ve depolama öykünücüsü kaynaklarla çalışmak için.
