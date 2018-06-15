---
title: Azure veritabanı güvenliğine genel bakış | Microsoft Docs
description: Bu makalede Azure veritabanı genel bir bakış güvenlik özellikleri sağlar.
services: security
documentationcenter: na
author: UnifyCloud
manager: mbaldwin
editor: TomSh
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/01/2017
ms.author: TomSh
ms.openlocfilehash: 44abf7a4fc24893146179b34d3357f54450decab
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
ms.locfileid: "34365153"
---
# <a name="azure-database-security-overview"></a>Azure veritabanı güvenliğine genel bakış

Güvenlik veritabanları yönetmek için bir üst konudur ve her zaman Azure SQL veritabanı için bir öncelik olmuştur. Azure SQL veritabanı güvenlik duvarı kuralları ve bağlantı şifreleme ile bağlantı güvenliği destekler. Kullanıcı adı ve parola ile kimlik doğrulaması ve Azure Active Directory tarafından yönetilen kimlikleri kullanan Azure Active Directory (Azure AD) kimlik doğrulamasını destekler. Yetkilendirme rol tabanlı erişim denetimi kullanır.

Azure SQL veritabanı, gerçek zamanlı şifreleme ve şifre çözme veritabanları, ilişkili yedeklemelerinizi ve geri kalan işlem günlüğü dosyalarını uygulamasında yapılacak değişiklikler gerek kalmadan gerçekleştirerek Şifreleme destekler.

Microsoft Kurumsal verileri şifrelemek için ek yollar sağlar:

-   Hücre düzeyi şifreleme belirli sütunları veya veri bile hücrelerinin farklı şifreleme anahtarlarını şifrelemek kullanılabilir.
-   Bir donanım güvenlik modülü veya şifreleme anahtar hiyerarşinizdeki Merkezi Yönetim gerekiyorsa, Azure anahtar kasası SQL Server ile bir Azure sanal makinede (VM) kullanmayı düşünün.
-   Her zaman şifreli (şu anda önizlemede) şifreleme saydam uygulamalara yapar. Ayrıca, şifreleme anahtarları ile SQL veritabanını paylaşan olmadan istemci uygulamalar içinde hassas verileri şifrelemek istemcilerin sağlar.

Azure SQL veritabanı denetimi kuruluşların Azure Storage'da denetim olayları günlüğe kaydını etkinleştirir. SQL Veritabanı Denetimi ayrıca Microsoft Power BI ile tümleştirilerek ayrıntılı raporlar ve analizler oluşturulmasını sağlar.

Azure SQL veritabanları sıkı bir şekilde güvenli en yasal karşılamak için veya güvenlik gereksinimleri, HIPAA ve ISO 27001/27002 PCI DSS düzey 1 çeşitli. Geçerli güvenlik uyumluluk sertifikalar listesini kullanılabilir [Microsoft Azure Trust Center site](http://azure.microsoft.com/support/trust-center/services/).

Bu makalede, Microsoft Azure SQL veritabanları için yapılandırılmış, tablo ve ilişkisel bir veri güvenliği temelleri size yol göstermektedir. Bu makalede özellikle veri koruma, erişim denetimi ve öngörülebilir izleme kaynakları için başlangıç bilgileri sunulmaktadır.

## <a name="protection-of-data"></a>Veri koruma

SQL veritabanı şifreleme sağlayarak verilerinizin korunmasına yardımcı olur:

- Hareket halindeki verileriniz için [Aktarım Katmanı Güvenliği (TLS)](https://support.microsoft.com/kb/3135244).
- Veriler, kullanılmadıkları sırada için [saydam veri şifreleme](http://go.microsoft.com/fwlink/?LinkId=526242).
- Aracılığıyla kullanım verileri için [her zaman şifreli](https://msdn.microsoft.com/library/mt163865.aspx).

Verilerinizi şifrelemek için kullanabileceğiniz diğer yöntemler şunlardır:

-   [Hücre düzeyinde şifreleme](https://msdn.microsoft.com/library/ms179331.aspx): Belirli sütunları hatta veri hücrelerini farklı şifreleme anahtarlarıyla şifrelemenizi sağlar.
-   [Azure anahtar kasası Azure VM'deki SQL Server ile](http://blogs.technet.com/b/kv/archive/2015/01/12/using-the-key-vault-for-sql-server-encryption.aspx), bir donanım güvenlik modülü veya şifreleme anahtar hiyerarşinizdeki Merkezi Yönetim gerekiyorsa.

### <a name="encryption-in-motion"></a>Hareketli şifreleme

Tüm istemci/sunucu uygulamaları için ortak bir sorunu gizlilik gereksinimini aynıdır ortak ve özel ağlar üzerinden verileri taşır. Bir ağ üzerinden taşımaktan veriler şifrelenmez, yakalanan ve olması yetkisiz kullanıcılar tarafından çalınırsa, bir fırsat yok. Veritabanı Hizmetleri ile ilgilenirken veritabanı istemci ve sunucu arasında veri şifrelenir emin olun. Ayrıca birbirleriyle ve orta katman uygulamalarıyla iletişim veritabanı sunucuları arasında veri şifrelenir emin olun.

Ağ yönetirken sorunlardan biri güvenilmeyen bir ağ üzerinden uygulamalar arasında gönderilen verilerin güvenli hale getirir. Kullanabileceğiniz [TLS/SSL](https://docs.microsoft.com/windows-server/security/tls/transport-layer-security-protocol) sunucuları ve istemcileri kimlik doğrulaması ve kimliği doğrulanmış taraflar arasında iletileri şifrelemek için kullanın.

Kimlik doğrulama işleminde bir TLS/SSL istemcisi bir TLS/SSL sunucusuna ileti gönderir. Sunucu, sunucu kendi kimliğini doğrulamak için gerek duyduğu bilgilerle yanıt verir. İstemci ve sunucu, oturum anahtarlarının ek değişimini gerçekleştirir ve kimlik doğrulama iletişimi sona erer. Kimlik doğrulaması tamamlandığında, öğe SSL korumalı iletişim kimlik doğrulama işlemi sırasında oluşturulmuş simetrik şifreleme anahtarları aracılığıyla sunucu ve istemci arasında başlayabilirsiniz.

Veri "" için ve veritabanından esnasında tüm bağlantılar Azure SQL veritabanı şifreleme (TLS/SSL), her zaman gerektirir. SQL veritabanı sunucuları ve istemcileri kimlik doğrulaması ve kimliği doğrulanmış taraflar arasında iletileri şifrelemek için kullanmak için TLS/SSL kullanır. 

Uygulamanızın bağlantı dizesinde bağlantıyı şifrelemek ve sunucu sertifikası güvenmediğiniz to parametrelerini belirtmeniz gerekir. (Bağlantı dizenizi Azure portal dışında kopyalarsanız, bu sizin için gerçekleştirilir.) Aksi takdirde, bağlantı sunucusunun kimliğini doğrulamaz ve "man-in--middle" saldırılarına olacaktır. ADO.NET sürücüsü için örneğin, bu bağlantı dizesi parametreleri olan `Encrypt=True` ve `TrustServerCertificate=False`.

### <a name="encryption-at-rest"></a>Bekleme sırasında şifreleme
Veritabanını güvenli hale getirmek için bazı önlemler alabilir. Örneğin, güvenli bir sistemde tasarım, gizli varlıklar şifrelemek ve veritabanı sunucuları geçici bir güvenlik duvarı oluşturma. Ancak burada fiziksel medya (örneğin, sürücüleri veya yedekleme bantlarını) çalınması bir senaryoda, kötü amaçlı bir taraf yalnızca geri yükleyebilir veya veritabanını ve veri göz atın.

Bir veritabanındaki hassas verileri şifrelemek ve bir sertifika ile verileri şifrelemek için kullanılan anahtarları korumak için bir çözümdür. Bu çözüm herkes anahtarları olmadan verileri kullanmasını engeller, ancak bu tür bir koruma planlanmalıdır.

Bu sorun, SQL Server ve SQL veritabanı desteği çözmek için [saydam veri şifreleme](https://docs.microsoft.com/sql/relational-databases/securityrecryption/transparent-data-encryption-tde). Saydam veri şifreleme rest şifreleme verileri olarak bilinen SQL Server ve SQL veritabanı veri dosyalarını şifreler.

Saydam veri şifreleme, kötü amaçlı etkinliği tehdide karşı korunmasına yardımcı olur. Gerçek zamanlı şifreleme ve şifre çözme veritabanının, ilişkili yedeklemelerinizi ve geri kalan işlem günlüğü dosyalarını uygulamasında yapılacak değişiklikler gerek kalmadan gerçekleştirir.  

Saydam veri şifreleme veritabanının tamamını Depolama veritabanı şifreleme anahtarını adlı bir simetrik anahtar kullanarak şifreler. SQL veritabanında veritabanı şifreleme anahtarını bir yerleşik bir sunucu sertifikası tarafından korunur. Yerleşik bir sunucu sertifikası her SQL veritabanı sunucusu için benzersizdir.

Bir veritabanı bir coğrafi DR ilişkisinde ise, her bir sunucuda farklı bir anahtar tarafından korunur. İki veritabanı aynı sunucusuna bağlıysanız, bunlar aynı yerleşik sertifika paylaşır. Microsoft, bu sertifikalar otomatik olarak en az 90 günde döndürür. 

Daha fazla bilgi için bkz: [saydam veri şifreleme](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-tde).

### <a name="encryption-in-use-client"></a>Şifreleme kullanımda (istemci)

Çoğu veri ihlallerini kredi kartı numaraları veya kişisel olarak tanımlanabilir bilgiler gibi kritik veri hırsızlığı içerir. Veritabanları hassas bilgilerin treasure troves olabilir. Müşterilerin (Ulusal Kimlik numaraları gibi) kişisel verileriniz, gizli rekabet bilgileri ve fikri mülkiyet içerebilirler. Kaybolan veya çalınan verileri, özellikle müşteri, marka zarar, sağlayamazsınız ve ciddi cezalar--bile davaların neden olabilir.

![Kilit ve anahtar gösterilen her zaman şifreli özelliği](./media/azure-databse-security-overview/azure-database-fig1.png)

[Her zaman şifreli](https://msdn.microsoft.com/library/mt163865.aspx) Azure SQL Database veya SQL Server veritabanlarına depolanan önemli verileri korumak için tasarlanmış bir özelliktir. Her zaman şifreli istemcilerin istemci uygulamalar içinde hassas verileri şifrelemek ve hiçbir zaman veritabanı altyapısı (SQL veritabanı veya SQL Server) için şifreleme anahtarları açığa olanak sağlar.

Her zaman şifreli verileri kendi (ve görüntüleyebileceğini) kişiler ve verileri yönetme (ancak hiçbir erişimi olması gerekir) kişiler arasında ayrım sağlar. Bu, şirket içi Veritabanı yöneticileri, bulut veritabanı işleçleri veya diğer yüksek ayrıcalıklı ancak yetkisiz kullanıcıların şifrelenmiş verilere erişemiyor sağlamaya yardımcı olur.

Ayrıca, her zaman şifreli şifreleme saydam uygulamalara yapar. Böylece, otomatik olarak şifrelemek ve istemci uygulamasında hassas verilerin şifresini sürücüsü her zaman şifreli etkin istemci bilgisayarda yüklü. Sürücü, veritabanı motoruna verileri geçirmeden önce hassas sütunlardaki verileri şifreler. Böylece uygulama semantiğini korunur sürücü otomatik olarak sorgular yeniden yazar. Benzer şekilde, sürücü saydam şifrelenmiş veritabanı sütunlar, sorgu sonuçlarında bulunan depolanan verilerin şifresini çözer.

## <a name="access-control"></a>Erişim denetimi
Güvenlik sağlamak için SQL veritabanı kullanarak erişimi kontrol eder:

- IP adresine göre bağlantı sınırlamak güvenlik duvarı kuralları.
- Kullanıcıların kimliğini kanıtlamak gerekli kimlik doğrulama mekanizmaları.
- Belirli eylemleri ve veri kullanıcılarına sınırlamak yetkilendirme mekanizmaları.

### <a name="database-access"></a>Veritabanı erişimi

Veri koruma, verilerinizin erişimini denetleme ile başlar. Verilerinizi barındıran veri merkezinde fiziksel erişimi yönetir. Ağ katmanında güvenliği yönetmek için bir Güvenlik Duvarı'nı yapılandırabilirsiniz. Ayrıca oturum açma kimlik doğrulaması için yapılandırarak ve sunucu ve veritabanı rolleri için izinleri tanımlama erişimi denetler.

#### <a name="firewall-and-firewall-rules"></a>Güvenlik duvarı ve güvenlik duvarı kuralları

[Azure SQL veritabanı](https://azure.microsoft.com/services/sql-database/) Azure ve diğer Internet tabanlı uygulamalar için bir ilişkisel veritabanı hizmeti sağlar. Güvenlik duvarları, verilerinizin korunmasına yardımcı olmak üzere, hangi bilgisayarların izinli olduğunu belirtmenize kadar veritabanı sunucunuza tüm erişimi engeller. Güvenlik duvarı, her bir isteğin kaynak IP adresine göre veritabanlarına erişim verir. Daha fazla bilgi edinmek için bkz. [Azure SQL Veritabanı güvenlik duvarı kurallarına genel bakış](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure).

Azure SQL veritabanı hizmetinin yalnızca TCP 1433 bağlantı noktasını kullanılabilir. Bilgisayarınızdan bir SQL veritabanına erişmek için istemci bilgisayar güvenlik duvarı TCP bağlantı noktası 1433 üzerinde giden TCP iletişimi sağlayan emin olun. Gelen bağlantılar diğer uygulamalar için gerekli değildir, TCP bağlantı noktası 1433 engellemek.

#### <a name="authentication"></a>Kimlik Doğrulaması

Kimlik doğrulaması, veritabanına bağlanırken kimliğinizi nasıl kanıtlayacağınızı belirtir. SQL Veritabanı iki kimlik doğrulaması türünü destekler:

-   **SQL Server kimlik doğrulaması**: mantıksal bir SQL örneği oluşturulduğunda, SQL veritabanı abone hesabı olarak adlandırılan bir tek oturum açma hesabı oluşturulur. Bu hesabı kullanarak bağlanır [SQL Server kimlik doğrulaması](https://docs.microsoft.com/azure/sql-database/sql-database-security-overview) (kullanıcı adı ve parola). Bu hesap mantıksal sunucu örneğinde ve bu örneğe ekli tüm kullanıcı hesaplarında yöneticidir. Abonelik hesap izinlerini kısıtlanamaz. Bu hesaplardan yalnızca biri mevcut olabilir.
-   **Azure Active Directory kimlik doğrulaması**: [Azure AD kimlik doğrulaması](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) Azure AD'de kimlikleri kullanarak Azure SQL Database ve Azure SQL Data Warehouse bağlayan bir mekanizmadır. Veritabanı kullanıcı kimlikleri merkezi olarak yönetmek için kullanabilirsiniz.

![SQL veritabanı ile Azure AD kimlik doğrulaması](./media/azure-databse-security-overview/azure-database-fig2.png)

 Azure AD kimlik doğrulaması avantajları şunlardır:
  - SQL Server kimlik doğrulaması için bir alternatif sunar.
  - Veritabanı sunucuları arasında kullanıcı kimlikleri artışı engellemeye yardımcı olur ve tek bir yerde parola döndürme sağlar.
  - Dış (Azure AD) gruplarını kullanarak veritabanı izinlerini yönetebilir.
  - Tümleşik Windows kimlik doğrulaması ve Azure AD destekleyen kimlik doğrulama başka biçimlerde etkinleştirerek bunu depolanmasını parolaları ortadan kaldırabilirsiniz.

#### <a name="authorization"></a>Yetkilendirme
[Yetkilendirme](https://docs.microsoft.com/azure/sql-database/sql-database-manage-logins) bir kullanıcı bir Azure SQL veritabanı içinde neler yapabileceğinizi için başvuruyor. Kullanıcı hesabınızın veritabanı tarafından denetlenen [rol üyeliklerini](https://msdn.microsoft.com/library/ms189121) ve [nesne düzeyi izinleri](https://msdn.microsoft.com/library/ms191291.aspx). Yetkilendirme sorumlu erişmek için hangi güvenliği sağlanabilir kaynakları ve işlemleri kaynaklarla için izin verilen belirleme işlemidir.

### <a name="application-access"></a>Uygulama erişimi

#### <a name="dynamic-data-masking"></a>Dinamik veri maskeleme
Bir hizmet temsilcisi çağrı merkezinde arayanlar kendi sosyal güvenlik numarası veya kredi kartı numarası birkaç rakam tanımlayabilirsiniz. Ancak bu veri öğeleri tam olarak temsilcisiyle açılmamalıdır değil.

Sosyal Güvenlik numarası veya kredi kartı numarası herhangi bir sorgu sonuç kümesinde son dört rakamı dışındaki tüm maskeleri bir maskeleme kuralı tanımlayabilirsiniz.

![SQL veritabanı ve iş uygulamalar arasında gönderilen bir dizi maskeleme](./media/azure-databse-security-overview/azure-database-fig3.png)

Başka bir örnek olarak, kişisel bilgileri korumak için uygun veri maskesi tanımlanabilir. Bir geliştirici, ardından uyumluluk düzenlemeleri ihlal etmeden sorun giderme amacıyla üretim ortamlarında sorgulayabilirsiniz.

[SQL Veritabanı dinamik veri maskeleme](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started), hassas verilerin görünürlüğünü ayrıcalık sahibi olmayan kullanıcılardan gizleyerek sınırlar. Dinamik veri maskeleme Azure SQL veritabanı V12 sürümü için desteklenir.

[Dinamik veri maskeleme](https://docs.microsoft.com/sql/relational-databases/security/dynamic-data-masking) önlemenize yardımcı olan yetkisiz erişim için hassas verileri ne kadar uygulama katmanı üzerinde en az etkiyle ortaya çıkarmak için hassas verileri tanımlamanızı etkinleştirerek. Bu özellik, hassas verileri belirlenen veritabanı alanlarına yapılan sorgunun sonuç kümesinde gizleyen ancak veritabanındaki verileri değiştirmeyen ilke tabanlı bir güvenlik özelliğidir.


> [!Note]
> Dinamik veri maskeleme Azure veritabanı yönetici, sunucu yöneticisi veya güvenlik yetkilisi rolleri tarafından yapılandırılabilir.

#### <a name="row-level-security"></a>Satır Düzeyinde Güvenlik
Çok müşterili veritabanları için başka bir ortak güvenlik gereksinimi [satır düzeyi güvenlik](https://msdn.microsoft.com/library/dn765131.aspx). Bir sorgu yürütülürken kullanıcının özelliklerini temel alan bir veritabanı tablosundaki satırları erişimi denetlemek için bu özelliği kullanın. Örnek özellikleri grup üyeliği ve yürütme bağlamı dışında (.)

![Satır düzeyi güvenlik istemci uygulaması aracılığıyla bir tablodaki bir kullanıcıya erişim izni satırlar izin verme](./media/azure-databse-security-overview/azure-database-fig4.png)

Veritabanı katmanı bulunur yerine başka bir uygulama katmanındaki veriler koymadan erişim kısıtlama mantığı. Veri erişimini herhangi bir katmanı denenir her zaman veritabanı sistem erişim kısıtlamalarını uygular. Bu güvenlik sisteminizde daha güvenli ve sağlam güvenlik sisteminizi'nın yüzey alanını azaltarak hale getirir.

Satır düzeyi güvenlik koşulu tabanlı erişim denetimi sunar. Göz önünde bulundurarak meta verileri veya yönetici belirler başka bir ölçüt uygun şekilde sürebilir esnek, merkezi bir değerlendirme özellikleri. Koşulu kullanıcı kullanıcı özniteliklerini temel alarak veri uygun erişime sahip olup olmadığını belirlemek için bir ölçüt olarak kullanılır. Koşulu tabanlı erişim denetimi kullanarak etiket tabanlı erişim denetimi uygulayabilirsiniz.

## <a name="proactive-monitoring"></a>Öngörülebilir izleme
SQL veritabanı yardımcı sağlayarak verilerinizin güvenliğini *denetim* ve *tehdit algılama* özellikleri.

### <a name="auditing"></a>Denetim
[Azure SQL veritabanı denetimi](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) olaylar ve veritabanı içinde gerçekleşen değişiklikleri kavramanıza yeteneğinizi artırır. Güncelleştirmeleri ve veri sorguları örnektir.

SQL veritabanı denetimi veritabanı olaylarını ve Azure depolama hesabınızdaki bunları Denetim günlüğüne yazar izler. Denetim, yönetmeliklere uygunluğu korumanıza, veritabanı etkinliklerini anlamanıza ve ifade eden tutarsızlıklar ve iş sorunları gösterebilir veya güvenlik ihlalleri hakkında daha fazla bilgi kavramanıza yardımcı olabilir. Denetim sağlar ve uyumluluk standartlara uyulması kolaylaştırır, ancak Uyumluluk garanti etmez.

SQL veritabanı için denetim kullanabilirsiniz:

-   **Korumak** seçili olayların bir denetim izi. Denetlenecek veritabanı eylemleri kategorilerini tanımlayabilirsiniz.
-   **Rapor** veritabanı etkinlik. Etkinlik ve olay Raporlama ile hızlı bir şekilde başlamak için önceden yapılandırılmış raporları ve panoyu kullanabilirsiniz.
-   **Analiz** raporlar. Şüpheli olayları, olağan dışı etkinliği ve eğilimleri bulabilirsiniz.

Denetim iki yöntem vardır:

-   **BLOB denetimi**: günlükleri, Azure Blob depolama alanına yazılır. Daha yeni bir denetim yöntem budur. Daha yüksek performans sağlar, daha yüksek ayrıntı düzeyi nesne düzeyinde denetimi destekler ve daha uygun maliyetli olması.
-   **Tablo denetim**: günlükleri, Azure Table depolama alanına yazılır.

### <a name="threat-detection"></a>Tehdit algılama
[Azure SQL veritabanı tehdit algılama](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection) olası güvenlik tehditlerini gösteren kuşkulu etkinlikleri algılar. Bunlar ortaya çıktığında tehdit algılama veritabanında SQL eklemelerini gibi şüpheli olaylarına tepki vermek için kullanabilirsiniz. Uyarıları sağlar ve şüpheli olayları keşfetmek için Azure SQL veritabanı denetimi kullanımına izin verir.

![Saldırgan ve kötü amaçlı kişinin ile SQL veritabanı ve bir web uygulaması için tehdit algılama](./media/azure-databse-security-overview/azure-database-fig5.jpg)

Örneğin, SQL ekleme web uygulamaları için ortak güvenlik sorunları biridir. Veri tabanlı uygulamaları saldırmak için kullanılır. Saldırganlar uygulama giriş alanları, kötü amaçlı SQL deyimleri eklemesine uygulama güvenlik açıkları ihlal veya veritabanındaki verileri değiştirme yararlanın.

Bunlar ortaya çıktığında güvenlik görevlileri veya diğer atanmış yöneticileri şüpheli veritabanı etkinlikleri hakkında anında bildirim edinebilirsiniz. Her bir bildirim şüpheli Etkinlik ayrıntıları sağlar ve daha fazla araştırmak ve tehdidi azaltmak nasıl önerir.        

## <a name="centralized-security-management"></a>Merkezi güvenlik yönetimi

[Azure Güvenlik Merkezi](https://azure.microsoft.com/documentation/services/security-center/), tehditleri önlemenize, algılamanıza ve yanıtlamanıza yardımcı olur. Aboneliklerinizde tümleşik güvenlik izleme ve ilke yönetimi, Azure sağlar. Kaçabilecek tehditleri algılayabilir yardımcı olur ve güvenlik çözümlerinin geniş ekosistemiyle ile çalışır.

[Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-sql-database) tüm sunucular ve veritabanları güvenliğini görünürlük sağlayarak SQL veritabanındaki verileri korumaya yardımcı olur. Güvenlik Merkezi ile şunları yapabilirsiniz:

-   SQL veritabanı şifreleme ve denetim ilkeleri tanımlar.
-   SQL veritabanı kaynaklarınızın güvenliğini tüm aboneliklerinizi izleyin.
-   Hızlı bir şekilde tanımlamak ve güvenlik sorunları düzeltin.
-   Gelen uyarılar tümleştirmek [Azure SQL veritabanı tehdit algılama](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection).

Güvenlik Merkezi, rol tabanlı erişim destekler.

## <a name="azure-marketplace"></a>Azure Market

Azure Marketi, yeni ve bağımsız yazılım satıcılarının (ISV'ler) çözümlerini dünyanın dört bir yanındaki Azure müşterilerine sunduğu çevrimiçi bir uygulama ve hizmet marketidir.
Azure Market daha iyi hizmet vermemesini müşterileri ve ortakları için birleştirilmiş bir platformu Microsoft Azure iş ortağı ekosistemlerini birleştirir. Yapabilecekleriniz [arama çalıştırma](https://azuremarketplace.microsoft.com/marketplace/apps?search=Database%20Security&page=1) Azure Marketi'nde veritabanı güvenlik ürünleri görüntülemek için.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure SQL veritabanınızı güvenli](https://docs.microsoft.com/azure/sql-database/sql-database-security-tutorial)
- [Azure Güvenlik Merkezi ve Azure SQL veritabanı hizmeti](https://docs.microsoft.com/azure/security-center/security-center-sql-database)
- [SQL veritabanı tehdit algılama](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection)
- [SQL veritabanı performansı](https://docs.microsoft.com/azure/sql-database/sql-database-performance-tutorial)
