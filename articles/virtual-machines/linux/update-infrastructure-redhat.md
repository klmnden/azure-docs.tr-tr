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
ms.date: 1/7/2019
ms.author: borisb
ms.openlocfilehash: 018ad851b223caa4017d544ca12e2a6397654205
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60799203"
---
# <a name="red-hat-update-infrastructure-for-on-demand-red-hat-enterprise-linux-vms-in-azure"></a>Azure'da isteğe bağlı Red Hat Enterprise Linux VM'ler için Red Hat güncelleştirme altyapısı
 [Red Hat Update Infrastructure](https://access.redhat.com/products/red-hat-update-infrastructure) (RHUI) gibi Red Hat barındırılan depo içeriğini yansıtmak için özel depolar ile Azure özgü içerik oluşturmak ve son kullanıcı VM'ler için kullanılabilir hale getirmek amacıyla bulut sağlayıcıları sağlar.

Red Hat Enterprise Linux (RHEL) Kullandıkça Öde (PAYG) görüntüleri Azure RHUI erişmek için önceden yapılandırılmış olarak gelir. Ek bir yapılandırma gerekmez. En son güncelleştirmeleri almak için çalıştırın `sudo yum update` RHEL örneğinizin hazır olduktan sonra. Bu hizmet, RHEL PAYG yazılım ücretleri bir parçası olarak dahil edilir.

Yayımlama ve bekletme ilkeleri de dahil olmak üzere azure'daki RHEL görüntüleri hakkında daha fazla bilgi edinilebilir [burada](./rhel-images.md).

RHEL tüm sürümleri için Red Hat destek ilkeleri hakkında daha fazla bilgi bulunabilir [Red Hat Enterprise Linux yaşam döngüsü](https://access.redhat.com/support/policy/updates/errata) sayfası.

## <a name="important-information-about-azure-rhui"></a>Azure RHUI hakkında önemli bilgiler
* Azure RHUI, şu anda yalnızca en son alt sürüm her RHEL ailesi (RHEL6 veya RHEL7) destekler. RHUI küçük en son sürüme bağlı RHEL VM örneği yükseltmek için çalıştırmanız `sudo yum update`.

    Örneğin, bir RHEL 7.4 PAYG görüntüden bir VM sağlama ve çalıştırıyorsanız `sudo yum update`, RHEL 7.6 VM (en son alt sürüm RHEL7 ailesindeki) elde edersiniz.

    Bu davranışı önlemek için açıklandığı gibi kendi görüntünüzü oluşturmak gereken [oluşturup Azure için Red Hat tabanlı bir sanal makine yükleme](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) makalesi. Bir farklı güncelleştirme altyapınıza bağlamak gerekir ([doğrudan Red Hat içerik teslim](https://access.redhat.com/solutions/253273) veya [Red Hat uydu sunucu](https://access.redhat.com/products/red-hat-satellite)).

* Azure'da barındırılan RHUI erişimi PAYG RHEL görüntüsü fiyatına dahildir. Azure'da barındırılan RHUI PAYG RHEL VM'den kaydını kaldırırsanız, sanal makinenin bir VM Getir-kendi lisansını (KLG) türü dönüştürmez. Başka bir güncelleştirme kaynağı ile aynı VM kaydolursanız, neden olabilecek _dolaylı_ çift ücretleri. İlk kez Azure RHEL yazılım ücreti karşılığında ücret ödersiniz. İkinci kez önceden satın alınan Red Hat abonelikler için ücret ödersiniz. Tutarlı bir şekilde Azure'da barındırılan RHUI dışındaki bir güncelleştirme altyapısı kullanmanız gerekirse, oluşturma ve dağıtma (KLG türü) kendi görüntülerinizi göz önünde bulundurun. Bu işlem açıklanan [oluşturup Azure için Red Hat tabanlı bir sanal makine yükleme](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

* (SAP, SAP HANA için RHEL ve RHEL for SAP Business Applications RHEL) azure'da SAP PAYG RHEL görüntüleri SAP sertifika için gereken belirli RHEL alt sürüm kalır özel RHUI kanallar bağlıdır.

* İçindeki VM'ler için Azure'da barındırılan RHUI erişim sınırlıdır [Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653). Proxy kullanıyorsanız tüm VM trafiğe bir şirket içi ağ altyapısı aracılığıyla RHEL PAYG sanal makinelerin Azure RHUI erişmek kullanıcı tanımlı rotalar ayarlama gerekebilir.

## <a name="rhel-eus-and-version-locking-rhel-vms"></a>RHEL EUS ve sürüm kilitleme RHEL VM'ler
Bazı müşterilerin belirli RHEL alt yayın, RHEL Vm'lerine kilitleme isteyebilirsiniz. RHEL VM'nize belirli bir alt sürüm sürüm kilidi depoları için genişletilmiş güncelleştirme desteğini depoları işaret edecek şekilde güncelleştirerek kullanabilirsiniz. Belirli bir alt sürüme RHEL VM kilitlemek için aşağıdaki yönergeleri kullanın:

>[!NOTE]
> Bu, yalnızca EUS kullanılabilir RHEL sürümleri için geçerlidir. Bu makalenin yazıldığı sırada, bu RHEL 7.2 7.6 içerir. Daha fazla ayrıntı bulabilirsiniz [Red Hat Enterprise Linux yaşam döngüsü](https://access.redhat.com/support/policy/updates/errata) sayfası.

1. EUS olmayan depoları devre dışı bırakın:
    ```bash
    sudo yum --disablerepo='*' remove 'rhui-azure-rhel7'
    ```

1. EUS depoları ekleyin:
    ```bash
    yum --config='https://rhelimage.blob.core.windows.net/repositories/rhui-microsoft-azure-rhel7-eus.config' install 'rhui-azure-rhel7-eus'
    ```

1. Kilit releasever değişkeni:
    ```bash
    echo $(. /etc/os-release && echo $VERSION_ID) > /etc/yum/vars/releasever
    ```

    >[!NOTE]
    > Yukarıdaki yönerge RHEL ikincil sürümü için geçerli alt sürüm kilitleyecek. Yükseltme ve en son olmayan daha sonra ikincil sürümü kilitlemek için arıyorsanız, belirli bir ikincil sürüm girin. Örneğin, `echo 7.5 > /etc/yum/vars/releasever` RHEL 7.5, RHEL sürümüne kilitler

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

Örneğin, RHEL 7.4 bir eski RHEL VM görüntüsünü kullanıyorsanız (URN görüntü: `RedHat:RHEL:7.4:7.4.2018010506`), RHUI artık süresi dolmuş SSL istemci sertifikası nedeniyle bağlantı sorunları yaşar. Gördüğünüz hata görünebilir _"SSL eş reddedildi sertifikanızın süresi dolmuş olarak"_ veya _"hata: Depo için (repomd.xml) depo meta verileri alınamıyor:... Lütfen yolunu doğrulayın ve yeniden deneyin"_. Bu sorunu çözmek için lütfen aşağıdaki komutu kullanarak VM'yi RHUI istemci paketi güncelleştirin:

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

    a. Kontrol `/etc/yum.repos.d/rh-cloud.repo` dosyasını içeren bir başvuru `rhui-[1-3].microsoft.com` içinde `baseurl` , `[rhui-microsoft-azure-rhel*]` bölümü dosyasının. Varsa, yeni Azure RHUI kullanıyorsunuz.

    b. Aşağıdaki desene sahip bir konuma işaret ediyorsa `mirrorlist.*cds[1-4].cloudapp.net`, yapılandırma güncelleştirmesi gerekli değildir. Eski VM anlık görüntüsü kullanıyorsanız ve yeni Azure RHUI işaret edecek şekilde güncelleştirmeniz gerekir.

1. İçindeki sanal makineler için Azure'da barındırılan RHUI erişim sınırlıdır [Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653).

1. Yeni yapılandırmayı kullanıyorsanız, VM Azure IP aralığından bağlanır ve Azure RHUI, dosya Microsoft ya da Red Hat destek servis talebi için bağlantı kurulamıyor doğrulanmıştır.

### <a name="infrastructure-update"></a>Altyapı Güncelleştirmesi

Eylül 2016'da, biz güncelleştirilmiş bir Azure RHUI dağıtıldı. Nisan 2017'de biz eski Azure RHUI kapatın. Ayrıca, PAYG RHEL görüntüleri (veya bunların anlık görüntülerini) Eylül 2016'dan veya üzerini kullanıyorsanız, otomatik olarak yeni Azure RHUI bağlanırsınız. Ancak, eski anlık görüntüleri Vm'lerinizde varsa, aşağıdaki bölümde açıklandığı gibi Azure RHUI erişmek için kendi yapılandırmasını el ile güncelleştirmeniz gerekir.

Yeni Azure RHUI sunuculara dağıtılan [Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager/). Trafik Yöneticisi'nde, tek bir uç nokta (rhuı 1.microsoft.com) uygulama bölgesine bakılmaksızın herhangi bir VM tarafından kullanılabilir.

### <a name="manual-update-procedure-to-use-the-azure-rhui-servers"></a>Azure RHUI sunucularını kullanmak için el ile güncelleştirme yordamı
Bu yordam, yalnızca başvuru sağlanır. PAYG RHEL görüntüleri için Azure RHUI bağlanmak için doğru yapılandırma zaten var. Azure RHUI sunucularını kullanacak şekilde yapılandırmayı el ile güncelleştirmek için aşağıdaki adımları tamamlayın:

1. Curl aracılığıyla ortak anahtar imzasını indirin.

   ```bash
   curl -o RPM-GPG-KEY-microsoft-azure-release https://download.microsoft.com/download/9/D/9/9d945f05-541d-494f-9977-289b3ce8e774/microsoft-sign-public.asc
   ```

1. İndirilen anahtarın geçerlilik doğrulayın.

   ```bash
   gpg --list-packets --verbose < RPM-GPG-KEY-microsoft-azure-release
   ```

1. Çıktıyı denetleyin ve sonra `keyid` ve `user ID packet`.

   ```bash
   Version: GnuPG v1.4.7 (GNU/Linux)
   :public key packet:
           version 4, algo 1, created 1446074508, expires 0
           pkey[0]: [2048 bits]
           pkey[1]: [17 bits]
           keyid: EB3E94ADBE1229CF
   :user ID packet: "Microsoft (Release signing) <gpgsecurity@microsoft.com>"
   :signature packet: algo 1, keyid EB3E94ADBE1229CF
           version 4, created 1446074508, md5len 0, sigclass 0x13
           digest algo 2, begin of digest 1a 9b
           hashed subpkt 2 len 4 (sig created 2015-10-28)
           hashed subpkt 27 len 1 (key flags: 03)
           hashed subpkt 11 len 5 (pref-sym-algos: 9 8 7 3 2)
           hashed subpkt 21 len 3 (pref-hash-algos: 2 8 3)
           hashed subpkt 22 len 2 (pref-zip-algos: 2 1)
           hashed subpkt 30 len 1 (features: 01)
           hashed subpkt 23 len 1 (key server preferences: 80)
           subpkt 16 len 8 (issuer key ID EB3E94ADBE1229CF)
           data: [2047 bits]
   ```

1. Ortak anahtar yükleyin.

   ```bash
   sudo install -o root -g root -m 644 RPM-GPG-KEY-microsoft-azure-release /etc/pki/rpm-gpg
   sudo rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-microsoft-azure-release
   ```

1. İndirin, doğrulayın ve bir istemci RPM paketi Yöneticisi (RPM) yükleyin.

    >[!NOTE]
    >Paket sürümleri değiştirin. Azure RHUI el ile bağlanmanız durumunda, son görüntü galerisinden sağlayarak her RHEL ailesi için ve istemci paketinin en son sürümü bulabilirsiniz.

   a. İndirin.

    - RHEL 6:
        ```bash
        curl -o azureclient.rpm https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel6/rhui-azure-rhel6-2.2-74.noarch.rpm
        ```

    - RHEL 7:
        ```bash
        curl -o azureclient.rpm https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel7/rhui-azure-rhel7-2.2-74.noarch.rpm
        ```

   b. Doğrulayın.

   ```bash
   rpm -Kv azureclient.rpm
   ```

   c. Çıkış paketi imzasının Tamam olduğundan emin olmak için kontrol edin.

   ```bash
   azureclient.rpm:
       Header V3 RSA/SHA256 Signature, key ID be1229cf: OK
       Header SHA1 digest: OK (927a3b548146c95a3f6c1a5d5ae52258a8859ab3)
       V3 RSA/SHA256 Signature, key ID be1229cf: OK
       MD5 digest: OK (c04ff605f82f4be8c96020bf5c23b86c)
   ```

   d. RPM yükleyin.

    ```bash
    sudo rpm -U azureclient.rpm
    ```

1. Bitirdikten sonra sanal makineden Azure RHUI erişebildiğinizi doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar
* Azure Market PAYG görüntüden bir Red Hat Enterprise Linux VM oluşturma ve Azure'da barındırılan RHUI kullanılacak Git [Azure Marketi](https://azure.microsoft.com/marketplace/partners/redhat/).
* Azure'da Red Hat görüntülerini hakkında daha fazla bilgi için şuraya gidin [belgeleri sayfasını](./rhel-images.md).
* RHEL tüm sürümleri için Red Hat destek ilkeleri hakkında daha fazla bilgi bulunabilir [Red Hat Enterprise Linux yaşam döngüsü](https://access.redhat.com/support/policy/updates/errata) sayfası.
