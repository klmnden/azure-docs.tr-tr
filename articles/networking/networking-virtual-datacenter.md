---
title: 'Microsoft Azure sanal Datacenter: Bir ağ perspektif | Microsoft Docs'
description: Sanal veri merkezinizde Azure oluşturmayı öğrenin
services: networking
author: tracsman
manager: rossort
tags: azure-resource-manager
ms.service: virtual-network
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/3/2018
ms.author: jonor
ms.openlocfilehash: a62d52e30b04b525dc8ff685ed6c3033d6029542
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
ms.locfileid: "33942454"
---
# <a name="microsoft-azure-virtual-datacenter-a-network-perspective"></a>Microsoft Azure sanal Datacenter: Ağ perspektifi
**Microsoft Azure**: hızlı hareket, tasarruf, şirket içi uygulamaları ve verileri tümleştirme

## <a name="overview"></a>Genel Bakış
Azure uygulamalarını şirket içi geçirme, hatta önemli değişiklikler (bir yaklaşım "kaldırın ve shift" denir), güvenli ve ekonomik altyapısının yararlarını kuruluşların sağlar. Ancak, en iyi çeviklik bulut ile mümkün kılmak için kuruluşlar Azure özelliklerinden yararlanmak için kendi mimarileri gelişmesi. Microsoft Azure hiper ölçekli hizmetler ve altyapı, Kurumsal düzeydeki özellikleri ve güvenilirlik ve karma bağlantı için birçok seçenek sunar. Müşteriler, Internet üzerinden veya özel ağ bağlantısı sağlayan Azure ExpressRoute ile bu bulut hizmetlerine erişmek seçebilirsiniz. Microsoft Azure platformu sorunsuz bir şekilde altyapılarını bulutunu oluşturmak ve genişletmek için çok katmanlı mimarileri olanak tanır. Ayrıca, Microsoft iş ortakları, güvenlik hizmetleri ve Azure'da çalışması için optimize sanal gereçler sunarak gelişmiş özellikler sağlar.

Bu makalede n tüm buluta taşıma hakkında düşünürken, birçok müşterilerin karşılaştığı desenleri ve mimari ölçek, performans ve güvenlik sorunları çözmek için kullanılan tasarımları genel bakış sağlar. Farklı Kuruluş BT rolleri yönetim içine sığması nasıl bir genel bakış ve sistem İdaresi ayrıca güvenlik gereksinimleri için Vurgu ile ele alınan ve en iyi duruma getirme maliyeti.

## <a name="what-is-a-virtual-data-center"></a>Sanal veri merkezi nedir?
Erken gün içinde bulut çözümleri ortak spektrumun içinde ana bilgisayar tek, nispeten yalıtılmış uygulamalar için tasarlanmıştır. Bu yaklaşım da birkaç yıldır çalışmıştır. Ancak, bulutun avantajlarını çözümleri belirgin hale geldi ve birden çok büyük ölçekli iş yüklerini olan ve adresleme güvenlik, güvenilirlik, performans, bulut üzerinde barındırılan dağıtımların bir maliyetlerinin veya daha fazla bölgeler bulut hizmetinin yaşam döngüsü boyunca önemli hale geldi.

Aşağıdaki bulut dağıtım diyagramı (sarı kutusu) iş yüklerinde güvenlik açıkları (kırmızı kutu) ve en iyi duruma getirme ağ sanal Gereçleri yer bazı örnekler gösterilmektedir.

[![0]][0]

Sanal veri merkezi (vDC), kurumsal iş yüklerinin ve büyük ölçekli uygulamalar genel bulutta destekleme olduğunda ortaya çıkan sorunları uğraşmanız gereken desteklemek için ölçeklendirme için bu gerekliliğini gelen doğdu.

VDC yalnızca uygulama iş yükleri bulut ancak aynı zamanda ağ, güvenlik, yönetim ve altyapı (örneğin, DNS ve Dizin Hizmetleri) değil. Genellikle ayrıca bir şirket içi ağ veya veri merkezi dön özel bir bağlantı sağlar. Giderek daha fazla iş yüklerini Azure'a taşımak gibi bu iş yükleri yerleştirilir nesneleri ve destekleyici altyapının yapılandırmanızda önemlidir. Dikkatle kaynakları nasıl yapılandırıldığı hakkında düşünmeye "bağımsız veri akışı, güvenlik modelleri ve uyumluluk sorunları ayrı olarak yönetilmelidir iş yükü Adaları" yüzlerce artışı önleyebilirsiniz.

Sanal veri merkezi temelde ortak destekleyici İşlevler, özellikler ve altyapı ile ayrı ancak ilgili varlıklar koleksiyonudur. İş yüklerinizi tümleşik bir vDC görüntüleyerek, Ölçek, bileşeni ve veri akış merkezileşmeyi, daha kolay işlemleri, yönetim ve uyumluluk denetimleri yanı sıra ile en iyi duruma getirilmiş güvenlik ekonomileri nedeniyle daha az maliyet sağlarsınız.

> [!NOTE]
> VDC olduğunu anlamak önemlidir **değil** ayrık Azure ürün, ancak çeşitli özellikleri ve yetenekleri tam gereksinimlerinizi karşılayacak şekilde birleşimi. vDC kaynakları ve bulutta becerilerini en üst düzeye çıkarmak için bir iş yükleri ve Azure kullanımı hakkında düşünmeye yoludur. Sanal DC bu nedenle bir modüler kuruluş roller ve sorumluluklar uyarak Azure BT Hizmetleri oluşturmak nasıl bir yaklaşımdır.

VDC iş yüklerini ve uygulamaları Azure içine aşağıdaki senaryolar için alma kuruluşların yardımcı olabilir:

-   Birden çok ilişkili iş yüklerinin barındırma
-   Bir şirket içi ortamına geçirme iş yüklerini azure'a
-   Paylaşılan veya merkezi güvenliği ve erişim gereksinimleri iş yüklerinde uygulama
-   DevOps ve merkezi BT büyük bir kuruluş için uygun şekilde karıştırma

VDC, avantajları kilidini açmak için anahtarıdır (hub ve bağlı bileşen) merkezi bir topoloji Azure özelliklerinin bir karışımını: [Azure VNet][VNet], [Nsg'ler][NSG], [VNet eşlemesi][VNetPeering], [kullanıcı tanımlı yolları (UDR)][UDR]ve Azure kimlikle [rol temel erişim denetimi (RBAC)][RBAC].

## <a name="who-needs-a-virtual-data-center"></a>Sanal bir veri merkezi gerek duyan?
Birkaç Azure iş yüklerinin birden çok taşınması gereken herhangi bir Azure müşteriye ortak kaynakları kullanma hakkında düşünmekten yararlı olabilir. Büyüklük bağlı olarak, bir vDC oluşturmak için kullanılan bileşenleri ve desenler kullanılarak hatta tek tek uygulamalar yararlı olabilir.

Kuruluşunuzda bir merkezi BT, ağ güvenliği varsa ve/veya uyumluluk takım/departmanı, vDC İlkesi noktaları, vergi, ayrımı zorlamak ve uygulama vermiş kadar özgürlük ve gereksinimlerinize uygun olarak denetim ekipleri karşın temel alınan ortak bileşenlerinin bütünlüğünü sağlamak sağlayabilir.

DevOps için aradığınız kuruluşlar, Azure kaynaklarının yetkili cep sağlamak ve (ortak bir aboneliğe abonelik veya kaynak grubu) grubu içindeki toplam denetime sahip, ancak ağ ve güvenlik sınırları VNet ve kaynak grubu bir hub bir merkezi ilke tarafından tanımlandığı şekilde uyumlu kalmasını sağlamak için vDC kavramları kullanabilirler.

## <a name="considerations-on-implementing-a-virtual-data-center"></a>Sanal bir veri merkezi uygulama konusunda dikkat edilecek noktalar
VDC tasarlarken dikkate alınması gereken birkaç bileşendirler sorunlar vardır:

-   Kimlik ve Dizin Hizmetleri
-   Güvenlik altyapısı
-   Bulut için bağlantı
-   Bağlantı bulut içinde

##### <a name="identity-and-directory-service"></a>*Kimlik ve dizin hizmeti*
Kimlik ve Dizin Hizmetleri olan temel bir yönü, tüm veri merkezleri, her iki şirket içi ve bulut. Kimlik, erişim ve yetkilendirme hizmetleri vDC içinde tüm yönlerini ilgilidir. Yalnızca yetkili kullanıcılar ve işlemlerin Azure hesabınızı ve kaynaklarınızı erişim sağlamaya yardımcı olmak için Azure kimlik doğrulaması için çeşitli türlerde kimlik bilgilerini kullanır. Bunlar, (Azure hesabınıza erişmeniz için) parolalar, şifreleme anahtarları, dijital imzalar ve sertifikaları içerir. [*Azure çok faktörlü kimlik doğrulaması* (MFA)] [ MFA] Azure hizmetlerine erişmek için güvenlik ek katmanıdır. Azure MFA sağlar kolay doğrulama seçeneklerini aralıklı güçlü kimlik doğrulaması — telefon araması, SMS mesajı veya mobil uygulama bildirimi — ve müşteriler tercih yöntemini seçmenize olanak sağlar.

Büyük bir kuruluş tek tek kimlik yönetimi, kimlik doğrulama, yetkilendirme, rolleri ve ayrıcalıkları içinde veya vDC açıklar bir kimlik yönetimi işlem tanımlanması gerekir. Bu işlem hedefleri, maliyet, kapalı kalma süresi ve yinelenen manuel görevleri yükünü azaltırken güvenlik ve verimliliği artırmak için olmalıdır.

Kuruluşlar/farklı satırı-in-işletmeler için (LOB'lar) Hizmetleri zorlu bir karışımını gerektirebilir ve çalışanlar genellikle farklı projelerle söz konusu olduğunda farklı roller sahiptir. VDC farklı takımlar her iyi yönetimini çalıştıran sistemlere almak için belirli bir rol tanımları arasında iyi iş Birliği gerektirir. Matris ve sorumlulukları, erişim hakları çok karmaşık olabilir. VDC Identity management aracılığıyla gerçekleştirilir [ *Azure Active Directory* (AAD)] [ AAD] ve rol tabanlı erişim denetimi (RBAC).

Bir dizin hizmeti, bulma, yönetme, yönetme ve gündelik öğeleri ve ağ kaynakları düzenleme için bir paylaşılan bilgileri altyapısıdır. Bu kaynaklar, birimler, klasörleri, dosyaları, yazıcılar, kullanıcılar, gruplar, cihazları ve diğer nesneleri içerebilir. Ağ üzerindeki her bir kaynağın bir nesne dizin sunucusu tarafından kabul edilir. Bir kaynak hakkında bilgi, bu kaynak veya nesne ile ilişkili öznitelikleri koleksiyonu olarak depolanır.

Oturum açma için Azure Active Directory (AAD üzerinde) tüm Microsoft online iş hizmetlerini kullanır ve diğer kimlik gerekiyor. Azure Active Directory, temel dizin hizmetleri, gelişmiş kimlik yönetimi ve uygulama erişim yönetimi özelliklerini bir araya getiren kapsamlı bir kimlik ve erişim yönetimi bulut çözümüdür. AAD ile şirket içi Active tek oturum açma için tüm bulut tabanlı ve yerel olarak barındırılan (şirket içi) etkinleştirmek için dizin tümleştirilebilir uygulamalar. Şirket içi Active Directory kullanıcı özniteliklerini otomatik olarak AAD'ye eşitlenebilir.

Tek bir genel yönetici vDC tüm izinleri atamak için gerekli değildir. Bunun yerine her belirli bölüm (ya da Grup kullanıcılar veya dizin hizmetindeki Hizmetleri) vDC içinde kendi kaynaklarını yönetmek için gerekli izinlere sahip olabilirler. İzinleri yapılandırma Dengeleme gerektirir. Çok fazla izinler performans verimliliği engel ve çok az veya gevşek izinleri güvenlik riskleri artırabilirsiniz. Azure rol tabanlı erişim denetimi (RBAC) vDC kaynaklar için ayrıntılı erişim yönetimi sunarak bu sorunu gidermek için yardımcı olur.

##### <a name="security-infrastructure"></a>*Güvenlik altyapısı*
Bir vDC bağlamında güvenlik altyapısı çoğunlukla vDC'ın belirli bir sanal ağ kesimindeki trafiği ayrımı ilgili ve giriş ve çıkış denetleme vDC akar. Azure dağıtımlar arasında yetkisiz ve istenmeyen trafiği engelleyen çok kiracılı mimariyi temel alır, sanal ağ (VNet) yalıtım kullanarak, erişim denetim listeleri (ACL'ler), yük dengeleyicileri ve trafik akışını ilkeleri ile birlikte IP filtreleri. İç ağ trafiğini ağ adresi çevirisi (NAT) dış trafiğinden ayırır.

Azure doku Kiracı İş yükleri için altyapı kaynakları ayırır ve sanal makineleri (VM'ler) iletişimi yönetir. Azure hiper yönetici VM'ler arasında bellek ve işlem ayrımı zorunlu kılan ve güvenli bir şekilde yollar ağ trafiği için konuk işletim sistemi kiracılar.

##### <a name="connectivity-to-the-cloud"></a>*Bulut için bağlantı*
VDC müşteriler, iş ortakları ve/veya iç kullanıcılar için Hizmetleri sunmak için dış ağlarla bağlantısı gerekir. Bu genellikle yalnızca internet, aynı zamanda şirket içi ağlar ve veri merkezlerine bağlantı anlamına gelir.

Müşteriler ne denetlemek için güvenlik ilkelerini oluşturabilirsiniz ve belirli vDC barındırılan nasıl için/Internet'ten erişilebilir (ile filtreleme ve trafiğini incelemesi) ağ sanal Gereçleri kullanma ve özel ilkeleri ve (kullanıcı tanımlı Yönlendirme ve ağ güvenlik grupları) filtreleme ağ yönlendirme hizmetleri.

Kuruluşlar çoğunlukla VDC şirket içi veri merkezleri veya diğer kaynaklara bağlanmak gerekir. Azure ve şirket içi ağlar arasındaki bağlantıyı, bu nedenle etkili bir mimari tasarlarken önemli en boy olur. İşletmenin vDC ve şirket arasındaki bir bağlantısı oluşturmak için iki farklı yol vardır: transit Internet üzerinden ve/veya özel doğrudan bağlantılar.

Bir [ **Azure siteden siteye VPN** ] [ VPN] bir bağlantı şirket içi ağlar ve güvenli şifreli bağlantıları (IPSec/IKE tüneli) kurulan vDC arasında Internet üzerinden hizmetidir. Azure siteden siteye bağlantı oluşturmak hızlı, esnek ve tüm bağlantıları Internet üzerinden bağlandıkları herhangi başka tedarik gerektirmez.

[**ExpressRoute** ] [ ExR] vDC ve şirket içi ağlar arasında özel bağlantılar oluşturmanızı sağlayan bir Azure bağlantısı hizmetidir. ExpressRoute bağlantıları değil genel Internet üzerinden gidin ve daha yüksek güvenlik, güvenilirlik ve tutarlı gecikme süresi ile birlikte yüksek hızları (en fazla 10 GB/sn) sunar. ExpressRoute müşteriler özel bağlantıları ile ilişkili uyumluluk kuralları faydaları elde edebilirsiniz ExpressRoute olarak VDC için çok kullanışlıdır.

ExpressRoute bağlantılarını dağıtma ile bir ExpressRoute servis sağlayıcı çekici içerir. Hızla başlamanıza gerek müşteriler, bu başlangıçta siteden siteye VPN vDC arasında bağlantı kurmak için kullanılacak ortak olan şirket içi kaynakları ve ExpressRoute bağlantısı geçirme.

##### <a name="connectivity-within-the-cloud"></a>*Bağlantı bulut içinde*
[Sanal ağlar] [ VNet] ve [VNet eşlemesi] [ VNetPeering] temel ağ bağlantısı vDC içinde hizmetleridir. Bir sanal ağ yalıtım vDC kaynaklar için doğal bir sınır garanti eder ve farklı sanal ağlar aynı Azure bölgesinde içinde veya hatta bölgeler arasında intercommunication VNet eşlemesi sağlar. VNet içinde ve sanal ağlar arasındaki trafik denetimi erişim denetim listeleri belirtilen güvenlik kuralları kümesi eşleşmesi gerekir ([ağ güvenlik grubu][NSG]), [ağ sanal Gereçleri][NVA]ve özel yönlendirme tabloları ([UDR][UDR]).

## <a name="virtual-data-center-overview"></a>Sanal veri merkezi genel bakış

### <a name="topology"></a>Topoloji
Sanal veri merkezi içinde tek bir Azure bölgesi hub ve bağlı bileşen modeli genişletilmiş

[![1]][1]

Hub denetleyen ve farklı bölgelere arasında giriş ve/veya çıkış trafiği inceler merkezi bölgedir: Internet, şirket içi ve bağlı bileşen. Hub ve bağlı bileşen topolojisi BT departmanı yetersizliğini ve etkilenme olasılığını azaltırken merkezi bir konumda güvenlik ilkelerini zorlamak için etkili bir yol sağlar.

Hub'a bağlı bileşen tarafından tüketilen ortak hizmet bileşenleri içerir. Ortak merkezi hizmetlerinin bazı tipik örnekler şunlardır:

-   Kullanıcı kimlik doğrulaması üçüncü tarafların iş yükleri erişim spoke almadan önce güvenilmeyen ağlara erişim için gerekli Windows Active Directory altyapınızla (ilgili ADFS hizmeti)
-   Erişim kaynakları şirket içi ve Internet'te bağlı bileşen iş yükü için adlandırma çözümlemek için bir DNS hizmeti
-   Çoklu oturum açma iş yükleri üzerinde uygulamak için bir PKI altyapısı
-   Internet ve bağlı bileşen arasında akış denetimi (TCP/UDP)
-   Şirket içi ve bağlı bileşen arasında akış denetimi
-   İsterseniz, akış denetimi ve başka bir uç arasında

VDC birden fazla bağlı bileşen arasında paylaşılan hub altyapısını kullanarak genel maliyeti azaltır.

Her bağlı bileşen rol konak farklı türlerde iş yükleri için olabilir. Bağlı bileşen modüler bir yaklaşım için yinelenebilir dağıtımlara sağlayabilirsiniz (örneğin, geliştirme ve test, kullanıcı kabul testi, ön üretim ve üretim) aynı iş yüklerinin. Bağlı bileşen kurabilmeleri ve farklı gruplar (örneğin, DevOps gruplar), kuruluşunuzdaki etkinleştirmek için de kullanılabilir. Bir bağlı bileşen içinde temel iş yükü veya katmanı arasındaki trafik denetimi ile karmaşık çok katmanlı iş yükleri dağıtmak mümkündür.

##### <a name="subscription-limits-and-multiple-hubs"></a>Abonelik sınırlarını ve birden çok hub'ları
Azure üzerinde bir türü ne olursa olsun, her bileşen bir Azure aboneliği dağıtılır. Farklı Azure abonelikleri Azure bileşenlerinde yalıtım erişim ve yetkilendirme farklı düzeyleri ayarlama gibi farklı LOB'lar gereksinimlerini karşılayabilen.

Her BT sistemi olduğu gibi olsa da, platformlar sınırları tek bir vDC sayıda bağlı bileşen, ölçeği artırabilirsiniz. Hub dağıtımı kısıtlamaları ve sınırları olan belirli bir Azure aboneliği için bağlı (örneğin, VNet eşlemeler - max sayısını görmek [Azure aboneliği ve hizmet sınırları, kotaları ve kısıtlamaları] [ Limits] Ayrıntılar için). Burada sınırları bir sorun olabilir durumlarda mimarisi ölçeklendirebilirsiniz kadar hub ve bağlı bileşen oluşan bir küme için bir tek hub-bağlı bileşen modelini genişleterek daha fazla. Bir veya daha fazla Azure bölgelerindeki çok sayıda hub VNet eşlemesi, ExpressRoute veya siteden siteye VPN kullanarak birbirine bağlanabilir.

[![2]][2]

Çok sayıda hub giriş maliyeti ve yönetim çaba sisteminin artırır ve yalnızca ölçeklenebilirlik tarafından hizalı (örnek: sistem sınırlarını veya artıklık) ve bölgesel çoğaltma (örnekler: son kullanıcı performans veya olağanüstü durum kurtarma). Senaryolarda gerektiren çok sayıda hub, tüm hub'lara Hizmetleri işletimsel kolaylaştırmak için aynı kümesi sunmak çalışmalarımızı.

##### <a name="interconnection-between-spokes"></a>Bağlı bileşen arasında bağlantısı
Tek bir bağlı bileşen içinde karmaşık çok katmanları iş yükleri uygulamak mümkündür. Çok katmanlı yapılandırmaları aynı sanal ağ alt ağ (her katman için bir tane) kullanan ve Nsg'ler kullanarak akışları filtreleme uygulanabilir.

Öte yandan, bir Mimarı birden çok sanal ağlar arasında çok katmanlı bir iş yükü dağıtmak isteyebilirsiniz. VNet eşlemesi kullanarak, bağlı bileşen diğer bağlı bileşen aynı hub ya da farklı hub bağlanabilir. Bu senaryonun genel bir örnek veritabanı farklı bir spoke (VNet) içinde dağıtılırken uygulama işleme sunucularının bir spoke (VNet) olduğu durumdur. Bu durumda, VNet eşlemesi ile bağlı bileşen birbirine ve böylece hub'ı aracılığıyla transiting kaçınmak kolaydır. Hub atlayarak önemli güvenlik veya hub'ı yalnızca bulunabilecek noktaları denetimini atla değil emin olmak için bir mimari ve güvenlik inceledikten gerçekleştirilmesi gerekir.

[![3]][3]

Bağlı bileşen da hub'ı olarak davranan bir spoke birbirine bağlanabilir. Bu yaklaşımın iki düzeyli hiyerarşisi oluşturur: spoke (düzey 0) daha yüksek düzeydeki alt bağlı bileşen (düzey 1) hiyerarşi hub'ını haline gelir. Ya da Internet veya şirket içi ağ genişletme ulaşmak için merkezi hub trafiği iletmek vDC, bağlı bileşen gerekir. Bir hub iki düzeyde mimarisiyle basit hub bağlı ilişki yararları kaldırır karmaşık yönlendirme tanıtır.

Azure karmaşık topolojiler olanak sağlasa da, temel ilkeler vDC kavram Yinelenebilirlik ve Basitlik biridir. Yönetim çabasını en aza indirmek için basit hub bağlı önerilen vDC referans mimarisi tasarımdır.

### <a name="components"></a>Bileşenler
Sanal veri merkezi dört temel bileşen türü yapılır: **altyapı**, **Çevre ağları**, **iş yükleri**, ve **izleme**.

Her bileşen türü çeşitli Azure özellikleri ve kaynakları oluşur. VDC birden çok bileşenleri türleri ve aynı bileşen türü birden çok varyasyonları örneklerini yapılır. Örneğin, farklı uygulamaların temsil birçok farklı, mantıksal olarak ayrılmış iş yükü örnekleri olabilir. Sonuçta vDC oluşturmak için bu farklı bileşeni türlerinin ve örneklerinin'nı kullanın.

[![4]][4]

VDC önceki üst düzey mimarisini hub bağlı bileşen topolojisi farklı bölgelerde kullanılır farklı bileşen türleri gösterir. Diyagram mimarisi çeşitli kısımlarını altyapı bileşenlerini gösterir.

Bir (için bir şirket içi DC'ye veya vDC) iyi bir uygulama erişim hakları ve ayrıcalıkları grup tabanlı olmalıdır. Erişim ilkeleri ekipleri ve yapılandırma hataları en aza içinde yardımları genelinde tutarlı bir şekilde koruma grupları ile ilgili, tek tek kullanıcılar yerine yardımcı olur. Atama ve kullanıcıların ve uygun gruplardan kaldırma belirli bir kullanıcı ayrıcalıkları güncel tutulmasına yardımcı olur.

Her rol grubunu hangi Grup hangi iş yükü ile ilişkili olduğunu belirlemek kolaylaşır adları benzersiz bir önek olması gerekir. Örneğin, bir kimlik doğrulama hizmeti barındıran bir iş yükü adlı grupları olabilir *AuthServiceNetOps, AuthServiceSecOps, AuthServiceDevOps ve AuthServiceInfraOps.* Benzer şekilde merkezi rol veya rolleri belirli bir hizmeti ile ilgili değil, "Corp" ile başlayan için *CorpNetOps* örneğin.

Birçok kuruluş aşağıdaki grupların çeşitlemesi rolleri önemli bir dökümünü sağlamak için kullanın:

-   *Merkezi BT grubu (Corp)* (örneğin, ağ ve güvenlik) altyapı bileşenlerini denetlemek için sahiplik haklarına sahiptir ve bu nedenle abonelikte katılımcı rolünüz (ve hub denetime sahip) gerekir ve bağlı bileşen katkıda bulunan haklarına ağ. Büyük kuruluş sık'Bu yönetim sorumlulukları gibi birden çok ekibin arasında bölme; bir ağ işlemlerini (CorpNetOps) grubu (odaklanılan özel ağ üzerinde) ve güvenlik işlemleri (CorpSecOps) grubu (için güvenlik duvarı ve güvenlik ilkesini sorumlu). Bu belirli bir durumda, iki farklı gruplar bu özel roller atama için oluşturulması gerekir.
-   *Geliştirme & test (AppDevOps) grubu* (uygulamalarına veya hizmetlerine) iş yükleri dağıtmak için sorumluluk sahiptir. Bu grup, sanal makine katkıda bulunan rolü Iaas dağıtımları ve/veya bir veya daha fazla PaaS katkıda bulunanlar rolleri alır (bkz [Azure rol tabanlı erişim denetimi için yerleşik roller][Roles]). İsteğe bağlı olarak geliştirme ve test takım güvenlik ilkeleri (Nsg'ler) ve hub veya belirli bir bağlı bileşen içinde yönlendirme ilkeleri (UDR) üzerinde görünürlük olması gerekebilir. Bu nedenle, iş yükleri için katkıda bulunan rollerinin yanı sıra bu grubun Ayrıca ağ okuyucu rolü gerekir.
-   *İşlem ve bakımı grubu (CorpInfraOps veya AppInfraOps)* üretim iş yükleri yönetme sorumluluğunu sahip. Bu grup, aboneliği katkıda bulunan herhangi bir üretim aboneliğiniz iş yükleri üzerinde olması gerekir. Bunlar üretim ortamında olası yapılandırma sorunlarını çözmek için bir ek yükseltme destek ekibi grubunun rolüne sahip abonelik katkıda bulunan üretim ve merkezi hub abonelik gerekiyorsa bazı kuruluşlar da değerlendirebilir.

VDC hub'ı yönetme merkezi BT grupları için oluşturulan grupları iş yükü düzeyinde karşılık gelen gruplarınız şekilde yapılandırılmıştır. Hub kaynakları yönetmeye ek olarak yalnızca merkezi BT grupları dış erişim ve abonelik en üst düzey izinlerini denetlemek gerçekleştirebilir. Ancak, iş yükü grupları kaynakları ve merkezi BT üzerinde bağımsız olarak kendi sanal izinlerini denetlemek gerçekleştirebilir.

VDC güvenli bir şekilde birden çok proje farklı satırı-in-işletmeler arasında (LOB'lar) barındırmak için bölümlenmiş gerekir. Tüm projeleri farklı yalıtılmış ortamlara (geliştirme, UAT, üretim) gerektirir. Her bu ortamlar için ayrı Azure abonelikleri doğal yalıtımı sağlar.

[![5]][5]

Önceki diyagramda, bir kuruluşun projeleri, kullanıcılar, gruplar ve Azure bileşenleri dağıtıldığı ortamlar arasındaki ilişkiyi gösterir.

Genellikle, BT, bir ortam (veya katman) birden çok uygulama, dağıtılan ve yürütülen bir sistemdir. Büyük kuruluşlar kullanmak bir geliştirme ortamı (değişiklikler ilk olarak yapılan ve test) ve bir üretim ortamında (ne son kullanıcılar kullanın). Bu ortamlarda, genellikle birkaç hazırlık ortamları bunları aşamalı dağıtımı (ürün) izin verecek şekilde BETWEEN, test ve sorun oluşması durumunda geri alma ile ayrılır. Dağıtım mimarilerinin büyük ölçüde farklılık, ancak genellikle basic (Geliştirme) geliştirme sırasında başlayıp (üretim) üretim sırasında işlemi hala izlenir.

Çok katmanlı ortamlar bu tür için ortak bir mimarisini DevOps (geliştirme ve test etme), UAT (hazırlama) ve üretim ortamlarını oluşur. Kuruluşlar, tek veya birden çok Azure AD kiracılarıyla erişim ve bu ortamlara hakları tanımlamak için yararlanabilirsiniz. Önceki diyagramda söz konusu iki yerde farklı gösterilmektedir Azure AD kiracılarıyla kullanılır: DevOps ve UAT ve üretim için özel olarak diğer için bir tane.

Varlığı farklı Azure AD kiracılar ortamlar arasında ayrım zorlar. Aynı kullanıcı grubunu (örneğin, merkezi BT) gereken farklı bir kullanarak kimlik doğrulaması farklı bir AD Kiracı erişmek için URI roller veya bir projenin DevOps veya üretim ortamları izinleri değiştirin. Farklı ortamlar erişmek için farklı kullanıcı kimlik doğrulaması varlığını olası kesintileri ve insan hataları neden diğer sorunları azaltır.

#### <a name="component-type-infrastructure"></a>Bileşen türü: altyapısı
Bu bileşen türü destekleyici altyapının çoğunu bulunduğu olur. Ayrıca Burada, merkezi BT, güvenlik ve/veya uyumluluk ekipler, çoğu zaman.

[![6]][6]

Altyapı bileşenlerini arası vDC'in farklı bileşenleri arasında bir bağlantı sağlayın ve hub ve bağlı bileşen yok. Yönetme ve altyapı bileşenleri koruma sorumluluğunu genelde Orta atanan BT ve/veya güvenlik ekibine.

BT altyapı grubu, birincil görevleri kuruluş genelinde IP adresi şema tutarlılığı garanti için biridir. Özel IP adres alanı tutarlı olacak şekilde vDC ihtiyaçları için atanan ve şirket içi ağlarınız Atanan özel IP adresleri ile çakışan değil.

Şirket içi sınır yönlendiricileri veya Azure ortamlarda NAT IP adresi çakışmalarını önleyebilirsiniz olsa da, altyapı bileşenlerinizi zorluklar ekler. Yönetim basitliği IP sorunları işlemek için NAT kullanarak önerilen bir çözüm değildir şekilde vDC, anahtar amaçlarını biridir.

Altyapı bileşenlerini aşağıdaki işlevselliği içerir:

-   [**Kimlik ve Dizin Hizmetleri**][AAD]. Her kaynak türü azure'da erişimi dizin hizmetinde depolanan kimlik tarafından denetlenir. Dizin hizmeti yalnızca kullanıcılar listesi, aynı zamanda kaynaklara erişim hakkı belirli bir Azure aboneliğinin depolar. Bu hizmetler yalnızca bulut bulunabilir veya Active Directory'de depolanan şirket içi kimlik ile eşitlenebilir.
-   [**Sanal ağ**][VPN]. Sanal ağlar vDC ana bileşenlerinin biridir ve trafik yalıtımı sınır Azure platformunda oluşturmanızı sağlar. Tek veya birden çok sanal ağ kesimleri, her biri belirli bir IP ağ öneki (alt ağ), bir sanal ağ oluşur. Sanal ağ nerede Iaas sanal makineleri ve PaaS Hizmetleri özel iletişim kurup bir iç çevre alanı tanımlar. Sanal makineleri (ve PaaS Hizmetleri) bir sanal ağ sanal makineleri doğrudan iletişim olamaz (ve PaaS Hizmetleri) farklı bir sanal ağ, her iki sanal ağlar aynı abonelik kapsamında aynı müşteri tarafından oluşturulan olsa bile. Yalıtım müşteri sanal makineleri sağlar önemli bir özelliktir ve iletişim bir sanal ağ içindeki özel kalır.
-   [**UDR**][UDR]. Bir sanal ağ trafiğini, varsayılan sistem yönlendirme tablosuna dayalı tarafından yönlendirilir. Bir kullanıcı tanımlamak, ağ yöneticilerinin özel bir yönlendirme tablosu sistem yönlendirme tablosu davranışını üzerine ve sanal ağ içinde bir iletişim yolu tanımlamak için bir veya daha fazla alt ağlara ilişkilendirebilirsiniz yoldur. Belirli özel VM'ler ve/veya ağ sanal Gereçleri ve yük dengeleyici hub ve bağlı bileşen mevcut yoluyla spoke aktarım gelen o çıkış trafiği Udr'ler varlığını güvence altına alır.
-   [**NSG**][NSG]. Bir ağ güvenlik grubu, trafik IP kaynakları, hedef IP, protokolleri, IP kaynak bağlantı noktaları ve IP hedef bağlantı noktalarına filtreleme gibi davranan güvenlik kuralları listesidir. NSG bir alt ağ, bir Azure VM veya her ikisi ile ilişkili bir sanal NIC kartı uygulanabilir. Nsg'ler hub ve bağlı bileşen doğru akış denetimi uygulamak için gereklidir. NSG tarafından karşılanan güvenlik düzeyini hangi bağlantı noktalarını açmanız ve hangi amaçla bir işlev değil. Müşteriler, ana bilgisayar tabanlı güvenlik duvarları gibi IPtables veya Windows Güvenlik Duvarı ek VM başına filtrelerle uygulamalıdır.
-   [**DNS**][DNS]. VDC Vnet'lerdeki kaynaklar ad çözümlemesi DNS yoluyla sağlanır. Azure DNS hizmetleri her ikisi için de sağlar [ortak][DNS] ve [özel] [ PrivateDNS] ad çözümlemesi. Özel bölgeler bir sanal ağ içinde hem de sanal ağlar arasındaki ad çözümlemesi sağlar. Sanal ağlar aynı bölgede aynı zamanda bölgeler ve abonelikler arasında özel bölgeler yalnızca aralık olabilir. Ortak çözüm için Microsoft Azure altyapısı kullanılarak ad çözümlemesi sağlamanın Azure DNS DNS etki alanı için bir barındırma hizmeti sağlar. Etki alanlarınızı Azure'da barındırarak DNS kayıtlarınızı diğer Azure hizmetlerinde kullandığınız kimlik bilgileri, API’ler, araçlar ve faturalarla yönetebilirsiniz.
-   [**Abonelik** ] [ SubMgmt] ve [ **kaynak grubu Yönetim**][RGMgmt]. Bir abonelik birden çok kaynak grupları oluşturmak için doğal bir sınır tanımlar. Bir Abonelikteki kaynakların kaynak grupları adlı mantıksal kapsayıcılarında birlikte birleştirilir. Kaynak grubu vDC kaynakları düzenlemek için mantıksal bir grubu temsil eder.
-   [**RBAC**][RBAC]. RBAC, kullanıcıların yalnızca belirli bir alt kümesi eylemler için sınırlamanıza izin vererek belirli Azure kaynaklarına erişim hakkı birlikte harita Kurumsal rol için mümkündür. RBAC ile kullanıcılar, gruplar ve ilgili kapsam içinde uygulamalar için uygun rolü atayarak erişim izni verebilir. Rol atamasının kapsamı, bir Azure aboneliği, bir kaynak grubu veya tek bir kaynak olabilir. RBAC devralma izin verir. Bir üst kapsamda atanan bir rolü de içerdiği alt öğelerine erişim verir. RBAC kullanarak, görevlerini kurabilmeleri ve işlerini yapmak için gereksinim duydukları kullanıcılara sadece erişim miktarını verebilirsiniz. Örneğin, bir çalışan başka bir SQL veritabanları aynı abonelik içinde yönetebilirsiniz sırada bir Abonelikteki sanal makineleri yönetme izin vermek için RBAC kullanın.
-   [**VNet eşlemesi**][VNetPeering]. VDC altyapısı oluşturmak için kullanılan temel VNet eşlemesi, iki sanal ağlar (Vnet'ler) bağlanan bir mekanizma üzerinden Azure veri merkezi ağı veya bölgeler arasında Azure dünya çapında omurga kullanarak aynı bölgede özelliğidir.

#### <a name="component-type-perimeter-networks"></a>Bileşen türü: Çevre ağları
[Çevre ağı] [ DMZ] bileşenleri (çevre ağı olarak da bilinir) şirket içi veya fiziksel veri merkezi ağlarını, Internet'ten herhangi bağlantısı ile ağ bağlantısı sağlamak izin verir. Burada, ağ ve güvenlik muhtemelen çoğu zaman, ekipler de olabilir.

Gelen paket Kimlikleri ve IP'leri, güvenlik duvarı gibi hub'ında güvenlik Gereçleri bağlı bileşen arka uç sunucuların ulaşmadan önce akışına. Internet'e bağlı paketler iş yükleri de çevre ağındaki güvenlik Gereçleri ilke zorlaması, denetleme ve denetim amacıyla, ağ ayrılmadan önce akışına.

Çevre ağ bileşenleri aşağıdaki özellikleri sağlar:

-   [Sanal ağlar][VNet], [UDR][UDR], [NSG][NSG]
-   [Ağ sanal gereç][NVA]
-   [Yük Dengeleyici][ALB]
-   [Uygulama ağ geçidi][AppGW] / [WAF][WAF]
-   [Genel IP'ler][PIP]

Genellikle, Orta BT ve güvenlik ekiplerinden gereksinim tanımı ve Çevre ağları işlemlerini sorumluluğu.

[![7]][7]

Önceki diyagramda iki çevreyi erişimi olan zorlama hub'ı yerleşik hem de Internet ve bir şirket içi ağ gösterir. Tek bir hub Internet çevre ağına LOB'lar, Web uygulaması Güvenlik Duvarı (WAFs) ve/veya güvenlik duvarları birden çok grupları kullanarak çok sayıda desteklemek üzere ölçeği artırabilirsiniz.

[**Sanal ağlar** ] [ VNet] hub genellikle farklı türde filtreleme ve trafik için veya NVAs, WAFs ve Azure üzerinden internet'ten inceleniyor hizmetlerini barındırmak için birden çok alt ağa sahip bir VNet üzerine inşa edilmiştir Uygulama ağ geçidi.

[**UDR** ] [ UDR] UDR kullanarak, müşterilerin dağıtabilir güvenlik duvarları, Kimlikleri/IP'leri ve diğer sanal gereçler ve ağ trafiğini yönlendirmek güvenlik sınırı ilke zorlaması için bu güvenlik Gereçleri aracılığıyla Denetim ve denetleme. Udr'ler hub ve bağlı bileşen trafiği belirli özel VM'ler, ağ sanal Gereçleri ve vDC tarafından kullanılan yük dengeleyici transits güvence altına almak için oluşturulabilir. Doğru sanal gereçler VM'ler spoke yoldaki yerleşik kaynaklandığı bu trafiği garanti etmek için bir UDR spoke alt ağlarında sonraki atlama olarak iç yük dengeleyici ön uç IP adresini ayarlayarak ayarlanması gerekir. İç yük dengeleyicisi sanal gereçler (yük dengeleyici arka uç havuzu) için iç trafiği dağıtır.

[![8]][8]

[**Ağ sanal Gereçleri** ] [ NVA] hub'ı, çevre ağı ile internet erişimi normalde güvenlik duvarları ve/veya Web uygulaması Güvenlik Duvarı (WAFs) bir gruba yönetilir.

Bu uygulamalar çeşitli güvenlik açıkları ve olası güvenlik açıklarına yaşar eğilimindedir ve farklı LOB'lar yaygın olarak birçok web uygulaması kullanın. Web uygulamaları güvenlik duvarları, genel bir Güvenlik Duvarı'den daha fazla derinlemesine web uygulamaları (HTTP/HTTPS) saldırıları algılamak için kullanılan ürün için özel bir köpek ' dir. Gelenek güvenlik duvarı teknolojisi ile karşılaştırıldığında, WAFs iç web sunucularını tehditlere karşı korumak için belirli özellik kümesine sahip.

Bir güvenlik duvarı grubu art arda bağlı bileşen içinde barındırılan iş yüklerini korumak için güvenlik kuralları kümesi ile aynı ortak yönetim altında çalışan güvenlik duvarı bir grubudur ve erişimi denetleme içi ağlar. Bir güvenlik duvarı grubu daha az WAF ile karşılaştırıldığında yazılım özelleştirilmiş sahiptir, ancak filtre ve herhangi bir türde çıkış ve giriş trafiği incelemek için kapsamlı uygulama kapsama sahip. Güvenlik Duvarı grupları ağ sanal Azure marketi'ndeki kullanılabilir olan uygulamaları (NVAs) aracılığıyla Azure'da normal olarak uygulanır.

Internet üzerinde trafiği için bir dizi NVAs kullanmak için önerilen ve trafik kaynaklanan için başka bir şirket içi. Hiçbir güvenlik çevre ağ trafiği iki küme arasındaki sağladığı gibi her ikisi için yalnızca bir kümesini NVAs kullanarak bir güvenlik riski oluşturur. Ayrı NVAs kullanarak denetim güvenlik kuralları karmaşıklığını azaltır ve hangi kuralları hangi gelen ağ isteğine karşılık gelen temizleyin kolaylaştırır.

En büyük kuruluşlar birden çok etki alanı yönetin. Azure DNS, belirli bir etki alanı için DNS kayıtlarını barındırmak için kullanılabilir. Örnek olarak, Azure dış yük dengeleyici (veya WAFs) sanal IP adresi (VIP) bir Azure DNS kaydının A kaydı kaydedilebilir.

[**Azure yük dengeleyici** ] [ ALB] Azure yük dengeleyici yüksek oranda kullanılabilir bir gelen trafiği yükü dengelenmiş bir kümede tanımlanmış hizmet örnekleri arasında dağıtabilirsiniz katman 4 (TCP, UDP) hizmeti sunar. Yük Dengeleyici ön uç noktalarından (genel IP uç noktaları veya özel IP uç noktaları) gönderilen trafiğin ile veya olmadan adres çevirisi arka uç IP adresi havuzu (örnekler olan; kümesi için yeniden dağıtılabilir Ağ sanal Gereçleri veya VM'ler).

Azure yük dengeleyici sistem durumunu da çeşitli sunucu örnekleri araştırma ve sağlıksız örneğine trafiği göndermeye yük dengeleyici yanıt bir araştırma başarısız olduğunda durdurulur. VDC içinde bir dış yük dengeleyici varlığını (örneğin, NVAs trafiği Bakiye) hub ve bağlı bileşen (çok katmanlı uygulaması farklı VM'ler arasındaki trafiği Dengeleme gibi görevleri gerçekleştirmek için) sunuyoruz.

[**Uygulama ağ geçidi** ] [ AppGW] Microsoft Azure uygulama ağ geçidi olan uygulama teslim Denetleyicisi'ni (ADC) sağlayan ayrılmış bir sanal uygulama bir hizmet olarak çeşitli katman 7 Yük Dengeleme sunumu Uygulamanız için özellikleri. Uygulama ağ geçidi için CPU yoğunluklu SSL sonlandırma boşaltarak web grubu verimliliği en iyi duruma getirme imkan tanır. Ayrıca, gelen trafiğin “hepsini bir kez deneme” yaklaşımıyla dağıtımı, tanımlama bilgisi tabanlı oturum benzeşimi, URL’yi yol tabanlı yönlendirme ve tek bir uygulama ağ geçidi arkasında birden fazla web sitesi barındırma gibi diğer 7. katman yönlendirme özelliklerini sağlar. Application gateway WAF SKU’su kapsamında bir web uygulaması güvenlik duvarı da (WAF) sağlanır. Bu SKU ortak web Güvenlik Açıkları ve açıkları web uygulamaları için koruma sağlar. Application Gateway; İnternet'e yönelik ağ geçidi, yalnızca dahili ağ geçidi veya bu ikisinin bir birleşimi olarak yapılandırılabilir. 

[**Genel IP'ler** ] [ PIP] bazı Azure özellikleri etkinleştirmek, hizmet uç noktalar kaynağınıza internet'ten erişilmesine izin veren genel bir IP adresi ile ilişkilendirilecek. Bu uç nokta iç adresi ve bağlantı noktası Azure sanal ağı üzerinde trafiği yönlendirmek için ağ adresi çevirisi (NAT) kullanır. Bu yol, sanal ağa geçirmek dış trafiği için birincil yoludur. Genel IP adresleri, hangi trafik geçirilen ve nasıl ve nerede açın sanal ağ çevrilir belirlemek için yapılandırılabilir.

#### <a name="component-type-monitoring"></a>Bileşen türü: izleme
İzleme bileşenleri görünürlük ve tüm diğer bileşenleri türlerinden uyarı sağlar. Tüm takımların bileşenleri için izlemeyi erişimi olması gerekir ve Hizmetleri erişime sahip. Merkezi yardım masasına veya işlemleri takımlar varsa, bu bileşenler tarafından sağlanan verilere erişim tümleşik gerekir.

Günlüğe kaydetme ve Hizmetleri Azure davranışını izlemek için izleme farklı türlerde Azure sunar kaynaklarını barındırılan. İdare ve azure'da iş yüklerini denetime bağlı değil yalnızca üzerinde toplama günlük verilerini, aynı zamanda belirli bildirilen olaylara dayanarak tetikleyici eylemleri yeteneği olur.

[**Azure İzleyici** ] [ Monitor] -Azure ayrı ayrı bir spesifik rol ya da görev İzleme alanı gerçekleştirmek birden fazla hizmet içeriyor. Birlikte, bu hizmetleri toplama, çözümleme ve uygulamanız ve bunları destekleyen Azure kaynaklarını telemetrisinden işlevi gören için kapsamlı bir çözüm sunar. Karma bir ortamı izleme sağlamak için kritik şirket içi kaynakları izlemek için de çalışabilir. Araçlar ve kullanılabilir verileri anlamak, uygulamanız için tam bir izleme stratejisi geliştirme ilk adımdır.

Azure günlükleri başlıca iki türde vardır:

-   [**Etkinlik günlükleri** ] [ ActLog] ("İşlem günlüğü" olarak da bilinir) Azure aboneliğindeki kaynaklar üzerinde gerçekleştirilen işlemler hakkında bilgi sağlar. Bu günlükler aboneliklerinizi denetim düzlemi olayları bildirin. Her Azure kaynak denetim günlüklerini oluşturur.

-   [**Azure tanılama günlüklerini** ] [ DiagLog] olan bu kaynakla ilgili zengin, sık sık veri sağlayan bir kaynak tarafından oluşturulan günlükleri. Bu günlükler içeriğini kaynak türüne göre değişir.

[![9]][9]

Bir vDC, Nsg'ler günlükleri, özellikle bu bilgileri izlemek son derece önemlidir:

-   [**Olay günlükleri**][NSGLog]: hangi NSG kuralları VM'ler ve örnek rolleriniz MAC adresine dayalı uygulanır hakkında bilgi sağlar.
-   [**Sayaç günlükleri**][NSGLog]: her NSG kural reddetmek veya trafiğine izin vermek üzere yürütüldü kaç kez izler.

Tüm günlükler, Denetim, statik çözümleme veya yedekleme amacıyla Azure depolama hesaplarında depolanabilir. Günlükler, bir Azure depolama hesabında depolanır, müşteriler çerçeveler farklı türlerini kullanan almak için hazırla, çözümlemek ve durumunu ve bulut kaynaklarına durumunu bildirmek için bu verileri görselleştirin.

Büyük ölçekli işletmeler, şirket içi sistemleri izleme için standart bir çerçeve almış olması ve bulut dağıtımları tarafından oluşturulan günlükleri tümleştirmek için bu framework genişletebilirsiniz. İster kuruluş bulutta tüm günlük tutmak için [günlük analizi] [LogAnalytics] harika bir seçenektir. Günlük analizi bulut tabanlı bir hizmet olarak uygulandığından, hazır ve çalışır hızlı bir şekilde minimal yatırım altyapı hizmetleri ile sağlayabilirsiniz. Günlük analizi, ayrıca, varolan yönetim yatırımlarınızı buluta genişletmek için System Center Operations Manager gibi System Center bileşenleri ile tümleştirebilirsiniz.

Günlük analizi yardımcı toplamak, bağıntılı, arama ve işletim sistemleri, uygulamalar ve altyapı bulut bileşenleri tarafından oluşturulan günlük ve performans verilerini hareket azure'da bir hizmettir. Müşterilerin vDC içinde iş yükleri arasında tüm kayıtları çözümlemek için tümleşik arama ve özel panolar kullanarak gerçek zamanlı operasyonel Öngörüler verir.

[Ağ Performans İzleyicisi'ni (NPM)] [ NPM] çözüm OMS içinde ayrıntılı ağ bilgileri için Azure ağlar ve şirket içi ağlarda, tek bir görünümünü de dahil olmak üzere uçtan, sağlayabilir. ExpressRoute ve kamu hizmetleri için belirli monitörlerle.

#### <a name="component-type-workloads"></a>Bileşen türü: iş yükleri
İş yükü, gerçek uygulama ve hizmetlerinize bulunduğu bileşenleridir. Burada, uygulama geliştirme çoğu zaman, ekipler de olabilir.

İş yükünün olanaklar gerçekten sonsuzdur. Birkaç olası iş yükü türleri şunlardır:

**İç iş KOLU uygulamaları**

İş kolu satır, bir kuruluşun devam eden işlem kritik bilgisayar uygulamaları uygulamalardır. İş KOLU uygulamaları, bazı ortak özelliklere sahiptir:

-   **Etkileşimli**. İş KOLU uygulamaları doğası gereği etkileşimli: veriler girilir ve sonuç/raporları döndürülür.
-   **Veri tabanlı**. İş KOLU uygulamaları veritabanları veya diğer depolama için sık erişimli yoğun verilerdir.
-   **Tümleşik**. Uygulamaları teklif tümleştirme diğer sistemlerle içinde veya kuruluş dışından LOB.

**Müşteri Web sitelerini (Internet veya dahili e yönelik) karşılıklı** internetle etkileşimde çoğu uygulamayı web siteleridir. Azure Iaas VM'de veya bir web sitesini çalıştırmak için özelliği sunan bir [Azure Web Apps] [ WebApps] site (PaaS). Azure Web uygulamaları vDC spoke Web uygulamaları dağıtımını izin sanal ağlar ile tümleştirmeyi destekler. VNET Tümleştirmesi ile dahili kullanıma yönelik web siteleri bakıldığında uygulamalarınız için bir Internet uç nokta kullanıma kopyalaması gerekmeyen özel dışındaki Internet yönlendirilebilir adreslerden özel sanal ağınızı aracılığıyla kaynaklara kullanın.

**Büyük veri analizi** veri çok büyük bir birime ölçeği gerektiğinde veritabanları düzgün ölçeği değil. Hadoop teknoloji dağıtılmış sorgular çok sayıda düğümü üzerinde paralel olarak çalıştırmak için bir sistem sunar. Müşterilerin Iaas Vm'si veya PaaS veri iş yüklerini çalıştırmaya yönelik seçeneği sahip ([Hdınsight][HDI]). Hdınsight bağlı bileşen vDC, kümede dağıtılabilir, konum tabanlı bir sanal ağ dağıtma destekler.

**Olaylar ve ileti**
[Azure Event Hubs] [ EventHubs] toplar, dönüştüren ve milyonlarca etkinliği depolayan bir hiper ölçek telemetri alım hizmetidir. Olarak dağıtılmış bir akış platform, düşük gecikme süresi ve yapılandırılabilir zaman bekletme, Azure'da telemetri oldukça büyük miktardaki alma ve bu verileri birden çok uygulamalardan Okuma olanak sağlar. Event Hubs ile tek bir akış gerçek zamanlı ve toplu tabanlı ardışık düzen destekleyebilir.

İleti hizmeti uygulamalar ve hizmetler arasında yüksek oranda güvenilir bir bulut aracılığıyla uygulanabilir [Azure Service Bus] [ ServiceBus] teklifleri zaman uyumsuz ile birlikte istemci ve sunucu arasında Mesajlaşma aracılı yapılandırılmış ilk olarak ilk çıkar (FIFO) Mesajlaşma ve yayımlama/abonelik yetenekleri.

[![10]][10]

### <a name="multiple-vdc"></a>Birden çok vDC
Şu ana kadar bu makalede temel bileşenleri ve esnek bir vDC katkıda mimarisi açıklayan bir tek vDC odaklanan. Azure yük dengeleyici, NVAs, kullanılabilirlik kümeleri, diğer mekanizmaları birlikte ölçek kümeleri gibi Azure özellikleri düz SLA düzeyleri üretim hizmetlerinizi oluşturmanıza olanak sağlayan bir sistem katkıda bulunun.

Ancak, tek bir vDC tek bir bölge içinde barındırılan ve tüm bu bölge etkileyebilecek önemli kesinti savunmasızdır. Yüksek SLA elde etmek istediğiniz müşterilerin dağıtımları farklı bölgelerde yerleştirilen iki (veya daha fazla) VDC aynı projesinin hizmetleriyle korumanız gerekir.

SLA sorunları ek olarak, burada birden çok VDC dağıtma mantıklı birkaç yaygın senaryo vardır:

-   Bölge/genel durum
-   Olağanüstü Durum Kurtarma
-   DC arasındaki trafiği yönlendir mekanizması

#### <a name="regionalglobal-presence"></a>Bölge/genel durum
Azure veri merkezleri, dünya çapında çok sayıda bölgelerde yok. Birden çok Azure veri merkezlerinde seçerken, müşterilerin iki ilgili faktörleri göz önünde bulundurmanız gereken: coğrafi uzaklıklar ve gecikme süresi. Müşteriler VDC vDC ve en iyi kullanıcı deneyimi sunmak için son kullanıcıların arasındaki uzaklığı arasındaki coğrafi uzaklığı değerlendirmeniz gerekir.

VDC barındırıldığı Azure bölgesi da gerekir, kuruluşunuzun altında faaliyet yasal dairesi tarafından kurulan yasal düzenleme gereksinimlerine uygun olması.

#### <a name="disaster-recovery"></a>Olağanüstü Durum Kurtarma
Bir olağanüstü durum kurtarma planı uyarlamasını kesinlikle ilgilenen iş yükü türüne ve farklı VDC arasında iş yükünü durumunu eşitleme özelliği ilişkilidir. İdeal olarak, müşterilerin çoğu uygulama verilerinin hızlı yük devretme mekanizması uygulamak için iki farklı VDC içinde çalışan dağıtımlar arasında eşitlemek istediğiniz. Çoğu uygulama için gecikme süresi duyarlıdır ve veri eşitlemesine olası zaman aşımı ve gecikme neden.

Eşitleme veya farklı VDC uygulamaların sinyal izleme bunların arasındaki iletişimi gerektirir. Farklı bölgelerdeki iki VDC üzerinden bağlı olması gerekir:

-   VNet eşlemesi - VNet eşlemesi bağlanabilir hub'ları bölgeler arasında
-   ExpressRoute aynı expressroute bağlantı hattı vDC hub'ları bağlıyken eşliği özel
-   birden çok ExpressRoute bağlantı hatları, şirket omurga bağlı ve ExpressRoute bağlantı hatları için vDC kafes bağlı
-   Her Azure bölgesindeki vDC hub'larınız arasında siteden siteye VPN bağlantıları

Genellikle VNet eşlemesi veya ExpressRoute bağlantıları tercih edilen yüksek bant genişliği ve tutarlı gecikme nedeniyle Microsoft omurga transiting zaman sistemdir.

Farklı bölgelerde bulunan iki (veya daha fazla) farklı VDC arasında dağıtılan bir uygulama doğrulamak için hiçbir Sihirli tarif yoktur. Müşteriler gecikme süresi ve bant genişliği bağlantıları ve hedef zaman uyumlu veya zaman uyumsuz veri çoğaltma uygun olup olmadığını ve hangi, iş yükleri için en iyi kurtarma süresi hedefi (RTO) olabilir doğrulamak için ağ niteliğe testleri çalıştırmanız gerekir.

#### <a name="mechanism-to-divert-traffic-between-dc"></a>DC arasındaki trafiği yönlendir mekanizması
Başka bir DC'ye trafiği gelen yönlendir için etkili bir teknik DNS temel alır. [Azure Traffic Manager] [ TM] içinde belirli bir vDC en uygun ortak uç nokta için son kullanıcı trafiği yönlendirmek için etki alanı adı sistemi (DNS) mekanizması kullanır. Araştırmalar, üzerinden trafik Yöneticisi düzenli aralıklarla farklı VDC ortak uç noktalarını hizmetinin durumunu denetler ve bu uç başarısız olması durumunda, bu otomatik olarak ikincil vDC yönlendirir.

Trafik Yöneticisi Azure genel Uç noktalara çalışır ve, örneğin, Denetim/yönlendir uygun vDC Azure Vm'leri ve Web uygulamaları için trafiği için kullanılabilir. Trafik Yöneticisi bile bir tüm Azure bölgesi başarısızlığı karşısında esnektir ve birkaç ölçüte (örneği için belirli bir vDC veya istemci için en düşük ağ gecikmesini ile vDC seçerek hizmetinde hatası) göre farklı VDC içinde hizmet uç noktaları için kullanıcı trafiğinin dağıtımını denetleyebilirsiniz.

### <a name="conclusion"></a>Sonuç
Sanal veri merkezi bir veri merkezi geçiş için ölçeklenebilir mimari maliyetlerini azaltır ve sistem yönetimini basitleştirme, bulut kaynak kullanımını en üst düzeye çıkarır oluşturmak için özellikleri ve yetenekleri bileşimini kullanır buluta yaklaşımdır. VDC kavram hub'ında ortak paylaşılan hizmetler sağlamayı ve bağlı bileşen içinde belirli uygulamalar/iş yükleri vererek bir hub'a bağlı bileşen topolojisi temel alır. VDC şirket rolleri, burada farklı Departmanlar (Merkezi BT, DevOps, işlem ve bakımı) birlikte her rolleri ve görevleri belirli bir listesiyle birlikte çalışacak yapısını eşleşir. VDC "Kaldırın ve Shift" geçiş için gereksinimleri karşılıyor ancak aynı zamanda yerel bulut dağıtımları için birçok avantaj sağlar.

## <a name="references"></a>Başvurular
Aşağıdaki özellikler, bu belgede ele alınan. Daha fazla bilgi için bağlantılar'ı tıklatın.

| | | |
|-|-|-|
|Ağ Özellikleri|Yük Dengeleme|Bağlantı|
|[Azure sanal ağlar][VNet]</br>[Ağ güvenlik grupları][NSG]</br>[NSG günlüklerini][NSGLog]</br>[Kullanıcı tanımlı yönlendirme][UDR]</br>[Ağ sanal Gereçleri][NVA]</br>[Genel IP adresleri][PIP]</br>[DNS]|[Azure yük dengeleyici (L3) ][ALB]</br>[Uygulama ağ geçidi (L7) ][AppGW]</br>[Web uygulaması güvenlik duvarı][WAF]</br>[Azure trafik Yöneticisi][TM] |[VNet eşlemesi][VNetPeering]</br>[Sanal özel ağ][VPN]</br>[ExpressRoute][ExR]
|Kimlik</br>|İzleme</br>|En İyi Uygulamalar</br>|
|[Azure Active Directory][AAD]</br>[Çok faktörlü kimlik doğrulaması][MFA]</br>[Rol tabanlı erişim denetimleri][RBAC]</br>[Varsayılan AAD rolleri][Roles] |[Azure İzleyicisi][Monitor]</br>[Etkinlik günlükleri][ActLog]</br>[Tanılama günlükleri][DiagLog]</br>[Microsoft Operations Management Suite][OMS]</br>[Ağ Performansı İzleyicisi][NPM]|[En iyi yöntemler Çevre ağları][DMZ]</br>[Abonelik Yönetimi][SubMgmt]</br>[Kaynak grubu Yönetim][RGMgmt]</br>[Azure abonelik limitleri][Limits] |
|Diğer Azure Hizmetleri|
|[Azure Web uygulamaları][WebApps]</br>[Hdınsights (Hadoop) ][HDI]</br>[Event Hubs][EventHubs]</br>[Service Bus][ServiceBus]|



## <a name="next-steps"></a>Sonraki Adımlar
 - Araştır [VNet eşlemesi][VNetPeering], vDC hub ve bağlı bileşen tasarımları için underpinning teknolojisi
 - Uygulama [AAD] [ AAD] kullanmaya başlamak için [RBAC] [ RBAC] araştırması
 - Bir abonelik ve kaynak yönetim modeli geliştirmek ve RBAC yapısı gereksinimlerini karşılamak için model ve kuruluşunuzun ilkeleri. En önemli etkinlik planlama. Yeniden düzenlemeyi, birleşmeler, yeni ürün satırlar, vb. için pratik, kadar planlayın.

<!--Image References-->
[0]: ./media/networking-virtual-datacenter/redundant-equipment.png "bileşen çakışma örnekleri" 
[1]: ./media/networking-virtual-datacenter/vdc-high-level.png "hub ve bağlı bileşen vDC üst düzey örneği"
[2]: ./media/networking-virtual-datacenter/hub-spokes-cluster.png "hub ve bağlı bileşen kümesi"
[3]: ./media/networking-virtual-datacenter/spoke-to-spoke.png "spoke spoke"
[4]: ./media/networking-virtual-datacenter/vdc-block-level-diagram.png "blok düzeyinde vDC diyagramı"
[5]: ./media/networking-virtual-datacenter/users-groups-subsciptions.png "kullanıcılar, gruplar, abonelikleri ve projeleri"
[6]: ./media/networking-virtual-datacenter/infrastructure-high-level.png "üst düzey altyapı diyagramı"
[7]: ./media/networking-virtual-datacenter/highlevel-perimeter-networks.png "üst düzey altyapı diyagramı"
[8]: ./media/networking-virtual-datacenter/vnet-peering-perimeter-neworks.png "VNet eşlemesi ve Çevre ağları"
[9]: ./media/networking-virtual-datacenter/high-level-diagram-monitoring.png "izleme için üst düzey diyagramı"
[10]: ./media/networking-virtual-datacenter/high-level-workloads.png "iş yükü için üst düzey diyagramı"

<!--Link References-->
[Limits]: https://docs.microsoft.com/azure/azure-subscription-service-limits
[Roles]: https://docs.microsoft.com/azure/role-based-access-control/built-in-roles
[VNet]: https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview
[NSG]: https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg
[DNS]: https://docs.microsoft.com/azure/dns/dns-overview
[PrivateDNS]: https://docs.microsoft.com/azure/dns/private-dns-overview
[VNetPeering]: https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview 
[UDR]: https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview 
[RBAC]: https://docs.microsoft.com/azure/role-based-access-control/overview
[MFA]: https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication
[AAD]: https://docs.microsoft.com/azure/active-directory/active-directory-whatis
[VPN]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways 
[ExR]: https://docs.microsoft.com/azure/expressroute/expressroute-introduction 
[NVA]: https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/nva-ha
[SubMgmt]: https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-subscription-governance 
[RGMgmt]: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview
[DMZ]: https://docs.microsoft.com/azure/best-practices-network-security
[ALB]: https://docs.microsoft.com/azure/load-balancer/load-balancer-overview
[PIP]: https://docs.microsoft.com/azure/virtual-network/resource-groups-networking#public-ip-address
[AppGW]: https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction
[WAF]: https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview
[Monitor]: https://docs.microsoft.com/azure/monitoring-and-diagnostics/
[ActLog]: https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs 
[DiagLog]: https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs
[NSGLog]: https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log
[OMS]: https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview
[NPM]: https://docs.microsoft.com/azure/log-analytics/log-analytics-network-performance-monitor
[WebApps]: https://docs.microsoft.com/azure/app-service/
[HDI]: https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-introduction
[EventHubs]: https://docs.microsoft.com/azure/event-hubs/event-hubs-what-is-event-hubs 
[ServiceBus]: https://docs.microsoft.com/azure/service-bus-messaging/service-bus-messaging-overview
[TM]: https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview
