---
title: "Azure SQL veritabanınızı güvenli | Microsoft Docs"
description: "Teknikleri ve Azure SQL veritabanınızın güvenliğini sağlamak için özellikleri hakkında bilgi edinin."
services: sql-database
documentationcenter: 
author: DRediske
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,security
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: On Demand
ms.date: 06/28/2017
ms.author: daredis
ms.openlocfilehash: d25a0461bf194808f9bd66ddbd120448620eeba0
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2017
---
# <a name="secure-your-azure-sql-database"></a>Azure SQL veritabanınızı güvenli

SQL Veritabanı veritabanınıza erişimi sınırlamak için güvenlik duvarı kurallarını, kullanıcıların kimliğini doğrulamak için kimlik doğrulama sistemlerini, rol tabanlı üyelikler ve izinler ile veri yetkilendirmeyi, satır düzeyi güvenliği ve dinamik veri maskelemeyi kullanarak verilerinizin güvenliğini sağlar.

Veritabanınızı kötü amaçlı kullanıcılar ya da yalnızca birkaç basit adımla yetkisiz erişime karşı koruma artırabilir. Bu öğreticide, öğrenin: 

> [!div class="checklist"]
> * Azure portalında sunucunuz için sunucu düzeyinde güvenlik duvarı kuralları ayarlayın
> * SSMS kullanarak, veritabanı için veritabanı düzeyinde güvenlik duvarı kuralları ayarlayın
> * Güvenli bağlantı dizesi kullanarak veritabanınıza bağlanmak
> * Kullanıcı erişimini yönetme
> * Şifreleme ile verilerinizi koruma
> * SQL veritabanı denetimi etkinleştir
> * SQL veritabanı tehdit algılama etkinleştir

Bir Azure aboneliğiniz yoksa [ücretsiz bir hesap oluşturma](https://azure.microsoft.com/free/) başlamadan önce.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakilere sahip olduğunuzdan emin olun:

- En son sürümü yüklü [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS). 
- Yüklü Microsoft Excel
- Bir Azure SQL server ve veritabanı - oluşturulan bkz [Azure portalında bir Azure SQL veritabanı oluşturma](sql-database-get-started-portal.md), [Azure CLI kullanarak tek bir Azure SQL veritabanı oluşturma](sql-database-get-started-cli.md), ve [PowerShell kullanarak tek bir Azure SQL veritabanı oluşturma](sql-database-get-started-powershell.md). 

## <a name="log-in-to-the-azure-portal"></a>Azure portalında oturum açma

[Azure Portal](https://portal.azure.com/)’da oturum açın.

## <a name="create-a-server-level-firewall-rule-in-the-azure-portal"></a>Azure portalında sunucu düzeyinde bir güvenlik duvarı kuralı oluşturma

SQL veritabanları Azure güvenlik duvarı tarafından korunur. Varsayılan olarak, diğer Azure hizmetleriyle bağlantılarından dışında sunucusunu ve veritabanlarını Sunucusu'ndaki tüm bağlantıları reddedilir. Daha fazla bilgi için bkz. [Azure SQL Veritabanı'nda sunucu düzeyinde ve veritabanı düzeyinde güvenlik duvarı kuralları yapılandırma](sql-database-firewall-configure.md).

En güvenli yapılandırma, 'OFF olarak Azure hizmetlerine erişime izin ver' ayarlamaktır. Bir Azure VM veya Bulut hizmetinden veritabanına bağlanmak gerekiyorsa, oluşturmalısınız bir [ayrılmış IP](../virtual-network/virtual-networks-reserved-public-ip.md) ve yalnızca ayrılmış IP adresi erişim güvenlik duvarı aracılığıyla izin verin. 

Oluşturmak için bu adımları bir [SQL veritabanı sunucu düzeyinde güvenlik duvarı kuralı](sql-database-firewall-configure.md) sunucunuz belirli bir IP adresinden gelen bağlantılara izin vermek için. 

> [!NOTE]
> Önceki öğreticileri veya quickstarts birini kullanarak Azure'da bir örnek veritabanı oluşturdunuz ve bu öğreticileri gitti sırasındaki aynı IP adresine sahip bir bilgisayarda bu öğreticiyi gerçekleştirmeden, sunucu düzeyinde güvenlik duvarı kuralı oluşturmuş şekilde, bu adımı atlayabilirsiniz.
>

1. Tıklatın **SQL veritabanları** üzerinde Güvenlik Duvarı'nı yapılandırmak istediğiniz veritabanı kural için tıklatın ve sol taraftaki menüden **SQL veritabanları** sayfası. Veritabanınız için genel bakış sayfası açılır ve tam sunucu adını gösteren (gibi **mynewserver 20170313.database.windows.net**) ve diğer yapılandırmalar için seçenekler sağlar.

      ![sunucu güvenlik duvarı kuralı](./media/sql-database-security-tutorial/server-firewall-rule.png) 

2. Önceki görüntüde gösterildiği gibi araç çubuğundaki **sunucu güvenlik duvarı ayarla** öğesine tıklayın. SQL Veritabanı sunucusu için **Güvenlik duvarı ayarları** sayfası açılır. 

3. Tıklatın **istemci IP'si Ekle** genel portal ile bağlı bilgisayarın IP adresini ekleyin veya güvenlik duvarı kuralı el ile girin ve ardından araç çubuğundaki **kaydetmek**.

      ![sunucu güvenlik duvarı kuralı ayarla](./media/sql-database-security-tutorial/server-firewall-rule-set.png) 

4. **Tamam**’a tıklayın ve ardından **Güvenlik duvarı ayarları** sayfasını kapatmak için **X** öğesine tıklayın.

Şimdi belirtilen IP adresi veya IP adresi aralığı sunucusuyla herhangi bir veritabanına bağlanabilir.

> [!NOTE]
> SQL Veritabanı 1433 numaralı bağlantı noktası üzerinden iletişim kurar. Bir kurumsal ağ içerisinden bağlanmaya çalışıyorsanız, ağınızın güvenlik duvarı tarafından 1433 numaralı bağlantı noktası üzerinden giden trafiğe izin verilmiyor olabilir. Bu durumda, BT departmanınız 1433 numaralı bağlantı noktasını açmadığı sürece Azure SQL veritabanı sunucusuna bağlanamazsınız.
>

## <a name="create-a-database-level-firewall-rule-using-ssms"></a>SSMS kullanarak bir veritabanı düzeyinde güvenlik duvarı kuralı oluşturma

Veritabanı düzeyinde güvenlik duvarı kurallarını etkinleştirmek, aynı mantıksal sunucu içindeki farklı veritabanları için farklı bir güvenlik duvarı ayarları oluşturma ve taşınabilir - takip etmeleri sırasında veritabanı anlamına gelen güvenlik duvarı kuralları oluşturmak için bir [yük devretme](sql-database-geo-replication-overview.md) yerine SQL server üzerinde depolanıyor. İlk sunucu düzeyinde güvenlik duvarı kuralını yapılandırdıktan sonra veritabanı düzeyinde güvenlik duvarı kuralları yalnızca Transact-SQL deyimi kullanarak yapılandırılmış ve yalnızca olabilir. Daha fazla bilgi için bkz. [Azure SQL Veritabanı'nda sunucu düzeyinde ve veritabanı düzeyinde güvenlik duvarı kuralları yapılandırma](sql-database-firewall-configure.md).

Bir veritabanı özel güvenlik duvarı kuralı oluşturmak için şu adımları izler.

1. Örneğin kullanarak, bir veritabanına bağlanmak [SQL Server Management Studio](./sql-database-connect-query-ssms.md).

2. Nesne Gezgini'nde, önce için bir güvenlik duvarı kuralı eklemek istediğiniz veritabanını sağ tıklatın **yeni sorgu**. Veritabanınıza bağlı boş bir sorgu penceresi açılır.

3. Sorgu penceresinde, genel IP adresi IP adresini değiştirin ve aşağıdaki sorguyu çalıştırın:

    ```sql
    EXECUTE sp_set_database_firewall_rule N'Example DB Rule','0.0.0.4','0.0.0.4';
    ```

4. Araç çubuğunda tıklatın **yürütme** güvenlik duvarı kuralı oluşturun.

## <a name="view-how-to-connect-an-application-to-your-database-using-a-secure-connection-string"></a>Güvenli bağlantı dizesi kullanarak veritabanını bir uygulamaya bağlanmak nasıl görüntüleyin

Bir istemci uygulaması ve SQL veritabanı arasında güvenli, şifreli bir bağlantı sağlamak için bağlantı dizesi için yapılandırılması gerekir:

- Şifreli bir bağlantı isteği ve
- Sunucu sertifikası güvenmediğiniz için. 

Bu Aktarım Katmanı Güvenliği (TLS) kullanarak bağlantı kurar ve man-in--middle saldırıları riski azaltılır. Doğru yapılandırılmış bağlantı dizeleri desteklenen istemci için SQL veritabanınızın sürücüleri Azure portalından ADO.net için bu ekran görüntüsünde gösterildiği gibi alabilirsiniz.

1. Seçin **SQL veritabanları** sol taraftaki menüden ve veritabanınızı tıklayın **SQL veritabanları** sayfası.

2. Üzerinde **genel bakış** sayfasında veritabanınız için **veritabanı bağlantı dizelerini Göster**.

3. Tam **ADO.NET** bağlantı dizesini gözden geçirin.

    ![ADO.NET bağlantı dizesi](./media/sql-database-security-tutorial/adonet-connection-string.png)

## <a name="creating-database-users"></a>Veritabanı kullanıcısı oluşturma

Herhangi bir kullanıcı oluşturmadan önce ilk Azure SQL veritabanı tarafından desteklenen iki kimlik doğrulama türleri birini seçmeniz gerekir: 

**SQL kimlik doğrulaması**, kullanan kullanıcı adı ve parola oturum açmalar ve yalnızca bir mantıksal sunucu içinde belirli bir veritabanı bağlamında geçerli olan kullanıcılar için. 

**Azure Active Directory kimlik doğrulaması**, Azure Active Directory tarafından yönetilen kimlikleri kullanır. 

Kullanmak istiyorsanız, [Azure Active Directory](./sql-database-aad-authentication.md) devam etmeden önce SQL veritabanında kimlik doğrulaması için doldurulan bir Azure Active Directory bulunmalıdır.

SQL kimlik doğrulaması kullanarak bir kullanıcı oluşturmak için aşağıdaki adımları izleyin:

1. Örneğin kullanarak, bir veritabanına bağlanmak [SQL Server Management Studio](./sql-database-connect-query-ssms.md) server yönetici kimlik bilgilerinizi kullanarak.

2. Nesne Gezgini'nde, önce yeni bir kullanıcı eklemek istediğiniz veritabanını sağ tıklatın **yeni sorgu**. Seçilen veritabanına bağlı bir boş sorgu penceresi açar.

3. Sorgu penceresine aşağıdaki sorguyu girin:

    ```sql
    CREATE USER ApplicationUser WITH PASSWORD = 'YourStrongPassword1';
    ```

4. Araç çubuğunda tıklatın **yürütme** kullanıcı oluşturmak için.

5. Varsayılan olarak, kullanıcının veritabanına bağlanabilirsiniz, ancak okuma veya veri yazma izni olduğundan. Yeni oluşturulan kullanıcı bu izinleri vermek için yeni bir sorgu penceresinde aşağıdaki iki komutu yürütme

    ```sql
    ALTER ROLE db_datareader ADD MEMBER ApplicationUser;
    ALTER ROLE db_datawriter ADD MEMBER ApplicationUser;
    ```

Yeni kullanıcılar oluşturma gibi yönetici görevleri çalıştırmak gerekli olmadıkça, veritabanına bağlanmak için veritabanı düzeyinde bu yönetici olmayan bir hesap oluşturmak için en iyi bir uygulamadır. Lütfen gözden [Azure Active Directory öğretici](./sql-database-aad-authentication-configure.md) Azure Active Directory'yi kullanarak kimlik doğrulaması yapmayı üzerinde.


## <a name="protect-your-data-with-encryption"></a>Şifreleme ile verilerinizi koruma

Azure SQL veritabanında saydam veri şifreleme (TDE) şifrelenmiş veritabanına erişen uygulama herhangi bir değişiklik gerektirmeden verilerinizi REST, otomatik olarak şifreler. Yeni oluşturulan veritabanları için TDE varsayılan olarak açıktır. Veritabanınız için TDE etkinleştirmek ya da TDE açık olduğunu doğrulamak için şu adımları izleyin:

1. Seçin **SQL veritabanları** sol taraftaki menüden ve veritabanınızı tıklayın **SQL veritabanları** sayfası. 

2. Tıklayın **saydam veri şifreleme** TDE için yapılandırma sayfasını açın.

    ![Saydam Veri Şifrelemesi](./media/sql-database-security-tutorial/transparent-data-encryption-enabled.png)

3. Gerekirse, ayarlamak **veri şifreleme** on tıklatıp **kaydetmek**.

Şifreleme işlemi arka planda başlatılır. SQL veritabanı için kullanılacak bağlanarak ilerleme durumunu izleyebilirsiniz [SQL Server Management Studio](./sql-database-connect-query-ssms.md) encryption_state sütunu sorgulamak `sys.dm_database_encryption_keys` görünümü.

## <a name="enable-sql-database-auditing-if-necessary"></a>SQL veritabanı denetimi, gerekirse etkinleştirme

Azure SQL veritabanı denetimi veritabanı olaylarını ve Azure depolama hesabınızdaki bunları Denetim günlüğüne yazar izler. Denetim, yönetmeliklere uygunluğu korumanıza, veritabanı etkinliklerini anlamanıza ve ifade eden tutarsızlıklar ve olası güvenlik ihlallerini gösterebilir anormallikleri kavramanıza yardımcı olabilir. Denetim İlkesi SQL veritabanınız için bir varsayılan oluşturmak için aşağıdaki adımları izleyin:

1. Seçin **SQL veritabanları** sol taraftaki menüden ve veritabanınızı tıklayın **SQL veritabanları** sayfası. 

2. Ayarlar dikey penceresinde seçin **denetim ve tehdit algılama**. Bildirim olduğunu ve sunucu düzeyi denetimi devre: bir **sunucu ayarlarını görüntüleyin** bağlantı görüntülemek veya sunucunun denetim ayarları'nı bu bağlamından değiştirmenize olanak sağlar.

    ![Denetim dikey penceresi](./media/sql-database-security-tutorial/auditing-get-started-settings.png)

3. Bir denetim türü (veya konum?) etkinleştirmeyi tercih ediyorsanız olandan farklı sunucu düzeyinde belirtilen, kapatma **ON** denetleme ve **Blob** denetim türü. Sunucu Blob denetimi etkinse, yapılandırılmış veritabanı denetimi yan yana sunucu Blob denetim yer alır.

    ![Denetim Aç](./media/sql-database-security-tutorial/auditing-get-started-turn-on.png)

4. Seçin **depolama ayrıntıları** denetim günlüklerini depolama dikey penceresini açın. Burada günlüklerine kaydedilir ve daha sonra eski günlükleri silinecek, saklama dönemi, ardından Azure depolama hesabı seçin **Tamam** altındaki. 

   > [!TIP]
   > En iyi denetim raporları şablonları almak için tüm denetlenen veritabanları için aynı depolama hesabı kullanın.
   > 

5. **Kaydet** düğmesine tıklayın.

> [!IMPORTANT]
> Denetlenen olayları özelleştirmek istiyorsanız, PowerShell veya REST API - bunu yapabilirsiniz bkz [SQL veritabanı denetimi](sql-database-auditing.md) daha fazla ayrıntı için.
>

## <a name="enable-sql-database-threat-detection"></a>SQL veritabanı tehdit algılama etkinleştir

Tehdit algılama, müşterilerin algılamak ve anormal etkinlikler güvenlik uyarıları sağlayarak göründüklerinde olası risklere yanıt sağlayan bir güvenlik yeni bir katman sağlar. Kullanıcılar, bunlar erişim, ihlal ya da veritabanındaki verileri yararlanma girişimi sonucu olmadığını belirlemek için SQL veritabanı denetimi kullanarak şüpheli olayları gözden geçirebilirsiniz. Tehdit algılama, veritabanına Uzman güvenlik olması veya sistemleri izleme Gelişmiş Güvenlik yönetmek zorunda kalmadan adresi olası tehditlere kolaylaştırır.
Örneğin, tehdit algılama olası SQL ekleme girişimlerini gösteren belirli anormal veritabanı etkinliklerini algılar. SQL ekleme veri güdümlü uygulamaları saldırmak için kullanılıyorsa Internet üzerinde ortak Web uygulaması güvenlik sorunlarını biridir. Saldırganlar ihlal veya veritabanındaki verileri değiştirmek için uygulama giriş alanları, kötü amaçlı SQL deyimleri eklemesine uygulama güvenlik açıkları yararlanın.

1. İzlemek istediğiniz SQL veritabanı yapılandırma dikey penceresine gidin. Ayarlar dikey penceresinde seçin **denetim ve tehdit algılama**.

    ![Gezinti Bölmesi](./media/sql-database-security-tutorial/auditing-get-started-settings.png)
2. İçinde **denetim ve tehdit algılama** yapılandırma dikey Aç **ON** denetleme, hangi görüntüler tehdit algılama ayarlar.

3. Kapatma **ON** tehdit algılama.

4. Güvenlik Uyarıları anormal veritabanı etkinliklerini algılandığında alacak e-postaları listesini yapılandırın.

5. Tıklatın **kaydetmek** içinde **denetim ve tehdit algılama** yeni veya güncelleştirilmiş denetim kaydedin ve tehdit algılama İlkesi dikey penceresi.

    ![Gezinti Bölmesi](./media/sql-database-security-tutorial/td-turn-on-threat-detection.png)

    Anormal veritabanı etkinliklerini algılanmazsa, anormal veritabanı etkinliklerini algılandığında bir e-posta bildirimi alırsınız. E-posta anormal etkinlikler, veritabanı adı, sunucu adını ve olay süresi yapısını dahil olmak üzere şüpheli güvenlik olayı üzerinde bilgi sağlar. Ayrıca, olası nedenler bilgileri sağlarız ve önerilen eylemleri araştırmak ve veritabanına olası tehdidi azaltmak için. Sonraki adımlarda size yol yapmanız gerekenler aracılığıyla gibi e-posta almanız gerekir:

    ![Tehdit algılama e-posta](./media/sql-database-threat-detection-get-started/4_td_email.png)

6. E-posta ile tıklayın **Azure SQL denetim günlüğü** bağlantı, hangi Azure Portalı'nı başlatın ve ilgili denetim kayıtları şüpheli olay sırada geçici gösterir.

    ![Denetim kaydı](./media/sql-database-threat-detection-get-started/5_td_audit_records.png)

7. Denetim kayıtlarının SQL deyimini gibi şüpheli veritabanı etkinlikleri hakkında daha fazla ayrıntı görüntülemek için tıklatın başarısızlık nedeni ve istemci IP.

    ![Kayıt ayrıntıları](./media/sql-database-security-tutorial/6_td_audit_record_details.png)

8. Denetim kayıtlarının dikey penceresinde tıklayın **Excel'de açın** açmak için önceden yapılandırılmış excel almak ve daha derin denetim günlüğü şüpheli olay sırada geçici analizini çalıştırmak için şablon.

    > [!NOTE]
    > Excel 2010 veya üzeri, güç sorgu ve **hızlı Birleştir** ayarı gereklidir.

    ![Kayıtları Excel'de Aç](./media/sql-database-threat-detection-get-started/7_td_audit_records_open_excel.png)

9. Yapılandırmak için **hızlı Birleştir** - ayarlamayı **POWER QUERY** Şerit sekmesi, select **seçenekleri** Seçenekleri iletişim kutusunu görüntülemek için. Gizlilik bölümünü seçin ve ikinci seçeneği 'Gizlilik düzeylerini yoksayın ve potansiyel performansı geliştirin' - belirtin:

    ![Excel hızlı Birleştir](./media/sql-database-threat-detection-get-started/8_td_excel_fast_combine.png)

10. SQL denetim günlüklerini yüklemek için sekme doğru ayarlandığından ve 'Data' Şerit'i seçin ve 'Tümünü Yenile' düğmesini tıklatın ayarlarını parametrelerinde emin olun.

    ![Excel parametreleri](./media/sql-database-threat-detection-get-started/9_td_excel_parameters.png)

11. Sonuçları görünür **SQL denetim günlüklerini** algılandı anormal etkinlikler daha derin çözümlenmesi çalıştırın ve güvenlik olay uygulamanızda etkisini olanak tanıyan sayfası.


## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, kötü amaçlı kullanıcılar ya da yalnızca birkaç basit adımla yetkisiz erişime karşı veritabanınızın korumasını geliştirmek öğrendiniz.  Şunları öğrendiniz: 

> [!div class="checklist"]
> * Sunucu ve veya veritabanı için güvenlik duvarı kuralları ayarlayın
> * Güvenli bağlantı dizesi kullanarak veritabanınıza bağlanmak
> * Kullanıcı erişimini yönetme
> * Şifreleme ile verilerinizi koruma
> * SQL veritabanı denetimi etkinleştir
> * SQL veritabanı tehdit algılama etkinleştir

Coğrafi olarak dağıtılan bir veritabanı uygulaması hakkında bilgi almak için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
>[Coğrafi olarak dağıtılmış bir veritabanı uygulama](sql-database-implement-geo-distributed-database.md)

