---
title: Red Hat Update Infrastructure | Microsoft Docs
description: Microsoft azure'da isteğe bağlı Red Hat Enterprise Linux örnekleri için Red Hat Update Infrastructure hakkında bilgi edinin
services: virtual-machines-linux
documentationcenter: ''
author: BorisB2015
manager: jeconnoc
editor: ''
ms.assetid: f495f1b4-ae24-46b9-8d26-c617ce3daf3a
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 6/6/2019
ms.author: borisb
ms.openlocfilehash: 4315a849f3f117633f5f6a9d93c995ece9f527a3
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67076995"
---
# <a name="red-hat-update-infrastructure-for-on-demand-red-hat-enterprise-linux-vms-in-azure"></a>Azure'da isteğe bağlı Red Hat Enterprise Linux VM'ler için Red Hat güncelleştirme altyapısı
 [Red Hat Update Infrastructure](https://access.redhat.com/products/red-hat-update-infrastructure) (RHUI) gibi Red Hat barındırılan depo içeriğini yansıtmak için özel depolar ile Azure özgü içerik oluşturmak ve son kullanıcı VM'ler için kullanılabilir hale getirmek amacıyla bulut sağlayıcıları sağlar.

Red Hat Enterprise Linux (RHEL) Kullandıkça Öde (PAYG) görüntüleri Azure RHUI erişmek için önceden yapılandırılmış olarak gelir. Ek bir yapılandırma gerekmez. En son güncelleştirmeleri almak için çalıştırın `sudo yum update` RHEL örneğinizin hazır olduktan sonra. Bu hizmet, RHEL PAYG yazılım ücretleri bir parçası olarak dahil edilir.

Yayımlama ve bekletme ilkeleri de dahil olmak üzere azure'daki RHEL görüntüleri hakkında daha fazla bilgi edinilebilir [burada](./rhel-images.md).

RHEL tüm sürümleri için Red Hat destek ilkeleri hakkında daha fazla bilgi bulunabilir [Red Hat Enterprise Linux yaşam döngüsü](https://access.redhat.com/support/policy/updates/errata) sayfası.

## <a name="important-information-about-azure-rhui"></a>Azure RHUI hakkında önemli bilgiler
* Azure RHUI Azure'da oluşturulan tüm RHEL PAYG VM'lerin desteklediği güncelleştirme altyapısıdır. Bu size Abonelik Yöneticisi veya uydu veya diğer güncelleştirme kaynağı PAYG RHEL Vm'lerinizi kaydettikten kullanımını değil, ancak bunun yapılması PAYG VM ile dolaylı çift faturalandırma uygulanmasına neden olur. Aşağıdaki noktası ayrıntıları için bkz.
* Azure'da barındırılan RHUI erişimi PAYG RHEL görüntüsü fiyatına dahildir. Azure'da barındırılan RHUI PAYG RHEL VM'den kaydını kaldırırsanız, sanal makinenin bir VM Getir-kendi lisansını (KLG) türü dönüştürmez. Başka bir güncelleştirme kaynağı ile aynı VM kaydolursanız, neden olabilecek _dolaylı_ çift ücretleri. İlk kez Azure RHEL yazılım ücreti karşılığında ücret ödersiniz. İkinci kez önceden satın alınan Red Hat abonelikler için ücret ödersiniz. Tutarlı bir şekilde Azure'da barındırılan RHUI dışındaki bir güncelleştirme altyapısı kullanmanız gerekirse, kullanmak için kaydetme göz önünde bulundurun [BYOS RHEL görüntüleri](https://aka.ms/rhel-byos).
* RHUI varsayılan davranışını çalıştırdığınızda RHEL VM'nize küçük en son sürüme yükseltmek için olan `sudo yum update`.

    Örneğin, bir RHEL 7.4 PAYG görüntüden bir VM sağlama ve çalıştırıyorsanız `sudo yum update`, RHEL 7.6 VM (en son alt sürüm RHEL7 ailesindeki) elde edersiniz.

    Bu davranışı önlemek için geçiş yapabilirsiniz [genişletilmiş güncelleştirme destek kanallarını](#rhel-eus-and-version-locking-rhel-vms) veya kendi görüntünüzü açıklandığı [oluşturup Azure için Red Hat tabanlı bir sanal makine yükleme](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) makalesi. Kendi görüntünüzü oluşturursanız, farklı güncelleştirme altyapısını bağlanması gereken ([doğrudan Red Hat içerik teslim](https://access.redhat.com/solutions/253273) veya [Red Hat uydu sunucu](https://access.redhat.com/products/red-hat-satellite)).



* (SAP, SAP HANA için RHEL ve RHEL for SAP Business Applications RHEL) azure'da SAP PAYG RHEL görüntüleri SAP sertifika için gereken belirli RHEL alt sürüm kalır özel RHUI kanallar bağlıdır.

* İçindeki VM'ler için Azure'da barındırılan RHUI erişim sınırlıdır [Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653). Proxy kullanıyorsanız tüm VM trafiğe bir şirket içi ağ altyapısı aracılığıyla RHEL PAYG sanal makinelerin Azure RHUI erişmek kullanıcı tanımlı rotalar ayarlama gerekebilir.

## <a name="rhel-eus-and-version-locking-rhel-vms"></a>RHEL EUS ve sürüm kilitleme RHEL VM'ler
Bazı müşterilerin belirli RHEL alt yayın, RHEL Vm'lerine kilitleme isteyebilirsiniz. RHEL VM'nize belirli bir alt sürüm sürüm kilidi depoları için genişletilmiş güncelleştirme desteğini depoları işaret edecek şekilde güncelleştirerek kullanabilirsiniz. Ayrıca, sürüm kilitleme EUS işlemi geri alabilirsiniz.

>[!NOTE]
> EUS RHEL ek özellikler üzerinde desteklenmiyor. Bu, RHEL ek özellikler kanaldan genellikle kullanılabilir olan bir paket yüklüyorsanız, EUS sırada üzerinde bunu mümkün olmayacağını anlamına gelir. Red Hat ek özellikler ürün yaşam döngüsü ayrıntılı [burada](https://access.redhat.com/support/policy/updates/extras/).

Bu makalenin yazıldığı sırada, RHEL için EUS desteği sona erdi < 7.3 =. "Red Hat Enterprise Linux uzun destek eklentiler" bölümüne bakın [Red Hat belgeleri](https://access.redhat.com/support/policy/updates/errata/) daha fazla ayrıntı için.
* 31 Ağustos 2019 RHEL 7.4 EUS desteği sona eriyor
* 30 Nisan 2020 RHEL 7.5 EUS desteği sona eriyor
* 31 Ekim 2020 RHEL 7.6 EUS desteği sona eriyor

### <a name="switch-a-rhel-vm-to-eus-version-lock-to-a-specific-minor-version"></a>RHEL VM EUS (sürüm kilidi için belirli bir alt sürümü) geçin
RHEL VM (kök olarak çalıştırın) belirli bir alt sürüme kilitlemek için aşağıdaki yönergeleri kullanın:

>[!NOTE]
> Bu, yalnızca EUS kullanılabilir RHEL sürümleri için geçerlidir. Bu makalenin yazıldığı sırada, bu RHEL 7.2 7.6 içerir. Daha fazla ayrıntı bulabilirsiniz [Red Hat Enterprise Linux yaşam döngüsü](https://access.redhat.com/support/policy/updates/errata) sayfası.

1. EUS olmayan depoları devre dışı bırakın:
    ```bash
    yum --disablerepo='*' remove 'rhui-azure-rhel7'
    ```

1. EUS depoları ekleyin:
    ```bash
    yum --config='https://rhelimage.blob.core.windows.net/repositories/rhui-microsoft-azure-rhel7-eus.config' install 'rhui-azure-rhel7-eus'
    ```

1. (Kök olarak çalıştırın) releasever değişkeni kilit:
    ```bash
    echo $(. /etc/os-release && echo $VERSION_ID) > /etc/yum/vars/releasever
    ```

    >[!NOTE]
    > Yukarıdaki yönerge RHEL ikincil sürümü için geçerli alt sürüm kilitleyecek. Yükseltme ve en son olmayan daha sonra ikincil sürümü kilitlemek için arıyorsanız, belirli bir ikincil sürüm girin. Örneğin, `echo 7.5 > /etc/yum/vars/releasever` RHEL 7.5, RHEL sürümüne kilitler

1. RHEL VM'nize güncelleştir
    ```bash
    sudo yum update
    ```

### <a name="switch-a-rhel-vm-back-to-non-eus-remove-a-version-lock"></a>RHEL VM geri olmayan-(sürüm kilidi kaldırmak) EUS için geçiş
Aşağıdaki kök olarak çalıştırın:
1. Releasever dosyayı kaldırın:
    ```bash
    rm /etc/yum/vars/releasever
     ```

1. EUS depoları devre dışı bırakın:
    ```bash
    yum --disablerepo='*' remove 'rhui-azure-rhel7-eus'
   ```

1. RHEL VM'nize güncelleştir
    ```bash
    sudo yum update
    ```

## <a name="the-ips-for-the-rhui-content-delivery-servers"></a>RHUI içerik teslim sunucular için IP'ler

RHUI RHEL isteğe bağlı görüntüleri kullanılabilir olduğu tüm bölgelerde kullanılabilir. Şu anda listelenen tüm genel bölgelerde içerir [Azure durum Panosu](https://azure.microsoft.com/status/) sayfa, Azure ABD devlet kurumları ve Microsoft Azure Almanya bölgeleri.

Daha fazla RHEL PAYG Vm'lerden erişimi kısıtlamak için bir ağ yapılandırması kullanıyorsanız, emin aşağıdaki IP'leri verilir `yum update` nde olduğunuzdan ortamına bağlı olarak çalışmak için:


```
# Azure Global
13.91.47.76
40.85.190.91
52.187.75.218
52.174.163.213
52.237.203.198

# Azure US Government
13.72.186.193
13.72.14.155
52.244.249.194

# Azure Germany
51.5.243.77
51.4.228.145
```

## <a name="azure-rhui-infrastructure"></a>Azure RHUI altyapı


### <a name="update-expired-rhui-client-certificate-on-a-vm"></a>Bir VM üzerinde süresi dolmuş RHUI istemci sertifikasını güncelleştir

Örneğin, RHEL 7.4 bir eski RHEL VM görüntüsünü kullanıyorsanız (URN görüntü: `RedHat:RHEL:7.4:7.4.2018010506`), RHUI artık süresi dolmuş SSL istemci sertifikası nedeniyle bağlantı sorunları yaşar. Gördüğünüz hata görünebilir _"SSL eş reddedildi sertifikanızın süresi dolmuş olarak"_ veya _"hata: Depo için (repomd.xml) depo meta verileri alınamıyor:... Lütfen yolunu doğrulayın ve yeniden deneyin"_ . Bu sorunu çözmek için lütfen aşağıdaki komutu kullanarak VM'yi RHUI istemci paketi güncelleştirin:

```bash
sudo yum update -y --disablerepo='*' --enablerepo='*microsoft*'
```

Alternatif olarak, çalışan `sudo yum update` başka depolar için görürsünüz "süresi dolmuş SSL sertifikası" hataları rağmen (sürümüne bağlı olarak, RHEL), istemci sertifika paketini de güncelleştirebilirsiniz. Bu güncelleştirme başarılı olursa, çalıştırılacak mümkün olmayacak şekilde diğer RHUI depolarda normal bağlantısı, geri yüklenmelidir `sudo yum update` başarıyla.

404 hatası çalışırken karşılaşırsanız bir `yum update`, yum önbelleğinizi yenilemek için aşağıdakileri deneyin:
```bash
sudo yum clean all;
sudo yum makecache
```

### <a name="troubleshoot-connection-problems-to-azure-rhui"></a>Azure RHUI için bağlantı sorunlarını giderme
Azure RHEL PAYG VM'den Azure RHUI bağlanma konusunda sorunlar karşılaşırsanız, aşağıdaki adımları izleyin:

1. VM yapılandırması Azure RHUI uç noktası için inceleyin:

    1. Kontrol `/etc/yum.repos.d/rh-cloud.repo` dosyasını içeren bir başvuru `rhui-[1-3].microsoft.com` içinde `baseurl` , `[rhui-microsoft-azure-rhel*]` bölümü dosyasının. Varsa, yeni Azure RHUI kullanıyorsunuz.

    1. Aşağıdaki desene sahip bir konuma işaret ediyorsa `mirrorlist.*cds[1-4].cloudapp.net`, yapılandırma güncelleştirmesi gerekli değildir. Eski VM anlık görüntüsü kullanıyorsanız ve yeni Azure RHUI işaret edecek şekilde güncelleştirmeniz gerekir.

1. İçindeki sanal makineler için Azure'da barındırılan RHUI erişim sınırlıdır [Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653).

1. Yeni yapılandırmayı kullanıyorsanız, VM Azure IP aralığından bağlanır ve Azure RHUI, dosya Microsoft ya da Red Hat destek servis talebi için bağlantı kurulamıyor doğrulanmıştır.

### <a name="infrastructure-update"></a>Altyapı Güncelleştirmesi

Eylül 2016'da, biz güncelleştirilmiş bir Azure RHUI dağıtıldı. Nisan 2017'de biz eski Azure RHUI kapatın. Ayrıca, PAYG RHEL görüntüleri (veya bunların anlık görüntülerini) Eylül 2016'dan veya üzerini kullanıyorsanız, otomatik olarak yeni Azure RHUI bağlanırsınız. Ancak, eski anlık görüntüleri Vm'lerinizde varsa, aşağıdaki bölümde açıklandığı gibi Azure RHUI erişmek için kendi yapılandırmasını el ile güncelleştirmeniz gerekir.

Yeni Azure RHUI sunuculara dağıtılan [Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager/). Trafik Yöneticisi'nde, tek bir uç nokta (rhuı 1.microsoft.com) uygulama bölgesine bakılmaksızın herhangi bir VM tarafından kullanılabilir.

### <a name="manual-update-procedure-to-use-the-azure-rhui-servers"></a>Azure RHUI sunucularını kullanmak için el ile güncelleştirme yordamı
Bu yordam, yalnızca başvuru sağlanır. PAYG RHEL görüntüleri için Azure RHUI bağlanmak için doğru yapılandırma zaten var. Azure RHUI sunucularını kullanacak şekilde yapılandırmayı el ile güncelleştirmek için aşağıdaki adımları tamamlayın:

- RHEL 6:
  ```bash
  yum --config='https://rhelimage.blob.core.windows.net/repositories/rhui-microsoft-azure-rhel6.config' install 'rhui-azure-rhel6'
  ```

- RHEL 7:
  ```bash
  yum --config='https://rhelimage.blob.core.windows.net/repositories/rhui-microsoft-azure-rhel7.config' install 'rhui-azure-rhel7'
  ```

## <a name="next-steps"></a>Sonraki adımlar
* Azure Market PAYG görüntüden bir Red Hat Enterprise Linux VM oluşturma ve Azure'da barındırılan RHUI kullanılacak Git [Azure Marketi](https://azure.microsoft.com/marketplace/partners/redhat/).
* Azure'da Red Hat görüntülerini hakkında daha fazla bilgi için şuraya gidin [belgeleri sayfasını](./rhel-images.md).
* RHEL tüm sürümleri için Red Hat destek ilkeleri hakkında daha fazla bilgi bulunabilir [Red Hat Enterprise Linux yaşam döngüsü](https://access.redhat.com/support/policy/updates/errata) sayfası.
