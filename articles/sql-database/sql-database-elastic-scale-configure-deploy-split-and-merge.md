---
title: Ayırma-birleştirme hizmetini dağıtma | Microsoft Docs
description: Ayırma-birleştirme çok parçalı veritabanları arasında veri taşıma için kullanın.
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 12/04/2018
ms.openlocfilehash: 5aff7e93dcfaa5320be0d6f7d427abcdc88c69e4
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60585517"
---
# <a name="deploy-a-split-merge-service-to-move-data-between-sharded-databases"></a>Parçalı veritabanları arasında veri taşıma için bir ayırma-birleştirme hizmetini dağıtma

Ayırma-Birleştirme aracı, parçalı veritabanları arasında veri taşımanıza olanak tanır. Bkz: [ölçeği genişletilen bulut veritabanları arasında veri taşıma](sql-database-elastic-scale-overview-split-and-merge.md)

## <a name="download-the-split-merge-packages"></a>Ayırma-birleştirme paketlerini indirin
1. En son NuGet sürümü [NuGet](https://docs.nuget.org/docs/start-here/installing-nuget).
2. Bir komut istemi açın ve nuget.exe karşıdan yüklediğiniz dizine gidin. İndirme için PowerShell komutlarını içerir.
3. En son bölme-birleştirme paketi geçerli dizine indirmek aşağıdaki komutu:
   ```
   nuget install Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge
   ```  

Adlı bir dizinde dosyalar yerleştirilir **Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge.x.x.xxx.x** burada *x.x.xxx.x* sürüm numarasını yansıtır. Ayırma-birleştirme hizmeti dosyalarda Bul **content\splitmerge\service** alt dizini ve ayırma-birleştirme PowerShell betikleri (ve gerekli istemci DLL'ler) **content\splitmerge\powershell** alt dizini.

## <a name="prerequisites"></a>Önkoşullar
1. Ayırma-birleştirme durumu veritabanı olarak kullanılacak bir Azure SQL DB veritabanı oluşturun. [Azure Portal](https://portal.azure.com) gidin. Yeni bir **SQL veritabanı**. Veritabanı bir ad verin ve yeni yönetici ve parola oluşturun. Adını ve daha sonra kullanmak için parolayı kaydettiğinizden emin olun.
2. Azure SQL veritabanı sunucunuza bağlanmak Azure Services verdiğinden emin olun. Portalda, içinde **güvenlik duvarı ayarlarını**, olun **Azure hizmetlerine erişime izin ver** ayarı **üzerinde**. "Kaydet" simgesine tıklayın.
3. Tanılama çıkışı için bir Azure depolama hesabı oluşturun.
4. Ayırma-birleştirme hizmeti için bir Azure bulut hizmeti oluşturun.

## <a name="configure-your-split-merge-service"></a>Ayırma-birleştirme hizmetinizi yapılandırın
### <a name="split-merge-service-configuration"></a>Ayırma-birleştirme hizmetini yapılandırma
1. Ayırma-birleştirme derlemeleri indirdiğiniz klasörde bir kopyasını oluşturmak **ServiceConfiguration.Template.cscfg** birlikte sevk edilen dosya **SplitMergeService.cspkg** ve yeniden adlandırın **ServiceConfiguration.cscfg**.
2. Açık **ServiceConfiguration.cscfg** sertifika parmak izleri biçimi gibi girişlerdeki doğrular Visual Studio gibi bir metin düzenleyicisinde.
3. Yeni bir veritabanı oluşturun veya mevcut bir veritabanı bölme-birleştirme işlemleri için durum veritabanı işlevi görür ve o veritabanının bağlantı dizesini almak için seçin. 
   
   > [!IMPORTANT]
   > Şu anda durumu veritabanı Latin harmanlama kullanmanız gerekir (SQL\_Latin1\_genel\_CP1\_CI\_AS). Daha fazla bilgi için [Windows harmanlama adı (Transact-SQL)](https://msdn.microsoft.com/library/ms188046.aspx).
   >

   Azure SQL DB ile bağlantı dizesi genellikle formu şöyledir:
      ```
      Server=myservername.database.windows.net; Database=mydatabasename;User ID=myuserID; Password=mypassword; Encrypt=True; Connection Timeout=30
      ```

4. Her ikisi de cscfg dosyasında bu bağlantı dizesini girin **SplitMergeWeb** ve **SplitMergeWorker** rol bölümlerde ElasticScaleMetadata ayarı.
5. İçin **SplitMergeWorker** rolü için Azure depolama için geçerli bir bağlantı dizesi girin **WorkerRoleSynchronizationStorageAccountConnectionString** ayarı.

### <a name="configure-security"></a>Güvenliği yapılandırma
Hizmet güvenliği yapılandırmak ayrıntılı yönergeler için başvurmak [bölme-birleştirme güvenliği yapılandırma](sql-database-elastic-scale-split-merge-security-configuration.md).

Bu öğretici için bir basit bir test dağıtım amacı doğrultusunda, hizmet almaya gerçekleştirilir ve çalışan yapılandırma adımları en az bir dizi olacaktır. Bu adımları yalnızca bir makine/bunları hizmetiyle iletişim kurmak için yürütme hesabını etkinleştirin.

### <a name="create-a-self-signed-certificate"></a>Otomatik olarak imzalanan sertifika oluşturma
Yeni bir dizin oluşturun ve aşağıdaki komutu kullanarak bu dizinden yürütmeniz bir [Visual Studio için geliştirici komut istemi](https://msdn.microsoft.com/library/ms229859.aspx) penceresi:

   ```
    makecert ^
    -n "CN=*.cloudapp.net" ^
    -r -cy end -sky exchange -eku "1.3.6.1.5.5.7.3.1,1.3.6.1.5.5.7.3.2" ^
    -a sha256 -len 2048 ^
    -sr currentuser -ss root ^
    -sv MyCert.pvk MyCert.cer
   ```

Özel anahtarı korumak için bir parola istenir. Güçlü bir parola girin ve onaylayın. Bir kez daha, daha sonra kullanılmak üzere parola istenir. Tıklayın **Evet** sonunda güvenilen sertifika yetkilileri kök deposuna aktarın.

### <a name="create-a-pfx-file"></a>Bir PFX dosyası oluşturma
Makecert yürütüldüğü penceresinden aşağıdaki komutu yürütün; bir sertifika oluşturmak için kullanılan parolanın aynısını kullanın:

    pvk2pfx -pvk MyCert.pvk -spc MyCert.cer -pfx MyCert.pfx -pi <password>

### <a name="import-the-client-certificate-into-the-personal-store"></a>İstemci sertifikası Kişisel depoya içeri aktarma
1. Windows Gezgini'nde çift tıklayarak **MyCert.pfx**.
2. İçinde **Sertifika Alma Sihirbazı** seçin **geçerli kullanıcının** tıklatıp **sonraki**.
3. Dosya yolunu doğrulayın ve tıklatın **sonraki**.
4. Parolayı yazın, bırakın **dahil tüm genişletilmiş özellikleri** tıklatın ve seçili **sonraki**.
5. Bırakın **[…] sertifika deposunu otomatik olarak Seç**  tıklatın ve seçili **sonraki**.
6. Tıklayın **son** ve **Tamam**.

### <a name="upload-the-pfx-file-to-the-cloud-service"></a>Bulut hizmetine PFX dosyasını karşıya yükle
1. [Azure Portal](https://portal.azure.com) gidin.
2. Seçin **bulut Hizmetleri**.
3. Ayırma/birleştirme hizmeti için yukarıda oluşturulan bulut hizmeti seçin.
4. Tıklayın **sertifikaları** en üstteki menüde.
5. Tıklayın **karşıya** alt çubuğundaki.
6. PFX dosyasını seçin ve yukarıdaki aynı parolayı girin.
7. Tamamlandığında, listedeki yeni girişi sertifika parmak izini kopyalayın.

### <a name="update-the-service-configuration-file"></a>Hizmet yapılandırma dosyasını güncelleştir
Yukarıda bu ayarlar parmak izi/değer özniteliği kopyaladığınız sertifika parmak izini yapıştırın.
Çalışan rolü için:
   ```
    <Setting name="DataEncryptionPrimaryCertificateThumbprint" value="" />
    <Certificate name="DataEncryptionPrimary" thumbprint="" thumbprintAlgorithm="sha1" />
   ```

Web rolü için:

   ```
    <Setting name="AdditionalTrustedRootCertificationAuthorities" value="" />
    <Setting name="AllowedClientCertificateThumbprints" value="" />
    <Setting name="DataEncryptionPrimaryCertificateThumbprint" value="" />
    <Certificate name="SSL" thumbprint="" thumbprintAlgorithm="sha1" />
    <Certificate name="CA" thumbprint="" thumbprintAlgorithm="sha1" />
    <Certificate name="DataEncryptionPrimary" thumbprint="" thumbprintAlgorithm="sha1" />
   ```

Lütfen şifreleme, sunucu sertifikası ve istemci sertifikaları için CA sertifikaları için üretim dağıtımları ayrı not kullanılmalıdır. Bu ayrıntılı yönergeler için bkz: [Güvenlik Yapılandırması](sql-database-elastic-scale-split-merge-security-configuration.md).

## <a name="deploy-your-service"></a>Hizmetinizi dağıtmadan
1. [Azure portal](https://portal.azure.com)'a gidin
2. Daha önce oluşturduğunuz bir bulut hizmeti seçin.
3. **Genel Bakış**'a tıklayın.
4. Hazırlık ortamı seçin, ardından tıklatın **karşıya**.
5. İletişim kutusunda, bir dağıtım etiketini girin. 'Paketi' ve 'Configuration' için 'Dan yerel' tıklatın ve seçin **SplitMergeService.cspkg** dosyası ve daha önce yapılandırılmış, cscfg dosyası.
6. Etiketli onay kutusunu emin **bir veya daha fazla rol tek bir örnek içeriyorsa bile Dağıt** denetlenir.
7. Sağ alt dağıtımına başlamak için onay düğmesine basın. Bunun tamamlanması birkaç dakika bekler.


## <a name="troubleshoot-the-deployment"></a>Dağıtım sorunlarını giderme
Çevrimiçi olması web rolünüzü başarısız olursa, buna büyük olasılıkla güvenlik yapılandırması ile ilgili bir sorun var. SSL yukarıda açıklanan şekilde yapılandırıldığından emin olun.

Çevrimiçi olması, çalışan rolü başarısız olur, ancak web rolünüz başarılı, daha önce oluşturduğunuz durumu veritabanına bağlanırken bir sorun büyük olasılıkla olur.

* Cscfg bağlantı dizesinin doğru olduğundan emin olun.
* Sunucu ve veritabanı var olduğundan ve kullanıcı kimliğini ve parolasını doğru olduğunu denetleyin.
* Azure SQL veritabanı için bağlantı dizesi biçiminde olmalıdır:

   ```  
   Server=myservername.database.windows.net; Database=mydatabasename;User ID=myuserID; Password=mypassword; Encrypt=True; Connection Timeout=30
   ```

* Sunucu adı ile başlamayan olun **https://** .
* Azure SQL veritabanı sunucunuza bağlanmak Azure Services verdiğinden emin olun. Bunu yapmak için veritabanınızı portalda açın ve emin **Azure hizmetlerine erişime izin ver** ayarı **üzerinde**.

## <a name="test-the-service-deployment"></a>Hizmet dağıtımı test etme
### <a name="connect-with-a-web-browser"></a>Bir web tarayıcısı ile bağlanma
Ayırma-birleştirme hizmeti web uç noktası belirleyin. Giderek bu portalda bulabilirsiniz **genel bakış** bulut hizmetinizin ve altında bakarak **Site URL'si** işlecin sağ tarafındaki. Değiştirin **http://** ile **https://** sonra HTTP uç noktasına varsayılan güvenlik ayarlarını devre dışı bırakın. Sayfa için bu URL'yi tarayıcınıza yükleyin.

### <a name="test-with-powershell-scripts"></a>PowerShell betikleri ile test
Dağıtım ve ortamınızı dahil edilmiş örnek PowerShell betiklerini çalıştırarak test edilebilir.

Bulunan komut dosyaları şunlardır:

1. **SetupSampleSplitMergeEnvironment.ps1** -test veri katmanını oluşturan ayırma/birleştirme için ayarlar (ayrıntılı bir açıklaması için aşağıdaki tabloya bakın)
2. **ExecuteSampleSplitMerge.ps1** -test test işlemlerini yürüten veri katmanı (ayrıntılı bir açıklaması için aşağıdaki tabloya bakın)
3. **GetMappings.ps1** - üst düzey örnek parça eşlemeleri geçerli durumunu yazdıran komut dosyası.
4. **ShardManagement.psm1** -ShardManagement API'sini sarmalayan Yardımcısı betiği
5. **SqlDatabaseHelpers.psm1** -yardımcı betik oluşturma ve SQL veritabanlarını yönetme
   
   <table style="width:100%">
     <tr>
       <th>PowerShell dosyası</th>
       <th>Adımlar</th>
     </tr>
     <tr>
       <th rowspan="5">SetupSampleSplitMergeEnvironment.ps1</th>
       <td>1.    Parça eşleme Yöneticisi veritabanını oluşturur</td>
     </tr>
     <tr>
       <td>2.    2. parça veritabanı oluşturur.
     </tr>
     <tr>
       <td>3.    Bu veritabanları (mevcut bir parça söz konusu veritabanlarında eşler siler) için bir parça eşlemesi oluşturur. </td>
     </tr>
     <tr>
       <td>4.    Parçalar küçük bir örnek tablo oluşturur ve parçalar tabloda doldurur.</td>
     </tr>
     <tr>
       <td>5.    Parçalı tablo için bir adet Schemaınfo bildirir.</td>
     </tr>
   </table>
   <table style="width:100%">
     <tr>
       <th>PowerShell dosyası</th>
       <th>Adımlar</th>
     </tr>
   <tr>
       <th rowspan="4">ExecuteSampleSplitMerge.ps1 </th>
       <td>1.    Yarı ilk parça verilerden ikinci parçaya böler ayırma-birleştirme hizmetini web ön bir ayırma isteği gönderir.</td>
     </tr>
     <tr>
       <td>2.    Web ön uç bölünmüş istek durumu için yoklama ve istek tamamlanana kadar bekler.</td>
     </tr>
     <tr>
       <td>3.    Veri ikinci parçadan veri geri ilk parçaya taşıyan ayırma-birleştirme hizmetini web ön uç için birleştirme isteği gönderir.</td>
     </tr>
     <tr>
       <td>4.    Web ön uç için birleştirme isteği durumunu yoklar ve istek tamamlanana kadar bekler.</td>
     </tr>
   </table>
   
## <a name="use-powershell-to-verify-your-deployment"></a>Dağıtımınızı doğrulamak için PowerShell kullanma
1. Yeni bir PowerShell penceresi açın ve ayırma-birleştirme paketi karşıdan yüklediğiniz dizine gidin ve ardından "powershell" dizinine gidin.
2. Azure SQL veritabanı sunucusu oluşturun (veya var olan bir sunucu seçin) parça eşleme Yöneticisi ve parçalar oluşturulacağı.
   
   > [!NOTE]
   > SetupSampleSplitMergeEnvironment.ps1 betik bu veritabanları aynı sunucu üzerinde betik basit tutmak için varsayılan olarak oluşturur. Bu bir kısıtlama bölme-birleştirme hizmetinin kendisi değil.
   >
   
   Veritabanları okuma/yazma erişimi olan bir SQL kimlik doğrulaması oturum açma verilerini taşımak ve parça eşlemesi güncelleştirmek bölme-birleştirme hizmeti için gerekli olacaktır. Ayırma-birleştirme hizmetini bulutta çalıştığından, tümleşik kimlik doğrulaması şu anda desteklemiyor.
   
   Azure SQL server, bu betikleri çalıştıran makinenin IP adresinden erişime izin verecek şekilde yapılandırıldığından emin olun. Bu ayar, Azure SQL sunucusu altında bulabilirsiniz / configuration / izin verilen IP adresleri.
3. Örnek ortamı oluşturmak için SetupSampleSplitMergeEnvironment.ps1 betiğini yürütün.
   
   Bu betiğin çalıştırılması, parça eşleme Yöneticisi veritabanını ve parçalar yapıları var olan bir parça eşleme yönetimi verilerini siler. Parça eşlemesi veya parçalar yeniden başlatmak istiyorsanız, komut dosyasını yeniden çalıştırmak yararlı olabilir.
   
   Örnek komut satırı:

   ```   
     .\SetupSampleSplitMergeEnvironment.ps1 
   
         -UserName 'mysqluser' 
         -Password 'MySqlPassw0rd' 
         -ShardMapManagerServerName 'abcdefghij.database.windows.net'
   ```      
4. Örnek ortamda şu anda mevcut eşlemeleri görüntülemek için Getmappings.ps1 betiği yürütün.
   
   ```
     .\GetMappings.ps1 
   
         -UserName 'mysqluser' 
         -Password 'MySqlPassw0rd' 
         -ShardMapManagerServerName 'abcdefghij.database.windows.net'

   ```         
5. Bir bölme işlemi (yarı veri üzerinde ilk parça ikinci parçaya taşıma) ve ardından (geri taşıyarak ilk parça verileri) bir birleştirme işlemi yürütmek için ExecuteSampleSplitMerge.ps1 betiği yürütün. Yapılandırılmış SSL ve http uç noktası devre dışı kaldı, https:// uç nokta yerine kullandığınızdan emin olun.
   
   Örnek komut satırı:

   ```   
     .\ExecuteSampleSplitMerge.ps1
   
         -UserName 'mysqluser' 
         -Password 'MySqlPassw0rd' 
         -ShardMapManagerServerName 'abcdefghij.database.windows.net' 
         -SplitMergeServiceEndpoint 'https://mysplitmergeservice.cloudapp.net' 
         -CertificateThumbprint '0123456789abcdef0123456789abcdef01234567'
   ```      
   
   Alırsanız aşağıdaki hatayı, büyük olasılıkla Web uç noktasının sertifikayla ilgili bir sorun olduğu. Sık kullandığınız Web tarayıcınızı ile Web uç noktasına bağlanmayı deneyin ve bir sertifika hatası olup olmadığını denetleyin.
   
     ```
     Invoke-WebRequest : The underlying connection was closed: Could not establish trust relationship for the SSL/TLSsecure channel.
     ```
   
   Başarılı olursa çıktı gibi görünmelidir aşağıda:
   
   ```
   > .\ExecuteSampleSplitMerge.ps1 -UserName 'mysqluser' -Password 'MySqlPassw0rd' -ShardMapManagerServerName 'abcdefghij.database.windows.net' -SplitMergeServiceEndpoint 'http://mysplitmergeservice.cloudapp.net' -CertificateThumbprint 0123456789abcdef0123456789abcdef01234567
   > Sending split request
   > Began split operation with id dc68dfa0-e22b-4823-886a-9bdc903c80f3
   > Polling split-merge request status. Press Ctrl-C to end
   > Progress: 0% | Status: Queued | Details: [Informational] Queued request
   > Progress: 5% | Status: Starting | Details: [Informational] Starting split-merge state machine for request.
   > Progress: 5% | Status: Starting | Details: [Informational] Performing data consistency checks on target     shards.
   > Progress: 20% | Status: CopyingReferenceTables | Details: [Informational] Moving reference tables from     source to target shard.
   > Progress: 20% | Status: CopyingReferenceTables | Details: [Informational] Waiting for reference tables copy     completion.
   > Progress: 20% | Status: CopyingReferenceTables | Details: [Informational] Moving reference tables from     source to target shard.
   > Progress: 44% | Status: CopyingShardedTables | Details: [Informational] Moving key range [100:110) of     Sharded tables
   > Progress: 44% | Status: CopyingShardedTables | Details: [Informational] Successfully copied key range     [100:110) for table [dbo].[MyShardedTable]
   > ...
   > ...
   > Progress: 90% | Status: Completing | Details: [Informational] Successfully deleted shardlets in table     [dbo].[MyShardedTable].
   > Progress: 90% | Status: Completing | Details: [Informational] Deleting any temp tables that were created     while processing the request.
   > Progress: 100% | Status: Succeeded | Details: [Informational] Successfully processed request.
   > Sending merge request
   > Began merge operation with id 6ffc308f-d006-466b-b24e-857242ec5f66
   > Polling request status. Press Ctrl-C to end
   > Progress: 0% | Status: Queued | Details: [Informational] Queued request
   > Progress: 5% | Status: Starting | Details: [Informational] Starting split-merge state machine for request.
   > Progress: 5% | Status: Starting | Details: [Informational] Performing data consistency checks on target     shards.
   > Progress: 20% | Status: CopyingReferenceTables | Details: [Informational] Moving reference tables from     source to target shard.
   > Progress: 44% | Status: CopyingShardedTables | Details: [Informational] Moving key range [100:110) of     Sharded tables
   > Progress: 44% | Status: CopyingShardedTables | Details: [Informational] Successfully copied key range     [100:110) for table [dbo].[MyShardedTable]
   > ...
   > ...
   > Progress: 90% | Status: Completing | Details: [Informational] Successfully deleted shardlets in table     [dbo].[MyShardedTable].
   > Progress: 90% | Status: Completing | Details: [Informational] Deleting any temp tables that were created     while processing the request.
   > Progress: 100% | Status: Succeeded | Details: [Informational] Successfully processed request.
   > 
   ```
6. Diğer veri türleri ile denemeler yapın! Tüm bu betikler, anahtar türü belirtmenizi sağlayan isteğe bağlı - ShardKeyType parametresi getirin. Varsayılan, Int32 olmakla birlikte, Int64, GUID ya da ikili de belirtebilirsiniz.

## <a name="create-requests"></a>Oluşturma istekleri
Hizmet, web kullanıcı arabirimini kullanarak veya içe aktarılması ve kullanılması, web rolü isteklerini göndereceğiniz SplitMerge.psm1 PowerShell modülünü kullanılabilir.

Hizmet veri parçalı tabloları hem de başvuru tabloları taşıyabilirsiniz. Parçalı tablo parçalama anahtar sütunu ve her parça üzerinde farklı satır veri sahiptir. Başvuru tablosu, her parça aynı satır verileri içerecek şekilde parçalı değil. Başvuru tabloları genellikle değiştirmez ve sorgularda parçalı tablolarla katılmak için kullanılan veriler için kullanışlıdır.

Ayırma-birleştirme işlemi gerçekleştirmek için taşınmış istediğiniz başvuru tabloları ve parçalı tabloları bildirmeniz gerekir. Bu ile gerçekleştirilir **adet Schemaınfo** API. Bu API bulunduğu **Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.Schema** ad alanı.

1. Parçalı her tablo için oluşturun bir **ShardedTableInfo** tablonun üst şema adını tanımlayan nesne (isteğe bağlı, varsayılan "dbo" olarak), tablo adını ve parçalama anahtarı içeren bu tablodaki sütun adı.
2. Her başvuru tablosu için oluşturma bir **ReferenceTableInfo** tablonun ana şema adı açıklayan nesne (isteğe bağlı, varsayılan olarak "dbo") ve tablo adı.
3. Yukarıdaki TableInfo nesneleri yeni bir ekleme **adet Schemaınfo** nesne.
4. Bir başvuru almak bir **ShardMapManager** nesne ve çağrı **GetSchemaInfoCollection**.
5. Ekleme **adet Schemaınfo** için **SchemaInfoCollection**, parça eşleme adı sağlar.

Buna örnek olarak SetupSampleSplitMergeEnvironment.ps1 komut dosyasında görülebilir.

Ayırma-birleştirme hizmetini hedef veritabanının (veya veritabanındaki herhangi bir tablo için şema) sizin için oluşturmaz. Hizmete istek göndermeden önce önceden oluşturulmuş olmalıdır.

## <a name="troubleshooting"></a>Sorun giderme
Görebileceğiniz aşağıdaki örnek powershell betiklerini çalıştırırken, ileti:

   ```
   Invoke-WebRequest : The underlying connection was closed: Could not establish trust relationship for the SSL/TLS secure channel.
   ```

Bu hata, SSL sertifikası doğru yapılandırılmamış anlamına gelir. Lütfen "Bağlanıyor bir web tarayıcısına sahip" bölümündeki yönergeleri izleyin.

İstek gönderilemiyor bu görebilirsiniz:

```
[Exception] System.Data.SqlClient.SqlException (0x80131904): Could not find stored procedure 'dbo.InsertRequest'. 
```

Bu durumda, yapılandırma dosyanızı ayarını özellikle denetlemek **WorkerRoleSynchronizationStorageAccountConnectionString**. Çalışan rolü ilk kullanımda meta verileri veritabanı başarıyla başlatılamadı, bu hata genellikle gösterir. 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/allowed-services.png
[2]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/manage.png
[3]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/staging.png
[4]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/upload.png
[5]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/storage.png

