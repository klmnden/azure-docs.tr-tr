---
title: "En iyi güvenlik uygulamaları, Azure veritabanı | Microsoft Docs"
description: "Bu makalede Azure veritabanı güvenliği için en iyi yöntemler kümesi sağlar."
services: security
documentationcenter: na
author: unifycloud
manager: swadhwa
editor: tomsh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: tomsh
ms.openlocfilehash: b3a9befe5c6607c108e11b583f8b67c483710021
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2017
---
# <a name="azure-database-security-best-practices"></a>En iyi güvenlik uygulamaları, Azure veritabanı

Veritabanları yönetirken güvenlik güvenliğin çok önemli olduğu ve her zaman Azure SQL veritabanı için bir öncelik olmuştur. Veritabanlarınızı sıkı bir şekilde en yasal karşılamak amacıyla güvenli hale getirilebilir veya HIPAA, ISO 27001/27002 ve PCI DSS düzey 1, diğerleri de dahil olmak üzere, güvenlik gereksinimleri. Geçerli güvenlik uyumluluk sertifikalar listesini kullanılabilir [Microsoft Trust Center site](http://azure.microsoft.com/support/trust-center/services/). Yasal düzenleme gereksinimlerine bağlı olarak belirli Azure veri merkezleri veritabanlarınızı yerleştirmek seçebilirsiniz.

Bu makalede, en iyi güvenlik uygulamaları, Azure veritabanı koleksiyonunu aşağıdakiler ele alınacaktır. Bu en iyi uygulamaları bizim deneyimlerden Azure veritabanı güvenliği ile elde edilen ve müşteri deneyimleri bulunun.

En iyi her uygulama için biz açıklamaktadır:

-   En iyi uygulama nedir
-   Bu en iyi uygulama etkinleştirmek istediğiniz neden
-   En iyi uygulama olarak etkinleştirmek başarısız olursa ne sonucu olabilir
-   Nasıl en iyi uygulama olarak etkinleştirmek bilgi edinebilirsiniz

Bu Azure veritabanı güvenlik en iyi yöntemler makalesi anlaşma fikir ve Azure platformu özellikleri dayalıdır ve bu makalenin yazıldığı sırada oldukları gibi özelliğini ayarlar. Zaman içinde görüşlerini ve teknolojileri değiştirebilirsiniz ve bu makalede bu değişiklikleri yansıtacak şekilde düzenli olarak güncelleştirilir.

Bu makalede ele alınan azure veritabanı en iyi yöntemler şunlardır:

-   Veritabanı erişimi kısıtlamak için güvenlik duvarı kurallarını kullanın
-   Veritabanı kimlik doğrulamasını etkinleştir
-   Şifreleme kullanarak verilerinizi koruma
-   Aktarımdaki verileri korumak
-   Veritabanı denetimi etkinleştir
-   Veritabanı tehdit algılama etkinleştir


## <a name="use-firewall-rules-to-restrict-database-access"></a>Veritabanı erişimi kısıtlamak için güvenlik duvarı kurallarını kullanın

Microsoft Azure SQL Veritabanı, Azure ile diğer İnternet tabanlı uygulamalar için ilişkisel veritabanı hizmeti sunar. Erişim güvenliğini sağlamak için SQL veritabanı bağlantısı IP adresi, kimliğini kanıtlamak kullanıcıların kimlik doğrulama mekanizmaları ve yetkilendirme mekanizmaları belirli eylemleri ve veri kullanıcılarına sınırlama tarafından sınırlayarak güvenlik duvarı kuralları ile erişimi denetler. Hangi bilgisayarların izniniz belirtene kadar güvenlik duvarları, veritabanı sunucunuza tüm erişimi engelliyor. Güvenlik duvarı, her bir isteğin kaynak IP adresine göre veritabanlarına erişim verir.

![Güvenlik duvarı kuralları](./media/azure-database-security-best-practices/azure-database-security-best-practices-Fig1.png)

Azure SQL Veritabanı hizmeti yalnızca 1433 numaralı TCP bağlantı noktasından kullanılabilir. Bilgisayarınızdan bir SQL Veritabanına erişmek için istemci bilgisayarınızdaki güvenlik duvarının 1433 numaralı TCP bağlantı noktasından giden TCP iletişimine izin verdiğinden emin olun. Diğer uygulamalar için gerekli değildir, TCP bağlantı noktası 1433 gelen bağlantıları engelle güvenlik duvarı kurallarını kullanma.

Bağlantı işleminin bir parçası olarak, Azure sanal makinelerinden gelen bağlantılar her bir çalışan rolü için farklı olan bir IP adresine ve bağlantı noktasına yönlendirilir. Bağlantı noktası numarası 11000 ile 11999 arasındadır. TCP bağlantı noktaları hakkında daha fazla bilgi için bkz: [1433 ADO.NET 4.5 ve SQL Database2 dışındaki bağlantı noktaları](https://docs.microsoft.com/azure/sql-database/sql-database-develop-direct-route-ports-adonet-v12).

> [!Note]
> SQL Veritabanındaki güvenlik duvarı kuralları hakkında daha fazla bilgi için bkz. [SQL Veritabanı güvenlik duvarı kuralları](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure).

## <a name="enable-database-authentication"></a>Veritabanı kimlik doğrulamasını etkinleştir
SQL veritabanı iki tür kimlik doğrulaması, SQL kimlik doğrulaması ve Azure Active Directory kimlik doğrulaması (Azure AD kimlik doğrulaması) destekler.

**SQL kimlik doğrulaması** aşağıdaki durumlarda önerilir:

-   Karma işletim sistemleriyle ortamlarını desteklemek SQL Azure burada tüm kullanıcılar tarafından bir Windows etki alanı kimlik doğrulaması yapılmıyor sağlar.
-   SQL Azure'nın daha eski uygulamaları ve SQL Server kimlik doğrulaması gerektiren üçüncü taraflarca sağlanan uygulamaları desteği sağlar.
-   Bilinmeyen veya güvenilmeyen etki alanlarından bağlanmasına olanak sağlar. Örneği için kurulmuş müşteriler ile eriştikleri bir uygulama, emirleri durumunu almak için SQL Server oturumları atanır.
-   SQL Azure'nın Web tabanlı uygulamaları desteklemek, burada kullanıcılar kendi kimlikleri oluşturmak sağlar.
-   Karmaşık izni hiyerarşi bilinen, önceden ayarlanmış SQL Server oturum tabanlı kullanarak uygulamalarını dağıtmak yazılım geliştiricileri sağlar.

> [!Note]
> Ancak, SQL Server kimlik doğrulaması, Kerberos güvenlik protokolünü kullanamazsınız.

Kullanırsanız **SQL kimlik doğrulaması** yapmanız gerekir:

-   Güçlü kimlik bilgileri kendiniz yönetin.
-   Kimlik bilgileri bağlantı dizesinde koruyun.
-   (Büyük olasılıkla) ağ üzerinden Web sunucusundan veritabanına geçirilen kimlik bilgileri koruyun. Daha fazla bilgi için bkz: [nasıl yapılır: SQL Server kullanarak SQL kimlik doğrulaması, ASP.NET 2.0 bağlanmak](https://msdn.microsoft.com/library/ms998300.aspx).

**Azure Active Directory kimlik doğrulaması** Microsoft Azure SQL veritabanına bağlayan bir mekanizmadır ve [SQL Data Warehouse](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-overview-what-is) kimlikleri Azure Active Directory (Azure AD) kullanarak. Azure AD kimlik doğrulaması ile veritabanı kullanıcıları kimlikleri ve diğer Microsoft Hizmetleri tek bir merkezi konumda merkezi olarak yönetebilir. Merkezi kimlik yönetimi, veritabanı kullanıcıları yönetmek için tek bir yer sağlar ve izin Yönetimi basitleştirir. Avantajları şunlardır:

-   SQL Server kimlik doğrulaması için bir alternatif sunar.
-   Yardımcı veritabanı sunucuları arasında kullanıcı kimlikleri artışı durdurun.
-   Tek bir yerde parola döndürme sağlar.
-   Müşteriler, dış (AAD) gruplarını kullanarak veritabanı izinlerini yönetebilir.
-   Tümleşik Windows kimlik doğrulaması ve Azure Active Directory tarafından desteklenen kimlik doğrulama başka biçimlerde etkinleştirerek bunu depolanmasını parolaları ortadan kaldırabilirsiniz.
-   Azure AD kimlik doğrulaması kapsanan veritabanı kullanıcıları veritabanı düzeyinde kimlikleri doğrulamak için kullanır.
-   Azure AD, SQL veritabanına bağlanma uygulamalar için belirteç tabanlı kimlik doğrulamasını destekler.
-   Azure AD kimlik doğrulaması, bir yerel Azure Active Directory etki alanı eşitleme olmadan için ADFS (etki alanı Federasyonu) veya yerel kullanıcı/parola kimlik doğrulamasını destekler.
-   Azure AD, Active Directory Evrensel çok faktörlü kimlik doğrulama (MFA) içeren kimlik doğrulaması kullanan SQL Server Management Studio bağlantılarını destekler. MFA kolay doğrulama seçeneklerini aralıklı güçlü kimlik doğrulaması içerir — telefon araması, SMS mesajı, akıllı kartlar ve PIN ya da mobil uygulama bildirimi. Daha fazla bilgi için bkz: [SSMS desteklemek için SQL Database ve SQL Data Warehouse ile Azure AD MFA](https://docs.microsoft.com/azure/sql-database/sql-database-ssms-mfa-authentication).

Yapılandırma adımları yapılandırmak ve Azure Active Directory kimlik doğrulaması kullanmak için aşağıdaki yordamları içerir.

-   Oluşturma ve Azure AD doldurabilirsiniz.
-   İsteğe bağlı: İlişkilendirmek veya şu anda Azure aboneliğinizle ilişkili olan active directory değiştirin.
-   Azure SQL server için Azure Active Directory Yöneticisi oluşturmak veya [Azure SQL Data Warehouse](https://azure.microsoft.com/services/sql-data-warehouse/).
- İstemci bilgisayarları yapılandırın.
-   Azure AD kimlikleri eşlenen veritabanınızdaki kapsanan veritabanı kullanıcıları oluşturun.
-   Veritabanınız için Azure AD kimlikleri kullanarak bağlanın.

Ayrıntılı bilgileri bulabilirsiniz [burada](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication).

## <a name="protect-your-data-using-encryption"></a>Şifreleme kullanarak verilerinizi koruma

[Azure SQL veritabanında saydam veri şifreleme (TDE)](https://msdn.microsoft.com/library/dn948096.aspx) gerçek zamanlı şifreleme ve şifre çözme veritabanının, ilişkili yedeklemelerinizi gerçekleştirerek kötü amaçlı etkinliği tehdit karşı korumaya yardımcı olur ve işlem günlüğü dosyalarını REST uygulamasında yapılacak değişiklikler gerektirir. TDE, veritabanının tamamını Depolama veritabanı şifreleme anahtarını adlı bir simetrik anahtar kullanarak şifreler.

Tüm depolama bile şifreli olduğunda, aynı zamanda, veritabanınızı şifrelemek çok önemlidir. Bu veri koruması derinliği yaklaşımda savunma uygulamasıdır. Azure SQL veritabanı kullanıyorsanız ve kredi kartı veya sosyal güvenlik numarası gibi hassas verileri korumak istiyorsanız, çoğu endüstri standartları (örn., HIPAA, PCI) gereksinimlerini karşılayan FIPS 140-2 doğrulanmış 256 bit AES şifreleme veritabanlarıyla şifreleyebilirsiniz .

Dosyaları ile ilgili anlaşılması önemlidir [arabellek havuzu genişletme (BPE)](https://docs.microsoft.com/sql/database-engine/configure-windows/buffer-pool-extension) TDE kullanarak bir veritabanı şifreli olduğunda şifrelenmez. Dosya sistemi düzeyinde şifreleme araçları gibi kullanmalısınız [BitLocker](https://technet.microsoft.com/library/cc732774) veya [şifreleme dosya sistemi (EFS)](https://technet.microsoft.com/library/cc700811.aspx) için BPE ilgili dosyaları.

Yetkili bir kullanıcı bu yana veritabanı ile TDE, şifreli olsa bile bir güvenlik yöneticisi veya bir veritabanı yöneticisi verilere erişebilirsiniz gibi aşağıdaki önerileri de izlemelisiniz:

-   Veritabanı düzeyinde SQL kimlik doğrulamasını etkinleştirin.
-   Azure AD kimlik doğrulama kullanarak [RBAC rolleri](https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is).
-   Kullanıcılar ve uygulamalar ayrı hesaplarının kimliğini doğrulamak için kullanmanız gerekir. Bu şekilde kullanıcılar ve uygulamalar için izinler sınırlayabilir ve kötü amaçlı etkinliği riskleri azaltın.
-   Uygulama veritabanı düzeyi güvenlik (örneğin, db_datareader veya db_datawriter) sabit veritabanı rollerinin veya kullanarak, seçili veritabanı nesnelerini açık izinleri vermek, uygulamanız için özel roller oluşturabilirsiniz.

Verilerinizi şifrelemek için kullanabileceğiniz diğer yöntemler şunlardır:

-   [Hücre düzeyinde şifreleme](https://msdn.microsoft.com/library/ms179331.aspx): Belirli sütunları hatta veri hücrelerini farklı şifreleme anahtarlarıyla şifrelemenizi sağlar.
-   Şifreleme her zaman şifreli kullanarak kullanılıyor: [her zaman şifreli](https://msdn.microsoft.com/library/mt163865.aspx) istemcilerin istemci uygulamalar içinde hassas verileri şifrelemek ve hiçbir zaman veritabanı altyapısı (SQL veritabanı veya SQL Server) için şifreleme anahtarları açığa olanak sağlar. Sonuç olarak, her zaman şifreli verileri kendi (ve görüntüleyebileceğini) olanlar arasında ayrım sağlar ve kişilere verileri yönetme (ancak hiçbir erişimi olması gerekir).
-   Satır düzeyi güvenlik kullanarak: satır düzeyi güvenlik (örneğin, grubu üyeliği veya yürütme bağlamı) sorgu yürütülürken kullanıcı özellikler temelinde bir veritabanı tablosundaki satırları erişimi denetlemek müşteriler olanak tanır. Daha fazla bilgi için bkz. [Satır düzeyi güvenlik](https://msdn.microsoft.com/library/dn765131).

## <a name="protect-data-in-transit"></a>Aktarımdaki verileri korumak
Aktarımdaki verileri koruma temel veri koruma stratejinizin parçası olmalıdır. Verileri geri ve İleri birçok konumlardan taşıma olduğundan, genel, her zaman SSL/TLS protokolleri farklı konumlar arasında veri değişimi için kullanmanız önerilir. Bazı durumlarda, şirket içi ve bulut arasındaki tüm iletişim kanalını ayırmak isteyebilirsiniz sanal özel ağ (VPN) kullanarak altyapı.

Şirket içi altyapınızı ve Azure arasında taşıma verileri için HTTPS veya VPN gibi uygun güvenlik önlemleri göz önünde bulundurmalısınız.

Azure birden çok iş istasyonlarında bulunan şirket içi erişimi güvenli hale getirmek için gereken kuruluşlar için kullanmak [Azure siteden siteye VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-site-to-site-create).

Güvenli hale getirmek için gereken kuruluşlar bireysel iş istasyonlarında bulunan erişimden şirket içi veya şirket dışı Azure'a kullanmayı düşünebilirsiniz [noktası siteye VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-point-to-site-create).

Büyük veri kümeleri taşınabilir ayrılmış bir yüksek hızlı WAN bağlantısı üzerinden gibi [ExpressRoute](https://azure.microsoft.com/services/expressroute/). ExpressRoute kullanmayı seçerseniz, ayrıca uygulama düzeyi kullanarak verileri şifreleyebilir [SSL/TLS](https://support.microsoft.com/kb/257591) veya diğer protokoller için ek koruma.

Azure Portalı aracılığıyla Azure Storage ile etkileşim, tüm işlemleri HTTPS oluşur. [Storage REST API'sini](https://msdn.microsoft.com/library/azure/dd179355.aspx) HTTPS de kullanılabilir ile etkileşim kurmasına üzerinden [Azure Storage](https://azure.microsoft.com/services/storage/) ve [Azure SQL veritabanı](https://azure.microsoft.com/services/sql-database/).

Aktarımdaki verileri korumak için başarısız olan kuruluşlar için daha açıktır [man-in--middle saldırıları](https://technet.microsoft.com/library/gg195821.aspx), [gizli dinleme](https://technet.microsoft.com/library/gg195641.aspx) ve oturumu ele geçirme. Bu tür saldırıları gizli verilere erişmesini ilk adımı olabilir.

Makaleyi okuyarak Azure VPN seçenek hakkında daha fazla bilgi edinmek için [planlama ve tasarım VPN ağ geçidi için](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-plan-design).

## <a name="enable-database-auditing"></a>Veritabanı denetimi etkinleştir
SQL Server veritabanı altyapısı veya tek bir veritabanının bir örneğini denetim, izleme ve veritabanı Motoru'nu meydana gelen olayları günlüğe kaydetme içerir. SQL Server audit, sunucu düzeyi olayları için sunucunun denetim özellikleri ve veritabanı düzeyi olayları için veritabanı denetim belirtimleri içerebilir sunucu denetimleri oluşturmanızı sağlar. Denetlenen olayları, olay günlükleri veya dosyaları denetlemek için yazılabilir.

Kamu veya yüklemenizi standartları gereksinimleri bağlı olarak SQL Server için denetleme birkaç düzeyi vardır. SQL Server Audit işlemleri etkinleştirmek, depolamak ve denetimleri üzerinde çeşitli sunucu ve veritabanı nesnelerini görüntülemek için olmalıdır ve araçları sağlar.

[Azure SQL veritabanı denetimi](https://docs.microsoft.com/azure/sql-database/sql-database-auditing) parçaları veritabanı olayları ve Azure depolama hesabınızdaki bunları Denetim günlüğüne yazar.

Denetim mevzuatla uyumluluk, veritabanı etkinliğini anlama ve işletme sorunlarını veya şüpheli güvenlik ihlallerini işaret edebilecek farklılıklar ve anormal durumlar hakkında öngörü sahip olmanıza yardımcı olabilir.

Denetim sağlar ve uyumluluk standartlara uyulması kolaylaştırır, ancak Uyumluluk garanti etmez.

Veritabanı denetim ve bunun nasıl etkinleştirileceğini hakkında daha fazla bilgi için lütfen makaleyi okuyun [etkinleştirmek denetim ve tehdit algılama Azure Güvenlik Merkezi'nde SQL sunucularında](https://docs.microsoft.com/azure/security-center/security-center-enable-auditing-on-sql-servers).

## <a name="enable-database-threat-detection"></a>Veritabanı tehdit algılama etkinleştir
SQL tehdit algılama, algılamak ve anormal etkinlikler güvenlik uyarıları sağlayarak göründüklerinde olası risklere yanıt sağlar. Şüpheli veritabanı etkinliklerini, olası güvenlik açıkları ve SQL ekleme saldırıları yanı sıra anormal veritabanı erişimi desenleri bağlı bir uyarı alırsınız. SQL tehdit algılama uyarıları şüpheli etkinlik ayrıntılarını sağlayın ve eylem araştırmak ve tehdidi azaltmak nasıl öneririz.

Örneğin, SQL ekleme veri güdümlü uygulamaları saldırmak için kullanılıyorsa Internet üzerinde ortak Web uygulaması güvenlik sorunlarını biridir. Saldırganlar uygulama giriş alanları, kötü amaçlı SQL deyimleri eklemesine uygulama güvenlik açıkları ihlal veya veritabanındaki verileri değiştirme yararlanın.

Tehdit algılama için Azure portal bakın, veritabanınızdaki ayarlama konusunda bilgi edinmek için [SQL veritabanı tehdit algılama](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection).

Ayrıca, SQL tehdit algılama uyarıları ile tümleşir [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/). Sizi bu hizmeti 60 gün süresince ücretsiz denemeye davet ediyoruz.

Veritabanı tehdit algılama ve etkinleştirmek hakkında daha fazla bilgi için lütfen makaleyi okuyun [etkinleştirmek denetim ve tehdit algılama Azure Güvenlik Merkezi'nde SQL sunucularında](https://docs.microsoft.com/azure/security-center/security-center-enable-auditing-on-sql-servers).

## <a name="conclusion"></a>Sonuç
Azure veritabanı birçok kuruluş ve yasal uyumluluk gereksinimlerinin karşılanmasına güvenlik özelliklerinin tam aralıklı bir sağlam veritabanı platformudur. Verilerinizi fiziksel erişimi denetleme ve dosya, sütun veya satır düzeyinde saydam veri şifreleme, hücre düzeyi şifreleme veya satır düzeyi güvenlik, veri güvenliği için çeşitli seçenekler kullanarak verileri korumaya yardımcı olabilir. Her zaman şifreli ayrıca işlemlerini şifrelenmiş veriler karşı uygulama güncelleştirmeleri işlemini basitleştirme sağlar. Buna karşılık, SQL veritabanı etkinliği günlükleri denetim erişimi verileri nasıl ve ne zaman eriştiği bilmeleri sağlayarak gereken bilgileri sağlar.

## <a name="next-steps"></a>Sonraki adımlar
- Güvenlik duvarı kuralları hakkında daha fazla bilgi için bkz: [güvenlik duvarı kuralları](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure).
- Kullanıcılar ve oturum açma bilgileri hakkında bilgi almak için bkz. [Oturum açma bilgilerini yönetme](https://docs.microsoft.com/azure/sql-database/sql-database-manage-logins).
- Bir öğretici için bkz: [Azure SQL veritabanınıza güvenli](https://docs.microsoft.com/azure/sql-database/sql-database-security-tutorial).
