---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 0b5d9deacdd4266da30f17c95b6e575a652d2f76
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188413"
---
Bu yordamda şunları yapacaksınız:

1. [Bakımcı yürütülebilir dosyayı çalıştırmak üzere hazırlayalım](#to-prepare-to-run-the-maintainer) .
2. [Geri Dönüşüm Kutusu ve içerik veritabanını hemen yalnız bırakılmış Blobları silme için hazırlama](#to-prepare-the-content-database-and-recycle-bin-to-immediately-delete-orphaned-blobs).
3. [Maintainer.exe çalıştırma](#to-run-the-maintainer).
4. [Geri Dönüşüm Kutusu ayarlarını ve içerik veritabanını geri](#to-revert-the-content-database-and-recycle-bin-settings).

#### <a name="to-prepare-to-run-the-maintainer"></a>Bakımcı çalıştırmaya hazırlamak için
1. Ön uç Web sunucusunda, yönetici olarak SharePoint 2013 Yönetim Kabuğu'nu açın.
2. Klasöre gidin *önyükleme sürücüsü*: \Program SQL Uzak Blob Depolama 10.50\Maintainer\.
3. Yeniden adlandırma **Microsoft.Data.SqlRemoteBlobs.Maintainer.exe.config** için **web.config**.
4. Kullanım `aspnet_regiis -pdf connectionStrings` web.config dosyasının şifresini çözmek için.
5. Şifresi çözülmüş web.config dosyasında altında `connectionStrings` düğümü, SQL server örneğinizi ve içerik veritabanı adı için bağlantı dizesi ekleyin. Aşağıdaki örneğe bakın.
   
    `<add name=”RBSMaintainerConnectionWSSContent” connectionString="Data Source=SHRPT13-SQL12\SHRPT13;Initial Catalog=WSS_Content;Integrated Security=True;Application Name=&quot;Remote Blob Storage Maintainer for WSS_Content&quot;" providerName="System.Data.SqlClient" />`
6. Kullanım `aspnet_regiis –pef connectionStrings` web.config dosyasını yeniden şifrelenemedi. 
7. Web.config Microsoft.Data.SqlRemoteBlobs.Maintainer.exe.config için yeniden adlandırın. 

#### <a name="to-prepare-the-content-database-and-recycle-bin-to-immediately-delete-orphaned-blobs"></a>İçerik Hazırlama için veritabanı ve hemen silmek için Geri Dönüşüm Kutusu'nu Blobları yalnız bırakılmış
1. SQL Server'da, SQL Management Studio'da, hedef içerik veritabanı için aşağıdaki güncelleştirme sorguları çalıştırın: 
   
       `use WSS_Content`
   
       `exec mssqlrbs.rbs_sp_set_config_value ‘garbage_collection_time_window’ , ’time 00:00:00’`
   
       `exec mssqlrbs.rbs_sp_set_config_value ‘delete_scan_period’ , ’time 00:00:00’`
2. Web ön uç sunucusunda altında **Merkezi Yönetim**, Düzen **Web uygulaması genel ayarları** istenen içerik veritabanı geçici olarak geri dönüşüm kutusu devre dışı bırakmak. İlgili tüm site koleksiyonları için bu eylem ayrıca geri dönüşüm kutusu boş. Bunu yapmak için tıklatın **Merkezi Yönetim** -> **Uygulama Yönetimi** -> **Web uygulamaları (web uygulamalarını yönet)**  ->  **SharePoint - 80** -> **genel uygulama ayarları**. Ayarlama **Geri Dönüşüm Kutusu'nu durumu** için **OFF**.
   
    ![Web uygulaması genel ayarları](./media/storsimple-sharepoint-adapter-garbage-collection/HCS_WebApplicationGeneralSettings-include.png)

#### <a name="to-run-the-maintainer"></a>Bakımcı çalıştırmak için
* Ön uç web sunucusunda, SharePoint 2013 Yönetim Kabuğu'nda Bakımcı şu şekilde çalıştırın:
  
      `Microsoft.Data.SqlRemoteBlobs.Maintainer.exe -ConnectionStringName RBSMaintainerConnectionWSSContent -Operation GarbageCollection -GarbageCollectionPhases rdo`
  
  > [!NOTE]
  > Yalnızca `GarbageCollection` işlemi, şu anda StorSimple için desteklenir. Ayrıca Microsoft.Data.SqlRemoteBlobs.Maintainer.exe için verilen parametreleri büyük küçük harfe duyarlı olduğunu unutmayın. 
  > 
  > 

#### <a name="to-revert-the-content-database-and-recycle-bin-settings"></a>Geri Dönüşüm Kutusu ayarlarını ve içerik veritabanını geri almak için
1. SQL Server'da, SQL Management Studio'da, hedef içerik veritabanı için aşağıdaki güncelleştirme sorguları çalıştırın:
   
      `use WSS_Content`
   
      `exec mssqlrbs.rbs_sp_set_config_value ‘garbage_collection_time_window’ , ‘days 30’`
   
      `exec mssqlrbs.rbs_sp_set_config_value ‘delete_scan_period’ , ’days 30’`
   
      `exec mssqlrbs.rbs_sp_set_config_value ‘orphan_scan_period’ , ’days 30’`
2. Web ön uç sunucusunda, **Merkezi Yönetim**, Düzenle **Web uygulaması genel ayarları** yeniden Geri Dönüşüm Kutusu'nu etkinleştirmek istediğiniz içerik veritabanı. Bunu yapmak için tıklatın **Merkezi Yönetim** -> **Uygulama Yönetimi** -> **Web uygulamaları (web uygulamalarını yönet)**  ->  **SharePoint - 80** -> **genel uygulama ayarları**. Geri Dönüşüm Kutusu'nu durumu kümesine **ON**.

