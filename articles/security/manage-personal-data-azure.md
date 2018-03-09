---
title: "Microsoft Azure kişisel verileri yönetmek | Microsoft Docs"
description: "nasıl düzeltin, güncelleştirme, silme ve Azure Active Directory ve Azure SQL veritabanı genel veri koruma düzenleme (GDPR) ile uyumlu yardımcı olması için kişisel verileri dışarı aktarma"
services: security
documentationcenter: na
author: barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/06/2018
ms.author: barclayn
ms.openlocfilehash: 41c0cc4eb3697aa79abeabddc98a84598ce4ea50
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
---
# <a name="manage-personal-data-in-microsoft-azure"></a>Microsoft Azure kişisel verileri yönetmek

Bu makale, düzeltin, güncelleştirme, silme ve Azure Active Directory ve Azure SQL veritabanı genel veri koruma düzenleme (GDPR) ile uyum sağlamak için kişisel verileri dışarı aktarma konusunda rehberlik sağlar.

## <a name="scenario"></a>Senaryo

Üst uç hedef Düğünler İrlanda ve dünyanın her iki bir yerel ve uluslararası müşteri için temel için tek adrestir alışveriş Dublin tabanlı şirket sağlar. Ofisleri, müşteriler, çalışanlar ve bunlar sunar görebildikleri tam olarak hizmet vermek için Satıcılar tüm dünyada bulunan sahiptirler.

Diğer birçok öğeler arasında şirket yemek Alerjiler ve dietary Tercihler dahil LCV'ler izler. Evlilik konuklar arabası, horseback gözatan, bot gibi üstündeçalýþan, vb. için çeşitli etkinlikleri kaydetmek ve hatta birbiriyle merkezi bir web sayfasında olayla ay sırasında etkileşim. Şirket, çalışanlar, satıcılar, müşteriler ve Evlilik konuklar kişisel bilgileri toplar. İş uluslararası yapısı nedeniyle şirket düzenleme birden çok düzeyi ile uyumlu olmalıdır.

## <a name="problem-statement"></a>Sorun bildirimi

- Veri admins doğru yanlış kişisel bilgileri ve güncelleştirme eksik ya da değişen kişisel bilgilere sahip olması gerekir.

- Veri admins veri konu istek üzerine kişisel bilgileri silmek mümkün olması gerekir.

- Verileri dışarı aktarma ve veri konu ortak, yapılandırılmış bir biçimde bilgilendirilmesine istek üzerine sağlamak veri admins gerekir.

## <a name="company-goals"></a>Şirketin hedeflerine

- Yanlış veya tamamlanmamış müşteri, Evlilik konuk, çalışan ve satıcı kişisel bilgileri düzeltildi veya gerekir Azure Active Directory ve Azure SQL veritabanı güncelleştirildi.

- Kişisel bilgiler Azure Active Directory ve Azure SQL veritabanı veri konu istek üzerine silinmesi gerekir.

- Kişisel veriler ortak, yapılandırılmış bir biçimde bir veri konu istek üzerine aktarılması gerekir.

## <a name="solutions"></a>Çözümler

### <a name="azure-active-directory-rectifycorrect-inaccurate-or-incomplete-personal-data-and-erasedelete-personal-datauser-profiles"></a>Azure Active Directory: yanlış veya tamamlanmamış kişisel verileri düzeltmek/düzeltin ve silme/kişisel veri/kullanıcı profillerini Sil

[Azure Active Directory](https://azure.microsoft.com/services/active-directory/) Microsoft'un bulut tabanlı, çok kiracılı dizin ve kimlik yönetimi hizmetidir.
Düzeltin, güncelleştirmek veya müşteri ve çalışan kullanıcı profilleri ve bir kullanıcının adını, iş başlığı, adresi veya telefon numarası gibi kişisel verileri içerdiğini kullanıcı iş bilgilerini silmek, [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) (AAD) kullanarak ortam [Azure portal](https://portal.azure.com/).

Dizin için genel yönetici olan bir hesapla oturum gerekir.

#### <a name="how-do-i-correct-or-update-user-profile-and-work-information-in-azure-active-directory"></a>Nasıl düzeltin veya kullanıcı profili ve iş güncelleştirme bilgileri Azure Active Directory'de?

1. Oturum [Azure portal](https://portal.azure.com) dizini için genel yönetici olan bir hesapla.

2. Seçin **tüm hizmetleri**, girin **kullanıcılar ve gruplar** metin kutusuna ve ardından **Enter**.

    ![media/image1.png](media/manage-personal-data-azure/image001.png)

3. Üzerinde **kullanıcılar ve gruplar** dikey penceresinde, select **kullanıcılar**.

    ![media/image2.png](media/manage-personal-data-azure/image003.png)

4. Üzerinde **kullanıcılar ve gruplar - kullanıcıların** dikey penceresinde, listeden bir kullanıcı seçin ve ardından dikey penceresinde seçili kullanıcı, seçin **profil** düzeltildi veya güncelleştirilmesi için gereken kullanıcı profili bilgilerini görüntülemek için.

    ![media/image3.png](media/manage-personal-data-azure/image005.png)

5. Düzeltin veya bilgileri güncelleştirmek ve ardından komut çubuğunda seçin **kaydedin.**

6.  Seçilen kullanıcı için dikey seçin **iş bilgisi** düzeltildi veya güncelleştirilmesi için gereken kullanıcı iş bilgilerini görüntülemek için.

    ![media/image4.png](media/manage-personal-data-azure/image007.png)

7. Düzeltin veya kullanıcı iş bilgilerini güncelleştirin ve ardından komut çubuğunda seçin **kaydedin.**

#### <a name="how-do-i-delete-a-user-profile-in-azure-active-directory"></a>Azure Active Directory'de bir kullanıcı profili nasıl silebilirim?

1. Oturum [Azure portal](https://portal.azure.com) dizini için genel yönetici olan bir hesapla.

2. Seçin **tüm hizmetleri**, girin **kullanıcılar ve gruplar** metin kutusuna ve ardından **Enter**.

    ![](media/manage-personal-data-azure/image001.png)

3. Üzerinde **kullanıcılar ve gruplar** dikey penceresinde, select **kullanıcılar**.

    ![media/image2.png](media/manage-personal-data-azure/image003.png)

4. **Kullanıcılar ve gruplar - Kullanıcılar** dikey penceresinde, listeden bir kullanıcı seçin.

    ![media/image3.png](media/manage-personal-data-azure/image007.png)

5. Seçilen kullanıcı için dikey seçin **genel bakış**ve ardından komut çubuğunda **silmek**.

    ![](media/manage-personal-data-azure/image013.png)

### <a name="sql-database-rectifycorrect-inaccurate-or-incomplete-personal-data-erasedelete-personal-data-export-personal-data"></a>SQL veritabanı: düzeltin ve düzeltmek yanlış veya tamamlanmamış kişisel veri; Kişisel verileri silme veya silme; Kişisel verileri dışarı aktarma 

[Azure SQL veritabanı](https://azure.microsoft.com/services/sql-database/?v=16.50) yapı ve uygulamalarını geliştiriciler yardımcı olan bir bulut veritabanıdır.

Kişisel veriler güncelleştirilebilir [Azure SQL veritabanı](https://azure.microsoft.com/services/sql-database/?v=16.50) standart SQL sorguları kullanılarak da silinebilir. Ayrıca, kişisel verileri içeri ve Dışarı Aktarma Sihirbazı çeşitli yöntemler, Azure SQL Server dahil olmak üzere SQL veritabanı kullanma ve biçimleri, bir BACPAC dosyası dahil olmak üzere çeşitli aktarılabilir.

#### <a name="how-do-i-correct-update-or-erase-personal-data-in-sql-database"></a>Nasıl düzeltin, güncelleştirme veya SQL veritabanında kişisel verileri silme?

Düzeltmek veya kişisel verileri SQL veritabanında güncelleştirmek öğrenmek için ziyaret [güncelleştirme (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/queries/update-transact-sql), [güncelleştirme metin](https://docs.microsoft.com/sql/t-sql/queries/updatetext-transact-sql), [güncelleştirme ile ortak tablo ifadesinin](https://docs.microsoft.com/sql/t-sql/queries/with-common-table-expression-transact-sql), veya [Güncelleştirme metni yaz](https://docs.microsoft.com/sql/t-sql/queries/writetext-transact-sql) belgeleri.

SQL veritabanı kişisel verileri silme hakkında bilgi için [(Transact-SQL) Delete](https://docs.microsoft.com/sql/t-sql/statements/delete-transact-sql) belgeleri.

#### <a name="how-do-i-export-personal-data-to-a-bacpac-file-in-sql-database"></a>Nasıl ı kişisel verileri SQL veritabanındaki bir BACPAC dosyası dışarı?

Bir BACPAC dosya SQL veritabanı veri ve meta verileri içerir ve bir BACPAC uzantılı bir zip dosyası. Bu yapılabilir kullanarak [Azure portal](https://portal.azure.com/), SQLPackage komut satırı yardımcı programı, SQL Server Management Studio (SSMS) veya PowerShell.

Verilerini bir BACPAC dosyasına verme konusunda bilgi almak için şurayı ziyaret edin [bir Azure SQL veritabanı BACPAC dosyaya](https://docs.microsoft.com/azure/sql-database/sql-database-export) sayfasında, yukarıda listelenen her bir yöntemin ayrıntılı yönergeler içerir.

Nasıl ı kişisel verileri SQL veritabanı SQL Server içeri ve Dışarı Aktarma Sihirbazı ile verilecek?

Bu sihirbaz verileri bir kaynaktan bir hedefe kopyalamanıza yardımcı olur. Alma Sihirbazı'na giriş için de dahil olmak üzere izin bilgileri ve aracı ile ilgili Yardım almak için ziyaret nasıl [içeri ve dışarı aktarma veri SQL Server içeri ve Dışarı Aktarma Sihirbazı ile](https://docs.microsoft.com/sql/integration-services/import-export-data/import-and-export-data-with-the-sql-server-import-and-export-wizard) web sayfası.

Sihirbazın adımlarını genel bakış için ziyaret [SQL Server içeri ve Dışarı Aktarma Sihirbazı'nda adımları](https://docs.microsoft.com/sql/integration-services/import-export-data/steps-in-the-sql-server-import-and-export-wizard) web sayfası.

## <a name="next-steps"></a>Sonraki Adımlar:

[Azure SQL Veritabanı](https://azure.microsoft.com/services/sql-database/?v=16.50) 

[Azure Active Directory](https://azure.microsoft.com/services/active-directory/)

