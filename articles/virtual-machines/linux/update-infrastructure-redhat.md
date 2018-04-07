---
title: Red Hat güncelleştirme altyapısı | Microsoft Docs
description: Microsoft Azure isteğe bağlı Red Hat Enterprise Linux örneklerinin Red Hat güncelleştirme altyapısı hakkında bilgi edinin
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
ms.date: 04/02/2018
ms.author: borisb
ms.openlocfilehash: b69cc226ca5b4f48747b033e0da5e7f991be112e
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="red-hat-update-infrastructure-for-on-demand-red-hat-enterprise-linux-vms-in-azure"></a>Red Hat güncelleştirme altyapısı için isteğe bağlı Red Hat Enterprise Linux VM'ler için Azure'da
 [Red Hat güncelleştirme altyapısını](https://access.redhat.com/products/red-hat-update-infrastructure) (RHUI) Red Hat barındırılan depo içeriğini yansıtmak, özel depoları ile Azure özgü içerik oluşturmak ve son kullanıcı VM'ler için kullanılabilir hale getirmek için Azure gibi bulut sağlayıcıları sağlar.

Red Hat Enterprise Linux (RHEL) Kullandıkça Öde (PAYG) görüntüleri Azure RHUI erişmek için önceden yapılandırılmış olarak sunulur. Ek bir yapılandırma gerekmez. En son güncelleştirmeleri almak için şunu çalıştırın `sudo yum update` RHEL örneğinizi hazır olduktan sonra. Bu hizmet RHEL PAYG yazılım ücretleri bir parçası olarak dahil edilir.

## <a name="important-information-about-azure-rhui"></a>Azure RHUI hakkında önemli bilgiler
* Azure RHUI şu anda yalnızca en son alt sürüm her RHEL ailesi içinde (RHEL6 veya RHEL7) destekler. Son ikincil sürüme RHUI için bağlı bir RHEL VM örneği yükseltmek için çalıştırmanız `sudo yum update`.

    Örneğin, bir VM RHEL 7.2 PAYG görüntüden sağlamak ve çalıştırırsanız `sudo yum update`, RHEL 7.4 VM (en son ikincil sürüm RHEL7 ailesindeki) ile biter.

    Bu davranışı önlemek için açıklandığı gibi kendi görüntünüzü oluşturmak gereken [oluşturma ve karşıya yükleme bir Red Hat tabanlı sanal makine için Azure](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) makalesi. Farklı güncelleştirme altyapısını bağlanması gereken sonra ([doğrudan Red Hat içerik teslim sunucuları](https://access.redhat.com/solutions/253273) veya [Red Hat uydu sunucu](https://access.redhat.com/products/red-hat-satellite)).

* Azure barındırılan RHUI erişimi RHEL PAYG görüntü fiyatına dahil edilir. Azure barındırılan RHUI PAYG RHEL VM'den kaydı varsa, sanal makine VM Getir bilgisayarınızı-kendi-lisans (KLG) türünü dönüştürmez. Başka bir güncelleştirme kaynağı ile aynı VM kaydolursanız, neden olabilecek _dolaylı_ çift giderler. Azure RHEL yazılım ücret ilk kez ücret ödersiniz. İkinci kez önceden satın alınan Red Hat abonelikler için ücret ödersiniz. Tutarlı bir şekilde Azure barındırılan RHUI dışında bir güncelleştirme altyapısı kullanmanız gerekiyorsa, oluşturma ve dağıtma (KLG-türü) görüntüleri göz önünde bulundurun. Bu işlem açıklanan [oluşturma ve karşıya yükleme bir Red Hat tabanlı sanal makine için Azure](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

* Azure (SAP HANA için RHEL) ve SAP Business uygulamaları için RHEL RHEL PAYG görüntülerinin iki sınıf SAP sertifika için gereken belirli RHEL alt sürüm kaldığı ayrılmış RHUI kanalları bağlıdır. 

* Azure barındırılan RHUI erişimi sınırlıdır VM [Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653). Proxy iseniz tüm VM trafik bir şirket içi ağ altyapısı aracılığıyla Azure RHUI erişmek RHEL PAYG VM'ler için kullanıcı tanımlı yollar ayarlamanız gerekebilir.

### <a name="the-ips-for-the-rhui-content-delivery-servers"></a>RHUI içerik teslim sunucuları için IP'leri

RHUI RHEL isteğe bağlı görüntüleri kullanılabildiği tüm bölgelerde kullanılabilir. Şu anda listelenen tüm genel bölgeler içeren [Azure durum Panosu](https://azure.microsoft.com/status/) sayfa, Azure ABD devlet kurumları ve Microsoft Azure Almanya bölgeleri. 

Daha fazla RHEL PAYG Vm'lerden erişimi kısıtlamak için bir ağ yapılandırması kullanıyorsanız, emin olun aşağıdaki IP'leri verilir için `yum update` sizin gerçekten ortamına bağlı olarak çalışmak için: 

```
# Azure Global
13.91.47.76
40.85.190.91
52.187.75.218
52.174.163.213
52.237.203.198

# Azure US Government
13.72.186.193

# Azure Germany
51.5.243.77
51.4.228.145
```

## <a name="rhui-azure-infrastructure-update"></a>RHUI Azure Altyapı Güncelleştirmesi

Eylül 2016'da, güncelleştirilmiş bir Azure RHUI dağıtıldı. Nisan 2017 ' eski Azure RHUI kapatın. RHEL PAYG resimlerinin (ya da kendi anlık görüntüler) Eylül 2016 veya üzeri kullanıyorsanız, otomatik olarak yeni Azure RHUI bağlandığınız. Ancak, eski anlık görüntüleri Vm'leriniz varsa, aşağıdaki bölümde açıklandığı gibi Azure RHUI erişmek için kendi yapılandırmasını el ile güncelleştirmeniz gerekir.

Yeni Azure RHUI sunucuları ile dağıtılan [Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager/). Trafik Yöneticisi'nde, tek bir uç noktası (rhui-1.microsoft.com) bölge bağımsız olarak herhangi bir VM tarafından kullanılabilir. 

### <a name="troubleshoot-connection-problems-to-azure-rhui"></a>Azure RHUI için bağlantı sorunlarını giderme
Azure RHUI Azure RHEL PAYG VM'den bağlanmakta sorunlarla karşılaşıyorsanız, şu adımları izleyin:

1. Azure RHUI uç noktası için VM yapılandırmasını inceleyin:

    a. Olup olmadığını denetleyin `/etc/yum.repos.d/rh-cloud.repo` dosyasını içeren bir başvuru `rhui-[1-3].microsoft.com` içinde `baseurl` , `[rhui-microsoft-azure-rhel*]` dosyasının bölümü. Destekliyorsa, yeni Azure RHUI kullanıyorsunuz.

    b. Aşağıdaki desende bir konuma işaret ediyorsa `mirrorlist.*cds[1-4].cloudapp.net`, yapılandırma güncelleştirmesi gerekli değildir. Eski VM anlık görüntüsü kullanıyorsanız ve yeni Azure RHUI işaret edecek şekilde güncelleştirmeniz gerekir.

2. Azure barındırılan RHUI erişimi Vm'lere [Azure veri merkezi IP aralıkları] içinde sınırlı (https://www.microsoft.com/download/details.aspx?id=41653).
 
3. Yeni yapılandırmayı kullanıyorsanız, VM Azure IP aralığında bağlanır ve Azure RHUI, dosya Microsoft ya da Red Hat destek servis talebi için bağlantı kuramıyor doğrulanmıştır.

### <a name="manual-update-procedure-to-use-the-azure-rhui-servers"></a>Azure RHUI sunucuları kullanmak üzere el ile güncelleştirme yordamı
Bu yordam yalnızca başvuru amacıyla sağlanır. RHEL PAYG görüntüleri Azure RHUI bağlamak için doğru yapılandırma zaten var. El ile Azure RHUI sunucularını kullanacak şekilde yapılandırmasını güncelleştirmek için aşağıdaki adımları tamamlayın:

1. Ortak anahtar imza curl aracılığıyla indirin.

   ```bash
   curl -o RPM-GPG-KEY-microsoft-azure-release https://download.microsoft.com/download/9/D/9/9d945f05-541d-494f-9977-289b3ce8e774/microsoft-sign-public.asc 
   ```

2. İndirilen anahtar geçerliliğini doğrulayın.

   ```bash
   gpg --list-packets --verbose < RPM-GPG-KEY-microsoft-azure-release
   ```

3. Çıktı denetleyin ve ardından doğrulayın `keyid` ve `user ID packet`.

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

4. Ortak anahtar yükleyin.

   ```bash
   sudo install -o root -g root -m 644 RPM-GPG-KEY-microsoft-azure-release /etc/pki/rpm-gpg
   sudo rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-microsoft-azure-release
   ```

5. İndirin, doğrulamak ve istemci RPM Paket Yöneticisi (RPM) yükleyin.
    
    >[!NOTE]
    >Paket sürümlerinin değiştirin. El ile Azure RHUI bağlanıyorsanız, Galeri son görüntüden sağlama tarafından her RHEL ailesi için istemci paketi en son sürümünü bulabilirsiniz.
  
   a. Karşıdan yükleyin. 
   
    - RHEL 6 için:
        ```bash
        curl -o azureclient.rpm https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel6/rhui-azure-rhel6-2.1-32.noarch.rpm 
        ```
    
    - RHEL 7 için:
        ```bash
        curl -o azureclient.rpm https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel7/rhui-azure-rhel7-2.1-19.noarch.rpm  
        ```

   b. Doğrulayın.

   ```bash
   rpm -Kv azureclient.rpm
   ```

   c. Paketin imzasını Tamam olduğundan emin olmak için çıktı denetleyin.

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

6. Bitirdikten sonra sanal makineden Azure RHUI erişebildiğinizden emin olun.

## <a name="next-steps"></a>Sonraki adımlar
Azure Market PAYG görüntüden bir Red Hat Enterprise Linux VM oluşturun ve Azure barındırılan RHUI kullanmak için Git [Azure Marketi](https://azure.microsoft.com/marketplace/partners/redhat/). 
