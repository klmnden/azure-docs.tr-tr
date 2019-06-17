---
title: Azure Container Instances güvenlik konuları
description: Azure Container Instances ve herhangi bir kapsayıcı platformu için genel güvenlik konuları için görüntüler ve gizli dizileri güvenli hale getirmek için öneriler
services: container-instances
author: dlepow
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 04/29/2019
ms.author: danlep
ms.custom: ''
ms.openlocfilehash: dc516277d79e37500e73e1aee6b88c908acb9b1c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64944004"
---
# <a name="security-considerations-for-azure-container-instances"></a>Azure Container Instances için güvenlik konuları

Bu makale, Azure Container Instances'a kapsayıcı uygulamalarını çalıştıran kullanımıyla ilgili güvenlik konuları tanıtır. Konu başlıkları şunlardır:

> [!div class="checklist"]
> * **Güvenlik önerilerini** Azure Container Instances için görüntüler ve gizli anahtarları yönetme
> * **Kapsayıcı ekosistemi için Değerlendirmeler** herhangi bir kapsayıcı platformu için kapsayıcı yaşam döngüsü boyunca

## <a name="security-recommendations-for-azure-container-instances"></a>Azure Container Instances için güvenlik önerileri

### <a name="use-a-private-registry"></a>Özel bir kayıt kullanın

Kapsayıcı, bir veya daha fazla depoda depolanan görüntülerden oluşturulur. Bu depolar genel bir kayıt defteri gibi ait olabilir [Docker Hub](https://hub.docker.com), veya bir özel kayıt defterine. Özel bir kayıt defteri örneği olarak şirket içinde veya sanal özel bulutta kullanılabilen [Docker Trusted Registry](https://docs.docker.com/datacenter/dtr/2.0/) gösterilebilir. Bulut tabanlı özel kapsayıcı kayıt defteri Hizmetleri dahil olmak üzere, ayrıca kullanabileceğiniz [Azure Container Registry](../container-registry/container-registry-intro.md). 

Genel kullanıma açık kapsayıcı görüntülerinde de güvenlik garanti değil. Kapsayıcı görüntüleri birden çok yazılım katmanından oluşur ve her yazılım katmanında güvenlik açıkları olabilir. Tehdit saldırıları azaltmak için depolama ve Azure Container Registry veya Docker Trusted Registry gibi özel bir kayıt defterinden görüntüleri alma. Yönetilen özel kayıt defteri sağlamanın yanı sıra Azure Container Registry destekler [hizmet sorumlusu tabanlı kimlik doğrulaması](../container-registry/container-registry-authentication.md) temel kimlik doğrulaması akışları için Azure Active Directory aracılığıyla. Bu kimlik doğrulaması (çekin) salt okunur ve yazma (itmeden) sahip izinleri için rol tabanlı erişim içerir.

### <a name="monitor-and-scan-container-images"></a>Kapsayıcı görüntülerini taramak ve izleme

Güvenlik İzleme ve tarama çözümleri [Twistlock](https://azuremarketplace.microsoft.com/marketplace/apps/twistlock.twistlock?tab=Overview) ve [Aqua Security](https://azuremarketplace.microsoft.com/marketplace/apps/aqua-security.aqua-security?tab=Overview) Azure Marketi aracılığıyla edinilebilir. Özel bir kayıt defterine kapsayıcı görüntülerini taramak ve olası güvenlik açıklarını tanımlamak için kullanabilirsiniz. Farklı çözümlerin sağladığı tarama derinliğini anlamak önemlidir. 

### <a name="protect-credentials"></a>Kimlik bilgilerini koruyun

Kapsayıcılar, çeşitli kümeleri ve Azure bölgeleri arasında yayılabilir. Bu nedenle, oturumlar veya parolalar veya belirteçleri gibi API erişimi için gerekli kimlik bilgileri güvenlik altına almanız gerekir. Yalnızca ayrıcalıklı kullanıcıların kapsayıcıların aktarımda ve bekleme sırasında erişebildiğinden emin olun. Tüm kimlik bilgisi gizli dizileri stok ve ardından kapsayıcı platformları için tasarlanan yeni çıkan gizli dizileri yönetim araçlarını kullanmak, geliştiriciler gerektirir.  Çözümünüzü şifreli veritabanlarına, aktarım sırasında ve en az ayrıcalıklı gizli veriler için TLS şifrelemesi içerdiğinden emin olun [rol tabanlı erişim denetimi](../role-based-access-control/overview.md). [Azure Key Vault](../key-vault/key-vault-secure-your-key-vault.md) kapsayıcılı uygulamalar için şifreleme anahtarları ve gizli anahtarları (örneğin, sertifikalar, bağlantı dizeleri ve parolalar gibi) koruyan bir bulut hizmeti olduğunu. Bu veriler hassas ve iş açısından kritik olduğundan, yalnızca yetkili uygulamaların ve kullanıcıların bunlara erişebilmesi için güvenli erişim anahtarınızı kasaları.

## <a name="considerations-for-the-container-ecosystem"></a>Kapsayıcı ekosistemi için dikkat edilmesi gerekenler

Etkili bir şekilde, yönetilen ve iyi uygulanan aşağıdaki güvenlik önlemlerini, güvenli ve kapsayıcı ekosisteminizi korumanıza yardımcı olabilir. Bu ölçümler, Üretim dağıtımı ve bir dizi kapsayıcı düzenleyicileri, konaklar ve platformlar için geliştirme kapsayıcı yaşam döngüsü boyunca geçerlidir. 

### <a name="use-vulnerability-management-as-part-of-your-container-development-lifecycle"></a>Kapsayıcı geliştirme yaşam döngünüzü bir parçası olarak güvenlik açığı Yönetimi'ni kullanın 

Kapsayıcı geliştirme yaşam döngüsü boyunca etkin güvenlik açığı Yönetimi'ni kullanarak, tanımlamak ve daha ciddi bir sorun haline gelmeden önce güvenlik sorunları gidermek ekledikçe geliştirin. 

### <a name="scan-for-vulnerabilities"></a>Güvenlik açıkları için tarayın 

Yeni güvenlik açıkları, sürekli bir işlem için tarama ve güvenlik açıklarını belirlemeye, bu nedenle her zaman keşfedildiğinde uyarır. Güvenlik Açığı taraması kapsayıcı yaşam döngüsü boyunca dahil:

* Geliştirme ardışık düzeninizde son onay, görüntüleri bir genel veya özel kayıt defterine göndermeden önce bir güvenlik açığı taraması kapsayıcılarında gerçekleştirmeniz gerekir. 
* Geliştirme sırasında şekilde kaçırılan izliyorsanız tanımlamak ve kapsayıcı görüntüleri kullanılan kod içinde mevcut olabilecek yeni bulunan tüm güvenlik açıklarını gidermek için kayıt defterindeki kapsayıcı görüntülerini taramak devam edin.  

### <a name="map-image-vulnerabilities-to-running-containers"></a>Çalışan kapsayıcılar görüntü güvenlik açıklarından eşleme 

Eşleme güvenlik açıklarını güvenlik sorunlarının azalmasına veya çözümlenen için çalışan kapsayıcılar, kapsayıcı görüntüleri tanımlanan bir çeşit olması gerekir.  

### <a name="ensure-that-only-approved-images-are-used-in-your-environment"></a>Yalnızca onaylanan görüntüleri ortamınızda kullanıldığından emin olun 

Yeterli değişiklik ve bilinmeyen kapsayıcılar da izin olmadan bir kapsayıcı ekosisteminde geçiciliğine yoktur. Yalnızca onaylanan kapsayıcı görüntüleri sağlar. Araçları ve işlemleri için izleme ve onaylanmamış kapsayıcı görüntülerini kullanımını önlemek için yerinde vardır. 

Saldırı yüzeyini azaltma ve kritik güvenlik hataları yapmasını geliştiriciler engelleme etkili bir geliştirme ortamınıza kapsayıcı görüntülerini akışını denetlemek için yoludur. Örneğin, bir temel görüntü, tercihen yalın olarak tek bir Linux dağıtımı tasdik (Alpine CoreOS yerine veya Ubuntu) için olası saldırılara karşı yüzeyini en aza indirmek için. 

İmzalama veya izinden görüntüsü kapsayıcılar bütünlüğünü sağlayan bir gözetim zinciri sağlayabilir. Örneğin, Azure Container Registry, Docker'ın destekler [içerik güven](https://docs.docker.com/engine/security/trust/content_trust) modeli, bir kayıt defterine gönderilmesini görüntüleri imzalamak görüntü yayımcılarını sağlayan ve yalnızca imzalı görüntüleri çekmek için görüntü tüketiciler.

### <a name="permit-only-approved-registries"></a>Onaylanan kayıt defterleri yalnızca izin ver 

Ortamınızın yalnızca onayladığı yansımaları kullandığı sağlama yalnızca onaylanan kapsayıcı kayıt defterleri kullanımına izin vermek için uzantısıdır. Onaylanan kapsayıcı kayıt defterleri kullanılmasını gerektirmek, olası güvenlik sorunlarını veya bilinmeyen güvenlik açıklarını giriş için sınırlandırarak riski maruz kalma riskinizi azaltır. 

### <a name="ensure-the-integrity-of-images-throughout-the-lifecycle"></a>Yaşam döngüsü boyunca görüntüleri bütünlüğünü 

Kapsayıcı yaşam döngüsü boyunca güvenlik yönetme parçası olan kayıt defterindeki kapsayıcı görüntülerini bütünlüğünü sağlamak ve değiştirilemez veya üretim ortamına dağıtmış gibi. 

* Görüntüleri güvenlik açıkları, küçük bile olsa, bir üretim ortamında çalıştırılmasına izin verilmeyen. İdeal olarak, üretimde dağıtılan tüm görüntüler, özel bir kayıt defterine birkaç erişilebilir kaydedilmelidir. Üretim görüntülerinin sayısını, bunların etkili bir şekilde yönetilebilir emin olmak için küçük tutun.

* Genel kullanıma açık kapsayıcı görüntüden yazılımların kaynak saptamak zor olduğundan, katman kaynağının bilgi sağlamak için kaynaktan görüntüleri oluşturun. Kendi oluşturdukları bir kapsayıcı görüntüsünde bir güvenlik açığı olduğunda müşteriler çözüme daha hızlı ulaşabilir. Genel bir görüntü ile müşteriler düzeltmesi veya yayımcıdan başka bir güvenli görüntü almak için genel görüntünün kökünü bulur gerekecektir. 

* Üretimde dağıtılan baştan sona taranmış bir görüntüsünü uygulama ömrü boyunca güncel olması garanti edilmez. Görüntünün önceden bilinmeyen veya üretim dağıtımından sonra sunulan katmanlarında güvenlik açıkları bildirilebilir. 

  Süresi dolmuş veya bir süredir güncelleştirilmeyen görüntülerin belirlenmesi üretimde dağıtılan görüntülerin düzenli aralıklarla denetleyin. Kapalı kalma süresi olmadan kapsayıcı görüntülerini güncelleştirmek için mavi-yeşil dağıtım yöntemlerini ve sıralı yükseltme mekanizmalarını kullanabilirsiniz. Önceki bölümde açıklanan araçları kullanarak görüntüleri tarayabilirsiniz. 

* Tümleşik güvenlik güvenli görüntülerinizi oluşturmak ve bunları özel kayıt defterinize itme için tarama ile sürekli tümleştirme (CI) işlem hattı kullanın. CI çözümüne yerleşik güvenlik açığı taraması, tüm sınamaları geçen görüntülerin üretim iş yüklerinin dağıtıldığı özel kayıt defterine gönderilmesini sağlar. 

  CI işlem hattı hatası, güvenlik açığı olan görüntülerin üretim iş yükü dağıtımları için kullanılan özel kayıt defterine gönderildi değil sağlar. Ayrıca, görüntü güvenliği çok sayıda görüntü varsa taramasını otomatikleştirir. Aksi durumda, görüntüleri güvenlik açıkları için el ile denetlemek çok zor olabileceği gibi hata riskini de artırır. 

### <a name="enforce-least-privileges-in-runtime"></a>Çalışma zamanı en düşük ayrıcalık zorla 

En düşük ayrıcalık kavramını da kapsayıcıları için geçerli bir temel güvenlik en iyi uygulamadır. Bir güvenlik açığından yararlanan, genellikle saldırganın erişim verir ve bu riskli uygulama veya işlemin ayrıcalıkları eşit. En düşük ayrıcalıklarla kapsayıcıları çalıştırmak ve işin tamamlanması için gerekli erişimle risk maruz kalma riskinizi azaltır sağlama. 

### <a name="reduce-the-container-attack-surface-by-removing-unneeded-privileges"></a>Gereksiz ayrıcalıkları kaldırarak kapsayıcı saldırı yüzeyini azaltma 

Herhangi bir kullanılmayan veya gereksiz işlemleri veya ayrıcalıkları kapsayıcı çalışma zamanını şuradan kaldırarak Ayrıca olası saldırı yüzeyini en aza indirebilirsiniz. Ayrıcalıklı kapsayıcıları kök olarak çalıştırın. Kötü niyetli bir kullanıcı ya da iş yükü ayrıcalıklı bir kapsayıcıda çıkışları ise kapsayıcıyı kök olarak söz konusu sistemdeki ardından çalıştırın.

### <a name="whitelist-files-and-executables-that-the-container-is-allowed-to-access-or-run"></a>Beyaz liste dosyaları ve kapsayıcı erişmek veya çalıştırmak için izin verilen yürütülebilir dosyalar 

Değişkenler veya bilinmeyenli iki denklemi sayısının azaltılması, tutarlı ve güvenilir bir ortam sağlamak yardımcı olur. Erişebilmeleri kapsayıcılar veya yalnızca tüm farklı çalıştır veya izin verilenler listesinde dosyaları ve yürütülebilir dosyalar sınırlama, riskini Etkilenme sınırlama, kendini kanıtlamış bir yöntemdir.  

En baştan uygulandığında bir beyaz liste yönetmek çok kolaydır. Dosyaları ve yürütülebilir dosyaları uygulamanın düzgün çalışması için gerekli olan bilgi edindiğiniz gibi bir beyaz liste denetimi ve yönetilebilirlik ölçüsü sağlar. 

Bir beyaz liste yalnızca saldırı yüzeyini azaltan ancak ayrıca anormallikleri için temel sağlayan ve kapsayıcı kırılımı senaryoları ve "gürültülü komşu" Kullanım örneklerini engelleyebilirsiniz. 

### <a name="enforce-network-segmentation-on-running-containers"></a>Kapsayıcıları çalıştırma ağ kesimleme zorla  

Güvenlik risklerini başka bir alt ağdaki bir alt ağ kapsayıcılarda korunmasına yardımcı olmak için ağ kesimleme (veya nano segmentasyona) sürdürmek ya da çalışan kapsayıcılar arasındaki ayrımı. Bakımı ağ kesimleme de uyumluluk mandates karşılamak için gerekli olan sektörün kapsayıcılar kullanmak için gerekli olabilir.  

Örneğin, iş ortağı aracı [Açık Deniz Mavisi](https://azuremarketplace.microsoft.com/marketplace/apps/aqua-security.aqua-security?tab=Overview) nano kesimlemesi için otomatikleştirilmiş bir yaklaşımın sağlar. Açık Deniz Mavisi, çalışma zamanında kapsayıcı ağ etkinliklerini izler. Bu, tüm gelen ve giden ağ bağlantıları buradan diğer kapsayıcılar, hizmetleri, IP adresleri ve genel internet tanımlar. Nano segmentasyona izlenen trafiğe göre otomatik olarak oluşturulur. 

### <a name="monitor-container-activity-and-user-access"></a>Kapsayıcı etkinliği ve kullanıcı erişimi izleme 

Tüm BT ortamında olduğu gibi ile tutarlı bir şekilde herhangi bir şüpheli veya kötü amaçlı etkinliği hızlı bir şekilde tanımlamak için kapsayıcı ekosisteminizi etkinliği ve kullanıcı erişimini izlemeniz gerekir. Azure izleme çözümleri dahil olmak üzere kapsayıcı sağlar:

* [Kapsayıcılar için Azure İzleyici](../azure-monitor/insights/container-insights-overview.md) Azure Kubernetes Service (AKS) barındırılan Kubernetes ortamlara dağıtılan iş yüklerinizin performansını izlemeniz gerekir. Kapsayıcılar için Azure İzleyici denetleyicileri, düğümleri ve Kubernetes ölçümler API aracılığıyla kullanılabilir olan kapsayıcıları bellekten toplanması ve işlemci ölçümleri performans görünürlük sağlar. 

* [Azure kapsayıcı izleme çözümünü](../azure-monitor/insights/containers.md) görüntüleyin ve diğer Docker ve Windows yönetmenize yardımcı kapsayıcı konağında tek bir konumda. Örneğin:

  * Kapsayıcılar ile kullanılan komutları gösteren ayrıntılı denetim bilgileri görüntüleyin. 
  * Kapsayıcılar, Docker veya Windows konak uzaktan görüntüleme gerek kalmadan Merkezi günlük arama ve görüntüleme sorunlarını giderin.  
  * Bir konakta gürültülü ve alıcı aşırı kaynakları olabilir kapsayıcıları bulun.
  * Merkezileştirilmiş CPU, bellek, depolama ve kapsayıcılar için ağ kullanım ve performans bilgilerini görüntüleyin.  

  Çözüm, Docker Swarm, DC/OS, yönetilmeyen Kubernetes, Service Fabric ve Red Hat OpenShift kapsayıcı düzenleyicileri destekler. 

### <a name="monitor-container-resource-activity"></a>Kapsayıcı kaynak etkinliğini izleme 

Dosyalar, ağ ve kapsayıcılarınızı erişen diğer kaynaklar gibi kaynak etkinliğinizi izleyin. Kaynak etkinliği ve kullanım izleme, performans izleme ve bir güvenlik önlemi olarak yararlıdır. 

[Azure İzleyici](../azure-monitor/overview.md) ölçümler, etkinlik günlükleri ve tanılama günlükleri koleksiyonunu sağlayarak Azure Hizmetleri için çekirdek izleme sağlar. Örneğin, etkinlik günlüğü size yeni kaynakların ne zaman oluşturulduğunu veya değiştirildiğini bildirir. 

Farklı kaynaklar için ve hatta bir sanal makinenin içindeki işletim sistemi için bile performans istatistikleri sağlayan ölçümler kullanılabilir. Azure portalında gezginlerden biriyle bu verileri görüntüleyebilir ve bu ölçümlere dayalı uyarılar oluşturabilirsiniz. Azure İzleyici, hızlı ölçümler, zaman açısından kritik uyarılar ve bildirimler için kullanılması gerektiği şekilde, (1 dakika kadar 5 dakika), işlem hattı sağlar. 

### <a name="log-all-container-administrative-user-access-for-auditing"></a>Tüm kapsayıcı yönetici kullanıcı erişim denetimi için oturum açın 

Bir doğru denetim izi yönetim erişimine kapsayıcı ekosistemi, kapsayıcı kayıt defteri ve kapsayıcı görüntüleri korur. Bu günlükler, denetim amacıyla gerekli olabilir ve her türlü güvenlik olayıyla sonra adli kanıt olarak yararlı olacaktır. Kullanabileceğiniz [Azure kapsayıcı izleme çözümünü](../azure-monitor/insights/containers.md) bu amaç elde etmek için. 

## <a name="next-steps"></a>Sonraki adımlar

* Kapsayıcı çözümlerinden güvenlik açıklarıyla yönetme hakkında daha fazla bilgi [Twistlock](https://www.twistlock.com/solutions/microsoft-azure-container-security/) ve [Aqua Security](https://www.aquasec.com/solutions/azure-container-security/).

* Daha fazla bilgi edinin [azure'da kapsayıcı güvenliği](https://azure.microsoft.com/resources/container-security-in-microsoft-azure/).