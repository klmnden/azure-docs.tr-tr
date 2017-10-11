---
title: "Veri kaynakları Azure veri Kataloğu'nda desteklenen | Microsoft Docs"
description: "Bu makalede belirtimleri şu anda desteklenen veri kaynakları listeler."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: jstevens
editor: 
tags: 
ms.assetid: fd4345ca-2ed8-4c5e-9c4b-f954be2fc9f9
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: d6867c73bc6ea3c238cceef8a68466d451f3365c
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2017
---
# <a name="supported-data-sources-in-azure-data-catalog"></a>Azure veri Kataloğu desteklenen veri kaynakları

Bir tıklayın veya ortak bir API kullanarak meta verileri yayımlayabilirsiniz-kez kayıt aracı veya bilgileri el ile doğrudan Azure veri Kataloğu'na girerek web portalı. Aşağıdaki tabloda katalog Bugün ve yayımlama özellikleri tarafından her biri için desteklenen tüm veri kaynakları özetler. Ayrıca her veri kaynağı bizim "birlikte açma" portal deneyimlerden başlatabilirsiniz dış veri araçları listelenmiştir. İkinci tablonun her veri kaynağı bağlantı özelliği daha fazla teknik belirtimini içerir.


## <a name="list-of-supported-data-sources"></a>Desteklenen veri kaynaklarının listesi

<table>
    <tr>
       <td><b>Veri kaynağı nesnesi</b></td>
       <td><b>API</b></td>
       <td><b>El ile giriş</b></td>
       <td><b>Kayıt Aracı</b></td>
       <td><b>Birlikte açma araçları</b></td>
       <td><b>Notlar</b></td>
    </tr>
    <tr>
      <td>Azure Data Lake Store dizini</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Azure Data Lake Store dosya</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Azure Blob depolama</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Power BI</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Azure depolama dizini</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Power BI</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Azure depolama tablosu</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td>
        <font size="2"></font>
      </td>
      <td>
        <font size="2"></font>
      </td>
    </tr>
    <tr>
      <td>HDFS dizini</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>HDFS dosyası</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Hive tablosu</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Hive görünümü</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>MySQL tablosu</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, Power BI</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>MySQL görünümü</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, Power BI</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Oracle veritabanı tablosu</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, Power BI</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Oracle veritabanı görünümü</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, Power BI</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Diğer (genel varlık)</td>
      <td>✓</td>
      <td>✓</td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Azure SQL Data Warehouse tablo</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, Power BI, SQL Server veri araçları</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>SQL veri ambarı görünümü</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, Power BI, SQL Server veri araçları</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services boyut</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, Power BI</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services KPI</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, Power BI</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services ölçü</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, Power BI</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services tablo</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, Power BI</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>SQL Server Reporting Services rapor</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Tarayıcı</font></td>
      <td><font size=2>Yalnızca yerel mod sunucuları. SharePoint modu desteklenmiyor.</font></td>
    </tr>
    <tr>
      <td>SQL Server tablosu</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, Power BI, SQL Server veri araçları</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>SQL Server görünümü</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, Power BI, SQL Server veri araçları</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Teradata tablosu</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Teradata görünümü</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>SAP HANA görünümü</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Power BI</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>DB2 tablosu</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>DB2 görünümü</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Dosya sistemi dosyasına</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>FTP dizin</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>FTP dosyası</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>HTTP raporu</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>HTTP uç noktası</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>HTTP dosya</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>OData varlık kümesi</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>OData işlevi</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>PostgreSQL tablosu</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>PostgreSQL görünümü</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>SAP HANA görünümü</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td> Salesforce nesnesi</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>SharePoint listesi </td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Azure Cosmos DB koleksiyonu</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Genel ODBC tablosu</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Genel ODBC görünümü</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Cassandra tablosu</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2>Genel bir ODBC varlık Yayımla</font></td>
    </tr>
    <tr>
      <td>Cassandra görünümü</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2>Genel bir ODBC varlık Yayımla</font></td>
    </tr>
    <tr>
      <td>Sybase tablosu</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Sybase görünümü</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>MongoDB tablosu</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2>Genel bir ODBC varlık Yayımla</font></td>
    </tr>
    <tr>
      <td>MongoDB görünümü</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2>Genel bir ODBC varlık Yayımla</font></td>
    </tr>
</table>

Ek kaynaklar için destek gerekiyorsa, bir özellik isteği gönderin [Azure veri Kataloğu Forumu](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409).


## <a name="data-source-reference-specification"></a>Veri kaynağı başvurusu belirtimi
> [!NOTE]
> **DSL yapısı** sütunu aşağıdaki tabloda, Azure veri Kataloğu tarafından kullanılan bağlantı özellikleri "Adres" özellik paketi için listeler. Diğer bir deyişle, "Adres" özellik paketi diğer Azure veri Kataloğu devam ederse, ancak kullanmayan veri kaynağının bağlantı özelliklerini içerebilir.

<table>
    <tr>
       <td><b>Kaynak türü</b></td>
       <td><b>Varlık türü</b></td>
       <td><b>Nesne türleri</b></td>
       <td><b>DSL yapısı<b></td>
    </tr>
    <tr>
      <td>Azure Data Lake Store</td>
      <td>Kapsayıcı</td>
      <td>Data Lake</td>
      <td>
        <font size=2>Protokol: webhdfs <br>Kimlik doğrulaması: {temel, oauth} <br>Adresi: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>Azure Data Lake Store</td>
      <td>Tablo</td>
      <td>Dizin, dosya</td>
      <td>
        <font size=2>Protokol: webhdfs <br>Kimlik doğrulaması: {temel, oauth} <br>Adresi: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>Azure Storage</td>
      <td>Kapsayıcı</td>
      <td>Kapsayıcı</td>
      <td>
        <font size=2>Protokol: azure BLOB sayısı <br>Kimlik doğrulaması: {azure erişim tuşu} <br>Adresi: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;etki alanı <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;hesabı <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;kapsayıcı</font>
      </td>
    </tr>
    <tr>
      <td>Azure Storage</td>
      <td>Tablo</td>
      <td>BLOB, dizini</td>
      <td>
        <font size=2>Protokol: azure BLOB sayısı <br>Kimlik doğrulaması: {azure erişim tuşu} <br>Adresi: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;etki alanı <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;hesabı <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;kapsayıcı <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adı</font>
      </td>
    </tr>
    <tr>
      <td>Azure Storage</td>
      <td>Kapsayıcı</td>
      <td>Kapsayıcı</td>
      <td>
        <font size=2>Protokol: azure tabloları <br>Kimlik doğrulaması: {azure erişim tuşu} <br>Adresi: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;etki alanı <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;hesabı</font>
      </td>
    </tr>
    <tr>
      <td>Azure Storage</td>
      <td>Tablo</td>
      <td>Tablo</td>
      <td>
        <font size=2>Protokol: azure tabloları <br>Kimlik doğrulaması: {azure erişim tuşu} <br>Adresi: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;etki alanı <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;hesabı <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adı</font>
      </td>
    </tr>
    <tr>
      <td>Cosmos</td>
      <td>Kapsayıcı</td>
      <td>Sanal küme</td>
      <td>
        <font size=2>Protokol: cosmos <br>Kimlik doğrulaması: {basic, windows} <br>Adresi: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>Cosmos</td>
      <td>Tablo</td>
      <td>Akış, akış kümesi görünümü</td>
      <td>
        <font size=2>Protokol: cosmos <br>Kimlik doğrulaması: {basic, windows} <br>Adresi: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>Datazen</td>
      <td>Kapsayıcı</td>
      <td>Site</td>
      <td>
        <font size=2>Protokol: http <br>Kimlik doğrulaması: {none, basic, windows, oauth} <br>Adresi: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>Datazen</td>
      <td>Rapor</td>
      <td>Rapor, Pano</td>
      <td>
        <font size=2>Protokol: http <br>Kimlik doğrulaması: {none, basic, windows, oauth} <br>Adresi: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>DB2</td>
      <td>Kapsayıcı</td>
      <td>Database</td>
      <td>
        <font size=2>Protokol: db2 <br>Kimlik doğrulaması: {basic, windows} <br>Adresi: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Sunucu <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Veritabanı</font>
      </td>
    </tr>
    <tr>
      <td>DB2</td>
      <td>Tablo</td>
      <td>Tablo, Görünüm</td>
      <td>
        <font size=2>Protokol: db2 <br>Kimlik doğrulaması: {basic, windows} <br>Adresi: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Sunucu <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Veritabanı <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Nesne <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Şema</font>
      </td>
    </tr>
    <tr>
      <td>Dosya sistemi</td>
      <td>Tablo</td>
      <td>Dosya</td>
      <td>
        <font size=2>Protokol: dosya <br>Kimlik doğrulaması: {none, basic, windows} <br>Adresi: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;yol</font>
      </td>
    </tr>
    <tr>
      <td>FTP</td>
      <td>Tablo</td>
      <td>Dizin, dosya</td>
      <td>
        <font size=2>Protokol: ftp <br>Kimlik doğrulaması: {none, basic, windows} <br>Adresi: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>Hadoop dağıtılmış dosya sistemi</td>
      <td>Kapsayıcı</td>
      <td>Küme</td>
      <td>
        <font size=2>Protokol: webhdfs <br>Kimlik doğrulaması: {temel, oauth} <br>Adresi: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>Hadoop dağıtılmış dosya sistemi</td>
      <td>Tablo</td>
      <td>Dizin, dosya</td>
      <td>
        <font size=2>Protokol: webhdfs <br>Kimlik doğrulaması: {temel, oauth} <br>Adresi: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>Hive</td>
      <td>Kapsayıcı</td>
      <td>Database</td>
      <td>
        <font size=2>Protokol: hive <br>Kimlik doğrulaması: {Hdınsight, basic, username, hiçbiri} <br>Adresi: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Sunucu <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Veritabanı <br>connectionProperties: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;serverProtocol: {hive2}</font>
      </td>
    </tr>
    <tr>
      <td>Hive</td>
      <td>Tablo</td>
      <td>Tablo, Görünüm</td>
      <td>
        <font size=2>Protokol: hive <br>Kimlik doğrulaması: {Hdınsight, basic, username, hiçbiri} <br>Adresi: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Sunucu <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Veritabanı <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Nesne <br>connectionProperties: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;serverProtocol: {hive2}</font>
      </td>
    </tr>
    <tr>
      <td>HTTP</td>
      <td>Kapsayıcı</td>
      <td>Site</td>
      <td>
        <font size=2>Protokol: http <br>Kimlik doğrulaması: {none, basic, windows, oauth} <br>Adresi: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>HTTP</td>
      <td>Rapor</td>
      <td>Rapor, Pano</td>
      <td>
        <font size=2>Protokol: http <br>Kimlik doğrulaması: {none, basic, windows, oauth} <br>Adresi: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>HTTP</td>
      <td>Tablo</td>
      <td>Uç noktası, dosya</td>
      <td>
        <font size=2>Protokol: http <br>Kimlik doğrulaması: {none, basic, windows, oauth} <br>Adresi: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>MySQL</td>
      <td>Kapsayıcı</td>
      <td>Database</td>
      <td>
        <font size=2>Protokol: mysql <br>Kimlik doğrulaması: {Protokolü, windows} <br>Adresi: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Sunucu <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Veritabanı</font>
      </td>
    </tr>
    <tr>
      <td>MySQL</td>
      <td>Tablo</td>
      <td>Tablo, Görünüm</td>
      <td>
        <font size=2>Protokol: mysql <br>Kimlik doğrulaması: {Protokolü, windows} <br>Adresi: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Sunucu <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Veritabanı <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Nesne</font>
      </td>
    </tr>
    <tr>
      <td>OData</td>
      <td>Kapsayıcı</td>
      <td>Varlık kapsayıcısı</td>
      <td>
        <font size=2>Protokol: odata <br>Kimlik doğrulaması: {none, basic, windows} <br>Adresi: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>OData</td>
      <td>Tablo</td>
      <td>Varlık kümesi, işlevi</td>
      <td>
        <font size=2>Protokol: odata <br>Kimlik doğrulaması: {none, basic, windows} <br>Adresi: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Kaynak</font>
      </td>
    </tr>
    <tr>
      <td>Oracle Veritabanı</td>
      <td>Kapsayıcı</td>
      <td>Database</td>
      <td>
        <font size=2>Protokol: oracle <br>Kimlik doğrulaması: {Protokolü, windows} <br>Adresi: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Sunucu <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Veritabanı</font>
      </td>
    </tr>
    <tr>
      <td>Oracle Veritabanı</td>
      <td>Tablo</td>
      <td>Tablo, Görünüm</td>
      <td>
        <font size=2>Protokol: oracle <br>Kimlik doğrulaması: {Protokolü, windows} <br>Adresi: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Sunucu <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Veritabanı <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Şema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Nesne</font>
      </td>
    </tr>
    <tr>
      <td>PostgreSQL</td>
      <td>Kapsayıcı</td>
      <td>Database</td>
      <td>
        <font size=2>Protokol: postgresql <br>Kimlik doğrulaması: {basic, windows} <br>Adresi: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Sunucu <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Veritabanı</font>
      </td>
    </tr>
    <tr>
      <td>PostgreSQL</td>
      <td>Tablo</td>
      <td>Tablo, Görünüm</td>
      <td>
        <font size=2>Protokol: postgresql <br>Kimlik doğrulaması: {basic, windows} <br>Adresi: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Sunucu <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Veritabanı <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Şema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Nesne</font>
      </td>
    </tr>
    <tr>
      <td>Power BI</td>
      <td>Kapsayıcı</td>
      <td>Site</td>
      <td>
        <font size=2>Protokol: http <br>Kimlik doğrulaması: {none, basic, windows, oauth} <br>Adresi: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>Power BI</td>
      <td>Rapor</td>
      <td>Rapor, Pano</td>
      <td>
        <font size=2>Protokol: http <br>Kimlik doğrulaması: {none, basic, windows, oauth} <br>Adresi: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>Power Query</td>
      <td>Tablo</td>
      <td>Veri karma</td>
      <td>
        <font size=2>Protokolü: güç sorgu <br>Kimlik doğrulaması: {oauth} <br>Adresi: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>Salesforce</td>
      <td>Tablo</td>
      <td>Nesne</td>
      <td>
        <font size=2>Protokol: salesforce-com <br>Kimlik doğrulaması: {basic, windows} <br>Adresi: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;loginServer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;sınıfı <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ItemName</font>
      </td>
    </tr>
    <tr>
      <td>SAP HANA</td>
      <td>Kapsayıcı</td>
      <td>Sunucu</td>
      <td>
        <font size=2>Protokol: sap hana sql <br>Kimlik doğrulaması: {Protokolü, windows} <br>Adresi: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Sunucu</font>
      </td>
    </tr>
    <tr>
      <td>SAP HANA</td>
      <td>Tablo</td>
      <td>Görünüm</td>
      <td>
        <font size=2>Protokol: sap hana sql <br>Kimlik doğrulaması: {Protokolü, windows} <br>Adresi: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Sunucu <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Şema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Nesne</font>
      </td>
    </tr>
    <tr>
      <td>SharePoint</td>
      <td>Tablo</td>
      <td>Liste</td>
      <td>
        <font size=2>Protokol: sharepoint listesi <br>Kimlik doğrulaması: {basic, windows} <br>Adresi: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>SQL Veri Ambarı</td>
      <td>Komut</td>
      <td>Saklı yordam</td>
      <td>
        <font size=2>Protokol: tds <br>Kimlik doğrulaması: {Protokolü, windows} <br>Adresi: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Sunucu <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Veritabanı <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Şema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Nesne</font>
      </td>
    </tr>
    <tr>
      <td>SQL Veri Ambarı</td>
      <td>TableValuedFunction</td>
      <td>Tablo değerli işlevi</td>
      <td>
        <font size=2>Protokol: tds <br>Kimlik doğrulaması: {Protokolü, windows} <br>Adresi: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Sunucu <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Veritabanı <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Şema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Nesne</font>
      </td>
    </tr>
    <tr>
      <td>SQL Veri Ambarı</td>
      <td>Kapsayıcı</td>
      <td>Database</td>
      <td>
        <font size=2>Protokol: tds <br>Kimlik doğrulaması: {Protokolü, windows} <br>Adresi: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Sunucu <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Veritabanı</font>
      </td>
    </tr>
    <tr>
      <td>SQL Veri Ambarı</td>
      <td>Tablo</td>
      <td>Tablo, Görünüm</td>
      <td>
        <font size=2>Protokol: tds <br>Kimlik doğrulaması: {Protokolü, windows} <br>Adresi: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Sunucu <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Veritabanı <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Şema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Nesne</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server</td>
      <td>Komut</td>
      <td>Saklı yordam</td>
      <td>
        <font size=2>Protokol: tds <br>Kimlik doğrulaması: {Protokolü, windows} <br>Adresi: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Sunucu <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Veritabanı <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Şema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Nesne</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server</td>
      <td>TableValuedFunction</td>
      <td>Tablo değerli işlevi</td>
      <td>
        <font size=2>Protokol: tds <br>Kimlik doğrulaması: {Protokolü, windows} <br>Adresi: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Sunucu <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Veritabanı <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Şema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Nesne</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server</td>
      <td>Kapsayıcı</td>
      <td>Database</td>
      <td>
        <font size=2>Protokol: tds <br>Kimlik doğrulaması: {Protokolü, windows} <br>Adresi: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Sunucu <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Veritabanı</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server</td>
      <td>Tablo</td>
      <td>Tablo, Görünüm</td>
      <td>
        <font size=2>Protokol: tds <br>Kimlik doğrulaması: {Protokolü, windows} <br>Adresi: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Sunucu <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Veritabanı <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Şema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Nesne</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services çok boyutlu</td>
      <td>Kapsayıcı</td>
      <td>modeli</td>
      <td>
        <font size=2>Protokolü: Analiz Hizmetleri <br>Kimlik doğrulaması: {windows, temel, anonim, hiçbiri} <br>Adresi: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Sunucu <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Veritabanı <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;modeli</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services çok boyutlu</td>
      <td>KPI</td>
      <td>KPI</td>
      <td>
        <font size=2>Protokolü: Analiz Hizmetleri <br>Kimlik doğrulaması: {windows, temel, anonim, hiçbiri} <br>Adresi: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Sunucu <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Veritabanı <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;modeli <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Nesne <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objectType: {KPI}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services çok boyutlu</td>
      <td>Ölçü birimi</td>
      <td>Ölçü birimi</td>
      <td>
        <font size=2>Protokolü: Analiz Hizmetleri <br>Kimlik doğrulaması: {windows, temel, anonim, hiçbiri} <br>Adresi: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Sunucu <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Veritabanı <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;modeli <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Nesne <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objectType: {Measure}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services çok boyutlu</td>
      <td>Tablo</td>
      <td>Boyut</td>
      <td>
        <font size=2>Protokolü: Analiz Hizmetleri <br>Kimlik doğrulaması: {windows, temel, anonim, hiçbiri} <br>Adresi: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Sunucu <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Veritabanı <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;modeli <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Nesne <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objectType: {boyutu}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services tablo</td>
      <td>Kapsayıcı</td>
      <td>modeli</td>
      <td>
        <font size=2>Protokolü: Analiz Hizmetleri <br>Kimlik doğrulaması: {windows, temel, anonim, hiçbiri} <br>Adresi: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Sunucu <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Veritabanı <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;modeli</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services tablo</td>
      <td>KPI</td>
      <td>KPI</td>
      <td>
        <font size=2>Protokolü: Analiz Hizmetleri <br>Kimlik doğrulaması: {windows, temel, anonim, hiçbiri} <br>Adresi: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Sunucu <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Veritabanı <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;modeli <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Nesne <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objectType: {KPI}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services tablo</td>
      <td>Ölçü birimi</td>
      <td>Ölçü birimi</td>
      <td>
        <font size=2>Protokolü: Analiz Hizmetleri <br>Kimlik doğrulaması: {windows, temel, anonim, hiçbiri} <br>Adresi: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Sunucu <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Veritabanı <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;modeli <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Nesne <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objectType: {Measure}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services tablo</td>
      <td>Tablo</td>
      <td>Tablo</td>
      <td>
        <font size=2>Protokolü: Analiz Hizmetleri <br>Kimlik doğrulaması: {windows, temel, anonim, hiçbiri} <br>Adresi: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Sunucu <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Veritabanı <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;modeli <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Nesne <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objectType: {Table}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Raporlama Hizmetleri</td>
      <td>Kapsayıcı</td>
      <td>Sunucu</td>
      <td>
        <font size=2>Protokol: Raporlama Hizmetleri <br>Kimlik doğrulaması: {windows} <br>Adresi: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Sunucu <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Sürüm: {ReportingService2010}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Raporlama Hizmetleri</td>
      <td>Rapor</td>
      <td>Rapor</td>
      <td>
        <font size=2>Protokol: Raporlama Hizmetleri <br>Kimlik doğrulaması: {windows} <br>Adresi: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Sunucu <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;yol <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Sürüm: {ReportingService2010}</font>
      </td>
    </tr>
    <tr>
      <td>Teradata</td>
      <td>Kapsayıcı</td>
      <td>Database</td>
      <td>
        <font size=2>Protokol: teradata <br>Kimlik doğrulaması: {Protokolü, windows} <br>Adresi: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Sunucu <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Veritabanı</font>
      </td>
    </tr>
    <tr>
      <td>Teradata</td>
      <td>Tablo</td>
      <td>Tablo, Görünüm</td>
      <td>
        <font size=2>Protokol: teradata <br>Kimlik doğrulaması: {Protokolü, windows} <br>Adresi: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Sunucu <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Veritabanı <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Nesne</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Ana Veri Hizmetleri</td>
      <td>Kapsayıcı</td>
      <td>modeli</td>
      <td>
        <font size="2">Protokol: mssql-mds <br>Kimlik doğrulaması: {windows} <br>Adresi: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;modeli <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Sürüm</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Ana Veri Hizmetleri</td>
      <td>Tablo</td>
      <td>Varlık</td>
      <td>
        <font size="2">Protokol: mssql-mds <br>Kimlik doğrulaması: {windows} <br>Adresi: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;modeli <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Sürüm <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Varlık</font>
      </td>
    </tr>
    <tr>
      <td>Azure Cosmos DB</td>
      <td>Kapsayıcı</td>
      <td>Database</td>
      <td>
        <font size=2>Protokol: belge-db <br>Kimlik doğrulaması: {azure erişim tuşu} <br>Adresi: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Veritabanı</font>
      </td>
    </tr>
    <tr>
      <td>Azure Cosmos DB</td>
      <td>Koleksiyon</td>
      <td>Koleksiyon</td>
      <td>
        <font size=2>Protokol: belge-db <br>Kimlik doğrulaması: {azure erişim tuşu} <br>Adresi: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Veritabanı <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;koleksiyonu</font>
      </td>
    </tr>
    <tr>
      <td>Genel ODBC</td>
      <td>Kapsayıcı</td>
      <td>Database</td>
      <td>
        <font size=2>Protokol: odbc <br>Kimlik doğrulaması: {basic, windows} <br>Adresi: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Seçenekler <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Veritabanı</font>
      </td>
    </tr>
    <tr>
      <td>Genel ODBC</td>
      <td>Tablo</td>
      <td>Tablo, Görünüm</td>
      <td>
        <font size=2>Protokol: odbc <br>Kimlik doğrulaması: {basic, windows} <br>Adresi: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Seçenekler <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Veritabanı <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Nesne <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Şema</font>
      </td>
    </tr>
    <tr>
      <td>Sybase</td>
      <td>Kapsayıcı</td>
      <td>Database</td>
      <td>
        <font size=2>Protokol: sybase <br>Kimlik doğrulaması: {basic, windows} <br>Adresi: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Sunucu <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Veritabanı</font>
      </td>
    </tr>
    <tr>
      <td>Sybase</td>
      <td>Tablo</td>
      <td>Tablo, Görünüm</td>
      <td>
        <font size=2>Protokol: sybase <br>Kimlik doğrulaması: {basic, windows} <br>Adresi: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Sunucu <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Veritabanı <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Şema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Nesne</font>
      </td>
    </tr>
    <tr>
      <td>Diğer (Yukarıdakilerin hiçbiri)</td>
      <td>\*</td>
      <td>\*</td>
      <td>
        <font size=2>Protokol: genel-varlık <br>Adresi: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;AssetID</font>
      </td>
    </tr>
</table>
