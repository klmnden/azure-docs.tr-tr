---
title: Veri yükleme SQL Server'dan Azure SQL veri ambarı (SSIS) içine | Microsoft Docs
description: SQL veri ambarı için çok çeşitli veri kaynakları verileri taşımak için bir SQL Server Integration Services (SSIS) paketi oluşturmak gösterilmiştir.
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: jhubbard
editor: ''
ms.assetid: e2c252e9-0828-47c2-a808-e3bea46c134a
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 03/30/2017
ms.author: cakarst;douglasl;barbkess
ms.openlocfilehash: 6c9cebdd715b6997d0633bc725a3945ba9e0c357
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2018
ms.locfileid: "30928832"
---
# <a name="load-data-from-sql-server-into-azure-sql-data-warehouse-ssis"></a>SQL Server'dan Azure SQL veri ambarı (SSIS) içine veri yükleme
> [!div class="op_single_selector"]
> * [SSIS](sql-data-warehouse-load-from-sql-server-with-integration-services.md)
> * [PolyBase](sql-data-warehouse-load-from-sql-server-with-polybase.md)
> * [bcp](sql-data-warehouse-load-from-sql-server-with-bcp.md)
> 
> 

SQL Server'dan Azure SQL Data Warehouse'a veri yüklemek için bir SQL Server Integration Services (SSIS) paketi oluşturun. İsteğe bağlı olarak yeniden yapılandırmayı, dönüştürme ve aracılığıyla SSIS veri akışı geçerken verileri temizler.

Bu öğreticide şunları yapacaksınız:

* Visual Studio'da yeni bir Integration Services projesi oluşturun.
* (Kaynak) olarak SQL Server ve SQL Data Warehouse (hedef) olarak da dahil olmak üzere veri kaynaklarına bağlayın.
* Bir SSIS paketi tasarım verileri, kaynaktan hedefe yükler.
* Verileri yüklemek için SSIS paketi çalıştırın.

Bu öğretici, SQL Server veri kaynağı olarak kullanır. SQL Server, şirket içinde veya bir Azure sanal makinesi çalışıyor.

## <a name="basic-concepts"></a>Temel kavramlar
Paket SSIS iş birimidir. İlişkili paketleri projelerinde gruplandırılır. SQL Server veri araçları ile Visual Studio'da projeler ve tasarım paketler oluşturun. Tasarım işlemi, sürükleyin ve bileşenleri Araç Kutusu'ndan tasarım yüzeyine bırak, bunları bağlamak ve bunların özelliklerini ayarlamak görsel bir işlemdir. Paketinizi tamamladıktan sonra isteğe bağlı olarak, SQL Server için kapsamlı yönetim, izleme ve güvenlik için dağıtabilirsiniz.

## <a name="options-for-loading-data-with-ssis"></a>SSIS ile veri yükleme seçenekleri
SQL Server Integration Services (SSIS) bağlanan ve bu verileri SQL Data Warehouse'a, yükleme için çeşitli seçenekler sağlayan esnek araçlar kümesidir.

1. SQL veri ambarı'na bağlanmak için bir ADO NET hedef kullanın. En az yapılandırma seçenekleri içerdiğinden Bu öğretici bir ADO NET hedef kullanır.
2. SQL veri ambarı'na bağlanmak için bir OLE DB hedef kullanın. Bu seçenek ADO NET hedef daha biraz daha iyi performans sağlayabilir.
3. Azure Blob karşıya yükleme görevi, verileri Azure Blob Depolama hazırlamak için kullanın. Ardından SSIS SQL Yürüt görevi verileri SQL Data Warehouse'a yükler Polybase komut dosyasını başlatmak için kullanın. Bu seçenek, burada listelenen üç seçeneklerden en iyi performans sağlar. Azure Blob karşıya yükleme görev almak için indirin [Microsoft SQL Server 2016 tümleştirme hizmetleri özellik paketi Azure][Microsoft SQL Server 2016 Integration Services Feature Pack for Azure]. Polybase hakkında daha fazla bilgi için bkz: [PolyBase Kılavuzu][PolyBase Guide].

## <a name="before-you-start"></a>Başlamadan önce
Bu öğreticide ilerleyebilmeniz için şunlar gereklidir:

1. **SQL Server Integration Services (SSIS)**. SSIS SQL Server'ın bir bileşenidir ve bir değerlendirme sürümü veya SQL Server'ın lisanslı bir sürüm gerektirir. SQL Server 2016 Önizleme'nin değerlendirme sürümünü almak için bkz: [SQL Server değerlendirme][SQL Server Evaluations].
2. **Visual Studio**. Ücretsiz Visual Studio Community sürümü almak için bkz: [Visual Studio Community][Visual Studio Community].
3. **SQL Server veri araçları Visual Studio (SSDT) için**. Visual Studio için SQL Server veri araçları almak için bkz: [karşıdan SQL Server veri Araçları'nı (SSDT)][Download SQL Server Data Tools (SSDT)].
4. **Örnek veri**. Bu öğretici, SQL Data Warehouse'a yüklenecek SQL Server kaynak verisi olarak AdventureWorks örnek veritabanında depolanan örnek verileri kullanır. AdventureWorks örnek veritabanını almak için bkz: [AdventureWorks 2014 örnek veritabanları][AdventureWorks 2014 Sample Databases].
5. **SQL veri ambarı veritabanı ve izinleri**. Bu öğreticide SQL Data Warehouse örneğine bağlanır ve verileri içine yükler. Bir tablo oluşturmak için ve veri yüklemek için izinlere sahip olmanız gerekir.
6. **Bir güvenlik duvarı kuralı**. SQL veri ambarına veri karşıya yüklemeden önce bir güvenlik duvarı kuralı, SQL Data Warehouse, yerel bilgisayarınızın IP adresi ile oluşturmak zorunda.

## <a name="step-1-create-a-new-integration-services-project"></a>1. adım: yeni bir Integration Services projesi oluşturma
1. Visual Studio'yu başlatın.
2. Üzerinde **dosya** menüsünde, select **yeni | Proje**.
3. Gidin **yüklü | Şablonları | İş Zekası | Integration Services** proje türleri.
4. Seçin **Integration Services proje**. İçin değerler sağlayın **adı** ve **konumu**ve ardından **Tamam**.

Visual Studio açar ve yeni bir Integration Services (SSIS) projesi oluşturur. Ardından Visual Studio tek yeni SSIS paketi (Package.dtsx) Tasarımcı projede açar. Aşağıdaki ekran alanlarda bakın:

* Solda, **araç** SSIS bileşenlerinin.
* Ortadaki birden çok sekmelerle tasarım yüzeyi. Genellikle en az kullandığınız **akış denetimi** ve **veri akışını** sekmeleri.
* Sağ taraftaki **Çözüm Gezgini** ve **özellikleri** bölmeleri.
  
    ![][01]

## <a name="step-2-create-the-basic-data-flow"></a>2. adım: temel veri akışı oluşturma
1. Bir veri akış görevi Araç Kutusu'ndan tasarım yüzeyine merkezine sürükleyin (üzerinde **akış denetimi** sekmesi).
   
    ![][02]
2. Veri akış sekmesine geçmek için veri akış görevi çift tıklatın.
3. Araç kutusunda diğer kaynakları listesinden bir ADO.NET kaynak tasarım yüzeyine sürükleyin. Halen seçiliyken kaynak bağdaştırıcısıyla adının değiştirme **SQL Server Kaynak** içinde **özellikleri** bölmesi.
4. Araç kutusunda diğer hedefler listesinden bir ADO.NET hedef ADO.NET kaynak altında tasarım yüzeyine sürükleyin. Halen seçiliyken hedef bağdaştırıcısıyla adının değiştirme **SQL DW hedef** içinde **özellikleri** bölmesi.
   
    ![][09]

## <a name="step-3-configure-the-source-adapter"></a>3. adım: kaynak bağdaştırıcısını yapılandırın
1. Kaynak bağdaştırıcısı açmak için çift **ADO.NET Kaynak Düzenleyici**.
   
    ![][03]
2. Üzerinde **Bağlantı Yöneticisi** sekmesinde **ADO.NET Kaynak Düzenleyici**, tıklatın **yeni** düğmesine **ADO.NET Bağlantı Yöneticisi** açmak için liste **ADO.NET Bağlantı Yöneticisi'ni yapılandırmak** iletişim kutusuna ve bu öğreticinin yükler verileri SQL Server veritabanı için bağlantı ayarlarını oluşturun.
   
    ![][04]
3. İçinde **ADO.NET Bağlantı Yöneticisi'ni yapılandırma** iletişim kutusu, tıklatın **yeni** açmak için düğmeye **Bağlantı Yöneticisi** iletişim kutusuna ve yeni bir veri bağlantısı oluştur.
   
    ![][05]
4. İçinde **Bağlantı Yöneticisi** iletişim kutusunda, şunları yapın.
   
   1. İçin **sağlayıcısı**, SqlClient veri sağlayıcısı seçin.
   2. İçin **sunucu adı**, SQL Server adı girin.
   3. İçinde **sunucuda oturum açın** bölümünde, seçin veya kimlik doğrulama bilgilerini girin.
   4. İçinde **veritabanına bağlan** bölümünde, AdventureWorks örnek veritabanını seçin.
   5. Tıklatın **Bağlantıyı Sına**.
      
       ![][06]
   6. Bağlantı testi sonuçlarını raporlar iletişim kutusunu tıklatıp **Tamam** geri dönmek için **Bağlantı Yöneticisi** iletişim kutusu.
   7. İçinde **Bağlantı Yöneticisi** iletişim kutusu, tıklatın **Tamam** geri dönmek için **ADO.NET Bağlantı Yöneticisi'ni yapılandırma** iletişim kutusu.
5. İçinde **ADO.NET Bağlantı Yöneticisi'ni yapılandırma** iletişim kutusu, tıklatın **Tamam** geri dönmek için **ADO.NET Kaynak Düzenleyici**.
6. İçinde **ADO.NET Kaynak Düzenleyici**, **tablo veya Görünüm adı** listesinde **Sales.SalesOrderDetail** tablo.
   
    ![][07]
7. Tıklatın **Önizleme** veri kaynak tablonun ilk 200 satırı görmek için **Önizleme sorgu sonuçları** iletişim kutusu.
   
    ![][08]
8. İçinde **Önizleme sorgu sonuçları** iletişim kutusu, tıklatın **Kapat** geri dönmek için **ADO.NET Kaynak Düzenleyici**.
9. İçinde **ADO.NET Kaynak Düzenleyici**, tıklatın **Tamam** veri kaynağı yapılandırmayı tamamlamak için.

## <a name="step-4-connect-the-source-adapter-to-the-destination-adapter"></a>4. adım: kaynak bağdaştırıcısı hedef bağdaştırıcısına bağlanacak.
1. Tasarım yüzeyine kaynak bağdaştırıcıyı seçin.
2. Kaynak bağdaştırıcısından genişletir mavi oku seçin ve yerine tutturur kadar hedef Düzenleyici'ye sürükleyin.
   
    ![][10]
   
    Tipik bir SSIS paketi yeniden yapılandırmayı, dönüştürme ve aracılığıyla SSIS veri akışı geçerken, verilerini temizlemek için diğer bileşenlerini SSIS araç kaynak ve hedef arasında bir sayı kullanın. Bu örnek olabildiğince basit tutmak için biz kaynak doğrudan hedefe bağlandığınız.

## <a name="step-5-configure-the-destination-adapter"></a>5. adım: hedef bağdaştırıcısını yapılandırın
1. Hedef bağdaştırıcı açmak için çift **ADO.NET hedef Düzenleyicisi**.
   
    ![][11]
2. Üzerinde **Bağlantı Yöneticisi** sekmesinde **ADO.NET hedef Düzenleyicisi**, tıklatın **yeni** düğmesine **Bağlantı Yöneticisi** açmak için liste **ADO.NET Bağlantı Yöneticisi'ni yapılandırmak** iletişim kutusuna ve bu öğreticinin veri yükler Azure SQL veri ambarı veritabanı için bağlantı ayarlarını oluşturun.
3. İçinde **ADO.NET Bağlantı Yöneticisi'ni yapılandırma** iletişim kutusu, tıklatın **yeni** açmak için düğmeye **Bağlantı Yöneticisi** iletişim kutusuna ve yeni bir veri bağlantısı oluştur.
4. İçinde **Bağlantı Yöneticisi** iletişim kutusunda, şunları yapın.
   1. İçin **sağlayıcısı**, SqlClient veri sağlayıcısı seçin.
   2. İçin **sunucu adı**, SQL veri ambarı adını girin.
   3. İçinde **sunucuda oturum açın** bölümünde, select **kullanım SQL Server kimlik doğrulaması** ve kimlik doğrulama bilgilerini girin.
   4. İçinde **veritabanına bağlan** bölümünde, var olan bir SQL veri ambarı veritabanını seçin.
   5. Tıklatın **Bağlantıyı Sına**.
   6. Bağlantı testi sonuçlarını raporlar iletişim kutusunu tıklatıp **Tamam** geri dönmek için **Bağlantı Yöneticisi** iletişim kutusu.
   7. İçinde **Bağlantı Yöneticisi** iletişim kutusu, tıklatın **Tamam** geri dönmek için **ADO.NET Bağlantı Yöneticisi'ni yapılandırma** iletişim kutusu.
5. İçinde **ADO.NET Bağlantı Yöneticisi'ni yapılandırma** iletişim kutusu, tıklatın **Tamam** geri dönmek için **ADO.NET hedef Düzenleyicisi**.
6. İçinde **ADO.NET hedef Düzenleyicisi**, tıklatın **yeni** yanına **tablo veya görünüm kullanın** açmak için liste **Create Table** kaynak tablosu ile eşleşen bir sütun listesi ile yeni bir hedef tablo oluşturmak için iletişim kutusu.
   
    ![][12a]
7. İçinde **Create Table** iletişim kutusunda, şunları yapın.
   
   1. Hedef tablo adını değiştirin **satış siparişi ayrıntısını**.
   2. Kaldırma **ROWGUID** sütun. **Uniqueidentifier** veri türü, SQL veri ambarı'nda desteklenmiyor.
   3. Veri türünü değiştirmek **LineTotal** sütuna **para**. **Ondalık** veri türü, SQL veri ambarı'nda desteklenmiyor. Desteklenen veri türleri hakkında daha fazla bilgi için bkz: [CREATE TABLE (Azure SQL Data Warehouse, paralel veri ambarı)][CREATE TABLE (Azure SQL Data Warehouse, Parallel Data Warehouse)].
      
       ![][12b]
   4. Tıklatın **Tamam** tablo oluşturma ve dönmek için **ADO.NET hedef Düzenleyicisi**.
8. İçinde **ADO.NET hedef Düzenleyicisi**seçin **eşlemeleri** kaynağındaki sütunları hedef sütunlara nasıl eşlendiğini görmek için sekmesini.
   
    ![][13]
9. Tıklatın **Tamam** veri kaynağı yapılandırmayı tamamlamak için.

## <a name="step-6-run-the-package-to-load-the-data"></a>6. adım: verileri yüklemek için paket çalıştırma
Paket tıklayarak çalıştırın **Başlat** düğmesi araç çubuğunda veya birini seçerek **çalıştırmak** seçeneklerinden **hata ayıklama** menüsü.

Paket çalışmaya başladığında, etkinlik ve bunun yanı sıra o ana kadarki işlenen satır sayısını belirtmek için sarı dönen Tekerlek bakın.

![][14]

Paket çalışması sona erdiğinde, başarı ve bunun yanı sıra toplam hedefe kaynağından yüklenen verilerin satır sayısını belirtmek için yeşil onay işareti görürsünüz.

![][15]

Tebrikler! SQL Server Integration Services, Azure SQL Data Warehouse'a veri yükleme başarıyla kullandığınız.

## <a name="next-steps"></a>Sonraki adımlar
* SSIS veri akışı hakkında daha fazla bilgi edinin. Buradan başlayın: [veri akışını][Data Flow].
* Hata ayıklama ve paketleri hak tasarım ortamında sorun giderme hakkında bilgi edinin. Buradan başlayın: [paket geliştirme için sorun giderme araçları][Troubleshooting Tools for Package Development].
* SSIS projeleri ve paketleri Integration Services sunucusu veya başka bir depolama konumuna dağıtmayı öğrenin. Buradan başlayın: [, dağıtım projeleri ve paketleri][Deployment of Projects and Packages].

<!-- Image references -->
[01]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ssis-designer-01.png
[02]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ssis-data-flow-task-02.png
[03]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-source-03.png
[04]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-connection-manager-04.png
[05]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-connection-05.png
[06]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/test-connection-06.png
[07]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-source-07.png
[08]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/preview-data-08.png
[09]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/source-destination-09.png
[10]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/connect-source-destination-10.png
[11]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-destination-11.png
[12a]: ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/destination-query-before-12a.png
[12b]: ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/destination-query-after-12b.png
[13]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/column-mapping-13.png
[14]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/package-running-14.png
[15]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/package-success-15.png

<!-- Article references -->

<!-- MSDN references -->
[PolyBase Guide]: https://msdn.microsoft.com/library/mt143171.aspx
[Download SQL Server Data Tools (SSDT)]: https://msdn.microsoft.com/library/mt204009.aspx
[CREATE TABLE (Azure SQL Data Warehouse, Parallel Data Warehouse)]: https://msdn.microsoft.com/library/mt203953.aspx
[Data Flow]: https://msdn.microsoft.com/library/ms140080.aspx
[Troubleshooting Tools for Package Development]: https://msdn.microsoft.com/library/ms137625.aspx
[Deployment of Projects and Packages]: https://msdn.microsoft.com/library/hh213290.aspx

<!--Other Web references-->
[Microsoft SQL Server 2016 Integration Services Feature Pack for Azure]: http://go.microsoft.com/fwlink/?LinkID=626967
[SQL Server Evaluations]: https://www.microsoft.com/en-us/evalcenter/evaluate-sql-server-2016
[Visual Studio Community]: https://www.visualstudio.com/en-us/products/visual-studio-community-vs.aspx
[AdventureWorks 2014 Sample Databases]: https://msftdbprodsamples.codeplex.com/releases/view/125550
