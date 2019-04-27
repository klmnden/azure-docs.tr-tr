---
title: Azure veritabanı güvenliğine genel bakış | Microsoft Docs
description: Bu makalede, Azure veritabanı genel bir bakış güvenlik özellikleri sağlar.
services: security
documentationcenter: na
author: UnifyCloud
manager: barbkess
editor: TomSh
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/30/2018
ms.author: TomSh
ms.openlocfilehash: 7e0e93c82279ec1a4fbecbbf27c7a1866286b2f8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60587649"
---
# <a name="azure-database-security-overview"></a>Azure veritabanı güvenliğine genel bakış

Güvenlik veritabanlarını yönetmek için en önemli olduğu ve her zaman Azure SQL veritabanı için bir öncelik olmuştur. Azure SQL veritabanı güvenlik duvarı kurallarını ve bağlantı şifrelemesi ile bağlantı güvenliği destekler. Bu, kullanıcı adı ve parola ile kimlik doğrulaması ve Azure Active Directory tarafından yönetilen kimlikleri kullanan Azure Active Directory (Azure AD) kimlik doğrulamasını destekler. Yetkilendirme, rol tabanlı erişim denetimi kullanır.

Azure SQL veritabanı, gerçek zamanlı şifreleme ve şifre çözme, veritabanları, ilişkili yedeklemeler ve işlem günlük dosyaları, bekleyen uygulamada değişiklik gerektirmeden gerçekleştirerek şifrelemesini destekler.

Microsoft Kurumsal verileri şifrelemek için ek yol sağlar:

-   Hücre düzeyinde şifreleme, ya da belirli sütunları hatta veri hücrelerini farklı şifreleme anahtarlarıyla şifrelemenizi kullanılabilir.
-   Bir donanım güvenlik modülüne veya şifreleme anahtar hiyerarşinin Merkezi Yönetim gerekiyorsa, Azure Key Vault, SQL Server ile bir Azure sanal makinesinde (VM) kullanarak göz önünde bulundurun.
-   Her zaman şifreli (şu anda önizlemede) şifreleme saydam uygulamaları yapar. Ayrıca, şifreleme anahtarlarını SQL veritabanıyla paylaşmadan istemci uygulamaları içindeki hassas verileri şifrelemek istemcilerin da sağlar.

Azure SQL veritabanı denetimi, kuruluşların Azure Depolama'da bir denetim olayları günlüğe kaydını etkinleştirir. SQL Veritabanı Denetimi ayrıca Microsoft Power BI ile tümleştirilerek ayrıntılı raporlar ve analizler oluşturulmasını sağlar.

Azure SQL veritabanları sıkı bir şekilde güvenli en yasal karşılamak için veya güvenlik gereksinimleri, HIPAA, ISO 27001/27002 ve PCI DSS düzey 1 gibi. Güvenlik uyumluluk sertifikaları güncel bir listesi kullanılabilir [Microsoft Azure Trust Center site](https://azure.microsoft.com/support/trust-center/services/).

Bu makalede, Microsoft Azure SQL veritabanları için yapılandırılmış, tablo ve ilişkisel veri güvenliği temelleri gösterilmektedir. Bu makalede özellikle veri koruma, erişim denetimi ve öngörülebilir izleme kaynakları için başlangıç bilgileri sunulmaktadır.

## <a name="protection-of-data"></a>Veri koruma

SQL veritabanı şifreleme sağlayarak verilerinizin korunmasına yardımcı olur:

- Hareket halindeki veriler için [Aktarım Katmanı Güvenliği (TLS)](https://support.microsoft.com/kb/3135244).
- Veriler, kullanılmadıkları için [saydam veri şifrelemesi](https://go.microsoft.com/fwlink/?LinkId=526242).
- Aracılığıyla veri [Always Encrypted](https://msdn.microsoft.com/library/mt163865.aspx).

Verilerinizi şifrelemek için kullanabileceğiniz diğer yöntemler şunlardır:

-   [Hücre düzeyinde şifreleme](https://msdn.microsoft.com/library/ms179331.aspx): Belirli sütunları hatta veri hücrelerini farklı şifreleme anahtarlarıyla şifrelemenizi sağlar.
-   [Azure VM'de SQL Server ile Azure anahtar kasası](https://blogs.technet.com/b/kv/archive/2015/01/12/using-the-key-vault-for-sql-server-encryption.aspx), donanım güvenlik modülüne veya şifreleme anahtar hiyerarşinin Merkezi Yönetim gerekiyorsa.

### <a name="encryption-in-motion"></a>Hareket halindeki şifreleme

Tüm istemci/sunucu uygulamaları için ortak bir sorunu ortak ve özel ağlar üzerinden veri taşıyan gizlilik gereksinimi aynıdır. Bir ağ üzerinden taşınan veriler şifrelenmez, yakalanan ve olması yetkisiz kullanıcılar tarafından çalınırsa, şansı olur. Veritabanı Hizmetleri ile ilgilenirken veritabanı istemci ve sunucu arasında verilerin şifrelendiğinden emin emin olun. Ayrıca birbirleriyle ve orta katman uygulamaları ile iletişim kurmak veritabanı sunucuları arasındaki verilerin şifrelendiğinden emin olun.

Ağ yönetirken karşılaşılan sorunlardan biri güvenilmeyen bir ağ üzerinden uygulamalar arasında gönderilen verilerin güvenli hale getirir. Kullanabileceğiniz [TLS/SSL](https://docs.microsoft.com/windows-server/security/tls/transport-layer-security-protocol) sunucuların ve istemcilerin kimliğini doğrulamak ve kimliği doğrulanmış taraflar arasında iletileri şifrelemek için kullanın.

Kimlik doğrulama işleminde, bir TLS/SSL istemcisi bir TLS/SSL sunucusuna ileti gönderir. Sunucu, sunucunun kendi kimliğini doğrulamak için gerek duyduğu bilgilerle yanıt verir. İstemci ve sunucu, oturum anahtarlarının ek değişimini gerçekleştirir ve kimlik doğrulama iletişimi sona erer. Kimlik doğrulaması tamamlandığında, SSL korumalı iletişim sunucu ve istemci arasında kimlik doğrulama işlemi sırasında oluşturulmuş simetrik şifreleme anahtarları ile başlar.

Veritabanına gelen ve giden veri "taşıma durumunda" olduğu sürece tüm bağlantılar için Azure SQL veritabanı (TLS/SSL) şifrelemesi, her zaman gerekir. SQL veritabanı sunucuları ve istemcileri kimlik doğrulaması ve kimliği doğrulanmış taraflar arasında iletileri şifrelemek için kullanmak için TLS/SSL kullanır. 

Uygulamanızın bağlantı dizesinde bağlantıyı şifrelemek ve sunucu sertifikasına güvenmeyecek için parametreleri belirtmeniz gerekir. (Bağlantı dizenizi Azure portalından kopyalarsanız bu sizin yerinize gerçekleştirilir.) Aksi halde bağlantı sunucunun kimliğini doğrulamaz ve "adam-in--middle" saldırılarına açık olacaktır. ADO.NET sürücüsü için örneğin, bu bağlantı dizesi parametreleri olan `Encrypt=True` ve `TrustServerCertificate=False`.

### <a name="encryption-at-rest"></a>Bekleme sırasında şifreleme

Veritabanı güvenliğini sağlamaya yardımcı olmak için çeşitli önlemler alabilir. Örneğin, güvenli bir sistemi tasarlamak, gizli varlıkları şifrelemek ve veritabanı sunucular etrafında bir güvenlik duvarı oluşturun. Ancak fiziksel medyaya (örneğin, sürücüler veya yedekleme bantlarının) burada çalınırsa bir senaryoda, kötü amaçlı bir taraf yalnızca geri yükleyebilir veya veritabanını ve veri göz atın.

Bir veritabanındaki hassas verileri şifrelemek ve bir sertifika ile verileri şifrelemek için kullanılan anahtarları koruma çözümüdür. Bu çözüm herkes anahtarları olmadan veri kullanmasını engeller. ancak bu tür bir koruma hazırlıklı olmak gerekir.

Bu sorun, SQL Server ve SQL veritabanı desteği çözmek için [saydam veri şifrelemesi](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql?view=azuresqldb-current&viewFallbackFrom=sql-server-2017). Saydam veri şifrelemesi, bekleyen verileri şifreleme olarak bilinen, SQL Server ve SQL veritabanı veri dosyaları şifreler.

Saydam veri şifrelemesi, kötü amaçlı etkinlik tehditlerine karşı koruma yardımcı olur. Bu gerçek zamanlı şifreleme ve şifre çözme veritabanını, ilişkili yedeklemeler ve işlem günlük dosyaları bekleme sırasında uygulamada değişiklik gerektirmeden gerçekleştirir.  

Saydam veri şifrelemesi tüm veritabanı depolama, veritabanı şifreleme anahtarı olarak adlandırılan bir simetrik anahtarı'nı kullanarak şifreler. SQL veritabanı'nda veritabanı şifreleme anahtarını bir yerleşik bir sunucu sertifikası tarafından korunur. Yerleşik bir sunucu sertifikası, her SQL veritabanı sunucusu için benzersizdir.

Bir veritabanı bir coğrafi-DR ilişkisi içinde ise, her sunucuda farklı bir anahtar tarafından korunur. İki veritabanı aynı sunucuya bağlandıysanız yerleşik aynı sertifikayı paylaşırlar. Microsoft, bu sertifikalar otomatik olarak en az 90 günde döndürür. 

Daha fazla bilgi için [saydam veri şifrelemesi](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-tde).

### <a name="encryption-in-use-client"></a>Şifrelemesinin kullanılmadığını (istemci)

Çoğu veri ihlallerini kredi kartı numaraları veya kişisel olarak tanımlanabilir bilgiler gibi kritik verilerin çalınması içerir. Veritabanları, hassas bilgi hazinesi troves olabilir. Müşterilerin (Ulusal Kimlik numaraları gibi) kişisel verileriniz, rekabetçi olan gizli bilgileri ve fikri mülkiyet içerebilirler. Kaybolan veya çalınan veri, özellikle müşteri verileri, marka zarar, rekabet açısından size zarar ve ciddi cezaları--bile Davalar neden olabilir.

![Kilit ve anahtar gösterilen her zaman şifreli özelliği](./media/azure-databse-security-overview/azure-database-fig1.png)

[Her zaman şifreli](https://msdn.microsoft.com/library/mt163865.aspx) Azure SQL veritabanı veya SQL Server veritabanlarında depolanan hassas verileri korumak için tasarlanan bir özelliktir. Her zaman şifreli, istemci uygulamaları içindeki hassas verileri şifrelemek ve hiçbir zaman şifreleme anahtarları (SQL veritabanı veya SQL Server) veritabanı altyapısı açığa etmesine olanak tanır.

Her zaman şifreli verileri (ve görüntüleyebileceğini) kişiler ve kişiler yönetenler (ama hiçbir erişimi olması gereken) arasında bir ayrım sağlar. Şirket içi Veritabanı yöneticileri, bulut veritabanı işleçleri veya diğer yüksek ayrıcalıklı ancak yetkisiz kullanıcıların şifrelenmiş verilere erişemez olun yardımcı olur.

Ayrıca, Always Encrypted şifreleme saydam uygulamaları yapar. Bu otomatik olarak şifreler ve istemci uygulamasındaki hassas verilerin şifresini çözmek için Always Encrypted özellikli bir sürücü istemci bilgisayara yüklenir. Sürücü, veritabanı motoruna verileri geçirmeden önce hassas sütunlardaki verileri şifreler. Böylece uygulama semantiği korunur sürücü sorgular otomatik olarak yeniden yazar. Benzer şekilde, sürücü şeffaf bir şekilde sorgu sonuçlarında yer alan, şifrelenmiş veritabanına sütunlarda depolanan, verilerin şifresini çözer.

## <a name="access-control"></a>Erişim denetimi

Güvenliği sağlamak için kullanarak SQL veritabanı erişimi denetler:

- IP adresine göre bağlantıyı sınırlayan güvenlik duvarı kuralları.
- Kullanıcıların kimliğini kanıtlamasını gerektiren kimlik doğrulama mekanizmaları.
- Kullanıcıları belirli eylemler ve verilerle sınırlayan yetkilendirme sistemleriyle.

### <a name="database-access"></a>Veritabanı erişimi

Veri koruma, verilerinize erişimi denetleme ile başlar. Verilerinizi barındıran veri merkezinde fiziksel erişimi yönetir. Ağ katmanında güvenliği yönetmek için bir güvenlik duvarını yapılandırabilirsiniz. Ayrıca oturum açma kimlik doğrulaması için yapılandırarak ve izinleri için sunucu ve veritabanı rolleri tanımlama erişimi denetler.

#### <a name="firewall-and-firewall-rules"></a>Güvenlik duvarı ve güvenlik duvarı kuralları

[Azure SQL veritabanı](https://azure.microsoft.com/services/sql-database/) Azure ve diğer internet tabanlı uygulamalar için ilişkisel veritabanı hizmetidir. Güvenlik duvarları, verilerinizin korunmasına yardımcı olmak üzere, hangi bilgisayarların izinli olduğunu belirtmenize kadar veritabanı sunucunuza tüm erişimi engeller. Güvenlik duvarı, her bir isteğin kaynak IP adresine göre veritabanlarına erişim verir. Daha fazla bilgi edinmek için bkz. [Azure SQL Veritabanı güvenlik duvarı kurallarına genel bakış](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure).

Azure SQL veritabanı hizmeti yalnızca 1433 numaralı TCP bağlantı kullanılabilir. Bilgisayarınızdan bir SQL veritabanına erişmek için istemci bilgisayar Güvenlik Duvarı'nın 1433 numaralı TCP bağlantı noktasını giden TCP iletişimine izin verdiğinden emin olun. Diğer uygulamalar için gelen bağlantı gerekli değilse 1433 numaralı TCP bağlantı noktasını engelleyebilirsiniz.

#### <a name="authentication"></a>Kimlik Doğrulaması

Kimlik doğrulaması, veritabanına bağlanırken kimliğinizi nasıl kanıtlayacağınızı belirtir. SQL Veritabanı iki kimlik doğrulaması türünü destekler:

-   **SQL Server kimlik doğrulamasını**: Mantıksal bir SQL örneği oluşturulduğunda SQL Veritabanı Abone Hesabı adıyla tek bir oturum açma hesabı oluşturulur. Bu hesabı kullanarak bağlanır [SQL Server kimlik doğrulamasını](https://docs.microsoft.com/azure/sql-database/sql-database-security-overview) (kullanıcı adı ve parola). Bu hesap mantıksal sunucu örneğinde ve bu örneğe ekli tüm kullanıcı hesaplarında yöneticidir. Abone hesabının izinleri kısıtlanamaz. Bu hesaplardan yalnızca biri mevcut olabilir.
-   **Azure Active Directory kimlik doğrulaması**: [Azure AD kimlik doğrulaması](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) Azure SQL veritabanı ve Azure SQL veri ambarı için Azure AD'de kimlik kullanarak bağlanma bir mekanizmadır. Veritabanı kullanıcıları kimliklerini merkezi olarak yönetmek için kullanabilirsiniz.

![SQL veritabanı ile Azure AD kimlik doğrulaması](./media/azure-databse-security-overview/azure-database-fig2.png)

 Azure AD kimlik doğrulamasının avantajları şunlardır:
  - Bu SQL Server kimlik doğrulaması için bir alternatif sağlar.
  - Veritabanı sunucuları arasında kullanıcı kimliklerini çoğalan durdurmaya yardımcı olur ve tek bir yerde parola dönüşü sağlar.
  - Dış (Azure AD) grupları kullanarak veritabanı izinlerini yönetebilir.
  - Tümleşik Windows kimlik doğrulaması ve diğer destekleyen Azure AD Authentication'ı etkinleştirerek, parolaları ortadan kaldırabilir.

#### <a name="authorization"></a>Yetkilendirme

[Yetkilendirme](https://docs.microsoft.com/azure/sql-database/sql-database-manage-logins) bir kullanıcının bir Azure SQL veritabanında yapabileceklerini için ifade eder. Kullanıcı hesabınızın veritabanı tarafından denetlenir [rol üyeliklerini](https://msdn.microsoft.com/library/ms189121) ve [nesne düzeyi izinleri](https://msdn.microsoft.com/library/ms191291.aspx). Yetkilendirme bir sorumluya erişmek güvenli kılınabilir kaynakları ve işlemleri için bu kaynakları izin belirleme işlemidir.

### <a name="application-access"></a>Uygulama erişimi

#### <a name="dynamic-data-masking"></a>Dinamik veri maskeleme

Temsilcisiyle bir çağrı merkezi, sosyal güvenlik numarası veya kredi kartı numarası birkaç basamak çağıranlar tanımlayabilirsiniz. Ancak, bu veri öğeleri temsilcisiyle tamamen sunulmamalıdır.

Bir sosyal güvenlik numarası veya herhangi bir sorgunun sonuç kümesinde kredi kartı numarasının son dört rakamı dışındaki tüm maskeleri bir maskeleme kuralı tanımlayabilirsiniz.

![SQL veritabanı ve iş uygulamalar arasında gönderilen bir dizi maskeleme](./media/azure-databse-security-overview/azure-database-fig3.png)

Başka bir örnek olarak, kişisel bilgileri korumak için uygun veri maskesi tanımlanabilir. Bir geliştirici sonra uyumluluk düzenlemelerini ihlal etmeden sorun giderme amacıyla üretim ortamlarında sorgulayabilirsiniz.

[SQL Veritabanı dinamik veri maskeleme](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started), hassas verilerin görünürlüğünü ayrıcalık sahibi olmayan kullanıcılardan gizleyerek sınırlar. Dinamik veri maskeleme, Azure SQL veritabanının V12 sürümü desteklenir.

[Dinamik veri maskeleme](https://docs.microsoft.com/sql/relational-databases/security/dynamic-data-masking) önlemeye yardımcı olur yetkisiz erişim için hassas verileri uygulama katmanı üzerinde en az etki ile ortaya çıkarmak için hassas verilerin ne kadar atamak sağlayarak. Bu özellik, hassas verileri belirlenen veritabanı alanlarına yapılan sorgunun sonuç kümesinde gizleyen ancak veritabanındaki verileri değiştirmeyen ilke tabanlı bir güvenlik özelliğidir.

> [!Note]
> Dinamik veri maskeleme, Azure veritabanı yöneticisi, sunucu yöneticisi veya güvenlik yöneticisi rolü tarafından yapılandırılabilir.

#### <a name="row-level-security"></a>Satır Düzeyinde Güvenlik

Çok kiracılı veritabanları için ortak bir güvenlik gereksinimi [satır düzeyi güvenlik](https://msdn.microsoft.com/library/dn765131.aspx). Sorguyu yürüten kullanıcının özelliklerine dayanan bir veritabanı tablosundaki satırlara erişimi denetlemek için bu özelliği kullanabilirsiniz. (Örneğin özellikleri grup üyeliği ve yürütme bağlamı sahiptir.)

![Bir istemci uygulaması aracılığıyla bir tablodaki satırlara erişimi bir kullanıcı izin vererek satır düzeyi güvenlik](./media/azure-databse-security-overview/azure-database-fig4.png)

Veritabanı katmanı bulunur yerine koy verileri başka bir uygulama katmanı kullanarak erişimi kısıtlama mantığı. Veri erişimini herhangi bir katmanı denenir her zaman veritabanı sistem erişim kısıtlamalarını uygular. Bu güvenlik sisteminizin daha güvenli ve sağlam güvenlik sisteminizin yüzey alanını azaltarak sağlar.

Satır düzeyi güvenlik koşulu tabanlı erişim denetimi sunar. Bunu göz önünde bulundurarak meta veriler ya da yönetici belirler diğer ölçütlere uygun şekilde kullanabileceğiniz esnek, merkezi bir değerlendirme özelliklerine sahiptir. Koşul, bir ölçüt olarak kullanıcı üzerinde kullanıcı özniteliklerine bağlı verilere uygun erişime sahip olup olmadığını belirlemek için kullanılır. Koşul tabanlı erişim denetimi kullanarak etiket tabanlı erişim denetimi uygulayabilirsiniz.

## <a name="proactive-monitoring"></a>Öngörülebilir izleme

SQL veritabanı, sağlayarak verilerinizin korunmasına yardımcı olur *denetim* ve *tehdit algılama* özellikleri.

### <a name="auditing"></a>Denetim

[Azure SQL veritabanı denetimi](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) olayları ve veritabanı içinde gerçekleşen değişiklikleri, anlayış kazanmak için yeteneğinizi artırır. Güncelleştirmeleri ve veri sorguları verilebilir.

SQL veritabanı denetimi veritabanı olaylarını ve Azure depolama hesabınızdaki bir denetim günlüğüne yazar izler. Denetim mevzuatla uyumluluk, veritabanı etkinliğini anlama ve tutarsızlıklar ve anomaliler iş sorunlara işaret ediyor olabilir veya şüpheli güvenlik ihlallerini kavramanıza yardımcı olabilir. Denetim sağlar ve uyumluluk standartlarını kıldığı kolaylaştıran ancak Uyumluluk garanti etmez.

SQL veritabanı için denetim kullanabilirsiniz:

- **Korumak** seçili olayların bir denetim kaydı. Denetlenecek veritabanı eylemlerin kategoriler tanımlayabilirsiniz.
- **Rapor** veritabanı etkinlikleri. Etkinlik ve olay Raporlama ile hızla gerçekleştirmek için önceden yapılandırılmış raporları ve panoyu kullanabilirsiniz.
- **Analiz** raporlar. Şüpheli olayları, olağan dışı etkinliği ve eğilimleri bulabilirsiniz.

Denetim iki yöntem vardır:

-   **BLOB denetimi**: Günlükler Azure Blob depolamaya yazılır. Daha yeni bir denetim yöntem budur. Daha yüksek performans sağlar, daha yüksek ayrıntı düzeyi nesne düzeyinde denetimi destekler ve daha uygun maliyetlidir.
-   **Tablo denetimi**: Günlükleri, Azure tablo depolama alanına yazılır.

### <a name="threat-detection"></a>Tehdit algılama

[Azure SQL veritabanı için Gelişmiş tehdit koruması](https://docs.microsoft.com/azure/sql-database/sql-advanced-threat-protection) olası güvenlik tehditlerine işaret eden şüpheli etkinlikleri algılar. Oluşunca, tehdit algılama veritabanındaki SQL ekleme gibi şüpheli olaylara yanıt vermek için kullanabilirsiniz. Bu uyarılar sağlar ve şüpheli olayların incelenmesi için Azure SQL veritabanı denetim kullanılmasına izin verir.

![Dışarıdaki bir saldırgan ve kötü amaçlı Insider ile SQL veritabanı ve bir web uygulaması için tehdit algılama](./media/azure-databse-security-overview/azure-database-fig5.jpg)

SQL Gelişmiş tehdit Koruması (ATP), veri bulma & Sınıflandırma, güvenlik açığı değerlendirmesi ve tehdit algılama gibi gelişmiş SQL güvenlik özellikleri sunar. 

- [Veri bulma & sınıflandırma](../sql-database/sql-database-data-discovery-and-classification.md)
- [Güvenlik Açığı Değerlendirmesi](../sql-database/sql-vulnerability-assessment.md)  
- [Tehdit algılama](../sql-database/sql-database-threat-detection.md)

[Gelişmiş tehdit koruması PostgreSQL için Azure veritabanı](../postgresql/concepts-data-access-and-security-threat-protection.md) yeni bir algılama ve anormal etkinliklerde güvenlik uyarıları sağlayarak oluşunca potansiyel tehditleri olanak tanıyan bir güvenlik katmanı sağlar. Kullanıcılar şüpheli veritabanı etkinlikleri ve olası güvenlik açıklarını yanı sıra anormal veritabanı erişim ve sorgu desenleri sırasında bir uyarı alırsınız. PostgreSQL için Azure veritabanı için Gelişmiş tehdit koruması ile Azure Güvenlik Merkezi uyarıları birleştirir. Uyarı türü şunlardır:

- Olağan dışı bir konumdan erişim
- Azure veri merkezinden erişim 
- Erişim 
- Zararlı olabilecek bir uygulamadan erişim 
- Deneme yanılma kimlik PostgreSQL için Azure veritabanı 

[Gelişmiş tehdit koruması MySQL için Azure veritabanı](../mysql/concepts-data-access-and-security-threat-protection.md) PostgreSQL gelişmiş koruma benzer koruma sağlar.  

## <a name="centralized-security-management"></a>Güvenlik Merkezi Yönetimi

[Azure Güvenlik Merkezi](https://azure.microsoft.com/documentation/services/security-center/), tehditleri önlemenize, algılamanıza ve yanıtlamanıza yardımcı olur. Bu, Azure aboneliklerinizde tümleşik güvenlik izleme ve ilke yönetimi sağlar. Aksi takdirde gözden kaçan geçebilir tehditleri algılamanıza yardımcı olur ve güvenlik çözümlerinin geniş ekosistemiyle çalışır.

[Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-sql-database) güvenliğini, sunucularınıza ve veritabanlarınıza yönelik görünürlük sağlayarak SQL veritabanındaki verileri korumaya yardımcı olur. Güvenlik Merkezi ile şunları yapabilirsiniz:

- SQL veritabanı şifreleme ve denetim için ilkeler tanımlarsınız.
- SQL veritabanı kaynaklarının tüm aboneliklerinizde izleyin.
- Hızlı bir şekilde belirleyin ve güvenlik sorunlarını düzeltmesine.
- Uyarılardan tümleştirme [Azure SQL veritabanı tehdit algılama](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection).

Güvenlik Merkezi, rol tabanlı erişimi destekler.

## <a name="sql-information-protection"></a>SQL Information Protection

[SQL bilgi koruması](../sql-database/sql-database-data-discovery-and-classification.md) otomatik olarak bulur ve hassas olabilecek verileri sınıflandırır, sınıflandırma öznitelikleri ile hassas verilere kalıcı olarak etiketleme için etiketleme bir mekanizma sağlar ve ayrıntılı bir panoya gösteren sağlar Veritabanı sınıflandırma durumu.  

Ayrıca, böylece hassas verileri ayıklamak sorguları açıkça denetlenebilir SQL sorguları duyarlılığına sonuç kümesi ve veri korumalı hesaplar. Azure SQL veritabanı veri bulma ve sınıflandırma SQL bilgi koruması hakkında daha fazla bilgi için bkz.

Yapılandırabileceğiniz [SQL bilgi koruması ilkeleri](../security-center/security-center-info-protection-policy.md) Azure Güvenlik Merkezi'nde.

## <a name="azure-marketplace"></a>Azure Market

Azure Marketi, yeni ve bağımsız yazılım satıcılarının (ISV'ler) çözümlerini dünyanın dört bir yanındaki Azure müşterilerine sunduğu çevrimiçi bir uygulama ve hizmet marketidir.
Azure marketi, Microsoft Azure iş ortağı ekosistemlerini daha iyi hizmet etmesi müşteriler ve ortaklar birleşik bir platformda birleştirir. Yapabilecekleriniz [arama çalıştırma](https://azuremarketplace.microsoft.com/marketplace/apps?search=Database%20Security&page=1) Azure Marketi'nde veritabanı güvenlik ürünleri görüntülemek için.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure SQL veritabanınızın güvenliğini sağlama](https://docs.microsoft.com/azure/sql-database/sql-database-security-tutorial)
- [Azure Güvenlik Merkezi ve Azure SQL veritabanı hizmeti](https://docs.microsoft.com/azure/security-center/security-center-sql-database)
- [SQL veritabanı tehdit algılama](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection)
- [SQL veritabanı performansı](https://docs.microsoft.com/azure/sql-database/sql-database-performance-tutorial)
