---
title: "Linux destekli dağıtımlar | Microsoft Docs"
description: "Linux hakkında Ubuntu ve CentOS, Oracle ve SUSE için yönergeler de dahil olmak üzere, Azure destekli dağıtımlar hakkında bilgi edinin."
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: tysonn
tags: azure-service-management,azure-resource-manager
ms.assetid: 2777a526-c260-4cb9-a31a-bdfe1a55fffc
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/02/2017
ms.author: szark
ms.openlocfilehash: 39cb2464eb593a29c4436afb5c14419b704ebff4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="linux-on-distributions-endorsed-by-azure"></a>Linux üzerinde tarafından Azure destekli dağıtımlar
İş ortakları Azure Marketi Linux görüntüleri sağlar. Daha fazla özellikleri destekli dağıtım listesine eklemek için çeşitli Linux toplulukları ile çalışıyoruz. Bu arada, Market görüntüsünden kullanılabilir değil dağıtımları için her zaman kendi Linux yönergeleri izleyerek getirebilir [oluşturma ve Linux işletim sistemini içeren bir sanal sabit disk karşıya](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

## <a name="supported-distributions-and-versions"></a>Desteklenen dağıtımları ve sürümleri
Aşağıdaki tabloda Linux dağıtımları ve Azure üzerinde desteklenen sürümleri listelenmektedir. Başvurmak [Linux desteği görüntüleri Microsoft Azure'da](https://support.microsoft.com/en-us/kb/2941892) daha ayrıntılı bilgi için.

Hyper-V ve Microsoft Azure Linux Tümleştirme hizmetleri (LIS) sürücülerini Yukarı Akış Linux çekirdek doğrudan Microsoft katkı çekirdek modülleri ' dir.  Bazı LIS sürücüleri dağıtım 's çekirdeğe varsayılan olarak oluşturulur. Red Hat Enterprise (RHEL) tabanlı eski dağıtımları / CentOS ayrı bir yükleme olarak kullanılabilir [Hyper-V için Linux Tümleştirme hizmetleri sürümü 4.1](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409). Bkz: [Linux çekirdek gereksinimleri](create-upload-generic.md#linux-kernel-requirements) LIS sürücüleri hakkında daha fazla bilgi için.

Azure Linux Aracısı'nı Azure Marketi görüntülerinde önceden yüklü olduğu ve dağıtım ait paket depodan genellikle kullanılabilir. Kaynak kodu bulunabilir [GitHub](https://github.com/azure/walinuxagent).

| Dağıtım | Sürüm | Sürücüler | Aracı |
| --- | --- | --- | --- |
| CentOS |CentOS 6.3 + 7.0 + |CentOS 6.3: [LIS indirin](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409)<p>CentOS 6.4 +: çekirdek |Paketi: İçinde [repo](http://olcentgbl.trafficmanager.net/openlogic/6/openlogic/x86_64/RPMS/) "WALinuxAgent" altında <br/>Kaynak kodu: [GitHub](https://github.com/Azure/WALinuxAgent) |
| [CoreOS](https://coreos.com/docs/running-coreos/cloud-providers/azure/) |494.4.0+ |Çekirdek |Kaynak kodu: [GitHub](https://github.com/coreos/coreos-overlay/tree/master/app-emulation/wa-linux-agent) |
| Debian |Debian 7,9 +, 8.2 + |Çekirdek |Paketi: "waagent" altında bağlantıların bulunması <br/>Kaynak kodu: [GitHub](https://github.com/Azure/WALinuxAgent) |
| Oracle Linux |6.4+, 7.0+ |Çekirdek |Paketi: "WALinuxAgent" altında bağlantıların bulunması <br/>Kaynak kodu: [GitHub](http://go.microsoft.com/fwlink/p/?LinkID=250998) |
| Red Hat Enterprise Linux |RHEL 6.7 + 7.1 + |Çekirdek |Paketi: "WALinuxAgent" altında bağlantıların bulunması <br/>Kaynak kodu: [GitHub](https://github.com/Azure/WALinuxAgent) |
| SUSE Linux Enterprise |SLES/SLES SAP için<br>11 SP4<br>12 SP1 +|Çekirdek |Paketi:<p> 11 inç için [bulut: Araçlar](https://build.opensuse.org/project/show/Cloud:Tools) deposu<br>için "Genel bulut" modülünde "python-azure-agent" altında bulunan 12<br/>Kaynak kodu: [GitHub](http://go.microsoft.com/fwlink/p/?LinkID=250998) |
| openSUSE |openSUSE artık 42.1 + |Çekirdek |Paketi: İçinde [bulut: Araçlar](https://build.opensuse.org/project/show/Cloud:Tools) "python-azure-agent" altında deposu <br/>Kaynak kodu: [GitHub](https://github.com/Azure/WALinuxAgent) |
| Ubuntu |Ubuntu 12.04, 14.04, 16.04, 16.10 |Çekirdek |Paketi: "walinuxagent" altında bağlantıların bulunması <br/>Kaynak kodu: [GitHub](https://github.com/Azure/WALinuxAgent) |

## <a name="partners"></a>İş Ortakları

### <a name="coreos"></a>CoreOS
[https://CoreOS.com/docs/Running-CoreOS/cloud-providers/Azure/](https://coreos.com/docs/running-coreos/cloud-providers/azure/)

CoreOS Web sitesinden:

*CoreOS güvenlik, tutarlılık ve güvenilirlik için tasarlanmıştır. CoreOS yum aracılığıyla veya apt paketleri yüklemek yerine, hizmetlerinizi soyutlama daha yüksek düzeyde yönetmek için Linux kapsayıcıları kullanır. Tek bir hizmetin kodu ve tüm bağımlılıkları, bir veya daha çok CoreOS makinelerde çalıştırılabilir bir kapsayıcıdaki paketlenir.*

### <a name="credativ"></a>Credativ
[http://www.credativ.co.uk/credativ-blog/debian-images-Microsoft-Azure](http://www.credativ.co.uk/credativ-blog/debian-images-microsoft-azure)

Credativ bağımsız danışmanlık ve ücretsiz yazılımlar kullanarak geliştirme ve profesyonel çözümleri uyarlamasını uzmanlaşmış Hizmetleri şirket ' dir. Önde gelen açık kaynak uzmanlarıyla Credativ destek kullanan çok sayıda BT departmanları ile uluslararası tanıma sahip. Microsoft ile birlikte Credativ şu anda karşılık gelen Debian görüntüleri Debian 8 (Jessie) ve Debian için 7 (Wheezy) önce hazırlanıyor. Her iki görüntüleri Azure üzerinde çalışmak üzere özel olarak tasarlanmıştır ve platform kolayca yönetilebilir. Credativ uzun süreli Bakım ve kendi açık kaynak destek merkezleri aracılığıyla Azure Debian görüntülerini güncelleştirme de destekler.

### <a name="oracle"></a>Oracle
[http://www.Oracle.com/technetwork/topics/cloud/FAQ-1963009.HTML](http://www.oracle.com/technetwork/topics/cloud/faq-1963009.html)

Oracle'nın genel ve özel bulut çözümleri geniş bir yelpazesini sunmaya stratejidir. Stratejisi müşteriler seçim ve bunlar Oracle bulutlarındaki Oracle yazılım ve diğer Bulutları nasıl dağıtıldığına esneklik sağlar. Microsoft Oracle'nın yöneticileriyle, müşterilerin Microsoft Genel ve özel Bulutlar sertifika güvenle Oracle yazılım dağıtma ve Oracle kaynağından destek sağlar.  Oracle'nın taahhüt ve Oracle ortak ve özel bulut çözümleri yatırım değişmez.

### <a name="red-hat"></a>Red Hat
[http://www.RedHat.com/en/partners/Strategic-Alliance/Microsoft](http://www.redhat.com/en/partners/strategic-alliance/microsoft)

Açık kaynak çözümlerinin dünyanın en önde gelen sağlayıcısı, birden fazla % 90'ını iş sorunlarını çözmesine, kendi BT Hizala Fortune 500 şirketleri Red Hat yardımcı olur ve iş stratejilerini ve teknoloji gelecek için hazırlayın. Red Hat bunu bir açık iş modeli ve uygun maliyetli, tahmin edilebilir abonelik modeli aracılığıyla güvenli çözümler sağlayarak gerçekleştirir.

### <a name="suse"></a>SUSE
[http://www.SuSE.com/SuSE-Linux-Enterprise-Server-on-Azure](http://www.suse.com/suse-linux-enterprise-server-on-azure)

SUSE Linux Enterprise Server Azure ile ilgili üst düzey güvenilirlik ve güvenlik için bulut sağlayan bir kanıtlanmış platformudur. SUSE'ın çok yönlü Linux platformuna kolayca yönetilebilir bulut ortamı sağlamak için Azure bulut hizmetleriyle sorunsuz şekilde entegre olur. SUSE Linux Enterprise Server için 1800'den fazla bağımsız yazılım satıcılarından birden fazla 9,200 sertifikalı uygulamaları ile desteklenen veri merkezinde çalışan iş yüklerini güvenle Azure'da dağıtılabilir SUSE sağlar.

### <a name="canonical"></a>Canonical
[http://www.ubuntu.com/cloud/Azure](http://www.ubuntu.com/cloud/azure)

Kurallı mühendislik ve açık bir topluluk idare sürücü Ubuntu'nın başarılı olan istemci, sunucu ve bulut tüketicileri için kişisel bulut hizmetlerini içerir. Ubuntu, bulut için Phone bir birleşik, ücretsiz platformunda kurallı'nin sunulmasıyla, telefon, tablet, TV ve Masaüstü için tutarlı arabirimleri ailesi sağlar. Bu Vizyon Ubuntu tüketici elektronik alıcılar ve tek tek ekiplerindeki arasında sık kullanılan genel bulut sağlayıcıları'ndan farklı kurumlara için ilk seçim yapar.

Geliştiriciler ve dünyanın mühendislik merkezleri ile Canonical donanım üreticileri, içerik sağlayıcıları ve yazılım geliştiricilerin Ubuntu çözümleri Pazar PC'ler, sunucular ve el aygıtları için duruma getirmek için benzersiz olarak yerleştirilir.
