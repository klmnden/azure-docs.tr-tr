---
title: Sanal ağ uç noktaları ve tek ve havuza alınmış Azure SQL veritabanlarına yönelik kuralları | Microsoft Docs
description: Bir alt ağ, sanal ağ hizmet uç noktası olarak işaretleyin. Ardından uç noktası olarak Azure SQL veritabanınızda ACL uygulamak için bir sanal ağ kuralı. SQL veritabanı, ardından tüm sanal makineler ve alt ağdaki diğer düğümlere gelen iletişimi kabul eder.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: oslake
ms.author: moslake
ms.reviewer: vanto, genemi
manager: craigg
ms.date: 03/12/2019
ms.openlocfilehash: 8c33cd7fe702f46f9c88643895b96445a9aa6a78
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58805202"
---
# <a name="use-virtual-network-service-endpoints-and-rules-for-database-servers"></a>Veritabanı sunucuları için sanal ağ hizmet uç noktaları ve kuralları kullanma

*Sanal ağ kuralları* olduğunu denetleyen bir güvenlik duvarı olup olmadığını tek veritabanlarının ve azure'daki esnek havuz için veritabanı sunucusu [SQL veritabanı](sql-database-technical-overview.md) veya veritabanlarınızı [SQL veri Ambarı](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) belirli alt ağları sanal ağlardaki gönderildiği iletişimi kabul eder. Bu makalede, sanal ağ kuralı özelliği bazen Azure SQL veritabanı ve SQL veri ambarı iletişimi güvenli bir şekilde izin vermek için en iyi seçenek olup neden açıklar.

> [!IMPORTANT]
> Bu makale, Azure SQL server ve Azure SQL sunucusu üzerinde oluşturulmuş olan hem SQL veritabanı ve SQL veri ambarı veritabanları için geçerlidir. Kolaylık açısından, hem SQL Veritabanı hem de SQL Veri Ambarı için SQL Veritabanı terimi kullanılmaktadır. Bu makale *değil* geçerli bir **yönetilen örnek** Azure SQL veritabanı'nda dağıtım çünkü kendisiyle ilişkilendirilmiş bir hizmet uç noktası yok.

Bir sanal ağ kuralı oluşturmak için öncelikle olmalıdır bir [sanal ağ hizmet uç noktası] [ vm-virtual-network-service-endpoints-overview-649d] kuralın başvurmak.

## <a name="how-to-create-a-virtual-network-rule"></a>Bir sanal ağ kuralı oluşturma

Yalnızca bir sanal ağ kuralı oluşturursanız, devam açıklama ve adımları atlayabilirsiniz [bu makalenin ilerleyen bölümlerinde](#anchor-how-to-by-using-firewall-portal-59j).

<a name="anch-terminology-and-description-82f" />

## <a name="terminology-and-description"></a>Terminoloji ve açıklaması

**Sanal ağ:** Azure aboneliğinizle ilişkili sanal ağları olabilir.

**Alt ağı:** Bir sanal ağ içeren **alt ağlar**. Tüm Azure sahip olduğunuz sanal makinelerin (VM'ler), alt ağa atanır. Bir alt ağ, birden çok VM veya başka bir işlem düğümünde içerebilir. Sanal ağınızın dışında düğümleri erişime izin vermek için güvenlik yapılandırmadığınız sürece, sanal ağınızın erişemiyor işlem.

**Sanal ağ hizmet uç noktası:** A [sanal ağ hizmet uç noktası] [ vm-virtual-network-service-endpoints-overview-649d] özellik değerleri içeren bir veya daha fazla biçimsel Azure hizmet türü adları bir alt ağ. Bu makalede şu tür adı ile ilgilenen **Microsoft.Sql**, adlandırılmış SQL veritabanı, Azure hizmetini ifade eder.

**Sanal ağ kuralı:** SQL veritabanı sunucunuz için bir sanal ağ kuralı, SQL veritabanı sunucunuza erişim denetim listesinde (ACL) listelenen bir alt ağdır. SQL veritabanınız için ACL olması için alt ağ içermelidir **Microsoft.Sql** tür adı.

Bir sanal ağ kuralı bir alt ağda bulunan her düğüme gelen iletişimleri kabul etmek için SQL veritabanı sunucunuza söyler.

<a name="anch-benefits-of-a-vnet-rule-68b" />

## <a name="benefits-of-a-virtual-network-rule"></a>Bir sanal ağ kuralı avantajları

Eylem gerçekleştirmeniz kadar alt ağlar üzerindeki VM'ler, SQL veritabanı ile iletişim kuramıyor. İletişim kuran bir sanal ağ kuralı oluşturulmasını eylemdir. Sanal ağ kuralı yaklaşım seçme stratejinin güvenlik duvarı tarafından sunulan rakip güvenlik seçenekleri içeren bir karşılaştırma ve karşıtlık tartışma gerektirir.

### <a name="a-allow-access-to-azure-services"></a>A. Azure hizmetlerine erişime izin ver

Güvenlik Duvarı bölmesi olan bir **açık/kapalı** etiketli bir düğme **Azure hizmetlerine erişime izin ver**. **ON** ayarı, tüm Azure IP adresleri ve tüm Azure alt ağlar arasındaki iletişimler sağlar. Bu Azure IP'ler veya alt ağlara sahip değil. Bu **ON** ayardır SQL veritabanınız olmasını istediğinizden daha büyük olasılıkla daha açık. Sanal ağ kuralı özelliği çok daha ayrıntılı denetim olanağı sunar.

### <a name="b-ip-rules"></a>B. IP kuralları

SQL veritabanı güvenlik duvarı iletişimi SQL veritabanı'na kabul edilen IP adresi aralıklarını belirtmenizi sağlar. Bu yaklaşım, Azure özel ağ dışından kararlı IP adresleri için uygundur. Ancak Azure özel ağ içindeki birçok düğümleri ile yapılandırılan *dinamik* IP adresleri. Sanal makinenizin ne zaman yeniden gibi dinamik IP adresleri değişebilir. Bu bir güvenlik duvarı kuralı, bir üretim ortamında dinamik bir IP adresi belirtmek için folly olacaktır.

IP seçeneği elde ederek hurda bir *statik* , VM için IP adresi. Ayrıntılar için bkz [Azure portalını kullanarak bir sanal makine için özel IP adreslerini yapılandırın][vm-configure-private-ip-addresses-for-a-virtual-machine-using-the-azure-portal-321w].

Ancak, statik IP yaklaşım yönetmek zor olabilir ve uygun ölçekte kullanıldıklarında maliyeti yüksek. Sanal ağ kuralları oluşturmak ve yönetmek için daha kolay okunuyor.

> [!NOTE]
> Bir alt ağ üzerinde SQL veritabanı henüz sahip olamaz. Azure SQL veritabanı sunucunuzdaki bir düğümde sanal ağınızdaki bir alt ağ, sanal ağ içindeki tüm düğümleri, SQL veritabanı ile iletişim kurulamadı. Bu durumda, sanal makinelerinizin herhangi bir sanal ağ kuralları veya IP kuralları gerek kalmadan SQL veritabanı ile iletişim kurulamadı.

Ancak Eylül 2017'den itibaren Azure SQL veritabanı hizmeti henüz bir alt ağa atanabilir hizmetleri arasında değil.

<a name="anch-details-about-vnet-rules-38q" />

## <a name="details-about-virtual-network-rules"></a>Sanal ağ kuralları hakkında ayrıntılar

Bu bölümde, sanal ağ kuralları hakkında bazı ayrıntılar açıklanmaktadır.

### <a name="only-one-geographic-region"></a>Yalnızca tek bir coğrafi bölge

Her sanal ağ hizmet uç noktası yalnızca bir Azure bölgesine geçerlidir. Uç nokta, alt ağından gelen iletişimi kabul etmek üzere diğer bölgeler etkinleştirmez.

Herhangi bir sanal ağ kuralı, temel alınan bitim uygulandığı bölgeye sınırlıdır.

### <a name="server-level-not-database-level"></a>Sunucu düzeyinde, veritabanı düzeyinde

Tüm Azure SQL veritabanı sunucunuza, yalnızca belirli bir veritabanını sunucuya her sanal ağ kuralı uygular. Diğer bir deyişle, sunucu düzeyinde-, veritabanı düzeyinde değil, sanal ağ kuralı uygular.

- Buna karşılık, IP kuralları ya da düzeyinde uygulayabilirsiniz.

### <a name="security-administration-roles"></a>Güvenlik Yönetim rolleri

Sanal ağ hizmet uç noktaları Yönetim güvenlik rollerini ayrımı yoktur. Eylem her aşağıdaki roller gereklidir:

- **Ağ Yöneticisi:** &nbsp; Uç noktada bırakın.
- **Veritabanı Yöneticisi:** &nbsp; Erişim denetimi listesi (ACL) belirli alt SQL veritabanı sunucusuna eklemek için güncelleştirin.

*RBAC alternatif:*

Ağ Yöneticisi ve veritabanı yöneticisi rollerini sanal ağ kuralları yönetmek için gerekli olandan daha fazla özelliğe sahip. Yalnızca bir alt kümesini yeteneklerini gereklidir.

Kullanma seçeneğiniz [rol tabanlı erişim denetimi (RBAC)] [ rbac-what-is-813s] özellikleri yalnızca gerekli kısmı olan tek bir özel rol oluşturmak için azure'da. Özel rol ağ yöneticisi ya da veritabanı yöneticisi içeren yerine kullanılabilir. Diğer iki ana Yöneticisi rollere kullanıcı ekleyerek yerine özel bir rol için bir kullanıcı eklerseniz, güvenlik açıklarını'nın yüzey alanını düşüktür.

> [!NOTE]
> Bazı durumlarda, Azure SQL veritabanı ve sanal ağ alt ağı farklı Aboneliklerde bulunuyor. Bu durumlarda aşağıdaki yapılandırmaları emin olmanız gerekir:
> - Her iki aboneliğin aynı Azure Active Directory kiracısı olmalıdır.
> - Kullanıcı, hizmet uç noktaları etkinleştiriliyor ve verilen bir sunucu için bir sanal ağ alt ağı ekleme gibi işlemleri başlatmak için gerekli izinlere sahip.
> - Her iki aboneliğin Microsoft.Sql sağlayıcı kayıtlı olması gerekir.

## <a name="limitations"></a>Sınırlamalar

Azure SQL veritabanı için sanal ağ kuralları özelliği aşağıdaki sınırlamalara sahiptir:

- Bir Web uygulaması, bir VNet/alt ağ içinde bir özel IP eşlenebilir. Hizmet uç noktaları belirtilen VNet/alt ağ üzerinde etkin olsa bile, bir Azure genel IP kaynağı, bir VNet/alt ağ kaynak sunucuya Web uygulamasından bağlantıları gerekir. Sanal ağ güvenlik duvarı kuralları olan bir sunucuyu bir Web uygulamasından bağlantıyı etkinleştirmek için şunları yapmanız gerekir **izin Azure Hizmetleri için erişim sunucusu** sunucusunda.

- SQL veritabanınız için Güvenlik Duvarı'nda, her sanal ağ kuralı bir alt ağ başvuruyor. Bu başvurulan tüm alt ağlar SQL veritabanını barındıran aynı coğrafi bölgede barındırılması gerekir.

- Her Azure SQL veritabanı sunucusu, 128 ACL girişleri herhangi belirli bir sanal ağ için en fazla olabilir.

- Sanal ağ kuralları yalnızca Azure Resource Manager sanal ağlara uygulanır. ve değil [Klasik dağıtım modeli] [ arm-deployment-model-568f] ağlar.

- Kapatma şirket sanal ağ hizmet uç noktaları Azure SQL veritabanı MySQL ve PostgreSQL Azure Hizmetleri için uç noktaları da sağlar. Ancak, uç noktaları ON ile uç noktalardan gelen MySQL veya PostgreSQL örneklerine bağlanma girişimleri başarısız olabilir.
  - Temel nedeni, MySQL ve PostgreSQL büyük olasılıkla yapılandırılmış bir sanal ağ kuralı yoktur olmasıdır. Bir sanal ağ kuralı için MySQL için Azure veritabanı yapılandırmalısınız ve PostgreSQL ve bağlantı başarılı olur.

- Güvenlik Duvarı, IP adresi aralıklarını aşağıdaki ağ öğeleri için geçerlidir, ancak bu sanal ağ kuralları yapın:
  - [Siteden siteye (S2S) sanal özel ağ (VPN)][vpn-gateway-indexmd-608y]
  - Aracılığıyla şirket [ExpressRoute][expressroute-indexmd-744v]

### <a name="considerations-when-using-service-endpoints"></a>Hizmet uç noktaları kullanmayla ilgili konular

Azure SQL veritabanı için hizmet uç noktaları kullanarak, aşağıdaki konuları gözden geçirin:

- **Azure SQL veritabanına genel IP'ler için giden gereklidir**: Ağ güvenlik grupları (Nsg'ler) bağlantısına izin vermek üzere Azure SQL veritabanı IP'ler için açık olması gerekir. NSG kullanarak bunu yapabilirsiniz [hizmet etiketleri](../virtual-network/security-overview.md#service-tags) Azure SQL veritabanı için.

### <a name="expressroute"></a>ExpressRoute

Kullanıyorsanız [ExpressRoute](../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json) şirket içinden ortak eşleme veya Microsoft eşlemesi için kullanılan NAT IP adreslerini tanımlamanız gerekecek. Ortak eşleme için, her bir ExpressRoute varsayılan olarak bağlantı hattında trafik Microsoft Azure omurga ağına girdiğinde Azure hizmet trafiğine uygulanan iki NAT IP adresi kullanılır. Microsoft eşlemesi için, kullanılan NAT IP adresleri müşteri tarafından sağlanır veya hizmet sağlayıcısı tarafından sağlanır. Hizmet kaynaklarınıza erişime izin vermek için, bu genel IP adreslerine kaynak IP güvenlik duvarı ayarında izin vermeniz gerekir. Ortak eşleme ExpressRoute bağlantı hattı IP adreslerinizi bulmak için Azure portalında [ExpressRoute ile bir destek bileti açın](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview). [ExpressRoute genel ve Microsoft eşlemesi için NAT](../expressroute/expressroute-nat.md?toc=%2fazure%2fvirtual-network%2ftoc.json#nat-requirements-for-azure-public-peering) hakkında daha fazla bilgi edinin.
  
Azure SQL veritabanı için bağlantı hattınızın gelen iletişimlere izin vermesi için IP ağ kuralları, NAT'ın, genel IP adresleri için oluşturun

<!--
FYI: Re ARM, 'Azure Service Management (ASM)' was the old name of 'classic deployment model'.
When searching for blogs about ASM, you probably need to use this old and now-forbidden name.
-->

## <a name="impact-of-removing-allow-azure-services-to-access-server"></a>'Hizmetlerinin sunucuya erişimine izin vermek için Azure' ı kaldırmanın etkisi

Çok sayıda kullanıcı, kaldırmak istediğiniz **izin Azure Hizmetleri için erişim sunucusu** kendi Azure SQL Sunucuları'ndan bir VNet güvenlik duvarı kuralı ile değiştirin.
Ancak bu kaldırma, aşağıdaki özellikleri etkiler:

### <a name="import-export-service"></a>İçeri aktarma, dışarı aktarma hizmeti

Azure vm'lerinde Azure SQL veritabanı içeri aktarma dışarı aktarma hizmeti çalışır. Bu VM'ler, ağınızda değildir ve bu nedenle Azure IP, veritabanına bağlanırken alın. Kaldırma **izin Azure Hizmetleri için erişim sunucusu** bu VM'ler, veritabanlarına erişim bakımından mümkün olmayacaktır.
Sorunu geçici olarak çalışabilir. BACPAC içeri aktarma çalıştırın veya DACFx API'sini kullanarak kodunuzda doğrudan aktarın. Bu güvenlik duvarı kuralı ayarladığınız VNet-alt ağdaki bir sanal makinede dağıtıldığından emin olun.

### <a name="sql-database-query-editor"></a>SQL veritabanı sorgu Düzenleyicisi

Azure SQL veritabanı sorgu Düzenleyicisi, azure'da sanal makineler üzerinde dağıtılır. Bu VM'ler, ağınızda değildir. Bu nedenle, veritabanına bağlanırken Vm'leri Azure IP alın. Kaldırma **izin Azure Hizmetleri için erişim sunucusu**, bu Vm'leri veritabanlarınızı erişmek mümkün olmayacaktır.

### <a name="table-auditing"></a>Tablo denetimi

Şu anda SQL veritabanınızda denetimini etkinleştirmek için iki yolu vardır. Azure SQL sunucunuz üzerinde hizmet uç noktaları etkinleştirdikten sonra tablo denetimi başarısız olur. Risk azaltma burada Blob Denetimi'ne taşımaktır.

### <a name="impact-on-data-sync"></a>Veri Eşitleme etkisi

Azure SQL veritabanı, Azure IP'ler kullanarak veritabanlarınızı bağlanan veri eşitleme özelliği vardır. Hizmet uç noktaları kullanırken, kapanır emin olma olasılığı yüksektir **izin Azure Hizmetleri için erişim sunucusu** SQL veritabanı sunucunuza erişim. Veri eşitleme özelliği çalışmamasına neden olur.

## <a name="impact-of-using-vnet-service-endpoints-with-azure-storage"></a>Azure depolama ile sanal ağ hizmet uç noktaları kullanma etkileri

Azure depolama, Azure depolama hesabınızın bağlantısını sınırlamanıza olanak tanıyan aynı özellik uygulamıştır. Azure SQL Server tarafından kullanılan bir Azure depolama hesabı ile bu özelliği kullanmayı tercih ederseniz, bir sorunla karşılaşırsanız çalıştırabilirsiniz. Sonraki bir listesi ve bu tarafından etkilenen Azure SQL veritabanı ve Azure SQL veri ambarı özellikleri bir tartışma olduğu.

### <a name="azure-sql-data-warehouse-polybase"></a>Azure SQL veri ambarı PolyBase

PolyBase, verileri Azure depolama hesaplarını Azure SQL Data Warehouse'a yüklemek için yaygın olarak kullanılır. Verilerden yüklenmekte olan Azure depolama hesabına yalnızca bir sanal ağ alt kümesine erişim getiriyorsa, PolyBase kullanılarak hesabı bağlantı çalışmamasına neden olur. Her iki PolyBase etkinleştirmek için alma ve senaryoları sanal ağa güvenli Azure Depolama'ya bağlanan Azure SQL veri ambarı ile dışarı aktarma, aşağıda belirtilen adımları izleyin:

#### <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> Azure Resource Manager PowerShell modülü, Azure SQL veritabanı tarafından hala desteklenmektedir, ancak tüm gelecekteki geliştirme için Az.Sql modüldür. Bu cmdlet'ler için bkz. [Azurerm.SQL'e](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Az modül ve AzureRm modülleri komutları için bağımsız değişkenler büyük ölçüde aynıdır.

1.  Bunu kullanarak Azure PowerShell'i yükleyin [Kılavuzu](https://docs.microsoft.com/powershell/azure/install-az-ps).
2.  Genel amaçlı v1 veya blob depolama hesabı varsa, genel amaçlı v2'ye yükseltmeniz gerekir bu kullanarak [Kılavuzu](https://docs.microsoft.com/azure/storage/common/storage-account-upgrade).
3.  Olmalıdır **güvenilen Microsoft hizmetlerinin bu depolama hesabına erişmesine izin ver** Azure depolama hesabı altında açık **güvenlik duvarları ve sanal ağları** ayarlar menüsü. Bu [Kılavuzu](https://docs.microsoft.com/azure/storage/common/storage-network-security#exceptions) daha fazla bilgi için.
 
#### <a name="steps"></a>Adımlar
1. PowerShell'de, **SQL veritabanı sunucunuza kaydetmek** ile Azure Active Directory (AAD):

   ```powershell
   Connect-AzAccount
   Select-AzSubscription -SubscriptionId your-subscriptionId
   Set-AzSqlServer -ResourceGroupName your-database-server-resourceGroup -ServerName your-database-servername -AssignIdentity
   ```
    
   1. Oluşturma bir **genel amaçlı v2 depolama hesabı** bu kullanarak [Kılavuzu](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account).

   > [!NOTE]
   > - Genel amaçlı v1 veya blob depolama hesabı varsa, şunları yapmalısınız **v2'ye yükseltmeniz** bu kullanarak [Kılavuzu](https://docs.microsoft.com/azure/storage/common/storage-account-upgrade).
   > - Azure Data Lake depolama Gen2 ile'ilgili bilinen sorunlar için lütfen şuna bakın [Kılavuzu](https://docs.microsoft.com/azure/storage/data-lake-storage/known-issues).
    
1. Depolama hesabınız kapsamında gidin **erişim denetimi (IAM)**, tıklatıp **rol ataması Ekle**. Ata **depolama Blob verileri katkıda bulunan** SQL veritabanı sunucunuza RBAC rolü.

   > [!NOTE] 
   > Bu adım yalnızca sahibi ayrıcalığa sahip üyeleri gerçekleştirebilir. Azure kaynakları için çeşitli yerleşik roller için şuna başvurun [Kılavuzu](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles).
  
1. **Azure depolama hesabı bağlantı Polybase:**

   1. Veritabanı oluşturma **[ana anahtarı](https://docs.microsoft.com/sql/t-sql/statements/create-master-key-transact-sql)** , biri daha önce oluşturmadıysanız:
       ```SQL
       CREATE MASTER KEY [ENCRYPTION BY PASSWORD = 'somepassword'];
       ```
    
   1. Veritabanı kapsamlı kimlik bilgileri ile oluşturun **Kimliği = 'Yönetilen hizmet Kimliği'**:

       ```SQL
       CREATE DATABASE SCOPED CREDENTIAL msi_cred WITH IDENTITY = 'Managed Service Identity';
       ```
       > [!NOTE] 
       > - Bu mekanizma kullandığından gizlilik ile Azure depolama erişim anahtarı belirtin. gerek [yönetilen kimliği](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) altında kapsar.
       > - Kimlik adı olmalıdır **'Yönetilen hizmet Kimliği'** sanal ağa güvenli Azure depolama hesabı ile çalışmak PolyBase bağlantı.    
    
   1. Dış veri kaynağı ile abfss oluşturun: / / şeması PolyBase kullanarak, genel amaçlı v2 depolama hesabınıza bağlanmak için:

       ```SQL
       CREATE EXTERNAL DATA SOURCE ext_datasource_with_abfss WITH (TYPE = hadoop, LOCATION = 'abfss://myfile@mystorageaccount.dfs.core.windows.net', CREDENTIAL = msi_cred);
       ```
       > [!NOTE] 
       > - Genel amaçlı v1 veya blob depolama hesabı ile ilişkili dış tablolar zaten varsa önce bu dış tabloları kaldırın ve ardından ilgili dış veri kaynağını bırakın gerekir. Dış veri kaynağı ile abfss oluşturup: / / yukarıdaki gibi genel amaçlı v2 depolama hesabına bağlanma şeması ve bu yeni bir dış veri kaynağını kullanan tüm dış tabloları yeniden oluşturun. Kullanabileceğinizi [oluşturma ve yayımlama betiklerini Sihirbazı](https://docs.microsoft.com/sql/ssms/scripting/generate-and-publish-scripts-wizard) kolaylaştırmak için tüm dış tablolar oluşturma-betikleri oluşturmak için.
       > - Abfss hakkında daha fazla bilgi: / / şeması, bu [Kılavuzu](https://docs.microsoft.com/azure/storage/data-lake-storage/introduction-abfs-uri).
       > - CREATE EXTERNAL DATA SOURCE hakkında daha fazla bilgi için bu başvuru [Kılavuzu](https://docs.microsoft.com/sql/t-sql/statements/create-external-data-source-transact-sql).
        
   1. Sorgu kullanarak normal olarak [dış tablolar](https://docs.microsoft.com/sql/t-sql/statements/create-external-table-transact-sql).

### <a name="azure-sql-database-blob-auditing"></a>Azure SQL veritabanı Blob denetimi

BLOB denetimi denetim günlükleri, kendi depolama hesabınıza gönderir. Bu depolama hesabı kullanıyorsa sanal ağ hizmet uç noktaları özelliği, Azure SQL veritabanı'ndan depolama hesabı bağlantı çalışmamasına neden olur.

## <a name="adding-a-vnet-firewall-rule-to-your-server-without-turning-on-vnet-service-endpoints"></a>Üzerinde sanal ağ hizmet uç noktaları açmadan sunucunuza bir VNet güvenlik duvarı kuralı ekleme

Uzun zaman önce bu özelliği geliştirilmiştir önce Güvenlik Duvarı'nda canlı bir sanal ağ kuralı uygulayabileceğine önce sanal ağ hizmet uç noktaları açmaları gerekiyordu. Uç noktaları, Azure SQL veritabanı için belirli bir sanal ağ alt ilgili. Ancak artık Ocak 2018'den itibaren bu gereksinim ayarlayarak atlayabilir **IgnoreMissingVNetServiceEndpoint** bayrağı.

Yalnızca ayar bir güvenlik duvarı kuralı sunucu sağlanmasına yardımcı olmaz. Ayrıca sanal ağ hizmet uç noktaları için etkili güvenlik açmanız gerekir. Hizmet uç noktaları açtığınızda, sanal ağ alt ağınız üzerinde Kapalı öğesinden Geçiş tamamlanana kadar kapalı kalma süresi karşılaşır. Bu özellikle büyük sanal ağlar bağlamında geçerlidir. Kullanabileceğiniz **IgnoreMissingVNetServiceEndpoint** azaltmak veya geçiş sırasında kapalı kalma süresini ortadan kaldırmak için bayrak.

Ayarlayabileceğiniz **IgnoreMissingVNetServiceEndpoint** PowerShell kullanarak bayrağı. Ayrıntılar için bkz [Azure SQL veritabanı için bir sanal ağ hizmet uç noktası ve kuralı oluşturmak için PowerShell][sql-db-vnet-service-endpoint-rule-powershell-md-52d].

## <a name="errors-40914-and-40615"></a>40914 ve 40615 hataları

Bağlantı hatası 40914 ilişkili *sanal ağ kuralları*, Azure Portalı'nda güvenlik duvarı bölmesinde belirtilen. Hata 40615 benzerdir, ilişkili dışında *IP adresi kuralları* güvenlik duvarı.

### <a name="error-40914"></a>Hata 40914

*İleti metni:* Sunucu açamıyor '*[sunucu-adı]*' oturum açma tarafından istenen. İstemcinin sunucuya erişmesine izin verilmiyor.

*Hata açıklaması:* Sanal ağ sunucu uç noktaları olan bir alt ağda istemcisidir. Ancak, Azure SQL veritabanı sunucusunun SQL veritabanıyla iletişim kurmak için sağ alt ağa izin veren sanal ağ kuralı yok.

*Hata çözünürlüğü:* Azure portal'ın güvenlik duvarı bölmede sanal ağ kuralları denetimi kullanın [bir sanal ağ kuralı ekleyin](#anchor-how-to-by-using-firewall-portal-59j) alt ağ.

### <a name="error-40615"></a>Hata 40615

*İleti metni:* Sunucu açamıyor '{0}' oturum açma tarafından istenen. İstemci IP adresi ile{1}' sunucuya erişmesine izin verilmiyor.

*Hata açıklaması:* İstemci, Azure SQL veritabanı sunucusuna bağlanmak için yetkili değil bir IP adresinden bağlanmaya çalışıyor. Sunucu güvenlik duvarında, istemcinin belirtilen IP adresinden SQL Veritabanı ile iletişim kurmasına izin veren bir IP adresi kuralı yok.

*Hata çözünürlüğü:* İstemcinin IP adresini bir IP kuralı olarak girin. Azure Portalı'nda güvenlik duvarı bölmesini kullanarak bunu.

Birden fazla SQL veritabanı hata iletilerinin listesini belgelenen [burada][sql-database-develop-error-messages-419g].

<a name="anchor-how-to-by-using-firewall-portal-59j" />

## <a name="portal-can-create-a-virtual-network-rule"></a>Portal bir sanal ağ kuralı oluşturabilirsiniz.

Bu bölümde, nasıl kullanabileceğinizi gösteren [Azure portalında] [ http-azure-portal-link-ref-477t] oluşturmak için bir *sanal ağ kuralı* Azure SQL veritabanı. SQL veritabanınız olarak etiketlenmiş belirli bir alt ağından gelen iletişimi kabul etmek üzere kural söyler bir *sanal ağ hizmet uç noktası*.

> [!NOTE]
> Hizmet uç noktası için Azure SQL veritabanı sunucunuzun VNet güvenlik duvarı kuralları eklemek istiyorsanız, önce bu hizmet uç noktaları alt ağ için açık olduğundan emin olun.
>
> Hizmet uç noktaları alt ağ için etkin olmayan, portal, bunları etkinleştirmek için sorar. Tıklayın **etkinleştirme** aynı dikey penceresinde kural eklediğiniz düğmesi.

## <a name="powershell-alternative"></a>PowerShell alternatif

Bir PowerShell Betiği, sanal ağ kuralları oluşturabilirsiniz. Önemli cmdlet **yeni AzSqlServerVirtualNetworkRule**. İlgileniyorsanız, bkz. [Azure SQL veritabanı için bir sanal ağ hizmet uç noktası ve kuralı oluşturmak için PowerShell][sql-db-vnet-service-endpoint-rule-powershell-md-52d].

## <a name="rest-api-alternative"></a>REST API alternatif

Dahili olarak, SQL VNet eylemleri için PowerShell cmdlet'leri, REST API'lerini çağırma. REST API'lerini doğrudan çağırabilir.

- [Sanal ağ kuralları: İşlemleri][rest-api-virtual-network-rules-operations-862r]

## <a name="prerequisites"></a>Önkoşullar

Belirli sanal ağ hizmet uç noktası ile etiketlenmiş bir alt ağ zaten olmalıdır *tür adı* Azure SQL veritabanı ile ilgili.

- İlgili uç nokta türü adı **Microsoft.Sql**.
- Alt ağınız tür adıyla etiketlenmiş olması değil olup [alt ağınızın bir uç nokta olduğunu doğrulayın][sql-db-vnet-service-endpoint-rule-powershell-md-a-verify-subnet-is-endpoint-ps-100].

<a name="a-portal-steps-for-vnet-rule-200" />

## <a name="azure-portal-steps"></a>Azure portal adımları

1. [Azure portalında][http-azure-portal-link-ref-477t] oturum açın.

2. Ardından portala gitmek **SQL sunucuları** &gt; **güvenlik duvarı / sanal ağlar**.

3. Ayarlama **Azure hizmetlerine erişime izin ver** off denetimi.

    > [!IMPORTANT]
    > AÇIK olarak ayarlanmış denetimi değiştirmeden bırakırsanız, Azure SQL veritabanı sunucunuza tüm alt ağından gelen iletişimi kabul eder. AÇIK olarak ayarlanmış denetimi bırakarak bir güvenlik açısından aşırı erişimi olabilir. Microsoft Azure sanal ağ hizmet uç noktası özelliği, SQL veritabanı, sanal ağ kuralı özelliği ile koordinasyon halinde birlikte, güvenlik yüzey alanını azaltabilirsiniz.

4. Tıklayın **+ Ekle varolan** de denetimi **sanal ağlar** bölümü.

    ![Ekle'ye var (alt ağ uç noktası, SQL kural olarak).][image-portal-firewall-vnet-add-existing-10-png]

5. Yeni **Create/Update** bölmesinde, denetimler, Azure kaynaklarınızın adları ile doldurun.

    > [!TIP]
    > Doğru içermelidir **adres ön eki** alt ağınız için. Portalda değeri bulabilirsiniz.
    > Gezinme **tüm kaynakları** &gt; **tüm türleri** &gt; **sanal ağları**. Filtreyi sanal ağlarınıza görüntüler. Sanal Ağ'a tıklayın ve ardından **alt ağlar**. **Adres ARALIĞI** sütun ihtiyacınız adres ön eki vardır.

    ![Yeni kural için alanları doldurun.][image-portal-firewall-create-update-vnet-rule-20-png]

6. Tıklayın **Tamam** bölmesinin alt kısmındaki düğmesi.

7. Sonuçta elde edilen sanal ağ kuralı güvenlik duvarı bölmesinde görürsünüz.

    ![Yeni Kural güvenlik duvarı bölmesinde görürsünüz.][image-portal-firewall-vnet-result-rule-30-png]

> [!NOTE]
> Aşağıdaki durumlar veya durumları için kurallar geçerlidir:
> - **Hazır:** Başlattığınız işleminin başarılı olduğunu gösterir.
> - **Başarısız oldu:** Başlattığınız bir işlemin başarısız olduğunu gösterir.
> - **Silindi:** Yalnızca silme işlemi için geçerlidir ve kural silindi ve artık geçerli olduğunu gösterir.
> - **Devam ediyor:** İşlemi sürmekte olduğunu gösterir. Eski kural, işlem bu durumda olduğunda geçerlidir.

<a name="anchor-how-to-links-60h" />

## <a name="related-articles"></a>İlgili makaleler

- [Azure sanal ağ hizmet uç noktaları][vm-virtual-network-service-endpoints-overview-649d]
- [Azure SQL veritabanı sunucu düzeyinde ve veritabanı düzeyinde güvenlik duvarı kuralları][sql-db-firewall-rules-config-715d]

Azure SQL veritabanı için sanal ağ kuralı özelliği geç Eylül 2017'de kullanıma sunulmuştur.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure SQL veritabanı için bir sanal ağ hizmet uç noktası ve bir sanal ağ kuralı oluşturmak için PowerShell kullanın.][sql-db-vnet-service-endpoint-rule-powershell-md-52d]
- [Sanal ağ kuralları: İşlemleri] [ rest-api-virtual-network-rules-operations-862r] REST API'leri

<!-- Link references, to images. -->

[image-portal-firewall-vnet-add-existing-10-png]: media/sql-database-vnet-service-endpoint-rule-overview/portal-firewall-vnet-add-existing-10.png

[image-portal-firewall-create-update-vnet-rule-20-png]: media/sql-database-vnet-service-endpoint-rule-overview/portal-firewall-create-update-vnet-rule-20.png

[image-portal-firewall-vnet-result-rule-30-png]: media/sql-database-vnet-service-endpoint-rule-overview/portal-firewall-vnet-result-rule-30.png

<!-- Link references, to text, Within this same GitHub repo. -->

[arm-deployment-model-568f]: ../azure-resource-manager/resource-manager-deployment-model.md

[expressroute-indexmd-744v]: ../expressroute/index.yml

[rbac-what-is-813s]:../role-based-access-control/overview.md

[sql-db-firewall-rules-config-715d]: sql-database-firewall-configure.md

[sql-database-develop-error-messages-419g]: sql-database-develop-error-messages.md

[sql-db-vnet-service-endpoint-rule-powershell-md-52d]: sql-database-vnet-service-endpoint-rule-powershell.md

[sql-db-vnet-service-endpoint-rule-powershell-md-a-verify-subnet-is-endpoint-ps-100]: sql-database-vnet-service-endpoint-rule-powershell.md#a-verify-subnet-is-endpoint-ps-100

[vm-configure-private-ip-addresses-for-a-virtual-machine-using-the-azure-portal-321w]: ../virtual-network/virtual-networks-static-private-ip-arm-pportal.md

[vm-virtual-network-service-endpoints-overview-649d]: https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview

[vpn-gateway-indexmd-608y]: ../vpn-gateway/index.yml

<!-- Link references, to text, Outside this GitHub repo (HTTP). -->

[http-azure-portal-link-ref-477t]: https://portal.azure.com/

[rest-api-virtual-network-rules-operations-862r]: https://docs.microsoft.com/rest/api/sql/virtualnetworkrules

<!-- ??2
#### Syntax related articles
- REST API Reference, including JSON

- Azure CLI

- ARM templates
-->
