<!--author=SharS last changed: 9/17/15-->

Bu yordamda şunları yapacaksınız:

1. [Bakımcı yürütülebilir dosyayı çalıştırmak için hazırlık](#to-prepare-to-run-the-maintainer) .
2. [Geri Dönüşüm Kutusu ve içerik veritabanını yalnız bırakılmış BLOB'lar anında silme işlemi için hazırlama](#to-prepare-the-content-database-and-recycle-bin-to-immediately-delete-orphaned-blobs).
3. [Maintainer.exe Çalıştır](#to-run-the-maintainer).
4. [Geri Dönüşüm Kutusu ayarlarını ve içerik veritabanını geri](#to-revert-the-content-database-and-recycle-bin-settings).

#### <a name="to-prepare-to-run-the-maintainer"></a>Bakımcı çalıştırmak hazırlamak için
1. Web ön uç sunucusunda, SharePoint 2013 Yönetim Kabuğu'nu bir yönetici olarak açın.
2. Klasöre gidin *önyükleme sürücüsü*: \Program SQL Uzak Blob Depolama 10.50\Maintainer\.
3. Yeniden Adlandır **Microsoft.Data.SqlRemoteBlobs.Maintainer.exe.config** için **web.config**.
4. Kullanım `aspnet_regiis -pdf connectionStrings` web.config dosyasının şifresini çözmek için.
5. Şifresi çözülmüş web.config dosyasında altında `connectionStrings` düğümü, SQL server örneğinizi ve içerik veritabanı adı için bağlantı dizesini ekleyin. Aşağıdaki örneğe bakın.
   
    `<add name=”RBSMaintainerConnectionWSSContent” connectionString="Data Source=SHRPT13-SQL12\SHRPT13;Initial Catalog=WSS_Content;Integrated Security=True;Application Name=&quot;Remote Blob Storage Maintainer for WSS_Content&quot;" providerName="System.Data.SqlClient" />`
6. Kullanım `aspnet_regiis –pef connectionStrings` web.config dosyasını yeniden şifrelemek için. 
7. Web.config Microsoft.Data.SqlRemoteBlobs.Maintainer.exe.config için yeniden adlandırın. 

#### <a name="to-prepare-the-content-database-and-recycle-bin-to-immediately-delete-orphaned-blobs"></a>İçerik Hazırlama için veritabanı ve hemen silmek için Geri Dönüşüm Kutusu'nu BLOB'lar yalnız bırakılmış
1. SQL Server'da aşağıdaki güncelleştirme sorguları hedef içerik veritabanı için SQL Management Studio'da çalıştırın: 
   
       `use WSS_Content`
   
       `exec mssqlrbs.rbs_sp_set_config_value ‘garbage_collection_time_window’ , ’time 00:00:00’`
   
       `exec mssqlrbs.rbs_sp_set_config_value ‘delete_scan_period’ , ’time 00:00:00’`
2. Web ön uç sunucusunda altında **Merkezi Yönetim**, düzenleme **Web uygulaması genel ayarları** istenen içerik veritabanı geçici olarak geri dönüşüm kutusu devre dışı bırakmak için. Tüm ilgili site koleksiyonları için bu eylem ayrıca geri dönüşüm kutusu boş. Bunu yapmak için tıklatın **Merkezi Yönetim** -> **Uygulama Yönetimi** -> **Web uygulamaları (web uygulamalarını yönet)**  ->  **SharePoint - 80** -> **genel uygulama ayarları**. Ayarlama **Geri Dönüşüm Kutusu'nu durumu** için **OFF**.
   
    ![Web uygulaması genel ayarları](./media/storsimple-sharepoint-adapter-garbage-collection/HCS_WebApplicationGeneralSettings-include.png)

#### <a name="to-run-the-maintainer"></a>Bakımcı çalıştırmak için
* Web ön uç sunucusunda, SharePoint 2013 Yönetim Kabuğu'nda aşağıdaki gibi Bakımcı çalıştırın:
  
      `Microsoft.Data.SqlRemoteBlobs.Maintainer.exe -ConnectionStringName RBSMaintainerConnectionWSSContent -Operation GarbageCollection -GarbageCollectionPhases rdo`
  
  > [!NOTE]
  > Yalnızca `GarbageCollection` işlemi şu anda StorSimple için desteklenir. Ayrıca Microsoft.Data.SqlRemoteBlobs.Maintainer.exe için verilen parametreleri büyük küçük harfe duyarlı olduğunu unutmayın. 
  > 
  > 

#### <a name="to-revert-the-content-database-and-recycle-bin-settings"></a>Geri Dönüşüm Kutusu ayarlarını ve içerik veritabanını geri almak için
1. SQL Server'da aşağıdaki güncelleştirme sorguları hedef içerik veritabanı için SQL Management Studio'da çalıştırın:
   
      `use WSS_Content`
   
      `exec mssqlrbs.rbs_sp_set_config_value ‘garbage_collection_time_window’ , ‘days 30’`
   
      `exec mssqlrbs.rbs_sp_set_config_value ‘delete_scan_period’ , ’days 30’`
   
      `exec mssqlrbs.rbs_sp_set_config_value ‘orphan_scan_period’ , ’days 30’`
2. Web ön uç sunucusunda, **Merkezi Yönetim**, düzenleme **Web uygulaması genel ayarları** Geri Dönüşüm Kutusu'nu yeniden etkinleştirmek istediğiniz içerik veritabanı için. Bunu yapmak için tıklatın **Merkezi Yönetim** -> **Uygulama Yönetimi** -> **Web uygulamaları (web uygulamalarını yönet)**  ->  **SharePoint - 80** -> **genel uygulama ayarları**. Geri Dönüşüm Kutusu'nu durum kümesine **ON**.

