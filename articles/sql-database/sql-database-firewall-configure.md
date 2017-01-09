---
title: "SQL Veritabanı güvenlik duvarı kurallarına genel bakış | Microsoft Docs"
description: "Erişimi yönetmek amacıyla, sunucu düzeyinde ve veritabanı düzeyinde güvenlik kuralları kullanarak SQL veritabanı güvenlik duvarını yapılandırma hakkında bilgi edinin."
keywords: "veritabanı güvenlik duvarı"
services: sql-database
documentationcenter: 
author: BYHAM
manager: jhubbard
editor: cgronlun
tags: 
ms.assetid: ac57f84c-35c3-4975-9903-241c8059011e
ms.service: sql-database
ms.custom: authentication and authorization
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 11/23/2016
ms.author: rickbyh;carlrab
translationtype: Human Translation
ms.sourcegitcommit: e5b5751facb68ae4a62e3071fe4dfefc02434a9f
ms.openlocfilehash: ae1cacf0ff003e69a16d6beac48abc36a7f18896


---
# <a name="overview-of-azure-sql-database-firewall-rules"></a>Azure SQL Veritabanı güvenlik duvarı kurallarına genel bakış 
> [!div class="op_single_selector"]
> * [Genel Bakış](sql-database-firewall-configure.md)
> * [Azure Portal](sql-database-configure-firewall-settings.md)
> * [TSQL](sql-database-configure-firewall-settings-tsql.md)
> * [PowerShell](sql-database-configure-firewall-settings-powershell.md)
> * [REST API](sql-database-configure-firewall-settings-rest.md)
> 
> 

Microsoft Azure SQL Veritabanı, Azure ile diğer İnternet tabanlı uygulamalar için ilişkisel veritabanı hizmeti sunar. Güvenlik duvarları, verilerinizin korunmasına yardımcı olmak üzere, hangi bilgisayarların izinli olduğunu belirtmenize kadar veritabanı sunucunuza tüm erişimi engeller. Güvenlik duvarı, her bir isteğin kaynak IP adresine göre veritabanlarına erişim verir.

Güvenlik duvarınızı yapılandırmak için kabul edilebilir IP adreslerinin aralıklarını belirten güvenlik duvarı kuralları oluşturun. Sunucu ve veritabanı düzeylerinde güvenlik duvarı kuralları oluşturabilirsiniz.

* **Sunucu düzeyinde güvenlik duvarı kuralları:** Bu kurallar istemcilerin tüm Azure SQL sunucusuna, yani aynı mantıksal sunucu içindeki tüm veritabanlarına erişmesini sağlar. Bu kurallar **ana** veritabanına depolanır. Sunucu düzeyinde güvenlik duvarı kuralları, portal ya da Transact-SQL deyimleri kullanılarak yapılandırılabilir. Azure portalı veya PowerShell kullanarak sunucu düzeyinde güvenlik duvarı kuralları oluşturmak için abonelik sahibi veya abonelik katkıda bulunanı olmanız gerekir. Transact-SQL kullanarak sunucu düzeyinde güvenlik duvarı kuralı oluşturmak için SQL Veritabanı örneğine sunucu düzeyi asıl oturum açma bilgileriyle veya Azure Active Directory yöneticisi olarak bağlanmanız gerekir (başka bir deyişle, sunucu düzeyi güvenlik duvarı kuralının önce Azure düzeyi izinlere sahip bir kullanıcı tarafından oluşturulması gerekir).
* **Veritabanı düzeyinde güvenlik kuralı duvarları:** Bu kurallar istemcilerin Azure SQL Veritabanı sunucunuzdaki tek veritabanlarına erişmesini sağlar. Her bir veritabanı için oluşturabileceğiniz bu kurallar, tek veritabanlarına depolanır. (**Ana** veritabanı için veritabanı düzeyinde güvenlik duvarı kuralları oluşturabilirsiniz.) Bu kurallar aynı mantıksal sunucu içindeki bazı (güvenli) veritabanlarına erişimi kısıtlamada yararlı olabilir. Veritabanı düzeyinde güvenlik duvarı kuralları yalnızca Transact-SQL deyimleri kullanılarak yapılandırılabilir.

**Öneri:** Microsoft, güvenliği artırmak ve veritabanınızı daha taşınabilir hale getirmek açısından, mümkün olan durumlarda veritabanı düzeyinde güvenlik duvarı kurallarının kullanılmasını önerir. Aynı erişim gereksinimlerine sahip birçok veritabanınız varsa ve her veritabanını ayrı ayrı yapılandırmaya zaman harcamak istemiyorsanız sunucu düzeyinde güvenlik duvarı kurallarını yöneticiler için kullanabilirsiniz.

> [!Note]
> İş sürekliliği bağlamında taşınabilir veritabanları hakkında bilgi edinmek için bkz. [Olağanüstü durum kurtarma için kimlik doğrulama gereksinimleri](sql-database-geo-replication-security-config.md).
>

## <a name="firewall-overview"></a>Güvenlik duvarına genel bakış
Başlangıçta, Azure SQL sunucunuza tüm Transact-SQL erişimleri güvenlik duvarı tarafından engellenir. Azure SQL sunucunuzu kullanmaya başlamak için, Azure portalına gitmeniz ve Azure SQL sunucunuza erişiminizi etkinleştirecek olan bir ya da daha fazla sunucu düzeyinde güvenlik duvarı kuralını belirlemeniz gerekir. İnternet’ten gelen hangi IP adresi aralıklarının izinli olduğuna ve Azure uygulamalarının Azure SQL sunucunuza bağlanmaya çalışıp çalışamayacaklarına ilişkin güvenlik duvarı kurallarını belirleyin.

Azure SQL sunucunuzdaki veritabanlarından yalnızca birine seçmeli olarak erişim vermek için, gerekli veritabanına yönelik bir veritabanı düzeyinde kural oluşturmanız gerekir. Veritabanı güvenlik duvarı kuralı için, sunucu düzeyinde güvenlik duvarı kuralında belirtilen IP adres aralığının dışında olan bir IP adres aralığı belirtin ve istemci IP adresinin veritabanı düzeyindeki kuralda belirtilen aralığa denk geldiğinden emin olun.

İnternet’ten ve Azure’dan gelen bağlantı denemeleri, Azure SQL sunucunuza veya SQL Veritabanına ulaşmadan önce aşağıdaki diyagramda gösterildiği gibi ilk olarak güvenlik duvarından geçmelidir:

   ![Güvenlik duvarı yapılandırmasını açıklayan diyagram.][1]

## <a name="connecting-from-the-internet"></a>İnternet'ten bağlanma
Bir bilgisayar İnternet'ten, veritabanı sunucunuza bağlanmaya çalıştığında, güvenlik duvarı ilk olarak isteğin kaynak IP adresini güvenlik duvarı kurallarına karşı denetler:

* İstek IP adresi sunucu düzeyinde güvenlik duvarı kurallarında belirtilen aralıklardan biri içindeyse, Azure SQL Veritabanı sunucunuza erişim izni verilir.
* İsteğin IP adresi sunucu düzeyinde güvenlik duvarı kuralında belirtilen aralıklardan biri içinde değilse, veritabanı tabanı düzeyinde güvenlik duvarı kuralları denetlenir. İsteğin IP adresi veritabanı düzeyinde güvenlik duvarı kurallarında belirtilen aralıklardan biri içindeyse, bağlantı yalnızca eşleşen veritabanı düzeyinde kurala sahip veritabanına verilir.
* İsteğin IP adresi sunucu düzeyinde veya veritabanı düzeyinde kuralların herhangi birinde belirtilen aralıklar dahilinde değilse, bağlantı isteği başarısız olur.

> [!NOTE]
> Azure SQL Veritabanına yerel bilgisayarınızdan erişmek için, ağ ve yerel bilgisayarınız üzerindeki güvenlik duvarının TCP bağlantı noktası 1433 üzerinde giden iletişime izin verdiğinden emin olun.
> 

## <a name="connecting-from-azure"></a>Azure'dan bağlanma
Azure’daki uygulamaların Azure SQL sunucunuza bağlanmasına izin verebilmeniz için Azure bağlantılarının etkinleştirilmesi gerekir. Azure’dan bir uygulama, veritabanı sunucunuza bağlanmayı denediğinizde güvenlik duvarı Azure bağlantılarına izin verildiğini doğrular. Başlangıç ve bitiş adresi 0.0.0.0’a eşit olan bir güvenlik duvarı ayarı, bu bağlantılara izin verildiğini gösterir. Bağlantı denemesine izin verilmezse, istek Azure SQL Veritabanı sunucusuna ulaşmaz.

> [!IMPORTANT]
> Bu seçenek, diğer müşterilerin aboneliklerinden gelen bağlantılar dahil Azure’dan tüm bağlantılara izin verecek şekilde güvenlik duvarınızı yapılandırır. Bu seçeneği belirlerken, oturum açma ve kullanıcı izinlerinizin erişimi yalnızca yetkili kullanıcılarla sınırladığından emin olun.
> 

> [!NOTE]
>  Daha fazla bilgi edinmek için [ADO.NET 4.5 ve SQL Veritabanı için 1433’ten sonraki bağlantı noktaları](sql-database-develop-direct-route-ports-adonet-v12.md) konusunun **SQL Veritabanı: Dış ve iç karşılaştırması** bölümüne bakın
>  

## <a name="creating-the-first-server-level-firewall-rule"></a>Sunucu düzeyinde ilk güvenlik duvarı kuralını oluşturma
Sunucu düzeyinde ilk güvenlik duvarı kuralı, [Azure portalı](https://portal.azure.com/) kullanılarak veya REST API ya da Azure PowerShell programlı bir şekilde kullanılarak oluşturulabilir. Sunucu düzeyinde sonraki güvenlik duvarı kuralları ise bu yöntemler kullanılarak ve Transact-SQL aracılığıyla oluşturulup yönetilebilir. Performansı artırmak için sunucu düzeyinde güvenlik duvarı kuralları veritabanı düzeyinde geçici olarak önbelleğe alınır. Önbelleği yenilemek için bkz. [DBCC FLUSHAUTHCACHE](https://msdn.microsoft.com/library/mt627793.aspx). Sunucu düzeyinde güvenlik duvarı kuralları hakkında daha fazla bilgi için bkz. [Nasıl yapılır: Azure portalını kullanarak Azure SQL sunucusu güvenlik duvarı kuralı yapılandırma](sql-database-configure-firewall-settings.md).

## <a name="creating-database-level-firewall-rules"></a>Veritabanı düzeyinde güvenlik duvarı kuralları oluşturma
Sunucu düzeyinde ilk güvenlik duvarınızı yapılandırdıktan sonra erişimi bazı veritabanlarıyla kısıtlamak isteyebilirsiniz. Veritabanı düzeyinde güvenlik kuralı için, sunucu düzeyinde güvenlik duvarı kuralında belirtilen aralığın dışındaki bir IP adresi aralığını belirtirseniz, yalnızca veritabanı düzeyi aralığındaki IP adreslerine sahip istemciler veritabanına erişebilir. Bir veritabanı için en fazla 128 veritabanı düzeyinde güvenlik duvarı kuralınız olabilir. Ana ve kullanıcı veritabanları için veritabanı düzeyinde güvenlik duvarı kuralları, Transact-SQL aracılığıyla oluşturulup yönetilebilir. Veritabanı düzeyinde güvenlik duvarı kurallarını yapılandırma hakkında daha fazla bilgi için bkz. [sp_set_database_firewall_rule (Azure SQL Veritabanları)](https://msdn.microsoft.com/library/dn270010.aspx).

## <a name="programmatically-managing-firewall-rules"></a>Güvenlik duvarı kurallarını programlı bir şekilde yönetme
Güvenlik duvarı kuralları, Azure portalına ek olarak Transact-SQL, REST API ve Azure PowerShell ile programlı bir şekilde yönetilebilir. Aşağıdaki tablolarda her yöntem için kullanılabilen komut kümesi açıklanmaktadır.

### <a name="transact-sql"></a>Transact-SQL
| Katalog Görünümü veya Saklı Yordam | Düzey | Açıklama |
| --- | --- | --- |
| [sys.firewall_rules](https://msdn.microsoft.com/library/dn269980.aspx) |Sunucu |Sunucu düzeyinde geçerli güvenlik duvarı kurallarını gösterir |
| [sp_set_firewall_rule](https://msdn.microsoft.com/library/dn270017.aspx) |Sunucu |Sunucu düzeyinde güvenlik duvarı kuralları oluşturur veya güncelleştirir |
| [sp_delete_firewall_rule](https://msdn.microsoft.com/library/dn270024.aspx) |Sunucu |Sunucu düzeyinde güvenlik duvarı kurallarını kaldırır |
| [sys.database_firewall_rules](https://msdn.microsoft.com/library/dn269982.aspx) |Database |Veritabanı düzeyinde geçerli güvenlik duvarı kurallarını gösterir |
| [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx) |Database |Veritabanı düzeyinde güvenlik duvarı kuralları oluşturur veya güncelleştirir |
| [sp_delete_database_firewall_rule](https://msdn.microsoft.com/library/dn270030.aspx) |Veritabanları |Veritabanı düzeyinde güvenlik duvarı kurallarını kaldırır |

### <a name="rest-api"></a>REST API
| API | Düzey | Açıklama |
| --- | --- | --- |
| [Güvenlik Duvarı Kurallarını Listele](https://msdn.microsoft.com/library/azure/dn505715.aspx) |Sunucu |Sunucu düzeyinde geçerli güvenlik duvarı kurallarını gösterir |
| [Güvenlik Duvarı Kuralı Oluştur](https://msdn.microsoft.com/library/azure/dn505712.aspx) |Sunucu |Sunucu düzeyinde güvenlik duvarı kuralları oluşturur veya güncelleştirir |
| [Güvenlik Duvarı Kuralı Ayarla](https://msdn.microsoft.com/library/azure/dn505707.aspx) |Sunucu |Sunucu düzeyinde mevcut güvenlik duvarı kuralının özelliklerini güncelleştirir |
| [Güvenlik Duvarı Kuralını Sil](https://msdn.microsoft.com/library/azure/dn505706.aspx) |Sunucu |Sunucu düzeyinde güvenlik duvarı kurallarını kaldırır |

### <a name="azure-powershell"></a>Azure PowerShell
| Cmdlet | Düzey | Açıklama |
| --- | --- | --- |
| [Get-AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546731.aspx) |Sunucu |Sunucu düzeyinde geçerli güvenlik duvarı kurallarını döndürür |
| [New-AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546724.aspx) |Sunucu |Sunucu düzeyinde yeni bir güvenlik duvarı kuralı oluşturur |
| [Set-AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546739.aspx) |Sunucu |Sunucu düzeyinde mevcut güvenlik duvarı kuralının özelliklerini güncelleştirir |
| [Remove-AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546727.aspx) |Sunucu |Sunucu düzeyinde güvenlik duvarı kurallarını kaldırır |

> [!NOTE]
> Güvenlik duvarı ayarlarında yapılan değişikliklerin etkili olması beş dakikaya kadar sürebilir.
> 
> 

## <a name="troubleshooting-the-database-firewall"></a>Veritabanı güvenlik duvarı sorunlarını giderme
Microsoft Azure SQL Veritabanı hizmetine erişim beklediğiniz gibi davranmadığında aşağıdaki noktaları göz önünde bulundurun:

* **Yerel güvenlik duvarı yapılandırması:** Bilgisayarınızın Azure SQL Veritabanına erişebilmesi için bilgisayarınızda TCP bağlantı noktası 1433 için bir güvenlik duvarı özel durumu oluşturmanız gerekir. Azure bulut limitleri içerisinde bağlantı oluşturuyorsanız başka bağlantı noktalarını da açmanız gerekebilir. Daha fazla bilgi edinmek için [ADO.NET 4.5 ve SQL Veritabanı V12 için 1433’ten sonraki bağlantı noktaları](sql-database-develop-direct-route-ports-adonet-v12.md) konusunun **SQL Veritabanının V12 Sürümü: Dış ve iç karşılaştırması** bölümüne bakın.
* **Ağ adresi çevirisi (NAT):** NAT nedeniyle, bilgisayarınızın Azure SQL Veritabanına bağlanmak için kullandığı IP adresi, bilgisayarınızın IP yapılandırma ayarlarında gösterilen IP adresinden farklı olabilir. Bilgisayarınızın Azure’a bağlanmak için kullandığı IP adresini görüntülemek üzere portalda oturum açın ve veritabanınızı barındıran sunucudaki **Yapılandır** sekmesine gidin. **İzin Verilen IP Adresleri** bölümü altında **Geçerli İstemci IP Adresi** gösterilir. Bu bilgisayarın sunucuya erişmesine izin vermek için **İzin Verilen IP Adresleri** bölümünde **Ekle**’ye tıklayın.
* **İzin verilenler listesindeki değişiklikler henüz uygulanmadı:** Azure SQL Veritabanı güvenlik duvarı yapılandırmasındaki değişikliklerin etkili olması beş dakikaya kadar sürebilir.
* **Kullanıcı adı yetkili değil veya yanlış parola kullanıldı:** Azure SQL Veritabanı sunucusunda bir kullanıcı adı yetkili değilse veya kullanılan parola yanlışsa, Azure SQL Veritabanı sunucusuyla bağlantı reddedilir. Bir güvenlik duvarı ayarının oluşturulması yalnızca istemcilere sunucunuzla bağlantı kurmayı deneme fırsatı sunar; her istemci gerekli güvenlik kimlik bilgilerini belirtmek zorundadır. Oturumları hazırlama hakkında daha fazla bilgi için bkz. Azure SQL Veritabanında Veritabanı, Oturum ve Kullanıcıları Yönetme.
* **Dinamik IP adresi:** Dinamik IP adresiyle kurulmuş bir İnternet bağlantınız varsa ve güvenlik duvarını aşmakta sorun yaşıyorsanız aşağıdaki çözümlerden birini deneyebilirsiniz:
  
  * Azure SQL Veritabanı sunucusuna erişen istemci bilgisayarlarınıza atanmış IP adresi aralığını İnternet Servis Sağlayıcınıza (ISS) sorun ve IP adresi aralığını güvenlik duvarı kuralı olarak ekleyin.
  * İstemci bilgisayarlarınız için bunun yerine statik IP adresi alın ve IP adreslerini güvenlik duvarı kuralları olarak ekleyin.

## <a name="next-steps"></a>Sonraki adımlar
Sunucu düzeyinde ve veritabanı düzeyinde güvenlik duvarı kuralları oluşturma hakkında makaleler için bkz.:

* [Azure portalını kullanarak Azure SQL Veritabanı sunucu düzeyinde güvenlik duvarı kuralları yapılandırma](sql-database-configure-firewall-settings.md)
* [T-SQL kullanarak Azure SQL Veritabanı’nda sunucu düzeyinde ve veritabanı düzeyinde güvenlik duvarı kuralları yapılandırma](sql-database-configure-firewall-settings-tsql.md)
* [PowerShell kullanarak Azure SQL Veritabanı sunucu düzeyinde güvenlik duvarı kuralları yapılandırma](sql-database-configure-firewall-settings-powershell.md)
* [REST API kullanarak Azure SQL Veritabanı sunucu düzeyinde güvenlik duvarı kuralları yapılandırma](sql-database-configure-firewall-settings-rest.md)

Veritabanı oluşturmaya yönelik bir öğretici için bkz. [Azure portalını kullanarak dakikalar içinde bir SQL veritabanı oluşturma](sql-database-get-started.md).
Açık kaynak veya üçüncü taraf uygulamalardan bir Azure SQL veritabanına bağlanma konusunda yardım için bkz. [SQL Veritabanına yönelik istemci hızlı başlatma kod örnekleri](https://msdn.microsoft.com/library/azure/ee336282.aspx).
Veritabanlarına nasıl gidileceğini anlamak için bkz. [Veritabanı erişimi ve oturum güvenliğini yönetme](https://msdn.microsoft.com/library/azure/ee336235.aspx).

## <a name="additional-resources"></a>Ek kaynaklar
* [Veritabanınızı güvenli hale getirme](sql-database-security-overview.md)
* [SQL Server Veritabanı Altyapısı ve Azure SQL Veritabanı için Güvenlik Merkezi](https://msdn.microsoft.com/library/bb510589)

<!--Image references-->
[1]: ./media/sql-database-firewall-configure/sqldb-firewall-1.png



<!--HONumber=Dec16_HO4-->


