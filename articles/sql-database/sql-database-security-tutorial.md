---
title: Azure SQL veritabanınızın güvenliğini sağlama | Microsoft Docs
description: Azure SQL veritabanınızın güvenliğini sağlamaya yönelik teknik ve özellikler hakkında bilgi edinin.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: tutorial
author: DRediske
ms.author: daredis
ms.reviewer: vanto, carlrab
manager: craigg
ms.date: 09/07/2018
ms.openlocfilehash: b81e76201f7f751ee01e903d83f316811abaf483
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2018
ms.locfileid: "49955488"
---
# <a name="secure-your-azure-sql-database"></a>Azure SQL Veritabanınızın güvenliğini sağlama

SQL Veritabanı aşağıdakileri yaparak verilerinizin güvenliğini sağlar: 
- Güvenlik duvarı kurallarını kullanarak veritabanınıza erişimi sınırlandırma 
- Kimlik gerektiren kimlik doğrulama mekanizmaları kullanma
- Rol temelli üyelikler ve izinler aracılığıyla verilere yetki verme, 
- Satır düzeyi güvenlik
- Dinamik veri maskeleme

SQL Veritabanı ayrıca çok yönlü izleme, denetleme ve tehdit algılama özelliklerine sahiptir. 

Yalnızca birkaç basit adımda kötü amaçlı kullanıcılara ya da yetkisiz erişime karşı veritabanınızın korumasını artırabilirsiniz. Bu öğreticide şunları öğrenirsiniz: 

> [!div class="checklist"]
> * Azure portalında sunucunuza yönelik güvenlik duvarı kurallarını ayarlama
> * SSMS kullanarak veritabanınıza yönelik güvenlik duvarı kurallarını ayarlama
> * Güvenli bir bağlantı dizesi kullanarak veritabanınıza bağlanma
> * Kullanıcı erişimini yönetme
> * Şifreleme ile verilerinizi koruma
> * SQL Veritabanı denetimini etkinleştirme
> * SQL Veritabanı tehdit algılamayı etkinleştirme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakileri yaptığınızdan emin olun:

- [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)’nun (SSMS) en yeni sürümü yüklendi. 
- Microsoft Excel yükleme
- Bir Azure SQL sunucusu ve veritabanı oluşturma - Bkz. [Azure portalında Azure SQL veritabanı oluşturma](sql-database-get-started-portal.md), [Azure CLI kullanarak tek bir Azure SQL veritabanı oluşturma](sql-database-cli-samples.md) ve [PowerShell kullanarak tek bir Azure SQL veritabanı oluşturma](sql-database-powershell-samples.md). 

## <a name="log-in-to-the-azure-portal"></a>Azure portalında oturum açma

[Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="create-a-server-level-firewall-rule-in-the-azure-portal"></a>Azure portalında sunucu düzeyinde bir güvenlik duvarı kuralı oluşturma

SQL veritabanları Azure’daki bir güvenlik duvarı tarafından korunur. Varsayılan olarak, diğer Azure hizmetlerinden gelen bağlantılar dışında sunucuya ve sunucu içindeki veritabanlarına yönelik tüm bağlantılar reddedilir. Daha fazla bilgi için bkz. [Azure SQL Veritabanı'nda sunucu düzeyinde ve veritabanı düzeyinde güvenlik duvarı kuralları yapılandırma](sql-database-firewall-configure.md).

En güvenli yapılandırma, 'Azure hizmetlerine erişime izin ver' ayarının KAPALI olarak belirlenmesidir. Veritabanına bir Azure VM veya bulut hizmetinden bağlanmanız gerekirse bir [Ayrılmış IP (klasik dağıtım)](../virtual-network/virtual-networks-reserved-public-ip.md) oluşturmanız ve yalnızca ayrılmış IP adresinin güvenlik duvarı üzerinden erişmesine izin vermeniz gerekir. [Resource Manager](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm) dağıtım modelini kullanıyorsanız, kaynağa ayrılmış bir Genel IP adresi atanır ve güvenlik duvarı üzerinden bu IP adresinin erişmesine izin vermeniz gerekir.

Sunucunuzun belirli bir IP adresinden bağlantılara izin vermesi için bir [SQL Veritabanı sunucu düzeyinde güvenlik duvarı kuralı](sql-database-firewall-configure.md) oluşturmak üzere aşağıdaki adımları izleyin. 

> [!NOTE]
> Azure’da önceki öğretici ya da hızlı başlangıçları kullanarak örnek bir veritabanı oluşturduysanız ve bu öğreticiyi önceki öğreticilerle aynı IP adresine sahip bir bilgisayarda uyguluyorsanız, sunucu düzeyinde güvenlik duvarı kuralını zaten oluşturmuş olacağınız için bu adımı atlayabilirsiniz.
>

1. Soldaki menüden **SQL veritabanları**’na ve **SQL veritabanları** sayfasında güvenlik duvarı kuralını yapılandırmak istediğiniz veritabanına tıklayın. Veritabanınıza ilişkin genel bakış sayfası açılır ve tam sunucu adı (örneğin, **mynewserver-20170313.database.windows.net**) görüntülenerek daha fazla yapılandırma seçeneği sunulur.

      ![sunucu güvenlik duvarı kuralı](./media/sql-database-security-tutorial/server-firewall-rule.png) 

2. Önceki görüntüde gösterildiği gibi araç çubuğundaki **sunucu güvenlik duvarı ayarla** öğesine tıklayın. SQL Veritabanı sunucusu için **Güvenlik duvarı ayarları** sayfası açılır. 

3. Araç çubuğundaki **İstemci IP'si ekle**’ye tıklayarak portala bağlı bilgisayarın genel IP adresini ekleyin ya da güvenlik duvarı kuralını el ile girerek **Kaydet**’e tıklayın.

      ![sunucu güvenlik duvarı kuralı ayarla](./media/sql-database-security-tutorial/server-firewall-rule-set.png) 

4. **Tamam**’a tıklayın ve ardından **Güvenlik duvarı ayarları** sayfasını kapatmak için **X** öğesine tıklayın.

Artık sunucuda belirtilen IP adresine veya IP adresi aralığına sahip herhangi bir veritabanına bağlanabilirsiniz.

> [!NOTE]
> SQL Veritabanı 1433 numaralı bağlantı noktası üzerinden iletişim kurar. Bir kurumsal ağ içerisinden bağlanmaya çalışıyorsanız, ağınızın güvenlik duvarı tarafından 1433 numaralı bağlantı noktası üzerinden giden trafiğe izin verilmiyor olabilir. Bu durumda, BT departmanınız 1433 numaralı bağlantı noktasını açmadığı sürece Azure SQL veritabanı sunucusuna bağlanamazsınız.
>

## <a name="create-a-database-level-firewall-rule-using-ssms"></a>SSMS kullanarak veritabanı düzeyinde güvenlik duvarı kuralı oluşturma

Veritabanı düzeyinde güvenlik duvarı kurallarını kullanarak aynı mantıksal sunucu içinde farklı veritabanları için farklı güvenlik duvarı kuralları oluşturabilir ve taşınabilir özellikte olan (yani SQL sunucusunda depolanmak yerine [yük devretme](sql-database-geo-replication-overview.md) sırasında veritabanını izleyen) güvenlik duvarı kuralları oluşturabilirsiniz. Veritabanı düzeyinde güvenlik duvarı kuralları yalnızca Transact-SQL deyimleri kullanılarak ve ancak ilk sunucu düzeyinde güvenlik duvarı kuralınızı yapılandırmanızdan sonra yapılandırılabilir. Daha fazla bilgi için bkz. [Azure SQL Veritabanı'nda sunucu düzeyinde ve veritabanı düzeyinde güvenlik duvarı kuralları yapılandırma](sql-database-firewall-configure.md).

Veritabanına özel bir güvenlik duvarı kuralı oluşturmak için aşağıdaki adımları izleyin.

1. Örneğin [SQL Server Management Studio](./sql-database-connect-query-ssms.md) kullanarak veritabanınıza bağlanın.

2. Nesne Gezgini'nde, güvenlik duvarı kuralı eklemek istediğiniz veritabanına sağ tıklayın ve **Yeni Sorgu**’ya tıklayın. Veritabanınıza bağlı boş bir sorgu penceresi açılır.

3. Sorgu penceresinde, genel IP adresinizin IP adresini değiştirin ve sonra aşağıdaki sorguyu yürütün:

    ```sql
    EXECUTE sp_set_database_firewall_rule N'Example DB Rule','0.0.0.4','0.0.0.4';
    ```

4. Araç çubuğunda **Yürüt**’e tıklayarak güvenlik duvarı kuralını oluşturun.

## <a name="view-how-to-connect-an-application-to-your-database-using-a-secure-connection-string"></a>Güvenli bir bağlantı dizesi kullanarak bir uygulamayı veritabanınıza bağlama işlemini görüntüleyin

Bir istemci uygulama ile SQL Veritabanı arasında güvenli, şifreli bir bağlantı sağlamak için bağlantı dizesinin için şu şekilde yapılandırılması gerekir:

- Şifreli bir bağlantı isteyecek ve
- Sunucu sertifikasına güvenmeyecek şekilde. 

Bu yapılandırma, Aktarım Katmanı Güvenliği (TLS) kullanarak bağlantı kurar ve ortadaki adam saldırılarının riskini azaltır. SQL Veritabanınızın desteklenen istemci sürücüleri için doğru şekilde yapılandırılmış bağlantı dizelerini, bu ekran görüntüsünde ADO.net için gösterildiği gibi Azure portalından alabilirsiniz. TLS ve bağlantı hakkında daha fazla bilgi için bkz. [TLS konuları](sql-database-connect-query.md#tls-considerations-for-sql-database-connectivity).

1. Soldaki menüden **SQL veritabanları**’nı seçin ve **SQL veritabanları** sayfasında veritabanınıza tıklayın.

2. Veritabanınızın **Genel bakış** sayfasında **Veritabanı bağlantı dizelerini göster**’e tıklayın.

3. Tam **ADO.NET** bağlantı dizesini gözden geçirin.

    ![ADO.NET bağlantı dizesi](./media/sql-database-security-tutorial/adonet-connection-string.png)

## <a name="creating-database-users"></a>Veritabanı kullanıcıları oluşturma

Herhangi bir kullanıcı oluşturmadan önce ilk olarak Azure SQL Veritabanı tarafından desteklenen iki kimlik doğrulama türünden birini seçmeniz gerekir: 

Yalnızca bir mantıksal sunucu içindeki belirli bir veritabanı bağlamında geçerli olan oturumlar ve kullanıcılar için kullanıcı adı ve parola kullanan **SQL Kimlik Doğrulaması**. 

Azure Active Directory tarafından yönetilen kimlikleri kullanan **Azure Active Directory Kimlik Doğrulaması**. 

SQL Veritabanında kimlik doğrulaması yapmak için [Azure Active Directory](./sql-database-aad-authentication.md)’yi kullanmak isterseniz, devam edebilmeniz için doldurulmuş bir Azure Active Directory mevcut olmalıdır.

SQL Kimlik Doğrulaması kullanarak kullanıcı oluşturmak için aşağıdaki adımları izleyin:

1. Örneğin [SQL Server Management Studio](./sql-database-connect-query-ssms.md) kullanarak sunucu yöneticisi kimlik bilgilerinizle veritabanınıza bağlanın.

2. Nesne Gezgini'nde, yeni kullanıcı eklemek istediğiniz veritabanına sağ tıklayın ve **Yeni Sorgu**’ya tıklayın. Seçili veritabanına bağlı boş bir sorgu penceresi açılır.

3. Sorgu penceresine aşağıdaki sorguyu girin:

    ```sql
    CREATE USER ApplicationUser WITH PASSWORD = 'YourStrongPassword1';
    ```

4. Araç çubuğunda **Yürüt**’e tıklayarak kullanıcıyı oluşturun.

5. Varsayılan olarak, kullanıcı veritabanına bağlanabilir ancak verileri okuma veya yazma izni yoktur. Yeni oluşturulan kullanıcıya bu izinleri vermek için yeni bir sorgu penceresinde aşağıdaki iki komutu yürütün

    ```sql
    ALTER ROLE db_datareader ADD MEMBER ApplicationUser;
    ALTER ROLE db_datawriter ADD MEMBER ApplicationUser;
    ```

Yeni kullanıcı oluşturmak gibi yönetici görevleri yürütmeniz gerekmiyorsa, veritabanınıza bağlanmak için yönetici olmayan bu hesapların veritabanı düzeyinde oluşturulması en iyi uygulamadır. Azure Active Directory kullanarak kimlik doğrulama işlemi için lütfen [Azure Active Directory öğreticisini](./sql-database-aad-authentication-configure.md) inceleyin.


## <a name="protect-your-data-with-encryption"></a>Şifreleme ile verilerinizi koruma

Azure SQL Veritabanı saydam veri şifrelemesi (TDE), şifrelenmiş veritabanına erişen uygulamada herhangi bir değişiklik yapılmasını gerektirmeden bekleyen verilerinizi otomatik olarak şifreler. Yeni oluşturulan veritabanları için TDE varsayılan olarak açıktır. Veritabanınız için TDE’yi etkinleştirmek ya da TDE’nin açık olduğunu doğrulamak için şu adımları izleyin:

1. Soldaki menüden **SQL veritabanları**’nı seçin ve **SQL veritabanları** sayfasında veritabanınıza tıklayın. 

2. **Saydam veri şifrelemesi**’ne tıklayarak TDE yapılandırma sayfasını açın.

    ![Saydam Veri Şifrelemesi](./media/sql-database-security-tutorial/transparent-data-encryption-enabled.png)

3. Gerekirse, **Veri şifreleme** ayarını AÇIK olarak belirleyip **Kaydet**’e tıklayın.

Şifreleme işlemi arka planda başlatılır. [SQL Server Management Studio](./sql-database-connect-query-ssms.md) ile SQL Veritabanına bağlanıp [sys.dm_database_encryption_keys](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql?view=sql-server-2017) görünümünün encryption_state sütununu sorgulayarak ilerleme durumunu izleyebilirsiniz. 3 durumu veritabanının şifrelendiğini belirtir. 

## <a name="enable-sql-database-auditing-if-necessary"></a>Gerekirse SQL Veritabanı denetimini etkinleştirin

Azure SQL Veritabanı Denetimi, veritabanı olaylarını izler ve olayları Azure Depolama hesabınızdaki bir denetim günlüğüne yazar. Denetim mevzuatla uyumluluk, veritabanı etkinliğini anlama ve olası güvenlik ihlallerini işaret edebilecek farklılıklar ve anormal durumlar hakkında öngörü sahip olmanıza yardımcı olabilir. SQL veritabanınız için bir varsayılan bir denetim ilkesi oluşturmak üzere aşağıdaki adımları izleyin:

1. Soldaki menüden **SQL veritabanları**’nı seçin ve **SQL veritabanları** sayfasında veritabanınıza tıklayın. 

2. Ayarlar dikey penceresinde **Denetim ve Tehdit Algılama**’yı seçin. Sunucu düzeyinde denetimin devre dışı olduğuna ve bu bağlamdan sunucu denetim ayarlarını görüntülemenize ya da değiştirmenize olanak tanıyan yeni bir **Sunucu ayarlarını görüntüleme** bağlantısı olduğuna dikkat edin.

    ![Denetim Dikey Penceresi](./media/sql-database-security-tutorial/auditing-get-started-settings.png)

3. Sunucu düzeyinde belirtilenden farklı bir Denetim türü (veya konumu?) etkinleştirmeyi tercih ederseniz, Denetimi **AÇIK** duruma getirin ve **Blob** Denetim Türünü seçin. Sunucu Blob denetimi etkinse, veritabanı ile yapılandırılmış denetim, sunucu Blob denetimi ile yan yana bulunur.

    ![Denetimi açma](./media/sql-database-security-tutorial/auditing-get-started-turn-on.png)

4. **Depolama Ayrıntıları**’nı seçerek Denetim Günlükleri Depolama Dikey Penceresini açın. Günlüklerin kaydedileceği Azure depolama hesabını ve sonrasında eski günlüklerin silineceği elde tutma süresini seçip alt kısımdaki **Tamam** düğmesine tıklayın. 

   > [!TIP]
   > Denetim raporları şablonlarından en iyi şekilde yararlanmak üzere, denetlenen tüm veritabanları için aynı depolama hesabını kullanın.
   > 

5. **Kaydet**’e tıklayın.

> [!IMPORTANT]
> Denetlenen olayları özelleştirmek isterseniz, PowerShell veya REST API kullanarak bunu yapabilirsiniz; daha fazla bilgi için bkz. [SQL veritabanı denetimi](sql-database-auditing.md).
>

## <a name="enable-sql-database-threat-detection"></a>SQL Veritabanı tehdit algılamayı etkinleştirme

Tehdit Algılama ile sunulan yeni güvenlik katmanı müşterilerin anormal etkinliklerde güvenlik uyarısı oluşturarak potansiyel tehditleri tespit etmesini ve onlara karşı harekete geçmesini sağlar. Kullanıcılar SQL Veritabanı Denetimini kullanarak şüpheli etkinlikleri araştırıp olayın veritabanındaki verilerle ilgili erişim, güvenlik ihlali veya yetkisiz erişim kaynaklı olup olmadığını belirleyebilir. Tehdit Algılama sayesinde güvenlik uzmanı olmadan veya gelişmiş güvenlik izleme sistemlerine ihtiyaç duymadan potansiyel tehditlerle baş edebilirsiniz.
Örneğin, Tehdit Algılama olası SQL ekleme girişimlerini gösteren belirli anormal veritabanı etkinliklerini algılar. SQL ekleme, İnternet üzerindeki en yaygın Web uygulaması güvenlik sorunlarından biridir ve veri temelli uygulamalara saldırmak için kullanılır. Saldırganlar uygulama giriş alanlarına kötü amaçlı SQL deyimleri eklemek ya da veritabanındaki verileri ihlal etmek veya değiştirmek amacıyla uygulamanın güvenlik açıklarından yararlanır.

1. İzlemek istediğiniz SQL veritabanının yapılandırma dikey penceresine gidin. Ayarlar dikey penceresinde **Denetim ve Tehdit Algılama**’yı seçin.

    ![Gezinti bölmesi](./media/sql-database-security-tutorial/auditing-get-started-settings.png)
2. **Denetim ve Tehdit Algılama** yapılandırma dikey penceresinde denetimi **AÇIK** duruma getirerek tehdit algılama ayarlarını görüntüleyin.

3. Tehdit algılamayı **AÇIK** duruma getirin.

4. Anormal veritabanı etkinliklerinin algılanması üzerine güvenlik uyarıları alacak e-postaların listesini yapılandırın.

5. Yeni veya güncelleştirilmiş denetim ve tehdit algılama ilkesini kaydetmek için **Denetim ve Tehdit algılama** dikey penceresinde **Kaydet**’e tıklayın.

    ![Gezinti bölmesi](./media/sql-database-security-tutorial/td-turn-on-threat-detection.png)

    Anormal veritabanı etkinlikleri algılanırsa, anormal veritabanı etkinliklerinin algılanması üzerine bir e-posta bildirimi alırsınız. E-postada şüpheli güvenlik olayına ilişkin anormal etkinliklerin niteliği, veritabanı adı, sunucu adı ve olay zamanı gibi bilgiler verilir. Bunun yanında, veritabanı için geçerli olabilecek tehditleri araştırmak ve azaltmak üzere olası nedenler ve önerilen eylemler hakkında bilgi verilir. Aşağıdaki adımlarda böyle bir e-posta almanız durumunda yapmanız gerekenler gösterilmektedir:

    ![Tehdit algılama e-postası](./media/sql-database-threat-detection-get-started/4_td_email.png)

6. E-postada **Azure SQL Denetim Günlüğü** bağlantısına tıklayarak Azure portalını başlatın ve şüpheli olayın gerçekleştiği zamana yakın, ilgili denetim kayıtlarını görüntüleyin.

    ![Denetim kayıtları](./media/sql-database-threat-detection-get-started/5_td_audit_records.png)

7. Şüpheli veritabanı etkinlikleriyle ilgili SQL deyimi, hata nedeni ve istemci IP’si gibi daha fazla bilgi görüntülemek için denetim kayıtlarına tıklayın.

    ![Kayıt ayrıntıları](./media/sql-database-security-tutorial/6_td_audit_record_details.png)

8. Denetim Kayıtları dikey penceresinde **Excel’de Aç**’a tıklayarak, içeri aktarılmak üzere önceden yapılandırılmış bir Excel dosyası açın ve şüpheli olayın zamanına yakın denetim günlüğü üzerinde daha derin bir analiz gerçekleştirin.

    > [!NOTE]
    > Excel 2010 veya üzeri, Power Query ve **Hızlı Birleştir** ayarı gereklidir.

    ![Kayıtları Excel'de açma](./media/sql-database-threat-detection-get-started/7_td_audit_records_open_excel.png)

9. **Hızlı Birleştir** ayarını yapılandırmak için: **POWER QUERY** şerit sekmesinde **Seçenekler** öğesini seçerek Seçenekler iletişim kutusunu görüntüleyin. Gizlilik bölümünü seçin ve ikinci seçeneği belirleyin: 'Gizlilik Düzeylerini yoksayın ve potansiyel performansı geliştirin':

    ![Excel hızlı birleştir](./media/sql-database-threat-detection-get-started/8_td_excel_fast_combine.png)

10. SQL denetim günlüklerini yüklemek için ayarlar sekmesindeki parametrelerin doğru ayarlandığından emin olun ve sonra 'Veri' şeridini seçip 'Tümünü Yenile' düğmesine tıklayın.

    ![Excel parametreleri](./media/sql-database-threat-detection-get-started/9_td_excel_parameters.png)

11. Sonuçlar, algılanan anormal etkinliklerin daha derin bir analizini gerçekleştirmenize ve güvenlik olayının uygulamanızdaki etkisini azaltmanıza olanak tanıyan **SQL Denetim Günlüklerini** sayfasında görünür.


## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, yalnızca birkaç basit adımda kötü amaçlı kullanıcılara ya da yetkisiz erişime karşı veritabanınızın korumasını artırmayı öğrendiniz.  Şunları öğrendiniz: 

> [!div class="checklist"]
> * Sunucunuz ve/veya veritabanınız için güvenlik duvarı kurallarını ayarlama
> * Güvenli bir bağlantı dizesi kullanarak veritabanınıza bağlanma
> * Kullanıcı erişimini yönetme
> * Şifreleme ile verilerinizi koruma
> * SQL Veritabanı denetimini etkinleştirme
> * SQL Veritabanı tehdit algılamayı etkinleştirme

Coğrafi olarak dağıtılmış bir veritabanı uygulama hakkında bilgi edinmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
>[Coğrafi olarak dağıtılmış bir veritabanı uygulama](sql-database-implement-geo-distributed-database.md)

