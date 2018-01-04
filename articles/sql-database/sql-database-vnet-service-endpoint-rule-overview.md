---
title: "Sanal ağ hizmet uç noktaları ve Azure SQL veritabanı için kuralları | Microsoft Docs"
description: "Bir alt ağ, sanal ağ hizmeti uç noktası olarak işaretleyin. Ardından uç noktası olarak bir sanal ağ kuralı Azure SQL veritabanınıza ACL. SQL veritabanı sonra tüm sanal makineler ve alt ağdaki diğer düğümlere gelen iletişimi kabul eder."
services: sql-database
documentationcenter: 
author: MightyPen
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: VNet Service endpoints
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: On Demand
ms.date: 11/13/2017
ms.author: genemi
ms.openlocfilehash: ce223fbd6a69bc789f902f9478b5255edfd44844
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2017
---
# <a name="use-virtual-network-service-endpoints-and-rules-for-azure-sql-database"></a>Azure SQL veritabanı için sanal ağ hizmet uç noktaları ve kurallarını kullan

*Sanal ağ kuralları* Azure SQL veritabanı sunucunuzun sanal ağlarda bulunan belirli alt ağları gönderildiği iletişim kabul edip etmeyeceğini denetleyen bir güvenlik duvarı güvenlik özelliğidir. Bu makalede neden sanal ağ kuralı özellik bazen güvenli bir şekilde Azure SQL veritabanınıza iletişime izin vermek için en iyi seçenektir açıklanmaktadır.

Bir sanal ağ kuralı oluşturmak için öncelikle olmalıdır bir [sanal ağ hizmeti uç noktası] [ vm-virtual-network-service-endpoints-overview-649d] kuralın başvurmak.


> [!NOTE]
> Azure SQL veritabanı için bu özellik Önizleme Azure genel bulut tüm bölgelerde kullanılabilir.

#### <a name="how-to-create-a-virtual-network-rule"></a>Bir sanal ağ kuralı oluşturma

Yalnızca bir sanal ağ kuralı oluşturursanız, şimdi adımları ve açıklama atlayabilirsiniz [bu makalenin ilerisinde yer](#anchor-how-to-by-using-firewall-portal-59j).






<a name="anch-terminology-and-description-82f" />

## <a name="terminology-and-description"></a>Terminoloji ve açıklaması

**Sanal ağ:** Azure aboneliğinizle ilişkili sanal ağına sahip olabilir.

**Alt ağ:** içeren bir sanal ağ **alt ağlar**. Tüm Azure sanal olan makineleri (VM'ler) alt ağlara atanır. Bir alt ağ, birden çok VM veya diğer işlem düğümleri içerebilir. Erişime izin vermek için güvenlik yapılandırmadığınız sürece sanal ağınızın dışında olan düğümleri sanal ağınıza erişemez işlem.

**Sanal Ağ Hizmeti uç noktası:** A [sanal ağ hizmeti uç noktası] [ vm-virtual-network-service-endpoints-overview-649d] özellik değerleri içeren bir veya daha fazla resmi Azure hizmeti tür adları bir alt ağ. Bu makalede biz tür adını ilgilendiğiniz **Microsoft.Sql**, adlandırılmış SQL Database, Azure hizmetine başvurduğu.

**Sanal ağ kuralı:** SQL veritabanı sunucunuz için bir sanal ağ kuralı SQL Database sunucunuza erişim denetim listesinde (ACL) listelenen bir alt ağıdır. ACL SQL veritabanınızın olması için alt ağı içermesi gerekir **Microsoft.Sql** türü adı.

Bir sanal ağ kuralı alt ağda yer her düğüm gelen iletişimleri kabul etmek için SQL veritabanı sunucunuzun söyler.







<a name="anch-benefits-of-a-vnet-rule-68b" />

## <a name="benefits-of-a-virtual-network-rule"></a>Bir sanal ağ kuralı yararları

Eylem kazanana kadar alt ağlar üzerindeki VM'ler, SQL veritabanıyla iletişim kuramıyor. İletişim kuran bir sanal ağ kuralı oluşturulmasını eylemdir. Sanal ağ kuralı yaklaşım seçme stratejinin güvenlik duvarı tarafından sunulan rakip güvenlik seçenekleri içeren bir karşılaştırma karşıtlıklı tartışma gerektirir.

#### <a name="a-allow-access-to-azure-services"></a>A. Azure hizmetlerine erişime izin ver

Güvenlik Duvarı bölmesi olan bir **açık/kapalı** etiketli düğmesi **Azure hizmetlerine erişime izin ver**. **ON** ayarı tüm Azure IP adresleri ve tüm Azure alt iletişimlerinden sağlar. Bu Azure IP veya alt size ait değil. Bu **ON** ayardır SQL veritabanınız olmasını istediğiniz daha büyük olasılıkla daha açık. Sanal ağ kuralı özelliği çok daha ayrıntılı bir denetim sunar.

#### <a name="b-ip-rules"></a>B. IP kuralları

SQL veritabanı güvenlik duvarı iletişimi SQL veritabanına kabul edilen IP adres aralıklarını belirtmenize olanak tanır. Bu yaklaşım, Azure özel ağı dışında bulunan kararlı IP adresleri için uygundur. Ancak Azure özel ağ içindeki birçok düğümleri ile yapılandırılan *dinamik* IP adresleri. Dinamik IP adresleri, VM ne zaman yeniden gibi değiştirebilirsiniz. Bir üretim ortamında bir güvenlik duvarı kuralı dinamik bir IP adresi belirtmek üzere folly olacaktır.

Elde ederek IP seçeneği hurda bir *statik* , VM için IP adresi. Ayrıntılar için bkz [Azure portalını kullanarak bir sanal makine için özel IP adreslerini yapılandırın][vm-configure-private-ip-addresses-for-a-virtual-machine-using-the-azure-portal-321w].

Ancak, statik IP yaklaşım yönetmek zor hale gelebilir ve ölçekte yapıldığında pahalıdır. Sanal ağ kuralları oluşturmak ve yönetmek için daha kolay.

#### <a name="c-cannot-yet-have-sql-database-on-a-subnet"></a>C. SQL veritabanı bir alt ağda henüz sahip olamaz

Azure SQL veritabanı sunucunuzun sanal ağınızdaki bir alt ağdaki bir düğüm varsa, sanal ağ içindeki tüm düğümleri, SQL veritabanı ile iletişim kuramadı. Bu durumda, Vm'leriniz herhangi bir sanal ağ kuralları veya IP kuralları gerek kalmadan SQL veritabanı ile iletişim kuramadı.

Ancak Eylül 2017 sürümünden itibaren Azure SQL veritabanı hizmetinin henüz bir alt ağa atanabilir hizmetleri arasında değil.






<a name="anch-details-about-vnet-rules-38q" />

## <a name="details-about-virtual-network-rules"></a>Sanal ağ kurallarıyla ilgili ayrıntıları

Bu bölümde, sanal ağ kuralları hakkında birkaç Ayrıntılar açıklanmaktadır.

#### <a name="only-one-geographic-region"></a>Yalnızca tek bir coğrafi bölge

Her sanal ağ hizmeti uç noktası, yalnızca bir Azure bölgesi için geçerlidir. Uç nokta, alt ağından gelen iletişimi kabul etmek üzere diğer bölgeler etkinleştirmez.

Tüm sanal ağ kuralı, temel alınan uç noktasında uygulandığı bölge sınırlıdır.

#### <a name="server-level-not-database-level"></a>Sunucu düzeyinde, veritabanı düzeyi

Tüm Azure SQL Database sunucunuza, yalnızca belirli bir veritabanını sunucu üzerindeki her sanal ağ kuralı uygular. Diğer bir deyişle, sunucu düzeyinde-, veritabanı düzeyi değil, sanal ağ kuralı uygular.

- Buna karşılık, IP kuralları ya da düzeyinde uygulayabilirsiniz.

#### <a name="security-administration-roles"></a>Güvenlik Yönetim rolleri

Sanal ağ hizmet uç noktaları Yönetim güvenlik rollerini ayrılması yoktur. Eylem her aşağıdaki rolleri gereklidir:

- **Ağ Yöneticisi:** &nbsp; noktadaki açın.
- **Veritabanı Yöneticisi:** &nbsp; erişim denetim listesi (ACL) SQL veritabanı sunucusuna belirtilen alt ağ Ekle şekilde güncelleştirin.

*RBAC alternatif:*

Ağ Yöneticisi ve veritabanı yöneticisi rollerini sanal ağ kuralları yönetmek için gerekli olandan daha fazla özelliklere sahiptir. Yalnızca bir alt kümesini yeteneklerini gereklidir.

Kullanma seçeneğiniz [rol tabanlı erişim denetimi (RBAC)] [ rbac-what-is-813s] özellikleri yalnızca gerekli kısmı sahip tek bir özel rolü oluşturmak için Azure. Özel rol ağ yönetici veya veritabanı yöneticisine içeren yerine kullanılabilir. Güvenlik açıklarını'nın yüzey alanını, diğer iki ana yönetici rolü için kullanıcı ekleme ve özel bir rol için bir kullanıcı eklerseniz düşüktür.

> [!NOTE]
> Bazı durumlarda, Azure SQL Database ve sanal alt farklı Aboneliklerde bulunuyor. Bu durumlarda, aşağıdaki yapılandırmaları emin olmalısınız:
> - Her iki aboneliğin aynı Azure Active Directory Kiracı içinde olmalıdır.
> - Kullanıcının hizmet uç noktaları etkinleştirme ve sanal alt verilen sunucuya ekleme gibi işlemleri başlatmak için gerekli izinlere sahip.

## <a name="limitations"></a>Sınırlamalar

Azure SQL veritabanı için sanal ağ kuralları özelliği aşağıdaki sınırlamalara sahiptir:

- Şu anda Azure Web uygulaması bir alt ağ olan **hizmet uç noktaları** üzerinde mu henüz beklenen şekilde işlevi açık değil. Bu işlevselliği etkinleştirmek için çalışıyoruz.
    - Bu özellik tamamen uygulanana kadar hizmet uç noktaları için SQL açık olmayan farklı bir alt Web uygulamanızı gitme öneririz.

- SQL veritabanınız için Güvenlik Duvarı'nda, her sanal ağ kuralı bir alt ağ başvurur. Bu başvurulan tüm alt ağlar SQL veritabanını barındıran aynı coğrafi bölgede barındırılması gerekir.

- Her Azure SQL veritabanı sunucusu, belirli herhangi bir sanal ağ için 128 ACL girişleri kadar olabilir.

- Sanal ağ kuralları yalnızca Azure Resource Manager sanal ağlar için geçerlidir; ve değil [Klasik dağıtım modeli] [ arm-deployment-model-568f] ağlar.

- Azure SQL veritabanı kapatma ON sanal ağ hizmet uç noktaları ayrıca MySQL ve PostGres Azure Hizmetleri için uç noktaları sağlar. Ancak, uç noktaları ON ile MySQL veya Postgres örneklerine uç noktalarından bağlanma girişimi başarısız olur.
    - Bu MySQL temel nedeni ve PostGres başarısız şu anda desteklemiyor.

- Güvenlik Duvarı'nda, IP adres aralıklarını aşağıdaki ağ öğelere uygulanır, ancak sanal ağ kuralları yapın:
    - [Siteden siteye (S2S) sanal özel ağ (VPN)][vpn-gateway-indexmd-608y]
    - Aracılığıyla şirket içi [ExpressRoute][expressroute-indexmd-744v]

#### <a name="expressroute"></a>ExpressRoute

Ağ kullanımı aracılığıyla Azure ağına bağlı olup olmadığını [ExpressRoute][expressroute-indexmd-744v], her bağlantı hattı adresindeki Microsoft Edge iki ortak IP adresi ile yapılandırılır. İki IP adresi Microsoft Services gibi Azure depolama için Azure ortak eşleme kullanarak bağlanmak için kullanılır.

Azure SQL veritabanı bağlantı hattınız arasında iletişimi izin vermek için ortak IP adresleri, bağlantı hatları için IP ağ kuralları oluşturmanız gerekir. Expressroute bağlantı hattı ortak IP adreslerini bulmak için Azure portalını kullanarak ExpressRoute ile bir destek bileti açın.


<!--
FYI: Re ARM, 'Azure Service Management (ASM)' was the old name of 'classic deployment model'.
When searching for blogs about ASM, you probably need to use this old and now-forbidden name.
-->

## <a name="impact-of-removing-allow-all-azure-services"></a>'Tüm Azure hizmetlerini izin ver' kaldırmanın etkisi

Çok sayıda kullanıcı, kaldırmak istediğiniz **tüm Azure hizmetlerini izin** kendi Azure SQL sunucularından ve bir sanal ağ güvenlik duvarı kuralı ile değiştirin.
Ancak bu kaldırma aşağıdaki Azure SQLDB özellikleri etkiler:

#### <a name="import-export-service"></a>İçeri aktarma dışarı aktarma hizmeti
Azure vm'lerinde Azure SQLDB alma dışarı aktarma hizmeti çalışır. Bu sanal makineleri, sanal ağınızda değildir ve bu nedenle, veritabanına bağlanırken bir Azure IP alın. Kaldırma **tüm Azure hizmetlerini izin** bu Vm'lere veritabanlarınızı erişmek mümkün olmaz.
Soruna geçici çözüm bulabilirsiniz. BACPAC alma çalıştırmak veya DACFx API'sini kullanarak kodunuzda doğrudan dışa aktarın. Bu VNet-güvenlik duvarı kuralı ayarladığınız alt ağda bir VM'de dağıtılır emin olun.

#### <a name="sql-database-query-editor"></a>SQL veritabanı sorgu Düzenleyicisi
Azure vm'lerinde Azure SQL veritabanı sorgu Düzenleyicisi'ni dağıtılır. Bu sanal makineleri, sanal ağınızda değildir. Bu nedenle sanal makineleri, veritabanına bağlanırken bir Azure IP alın. Kaldırma **tüm Azure hizmetlerini izin**, bu sanal makineleri edemeyecek veritabanlarınızı erişmek.

#### <a name="table-auditing"></a>Tablo denetim
Şu anda SQL veritabanınızın denetimini etkinleştirmek için iki yolu vardır. Azure SQL sunucusunda hizmet uç noktaları etkinleştirdikten sonra tablo denetimi başarısız olur. Burada azaltma Blob denetimi taşımaktır.


## <a name="impact-of-using-vnet-service-endpoints-with-azure-storage"></a>Azure storage ile VNet hizmet uç noktaları kullanma etkisi

Azure depolama, depolama hesabı bağlantı sınırlamak izin veren aynı özellik uyguladı.
Bir Azure SQL Server tarafından kullanılan bir depolama hesabıyla bu özelliği kullanmayı seçerseniz, sorunlar çalıştırabilirsiniz. Sonraki bir listesi ve bu tarafından etkilenen Azure SQLDB özelliklerinin tartışma olduğu.

#### <a name="azure-sqldw-polybase"></a>Azure SQLDW PolyBase
PolyBase, veri depolama hesaplarından Azure SQLDW yüklemek için yaygın olarak kullanılır. Yalnızca bir sanal alt ağ kümesi erişim verilerini yükleme depolama hesabı sınırları, hesap PolyBase bağlantısını çalışmamasına neden olur.

#### <a name="azure-sqldb-blob-auditing"></a>Azure SQLDB Blob denetimi
BLOB denetimi denetim günlüklerini kendi depolama hesabına iter. Bu depolama hesabını NCEKİ Hizmeti uç noktaları özelliğini kullanıyorsa, depolama hesabı Azure SQLDB bağlantısını kesintiye uğrar.


## <a name="errors-40914-and-40615"></a>40914 ve 40615 hataları

Bağlantı hatası 40914 ilişkili *sanal ağ kuralları*, güvenlik duvarı bölmede Azure Portalı'nda belirtildiği gibi. Hata 40615 benzer, ilişkili dışında *IP adresi kuralları* Güvenlik Duvarı'nda.

#### <a name="error-40914"></a>Hata 40914

*İleti metni:* sunucu açamıyor '*[sunucu-adı]*' oturum açma tarafından istenen. İstemci, sunucuya erişimine izin verilmiyor.

*Hata açıklaması:* istemci sanal ağ sunucu uç noktasına sahip bir alt ağda değil. Ancak Azure SQL veritabanı sunucusunun SQL veritabanıyla iletişim kurmak için sağ alt veren bir sanal ağ kuralı yok.

*Hata çözümleme:* üzerinde güvenlik duvarı bölmesinde sanal ağ kuralları denetlemek için kullanım Azure portalının [bir sanal ağ kuralı ekleyin](#anchor-how-to-by-using-firewall-portal-59j) alt ağ.

#### <a name="error-40615"></a>Hata 40615

*İleti metni:* sunucu '{0}' oturum açma tarafından istenen açamıyor. IP adresi '{1}' olan istemcinin sunucuya erişmek için izin verilmiyor.

*Hata açıklaması:* istemci Azure SQL veritabanı sunucusuna bağlanmak için yetkili olmayan bir IP adresinden bağlanmaya çalışıyor. Sunucu güvenlik duvarı verilen IP adresinden SQL veritabanıyla iletişim kurmak bir istemci veren IP adresi kuralı yok.

*Hata çözünürlük:* IP kural olarak istemcinin IP adresini girin. Azure portalında güvenlik duvarı bölmesini kullanarak bunu.


Birkaç SQL veritabanı hata iletilerinin listesini belgelenen [burada][sql-database-develop-error-messages-419g].





<a name="anchor-how-to-by-using-firewall-portal-59j" />

## <a name="portal-can-create-a-virtual-network-rule"></a>Portal bir sanal ağ kuralı oluşturabilirsiniz

Bu bölümde nasıl kullanabileceğinizi gösteren [Azure portal] [ http-azure-portal-link-ref-477t] oluşturmak için bir *sanal ağ kuralı* Azure SQL veritabanınızda. Kural olarak etiketlenmiş belirli bir alt ağ gelen iletişimi kabul etmek için SQL veritabanı söyler bir *sanal ağ hizmeti uç noktası*.

#### <a name="powershell-alternative"></a>PowerShell alternatif

Bir PowerShell komut dosyası ayrıca sanal ağ kuralları oluşturabilirsiniz. Önemli cmdlet **yeni AzureRmSqlServerVirtualNetworkRule**. İlgileniyorsanız, bkz: [Azure SQL veritabanı için bir sanal ağ hizmeti uç noktası ve kuralı oluşturmak için PowerShell][sql-db-vnet-service-endpoint-rule-powershell-md-52d].

#### <a name="prerequisites"></a>Önkoşullar

Belirli sanal ağ hizmeti uç noktası ile etiketli bir alt ağ zaten olmalıdır *türü adı* Azure SQL veritabanı ilgilidir.

- İlgili uç nokta türü adı **Microsoft.Sql**.
- Alt ağınız tür adıyla etiketli değil, bkz: [doğrulayın alt ağınızı bir uç nokta][sql-db-vnet-service-endpoint-rule-powershell-md-a-verify-subnet-is-endpoint-ps-100].

<a name="a-portal-steps-for-vnet-rule-200" />

### <a name="azure-portal-steps"></a>Azure portal adımları

1. Oturum [Azure portal][http-azure-portal-link-ref-477t].

2. Portala gider **SQL sunucuları** &gt; **güvenlik duvarı / sanal ağlar**.

3. Ayarlama **Azure hizmetlerine erişime izin ver** off denetim.

    > [!IMPORTANT]
    > AÇIK olarak ayarlanmış denetim değiştirmeden bırakırsanız, Azure SQL veritabanı sunucunuzun ağdaki hiçbir alt gelen iletişimi kabul eder. AÇIK olarak ayarlanmış denetim bırakarak bir güvenlik bakış açısı aşırı erişimden olabilir. SQL veritabanı'nın sanal ağ kuralı özelliği ile koordinasyon halinde Microsoft Azure sanal ağ hizmeti uç noktası özelliği, güvenlik yüzey alanınızı birlikte azaltabilir.

4. Tıklatın **+ Ekle varolan** denetlemek, buna **sanal ağlar** bölümü.

    ![Ekle'yi tıklatın (alt ağ uç noktası, SQL kural olarak) mevcut.][image-portal-firewall-vnet-add-existing-10-png]

5. Yeni **oluştur/güncelleştir** bölmesinde, Azure kaynaklarınızı adlarını denetimleriyle doldurun.

    > [!TIP]
    > Doğru içermelidir **adres ön eki** alt ağınız için. Değer portalında bulabilirsiniz.
    > Gezinme **tüm kaynakları** &gt; **tüm türleri** &gt; **sanal ağlar**. Filtreyi sanal ağlarınıza görüntüler. Sanal ağınızı tıklayın ve ardından **alt ağlar**. **Adres ARALIĞI** sütununda ihtiyacınız adres öneki.

    ![Yeni kural için alanları doldurun.][image-portal-firewall-create-update-vnet-rule-20-png]

6. Tıklatın **Tamam** bölmesinin yakın düğmesi.

7.  Sonuçta elde edilen sanal ağ kuralı üzerinde güvenlik duvarı bölmesine bakın.

    ![Yeni Kural üzerinde güvenlik duvarı bölmesine bakın.][image-portal-firewall-vnet-result-rule-30-png]


> [!NOTE]
> Aşağıdaki durumlar veya durumları için kurallar geçerlidir:
> - **Hazır:** başlattığınız işleminin başarılı olduğunu gösterir.
> - **Başarısız oldu:** , başlatılan işlem başarısız oldu belirtir.
> - **Silindi:** yalnızca silme işlemi için geçerlidir ve kuralı silindi ve artık geçerli olduğunu gösterir.
> - **Devam ediyor:** işlemi devam ediyor belirtir. İşlemi bu durumdayken eski kuralı uygular.


<a name="anchor-how-to-links-60h" />

## <a name="related-articles"></a>İlgili makaleler

- [Azure sanal ağı hizmet uç noktaları][vm-virtual-network-service-endpoints-overview-649d]
- [Azure SQL veritabanı sunucusu ve veritabanı düzeyi güvenlik duvarı kuralları][sql-db-firewall-rules-config-715d]

Azure SQL veritabanı için sanal ağ kuralı özelliği geç Eylül 2017 içinde kullanılabilir hale geldi.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure SQL veritabanı için bir sanal ağ hizmeti uç noktası ve bir sanal ağ kuralı oluşturmak için PowerShell kullanın.][sql-db-vnet-service-endpoint-rule-powershell-md-52d]


<!-- Link references, to images. -->

[image-portal-firewall-vnet-add-existing-10-png]: media/sql-database-vnet-service-endpoint-rule-overview/portal-firewall-vnet-add-existing-10.png

[image-portal-firewall-create-update-vnet-rule-20-png]: media/sql-database-vnet-service-endpoint-rule-overview/portal-firewall-create-update-vnet-rule-20.png

[image-portal-firewall-vnet-result-rule-30-png]: media/sql-database-vnet-service-endpoint-rule-overview/portal-firewall-vnet-result-rule-30.png



<!-- Link references, to text, Within this same Github repo. -->

[arm-deployment-model-568f]: ../azure-resource-manager/resource-manager-deployment-model.md

[expressroute-indexmd-744v]: ../expressroute/index.md

[rbac-what-is-813s]: ../active-directory/role-based-access-control-what-is.md

[sql-db-firewall-rules-config-715d]: sql-database-firewall-configure.md

[sql-database-develop-error-messages-419g]: sql-database-develop-error-messages.md

[sql-db-vnet-service-endpoint-rule-powershell-md-52d]: sql-database-vnet-service-endpoint-rule-powershell.md

[sql-db-vnet-service-endpoint-rule-powershell-md-a-verify-subnet-is-endpoint-ps-100]: sql-database-vnet-service-endpoint-rule-powershell.md#a-verify-subnet-is-endpoint-ps-100

[vm-configure-private-ip-addresses-for-a-virtual-machine-using-the-azure-portal-321w]: ../virtual-network/virtual-networks-static-private-ip-arm-pportal.md

[vm-virtual-network-service-endpoints-overview-649d]: https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview

[vpn-gateway-indexmd-608y]: ../vpn-gateway/index.md



<!-- Link references, to text, Outside this Github repo (HTTP). -->

[http-azure-portal-link-ref-477t]: https://portal.azure.com/




<!-- ??2
#### Syntax related articles
- REST API Reference, including JSON

- Azure CLI

- ARM templates
-->
