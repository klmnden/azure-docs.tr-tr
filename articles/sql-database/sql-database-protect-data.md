---
title: "Azure SQL Veritabanı Veri Koruma Özelliklerine Genel Bakış | Microsoft Belgeleri"
description: "Bir Azure SQL veritabanındaki verileri koruma hakkında bilgi edinin."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: secure and protect
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-management
ms.date: 12/21/2016
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: f4712d70c0323e607ddcc021809f8097a621730d
ms.openlocfilehash: 67b32b224d86e8d0ed06d8248c3a2c5f5569c5cb


---
# <a name="protecting-data-within-your-sql-database"></a>SQL veritabanınızdaki verileri koruma

SQL Veritabanı, verilerinizi korumak için şifreleme kullanır.  

## <a name="overview"></a>Genel Bakış

SQL Veritabanı, hareket halindeki verileriniz için [Aktarım Katmanı Güvenliği](https://support.microsoft.com/en-us/kb/3135244), bekleyen veriler için [Saydam Veri Şifrelemesi](http://go.microsoft.com/fwlink/?LinkId=526242) ve kullanılan veriler için [Always Encrypted](https://msdn.microsoft.com/library/mt163865.aspx) kullanarak verilerinizi şifreler ve güvenliğini sağlar. Bu SQL Veritabanı koruma özellikleri hakkında ayrıntılı bilgi için bkz. [Veri koruma ve güvenlik](sql-database-protect-data.md).

Verilerinizi şifrelemek için kullanabileceğiniz diğer yöntemler şunlardır:

* [Hücre düzeyinde şifreleme](https://msdn.microsoft.com/library/ms179331.aspx): Belirli sütunları hatta veri hücrelerini farklı şifreleme anahtarlarıyla şifrelemenizi sağlar.
* Donanım Güvenlik Modülüne veya şifreleme anahtarı hiyerarşinizi bir noktadan yönetmeye ihtiyacınız varsa [Azure VM'de SQL Server ile Azure Anahtar Kasası](http://blogs.technet.com/b/kv/archive/2015/01/12/using-the-key-vault-for-sql-server-encryption.aspx)'nı kullanın.


SQL Veritabanı, denetim ve tehdit algılama özellikleriyle verilerinizi korur. 

### <a name="auditing"></a>Denetim
SQL Veritabanı Denetimi, veritabanı etkinliklerini izler ve veritabanı olaylarını Azure Depolama hesabınızdaki bir denetim günlüğüne kaydederek düzenlemelere uyumluluğu sağlamanıza yardımcı olur. Denetim sayesinde devam eden veritabanı etkinliklerini anlayabilir, ayrıca olası tehditleri veya kötüye kullanım ve güvenlik ihlali şüphelerini soruşturmak için geçmiş etkinlikleri analiz edebilir ve araştırabilirsiniz. Daha fazla bilgi için bkz. [SQL Veritabanı Denetimini kullanmaya başlayın](sql-database-auditing-get-started.md).  

### <a name="threat-detection"></a>Tehdit algılama
Tehdit Algılama, Azure SQL Veritabanı hizmetine yerleşik bir güvenlik katmanı ekleyerek denetim özelliklerini tamamlar. Sürekli çalışan bu sistem anormal veritabanı etkinliklerini öğrenir, profilini çıkarır ve bunları tespit eder. Şüpheli etkinlikler, potansiyel açıklar, SQL ekleme saldırıları ve anormal veritabanı erişim modellerinin tespit edilmesi halinde uyarı alırsınız. Verilen bilgilendirici ve eyleme dönüştürülebilen talimatları uygulayarak uyarıları yanıtlayabilirsiniz. Daha fazla bilgi için bkz. [SQL Veritabanı Tehdit Algılamayı kullanmaya başlayın](sql-database-threat-detection-get-started.md).  

## <a name="connection-security"></a>Bağlantı güvenliği
Bağlantı Güvenliği, veritabanı bağlantılarını güvenlik duvarı kuralları ve bağlantı şifrelemesi kullanarak kısıtlamayı ve bu bağlantıların güvenliğini sağlamayı kapsar.

Güvenlik duvarı kuralları hem sunucu hem de veritabanı tarafından açık olarak beyaz listeye eklenmeyen IP adreslerinden gelen bağlantıları reddetmek için kullanılır. Uygulamanızın veya istemci bilgisayarınızın genel IP adresinin yeni bir veritabanına bağlanma girişiminde bulunmasına izin vermek için öncelikle Klasik Azure Portalı, REST API veya PowerShell kullanarak sunucu düzeyi güvenlik duvarı kuralı oluşturmanız gerekir. En iyi uygulama olarak sunucu güvenlik duvarınızdan geçmesine izin verilen IP adresi aralıklarını mümkün olduğunca sınırlı tutmanız gerekir. Daha fazla bilgi için bkz. [Azure SQL Database Güvenlik Duvarı](https://msdn.microsoft.com/library/ee621782).

Veritabanına gelen ve veritabanından giden veriler "taşıma durumunda" olduğu Azure SQL Veritabanına yapılan tüm bağlantılar için şifreleme (SSL/TLS) gerekir. Uygulamanızın bağlantı dizesinde bağlantıyı şifrelemek ve sunucu sertifikasına *güvenmemek* için parametreleri belirtmeniz gerekir (bağlantı dizesini Klasik Azure Portalından kopyalarsanız bu sizin yerinize gerçekleştirilir. Aksi halde bağlantı sunucunun kimliğini doğrulamaz ve "bağlantıyı izinsiz izleme" saldırılarına açık olacaktır. Örneğin ADO.NET sürücüsü için bu bağlantı dizesi parametreleri **Encrypt=True** ve **TrustServerCertificate=False** olacaktır. Daha fazla bilgi için bkz. [Azure SQL Veritabanı Bağlantısı Şifreleme ve Sertifika Doğrulama](https://msdn.microsoft.com/library/azure/ff394108#encryption).

## <a name="authentication"></a>Kimlik Doğrulaması
Kimlik doğrulaması, veritabanına bağlanırken kimliğinizi nasıl kanıtlayacağınızı belirtir. SQL Veritabanı iki kimlik doğrulaması türünü destekler:

* **SQL Kimlik Doğrulaması**: Kullanıcı adı ve parola kullanır
* **Azure Active Directory Kimlik Doğrulaması**: Azure Active Directory tarafından yönetilen kimlikleri kullanır, yönetilen ve tümleşik etki alanları ile kullanılabilir

Veritabanınıza ait mantıksal sunucuyu oluşturduktan sonra kullanıcı adı ve parola belirleyerek "sunucu yöneticisi" oturum açma bilgisi oluşturdunuz. Bu kimlik bilgilerini kullanarak veritabanı sahibi veya "dbo" olarak sunucudaki tüm veritabanları için kimlik doğrulamasından geçebilirsiniz. Azure Active Directory Kimlik Doğrulamasını kullanmak istiyorsanız, Azure AD kullanıcılarını ve gruplarını yönetme izni olan "Azure AD yöneticisi" adlı başka bir sunucu yöneticisi daha oluşturmanız gerekir. Bu yönetici normal bir sunucu yöneticisinin gerçekleştirebileceği tüm işlemleri yapabilir. Azure Active Directory kimlik doğrulamasını etkinleştirme amacıyla Azure AD yöneticisi oluşturma adımları için bkz. [Azure Active Directory Kimlik Doğrulaması kullanarak SQL Veritabanına Bağlanma](sql-database-aad-authentication.md).

En iyi uygulama olarak, uygulamanızın kimlik doğrulaması için farklı bir hesap kullanması gerekir. Bu şekilde uygulamaya verilen izinleri sınırlayabilir ve uygulama kodunuzun SQL ekleme saldırısına açık olması durumunda kötü niyetli etkinlik risklerini azaltabilirsiniz. Önerilen yaklaşım, uygulamanızın doğrudan veritabanında kimlik doğrulamasından geçmesi için [bağımsız veritabanı kullanıcısı](https://msdn.microsoft.com/library/ff929188) oluşturmaktır. Kullanıcı veritabanınıza sunucu yöneticisi oturum açma hesabıyla bağlıyken aşağıdaki T-SQL komutunu çalıştırarak SQL Kimlik Doğrulaması kullanan bağımsız veritabanı kullanıcısı oluşturabilirsiniz:

```
CREATE USER ApplicationUser WITH PASSWORD = 'strong_password'; -- SQL Authentication
```

Bir Azure AD yöneticisi oluşturduysanız, kullanıcı veritabanınıza Azure AD yöneticisi olarak bağlıyken aşağıdaki T-SQL komutunu çalıştırarak Azure Active Directory Kimlik Doğrulaması kullanan bağımsız veritabanı kullanıcısı oluşturabilirsiniz:

```
CREATE USER [Azure_AD_principal_name | Azure_AD_group_display_name] FROM EXTERNAL PROVIDER; -- Azure Active Directory Authentication
```

Her iki durumda da veritabanına bağlanmak için uygulamanızın bağlantı dizesinde sunucu yöneticisi oturum açma bilgileri yerine bu kullanıcı kimlik bilgileri belirtilmelidir.

SQL Veritabanında kimlik doğrulaması gerçekleştirme hakkında daha fazla bilgi edinmek için bkz. [Azure SQL Veritabanında Veritabanlarını ve Oturum Açma İşlemlerini Yönetme](sql-database-manage-logins.md).

## <a name="authorization"></a>Yetkilendirme
Yetkilendirme, bir Azure SQL Veritabanında gerçekleştirebileceklerinizi tanımlar ve bu durum, kullanıcı hesabınızın rol üyelikleri ve izinleri ile denetlenir. En iyi uygulama olarak, kullanıcılarınıza gerekli olan en düşük ayrıcalıkları tanımanız gerekir. Azure SQL Veritabanı, T-SQL rolleri ile bunu yönetmeyi kolaylaştırır:

```
ALTER ROLE db_datareader ADD MEMBER ApplicationUser; -- allows ApplicationUser to read data
ALTER ROLE db_datawriter ADD MEMBER ApplicationUser; -- allows ApplicationUser to write data
```

Bağlantı kurmak için kullandığınız sunucu yöneticisi hesabı, veritabanında tüm işlemleri gerçekleştirme yetkisi olan db_owner rolünün üyesidir. Bu hesabı şema yükseltmeleri ve diğer yönetimsel işlemlerde kullanmak üzere saklayın. Uygulamanızdan veritabanına, uygulamanızın ihtiyaç duyduğu en düşük ayrıcalıklarla bağlanmak için daha sınırlı izinlere sahip olan "ApplicationUser" hesabını kullanın.

Bir kullanıcının Azure SQL Veritabanında yapabileceklerini sınırlamak için kullanabileceğiniz başka yöntemler de mevcuttur:

* db_datareader ve db_datawriter haricindeki [Veritabanı Rollerini](https://msdn.microsoft.com/library/ms189121) kullanarak daha güçlü uygulama kullanıcı hesapları veya daha zayıf yönetim hesapları oluşturabilirsiniz.
* Ayrıntılı [İzinleri](https://msdn.microsoft.com/library/ms191291) kullanarak veritabanındaki her bir sütun, tablo, görünüm, yordam ve diğer nesneler için gerçekleştirebileceğiniz işlemleri denetleyebilirsiniz.
* [Kimliğe Bürünme](https://msdn.microsoft.com/library/vstudio/bb669087) ve [modül imzalama](https://msdn.microsoft.com/library/bb669102) ile izinler geçici olarak ve güvenli bir şekilde artırılabilir.
* [Satır Düzeyinde Güvenlik](https://msdn.microsoft.com/library/dn765131) ile bir kullanıcının erişebileceği satırlar sınırlandırılabilir.
* [Veri Maskeleme](sql-database-dynamic-data-masking-get-started.md) ile hassas verilerin kapsamı sınırlandırılabilir.
* [Saklı yordamlar](https://msdn.microsoft.com/library/ms190782) ile veritabanında gerçekleştirilebilecek eylemler sınırlandırılabilir.

Veritabanlarını ve mantıksal sunucuları Klasik Azure Portalından veya Azure Resource Manager API kullanarak yönetmek, portalınızın kullanıcı hesabına atanan roller tarafından denetlenir. Bu konu hakkında daha fazla bilgi için bkz. [Azure portalında rol tabanlı erişim denetimi](../active-directory/role-based-access-control-configure.md).

## <a name="encryption"></a>Şifreleme
Azure SQL Veritabanı, [Saydam Veri Şifrelemesi](http://go.microsoft.com/fwlink/?LinkId=526242) ile "beklemede" olan veya veritabanı dosyalarında ve yedeklerinde saklanan verileri şifreleyerek verilerinizin korunmasına yardımcı olabilir. Veritabanınızı şifrelemek için veritabanı sahibi olarak bağlanın ve şu komutu çalıştırın:

```
ALTER DATABASE [AdventureWorks] SET ENCRYPTION ON;
```

Gizli verilerinizi şifrelemek için kullanabileceğiniz diğer yöntemler şunlardır:

* [Hücre düzeyinde şifreleme](https://msdn.microsoft.com/library/ms179331.aspx): Belirli sütunları hatta veri hücrelerini farklı şifreleme anahtarlarıyla şifrelemenizi sağlar.
* Donanım Güvenlik Modülüne veya şifreleme anahtarı hiyerarşinizi bir noktadan yönetmeye ihtiyacınız varsa [Azure VM'de SQL Server ile Azure Anahtar Kasası](http://blogs.technet.com/b/kv/archive/2015/01/12/using-the-key-vault-for-sql-server-encryption.aspx)'nı kullanın.
* [Her Zaman Şifreli](https://msdn.microsoft.com/library/mt163865.aspx) (önizleme): Şifrelemeyi uygulamalar için saydam hale getirir ve istemcilerin hassas verileri istemci uygulamalarının içinde ve şifreleme anahtarlarını SQL Veritabanıyla paylaşmadan şifrelemesini sağlar.

## <a name="auditing"></a>Denetim
Veritabanı olaylarını denetleme ve izleme, düzenlemelere uygun hareket etmenize ve şüpheli etkinlikleri tanımlamanıza yardımcı olabilir. SQL Veritabanı Denetimi, veritabanınızdaki olayları Azure Depolama hesabınızdaki bir denetim günlüğüne kaydetmenizi sağlar. SQL Veritabanı Denetimi ayrıca Microsoft Power BI ile tümleştirilerek ayrıntılı raporlar ve analizler oluşturulmasını sağlar. Daha fazla bilgi için bkz. [SQL Veritabanı Denetimini kullanmaya başlayın](sql-database-auditing-get-started.md).

SQL Veritabanı Tehdit Algılama, Denetimin üzerine ek bir güvenlik katmanı sunar. Bu özellik, anormal etkinliklerde güvenlik uyarıları oluşturarak tehditlere anında müdahale etmenizi sağlar. Daha fazla bilgi için bkz. [SQL Veritabanı Tehdit Algılamayı kullanmaya başlayın](sql-database-threat-detection-get-started.md).  

## <a name="compliance"></a>Uyumluluk
Yukarıda yer verilen ve uygulamanızın çeşitli güvenlik gereksinimlerini karşılamasına yardımcı olabilecek özelliklere ve işlevlere ek olarak Azure SQL Veritabanı düzenli denetimlere de katılmaktadır ve bir dizi uyumluluk standardı için belgelendirilmiştir. Daha fazla bilgi için günceli [SQL Veritabanı uyumluluk sertifikası](https://azure.microsoft.com/support/trust-center/services/) listesine ulaşabileceğiniz [Microsoft Azure Güven Merkezi](https://azure.microsoft.com/support/trust-center/) sayfasına bakın.


## <a name="next-steps"></a>Sonraki adımlar

- SQL Veritabanı güvenlik özelliklerine genel bakış için bkz. [SQL güvenliğine genel bakış](sql-database-security-overview.md).
- SQL Veritabanındaki erişim denetimi özelliklerinin kullanımı hakkında ayrıntılı bilgi için bkz. [Erişim denetimi](sql-database-control-access.md).
- Öngörülebilir izleme hakkında daha fazla bilgi için bkz. [SQL Veritabanı Denetimini kullanmaya başlama](sql-database-auditing-get-started.md) ve [SQL Veritabanı Tehdit Algılamayı kullanmaya başlama](sql-database-threat-detection-get-started.md).



<!--HONumber=Dec16_HO4-->


