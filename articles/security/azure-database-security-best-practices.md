---
title: Azure veritabanı en iyi güvenlik yöntemleri | Microsoft Docs
description: Bu makalede Azure veritabanı güvenliği için en iyi yöntemler kümesi sağlar.
services: security
documentationcenter: na
author: unifycloud
manager: barbkess
editor: tomsh
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/20/2018
ms.author: tomsh
ms.openlocfilehash: 3e244f89904ce9aca161ed1ea435f4137e42bc5d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60587530"
---
# <a name="azure-database-security-best-practices"></a>Azure veritabanı en iyi güvenlik uygulamaları
Güvenlik veritabanlarını yönetmek için güvenliğin çok önemli olduğu ve her zaman için bir öncelik olmuştur [Azure SQL veritabanı](https://docs.microsoft.com/azure/sql-database/). Veritabanlarınızı sıkı bir şekilde en yasal karşılamak amacıyla güvenli hale getirilebilir veya güvenlik gereksinimleri, HIPAA, ISO 27001/27002 ve PCI DSS düzey 1 gibi. Güvenlik uyumluluk sertifikaları güncel bir listesi kullanılabilir [Microsoft Trust Center site](https://azure.microsoft.com/support/trust-center/services/). Ayrıca, Mevzuat gereksinimlerine göre belirli Azure veri merkezlerinde veritabanlarınızı yerleştirmek seçebilirsiniz.

Bu makalede, en iyi güvenlik uygulamaları, Azure veritabanı koleksiyonu ele alır. Bu en iyi uygulamaları, Azure veritabanı ile güvenliği deneyimlerimizden türetilmiştir ve müşteri deneyimleri bulunun.

En iyi her uygulama için biz açıklar:

-   En iyi nedir
-   Bu en iyi etkinleştirmek istediğiniz neden
-   En iyi etkinleştirme başarısız olursa ne sonuç olabilir
-   Nasıl en iyi etkinleştirmek bilgi edinebilirsiniz

Bu Azure veritabanı güvenlik en iyi yöntemler makalesi, fikir birliğine varılmış fikrim ve Azure platformu özellikleri temel alır ve bu makalenin yazıldığı sırada oldukları gibi özelliğini ayarlar. Fikirlerini ve teknolojileri zamanla değişir ve bu makalede, bu değişiklikleri yansıtacak şekilde düzenli olarak güncelleştirilir.

## <a name="use-firewall-rules-to-restrict-database-access"></a>Veritabanı erişimi kısıtlamak için güvenlik duvarı kuralları kullanma
Microsoft Azure SQL veritabanı, Azure ve diğer internet tabanlı uygulamalar için ilişkisel veritabanı hizmetidir. Erişim güvenliğini sağlamak için SQL veritabanı ile erişim denetimleri:

- IP adresine göre bağlantıyı sınırlayan güvenlik duvarı kuralları.
- Kullanıcıların kimliğini kanıtlamasını gerektiren kimlik doğrulama mekanizmaları.
- Kullanıcıları belirli eylemler ve verilerle sınırlayan yetkilendirme sistemleriyle.

Güvenlik duvarları, hangi bilgisayarların izinli olduğunu belirtmenize kadar veritabanı sunucunuza tüm erişimi engeller. Güvenlik duvarı, her bir isteğin kaynak IP adresine göre veritabanlarına erişim verir.

Aşağıdaki şekil, SQL veritabanı'nda bir sunucu güvenlik duvarı ayarlandığı gösterir:

![Güvenlik duvarı kuralları](./media/azure-database-security-best-practices/azure-database-security-best-practices-Fig1.png)

Azure SQL veritabanı hizmeti yalnızca 1433 numaralı TCP bağlantı kullanılabilir. Bilgisayarınızdan bir SQL veritabanına erişmek için istemci bilgisayar Güvenlik Duvarı'nın 1433 numaralı TCP bağlantı noktasını giden TCP iletişimine izin verdiğinden emin olun. Diğer uygulamalar için bu bağlantıları ihtiyacınız yoksa güvenlik duvarı kurallarını kullanarak 1433 numaralı TCP bağlantı noktasını gelen bağlantıları engelleyin.

Bağlantı işleminin bir parçası olarak, Azure sanal makinelerinden gelen bağlantılar bir IP adresi ve her bir çalışan rolü için benzersiz bir bağlantı noktasına yönlendirilir. Bağlantı noktası numarası 11000 ile 11999 arasındadır. TCP bağlantı noktaları hakkında daha fazla bilgi için bkz. [ADO.NET 4.5 için 1433 dışındaki bağlantı noktaları](../sql-database/sql-database-develop-direct-route-ports-adonet-v12.md).

SQL Veritabanındaki güvenlik duvarı kuralları hakkında daha fazla bilgi için bkz. [SQL Veritabanı güvenlik duvarı kuralları](../sql-database/sql-database-firewall-configure.md).

> [!Note]
> IP kurallarının yanı sıra, güvenlik duvarı sanal ağ kuralları yönetir. Sanal ağ kuralları, sanal ağ hizmet uç noktaları üzerinde temel alır. Sanal ağ kuralları, bazı durumlarda IP kurallara tercih edilebilir. Daha fazla bilgi için bkz. [sanal ağ hizmet uç noktaları ve Azure SQL veritabanı için kuralları](../sql-database/sql-database-vnet-service-endpoint-rule-overview.md).

## <a name="enable-database-authentication"></a>Veritabanı kimlik doğrulamasını etkinleştirme
SQL veritabanı iki tür kimlik doğrulaması, SQL Server kimlik doğrulaması ve Azure AD kimlik doğrulamasını destekler.

### <a name="sql-server-authentication"></a>*SQL Server kimlik doğrulaması*

Avantajları şunlardır:

- Burada tüm kullanıcıların bir Windows etki alanı tarafından doğrulanmaz ortamları karma işletim sistemleriyle desteklemek SQL veritabanı sağlar.
- Eski uygulamaları ve iş ortağı tarafından sağlanan ve SQL Server kimlik doğrulaması gerektiren uygulamaları desteklemek SQL veritabanı sağlar.
- Bilinmeyen veya güvenilmeyen etki alanlarından bağlanmasına izin verir. Bir örnek, kurulan müşteri siparişlerinin durumunu almak için atanan SQL Server oturumu ile eriştikleri bir uygulamadır.
- SQL veritabanı, web tabanlı uygulamaları desteklemek, burada kullanıcılar kendi kimlikleri oluşturma sağlar.
- Bilinen, önceden oluşturulmuş SQL Server oturumu tabanlı bir karmaşık izin hiyerarşisi'ni kullanarak uygulamalarını dağıtmak yazılım geliştiricilerinin sağlar.

> [!NOTE]
> SQL Server kimlik doğrulaması, Kerberos güvenlik protokolünü kullanamazsınız.

SQL Server kimlik doğrulamasını kullanıyorsanız, şunları yapmalısınız:

- Güçlü kimlik bilgileri kendiniz yönetirsiniz.
- Bağlantı dizesindeki kimlik bilgilerini koruyun.
- Ağ üzerinden web sunucusu vm'sinden veritabanı geçirilen kimlik bilgileri (büyük olasılıkla) koruyun. Daha fazla bilgi için [nasıl yapılır: ASP.NET 2.0 SQL kimlik doğrulaması kullanarak SQL sunucusuna bağlanmak](/previous-versions/msp-n-p/ff648340(v=pandp.10)).

### <a name="azure-active-directory-ad-authentication"></a>*Azure Active Directory (AD) kimlik doğrulaması*
Azure AD kimlik doğrulaması, Azure SQL veritabanı'na bağlanma mekanizması olduğundan ve [SQL veri ambarı](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) Azure AD'de kimlik kullanarak. Azure AD kimlik doğrulaması ile veritabanı kullanıcılarının kimliklerini ve diğer Microsoft Hizmetleri tek bir merkezi konumda yönetebilir. Merkezi kimlik yönetimi, veritabanı kullanıcıları yönetmek için tek bir yerde sağlar ve izin yönetimini kolaylaştırır.

> [!NOTE]
> Azure AD kimlik doğrulaması SQL Server kimlik doğrulamasının kullanılması öneririz.

Avantajları şunlardır:

- Bu SQL Server kimlik doğrulaması için bir alternatif sağlar.
- Veritabanı sunucuları arasında kullanıcı kimliklerini çoğalan Durdur yardımcı olur.
- Tek bir yerde parola dönüşü sağlar.
- Müşteriler, dış (Azure AD) grupları kullanarak veritabanı izinlerini yönetebilir.
- Tümleşik Windows kimlik doğrulaması ve diğer Azure Active Directory tarafından desteklenen kimlik doğrulama etkinleştirerek bu parolaları ortadan kaldırabilir.
- Kapsanan veritabanı kullanıcıları ve veritabanı düzeyinde kimlikleri doğrulamak için kullanır.
- SQL veritabanına bağlanan uygulamalar için belirteç tabanlı kimlik doğrulamayı destekler.
- Bu AD FS (etki alanı Federasyon) veya yerel kullanıcı/parola kimlik doğrulaması için etki alanı eşitleme olmadan yerel bir Azure Active Directory örneğini destekler.
- Azure AD Active Directory Evrensel multi-Factor Authentication içeren kimlik doğrulaması kullanan SQL Server Management Studio bağlantılarından destekler. Çok faktörlü kimlik doğrulaması sağlayan çeşitli doğrulama seçenekleri ile güçlü kimlik doğrulaması — telefon araması, SMS mesajı, akıllı kartlar ve PIN ya da mobil uygulama bildirimi. Daha fazla bilgi için [SQL veritabanı ve SQL veri ambarı ile Azure AD multi-Factor Authentication için SSMS desteği](../sql-database/sql-database-ssms-mfa-authentication.md).

Yapılandırma adımları yapılandırmak ve Azure AD kimlik doğrulamasını kullanmak için aşağıdaki yordamları içerir:

- Oluşturma ve Azure AD doldurabilirsiniz.
- İsteğe bağlı: İlişkilendirme veya şu anda Azure aboneliğinizle ilişkili Active Directory örneğine değiştirin.
- Azure SQL veritabanı için bir Azure Active Directory Yöneticisi oluşturma veya [Azure SQL veri ambarı](https://azure.microsoft.com/services/sql-data-warehouse/).
- İstemci bilgisayarlarınızın yapılandırın.
- Azure AD kimlikleri için eşlenmiş veritabanında bağımsız veritabanı kullanıcılarını oluşturun.
- Azure AD kimlikleri kullanarak veritabanınıza bağlanın.

Ayrıntılı bilgiler bulabilirsiniz [kullanımı Azure Active Directory kimlik doğrulamasını SQL veritabanı, yönetilen örneği veya SQL veri ambarı ile](../sql-database/sql-database-aad-authentication.md).

## <a name="protect-your-data-by-using-encryption-and-row-level-security"></a>Şifreleme ve satır düzeyinde güvenlik kullanarak verilerinizi koruyun
[Azure SQL veritabanı saydam veri şifrelemesi](../sql-database/transparent-data-encryption-azure-sql.md) diskteki verilerin korunmasına yardımcı olur ve donanım yetkisiz erişime karşı korur. Bu gerçek zamanlı şifreleme ve şifre çözme veritabanını, ilişkili yedeklemeler ve işlem günlük dosyaları bekleme sırasında uygulamada değişiklik gerektirmeden gerçekleştirir. Saydam veri şifrelemesi tüm veritabanı depolama, veritabanı şifreleme anahtarı olarak adlandırılan bir simetrik anahtarı'nı kullanarak şifreler.

Tüm depolama bile şifrelenir, ayrıca veritabanı şifrelemek önemlidir. Bu veri koruma için derinlemesine savunma yaklaşım uygulamasıdır. Azure SQL veritabanı kullanıyorsanız ve hassas verileri (örneğin, kredi kartı veya sosyal güvenlik numaraları) korumak istediğiniz, veritabanları ile FIPS 140-2 doğrulanmış 256 bit AES şifreleme şifreleyebilirsiniz. Bu şifreleme, çoğu endüstri standartları (örneğin, HIPAA ve PCI) gereksinimlerini karşılar.

İlgili dosyaların [arabellek havuzu uzantısı (BPE)](https://docs.microsoft.com/sql/database-engine/configure-windows/buffer-pool-extension) saydam veri şifrelemesi kullanarak bir veritabanı şifrelediğinizde şifrelenmez. Dosya sistemi düzeyinde şifreleme araçları gibi kullanmalısınız [BitLocker](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732774(v=ws.11)) veya [şifreleme dosya sistemi (EFS)](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc749610(v%3dws.10)) BPE ilgili dosyalar için.

Yetkili bir kullanıcı bir güvenlik yöneticisi veya bir veritabanı yöneticisi gibi veri erişebildiğinden, veritabanı saydam veri şifrelemesi ile şifrelenir olsa bile bu önerileri de izlemelidir:

- Veritabanı düzeyinde SQL Server kimlik doğrulamasını etkinleştirin.
- Kullanarak, Azure AD kullanım kimlik doğrulaması [RBAC rollerini](../role-based-access-control/overview.md).
- Kullanıcılar ve uygulamalar ayrı hesaplar kimliğini doğrulamak için kullandığınızdan emin olun. Bu şekilde kullanıcılar ve uygulamalar için verilen izinleri sınırlayabilir ve kötü amaçlı etkinlik riskini azaltır.
- Sabit veritabanı rolleri (örneğin, db_datareader veya db_datawriter) kullanarak veritabanı düzeyinde güvenlik uygulayın. Veya uygulamanız için seçilen veritabanı nesnelerini açık izinler vermek için özel roller oluşturabilirsiniz.

Verilerinizin güvenliğini sağlamak diğer yolları için göz önünde bulundurun:

- [Hücre düzeyinde şifreleme](/sql/relational-databases/security/encryption/encrypt-a-column-of-data): Belirli sütunları hatta veri hücrelerini farklı şifreleme anahtarlarıyla şifrelemenizi sağlar.
- [Her zaman şifreli](/sql/relational-databases/security/encryption/always-encrypted-database-engine), istemcilerin istemci uygulamaları içindeki hassas verileri şifrelemek ve hiçbir zaman şifreleme anahtarları (SQL veritabanı veya SQL Server) veritabanı altyapısı açığa sağlar. Sonuç olarak, her zaman şifreli verileri (ve görüntüleyebileceğini) arasında bir ayrım sağlar ve kişilere yönetenler (ama hiçbir erişimi olması gerekir).
- [Satır düzeyi güvenlik](/sql/relational-databases/security/row-level-security), sorguyu yürüten kullanıcının özelliklerine dayanan bir veritabanı tablosundaki satırlara erişimi denetlemek müşterilerin sağlar. (Örneğin özellikleri grup üyeliği ve yürütme bağlamı sahiptir.)

Veritabanı düzeyinde şifreleme kullanmayan kuruluşlarda SQL veritabanlarında bulunan verilerini tehlikeye saldırılara daha maruz kalabilir.

SQL veritabanı saydam veri şifrelemesi hakkında daha fazla makaleyi okuyarak bilgi [Azure SQL veritabanı ile saydam veri şifrelemesi](https://msdn.microsoft.com/library/0bf7e8ff-1416-4923-9c4c-49341e208c62.aspx).

## <a name="enable-database-auditing"></a>Veritabanı denetimini etkinleştirme
SQL Server veritabanı altyapısı veya tek bir veritabanı örneğini denetim olayları günlüğe kaydetme ve izleme gerektirir. SQL Server için sunucu düzeyinde olaylar için belirtimleri ve veritabanı düzeyinde olaylar için belirtimleri içeren denetimleri oluşturabilirsiniz. Denetlenen olayları, olay günlüklerini veya dosyaları denetlemek için yazılabilir.

Kamu veya standartları gereksinimleri, yükleme için bağlı olarak SQL Server için Denetim birkaç düzeyi vardır. SQL Server denetimi etkinleştirme, depolama ve çeşitli sunucu ve veritabanı nesnelerini denetimleri görüntüleme için Araçlar ve işlemler sağlar.

[Azure SQL veritabanı denetimi](../sql-database/sql-database-auditing.md) veritabanı olaylarını izler ve bir denetim günlüğüne Azure depolama hesabınızdaki yazar.

Denetim mevzuatla uyumluluk, veritabanı etkinliğini anlama ve tutarsızlıklar ve işletme sorunlarını veya güvenlik ihlallerini işaret ediyor olabilir anormallikleri bulmak olmanıza yardımcı olabilir. Denetim uyumluluk standartlarını kıldığı kolaylaştırır, ancak Uyumluluk garanti etmez.

Veritabanı denetim ve nasıl etkinleştirileceğini hakkında daha fazla bilgi için bkz: [SQL veritabanı denetimini kullanmaya başlama](../sql-database/sql-database-auditing.md).

## <a name="enable-database-threat-detection"></a>Veritabanı tehdit algılamayı etkinleştirme
Tehdit koruması algılama gider. Veritabanı tehdit koruması içerir:

- Bulma ve verilerinizi koruyabilirsiniz. böylece, en hassas verileriniz sınıflandırma.
- Veritabanınızın korumasını sağlayacak şekilde güvenli yapılandırma veritabanınızda uygulama.
- Algılama ve olası tehditleri hızlı şekilde ortaya çıktıkları yanıt vereceğini ve düzeltin.

**En iyi yöntem**: Bulma, sınıflandırmak ve veritabanlarınızı hassas verileri etiketleyin.   
**Ayrıntı**: Etkinleştirerek SQL veritabanınızdaki verileri sınıflandırmak [veri bulma ve sınıflandırma](../sql-database/sql-database-data-discovery-and-classification.md) Azure SQL veritabanı'nda. Hassas verileriniz üzerinde Azure panosuna erişim İzleyicisi veya raporları indirin.

**En iyi yöntem**: Veritabanı güvenlik proaktif olarak azaltabilmesi veritabanı güvenlik açıklarını izleyin.   
**Ayrıntı**: Azure SQL veritabanı [güvenlik açığı değerlendirmesi](../sql-database/sql-vulnerability-assessment.md) hizmeti, olası veritabanı güvenlik açıklarını tarar. Hizmet, güvenlik açıklarını bayrak ve yanlış yapılandırmalar, aşırı izinleri ve korumasız hassas veriler gibi iyi uygulamalardan sapmaları Göster kurallarından oluşan bir Bilgi Bankası kullanır.

Microsoft en iyi yanı sıra, veritabanınızı ve değerli verileri büyük riskleri sunan güvenlik sorunlarını kuralları dayanır. Bunlar, hem veritabanı düzeyinde bir sorun hem de sunucu güvenlik duvarı ayarları ve sunucu düzeyi izinleri gibi sunucu düzeyi güvenlik sorunları kapsar. Bu kurallar, uyumluluk standartlarını karşılamak için yasal kurumların gereksinimlerinden çoğunu gösterir.

**En iyi yöntem**: Tehdit algılamayı etkinleştirme.  
**Ayrıntı**:  Azure SQL veritabanı'nı etkinleştirme [tehdit algılama](../sql-database/sql-database-threat-detection.md) araştırmak ve tehditleri nasıl güvenlik uyarıları ve öneriler almak için. Uyarılar hakkında şüpheli veritabanı etkinlikleri, potansiyel açıklar ve SQL ekleme saldırılarına karşı yanı sıra anormal veritabanı erişim ve sorgu desenleri olursunuz.

[Gelişmiş tehdit koruması](../sql-database/sql-advanced-threat-protection.md) Gelişmiş SQL güvenlik özellikleri için birleştirilmiş bir pakettir. Daha önce bahsedilen hizmetleri içerir: Veri bulma ve Sınıflandırma, güvenlik açığı değerlendirmesi ve tehdit algılama. Etkinleştirme ve bu özellikleri yönetmek için tek bir konum sağlar.

Bu özelliklerin yardımcı olur:

- Veri gizliliği standartlarını ve yasal uyumluluk gereksinimlerini karşılar.
- Veritabanlarınızı erişimi denetlemek ve sağlamlaştırmak güvenliklerini.
- Değişiklikleri izlemek zor olduğu bir dinamik veritabanı ortam izleyin.
- Algılama ve olası tehditlere yanıt verin.

Ayrıca, tehdit algılama, uyarıları Azure Güvenlik Merkezi ile tüm Azure kaynaklarınızın güvenlik durumunu genel bir görünüm için tümleştirir.

## <a name="next-steps"></a>Sonraki adımlar
Bkz: [Azure güvenlik en iyi uygulamaları ve desenleri](security-best-practices-and-patterns.md) kullanmak üzere daha fazla güvenlik için en iyi yöntemler, tasarlama, dağıtma ve Azure'ı kullanarak bulut çözümlerinizi yönetme.

Aşağıdaki kaynaklar, Azure güvenliği ve ilgili Microsoft Hizmetleri hakkında daha fazla genel bilgi sağlamak kullanılabilir:
* [Azure güvenlik ekibi blogu](https://blogs.msdn.microsoft.com/azuresecurity/) - Azure güvenliği en güncel bilgi için
* [Microsoft Security Response Center](https://technet.microsoft.com/library/dn440717.aspx) - Azure ile ilgili sorunlar dahil Microsoft güvenlik açıklarının bildirilebileceği veya e-posta aracılığıyla secure@microsoft.com
