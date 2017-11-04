---
title: "SQL Server DB Azure SQL veritabanına geçirme | Microsoft Docs"
description: "SQL Server veritabanını Azure SQL veritabanına geçirme öğrenin."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,migrate
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: Active
ms.date: 09/01/2017
ms.author: carlrab
ms.openlocfilehash: 526222944974c08f92aec2a8418e9b42401bc4d3
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2017
---
# <a name="migrate-your-sql-server-database-to-azure-sql-database"></a>SQL Server veritabanını Azure SQL veritabanına geçirme

SQL Server'ınızdaki taşıma veritabanını Azure SQL veritabanına Azure içinde boş bir SQL veritabanı oluşturma ve ardından kullanma olarak kadar basittir [veri geçiş Yardımcısı](https://www.microsoft.com/download/details.aspx?id=53595) Azure'da veritabanını almak için (DMA). Bu öğreticide, öğrenin:

> [!div class="checklist"]
> * (Yeni veya var olan bir Azure SQL Database sunucusu kullanarak) Azure portalında boş bir Azure SQL veritabanı oluşturma
> * (Daha önce oluşturduysanız) bir sunucu düzeyinde güvenlik duvarı Azure portalında oluşturma
> * Kullanım [veri geçiş Yardımcısı](https://www.microsoft.com/download/details.aspx?id=53595) (DMA), SQL Server veritabanınızın boş Azure SQL veritabanına aktarmak için 
> * Kullanım [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) veritabanı özelliklerini değiştirmek için (SSMS).

Bir Azure aboneliğiniz yoksa [ücretsiz bir hesap oluşturma](https://azure.microsoft.com/free/) başlamadan önce.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdaki ön koşulların karşılandığından emin olun:

- En son sürümü yüklü [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS).  
- En son sürümü yüklü [veri geçiş Yardımcısı](https://www.microsoft.com/download/details.aspx?id=53595) (DMA).
- Tanımladınız ve geçirmek için bir veritabanı erişimi. Bu öğretici kullanır [SQL Server 2008R2 AdventureWorks OLTP veritabanını](https://msftdbprodsamples.codeplex.com/releases/view/59211) örneğindeki SQL Server 2008R2 ya da daha yeni, ancak siz tercih ettiğiniz herhangi bir veritabanını kullanabilirsiniz. Uyumluluk sorunlarını gidermek için kullanmak [SQL Server veri araçları](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt)

## <a name="log-in-to-the-azure-portal"></a>Azure portalında oturum açma

[Azure Portal](https://portal.azure.com/)’da oturum açın.

## <a name="create-a-blank-sql-database"></a>Boş bir SQL veritabanı oluşturma

Azure SQL veritabanı bir dizi [işlem ve depolama kaynağı](sql-database-service-tiers.md) ile oluşturulur. Veritabanı bir [Azure kaynak grubu](../azure-resource-manager/resource-group-overview.md) ve bir [Azure SQL Veritabanı mantıksal sunucusu](sql-database-features.md) içinde oluşturulur. 

Boş bir SQL veritabanı oluşturmak için aşağıdaki adımları izleyin. 

1. Azure portalının sol üst köşesinde bulunan **Yeni** düğmesine tıklayın.

2. **Yeni** penceresinden **Veritabanları**’nı seçin ve **Yeni** sayfasında **SQL Veritabanı** altından **Oluştur**’u seçin.

   ![Boş veritabanı oluşturma](./media/sql-database-design-first-database/create-empty-database.png)

3. SQL Veritabanı formunu, önceki görüntüde gösterildiği gibi aşağıdaki bilgilerle doldurun:   

   | Ayar       | Önerilen değer | Açıklama | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Veritabanı adı** | mySampleDatabase | Geçerli veritabanı adları için bkz. [Veritabanı Tanımlayıcıları](https://docs.microsoft.com/sql/relational-databases/databases/database-identifiers). | 
   | **Abonelik** | Aboneliğiniz  | Abonelikleriniz hakkında daha ayrıntılı bilgi için bkz. [Abonelikler](https://account.windowsazure.com/Subscriptions). |
   | **Kaynak grubu** | myResourceGroup | Geçerli kaynak grubu adları için bkz. [Adlandırma kuralları ve kısıtlamalar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions). |
   | **Kaynak seçin** | Boş veritabanı | Boş bir veritabanı oluşturulması gerektiğini belirtir. |

4. Yeni veritabanınız için yeni bir sunucu oluşturup yapılandırmak üzere **Sunucu**’ya tıklayın. Doldurmak **yeni sunucu form** aşağıdaki bilgilerle: 

   | Ayar       | Önerilen değer | Açıklama | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Sunucu adı** | Genel olarak benzersiz bir ad | Geçerli sunucu adları için bkz. [Adlandırma kuralları ve kısıtlamalar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions). | 
   | **Sunucu yöneticisi oturum açma bilgileri** | Geçerli bir ad | Geçerli oturum açma adları için bkz. [Veritabanı Tanımlayıcıları](https://docs.microsoft.com/sql/relational-databases/databases/database-identifiers).|
   | **Parola** | Geçerli bir parola | Parolanız en az sekiz karakter olmalıdır ve aşağıdaki kategoriden üçünden karakterler içermelidir: büyük harf karakterler, küçük harfler, sayılar ve alfasayısal olmayan karakter. |
   | **Konum** | Geçerli bir konum | Bölgeler hakkında bilgi için bkz. [Azure Bölgeleri](https://azure.microsoft.com/regions/). |

   ![create database-server](./media/sql-database-design-first-database/create-database-server.png)

5. **Seç**'e tıklayın.

6. Hizmet katmanını, DTU'ların sayısını ve depolama alanı miktarını belirtmek için **Fiyatlandırma katmanı**’na tıklayın. Dtu'lar ve her hizmet katmanı için kullanılabilir olan depolama sayısı seçeneklerini araştırın. 

7. Bu öğretici için seçin **standart** hizmet katmanı ve seçmek için kaydırıcıyı kullanın **100 Dtu'lar (S3)** ve **400** GB depolama alanı.

   ![create database-s1](./media/sql-database-design-first-database/create-empty-database-pricing-tier.png)

8. **Ek Depolama** seçeneğini kullanmak için önizleme koşullarını kabul edin. 

   > [!IMPORTANT]
   > \* Mevcut depolama alanından büyük depolama alanları önizleme aşamasındadır ve ek maliyetler uygulanır. Ayrıntılar için bkz. [SQL Veritabanı fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/). 
   >
   >\* Premium katmanlarda 1 TB’den fazla depolama alanı şu bölgelerde sunulmaktadır: ABD Doğu2, Batı ABD, US Gov Virginia, Batı Avrupa, Almanya Orta, Güneydoğu Asya, Japonya Doğu, Avustralya Doğu, Kanada Orta ve Kanada Doğu. Bkz. [P11 P15 Geçerli Sınırlamalar](sql-database-resource-limits.md#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).  
   > 

9. Sunucu katmanını, DTU'ların sayısını ve depolama alanı miktarını seçtikten sonra **Uygula**’ya tıklayın.  

10. Seçin bir **harmanlama** boş veritabanı için (Bu öğretici için varsayılan değeri kullanın). Harmanlamaları hakkında daha fazla bilgi için bkz: [harmanlamaları](https://docs.microsoft.com/sql/t-sql/statements/collations)

11. SQL Veritabanı formunu tamamladıktan sonra veritabanını sağlamak için **Oluştur**’a tıklayın. Sağlama birkaç dakika sürer. 

12. Araç çubuğunda **Bildirimler**’e tıklayarak dağıtım işlemini izleyin.
    
     ![bildirim](./media/sql-database-get-started-portal/notification.png)

## <a name="create-a-server-level-firewall-rule"></a>Sunucu düzeyinde bir güvenlik duvarı kuralı oluşturma

SQL Veritabanı hizmeti, güvenlik duvarını belirli IP adreslerine açmaya yönelik bir güvenlik duvarı kuralı oluşturulmadıkça, dış uygulama ve araçların sunucuya ya da sunucu üzerindeki herhangi bir veritabanına bağlanmasını engelleyen sunucu düzeyinde bir güvenlik duvarı kuralı oluşturur. SQL Veritabanı güvenlik duvarı üzerinden yalnızca IP adresinize yönelik dış bağlantıları etkinleştirmek üzere istemcinizin IP adresi için bir [SQL Veritabanı sunucu düzeyi güvenlik duvarı kuralı](sql-database-firewall-configure.md) oluşturmak için bu adımları izleyin. 

> [!NOTE]
> SQL Veritabanı 1433 numaralı bağlantı noktası üzerinden iletişim kurar. Bir kurumsal ağ içerisinden bağlanmaya çalışıyorsanız, ağınızın güvenlik duvarı tarafından 1433 numaralı bağlantı noktası üzerinden giden trafiğe izin verilmiyor olabilir. Bu durumda BT departmanınız 1433 numaralı bağlantı noktasını açmadığı sürece Azure SQL Veritabanı sunucunuza bağlanamazsınız.
>

1. Dağıtım tamamlandıktan sonra, soldaki menüden **SQL veritabanları**'na ve ardından **SQL veritabanları** sayfasında **mySampleDatabase** öğesine tıklayın. Veritabanınıza ilişkin genel bakış sayfası açılır ve tam sunucu adı (örneğin, **mynewserver-20170824.database.windows.net**) görüntülenerek daha fazla yapılandırma seçeneği sunulur. 

2. Sonraki hızlı başlangıçlarda sunucunuza ve veritabanlarına bağlanmak için bu tam sunucu adını kopyalayın. 

   ![sunucu adı](./media/sql-database-get-started-portal/server-name.png) 

3. Tıklatın **ayarlayın sunucu Güvenlik Duvarı** araç çubuğunda. SQL Veritabanı sunucusu için **Güvenlik duvarı ayarları** sayfası açılır. 

   ![sunucu güvenlik duvarı kuralı](./media/sql-database-get-started-portal/server-firewall-rule.png) 

4. Geçerli IP adresinizi yeni bir güvenlik duvarı kuralına eklemek için araç çubuğunda **İstemci IP’si Ekle** öğesine tıklayın. Güvenlik duvarı kuralı, 1433 numaralı bağlantı noktasını tek bir IP adresi veya bir IP adresi aralığı için açabilir.

5. **Kaydet** düğmesine tıklayın. Geçerli IP adresiniz için mantıksal sunucuda 1433 numaralı bağlantı noktası açılarak sunucu düzeyinde güvenlik duvarı kuralı oluşturulur.

6. **Tamam**’a tıklayın ve sonra **Güvenlik duvarı ayarları** sayfasını kapatın.

Şimdi SQL veritabanı sunucusu ve SQL Server Management Studio, veri geçiş Yardımcısı'nı veya önceki yordamda oluşturduğunuz server yönetici hesabı kullanarak bu IP adresinden tercih ettiğiniz başka bir araç kullanarak veritabanlarını bağlanabilirsiniz.

> [!IMPORTANT]
> Varsayılan olarak, SQL Veritabanı güvenlik duvarı üzerinden erişim tüm Azure hizmetleri için etkindir. Tüm Azure hizmetleri için devre dışı bırakmak isterseniz bu sayfadaki **KAPALI** öğesine tıklayın.

## <a name="sql-server-connection-information"></a>SQL Server bağlantı bilgileri

Azure SQL Veritabanı sunucunuzun tam sunucu adını Azure portaldan alabilirsiniz. Veri geçiş Yardım ve SQL Server Management Studio dahil olmak üzere istemci araçlarını kullanarak, Azure SQL sunucunuza bağlanmak için tam sunucu adını kullanın.

1. [Azure Portal](https://portal.azure.com/)’da oturum açın.
2. Soldaki menüden **SQL Veritabanları**’nı seçin ve **SQL veritabanları** sayfasında veritabanınıza tıklayın. 
3. Veritabanınızın Azure portal sayfasındaki **Temel Bilgiler** bölmesinde, **Sunucu adını** bulup kopyalayın.

   ![bağlantı bilgileri](./media/sql-database-get-started-portal/server-name.png)

## <a name="migrate-your-database"></a>Veritabanınızın geçişini yapma

Kullanmak için aşağıdaki adımları izleyin  **[veri geçiş Yardımcısı](https://www.microsoft.com/download/details.aspx?id=53595)**  veritabanınız Azure SQL veritabanı geçiş için hazır olup olmadığını değerlendirmek için ve geçiş işlemini tamamlamak için.

1. Açık **veri geçiş Yardımcısı**. Geçirmeyi planladığınız veritabanını içeren SQL Server örneği bağlantısı ve internet bağlantısı ile DMA herhangi bir bilgisayarda çalıştırabilirsiniz. Geçirmekte olduğunuz SQL Server örneğini barındıran bilgisayara yüklemeniz gerekmez. Bir önceki yordamda oluşturduğunuz güvenlik duvarı kuralı, veri geçiş Yardımcısı'nı çalıştırdığınız bilgisayarı olması gerekir.

     ![açık veri geçiş Yardımcısı](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-open.png)

2. Sol menüde tıklatın **+ yeni** oluşturmak için bir **değerlendirme** projesi. İstenen değerleri doldurun ve ardından **oluşturma**:

   | Ayar      | Önerilen değer | Açıklama | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | Proje türü | Geçiş | Veritabanınız için geçiş değerlendirmek veya değerlendirmek seçmek üzere seçin ve aynı iş akışının bir parçası olarak geçiş |
   |Proje adı|Geçiş Öğreticisi| Açıklayıcı bir ad |
   |Kaynak sunucu türü| SQL Server | Şu anda desteklenen tek kaynak budur |
   |Hedef sunucu türü| Azure SQL Database| Seçenekler şunlardır: Azure SQL veritabanı, SQL Server, SQL Server üzerinde Azure sanal makineler |
   |Geçiş kapsamı| Şeması ve verisi| Seçenekler şunlardır: yalnızca şeması ve verisi, yalnızca şema veri |
   
   ![Yeni veri geçiş Yardımcısı projesi](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-new-project.png)

3.  Üzerinde **kaynak seçme** sayfasında, istenen değerleri doldurun ve ardından **Bağlan**:

    | Ayar      | Önerilen değer | Açıklama | 
    | ------------ | ------------------ | ------------------------------------------------- | 
    | Sunucu adı | Sunucu adı veya IP adresi | Sunucu adı veya IP adresi |
    | Kimlik doğrulama türü | Tercih edilen kimlik doğrulama türü| Seçimleri: Kimlik doğrulaması, Active Directory parola kimlik doğrulaması Windows kimlik doğrulaması, SQL Server kimlik doğrulaması, Active Directory tümleşik |
    | Kullanıcı adı | Oturum açma adı | Oturumunuzla olmalıdır **denetim SUNUCUSUNA** izinleri |
    | Parola| Parolanızı | Parolanızı |
    | Bağlantı özellikleri| Seçin **bağlantıyı şifrelemek** ve **güven sunucu sertifikası** ortamınız için uygun şekilde. | Sunucunuz için Bağlan uygun Özellikler'i seçin |

    ![Yeni veri geçiş select kaynağı](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-source.png)

5. Azure SQL veritabanına geçirme ve ardından kaynak sunucunuzdan tek bir veritabanı seçin **sonraki**. Bu öğretici için yalnızca tek bir veritabanı yok.

6. Üzerinde **Select hedef** sayfasında, istenen değerleri doldurun ve ardından **Bağlan**:

    | Ayar      | Önerilen değer | Açıklama | 
    | ------------ | ------------------ | ------------------------------------------------- | 
    | Sunucu adı | Tam Azure veritabanı sunucunuzun adını | Önceki yordamda tam Azure veritabanı sunucusu adı |
    | Kimlik doğrulama türü | SQL Server Kimlik Doğrulaması | SQL Server kimlik doğrulaması yalnızca Bu öğretici yazılır, ancak Active Directory tümleşik kimlik doğrulaması ve Active Directory parola kimlik doğrulaması Azure SQL veritabanı tarafından da desteklenir seçenektir |
    | Kullanıcı adı | Oturum açma adı | Oturumunuzla olmalıdır **denetim veritabanı** kaynak veritabanı izinleri |
    | Parola| Parolanızı | Parolanızı |
    | Bağlantı özellikleri| Seçin **bağlantıyı şifrelemek** ve **güven sunucu sertifikası** ortamınız için uygun şekilde. | Sunucunuz için Bağlan uygun Özellikler'i seçin |

    ![Yeni veri geçiş select hedefi](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-target.png)

7. Bir önceki yordamda oluşturduğunuz hedef sunucudan veritabanını seçin ve ardından **sonraki** kaynak veritabanı şeması değerlendirme işlemini başlatmak için. Bu öğretici için yalnızca tek bir veritabanı yok. Bu veritabanı uyumluluk düzeyi 140 için Azure SQL veritabanındaki tüm yeni veritabanları için varsayılan uyumluluk düzeyi olduğu ayarlandığına dikkat edin.

   > [!IMPORTANT] 
   > Azure SQL veritabanına veritabanınızı geçirdikten sonra geriye dönük uyumluluk amacıyla bir belirtilen uyumluluk düzeyinde veritabanı çalışmaya seçebilirsiniz. Uygulamaları ve belirli bir uyumluluk düzeyinde bir veritabanı işletim seçenekleri hakkında daha fazla bilgi için bkz: [ALTER veritabanı uyumluluk düzeyi](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level). Ayrıca bkz. [ALTER veritabanı kapsamlı yapılandırma](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) Uyumluluk Düzeyleri ilgili ek veritabanı düzeyi ayarları hakkında bilgi için.
   >

8. Üzerinde **nesneleri seçin** sayfasında, kaynak veritabanı sonra şema değerlendirmesi işlemi tamamlandığında, geçiş işlemi için seçilen nesneleri gözden geçirin ve sorunları içeren nesneleri gözden geçirin. Örneğin, **dbo.uspSearchCandidateResumes** için nesne **SERVERPROPERTY('LCID')** davranış değişiklikleri ve **HumanResourcesJobCandidate** nesnesi Tam metin araması değiştirir. 

   > [!IMPORTANT] 
   > Kaynak veritabanınıza geçirdiğinizde veritabanının tasarım ve uygulamanızın tasarım bağlı olarak, ya da değiştirmeniz gerekebilir veya hem veritabanınız veya uygulamanızın geçişten sonra (ve bazı durumlarda, geçiş işleminden önce). Geçişinizi etkileyebilecek Transact-SQL farkları hakkında daha fazla bilgi için bkz: [çözme Transact-SQL farkları SQL veritabanı geçiş sırasında](sql-database-transact-sql-information.md).

     ![Yeni veri geçiş değerlendirme ve Nesne Seçimi](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-assessment-results.png)

9. Tıklatın **oluştur SQL komut dosyası** kaynak veritabanı şema nesnelerindeki komut dosyası için. 
10. Oluşturulan komut dosyasını gözden geçirin ve ardından **sonraki sorun** tanımlanan değerlendirme sorunları ve önerileri gözden geçirmek için gerektiği gibi. Örneğin, tam metin araması için yükseltme sırasında tam metin özellikleri yararlanarak uygulamalarınızı test önerilir. İsterseniz betiği kopyalayın ya da kaydedin.

     ![Yeni veri geçiş oluşturulan komut dosyası](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-generated-script.png)

11. Tıklatın **dağıtma şema** ve şema geçiş işlemini izleyebilirsiniz.

     ![Yeni veri geçiş şema geçiş](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-schema-migration.png)

12. Şema geçiş tamamlandığında, sonuçları hataları gözden geçirin ve bulunmazlar varsayıldığında, ardından **veri geçişi**.
13. Üzerinde **tabloları seçme** sayfasında, geçiş işlemi için seçilen tabloları gözden geçirin ve ardından **Başlat veri geçişi**.

     ![Yeni veri geçiş veri geçişi](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-data-migration.png)

14. Geçiş işlemini izleyebilirsiniz.

     ![Yeni veri geçiş veri geçiş işlemi](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-data-migration-process.png)

## <a name="connect-to-the-database-with-ssms"></a>Veritabanı SSMS ile bağlanma

Kullanım [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms) Azure SQL Database sunucunuza bağlantı kuramıyor.

1. SQL Server Management Studio’yu açın.

2. **Sunucuya Bağlan** iletişim kutusuna şu bilgileri girin:

   | Ayar       | Önerilen değer | Açıklama | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | Sunucu türü | Veritabanı altyapısı | Bu değer gereklidir |
   | Sunucu adı | Tam sunucu adı | Adı, şunun gibi olmalıdır: **mynewserver20170824.database.windows.net**. |
   | Kimlik Doğrulaması | SQL Server Kimlik Doğrulaması | Bu öğreticide yapılandırdığımız tek kimlik doğrulaması türü SQL Kimlik Doğrulamasıdır. |
   | Oturum Aç | Sunucu yöneticisi hesabı | Bu, sunucuyu oluştururken belirttiğiniz hesaptır. |
   | Parola | Sunucu yöneticisi hesabınızın parolası | Bu, sunucuyu oluştururken belirttiğiniz paroladır. |

   ![sunucuya bağlan](./media/sql-database-connect-query-ssms/connect.png)

3. **Sunucuya bağlan** iletişim kutusunda **Seçenekler**’e tıklayın. **Veritabanına bağlan** bölümünde bu veritabanına bağlanmak için **mySampleDatabase** yazın.

   ![sunucuda veritabanına bağlanma](./media/sql-database-connect-query-ssms/options-connect-to-db.png)  

4. **Bağlan**'a tıklayın. SSMS’te Nesne Gezgini penceresi açılır. 

5. Nesne Gezgini’nde **Veritabanları**’nı ve ardından **mySampleDatabase** öğesini genişleterek nesneleri örnek veritabanında görüntüleyin.

   ![Veritabanı nesneleri](./media/sql-database-connect-query-ssms/connected.png)  

## <a name="change-database-properties"></a>Veritabanı özelliklerini değiştir

Hizmet katmanı, performans düzeyi ve SQL Server Management Studio'yu kullanarak uyumluluk düzeyini değiştirebilirsiniz. İçeri aktarma aşaması sırasında en iyi performans için daha yüksek bir performans katmanı veritabanına aktarmak, ancak içeri aktarılan veritabanını etkin olarak kullanmaya hazır olana kadar tasarruf için alma işlemi tamamlandıktan sonra ölçeklendirmenizi öneririz. Uyumluluk düzeyinin değiştirilmesi, daha iyi performans ve Azure SQL veritabanı hizmetinin en yeni özelliklere erişim elde etmek. Eski bir veritabanını geçirdiğinizde, veritabanı uyumluluk düzeyi düzeyinde içeri aktarılmakta olan veritabanı ile uyumlu olan en düşük desteklenen korunur. Daha fazla bilgi için bkz: [iyileştirilmiş sorgu performansı ile Azure SQL veritabanı uyumluluk düzeyi 130](sql-database-compatibility-level-query-performance-130.md).

1. Nesne Gezgini'nde sağ **mySampleDatabase** ve ardından **yeni sorgu**. Veritabanına bağlı bir sorgu penceresi açılır.

2. Hizmet katmanı ayarlamak için aşağıdaki komutu yürütün **standart** ve performans düzeyine **S1**.

    ```sql
    ALTER DATABASE mySampleDatabase 
    MODIFY 
        (
        EDITION = 'Standard'
        , MAXSIZE = 250 GB
        , SERVICE_OBJECTIVE = 'S1'
    );
    ```

## <a name="next-steps"></a>Sonraki adımlar 
Bu öğreticide için öğrenilen:

> * Azure portalında boş bir Azure SQL veritabanı oluşturma 
> * Azure portalında bir sunucu düzeyinde güvenlik duvarı oluşturma 
> * Kullanım [veri geçiş Yardımcısı](https://www.microsoft.com/download/details.aspx?id=53595) (DMA), SQL Server veritabanınızın boş Azure SQL veritabanına aktarmak için 
> * Kullanım [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) veritabanı özelliklerini değiştirmek için (SSMS).

Veritabanınızın güvenliğini sağlamak öğrenmek için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [Azure SQL veritabanınızı güvenli](sql-database-security-tutorial.md).


