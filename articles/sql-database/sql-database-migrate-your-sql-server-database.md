---
title: DMA kullanarak SQL Server DB’yi Azure SQL Veritabanı’na geçirme | Microsoft Docs
description: DMA kullanarak SQL Server veritabanınızı Azure SQL Veritabanı’na geçirmeyi öğrenin.
services: sql-database
author: sachinpMSFT
manager: craigg
ms.service: sql-database
ms.custom: mvc,migrate
ms.topic: tutorial
ms.date: 07/02/2018
ms.author: carlrab
ms.openlocfilehash: ceab627d98149774a3eb767ee56d688f9c11ff99
ms.sourcegitcommit: 756f866be058a8223332d91c86139eb7edea80cc
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2018
ms.locfileid: "37346850"
---
# <a name="migrate-your-sql-server-database-to-azure-sql-database-using-dma"></a>DMA kullanarak SQL Server veritabanınızı Azure SQL Veritabanı’na geçirme

SQL Server veritabanınızı Azure SQL Veritabanı’na taşımak, Azure’da boş bir SQL veritabanı oluşturup ardından [Data Migration Yardımcısı](https://www.microsoft.com/download/details.aspx?id=53595)’nı (DMA) kullanarak veritabanını Azure’a aktarmanız kadar basittir. Ek geçiş seçenekleri için bkz. [Veritabanınızı Azure SQL Veritabanı’na geçirme](sql-database-cloud-migrate.md).

> [!IMPORTANT]
> Azure SQL Veritabanı Yönetilen Örneği’ne geçiş için bkz. [SQL Server’dan Yönetilen Örneğe Geçirme](sql-database-managed-instance-migrate.md)

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Azure Portal'da boş bir Azure SQL veritabanı oluşturma (yeni veya mevcut Azure SQL Veritabanı sunucusunu kullanarak)
> * Azure portalında sunucu düzeyinde bir güvenlik duvarı oluşturma (daha önce oluşturulmadıysa)
> * SQL Server veritabanınızı boş Azure SQL veritabanına aktarmak için [Veri Geçiş Yardımcısı](https://www.microsoft.com/download/details.aspx?id=53595)'nı (DMA) kullanma 
> * Veritabanı özelliklerini değiştirmek için [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)'yu (SSMS) kullanma.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdaki ön koşulların karşılandığından emin olun:

- [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)’nun (SSMS) en yeni sürümü yüklendi.  
- [Veri Geçiş Yardımcısı](https://www.microsoft.com/download/details.aspx?id=53595)'nın (DMA) en yeni sürümü yüklendi.
- Geçiş için bir veritabanı tanımladınız ve bu veritabanına erişiminiz var. Bu öğreticide,SQL Server 2008 R2 veya daha yeni sürümünün örneği üzerinde [SQL Server 2008R2 AdventureWorks OLTP veritabanı](https://msftdbprodsamples.codeplex.com/releases/view/59211) kullanılır, ama siz dilediğiniz veritabanını kullanabilirsiniz. Uyumluluk sorunlarını çözmek için [SQL Server Veri Araçları](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt)'nı kullanın

## <a name="log-in-to-the-azure-portal"></a>Azure portalında oturum açma

[Azure Portal](https://portal.azure.com/)’da oturum açın.

## <a name="create-a-blank-sql-database"></a>Boş bir SQL veritabanı oluşturma

Azure SQL veritabanı bir dizi [işlem ve depolama kaynağı](sql-database-service-tiers-dtu.md) ile oluşturulur. Veritabanı bir [Azure kaynak grubu](../azure-resource-manager/resource-group-overview.md) ve bir [Azure SQL Veritabanı mantıksal sunucusu](sql-database-features.md) içinde oluşturulur. 

Boş bir SQL veritabanı oluşturmak için aşağıdaki adımları izleyin. 

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesine tıklayın.

2. **Yeni** penceresinden **Veritabanları**’nı seçin ve **Yeni** sayfasında **SQL Veritabanı** altından **Oluştur**’u seçin.

   ![create empty-database](./media/sql-database-design-first-database/create-empty-database.png)

3. SQL Veritabanı formunu, önceki görüntüde gösterildiği gibi aşağıdaki bilgilerle doldurun:   

   | Ayar       | Önerilen değer | Açıklama | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Veritabanı adı** | mySampleDatabase | Geçerli veritabanı adları için bkz. [Veritabanı Tanımlayıcıları](https://docs.microsoft.com/sql/relational-databases/databases/database-identifiers). | 
   | **Abonelik** | Aboneliğiniz  | Abonelikleriniz hakkında daha ayrıntılı bilgi için bkz. [Abonelikler](https://account.windowsazure.com/Subscriptions). |
   | **Kaynak grubu** | myResourceGroup | Geçerli kaynak grubu adları için bkz. [Adlandırma kuralları ve kısıtlamalar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions). |
   | **Kaynak seçme** | Boş veritabanı | Boş bir veritabanı oluşturulması gerektiğini belirtir. |

4. Yeni veritabanınız için yeni bir sunucu oluşturup yapılandırmak üzere **Sunucu**’ya tıklayın. **Yeni sunucu formu**’nu aşağıdaki bilgilerle doldurun: 

   | Ayar       | Önerilen değer | Açıklama | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Sunucu adı** | Genel olarak benzersiz bir ad | Geçerli sunucu adları için bkz. [Adlandırma kuralları ve kısıtlamalar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions). | 
   | **Sunucu yöneticisi oturum açma bilgileri** | Geçerli bir ad | Geçerli oturum açma adları için bkz. [Veritabanı Tanımlayıcıları](https://docs.microsoft.com/sql/relational-databases/databases/database-identifiers).|
   | **Parola** | Geçerli bir parola | Parolanızda en az 8 karakter bulunmalı ve parolanız şu üç kategoriden karakterler içermelidir: büyük harf karakterler, küçük harf karakterler, sayılar ve alfasayısal olmayan karakterler. |
   | **Konum** | Geçerli bir konum | Bölgeler hakkında bilgi için bkz. [Azure Bölgeleri](https://azure.microsoft.com/regions/). |

   ![create database-server](./media/sql-database-design-first-database/create-database-server.png)

5. **Seç**'e tıklayın.

6. Hizmet katmanını, DTU'ların sayısını ve depolama alanı miktarını belirtmek için **Fiyatlandırma katmanı**’na tıklayın. Her hizmet katmanı için kullanılabilir DTU sayısı ve depolama alanı seçeneklerini araştırın. 

7. Bu öğreticide, **standart** hizmet katmanını seçip kaydırıcıyı kullanarak **100 DTU (S3)** ve **400** GB depolama alanını seçin.

   ![create database-s1](./media/sql-database-design-first-database/create-empty-database-pricing-tier.png)

8. **Ek Depolama** seçeneğini kullanmak için önizleme koşullarını kabul edin. 

   > [!IMPORTANT]
   > Premium katmanında 1 TB’tan fazla depolama alanı, şu an için şu bölgeler dışındaki tüm bölgelerde kullanılabilir: Orta Batı ABD, Çin Doğu, USDoDCentral, USGov Iowa, Almanya Orta, USDoDEast, US Gov Güneybatı, Almanya Kuzeydoğu, Çin Kuzey. Diğer bölgelerde Premium katmanda depolama için 1 TB üst sınırı uygulanır. Bkz. [P11 P15 Geçerli Sınırlamalar]( sql-database-dtu-resource-limits-single-databases.md#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).  

9. Sunucu katmanını, DTU'ların sayısını ve depolama alanı miktarını seçtikten sonra **Uygula**’ya tıklayın.  

10. Boş veritabanı için bir **harmanlama** seçin (bu öğretici için varsayılan değeri kullanın). Harmanlamalar hakkında daha fazla bilgi için bkz. [Harmanlamalar](https://docs.microsoft.com/sql/t-sql/statements/collations)

11. SQL Veritabanı formunu tamamladıktan sonra veritabanını sağlamak için **Oluştur**’a tıklayın. Sağlama birkaç dakika sürer. 

12. Araç çubuğunda **Bildirimler**’e tıklayarak dağıtım işlemini izleyin.
    
     ![bildirim](./media/sql-database-get-started-portal/notification.png)

## <a name="create-a-server-level-firewall-rule"></a>Sunucu düzeyinde bir güvenlik duvarı kuralı oluşturma

SQL Veritabanı hizmeti, güvenlik duvarını belirli IP adreslerine açmaya yönelik bir güvenlik duvarı kuralı oluşturulmadıkça, dış uygulama ve araçların sunucuya ya da sunucu üzerindeki herhangi bir veritabanına bağlanmasını engelleyen sunucu düzeyinde bir güvenlik duvarı kuralı oluşturur. SQL Veritabanı güvenlik duvarı üzerinden yalnızca IP adresinize yönelik dış bağlantıları etkinleştirmek üzere istemcinizin IP adresi için bir [SQL Veritabanı sunucu düzeyi güvenlik duvarı kuralı](sql-database-firewall-configure.md) oluşturmak için bu adımları izleyin. 

> [!NOTE]
> SQL Veritabanı 1433 numaralı bağlantı noktası üzerinden iletişim kurar. Bir kurumsal ağ içerisinden bağlanmaya çalışıyorsanız, ağınızın güvenlik duvarı tarafından 1433 numaralı bağlantı noktası üzerinden giden trafiğe izin verilmiyor olabilir. Bu durumda BT departmanınız 1433 numaralı bağlantı noktasını açmadığı sürece Azure SQL Veritabanı sunucunuza bağlanamazsınız.
>

1. Dağıtım tamamlandıktan sonra, soldaki menüden **SQL veritabanları**'na ve ardından **SQL veritabanları** sayfasında **mySampleDatabase** öğesine tıklayın. Veritabanınıza ilişkin genel bakış sayfası açılır ve tam sunucu adı (örneğin, **mynewserver-20170824.database.windows.net**) görüntülenerek daha fazla yapılandırma seçeneği sunulur. 

2. Bu tam sunucu adını, sonraki hızlı başlangıçlarda sunucunuza ve veritabanlarına bağlanırken kullanmak için kopyalayın. 

   ![sunucu adı](./media/sql-database-get-started-portal/server-name.png) 

3. Araç çubuğunda **Sunucu güvenlik duvarını ayarla**’ya tıklayın. SQL Veritabanı sunucusu için **Güvenlik duvarı ayarları** sayfası açılır. 

   ![sunucu güvenlik duvarı kuralı](./media/sql-database-get-started-portal/server-firewall-rule.png) 

4. Geçerli IP adresinizi yeni bir güvenlik duvarı kuralına eklemek için araç çubuğunda **İstemci IP’si Ekle** öğesine tıklayın. Güvenlik duvarı kuralı, 1433 numaralı bağlantı noktasını tek bir IP adresi veya bir IP adresi aralığı için açabilir.

5. **Kaydet**’e tıklayın. Geçerli IP adresiniz için mantıksal sunucuda 1433 numaralı bağlantı noktası açılarak sunucu düzeyinde güvenlik duvarı kuralı oluşturulur.

6. **Tamam**’a tıklayın ve sonra **Güvenlik duvarı ayarları** sayfasını kapatın.

Artık SQL Server Management Studio’yu, Veri Geçiş Yardımcısı'nı veya seçtiğiniz başka bir aracı kullanarak, önceki yordamda oluşturduğunuz sunucu yöneticisi hesabıyla bu IP adresinden SQL Veritabanı sunucusuna ve sunucuya ait veritabanlarına bağlanabilirsiniz.

> [!IMPORTANT]
> Varsayılan olarak, SQL Veritabanı güvenlik duvarı üzerinden erişim tüm Azure hizmetleri için etkindir. Tüm Azure hizmetleri için devre dışı bırakmak isterseniz bu sayfadaki **KAPALI** öğesine tıklayın.

## <a name="sql-server-connection-information"></a>SQL Server bağlantı bilgileri

Azure SQL Veritabanı sunucunuzun tam sunucu adını Azure portaldan alabilirsiniz. Veri Geçiş Yardımcısı ve SQL Server Management Studio gibi istemci araçlarını kullanarak Azure SQL sunucunuza bağlanmak için tam sunucu adını kullanırsınız.

1. [Azure Portal](https://portal.azure.com/)’da oturum açın.
2. Soldaki menüden **SQL Veritabanları**’nı seçin ve **SQL veritabanları** sayfasında veritabanınıza tıklayın. 
3. Veritabanınızın Azure portal sayfasındaki **Temel Bilgiler** bölmesinde, **Sunucu adını** bulup kopyalayın.

   ![bağlantı bilgileri](./media/sql-database-get-started-portal/server-name.png)

## <a name="migrate-your-database"></a>Veritabanınızın geçişini yapma

**[Veri Geçiş Yardımcısı](https://www.microsoft.com/download/details.aspx?id=53595)**'nı kullanarak veritabanınızın Azure SQL Veritabanına geçişe hazır olup olmadığını değerlendirmek ve geçişi tamamlamak için bu adımları izleyin.

1. **Veri Geçiş Yardımcısı**'nı açın. Geçirmeyi planladığınız veritabanını içeren SQL Server örneğiyle bağlantısı olan ve ayrıca İnternet bağlantısı da olan herhangi bir bilgisayarda DMA'yı çalıştırabilirsiniz. Bunu, geçirmekte olduğunuz SQL Server örneği barındıran bilgisayara yüklemeniz gerekmez. Önceki yordamda oluşturduğunuz güvenlik duvarı kuralı, Veri Geçiş Yardımcısı'nı çalıştırdığınız bilgisayara yönelik olmalıdır.

     ![veri geçiş yardımcısını açın](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-open.png)

2. Sol taraftaki menüde **+ Yeni**'ye tıklayarak **Assessment** projesi oluşturun. İstenen değerleri doldurun ve **Oluştur**'a tıklayın:

   | Ayar      | Önerilen değer | Açıklama | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | Proje türü | Geçiş | Veritabanınızı geçiş için değerlendirmeyi veya değerlendirme ile geçişi aynı iş yükünün parçası olarak gerçekleştirmeyi seçin |
   |Proje adı|Geçiş öğreticisi| Açıklayıcı bir ad |
   |Kaynak sunucu türü| SQL Server | Bu, şu anda desteklenen tek kaynaktır |
   |Hedef sunucu türü| Azure SQL Database| Seçenekler şunlardır: Azure SQL Veritabanı, SQL Server, Azure Sanal Makinelerinde SQL Server |
   |Geçiş Kapsamı| Şema ve veri| Seçenekler şunlardır: Şema ve veri, yalnızca şema, yalnızca veri |
   
   ![yeni veri geçiş yardımcısı projesi](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-new-project.png)

3.  **Kaynak seç** sayfasında, istenen değerleri doldurun ve sonra da **Bağlan**'a tıklayın:

    | Ayar      | Önerilen değer | Açıklama | 
    | ------------ | ------------------ | ------------------------------------------------- | 
    | Sunucu adı | Sunucu adınız veya IP adresiniz | Sunucu adınız veya IP adresiniz |
    | Kimlik doğrulaması türü | Tercih ettiğiniz kimlik doğrulama türü| Seçenekler: Windows Kimlik Doğrulaması, SQL Server Kimlik Doğrulaması, Active Directory Tümleşik Kimlik Doğrulaması, Active Directory Parola Kimlik Doğrulaması |
    | Kullanıcı adı | Oturum açma adınız | Oturum açma bilgilerinizin **SUNUCU DENETLEME** izinleri olmalıdır |
    | Parola| Parolanız | Parolanız |
    | Bağlantı özellikleri| Ortamınıza uygun olarak **Bağlantıyı şifrele**'yi ve **Sunucu sertifikasına güven**'i seçin. | Sunucunuza bağlanmak için uygun özellikleri seçin |

    ![yeni veri geçişi kaynağı seçme](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-source.png)

5. Azure SQL Veritabanı'na geçirmek üzere kaynak sunucunuzdan tek bir veritabanı seçin ve ardından **İleri**'ye tıklayın. Bu öğretici için, yalnızca bir veritabanı vardır.

6. **Hedef seç** sayfasında, istenen değerleri doldurun ve sonra da **Bağlan**'a tıklayın:

    | Ayar      | Önerilen değer | Açıklama | 
    | ------------ | ------------------ | ------------------------------------------------- | 
    | Sunucu adı | Tam Azure Veritabanı sunucu adınız | Önceki yordamdan gelen tam Azure Veritabanı sunucu adınız |
    | Kimlik doğrulaması türü | SQL Server Kimlik Doğrulaması | Bu öğretici yazıldığı sırada SQL Server kimlik doğrulaması tek seçenekti, ama Azure SQL Veritabanı Active Directory Tümleşik Kimlik Doğrulaması ile Active Directory Parola Kimlik Doğrulaması'nı da destekler |
    | Kullanıcı adı | Oturum açma adınız | Oturum açma bilgilerinizin kaynak veritabanı üzerinde **VERİTABANINI DENETLEME** izinleri olmalıdır |
    | Parola| Parolanız | Parolanız |
    | Bağlantı özellikleri| Ortamınıza uygun olarak **Bağlantıyı şifrele**'yi ve **Sunucu sertifikasına güven**'i seçin. | Sunucunuza bağlanmak için uygun özellikleri seçin |

    ![yeni veri geçişi hedefi seçme](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-target.png)

7. Önceki yordamda oluşturduğunuz hedef sunucudan veritabanını seçin ve ardından **İleri**'ye tıklayarak kaynak veritabanı şemasını değerlendirme işlemini başlatın. Bu öğretici için, yalnızca bir veritabanı vardır. Bu veritabanı için uyumluluk düzeyinin 140 olarak ayarlandığına dikkat edin. Bu, Azure SQL Veritabanı'nda tüm yeni veritabanları için varsayılan uyumluluk düzeyidir.

   > [!IMPORTANT] 
   > Veritabanınızı Azure SQL Veritabanı'na geçirdikten sonra, veritabanını geriye dönük uyumluluk amacıyla belirtilen uyumluluk düzeyinde çalıştırmayı seçebilirsiniz. Veritabanını belirli bir uyumluluk düzeyinde çalıştırmanın etkileri ve buna yönelik seçenekler hakkında daha fazla bilgi için, bkz. [Veritabanı Uyumluluk Düzeyini Değiştirme](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level). Uyumluluk düzeyleriyle ilgili ek veritabanı düzeyi ayarları hakkında bilgi için, ayrıca bkz. [VERİTABANI KAPSAMLI YAPILANDIRMAYI DEĞİŞTİRME](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql).
   >

8. **Nesneleri seçin** sayfasında, kaynak veritabanı şeması değerlendirme işlemi tamamlandıktan sonra, geçiş için seçilen nesneleri ve sorunlu nesneleri gözden geçirin. Örneğin, **SERVERPROPERTY('LCID')** davranış değişiklikleri için **dbo.uspSearchCandidateResumes** nesnesini gözden geçirin ve Tam Metin Arama değişiklikleri için de **HumanResourcesJobCandidate** nesnesini gözden geçirin. 

   > [!IMPORTANT] 
   > Veritabanının tasarımına ve uygulamanızın tasarımına bağlı olarak, kaynak veritabanınızı geçirirken, geçiş sonrasında veritabanınız ve uygulamanızdan birinde veya her ikisinde değişiklik yapmanız gerekebilir (ve bazı durumlarda, geçişten önce de gerekebilir). Geçişinizi etkileyebilecek Transact-SQL farklılıkları hakkında bilgi için bkz. [SQL Veritabanına geçiş sırasında Transact-SQL farklılıklarını çözümleme](sql-database-transact-sql-information.md).

     ![yeni veri geçişi değerlendirme ve nesne seçimi](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-assessment-results.png)

9. Kaynak veritabanında şema nesnelerinin betiğini sağlamak için **SQL betiği oluştur**'a tıklayın. 
10. Oluşturulan betiği gözden geçirin ve ardından tanımlanan değerlendirme sorunları ve önerilerini gözden geçirmek için gerektiği gibi **Sonraki sorun**'a tıklayın. Örneğin Tam Metin Araması için, yükseltme yaptığınızda uygulamalarınızın Tam Metin özelliklerinden yararlanıp yararlanmadığını test etmeniz önerilir. İsterseniz, betiği kaydedebilir veya kopyalayabilirsiniz.

     ![yeni veri geçişi oluşturulan betik](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-generated-script.png)

11. **Şemayı dağıt**'a tıklayın ve şema geçiş işlemini izleyin.

     ![yeni veri geçişi şema geçişi](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-schema-migration.png)

12. Şema geçişi tamamlandığında, hatalar için sonuçları gözden geçirin ve ardından, hata olmadığını varsayarsak, **Verileri geçir**'e tıklayın.
13. **Tablo seçin** sayfasında, geçiş için seçilen tabloları gözden geçirin ve **Veri geçişini başlat**'a tıklayın.

     ![yeni veri geçişi veri geçişi](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-data-migration.png)

14. Geçiş işlemini izleyin.

     ![yeni veri geçişi veri geçiş işlemi](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-data-migration-process.png)

## <a name="connect-to-the-database-with-ssms"></a>SSMS ile veritabanına bağlanma

[SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms)’yu kullanarak Azure SQL Veritabanı sunucunuzla bağlantı kurun.

1. SQL Server Management Studio’yu açın.

2. **Sunucuya Bağlan** iletişim kutusuna şu bilgileri girin:

   | Ayar       | Önerilen değer | Açıklama | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | Sunucu türü | Veritabanı altyapısı | Bu değer gereklidir |
   | Sunucu adı | Tam sunucu adı | Ad şunun gibi olmalıdır: **mynewserver20170824.database.windows.net**. |
   | Kimlik Doğrulaması | SQL Server Kimlik Doğrulaması | Bu öğreticide yapılandırdığımız tek kimlik doğrulaması türü SQL Kimlik Doğrulamasıdır. |
   | Oturum Aç | Sunucu yöneticisi hesabı | Bu, sunucuyu oluştururken belirttiğiniz hesaptır. |
   | Parola | Sunucu yöneticisi hesabınızın parolası | Bu, sunucuyu oluştururken belirttiğiniz paroladır. |

   ![sunucuya bağlan](./media/sql-database-connect-query-ssms/connect.png)

3. **Sunucuya bağlan** iletişim kutusunda **Seçenekler**’e tıklayın. **Veritabanına bağlan** bölümünde bu veritabanına bağlanmak için **mySampleDatabase** yazın.

   ![sunucuda veritabanına bağlanma](./media/sql-database-connect-query-ssms/options-connect-to-db.png)  

4. **Bağlan**'a tıklayın. SSMS’te Nesne Gezgini penceresi açılır. 

5. Nesne Gezgini’nde **Veritabanları**’nı ve ardından **mySampleDatabase** öğesini genişleterek nesneleri örnek veritabanında görüntüleyin.

   ![veritabanı nesneleri](./media/sql-database-connect-query-ssms/connected.png)  

## <a name="change-database-properties"></a>Veritabanı özelliklerini değiştirme

SQL Server Management Studio'yu kullanarak hizmet katmanını, performans düzeyini ve uyumluluk düzeyini değiştirebilirsiniz. İçeri aktarma aşamasında, en iyi performansı elde etmek için daha yüksek bir performans katmanındaki veritabanına aktarmanızı öneririz; ama içeri aktarma tamamlandıktan sonra, içeri aktarılan veritabanını etkin bir şekilde kullanmaya hazır olana kadar paradan tasarruf etmek için ölçeği aşağı çekin. Uyumluluk düzeyinin değiştirilmesi daha iyi bir performans getirebilir ve Azure SQL Veritabanı hizmetinin en yeni özelliklerine erişim sağlar. Daha eski bir veritabanını geçirirken, onun veritabanı uyumluluk düzeyi desteklenen ve içeri aktarılan veritabanıyla uyumlu olan en düşük düzeyde tutulur. Daha fazla bilgi için bkz. [Azure SQL Veritabanı'nda uyumluluk düzeyi 130 ile geliştirilen sorgu performansı](sql-database-compatibility-level-query-performance-130.md).

1. Nesne Gezgini’nde **mySampleDatabase** öğesine sağ tıklayın ve sonra da **Yeni Sorgu**’ya tıklayın. Veritabanınıza bağlı bir sorgu penceresi açılır.

2. Hizmet katmanını **Standart** ve performans düzeyini **S1** olarak ayarlamak için aşağıdaki komutu yürütün.

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
Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> * Azure portalında boş bir Azure SQL veritabanı oluşturma 
> * Azure portalında sunucu düzeyinde bir güvenlik duvarı oluşturma 
> * SQL Server veritabanınızı boş Azure SQL veritabanına aktarmak için [Veri Geçiş Yardımcısı](https://www.microsoft.com/download/details.aspx?id=53595)'nı (DMA) kullanma 
> * Veritabanı özelliklerini değiştirmek için [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)'yu (SSMS) kullanma.

Veritabanınızın güvenliğini sağlamayı öğrenmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Azure SQL Veritabanınızın güvenliğini sağlayın](sql-database-security-tutorial.md).


