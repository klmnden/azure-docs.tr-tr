---
title: Tek veya havuza alınmış bir veritabanını Azure SQL veritabanı'nda güvenli | Microsoft Docs
description: Size öğretir teknikleri ve tek veya havuza alınmış veritabanını Azure SQL veritabanı koruma özellikleri hakkında bir öğretici.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.topic: tutorial
author: VanMSFT
ms.author: vanto
ms.reviewer: carlrab
manager: craigg
ms.date: 02/08/2019
ms.custom: seoapril2019
ms.openlocfilehash: d09af0a4c2d09004d5c1bbf3261a14850eef7714
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60582711"
---
# <a name="tutorial-secure-a-single-or-pooled-database"></a>Öğretici: Tek veya havuza alınmış veritabanını koruma

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> - Sunucu düzeyinde ve veritabanı düzeyinde güvenlik duvarı kuralları oluşturma
> - Bir Azure Active Directory (AD) Yöneticisi'ni yapılandırma
> - SQL kimlik doğrulaması, Azure AD kimlik doğrulaması ve güvenli bağlantı dizeleri kullanıcı erişimini yönetme
> - Gelişmiş veri güvenliği, denetimi, veri maskeleme ve şifreleme gibi güvenlik özelliklerini etkinleştirme

Azure SQL veritabanı, tek veya havuza veritabanı içindeki veriler olanak sağlayarak korur:

- Güvenlik duvarı kurallarını kullanarak erişimi sınırlama
- Kimlik gerektiren kimlik doğrulama mekanizmaları kullanma
- Rol tabanlı üyelikler ve izinler ile yetkilendirme kullanın.
- Güvenlik özelliklerini etkinleştirme

> [!NOTE]
> Ağ güvenlik kuralları ve özel uç noktaları makalesinde açıklanan şekilde kullanarak bir Azure SQL veritabanı yönetilen örneğinde güvenli [Azure SQL veritabanı yönetilen örneği](sql-database-managed-instance-index.yml) ve [bağlantı mimarisi](sql-database-managed-instance-connectivity-architecture.md).

Daha fazla bilgi için bkz. [Azure SQL veritabanı güvenliğine genel bakış](/azure/sql-database/sql-database-security-index) ve [özellikleri](sql-database-security-overview.md) makaleler.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdaki önkoşulların karşılandığından emin olun:

- [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms)
- Bir Azure SQL server ve veritabanı
  - Oluşturabilir [Azure portalında](sql-database-single-database-get-started.md), [CLI](sql-database-cli-samples.md), veya [PowerShell](sql-database-powershell-samples.md)

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

Bu öğreticideki tüm adımları için oturum açın [Azure portalı](https://portal.azure.com/)

## <a name="create-firewall-rules"></a>Güvenlik duvarı kuralları oluşturma

SQL veritabanları, Azure güvenlik duvarları tarafından korunur. Varsayılan olarak, sunucu ve veritabanı için tüm bağlantılar, diğer Azure hizmetlerinden gelen bağlantılar dışında reddedilir. Daha fazla bilgi için bkz. [Azure SQL veritabanı sunucu düzeyinde ve veritabanı düzeyinde güvenlik duvarı kuralları](sql-database-firewall-configure.md).

Ayarlama **Azure hizmetlerine erişime izin ver** için **OFF** için en güvenli yapılandırma. Ardından, oluşturun bir [ayrılmış IP (Klasik dağıtım)](../virtual-network/virtual-networks-reserved-public-ip.md) , bir Azure sanal makine veya Bulut hizmeti gibi bağlanmak ve yalnızca bu IP adresi erişim güvenlik duvarı üzerinden izin vermek için gereken kaynak için. Kullanıyorsanız [Kaynak Yöneticisi'ni](/azure/virtual-network/virtual-network-ip-addresses-overview-arm) dağıtım modeli, ayrılmış genel IP adresi için her bir kaynak gerekli.

> [!NOTE]
> SQL Veritabanı 1433 numaralı bağlantı noktası üzerinden iletişim kurar. Kurumsal ağ içinden gelen bağlanmaya çalışıyorsanız, ağınızın güvenlik duvarı tarafından 1433 numaralı bağlantı noktası üzerinden giden trafiğe verilmez. Bu durumda, yöneticinize 1433 numaralı bağlantı noktasını açmadığı sürece Azure SQL veritabanı sunucusuna bağlanamıyor.

### <a name="set-up-sql-database-server-firewall-rules"></a>SQL veritabanı sunucusu güvenlik duvarı kurallarını ayarlayın

Sunucu düzeyinde IP güvenlik duvarı kuralları, aynı SQL veritabanı sunucu içindeki tüm veritabanlarına uygulayın.

Sunucu düzeyinde güvenlik duvarı kurallarını ayarlamak için:

1. Azure portalında **SQL veritabanları** sol menüdeki ve veritabanınızı seçin **SQL veritabanları** sayfası.

    ![sunucu güvenlik duvarı kuralı](./media/sql-database-security-tutorial/server-name.png)

    > [!NOTE]
    > Tam sunucu adınız kopyaladığınızdan emin olun (gibi *yourserver.database.windows.net*) öğreticinin ilerleyen bölümlerinde kullanmak için.

1. Üzerinde **genel bakış** sayfasında **sunucu güvenlik duvarını Ayarla**. **Güvenlik Duvarı ayarları** veritabanı sunucusu için sayfası açılır.

   1. Seçin **istemci IP'si Ekle** geçerli IP adresinizi yeni bir güvenlik duvarı kuralına eklemek için araç çubuğunda. Kural, tek bir IP adresi veya bir IP adresi aralığı için 1433 numaralı bağlantı noktasını açabilirsiniz. **Kaydet**’i seçin.

      ![sunucu güvenlik duvarı kuralı ayarla](./media/sql-database-security-tutorial/server-firewall-rule2.png)

   1. Seçin **Tamam** kapatın **Güvenlik Duvarı ayarları** sayfası.

Artık sunucuda belirtilen IP adresine veya IP adresi aralığına sahip herhangi bir veritabanına bağlanabilirsiniz.

> [!IMPORTANT]
> Varsayılan olarak, SQL veritabanı güvenlik duvarı üzerinden erişim tüm Azure Hizmetleri için altında etkin **Azure hizmetlerine erişime izin ver**. Seçin **OFF** erişim tüm Azure Hizmetleri için devre dışı bırakmak için.

### <a name="setup-database-firewall-rules"></a>Veritabanı güvenlik duvarı kuralları ayarla

Veritabanı düzeyinde güvenlik duvarı kuralları yalnızca tek tek veritabanları için geçerlidir. Veritabanı bu kurallar, bir sunucu yük devretme sırasında korur. Veritabanı düzeyinde güvenlik duvarı kuralları yalnızca Transact-SQL (T-SQL) deyimleri kullanılarak yapılandırılabilir ve sonra yalnızca sunucu düzeyinde güvenlik duvarı kuralı yapılandırdınız.

Veritabanı düzeyinde güvenlik duvarı kuralı ayarlamak için:

1. Örneğin kullanarak veritabanına bağlanma [SQL Server Management Studio](./sql-database-connect-query-ssms.md).

1. İçinde **Nesne Gezgini**, veritabanına sağ tıklayın ve seçin **yeni sorgu**.

1. Sorgu penceresinde, bu deyimi ekleyin ve genel IP adresi IP adresini değiştirin:

    ```sql
    EXECUTE sp_set_database_firewall_rule N'Example DB Rule','0.0.0.4','0.0.0.4';
    ```

1. Araç çubuğunda **yürütme** güvenlik duvarı kuralı oluşturun.

> [!NOTE]
> Kullanarak SSMS'de sunucu düzeyinde güvenlik duvarı kuralı oluşturabilirsiniz [sp_set_firewall_rule](/sql/relational-databases/system-stored-procedures/sp-set-firewall-rule-azure-sql-database?view=azuresqldb-current) bağlı olmanız gerekir ancak komut *ana* veritabanı.

## <a name="create-an-azure-ad-admin"></a>Bir Azure AD Yöneticisi oluşturma

Uygun Azure Active Directory (AD) yönetilen etki alanını kullandığınızdan emin olun. AD etki alanı seçmek için Azure portalının sağ üst köşenin kullanın. Aynı abonelik, her ikisi için kullanılır, bu işlemi onaylar Azure AD ve Azure SQL veritabanını veya veri ambarını barındıran SQL Server.

   ![ad seçin](./media/sql-database-security-tutorial/8choose-ad.png)

Azure AD Yöneticisi ayarlamak için:

1. Azure portalında üzerinde **SQL server** sayfasında **Active Directory Yöneticisi**. Ardından **yönetici Ayarla**.

    ![active directory seçme](./media/sql-database-security-tutorial/admin-settings.png)  

    > [!IMPORTANT]
    > "Şirket Yöneticisi" veya "Genel yönetici" Bu görevi gerçekleştirmek için olmanız gerekir.

1. Üzerinde **yönetici Ekle** sayfa, arama ve AD kullanıcısı veya grubu seçin ve seçin **seçin**. Tüm üyeleri ve grupları Active Directory sitelerinizden listelenir ve gri renkte girişler, Azure AD yönetici olarak desteklenmez. Bkz: [Azure AD özellikleri ve sınırlamaları](sql-database-aad-authentication.md#azure-ad-features-and-limitations).

    ![Yönetici Seç](./media/sql-database-security-tutorial/admin-select.png)

    > [!IMPORTANT]
    > Rol tabanlı erişim denetimi (RBAC), yalnızca portala uygular ve SQL Server yayılan değil.

1. Üst kısmındaki **Active Directory Yöneticisi** sayfasında **Kaydet**.

    Yönetici değiştirme işlemini birkaç dakika sürebilir. Yeni yönetici görünür **Active Directory Yöneticisi** kutusu.

> [!NOTE]
> Bir Azure AD Yöneticisi ayarlarken, yeni yönetici adı (kullanıcı veya grup) bir SQL Server kimlik doğrulaması kullanıcı var olamaz *ana* veritabanı. Varsa, kurulum başarısız ve böyle bir yönetici adı zaten var olduğunu gösteren, değişiklikleri geri alma. SQL Server kimlik doğrulaması kullanıcı Azure AD parçası olmadığından, kullanıcının Azure AD kimlik doğrulaması kullanarak bağlanmak için her çabayı başarısız olur.

Yapılandırma Azure AD hakkında daha fazla bilgi için bkz:

- [Şirket içi kimliklerinizi Azure AD ile tümleştirme](../active-directory/hybrid/whatis-hybrid-identity.md)
- [Kendi etki alanı adınızı Azure AD'ye ekleme](../active-directory/active-directory-domains-add-azure-portal.md)
- [Microsoft Azure artık Windows Server AD ile Federasyonu destekliyor](https://azure.microsoft.com/blog/20../../windows-azure-now-supports-federation-with-windows-server-active-directory/)
- [Azure AD dizininizi yönetme](../active-directory/fundamentals/active-directory-administer.md)
- [PowerShell kullanarak Azure AD'yi yönetme](/powershell/azure/overview?view=azureadps-2.0)
- [Karma kimlik için gereken bağlantı noktaları ve protokoller](../active-directory/hybrid/reference-connect-ports.md)

## <a name="manage-database-access"></a>Veritabanı erişimi yönetme

Veritabanına kullanıcı ekleme veya bir kullanıcı erişimi ile güvenli bağlantı dizeleri tarafından veritabanı erişimi yönetin. Bağlantı dizeleri, dış uygulamalar için yararlıdır. Daha fazla bilgi için bkz. [Azure SQL erişim denetimi](sql-database-control-access.md) ve [AD kimlik doğrulaması](sql-database-aad-authentication.md).

Kullanıcıları eklemek için veritabanı kimlik doğrulaması türünü seçin:

- **SQL kimlik doğrulaması**, oturum açma için kullanıcı adı ve parola kullanın ve yalnızca sunucu içindeki belirli bir veritabanı bağlamında geçerli değil

- **Azure AD kimlik doğrulaması**, Azure AD tarafından yönetilen kimlikleri kullanın

### <a name="sql-authentication"></a>SQL kimlik doğrulaması

SQL kimlik doğrulaması ile bir kullanıcı eklemek için:

1. Örneğin kullanarak veritabanına bağlanma [SQL Server Management Studio](./sql-database-connect-query-ssms.md).

1. İçinde **Nesne Gezgini**, veritabanına sağ tıklayın ve seçin **yeni sorgu**.

1. Sorgu penceresinde aşağıdaki komutu girin:

    ```sql
    CREATE USER ApplicationUser WITH PASSWORD = 'YourStrongPassword1';
    ```

1. Araç çubuğunda **yürütme** kullanıcı oluşturmak için.

1. Varsayılan olarak, kullanıcı veritabanına bağlanabilir ancak verileri okuma veya yazma izni yoktur. Bu izinleri vermek için yeni bir sorgu penceresinde aşağıdaki komutları yürütün:

    ```sql
    ALTER ROLE db_datareader ADD MEMBER ApplicationUser;
    ALTER ROLE db_datawriter ADD MEMBER ApplicationUser;
    ```

> [!NOTE]
> Yeni kullanıcı oluşturmak gibi yönetici görevleri yürütmeniz gerekmiyorsa, yönetici olmayan hesapların veritabanı düzeyinde oluşturun.

### <a name="azure-ad-authentication"></a>Azure AD kimlik doğrulaması

Veritabanı kullanıcıları yer alan olarak oluşturduğunuz Azure Active Directory kimlik doğrulaması gerektirir. Bağımsız veritabanı kullanıcısı eşler veritabanıyla ilişkili Azure AD dizininde bir kimliğe ve oturum açma gerektirmeyen sahip *ana* veritabanı. Azure AD kimlik ya da tek bir kullanıcı veya grup için olabilir. Daha fazla bilgi için bkz [bağımsız veritabanı kullanıcıları veritabanınızı taşınabilir hale](https://msdn.microsoft.com/library/ff929188.aspx) ve gözden geçirme [Azure AD'ye öğretici](./sql-database-aad-authentication-configure.md) Azure AD kullanarak kimlik doğrulaması gerçekleştirmeyle ilgili.

> [!NOTE]
> Veritabanı kullanıcıları (Yöneticiler hariç), Azure portalını kullanarak oluşturulamıyor. Azure RBAC rolleri için SQL sunucuları, veritabanları veya veri ambarları yayılmaz. Bunlar, yalnızca Azure kaynaklarını yönetmek için kullanılır ve veritabanı izinleri geçerli değildir.
>
> Örneğin, *SQL Server Katılımcısı* rol, bir veritabanı veya veri ambarı'na bağlanmak için erişim vermek değil. Bu izni, T-SQL deyimlerini kullanarak veritabanı içinde verilmelidir.

> [!IMPORTANT]
> İki nokta üst üste gibi özel karakterler `:` ya da ve işareti `&` T-SQL kullanıcı adları desteklenmez `CREATE LOGIN` ve `CREATE USER` deyimleri.

Azure AD kimlik doğrulamasını bir kullanıcı eklemek için:

1. Bir Azure AD hesabı ile kullanarak Azure SQL sunucunuza bağlanmak en az *ALTER herhangi bir kullanıcı* izni.

1. İçinde **Nesne Gezgini**, veritabanına sağ tıklayın ve seçin **yeni sorgu**.

1. Sorgu penceresinde aşağıdaki komutu girin ve değiştirme `<Azure_AD_principal_name>` asıl adını bir Azure AD kullanıcısı veya Azure AD grubunun görünen adı:

   ```sql
   CREATE USER <Azure_AD_principal_name> FROM EXTERNAL PROVIDER;
   ```

> [!NOTE]
> Azure AD kullanıcıları, veritabanı meta verilerde türüyle işaretlenir `E (EXTERNAL_USER)` ve türü `X (EXTERNAL_GROUPS)` grupları için. Daha fazla bilgi için [sys.database_principals](/sql/relational-databases/system-catalog-views/sys-database-principals-transact-sql).

### <a name="secure-connection-strings"></a>Güvenli bağlantı dizeleri

İstemci uygulaması ve SQL veritabanı arasında güvenli, şifreli bir bağlantı sağlamak için bir bağlantı dizesi için yapılandırılması gerekir:

- Şifreli bir bağlantı isteği
- Sunucu sertifikasına güvenmeyecek

Bağlantı, Aktarım Katmanı Güvenliği (TLS) kullanarak oluşturulmuş ve bir adam-de-ortadaki adam saldırısı riskini azaltır. Bağlantı dizeleri veritabanı başına kullanılabilir ve ADO.NET, JDBC, ODBC ve PHP gibi istemci sürücüleri desteklemek için önceden yapılandırılmıştır. TLS ve bağlantı hakkında daha fazla bilgi için bkz. [TLS konuları](sql-database-connect-query.md#tls-considerations-for-sql-database-connectivity).

Güvenli bir bağlantı dizesi kopyalamak için:

1. Azure portalında **SQL veritabanları** sol menüdeki ve veritabanınızı seçin **SQL veritabanları** sayfası.

1. Üzerinde **genel bakış** sayfasında **veritabanı bağlantı dizelerini Göster**.

1. Bir sürücü sekmesini seçin ve tam bağlantı dizesini kopyalayın.

    ![ADO.NET bağlantı dizesi](./media/sql-database-security-tutorial/connection.png)

## <a name="enable-security-features"></a>Güvenlik özelliklerini etkinleştirme

Azure SQL veritabanı, Azure portalını kullanarak erişilen güvenlik özellikleri sağlar. Bu özellikler yalnızca veritabanı üzerinde kullanılabilir olduğu hem veritabanı hem de veri maskeleme dışında bir sunucu için kullanılabilir. Daha fazla bilgi için bkz. [gelişmiş veri güvenliği](sql-database-advanced-data-security.md), [denetim](sql-database-auditing.md), [dinamik veri maskeleme](sql-database-dynamic-data-masking-get-started.md), ve [saydam veri şifrelemesi](transparent-data-encryption-azure-sql.md).

### <a name="advanced-data-security"></a>Gelişmiş veri güvenliği

Gelişmiş Veri güvenlik özelliği olası tehditleri algılar, oluşur ve anormal etkinliklerde güvenlik uyarıları sağlar. Kullanıcılar denetim özelliğini kullanarak bu şüpheli etkinlikleri araştırıp ve olay erişim, güvenlik ihlali veya veritabanındaki verileri yararlanma olup olmadığını belirlemek. Kullanıcılar bir güvenlik açığı değerlendirmesi ve veri bulma ve sınıflandırma aracı içeren bir güvenliğine genel bakış da sağlanır.

> [!NOTE]
> Bir örnek tehdit SQL eklemesi, burada saldırganlar uygulama için girdiler olarak kötü amaçlı SQL ekleme bir işlem olarak. Uygulama daha sonra farkında olmadan kötü amaçlı SQL yürütme ve saldırganlar güvenlik ihlali veya veritabanındaki verileri değiştirme erişimi verin.

Gelişmiş veri güvenliği etkinleştirmek için:

1. Azure portalında **SQL veritabanları** sol menüdeki ve veritabanınızı seçin **SQL veritabanları** sayfası.

1. Üzerinde **genel bakış** sayfasında **sunucu adı** bağlantı. Veritabanı Sunucu sayfasına açılır.

1. Üzerinde **SQL server** sayfasında, bulmak **güvenlik** seçin ve bölüm **gelişmiş veri güvenliği**.

   1. Seçin **ON** altında **gelişmiş veri güvenliği** özelliği etkinleştirmek için. Güvenlik Açığı değerlendirme sonuçlarını kaydetmek için bir depolama hesabı seçin. Daha sonra **Kaydet**’e tıklayın.

      ![Gezinti bölmesi](./media/sql-database-security-tutorial/threat-settings.png)

      Ayrıca, güvenlik uyarıları, depolama ayrıntıları ve tehdit algılama türleri alacak e-postalar yapılandırabilirsiniz.

1. Geri dönüp **SQL veritabanları** sayfasını seçin ve veritabanı **gelişmiş veri güvenliği** altında **güvenlik** bölümü. Burada veritabanı için çeşitli güvenlik göstergeler bulabilirsiniz.

    ![Tehdit durumu](./media/sql-database-security-tutorial/threat-status.png)

Anormal etkinlikler algılanırsa, olayla ilgili bilgileri içeren bir e-posta alırsınız. Bu etkinlik, veritabanı, sunucu, olay saati, olası nedenleri yapısını içerir ve önerilen araştırıp olası tehdidi azaltmak için Eylemler. Bu tür e-posta aldıysanız seçin **Azure SQL denetim günlüğü** bağlantıyı Azure portalını başlatma ve etkinliğin saati için ilgili denetim kayıtlarını göstermek için.

   ![Tehdit algılama e-postası](./media/sql-database-security-tutorial/threat-email.png)

### <a name="auditing"></a>Denetim

Denetim özelliği, veritabanı olaylarını izler ve bir denetim günlüğüne ya da bir Azure depolama, Azure İzleyici günlüklerine veya olay hub'ına olayları yazar. Denetim mevzuatla uyumluluk, veritabanı etkinliğini anlama ve tutarsızlıklar ve olası güvenlik ihlallerini işaret edebilecek anomalileri kavramanıza yardımcı olur.

Denetimi etkinleştirmek için:

1. Azure portalında **SQL veritabanları** sol menüdeki ve veritabanınızı seçin **SQL veritabanları** sayfası.

1. İçinde **güvenlik** bölümünden **denetim**.

1. Altında **denetim** ayarları, aşağıdaki değerleri ayarlayın:

   1. Ayarlama **denetim** için **ON**.

   1. Seçin **denetim günlük hedefi** aşağıdakilerden biri olarak:

       - **Depolama**, burada olay günlüklerine kaydedilir ve olarak indirilebilir bir Azure depolama hesabına *.xel* dosyaları

          > [!TIP]
          > Rapor şablonları denetim gelen en iyi şekilde yararlanmak için denetlenen tüm veritabanları için aynı depolama hesabını kullanın.

       - **Log Analytics**, sorgu ya da daha fazla analiz için olayları otomatik olarak depolayan

           > [!NOTE]
           > A **Log Analytics çalışma alanı** analiz, özel uyarı kuralları ve Excel veya Power BI dışarı aktarma gibi gelişmiş özellikleri desteklemek için gereklidir. Sorgu Düzenleyicisi, bir çalışma alanı kullanılabilir.

       - **Olay hub'ı**, olaylar, diğer uygulamalarda kullanmak için yönlendirilmesini sağlar

   1. **Kaydet**’i seçin.

      ![Denetim ayarları](./media/sql-database-security-tutorial/audit-settings.png)

1. Seçebileceğiniz artık **denetim günlüklerini görüntüle** veritabanı olayları verilerini görüntülemek için.

    ![Denetim kayıtları](./media/sql-database-security-tutorial/audit-records.png)

> [!IMPORTANT]
> Bkz: [SQL veritabanı denetimi](sql-database-auditing.md) daha fazla denetim olaylarını PowerShell veya REST API'sini kullanarak özelleştirmek nasıl.

### <a name="dynamic-data-masking"></a>Dinamik veri maskeleme

Veri maskeleme özelliği, veritabanındaki hassas verileri otomatik olarak gizler.

Veri maskeleme etkinleştirmek için:

1. Azure portalında **SQL veritabanları** sol menüdeki ve veritabanınızı seçin **SQL veritabanları** sayfası.

1. İçinde **güvenlik** bölümünden **dinamik veri maskeleme**.

1. Altında **dinamik veri maskeleme** ayarları, select **Ekle maskesi** bir maskeleme kuralı eklemek için. Azure, kullanılabilir veritabanı şemaları, tabloları ve sütunları seçmek için otomatik olarak doldurulur.

    ![Maske ayarları](./media/sql-database-security-tutorial/mask-settings.png)

1. **Kaydet**’i seçin. Seçili bilgileri için gizlilik artık maskelenir.

    ![Örnek maskesi](./media/sql-database-security-tutorial/mask-query.png)

### <a name="transparent-data-encryption"></a>Saydam veri şifrelemesi

Şifreleme özelliği otomatik olarak bekleyen verilerinizi şifreler ve şifrelenmiş veritabanına erişen uygulamalar için herhangi bir değişiklik gerektirmez. Yeni veritabanları için şifreleme varsayılan olarak açıktır. Ayrıca verileri SSMS kullanarak şifreleme de yapabilirsiniz ve [her zaman şifreli](sql-database-always-encrypted.md) özelliği.

Etkinleştirmek veya şifrelemesini doğrulamak için:

1. Azure portalında **SQL veritabanları** sol menüdeki ve veritabanınızı seçin **SQL veritabanları** sayfası.

1. İçinde **güvenlik** bölümünden **saydam veri şifrelemesi**.

1. Gerekirse, ayarlayın **veri şifreleme** için **ON**. **Kaydet**’i seçin.

    ![Saydam Veri Şifrelemesi](./media/sql-database-security-tutorial/encryption-settings.png)

> [!NOTE]
> Şifreleme durumunu görüntülemek için veritabanı kullanarak bağlanmak [SSMS](./sql-database-connect-query-ssms.md) ve sorgu `encryption_state` sütununun [sys.dm_database_encryption_keys](/sql/relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql) görünümü. Durumunu `3` veritabanı şifrelenir gösterir.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, yalnızca birkaç basit adımla veritabanınızın güvenliğini artırmak öğrendiniz. Şunları öğrendiniz:

> [!div class="checklist"]
> - Sunucu düzeyinde ve veritabanı düzeyinde güvenlik duvarı kuralları oluşturma
> - Bir Azure Active Directory (AD) Yöneticisi'ni yapılandırma
> - SQL kimlik doğrulaması, Azure AD kimlik doğrulaması ve güvenli bağlantı dizeleri kullanıcı erişimini yönetme
> - Gelişmiş veri güvenliği, denetimi, veri maskeleme ve şifreleme gibi güvenlik özelliklerini etkinleştirme

Coğrafi dağıtım uygulama hakkında bilgi edinmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
>[Coğrafi olarak dağıtılmış bir veritabanı uygulama](sql-database-implement-geo-distributed-database.md)
