---
title: Azure Kubernetes Service (AKS) ilkelerini destekler
description: Azure Kubernetes Service (AKS) destek ilkeleri, paylaşılan Sorumluluklar, Önizleme/alfa/Beta özellikleri hakkında bilgi edinin.
services: container-service
author: jnoller
ms.service: container-service
ms.topic: article
ms.date: 04/01/2019
ms.author: jenoller
ms.openlocfilehash: f173fc7c794729eae8c60cceefa88d153800a816
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61032483"
---
# <a name="azure-kubernetes-service-aks-support-policies"></a>Azure Kubernetes Service (AKS) ilkelerini destekler

Bu makalede AKS teknik destek ilkeleri, kısıtlamalar, ayrıntılarla sağlar ve çalışan düğümü yönetimi, yönetilen dahil olmak üzere ayrıntılarını düzlemi bileşenler, üçüncü taraf açık kaynak bileşenlerini ve güvenlik denetim / düzeltme eki yönetimi.

## <a name="service-updates--releases"></a>Hizmet güncelleştirmeleri ve sürümler

* Sürüm bilgileri için bkz. [AKS sürüm notları][1]
* Genel Önizleme özellikleri için bkz: [AKS Önizleme özellikleri ve ilgili projeler][2]

## <a name="what-is-managed"></a>Ne 'yönetilen'

Alt düzey denetim ve özelleştirme seçenekleri için kullanıcının yararlanmasını gösterdiğiniz, temel Iaas bulut bileşenlerini işlem ve ağ gibi yaygın yapılandırmalar kümesini temsil eden anahtar teslim Kubernetes dağıtımı AKS hizmetidir ve Kubernetes için gereken özellikleri. Bu hizmet kullanan müşteriler, sınırlı sayıda özelleştirmeleri, dağıtım ve diğer seçenekleri vardır. Bu da müşterilerin endişe veya Kubernetes kümesi doğrudan yönetmek gerekmez anlamına gelir.

AKS ile müşteri tam olarak yönetilen alır **denetim düzlemi** – burada tüm bileşenleri ve hizmetleri çalıştırmak ve son kullanıcılara Kubernetes kümeleri sağlamak için gereken denetim düzlemi içerir. Tüm Kubernetes bileşenleri tutulur ve Microsoft tarafından işletilen.

İle yönetilen **denetim düzlemi** aşağıdaki bileşenleri yönetilir ve Microsoft tarafından izlenen:

* Kubelet / Kubernetes API sunucusu
* Etcd veya uyumlu bir anahtar/değer deposu – servis, ölçeklenebilirlik ve çalışma zamanı kalitesini de dahil olmak üzere
* DNS Hizmetleri (örneğin, kube-dns / CoreDNS)
* Kubernetes Proxy/ağ

AKS %100 managed değil **küme** çözüm. Belirli bileşenler (örneğin, çalışan düğümleri) belirli sahip **paylaşılan sorumlulukları** çalışmaları nereye AKS kümesini korumanız kullanıcı etkileşimi gerektirir. Örneğin, çalışan düğümüne işletim sistemi güvenlik düzeltme eki uygulama.

 **Yönetilen**, Microsoft ve AKS takım dağıtır, anlamına gelir çalışır ve bu hizmetlerin işlevselliği ve kullanılabilirlik için sorumludur. **Müşteriler, bu bileşenlerin alter olamaz**. Özelleştirme, tutarlı ve ölçeklenebilir kullanıcı deneyimi sağlamak için sınırlıdır. Tamamen özelleştirilebilir çözümü için bkz: [AKS altyapısı](https://github.com/Azure/aks-engine).

> [!NOTE]
> Azure Kubernetes hizmeti çalışan düğümleri normal Azure Iaas kaynaklarının, Azure Portalı'nda görünür ancak bu sanal makineleri özel bir Azure kaynak grubu içinde dağıtılan anlaşılması önemlidir (MC ile önek\\*). Bir kullanıcı, SSH bunları yalnızca normal sanal makineler gibi değiştirebilir (ancak, temel işletim sistemi görüntüsünü değiştiremezsiniz ve değişiklikleri değil bir güncelleştirme ile kalıcı veya yeniden başlatmada tekrar) ve diğer Azure kaynaklarında iliştirdiğiniz veya aksi halde değiştirin. **Ancak, bu bant yönetimi ve özelleştirmesi dışında yapılması AKS kümesi desteklenmeyen hale gelebilir anlamına gelir. Çalışan düğümü değişikliğinin Microsoft Support yönlendirilmezseniz kaçının.**

## <a name="what-is-shared-responsibility"></a>Sorumluluk paylaşılan nedir

Küme oluşturma sırasında AKS Kubernetes çalışan düğümü müşteri tarafından tanımlanan bir dizi oluşturur. Bu düğümler belirtildiği gibi müşteri iş yüklerinin yürütüldüğü ' dir. Müşterilerin kendi ve görüntüleyebilir veya bu çalışan düğümleri değiştirebilirsiniz.

Bu düğümler özel kod yürüten ve hassas verileri depolamak için Microsoft desteğine sahip **sınırlı erişim** tüm müşteri için küme düğümleri. Microsoft destek oturum açın, komutları yürütün veya express müşteri izni ve/veya Yardım desteği ekibi tarafından belirtildiği gibi hata ayıklama komutları yürütmek için olmadan bu düğümler için günlükleri görüntüleyin.

Bu alt düğümler hassas yapısı nedeniyle, bu düğümler 'arka planda' yönetim sınırlamak için çok dikkatli Microsoft alır. Kubernetes düğümlerini etcd ve diğer bileşenleri başarısız (Microsoft tarafından yönetilen) ana olsa bile, iş yükü çoğu durumda çalışmaya devam eder. Çalışan düğümlerinin bakım değiştirilirse, bu iş yükü ve/veya veri kaybına neden ve desteklenmeyen küme oluşturma.

## <a name="azure-kubernetes-service-support-coverage"></a>Azure Kubernetes hizmeti destek kapsamı

**Microsoft teknik destek için aşağıdakileri sağlar:**

* Sağlanan ve Kubernetes hizmeti (örneğin, API sunucusu) tarafından desteklenen tüm Kubernetes bileşenleri bağlantısı
* Yönetim, çalışma süresi, QoS ve Kubernetes işlemlerini düzlemi Hizmetleri (ana Kubernetes API sunucusu düğümleri etcd, örneğin kube-dns. denetleme
* Etcd destek olağanüstü durum planlama ve küme durumu geri yüklemesinin her 30 dakikada bir otomatik ve şeffaf yedeklerini tüm etcd verileri içerir. Bu yedeklemeler doğrudan müşteriler/kullanıcıları için kullanılabilir değil ve olasılığına karşı veri güvenirliliğini ve tutarlılık sağlamak için kullanılır
* Kubernetes için Azure bulut sağlayıcısı sürücü diğer azure'a tümleştirmeler gibi herhangi bir tümleştirme noktası kalıcı ağ birimler (Kubernetes ve Azure CNI) yük Dengeleyiciler gibi hizmetleri
* Sorular veya sorunlar özelleştirme denetim düzlemi bileşenlerin Kubernetes API sunucusu, etcd ve kube-dns gibi geçici bir çözüm.
* Ağ konuları hakkında Azure CNI, Kubernetes veya diğer ağ gibi erişim ve işlevsellik sorunları (DNS çözümlemesi, paket kaybı, Yönlendirme ve benzeri) verir.
  * Desteklenen ağ senaryoları Kubernetes (Temel) ve ağ oluşturma (Azure CNI) Gelişmiş içerir küme ve ilişkili bileşenleri, diğer Azure Hizmetleri ve uygulamaları bağlantısı içinde. Ayrıca, giriş denetleyicileri ve giriş/yük dengeleyici yapılandırması, ağ performansı ve gecikme süresi Microsoft tarafından desteklenir.

**Microsoft teknik destek için aşağıdakileri sağlamaz:**

* Danışmanlık / "Nasıl yapılır" kullanımı Kubernetes soru, örneğin özel giriş denetleyicileri, uygulama iş yükü sorular, oluşturma ve üçüncü taraf OSS paketlerini veya araçları kapsam dışındadır.
  * Danışmanlık biletlerini AKS küme işlevselliğini özelleştirme ayarlama – örn Kubernetes operations sorunları/Yardım-tos kapsamında olan.
* Kubernetes bir parçası olarak sağlanmayan üçüncü taraf açık kaynak projeleri, Denetim düzlemi veya AKS kümeleriyle Istio, Helm, Envoy ve diğerleri gibi dağıtılabilir.
  * Helm ve Kured, gibi üçüncü taraf açık kaynak projeleri için en iyi girişim desteği için örnekler ve Microsoft belgelerine ve burada bu üçüncü taraf açık kaynak aracı Kubernetes Azure bulut sağlayıcısı ile tümleşerek sağlanan uygulamaları sağlanır veya diğer AKS özgü hatalar.
* Üçüncü taraf kapalı kaynak yazılım – bu tarama araçlarını, cihazlar veya yazılım ağ güvenlik içerebilir.
* "Çoklu bulut" ya da çok satıcılı derleme yönüyle ilgili sorunlar desteklenmez, örneğin bir federe çoklu genel bulut satıcı çözümü çalıştırma desteklenmiyor.
* Resmi belgelenen dışındaki, belirli ağ özelleştirmeleri [AKS belgelerini][3].
  > [!NOTE]
  > Sorunlar ve hatalar ağ güvenlik gruplarının çevresinde desteklenir. Örneğin, destek, Nsg'ler güncelleştirmek başarısız olan veya beklenmeyen NSG veya yük dengeleyici davranışını çevresinde soruları yanıtlayabilir.

## <a name="azure-kubernetes-service-support-coverage-worker-nodes"></a>Azure Kubernetes hizmeti destek kapsamı (çalışan düğümleri)

### <a name="microsoft-responsibilities-for-azure-kubernetes-service-worker-nodes"></a>Azure Kubernetes hizmeti çalışan düğümleri için Microsoft sorumlulukları

Belirtilen `shared responsibility` bölümünde, çalışan düğümleri kalan bir Eklem sorumluluk modeline Kubernetes burada:

* Temel işletim sistemi görüntüsü (örneğin, izleme ve aracılar ağ) gerekli eklemelerle sağlar
* Çalışan düğümlerinin işletim sistemi düzeltme eklerini otomatik teslimi
* Kubernetes ile ilgili sorunları otomatik düzeltme denetim düzlemi bileşenleri gibi çalışan düğümlerinde çalışan:
  * kube-Ara
  * Kubernetes için iletişim yolları sağlamak ağ tüneller ana bileşenleri
  * kubelet
  * docker/moby arka plan programı

> [!NOTE]
> Bir denetim düzlemi bileşeni bir çalışan düğümü üzerinde çalışmaz durumda ise, AKS takım bu sorunu çözmek için tüm çalışan düğümü yeniden başlatma gerekebilir. Bu bir kullanıcı adına yapılan ve müşteri yükseltme (Müşteriler etkin iş yükü ve verilere erişmek son) yapılmadığı sürece gerçekleştirilmeyen. Olası AKS personel gerekli hale getirmek için çalışır olduğunda uygulama olmayan etkileyen yeniden başlatın.

### <a name="customer-responsibilities-for-azure-kubernetes-service-worker-nodes"></a>Azure Kubernetes hizmeti çalışan düğümleri için müşteri sorumlulukları

**Microsoft desteklemez:**

- Otomatik olarak yeniden başlatma çalışan düğümlerinin işletim sistemi için etkili olması için düzeltme eki düzeyi **(şu anda, aşağıya bakın)**

İşletim sistemi düzeltme ekleri, çalışan düğümlerine dağıtılır, ancak bunu **müşteri sorumluluk** değişikliklerin etkili olması çalışan düğümü yeniden başlatmak için. Otomatik Düzeltme bu paylaşılan kitaplıklar, SSHD gibi Daemon'ları ve diğer sistem/işletim sistemi düzeyinde bileşenleri içerir.

Kubernetes yükseltmeler **müşterilerin yükseltme yürütülmesi için sorumlu** Azure Denetim Masası'nı veya Azure CLI aracılığıyla. Bu, güvenlik veya Kubernetes işlevselliği geliştirmeleri içeren güncelleştirmeleri için geçerlidir.

> [!NOTE]
> AKS olduğu gibi bir `managed service` düzeltme ekleri, güncelleştirmeleri, günlük toplama ve diğer tamamen yönetilen ve El değmeden hizmeti sağlamak için daha fazla bilgi için sorumluluk kaldırma son hedeflerimiz hizmeti içerir. Bizim altyapımız yönetim uçtan uca arttıkça gelecek sürümlerde bu öğeler (düğümün, otomatik düzeltme eki uygulama) kaldırılabilir.

### <a name="security-issues-and-patching"></a>Güvenlik sorunlarını ve düzeltme eki uygulama

Bazı durumlarda (örneğin, CVEs), bir veya daha fazla bileşen AKS hizmeti bir güvenlik açığı bulunabilir. Böyle senaryolarda, AKS Mümkünse sorunu gidermek için etkilenen tüm kümelerin düzeltme eki uygulamanıza veya kullanıcılar için yükseltme rehberlik.

Bu tür bir güvenlik açığı tarafından etkilenen çalışan düğümleri için kesintisiz düzeltme eki bir sorun varsa, AKS ekibi bu düzeltmeyi uygulamak ve değişikliği kullanıcılara bildirmek.

Bir güvenlik düzeltme eki gerektiriyorsa, çalışan düğümü yeniden başlatır, Microsoft, müşteri bu gereksinimi bilgilendirir ve yeniden yürütme veya güncelleştirmek için küme düzeltme eki almaya müşteri sorumludur. Kullanıcılar, düzeltme ekleri AKS yönergeler uyarınca geçerli değil, küme için sorunu/sorunları savunmasız devam eder.

### <a name="node-maintenance-and-access"></a>Düğüm Bakım ve erişim

Müşteriler sahipliğini altında ve çalışan düğümleri paylaşılan Sorumluluklar olduğundan müşteriler bu çalışanlar oturum ve paketlerini kaldırma ve yeni paketleri yükleme çekirdek güncelleştirmeleri gibi zararlı olabilecek değişiklikleri gerçekleştirmek.

Müşteriler yıkıcı, veya çevrimdışı duruma için Denetim düzlemi bileşenleri tetikleyen Eylemler gerçekleştirin ya da aksi işlevsel olmayan duruma, AKS hizmeti bu hatayı algılar ve çalışan düğümü için önceki çalışma geri yüklemek için otomatik düzeltme gerçekleştirme durumu.

Müşteriler, oturum açın ve çalışan düğümleri alter rağmen açıktır *önerilmez* çünkü bu kümenizi desteklenmeyen hale getirebilirsiniz.

## <a name="network-ports-access-and-network-security-groups"></a>Ağ bağlantı noktaları, erişimi ve ağ güvenlik grupları

Yönetilen bir hizmet olarak AKS belirli ağ ve bağlantı gereksinimleri vardır. Bu gereksinimleri normal Iaas bileşenleri daha az esnektir. Diğer Iaas bileşenleri, kümenizin desteklenmeyen (giden bağlantı noktası 443 engelleme gibi güvenlik duvarı kuralları) belirli işlemleri (örneğin, ağ güvenlik grubu kurallarını, belirli bir bağlantı noktası engelleme, URL beyaz listeye ekleme ve benzeri özelleştirme) işleyebilir.

> [!NOTE]
> Çıkış (örneğin, beyaz açık etki alanı/bağlantı noktası) kümenizden tamamen kilitlemek desteklenen bir AKS senaryo şu anda değil. URL ve bağlantı noktaları listesi uyarı olmadan değişebilir ve bir bilet Azure destek birimi tarafından sağlanabilir. Kabul edebileceğiniz kullanmaya başlayan müşteriler için sağlanan listedir *kullanılabilirlik kümenizin herhangi bir zamanda etkilenebilir.*

## <a name="alphabeta-kubernetes-features-not-supported"></a>Alfa/Beta Kubernetes özellikleri (desteklenmiyor)

AKS, yalnızca içinde yukarı akış Kubernetes proje kararlı özelliklerini destekler. Yukarı Akış Kubernetes alfa/Beta özelliklerin, aksi takdirde bildirilmesi ve belgelenen sürece desteklenmez.

Burada alfa veya Beta özelliği önce GA alınamaz iki durum vardır:

* Müşteriler, AKS ürün, destek ya da mühendislik ekipleri ile karşıladığınızdan ve bu yeni özellikleri denemek için açıkça istendi.
* Bu özellikler silinmiş [bir özellik bayrağını etkin] [ 2] (açık kabul et)

## <a name="preview-features--feature-flags"></a>Özelliklerin önizlemesini / özellik bayrakları

Özellikler ve genişletilmiş test, topluluk ve kullanıcı geri bildirimi gerektiren işlevler için Microsoft yeni önizleme özellikleri veya bir özellik bayrağının arkasında özellikleri serbest bırakır. Bu özellikler, yayın öncesi düşünülmelidir / çalıştırdığınız ve Beta maruz kullanıcılar bu yeni özellikleri denemek için bir şans verin.

Ancak, bu Önizleme / özellikleri değil amacı, üretim kullanımı için – API'leri, davranış değişikliği, hata düzeltmeleri ve diğer değişiklikler, yapılabilir özellik bayrağını kararsız kümeleri ve kapalı kalma süresi neden olabilir. Bu özellikler için desteği, hata/sorun Raporlama ile sınırlıdır. Bu özelliklere üretim sistemlerine ya da abonelik etkinleştirmeyin.

> [!NOTE]
> Önizleme özelliklerini etkinleştirme etkinleşir Azure **abonelik** düzeyi. Önizleme özellikleri, normal işlemleri etkileyen varsayılan API davranışını değiştirebilirsiniz gibi üretim abonelikte yüklemeyin.

## <a name="upstream-bugs-and-issues"></a>Yukarı Akış hatalar ve sorunlar

Geliştirme hızını Yukarı Akış Kubernetes projeye göz önünde bulundurulduğunda, var. neredeyse şaşmaz biçimde yama yapılamıyor veya AKS sisteminde geçici çalışan ve bunun yerine (örneğin, Kubernetes, düğüm/çalışan işletim sistemleri ve çekirdekler) Yukarı Akış projeleri için daha büyük düzeltme iste hataları (Azure bulut sağlayıcısı gibi) olduğumuz bileşenleri için AKS/Azure personelinin sorunu Yukarı Akış topluluğa düzeltme için kararlıyız.

Bir teknik destek sorununu bir veya daha fazla Yukarı Akış hataları için kök neden olduğu durumlarda, AKS desteklemek ve mühendislik aşağıdakileri yapar:

* Tanımlamak ve Yukarı Akış hataları neden bu küme ve/veya iş yükü etkilediğini dair tüm destekleyici ayrıntıları ile bağlayın. Müşteriler gerekli depoları / sorunlarını izlemek ve ne zaman yeni bir Kubernetes/diğer sürüm fix(es) içerecektir görmek için sorunları için bağlantılarla birlikte sağlanır
* Olası belgele veya risk azaltma işlemleri: Bazı durumlarda, sorunun geçici çözümü – bu durumda, mümkün olabilir bir "[bilinen sorun](https://github.com/Azure/AKS/issues?q=is%3Aissue+is%3Aopen+label%3Aknown-issue)" açıklayan AKS havuzda Dosyalanan:
  * Sorun ve Yukarı Akış hataları Bağla
  * Geçici çözüm ve çözüm yükseltme/diğer Kalıcılık ayrıntılarla
  * Kaba zaman çizelgesi üzerinde Yukarı Akış yayın temposudur tabanlı eklemek için

[1]: https://github.com/Azure/AKS/releases
[2]: https://github.com/Azure/AKS/blob/master/previews.md
[3]: https://docs.microsoft.com/azure/aks/
