---
title: Azure sanal veri merkezi - ağ perspektifi
description: Azure sanal veri merkezinizde oluşturmayı öğrenin
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
ms.openlocfilehash: 2c8ca8bcce43596d521fa9c81438ac6a16f6dcdf
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37445390"
---
# <a name="azure-virtual-datacenter-a-network-perspective"></a>Azure sanal veri merkezi: Ağ perspektifi
**Microsoft Azure**: daha hızlı ilerlemenize, para tasarrufu, şirket içi uygulamaları ve verileri tümleştirin

## <a name="overview"></a>Genel Bakış
Şirket içi uygulamaları azure'a geçirme, hatta önemli değişiklikler ("lift- and -shift"olarak bilinen yaklaşım), kuruluşların güvenli ve ekonomik bir altyapının avantajlarından sağlar. Ancak, çevikliği en iyi bulut ile mümkün kılmak için kuruluşlar Azure özelliklerinden yararlanmak için kendi mimarilerini evrim Geçiren. Microsoft Azure, Hiper ölçekli hizmetler ve altyapı, Kurumsal düzeydeki özellikleri ve güvenilirlik ve karma bağlantı için birçok seçenek sunar. Müşteriler, Internet üzerinden veya özel ağ bağlantısı sağlayan Azure ExpressRoute ile bu bulut hizmetlerine erişmek seçebilirsiniz. Microsoft Azure platformu, müşterilerin sorunsuz bir şekilde altyapılarını buluta genişletin ve çok katmanlı mimariler oluşturun olanak tanır. Ayrıca, Microsoft iş ortaklarının güvenlik hizmetleri ve Azure'da çalıştırmak için iyileştirilmiş sanal Gereçleri sunarak gelişmiş özellikler sunar.

Bu makalede hep beraber buluta geçme konusunda düşünürken birçok müşterilerin karşılaştığı desenleri ve mimari ölçek, performans ve güvenlik sorunlarını çözmek için kullanılan tasarımlarına genel bir bakış sağlar. Farklı Kuruluş BT rolleri yönetime sığması nasıl bir genel bakış ve sistemin idare da açıklanan güvenlik gereksinimlerine Vurgu ile ve en iyi duruma getirme maliyeti.

## <a name="what-is-a-virtual-data-center"></a>Bir sanal veri merkezi nedir?
Erken gün içinde bulut çözümleri genel spektrumda konak tek, nispeten yalıtılmış uygulamalar için tasarlanmıştır. Bu yaklaşım da birkaç yıldır çalışmıştır. Ancak, bulut avantajlarını çözümleri belirgin hale geldi ve birden çok büyük ölçekli iş yüklerini ve adresleme güvenlik, güvenilirlik, performans, bulut üzerinde barındırılan dağıtımların bir maliyetlerinin artmasından veya daha fazla bölge yaşam döngüsü boyunca önemli hale geldi Bulut hizmeti.

Aşağıdaki bulut dağıtım diyagramı (sarı kutusu) iş yüklerinde (kırmızı kutu) güvenlik açıklarını ve en iyi duruma getirme ağ sanal Gereçleri yer bazı örnekler gösterilmektedir.

[![0]][0]

Sanal veri merkezi (vDC), kurumsal iş yükleri ve genel bulutta büyük ölçekli uygulamaları destekleyen olduğunda ortaya çıkan sorunları başa çıkmanıza gerek destekleyecek biçimde ölçeklendirilen için bu seçeneği kadar yaratma düşüncesinden.

VDC yalnızca uygulama iş yüklerini bulut ancak Ayrıca ağ, güvenlik, yönetim ve altyapı (örneğin, DNS ve Dizin Hizmetleri) değil. Genellikle de bir şirket içi ağ ya da veri merkezine dön özel bir bağlantı sağlar. Giderek daha fazla iş yüklerini Azure'a taşırken, destekleyici altyapı ve bu iş yükleri yerleştirilir nesneleri dikkat etmeniz önemlidir. Kaynakları nasıl yapılandırılmıştır hakkında dikkatle düşünmeye "bağımsız veri akışı, güvenlik modelleri ve uyumluluk sorunları ile ayrı olarak yönetilmelidir iş yükü Adaları" yüzlerce çoğalan önleyebilirsiniz.

Bir sanal veri merkezi aslında farklı ancak ilişkili varlıklarla ortak destekleyici İşlevler, özellikler ve altyapı bir koleksiyonudur. İş yüklerinizi tümleşik bir vDC görüntüleyerek, ekonomik ölçeklendirmenin, bileşen ve veri akışı merkezileştirme, birlikte daha kolay işlemler, yönetim ve uyumluluk denetimleri ile en iyi duruma getirilmiş güvenlik nedeniyle daha az maliyet hayata geçirebilirsiniz.

> [!NOTE]
> VDC olduğunu anlamak önemlidir **değil** ayrı bir Azure ürün, ancak çeşitli özellikleri ve yetenekleri tam gereksinimlerinizi karşılayacak şekilde birleşimi. vDC kaynakları ve bulutta yeteneklerini en üst düzeye çıkarmak için iş yüklerinizi ve Azure kullanım hakkında düşünmeye yoludur. Sanal DC bu nedenle bir modüler BT hizmetlerini azure'da kuruluş rolleri ve sorumlulukları uyarak oluşturmak nasıl bir yaklaşımdır.

VDC aşağıdaki senaryolar için iş yüklerini ve uygulamaları azure'a elde etmesine yardımcı olabilir:

-   Birden fazla ilgili iş yüklerini barındırma
-   Şirket içi ortamdan geçirme iş yüklerini azure'a
-   Paylaşılan veya merkezi sürüm denetimi güvenlik ve erişim gereksinimlerini iş yüklerinde uygulama
-   DevOps ve BT merkezi büyük bir kuruluş için uygun şekilde karıştırma

VDC, avantajları kilidini açmak için anahtardır (hub ve bağlı bileşenler) merkezi bir topoloji Azure özelliklerinin bir karışımını: [Azure VNet][VNet], [Nsg'ler] [ NSG], [VNet eşlemesi][VNetPeering], [kullanıcı tanımlı yollar (UDR)][UDR]ve Azure kimliğiyle [rolü tabanı Erişim denetimi (RBAC)][RBAC].

## <a name="who-needs-a-virtual-data-center"></a>Bir sanal veri merkezi gerek duyan?
Birkaç iş yüklerinin azure'a birden çok taşımak için gereken herhangi bir Azure müşterisi ortak kaynakları kullanma hakkında düşünmeye yararlanabilir. Ölçeğine bağlı olarak, bir vDC oluşturmak için kullanılan bileşenler ve düzenleri kullanarak hatta tek tek uygulamalar yararlı olabilir.

Kuruluşunuzda merkezi bir BT, ağ, güvenlik varsa ve/veya uyumluluk takım/departman, bir vDC yardımcı uygulama ekipleri kadar özgürlüğünü sırasında ortak bileşenler'ın bütünlüğünü sağlamak ve ilke noktaları, görev ayrımı prensibini uygulamak olabilir ve gereksinimlerinize uygun olarak denetleyebilirsiniz.

DevOps için isteyen kuruluşlar, Azure kaynaklarınızın yetkili fotogerçekçi sağlamak ve bu gruba (ortak bir abonelikte ya da abonelik veya kaynak grubu), ancak ağ toplam denetimine sahip olduklarından emin olmak için vDC kavramları kullanabiliyor ve Merkez sanal ağı ve kaynak grubunda bir merkezi ilke tarafından tanımlanan güvenlik sınırları sürdürün.

## <a name="considerations-on-implementing-a-virtual-data-center"></a>Bir sanal veri merkezi uygulama konusunda dikkat edilmesi gerekenler
VDC tasarlarken dikkate alınması gereken birkaç pivotal sorunlar vardır:

-   Kimlik ve Dizin Hizmetleri
-   Güvenlik altyapısı
-   Bulut bağlantısı
-   Bulut içindeki bağlantı

##### <a name="identity-and-directory-service"></a>*Kimlik ve dizin hizmeti*
Kimlik ve Dizin Hizmetleri, temel bir yönü, tüm veri merkezleri, hem şirket içi ve bulut. Kimlik, erişim ve yetkilendirme hizmetleri vDC içinde tüm yönlerini ilgilidir. Yalnızca yetkili kullanıcıların ve işlemleri Azure hesabınızı ve kaynaklara erişim sağlamak için Azure kimlik doğrulaması için birden fazla kimlik bilgilerini kullanır. Bunlar, (Azure hesabı için) parolalar, şifreleme anahtarlarını, dijital imzalar ve sertifikalar içerir. [*Azure multi-Factor Authentication* (MFA)] [ MFA] Azure hizmetlerine erişmek için güvenlik ek katmanıdır. Azure MFA, güçlü kimlik doğrulaması ile çeşitli kolay doğrulama seçenekleri sağlar: telefon araması, SMS mesajı veya mobil uygulama bildirimi — ve tercih ettikleri yöntemi seçme özgürlüğü tanıyın.

Ayrı kimlik yönetimini, kimlik doğrulaması, yetkilendirme, roller ve ayrıcalıkları içinde veya arasında vDC açıklayan bir Kimlik Yönetimi işlemi tanımlamak büyük bir kurumsal gerekir. Bu işlem amaçlarını, maliyet, kapalı kalma süresi ve yinelenen manuel görevleri yükünü azaltırken, güvenlik ve verimliliğini artırmak için olmalıdır.

Kuruluşlar/Hizmetleri farklı satır-ın-işletmeler (LOB) için ihtiyaç duyulan bir karışımını gerektirebilir ve çalışanların genellikle farklı projeleriyle söz konusu olduğunda farklı rollere sahip. VDC her iyi idareye çalıştıran sistemlere almak için belirli bir rol tanımları farklı ekipler arasında daha iyi işbirliği gerektirir. Sorumlulukları ve erişim hakları matrisini son derece karmaşık olabilir. VDC Identity Management'ta aracılığıyla gerçekleştirilir [ *Azure Active Directory* (AAD)] [ AAD] ve rol tabanlı Access Control (RBAC).

Bir dizin hizmeti, bulma, yönetme, yönetme ve gündelik öğeleri ve ağ kaynaklarını düzenleme için bir paylaşılan bilgileri altyapısıdır. Bu kaynaklar, birimler, klasörler, dosyaları, yazıcılar, kullanıcılar, gruplar, cihazları ve diğer nesneleri içerebilir. Ağ üzerindeki her bir kaynak dizin sunucusu tarafından bir nesne olarak kabul edilir. Bir kaynak hakkında bilgiler, ilgili kaynağın veya nesne ile ilişkili öznitelikleri koleksiyonu olarak depolanır.

Tüm Microsoft online iş Hizmetleri, oturum açma için Azure Active Directory (AAD üzerinde) kullanan ve diğer kimlik gerekiyor. Azure Active Directory, temel dizin hizmetleri, gelişmiş kimlik yönetimi ve uygulama erişim yönetimi özelliklerini bir araya getiren kapsamlı bir kimlik ve erişim yönetimi bulut çözümüdür. AAD ile şirket içi Active çoklu oturum açma için tüm bulut tabanlı ve yerel olarak barındırılan (şirket içi) etkinleştirmek için dizin tümleştirilebilir uygulamalar. Şirket içi Active Directory kullanıcı öznitelikleri, AAD için otomatik olarak eşitlenebilir.

Tek bir genel yönetici, bir vDC tüm izinleri atamak için gerekli değildir. Bunun yerine her belirli bir bölüm (ya da grup kullanıcı ya da dizin hizmetinde Hizmetleri) bir vDC içinde kendi kaynaklarını yönetmek için gerekli izinlere sahip olabilir. İzinleri yapılandırma Dengeleme gerektirir. Çok fazla İznim performans verimliliği engel ve çok az veya gevşek izinleri, güvenlik risklerini artırabilir. Azure rol tabanlı erişim denetimi (RBAC) vDC kaynakları için ayrıntılı erişim yönetimi sunarak bu sorunu gidermek için yardımcı olur.

##### <a name="security-infrastructure"></a>*Güvenlik altyapısı*
Güvenlik altyapısı, bir vDC bağlamında çoğunlukla vDC'ın belirli bir sanal ağ kesimindeki trafiği ayrımı ilgili ve vDC giriş ve çıkış denetlemek nasıl akar. Azure dağıtımlar arasında yetkisiz ve istenmeyen trafiği engeller çok kiracılı mimarisini temel alır, erişim denetimi listeleri (ACL'ler), yük Dengeleyiciler ve IP filtreleri, trafik akış ilkeleri ile birlikte kullanarak sanal ağ (VNet) yalıtım. Ağ adresi çevirisi (NAT), iç ağ trafiğini dış trafikten ayırır.

Azure dokusunu altyapı kaynaklarını Kiracı iş yüklerini ayırır ve sanal makineleri (VM'ler) iletişimi yönetir. Azure hiper Yöneticisi VM'ler arasında bellek ve işlem ayrımı yapılmasını zorunlu kılan ve yollarını trafiği konuk işletim sistemi kiracılarına güvenli bir şekilde ağ.

##### <a name="connectivity-to-the-cloud"></a>*Bulut bağlantısı*
VDC müşteriler, iş ortakları ve/veya iç kullanıcılar için hizmet sunmak için dış ağ ile bağlantı gerekir. Bu genellikle bağlantı yalnızca İnternet'e, aynı zamanda şirket içi ağlar ve veri merkezlerine anlamına gelir.

Müşteriler ne denetlemek için güvenlik ilkelerini oluşturabilirsiniz ve belirli vDC barındırılan nasıl için/Internet'ten erişilebilir ağ sanal Gereçleri (trafik filtreleme ve İnceleme ile) kullanarak ve özel yönlendirme ilkeleri ve filtreleme (Ağ Hizmetleri Kullanıcı tanımlı Yönlendirme ve ağ güvenlik grupları).

Kuruluşlar genellikle şirket içi veri merkezleri ve diğer kaynaklara VDC bağlanmanız gerekir. Azure ve şirket içi ağlar arasında bağlantı, bu nedenle etkili bir mimari tasarlarken önemli bir yönü olur. Kuruluşların Azure'da vDC ve şirket arasındaki bir bağlantısı oluşturmak için iki farklı yol vardır: Internet üzerinden ve/veya özel doğrudan bağlantılar tarafından geçiş.

Bir [ **Azure siteden siteye VPN** ] [ VPN] bir iç bağlantı şirket içi ağ arasında Internet üzerinden hizmetidir ve güvenli aracılığıyla kurulan vDC şifrelenir bağlantılar (IPSec/IKE tüneller). Esnek, hızlı oluşturmak Azure siteden siteye bağlantı ve tüm bağlantıları Internet üzerinden bağlandıkları herhangi başka bir tedarik gerektirmez.

[**ExpressRoute** ] [ ExR] vDC ve şirket içi ağlar arasında özel bağlantılar oluşturmanızı sağlayan bir Azure bağlantısı hizmetidir. ExpressRoute bağlantıları değil genel Internet üzerinden gidin ve daha yüksek güvenlik, güvenilirlik ve tutarlı bir gecikme süresi ile birlikte yüksek hız (en fazla 10 GB/sn) sunar. ExpressRoute müşterilere uyumluluk kuralları ile özel bağlantılar ilişkili avantajlarını elde edebilirsiniz ExpressRoute olarak VDC için çok yararlı olur.

ExpressRoute bağlantıları dağıtımı, bir ExpressRoute hizmet sağlayıcısı ile ilgi çekici içerir. Hızlıca başlamak için ihtiyacınız olan müşteriler, başlangıçta vDC arasında bağlantı kurmak için siteden siteye VPN kullanımı yaygındır şirket içi kaynaklara ve ardından ExpressRoute bağlantısı geçirin.

##### <a name="connectivity-within-the-cloud"></a>*Bulut içindeki bağlantı*
[Sanal ağlar] [ VNet] ve [VNet eşlemesi] [ VNetPeering] temel ağ bağlantısı bir vDC içinde hizmetleridir. Bir sanal ağ yalıtım vDC kaynaklar için doğal bir sınır garanti eder ve intercommunication aynı Azure bölgesindeki veya hatta farklı bölgelerdeki farklı sanal ağlar arasında VNet eşlemesi sağlar. Trafik denetimi içinde bir sanal ağ arasında sanal ağlar erişim denetim listeleri belirtilen güvenlik kural kümesinin eşleşmesi gerekir ([ağ güvenlik grubu][NSG]), [ağ sanal Gereçleri ] [ NVA]ve özel yönlendirme tablolarını ([UDR][UDR]).

## <a name="virtual-data-center-overview"></a>Sanal veri merkezi genel bakış

### <a name="topology"></a>Topoloji
Tek bir Azure bölgesindeki sanal veri merkezi modeli hub ve bağlı bileşenler genişletilmiş

[![1]][1]

Hub denetleyen ve farklı bölgeler arasında giriş ve/veya çıkış trafiği inceleyen merkezi bölgedir: Internet, şirket içi ve bağlı bileşenler. Hub ve bağlı bileşen topolojisi BT departmanı, yanlış yapılandırma ve etkilenme olasılığını azaltırken merkezi bir konumda güvenlik ilkelerini zorlamak için etkili bir yol sağlar.

Hub uçlar tarafından ortak hizmet bileşenleri içerir. Yaygın yönetim hizmeti bazı tipik örnekler şunlardır:

-   Kullanıcı kimlik doğrulaması üçüncü tarafların uç iş yükleri erişim almadan önce güvenilmeyen ağlara erişim için gereken Windows Active Directory altyapınızla (ilgili ADFS hizmeti)
-   Uçlar, şirket kaynaklarına erişim ve İnternet'te iş yükü için adlandırma çözümlemek için DNS hizmeti
-   Çoklu oturum açma iş yüklerinde uygulamak için bir PKI altyapısı
-   Uçlar ve Internet arasında akış denetimi (TCP/UDP)
-   Şirket içi ve uç arasında akış denetimi
-   İsterseniz, Denetim ve başka bir uç arasında akış

VDC birden çok uçlar arasında paylaşılan hub altyapıyı kullanarak genel maliyeti azaltır.

Konak farklı türde iş yükleri için her uç rolünü olabilir. Uçlar için yinelenebilir dağıtımlara modüler bir yaklaşım de sağlayabilirsiniz (örneğin, geliştirme ve test, kullanıcı kabul testi, ön üretim ile üretim) iş yüklerinin aynı. Uçlar, ayırmak ve farklı gruplar kuruluşunuzdaki (örneğin, DevOps grupları) etkinleştirmek için de kullanılabilir. Bir bileşen temel iş yükü veya karmaşık çok katmanlı iş yüklerinde Katmanlar arasındaki trafik denetimi ile dağıtmak mümkündür.

##### <a name="subscription-limits-and-multiple-hubs"></a>Abonelik limitleri ve birden çok hub'ları
Azure'da bir türü ne olursa olsun, her bileşen bir Azure aboneliğinde dağıtılır. Fark yaratan yetkilendirme ve erişim düzeyleri ayarlama gibi farklı LOB'lar, gereksinimlerini karşılayan bir yalıtım, farklı Azure aboneliklerinde Azure bileşenleri.

Her BT sistemi olduğu gibi olmasına rağmen platformları sınırları tek bir vDC uçlar, çok sayıda ölçeklendirme yapabilir. Hub dağıtımı kısıtlamaları ve sınırları olan belirli bir Azure aboneliği için bağlı (örneğin, sanal ağ eşleme - max bir sayısı bkz [Azure aboneliği ve hizmet limitleri, kotalar ve kısıtlamalar] [ Limits] Ayrıntılar için). Burada sınırları bir sorun olabilir durumlarda mimari ölçeklendirebilirsiniz kadar hub ve bağlı bileşenler kümesi için tek bir hub-uçlardan modeli genişleterek daha fazla. Bir veya daha fazla Azure bölgesinde, birden çok hub'ları, VNet eşlemesi, ExpressRoute veya siteden siteye VPN kullanarak birbirine bağlanabilir.

[![2]][2]

Birden çok hub'a maliyet ve yönetim eforunu sisteminin artırır ve yalnızca ölçeklenebilirlik tarafından hizalı (örnekler: sistem sınırlarını ya da yedekliliği) ve bölgesel çoğaltma (örnekler: son kullanıcı performans veya olağanüstü durum kurtarma). Senaryolarda birden çok hub'ları, tüm hub'ları gerektiren hizmetler için işletimsel bir kolayca aynı kümesi sunmak için çaba göstermelisiniz.

##### <a name="interconnection-between-spokes"></a>Uçlar arasında bağlantısı
İçinde tek bir bileşen, karmaşık çok katmanları iş yüklerini uygulamak da mümkündür. Çok katmanlı yapılandırmaları aynı sanal ağda alt ağlar (her katman için bir tane) kullanarak ve Nsg'ler kullanarak akışları filtreleme uygulanabilir.

Öte yandan, bir Mimarı, çok katmanlı iş yükü birden çok sanal ağlarda dağıtmak isteyebilirsiniz. VNet eşlemesi kullanarak uçlar aynı hub veya hub'ları farklı diğer uçlara bağlanabilirsiniz. Bu senaryonun tipik bir örnek veritabanı farklı bir bileşen (VNet) içinde dağıtılırken uygulama işleme sunucuları bir bileşen (VNet) içinde olduğu durumdur. Bu durumda, VNet eşlemesi ile uçlar birbiriyle bağlantılı ve böylece hub'ı aracılığıyla i yapılandırmamız kaçınmak kolaydır. Hub'ı atlama önemli güvenlik veya hub'ı yalnızca bulunabilecek noktaları denetimini atlama değil emin olmak için dikkatli bir mimari ve güvenlik incelemesini gerçekleştirilmesi gerekir.

[![3]][3]

Uçlar aynı zamanda bir merkez olarak davranan bir bileşen için birbirine bağlanabilir. Bu yaklaşımın iki düzeyli bir hiyerarşi oluşturur: uç daha yüksek düzeyde (düzeyi 0) hiyerarşinin daha düşük uçlar (düzey 1) hub haline gelir. VDC uçlar, şirket içi ağ veya internet çıkış ulaşmak için merkezi hub trafiği iletmek gerekir. İki düzeyi hub'ının bir mimariyle basit merkez-uç ilişki avantajlarını kaldırır karmaşık yönlendirme sunar.

Azure karmaşık topolojiler olanak tanısa da, temel ilkeler vDC kanıtı Yinelenebilirlik ve Basitlik biridir. Yönetim çabasını en aza indirmek için önerilen vDC başvuru mimarisi basit merkez-uç tasarım olur.

### <a name="components"></a>Bileşenler
Bir sanal veri merkezi dört temel bileşen türleri oluşur: **altyapı**, **Çevre ağları**, **iş yükleri**, ve **izleme**.

Her bileşen türü, çeşitli Azure özellikleri ve kaynakları oluşur. Bir vDC bileşenleri birden çok ve aynı bileşen türü birden çok çeşitleri örneklerini oluşur. Örneğin, farklı uygulamalar temsil eden birçok farklı, mantıksal olarak ayrı bir iş yükü örneği olabilir. Sonuçta vDC oluşturmak için bu farklı bileşen türlerini ve örnekleri'nı kullanın.

[![4]][4]

VDC önceki üst düzey mimarisi hub uçlar topolojisinin farklı bölgelerde kullanılan farklı bileşen türlerini gösterir. Diyagramda mimarinin çeşitli bölümleri altyapı bileşenleri gösterilmektedir.

Bir iyi (şirket içi DC'ye veya vDC için) erişim haklarını ve ayrıcalıklarını grup tabanlı olmalıdır. Erişim ilkeleri, takımda ve yardımlar yapılandırma hataları en aza indirir, tutarlı bir şekilde koruma grupları ile ilgili, bireysel kullanıcılar yerine yardımcı olur. Belirli bir kullanıcının ayrıcalıkları güncel tutarak, atama ve kullanıcıların ve uygun gruplardan kaldırma yardımcı olur.

Her rol grubu benzersiz bir önek hangi gruba hangi iş yükü ile ilişkilendirilmiş olduğunu tanımlamak kolaylaştıran adlarına sahip olmalıdır. Örneğin, bir kimlik doğrulama hizmetini barındıran bir iş yükü adlı grup olabilir *AuthServiceNetOps, AuthServiceSecOps AuthServiceDevOps ve AuthServiceInfraOps.* Benzer şekilde "Corp" ile başlar, rolleri veya belirli bir hizmete ilgili olmayan rollerin merkezi için *CorpNetOps* örneğin.

Çoğu kuruluş, önemli bir rol dökümünü sağlamak için aşağıdaki grupların bir değişim kullanın:

-   *Merkezi BT grubu (Corp)* (örneğin, ağ ve güvenlik) altyapı bileşenlerini denetlemek için sahiplik haklarına sahiptir ve bu nedenle, abonelik üzerinde katkıda bulunan rolüne sahip (ve hub'ın denetiminiz) gerekir ve Uçlar, ağ Katılımcısı hakları. Büyük kuruluş sık bu yönetim sorumlulukları gibi birden çok ekipler arasında yukarı split; bir ağ işlemlerini (CorpNetOps) grubu (odaklanılan özel ağ üzerinde) ve güvenlik işlemleri (CorpSecOps) grubu (güvenlik duvarı ve güvenlik ilkesi için sorumlu). Bu belirli durumda, iki farklı gruplar bu özel rol ataması için oluşturulması gerekir.
-   *Geliştirme & test (AppDevOps) grubu* (uygulamalarına veya hizmetlerine) iş yükleri dağıtmak için bir sorumluluğu vardır. Bu grup, sanal makine Katılımcısı rolü Iaas dağıtımları ve/veya bir veya daha fazla PaaS katkıda bulunanın rolleri alır (bkz [Azure rol tabanlı erişim denetimi için yerleşik roller][Roles]). İsteğe bağlı olarak geliştirme ve test takımı, güvenlik ilkeleri (Nsg'ler) ve hub veya belirli bir bileşen içinde yönlendirme ilkeleri (UDR) görünürlük sağlamak gerekebilir. Bu nedenle, iş yükleri için katkıda bulunan rollerine ek olarak bu Grup Ayrıca ağ okuyucu rolü gerekir.
-   *Çalışmasından ve korunmasından grubu (CorpInfraOps veya AppInfraOps)* üretim iş yüklerini yönetme sorumluluğunu sahip. Bu grup, herhangi bir üretim aboneliğinizi iş yükleri üzerinde bir abonelik katkıda bulunanı olmanız gerekir. Bir ek yükseltme desteği takım grubu rolüne sahip abonelik katkıda bulunanı merkezi hub aboneliği, üretim ve üretim olası yapılandırma sorunlarını çözmek için ihtiyaç duydukları, bazı kuruluşlar da değerlendirebilir ortam.

VDC hub'ı yönetme merkezi BT grupları için oluşturulan grupları iş yükü düzeyinde karşılık gelen gruplarınız şekilde yapılandırılmıştır. Hub'ı kaynakları yönetmenin yanı sıra yalnızca merkezi BT grupları dış erişim ve üst düzey izinleri abonelik üzerinde denetimi mümkün olacaktır. Ancak, iş yükü grupları kaynakları ve merkezi BT üzerinde bağımsız olarak, VNet izinlerini denetlemek hazırdır.

VDC güvenli bir şekilde birden çok proje farklı satır-ın-işletmeler arasında (LOB) barındırmak için bölümlenmiş olması gerekir. Tüm projeler farklı yalıtılmış ortamlara (geliştirme, UAT, üretim) gerektirir. Bu ortamların her biri için ayrı Azure Abonelikleri, doğal yalıtımı sağlar.

[![5]][5]

Önceki şemada, bir kuruluşun projeleri, kullanıcıları, grupları ve Azure bileşenlerini dağıtıldığı ortamların arasındaki ilişkiyi gösterir.

Buna genellikle BT, bir ortam (veya katman) birden çok uygulama, dağıtılan ve çalıştırılan bir sistemdir. Büyük kuruluşlar, bir geliştirme ortamı kullanın (değişiklikler ilk olarak yapılan ve test) ve bir üretim ortamına (son kullanıcıların ne kullanın). Bu ortamlarda, genellikle birden çok hazırlık ortamları arasında bunları aşamalı dağıtımı (ürün) izin vermek için test ve sorun oluşması durumunda geri alma ile ayrılır. Dağıtım mimarisi önemli ölçüde farklılık, ancak genellikle (Geliştirme) geliştirme için başlangıç ve bitiş üretimde (üretim) temel işlemi hala izlenir.

Bu tür bir çok katmanlı ortamlar için yaygın bir mimari, DevOps (geliştirme ve test), UAT (hazırlama) ve üretim ortamlarında oluşur. Kuruluşlar, tek veya birden çok Azure AD kiracılarıyla erişim ve bu ortamların haklarını tanımlamak için yararlanabilirsiniz. Önceki diyagramda bir servis talebi burada iki farklı gösterilmektedir Azure AD kiracılarıyla kullanılır: biri DevOps ve UAT, diğeri üretim için özel olarak.

Varlık farklı Azure AD kiracılar, ortamlar arasında ayrım zorlar. Aynı kullanıcı grubunu (örneğin, merkezi BT) gerekli kullanarak farklı bir kimlik doğrulaması farklı bir AD kiracınıza erişim URI roller veya bir projenin DevOps veya üretim ortamları izinlerini değiştirin. Farklı ortamlar erişmek için farklı kullanıcı kimlik doğrulama varlığı, olası kesintileri ve insan hataları nedeniyle diğer sorunları azaltır.

#### <a name="component-type-infrastructure"></a>Bileşen türü: altyapı
Destekleyici altyapının çoğunu yer aldığı bu bileşeni türüdür. Ayrıca Burada, merkezi BT, güvenlik ve/veya uyumluluk ekipleri harcama zamanlarının çoğunu.

[![6]][6]

Altyapı bileşenlerini bir vDC farklı bileşenleri arasında bir bağlantısı sağlamak ve hub ve bağlı bileşenler mevcuttur. Yönetmeye ve korumaya altyapı bileşenleri için sorumluluk genellikle Orta için atanan BT ve/veya güvenlik ekibi.

BT altyapısı ekibin birincil görevlerinden birini ve kuruluş genelinde IP adresi şemaları tutarlılığını garanti sağlamaktır. Özel IP adresi alanı tutarlı olmasını vDC ihtiyaçları için atanan ve şirket içi ağlarınızı atanmış özel IP adresleriyle çakışan değil.

Şirket içi uç yönlendiricileri ya da Azure ortamlarını NAT IP adresi çakışmaları vermemesine zorluk, altyapı bileşenleri için ekler. Yönetim basitliğinin IP kaygıları işlemek için NAT'ı kullanarak önerilen bir çözüm değildir vDC, anahtar amaçlarını biridir.

Altyapı bileşenlerini aşağıdaki işlevleri içerir:

-   [**Kimlik ve Dizin Hizmetleri**][AAD]. Azure'daki her kaynak türü için erişim, dizin hizmetinde depolanan bir kimlik tarafından denetlenir. Dizin hizmeti, yalnızca kullanıcıların listesini, aynı zamanda kaynaklara erişim haklarını belirli bir Azure aboneliğinde depolar. Bu hizmetler, yalnızca bulutta yer alan bulunabilir veya Active Directory'de depolanan şirket içi kimlik ile eşitlenebilir.
-   [**Sanal ağ**][VPN]. Sanal ağlar, bir vDC ana bileşenleri biridir ve trafik yalıtımı sınır Azure platformunda oluşturmanıza olanak sağlar. Bir sanal ağ, tek bir veya birden çok sanal ağ kesimleri, her biri belirli IP ağ öneki (alt ağ) oluşur. Sanal ağ, burada Iaas sanal makineleri ve PaaS Hizmetleri özel iletişim kurabilmesi için bir iç çevre alan tanımlar. VM'ler (ve PaaS Hizmetleri) bir sanal ağ, sanal makinelere doğrudan kuramıyor.%nÇözüm (ve PaaS Hizmetleri) farklı bir sanal ağ, her iki sanal ağ aynı abonelik altında aynı müşteri tarafından oluşturulmuş olsa bile. Yalıtım müşteri Vm'leri sağlar önemli bir özelliktir ve bir sanal ağ içindeki özel iletişimin kalır.
-   [**UDR**][UDR]. Bir sanal ağdaki trafik, varsayılan sistem yönlendirme tablosuna dayalı olarak yönlendirilir. Bir kullanıcıyı tanımlamak, ağ yöneticilerinin özel bir yönlendirme tablosu sistem yönlendirme tablosu davranışı üzerine ve sanal ağ içindeki bir iletişim yolunun tanımlamak için bir veya daha fazla alt ağlara ilişkilendirebilirsiniz yoldur. Udr'ler varlığını belirli özel sanal makineler ve/veya ağ sanal Gereçleri ve yük Dengeleyiciler hub ve bağlı bileşenler mevcut yoluyla uç aktarım öğesinden bu çıkış trafiği garanti eder.
-   [**NSG**][NSG]. Bir ağ güvenlik grubu trafik IP kaynakları, hedef IP, protokolleri, IP kaynak bağlantı noktaları ve IP hedef bağlantı noktaları üzerinde filtreleme gibi davranan güvenlik kuralları bir listesidir. NSG alt ağa, bir Azure VM veya her ikisi ile ilişkili bir sanal NIC kartı uygulanabilir. Nsg'ler, hub ve bağlı bileşenler doğru akış denetimi uygulamak için gereklidir. Bir işlevin hangi bağlantı noktalarını açmanız ve hangi amaçla NSG tarafından gösterilen güvenlik düzeyidir. Müşteriler, ek VM başına filtrelerle ana bilgisayar tabanlı güvenlik duvarları gibi IPtables veya Windows Güvenlik Duvarı'nı uygulamalıdır.
-   [**DNS**][DNS]. Bir vDC vnet'lerdeki kaynaklar ad çözümlemesi DNS üzerinden sağlanır. Azure DNS hizmeti hem de sunar [genel][DNS] ve [özel] [ PrivateDNS] ad çözümlemesi. Özel alanları, bir sanal ağdaki ve sanal ağlar arasında ad çözümleme sağlar. Yalnızca aralık özel bölgeleri aynı bölgede ancak bölgeler ve abonelikler arasında sanal ağlar arasında olabilir. Genel bir çözüm için Azure DNS, Microsoft Azure altyapısı kullanılarak ad çözümlemesi olanağı sağlayan bir DNS etki alanı, barındırma hizmeti sağlar. Etki alanlarınızı Azure'da barındırarak DNS kayıtlarınızı diğer Azure hizmetlerinde kullandığınız kimlik bilgileri, API’ler, araçlar ve faturalarla yönetebilirsiniz.
-   [**Abonelik** ] [ SubMgmt] ve [ **Kaynak Grup Yönetimi**][RGMgmt]. Bir abonelik, Azure'da birden çok kaynak gruplarını oluşturmak için doğal bir sınır tanımlar. Bir Abonelikteki kaynakları kaynak gruplarına adlı mantıksal kapsayıcılarda birlikte birleştirilir. Kaynak grubu, bir vDC kaynakları düzenlemek için bir mantıksal grubu temsil eder.
-   [**RBAC**][RBAC]. RBAC aracılığıyla Kimliğinizi, böylece kullanıcıların yalnızca belirli bir alt işlemlerin için kısıtlamak belirli Azure kaynaklarına erişim hakları yanı sıra harita Kurumsal rol için mümkündür. RBAC ile kullanıcılara, gruplara ve uygulamalara uygun kapsam içinde uygun role atayarak erişim verebilirsiniz. Rol atamasının kapsamı, bir Azure aboneliği, bir kaynak grubu veya tek bir kaynak olabilir. RBAC izinlerinin devralma sağlar. Atanmış bir üst kapsamda bir rol, ayrıca içerdiği alt öğeleri için erişim verir. RBAC kullanarak, görevleri ayırmak ve işlerini yapmak için ihtiyaçları olan kullanıcılara sadece erişim miktarını verin. Örneğin, aynı abonelik içinde başka bir SQL veritabanlarını yönetebilir, ancak bir Abonelikteki sanal makineleri yönetme bir çalışanın izin vermek için RBAC kullanın.
-   [**VNet eşlemesi**][VNetPeering]. VDC altyapısını oluşturmak için kullanılan temel özellik, Azure veri merkezi ağı veya bölgeler arasında Azure dünya çapında omurga kullanarak aracılığıyla aynı bölgedeki sanal ağ eşleme, iki sanal ağ (Vnet) bağlanan bir mekanizma değildir.

#### <a name="component-type-perimeter-networks"></a>Bileşen türü: Çevre ağları
[Çevre ağı] [ DMZ] bileşenleri (çevre ağı olarak da bilinir), şirket içinde veya fiziksel veri merkezi ağlarını, Internet'ten herhangi bir bağlantı ile ağ bağlantısı sağlamak sağlar. Ayrıca, ağ ve güvenlik takımlarınızın olasılığı zamanlarının çoğunu burada harcama olur.

Gelen paket güvenlik duvarı, Kimlikleri ve IPS gibi hub'ındaki güvenlik Gereçleri üzerinden uçlar arka uç sunucular ulaşmadan önce akış. İnternet'e bağlı iş yüklerini paketleri ayrıca ilke zorlaması, inceleme ve denetim amacıyla, çevre ağındaki güvenlik Gereçleri üzerinden ağ ayrılmadan önce akış.

Çevre ağ bileşenleri aşağıdaki özellikleri sağlar:

-   [Sanal ağlar][VNet], [UDR][UDR], [NSG][NSG]
-   [Ağ sanal Gereci][NVA]
-   [Yük Dengeleyici][ALB]
-   [Uygulama ağ geçidi][AppGW] / [WAF][WAF]
-   [Genel IP'ler][PIP]

Genellikle, merkezi BT ve güvenliği ekiplerinin gereksinim tanımı ve çevre ağ işlemleri için sorumluluk.

[![7]][7]

Önceki şemada, hub'ı yerleşik hem de İnternet'e ve bir şirket içi ağ erişimi olan iki duvarlar uygulanması gösterilmektedir. Tek bir hub internet çevre ağına LOB Web uygulaması güvenlik duvarları (Waf) ve/veya güvenlik duvarlarını birden çok grupları kullanarak, çok sayıda desteklemek üzere ölçeklendirilebilir.

[**Sanal ağlar** ] [ VNet] hub genellikle, farklı filtreleme ve trafiği Nva'lar Waf'ler ve Azure ile internet'ten ya da hizmetlerin türü barındırmak için birden çok alt ağ ile sanal ağ temel alınarak oluşturulmuştur Uygulama ağ geçitleri.

[**UDR** ] [ UDR] kullanarak UDR, müşterilerin dağıtabilir güvenlik duvarları, IDs/IPS ve diğer sanal Gereçleri ve ağ trafiğini yönlendirme için güvenlik sınırı ilke zorlaması, bu güvenlik Gereçleri üzerinden denetimi ve İnceleme. Udr, hub'ı hem de trafiği belirli bir özel sanal makineler, ağ sanal Gereçleri ve vDC tarafından kullanılan yük dengeleyicileri aracılığıyla transits garanti uçlar oluşturulabilir. Bu trafiğin uç Aktarımdaki yerleşik vm'lerden doğru sanal Gereçleri için oluşturulan sağlamak için bir UDR uç alt ağları sonraki atlama iç yük dengeleyici ön uç IP adresi ayarlayarak ayarlanması gerekir. İç load balancer sanal Gereçleri (yük dengeleyici arka uç havuzu) iç trafiği dağıtır.

[![8]][8]

[**Ağ sanal Gereçleri** ] [ NVA] hub'ında çevre ağı ile internet erişimi normalde güvenlik duvarları ve/veya Web uygulaması güvenlik duvarları (Waf) bir gruba yönetilir.

Bu uygulamalar, çeşitli güvenlik açıkları ve olası açıklardan yararlanmaya karşı etkilese eğilimindedir ve farklı LOB'lar yaygın olarak birçok web uygulamaları kullanın. Web uygulamaları güvenlik duvarları ürün daha derinlemesine daha genel bir güvenlik duvarı, web uygulamaları (HTTP/HTTPS) saldırıları algılamak için kullanılan özel bir türü var. Waf'ler yılda güvenlik duvarı teknolojisi ile karşılaştırıldığında, iç web sunucuları tehditlere karşı korumak için belirli özellikler kümesi vardır.

Bir güvenlik duvarı Grup güvenlik duvarları dağıtımınızla uçlar barındırılan iş yüklerini korumak için güvenlik kuralları kümesi ile aynı ortak yönetim altında çalışma grubudur ve erişimi denetlemek için şirket ağları. Bir güvenlik duvarı Grup küçük bir WAF ile karşılaştırıldığında yazılım özelleştirilmiş sahiptir, ancak filtrelemek ve herhangi bir türde çıkış ve giriş trafiği incelemek için geniş uygulama kapsamı vardır. Güvenlik Duvarı grupları, normalde ağ sanal, Azure Market'te kullanıma sunulan Gereçleri (Nva) üzerinden Azure'da uygulanır.

Internet'ten kaynaklanan trafik için bir Nva kümesi kullanmak için önerilen ve kaynaklanan trafik için başka şirket içi. Ağ trafiğinin iki kümesi arasında hiçbir güvenlik çevresi sağlayan tek bir Nva kümesi için her ikisini de kullanarak bir güvenlik riski aynıdır. Ayrı nva'larını kullanarak güvenlik kurallarının denetlenmesine karmaşıklığı azaltır ve hangi gelen ağ isteğine hangi kuralları karşılık geldiğini belirlemeyi kolaylaştırır.

En büyük kuruluşlar birden çok etki alanlarını yönetme. Azure DNS, belirli bir etki alanı için DNS kayıtlarını barındırmak için kullanılabilir. Örnek olarak, Azure dış yük dengeleyici (veya Waf'ler) sanal IP adresi (VIP) bir Azure DNS kaydının bir kayıttaki kaydedilebilir.

[**Azure Load Balancer** ] [ ALB] Azure load balancer, gelen trafiği yükü dengelenmiş bir kümede tanımlanmış hizmet örnekleri arasında dağıtabilirsiniz katman 4 (TCP, UDP) hizmeti, bir yüksek kullanılabilirlik sağlar. Yük dengeleyiciye ön uç noktalarından (genel IP uç noktaları veya özel IP uç noktalarını) gönderilen trafik ile veya olmadan adres çevirisi bir dizi arka uç IP adresi havuzu (örnekler alınıyor; yeniden dağıtılabilir Ağ sanal Gereçleri veya Vm'leri).

Azure Load Balancer de çeşitli sunucu örneklerinin sistem durumu araştırma ve bir araştırma yanıt veremediğinde yük dengeleyici sağlıksız örneğine trafik gönderen durdurur. Bir vDC içinde bir dış yük dengeleyici varlığını (örneğin, trafiği Nva'lar için Bakiye) hub ve bağlı bileşenler (çok katmanlı bir uygulamanın farklı VM'ler arasındaki trafiği Dengeleme gibi görevleri gerçekleştirmek için) sunuyoruz.

[**Uygulama ağ geçidi** ] [ AppGW] Microsoft Azure Application Gateway olduğu adanmış bir sanal gereç uygulama teslim denetleyicisi (ADC) sağlayan bir hizmet olarak sunan çeşitli katman 7 Yük Dengeleme Uygulamanız için özellikleri. CPU yoğun SSL sonlandırması yükünü application gateway'e boşaltarak web grubu üretkenliğini iyileştirme olanağı tanır. Ayrıca, gelen trafiğin “hepsini bir kez deneme” yaklaşımıyla dağıtımı, tanımlama bilgisi tabanlı oturum benzeşimi, URL’yi yol tabanlı yönlendirme ve tek bir uygulama ağ geçidi arkasında birden fazla web sitesi barındırma gibi diğer 7. katman yönlendirme özelliklerini sağlar. Application gateway WAF SKU’su kapsamında bir web uygulaması güvenlik duvarı da (WAF) sağlanır. Bu SKU için web uygulamalarını yaygın web güvenlik açıklarına ve açıklardan yararlanmaya karşı koruma sağlar. Application Gateway; İnternet'e yönelik ağ geçidi, yalnızca dahili ağ geçidi veya bu ikisinin bir birleşimi olarak yapılandırılabilir. 

[**Genel IP'ler** ] [ PIP] bazı Azure özellikleri, hizmet uç noktaları, kaynak için internet'ten erişilmesine izin veren genel bir IP adresini ilişkilendirmek etkinleştirin. Bu uç nokta, iç adresi ve bağlantı noktası Azure sanal ağındaki trafiği yönlendirmek için ağ adresi çevirisi (NAT) kullanır. Bu yol, sanal ağa geçirmeniz dış trafiği için birincil yoludur. Genel IP adresleri, hangi trafiğin geçirilir ve nasıl ve nerede sanal ağı açın çevrilir belirlemek için yapılandırılabilir.

#### <a name="component-type-monitoring"></a>Bileşen türü: izleme
İzleme bileşenleri, görünürlük ve tüm diğer bileşenleri türlerinden uyarılar sağlar. Tüm takımlar için bileşenleri izlemeye erişimi olması gereken ve Hizmetleri erişime sahip. Merkezi Yardım Masası veya işlem ekipleri varsa bu bileşenleri tarafından sağlanan verilere erişim tümleşik olması gerekir.

Günlüğe kaydetme ve izleme hizmetleri Azure davranışını izlemek için farklı türde Azure tekliflerini kaynakları barındıran. İdare ve azure'daki iş yüklerinin denetimi tabanlı bir günlük veri yalnızca üzerinde toplama, aynı zamanda belirli bildirilen etkinliklere göre eylem tetikleyici özelliği değildir.

[**Azure İzleyici** ] [ Monitor] -Azure İzleme alanı ayrı ayrı bir spesifik rol ya da görev gerçekleştiren birden çok hizmet içerir. Bu hizmetler birlikte, uygulamanız için telemetri verilerini toplama, analiz etme ve bu verilere göre eylemde bulunmaya ilişkin kapsamlı bir çözüm ve bunları destekleyen Azure kaynaklarını sunar. Karma bir izleme ortamı sağlamak için kritik şirket içi kaynakları izlemek için de çalışır. Kullanılabilir olan araç ve verilerin anlaşılması, uygulamanız için eksiksiz bir izleme stratejisi geliştirmenin ilk adımıdır.

Azure'da günlükleri başlıca iki türde vardır:

-   [**Etkinlik günlükleri** ] [ ActLog] ("İşlem günlüğü" olarak da bilinir) Azure aboneliğindeki kaynaklar üzerinde gerçekleştirilen işlemler hakkında bilgi sağlar. Bu günlükler, Abonelikleriniz için Denetim düzlemi olayları bildirin. Her bir Azure kaynak denetim günlüklerini üretir.

-   [**Azure tanılama günlükleri** ] [ DiagLog] olan bu kaynağın işlemiyle ilgili zengin, sık sık veri sağlayan bir kaynak tarafından oluşturulan günlükleri. Bu günlüklerin içeriği kaynak türüne göre değişir.

[![9]][9]

Bir vDC içinde Nsg'ler günlükleri, özellikle bu bilgileri izlemek son derece önemlidir:

-   [**Olay günlükleri**][NSGLog]: hangi NSG kuralları Vm'lere ve örnek rollerinizin MAC adresini temel alarak uygulanmış bilgiler sağlar.
-   [**Sayaç günlükleri**][NSGLog]: her bir NSG kuralı, trafiğine izin vermek veya reddetmek için yürütüldü kaç kez izler.

Tüm günlükleri, Denetim, statik analiz veya yedekleme amacıyla Azure depolama hesaplarında depolanabilir. Günlükleri bir Azure depolama hesabında depolanır, müşteriler farklı türde çerçeveleri kullanın alma, hazırlama, analiz ve durumunu ve bulut kaynakları durumunu raporlamak için bu verileri görselleştirin.

Büyük kuruluşlar, şirket içi sistemleri izlemek için standart bir çerçeve almış olması ve bulut dağıtımları tarafından oluşturulan günlükleri tümleştirmek için bu çerçeve genişletebilirsiniz. [Log Analytics] [LogAnalytics] istediğiniz kuruluşlar, bulutta tüm günlük tutmak için harika bir seçimdir. Log Analytics, bulut tabanlı bir hizmet olarak uygulandığından, çalışmaya hızlıca altyapı hizmetleri için çok az yatırım ile sağlayabilirsiniz. Log Analytics, mevcut yönetim yatırımlarınızın buluta genişletmek için System Center Operations Manager gibi System Center bileşenleri ile de tümleştirebilirsiniz.

Log Analytics, yardımcı olur toplayın, ilişkilendirin, arayın ve işletim sistemleri, uygulamalar ve altyapı bulut bileşenleri tarafından oluşturulan günlük ve performans verileri üzerinde işlem azure'da bir hizmettir. Bu müşteriler, bir vDC, iş yükleriniz arasında tüm kayıtları çözümlemek için tümleşik arama ve özel panoları kullanarak gerçek zamanlı operasyonel içgörüler sunar.

[Ağ Performansı İzleyicisi'ni (NPM)] [ NPM] çözümü OMS içinde ayrıntılı ağ bilgileri-Azure ağları ile şirket içi ağlar tek bir görünüm dahil olmak üzere uca, sağlayabilir. ExpressRoute ve kamu hizmetleri için belirli izleyicilerde.

#### <a name="component-type-workloads"></a>Bileşen türü: iş yükleri
İş yükü gerçek uygulamalarınızı ve hizmetlerinizi bulunduğu bileşenlerdir. Ayrıca, uygulama geliştirme takımlarınızın zamanlarının çoğunu burada harcama olur.

Gerçek anlamda iş yükü olasılıklar sınırsızdır. Olası iş yükü türleri birkaçı şunlardır:

**İç iş KOLU uygulamaları**

İş kolu satır, bir kuruluşun devam eden işlemi kritik bilgisayar uygulamaları uygulamalardır. LOB uygulamaları, bazı ortak özelliklere sahiptir:

-   **Etkileşimli**. LOB uygulamaları doğası gereği etkileşimli: veriler girilir ve sonuç/reports döndürülür.
-   **Veri odaklı**. LOB veri kullanımı yoğun veritabanları veya diğer depolama için sık erişimli uygulamalardır.
-   **Tümleşik**. Uygulamaları teklif tümleştirme içinde veya kuruluş dışındaki diğer sistemlerle LOB.

**Müşteri Web sitelerini (Internet veya dahili e yönelik) yan yana** internetle etkileşimde çoğu uygulama, web siteleridir. Azure Iaas VM'de veya bir web sitesi çalıştırmak yeteneği sunar bir [Azure Web Apps] [ WebApps] site (PaaS). Azure Web Apps, Web uygulamaları dağıtımını, bir vDC uç izin veren sanal ağlar ile tümleştirmeyi destekler. VNET Tümleştirmesi ile iç kullanıma yönelik web siteleri bakarak, uygulamalarınız için bir Internet uç noktanın kullanıma kopyalaması gerekmez kaynakları aracılığıyla özel ağınızdan özel olmayan Internet yönlendirilebilir adresleri kullanın.

**Büyük veri analizi** veri çok büyük bir birime ölçeği gerektiğinde, veritabanları düzgün bir şekilde ölçeği değil. Hadoop teknoloji, çok sayıda düğümü üzerinde paralel olarak dağıtılmış sorguları çalıştırmak için bir sistem sunar. Müşterilerin Iaas Vm'leri veya PaaS veri iş yüklerini çalıştırmaya yönelik seçeneği vardır ([HDInsight][HDI]). HDInsight, konum tabanlı bir Vnet'te dağıtma destekler, vDC, uçtaki bir kümeye de dağıtılabilir.

**Olayları ve mesajlaşma**
[Azure Event Hubs] [ EventHubs] toplayan, dönüştüren ve depolayan milyonlarca olayı bir hiper ölçekli bir telemetri alma hizmetidir. Dağıtılan bir akış platformu, düşük gecikme süresi ve Azure'a büyük hacimli telemetri alımı ve verilerin birden çok uygulamalardan Okuma olanak sağlayan yapılandırılabilir saklama süresi sunar. Event Hubs'ı tek bir akışın, hem gerçek zamanlı hem de toplu işlem tabanlı işlem hatlarını destekleyebilir.

Yüksek oranda güvenilir bir bulut Mesajlaşma hizmetidir uygulamalar ve hizmetler arasında aracılığıyla uygulanabilir [Azure Service Bus] [ ServiceBus] teklifler zaman uyumsuz ileti istemci ve sunucu arasında aracılı yapılandırılmış ilk-giren ilk çıkar (FIFO) Mesajlaşma ve yayımlama/abone olma özelliklerinin yanı sıra.

[![10]][10]

### <a name="multiple-vdc"></a>Birden çok vDC
Şu ana kadar bu makalede temel bileşenleri ve esnek bir vDC katkıda mimarisi açıklayan, tek bir vDC odaklanıyor. Azure yük dengeleyici, nva'ları, kullanılabilirlik kümeleri, başka mekanizmalar birlikte, Ölçek kümeleri gibi Azure özellikleri, düz SLA düzeyleri üretim hizmetlerinizi oluşturmanıza olanak tanıyan bir sistem katkıda bulunur.

Ancak, tek bir vDC tek bir bölgede barındırılır ve bu bölgenin tamamını etkileyebilir ana kesintiye neden savunmasızdır. Farklı bölgelerde yerleştirilen VDC iki (veya daha fazla), aynı projede dağıtımları hizmetleriyle korumak yüksek SLA elde etmek istediğiniz müşteriler gerekir.

SLA'sı sorunları ek olarak, birden çok VDC dağıtma yarayacak yerde birkaç yaygın senaryo vardır:

-   Bölge/genel durum
-   Olağanüstü Durum Kurtarma
-   DC arasında trafiği yöneltmektir mekanizması

#### <a name="regionalglobal-presence"></a>Bölge/genel durum
Azure veri merkezleri, dünya çapındaki çok sayıda bölgede bulunur. Birçok Azure veri merkezinde seçerken, müşterilerin iki ilgili faktörler göz önünde bulundurmanız gerekir: coğrafi uzaklıkları ve gecikme süresi. VDC vDC ve en iyi kullanıcı deneyimini sunmak için son kullanıcılar arasındaki uzaklığı arasındaki coğrafi uzaklıktan değerlendirmek müşterilerin gerekmektedir.

VDC barındırıldığı Azure bölgesi de kuruluşunuz altında çalıştığı yasal dairesi tarafından kurulan Mevzuat gereksinimlerini karşılaması için gerekir.

#### <a name="disaster-recovery"></a>Olağanüstü Durum Kurtarma
Bir olağanüstü durum kurtarma planının uygulanması önemle endişe iş yükü türüne ve iş yükü durumu farklı VDC arasında eşitleme olanağı ilişkilidir. İdeal olarak, müşterilerin çoğu hızlı yük devretme mekanizması uygulamak için iki farklı VDC içinde çalışan dağıtımlar arasında uygulama verilerini eşitleme istiyorsunuz. Çoğu uygulama için gecikme süresi duyarlıdır ve veri eşitlemesine olası zaman aşımı ve gecikme neden.

Eşitleme ya da farklı VDC uygulamaların sinyal izleme aralarında iletişim gerektirir. Farklı bölgelerdeki iki VDC üzerinden bağlanabilir:

-   Sanal ağ eşleme - VNet eşlemesi bağlanabilir hub'ları bölgeler arasında
-   ExpressRoute özel vDC hub'ları aynı ExpressRoute bağlantı hattına bağlı olduğunuzda eşleme
-   Şirket, omurgası yoluyla bağlı birden çok ExpressRoute bağlantı hatları ve ExpressRoute bağlantı hatlarına bağlı, bir vDC mesh
-   Her Azure bölgesinde vDC hubs'ınız arasında siteden siteye VPN bağlantıları

Genellikle VNet eşlemesi ya da ExpressRoute bağlantıları, Microsoft omurga i yapılandırmamız, daha yüksek bant genişliği ve tutarlı bir gecikme nedeniyle tercih edilen mekanizması mevcuttur.

Farklı bölgelerde bulunan iki (veya daha fazla) farklı VDC arasında dağıtılan bir uygulama doğrulamak için magic tarifi yoktur. Müşteriler gecikme süresi ve bant genişliği bağlantıları ve hedef zaman uyumlu veya zaman uyumsuz veri çoğaltmayı uygun olup olmadığını ve hangi iş yükleriniz için en iyi kurtarma süresi hedefi (RTO) olabilir doğrulamak için ağ nitelik testleri çalıştırmanız gerekir.

#### <a name="mechanism-to-divert-traffic-between-dc"></a>DC arasında trafiği yöneltmektir mekanizması
Başka bir DC'ye içinde trafiği gelen yöneltmektir için bir etkili teknik DNS temel alır. [Azure Traffic Manager] [ TM] en uygun genel bir uç nokta son kullanıcı trafiğini belirli bir vDC yönlendirmek için etki alanı adı sistemi (DNS) mekanizmasını kullanır. Araştırmalar aracılığıyla Traffic Manager hizmeti sistem durumunu farklı VDC içinde ortak Uç noktalara düzenli olarak denetler ve bu Uç noktalara hata durumunda, bu otomatik olarak ikincil vDC için yönlendirir.

Traffic Manager, Azure genel Uç noktalara çalışır ve, örneğin, Denetim/yöneltmektir uygun vDC trafiği Azure VM'ler ile Web uygulamaları için kullanılabilir. Traffic Manager karşılaşıldığında bile bir Azure bölgesinin tamamını başarısızlığı esnektir ve hizmet uç noktalarına (örneğin, başarısız bir hizmetin belirli bir vDC veya vDC seçme içinde çeşitli ölçütlere göre farklı VDC kullanıcı trafiğinin dağıtımını denetleyebilirsiniz en düşük ağ gecikme süresiyle istemci için).

### <a name="conclusion"></a>Sonuç
Sanal veri merkezi bir veri merkezi geçişi maliyetlerini düşürerek ve sistem yönetimini basitleştirir, bulut kaynak kullanımını en üst düzeye çıkarır Azure'da ölçeklenebilir bir mimari oluşturmak için özellikleri ve yetenekleri bir birleşimini kullanan buluta yaklaşımdır. VDC kavramı hub'ında ortak paylaşılan hizmetler sağlama ve uçlar belirli uygulamaları/iş yükleri vererek bir hub uçlar topolojisini temel alır. VDC şirket rolleri, burada farklı departmanlara (Merkezi BT, DevOps, işlem ve bakım), her rol ve görevleri belirli bir liste ile çalışılmasını yapısını eşleşir. VDC "Lift and Shift" Geçiş gereksinimleri karşılayan, ancak aynı zamanda yerel bulut dağıtımlarını birçok avantaj sağlar.

## <a name="references"></a>Başvurular
Aşağıdaki özellikler, bu belgede ele alınan. Daha fazla bilgi için bağlantılar'a tıklayın.

| | | |
|-|-|-|
|Ağ Özellikleri|Yük Dengeleme|Bağlantı|
|[Azure sanal ağları][VNet]</br>[Ağ güvenlik grupları][NSG]</br>[NSG günlüklerini][NSGLog]</br>[Kullanıcı tanımlı yönlendirme][UDR]</br>[Ağ sanal Gereçleri][NVA]</br>[Genel IP adresleri][PIP]</br>[DNS]|[Azure yük dengeleyici (L3) ][ALB]</br>[Uygulama ağ geçidi (L7) ][AppGW]</br>[Web uygulaması güvenlik duvarı][WAF]</br>[Azure Traffic Manager][TM] |[VNet eşlemesi][VNetPeering]</br>[Sanal özel ağ][VPN]</br>[ExpressRoute][ExR]
|Kimlik</br>|İzleme</br>|En İyi Uygulamalar</br>|
|[Azure Active Directory][AAD]</br>[Çok faktörlü kimlik doğrulaması][MFA]</br>[Rol temel erişim denetimleri][RBAC]</br>[Varsayılan AAD rolleri][Roles] |[Azure İzleyici][Monitor]</br>[Etkinlik günlükleri][ActLog]</br>[Tanılama günlükleri][DiagLog]</br>[Microsoft Operations Management Suite'e][OMS]</br>[Ağ Performansı İzleyicisi][NPM]|[En iyi Çevre ağları][DMZ]</br>[Abonelik Yönetimi][SubMgmt]</br>[Kaynak Grup Yönetimi][RGMgmt]</br>[Azure abonelik limitleri][Limits] |
|Diğer Azure Hizmetleri|
|[Azure Web Apps][WebApps]</br>[Hdınsights (Hadoop) ][HDI]</br>[Event Hubs][EventHubs]</br>[Service Bus][ServiceBus]|



## <a name="next-steps"></a>Sonraki Adımlar
 - Keşfedin [VNet eşlemesi][VNetPeering], vDC merkez ve uç tasarımları için taşlarından teknolojisi
 - Uygulama [AAD] [ AAD] kullanmaya başlamak için [RBAC] [ RBAC] keşfetme
 - Bir abonelik ve kaynak yönetim modeli geliştirmek ve RBAC yapısı gereksinimlerini karşılamak için model ve kuruluşunuzun ilkeleri. En önemli etkinlik planlamaktadır. Yeniden düzenlemeyi, birleşmeler, yeni ürün satırlar gibi şeyler için kullanışlı, artacak planlayın.

<!--Image References-->
[0]: ./media/networking-virtual-datacenter/redundant-equipment.png "bileşeni çakışma örnekleri" 
[1]: ./media/networking-virtual-datacenter/vdc-high-level.png "merkez ve uç vDC üst düzey bir örnek"
[2]: ./media/networking-virtual-datacenter/hub-spokes-cluster.png "hub ve bağlı bileşenler kümesi"
[3]: ./media/networking-virtual-datacenter/spoke-to-spoke.png "uç uç"
[4]: ./media/networking-virtual-datacenter/vdc-block-level-diagram.png "blok düzeyinde vDC diyagramı"
[5]: ./media/networking-virtual-datacenter/users-groups-subsciptions.png "kullanıcıları, grupları, abonelikleri ve projeleri"
[6]: ./media/networking-virtual-datacenter/infrastructure-high-level.png "üst düzey altyapı diyagramı"
[7]: ./media/networking-virtual-datacenter/highlevel-perimeter-networks.png "üst düzey altyapı diyagramı"
[8]: ./media/networking-virtual-datacenter/vnet-peering-perimeter-neworks.png "VNet eşlemesi ve Çevre ağları"
[9]: ./media/networking-virtual-datacenter/high-level-diagram-monitoring.png "izleme için yüksek düzey bir diyagramı"
[10]: ./media/networking-virtual-datacenter/high-level-workloads.png "iş yükü için yüksek düzey bir diyagramı"

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
