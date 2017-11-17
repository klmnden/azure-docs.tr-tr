---
title: "Linux VM'ler için Azure'da genel bakış | Microsoft Docs"
description: "Linux sanal makineleri ile Azure işlem, depolama ve ağ hizmetlerini açıklar."
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: rickstercdn
manager: timlt
editor: 
ms.assetid: 7965a80f-ea24-4cc2-bc43-60b574101902
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: overview
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/14/2016
ms.author: rclaus
ms.custom: H1Hack27Feb2017, mvc
ms.openlocfilehash: cef9abddf980c695040e99995eb325eeb182fad4
ms.sourcegitcommit: c7215d71e1cdeab731dd923a9b6b6643cee6eb04
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/17/2017
---
# <a name="azure-and-linux"></a>Azure ve Linux
Microsoft Azure koleksiyonudur büyüyen tümleşik genel bulut, depolama, ağ, mobil analytics, sanal makineler, veritabanları dahil olmak üzere Hizmetleri ve web&mdash;çözümlerinizi barındırma için idealdir.  Microsoft Azure şirket içi donanım için yatırım yapmanıza gerek olmadan istediğiniz zaman yalnızca kullandığınız hizmetler için ödeme yapmanızı sağlayan ölçeklenebilir bir bilgi işlem platformu sunar.  Azure, çözümlerinizin ölçeğini artırmaya hazır olduğunuzda müşterilerinizin ihtiyaçlarını karşılamak için gereken ölçeğe yükseltilmek için hazırdır.

Amazon'ın çeşitli özelliklerle tanıdık AWS, Azure vs AWS incelemek [tanımı eşleme belge](https://azure.microsoft.com/campaigns/azure-vs-aws/mapping/).

## <a name="regions"></a>Bölgeler
Microsoft Azure kaynakları dünyanın birden çok coğrafi bölgeler arasında dağıtılır.  "Bölge" tek bir coğrafi alan birden çok veri merkezlerinde temsil eder.  Genellikle kullanılabilir 34 bölgeleri dünyanın duyurdu bir ek 4 bölgesiyle sahibiz. Bizim genel kapsamı - genişletmek sürmektedir çünkü bölgelerin güncelleştirilmiş listesini var ve yeni duyurdu korur.

* [Azure Bölgeleri](https://azure.microsoft.com/regions/)

## <a name="availability"></a>Kullanılabilirlik
Tüm diskler için premium depolama alanına sahip VM dağıttığınız bir endüstri başında tek örnek sanal makine % 99,9 Hizmet düzeyi sözleşmesi sağlanan duyurdu.  Dağıtımınız için standart %99,95 VM hizmet düzeyi sözleşmesi nitelemek için sırayla yine bir kullanılabilirlik kümesi içinde iş yükü çalıştıran iki veya daha fazla sanal makineleri dağıtmak gerekir. Bu, sanal makineleri birden çok hata etki alanlarını bizim veri merkezlerinde dağıtılmış yanı sıra farklı bakım pencereleri konaklarla üzerine dağıtılan garanti eder. [Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_0/) şartları, Azure’un tamamının kullanılabilirlik garantisini açıklamaktadır.

## <a name="managed-disks"></a>Yönetilen Diskler

Yönetilen disklerde tanıtıcıları Azure depolama hesap oluşturma ve arka planda yönetimini ve depolama hesabı ölçeklenebilirlik sınırları hakkında endişelenmeniz gerekmez sağlar. Yalnızca disk boyutu ve performans Katmanı (standart veya Premium) belirtin ve Azure oluşturur ve disk tarafından yönetilir. Disk eklediğinizde veya VM ölçeğini artırıp azalttığınızda bile kullanılan depolama alanı konusunda endişelenmeniz gerekmez. Yeni VM oluşturuyorsanız [Azure CLI 2.0 kullanması](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) veya VM'ler ile yönetilen işletim sistemi ve veri diskleri oluşturmak için Azure portalı. Yönetilmeyen disklerle VM'ler varsa [yönetilen disklerle yedeklenmesi Vm'leriniz Dönüştür](convert-unmanaged-to-managed-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Ayrıca, her Azure bölgesinde bir depolama hesabındaki özel görüntülerinizi yönetebilir ve aynı abonelikte yüzlerce VM oluşturmak için kullanabilirsiniz. Yönetilen Diskler hakkında daha fazla bilgi için bkz. [Yönetilen Disklere Genel Bakış](../windows/managed-disks-overview.md).

## <a name="azure-virtual-machines--instances"></a>Azure sanal makineleri & örnekleri
Microsoft Azure, sağlanan ve çeşitli iş ortakları tarafından korunan popüler Linux dağıtımları sayısı çalıştırılmasını destekler.  Red Hat Enterprise, CentOS, SUSE Linux Enterprise, Debian, Ubuntu, CoreOS, RancherOS, FreeBSD ve daha fazla Azure Marketi gibi dağıtımları bulacaksınız. Biz etkin olarak daha da fazla özellikleri eklemek için çeşitli Linux toplulukları çalışmak [Azure destekli Linux Distro'lar](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) listesi.

Tercih edilen Linux distro tercih galeride mevcut değilse, "kendi Linux getirebilir" VM tarafından [oluşturma ve Azure Linux VHD'yi karşıya](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Azure sanal makineler, bilgi işlem Çevik bir şekilde çözümlerinin çeşitli dağıtmanıza olanak sağlamak. Neredeyse tüm iş yükü ve neredeyse tüm işletim sistemi - Windows, Linux, üzerinde herhangi bir dil dağıtabilir veya özel bir tane büyüyen iş ortakları listemiz herhangi birinde oluşturdu. Hala ne aradığınız görmüyorum?  Endişelenmeyin - ayrıca şirket içi kendi görüntülerinizden kullanıma sunabilirsiniz.

## <a name="vm-sizes"></a>VM boyutları
Azure'da VM dağıttığınızda, iş yükü için uygun bizim dizi boyutları biri içinde bir VM boyutu seçmek için adımıdır. Boyutu, sanal makine işlem gücü, bellek ve depolama kapasitesini de etkiler. VM çalıştıran ve ayrılan kaynakları tüketen zaman miktarına göre faturalandırılır. Tam bir listesi [sanal makinelerin boyutları](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Burada, bir VM boyutu (A, D, DS, G ve GS) bizim serisi birini seçmek için temel bazı yönergeler bulunmaktadır.
* A-series VM'ler giriş seviyesi VM'ler hafif iş yükleri ve geliştirme ve Test senaryoları için fiyatlandırılır bizim değerlerdir. Bunlar tüm bölgelerde geniş çapta kullanılabilir ve bağlanabilir ve sanal makineler için kullanılabilir tüm standart kaynakları kullanın.
* A-series (A8 - A11) özel işlem yoğunluklu yapılandırmaları yüksek performanslı bilgi işlem küme uygulamalar için uygun boyutlarıdır.
* D Serisi VM'ler, daha yüksek işlem gücüne ve geçici süreli disk performansına ihtiyaç duyan uygulamaları çalıştıracak şekilde tasarlanmıştır. D Serisi VM'ler daha hızlı işlemcilere, daha yüksek bellek-vCPU oranına ve geçici disk için katı hal sürücüsüne (SSD) sahiptir.
* Dv2-serisi, bizim D-serisi en son sürümü, daha güçlü bir CPU özellikleri. Dv2 Serisi CPU, D Serisi CPU'dan yaklaşık %35 daha hızlıdır. Üzerinde en son oluşturma dayalı 2.4 GHz Intel Xeon® E5-2673 v3 (Haskell) işlemci ve Intel Turbo artırma teknolojisi 2.0 ile 3,2 GHz gidebilirsiniz. Dv2 Serisi, D Serisi ile aynı bellek ve disk yapılandırmalarına sahiptir.
* En fazla belleği sunan G Serisi VM'ler, Intel Xeon E5 V3 ailesi işlemcilere sahip ana bilgisayarlarda çalışır.

Not: DS serisi ve GS serisi VM'ler Premium depolama erişimi - g/ç yoğun iş yükleri için yüksek performanslı, düşük gecikme süreli depolama bizim SSD yedeklenir. Premium Depolama belirli bölgelerde kullanılabilir. Ayrıntılar için bkz.

* [Premium Storage: Azure sanal makine iş yükleri için yüksek performanslı depolama](../windows/premium-storage.md)

## <a name="automation"></a>Otomasyon
Uygun bir DevOps kültür elde etmek için tüm altyapı kodu olmalıdır.  Ne zaman tüm altyapı kolayca olabilir kodda yaşadığı (Phoenix sunucuları) yeniden.  Azure Ansible, Chef, SaltStack ve Puppet gibi tooling ana otomasyon ile çalışır.  Ayrıca Azure Otomasyon için kendi araç sahiptir:

* [Azure şablonları](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Azure VMAccess](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Azure desteği sunulmadan [bulut init](http://cloud-init.io/) destekleyen çoğu Linux Distro'lar arasında.  Şu anda Canonical'ın Ubuntu VM bulut init varsayılan olarak etkin olan dağıtılır.  Kırmızı şapkalar RHEL, CentOS ve Fedora bulut init destekler, ancak Azure görüntüleri RedHat tarafından korunan bulut init yüklü gerekmez.  Bulut init RedHat ailesinde işletim sistemi kullanmak için bulut yüklü başlatma ile özel bir görüntü oluşturmanız gerekir.

* [Azure Linux VM'ler üzerinde bulut init kullanma](using-cloud-init.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="quotas"></a>Kotalar
Her Azure aboneliği, çok sayıda sanal makineleri projeniz için dağıtım etkileyebilir yerinde varsayılan kota sınırları vardır. Geçerli sınırlar abonelik başına her bölge için 20 VM olarak belirlenmiştir.  Kota sınırları hızlı bir şekilde oluşturulur ve bir sınır isteyen bir destek bileti dosyalama tarafından kolayca artırın.  Kota sınırları hakkında daha fazla ayrıntı için:

* [Azure aboneliği hizmet sınırları](../../azure-subscription-service-limits.md)

## <a name="partners"></a>İş Ortakları
Microsoft yakından ortaklarımızın kullanılabilir görüntüleri güncelleştirilen ve bir Azure çalışma zamanı için en iyi duruma getirilmiş emin olmak için birlikte çalışır.  Ortaklarımızın hakkında daha fazla bilgi için Market sayfaları denetleyin.

* Azure - Linux'ta [destekli dağıtımlar](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* SUSE - [Azure Market - SUSE Linux Enterprise Server](https://azuremarketplace.microsoft.com/en-us/marketplace/apps?search=%27SUSE%27)
* RedHat - [Azure Market - RedHat Enterprise Linux 7.2](https://azure.microsoft.com/marketplace/partners/redhat/redhatenterpriselinux72/)
* Kurallı - [Azure Market - Ubuntu Server 16.04 LTS](https://azure.microsoft.com/marketplace/partners/canonical/ubuntuserver1604lts/)
* Debian - [Azure Market - Debian 8 "Jessie"](https://azure.microsoft.com/marketplace/partners/credativ/debian8/)
* FreeBSD - [Azure Market - FreeBSD 10.3](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd103/)
* CoreOS - [Azure Market - CoreOS (kararlı)](https://azure.microsoft.com/marketplace/partners/coreos/coreosstable/)
* RancherOS - [Azure Market - RancherOS](https://azure.microsoft.com/marketplace/partners/rancher/rancheros/)
* Bitnami - [Azure için Bitnami kitaplığı](https://azure.bitnami.com/)
* Mesosphere - [Azure Market - Mesosphere DC/OS Azure ile ilgili](https://azure.microsoft.com/marketplace/partners/mesosphere/dcosdcos/)
* Docker - [Azure Market - Docker Swarm ile Azure kapsayıcı hizmeti](https://azure.microsoft.com/marketplace/partners/microsoft/acsswarms/)
* Jenkins - [Azure Market - CloudBees Jenkins Platform](https://azure.microsoft.com/marketplace/partners/cloudbees/jenkins-platformjenkins-platform/)

## <a name="getting-started-with-linux-on-azure"></a>Linux Azure üzerinde ile çalışmaya başlama
Azure'ı kullanmaya başlamak için bir Azure hesabı, Azure CLI yüklenmiş ve SSH ortak ve özel anahtar çiftini gerekir.

### <a name="sign-up-for-an-account"></a>Hesap için kaydolma
Azure bulut kullanmanın ilk adımı, bir Azure hesabı için kaydolun olmaktır.  Git [Azure hesaba kaydolmayı](https://azure.microsoft.com/pricing/free-trial/) başlamak için sayfa.

### <a name="install-the-cli"></a>CLI'yı yükleme
Yeni Azure hesabınızla, web tabanlı yönetim paneli Azure portal hemen kullanmaya başlayabilirsiniz.  Komut satırı aracılığıyla Azure Bulutu yönetmek için yüklemeniz `azure-cli`.  Yükleme [Azure CLI 2.0](/cli/azure/install-azure-cli) Mac veya Linux istasyonunuzda.

### <a name="create-an-ssh-key-pair"></a>SSH anahtar çifti oluşturma
Şimdi bir Azure hesabı, Azure web portalı ve Azure CLI sahiptir.  Sonraki adım, bir parola kullanmadan Linux kullanılan SSH ile SSH anahtar çifti oluşturmaktır.  [Linux ve Mac'de SSH anahtarları oluşturma](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) parolasız ve daha iyi güvenlik etkinleştirmek için.

### <a name="create-a-vm-using-the-cli"></a>CLI kullanarak VM oluşturma
CLI kullanarak bir Linux VM oluşturma çalıştığınız terminal ayrılmadan bir VM'yi dağıtmak için bir hızlı yoludur.  Her şeyi web portalında belirtebilirsiniz, bir komut satırı bayrağı veya anahtar kullanılabilir.  

* [CLI kullanarak bir Linux VM oluşturma](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="create-a-vm-in-the-portal"></a>Portalda bir VM oluşturma
Azure web Portalı'nda bir Linux VM oluşturma kolayca üzerine ve bir dağıtım almak için çeşitli seçenekler arasında bir yoludur.  Komut satırı bayrakları veya anahtarları kullanmak yerine, çeşitli seçenekler ve ayarlar iyi web yerleşimini görüntüleyebilirsiniz.  Komut satırı arabirimi kullanılabilir her şeyi da Portalı'nda mevcuttur.

* [Portalı kullanarak bir Linux VM oluşturma](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="login-using-ssh-without-a-password"></a>SSH olmadan parola kullanarak oturum açma
VM artık Azure üzerinde çalışan ve oturum açmak hazırsınız.  Parolalar SSH yoluyla oturum açmak için güvenli ve zaman alıcı kullanmaktır.  SSH anahtarları kullanarak, en güvenli yolu ve ayrıca oturum açmak için en hızlı yolu değildir.  Linux VM portalı veya CLI ile oluşturduğunuz zaman, iki kimlik doğrulama seçeneğiniz vardır.  SSH için bir parola seçerseniz, Azure oturum açma parolaları aracılığıyla izin vermek üzere VM yapılandırır.  Bir SSH ortak anahtarı kullanmayı seçerseniz, Azure VM yalnızca oturum açma bilgileri SSH anahtarları aracılığıyla izin verecek şekilde yapılandırır ve parola oturum açmalar devre dışı bırakır. Linux VM yalnızca SSH anahtar oturumları vererek güvenli hale getirmek için SSH ortak anahtarı seçeneği portalında veya CLI VM oluşturma sırasında kullanın.

## <a name="related-azure-components"></a>İlişkili Azure bileşenleri
## <a name="storage"></a>Depolama
* [Microsoft Azure Depolama'ya Giriş](../../storage/common/storage-introduction.md)
* [Azure CLI kullanarak bir Linux VM için bir disk ekleyin](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Nasıl bir Linux VM Azure portalında bir veri diski ekleme](attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="networking"></a>Ağ
* [Sanal ağ genel bakış](../../virtual-network/virtual-networks-overview.md)
* [Azure’da IP adresleri](../../virtual-network/virtual-network-ip-addresses-overview-arm.md)
* [Azure'da bir Linux VM için bağlantı noktalarını açma](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Azure portalında tam etki alanı adı oluşturma](portal-create-fqdn.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="containers"></a>Kapsayıcılar
* [Sanal makineler ve Azure kapsayıcı](containers.md)
* [Azure kapsayıcı hizmeti giriş](../../container-service/container-service-intro.md)
* [Azure Container Service kümesi dağıtma](../../container-service/dcos-swarm/container-service-deployment.md)

## <a name="next-steps"></a>Sonraki adımlar
Artık Azure üzerinde Linux genel bir bakış vardır.  Sonraki adım, daha yakından inceleyin ve birkaç VM'ler oluşturmaktır!

* [Örnek komut dosyalarını ortak görevler için bizim gittikçe artan AzureCLI keşfedin](cli-samples.md)
