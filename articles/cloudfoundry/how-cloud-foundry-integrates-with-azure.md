---
title: Bulut Foundry Azure ile nasıl tümleşik çalıştığını | Microsoft Docs
description: Enterprice deneyimini geliştirmek için bulut Foundry can utlize Azure hizmetleri nasıl açıklar
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
ms.openlocfilehash: 0959617185694d48c593996d5cd8c836098dd1cd
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37062215"
---
# <a name="integrate-cloud-foundry-with-azure"></a>Cloud Foundry ile Azure’ı tümleştirme

[Bulut Foundry](https://docs.cloudfoundry.org/) bulut sağlayıcılarının Iaas platformu üzerinde çalışan bir PaaS platformudur. Bulut sağlayıcıları arasında tutarlı uygulama dağıtım deneyimi sunar. Ayrıca, bu ayrıca ile Kurumsal düzeyde HA, çeşitli Azure hizmetleriyle ölçeklenebilirlik, tümleştirme ve maliyet tasarrufu.
Vardır [bulut Foundry 6 alt](https://docs.cloudfoundry.org/concepts/architecture/), olabilir esnek ölçek çevrimiçi dahil olmak üzere: yönlendirme, Mesajlaşma ve izleme kimlik doğrulaması, uygulama yaşam döngüsü yönetimi, hizmet yönetimi. Her alt sistemleri için bulut correspondent Azure hizmeti kullanmaya Foundry yapılandırabilirsiniz. 

![Bulut Foundry Azure tümleştirme mimarisi hakkında](media/CFOnAzureEcosystem-colored.png)

## <a name="1-high-availability-and-scalability"></a>1. Yüksek kullanılabilirlik ve ölçeklenebilirlik
### <a name="managed-disk"></a>Yönetilen Disk
Bosh disk oluşturma ve silme yordamları için Azure CPI (bulut Sağlayıcısı Arabirimi) kullanır. Varsayılan olarak, yönetilmeyen diskler kullanılır. El ile depolama hesapları oluşturun, sonra CF bildirim dosyalarında hesaplarını yapılandırmak müşteri gerektirir. Depolama hesabı başına disk sayısını sınırlama nedeniyle budur.
Şimdi [yönetilen Disk](https://azure.microsoft.com/services/managed-disks/) kullanılabilir, sanal makineler için yönetilen güvenli ve güvenilir disk depolama alanı sunar. Müşteri artık ihtiyaç ölçek ve HA için depolama hesabı uğraşmanız. Azure diskleri otomatik olarak düzenler. Yeni veya var olan bir dağıtıma olup olmadığını belirtir, Azure CPI oluşturma veya yönetilen disk geçiş CF dağıtımı sırasında işleyecek. PCF 1.11 ile desteklenir. Açık kaynak bulut Foundry da gözden geçirebilirsiniz [yönetilen Disk Kılavuzu](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/tree/master/docs/advanced/managed-disks) başvuru. 
### <a name="availability-zone-"></a>Kullanılabilirlik bölge *
Bulut Foundry tasarlanmıştır bulut yerel uygulama platformu [dört düzeyde yüksek kullanılabilirlik](https://docs.pivotal.io/pivotalcf/2-1/concepts/high-availability.html). Yazılım hataları ilk üç düzeyde CF sistem tarafından işlenebilir olsa da, platform hata toleransı bulut sağlayıcıları tarafından sağlanır. Anahtar CF bileşenleri HA çözüm bulut sağlayıcısının platformuyla korunmalıdır. Bu GoRouters, Diego Brains, CF veritabanını ve hizmet döşeme içerir. Varsayılan olarak, [Azure kullanılabilirlik kümesi](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/tree/master/docs/advanced/deploy-cloudfoundry-with-availability-sets) bir veri merkezinde kümeler arasında hataya dayanıklılık için kullanılır.
Yeni iyi olduğu, [Azure kullanılabilirlik bölge](https://docs.microsoft.com/azure/availability-zones/az-overview ) artık, veri merkezleri arasında düşük gecikme süresi artıklık ile ileri düzeyde hata toleransı getiren yayımlanmıştır.
2 + veri merkezleri halinde VM'ler kümesi koyarak HA Azure kullanılabilirlik bölge erişir, VM'ler her kümesi diğer ayarlar için yedek. Bir bölge kapalı ise, diğer olağanüstü durumdan yalıtılmış hala canlı kümeleridir.
> [!NOTE] 
> Azure kullanılabilirlik bölge değil sunulur tüm bölgelere henüz, en son denetleme [desteklenen bölgelerin listesi duyuru](https://docs.microsoft.com/azure/availability-zones/az-overview). Kaynak bulut Foundry açmak için kontrol [Azure kullanılabilirlik açık kaynak bulut Foundry Kılavuzu bölgenin](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/tree/master/docs/advanced/availability-zone).

## <a name="2-network-routing"></a>2. Ağ yönlendirme
Varsayılan olarak, Azure temel yük dengeleyici Gorouters iletme gelen CF API/uygulamaları istekleri için kullanılır. Diego beyin, MySQL, gibi CF Bileşenleri Ekle yük dengeleyici için HA trafiği dengelemek için de kullanabilirsiniz. Ayrıca, Azure tam olarak yönetilen Yük Dengeleme çözümlerinde kümesi sağlar. TLS sonlandırma ("SSL boşaltma") veya HTTP/HTTPS isteği uygulama katmanı işleme başına arıyorsanız, uygulama ağ geçidi göz önünde bulundurun. Yüksek kullanılabilirlik ve ölçeklenebilirlik Yük Dengeleme katman 4 için standart yük dengeleyiciye göz önünde bulundurun.
### <a name="azure-application-gateway-"></a>Azure uygulama ağ geçidi *
[Azure uygulama ağ geçidi](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction) SSL boşaltma, uçtan uca SSL, Web uygulaması güvenlik duvarı, tanımlama bilgisi tabanlı oturum benzeşimi ve daha fazlası dahil olmak üzere, çeşitli katman 7 Yük Dengeleme sağlar. Yapabilecekleriniz [açık kaynak bulut Foundry uygulama ağ geçidi yapılandırma](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/tree/master/docs/advanced/application-gateway). PCF için denetleme [PCF 2.1 sürüm notları](https://docs.pivotal.io/pivotalcf/2-1/pcf-release-notes/opsmanager-rn.html#azure-application-gateway) POC test için.

### <a name="azure-standard-load-balancer-"></a>Azure standart yük dengeleyici *
Azure yük dengeleyici katman 4 yük dengeleyici ' dir. Yük dengeli kümesi Hizmetleri'nde örnekleri arasında trafiği dağıtmak için kullanılır. Standart sürüm sağlar [Gelişmiş Özellikler](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview) temel sürüm üstünde. Örneğin, 1. Arka uç havuzu maksimum sınırı 1000 VM'ler için 100'den tetiklenir.  2. Uç noktaları artık tek kullanılabilirlik kümesi yerine birden çok kullanılabilirlik kümelerini destekler.  3. HA bağlantı noktaları, daha zengin izleme verilerini, vb. gibi ek özellikler. Azure kullanılabilirlik bölgesine taşıyorsanız, standart yük dengeleyici gereklidir. Yeni bir dağıtım için Azure standart yük dengeleyici ile başlatmanızı öneririz. 

## <a name="3-authentication"></a>3. Kimlik Doğrulaması 
[Foundry kullanıcı hesabı ve kimlik doğrulama bulut](https://docs.cloudfoundry.org/concepts/architecture/uaa.html) CF ve çeşitli bileşenleri için merkezi kimlik yönetimi hizmetidir. [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) Microsoft'un çok kiracılı, bulut tabanlı dizin ve kimlik yönetimi hizmetidir. Varsayılan olarak, UAA bulut Foundry kimlik doğrulaması için kullanılır. UAA, Gelişmiş bir seçenek olarak, bir dış kullanıcı deposu olarak Azure AD ile de destekler. Azure AD kullanıcıları Bulut Bulut Foundry hesabı olmadan LDAP kimliklerini kullanarak Foundry erişebilirsiniz. Aşağıdaki adımları izleyin [PCF UAA için Azure AD yapılandırma](http://docs.pivotal.io/p-identity/1-6/azure/index.html).

## <a name="4-data-storage-for-cloud-foundry-runtime-system"></a>4. Bulut Foundry çalışma zamanı sistemi için veri depolama
Bulut Foundry Azure blobdeposu veya Azure MySQL/PostgreSQL Hizmetleri uygulamanın çalışma zamanı sistem depolanması için harika genişletilebilirlik sağlar.
### <a name="azure-blobstore-for-cloud-foundry-cloud-controller-blobstore"></a>Bulut Foundry bulut denetleyicisi blobdeposu için Azure Blobdeposu
Bulut denetleyicisi blobdeposu buildpacks, damlacıklar, paketleri ve kaynak havuzları için kritik veri deposudur. Varsayılan olarak, NFS sunucusu için bulut denetleyici blobdeposu kullanılır. Tek hata noktası önlemek için dış deposu olarak Azure Blob Storage'ı kullanın. Kullanıma [bulut Foundry belgelerine](https://docs.cloudfoundry.org/deploying/common/cc-blobstore-config.html) arka plan için ve [Bileşendirler bulut Foundry seçeneklerinde](https://docs.pivotal.io/pivotalcf/2-0/customizing/azure.html).

### <a name="mysqlpostgresql-as-cloud-foundry-elastic-run-time-database-"></a>MySQL/PostgreSQL bulut Foundry esnek olarak çalıştırma zamanı veritabanı *
CF esnek çalışma zamanı iki önemli sistem veritabanları gerektirir:
#### <a name="ccdb"></a>CCDB 
Bulut denetleyicisi veritabanı.  Bulut denetleyicisi sisteme erişmek istemciler için REST API uç noktaları sağlar. CCDB tabloları düzenlemeler, boşluk, hizmetler, kullanıcı rolleri için ve daha fazla bulut denetleyicisi için depolar.
#### <a name="uaadb"></a>UAADB 
Kullanıcı hesabı ve kimlik doğrulama için veritabanı. İlgili kullanıcı kimlik doğrulaması depolar, örneğin şifreli veriler kullanıcı adları ve parolalar.

Varsayılan olarak, yerel sistem veritabanı (MySQL) kullanılabilir. HA ve ölçek, Dengeleme Azure yönetilen MySQL veya PostgreSQL Hizmetleri için. Yönergesi işte [CCDB, UAADB ve diğer sistem veritabanlarının açık kaynak bulut Foundry ile Azure MySQL/PostgreSQL etkinleştirme](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/tree/master/docs/advanced/configure-cf-external-databases-using-azure-mysql-postgres-service).

## <a name="5-open-service-broker"></a>5. Açık hizmet Aracısı
Azure hizmet Aracısı Azure hizmetlerine uygulama erişimini yönetmek için tutarlı arabirimi sunar. Yeni [Azure projesi için açık hizmet Aracısı](https://github.com/Azure/open-service-broker-azure) bulut Foundry, OpenShift ve Kubernetes uygulamaları hizmetleri sunmak için tek ve basit bir yol sağlar. Bkz: [Azure açık hizmet Aracısı PCF döşemeye](https://network.pivotal.io/products/azure-open-service-broker-pcf/) PCF dağıtım yönergeleri için.

## <a name="6-metrics-and-logging"></a>6. Ölçümleri ve günlüğe kaydetme
Azure günlük analizi kafa ölçümleri ileten bir bulut Foundry bileşenidir [bulut Foundry loggregator firehose](https://docs.cloudfoundry.org/loggregator/architecture.html) için [Azure günlük analizi](https://azure.microsoft.com/services/log-analytics/). Başlık ile toplamak, görüntülemek ve birden çok dağıtımlar arasında CF sistem sağlık ve performans ölçümleri analiz edin.
Tıklatın [burada](https://docs.microsoft.com/azure/cloudfoundry/cloudfoundry-oms-nozzle) Azure günlük analizi kafa hem açık kaynak hem de Bileşendirler bulut Foundry ortam, Azure günlük analizi OMS konsolundan ardından verilere nasıl dağıtıp öğrenin. 
> [!NOTE]
> PCF 2.0 BOSH sistem durumu ölçümleri VM'ler için Loggregator Firehose varsayılan olarak iletilir ve Azure günlük analizi OMS konsoluna tümleşiktir.

## <a name="7-cost-saving"></a>7. Maliyet tasarrufu
### <a name="cost-saving-for-devtest-environments"></a>Geliştirme ve Test ortamları için kaydetme maliyet
#### <a name="b-series-"></a>B-serisi: *
F-D VM seri sık Bileşendirler bulut Foundry üretim ortamı için önerilen ederken yeni "burstable" [B-serisi](https://azure.microsoft.com/blog/introducing-b-series-our-new-burstable-vm-size/) yeni seçenekler getirir. B-serisi burstable VM'ler CPU tam performansını sürekli olarak, web sunucuları gibi küçük veritabanları ve geliştirme gerekir ve ortamlarında test iş yükleri için idealdir. Bu iş yükleri genellikle burstable performans gereksinimleri vardır. $0,012 $ 0,05/saat (F1) kıyasla saate (B1), tam listesini görmek [VM boyutları](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/sizes-general) ve [fiyatları](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) Ayrıntılar için. 
#### <a name="managed-standard-disk"></a>Standart yönetilen Disk: 
Premium diskleri üretimde güvenilir performans için önerilen.  İle [yönetilen Disk](https://azure.microsoft.com/services/managed-disks/), standart depolama da benzer bir güvenilirlik, farklı performans teslim etmek. Performans duyarlı değil iş yükü için geliştirme ve Test veya kritik olmayan ortamı gibi yönetilen standart diskler daha düşük maliyetli alternatif bir seçenek sunar.  
### <a name="cost-saving-in-general"></a>Maliyet tasarrufu genel 
#### <a name="significant-vm-cost-saving-with-reserved-instances"></a>Önemli VM maliyet kaydetme ile ayrılmış örnekler: 
Bugün tüm CF VM'ler "isteğe bağlı" fiyatlandırma, ortamlar genellikle süresiz olarak kalması olsa bile kullanarak faturalandırılır. Şimdi yedek 1 ya da 3 yıllık dönem üzerinde VM kapasite ve indirim 45 %65 geçirmesine. Fatura sistemiyle ortamınızı herhangi bir değişiklik indirim uygulanır. Ayrıntılar için bkz [nasıl ayrılmış örnekleri works](https://azure.microsoft.com/pricing/reserved-vm-instances/). 
#### <a name="managed-premium-disk-with-smaller-sizes"></a>Premium Disk küçük boyutlarıyla yönetilen: 
Diskleri destek daha küçük disk boyutlarına, örneğin P4(32 GB) ve P6(64 GB) premium ve standart diskler için yönetilen. Küçük iş yükleri varsa, standart premium diskleri yönetilen premium diskleri geçirilirken maliyet kaydedebilirsiniz.
#### <a name="utilizing-azure-first-party-services"></a>Azure birinci taraf hizmetleri kullanan: 
Azure'nın birinci taraf hizmeti yararlanarak HA ve güvenilirlik bölümler belirtilen ek maliyet uzun vadeli yönetim düşürülür. 

Bileşendirler başlatılan bir [küçük ayak izini Ekle](https://docs.pivotal.io/pivotalcf/2-0/customizing/small-footprint.html) PCF müşteriler için bileşenler 2500 uygulama örnekleri çalıştıran yalnızca 4 VM'ler birlikte bulunur. Deneme sürümü ile kullanıma sunulmuştur [Azure markette](https://azuremarketplace.microsoft.com/marketplace/apps/pivotal.pivotal-cloud-foundry).

## <a name="next-steps"></a>Sonraki Adımlar
Azure tümleştirme özellikleri bulunan ilk [açık kaynak bulut Foundry](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/tree/master/docs/advanced/), Bileşendirler bulut Foundry üzerinde kullanılabilir olmadan önce. Özellikleri ile işaretlenen * PCF hala kullanılamıyor. Azure yığını ile bulut Foundry tümleştirme ya da bu belgede için kapsamında değildir.
PCF destek özellikleri işaretlenmiş için *, veya Azure yığını ile bulut Foundry tümleştirme için en son durumunu Pivotal ve Microsoft hesap yöneticinize başvurun. 

