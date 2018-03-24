---
title: Azure Site Recovery Azure çoğaltma mimarisinde VMware | Microsoft Docs
description: Bu makalede, bileşenleri ve şirket içi VMware sanal makineleri Azure Site Recovery ile azure'a çoğaltırken kullanılan mimariye genel bakış sağlar
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 03/19/2018
ms.author: raynew
ms.openlocfilehash: c1aa89f14edab7d0e560c20d6bc48480aff1631f
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="vmware-to-azure-replication-architecture"></a>VMware Azure çoğaltma mimarisi

Bu mimari ve işlemlerdeki çoğaltma, yük devri ve VMware sanal makineleri (VM'ler) Kurtarma sırasında kullanılan bir şirket içi VMware site ile Azure arasında kullanarak makalede [Azure Site Recovery](site-recovery-overview.md).


## <a name="architectural-components"></a>Mimari bileşenler

Aşağıdaki tablo ve grafik VMware çoğaltma Azure için kullanılan bileşenler üst düzey bir görünümünü sağlar.

**Bileşen** | **Gereksinim** | **Ayrıntılar**
--- | --- | ---
**Azure** | Bir Azure aboneliği, Azure Storage hesabını ve Azure ağı. | Şirket içi Vm'lerden gelen çoğaltılan veriler depolama hesabında depolanır. Şirket içinden Azure'a yük devretme çalıştırdığınızda azure VM'ler ile çoğaltılan veriler oluşturulur. Azure VM’leri oluşturulduğunda Azure sanal ağına bağlanır.
**Yapılandırma sunucusu makine** | Tek bir makine şirket içi. Bunu dağıtılabilir bir VMware VM indirilen OVF şablondan çalıştırmanızı öneririz.<br/><br/> Makine yapılandırma sunucusuna işlem sunucusu ve ana hedef sunucusu dahil tüm şirket içi Site Recovery bileşenlerini çalışır. | **Yapılandırma sunucusu**: şirket içi ve Azure arasındaki iletişimi düzenler ve veri çoğaltma yönetir.<br/><br/> **İşlem sunucusu**: yapılandırma sunucusundaki varsayılan olarak yüklüdür. Çoğaltma verileri alan; önbelleğe alma, sıkıştırma ve şifreleme iyileştirir; ve Azure depolama alanına gönderir. İşlem sunucusu çoğaltmak istediğiniz sanal makinelerin de Azure Site Recovery Mobility hizmeti yükler ve şirket içi makinelerin otomatik bulma işlemini gerçekleştirir. Dağıtımınız büyüdükçe, daha büyük birimleri çoğaltması trafiğini işlemek için ek, ayrı bir işlem sunucuları ekleyebilirsiniz.<br/><br/> **Ana hedef sunucusu**: yapılandırma sunucusundaki varsayılan olarak yüklüdür. Azure'dan yeniden çalışma sırasında çoğaltma verilerini işler. Büyük dağıtımlar için yeniden çalışma için bir ek, ayrı ana hedef sunucusu ekleyebilirsiniz.
**VMware sunucuları** | VMware sanal makineleri şirket içi vSphere ESXi sunucularda barındırılır. Ana bilgisayarları yönetmek için bir vCenter sunucusu öneririz. | Site Recovery dağıtımı sırasında VMware sunucularını kurtarma Hizmetleri Kasası'na ekleyin.
**Çoğaltılan makineler** | Mobility hizmeti, çoğaltılan her VMware VM yüklenir. | İşlem sunucusundan otomatik yüklemesine izin ver öneririz. Alternatif olarak, hizmeti el ile yüklemek veya bir otomatik dağıtım yöntemi, System Center Configuration Manager gibi kullanın.

**VMware-Azure arası mimari**

![Bileşenler](./media/vmware-azure-architecture/arch-enhanced.png)

## <a name="configuration-steps"></a>Yapılandırma adımları

VMware Azure olağanüstü durum kurtarma veya geçiş için geniş adımlar aşağıdaki gibidir:

1. **Azure bileşenleri ayarlamak**. Doğru izinlere sahip bir Azure hesabı, bir Azure depolama hesabı, bir Azure sanal ağı ve bir kurtarma Hizmetleri kasası gerekir. [Daha fazla bilgi edinin](tutorial-prepare-azure.md).
2. **Şirket içi yedekleme kümesi**. Site Recovery çoğaltmak için sanal makineleri otomatik olarak bulabilir bunlar VMware sunucusunda bir hesap ayarlama Mobility hizmeti bileşeninin çoğaltmak istediğiniz sanal makinelerin yüklemek için kullanılan bir hesabı kurma ve doğrulama içermesi bu VMware sunucuları ve sanal makineleri önkoşulları ile uyumlu. Ayrıca isteğe bağlı olarak, yük devretme sonrasında bu Azure Vm'lerine bağlanmak hazırlayabilirsiniz. Site Recovery, bir Azure depolama hesabı VM verilerini çoğaltır ve Azure Azure'a yük devretme çalıştırdığınızda verileri kullanarak VM'ler oluşturur. [Daha fazla bilgi edinin](vmware-azure-tutorial-prepare-on-premises.md).
3. **Çoğaltmayı ayarlama**. Konumu seçin, çoğaltmak istediğiniz. Kaynak çoğaltma ortamı ayarlayarak tek şirket içi VMware tüm şirket içi gerek duyduğunuz Site Recovery bileşenlerini çalıştıran VM (yapılandırma sunucusu) yapılandırın. Kurulum sonrasında kurtarma Hizmetleri kasasına yapılandırma sunucu makinesi kaydedin. Ardından, hedef ayarlarını seçin. [Daha fazla bilgi edinin](vmware-azure-tutorial.md).
4. **Bir çoğaltma ilkesi oluşturmak**. Çoğaltma nasıl olması gerektiğini belirten bir çoğaltma ilkesi oluşturun. 
    - **RPO eşiği**: Bu çoğaltma belirtilen süre içinde gerçekleşmezse, durumları olan izleme ayarı, bir uyarı (ve isteğe bağlı olarak bir e-posta) verilir. Örneğin, RPO eşik 30 dakikaya ayarlayın ve bir sorun oluşmasını 30 dakika Çoğaltmada engeller, bir olay oluşturulur. Bu ayar, çoğaltma etkilemez. Sürekli çoğaltma ve kurtarma noktaları birkaç dakikada oluşturulur
    - **Bekletme**: kurtarma noktası bekletme belirtir ne kadar süreyle kurtarma noktaları Azure'da saklanması. 0 ve premium depolama için 24 saat arasında bir değer belirtin veya yukarı ile standart depolama için 72 saat. Sıfırdan daha yüksek bir değer ayarlarsanız, en son kurtarma noktası ya da bir saklı noktası yük devretme gerçekleştirebilirsiniz. Kurtarma noktası bekletme penceresi sonra temizlenir.
    - **Kilitlenme tutarlı anlık görüntüleri**: varsayılan olarak, Site Recovery kilitlenme ile tutarlı anlık görüntü alır ve bunları birkaç dakikada kurtarma noktaları oluşturur. Bir kurtarma noktası kilitlenme birbiriyle veri bileşenlerinin tümünü yazma-sırası tutarlı olması durumunda tutarlı, anlık oldukları gibi kurtarma noktası oluşturulmadı. Daha iyi anlamak için bilgisayar sabit diskinizde veri durumunu bir güç kesintisi veya benzer olay sonra düşünün. Kilitlenme tutarlı bir kurtarma noktası, uygulamanızın veri tutarsızlıkları olmadan bir kilitlenme kurtarılır tasarlandıysa genellikle yeterli olur.
    - **Uygulamayla tutarlı anlık görüntüleri**: Bu değer sıfır değilse, dosya sisteminin tutarlı anlık görüntüler ve kurtarma noktaları oluşturmak VM'de çalıştırılan Mobility hizmeti çalışır. İlk çoğaltma tamamlandıktan sonra ilk anlık görüntüsü alınır. Sonra anlık görüntüler, belirttiğiniz sıklığında alınır. Yazma sipariş edilen yanı sıra tutarlı, çalışan uygulamaları kendi işlemlerin tamamlayın ve diske (uygulama sessiz moda) önbelleklerini, bir kurtarma noktası uygulama tutarlı olur. Uygulamayla tutarlı kurtarma noktaları, SQL, Oracle ve Exchange gibi veritabanı uygulamaları için önerilir. Kilitlenme tutarlı bir anlık görüntü yeterli ise, bu değeri 0 olarak ayarlayabilirsiniz.  
    - **Çoklu VM tutarlılığını**: isteğe bağlı olarak bir çoğaltma grubu oluşturabilirsiniz. Ardından, çoğaltma etkinleştirdiğinizde, o gruba VM'ler toplayabilirsiniz. Bir çoğaltma sanal makineleri çoğaltmak gruplamak ve üzerinden başarısız olduğunda kilitlenme tutarlı ve uygulamayla tutarlı kurtarma noktaları paylaşılan. Birden fazla makine arasında toplanması gereken anlık görüntüleri olarak iş yükü performansını etkileyebilecek beri bu seçeneği dikkatlice kullanmanız gerekir. Sanal makineleri aynı iş yükünü ve tutarlı olması gerekiyor çalıştırırsanız yalnızca bunu ve benzer karmaşıklıklarını VM'ye sahip. En fazla 8 sanal makineleri bir gruba ekleyebilirsiniz. 
5. **VM çoğaltmayı etkinleştirme**. Son olarak, şirket içi VMware VM'ler için çoğaltma işlemini etkinleştirir. Mobilite hizmetinin yüklenmesi için bir hesabı oluşturduysanız ve Site Recovery anında yükleme yapmalısınız belirtilen Mobility hizmeti çoğaltmayı etkinleştirme her VM yüklenecek. [Daha fazla bilgi edinin](vmware-azure-tutorial.md#enable-replication). Çoklu VM tutarlılığı için bir çoğaltma grubu oluşturduysanız, sanal makineleri bu gruba ekleyebilirsiniz.
6. **Yük devretme testi**. Her şeyi ayarlandıktan sonra sanal makineleri Azure'a beklendiği gibi yük olduğunu denetlemek için bir test yük devretme yapabilirsiniz. [Daha fazla bilgi edinin](tutorial-dr-drill-azure.md).
7. **Yük devretme**. VM'ler için Azure - yalnızca geçiş, bunu yapmak için tam bir yük devretme çalıştırın. Olağanüstü durum kurtarma ayarlıyorsanız gerektiği gibi bir tam yük devretme çalıştırabilirsiniz. Tam olağanüstü durum kurtarma için Azure, yük devretme işleminden sonra şirket içi siteniz olarak dönün ve kullanılabilir olduğunda başarısız olabilir. [Daha fazla bilgi edinin](vmware-azure-tutorial-failover-failback.md).

## <a name="replication-process"></a>Çoğaltma işlemi

1. Bir sanal makine için çoğaltmayı etkinleştirdiğinizde, çoğaltma ilkesiyle uygun şekilde çoğaltmak başlar. 
2. Trafik, internet üzerinden ortak uç noktaları Azure depolama alanına çoğaltır. Alternatif olarak, Azure ExpressRoute ile kullanabileceğiniz [ortak eşleme](../expressroute/expressroute-circuit-peerings.md#azure-public-peering). Trafik bir siteden siteye sanal özel ağ (VPN) bir şirket içi sitesinden Azure'a çoğaltma desteklenmiyor.
3. Bir başlangıç kopyasını VM verileri Azure depolama alanına çoğaltılır.
4. İlk çoğaltma sonlandırıldıktan sonra Azure delta değişikliklerini başlar. Bir makine için izlenen değişiklikler bir .hrl dosyasında saklanır.
5. İletişim şu şekilde olur:

    - Sanal makineleri şirket içi yapılandırma sunucusu HTTPS 443 numaralı bağlantı noktasında gelen çoğaltma yönetimi için iletişim.
    - Yapılandırma sunucusu Azure ile HTTPS 443 giden bağlantı noktası üzerinden çoğaltma düzenler.
    - VM'ler, gelen çoğaltma verilerini (yapılandırma sunucusu makinesinde çalışan) bağlantı noktası HTTPS 9443 işlem sunucusuna gönderir. Bu bağlantı noktası değiştirilebilir.
    - İşlem sunucusu çoğaltma verileri alan, en iyi duruma getirir ve bunu şifreler ve Azure depolama bağlantı noktası 443 üzerinden giden gönderir.


**Azure çoğaltma işlemi VMware**

![Çoğaltma işlemi](./media/vmware-azure-architecture/v2a-architecture-henry.png)

## <a name="failover-and-failback-process"></a>Yük devretme ve yeniden çalışma işlemi

Gerektiği gibi çoğaltma ayarlayın ve her şeyin beklendiği gibi çalıştığını denetlemek için bir olağanüstü durum kurtarma ayrıntısı seçeneğini (yük devretme testi) çalıştırdıktan sonra Yük devretme ve yeniden çalıştırabilirsiniz.

1. Tek bir makine için başarısız çalıştırmak veya kurtarma aynı anda birden çok VM vermesine planları oluşturun. Tek makine yük devretme yerine bir kurtarma planı avantajı şunlardır:
    - Tüm sanal makineleri uygulama arasında bir tek kurtarma planına dahil ederek uygulama bağımlılıkları model oluşturabilirsiniz.
    - Komut dosyaları, Azure runbook'lar ekleyin ve el ile gerçekleştirilecek eylemler duraklatılamadı.
2. İlk yük devretme tetikleme sonra iş yükü Azure sanal makineden erişme başlatmak üzere yürütün.
3. Birincil şirket içi sitenize yeniden kullanılabilir duruma geldiğinde, geri dönmesini hazırlayabilirsiniz. Yeniden çalışmak için bir yeniden çalışma altyapısını kurma gerekir dahil olmak üzere:

    * **Azure'da geçici işlem sunucusu**: Azure'dan başarısız olmasına, bir Azure VM'yi yedekleme azure'dan çoğaltmayı düzenlemek için bir işlem sunucusu olarak davranacak şekilde ayarlayın. Yeniden çalışma sona erdikten sonra bu VM'yi silebilirsiniz.
    * **VPN bağlantısı**: yeniden çalışmak için bir VPN bağlantısı (veya ExpressRoute) Azure ağından şirket içi siteye gerekir.
    * **Ayrı bir ana hedef sunucusu**: varsayılan olarak, şirket içi VMware VM yapılandırma sunucusunda ile yüklenen ana hedef sunucusu yeniden çalışma işler. Geri büyük miktarda trafik başarısız ihtiyacınız varsa, bu amaçla bir ayrı şirket içi ana hedef sunucusu ayarlayın.
    * **Yeniden çalışma ilkesi**: Şirket içi sitenize geri çoğaltmak için bir yeniden çalışma ilkeniz olmalıdır. Azure için şirket içi çoğaltma ilkesi oluşturduğunuzda bu ilke otomatik olarak oluşturuldu.
4. Bileşenleri yerinde olduktan sonra yeniden çalışma üç eylemler gerçekleşir:

    - 1. Aşama: Azure sanal makinelerini yeniden koruyun, böylece Azure'dan çoğaltmak şirket içi VMware sanal makinelerini dön.
    -  2. Aşama: şirket içi siteye bir yük devretme çalıştırın.
    - 3. Aşama: iş yükleri geri başarısız olmuş sonra şirket içi VM'ler için çoğaltma yeniden etkinleştirin.
    
 
**VMware azure'dan**

![Yeniden çalışma](./media/vmware-azure-architecture/enhanced-failback.png)


## <a name="next-steps"></a>Sonraki adımlar

İzleyin [Bu öğretici](vmware-azure-tutorial.md) VMware Azure çoğaltmayı etkinleştirmek için.
