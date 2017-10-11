---
title: "Azure veritabanı güvenliğine genel bakış | Microsoft Docs"
description: "Bu makalede Azure veritabanı genel bir bakış güvenlik özellikleri sağlar."
services: security
documentationcenter: na
author: UnifyCloud
manager: swadhwa
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/19/2017
ms.author: TomSh
ms.openlocfilehash: 036ce3dce28e7951bb39754c4351661fae85f06c
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2017
---
# <a name="azure-database-security-overview"></a>Azure veritabanı güvenliğine genel bakış

Veritabanları yönetirken güvenlik güvenliğin çok önemli olduğu ve her zaman Azure SQL veritabanı için bir öncelik olmuştur. Azure SQL veritabanı güvenlik duvarı kuralları ve bağlantı şifreleme ile bağlantı güvenliği destekler. Kullanıcı adı ve parola ile kimlik doğrulaması ve Azure Active Directory tarafından yönetilen kimlikleri kullanan Azure Active Directory kimlik doğrulaması, destekler. Yetkilendirme rol tabanlı erişim denetimi kullanır.

Azure SQL veritabanı, gerçek zamanlı şifreleme ve şifre çözme veritabanları, ilişkili yedeklemelerinizi ve geri kalan işlem günlüğü dosyalarını uygulamasında yapılacak değişiklikler gerek kalmadan gerçekleştirerek Şifreleme destekler.

Microsoft Kurumsal verileri şifrelemek için ek yollar sağlar:

-   Belirli sütunları veya veri bile hücrelerinin farklı şifreleme anahtarlarını şifrelemek için hücre düzeyi şifreleme.
-   Bir donanım güvenlik modülü veya şifreleme anahtar hiyerarşinizdeki Merkezi Yönetim gerekiyorsa, Azure VM'deki SQL Server ile Azure anahtar kasası kullanmayı düşünün.
-   Her zaman şifreli (şu anda önizlemede) şifreleme uygulamaları saydam yapar ve şifreleme anahtarları ile SQL veritabanını paylaşan olmadan istemci uygulamalar içinde hassas verileri şifrelemek istemcilerinin sağlar.

Azure SQL veritabanı denetimi kuruluşların kayıt olayları denetim oturumuna Azure depolama sağlar. SQL Veritabanı Denetimi ayrıca Microsoft Power BI ile tümleştirilerek ayrıntılı raporlar ve analizler oluşturulmasını sağlar.

 SQL Azure veritabanının sıkı bir şekilde güvenli en yasal karşılamak için veya güvenlik gereksinimleri, HIPAA ve ISO 27001/27002 PCI DSS düzey 1, diğerleri de dahil olmak üzere. Geçerli güvenlik uyumluluk sertifikalar listesini kullanılabilir [Microsoft Azure Trust Center site](http://azure.microsoft.com/support/trust-center/services/).

Bu makalede, Microsoft Azure SQL veritabanları için yapılandırılmış, tablolu ve ilişkisel veri güvenliği temelleri size yol göstermektedir. Bu makalede özellikle veri koruma, erişim denetimi ve öngörülebilir izleme kaynakları için başlangıç bilgileri sunulmaktadır.

Azure veritabanı güvenliğine genel bakış makalede aşağıdaki alanlar üzerinde odaklanır:

-   Veri koruma
-   Erişim denetimi
-   Öngörülebilir izleme
-   Merkezi güvenlik yönetimi
-   Azure Market

## <a name="protect-data"></a>Veri koruma

SQL veritabanı hareket kullanarak veriler için şifreleme sağlayarak, verilerinizi korur [Aktarım Katmanı Güvenliği](https://support.microsoft.com/kb/3135244)kullanarak verileri bekletin için [saydam veri şifreleme](http://go.microsoft.com/fwlink/?LinkId=526242)ve kullanım kullanarak verilerin [ Her zaman şifreli](https://msdn.microsoft.com/library/mt163865.aspx).

Bu bölümde, biz hakkında konuşma:

-   Hareketli şifreleme
-   Bekleme sırasında şifreleme
-   Şifreleme kullanımda (istemci)

Verilerinizi şifrelemek için kullanabileceğiniz diğer yöntemler şunlardır:

-   [Hücre düzeyinde şifreleme](https://msdn.microsoft.com/library/ms179331.aspx): Belirli sütunları hatta veri hücrelerini farklı şifreleme anahtarlarıyla şifrelemenizi sağlar.
-   Donanım Güvenlik Modülüne veya şifreleme anahtarı hiyerarşinizi bir noktadan yönetmeye ihtiyacınız varsa [Azure VM'de SQL Server ile Azure Anahtar Kasası](http://blogs.technet.com/b/kv/archive/2015/01/12/using-the-key-vault-for-sql-server-encryption.aspx)'nı kullanın.

### <a name="encryption-in-motion"></a>Hareketli şifreleme

Tüm istemci/sunucu uygulamaları için ortak bir sorunu gizlilik gereksinimini aynıdır ortak ve özel ağlar üzerinden verileri taşır. Bir ağ üzerinden taşımaktan veriler şifrelenmez, yakalanan ve olması yetkisiz kullanıcılar tarafından çalınırsa, fırsat yok. Veritabanı Hizmetleri ile ilgilenirken, birbirleriyle ve orta katman uygulamalarıyla iletişim veritabanı sunucuları arasındaki ve veritabanı istemci ve sunucu arasında veri şifrelenir emin olmanız gerekir.

Ağ yönetirken sorunlardan biri güvenilmeyen bir ağ üzerinden uygulamalar arasında gönderilen verilerin güvenli hale getirir. Kullanabileceğiniz [TLS/SSL](https://docs.microsoft.com/windows-server/security/tls/transport-layer-security-protocol) sunucuları ve istemcileri kimlik doğrulaması ve kimliği doğrulanmış taraflar arasında iletileri şifrelemek için kullanın.

Kimlik doğrulama işleminde, bir TLS/SSL istemcisi bir TLS/SSL sunucusuna ileti gönderir ve sunucu sunucu kendi kimliğini doğrulamak gerek duyduğu bilgilerle yanıt verir. İstemci ve sunucu, oturum anahtarlarının ek değişimini gerçekleştirir ve kimlik doğrulama iletişimi sona erer. Kimlik doğrulaması tamamlandığında, SSL korumalı iletişim kimlik doğrulama işlemi sırasında oluşturulmuş simetrik şifreleme anahtarları kullanılarak istemci ile sunucu arasında başlayabilirsiniz.

Veritabanına gelen ve veritabanından giden veriler "taşıma durumunda" olduğu Azure SQL Veritabanına yapılan tüm bağlantılar için şifreleme (SSL/TLS) gerekir. SQL Azure sunucularının ve istemcilerin kimliğini doğrulamak ve kimliği doğrulanmış taraflar arasında iletileri şifrelemek için kullanmak için TLS/SSL kullanır. Uygulamanızın bağlantı dizesinde parametreleri bağlantıyı şifrelemek ve (Klasik Azure portalı dışında bağlantı dizenizi kopyalarsanız, bu sizin için yapılır) sunucu sertifikası, aksi takdirde bağlantı güvenmediğiniz değil doğrular belirtmelisiniz sunucunun kimliğini ve "man-in--middle" saldırılarına olacaktır. ADO.NET sürücüsü için örneği için bu bağlantı dizesi şifrele parametreleridir = True ve TrustServerCertificate = False.

### <a name="encryption-at-rest"></a>Bekleme sırasında şifreleme
Güvenli bir sistemde tasarlama, gizli varlıklar şifreleme ve veritabanı sunucuları geçici bir güvenlik duvarı oluşturma gibi veritabanı güvenliğinin sağlanmasına yardımcı olmak için birkaç önlemler alabilir. Ancak, fiziksel ortam (örneğin, sürücüleri veya yedekleme bantlarını) burada çalınırsa bir senaryoda, kötü amaçlı bir taraf yalnızca geri yükleme veya veritabanını ve veri göz atın.

Bir çözüm veritabanındaki hassas verileri şifrelemek için ve bir sertifika ile verileri şifrelemek için kullanılan anahtarları korur. Bu anahtarları olmayan birisi verileri kullanmasını engeller, ancak bu tür bir koruma planlanmalıdır.

Bu sorunu çözmek için SQL Server ve Azure SQL desteği [saydam veri şifreleme (TDE)](https://docs.microsoft.com/sql/relational-databases/securityrecryption/transparent-data-encryption-tde). TDE rest şifreleme verileri olarak bilinen SQL Server ve Azure SQL veritabanı veri dosyalarını şifreler.

Azure SQL veritabanında saydam veri şifreleme, gerçek zamanlı şifreleme ve şifre çözme veritabanının, ilişkili yedeklemelerinizi gerçekleştirerek kötü amaçlı etkinliği tehdide karşı korunmasına yardımcı olur ve işlem günlüğü dosyalarını bekleyen değişiklik yapmaya gerek kalmadan uygulama.  

TDE, veritabanının tamamını Depolama veritabanı şifreleme anahtarını adlı bir simetrik anahtar kullanarak şifreler. SQL veritabanında veritabanı şifreleme anahtarını bir yerleşik bir sunucu sertifikası tarafından korunur. Yerleşik bir sunucu sertifikası her SQL veritabanı sunucusu için benzersizdir.

Bir veritabanı GeoDR ilişkisine ise, her bir sunucuda farklı bir anahtar tarafından korunur. İki veritabanı aynı sunucusuna bağlıysanız, bunlar aynı yerleşik sertifika paylaşır. Microsoft, bu sertifikalar otomatik olarak en az 90 günde döndürür. TDE genel bir açıklaması için bkz [saydam veri şifreleme (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-tde).

### <a name="encryption-in-use-client"></a>Şifreleme kullanımda (istemci)

Çoğu veri ihlallerini kredi kartı numaraları veya kişisel olarak tanımlanabilir bilgiler gibi kritik veri hırsızlığı içerir. Veritabanları hassas bilgilerin treasure troves olabilir. Müşterilerin kişisel verileriniz, gizli rekabet bilgileri ve fikri mülkiyet içerebilirler. Kaybolan veya çalınan verileri, özellikle müşteri, marka zarar, sağlayamazsınız ve ciddi cezalar sonuçlanabilir — bile davaların.

![Always Encrypted](./media/azure-databse-security-overview/azure-database-fig1.png)

[Her zaman şifreli](https://msdn.microsoft.com/library/mt163865.aspx) kredi kartı numaraları veya Ulusal tanımlama numaraları (örneğin, ABD sosyal güvenlik numarası), gibi Azure SQL Database veya SQL Server veritabanlarına depolanan önemli verileri korumak için tasarlanmış bir özelliktir. Her zaman şifreli istemcilerin istemci uygulamalar içinde hassas verileri şifrelemek ve hiçbir zaman veritabanı altyapısı (SQL veritabanı veya SQL Server) için şifreleme anahtarları açığa olanak sağlar.

Her zaman şifreli verileri kendi (ve görüntüleyebileceğini) olanlar arasında ayrım sağlar ve kişilere verileri yönetme (ancak hiçbir erişimi olması gerekir). Şirket içi Veritabanı yöneticileri sağlayarak bulut veritabanı işleçleri veya diğer yüksek ayrıcalıklı, yetkisiz kullanıcıların, ancak şifrelenmiş verilere erişemez

Ayrıca, her zaman şifreli şifreleme saydam uygulamalara yapar. Bu otomatik olarak şifrelemek ve istemci uygulamasında hassas verilerin şifresini çözmek için istemci bilgisayarda yüklü bir her zaman şifreli özellikli sürücüsü. Sürücü veritabanı motoruna verileri geçirmeden önce hassas sütunlardaki verileri şifreler ve böylece uygulama semantiğini korunur sorguları otomatik olarak yeniden yazar. Benzer şekilde, sürücü saydam şifrelenmiş veritabanı sütunlar, sorgu sonuçlarında bulunan depolanan verilerin şifresini çözer.

## <a name="access-control"></a>Erişim denetimi
SQL Veritabanı güvenliği sağlamak için erişimi IP adresine göre bağlantıyı sınırlayan güvenlik duvarı kuralları, kullanıcıların kimliğini kanıtlamasını gerektiren kimlik doğrulama sistemleri ve kullanıcıları belirli eylemler ve verilerle sınırlayan yetkilendirme sistemleriyle denetler.

### <a name="database-access"></a>Veritabanı erişimi

Veri koruma, verilerinizin erişimini denetleme ile başlar. Ağ katmanında güvenliği yönetmek için bir güvenlik duvarı yapılandırması sırasında verilerinizi barındıran veri merkezinde fiziksel erişimi yönetir. Ayrıca oturum açma kimlik doğrulaması için yapılandırarak ve sunucu ve veritabanı rolleri için izinleri tanımlama erişimi denetler.

Bu bölümde, biz hakkında konuşma:

-   Güvenlik duvarı ve güvenlik duvarı kuralları
-   Kimlik Doğrulaması
-   Yetkilendirme

#### <a name="firewall-and-firewall-rules"></a>Güvenlik duvarı ve güvenlik duvarı kuralları

Microsoft Azure SQL Veritabanı, Azure ile diğer İnternet tabanlı uygulamalar için ilişkisel veritabanı hizmeti sunar. Güvenlik duvarları, verilerinizin korunmasına yardımcı olmak üzere, hangi bilgisayarların izinli olduğunu belirtmenize kadar veritabanı sunucunuza tüm erişimi engeller. Güvenlik duvarı, her bir isteğin kaynak IP adresine göre veritabanlarına erişim verir. Daha fazla bilgi edinmek için bkz. [Azure SQL Veritabanı güvenlik duvarı kurallarına genel bakış](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure).

[Azure SQL veritabanı](https://azure.microsoft.com/services/sql-database/) hizmetidir yalnızca TCP bağlantı noktası 1433 ' kullanılabilir. Bilgisayarınızdan bir SQL Veritabanına erişmek için istemci bilgisayarınızdaki güvenlik duvarının 1433 numaralı TCP bağlantı noktasından giden TCP iletişimine izin verdiğinden emin olun. Başka uygulamalar için gerekli değilse 1433 numaralı bağlantı noktasından gelen TCP bağlantılarını engelleyin.

#### <a name="authentication"></a>Kimlik Doğrulaması

SQL veritabanı kimlik doğrulaması, veritabanına bağlanırken kimliğinizi nasıl kanıtlayacağınızı belirtir. SQL Veritabanı iki kimlik doğrulaması türünü destekler:

-   **SQL kimlik doğrulaması:** mantıksal bir SQL örneği oluşturulduğunda, SQL veritabanı abone hesabı olarak adlandırılan bir tek oturum açma hesabı oluşturulur. Bu hesabı kullanarak bağlanır [SQL Server kimlik doğrulaması](https://docs.microsoft.com/azure/sql-database/sql-database-security-overview) (kullanıcı adı ve parola). Bu hesap mantıksal sunucu örneğinde ve bu örneğe ekli tüm kullanıcı hesaplarında yöneticidir. Abone Hesabının izinleri kısıtlanamaz. Bu hesaplardan yalnızca biri mevcut olabilir.
-   **Azure Active Directory kimlik doğrulaması:** [Azure Active Directory kimlik doğrulaması](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) kimlikleri kullanarak Azure Active Directory (Microsoft Azure SQL Database ve SQL Data Warehouse bağlayan bir mekanizmadır Azure AD). Bu, veritabanı kullanıcıları kimlikleri merkezi olarak yönetmenizi sağlar.

![Kimlik Doğrulaması](./media/azure-databse-security-overview/azure-database-fig2.png)

 Azure Active Directory kimlik doğrulaması avantajları şunlardır:
  - SQL Server kimlik doğrulaması için bir alternatif sunar.
  - Ayrıca, veritabanı sunucuları arasında kullanıcı kimlikleri artışı engellemeye yardımcı olur & parola döndürme tek bir yerde sağlar.
  - Dış (Azure Active Directory) gruplarını kullanarak veritabanı izinlerini yönetebilir.
  - Tümleşik Windows kimlik doğrulaması ve Azure Active Directory tarafından desteklenen kimlik doğrulama başka biçimlerde etkinleştirerek bunu depolanmasını parolaları ortadan kaldırabilirsiniz.

#### <a name="authorization"></a>Yetkilendirme
[Yetkilendirme](https://docs.microsoft.com/azure/sql-database/sql-database-manage-logins) ne başvuran bir kullanıcı bir Azure SQL veritabanı içinde yapabilirsiniz ve bu kullanıcı hesabınızın veritabanı tarafından denetlenen [rol üyeliklerini](https://msdn.microsoft.com/library/ms189121) ve [nesne düzeyi izinleri](https://msdn.microsoft.com/library/ms191291.aspx). Yetkilendirme sorumlu erişmek için hangi güvenliği sağlanabilir kaynakları ve işlemleri kaynaklarla için izin verilen belirleme işlemidir.

### <a name="application-access"></a>Uygulama erişimi

Bu bölümde, biz hakkında konuşma:

-   Dinamik veri maskeleme
-   Satır düzeyi güvenlik

#### <a name="dynamic-data-masking"></a>Dinamik veri maskeleme
Çağrı merkezinde bir temsilcisiyle arayanlar, sosyal güvenlik numarası veya kredi kartı numarası birkaç rakam tanımlayabilir, ancak veri öğelerden tam olarak temsilcisiyle açılmamalıdır değil.

Maskeleri tüm ancak herhangi bir sosyal güvenlik numarası veya sonuç kredi kartı numarasının son dört rakamı herhangi bir sorgu kümesi bir maskeleme kuralı tanımlanabilir.

![Dinamik veri maskeleme](./media/azure-databse-security-overview/azure-database-fig3.png)

Bir geliştirici uyumluluk düzenlemeleri ihlal etmeden sorun giderme amacıyla üretim ortamlarında sorgulayabilmesi başka bir örnek olarak, kişisel bilgileri (PII) verileri korumak için uygun veri maskesi tanımlanabilir.

[SQL veritabanı dinamik veri maskeleme](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) ayrıcalıklı olmayan kullanıcılara maskeleyerek gizli verilerin açığa sınırlar. Dinamik veri maskeleme Azure SQL veritabanı V12 sürümü için desteklenir.

[Dinamik veri maskeleme](https://docs.microsoft.com/sql/relational-databases/security/dynamic-data-masking) önlemenize yardımcı olan yetkisiz erişim için hassas verileri ne kadar uygulama katmanı üzerinde en az etkiyle ortaya çıkarmak için hassas verileri tanımlamanızı etkinleştirerek. Bu özellik, hassas verileri belirlenen veritabanı alanlarına yapılan sorgunun sonuç kümesinde gizleyen ancak veritabanındaki verileri değiştirmeyen ilke tabanlı bir güvenlik özelliğidir.


> [!Note]
> Dinamik veri maskeleme Azure veritabanı yönetici, sunucu yöneticisi veya güvenlik yetkilisi rolleri tarafından yapılandırılabilir.

#### <a name="row-level-security"></a>Satır düzeyi güvenlik
Çok müşterili veritabanları için başka bir ortak güvenlik gereksinimi [satır düzeyi güvenlik](https://msdn.microsoft.com/library/dn765131.aspx). Bu özellik, bir sorgu (örneğin, grubu üyeliği veya yürütme bağlamı) kullanıcının özelliklerini temel alan bir veritabanı tablosundaki satırları erişimi denetlemenize olanak sağlar.

![Satır düzeyi güvenlik](./media/azure-databse-security-overview/azure-database-fig4.png)

Veritabanı katmanı bulunur yerine başka bir uygulama katmanındaki veriler koymadan erişim kısıtlama mantığı. Veri erişimini herhangi bir katmanı denenir her zaman veritabanı sistem erişim kısıtlamalarını uygular. Bu güvenlik sisteminizde daha güvenli ve sağlam güvenlik sisteminizi'nın yüzey alanını azaltarak hale getirir.

Satır düzeyi güvenlik koşul tabanlı erişim denetimi sunar. Göz önünde bulundurarak meta verileri veya yönetici belirler başka bir ölçüt uygun şekilde sürebilir esnek, merkezi ve koşulu tabanlı değerlendirme özellikleri. Koşulu kullanıcı kullanıcı özniteliklerini temel alarak veri uygun erişime sahip olup olmadığını belirlemek için bir ölçüt olarak kullanılır. Etiket tabanlı erişim denetimi koşulu tabanlı erişim denetimi kullanarak uygulanabilir.

## <a name="proactive-monitoring"></a>Öngörülebilir izleme
SQL veritabanı sağlayarak, verilerinizi korur **denetim** ve **tehdit algılama** özellikleri.

### <a name="auditing"></a>Denetim
SQL veritabanı denetimi olayları ve güncelleştirmeleri ve veri sorguları da dahil olmak üzere veritabanı içinde gerçekleşen değişiklikleri kavramanıza yeteneğinizi artırır.

[Azure SQL veritabanı denetimi](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) parçaları veritabanı olayları ve Azure depolama hesabınızdaki bunları Denetim günlüğüne yazar. Denetim mevzuatla uyumluluk, veritabanı etkinliğini anlama ve işletme sorunlarını veya şüpheli güvenlik ihlallerini işaret edebilecek farklılıklar ve anormal durumlar hakkında öngörü sahip olmanıza yardımcı olabilir. Denetim sağlar ve uyumluluk standartlara uyulması kolaylaştırır, ancak Uyumluluk garanti etmez.

SQL veritabanı denetimi yapmanıza olanak sağlar:

-   **Korumak** seçili olayların bir denetim izi. Denetlenecek veritabanı eylemleri kategorilerini tanımlayabilirsiniz.
-   **Rapor** veritabanı etkinlik. Etkinlik ve olay Raporlama ile hızlı bir şekilde başlamak için önceden yapılandırılmış raporları ve panoyu kullanabilirsiniz.
-   **Analiz** raporlar. Şüpheli olayları, olağan dışı etkinliği ve eğilimleri bulabilirsiniz.

İki denetim yöntemleri vardır:

-   **BLOB denetimi** -günlükleri, Azure Blob depolama alanına yazılır. Daha yüksek performans sağlayan, daha yüksek ayrıntı düzeyi nesne düzeyinde denetimi destekler ve daha düşük maliyetli bir yeni denetim yöntem budur.
-   **Tablo denetim** -günlüklerini Azure tablo depolama alanına yazılır.

### <a name="threat-detection"></a>Tehdit algılama
[Azure SQL veritabanı tehdit algılama](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection) olası güvenlik tehditlerini gösteren kuşkulu etkinlikleri algılar. Bunlar ortaya çıktığında tehdit algılama, veritabanındaki SQL eklemelerini gibi şüpheli olaylara yanıt olanak sağlar. Uyarıları sağlar ve şüpheli olayları keşfetmek için Azure SQL veritabanı denetimi kullanılmasına izin verir.

![Tehdit algılama](./media/azure-databse-security-overview/azure-database-fig5.jpg)

Örneğin, SQL ekleme veri güdümlü uygulamaları saldırmak için kullanılıyorsa Internet üzerinde ortak Web uygulaması güvenlik sorunlarını biridir. Saldırganlar uygulama giriş alanları, kötü amaçlı SQL deyimleri eklemesine uygulama güvenlik açıkları ihlal veya veritabanındaki verileri değiştirme yararlanın.

Bunlar ortaya çıktığında güvenlik görevlileri veya diğer atanmış yöneticileri şüpheli veritabanı etkinlikleri hakkında anında bildirim edinebilirsiniz. Her bir bildirim şüpheli Etkinlik ayrıntıları sağlar ve daha fazla araştırmak ve tehdidi azaltmak nasıl önerir.        

## <a name="centralized-security-management"></a>Merkezi güvenlik yönetimi

[Azure Güvenlik Merkezi](https://azure.microsoft.com/documentation/services/security-center/), tehditleri önlemenize, algılamanıza ve yanıtlamanıza yardımcı olur. Aboneliklerinizde, tümleşik güvenlik izleme ve ilke yönetimi sağlar; normal koşullarda gözden kaçabilecek tehditleri algılamaya yardımcı olur ve güvenlik çözümlerinin geniş ekosistemiyle çalışır.

[Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-sql-database) tüm sunucular ve veritabanları güvenliğini görünürlük sağlayarak SQL veritabanındaki verileri korumaya yardımcı olur. Güvenlik Merkezi ile şunları yapabilirsiniz:

-   SQL veritabanı şifreleme ve denetim ilkeleri tanımlar.
-   SQL veritabanı kaynaklarınızın güvenliğini tüm aboneliklerinizi izleyin.
-   Hızlı bir şekilde tanımlamak ve güvenlik sorunları düzeltin.
-   Gelen uyarılar tümleştirmek [Azure SQL veritabanı tehdit algılama](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection).
-   Güvenlik Merkezi, rol tabanlı erişim destekler.

## <a name="azure-marketplace"></a>Azure Market

Azure Marketi, yeni ve bağımsız yazılım satıcılarının (ISV'ler) çözümlerini dünyanın dört bir yanındaki Azure müşterilerine sunduğu çevrimiçi bir uygulama ve hizmet marketidir.
Azure Marketi, müşterilerimize ve iş ortaklarımıza daha iyi hizmet sunmamızı sağlayan Microsoft Azure iş ortağı ekosistemlerini tek ve birleşik bir platformda birleştirir. Tıklatın [burada](https://azuremarketplace.microsoft.com/marketplace/apps?search=Database%20Security&page=1) bakışta veritabanı güvenlik ürünlere Azure markette üzerinde kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinmek [Azure SQL veritabanınıza güvenli](https://docs.microsoft.com/azure/sql-database/sql-database-security-tutorial).
- Daha fazla bilgi edinmek [Azure Güvenlik Merkezi ve Azure SQL Database hizmeti](https://docs.microsoft.com/azure/security-center/security-center-sql-database).
- Tehdit algılama hakkında daha fazla bilgi için bkz: [SQL veritabanı tehdit algılama](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection).
- Daha fazla bilgi için bkz: [artırmak SQL veritabanı performans](https://docs.microsoft.com/azure/sql-database/sql-database-performance-tutorial). 
