<properties
    pageTitle="Esnetme İçin Veritabanını Etkinleştirme Sihirbazını çalıştırarak kullanmaya başlama | Microsoft Azure"
    description="Esnetme İçin Veritabanını Etkinleştirme Sihirbazını çalıştırarak, bir veritabanının Esnetme Veritabanı için nasıl yapılandırılacağını öğrenin."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager=""
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="05/17/2016"
    ms.author="douglasl"/>

# Esnetme İçin Veritabanını Etkinleştirme Sihirbazını çalıştırarak kullanmaya başlama

Bir veritabanını Esnetme Veritabanı için çalıştırmak üzere, Esnetme İçin Veritabanını Etkinleştirme Sihirbazını çalıştırın.  Bu konu başlığında, sihirbazda girmeniz gereken bilgiler ve yapmanız gereken seçimler açıklanmaktadır.

Esnetme Veritabanı hakkında daha fazla bilgi edinmek için bkz. [Esnetme Veritabanı](sql-server-stretch-database-overview.md).

## Sihirbazı başlatma

1.  SQL Server Management Studio'daki Nesne Gezgini'nde Esnetmeyi etkinleştirmek istediğiniz veritabanını seçin.

2.  Sağ tıklayın, **Görevler**'i ve **Esnet**'i seçin, ardından **Etkinleştir**'i seçerek sihirbazı başlatın.

## <a name="Intro"></a>Giriş
Sihirbazın amacını ve önkoşullarını gözden geçirin.

![Esnetme Veritabanı sihirbazı giriş sayfası][StretchWizardImage1]

## <a name="Tables"></a>Tabloları seçme
Esnetme için etkinleştirmek istediğiniz tabloları seçin.

![Esnetme Veritabanı sihirbazı tabloları seçme sayfası][StretchWizardImage2]

|Sütun|Açıklama|
|----------|---------------|
|(başlık yok)|Seçilen tabloyu Esnetme için etkinleştirmek üzere bu sütundaki onay kutusunu işaretleyin.|
|**Ad**|Tablodaki sütunun adını belirtir.|
|(başlık yok)|Bu sütunda bir simge olması genellikle bir engelleme sorunu nedeniyle seçilen tabloyu Esnetme için etkinleştiremeyeceğinizi gösterir. Bunun nedeni tablonun desteklenmeyen bir veri türünü kullanması olabilir. Simgenin üzerine gelerek bir araç ipucunda daha fazla bilgi görüntüleyin. Daha fazla bilgi için bkz. [Esnetme Veritabanı için yüzey alanı sınırlamaları ve engelleme sorunları](sql-server-stretch-database-limitations.md).|
|**Esnetilmiş**|Tablonun zaten etkin olup olmadığını gösterir.|
|**Geçiş**|Tüm bir tabloyu geçirebilir (**Tüm Tablo**) veya sihirbazda tarih temelli bir filtre koşulu belirtebilirsiniz. Geçirilecek satırları seçmek için farklı bir filtre koşulu kullanmak isterseniz sihirbazdan çıktıktan sonra filtre koşulunu belirtmek için ALTER TABLE deyimini çalıştırın. Filtre koşulu hakkında daha fazla bilgi için bkz. [Geçirilecek satırları seçmek için filtre koşulu kullanma (Esnetme Veritabanı)](sql-server-stretch-database-predicate-function.md). Koşulu uygulama hakkında daha fazla bilgi için bkz. [Esnetme Veritabanı'nı bir tablo için etkinleştirme](sql-server-stretch-database-enable-table.md) veya [ALTER TABLE (Transact-SQL)](https://msdn.microsoft.com/library/ms190273.aspx).|
|**Satırlar**|Tablodaki satır sayısını belirtir.|
|**Boyut (KB)**|Tablo boyutunu KB cinsinden belirtir.|

## <a name="Filter"></a>İsteğe bağlı olarak tarih temelli bir filtre koşulu belirtme

Geçirilecek satırları seçmek için tarih temelli bir filtre koşulu sağlamak isterseniz **Tabloları seçme** sayfasında aşağıdaki işlemleri yapın.

1.  **Esnetmek istediğiniz tabloları seçin** listesinde, tablonun satırında **Tüm Tablo**'ya tıklayın. **Esnetme için satırları seçin** iletişim kutusu açılır.

    ![Tarih temelli bir filtre koşulu tanımlama][StretchWizardImage2a]

2.  **Esnetme için satırları seçin** iletişim kutusunda, **Satırları Seçin** öğesini belirleyin.

3.  **Ad alanı** içinde filtre koşulu için bir ad sağlayın.

4.  **Where** yan tümcesi için tablodan bir tarih sütunu ile bir işleç seçin ve bir tarih değeri sağlayın.

5. Koşulu test etmek için **Denetle**'ye tıklayın. Koşul tablodan sonuçlar döndürüyorsa yani şartı karşılayan geçirilecek satırlar varsa test **Başarılı** sonucunu raporlar.

6.  **Tabloları seçme** sayfasına dönmek için Bitti'ye tıklayın.

    ![Bir filtre koşulu tanımlandıktan sonra Tabloları Seçme sayfası][StretchWizardImage2b]

## <a name="Configure"></a>Azure dağıtımını yapılandırma

1.  Microsoft Azure'da bir Microsoft hesabı ile oturum açın.

    ![Azure'da oturum açma - Esnetme Veritabanı sihirbazı][StretchWizardImage3]

2.  Esnetme Veritabanı için kullanılacak Azure aboneliğini seçin.

3.  Bir Azure bölgesi seçin. Yeni bir sunucu oluşturursanız sunucu bu bölgede oluşturulur.

    Gecikmeyi en aza indirmek üzere Azure bölgesi için SQL Server'ınızın bulunduğu bölgeyi seçin. Bölgeler hakkında daha fazla bilgi için bkz. [Azure Bölgeleri](https://azure.microsoft.com/regions/).

4.  Var olan bir sunucuyu kullanmak mı yoksa yeni bir Azure sunucusu oluşturmak mı istediğinizi belirtin.

    SQL Server'ınızdaki Active Directory, Azure Active Directory ile birleştirildiyse isteğe bağlı olarak SQL Server'ın uzak Azure sunucusu ile iletişim kurması için birleştirilmiş bir hizmet hesabı kullanabilirsiniz. Bu seçeneğin gereksinimleri hakkında daha fazla bilgi için bkz. [ALTER DATABASE SET Seçenekleri (Transact-SQL)](https://msdn.microsoft.com/library/bb522682.aspx).

    -   **Yeni sunucu oluşturma**

        1.  Sunucu yöneticisi için bir oturum açma adı ve parola oluşturun.

        2.  İsteğe bağlı olarak, SQL Server'ın uzak Azure sunucusu ile iletişim kurması için birleştirilmiş bir hizmet hesabı kullanın.

        ![Yeni Azure sunucusu oluşturma - Esnetme Veritabanı sihirbazı][StretchWizardImage4]

    -   **Var olan sunucu**

        1.  Var olan Azure sunucusunun adını girin veya seçin.

        2.  Kimlik doğrulama yöntemini seçin.

            -   **SQL Server Kimlik Doğrulaması** seçeneğini belirlerseniz yeni bir oturum açma adı ve parola oluşturun.

            -   SQL Server'ın uzak Azure sunucusu ile iletişim kurması için birleştirilmiş bir hizmet hesabı kullanmak üzere **Active Directory Tümleşik Kimlik Doğrulama**'yı seçin.

        ![Var olan Azure sunucusunu kullanma - Esnetme Veritabanı sihirbazı][StretchWizardImage5]

## <a name="Credentials"></a>Güvenli kimlik bilgileri
Bir veritabanı ana anahtarı oluşturmak için güçlü bir parola girin veya bir veritabanı ana anahtarı zaten varsa bunun parolasını girin.

Esnetme Veritabanı'nın uzak veritabanına bağlanırken kullandığı kimlik bilgilerini güvenli hale getirmek için bir veritabanı ana anahtarınızın olması gerekir.

![Esnetme Veritabanı sihirbazı güvenli kimlik bilgileri sayfası][StretchWizardImage6]

Veritabanı ana anahtarı hakkında daha fazla bilgi için bkz. [CREATE MASTER KEY (Transact-SQL)](https://msdn.microsoft.com/library/ms174382.aspx) ve [Veritabanı Ana Anahtarı Oluşturma](https://msdn.microsoft.com/library/aa337551.aspx). Sihirbazın oluşturduğu kimlik bilgileri hakkında daha fazla bilgi için bkz. [CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)](https://msdn.microsoft.com/library/mt270260.aspx).

## <a name="Network"></a>IP adresi seçme
SQL Server'ın uzak Azure sunucusu ile iletişim kurmasına izin veren bir güvenlik duvarı kuralını Azure'da oluşturmak için bir IP adresleri aralığı girin veya SQL Server'ınızın genel IP adresini kullanın.

Bu sayfada sağladığınız IP adresi veya adresleri, SQL Server tarafından başlatılmış gelen veriler, sorgular ve yönetim işlemlerinin Azure güvenlik duvarından geçmesine izin vermesini Azure sunucusuna bildirir. Sihirbaz SQL Server'daki güvenlik duvarı ayarlarında hiçbir şeyi değiştirmez.

![Esnetme Veritabanı sihirbazı IP adresi seçme sayfası][StretchWizardImage7]

## <a name="Summary"></a>Özet
Sihirbazda girdiğiniz değerleri ve belirlediğiniz seçenekleri gözden geçirin. Ardından Esnetmeyi etkinleştirmek için **Son**'u seçin.

![Esnetme Veritabanı sihirbazı özet sayfası][StretchWizardImage8]

## <a name="Results"></a>Sonuçlar
Sonuçları gözden geçirin.

İsteğe bağlı olarak, Esnetme Veritabanı İzleyicisi'nde veri geçişinin durumunu izlemeyi başlatmak için **İzleyici**'yi seçin. Daha fazla bilgi için bkz. [Veri geçişini izleme ve sorun giderme (Esnetme Veritabanı)](sql-server-stretch-database-monitor.md).

## <a name="KnownIssues"></a>Sihirbaz sorunlarını giderme
**Esnetme Veritabanı sihirbazı başarısız oldu.**
Esnetme Veritabanı henüz sunucu düzeyinde etkinleştirilmediyse ve Esnetme Veritabanı'nı etkinleştirmek için gerekli olan sistem yöneticisi izinleri olmadan sihirbazı çalıştırırsanız sihirbaz başarısız olur. Sistem yöneticisinden Esnetme Veritabanı'nı yerel sunucu örneğinde etkinleştirmesini isteyin ve ardından sihirbazı yeniden çalıştırın. Daha fazla bilgi için bkz. [Önkoşul: Esnetme Veritabanı'nı sunucuda etkinleştirme izni](sql-server-stretch-database-enable-database.md#EnableTSQLServer).

## Sonraki adımlar
Esnetme Veritabanı için ek tablolar etkinleştirin. Veri geçişini izleyin ve Esnetme etkinleştirilmiş veritabanlarını ve tabloları yönetin.

-   Ek tablolar etkinleştirmek için [Esnetme Veritabanı'nı bir tablo için etkinleştirin](sql-server-stretch-database-enable-table.md).

-   Veri geçişinin durumunu görmek için [Esnetme Veritabanı'nı İzleyin](sql-server-stretch-database-monitor.md).

-   [Esnetme Veritabanı'nı duraklatma ve sürdürme](sql-server-stretch-database-pause.md)

-   [Esnetme Veritabanı'nı yönetme ve sorun giderme](sql-server-stretch-database-manage.md)

-   [Esnetme etkinleştirilmiş veritabanlarını yedekleme ve geri yükleme](sql-server-stretch-database-backup.md)

## Ayrıca bkz.

[Esnetme Veritabanı'nı bir veritabanı için etkinleştirme](sql-server-stretch-database-enable-database.md)

[Esnetme Veritabanı'nı bir tablo için etkinleştirme](sql-server-stretch-database-enable-table.md)

[StretchWizardImage1]: ./media/sql-server-stretch-database-wizard/stretchwiz1.png
[StretchWizardImage2]: ./media/sql-server-stretch-database-wizard/stretchwiz2.png
[StretchWizardImage2a]: ./media/sql-server-stretch-database-wizard/stretchwiz2a.png
[StretchWizardImage2b]: ./media/sql-server-stretch-database-wizard/stretchwiz2b.png
[StretchWizardImage3]: ./media/sql-server-stretch-database-wizard/stretchwiz3.png
[StretchWizardImage4]: ./media/sql-server-stretch-database-wizard/stretchwiz4.png
[StretchWizardImage5]: ./media/sql-server-stretch-database-wizard/stretchwiz5.png
[StretchWizardImage6]: ./media/sql-server-stretch-database-wizard/stretchwiz6.png
[StretchWizardImage7]: ./media/sql-server-stretch-database-wizard/stretchwiz7.png
[StretchWizardImage8]: ./media/sql-server-stretch-database-wizard/stretchwiz8.png



<!----HONumber=Jun16_HO2-->


