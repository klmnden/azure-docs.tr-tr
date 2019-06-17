---
title: Cloud Foundry Azure ile nasıl tümleştirildiğini | Microsoft Docs
description: Cloud Foundry Kurumsal deneyimini geliştirmek için Azure hizmetleri nasıl kullanabileceğinizi açıklar
services: virtual-machines-linux
documentationcenter: ''
author: ningk
manager: jeconnoc
editor: ''
tags: Cloud-Foundry
ms.assetid: 00c76c49-3738-494b-b70d-344d8efc0853
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/11/2018
ms.author: ningk
ms.openlocfilehash: 7cbffdd40e574c7e906a9388b70ca9d32fd84649
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60198974"
---
# <a name="integrate-cloud-foundry-with-azure"></a>Cloud Foundry ile Azure’ı tümleştirme

[Cloud Foundry](https://docs.cloudfoundry.org/) , bulut sağlayıcıları Iaas platformu üzerinde çalışan bir PaaS platformudur. Bulut sağlayıcıları arasında tutarlı uygulama dağıtım deneyimi sunar. Ayrıca ölçeklenebilirliği, çeşitli Azure Hizmetleri, kurumsal sınıf, HA ile tümleştirin ve maliyet tasarrufu.
Vardır [Cloud Foundry 6 alt](https://docs.cloudfoundry.org/concepts/architecture/), olabilecek esnek ölçek çevrimiçi olarak da dahil olmak üzere: Yönlendirme, Mesajlaşma ve izleme kimlik doğrulaması, uygulama yaşam döngüsü yönetimi, hizmet yönetimi. Her alt sistemler için Cloud Foundry, Azure hizmet correspondent kullanmak için yapılandırabilirsiniz. 

![Azure tümleştirme mimarisi üzerinde cloud Foundry](media/CFOnAzureEcosystem-colored.png)

## <a name="1-high-availability-and-scalability"></a>1. Yüksek kullanılabilirlik ve ölçeklenebilirlik
### <a name="managed-disk"></a>Yönetilen Disk
Disk oluşturma ve silme yordamları için bosh Azure CPI (bulut Sağlayıcısı Arabirimi) kullanır. Varsayılan olarak, yönetilmeyen diskler olarak kullanılırlar. Müşteri el ile depolama hesapları oluşturmanız ve ardından hesapları CF bildirim dosyalarında yapılandırmanız gerekir. Depolama hesabı başına disk sayısı sınırlaması nedeniyle budur.
Artık [yönetilen Disk](https://azure.microsoft.com/services/managed-disks/) kullanılabilir, sanal makine için yönetilen disk güvenli ve güvenilir depolama sunar. Müşteri artık ihtiyacınız ölçeklendirme ve yüksek kullanılabilirlik için depolama hesabı ile dağıtılacak. Azure diskleri otomatik olarak düzenler. Yeni veya mevcut bir dağıtımı olsun, Azure CPI oluşturma veya CF dağıtımı sırasında yönetilen diskin geçiş işler. PCF 1.11 ile desteklenir. Açık kaynaklı Cloud Foundry da keşfedebilirsiniz [yönetilen Disk Kılavuzu](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/tree/master/docs/advanced/managed-disks) başvuru. 
### <a name="availability-zone-"></a>Kullanılabilirlik alanı *
Bulutta yerel uygulama platformu, Cloud Foundry ile tasarlanmış [dört yüksek kullanılabilirlik düzeyini](https://docs.pivotal.io/pivotalcf/2-1/concepts/high-availability.html). Yazılım hataları ilk üç düzeyde CF sistem tarafından işlenebilen ancak platform hata toleransı bulut sağlayıcıları tarafından sağlanır. Anahtar CF bileşenleri HA çözüm bir bulut sağlayıcısının platformuyla korunmalıdır. Bu GoRouters Diego Brains, CF veritabanını ve hizmet kutucuklar içerir. Varsayılan olarak, [Azure kullanılabilirlik kümesine](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/tree/master/docs/advanced/deploy-cloudfoundry-with-availability-sets) bir veri merkezi içinde kümeler arasında hataya dayanıklılık için kullanılır.
Yeni iyi olan [Azure kullanılabilirlik alanı](https://docs.microsoft.com/azure/availability-zones/az-overview ) artık, veri merkezleri arasında düşük gecikme süresi artıklık ile ileri düzeyde hata toleransı getirme yayımlanmıştır.
Azure kullanılabilirlik alanı, bir VM kümesi 2 + veri merkezlerine yerleştirerek HA elde eder, her VM kümesi diğer ayarlar için yedekli. Diğer kümeler olağanüstü durumdan yalıtılmış, bir bölgeye kapalı ise, hala etkin.
> [!NOTE] 
> Azure kullanılabilirlik alanı yok sunulur tüm bölgeler için henüz, en son kontrol [desteklenen bölgelerin listesi için Duyurunun](https://docs.microsoft.com/azure/availability-zones/az-overview). Açık kaynak Cloud Foundry için denetleme [açık kaynaklı Cloud Foundry kılavuz için Azure kullanılabilirlik alanı](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/tree/master/docs/advanced/availability-zone).

## <a name="2-network-routing"></a>2. Ağ yönlendirme
Varsayılan olarak, Azure temel yük dengeleyici için Gorouters iletmeden gelen CF API/apps istekler için kullanılır. CF bileşenleri Diego beyin, MySQL, gibi ERT yük dengeleyici için HA trafiği dengelemek için de kullanabilirsiniz. Azure ayrıca tam olarak yönetilen Yük Dengeleme çözümleri sunmaktadır. TLS sonlandırma ("SSL yük boşaltma") veya HTTP/HTTPS isteği uygulama katmanı işleme başına arıyorsanız, Application Gateway göz önünde bulundurun. Yüksek kullanılabilirlik ve ölçeklenebilirlik Yük Dengeleme katmanda 4 için standart yük dengeleyici göz önünde bulundurun.
### <a name="azure-application-gateway-"></a>Azure uygulama ağ geçidi *
[Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction) çeşitli 7. Katman Yük Dengeleme özellikleri, SSL yük boşaltma, uçtan uca SSL, Web uygulaması güvenlik duvarı, tanımlama bilgilerine dayalı oturum benzeşimi ve daha fazlası dahil olmak üzere sunar. Yapabilecekleriniz [açık kaynak Cloud Foundry uygulama ağ geçidi yapılandırma](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/tree/master/docs/advanced/application-gateway). PCF için denetleme [PCF 2.1 sürüm notları](https://docs.pivotal.io/pivotalcf/2-1/pcf-release-notes/opsmanager-rn.html#azure-application-gateway) POC test için.

### <a name="azure-standard-load-balancer-"></a>Azure standart Load Balancer *
Azure Load Balancer bir katman 4 yük dengeleyicidir. Yük dengeli bir kümedeki hizmetlerin örnekleri arasında trafiği dağıtmak için kullanılır. Standart sürüm sağlar [Gelişmiş Özellikler](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview) üzerinde temel sürüm. Örneğin, 1. Arka uç havuzu maksimum sınırı 100'den 1000 VM oluşturulur.  2. Uç noktaları yerine tek bir kullanılabilirlik kümesinde birden çok kullanılabilirlik kümeleri artık desteklenmektedir.  3. HA bağlantı noktaları, daha zengin bir izleme verilerini ve benzeri gibi ek özellikler. Standart load balancer için Azure kullanılabilirlik alanı taşıyorsanız gereklidir. Yeni bir dağıtım için Azure Standard Load Balancer ile başlatmak için önerilir. 

## <a name="3-authentication"></a>3. Kimlik Doğrulaması 
[Cloud Foundry kullanıcı hesabı ve kimlik doğrulaması](https://docs.cloudfoundry.org/concepts/architecture/uaa.html) CF ve çeşitli bileşenleri için merkezi kimlik yönetimi hizmetidir. [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) Microsoft'un çok kiracılı, bulut tabanlı dizin ve kimlik yönetimi hizmetidir. Varsayılan olarak, UAA Cloud Foundry kimlik doğrulaması için kullanılır. UAA, Gelişmiş bir seçenek olarak, bir dış kullanıcı deposu olarak Azure AD'ye de destekler. Azure AD kullanıcıları, Cloud Foundry Cloud Foundry hesabı olmadan LDAP kimliklerini kullanarak erişebilirsiniz. Bu adımları [PCF UAA için Azure AD yapılandırma](https://docs.pivotal.io/p-identity/1-6/azure/index.html).

## <a name="4-data-storage-for-cloud-foundry-runtime-system"></a>4. Cloud Foundry çalışma zamanı sistemi için veri depolama
Cloud Foundry, Azure blobdeposu veya uygulamanın çalışma zamanı sistemi depolanması için Azure MySQL/PostgreSQL Hizmetleri harika genişletilebilirlik sunar.
### <a name="azure-blobstore-for-cloud-foundry-cloud-controller-blobstore"></a>Cloud Foundry Bulutu denetleyicisi blobdeposu için Azure Blobdeposu
Bulutu denetleyicisi blobdeposu buildpacks, damlacıklar, paketleri ve kaynak havuzları için kritik veri deposudur. Varsayılan olarak, NFS sunucusu için bulut denetleyici blobdeposu kullanılır. Tek hata noktasını önlemek için dış depolama alanı olarak Azure Blob Depolama kullanın. Kullanıma [Cloud Foundry belgeleri](https://docs.cloudfoundry.org/deploying/common/cc-blobstore-config.html) arka plan için ve [Pivotal Cloud Foundry seçeneklerinde](https://docs.pivotal.io/pivotalcf/2-0/customizing/azure.html).

### <a name="mysqlpostgresql-as-cloud-foundry-elastic-run-time-database-"></a>MySQL/PostgreSQL Cloud Foundry elastik olarak çalıştırma zamanı veritabanı *
CF esnek çalışma zamanı iki önemli sistem veritabanı gerektirir:
#### <a name="ccdb"></a>CCDB 
Bulutu denetleyicisi veritabanı.  Bulut denetleyicisi sisteme erişmek istemciler için REST API uç noktaları sağlar. Kuruluşlar, alanları, hizmetler, kullanıcı rolleri için ve Bulutu denetleyicisi için daha fazla tablo CCDB depolar.
#### <a name="uaadb"></a>UAADB 
Veritabanı kullanıcı hesabı ve kimlik doğrulaması için. İlişkili kullanıcı kimlik doğrulaması depoladığı verilerin, örneğin şifrelenmiş kullanıcı adlarını ve parolaları.

Varsayılan olarak, yerel sistem veritabanı (MySQL) kullanılabilir. Yüksek kullanılabilirlik ve ölçek, yararlanarak Azure yönetilen MySQL veya PostgreSQL Hizmetleri. İşte yönergesinin [CCDB UAADB ve açık kaynak Cloud Foundry ile diğer sistem veritabanları için Azure MySQL/PostgreSQL etkinleştirme](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/tree/master/docs/advanced/configure-cf-external-databases-using-azure-mysql-postgres-service).

## <a name="5-open-service-broker"></a>5. Açık hizmet Aracısı
Azure hizmet Aracısı, Azure hizmetlerine uygulama erişimini yönetmek için tutarlı bir arabirim sunar. Yeni [Azure projesi için açık hizmet Aracısı](https://github.com/Azure/open-service-broker-azure) Cloud Foundry, OpenShift ve Kubernetes üzerinde uygulamaların Hizmetleri sunmak için tek ve kolay bir yol sağlar. Bkz: [PCF kutucuk için açık hizmet Aracısı Azure](https://network.pivotal.io/products/azure-open-service-broker-pcf/) PCF dağıtım yönergeleri için.

## <a name="6-metrics-and-logging"></a>6. Ölçümler ve günlüğe kaydetme
Azure Log Analytics Nozzle ölçümleri ileten bir Cloud Foundry bileşenidir [Cloud Foundry loggregator firehose](https://docs.cloudfoundry.org/loggregator/architecture.html) için [Azure İzleyicisi](https://azure.microsoft.com/services/log-analytics/). Nozzle ile toplama, görüntüleyebilir ve birden çok dağıtımlarınızda CF sistem durumu ve performans ölçümlerini analiz edin.
Tıklayın [burada](https://docs.microsoft.com/azure/cloudfoundry/cloudfoundry-oms-nozzle) İzleyici açık kaynak ve Pivotal Cloud Foundry ortamı hem Azure Log Analytics Nozzle dağıtma ve Azure'dan ardından verilere hakkında bilgi edinmek için konsol günlüğe kaydeder. 
> [!NOTE]
> PCF 2.0 BOSH VM'ler için sistem durumu ölçümleri Loggregator Firehose için varsayılan olarak iletilir ve Azure İzleyici günlüklerine konsoluna tümleştirilmiştir.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="7-cost-saving"></a>7. Maliyet tasarrufu
### <a name="cost-saving-for-devtest-environments"></a>Maliyet tasarrufu için geliştirme ve Test ortamları
#### <a name="b-series-"></a>B serisi: *
F ve D VM serisi Pivotal Cloud Foundry üretim ortamı için yaygın olarak önerilen durumdayken yeni "seri aktarıma uygun" [B serisi](https://azure.microsoft.com/blog/introducing-b-series-our-new-burstable-vm-size/) yeni seçenekler sunar. B serisi seri aktarıma uygun VM'ler, CPU'nun tam performansta küçük veritabanları ve geliştirme gibi web sunucuları gibi sürekli olarak ihtiyacınız yoksa ve test ortamları iş yükleri için idealdir. Bu iş yükleri genellikle seri aktarıma uygun performans gereksinimleri vardır. $0,012 / $ 0,05/saat (F1) kıyasla saat (B1), tam listesini görmek [VM boyutları](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-general) ve [fiyatları](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) Ayrıntılar için. 
#### <a name="managed-standard-disk"></a>Standart yönetilen Disk: 
Premium diskler üretimde güvenilir performansı için önerilen.  İle [yönetilen Disk](https://azure.microsoft.com/services/managed-disks/), standart depolama da benzer bir güvenilirlik, farklı bir performans sunun. Standart yönetilen diskler, performans açısından duyarlı olmayan iş yükünü, geliştirme ve Test ya da kritik olmayan bir ortam gibi daha düşük maliyetle alternatif bir seçenek sunar.  
### <a name="cost-saving-in-general"></a>Maliyet tasarrufu genel 
#### <a name="significant-vm-cost-saving-with-azure-reservations"></a>Önemli VM maliyeti kaydetme Azure ayırmaları ile: 
Bugün, tüm CF VM'lerin "isteğe bağlı" fiyatlandırması, ortamları genellikle süresiz olarak olmanıza rağmen kullanılarak ayrıca faturalandırılır. Şimdi yedek 1 veya 3 yıllık bir terim üzerinde VM kapasite ve 65 45 oranında indirim elde edebilirsiniz. Ortamınızda değişiklik yapmadan faturalandırma sisteminde indirimi uygulanmaz. Ayrıntılar için bkz [Azure ayırmaları works](https://azure.microsoft.com/pricing/reserved-vm-instances/). 
#### <a name="managed-premium-disk-with-smaller-sizes"></a>Premium Disk küçük boyutları ile yönetilen: 
Diskleri destek küçük disk boyutları P4(32 GB) ve P6(64 GB) hem premium hem de standart diskler için yönetilen. Küçük iş yükleriniz varsa, standart premium disklerden premium yönetilen disklere geçirirken maliyet tasarruf sağlayabilirsiniz.
#### <a name="use-azure-first-party-services"></a>Azure birinci taraf hizmetleri kullanın: 
Azure'un birinci taraf hizmetidir yararlanarak, maliyet, yüksek kullanılabilirlik ve güvenilirlik bölümler belirtilen yanı sıra uzun vadeli yönetim düşürecektir. 

Pivotal başlatılan bir [küçük ayak izine ERT](https://docs.pivotal.io/pivotalcf/2-0/customizing/small-footprint.html) PCF müşteriler için bileşenler 4 Vm'leri çalıştıran 2500 uygulama örnekleri, birlikte bulunur. Deneme sürümü ile kullanıma sunuldu [Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/apps/pivotal.pivotal-cloud-foundry).

## <a name="next-steps"></a>Sonraki Adımlar
Azure tümleştirme özellikleri bulunan ilk [açık kaynak Cloud Foundry](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/tree/master/docs/advanced/), Pivotal Cloud Foundry üzerinde kullanılabilir olmadan önce. Özellikleri ile işaretlenen * PCF yine de kullanılamıyor. Azure Stack ile cloud Foundry tümleştirme ya da bu belgenin kapsamında değildir.
PCF destek özellikleri ile işaretlenen için *, veya Azure Stack ile Cloud Foundry tümleştirme için en son durumu Pivotal ve Microsoft hesap yöneticinize başvurun. 

