---
title: "Esnetme için Veritabanını Etkinleştirme Sihirbazını çalıştırarak kullanmaya başlama | Microsoft Belgeleri"
description: "Esnetme İçin Veritabanını Etkinleştirme Sihirbazını çalıştırarak, bir veritabanının Esnetme Veritabanı için nasıl yapılandırılacağını öğrenin."
services: sql-server-stretch-database
documentationcenter: 
author: douglaslMS
manager: jhubbard
editor: 
ms.assetid: 1189ab95-ba84-459c-bfb1-57cdf36ee111
ms.service: sql-server-stretch-database
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 08/05/2016
ms.author: douglasl
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 0c171da72bbdbfc8c15c6e39fcc8d5000f6be087


---
# <a name="get-started-by-running-the-enable-database-for-stretch-wizard"></a>Esnetme İçin Veritabanını Etkinleştirme Sihirbazını çalıştırarak kullanmaya başlama
Bir veritabanını Esnetme Veritabanı için çalıştırmak üzere, Esnetme İçin Veritabanını Etkinleştirme Sihirbazını çalıştırın.  Bu konu başlığında, sihirbazda girmeniz gereken bilgiler ve yapmanız gereken seçimler açıklanmaktadır.

Esnetme Veritabanı hakkında daha fazla bilgi edinmek için bkz. [Esnetme Veritabanı](sql-server-stretch-database-overview.md).

> [!NOTE]
> Daha sonra Esnetme Veritabanı'nı devre dışı bırakmak isterseniz, bir tablo veya veritabanı için Esnetme Veritabanı'nın devre dışı bırakılmasının uzak nesnenin silinmesine neden olmadığını unutmayın. Uzak tabloyu veya uzak veritabanını silmek istiyorsanız Azure yönetim portalını kullanarak söz konusu tabloyu veya veritabanını kaldırmanız gerekir. Uzak nesneleri elle silene kadar bunlar için Azure ücreti ödemeye devam edersiniz. 
> 
> 

## <a name="launch-the-wizard"></a>Sihirbazı başlatma
1. SQL Server Management Studio'daki Nesne Gezgini'nde Esnetmeyi etkinleştirmek istediğiniz veritabanını seçin.
2. \-Sağ tıklayın, **Görevler**'i ve **Esnet**'i seçin, ardından **Etkinleştir**'i seçerek sihirbazı başlatın.

## <a name="a-nameintroaintroduction"></a><a name="Intro"></a>Giriş
Sihirbazın amacını ve önkoşullarını gözden geçirin.

Önemli önkoşullar şunlardır:

* Veritabanı ayarlarını değiştirmek için yönetici olmanız gerekir.
* Microsoft Azure aboneliğinizin olması gerekir.
* SQL Server uzak Azure sunucusuyla iletişim kurabilmelidir.

![Esnetme Veritabanı sihirbazı giriş sayfası][StretchWizardImage1]

## <a name="a-nametablesaselect-tables"></a><a name="Tables"></a>Tabloları seçme
Esnetme için etkinleştirmek istediğiniz tabloları seçin.

Sıralanmış listenin üst kısmında çok sayıda satır içeren tablolar görünür. Sihirbaz tabloların listesini göstermeden önce Esnetme Veritabanı tarafından o anda desteklenmeyen veri türleri için analiz eder.

![Esnetme Veritabanı sihirbazı tabloları seçme sayfası][StretchWizardImage2]

| Sütun | Açıklama |
| --- | --- |
| (başlık yok) |Seçilen tabloyu Esnetme için etkinleştirmek üzere bu sütundaki onay kutusunu işaretleyin. |
| **Adı** |Tablodaki sütunun adını belirtir. |
| (başlık yok) |Bu sütundaki bir simge, Esnetme için seçili tabloyu etkinleştirmenizi engellemeyen bir uyarıyı temsil edebilir\'. Ayrıca, tablo desteklenmeyen bir veri türü kullandığından Esnetme \- için seçili tabloyu etkinleştirmenizi engelleyen bir engelleme sorununu temsil edebilir. Simgenin üzerine gelerek bir araç ipucunda daha fazla bilgi görüntüleyin. Daha fazla bilgi için bkz. [Esnetme Veritabanı Sınırlamaları](sql-server-stretch-database-limitations.md). |
| **Esnetilmiş** |Esnetme için tablonun zaten etkin olup olmadığını gösterir. |
| **Geçirme** |Tablonun tamamını geçirebilir (**Tüm Tablo**) veya tabloda var olan bir sütun üzerinde bir filtre belirtebilirsiniz. Geçirilecek satırları seçmek için farklı bir filtre işlevi kullanmak isterseniz sihirbazdan çıktıktan sonra filtre işlevini belirtmek için ALTER TABLE deyimini çalıştırın. Filtre işlevi hakkında daha fazla bilgi için bkz. [Filtre işlevi kullanarak geçiş yapmak için satır seçme](sql-server-stretch-database-predicate-function.md). İşlevi uygulama hakkında daha fazla bilgi için bkz. [Esnetme Veritabanı'nı bir tablo için etkinleştirme](sql-server-stretch-database-enable-table.md) veya [ALTER TABLE (Transact-SQL)](https://msdn.microsoft.com/library/ms190273.aspx). |
| **Satırlar** |Tablodaki satır sayısını belirtir. |
| **Boyut (KB)** |Tablo boyutunu KB cinsinden belirtir. |

## <a name="a-namefilteraoptionally-provide-a-row-filter"></a><a name="Filter"></a>İsteğe bağlı olarak bir satır filtresi belirtin
Geçirilecek satırları seçmek için filtre işlevi sağlamak isterseniz **Tablo seçin** sayfasında aşağıdaki işlemleri yapın.

1. **Esnetmek istediğiniz tabloları seçin** listesinde, tablonun satırında **Tüm Tablo**'ya tıklayın. **Esnetme için satırları seçin** iletişim kutusu açılır.
   
   ![Filter işlevi tanımlama][StretchWizardImage2a]
2. **Esnetme için satırları seçin** iletişim kutusunda, **Satırları Seçin** öğesini belirleyin.
3. **Ad alanında** filtre işlevi için bir ad sağlayın.
4. **Where** yan tümcesi için tablodan bir sütun ile bir işleç seçin ve bir değer belirtin.
5. İşlevi test etmek için **Denetle**'ye tıklayın. İşlev tablodan sonuçlar döndürüyorsa yani şartı karşılayan geçirilecek satırlar varsa test **Başarılı** sonucunu raporlar.
   
   > [!NOTE]
   > Filtre sorgusunu gösteren metin kutusu salt okunurdur. Metin kutusundaki sorguyu düzenleyemezsiniz.
   > 
   > 
6. **Tabloları seçme** sayfasına dönmek için Bitti'ye tıklayın.

SQL Server’da filtre işlevi yalnızca sihirbazı tamamladığınızda oluşturulur. O zamana kadar **Tablo seçin** sayfasına geri dönerek filtre işlevini değiştirebilir veya yeniden adlandırabilirsiniz.

![Bir filtre işlevi tanımlandıktan sonra Tablo Seçin sayfası][StretchWizardImage2b]

Geçirilecek satırları seçmek için farklı bir filtre işlevi türü sağlamak isterseniz aşağıdaki işlemlerden birini yapın.  

* Sihirbazdan çıkın ve ALTER TABLE deyimini çalıştırarak tabloda Esnetmeyi etkinleştirin ve bir filtre işlevi belirtin. Daha fazla bilgi için bkz. [Esnetme Veritabanı'nı bir tablo için etkinleştirme](sql-server-stretch-database-enable-table.md).  
* Sihirbazdan çıktıktan sonra bir filtre işlevi belirtmek için ALTER TABLE deyimini çalıştırın. Gerekli adımlar için bkz. [Sihirbazı çalıştırdıktan sonra filtre işlevi ekleme](sql-server-stretch-database-predicate-function.md#addafterwiz).

## <a name="a-nameconfigureaconfigure-azure-deployment"></a><a name="Configure"></a>Azure dağıtımını yapılandırma
1. Microsoft Azure'da bir Microsoft hesabı ile oturum açın.
   
   ![Azure'da oturum açma - Esnetme Veritabanı sihirbazı][StretchWizardImage3]
2. Esnetme Veritabanı için kullanılacak mevcut Azure aboneliğini seçin.
3. Bir Azure bölgesi seçin.
   
   * Yeni bir sunucu oluşturursanız sunucu bu bölgede oluşturulur.  
   * Seçili bölgede mevcut sunucularınız varsa **Mevcut sunucu**’yu seçtiğinizde sihirbaz bunları listeler.
   
   Gecikmeyi en aza indirmek üzere Azure bölgesi için SQL Server'ınızın bulunduğu bölgeyi seçin. Bölgeler hakkında daha fazla bilgi için bkz. [Azure Bölgeleri](https://azure.microsoft.com/regions/).
4. Var olan bir sunucuyu kullanmak mı yoksa yeni bir Azure sunucusu oluşturmak mı istediğinizi belirtin.
   
   SQL Server'ınızdaki Active Directory, Azure Active Directory ile birleştirildiyse isteğe bağlı olarak SQL Server'ın uzak Azure sunucusu ile iletişim kurması için birleştirilmiş bir hizmet hesabı kullanabilirsiniz. Bu seçeneğin gereksinimleri hakkında daha fazla bilgi için bkz. [ALTER DATABASE SET Seçenekleri (Transact-SQL)](https://msdn.microsoft.com/library/bb522682.aspx).
   
   * **Yeni sunucu oluşturma**
     
     1. Sunucu yöneticisi için bir oturum açma adı ve parola oluşturun.
     2. İsteğe bağlı olarak, SQL Server'ın uzak Azure sunucusu ile iletişim kurması için birleştirilmiş bir hizmet hesabı kullanın.
     
     ![Yeni Azure sunucusu oluşturma - Esnetme Veritabanı sihirbazı][StretchWizardImage4]
   * **Mevcut sunucu**
     
     1. Mevcut Azure sunucusunu seçin.
     2. Kimlik doğrulama yöntemini seçin.
        
        * **SQL Server Kimlik Doğrulaması** seçeneğini belirlerseniz yönetici adı ve parola oluşturun.
        * SQL Server'ın uzak Azure sunucusu ile iletişim kurması için birleştirilmiş bir hizmet hesabı kullanmak üzere **Active Directory Tümleşik Kimlik Doğrulama**'yı seçin. Seçili sunucu Azure Active Directory ile tümleşik değilse bu seçenek görünmez.
     
     ![Var olan Azure sunucusunu kullanma - Esnetme Veritabanı sihirbazı][StretchWizardImage5]

## <a name="a-namecredentialsasecure-credentials"></a><a name="Credentials"></a>Güvenli kimlik bilgileri
Esnetme Veritabanı'nın uzak veritabanına bağlanırken kullandığı kimlik bilgilerini güvenli hale getirmek için bir veritabanı ana anahtarınızın olması gerekir.  

Bir veritabanı ana anahtarı zaten varsa bunun parolasını girin.  

![Esnetme Veritabanı sihirbazı güvenli kimlik bilgileri sayfası][StretchWizardImage6b]

Veritabanında mevcut bir ana anahtar yoksa güçlü bir parola girerek veritabanı ana anahtarını oluşturun.  

![Esnetme Veritabanı sihirbazı güvenli kimlik bilgileri sayfası][StretchWizardImage6]

Veritabanı ana anahtarı hakkında daha fazla bilgi için bkz. [CREATE MASTER KEY (Transact-SQL)](https://msdn.microsoft.com/library/ms174382.aspx) ve [Veritabanı Ana Anahtarı Oluşturma](https://msdn.microsoft.com/library/aa337551.aspx). Sihirbazın oluşturduğu kimlik bilgileri hakkında daha fazla bilgi için bkz. [CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)](https://msdn.microsoft.com/library/mt270260.aspx).

## <a name="a-namenetworkaselect-ip-address"></a><a name="Network"></a>IP adresi seçme
Azure’da SQL Server’ın uzak Azure sunucusu ile iletişim kurmasına izin veren bir güvenlik duvarı kuralı oluşturmak için alt ağ IP adresi aralığını (önerilir) veya SQL Server ortak IP adresini kullanın.

Bu sayfada sağladığınız IP adresi veya adresleri, SQL Server tarafından başlatılmış gelen veriler, sorgular ve yönetim işlemlerinin Azure güvenlik duvarından geçmesine izin vermesini Azure sunucusuna bildirir. Sihirbaz SQL Server'daki güvenlik duvarı ayarlarında hiçbir şeyi değiştirmez.

![Esnetme Veritabanı sihirbazı IP adresi seçme sayfası][StretchWizardImage7]

## <a name="a-namesummaryasummary"></a><a name="Summary"></a>Özet
Sihirbazda girdiğiniz değerler ile belirlediğiniz seçeneklerin yanı sıra Azure’da tahmin edilen maliyetleri gözden geçirin. Ardından Esnetmeyi etkinleştirmek için **Son**'u seçin.

![Esnetme Veritabanı sihirbazı özet sayfası][StretchWizardImage8]

## <a name="a-nameresultsaresults"></a><a name="Results"></a>Sonuçlar
Sonuçları gözden geçirin.

Veri geçişinin durumunu izlemek için bkz. [Veri geçişini izleme ve sorun giderme (Esnetme Veritabanı)](sql-server-stretch-database-monitor.md).

![Esnetme Veritabanı sihirbazı sonuç sayfası][StretchWizardImage9]

## <a name="a-nameknownissuesatroubleshooting-the-wizard"></a><a name="KnownIssues"></a>Sihirbaz sorunlarını giderme
**Esnetme Veritabanı sihirbazı başarısız oldu.**
Esnetme Veritabanı henüz sunucu düzeyinde etkinleştirilmediyse ve Esnetme Veritabanı'nı etkinleştirmek için gerekli olan sistem yöneticisi izinleri olmadan sihirbazı çalıştırırsanız sihirbaz başarısız olur. Sistem yöneticisinden Esnetme Veritabanı'nı yerel sunucu örneğinde etkinleştirmesini isteyin ve ardından sihirbazı yeniden çalıştırın. Daha fazla bilgi için bkz. [Önkoşul: Esnetme Veritabanı'nı sunucuda etkinleştirme izni](sql-server-stretch-database-enable-database.md#EnableTSQLServer).

## <a name="next-steps"></a>Sonraki adımlar
Esnetme Veritabanı için ek tablolar etkinleştirin. Veri geçişini izleyin ve Esnetme etkinleştirilmiş \-veritabanlarını ve tabloları yönetin.

* Ek tablolar etkinleştirmek için [Esnetme Veritabanı'nı bir tablo için etkinleştirin](sql-server-stretch-database-enable-table.md).
* Veri geçişinin durumunu görmek için bkz. [Veri geçişini izleme ve sorun giderme](sql-server-stretch-database-monitor.md).
* [Esnetme Veritabanı'nı duraklatma ve sürdürme](sql-server-stretch-database-pause.md)
* [Esnetme Veritabanı'nı yönetme ve sorun giderme](sql-server-stretch-database-manage.md)
* [Esnetme özellikli veritabanlarını yedekleme](sql-server-stretch-database-backup.md)

## <a name="see-also"></a>Ayrıca bkz.
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
[StretchWizardImage6b]: ./media/sql-server-stretch-database-wizard/stretchwiz6b.png
[StretchWizardImage7]: ./media/sql-server-stretch-database-wizard/stretchwiz7.png
[StretchWizardImage8]: ./media/sql-server-stretch-database-wizard/stretchwiz8.png
[StretchWizardImage9]: ./media/sql-server-stretch-database-wizard/stretchwiz9.png



<!--HONumber=Nov16_HO2-->


