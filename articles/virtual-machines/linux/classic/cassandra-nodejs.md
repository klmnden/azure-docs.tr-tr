---
title: Bir Cassandra kümesi azure'da Linux üzerinde Node.js ile çalıştırın.
description: Azure sanal Makineler'de Linux üzerinde Node.js uygulamasından bir Cassandra kümesi çalıştırmayı öğrenin
services: virtual-machines-linux
documentationcenter: nodejs
author: craigshoemaker
manager: routlaw
editor: ''
tags: azure-service-management
ms.assetid: 30de1f29-e97d-492f-ae34-41ec83488de0
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: cshoe
ms.openlocfilehash: d99c9732bb1bf494b87d2073ba002264c7a51634
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2018
ms.locfileid: "47221256"
---
# <a name="run-a-cassandra-cluster-on-linux-in-azure-with-nodejs"></a>Azure'da Node.js ile Linux üzerinde bir Cassandra kümesi çalıştırın

> [!IMPORTANT] 
> Azure'da oluşturmaya ve kaynaklarla çalışmaya yönelik iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modelini incelemektedir. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir. Resource Manager şablonları için [Datastax Enterprise](https://azure.microsoft.com/documentation/templates/datastax) ve [CentOS üzerinde Küme ve Cassandra Spark](https://azure.microsoft.com/documentation/templates/spark-and-cassandra-on-centos/).

## <a name="overview"></a>Genel Bakış
Microsoft Azure, hem Microsoft hem de işletim sistemleri, uygulama sunucuları, Mesajlaşma bir ara yazılım yanı sıra SQL ve NoSQL veritabanlarından hem ticari hem de açık kaynak modelleri içeren Microsoft olmayan yazılımları çalıştıran bir açık bir bulut platformudur. Azure dahil olmak üzere genel bulutlarda dayanıklı hizmetler derleme süreci dikkatlice planlanmalı ve bilinçli mimari iyi depolama katmanları olarak hem uygulama sunucuları için gerektirir. Cassandra'nın dağıtılmış depolama mimarisi doğal olarak hataya dayanıklı küme hataları için yüksek oranda kullanılabilir sistemlerini oluşturmaya yardımcı olur. Cassandra bulut ölçeğinde NoSQL veritabanı cassandra.apache.org Apache Software Foundation tarafından tutulan ' dir. Cassandra Java dilinde yazılır. Bu nedenle hem de Windows ve Linux platformlarında çalışır.

Bu makalenin odak noktası, Azure sanal Makineler'i ve sanal ağları kullanan bir tek ve çoklu veri merkezi kümesi Ubuntu'da Cassandra dağıtımı göstermektir. Küme dağıtımı için iyileştirilmiş üretim iş yükleri için çok disk düğüm yapılandırması, uygun halka topolojisi tasarımı ve gerekli çoğaltma, veri tutarlılığı, aktarım hızı ve yüksek desteklemek için modelleme verileri gerektirir, bu makalenin kapsamı dışında aynıdır Kullanılabilirlik gereksinimleri.

Göstermek için temel bir yaklaşım Cassandra kümesi oluşturma ile ilgili bu makale alır karşılaştırıldığında, Docker, Chef veya Puppet, altyapı dağıtımı çok daha kolay hale getirebilirsiniz.  

## <a name="the-deployment-models"></a>Dağıtım modelleri
Microsoft Azure ağ erişimi ayrıntılı ağ güvenliği elde etmek için sınırlı yalıtılmış özel kümeleri dağıtımını sağlar.  Bu makalede temel düzeyde Cassandra dağıtımı gösteren ilgili olduğundan, tutarlılık düzeyi ve aktarım hızı için en uygun depolama tasarımı üzerinde odaklanın değil. Aşağıda, kuramsal bir küme için ağ gereksinimleri listesi verilmiştir:

* Dış sistemler Cassandra veritabanı içinde veya Azure dışına erişemiyor
* Cassandra kümesi thrift trafiği için bir yük dengeleyicinin arkasına olması gerekir
* Her veri merkezinde bir Gelişmiş küme kullanılabilirlik için iki gruplardaki düğümleri Cassandra dağıtma
* Uygulama sunucu grubu doğrudan veritabanına erişime sahip yalnızca kümeyi bu nedenle, kilitleme
* SSH dışında hiçbir ortak ağ uç noktası
* Cassandra Çekirdekte sabit bir iç IP adresi gerekiyor

Cassandra, tek bir Azure bölgesinde veya birden çok bölgeye temel iş yükü dağıtılmış yapısını üzerinde dağıtılabilir. Son kullanıcılarınıza yaklaştırarak belirli bir coğrafyaya aynı Cassandra altyapısı aracılığıyla sunmak için çok bölgeli dağıtım modelini kullanabilirsiniz. Cassandra'nın yerleşik düğümü çoğaltma alır özen çok yöneticili eşitleme birden çok veri merkezlerinden kaynaklanan yazar ve uygulamaları verileri tutarlı bir görünüm sunar. Çok bölgeli dağıtımlar daha geniş bir Azure hizmet kesintisi ile risk azaltma da yardımcı olabilir. Cassandra'nın ayarlanabilir tutarlılık ve çoğaltma topolojisi, uygulamaların farklı RPO gereksinimlerini karşılama konusunda yardımcı olur.

### <a name="single-region-deployment"></a>Tek bölge dağıtımı
Şimdi tek bölge dağıtımı ile başlayın ve çok bölgeli modeli oluşturma dersleri toplar. Azure sanal ağı, böylece yukarıda belirtilen ağ güvenliği gereksinimleri karşılanmadığı yalıtılmış alt ağlar oluşturmak için kullanılır.  Ubuntu 14.04 LTS ve Cassandra 2.08 tek bölge dağıtımı oluştururken açıklanan işlemi kullanır. Ancak, işlem diğer Linux çeşitleri kolayca önemsenmeksizin devralınabilir. Tek bölge dağıtımı sistemle ilgili özellikleri bazıları aşağıda verilmiştir.  

**Yüksek Kullanılabilirlik:** böylece yüksek kullanılabilirlik için birden çok hata etki alanları arasında düğümlere yayılır mı Şekil 1'de gösterilen Cassandra düğümleri için iki kullanılabilirlik kümesi dağıtılır. Her kullanılabilirlik kümesi ile açıklanan Vm'leri 2 hata etki alanı için eşlenmiş. Azure hata etki alanı kavramını süresini (örneğin, donanım veya yazılım hatası) planlanmamış yönetmek için kullanır. Yükseltme etki alanı (örneğin, konak veya konuk işletim sistemi düzeltme eki uygulama/yükseltme işlemleri, uygulama yükseltmeleri) kavramı, zamanlanan saati yönetmek için kullanılır. Lütfen [Azure uygulamaları için yüksek kullanılabilirlik ve olağanüstü durum kurtarma](http://msdn.microsoft.com/library/dn251004.aspx) rolünü yüksek kullanılabilirlik'ı modemle hızlı bağlantılar sağlama, hata ve yükseltme etki alanları için.

![Tek bölge dağıtımı](./media/cassandra-nodejs/cassandra-linux1.png)

Şekil 1: Tek bölge dağıtımı

Not; Bu makalenin yazıldığı sırada, Azure Vm'leri belirli hata etki alanı için bir grup açık eşleme izin vermez Sonuç olarak, Şekil 1'de gösterilen bile dağıtım modeliyle, istatistiksel olarak olası tüm sanal makinelerin iki hata etki alanlarına yerine dört eşlenebilir.

**Yük Dengeleme Thrift trafiğini:** Thrift istemci kitaplıkları web sunucu içindeki iç yük dengeleyici aracılığıyla bağlanır. Bu "veri" alt ağına iç yük dengeleyici ekleme işlemini gerektirir (Şekil 1 bakın) Cassandra kümesi barındıran bulut hizmetinin bağlamında. İç load balancer tanımlandıktan sonra her düğüm ile önceden tanımlanmış bir yük dengeleyici adı ile bir yük dengeli küme, ek açıklamalar eklenmesi için yük dengeli uç nokta gerektirir. Bkz: [Azure iç Yük Dengeleme ](../../../load-balancer/load-balancer-internal-overview.md)daha fazla ayrıntı için.

**Küme çekirdekler:** yeni düğümler ile çekirdek düğümleri küme topolojisini bulmak için olarak çekirdekler için en yüksek oranda kullanılabilir düğümleri seçmek önemlidir. Her kullanılabilirlik kümesinden bir düğümü tek hata noktasını önlemek için çekirdek düğümleri atanır.

**Çoğaltma faktörü ve tutarlılık düzeyi:** Cassandra'nın yerleşik yüksek kullanılabilirlik ve veri dayanıklılığı nitelenen çoğaltma faktörü (RF - kopya kümesinde depolanan her bir satır sayısı) ve tutarlılık düzeyi (için yineleme sayısı sonucu çağırana döndürülmeden önce okunan/yazılan olabilir). CRUD sorgu verme sırasında tutarlılık düzeyi belirtildi ancak çoğaltma faktörü (bir ilişkisel veritabanına benzer) anahtar alanı oluşturma sırasında belirtilir. Cassandra belgelerine bakın [tutarlılık için yapılandırma](https://docs.datastax.com/en/cassandra/3.0/cassandra/dml/dmlConfigConsistency.html) tutarlılık Ayrıntılar ve çekirdek hesaplama formülü.

Cassandra veri bütünlüğü modelleri – tutarlılık ve nihai tutarlılık iki türünü destekler; Tutarlılık düzeyi ve çoğaltma faktörü birlikte tam ya da sonunda tutarlı bir yazma işlemi olarak verileri tutarlı olup olmadığını belirler. Örneğin, çekirdek belirten tutarlılık düzeyi veri tutarlılığı çekirdek (örneğin bir) elde etmek için gerektiği şekilde yazılacak yineleme sayısı aşağıdaki herhangi bir tutarlılık düzeyi bağlanırken her zaman sağlanır gibi sonunda tutarlı olan verileri sonuçlanır.

Yukarıda, çoğaltma faktörü 3 ve çekirdek ile 8 düğüm kümesi (2 düğüm yazılamaz veya okunamaz tutarlılık) okuma/yazma tutarlılık düzeyi, uygulama başlangıç bildirimde bulunmadan önce çoğaltma grubu başına en fazla 1 düğüm teorik kaybı hayatta kalamaz hata oluştu. Bu, tüm anahtar alanları da dengeli okuma/istekleri yazma olduğunu varsayar.  Dağıtılan bir küme için kullanılan parametreler şunlardır:

Aynı bölgede Cassandra kümesi yapılandırması:

| Parametre kümesi | Değer | Açıklamalar |
| --- | --- | --- |
| Düğüm (N) |8 |Kümedeki düğümlerin toplam sayısı |
| Çoğaltma faktörü (RF) |3 |Belirli bir satırın çoğaltmaların sayısı |
| Tutarlılık düzeyi (yazma) |QUORUM[(RF/2) +1) = 2] formülün sonucu yuvarlanır |En fazla 2 çoğaltma, yanıtını çağırana gönderilmeden önce yazar; 3 çoğaltma sonunda tutarlı bir şekilde yazılır. |
| Tutarlılık düzeyi (okuma) |Çekirdek [(RF/2) + 1 = 2] formülün sonucu yuvarlanır |2 çoğaltma, yanıtını çağırana göndermeden önce okur. |
| Çoğaltma stratejisi |NetworkTopologyStrategy bakın [veri çoğaltma](https://docs.datastax.com/en/cassandra/3.0/cassandra/architecture/archDataDistributeAbout.html) Cassandra belgelerinde daha fazla bilgi için |Dağıtım topolojisi anlar ve çoğaltmaları, düğümlerde yerleştirir, böylece tüm çoğaltmalar aynı rafa bitirme |
| Snitch |GossipingPropertyFileSnitch bakın [anahtarları](https://docs.datastax.com/en/cassandra/3.0/cassandra/architecture/archSnitchesAbout.html) Cassandra belgelerinde daha fazla bilgi için |NetworkTopologyStrategy snitch kavramı topoloji anlamak için kullanır. GossipingPropertyFileSnitch her düğüm için veri merkezi ve raf eşlemesi daha iyi kontrol sağlar. Küme, ardından bu bilgileri yaymak için dedikodu kullanır. Bu dinamik IP ayarı PropertyFileSnitch göre çok daha kolay olacaktır |

**Cassandra kümesi için Azure konuları:** Microsoft Azure sanal makineleri özelliğine disk kalıcılığı; Azure Blob Depolama kullanır Azure depolama, yüksek bir dayanıklılık düzeyine ulaşmak için her bir disk üç kopyaya kaydeder. Her bir Cassandra tablosuna veri satırının zaten üç yinelemede depolanmış anlamına gelir. Bu nedenle veri tutarlılığı zaten (RF) çoğaltma faktörü 1 olsa bile dikkate. Çoğaltma faktörü 1 olması ile ilgili temel sorun, tek bir Cassandra düğüm başarısız olsa bile uygulama kapalı kalma süresi deneyimleri ' dir. Ancak, bir düğüm Azure yapı denetleyicisi tarafından tanınan sorunları (örneğin, donanım, sistem yazılım hataları) çalışmıyorsa, aynı depolama sürücülerini kullanarak onun yerine yeni bir düğüm sağlar. Eskisini değiştirmek için yeni bir düğüm sağlama birkaç dakika sürebilir.  Benzer şekilde planlı bakım etkinlikleri konuk işletim sistemi değişiklikleri gibi Cassandra yükseltir ve uygulama değişiklikleri Azure yapı denetleyicisi kümedeki düğümler çalışırken gerçekleştirir.  Toplu yükseltmeler da birkaç düğüm aynı anda sürebilir ve bu küme kısa kapalı kalma süresi birkaç bölümleri için bu nedenle karşılaşabilirsiniz. Ancak, yerleşik Azure depolama yedekliliği nedeniyle veriler kaybolmaz.  

Yüksek kullanılabilirlik gerektirmeyen Azure'a dağıtılan sistemler için (örneğin yaklaşık % 99,9 olan 8.76 SA/yıl için eşdeğer; bkz [yüksek kullanılabilirlik](http://en.wikipedia.org/wiki/High_availability) Ayrıntılar için) ile RF çalıştırmak mümkün olabilir = 1 ve tutarlılık düzeyi bir =.  RF yüksek kullanılabilirlik gereksinimleri olan uygulamalar için 3 ve tutarlılık düzeyi = = çekirdek göstereceği düğümlerinden biri çoğaltmaları aşağı saati. RF = 1 geleneksel dağıtımlarında (örneğin şirket içi), disk hataları gibi sorunlardan kaynaklanan olası veri kaybı nedeniyle kullanılamaz.   

## <a name="multi-region-deployment"></a>Çok bölgeli dağıtım
Yukarıda açıklanan Cassandra'nın veri merkezi-kullanan çoğaltma ve tutarlılık modeli, tüm dış araçları gerek kalmadan çok bölgeli dağıtım ile yardımcı olur. Burada çok yöneticili yazma işlemleri için veritabanı yansıtma için Kurulum karmaşık olabilir geleneksel ilişkisel veritabanlarından farklı budur. Çok bölgeli kurulumunda Cassandra senaryolarda da dahil olmak üzere kullanım senaryoları ile yardımcı olabilir:

**Yakınlık tabanlı dağıtım:** Kiracı Kullanıcı Temizle eşleme ile çok kiracılı uygulamaları-için-bölge, çok bölgeli küme tarafından düşük gecikme süreleriyle benefited. Örneğin, bir öğrenme yönetim sistemleri için eğitim kurumları, Doğu ABD ve Batı ABD bölgeleri için ilgili kampüsler sunmak için Dağıtılmış bir kümede dağıtabilirsiniz analytics yanı sıra işlem. Verileri zaman okuma ve yazma işlemleri sırasında yerel olarak tutarlı olabilir ve her iki bölgede sonunda tutarlı olabilir. Ortam dağıtım, e-ticaret ve herhangi bir şey gibi diğer örnekleri vardır ve coğrafi yoğunlaşmıştır kullanıcı temel hizmet her şeyi, bu dağıtım modeli için iyi bir kullanım şeklidir.

**Yüksek Kullanılabilirlik:** Yedeklilik yazılım ve donanım yüksek kullanılabilirliğini'ı modemle hızlı bağlantılar sağlama önemli bir etken; güvenilir bulut sistemleri oluşturma, Microsoft Azure üzerinde ayrıntılı bilgi için bkz. Microsoft Azure üzerinde true yedeklilik elde yalnızca güvenilir bir çok bölgeli küme dağıtarak yoludur. Uygulamaları bir aktif-aktif veya Aktif-Pasif modunda dağıtılabilir ve bölgelerinden birini kapalı ise, Azure Traffic Manager için etkin bölgeyi trafiği yönlendirebilirsiniz.  Kullanılabilirlik 99,9, ise tek bölge dağıtımı ile bir kullanılabilirlik kümesinin 99,9999 formül tarafından hesaplanan iki bölgeli bir dağıtım elde edebilirsiniz: (1-(1-0.999) * (1-0.999)) * 100); Ayrıntılar için yukarıdaki incelemeye bakın.

**Olağanüstü durum kurtarma:** çok bölgeli Cassandra kümesi düzgün bir şekilde tasarlanmış, dayanacak geri dönülemez veri merkezi kesintilerine. Tek bir bölge kapalı ise, son kullanıcıya hizmet veren diğer bölgelere dağıtılan uygulamayı başlatabilirsiniz. Diğer iş sürekliliği belirtilmesinden gibi uygulama verileri zaman uyumsuz işlem hattındaki kaynaklanan bazı verilerin kaybolması için dayanıklı olması gerekir. Ancak, Cassandra kurtarma geleneksel veritabanı kurtarma işlemleri tarafından geçen süre çok swifter yapar. Şekil 2, her bölgede sekiz düğümleri tipik çok bölgeli dağıtım modeliyle gösterir. Her iki bölgeleri yansıtma görüntülerini Simetri aynı için olan; gerçek dünya tasarımları, iş yükü türü (örneğin işlem veya analiz), RPO, RTO, veri tutarlılığı ve kullanılabilirlik gereksinimlerine bağlıdır.

![Çok bölgeli dağıtım](./media/cassandra-nodejs/cassandra-linux2.png)

Şekil 2: Birden çok bölgede Cassandra dağıtımı

### <a name="network-integration"></a>Ağ tümleştirmesi
Kümeleri iki bölgeleri üzerinde bulunan özel ağlar için dağıtılan sanal makinelerin bir VPN tüneli aracılığıyla birbirleriyle iletişim kurar. VPN tüneli ağ dağıtım işlemi sırasında sağlanan iki yazılım ağ geçidi bağlanır. Her iki bölgeleri benzer ağ mimarisi bakımından "web" ve "verileri" alt ağlara sahip; Azure ağı, gereken sayıda alt ağı oluşturulmasına izin verir ve ağ güvenliği tarafından gerektiği şekilde ACL'leri uygulayın. Küme topolojisi tasarlarken, veri merkezi iletişim gecikmesi ve ekonomik etkisi göz önünde bulundurulması gereken trafiği ağ arası.

### <a name="data-consistency-for-multi-data-center-deployment"></a>Veri tutarlılığı için çoklu veri merkezi dağıtım
Aktarım hızı ve yüksek kullanılabilirlik kümesi topolojisi etkisi dikkat edilmesi gereken dağıtımları gerek dağıtılmış. Tutarlılık düzeyi ve RF çekirdek tüm veri merkezlerini kullanılabilirliğine bağlı olmayan böyle bir şekilde seçilmesi gerekir.
Yüksek tutarlılık gerektiren bir sistem için bir LOCAL_QUORUM tutarlılık düzeyi (için okuma ve yazma) veriler zaman uyumsuz olarak uzak bir veri merkezine çoğaltılır sırasında yerel okuma ve yazma işlemleri yerel düğümleri yerine getirildiğinden emin emin yapar.  Tablo 2 yazma daha sonra açıklandığı çok bölgeli küme için yapılandırma ayrıntılarını özetlemektedir.

**İki bölgeli Cassandra kümesi yapılandırma**

| Parametre kümesi | Değer | Açıklamalar |
| --- | --- | --- |
| Düğüm (N) |8 + 8 |Kümedeki düğümlerin toplam sayısı |
| Çoğaltma faktörü (RF) |3 |Belirli bir satırın çoğaltmaların sayısı |
| Tutarlılık düzeyi (yazma) |LOCAL_QUORUM [(sum(RF)/2) +1) = 4] formülün sonucu yuvarlanır |2 düğüm yazılmış ilk veri merkezine zaman uyumlu olarak; Çekirdek için gereken ek 2 düğüm 2 veri merkezi için zaman uyumsuz olarak yazılır. |
| Tutarlılık düzeyi (okuma) |LOCAL_QUORUM ((RF/2) + 1) = 2 formülün sonucu aşağı yuvarlanır |Okuma istekleri yalnızca bir bölgeden karşılanır; yanıtı istemciye gönderilmeden önce 2 düğüm okunur. |
| Çoğaltma stratejisi |NetworkTopologyStrategy bakın [veri çoğaltma](https://docs.datastax.com/en/cassandra/3.0/cassandra/architecture/archDataDistributeAbout.html) Cassandra belgelerinde daha fazla bilgi için |Dağıtım topolojisi anlar ve çoğaltmaları, düğümlerde yerleştirir, böylece tüm çoğaltmalar aynı rafa bitirme |
| Snitch |GossipingPropertyFileSnitch bakın [Snitches](https://docs.datastax.com/en/cassandra/3.0/cassandra/architecture/archSnitchesAbout.html) Cassandra belgelerinde daha fazla bilgi için |NetworkTopologyStrategy snitch kavramı topoloji anlamak için kullanır. GossipingPropertyFileSnitch her düğüm için veri merkezi ve raf eşlemesi daha iyi kontrol sağlar. Küme, ardından bu bilgileri yaymak için dedikodu kullanır. Bu dinamik IP ayarı PropertyFileSnitch göre çok daha kolay olacaktır |

## <a name="the-software-configuration"></a>YAZILIM YAPILANDIRMASI
Aşağıdaki yazılım sürümlerinden dağıtımı sırasında kullanılır:

<table>
<tr><th>Yazılım</th><th>Kaynak</th><th>Sürüm</th></tr>
<tr><td>JRE    </td><td>[JRE 8](http://www.oracle.com/technetwork/java/javase/downloads/server-jre8-downloads-2133154.html) </td><td>8U5</td></tr>
<tr><td>JNA    </td><td>[JNA](https://github.com/twall/jna) </td><td> 3.2.7</td></tr>
<tr><td>Cassandra</td><td>[Apache Cassandra 2.0.8](http://www.apache.org/dist/cassandra/)</td><td> 2.0.8</td></tr>
<tr><td>Ubuntu    </td><td>[Microsoft Azure](https://azure.microsoft.com/) </td><td>14.04 LTS</td></tr>
</table>

JRE indirdiklerinde, Oracle lisans el ile kabul etmeniz gerekir. Bu nedenle, dağıtımın basitleştirilmesi için gerekli olan tüm yazılımların masaüstüne indirin. Ardından, Küme dağıtımı için bir precursor olarak oluşturmak için Ubuntu şablon görüntüsü yükleyin.

Yukarıdaki yazılımın, yerel bilgisayarı iyi bilinen yükleme dizinine (örneğin, Windows üzerinde %TEMP%/downloads veya ~/Downloads birçok Linux dağıtımı veya Mac) indirin.

### <a name="create-ubuntu-vm"></a>UBUNTU SANAL MAKİNESİ OLUŞTURMA
Görüntünün birden fazla Cassandra düğümleri sağlamak için yeniden kullanılabilir, böylece işleminin bu adımında, Ubuntu görüntüsünü önkoşul yazılımlarının ile oluşturun.  

#### <a name="step-1-generate-ssh-key-pair"></a>1. adım: SSH anahtar çifti oluşturma
Azure, sağlama zamanında PEM veya DER ortak anahtarla kodlanmış x X509 gerekir. Azure'da Linux ile SSH kullanma nasıl konumunda bulunan yönergeleri kullanarak bir ortak/özel anahtar çifti oluşturun. Windows veya Linux üzerinde bir SSH istemcisi olarak putty.exe kullanmayı planlıyorsanız, PEM kodlu dönüştürmek zorunda puttygen.exe kullanarak PPK biçimine RSA özel anahtar. Bu yönergeler yukarıdaki web sayfasında bulunabilir.

#### <a name="step-2-create-ubuntu-template-vm"></a>2. adım: Ubuntu şablonu VM oluşturma
VM şablonu oluşturmak için Azure portalında oturum açın ve aşağıdaki sırayı kullanın: yeni, bilgi işlem, sanal makine, ilk GALERİ, UBUNTU, Ubuntu Server 14.04 LTS tıklayın ve ardından sağ oka tıklayın. Bir Linux VM oluşturma açıklayan bir öğretici için çalışan bir sanal makine Linux bkz.

#1 "sanal makine yapılandırma" ekranında aşağıdaki bilgileri girin:

<table>
<tr><th>ALAN ADI              </td><td>       ALAN DEĞERİ               </td><td>         AÇIKLAMALAR                </td><tr>
<tr><td>SÜRÜM YAYIN TARİHİ    </td><td> Aşağı açılan listeden bir tarih seçin</td><td></td><tr>
<tr><td>SANAL MAKİNE ADI    </td><td> CASS şablonu                   </td><td> Bu sanal makinenin adıdır </td><tr>
<tr><td>KATMANI                     </td><td> STANDART                           </td><td> Varsayılan değeri bırakın              </td><tr>
<tr><td>BOYUT                     </td><td> A1                              </td><td>G/ç gereksinimlerine göre VM; seçin. Bu amaç için varsayılan değeri bırakın. </td><tr>
<tr><td> YENİ KULLANICI ADI             </td><td> yerelyönetici                       </td><td> "Yönetici" Ubuntu 12. xx ve sonra bir ayrılmış kullanıcı adı olan</td><tr>
<tr><td> KİMLİK DOĞRULAMASI         </td><td> Onay kutusu                 </td><td>Bir SSH anahtarı ile güvenli istiyorsanız işaretleyin </td><tr>
<tr><td> SERTİFİKA             </td><td> Ortak anahtar sertifikası dosya adı </td><td> Daha önce oluşturulan ortak anahtarı kullanma</td><tr>
<tr><td> Yeni Parola    </td><td> güçlü parola </td><td> </td><tr>
<tr><td> Parolayı Onayla    </td><td> güçlü parola </td><td></td><tr>
</table>

"Sanal makine yapılandırma" #2 ekranında aşağıdaki bilgileri girin:

<table>
<tr><th>ALAN ADI             </th><th> ALAN DEĞERİ                       </th><th> AÇIKLAMALAR                                 </th></tr>
<tr><td> BULUT HİZMETİ    </td><td> Yeni bir bulut hizmeti oluştur    </td><td>Bir kapsayıcı işlem kaynaklarını sanal makineler gibi bulut hizmetidir.</td></tr>
<tr><td> BULUT HİZMETİ DNS ADI    </td><td>ubuntu-template.cloudapp.net    </td><td>Bir makine belirsiz yük dengeleyici ad verin</td></tr>
<tr><td> BÖLGE/BENZEŞİM GRUBU/SANAL AĞ </td><td>    Batı ABD    </td><td> Cassandra kümesi, web uygulamalarınızın içinden erişmek için bir bölge seçin</td></tr>
<tr><td>DEPOLAMA HESABI </td><td>    Varsayılanı kullan    </td><td>Belirli bir bölgede varsayılan depolama hesabını ya da önceden oluşturulmuş depolama hesabı kullanın</td></tr>
<tr><td>KULLANILABİLİRLİK KÜMESİ </td><td>    None </td><td>    Boş bırakın</td></tr>
<tr><td>UÇ NOKTALARI    </td><td>Varsayılanı kullan </td><td>    Varsayılan SSH yapılandırmasını kullan </td></tr>
</table>

Sağ oka tıklayın, #3 ekranda varsayılan değerleri bırakın. VM sağlama işlemini tamamlamak için "onay" düğmesine tıklayın. Birkaç dakika sonra sanal makine "ubuntu-template" adı ile bir "çalışıyor" durumda olması gerekir.

### <a name="install-the-necessary-software"></a>GEREKLİ YAZILIMI YÜKLEYİN
#### <a name="step-1-upload-tarballs"></a>1. adım: Karşıya yükleme tarballs
SCP veya pscp'ı kullanarak, daha önce indirilen yazılım komutu aşağıdaki biçimi kullanarak ~/downloads dizinine kopyalayın:

##### <a name="pscp-server-jre-8u5-linux-x64targz-localadminhk-cas-templatecloudappnethomelocaladmindownloadsserver-jre-8u5-linux-x64targz"></a>pscp server-jre-8u5-linux-x64.tar.gz localadmin@hk-cas-template.cloudapp.net:/home/localadmin/downloads/server-jre-8u5-linux-x64.tar.gz
Yukarıdaki komut da JRE Cassandra bitleri için yineleyin.

#### <a name="step-2-prepare-the-directory-structure-and-extract-the-archives"></a>2. adım: dizin yapısı hazırlamak ve arşivleri ayıklayın
VM'de oturum açın, dizin yapısını oluşturmak ve yazılım bash komut dosyası kullanarak bir süper kullanıcı olarak ayıklayın:

    #!/bin/bash
    CASS_INSTALL_DIR="/opt/cassandra"
    JRE_INSTALL_DIR="/opt/java"
    CASS_DATA_DIR="/var/lib/cassandra"
    CASS_LOG_DIR="/var/log/cassandra"
    DOWNLOADS_DIR="~/downloads"
    JRE_TARBALL="server-jre-8u5-linux-x64.tar.gz"
    CASS_TARBALL="apache-cassandra-2.0.8-bin.tar.gz"
    SVC_USER="localadmin"

    RESET_ERROR=1
    MKDIR_ERROR=2

    reset_installation ()
    {
       rm -rf $CASS_INSTALL_DIR 2> /dev/null
       rm -rf $JRE_INSTALL_DIR 2> /dev/null
       rm -rf $CASS_DATA_DIR 2> /dev/null
       rm -rf $CASS_LOG_DIR 2> /dev/null
    }
    make_dir ()
    {
       if [ -z "$1" ]
       then
          echo "make_dir: invalid directory name"
          exit $MKDIR_ERROR
       fi

       if [ -d "$1" ]
       then
          echo "make_dir: directory already exists"
          exit $MKDIR_ERROR
       fi

       mkdir $1 2>/dev/null
       if [ $? != 0 ]
       then
          echo "directory creation failed"
          exit $MKDIR_ERROR
       fi
    }

    unzip()
    {
       if [ $# == 2 ]
       then
          tar xzf $1 -C $2
       else
          echo "archive error"
       fi

    }

    if [ -n "$1" ]
    then
       SVC_USER=$1
    fi

    reset_installation
    make_dir $CASS_INSTALL_DIR
    make_dir $JRE_INSTALL_DIR
    make_dir $CASS_DATA_DIR
    make_dir $CASS_LOG_DIR

    #unzip JRE and Cassandra
    unzip $HOME/downloads/$JRE_TARBALL $JRE_INSTALL_DIR
    unzip $HOME/downloads/$CASS_TARBALL $CASS_INSTALL_DIR

    #Change the ownership to the service credentials

    chown -R $SVC_USER:$GROUP $CASS_DATA_DIR
    chown -R $SVC_USER:$GROUP $CASS_LOG_DIR
    echo "edit /etc/profile to add JRE to the PATH"
    echo "installation is complete"


Bu betik vim penceresine yapıştırın, satır başı kaldırdığınızdan emin olun ('\r ") aşağıdaki komutu kullanarak:

    tr -d '\r' <infile.sh >outfile.sh

#### <a name="step-3-edit-etcprofile"></a>3. adım: vb./profilini düzenle
Sonunda aşağıdakileri ekleyin:

    JAVA_HOME=/opt/java/jdk1.8.0_05
    CASS_HOME= /opt/cassandra/apache-cassandra-2.0.8
    PATH=$PATH:$HOME/bin:$JAVA_HOME/bin:$CASS_HOME/bin
    export JAVA_HOME
    export CASS_HOME
    export PATH

#### <a name="step-4-install-jna-for-production-systems"></a>4. adım: Yükleme JNA üretim sistemleri için
Aşağıdaki komut dizisi kullanın: aşağıdaki komutu jna 3.2.7.jar yükler ve platform jna 3.2.7.jar /usr/share.java dizin sudo apt-Get libjna java yükleyin

Cassandra başlangıç betiği bu jar dosyaları dışındaki bulabilmesi $CASS_HOME/lib dizininde simgesel bağlantılar oluştur:

    ln -s /usr/share/java/jna-3.2.7.jar $CASS_HOME/lib/jna.jar

    ln -s /usr/share/java/jna-platform-3.2.7.jar $CASS_HOME/lib/jna-platform.jar

#### <a name="step-5-configure-cassandrayaml"></a>5. adım: cassandra.yaml yapılandırma
[Bu yapılandırma, gerçek sağlama sırasında ince] tüm sanal makineler için gerekli yapılandırmayı yansıtacak şekilde her VM'de cassandra.yaml düzenleyin:

<table>
<tr><th>Alan Adı   </th><th> Değer  </th><th>    Açıklamalar </th></tr>
<tr><td>küme_adı </td><td>    "CustomerService"    </td><td> Dağıtımınızı yansıtır adı kullan</td></tr>
<tr><td>listen_address    </td><td>[boş bırakın]    </td><td> "Localhost" Sil </td></tr>
<tr><td>rpc_addres   </td><td>[boş bırakın]    </td><td> "Localhost" Sil </td></tr>
<tr><td>çekirdekler    </td><td>"10.1.2.4, 10.1.2.6, 10.1.2.8"    </td><td>Çekirdekler atanan tüm IP adresleri listesi.</td></tr>
<tr><td>endpoint_snitch </td><td> org.apache.cassandra.locator.GossipingPropertyFileSnitch </td><td> Bu veri merkezi ile sanal makinenin rafa çıkarımını yapma NetworkTopologyStrateg tarafından kullanılır.</td></tr>
</table>

#### <a name="step-6-capture-the-vm-image"></a>6. adım: sanal makine görüntüsünü yakalama
Ana bilgisayar adı (hk-CA-template.cloudapp.net) ve daha önce oluşturulan SSH özel anahtarı kullanarak sanal makinede oturum açın. Bkz. nasıl ssh komutunu kullanarak veya putty.exe oturum hakkında ayrıntılar için azure'da Linux ile SSH kullanma.

Aşağıdaki görüntü yakalamak için eylem dizisini yürütün:

##### <a name="1-deprovision"></a>1. Sağlamayı kaldırma
Komutunu "sudo waagent – sağlamayı kaldırma + kullanıcı" sanal makine örneği belirli bilgileri kaldırmak için. İçin bkz: [Linux sanal makinesi yakalama](capture-image-classic.md) görüntü yakalama işlemi hakkında daha fazla ayrıntı şablon olarak kullanmak için.

##### <a name="2-shut-down-the-vm"></a>2: sanal makineyi
Sanal makine vurgulanmış olduğundan emin olun ve altındaki komut çubuğundan kapatma bağlantısına tıklayın.

##### <a name="3-capture-the-image"></a>3: Görüntü Yakala
Sanal makine vurgulanmış olduğundan emin olun ve alt komut çubuğundan YAKALAMA bağlantısına tıklayın. Sonraki ekranda, verin (örneğin hk-cas-2-08-ub-14-04-2014071) bir görüntü adı, uygun görüntü açıklaması ve tıklayın "onay" YAKALAMA işlemini tamamlamak için işaretleyin.

Bu işlem birkaç saniye sürer ve görüntünün görüntü Galerisi GÖRÜNTÜLERİM bölümünde kullanılabilir olması gerekir. Kaynak VM görüntüsü başarıyla yakalandı sonra otomatik olarak silinir. 

## <a name="single-region-deployment-process"></a>Tek bölge dağıtım işlemi
**1. adım: sanal ağ oluşturma** Azure portalında oturum açın ve aşağıdaki tabloda gösterilen özniteliklere sahip bir sanal ağ (Klasik) oluşturun. Bkz: [Azure portalını kullanarak bir sanal ağ (Klasik) oluşturmak](../../../virtual-network/virtual-networks-create-vnet-classic-pportal.md) işleminin ayrıntılı adımlar için.      

<table>
<tr><th>VM öznitelik adı</th><th>Değer</th><th>Açıklamalar</th></tr>
<tr><td>Ad</td><td>vnet-cass-Batı-ABD</td><td></td></tr>
<tr><td>Bölge</td><td>Batı ABD</td><td></td></tr>
<tr><td>DNS Sunucuları</td><td>None</td><td>Bir DNS sunucusu kullanmıyorsanız gibi bu iletiyi yoksayın</td></tr>
<tr><td>Adres Alanı</td><td>10.1.0.0/16</td><td></td></tr>    
<tr><td>Başlangıç IP</td><td>10.1.0.0</td><td></td></tr>    
<tr><td>CIDR </td><td>/16 (65531)</td><td></td></tr>
</table>

Şu alt ağlar ekleyin:

<table>
<tr><th>Ad</th><th>Başlangıç IP</th><th>CIDR</th><th>Açıklamalar</th></tr>
<tr><td>web</td><td>10.1.1.0</td><td>/24 (251)</td><td>Web grubu için alt ağ</td></tr>
<tr><td>veri</td><td>10.1.2.0</td><td>/24 (251)</td><td>Veritabanı düğümleri için alt ağ</td></tr>
</table>

Veri ve Web alt ağlar, ağ güvenlik grupları kapsamını, bu makalenin kapsamı dışındadır aracılığıyla korunabilir.  

**2. adım: Sanal makineler sağlayın** önceden oluşturulmuş görüntüyü kullanarak bulut sunucusu "hk-c-svc-Batı" şu sanal makineleri oluşturun ve bunları aşağıda gösterildiği gibi ilgili alt ağlarına bağlayın:

<table>
<tr><th>Makine Adı    </th><th>Alt ağ    </th><th>IP Adresi    </th><th>Kullanılabilirlik kümesi</th><th>DC/raf</th><th>Çekirdek?</th></tr>
<tr><td>HK-c1-Batı-ABD    </td><td>veri    </td><td>10.1.2.4    </td><td>HK-c-aset-1    </td><td>DC = WESTUS raf raf1 = </td><td>Evet</td></tr>
<tr><td>HK-c2-Batı-ABD    </td><td>veri    </td><td>10.1.2.5    </td><td>HK-c-aset-1    </td><td>DC = WESTUS raf raf1 =    </td><td>Hayır </td></tr>
<tr><td>HK-c3-Batı-ABD    </td><td>veri    </td><td>10.1.2.6    </td><td>HK-c-aset-1    </td><td>DC = WESTUS raf rack2 =    </td><td>Evet</td></tr>
<tr><td>HK-c4-Batı-ABD    </td><td>veri    </td><td>10.1.2.7    </td><td>HK-c-aset-1    </td><td>DC = WESTUS raf rack2 =    </td><td>Hayır </td></tr>
<tr><td>HK-c5-Batı-ABD    </td><td>veri    </td><td>10.1.2.8    </td><td>HK-c-aset-2    </td><td>DC = WESTUS raf rack3 =    </td><td>Evet</td></tr>
<tr><td>HK-c6-Batı-ABD    </td><td>veri    </td><td>10.1.2.9    </td><td>HK-c-aset-2    </td><td>DC = WESTUS raf rack3 =    </td><td>Hayır </td></tr>
<tr><td>HK-c7-Batı-ABD    </td><td>veri    </td><td>10.1.2.10    </td><td>HK-c-aset-2    </td><td>DC = WESTUS raf rack4 =    </td><td>Evet</td></tr>
<tr><td>HK-c8-Batı-ABD    </td><td>veri    </td><td>10.1.2.11    </td><td>HK-c-aset-2    </td><td>DC = WESTUS raf rack4 =    </td><td>Hayır </td></tr>
<tr><td>HK-w1-Batı-ABD    </td><td>web    </td><td>10.1.1.4    </td><td>HK-w-aset-1    </td><td>                       </td><td>Yok</td></tr>
<tr><td>HK-w2-Batı-ABD    </td><td>web    </td><td>10.1.1.5    </td><td>HK-w-aset-1    </td><td>                       </td><td>Yok</td></tr>
</table>

Aşağıdaki işlem yukarıdaki VM'lerin listesini oluşturulmasını gerektirir:

1. Boş bir bulut hizmetinde belirli bir bölgede oluşturun.
2. Daha önce yakalanan görüntüden VM oluşturma ve daha önce oluşturduğunuz sanal ağa ekleyin; Bu VM'ler için yineleyin.
3. İç yük dengeleyici bulut hizmetine ekleme ve "verileri" alt ağına ekleme
4. Daha önce oluşturduğunuz her VM için önceden oluşturulmuş bir iç yük dengeleyiciye bağlı yük dengeli bir küme ile thrift trafiği için bir yük dengeli uç nokta Ekle

Yukarıdaki işlem, Azure portalını kullanarak yürütülebilecek; Windows makine (bir Windows makinesine erişiminiz yoksa azure'da VM kullanın) kullanın, tüm 8 VM otomatik olarak sağlamak için aşağıdaki PowerShell betiğini kullanın.

**Liste 1: sanal makine sağlama yönelik PowerShell Betiği**

        #Tested with Azure Powershell - November 2014
        #This powershell script deployes a number of VMs from an existing image inside an Azure region
        #Import your Azure subscription into the current Powershell session before proceeding
        #The process: 1. create Azure Storage account, 2. create virtual network, 3.create the VM template, 2. create a list of VMs from the template

        #fundamental variables - change these to reflect your subscription
        $country="us"; $region="west"; $vnetName = "your_vnet_name";$storageAccount="your_storage_account"
        $numVMs=8;$prefix = "hk-cass";$ilbIP="your_ilb_ip"
        $subscriptionName = "Azure_subscription_name";
        $vmSize="ExtraSmall"; $imageName="your_linux_image_name"
        $ilbName="ThriftInternalLB"; $thriftEndPoint="ThriftEndPoint"

        #generated variables
        $serviceName = "$prefix-svc-$region-$country"; $azureRegion = "$region $country"

        $vmNames = @()
        for ($i=0; $i -lt $numVMs; $i++)
        {
           $vmNames+=("$prefix-vm"+($i+1) + "-$region-$country" );
        }

        #select an Azure subscription already imported into Powershell session
        Select-AzureSubscription -SubscriptionName $subscriptionName -Current
        Set-AzureSubscription -SubscriptionName $subscriptionName -CurrentStorageAccountName $storageAccount

        #create an empty cloud service
        New-AzureService -ServiceName $serviceName -Label "hkcass$region" -Location $azureRegion
        Write-Host "Created $serviceName"

        $VMList= @()   # stores the list of azure vm configuration objects
        #create the list of VMs
        foreach($vmName in $vmNames)
        {
           $VMList += New-AzureVMConfig -Name $vmName -InstanceSize ExtraSmall -ImageName $imageName |
           Add-AzureProvisioningConfig -Linux -LinuxUser "localadmin" -Password "Local123" |
           Set-AzureSubnet "data"
        }

        New-AzureVM -ServiceName $serviceName -VNetName $vnetName -VMs $VMList

        #Create internal load balancer
        Add-AzureInternalLoadBalancer -ServiceName $serviceName -InternalLoadBalancerName $ilbName -SubnetName "data" -StaticVNetIPAddress "$ilbIP"
        Write-Host "Created $ilbName"
        #Add the thrift endpoint to the internal load balancer for all the VMs
        foreach($vmName in $vmNames)
        {
            Get-AzureVM -ServiceName $serviceName -Name $vmName |
                Add-AzureEndpoint -Name $thriftEndPoint -LBSetName "ThriftLBSet" -Protocol tcp -LocalPort 9160 -PublicPort 9160 -ProbePort 9160 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -InternalLoadBalancerName $ilbName |
                Update-AzureVM

            Write-Host "created $vmName"     
        }

**3. adım: Her VM'de Cassandra yapılandırma**

VM'de oturum açın ve aşağıdakileri gerçekleştirin:

* Veri merkezi ve raf özelliklerini belirtmek için $CASS_HOME/conf/cassandra-rackdc.properties düzenleyin:
  
       dc =EASTUS, rack =rack1
* Çekirdek düğümleri aşağıdaki gibi yapılandırmak için cassandra.yaml düzenleyin:
  
       Seeds: "10.1.2.4,10.1.2.6,10.1.2.8,10.1.2.10"

**4. adım: sanal makineleri başlatın ve kümeyi test edin**

Düğümlerin (örneğin hk-c1-Batı-ABD) birinde oturum açın ve küme durumunu görmek için aşağıdaki komutu çalıştırın:

       nodetool –h 10.1.2.4 –p 7199 status

Bir 8 düğüm kümesi için aşağıdakine benzer bir ekran görmeniz gerekir:

<table>
<tr><th>Durum</th><th>Adres    </th><th>Yükleme    </th><th>Belirteçler    </th><th>Sahibi </th><th>Konak kimliği    </th><th>Raf</th></tr>
<tr><th>GERİ AL    </td><td>10.1.2.4     </td><td>87.81 KB    </td><td>256    </td><td>38.0%    </td><td>GUID (kaldırıldı)</td><td>raf1</td></tr>
<tr><th>GERİ AL    </td><td>10.1.2.5     </td><td>41.08 KB    </td><td>256    </td><td>68.9%    </td><td>GUID (kaldırıldı)</td><td>raf1</td></tr>
<tr><th>GERİ AL    </td><td>10.1.2.6     </td><td>55.29 KB    </td><td>256    </td><td>68.8%    </td><td>GUID (kaldırıldı)</td><td>rack2</td></tr>
<tr><th>GERİ AL    </td><td>10.1.2.7     </td><td>55.29 KB    </td><td>256    </td><td>68.8%    </td><td>GUID (kaldırıldı)</td><td>rack2</td></tr>
<tr><th>GERİ AL    </td><td>10.1.2.8     </td><td>55.29 KB    </td><td>256    </td><td>68.8%    </td><td>GUID (kaldırıldı)</td><td>rack3</td></tr>
<tr><th>GERİ AL    </td><td>10.1.2.9     </td><td>55.29 KB    </td><td>256    </td><td>68.8%    </td><td>GUID (kaldırıldı)</td><td>rack3</td></tr>
<tr><th>GERİ AL    </td><td>10.1.2.10     </td><td>55.29 KB    </td><td>256    </td><td>68.8%    </td><td>GUID (kaldırıldı)</td><td>rack4</td></tr>
<tr><th>GERİ AL    </td><td>10.1.2.11     </td><td>55.29 KB    </td><td>256    </td><td>68.8%    </td><td>GUID (kaldırıldı)</td><td>rack4</td></tr>
</table>

## <a name="test-the-single-region-cluster"></a>Test tek bölgeli küme
Küme test etmek için aşağıdaki adımları kullanın:

1. Powershell komutu Get-Azureınternalloadbalancer komutu kullanarak iç yük dengeleyici (örneğin 10.1.2.101) IP adresini alın. Komutun sözdizimi aşağıda gösterilmiştir: Get-AzureLoadbalancer – [iç yük dengeleyici IP adresini birlikte ayrıntılarını görüntüler] ServiceName "hk-c-svc-Batı-ABD"
2. Web grubu VM (örneğin hk-w1-Batı-ABD) oturum Putty kullanarak veya ssh
3. $CASS_HOME/bin/cqlsh 10.1.2.101 yürütme 9160
4. Kümenin çalışıp çalışmadığını doğrulamak için aşağıdaki CQL komutları kullanın:
   
     İLE çoğaltma oluşturma anahtar alanı customers_ks = {'class': 'SimpleStrategy', 'replication_factor': 3};   Customers_ks kullanın.   Tablo Customers(customer_id int PRIMARY KEY, firstname text, lastname text); oluşturun   INSERT içine Customers(customer_id, firstname, lastname) VALUES(1, 'John', 'Doe');   INSERT INTO Customers(customer_id, firstname, lastname) VALUES (2, 'Jane', 'Etikan');
   
     SEÇİN *; MÜŞTERİLERDEN

Aşağıdaki sonuçları gibi bir şey görmeniz gerekir:

<table>
  <tr><th> customer_id </th><th> firstName </th><th> Soyadı </th></tr>
  <tr><td> 1 </td><td> John </td><td> Doe </td></tr>
  <tr><td> 2 </td><td> Jane </td><td> Doe </td></tr>
</table>

4. adımda oluşturulan anahtar ile 3 bir replication_factor SimpleStrategy kullanır. SimpleStrategy NetworkTopologyStrategy çok veri merkezinde ise dağıtımlar için tek bir veri merkezi dağıtımları önerilir. Bir replication_factor 3 düğüm hataları için dayanıklılık sağlar.

## <a id="tworegion"> </a>Çok bölgeli dağıtım işlemi
Tek bölge dağıtımı tamamlandı yararlanın ve ikinci bir bölgeye yüklemek için aynı işlemi yineleyin. Tek ve birden çok bölgeli dağıtımlar arasındaki temel fark, bölgeler arası iletişimi VPN tüneli kurulumu.; Vm'leri hazırlama ağ yüklemesiyle başlamanız ve Cassandra yapılandırın.

### <a name="step-1-create-the-virtual-network-at-the-2nd-region"></a>1. adım: 2 bölge düzeyinde sanal ağ oluşturma
Azure portalında oturum açın ve tabloda öznitelikleri göster ile bir sanal ağ oluşturun. Bkz: [Cloud-Only sanal ağ yapılandırma Azure portalında](../../../virtual-network/virtual-networks-create-vnet-classic-pportal.md) işleminin ayrıntılı adımlar için.      

<table>
<tr><th>Öznitelik Adı    </th><th>Değer    </th><th>Açıklamalar</th></tr>
<tr><td>Ad    </td><td>vnet-cass-Doğu-ABD</td><td></td></tr>
<tr><td>Bölge    </td><td>Doğu ABD</td><td></td></tr>
<tr><td>DNS Sunucuları        </td><td></td><td>Bir DNS sunucusu kullanmıyorsanız gibi bu iletiyi yoksayın</td></tr>
<tr><td>Noktadan siteye VPN bağlantısını yapılandırma</td><td></td><td>        Bu iletiyi yoksayın</td></tr>
<tr><td>Siteden siteye VPN'yi yapılandırın</td><td></td><td>        Bu iletiyi yoksayın</td></tr>
<tr><td>Adres Alanı    </td><td>10.2.0.0/16</td><td></td></tr>
<tr><td>Başlangıç IP    </td><td>10.2.0.0    </td><td></td></tr>
<tr><td>CIDR    </td><td>/16 (65531)</td><td></td></tr>
</table>

Şu alt ağlar ekleyin:

<table>
<tr><th>Ad    </th><th>Başlangıç IP    </th><th>CIDR    </th><th>Açıklamalar</th></tr>
<tr><td>web    </td><td>10.2.1.0    </td><td>/24 (251)    </td><td>Web grubu için alt ağ</td></tr>
<tr><td>veri    </td><td>10.2.2.0    </td><td>/24 (251)    </td><td>Veritabanı düğümleri için alt ağ</td></tr>
</table>


### <a name="step-2-create-local-networks"></a>2. adım: Yerel ağlar oluşturma
Azure sanal ağ içinde yerel bir ağ özel bir bulut ya da başka bir Azure bölgesine dahil olmak üzere uzak bir siteye eşleyen bir proxy adresi alandır. Bu proxy adres alanı bir uzak ağ geçidi yönlendirme ağ için doğru ağ hedeflere bağlıdır. Bkz: [bir Vnet'ten Vnet'e bağlantı yapılandırma](../../../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md) VNET-VNET bağlantısı oluşturma hakkında yönergeler için.

Aşağıdaki ayrıntıları başına iki yerel ağ oluşturun:

| Ağ Adı | VPN Ağ Geçidi Adresi | Adres Alanı | Açıklamalar |
| --- | --- | --- | --- |
| HK-lnet-Map-to-East-us |23.1.1.1 |10.2.0.0/16 |Yerel ağ oluşturulurken bir yer tutucu ağ geçidi adresi verin. Gerçek ağ geçidi adresi, ağ geçidi oluşturulduktan sonra doldurulur. İlgili uzak sanal ağ adres alanı eşleştiğinden emin olun; Bu örnekte Doğu ABD bölgesinde sanal ağ oluşturulur. |
| HK-lnet-Map-to-West-us |23.2.2.2 |10.1.0.0/16 |Yerel ağ oluşturulurken bir yer tutucu ağ geçidi adresi verin. Gerçek ağ geçidi adresi, ağ geçidi oluşturulduktan sonra doldurulur. İlgili uzak sanal ağ adres alanı eşleştiğinden emin olun; Bu örnekte Batı ABD bölgesinde sanal ağ oluşturulur. |

### <a name="step-3-map-local-network-to-the-respective-vnets"></a>3. adım: Harita "Yerel" ağ için ilgili sanal ağları
Azure portalı, her sanal ağ'ı seçin, "Yapılandır"'a tıklayın, "Yerel ağa bağlan" denetleyin ve aşağıdaki ayrıntıları başına yerel ağlar'ı seçin:

| Sanal Ağ | Yerel ağ |
| --- | --- |
| hk-vnet-west-us |HK-lnet-Map-to-East-us |
| hk-vnet-east-us |HK-lnet-Map-to-West-us |

### <a name="step-4-create-gateways-on-vnet1-and-vnet2"></a>4. adım: Ağ geçitleri ve VNET1'den vnet2'ye oluşturma
Her iki sanal ağ panosundan, VPN ağ geçidi sağlama işlemi tetiklemek için ağ geçidi Oluştur'a tıklayın. Birkaç dakika sonra her bir sanal ağ Pano gerçek bir ağ geçidi adresini görüntülemelidir.

### <a name="step-5-update-local-networks-with-the-respective-gateway-addresses"></a>5. adım: Güncelleştirme "Yerel" ağlarla ilgili "ağ geçidi" adresleri
Yalnızca sağlanan ağ geçitleri gerçek IP adresiyle yer tutucu ağ geçidi IP adresini değiştirmek için her iki yerel ağ düzenleyin. Aşağıdaki eşleme kullanın:

<table>
<tr><th>Yerel ağ    </th><th>Sanal Ağ Geçidi</th></tr>
<tr><td>HK-lnet-Map-to-East-us </td><td>Ağ geçidi için hk-vnet-Batı-ABD</td></tr>
<tr><td>HK-lnet-Map-to-West-us </td><td>Ağ geçidi hk-vnet-Doğu-ABD</td></tr>
</table>

### <a name="step-6-update-the-shared-key"></a>6. adım: paylaşılan anahtarı güncelleştirme
Her VPN ağ geçidi [her iki ağ geçitleri için çok anahtarını kullanın] IPSec anahtarını güncelleştirmek için aşağıdaki Powershell betiğini kullanın: Set-AzureVNetGatewayKey - VNetName hk-vnet-Doğu-ABD - LocalNetworkSiteName hk-lnet-map-to-west-us - SharedKey D9E76BKK Set-AzureVNetGatewayKey - VNetName hk-vnet-Batı-ABD - LocalNetworkSiteName hk-lnet-map-to-east-us - SharedKey D9E76BKK

### <a name="step-7-establish-the-vnet-to-vnet-connection"></a>7. adım: VNET-VNET bağlantısı
Azure portalından, ağ geçidi için ağ geçidi bağlantısı kurmak için her iki sanal ağ "PANO" menüsünü kullanın. "BAĞLAN" menü öğeleri alt araç çubuğunun kullanın. Birkaç dakika sonra panoyu bağlantı ayrıntılarını grafik görüntülemelidir.

### <a name="step-8-create-the-virtual-machines-in-region-2"></a>8. adım: sanal makineleri #2 bölgesinde oluşturun.
Bölge #1 dağıtımda aynı Azure depolama hesabına görüntü VHD dosyasını #2 bölgede kopyalama veya adımları izleyerek açıklandığı gibi bir Ubuntu görüntüsünü oluşturun ve görüntü oluşturun. Bu görüntü kullanın ve yeni bir bulut hizmetinde bir araya hk-c-svc-Doğu-ABD aşağıdaki listede yer alan sanal makineler oluşturun:

| Makine Adı | Alt ağ | IP Adresi | Kullanılabilirlik kümesi | DC/raf | Çekirdek? |
| --- | --- | --- | --- | --- | --- |
| HK-c1-Doğu-ABD |veri |10.2.2.4 |HK-c-aset-1 |DC = EASTUS raf raf1 = |Evet |
| HK-c2-Doğu-ABD |veri |10.2.2.5 |HK-c-aset-1 |DC = EASTUS raf raf1 = |Hayır |
| HK-c3-Doğu-ABD |veri |10.2.2.6 |HK-c-aset-1 |DC = EASTUS raf rack2 = |Evet |
| HK-c5-Doğu-ABD |veri |10.2.2.8 |HK-c-aset-2 |DC = EASTUS raf rack3 = |Evet |
| HK-c6-Doğu-ABD |veri |10.2.2.9 |HK-c-aset-2 |DC = EASTUS raf rack3 = |Hayır |
| HK-c7-Doğu-ABD |veri |10.2.2.10 |HK-c-aset-2 |DC = EASTUS raf rack4 = |Evet |
| HK-c8-Doğu-ABD |veri |10.2.2.11 |HK-c-aset-2 |DC = EASTUS raf rack4 = |Hayır |
| HK-w1-Doğu-ABD |web |10.2.1.4 |HK-w-aset-1 |Yok |Yok |
| HK-w2-Doğu-ABD |web |10.2.1.5 |HK-w-aset-1 |Yok |Yok |

Bölge #1 olarak aynı yönergeleri izleyin ancak 10.2.xxx.xxx adres alanı kullanın.

### <a name="step-9-configure-cassandra-on-each-vm"></a>9. adım: Her VM'de Cassandra yapılandırma
VM'de oturum açın ve aşağıdakileri gerçekleştirin:

1. Veri merkezi ve raf özelliklerini biçiminde belirtmek için $CASS_HOME/conf/cassandra-rackdc.properties Düzenle: dc = EASTUS raf raf1 =
2. Çekirdek düğümleri yapılandırmak için cassandra.yaml Düzenle: çekirdekler: "10.1.2.4,10.1.2.6,10.1.2.8,10.1.2.10,10.2.2.4,10.2.2.6,10.2.2.8,10.2.2.10"

### <a name="step-10-start-cassandra"></a>10. adım: Cassandra Başlat
Oturum her VM'ye ve aşağıdaki komutu çalıştırarak arka planda Cassandra Başlat: $CASS_HOME/bin/cassandra

## <a name="test-the-multi-region-cluster"></a>Çok bölgeli küme test
Artık her bir Azure bölgesinde 8 düğüm ile 16 düğüme Cassandra dağıtıldı. Bu düğümler aynı kümedeki ortak küme adı ve çekirdek düğüm yapılandırması da var. Küme test etmek için aşağıdaki işlemi kullanın:

### <a name="step-1-get-the-internal-load-balancer-ip-for-both-the-regions-using-powershell"></a>1. adım: PowerShell kullanarak iki bölgeleri için iç yük dengeleyici IP alma
* Get-Azureınternalloadbalancer - ServiceName "hk-c-svc-Batı-ABD"
* Get-Azureınternalloadbalancer - ServiceName "hk-c-svc-Doğu-ABD"  
  
    IP adreslerini Not (için örnek Batı - 10.1.2.101, Doğu - 10.2.2.101) görüntülenir.

### <a name="step-2-execute-the-following-in-the-west-region-after-logging-into-hk-w1-west-us"></a>2. adım: hk-w1-Batı-ABD oturum açtıktan sonra Batı bölgesinde aşağıdakileri yürütün
1. $CASS_HOME/bin/cqlsh 10.1.2.101 yürütme 9160
2. Aşağıdaki CQL komutları yürütün:
   
     İLE çoğaltma oluşturma anahtar alanı customers_ks = {'class': 'NetworkToplogyStrategy', 'WESTUS': 3, 'EASTUS': 3};   Customers_ks kullanın.   Tablo Customers(customer_id int PRIMARY KEY, firstname text, lastname text); oluşturun   INSERT içine Customers(customer_id, firstname, lastname) VALUES(1, 'John', 'Doe');   INSERT INTO Customers(customer_id, firstname, lastname) VALUES (2, 'Jane', 'Etikan');   SEÇİN *; MÜŞTERİLERDEN

Bir ekran aşağıdaki gibi görmeniz gerekir:

| customer_id | firstName | Soyadı |
| --- | --- | --- |
| 1 |John |Doe |
| 2 |Jane |Doe |

### <a name="step-3-execute-the-following-in-the-east-region-after-logging-into-hk-w1-east-us"></a>3. adım: hk-w1-Doğu-ABD oturum açtıktan sonra Doğu bölgesinde aşağıdakileri yürütün:
1. $CASS_HOME/bin/cqlsh 10.2.2.101 yürütme 9160
2. Aşağıdaki CQL komutları yürütün:
   
     Customers_ks kullanın.   Tablo Customers(customer_id int PRIMARY KEY, firstname text, lastname text); oluşturun   INSERT içine Customers(customer_id, firstname, lastname) VALUES(1, 'John', 'Doe');   INSERT INTO Customers(customer_id, firstname, lastname) VALUES (2, 'Jane', 'Etikan');   SEÇİN *; MÜŞTERİLERDEN

Batı bölgesi görüldüğü gibi aynı ekranı görmeniz gerekir:

| customer_id | firstName | Soyadı |
| --- | --- | --- |
| 1 |John |Doe |
| 2 |Jane |Doe |

Birkaç daha fazla ekler yürütün ve olanlar için Batı çoğaltılması bakın-bize kümenin parçası.

## <a name="test-cassandra-cluster-from-nodejs"></a>Node.js test Cassandra kümesi
"Web" katmanı daha önce oluşturduğunuz Linux Vm'leri birini kullanarak, daha önce eklenen verileri okumak için basit bir Node.js komut yürütme

**1. adım: Node.js ile Cassandra istemcisi yükleme**

1. Node.js ve npm'yi yükleyin
2. Düğüm paket "cassandra-istemci" yükleme npm kullanarak
3. Json dizesi alınan verilerin görüntülendiği Kabuk isteminde aşağıdaki komutu yürütün:
   
        var pooledCon = require('cassandra-client').PooledConnection;
        var ksName = "custsupport_ks";
        var cfName = "customers_cf";
        var hostList = ['internal_loadbalancer_ip:9160'];
        var ksConOptions = { hosts: hostList,
                             keyspace: ksName, use_bigints: false };
   
        function createKeyspace(callback){
           var cql = 'CREATE KEYSPACE ' + ksName + ' WITH strategy_class=SimpleStrategy AND strategy_options:replication_factor=1';
           var sysConOptions = { hosts: hostList,  
                                 keyspace: 'system', use_bigints: false };
           var con = new pooledCon(sysConOptions);
           con.execute(cql,[],function(err) {
           if (err) {
             console.log("Failed to create Keyspace: " + ksName);
             console.log(err);
           }
           else {
             console.log("Created Keyspace: " + ksName);
             callback(ksConOptions, populateCustomerData);
           }
           });
           con.shutdown();
        }
   
        function createColumnFamily(ksConOptions, callback){
          var params = ['customers_cf','custid','varint','custname',
                        'text','custaddress','text'];
          var cql = 'CREATE COLUMNFAMILY ? (? ? PRIMARY KEY,? ?, ? ?)';
        var con =  new pooledCon(ksConOptions);
          con.execute(cql,params,function(err) {
              if (err) {
                 console.log("Failed to create column family: " + params[0]);
                 console.log(err);
              }
              else {
                 console.log("Created column family: " + params[0]);
                 callback();
              }
          });
          con.shutdown();
        }
   
        //populate Data
        function populateCustomerData() {
           var params = ['John','Infinity Dr, TX', 1];
           updateCustomer(ksConOptions,params);
   
           params = ['Tom','Fermat Ln, WA', 2];
           updateCustomer(ksConOptions,params);
        }
   
        //update also inserts the record if none exists
        function updateCustomer(ksConOptions,params)
        {
          var cql = 'UPDATE customers_cf SET custname=?,custaddress=? where custid=?';
          var con = new pooledCon(ksConOptions);
          con.execute(cql,params,function(err) {
              if (err) console.log(err);
              else console.log("Inserted customer : " + params[0]);
          });
          con.shutdown();
        }
   
        //read the two rows inserted above
        function readCustomer(ksConOptions)
        {
          var cql = 'SELECT * FROM customers_cf WHERE custid IN (1,2)';
          var con = new pooledCon(ksConOptions);
          con.execute(cql,[],function(err,rows) {
              if (err)
                 console.log(err);
              else
                 for (var i=0; i<rows.length; i++)
                    console.log(JSON.stringify(rows[i]));
            });
           con.shutdown();
        }
   
        //exectue the code
        createKeyspace(createColumnFamily);
        readCustomer(ksConOptions)

## <a name="conclusion"></a>Sonuç
Microsoft Azure, bu alıştırmada gösterildiği gibi hem Microsoft hem de açık kaynak yazılım çalıştırılmasını sağlayan esnek bir platformdur. Yüksek oranda kullanılabilir Cassandra kümeleri, birden çok hata etki alanlarında küme düğümlerine yayılması aracılığıyla tek bir veri merkezi üzerinde dağıtılabilir. Cassandra kümeleri, birden çok bölgede coğrafi olarak uzak Azure olağanüstü durum düzeltme sistemler için de dağıtılabilir. Azure ve Cassandra birlikte yüksek oranda ölçeklenebilir, yüksek oranda kullanılabilir oluşumu etkinleştirir ve olağanüstü durum kurtarılabilir bulut Hizmetleri, günümüzün internet hizmetlerinin ölçeği.  

## <a name="references"></a>Başvurular
* [http://cassandra.apache.org](http://cassandra.apache.org)
* [http://www.datastax.com](http://www.datastax.com)
* [http://www.nodejs.org](http://www.nodejs.org)

