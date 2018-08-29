---
title: Sanal ağ hizmet uç noktaları ve Azure SQL veritabanı için kuralları | Microsoft Docs
description: Bir alt ağ, sanal ağ hizmet uç noktası olarak işaretleyin. Ardından uç noktası olarak Azure SQL veritabanınızda ACL uygulamak için bir sanal ağ kuralı. SQL veritabanı, ardından tüm sanal makineler ve alt ağdaki diğer düğümlere gelen iletişimi kabul eder.
services: sql-database
ms.service: sql-database
ms.prod_service: sql-database, sql-data-warehouse
author: DhruvMsft
manager: craigg
ms.custom: VNet Service endpoints
ms.topic: conceptual
ms.date: 08/28/2018
ms.reviewer: carlrab
ms.author: dmalik
ms.openlocfilehash: 223a8da0c3c940c57dfc58d9cc87a19ae45a64eb
ms.sourcegitcommit: a1140e6b839ad79e454186ee95b01376233a1d1f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2018
ms.locfileid: "43143819"
---
# <a name="use-virtual-network-service-endpoints-and-rules-for-azure-sql-database-and-sql-data-warehouse"></a>Azure SQL veritabanı ve SQL veri ambarı için sanal ağ hizmet uç noktaları ve kuralları kullanma

*Sanal ağ kuralları* olduğunu denetleyen bir güvenlik duvarı olup Azure [SQL veritabanı](sql-database-technical-overview.md) veya [SQL veri ambarı](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) sunucu uygulamasından gönderilen iletişimi kabul eder sanal ağlar içindeki belirli alt ağlar. Bu makalede, sanal ağ kuralı özelliği bazen Azure SQL veritabanınıza iletişimi güvenli bir şekilde vermeye yönelik en iyi seçenek olup neden açıklar.

> [!NOTE]
> Bu konu başlığı, Azure SQL sunucusunun yanı sıra Azure SQL sunucusu üzerinde oluşturulmuş olan SQL Veritabanı ve SQL Veri Ambarı veritabanları için de geçerlidir. Kolaylık açısından, hem SQL Veritabanı hem de SQL Veri Ambarı için SQL Veritabanı terimi kullanılmaktadır.

Bir sanal ağ kuralı oluşturmak için öncelikle olmalıdır bir [sanal ağ hizmet uç noktası] [ vm-virtual-network-service-endpoints-overview-649d] kuralın başvurmak.

#### <a name="how-to-create-a-virtual-network-rule"></a>Bir sanal ağ kuralı oluşturma

Yalnızca bir sanal ağ kuralı oluşturursanız, devam açıklama ve adımları atlayabilirsiniz [bu makalenin ilerleyen bölümlerinde](#anchor-how-to-by-using-firewall-portal-59j).






<a name="anch-terminology-and-description-82f" />

## <a name="terminology-and-description"></a>Terminoloji ve açıklaması

**Sanal ağ:** Azure aboneliğinizle ilişkili sanal ağı olabilir.

**Alt ağı:** içeren bir sanal ağ **alt ağlar**. Tüm Azure sahip olduğunuz sanal makinelerin (VM'ler), alt ağa atanır. Bir alt ağ, birden çok VM veya başka bir işlem düğümünde içerebilir. Sanal ağınızın dışında düğümleri erişime izin vermek için güvenlik yapılandırmadığınız sürece, sanal ağınızın erişemiyor işlem.

**Sanal ağ hizmet uç noktası:** A [sanal ağ hizmet uç noktası] [ vm-virtual-network-service-endpoints-overview-649d] özellik değerleri içeren bir veya daha fazla biçimsel Azure hizmet türü adları bir alt ağ. Bu makalede şu tür adı ile ilgilenen **Microsoft.Sql**, adlandırılmış SQL veritabanı, Azure hizmetini ifade eder.

**Sanal ağ kuralı:** SQL veritabanı sunucunuz için bir sanal ağ kuralı, SQL veritabanı sunucunuza erişim denetim listesinde (ACL) listelenen bir alt ağıdır. SQL veritabanınız için ACL olması için alt ağ içermelidir **Microsoft.Sql** tür adı.

Bir sanal ağ kuralı bir alt ağda bulunan her düğüme gelen iletişimleri kabul etmek için SQL veritabanı sunucunuza söyler.







<a name="anch-benefits-of-a-vnet-rule-68b" />

## <a name="benefits-of-a-virtual-network-rule"></a>Bir sanal ağ kuralı avantajları

Eylem gerçekleştirmeniz kadar alt ağlar üzerindeki VM'ler, SQL veritabanı ile iletişim kuramıyor. İletişim kuran bir sanal ağ kuralı oluşturulmasını eylemdir. Sanal ağ kuralı yaklaşım seçme stratejinin güvenlik duvarı tarafından sunulan rakip güvenlik seçenekleri içeren bir karşılaştırma ve karşıtlık tartışma gerektirir.

#### <a name="a-allow-access-to-azure-services"></a>A. Azure hizmetlerine erişime izin ver

Güvenlik Duvarı bölmesi olan bir **açık/kapalı** etiketli bir düğme **Azure hizmetlerine erişime izin ver**. **ON** ayarı, tüm Azure IP adresleri ve tüm Azure alt ağlar arasındaki iletişimler sağlar. Bu Azure IP'ler veya alt ağlara sahip değil. Bu **ON** ayardır SQL veritabanınız olmasını istediğinizden daha büyük olasılıkla daha açık. Sanal ağ kuralı özelliği çok daha ayrıntılı denetim olanağı sunar.

#### <a name="b-ip-rules"></a>B. IP kuralları

SQL veritabanı güvenlik duvarı iletişimi SQL veritabanı'na kabul edilen IP adresi aralıklarını belirtmenizi sağlar. Bu yaklaşım, Azure özel ağ dışından kararlı IP adresleri için uygundur. Ancak Azure özel ağ içindeki birçok düğümleri ile yapılandırılan *dinamik* IP adresleri. Sanal makinenizin ne zaman yeniden gibi dinamik IP adresleri değişebilir. Bu bir güvenlik duvarı kuralı, bir üretim ortamında dinamik bir IP adresi belirtmek için folly olacaktır.

IP seçeneği elde ederek hurda bir *statik* , VM için IP adresi. Ayrıntılar için bkz [Azure portalını kullanarak bir sanal makine için özel IP adreslerini yapılandırın][vm-configure-private-ip-addresses-for-a-virtual-machine-using-the-azure-portal-321w].

Ancak, statik IP yaklaşım yönetmek zor olabilir ve uygun ölçekte kullanıldıklarında maliyeti yüksek. Sanal ağ kuralları oluşturmak ve yönetmek için daha kolay okunuyor.

#### <a name="c-cannot-yet-have-sql-database-on-a-subnet"></a>C. SQL veritabanı, bir alt ağ üzerinde henüz sahip olamaz

Azure SQL veritabanı sunucunuzdaki bir düğümde sanal ağınızdaki bir alt ağ, sanal ağ içindeki tüm düğümleri, SQL veritabanı ile iletişim kurulamadı. Bu durumda, sanal makinelerinizin herhangi bir sanal ağ kuralları veya IP kuralları gerek kalmadan SQL veritabanı ile iletişim kurulamadı.

Ancak Eylül 2017'den itibaren Azure SQL veritabanı hizmeti henüz bir alt ağa atanabilir hizmetleri arasında değil.






<a name="anch-details-about-vnet-rules-38q" />

## <a name="details-about-virtual-network-rules"></a>Sanal ağ kuralları hakkında ayrıntılar

Bu bölümde, sanal ağ kuralları hakkında bazı ayrıntılar açıklanmaktadır.

#### <a name="only-one-geographic-region"></a>Yalnızca tek bir coğrafi bölge

Her sanal ağ hizmet uç noktası yalnızca bir Azure bölgesine geçerlidir. Uç nokta, alt ağından gelen iletişimi kabul etmek üzere diğer bölgeler etkinleştirmez.

Herhangi bir sanal ağ kuralı, temel alınan bitim uygulandığı bölgeye sınırlıdır.

#### <a name="server-level-not-database-level"></a>Sunucu düzeyinde, veritabanı düzeyinde

Tüm Azure SQL veritabanı sunucunuza, yalnızca belirli bir veritabanını sunucuya her sanal ağ kuralı uygular. Diğer bir deyişle, sunucu düzeyinde-, veritabanı düzeyinde değil, sanal ağ kuralı uygular.

- Buna karşılık, IP kuralları ya da düzeyinde uygulayabilirsiniz.

#### <a name="security-administration-roles"></a>Güvenlik Yönetim rolleri

Sanal ağ hizmet uç noktaları Yönetim güvenlik rollerini ayrımı yoktur. Eylem her aşağıdaki roller gereklidir:

- **Ağ Yöneticisi:** &nbsp; uç noktada açın.
- **Veritabanı Yöneticisi:** &nbsp; erişim denetim listesi (ACL) belirli alt SQL veritabanı sunucusuna eklemek için güncelleştirin.

*RBAC alternatif:*

Ağ Yöneticisi ve veritabanı yöneticisi rollerini sanal ağ kuralları yönetmek için gerekli olandan daha fazla özelliğe sahip. Yalnızca bir alt kümesini yeteneklerini gereklidir.

Kullanma seçeneğiniz [rol tabanlı erişim denetimi (RBAC)] [ rbac-what-is-813s] özellikleri yalnızca gerekli kısmı olan tek bir özel rol oluşturmak için azure'da. Özel rol ağ yöneticisi ya da veritabanı yöneticisi içeren yerine kullanılabilir. Diğer iki ana Yöneticisi rollere kullanıcı ekleyerek yerine özel bir rol için bir kullanıcı eklerseniz, güvenlik açıklarını'nın yüzey alanını düşüktür.

> [!NOTE]
> Bazı durumlarda, Azure SQL veritabanı ve sanal ağ alt ağı farklı Aboneliklerde bulunuyor. Bu durumlarda aşağıdaki yapılandırmaları emin olmanız gerekir:
> - Her iki aboneliğin aynı Azure Active Directory kiracısı olmalıdır.
> - Kullanıcı, hizmet uç noktaları etkinleştiriliyor ve verilen bir sunucu için bir sanal ağ alt ağı ekleme gibi işlemleri başlatmak için gerekli izinlere sahip.

## <a name="limitations"></a>Sınırlamalar

Azure SQL veritabanı için sanal ağ kuralları özelliği aşağıdaki sınırlamalara sahiptir:

- Bir Web uygulaması, bir VNet/alt ağ içinde bir özel IP eşlenebilir. Hizmet uç noktaları belirtilen VNet/alt ağ üzerinde etkin olsa bile, bir Azure genel IP kaynağı, bir VNet/alt ağ kaynak sunucuya Web uygulamasından bağlantıları gerekir. Sanal ağ güvenlik duvarı kuralları olan bir sunucuyu bir Web uygulamasından bağlantıyı etkinleştirmek için şunları yapmanız gerekir **tüm Azure Hizmetleri izin** sunucusunda.

- SQL veritabanınız için Güvenlik Duvarı'nda, her sanal ağ kuralı bir alt ağ başvuruyor. Bu başvurulan tüm alt ağlar SQL veritabanını barındıran aynı coğrafi bölgede barındırılması gerekir.

- Her Azure SQL veritabanı sunucusu, 128 ACL girişleri herhangi belirli bir sanal ağ için en fazla olabilir.

- Sanal ağ kuralları yalnızca Azure Resource Manager sanal ağlara uygulanır. ve değil [Klasik dağıtım modeli] [ arm-deployment-model-568f] ağlar.

- Kapatma şirket sanal ağ hizmet uç noktaları Azure SQL veritabanı MySQL ve PostgreSQL Azure Hizmetleri için uç noktaları da sağlar. Ancak, uç noktaları ON ile uç noktalardan gelen MySQL veya PostgreSQL örneklerine bağlanma girişimleri başarısız olur.
    - Temel nedeni, MySQL ve PostgreSQL şu anda başarısız desteklemiyor olmasıdır.

- Güvenlik Duvarı, IP adresi aralıklarını aşağıdaki ağ öğeleri için geçerlidir, ancak bu sanal ağ kuralları yapın:
    - [Siteden siteye (S2S) sanal özel ağ (VPN)][vpn-gateway-indexmd-608y]
    - Aracılığıyla şirket [ExpressRoute][expressroute-indexmd-744v]

#### <a name="considerations-when-using-service-endpoints"></a>Hizmet uç noktaları kullanmayla ilgili konular
Azure SQL veritabanı için hizmet uç noktaları kullanarak, aşağıdaki konuları gözden geçirin:

- **Azure SQL veritabanına genel IP'ler için giden gereklidir**: bağlantısına izin vermek üzere Azure SQL veritabanı IP'ler için ağ güvenlik grupları (Nsg'ler) açılır. NSG kullanarak bunu yapabilirsiniz [hizmet etiketleri](../virtual-network/security-overview.md#service-tags) Azure SQL veritabanı için.

#### <a name="expressroute"></a>ExpressRoute

Ağınız aracılığıyla Azure ağına bağlı olup olmadığını [ExpressRoute][expressroute-indexmd-744v], her bağlantı hattı, Microsoft Edge, iki genel IP adresi ile yapılandırılır. İki IP adresi, Microsoft Services gibi Azure depolama için Azure ortak eşleme kullanarak bağlanmak için kullanılır.

Azure SQL veritabanı için bağlantı hattınızın gelen iletişimlere izin vermesi için genel IP adresleri, bağlantı hatları için IP ağ kuralları oluşturmanız gerekir. ExpressRoute devreniz genel IP adreslerini bulmak için Azure portalını kullanarak ExpressRoute ile bir destek bileti açın.


<!--
FYI: Re ARM, 'Azure Service Management (ASM)' was the old name of 'classic deployment model'.
When searching for blogs about ASM, you probably need to use this old and now-forbidden name.
-->

## <a name="impact-of-removing-allow-all-azure-services"></a>'Tüm Azure Hizmetleri izin ver' ı kaldırmanın etkisi

Çok sayıda kullanıcı, kaldırmak istediğiniz **tüm Azure Hizmetleri izin** , Azure SQL Sunucuları'ndan bir VNet güvenlik duvarı kuralı ile değiştirin.
Ancak bu kaldırma, aşağıdaki Azure SQLDB özellikleri etkiler:

#### <a name="import-export-service"></a>İçeri aktarma, dışarı aktarma hizmeti
Azure SQLDB içeri aktarma dışarı aktarma hizmeti, azure'da sanal makineler üzerinde çalışır. Bu VM'ler, ağınızda değildir ve bu nedenle Azure IP, veritabanına bağlanırken alın. Kaldırma **tüm Azure Hizmetleri izin** bu VM'ler, veritabanlarına erişim bakımından mümkün olmayacaktır.
Sorunu geçici olarak çalışabilir. BACPAC içeri aktarma çalıştırın veya DACFx API'sini kullanarak kodunuzda doğrudan aktarın. Bu güvenlik duvarı kuralı ayarladığınız VNet-alt ağdaki bir sanal makinede dağıtıldığından emin olun.

#### <a name="sql-database-query-editor"></a>SQL veritabanı sorgu Düzenleyicisi
Azure SQL veritabanı sorgu Düzenleyicisi, azure'da sanal makineler üzerinde dağıtılır. Bu VM'ler, ağınızda değildir. Bu nedenle, veritabanına bağlanırken Vm'leri Azure IP alın. Kaldırma **tüm Azure Hizmetleri izin**, bu Vm'leri veritabanlarınızı erişmek mümkün olmayacaktır.

#### <a name="table-auditing"></a>Tablo denetimi
Şu anda SQL veritabanınızda denetimini etkinleştirmek için iki yolu vardır. Azure SQL sunucunuz üzerinde hizmet uç noktaları etkinleştirdikten sonra tablo denetimi başarısız olur. Risk azaltma burada Blob Denetimi'ne taşımaktır.

#### <a name="impact-on-data-sync"></a>Veri Eşitleme etkisi
Azure SQLDB Azure IP'ler kullanarak veritabanlarınızı bağlanan veri eşitleme özelliği vardır. Hizmet uç noktaları kullanırken, kapanır emin olma olasılığı yüksektir **tüm Azure Hizmetleri izin** mantıksal sunucunuza erişim. Veri eşitleme özelliği çalışmamasına neden olur.

## <a name="impact-of-using-vnet-service-endpoints-with-azure-storage"></a>Azure depolama ile sanal ağ hizmet uç noktaları kullanma etkileri

Azure depolama, depolama hesabınızın bağlantısını sınırlamanıza olanak tanıyan aynı özellik uygulamıştır.
Bir Azure SQL Server tarafından kullanılan bir depolama hesabı ile bu özelliği kullanmayı tercih ederseniz, bir sorunla karşılaşırsanız çalıştırabilirsiniz. Sonraki bir listesi ve bu tarafından etkilenen Azure SQLDB özelliklerinin tartışma olduğu.

#### <a name="azure-sqldw-polybase"></a>Azure SQLDW PolyBase
PolyBase, verileri depolama hesaplarından Azure SQLDW yüklemek için yaygın olarak kullanılır. Verilerden yüklenmekte olan depolama hesabı yalnızca bir sanal ağ alt kümesine erişim getiriyorsa, PolyBase kullanılarak hesabı bağlantı çalışmamasına neden olur. Bunun için bir risk azaltma yoktur ve daha fazla bilgi için Microsoft desteğine başvurabilirsiniz.

#### <a name="azure-sqldb-blob-auditing"></a>Azure SQLDB Blob denetimi
BLOB denetimi denetim günlükleri, kendi depolama hesabınıza gönderir. Olay hizmet uç noktaları özelliği bu depolama hesabı kullanıyorsa, SQLDB Azure depolama hesabı bağlantısı çalışmamasına neden olur.

## <a name="adding-a-vnet-firewall-rule-to-your-server-without-turning-on-vnet-service-endpoints"></a>Üzerinde sanal ağ hizmet uç noktaları açmadan sunucunuza bir VNET güvenlik duvarı kuralı ekleme

Uzun zaman önce bu özelliği geliştirilmiştir önce Güvenlik Duvarı'nda canlı bir sanal ağ kuralı uygulayabileceğine önce sanal ağ hizmet uç noktaları açmaları gerekiyordu. Uç noktaları, Azure SQL veritabanı için belirli bir sanal ağ alt ilgili. Ancak artık Ocak 2018'den itibaren bu gereksinim ayarlayarak atlayabilir **IgnoreMissingServiceEndpoint** bayrağı.

Yalnızca ayar bir güvenlik duvarı kuralı sunucu sağlanmasına yardımcı olmaz. Ayrıca sanal ağ hizmet uç noktaları için etkili güvenlik açmanız gerekir. Hizmet uç noktaları açtığınızda, sanal ağ alt ağınız üzerinde Kapalı öğesinden Geçiş tamamlanana kadar kapalı kalma süresi karşılaşır. Bu özellikle büyük sanal ağlar bağlamında geçerlidir. Kullanabileceğiniz **IgnoreMissingServiceEndpoint** azaltmak veya geçiş sırasında kapalı kalma süresini ortadan kaldırmak için bayrak.

Ayarlayabileceğiniz **IgnoreMissingServiceEndpoint** PowerShell kullanarak bayrağı. Ayrıntılar için bkz [Azure SQL veritabanı için bir sanal ağ hizmet uç noktası ve kuralı oluşturmak için PowerShell][sql-db-vnet-service-endpoint-rule-powershell-md-52d].


## <a name="errors-40914-and-40615"></a>40914 ve 40615 hataları

Bağlantı hatası 40914 ilişkili *sanal ağ kuralları*, Azure Portalı'nda güvenlik duvarı bölmesinde belirtilen. Hata 40615 benzerdir, ilişkili dışında *IP adresi kuralları* güvenlik duvarı.

#### <a name="error-40914"></a>Hata 40914

*İleti metni:* sunucu açamıyor '*[sunucu-adı]*' oturum açma tarafından istenen. İstemcinin sunucuya erişmesine izin verilmiyor.

*Hata açıklaması:* sanal ağ sunucu uç noktaları olan bir alt ağda istemcidir. Ancak, Azure SQL veritabanı sunucusunun SQL veritabanıyla iletişim kurmak için sağ alt ağa izin veren sanal ağ kuralı yok.

*Hata çözünürlüğü:* üzerinde güvenlik duvarı bölmesinde sanal ağ kuralları denetimi kullanın Azure portalının [bir sanal ağ kuralı ekleyin](#anchor-how-to-by-using-firewall-portal-59j) alt ağ.

#### <a name="error-40615"></a>Hata 40615

*İleti metni:* sunucu açamıyor '{0}' oturum açma tarafından istenen. İstemci IP adresi ile{1}' sunucuya erişmesine izin verilmiyor.

*Hata açıklaması:* istemci, Azure SQL veritabanı sunucusuna bağlanmak için yetkili değil bir IP adresinden bağlanmaya çalışıyor. Sunucu güvenlik duvarını belirli IP adresinden SQL veritabanıyla iletişim kurmak bir istemci veren IP adresi kuralı yok.

*Hata çözünürlüğü:* istemcinin IP adresini bir IP kuralı olarak girin. Azure Portalı'nda güvenlik duvarı bölmesini kullanarak bunu.


Birden fazla SQL veritabanı hata iletilerinin listesini belgelenen [burada][sql-database-develop-error-messages-419g].





<a name="anchor-how-to-by-using-firewall-portal-59j" />

## <a name="portal-can-create-a-virtual-network-rule"></a>Portal bir sanal ağ kuralı oluşturabilirsiniz.

Bu bölümde, nasıl kullanabileceğinizi gösteren [Azure portalında] [ http-azure-portal-link-ref-477t] oluşturmak için bir *sanal ağ kuralı* Azure SQL veritabanı. SQL veritabanınız olarak etiketlenmiş belirli bir alt ağından gelen iletişimi kabul etmek üzere kural söyler bir *sanal ağ hizmet uç noktası*.

> [!NOTE]
> Hizmet uç noktası için Azure SQL veritabanı sunucunuzun VNet güvenlik duvarı kuralları eklemek istiyorsanız, önce bu hizmet uç noktaları alt ağ için açık olduğundan emin olun.
>
> Hizmet uç noktaları alt ağ için etkin olmayan, portal, bunları etkinleştirmek için sorar. Tıklayın **etkinleştirme** aynı dikey penceresinde kural eklediğiniz düğmesi.

#### <a name="powershell-alternative"></a>PowerShell alternatif

Bir PowerShell Betiği, sanal ağ kuralları oluşturabilirsiniz. Önemli cmdlet **New-AzureRmSqlServerVirtualNetworkRule**. İlgileniyorsanız, bkz. [Azure SQL veritabanı için bir sanal ağ hizmet uç noktası ve kuralı oluşturmak için PowerShell][sql-db-vnet-service-endpoint-rule-powershell-md-52d].

#### <a name="rest-api-alternative"></a>REST API alternatif

Dahili olarak, SQL VNet eylemleri için PowerShell cmdlet'leri, REST API'lerini çağırma. REST API'lerini doğrudan çağırabilir.

- [Sanal ağ kuralları: işlemler][rest-api-virtual-network-rules-operations-862r]

#### <a name="prerequisites"></a>Önkoşullar

Belirli sanal ağ hizmet uç noktası ile etiketlenmiş bir alt ağ zaten olmalıdır *tür adı* Azure SQL veritabanı ile ilgili.

- İlgili uç nokta türü adı **Microsoft.Sql**.
- Alt ağınız tür adıyla etiketlenmiş olması değil olup [alt ağınızın bir uç nokta olduğunu doğrulayın][sql-db-vnet-service-endpoint-rule-powershell-md-a-verify-subnet-is-endpoint-ps-100].

<a name="a-portal-steps-for-vnet-rule-200" />

### <a name="azure-portal-steps"></a>Azure portal adımları

1. Oturum [Azure portalında][http-azure-portal-link-ref-477t].

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

7.  Sonuçta elde edilen sanal ağ kuralı güvenlik duvarı bölmesinde görürsünüz.

    ![Yeni Kural güvenlik duvarı bölmesinde görürsünüz.][image-portal-firewall-vnet-result-rule-30-png]


> [!NOTE]
> Aşağıdaki durumlar veya durumları için kurallar geçerlidir:
> - **Hazır:** başlattığınız işleminin başarılı olduğunu gösterir.
> - **Başarısız oldu:** , başlatılan işlem başarısız oldu belirtir.
> - **Silindi:** yalnızca silme işlemi için geçerlidir ve kural silindi ve artık geçerli olduğunu gösterir.
> - **Devam ediyor:** işlemi devam ediyor belirtir. Eski kural, işlem bu durumda olduğunda geçerlidir.


<a name="anchor-how-to-links-60h" />

## <a name="related-articles"></a>İlgili makaleler

- [Azure sanal ağ hizmet uç noktaları][vm-virtual-network-service-endpoints-overview-649d]
- [Azure SQL veritabanı sunucu düzeyinde ve veritabanı düzeyinde güvenlik duvarı kuralları][sql-db-firewall-rules-config-715d]

Azure SQL veritabanı için sanal ağ kuralı özelliği geç Eylül 2017'de kullanıma sunulmuştur.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure SQL veritabanı için bir sanal ağ hizmet uç noktası ve bir sanal ağ kuralı oluşturmak için PowerShell kullanın.][sql-db-vnet-service-endpoint-rule-powershell-md-52d]
- [Sanal ağ kuralları: İşlem] [ rest-api-virtual-network-rules-operations-862r] REST API'leri



<!-- Link references, to images. -->

[image-portal-firewall-vnet-add-existing-10-png]: media/sql-database-vnet-service-endpoint-rule-overview/portal-firewall-vnet-add-existing-10.png

[image-portal-firewall-create-update-vnet-rule-20-png]: media/sql-database-vnet-service-endpoint-rule-overview/portal-firewall-create-update-vnet-rule-20.png

[image-portal-firewall-vnet-result-rule-30-png]: media/sql-database-vnet-service-endpoint-rule-overview/portal-firewall-vnet-result-rule-30.png



<!-- Link references, to text, Within this same Github repo. -->

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



<!-- Link references, to text, Outside this Github repo (HTTP). -->

[http-azure-portal-link-ref-477t]: https://portal.azure.com/

[rest-api-virtual-network-rules-operations-862r]: https://docs.microsoft.com/rest/api/sql/virtualnetworkrules



<!-- ??2
#### Syntax related articles
- REST API Reference, including JSON

- Azure CLI

- ARM templates
-->
