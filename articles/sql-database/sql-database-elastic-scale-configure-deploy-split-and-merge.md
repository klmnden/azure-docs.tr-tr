---
title: Bölünmüş birleştirme hizmet dağıtma | Microsoft Docs
description: Bölünmüş birleştirme çok parçalı veritabanları arasında veri taşımak için kullanın.
services: sql-database
author: stevestein
manager: craigg
ms.service: sql-database
ms.custom: scale out apps
ms.topic: article
ms.date: 04/01/2018
ms.author: sstein
ms.openlocfilehash: 90f758bf5bc979dc4bc173b08dadaceeaa077317
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="deploy-a-split-merge-service"></a>Ayırma-birleştirme hizmetini dağıtma
Bölünmüş Birleştirme aracı parçalı veritabanları arasında veri taşımanıza olanak tanır. Bkz: [ölçeklendirilmiş bulut veritabanları arasında veri taşıma](sql-database-elastic-scale-overview-split-and-merge.md)

## <a name="download-the-split-merge-packages"></a>Bölünmüş birleştirme paketlerini yükleyin
1. En son NuGet sürümü [NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).
2. Bir komut istemi açın ve nuget.exe indirdiğiniz dizine gidin. İndirme PowerShell komutlarını içerir.
3. Geçerli dizine en son bölünmüş birleştirme paketini indirin komutu altında:
   ```
   nuget install Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge
   ```  

Dosyalar adlı bir dizinde yerleştirilir **Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge.x.x.xxx.x** nerede *x.x.xxx.x* sürüm numarasını yansıtır. Bölünmüş birleştirme hizmet dosyalarda Bul **content\splitmerge\service** alt dizini ve bölünmüş birleştirme PowerShell komut dosyaları (ve gerekli istemci DLL'ler) **content\splitmerge\powershell** alt dizin.

## <a name="prerequisites"></a>Önkoşullar
1. Bölünmüş birleştirme durumu veritabanı olarak kullanılacak bir Azure SQL DB veritabanı oluşturun. [Azure Portal](https://portal.azure.com) gidin. Yeni bir **SQL veritabanı**. Veritabanı bir ad verin ve yeni yönetici ve parola oluşturun. Adı ve daha sonra kullanmak için parolayı kaydettiğinizden emin olun.
2. Azure SQL DB sunucunuza bağlanmak Azure Services verdiğinden emin olun. Portalda, içinde **güvenlik duvarı ayarlarını**, olun **Azure hizmetlerine erişime izin ver** ayar **üzerinde**. "Kaydet" simgesine tıklayın.
   
   ![İzin verilen hizmetler][1]
3. Tanılama çıktı için kullanılacak bir Azure depolama hesabı oluşturun. Azure portalına gidin. Sol çubuğunda **kaynak oluşturma**, tıklatın **veri + depolama**, ardından **depolama**.
4. Bölünmüş birleştirme hizmetinizi içerecek bir Azure bulut hizmeti oluşturun.  Azure portalına gidin. Sol çubuğunda **kaynak oluşturma**, ardından **işlem**, **bulut hizmeti**, ve **oluşturma**. 

## <a name="configure-your-split-merge-service"></a>Bölünmüş birleştirme hizmetini yapılandırma
### <a name="split-merge-service-configuration"></a>Bölünmüş birleştirme hizmet yapılandırması
1. Bölünmüş birleştirme derlemeler indirdiğiniz klasörde bir kopyasını oluşturmak **ServiceConfiguration.Template.cscfg** yanında sevk dosya **SplitMergeService.cspkg** ve yeniden adlandırın **ServiceConfiguration.cscfg**.
2. Açık **ServiceConfiguration.cscfg** sertifika parmak izlerini biçimi gibi girişleri doğrular Visual Studio gibi bir metin düzenleyicisinde.
3. Yeni bir veritabanı oluşturmak veya var olan bir veritabanını bölünmüş birleştirme işlemleri için durum veritabanı olarak hizmet ve veritabanı bağlantı dizesini almak için seçin. 
   
   > [!IMPORTANT]
   > Şu anda durumu veritabanı Latin harmanlamayı kullanması gerekir (SQL\_Latin1\_genel\_CP1\_CI\_AS). Daha fazla bilgi için bkz: [Windows harmanlama adı (Transact-SQL)](https://msdn.microsoft.com/library/ms188046.aspx).
   >

   Azure SQL DB ile bağlantı dizesi genellikle şu şekildedir:
      ```
      Server=myservername.database.windows.net; Database=mydatabasename;User ID=myuserID; Password=mypassword; Encrypt=True; Connection Timeout=30
      ```

4. Cscfg dosyası hem de bu bağlantı dizesi girin **SplitMergeWeb** ve **SplitMergeWorker** ElasticScaleMetadata ayarı rol bölümlerde.
5. İçin **SplitMergeWorker** rolü, Azure depolama için geçerli bir bağlantı dizesi girin **WorkerRoleSynchronizationStorageAccountConnectionString** ayarı.

### <a name="configure-security"></a>Güvenliği yapılandırma
Hizmet güvenliği yapılandırmak ayrıntılı yönergeler için bkz [bölünmüş birleştirme Güvenlik Yapılandırması](sql-database-elastic-scale-split-merge-security-configuration.md).

Basit bir sınama dağıtım Bu öğreticinin amaçları doğrultusunda yapılandırma adımları en az bir dizi hizmet alınacağı gerçekleştirilen ve çalışan olacaktır. Bu adımları yalnızca bir makine/bunları hizmetiyle iletişim kurmak için yürütme hesabını etkinleştirin.

### <a name="create-a-self-signed-certificate"></a>Otomatik olarak imzalanan sertifika oluşturma
Yeni bir dizin oluşturun ve aşağıdaki komutu kullanarak bu dizininden yürütme bir [Visual Studio için geliştirici komut istemi](http://msdn.microsoft.com/library/ms229859.aspx) penceresi:

   ```
    makecert ^
    -n "CN=*.cloudapp.net" ^
    -r -cy end -sky exchange -eku "1.3.6.1.5.5.7.3.1,1.3.6.1.5.5.7.3.2" ^
    -a sha1 -len 2048 ^
    -sr currentuser -ss root ^
    -sv MyCert.pvk MyCert.cer
   ```

Özel anahtarını korumak için bir parola sorulur. Güçlü bir parola girin ve onaylayın. Ardından, bir kez daha sonra kullanılmak üzere parola istenir. Tıklatın **Evet** sonunda güvenilen sertifika yetkilileri kök deposuna aktarın.

### <a name="create-a-pfx-file"></a>Bir PFX dosyası oluşturma
Makecert yürütüldüğü penceresinden aşağıdaki komutu yürütün; bir sertifika oluşturmak için kullanılan parolanın aynısını kullanın:

    pvk2pfx -pvk MyCert.pvk -spc MyCert.cer -pfx MyCert.pfx -pi <password>

### <a name="import-the-client-certificate-into-the-personal-store"></a>İstemci sertifikası Kişisel depoya içeri aktarma
1. Windows Gezgini'nde çift tıklayarak **mycert.pfx'i**.
2. İçinde **Sertifika Alma Sihirbazı'nı** seçin **geçerli kullanıcı** tıklatıp **sonraki**.
3. Dosya yolunu doğrulayın ve tıklatın **sonraki**.
4. Parolayı yazın, bırakın **dahil tüm genişletilmiş özellikleri** denetlenir ve tıklatın **sonraki**.
5. Bırakın **[...] sertifika deposunu otomatik olarak Seç**  denetlenir ve tıklatın **sonraki**.
6. Tıklatın **son** ve **Tamam**.

### <a name="upload-the-pfx-file-to-the-cloud-service"></a>Bulut hizmetine PFX dosyasını karşıya yükle
1. [Azure Portal](https://portal.azure.com) gidin.
2. Seçin **bulut hizmetlerini**.
3. Bölünmüş/Merge hizmeti için yukarıda oluşturduğunuz bulut hizmeti seçin.
4. Tıklatın **sertifikaları** üst menüsünde.
5. Tıklatın **karşıya** alt çubuğunda.
6. PFX dosyasını seçin ve yukarıdaki aynı parolayı girin.
7. Tamamlandığında, sertifika parmak izini listede yeni girdiyi kopyalayın.

### <a name="update-the-service-configuration-file"></a>Hizmet yapılandırma dosyasını güncelleştir
Yukarıdaki bu ayarları parmak izi/value özniteliği kopyaladığınız sertifika parmak izi yapıştırın.
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

Lütfen üretim için sertifikaları dağıtımları ayrı not şifreleme, sunucu sertifikası ve istemci sertifikaları için CA için kullanılmalıdır. Bu ayrıntılı yönergeler için bkz: [Güvenlik Yapılandırması](sql-database-elastic-scale-split-merge-security-configuration.md).

## <a name="deploy-your-service"></a>Hizmet dağıtma
1. [Azure Portal](https://manage.windowsazure.com) gidin.
2. Tıklatın **bulut Hizmetleri** sol tarafta sekmesini tıklatın ve daha önce oluşturduğunuz bulut hizmeti seçin.
3. Tıklatın **Pano**.
4. Hazırlama ortamı seçin ve ardından **yeni bir hazırlama dağıtımı karşıya**.
   
   ![Hazırlanıyor][3]
5. İletişim kutusunda, bir dağıtım etiketini girin. 'Paketi' ve 'Configuration' için 'Den yerel' tıklatın ve seçin **SplitMergeService.cspkg** dosyası ve daha önce yapılandırılmış, cscfg dosyası.
6. Etiketli onay kutusunu emin **bir veya daha fazla rol tek bir örnek içeriyorsa bile Dağıt** denetlenir.
7. Dağıtımına başlamak için sayfanın sağ onay düğmesine basın. Tamamlanması birkaç dakika olması için bekler.

   ![Karşıya Yükle][4]

## <a name="troubleshoot-the-deployment"></a>Dağıtım sorunlarını gider
Çevrimiçi olması web rolünüz başarısız olursa, güvenlik yapılandırması ile ilgili bir sorun büyük olasılıkla olur. SSL yukarıda açıklandığı şekilde yapılandırıldığından emin olun.

Çevrimiçi olması, çalışan rolü başarısız olur, ancak web rolünüz başarılı olur, bu büyük olasılıkla daha önce oluşturduğunuz durum veritabanına bağlanırken bir sorundur.

* Cscfg bağlantı dizesinin doğru olduğundan emin olun.
* Sunucu ve veritabanı var olduğundan ve kullanıcı kimliği ve parola doğru olduğunu denetleyin.
* Azure SQL veritabanı için bağlantı dizesi biçiminde olmalıdır:

   ```  
   Server=myservername.database.windows.net; Database=mydatabasename;User ID=myuserID; Password=mypassword; Encrypt=True; Connection Timeout=30
   ```

* Sunucu adı ile başlamayan olun **https://**.
* Azure SQL DB sunucunuza bağlanmak Azure Services verdiğinden emin olun. Bunu yapmak için açık https://manage.windowsazure.com, sol taraftaki "SQL veritabanlarını"'yi tıklatın, en üstünde "Sunucuları"'i tıklatın ve sunucunuzu seçin. Tıklatın **yapılandırma** üst ve emin olun **Azure Hizmetleri** ayar "Evet" olarak ayarlayın. (Bu makalenin üst Önkoşullar bölümüne bakın).

## <a name="test-the-service-deployment"></a>Hizmet dağıtımı test etme
### <a name="connect-with-a-web-browser"></a>Bir web tarayıcısı ile bağlanma
Web uç noktası bölünmüş birleştirme hizmetinizin belirler. Giderek bu Azure Klasik Portalı'nda bulabilirsiniz **Pano** , bulut hizmeti ve altında arayan **Site URL'si** sağ tarafında. Değiştir **http://** ile **https://** varsayılan güvenlik ayarlarını HTTP uç noktası devre dışı olduğundan. Sayfa bu URL'sini tarayıcınıza yükleyin.

### <a name="test-with-powershell-scripts"></a>PowerShell komut dosyalarıyla test
Dağıtım ve ortamınıza dahil edilmiş örnek PowerShell komut dosyaları çalıştırarak test edilebilir.

Bulunan komut dosyaları şunlardır:

1. **SetupSampleSplitMergeEnvironment.ps1** - sets up a test data tier for Split/Merge (see table below for detailed description)
2. **ExecuteSampleSplitMerge.ps1** -test test işlemlerini yürüten veri katmanı (ayrıntılı bir açıklaması için aşağıdaki tabloya bakın)
3. **GetMappings.ps1** - üst düzey örnek parça eşlemeleri geçerli durumunu yazdırır komut dosyası.
4. **ShardManagement.psm1** -ShardManagement API sarmalar yardımcı komut dosyası
5. **SqlDatabaseHelpers.psm1** -Yardımcısı betik oluşturma ve SQL veritabanlarını yönetme
   
   <table style="width:100%">
     <tr>
       <th>PowerShell dosyası</th>
       <th>Adımlar</th>
     </tr>
     <tr>
       <th rowspan="5">SetupSampleSplitMergeEnvironment.ps1</th>
       <td>1.    Bir parça eşleme Yöneticisi veritabanı oluşturur</td>
     </tr>
     <tr>
       <td>2.    2 parça veritabanları oluşturur.
     </tr>
     <tr>
       <td>3.    Bu veritabanları (varolan bir parça bu veritabanlarını eşlemeleri siler) için bir parça eşleme oluşturur. </td>
     </tr>
     <tr>
       <td>4.    Her iki parça küçük bir örnek bir tablo oluşturur ve parça tabloda doldurur.</td>
     </tr>
     <tr>
       <td>5.    Parçalı tablo için SchemaInfo bildirir.</td>
     </tr>
   </table>
   <table style="width:100%">
     <tr>
       <th>PowerShell dosyası</th>
       <th>Adımlar</th>
     </tr>
   <tr>
       <th rowspan="4">ExecuteSampleSplitMerge.ps1 </th>
       <td>1.    Yarısı ilk parça verilerden ikinci parça için böler bölünmüş birleştirme hizmetine web ön uç bir bölme isteği gönderir.</td>
     </tr>
     <tr>
       <td>2.    Bölünmüş istek durumu için web ön uç yoklar ve istek tamamlanana kadar bekler.</td>
     </tr>
     <tr>
       <td>3.    Hangi veri ikinci parça ilk parça geri taşınır bölünmüş birleştirme hizmetine web ön uç için birleştirme isteği gönderir.</td>
     </tr>
     <tr>
       <td>4.    Birleştirme isteği durumu için web ön uç yoklar ve istek tamamlanana kadar bekler.</td>
     </tr>
   </table>
   
## <a name="use-powershell-to-verify-your-deployment"></a>Dağıtımınızı doğrulamak için PowerShell kullanın
1. Yeni bir PowerShell penceresi açın ve bölünmüş birleştirme paketi indirdiğiniz dizine gidin ve ardından "powershell" dizinine gidin.
2. Bir Azure SQL database sunucusu oluşturun (veya var olan bir sunucu seçin) burada parça eşleme Yöneticisi ve parça oluşturulur.
   
   > [!NOTE]
   > The SetupSampleSplitMergeEnvironment.ps1 script creates all these databases on the same server by default to keep the script simple. Bu bir kısıtlama bölünmüş birleştirme hizmetinin kendisini değil.
   >
   
   DBs okuma/yazma erişimi olan bir SQL kimlik doğrulaması oturum açma verilerini taşımak ve parça eşleme güncelleştirmek bölünmüş birleştirme hizmeti için gerekir. Bölünmüş birleştirme hizmetine bulutta çalışan olduğundan, tümleşik kimlik doğrulaması şu anda desteklemiyor.
   
   Azure SQL server bu komut dosyası çalıştırarak makinenin IP adresinden erişime izin verecek şekilde yapılandırıldığından emin olun. Bu ayar Azure SQL sunucusu altında bulabilirsiniz / configuration / izin verilen IP adreslerini.
3. Örnek ortamı oluşturmak için SetupSampleSplitMergeEnvironment.ps1 betiğini yürütün.
   
   Bu komut dosyasını çalıştıran var olan tüm parça eşleme yönetim verilerini ve parça parça eşleme Yöneticisi veritabanı yapıları temizleyin. Parça Bağlantıları'nı veya parça yeniden başlatmak istiyorsanız, komut dosyasını yeniden çalıştırmak yararlı olabilir.
   
   Örnek komut satırı:

   ```   
     .\SetupSampleSplitMergeEnvironment.ps1 
   
         -UserName 'mysqluser' 
         -Password 'MySqlPassw0rd' 
         -ShardMapManagerServerName 'abcdefghij.database.windows.net'
   ```      
4. Şu anda mevcut eşlemeleri örnek ortamında görüntülemek için Getmappings.ps1 betiğini yürütün.
   
   ```
     .\GetMappings.ps1 
   
         -UserName 'mysqluser' 
         -Password 'MySqlPassw0rd' 
         -ShardMapManagerServerName 'abcdefghij.database.windows.net'

   ```         
5. Bir bölme işlemi (yarım verileri üzerinde ilk parça ikinci parça taşıma) ve (verileri ilk parça geri taşıma) bir birleştirme işlemi yürütmek için ExecuteSampleSplitMerge.ps1 betiğini yürütün. SSL yapılandırılmış ve devre dışı http uç noktası sol, https:// uç noktası yerine kullandığınızdan emin olun.
   
   Örnek komut satırı:

   ```   
     .\ExecuteSampleSplitMerge.ps1
   
         -UserName 'mysqluser' 
         -Password 'MySqlPassw0rd' 
         -ShardMapManagerServerName 'abcdefghij.database.windows.net' 
         -SplitMergeServiceEndpoint 'https://mysplitmergeservice.cloudapp.net' 
         -CertificateThumbprint '0123456789abcdef0123456789abcdef01234567'
   ```      
   
   Alırsanız,, bu büyük olasılıkla Web uç noktanın sertifikayla ilgili bir sorun hatasıdır. Sık kullanılan Web tarayıcınızın Web noktayla bağlanmayı deneyin ve bir sertifika hatası olup olmadığını denetleyin.
   
     ```
     Invoke-WebRequest : The underlying connection was closed: Could not establish trust relationship for the SSL/TLSsecure channel.
     ```
   
   Başarılıysa, çıktı gibi görünmelidir altında:
   
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
6. Diğer veri türleri ile deneyin! Bu komut dosyalarının tümü anahtar türü belirtmenize olanak sağlayan isteğe bağlı - ShardKeyType parametresi alın. Varsayılan, Int32 olmakla birlikte, Int64, GUID ya da ikili de belirtebilirsiniz.

## <a name="create-requests"></a>Oluşturma istekleri
Hizmet, web kullanıcı arabirimini kullanarak veya içeri aktarma ve hangi web rolü aracılığıyla, istekleri gönderir SplitMerge.psm1 PowerShell modülünü kullanarak kullanılabilir.

Hizmet veri parçalı tablolar ve başvuru tabloları taşıyabilirsiniz. Parçalı tablo parçalama anahtar sütunu ve her parça üzerinde farklı satır veri sahiptir. Bir başvuru tablosu parçalı olmadığından her parça aynı satır verilerini içerir. Başvuru tabloları genellikle değiştirmez ve sorgularda parçalı tablolarla birleştirme için kullanılacak veriler için kullanışlıdır.

Bölünmüş birleştirme işlemi gerçekleştirmek için taşınmış istediğiniz başvuru tabloları ve parçalı tabloları bildirmeniz gerekir. Bu ile gerçekleştirilir **SchemaInfo** API. Bu API bulunduğu **Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.Schema** ad alanı.

1. Her parçalı tablo için oluşturun bir **ShardedTableInfo** tablonun üst şema adını tanımlayan nesne (isteğe bağlı, varsayılan olarak "dbo"), tablo adını ve parçalama anahtarı içeren bu tablodaki sütun adı.
2. Her başvuru tablosu için oluşturmak bir **ReferenceTableInfo** tablonun üst şema adını tanımlayan nesne (isteğe bağlı, varsayılan olarak "dbo") ve tablo adı.
3. Yukarıdaki TableInfo nesne yeni bir eklemek **SchemaInfo** nesnesi.
4. Bir başvuru almak bir **ShardMapManager** nesne ve çağrı **GetSchemaInfoCollection**.
5. Ekleme **SchemaInfo** için **SchemaInfoCollection**, parça eşleme adı sağlama.

Bunun bir örneğini SetupSampleSplitMergeEnvironment.ps1 komut dosyasında görülebilir.

Bölünmüş birleştirme hizmetine hedef veritabanı (veya veritabanındaki herhangi bir tablo için şema), oluşturmaz. Bunların hizmetine bir istek göndermeden önce önceden oluşturulmuş olması gerekir.

## <a name="troubleshooting"></a>Sorun giderme
Görebileceğiniz Aşağıda örnek powershell komut dosyası çalıştırarak iletisi:

   ```
   Invoke-WebRequest : The underlying connection was closed: Could not establish trust relationship for the SSL/TLS secure channel.
   ```

Bu hata, SSL sertifikası doğru yapılandırılmamış anlamına gelir. Lütfen 'Bağlanıyor bir web tarayıcısı' bölümünde yer alan yönergeleri izleyin.

İstek gönderemez varsa bu görebilirsiniz:

```
[Exception] System.Data.SqlClient.SqlException (0x80131904): Could not find stored procedure 'dbo.InsertRequest'. 
```

Bu durumda, yapılandırma dosyanızı ayarını özellikle denetlemek **WorkerRoleSynchronizationStorageAccountConnectionString**. Bu hata genellikle çalışan rolü başarıyla ilk kullanımda meta veri veritabanı başlatılamadı gösterir. 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/allowed-services.png
[2]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/manage.png
[3]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/staging.png
[4]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/upload.png
[5]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/storage.png

