---
title: Destek ilkeleri için Azure Kubernetes Service (AKS)
description: Azure Kubernetes Service (AKS) destek ilkeleri, paylaşılan sorumluluklar ve Önizleme (veya alfa veya beta) olan özellikler hakkında bilgi edinin.
services: container-service
author: jnoller
ms.service: container-service
ms.topic: article
ms.date: 04/01/2019
ms.author: jenoller
ms.openlocfilehash: 0d2c080be727d2ae13d6d9e5274f17cadffbe640
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65786470"
---
# <a name="support-policies-for-azure-kubernetes-service"></a>Destek ilkeleri için Azure Kubernetes hizmeti

Bu makalede, teknik destek ilkeleri ve kısıtlamaları için Azure Kubernetes Service (AKS) hakkında ayrıntılar sağlar. Makale ayrıca alt düğüm yönetimi, yönetilen denetim düzlemi bileşenleri, üçüncü taraf açık kaynak bileşenlerini ve güvenlik veya düzeltme eki yönetimi ayrıntıları.

## <a name="service-updates-and-releases"></a>Hizmet güncelleştirmeleri ve sürümler

* Sürüm bilgileri için bkz. [AKS sürüm notları](https://github.com/Azure/AKS/releases).
* Önizlemedeki özellikler hakkında daha fazla bilgi için bkz: [AKS Önizleme özellikleri ve ilgili projeler](https://github.com/Azure/AKS/blob/master/previews.md).

## <a name="managed-features-in-aks"></a>AKS özelliklerinde yönetilen

İşlem veya ağ bileşenleri gibi bir hizmet (Iaas) bulut bileşenleri olarak temel altyapı verin kullanıcıların erişim alt düzey denetim ve özelleştirme seçenekleri. Bunun aksine, AKS müşteriler yapılandırmaların ve özelliklerin ihtiyaç duydukları ortak kümesini sağlayan kullanıma hazır bir Kubernetes dağıtımı sağlar. AKS müşteriler özelleştirme, dağıtma ve diğer seçenekleri sınırlıdır. Bu müşteriler, Kubernetes kümelerini doğrudan yönetmek ya da endişe gerek yoktur.

AKS ile müşteri tam olarak yönetilen alır *denetim düzlemi*. Denetim düzlemi tüm bileşenleri ve müşterinin çalışır ve son kullanıcılara Kubernetes kümeleri sağlamak için gereken hizmetleri içerir. Tüm Kubernetes bileşenleri tutulur ve Microsoft tarafından işletilen.

Microsoft yönetir ve aşağıdaki bileşenleri Denetim Masası aracılığıyla izler:

* Kubelet veya Kubernetes API sunucusu
* Etcd veya uyumlu bir anahtar-değer deposu, hizmet kalitesi (QoS), ölçeklenebilirlik ve çalışma zamanı sağlama
* DNS Hizmetleri (örneğin, kube-dns veya CoreDNS)
* Kubernetes proxy ya da ağ iletişimi

AKS kümesi tamamen yönetilen çözüm değildir. Çalışan düğümleri gibi bazı bileşenleri *sorumluluk paylaşılan*, burada kullanıcılar AKS kümesi sağlamak gerekir. Kullanıcı girişi, örneğin, bir çalışan düğümü işletim sistemi (OS) güvenlik düzeltme eki uygulamak için gereklidir.

Hizmetler *yönetilen* anlamında Microsoft ve AKS takım dağıtır, çalışır ve hizmet kullanılabilirliği ve işlevselliği için sorumlu olduğunu. Müşteriler, bu yönetilen bileşenleri değiştirilemiyor. Microsoft, tutarlı ve ölçeklenebilir kullanıcı deneyimi sağlamak üzere özelleştirme sınırlar. Tamamen özelleştirilebilir çözümü için bkz: [AKS altyapısı](https://github.com/Azure/aks-engine).

> [!NOTE]
> AKS çalışan düğümleri normal Azure Iaas kaynakları Azure portalında görünür. Ancak bu sanal makineler bir özel bir Azure kaynak grubunda dağıtılan (MC ile önek\\*). AKS çalışan düğümleri değiştirmek mümkündür. Örneğin, AKS çalışan düğümü, sanal makineleri normal şekilde değiştirmek için bir Secure Shell (SSH) kullanabilirsiniz (ancak, temel işletim sistemi görüntüsünü değiştiremezsiniz ve değişiklikleri değil bir güncelleştirme ile kalıcı veya yeniden başlatmada tekrar) ve diğer Azure kaynakları için AKS ekleyebilirsiniz çalışan düğümleri. Ancak değişiklik yaptığınızda *bant yönetimi ve özelleştirmesi,* AKS kümesi desteklenmeyen hale gelebilir. Çalışan düğümlerinin Microsoft Support değişiklik yapmanızı bildirilmedikçe değiştirmekten kaçının.

## <a name="shared-responsibility"></a>Paylaşılan sorumluluk

Bir küme oluşturulduğunda, müşteri AKS oluşturur Kubernetes çalışan düğümü tanımlar. Müşteri iş yüklerinin bu düğümler üzerinde yürütülür. Müşterilerin kendi ve görüntüleyebilir veya çalışan düğümlerinin değiştirebilirsiniz.

Microsoft Support müşteri küme düğümlerine özel bir kod yürütebilir ve hassas verileri depolamak için bunları yalnızca sınırlı bir şekilde erişebilir. Microsoft Support, komutları yürütmek ya da bu düğümler olmadan express müşteri izni veya Yardım için günlükleri görüntülemek oturum açılamıyor.

Çalışan düğümlerinin önemli olduğundan, Microsoft arka plan yönetimleri sınırlamak için çok dikkatli alır. Çoğu durumda, iş yükünüz Kubernetes düğümleri, etcd ve diğer Microsoft tarafından yönetilen bileşenleri başarısız ana bile çalışmaya devam eder. Carelessly değiştirilmiş çalışan düğümlerine verileri ve iş yükleri kayıpları neden olabilir ve küme desteklenmeyen işleyebilirsiniz.

## <a name="aks-support-coverage"></a>AKS destek kapsamı

Microsoft teknik destek için aşağıdakileri sağlar:

* Kubernetes hizmeti sağlayan ve desteklediği, API sunucusu gibi tüm Kubernetes bileşenleri bağlantısı.
* Yönetim, çalışma süresi, QoS ve Kubernetes işlemlerini düzlemi Hizmetleri (ana düğüm Kubernetes API sunucusu, etcd ve kube-dns) denetler.
* Etcd. Destek, olağanüstü durum planlama ve küme durumu geri yüklemesinin her 30 dakikada bir otomatik ve şeffaf yedeklemeleri tüm etcd veri içerir. Bu yedeklemeler, müşterileriniz veya kullanıcılarınız için doğrudan kullanılamaz. Bunlar olasılığına karşı veri güvenirliliğini ve tutarlılık emin olun.
* Kubernetes için Azure bulut sağlayıcısı sürücüsündeki tüm tümleştirme noktaları. Bunlar, yük Dengeleyiciler, kalıcı birimleri veya ağ (Kubernetes ve Azure CNI) gibi diğer Azure Hizmetleri ile tümleştirmeler içerir.
* Sorular veya sorunlar Kubernetes API sunucusu, etcd ve kube-dns gibi denetim düzlemi bileşenlerinin özelleştirme hakkında daha fazla.
* Ağ, Azure CNI, kubernetes, veya diğer ağ erişimi ve işlevsellik gibi sorunları hakkında sorun. Sorunları DNS çözümlemesi, paket kaybı, yönlendirme vb. içerebilir. Microsoft, çeşitli ağ oluşturma senaryoları destekler:
  * Kubernetes (Temel) ve ağ iletişimi (Azure CNI) küme içinde Gelişmiş ve bileşenleri ilişkili
  * Diğer Azure Hizmetleri ve uygulamaları için bağlantı
  * Giriş denetleyicileri ve giriş veya yük dengeleyici yapılandırması
  * Ağ performansı ve gecikme süresi

Microsoft teknik destek için aşağıdakileri sağlamaz:

* Kubernetes kullanma hakkında sorular. Örneğin, Microsoft Support özel giriş denetleyicileri oluşturma, uygulama iş yüklerini kullanın veya üçüncü taraf veya açık kaynak yazılım paketleri veya araçları hakkında öneri sağlamaz.
  > [!NOTE]
  > Microsoft Support AKS kümesi işlevselliği, özelleştirme ve (örneğin, Kubernetes işlemlerinin sorunlarını ve yordamlar) ayarlama hakkında önerilerde bulunabilir.
* Kubernetes bir parçası olarak sağlanmayan üçüncü taraf açık kaynak projeleri, Denetim düzlemi veya AKS kümeleriyle dağıtılabilir. Bu projeler Istio, Helm, Envoy veya başkalarının içerebilir.
  > [!NOTE]
  > Microsoft Helm ve Kured gibi üçüncü taraf açık kaynak projeleri için mümkün olan en iyi destek sağlar. Kubernetes Azure bulut sağlayıcısı veya diğer AKS özgü hatalar ile üçüncü taraf açık kaynak aracı tümleşir burada Microsoft örnekler ve Microsoft belgelerine uygulamalardan destekler.
* Üçüncü taraf kapalı kaynaklı yazılım. Bu yazılım güvenlik tarama araçlarını ve ağ cihazları veya yazılım içerebilir.
* Multicloud veya birçok satıcıdan derleme yönüyle ilgili sorun. Örneğin, Microsoft, bir Federasyon multipublic bulut satıcı çözümü çalıştırma ile ilgili sorunları desteklemiyor.
* Ağ listelenenler dışında özelleştirmeleri [AKS belgelerini](https://docs.microsoft.com/azure/aks/).
  > [!NOTE]
  > Microsoft, sorunlar ve hatalar için ağ güvenlik grupları (Nsg'ler) ilgili desteklemiyor. Örneğin, Microsoft Support güncelleştirmek için bir NSG arızasından ya da beklenmeyen bir NSG veya yük dengeleyici davranışını hakkında sorular yanıt verebilirsiniz.

## <a name="aks-support-coverage-for-worker-nodes"></a>Çalışan düğümleri için AKS destek kapsamı

### <a name="microsoft-responsibilities-for-aks-worker-nodes"></a>AKS çalışan düğümleri için Microsoft sorumlulukları

Microsoft ve müşterileri Kubernetes çalışan düğümleri için sorumluluk paylaşma yeri:

* Temel işletim sistemi görüntüsünü ekleme (örneğin, izleme ve aracılar ağ) zorunludur.
* Çalışan düğümlerinin işletim sistemi düzeltme eklerini otomatik olarak alırsınız.
* Kubernetes ile ilgili sorunlar ve çalışan düğümleri üzerinde çalışmak bileşenleri otomatik olarak düzeltilir düzlemi denetler. Bileşenleri şunları içerir:
  * kube-Ara
  * Kubernetes için iletişim yolları sağlamak ağ tüneller ana bileşenleri
  * kubelet
  * Docker veya Moby arka plan programı

> [!NOTE]
> Bir denetim düzlemi bileşeni işletimsel, değilse bir çalışan düğümü üzerinde AKS takımın tüm çalışan düğümü yeniden başlatma gerekebilir. Yalnızca müşteri sorunu iletir, müşterinin etkin iş yükü ve verilere kendi kısıtlı erişim nedeniyle AKS takım bir çalışan düğümü yeniden başlatır. Mümkün olduğunda, uygulamanın gerekli yeniden başlatma etkilemesini AKS takım çalışır.

### <a name="customer-responsibilities-for-aks-worker-nodes"></a>AKS çalışan düğümleri için müşteri sorumlulukları

Microsoft, çalışan düğümlerinin işletim sistemi düzeyinde düzeltme eki uygulamak için otomatik olarak yeniden değil. İşletim sistemi düzeltme ekleri, çalışan düğümlerine teslim edilir olsa da *müşteri* değişiklikleri uygulamak için çalışan düğümü yeniden başlatma için sorumludur. Paylaşılan kitaplıklar, karma katı hal sürücüsü (SSHD) gibi Daemon'ları ve diğer bileşenleri sistem veya işletim sistemi düzeyinde otomatik olarak düzeltme.

Müşteriler, Kubernetes yükseltmeleri yürütmek için sorumludur. Bunlar Azure denetim masasında veya Azure CLI aracılığıyla yükseltme yürütebilir. Bu, güvenlik veya Kubernetes işlevselliği geliştirmeleri içeren güncelleştirmeleri için geçerlidir.

> [!NOTE]
> AKS olduğundan bir *yönetilen hizmet*, son hedeflerini güncelleştirmeleri, düzeltme ekleri sorumluluğunu kaldırılması ve hizmet yönetimi daha eksiksiz ve El değmeden yapmak için koleksiyon oturumu. Uçtan uca Yönetim için hizmetin kapasite arttıkça, gelecek sürümlerde bazı işlevleri (örneğin, düğümün ve otomatik düzeltme eki uygulama) sayabiliriz.

### <a name="security-issues-and-patching"></a>Güvenlik sorunlarını ve düzeltme eki uygulama

Bir veya daha fazla AKS bileşenlerinde güvenlik açığı bulunursa, AKS takım sorunu gidermek için etkilenen tüm kümelerin düzeltme eki. Alternatif olarak, takım kullanıcılar yükseltme yönergeleri sağlar.

Bir güvenlik hatası etkiler, kesintisiz düzeltme eki, kullanılabilir olduğunu, çalışan düğümleri için AKS takım, düzeltme eki uygulamak ve değişikliği kullanıcılara bildirmek.

Bir güvenlik düzeltme eki çalışan düğümü yeniden başlatma gerektirdiğinde, Microsoft müşterilerin bu gereksinimi bilgilendirir. Yeniden başlatmadan veya küme düzeltme eki almaya güncelleştirmek için müşteri sorumludur. Kullanıcıları düzeltme eklerine göre AKS Kılavuzu uygulamazsanız, kendi küme güvenlik sorundan olmaya devam edecektir.

### <a name="node-maintenance-and-access"></a>Düğüm Bakım ve erişim

Çalışan düğümü olan bir paylaşılan sorumluluklar ve müşteriler tarafından sahip olunan. Bu nedenle, müşterilerin kendi çalışan düğümleri için oturum açın ve çekirdek güncelleştirmeleri yükleme veya paketlerini kaldırma gibi zararlı değişiklik olanağına sahip.

Müşteriler bozucu değişiklik ya da Denetim düzlemi bileşenleri çevrimdışı veya işlevsiz neden AKS bu hatayı algılar ve otomatik olarak çalışan düğümü önceki çalışma durumuna geri yükleyin.

Müşteriler için oturum açın ve çalışan düğümleri değiştirme olsa da, bir küme desteklenmeyen değişiklik yapabilirsiniz çünkü bunun yapılması önerilmez.

## <a name="network-ports-access-and-nsgs"></a>Nsg'leri ağ bağlantı noktaları ve erişim

Yönetilen bir hizmet olarak AKS belirli ağ ve bağlantı gereksinimleri vardır. Bu gereksinimleri normal Iaas bileşenleri için daha az esnek gereksinimleridir. Aks'deki operations NSG kurallarını, belirli bir bağlantı noktası (örneğin, giden bağlantı noktası 443 block güvenlik duvarı kurallarını kullanarak) engelleme özelleştirme gibi ve URL'leri beyaz listeye ekleme kümenizi desteklenmeyen yapabilirsiniz.

> [!NOTE]
> AKS şu anda çıkış kümenizden (örneğin, açık bir etki alanı veya bağlantı noktası beyaz listeye ekleme) tamamen kilitlemenize izin vermez. URL ve bağlantı noktaları listesinde uyarmadan belgesidir. Bir Azure destek bileti oluşturarak, güncelleştirilmiş listesini alabilirsiniz. Yalnızca küme kullanılabilirliklerini etkilenebilir kabul ediyorsunuz müşterilerin listesidir *dilediğiniz zaman.*

## <a name="unsupported-alpha-and-beta-kubernetes-features"></a>Desteklenmeyen alfa ve beta Kubernetes özellikleri

AKS, Yukarı Akış Kubernetes projesi içinde yalnızca kararlı özelliklerini destekler. AKS, aksi takdirde, Yukarı Akış Kubernetes projede mevcut olan alfa ve beta özelliklerini desteklemez.

Genel kullanıma sunulan gelmeden önce iki senaryoda alfa veya beta özelliği piyasaya sunuluyor:

* Müşteriler, AKS ürün, destek ya da mühendislik ekipleri ile karşıladığınızdan ve bu yeni özellikleri denemek için istendi.
* Bu özellikler silinmiş [özellik bayrağı tarafından etkin](https://github.com/Azure/AKS/blob/master/previews.md). Müşteriler açıkça bu özellikleri kullanan kabul etmek gerekir.

## <a name="preview-features-or-feature-flags"></a>Önizleme özellikleri veya özellik bayrakları

Microsoft, özellikler ve genişletilmiş test edilmesini gerektirir işlevselliği ve kullanıcı geri bildirimi için yeni önizleme özellikleri veya özellikleri bir özellik bayrağının arkasında yayımlar. Bu özelliklerin ön sürümü veya beta özelliği göz önünde bulundurun.

Önizleme veya özellik bayrağını özellikleri üretim için kuruluşunuz değildir. API'ler ve davranışı, hata düzeltmeleri ve diğer değişiklikler süregiden değişiklikler, kararsız kümeleri ve kapalı kalma süresi neden olabilir.

Bu özellikler Önizleme aşamasındadır değil üretim için geliyordu ve yalnızca çalışma saatleri sırasında AKS teknik destek ekipleri tarafından desteklenen genel önizlemeye sunuldu 'en yüksek çaba' destek kapsamında fall özellikleridir. Lütfen ek bilgi için bkz:

* [Azure desteği SSS](https://azure.microsoft.com/support/faq/)

> [!NOTE]
> Önizleme özellikleri Azure göre etkili *abonelik* düzeyi. Önizleme özellikleri, bir üretim abonelikte yüklemeyin. Bir üretim abonelikte Önizleme özellikleri varsayılan API davranışı değiştirmek ve düzenli işlemleri etkiler.

## <a name="upstream-bugs-and-issues"></a>Yukarı Akış hatalar ve sorunlar

Geliştirme hızını Yukarı Akış Kubernetes projeye göz önünde bulundurulduğunda, hataları neredeyse şaşmaz biçimde ortaya çıkar. Bu hataların bazıları yama veya olamaz AKS sisteminde geçici olarak çalıştı. Bunun yerine, hata düzeltmeleri, Yukarı Akış projeleri (örneğin, Kubernetes, düğüm veya alt işletim sistemleri ve çekirdekler) daha büyük düzeltme eklerini gerektirir. Microsoft (Azure bulut sağlayıcısı gibi) sahip olduğu bileşenler, AKS ve Azure personel topluluğa Yukarı Akış sorunlarını düzeltmek için kabul edilir.

Bir teknik destek sorununu göre bir veya daha fazla Yukarı Akış hatalar kök-neden olduğunda, AKS desteği ve mühendislik ekipleri olur:

* Tanımlamak ve bu sorun, küme veya iş yüküne neden etkiler anlaşılmasına yardımcı olacak destekleyici biriktiğini Yukarı Akış hatalarla bağlayabilirsiniz. Sorunları izlemek ve ne zaman yeni bir yayın düzeltmeleri sağlayacak görmek için müşterileri gerekli depoları bağlantılarını alır.
* Olası geçici çözümleri veya risk azaltma işlemleri sağlar. Sorun azaltılabilir, bir [bilinen sorun](https://github.com/Azure/AKS/issues?q=is%3Aissue+is%3Aopen+label%3Aknown-issue) AKS depoda dosyalanır. Bilinen sorun dosyalama açıklanmaktadır:
  * Yukarı Akış hataları için bağlantılar dahil olmak üzere sorunu.
  * Geçici çözüm hakkında ayrıntıları ve yükseltmesi veya başka bir çözümün Kalıcılık.
  * Zaman çizelgelerini çıkışın eklenmek üzere, Yukarı Akış yayın temposu üzerinde temel alan kaba.
