---
title: "Cassandra ile Linux Azure üzerinde çalışan | Microsoft Docs"
description: "Cassandra küme Linux Azure Virtual Machines'de bir Node.js uygulamasını çalıştırma"
services: virtual-machines-linux
documentationcenter: nodejs
author: craigshoemaker
manager: routlaw
editor: 
tags: azure-service-management
ms.assetid: 30de1f29-e97d-492f-ae34-41ec83488de0
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: cshoe
ms.openlocfilehash: 28eb281d8d301fa5478afb0925c74349de92ca58
ms.sourcegitcommit: e38120a5575ed35ebe7dccd4daf8d5673534626c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/13/2017
---
# <a name="running-cassandra-with-linux-on-azure-and-accessing-it-from-nodejs"></a>Azure’da Linux ile Cassandra Çalıştırma ve Cassandra’ya Node.js ile Erişme
> [!IMPORTANT] 
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modeli kullanarak yer almaktadır. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir. Resource Manager şablonları için bkz: [Datastax Kurumsal](https://azure.microsoft.com/documentation/templates/datastax) ve [Spark küme ve Cassandra CentOS üzerinde](https://azure.microsoft.com/documentation/templates/spark-and-cassandra-on-centos/).

## <a name="overview"></a>Genel Bakış
Microsoft Azure hem de Microsoft işletim sistemleri, uygulama sunucuları, ileti Ara yanı sıra NoSQL ve SQL veritabanlarını hem ticari ve açık kaynak modellerinden iyi olarak Microsoft dışı yazılımlar olarak çalıştırılan bir açık bulut platformudur. Azure dahil olmak üzere genel Bulutları dayanıklı hizmetler oluşturma dikkatli planlama ve kasıtlı mimarisi iyi depolama katmanları olarak hem uygulama sunucuları için gerektirir. Doğal olarak Cassandra'nın dağıtılmış depolama mimarisi hataya dayanıklı küme hataları için yüksek oranda kullanılabilir sistemlerini oluşturmaya yardımcı olur. Cassandra bulut ölçeği cassandra.apache.org Apache yazılım Foundation tarafından tutulan NoSQL veritabanı olan; Cassandra Java'da yazılmış ve bu nedenle hem Linux yanı sıra Windows platformları çalıştırır.

Bu makaleyi odağını, Microsoft Azure sanal makineler ve sanal ağlar yararlanarak tek ve birden çok veri merkezi küme olarak Ubuntu üzerinde Cassandra dağıtım göstermektir. Küme dağıtımı en iyi duruma getirilmiş üretim iş yükleri için birden çok disk düğüm yapılandırması, uygun halka topolojisi tasarımı ve gerekli çoğaltma, veri tutarlılığı, üretilen iş ve yüksek desteklemek için modelleme verileri gerektirir, bu makalenin kapsamı dışında aynıdır Kullanılabilirlik gereksinimleri.

Ne göstermek için temel bir yaklaşım Cassandra küme oluşturma ile ilgili bu makalede alır, Docker, Chef veya altyapı dağıtımı çok kolaylaştırabilir Puppet karşılaştırılan.  

## <a name="the-deployment-models"></a>Dağıtım modelleri
Microsoft Azure ağ erişimini hassas ağ güvenliği elde kısıtlanabilir yalıtılmış özel kümeleri dağıtımını sağlar.  Bu makalede temel düzeyde Cassandra dağıtım gösteren ilgili olduğundan, biz tutarlılık düzeyi ve verimlilik için en iyi depolama tasarımı odaklanır değil. Ağ gereksinimleri bizim kuramsal kümesi için listesi aşağıdadır:

* Dış sistemler Cassandra veritabanından içinde veya Azure dışına erişemiyor
* Cassandra küme thrift trafiği için yük dengeleyici arkasında olması gerekir
* Her veri merkezinde bir Gelişmiş küme kullanılabilirlik için iki grup Cassandra düğümler dağıtma
* Uygulama sunucusu grubu veritabanına doğrudan erişimi yalnızca kümeyi bu nedenle, kilitleme
* Ortak ağ uç nokta SSH dışında
* Her Cassandra düğümü sabit bir iç IP adresi gerekiyor

Tek bir Azure bölgesine veya iş yükü dağıtılmış niteliğine göre birden çok bölgeye Cassandra dağıtılabilir. Bölgeli dağıtım modeli, son kullanıcıların belirli bir coğrafi konum aynı Cassandra altyapısı aracılığıyla yakın hizmet vermek için de kullanılabilir. Cassandra'nın yerleşik düğümü çoğaltma alır dikkatli çok ana eşitleme birden çok veri merkezleri kaynaklanan yazar ve uygulamalar verilere tutarlı bir görünümünü sunar. Çok bölge dağıtımı daha geniş Azure hizmet kesintisi risk azaltma ile de yardımcı olabilir. Cassandra'nın ince ayarlanabilir tutarlılık ve çoğaltma topolojisini uygulamaları farklı RPO ihtiyaçlarını karşılamak için yardımcı olur.

### <a name="single-region-deployment"></a>Tek bölge dağıtımı
Biz tek bölge dağıtımı ile başlamalı ve bir bölgeli modeli oluşturma learnings elde etme. Azure sanal ağı, yukarıda belirtilen ağ güvenliği gereksinimleri karşılanabilir yalıtılmış alt ağlar oluşturmak üzere kullanılır.  Tek bölge dağıtımı oluşturma'da açıklandığı işlemi Ubuntu 14.04 LTS ve Cassandra 2.08 kullanır; Ancak, işlem kolayca diğer Linux çeşitleri benimsenen. Tek bölge dağıtımı sistemle ilgili özelliklerini bazıları şunlardır:  

**Yüksek Kullanılabilirlik:** böylece düğümleri yüksek kullanılabilirlik için birden çok hata etki alanları arasında yayılır Şekil 1'de gösterilen Cassandra düğümler için iki kullanılabilirlik kümeleri dağıtılır. Her kullanılabilirlik kümesiyle açıklama VM'ler 2 hata etki alanları için eşlenmedi.  Microsoft Azure kullanır (örneğin, ana bilgisayar veya konuk işletim sistemi düzeltme eki uygulama/yükseltmeler, uygulama yükseltmelerini) yükseltme etki alanı kavramı sırasında aşağı planlanmamış süresini (örn. donanım veya yazılım hatası) yönetmek için hata etki alanı kavramı zamanlanan saati yönetmek için kullanılır. Lütfen bakın [Azure uygulamaları için yüksek kullanılabilirlik ve olağanüstü durum kurtarma](http://msdn.microsoft.com/library/dn251004.aspx) yüksek kullanılabilirlik modemle hızlı bağlantılar sağlama, hata ve yükseltme etki alanlarının rolü.

![Tek bölge dağıtımı](./media/cassandra-nodejs/cassandra-linux1.png)

Şekil 1: Tek bölge dağıtımı

Bu yazma zaman gruba belirli hata etki alanı VM'ler açık eşleme Azure izin vermeyen unutmayın; Bu nedenle, Şekil 1'de gösterilen bile dağıtım modeliyle, istatistiksel olarak olası tüm sanal makineleri iki hata etki alanlarını dört yerine eşlenmiş.

**Yük Dengeleme Thrift trafiği:** Thrift istemci kitaplıkları web sunucusu içinde bağlanmak aracılığıyla bir iç yük dengeleyici kümeye. Bu "data" alt ağına iç yük dengeleyici ekleme işlemini gerektirir (Şekil 1 bakın) Cassandra küme barındıran bulut hizmetini bağlamında. İç yük dengeleyicisi tanımlandıktan sonra her düğüm ile önceden tanımlanmış yük dengeleyicisi adı ile bir yük dengelenmiş küme ek açıklamalar eklenmesi için yük dengeli uç nokta gerektiriyor. Bkz: [Azure iç Yük Dengeleme ](../../../load-balancer/load-balancer-internal-overview.md)daha fazla ayrıntı için.

**Küme oluştururken çekirdeği:** yeni düğümler küme topolojisini bulmak için çekirdek düğümleri ile iletişim kuracak şekilde oluştururken çekirdeği için en yüksek oranda kullanılabilir düğümleri seçmek önemlidir. Her kullanılabilirlik kümesinden bir düğümü tek hata noktası önlemek için çekirdek düğüm olarak atanır.

**Çoğaltma faktörünü ve tutarlılık düzeyi:** Cassandra'nın yerleşik yüksek kullanılabilirlik ve veri dayanıklılığı işlemleri çoğaltma faktörüyle (RF - kopya kümesinde depolanan her bir satır sayısı) ve tutarlılık düzeyi (için yineleme sayısı sonucu çağırana döndürmeden önce okunan ve yazılan olabilir). Tutarlılık düzeyi CRUD sorgu verme sırasında belirtilen ancak çoğaltma faktörü KEYSPACE (ilişkisel bir veritabanına benzer) oluşturma sırasında belirtilir. Cassandra belgelerine bakın [tutarlılık için yapılandırma](http://www.datastax.com/documentation/cassandra/2.0/cassandra/dml/dml_config_consistency_c.html) tutarlılık ayrıntılarını ve çekirdek hesaplama formülü için.

Cassandra iki tür veri bütünlüğü modelleri – tutarlılık ve nihai tutarlılık destekler; Çoğaltma faktörünü ve tutarlılık düzeyi birlikte verileri yazma işlemi tamamlandığında veya sonuçta tutarlı hemen tutarlı olup olmayacağını belirler. Örneğin, veri tutarlılığı herhangi bir tutarlılık düzeyi bağlanırken tutarlılık düzeyi her zaman olduğu gibi çekirdek belirtme sağlar, elde etmek için gerektiği gibi yazılması için yineleme sayısı sonuçta tutarlı olan verileri (örneğin bir) ÇEKİRDEĞİNİ sonuçlanır.

Yukarıda, 3 ve çekirdek çoğaltma faktörüyle 8 düğüm kümesi (2 düğümleri okumak veya tutarlılık için yazılan) okuma/yazma tutarlılık düzeyi, çoğaltma grubu başına en fazla 1 düğümü teorik kaybı uygulama başlangıç bildirimde bulunmadan önce varlığını sürdürmesini hata oluştu. Bu, tüm anahtar alanları iyi dengelenmiş okuma/istekleri yazma olduğunu varsayar.  Dağıtılan küme için kullanacağız Parametreler şunlardır:

Tek bölge Cassandra küme yapılandırması:

| Küme parametresi | Değer | Açıklamalar |
| --- | --- | --- |
| Düğüm (N) sayısı |8 |Kümedeki düğümler toplam sayısı |
| Çoğaltma faktörü (RF) |3 |Belirli bir satırın çoğaltmaların sayısı |
| Tutarlılık düzeyi (yazma) |QUORUM[(RF/2) +1) = 2] formülü sonucu yuvarlanan |Yanıt çağırana gönderilmeden önce en çok 2 çoğaltma Yazar; 3 çoğaltma sonunda tutarlı bir şekilde yazılır. |
| Tutarlılık düzeyi (okuma) |Çekirdek [(RF/2) + 1 = 2] formül sonucu yuvarlanan |2 çoğaltma, yanıtını çağırana göndermeden önce okur. |
| Çoğaltma stratejisi |NetworkTopologyStrategy bakın [veri çoğaltma](http://www.datastax.com/documentation/cassandra/2.0/cassandra/architecture/architectureDataDistributeReplication_c.html) Cassandra belgelerinde daha fazla bilgi için |Dağıtım topolojisi anlar ve çoğaltmalar, düğümlerde yerleştirir, böylece tüm çoğaltmaların aynı rafa monte şunun yok |
| Snitch |GossipingPropertyFileSnitch bakın [Snitches](http://www.datastax.com/documentation/cassandra/2.0/cassandra/architecture/architectureSnitchesAbout_c.html) Cassandra belgelerinde daha fazla bilgi için |NetworkTopologyStrategy snitch kavramı topoloji anlamak için kullanır. GossipingPropertyFileSnitch veri merkezi ve raf her düğüme eşleme daha iyi denetim olanağı verir. Küme, dedikodu sonra bu bilgileri yaymak için kullanır. Bu dinamik IP ayarında PropertyFileSnitch göre çok daha kolaydır |

**Cassandra küme Azure dikkate alınacak noktalar:** Microsoft Azure sanal makineleri özelliğine disk kalıcılığını; Azure Blob Depolama kullanır Azure depolama her disk yüksek dayanıklılık için 3 çoğaltmalarının kaydeder. Her bir Cassandra tabloya eklenen veri satırının zaten 3 yinelemede depolanır ve bu nedenle veri tutarlılığını zaten (RF) çoğaltma faktörü 1 olsa bile dikkate anlamına gelir. Ana çoğaltma faktörle 1 olan tek bir Cassandra düğüm başarısız olsa bile uygulama kapalı kalma süresi yaşar sorunudur. Ancak, bir düğüm Azure yapı denetleyicisi tarafından tanınan sorunları (örneğin, donanım, sistem yazılım hataları) çalışmıyorsa, aynı depolama sürücülerini kullanarak onun yerine yeni bir düğüm hazırlayacağınız. Eski bir değiştirmek için yeni bir düğüm sağlama birkaç dakika sürebilir.  Benzer şekilde konuk işletim sistemi değişiklikleri gibi planlı bakım etkinlikler, Cassandra yükseltildikten ve kümedeki düğümlerin çalışırken Azure yapı denetleyicisi uygulama değişiklikleri gerçekleştirir.  Çalışırken yükseltme de birkaç düğüm aynı anda sürebilir ve bu nedenle küme birkaç bölümler için kısa kapalı kalma süresi karşılaşabilirsiniz. Ancak, veri yerleşik Azure Storage artıklık nedeniyle kayıp olmaz.  

Yüksek kullanılabilirlik gerektirmeyen Azure dağıtılan sistemler için (örneğin yaklaşık 99,9 olduğu 8.76 SA/yılına eşdeğer; bkz [yüksek kullanılabilirlik](http://en.wikipedia.org/wiki/High_availability) Ayrıntılar için) RF ile çalıştırmanız mümkün olabilir = 1 ve tutarlılık düzeyi = ONE.  Yüksek oranda kullanılabilirlik gereksinimleri, RF olan uygulamalar için 3 ve tutarlılık düzeyi = = çekirdek çoğaltmaları biri düğümlerinden biri aşağı süresini tolerans. RF = 1 geleneksel dağıtımlarda (örn. şirket içi), disk hataları gibi sorunlar kaynaklanan olası veri kaybı nedeniyle kullanılamaz.   

## <a name="multi-region-deployment"></a>Çok bölge dağıtımı
Yukarıda açıklanan Cassandra'nın veri merkezi kullanmayan çoğaltma ve tutarlılık modeli, tüm dış araçları gerek kalmadan sunuyoruz bölgeli dağıtım ile yardımcı olur. Burada, birden çok yöneticili yazmalar için veritabanı yansıtma için Kurulum oldukça karmaşık olabilir bu geleneksel ilişkisel veritabanlarından oldukça farklı değildir. Ayarlanmış bir çok bölgede Cassandra aşağıdakiler de dahil olmak üzere kullanım senaryoları ile yardımcı olabilir:

**Yakınlık dayalı dağıtım:** Kiracı Kullanıcı Temizle eşleme ile çok kiracılı uygulamalara-için-bölge bölgeli kümenin düşük gecikme tarafından benefited. Örneğin, bir öğrenme yönetim sistemleri için eğitim kurumları Doğu ABD ve Batı ABD bölgeler için ilgili artık kampüsünde sunmak için Dağıtılmış bir kümede dağıtabilirsiniz analytics yanı sıra işlem. Verileri zaman okuma ve yazma işlemleri sırasında yerel olarak tutarlı olabilir ve bölgeler arasında sonuçta tutarlı olabilir. Medya dağıtım, e-ticaret ve herhangi bir şey gibi diğer örnekler vardır ve yoğunlaşmıştır coğrafi kullanıcı temel görevi gören her şeyi, bu dağıtım modeli için iyi durumdur.

**Yüksek Kullanılabilirlik:** artıklık yazılım ve donanım yüksek kullanılabilirliğini modemle hızlı bağlantılar sağlama bir anahtar etken; yapı güvenilir bulut sistemleri Microsoft Azure üzerinde ayrıntılı bilgi için bkz. Microsoft Azure üzerinde doğru artıklık elde yalnızca güvenilir bir bölgeli küme dağıtarak yoludur. Uygulamaları bir etkin-etkin veya etkin-Pasif modu dağıtılabilir ve Azure trafik Yöneticisi bölgelerinden kapalı ise, etkin bölge trafiği yönlendirebilirsiniz.  Kullanılabilirlik 99,9, ise tek bölge dağıtımı ile iki bölge dağıtımı bir formülün hesaplanan 99.9999 kullanılabilirliğini elde edebilirsiniz: (1-(1-0.999) * (1-0.999)) * 100); Ayrıntılar için yukarıdaki incelemesine bakın.

**Olağanüstü durum kurtarma:** bölgeli Cassandra küme düzgün bir şekilde tasarlanmış, dayanacak geri dönülemez veri merkezi kesintilerini. Bir bölge kapalı ise, son kullanıcıların hizmet veren diğer bölgelere dağıtılan uygulamayı başlatabilirsiniz. Diğer iş sürekliliği belirtilmesinden gibi uygulama verileri zaman uyumsuz ardışık düzeninde kaynaklanan bazı veri kaybıyla dayanıklı olması gerekir. Ancak, Kurtarma Cassandra geleneksel veritabanı kurtarma işlemleri tarafından harcanan süre çok swifter yapar. Şekil 2 sekiz düğümlerle tipik bölgeli dağıtım modeli, her bölgede gösterir. Her iki bölgeden yansıtma görüntülerini simetrisi aynı için; yine de uygun istiyor musunuz? gerçek dünya tasarımları iş yükü türünü (örn. işlem veya analitik), RPO, RTO, veri tutarlılığı ve kullanılabilirlik gereksinimlerine bağlıdır.

![Çoklu bölge dağıtımı](./media/cassandra-nodejs/cassandra-linux2.png)

Şekil 2: Bölgeli Cassandra dağıtımı

### <a name="network-integration"></a>Ağ tümleştirme
Kümeleri üzerinde iki bölgede bulunan özel ağlara dağıtılan sanal makinelerin birbirleriyle VPN tüneli kullanarak iletişim kurar. VPN tüneli ağ dağıtım işlemi sırasında sağlanan iki yazılım ağ geçidi bağlanır. Her iki bölgeden "web" ve "data" alt bakımından benzer ağ mimarisi; yine de sahip istiyor musunuz? Azure ağı gerektiğinde kadar alt ağlar oluşturulmasına izin verir ve ağ güvenliği tarafından gerektiği şekilde ACL'ler uygulayın. Küme topolojisi arası tasarlama sırasında veri merkezi iletişim gecikmesi ve ağ trafiğini ekonomik etkisini ele alınması gerekir.

### <a name="data-consistency-for-multi-data-center-deployment"></a>Birden çok veri merkezi dağıtım için veri tutarlılığı
Dağıtılmış dağıtımları performans ve yüksek kullanılabilirlik küme topolojisi etkisini farkında olması gerekiyor. RF ve tutarlılık düzeyi çekirdek tüm veri merkezleri kullanılabilirliğine bağlı olmadığından bu şekilde seçilmesi gerekir.
Yüksek tutarlılık gereken bir sistem LOCAL_QUORUM tutarlılık düzeyi (için okuma ve yazma) veri uzak veri merkezleri için zaman uyumsuz olarak kopyalandığı sırada yerel okuma ve yazma işlemleri yerel düğümlerden karşılanır olduğundan emin olmanızı sağlar.  Tablo 2 yazma daha sonra özetlenen bölgeli küme yapılandırma ayrıntılarını özetlemektedir.

**İki bölge Cassandra küme yapılandırması**

| Küme parametresi | Değer | Açıklamalar |
| --- | --- | --- |
| Düğüm (N) sayısı |8 + 8 |Kümedeki düğümler toplam sayısı |
| Çoğaltma faktörü (RF) |3 |Belirli bir satırın çoğaltmaların sayısı |
| Tutarlılık düzeyi (yazma) |LOCAL_QUORUM [(sum(RF)/2) +1) = 4] formül sonucu yuvarlanan |2 düğümleri ilk veri merkezine zaman uyumlu olarak yazılır; Çekirdek için gereken ek 2 düğümleri 2 veri merkezine zaman uyumsuz olarak yazılır. |
| Tutarlılık düzeyi (okuma) |LOCAL_QUORUM ((RF/2) + 1) = formül sonucu aşağı yuvarlanmasını 2 |Okuma isteği yalnızca bir bölgesinden karşılanır; yanıt istemciye gönderilmeden önce 2 düğümleri salt okunurdur. |
| Çoğaltma stratejisi |NetworkTopologyStrategy bakın [veri çoğaltma](http://www.datastax.com/documentation/cassandra/2.0/cassandra/architecture/architectureDataDistributeReplication_c.html) Cassandra belgelerinde daha fazla bilgi için |Dağıtım topolojisi anlar ve çoğaltmalar, düğümlerde yerleştirir, böylece tüm çoğaltmaların aynı rafa monte şunun yok |
| Snitch |GossipingPropertyFileSnitch bakın [Snitches](http://www.datastax.com/documentation/cassandra/2.0/cassandra/architecture/architectureSnitchesAbout_c.html) Cassandra belgelerinde daha fazla bilgi için |NetworkTopologyStrategy snitch kavramı topoloji anlamak için kullanır. GossipingPropertyFileSnitch veri merkezi ve raf her düğüme eşleme daha iyi denetim olanağı verir. Küme, dedikodu sonra bu bilgileri yaymak için kullanır. Bu dinamik IP ayarında PropertyFileSnitch göre çok daha kolaydır |

## <a name="the-software-configuration"></a>YAZILIM YAPILANDIRMA
Aşağıdaki yazılım sürümlerinden dağıtımı sırasında kullanılır:

<table>
<tr><th>Yazılım</th><th>Kaynak</th><th>Sürüm</th></tr>
<tr><td>JRE    </td><td>[JRE 8](http://www.oracle.com/technetwork/java/javase/downloads/server-jre8-downloads-2133154.html) </td><td>8U5</td></tr>
<tr><td>JNA    </td><td>[JNA](https://github.com/twall/jna) </td><td> 3.2.7</td></tr>
<tr><td>Cassandra</td><td>[Apache Cassandra 2.0.8](http://www.apache.org/dist/cassandra/2.0.8/apache-cassandra-2.0.8-bin.tar.gz)</td><td> 2.0.8</td></tr>
<tr><td>Ubuntu    </td><td>[Microsoft Azure](https://azure.microsoft.com/) </td><td>14.04 LTS</td></tr>
</table>

JRE indirme el ile Oracle lisans kabulü gerektirdiğinden, dağıtım basitleştirmek için gerekli tüm yazılımı biz Küme dağıtımı için bir precursor olarak oluşturulacağını Ubuntu şablon görüntüsü daha sonra yüklemeyle masaüstüne indirin.

Yukarıdaki yazılımın bir dizine iyi bilinen yükleme (örneğin Windows %TEMP%/downloads veya ~/Downloads çoğu Linux dağıtımları veya Mac üzerinde) yerel bilgisayarda indirin.

### <a name="create-ubuntu-vm"></a>UBUNTU VM OLUŞTURMA
Böylece görüntü birçok Cassandra düğümlerini sağlamak için yeniden kullanılabilir işleminin bu adımında Ubuntu görüntü önkoşul yazılımı ile oluşturacağız.  

#### <a name="step-1-generate-ssh-key-pair"></a>1. adım: SSH anahtar çifti oluşturma
Azure sağlama aynı anda PEM ya da DER ortak anahtar kodlanmış bir X509 gerekir. Nasıl yapılır Linux Azure üzerinde ile SSH kullanma konumunda bulunan yönergeleri kullanarak bir genel/özel anahtar çifti oluşturur. Bir SSH istemcisi Windows veya Linux olarak putty.exe kullanmayı planlıyorsanız, kodlanmış PEM dönüştürmeniz gerekir puttygen.exe; kullanarak PPK biçimine RSA özel anahtarı Bunun için yönergeler yukarıdaki web sayfasında bulunabilir.

#### <a name="step-2-create-ubuntu-template-vm"></a>2. adım: Ubuntu şablonu VM oluşturma
VM şablonu oluşturmak için Klasik Azure portalında oturum açın ve aşağıdaki sırayı kullanır: yeni, işlem, sanal makine, başlangıç Galerisi, UBUNTU, Ubuntu Server 14.04 LTS tıklatın ve ardından sağ oka tıklayın. Bir Linux VM oluşturmayı açıklar bir öğretici için çalışan bir sanal makine Linux bkz.

#1 "sanal makine yapılandırma" ekranında aşağıdaki bilgileri girin:

<table>
<tr><th>ALAN ADI              </td><td>       ALAN DEĞERİ               </td><td>         AÇIKLAMALAR                </td><tr>
<tr><td>SÜRÜM YAYIN TARİHİ    </td><td> Aşağı açılan listeden bir tarih seçin</td><td></td><tr>
<tr><td>SANAL MAKİNE ADI    </td><td> CASS şablonu                   </td><td> Bu VM ana adıdır </td><tr>
<tr><td>KATMANI                     </td><td> STANDART                           </td><td> Varsayılan adı bırakın              </td><tr>
<tr><td>BOYUTU                     </td><td> A1                              </td><td>G/ç gereksinimlerine göre VM seçin; Bu amaç için varsayılan adı bırakın </td><tr>
<tr><td> YENİ BİR KULLANICI ADI             </td><td> yerelyönetici                       </td><td> "Yönetici" Ubuntu 12. xx ve sonra ayrılmış kullanıcı adı sağlanmış</td><tr>
<tr><td> KİMLİK DOĞRULAMASI         </td><td> Onay kutusu                 </td><td>Bir SSH anahtarı ile güvenli isteyip istemediğinizi denetleyin </td><tr>
<tr><td> SERTİFİKA             </td><td> Ortak anahtar sertifikası dosya adı </td><td> Daha önce oluşturulan ortak anahtarı kullanın</td><tr>
<tr><td> Yeni Parola    </td><td> güçlü parola </td><td> </td><tr>
<tr><td> Parolayı Onayla    </td><td> güçlü parola </td><td></td><tr>
</table>

"Sanal makine yapılandırma" ekranında #2 aşağıdaki bilgileri girin:

<table>
<tr><th>ALAN ADI             </th><th> ALAN DEĞERİ                       </th><th> AÇIKLAMALAR                                 </th></tr>
<tr><td> BULUT HİZMETİ    </td><td> Yeni bir bulut hizmeti oluştur    </td><td>Sanal makineler gibi bir kapsayıcı işlem kaynaklarını bulut hizmetidir</td></tr>
<tr><td> BULUT HİZMETİ DNS ADI    </td><td>ubuntu template.cloudapp.net    </td><td>Bir makine belirsiz yük dengeleyici ad verin</td></tr>
<tr><td> BÖLGE/BENZEŞİM GRUBU/SANAL AĞ </td><td>    Batı ABD    </td><td> Web uygulamalarınızın Cassandra küme erişimlerin bir bölge seçin</td></tr>
<tr><td>DEPOLAMA HESABI </td><td>    Varsayılanı kullan    </td><td>Belirli bir bölgedeki varsayılan depolama hesabı ya da önceden oluşturulmuş depolama hesabı kullanın</td></tr>
<tr><td>KULLANILABİLİRLİK KÜMESİ </td><td>    Hiçbiri </td><td>    Boş bırakın</td></tr>
<tr><td>UÇ NOKTALARI    </td><td>Varsayılanı kullan </td><td>    Varsayılan SSH yapılandırmasını kullanın </td></tr>
</table>

Sağ oka tıklayın, #3 ekranda Varsayılanları bırakabilir ve VM sağlama işlemini tamamlamak için "denetimi" düğmesini tıklatın. Birkaç dakika sonra VM adı "ubuntu-şablon" ile "çalışır" durumda olması gerekir.

### <a name="install-the-necessary-software"></a>GEREKLİ YAZILIMI YÜKLEYİN
#### <a name="step-1-upload-tarballs"></a>1. adım: Karşıya yükleme tarballs
SCP veya pscp kullanarak, önceden indirilen yazılım aşağıdaki komut biçimi kullanarak ~/downloads dizinine kopyalayın:

##### <a name="pscp-server-jre-8u5-linux-x64targz-localadminhk-cas-templatecloudappnethomelocaladmindownloadsserver-jre-8u5-linux-x64targz"></a>pscp server-jre-8u5-linux-x64.tar.gzlocaladmin@hk-cas-template.cloudapp.net:/home/localadmin/downloads/server-jre-8u5-linux-x64.tar.gz
Yukarıdaki komut için olduğu gibi Cassandra BITS de JRE yineleyin.

#### <a name="step-2-prepare-the-directory-structure-and-extract-the-archives"></a>2. adım: dizin yapısını hazırlamak ve arşivler Ayıkla
VM oturum ve dizin yapısını oluşturun ve yazılım bash komut dosyası kullanarak bir süper kullanıcı olarak ayıklayın:

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


Bu komut dosyası VIM penceresine yapıştırın, satır başı kaldırdığınızdan emin olun ('\r ") aşağıdaki komutu kullanarak:

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
Aşağıdaki komut dizisi kullanın: jna 3.2.7.jar yükleyeceğinizi ve platform jna 3.2.7.jar /usr/share.java dizinine apt sudo get libjna java şu komutu

Sembolik bağlantılar $CASS_HOME/lib dizininde oluşturun, böylece Cassandra başlangıç betiği bu Kavanoz bulabilirsiniz:

    ln -s /usr/share/java/jna-3.2.7.jar $CASS_HOME/lib/jna.jar

    ln -s /usr/share/java/jna-platform-3.2.7.jar $CASS_HOME/lib/jna-platform.jar

#### <a name="step-5-configure-cassandrayaml"></a>Adım 5: cassandra.yaml yapılandırma
[Biz bu gerçek sağlama sırasında ince ayar] tüm sanal makineler için gerekli yapılandırmayı yansıtacak şekilde her bir VM üzerinde cassandra.yaml düzenleyin:

<table>
<tr><th>Alan adı   </th><th> Değer  </th><th>    Açıklamalar </th></tr>
<tr><td>küme_adı </td><td>    "CustomerService"    </td><td> Dağıtımınızı yansıtır adı kullan</td></tr>
<tr><td>listen_address    </td><td>[boş bırakın]    </td><td> "Localhost" Sil </td></tr>
<tr><td>rpc_addres   </td><td>[boş bırakın]    </td><td> "Localhost" Sil </td></tr>
<tr><td>oluştururken Çekirdeği    </td><td>"10.1.2.4, 10.1.2.6, 10.1.2.8"    </td><td>Şu oluştururken çekirdeği atanan tüm IP adresleri listesi.</td></tr>
<tr><td>endpoint_snitch </td><td> org.apache.cassandra.locator.GossipingPropertyFileSnitch </td><td> Bu veri merkezi ve VM raf çıkarımını yapma NetworkTopologyStrateg tarafından kullanılır</td></tr>
</table>

#### <a name="step-6-capture-the-vm-image"></a>6. adım: VM görüntüsü yakalama
Ana bilgisayar adı (hk-CA-template.cloudapp.net) ve daha önce oluşturulan SSH özel anahtar kullanılarak sanal makinede oturum açın. Bkz: nasıl yapılır Linux üzerinde ssh komutunu kullanarak veya putty.exe oturum hakkında ayrıntılar için Azure ile SSH kullanma.

Görüntü yakalama eylemleri aşağıdaki dizisini yürütün:

##### <a name="1-deprovision"></a>1. Deprovision
Komutunu "sudo waagent-deprovision + kullanıcı" sanal makine örneği belirli bilgileri kaldırmak için. İçin bkz: [Linux sanal makine yakalama](capture-image.md) görüntü yakalama işlemi hakkında daha fazla ayrıntı şablon olarak kullanmak için.

##### <a name="2-shutdown-the-vm"></a>2: VM'yi kapatma
Sanal makine vurgulanmış olduğundan emin olun ve altındaki komut çubuğundan kapatma bağlantısına tıklayın.

##### <a name="3-capture-the-image"></a>3: görüntü yakalama
Sanal makine vurgulanmış olduğundan emin olun ve altındaki komut çubuğundan YAKALAMA bağlantısını tıklatın. Sonraki ekranda (örneğin hk-cas-2-08-ub-14-04-2014071) bir görüntü adı verin, görüntü açıklaması uygun ve YAKALAMA işlemini tamamlamak için "" işaretini tıklatın.

Bu işlem birkaç saniye sürer ve görüntünün görüntü Galerisi GÖRÜNTÜLERİM bölümünde kullanılabilir olmalıdır. Görüntüsü başarıyla yakalandı sonra kaynak VM otomatik olarak silinir. 

## <a name="single-region-deployment-process"></a>Tek bölge dağıtım işlemi
**1. adım: sanal ağ oluşturma** Azure portalı günlüğüne ve aşağıdaki tabloda gösterilen özniteliklere sahip bir sanal ağ (Klasik) oluşturun. Bkz: [Azure portalını kullanarak bir sanal ağ (Klasik) oluşturmak](../../../virtual-network/virtual-networks-create-vnet-classic-pportal.md) işleminin ayrıntılı adımlar için.      

<table>
<tr><th>VM öznitelik adı</th><th>Değer</th><th>Açıklamalar</th></tr>
<tr><td>Ad</td><td>vnet-cass-Batı-ABD</td><td></td></tr>
<tr><td>Bölge</td><td>Batı ABD</td><td></td></tr>
<tr><td>DNS sunucuları</td><td>Hiçbiri</td><td>Bir DNS sunucusu kullanmıyorsanız gibi bu iletiyi yoksayın</td></tr>
<tr><td>Adres alanı</td><td>10.1.0.0/16</td><td></td></tr>    
<tr><td>Başlangıç IP</td><td>10.1.0.0</td><td></td></tr>    
<tr><td>CIDR </td><td>/16 (65531)</td><td></td></tr>
</table>

Aşağıdaki alt ağlar ekleyin:

<table>
<tr><th>Ad</th><th>Başlangıç IP</th><th>CIDR</th><th>Açıklamalar</th></tr>
<tr><td>Web</td><td>10.1.1.0</td><td>/24 (251)</td><td>Alt ağ web grubu</td></tr>
<tr><td>Veri</td><td>10.1.2.0</td><td>/24 (251)</td><td>Veritabanı düğümleri için alt ağ</td></tr>
</table>

Veri ve Web alt ağlar, ağ güvenlik grupları kapsamını bu makalenin kapsamı dışındadır üzerinden korunabilir.  

**2. adım: Sanal makine sağlamak** daha önce oluşturduğunuz görüntüsünü kullanarak, bulut sunucusu "hk-c-svc-Batı" şu sanal makineleri oluşturur ve bunları aşağıda gösterildiği gibi ilgili alt ağa bağlayın:

<table>
<tr><th>Makine Adı    </th><th>Alt ağ    </th><th>IP Adresi    </th><th>Kullanılabilirlik kümesi</th><th>DC/raf</th><th>Çekirdek?</th></tr>
<tr><td>HK-c1-Batı-ABD    </td><td>Veri    </td><td>10.1.2.4    </td><td>HK-c-aset-1    </td><td>DC WESTUS raf = raf1 = </td><td>Evet</td></tr>
<tr><td>HK-c2-Batı-ABD    </td><td>Veri    </td><td>10.1.2.5    </td><td>HK-c-aset-1    </td><td>DC WESTUS raf = raf1 =    </td><td>Hayır </td></tr>
<tr><td>HK-c3-Batı-ABD    </td><td>Veri    </td><td>10.1.2.6    </td><td>HK-c-aset-1    </td><td>DC WESTUS raf = rack2 =    </td><td>Evet</td></tr>
<tr><td>HK-c4-Batı-ABD    </td><td>Veri    </td><td>10.1.2.7    </td><td>HK-c-aset-1    </td><td>DC WESTUS raf = rack2 =    </td><td>Hayır </td></tr>
<tr><td>HK-c5-Batı-ABD    </td><td>Veri    </td><td>10.1.2.8    </td><td>HK-c-aset-2    </td><td>DC WESTUS raf = rack3 =    </td><td>Evet</td></tr>
<tr><td>HK-c6-Batı-ABD    </td><td>Veri    </td><td>10.1.2.9    </td><td>HK-c-aset-2    </td><td>DC WESTUS raf = rack3 =    </td><td>Hayır </td></tr>
<tr><td>HK-c7-Batı-ABD    </td><td>Veri    </td><td>10.1.2.10    </td><td>HK-c-aset-2    </td><td>DC WESTUS raf = rack4 =    </td><td>Evet</td></tr>
<tr><td>HK-c8-Batı-ABD    </td><td>Veri    </td><td>10.1.2.11    </td><td>HK-c-aset-2    </td><td>DC WESTUS raf = rack4 =    </td><td>Hayır </td></tr>
<tr><td>HK-w1-Batı-ABD    </td><td>Web    </td><td>10.1.1.4    </td><td>HK-w-aset-1    </td><td>                       </td><td>Yok</td></tr>
<tr><td>HK-w2-Batı-ABD    </td><td>Web    </td><td>10.1.1.5    </td><td>HK-w-aset-1    </td><td>                       </td><td>Yok</td></tr>
</table>

Yukarıdaki listeye VM'lerin oluşturulması aşağıdaki işlem gerektirir:

1. Belirli bir bölgedeki bir boş bulut hizmeti oluşturma
2. Daha önce yakalanan görüntüden bir VM oluşturun ve daha önce oluşturduğunuz sanal ağa bağlayın; Bu tüm VM'ler için yineleyin
3. Bir iç yük dengeleyici bulut hizmetine eklemek ve "data" alt ağına bağlayın
4. Yük dengeli uç nokta yük dengelenmiş bir küme önceden oluşturulmuş iç yük dengeleyiciye bağlı aracılığıyla thrift trafiği için daha önce oluşturduğunuz her VM için ekleme

Yukarıdaki işlem, Klasik Azure portalını kullanarak çalıştırılabilir; Windows makine (kullanın) bir Windows makinesine erişiminiz yoksa azure'da VM bir kullan tüm 8 sanal makineleri otomatik olarak sağlamak için aşağıdaki PowerShell betiğini kullanın.

**Listesi 1: PowerShell Betiği sanal makineleri sağlama**

        #Tested with Azure Powershell - November 2014
        #This powershell script deployes a number of VMs from an existing image inside an Azure region
        #Import your Azure subscription into the current Powershell session before proceeding
        #The process: 1. create Azure Storage account, 2. create virtual network, 3.create the VM template, 2. crate a list of VMs from the template

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
        #Add add the thrift endpoint to the internal load balancer for all the VMs
        foreach($vmName in $vmNames)
        {
            Get-AzureVM -ServiceName $serviceName -Name $vmName |
                Add-AzureEndpoint -Name $thriftEndPoint -LBSetName "ThriftLBSet" -Protocol tcp -LocalPort 9160 -PublicPort 9160 -ProbePort 9160 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -InternalLoadBalancerName $ilbName |
                Update-AzureVM

            Write-Host "created $vmName"     
        }

**3. adım: Her VM Cassandra yapılandırma**

VM oturum açın ve aşağıdakileri gerçekleştirin:

* Veri merkezi ve raf özelliklerini belirtmek için $CASS_HOME/conf/cassandra-rackdc.properties düzenleyin:
  
       dc =EASTUS, rack =rack1
* Çekirdek düğümlerini aşağıdaki gibi yapılandırmak için cassandra.yaml düzenleyin:
  
       Seeds: "10.1.2.4,10.1.2.6,10.1.2.8,10.1.2.10"

**4. adım: sanal makineleri başlatın ve küme test etme**

Düğümler (örneğin hk-c1-Batı-us) birine oturum açın ve küme durumunu görmek için aşağıdaki komutu çalıştırın:

       nodetool –h 10.1.2.4 –p 7199 status

Benzer şekilde bir 8 düğüm kümesi için bir görüntü görmeniz gerekir:

<table>
<tr><th>Durum</th><th>Adres    </th><th>Yükleme    </th><th>Belirteçler    </th><th>Sahibi </th><th>Ana bilgisayar kimliği    </th><th>Raf</th></tr>
<tr><th>KALDIRMA    </td><td>10.1.2.4     </td><td>87.81 KB    </td><td>256    </td><td>38.0%    </td><td>GUID (kaldırılır)</td><td>raf1</td></tr>
<tr><th>KALDIRMA    </td><td>10.1.2.5     </td><td>41.08 KB    </td><td>256    </td><td>68.9%    </td><td>GUID (kaldırılır)</td><td>raf1</td></tr>
<tr><th>KALDIRMA    </td><td>10.1.2.6     </td><td>55.29 KB    </td><td>256    </td><td>68.8%    </td><td>GUID (kaldırılır)</td><td>rack2</td></tr>
<tr><th>KALDIRMA    </td><td>10.1.2.7     </td><td>55.29 KB    </td><td>256    </td><td>68.8%    </td><td>GUID (kaldırılır)</td><td>rack2</td></tr>
<tr><th>KALDIRMA    </td><td>10.1.2.8     </td><td>55.29 KB    </td><td>256    </td><td>68.8%    </td><td>GUID (kaldırılır)</td><td>rack3</td></tr>
<tr><th>KALDIRMA    </td><td>10.1.2.9     </td><td>55.29 KB    </td><td>256    </td><td>68.8%    </td><td>GUID (kaldırılır)</td><td>rack3</td></tr>
<tr><th>KALDIRMA    </td><td>10.1.2.10     </td><td>55.29 KB    </td><td>256    </td><td>68.8%    </td><td>GUID (kaldırılır)</td><td>rack4</td></tr>
<tr><th>KALDIRMA    </td><td>10.1.2.11     </td><td>55.29 KB    </td><td>256    </td><td>68.8%    </td><td>GUID (kaldırılır)</td><td>rack4</td></tr>
</table>

## <a name="test-the-single-region-cluster"></a>Tek bölge kümesi test
Küme test etmek için aşağıdaki adımları kullanın:

1. Powershell komutu Get-AzureInternalLoadbalancer komutunu kullanarak, iç yük dengeleyici IP adresi (örneğin Al  10.1.2.101). Komutun sözdizimi aşağıda gösterilmiştir: Get-AzureLoadbalancer – [IP adresini yanı sıra iç yük dengeleyicisi ayrıntılarını görüntüler] ServiceName "hk-c-svc-Batı-us"
2. Web grubu VM (örneğin hk-w1-Batı-us) günlüğüne Putty kullanarak veya ssh
3. $CASS_HOME/bin/cqlsh 10.1.2.101 yürütme 9160
4. Kümenin çalışıp çalışmadığını doğrulamak için aşağıdaki CQL komutları kullanın:
   
     İLE çoğaltma oluşturma KEYSPACE customers_ks = {'sınıfı': 'SimpleStrategy', 'replication_factor': 3};   Customers_ks; kullanın.   Tablo Customers(customer_id int PRIMARY KEY, firstname text, lastname text); oluşturma   INSERT INTO Customers(customer_id, firstname, lastname) VALUES(1, 'John', 'Doe');   INSERT INTO Customers(customer_id, firstname, lastname) değerleri (2, 'Jane', 'Etikan');
   
     SEÇİN * MÜŞTERİLERDEN;

Bir görünüm aşağıdaki gibi görmeniz gerekir:

<table>
  <tr><th> customer_id </th><th> FirstName </th><th> Soyadı </th></tr>
  <tr><td> 1 </td><td> John </td><td> Doe </td></tr>
  <tr><td> 2 </td><td> Jane </td><td> Doe </td></tr>
</table>

4. adımda oluşturduğunuz keyspace 3'ün bir replication_factor SimpleStrategy kullandığını unutmayın. SimpleStrategy NetworkTopologyStrategy çok veri merkezi ancak dağıtımlar için tek bir veri merkezi dağıtımları önerilir. Replication_factor 3 düğümü hataları için dayanıklılık sunar.

## <a id="tworegion"></a>Bölgeli dağıtım işlemi
İşlem tek bölge dağıtımı tamamlandı yararlanır ve ikinci bölge yüklemek için aynı işlemi yineleyin. Bir veya birden çok bölge dağıtımı arasındaki temel farklılık arası bölge iletişimi için VPN tüneli kurulduğundan; Biz başlatılır ağ yüklemesi ile sanal makineleri sağlamak ve Cassandra yapılandırın.

### <a name="step-1-create-the-virtual-network-at-the-2nd-region"></a>1. adım: 2 bölge sanal ağ oluşturma
Azure Klasik portalında oturum açın ve tablodaki öznitelikleri göster ile bir sanal ağ oluşturun. Bkz: [Azure Klasik Portalı'nda Cloud-Only sanal ağ yapılandırma](../../../virtual-network/virtual-networks-create-vnet-classic-pportal.md) işleminin ayrıntılı adımlar için.      

<table>
<tr><th>Öznitelik Adı    </th><th>Değer    </th><th>Açıklamalar</th></tr>
<tr><td>Ad    </td><td>vnet-cass-Doğu-us</td><td></td></tr>
<tr><td>Bölge    </td><td>Doğu ABD</td><td></td></tr>
<tr><td>DNS sunucuları        </td><td></td><td>Bir DNS sunucusu kullanmıyorsanız gibi bu iletiyi yoksayın</td></tr>
<tr><td>Noktadan siteye VPN bağlantısını yapılandırma</td><td></td><td>        Bu iletiyi yoksayın</td></tr>
<tr><td>Siteden siteye VPN'yi yapılandırın</td><td></td><td>        Bu iletiyi yoksayın</td></tr>
<tr><td>Adres alanı    </td><td>10.2.0.0/16</td><td></td></tr>
<tr><td>Başlangıç IP    </td><td>10.2.0.0    </td><td></td></tr>
<tr><td>CIDR    </td><td>/16 (65531)</td><td></td></tr>
</table>

Aşağıdaki alt ağlar ekleyin:

<table>
<tr><th>Ad    </th><th>Başlangıç IP    </th><th>CIDR    </th><th>Açıklamalar</th></tr>
<tr><td>Web    </td><td>10.2.1.0    </td><td>/24 (251)    </td><td>Alt ağ web grubu</td></tr>
<tr><td>Veri    </td><td>10.2.2.0    </td><td>/24 (251)    </td><td>Veritabanı düğümleri için alt ağ</td></tr>
</table>


### <a name="step-2-create-local-networks"></a>2. adım: Yerel ağlar oluşturma
Azure sanal ağ yerel bir ağda özel bir bulut ya da başka bir Azure bölgesi de dahil olmak üzere uzak bir siteye eşleyen bir proxy adresi alanıdır. Bu proxy adres alanı bir uzak ağ geçidi yönlendirme ağ için doğru ağ hedeflerine bağlıdır. Bkz: [Vnet'i Vnet'e bağlantı yapılandırma](../../../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md) VNET-VNET bağlantı kurma hakkında yönergeler için.

Aşağıdaki ayrıntıları başına iki yerel ağlar oluşturun:

| Ağ adı | VPN ağ geçidi adresi | Adres alanı | Açıklamalar |
| --- | --- | --- | --- |
| HK-lnet-Map-to-East-us |23.1.1.1 |10.2.0.0/16 |Yerel ağ oluşturulurken bir yer tutucu ağ geçidi adresi verin. Ağ geçidi oluşturulduktan sonra gerçek ağ geçidi adresi girilir. İlgili uzak VNET adres alanı tam olarak eşleştiğinden emin olun; Bu durumda Doğu ABD bölgesinde sanal ağ oluşturuldu. |
| HK-lnet-Map-to-West-us |23.2.2.2 |10.1.0.0/16 |Yerel ağ oluşturulurken bir yer tutucu ağ geçidi adresi verin. Ağ geçidi oluşturulduktan sonra gerçek ağ geçidi adresi girilir. İlgili uzak VNET adres alanı tam olarak eşleştiğinden emin olun; Bu durumda Batı ABD bölgesinde sanal ağ oluşturuldu. |

### <a name="step-3-map-local-network-to-the-respective-vnets"></a>"3. adım: Eşleme yerel" ağa ilgili sanal ağları
Azure Klasik portalından her sanal ağ seçin, "Yapılandır"'ı tıklatın, "Yerel ağa bağlan" denetleyin ve aşağıdaki ayrıntıları başına yerel ağlar seçin:

| Sanal Ağ | Yerel ağ |
| --- | --- |
| HK-vnet-Batı-ABD |HK-lnet-Map-to-East-us |
| HK-vnet-Doğu-us |HK-lnet-Map-to-West-us |

### <a name="step-4-create-gateways-on-vnet1-and-vnet2"></a>4. adım: Ağ geçitleri VNET1 ve vnet2'yi oluşturma
Her iki sanal ağlar panodan oluşturmak sağlama işlemi VPN ağ geçidi tetikleyecek ağ geçidi'ı tıklatın. Birkaç dakika sonra her sanal ağ Panosu gerçek ağ geçidi adresi görüntülemelidir.

### <a name="step-5-update-local-networks-with-the-respective-gateway-addresses"></a>5. adım: Güncelleştirme "Yerel" ağlarla ilgili "ağ geçidi" adresleri
Yalnızca sağlanan ağ geçitleri gerçek IP adresiyle yer tutucu ağ geçidi IP adresini değiştirmek için hem yerel ağlar düzenleyin. Aşağıdaki eşleme kullanın:

<table>
<tr><th>Yerel ağ    </th><th>Sanal Ağ Geçidi</th></tr>
<tr><td>HK-lnet-Map-to-East-us </td><td>Ağ geçidi hk-vnet-Batı-ABD</td></tr>
<tr><td>HK-lnet-Map-to-West-us </td><td>Ağ geçidi hk-vnet-Doğu-ABD</td></tr>
</table>

### <a name="step-6-update-the-shared-key"></a>6. adım: paylaşılan anahtar güncelleştir
Her VPN ağ geçidi [hem ağ geçitleri için artırmak amacıyla anahtarını kullanın] IPSec anahtarı güncelleştirmek için aşağıdaki Powershell betiğini kullanın: Set-AzureVNetGatewayKey - vnetname adlı hk-vnet-Doğu-us - LocalNetworkSiteName hk-lnet-map-to-west-us - SharedKey D9E76BKK Set-AzureVNetGatewayKey - vnetname adlı hk-vnet-Batı-us - LocalNetworkSiteName hk-lnet-map-to-east-us - SharedKey D9E76BKK

### <a name="step-7-establish-the-vnet-to-vnet-connection"></a>7. adım: VNET-VNET bağlantısı
Klasik Azure portalından, ağ geçidi için ağ geçidi bağlantısı kurmak için her iki sanal ağlar "PANO" menüsünü kullanın. "BAĞLAN" menü öğeleri alt kısımdaki araç kullanın. Birkaç dakika sonra Pano için grafik bağlantı ayrıntılarını görüntüleme.

### <a name="step-8-create-the-virtual-machines-in-region-2"></a>8. adım: Bölge #2 sanal makineler oluşturma
Aynı Azure depolama hesabı görüntüsünü VHD dosyasına #2 bölgede bulunan kopyalama veya adımları izleyerek bölge #1 dağıtımı'nda açıklandığı gibi Ubuntu görüntüsünü oluşturmak ve görüntü oluşturun. Bu görüntüyü kullanın ve aşağıdaki sanal makinelerin listesini yeni bir bulut hizmeti hk-c-svc-Doğu-us oluşturun:

| Makine Adı | Alt ağ | IP Adresi | Kullanılabilirlik kümesi | DC/raf | Çekirdek? |
| --- | --- | --- | --- | --- | --- |
| HK-c1-Doğu-us |Veri |10.2.2.4 |HK-c-aset-1 |DC EASTUS raf = raf1 = |Evet |
| HK-c2-Doğu-us |Veri |10.2.2.5 |HK-c-aset-1 |DC EASTUS raf = raf1 = |Hayır |
| HK-c3-Doğu-us |Veri |10.2.2.6 |HK-c-aset-1 |DC EASTUS raf = rack2 = |Evet |
| HK-c5-Doğu-us |Veri |10.2.2.8 |HK-c-aset-2 |DC EASTUS raf = rack3 = |Evet |
| HK-c6-Doğu-us |Veri |10.2.2.9 |HK-c-aset-2 |DC EASTUS raf = rack3 = |Hayır |
| HK-c7-Doğu-us |Veri |10.2.2.10 |HK-c-aset-2 |DC EASTUS raf = rack4 = |Evet |
| HK-c8-Doğu-us |Veri |10.2.2.11 |HK-c-aset-2 |DC EASTUS raf = rack4 = |Hayır |
| HK-w1-Doğu-us |Web |10.2.1.4 |HK-w-aset-1 |Yok |Yok |
| HK-w2-Doğu-us |Web |10.2.1.5 |HK-w-aset-1 |Yok |Yok |

Bölge #1 olarak aynı yönergeleri izleyin, ancak 10.2.xxx.xxx adres alanı kullanın.

### <a name="step-9-configure-cassandra-on-each-vm"></a>9. adım: Her VM Cassandra yapılandırma
VM oturum açın ve aşağıdakileri gerçekleştirin:

1. Veri merkezi ve raf özellikleri biçiminde belirtmek için $CASS_HOME/conf/cassandra-rackdc.properties Düzenle: dc EASTUS raf = raf1 =
2. Çekirdek düğümlerini yapılandırmak için cassandra.yaml Düzenle: oluştururken çekirdeği: "10.1.2.4,10.1.2.6,10.1.2.8,10.1.2.10,10.2.2.4,10.2.2.6,10.2.2.8,10.2.2.10"

### <a name="step-10-start-cassandra"></a>10. adım: Cassandra Başlat
Her bir VM oturum ve aşağıdaki komutu çalıştırarak arka planda Cassandra Başlat: $CASS_HOME/bin/cassandra

## <a name="test-the-multi-region-cluster"></a>Bölgeli küme test
Artık her Azure bölgesindeki 8 düğümlerle 16 düğüme Cassandra dağıtıldı. Aynı kümedeki ortak küme adı ve çekirdek düğüm yapılandırması, bu düğümler şunlardır. Küme sınamak için aşağıdaki yordamı kullanın:

### <a name="step-1-get-the-internal-load-balancer-ip-for-both-the-regions-using-powershell"></a>1. adım: PowerShell kullanarak her iki bölgeler için iç yük dengeleyici IP Al
* Get-AzureInternalLoadbalancer - ServiceName "hk-c-svc-Batı-us"
* Get-AzureInternalLoadbalancer - ServiceName "hk-c-svc-Doğu-us"  
  
    IP adreslerini not alın (örneğin - 10.1.2.101, Doğu - Batı 10.2.2.101) görüntülenir.

### <a name="step-2-execute-the-following-in-the-west-region-after-logging-into-hk-w1-west-us"></a>2. adım: aşağıdakileri oturum hk-w1-Batı-us açtıktan sonra Batı bölgesinde yürütün
1. $CASS_HOME/bin/cqlsh 10.1.2.101 yürütme 9160
2. Aşağıdaki CQL komutları yürütün:
   
     İLE çoğaltma oluşturma KEYSPACE customers_ks = {'sınıfı': 'NetworkToplogyStrategy', 'WESTUS': 3 'EASTUS': 3};   Customers_ks; kullanın.   Tablo Customers(customer_id int PRIMARY KEY, firstname text, lastname text); oluşturma   INSERT INTO Customers(customer_id, firstname, lastname) VALUES(1, 'John', 'Doe');   INSERT INTO Customers(customer_id, firstname, lastname) değerleri (2, 'Jane', 'Etikan');   SEÇİN * MÜŞTERİLERDEN;

Bir görünüm aşağıdaki gibi görmeniz gerekir:

| customer_id | FirstName | Soyadı |
| --- | --- | --- |
| 1 |John |Doe |
| 2 |Jane |Doe |

### <a name="step-3-execute-the-following-in-the-east-region-after-logging-into-hk-w1-east-us"></a>3. adım: oturum hk-w1-Doğu-us açtıktan sonra Doğu bölgesinde aşağıdakileri yürütün:
1. $CASS_HOME/bin/cqlsh 10.2.2.101 yürütme 9160
2. Aşağıdaki CQL komutları yürütün:
   
     Customers_ks; kullanın.   Tablo Customers(customer_id int PRIMARY KEY, firstname text, lastname text); oluşturma   INSERT INTO Customers(customer_id, firstname, lastname) VALUES(1, 'John', 'Doe');   INSERT INTO Customers(customer_id, firstname, lastname) değerleri (2, 'Jane', 'Etikan');   SEÇİN * MÜŞTERİLERDEN;

Batı bölgesini görüldüğü gibi aynı görüntü görmeniz gerekir:

| customer_id | FirstName | Soyadı |
| --- | --- | --- |
| 1 |John |Doe |
| 2 |Jane |Doe |

Birkaç daha fazla eklemeleri yürütün ve olanlar için Batı çoğaltıldığından emin bakın-bize kümesinin parçası.

## <a name="test-cassandra-cluster-from-nodejs"></a>Test Cassandra Node.js kümeden
"Web" crated Linux VM'ler, daha önce katmanı kullanarak, daha önce eklenen verileri okumak için basit bir Node.js betik yürütülmez.

**1. adım: Node.js ve Cassandra istemcisi yükleme**

1. Node.js ve npm yükleme
2. Düğüm paketi "cassandra-istemci" yükleme npm kullanma
3. Alınan veriler json dizesi görüntüleyen Kabuk isteminde aşağıdaki betiği yürütün:
   
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
   
        //update will also insert the record if none exists
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
Microsoft Azure tarafından bu alıştırmada gösterildiği gibi hem Microsoft, hem de açık kaynak yazılımının çalışmasını sağlayan esnek bir platformdur. Yüksek oranda kullanılabilir Cassandra kümeler, küme düğümlerini birden fazla hata etki alanlarında yayılmak aracılığıyla tek bir veri merkezi üzerinde dağıtılabilir. Cassandra kümeleri de olağanüstü durum kanıtını sistemler için birden çok coğrafi olarak birbirinden uzak Azure bölgeleri üzerinden dağıtılabilir. Azure ve Cassandra birlikte yüksek düzeyde ölçeklenebilir, yüksek oranda kullanılabilir yapımı sağlar ve olağanüstü durum kurtarılabilir bulut Hizmetleri tarafından bugünün internet Hizmetleri ölçeği.  

## <a name="references"></a>Başvurular
* [http://cassandra.apache.org](http://cassandra.apache.org)
* [http://www.datastax.com](http://www.datastax.com)
* [http://www.nodejs.org](http://www.nodejs.org)

