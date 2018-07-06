---
title: Microsoft Azure SUSE Linux vm'lerde SAP NetWeaver test etme | Microsoft Docs
description: Microsoft Azure SUSE Linux VM’lerde SAP NetWeaver’ı test etme
services: virtual-machines-linux
documentationcenter: ''
author: hermanndms
manager: jeconnoc
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: 645e358b-3ca1-4d3d-bf70-b0f287498d7a
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 09/14/2017
ms.author: hermannd
ms.openlocfilehash: cc4438a770a8092275373ccf8da9cc9951a1f906
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37858621"
---
# <a name="running-sap-netweaver-on-microsoft-azure-suse-linux-vms"></a>Microsoft Azure SUSE Linux VM’lerde SAP NetWeaver’ı çalıştırma
Bu makalede, Microsoft Azure SUSE Linux sanal makinelerinde (VM'ler) SAP NetWeaver çalıştırırken dikkate alınması gereken çeşitli şeyler açıklanmaktadır. 19 Mayıs 2016 itibariyle resmi olarak Azure SUSE Linux Vm'lerde SAP NetWeaver desteklenir. Linux sürümleri, SAP çekirdek sürümlerini ve diğer ön koşulları ile ilgili tüm ayrıntıları SAP notu 1928533 bulunabilir "azure'da SAP uygulamaları: desteklenen ürünler ve Azure VM türleri".
Linux vm'lerinde SAP hakkında daha fazla belgeleri şurada bulunabilir: [Linux sanal makinelerinde (VM'ler) SAP kullanma](get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Aşağıdaki bilgiler, bazı olası Tuzaklar önlemenize yardımcı.

## <a name="suse-images-on-azure-for-running-sap"></a>Çalışan SAP için SUSE görüntüleri azure'da
SAP NetWeaver, Azure üzerinde çalışan, SAP için SUSE Linux Enterprise Server SLES 12'yi (SPx) veya SLES kullanın - SAP notu 1928533 Ayrıca bkz. Azure marketi'ndeki ("SLES 11 SP3 SAP CAL için") özel bir SUSE görüntüsü olduğu halde görüntü genel kullanım için tasarlanmamıştır. Bu görüntü kullanmayın, için ayrıldığından [SAP Cloud Appliance Library](https://cal.sap.com/) çözüm.  

Azure'da tüm yüklemeleri için Azure Resource Manager dağıtım Çerçevesi'ni kullanmanız gerekir. SUSE SLES görüntüler ve sürümleri için Azure PowerShell veya Azure komut satırı arabirimi (CLI) kullanarak aramak için aşağıda gösterilen komutlarını kullanın. Ardından çıktı, örneğin, işletim sistemi görüntüsü yeni bir SUSE Linux VM dağıtmak için bir JSON şablonunu tanımlamak için kullanabilirsiniz.
Şu PowerShell komutlarını veya sonraki sürümler 1.0.1 Azure PowerShell sürümü için geçerlidir.

SAP yüklemeleri için standart SLES görüntüleri kullanmak hala mümkün olsa da yeni SLES SAP görüntülerini kullanmak için önerilir. Bu görüntüler Azure görüntü galerisinde kullanıma sunuldu. Karşılık gelen üzerinde bu görüntüleri hakkında daha fazla bilgi bulunabilir [Azure Market sayfasını]( https://azuremarketplace.microsoft.com/en-us/marketplace/apps/SUSE.SLES-SAP ) veya [hakkında SLES SAP için SUSE SSS web sayfası]( https://www.suse.com/products/sles-for-sap/frequently-asked-questions/ ).


* SUSE dahil olmak üzere, mevcut yayımcıların arayın:
  
   ```
   PS  : Get-AzureRmVMImagePublisher -Location "West Europe"  | where-object { $_.publishername -like "*US*"  }
   CLI : azure vm image list-publishers westeurope | grep "US"
   ```
* SUSE mevcut sunduğu arayın:
  
   ```
   PS  : Get-AzureRmVMImageOffer -Location "West Europe" -Publisher "SUSE"
   CLI : azure vm image list-offers westeurope SUSE
   ```
* SUSE SLES teklifleri arayın:
  
   ```
   PS  : Get-AzureRmVMImageSku -Location "West Europe" -Publisher "SUSE" -Offer "SLES"
   PS  : Get-AzureRmVMImageSku -Location "West Europe" -Publisher "SUSE" -Offer "SLES-SAP"
   CLI : azure vm image list-skus westeurope SUSE SLES
   CLI : azure vm image list-skus westeurope SUSE SLES-SAP
   ```
* SLES SKU belirli bir sürümünü arayın:
  
   ```
   PS  : Get-AzureRmVMImage -Location "West Europe" -Publisher "SUSE" -Offer "SLES" -skus "12-SP2"
   PS  : Get-AzureRmVMImage -Location "West Europe" -Publisher "SUSE" -Offer "SLES-SAP" -skus "12-SP2"
   CLI : azure vm image list westeurope SUSE SLES 12-SP2
   CLI : azure vm image list westeurope SUSE SLES-SAP 12-SP2
   ```

## <a name="installing-walinuxagent-in-a-suse-vm"></a>WALinuxAgent SUSE VM ile yükleme
WALinuxAgent adlı aracı Azure Marketi'nde bulunan SLES görüntüleri bir parçasıdır. (Örneğin bir SLES işletim sistemi sanal sabit disk (VHD) şirket içinden karşıya yüklenirken el ile) yükleme hakkında daha fazla bilgi için bkz:

* [OpenSUSE](http://software.opensuse.org/package/WALinuxAgent)
* [Azure](../../linux/endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [SUSE](https://www.suse.com/communities/blog/suse-linux-enterprise-server-configuration-for-windows-azure/)

## <a name="sap-enhanced-monitoring"></a>SAP Gelişmiş"izleme"
SAP Gelişmiş"izleme", Azure üzerinde SAP çalışmasına zorunlu bir önkoşuldur. Onay ayrıntıları SAP 2191498 "SAP Linux ile Azure: Gelişmiş izlemesini" unutmayın.

## <a name="attaching-azure-data-disks-to-an-azure-linux-vm"></a>Azure Linux VM'LERİNİ Azure veri diski ekleme
Hiçbir zaman bağlama Azure veri diskleri cihaz kimliğini kullanarak bir Azure Linux VM Bunun yerine, evrensel benzersiz tanımlayıcı (UUID) kullanın. Bağlama Azure veri diskleri için grafik araçları örneğin kullanırken dikkatli olun. /Etc/fstab girişleri denetleyin.

Cihaz kimliği ile değişiklik yapmış olabilir ve Azure VM'yi önyükleme işleminin ardından askıda sorunudur. Sorunu gidermek için /etc/fstab nofail parametresi ekleyebilirsiniz. Ancak, çünkü uygulamaları önce bağlama noktası olarak kullanabilir ve bir dış Azure veri diski önyüklemesi sırasında bağlı değildi durumunda kök dosya sistemine yazabilirsiniz nofail ile dikkatli olun.

Bağlama UUID aracılığıyla tek özel durumu izleyen bölümünde açıklandığı gibi bir işletim sistemi diski, sorun giderme amacıyla, iliştiriliyor.

## <a name="troubleshooting-a-suse-vm-that-isnt-accessible-anymore"></a>Artık erişilebilir olmayan bir SUSE VM sorunlarını giderme
Burada Azure SUSE VM'de önyükleme işleminin (örneğin, disk bağlamak için ilişkili hatanın ile) yanıt vermemeye başlıyor durumlar olabilir. Bu sorun, Azure portalında Azure sanal makineler v2 için önyükleme Tanılama özelliğini kullanarak doğrulayabilirsiniz. Daha fazla bilgi için [önyükleme tanılaması](https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/).

Sorunu çözmek için bir işletim sistemi bozuk VM'den Azure üzerinde başka bir SUSE sanal makine diski yoludur. Ardından sonraki bölümde açıklanan şekilde /etc/fstab düzenleme veya ağ udev kuralları kaldırma gibi uygun değişiklikler yapın.

Dikkate alınması gereken önemli bir unsur vardır. Aynı Azure Market görüntüsünden (örneğin, SLES 11 SP4) birkaç SUSE Vm'leri dağıtma, işletim sistemi diski, her zaman aynı UUID ile bağlanmasını neden olur. İki özdeş Uuıd'lerini aynı Azure Market görüntüsü sonuçları kullanarak dağıtılmış farklı bir VM'den bir işletim sistemi diski eklemek için bu nedenle, UUID kullanma. İki özdeş UUID'ler neden VM sorun giderme için ekli ve bozuk işletim sistemi diskinden özgün işletim sistemi diski yerine önyükleme kullanılır.

Sorunları önlemek için iki yolu vardır:

* Farklı Azure Market görüntüsü için sorun giderme sanal makinesi (örneğin, SLES 11 SPx SLES 12 yerine) kullanın.
* Bozuk işletim sistemi diskini başka bir VM'den UUID--Kullan'ı kullanarak iliştirme başka bir.

## <a name="uploading-a-suse-vm-from-on-premises-to-azure"></a>Şirket içinden azure'a SUSE VM karşıya yükleme
SUSE VM şirket içinden azure'a yüklemek için aşağıdaki adımları açıklaması için bkz: [Azure için SLES veya openSUSE sanal makine hazırlama](../../linux/suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Sonunda sağlamayı kaldırma adımı olmadan bir sanal makine (örneğin, canlı bir mevcut SAP yükleme yanı için ana bilgisayar adı) yüklemek istiyorsanız, aşağıdakileri denetleyin:

* İşletim sistemi diski UUID ve cihaz kimliğini kullanarak bağlı olduğundan emin olun Yalnızca içinde /etc/fstab UUİD'si değiştirme işletim sistemi diski için yeterli değil. Ayrıca, /boot/grub/menu.lst düzenleyerek veya YaST aracılığıyla önyükleyicinin uyum unutmayın.
* SUSE işletim sistemi diski VHDX biçimini kullanın ve Azure'a yükleme için VHD'ye dönüştürme, ağ aygıtı için eth1 eth0 değişiklikleri olasıdır. Daha sonra Azure'da önyükleme, sorunları önlemek için geri eth0 için açıklandığı gibi değiştirmek [kopyalanan SLES 11 VMware içinde eth0 düzeltme](https://dartron.wordpress.com/2013/09/27/fixing-eth1-in-cloned-sles-11-vmware/).

Ne makalesinde açıklanan ek olarak, bu dosya kaldırmanızı öneririz:

   /lib/udev/rules.d/75-persistent-net-generator.rules

Azure Linux birden çok NIC yok sürece olası sorunları önlemek için aracısını (waagent) da yükleyebilirsiniz.

## <a name="deploying-a-suse-vm-on-azure"></a>Azure üzerinde bir SUSE sanal makine dağıtma
Yeni Azure Resource Manager modelinde JSON şablonu dosyalarını kullanarak yeni SUSE sanal makineleri oluşturmanız gerekir. JSON şablon dosyası oluşturulduktan sonra PowerShell alternatif olarak, aşağıdaki CLI komutunu kullanarak bir VM dağıtabilirsiniz:

   ```
   azure group deployment create "<deployment name>" -g "<resource group name>" --template-file "<../../filename.json>"

   ```
JSON şablonu dosyaları hakkında daha fazla bilgi için bkz. [Azure Resource Manager şablonları yazma](../../../resource-group-authoring-templates.md) ve [Azure hızlı başlangıç şablonları](https://azure.microsoft.com/documentation/templates/).

CLI ve Azure Resource Manager hakkında daha fazla bilgi için bkz: [Mac, Linux ve Windows Azure Resource Manager ile Azure CLI kullanmak](../../../xplat-cli-azure-resource-manager.md).

## <a name="sap-license-and-hardware-key"></a>SAP lisans ve donanım anahtarı
Resmi Azure SAP sertifikaları için SAP lisansı için kullanılan SAP donanım anahtarına hesaplamak için yeni bir mekanizma sunulmuştur. SAP çekirdek yapmak için uyarlanmış olması gerekiyordu yeni algoritma kullanın. Linux için eski SAP çekirdek sürümleri, bu kod değişikliği içermiyordu. Bu nedenle, bazı durumlarda (örneğin, Azure VM yeniden boyutlandırma), SAP donanım anahtarı değiştirildi ve SAP lisansı olan artık geçerli. Bir çözüm ile daha yeni SAP Linux çekirdeklerinin sağlanır.  Ayrıntılı SAP çekirdek düzeltme ekleri, SAP notu 1928533 belgelenmiştir.

## <a name="suse-sapconf-package--tuned-adm"></a>SUSE sapconf paket / ayarlanmış adm
"SAP özgü ayarları kümesini yöneten sapconf" adlı bir paketi SUSE sağlar. Bu paket yaptığı ve nasıl yüklemek ve kullanmak hakkında daha fazla bilgi için bkz: [sapconf SAP sistemlerini bir SUSE Linux Enterprise Server hazırlamak için kullanarak](https://www.suse.com/communities/blog/using-sapconf-to-prepare-suse-linux-enterprise-server-to-run-sap-systems/) ve [sapconf nedir veya SUSE Linux Enterprise hazırlamayı öğrenin SAP sistemlerini çalıştırmak için sunucu? ](http://scn.sap.com/community/linux/blog/2014/03/31/what-is-sapconf-or-how-to-prepare-a-suse-linux-enterprise-server-for-running-sap-systems).

Bu arada, 'sapconf - ayarlanmış adm' yerini alan yeni bir aracı yoktur. Bir, iki bağlantı izleyerek bu aracı hakkında daha fazla bilgi bulabilirsiniz:

- SLES belgeler 'ayarlanmış adm' profili sap-hana hakkında [burada](https://www.suse.com/documentation/sles-for-sap-12/book_s4s/data/sec_saptune.html) 

- 'Ayarlanmış-adm'-'ile SAP iş yükleri için ayarlama sistemleri bulunabilir [burada](https://www.suse.com/documentation/sles-for-sap-12/pdfdoc/book_s4s/book_s4s.pdf) bölümde 6.2

## <a name="nfs-share-in-distributed-sap-installations"></a>Dağıtılmış SAP yüklemelerde NFS paylaşım
Dağıtılmış bir yüklemede--Örneğin, veritabanı ve SAP uygulama sunucuları ayrı VM'ler--yüklemek istediğiniz varsa, ağ dosya sistemi (NFS) aracılığıyla /sapmnt dizin paylaşabilirsiniz. NFS paylaşım için /sapmnt oluşturduktan sonra yükleme adımlarını ile ilgili sorunlar varsa, "no_root_squash" paylaşımı için ayarlanmış olup olmadığını denetleyin.

## <a name="logical-volumes"></a>Mantıksal birimler
Birini (örneğin, SAP veritabanı için), birden çok Azure veri diski üzerinde büyük bir mantıksal birim gerekirse geçmişte, bu Linux mantıksal birim Yöneticisi (LVM) tam olarak henüz Azure'da doğrulanmadı beri MDADM RAID yönetim aracı kullanmanız önerilir. Mdadm kullanarak azure'da Linux RAID ayarlama konusunda bilgi almak için bkz: [Linux'ta yazılım RAID yapılandırma](../../linux/configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Bu sırada, başlangıç Mayıs 2016 itibariyle Linux mantıksal birim yöneticisi Azure'da tam olarak desteklenir ve MDADM alternatif olarak kullanılabilir. Azure'da LVM ile ilgili daha fazla bilgi için okuyun:  
[Azure'da Linux sanal makinesi üzerinde LVM'yi yapılandırma](../../linux/configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="azure-suse-repository"></a>Azure SUSE deposu
Standart Azure SUSE depoya erişim ile ilgili bir sorun varsa, sıfırlamak için bir komutu kullanabilirsiniz. Bu tür sorunları, bir Azure bölgesinde özel bir işletim sistemi görüntüsü oluşturup sonra bu özel işletim sistemi görüntüsü üzerinde temel alan yeni Vm'leri dağıtmak için farklı bir Azure bölgesine resmi Kopyala gerçekleşebilir. VM içindeki aşağıdaki komutu çalıştırın:

   ```
   service guestregister restart
   ```

## <a name="gnome-desktop"></a>Gnome Masaüstü
Gnome masaüstü bir SAP GUI dahil olmak üzere tek bir VM içinde--tam bir SAP tanıtım sistemini yüklemek için kullanmak istiyorsanız Azure SLES görüntülerde yüklemek için bu ipucu tarayıcı ve SAP Yönetim Konsolu--kullanın:

   SLES 11 için:

   ```
   zypper in -t pattern gnome
   ```

   SLES 12:

   ```
   zypper in -t pattern gnome-basic
   ```

## <a name="sap-support-for-oracle-on-linux-in-the-cloud"></a>Oracle bulut Linux'ta SAP desteği
Sanallaştırılmış ortamlarda Oracle Linux'ta destek bir kısıtlama yoktur. Bu destek kısıtlama Azure'a özel bir konu olmamasına karşın, anlaşılması önemlidir. SAP SUSE veya Red Hat Azure gibi genel bulutta Oracle desteklemez. Sırada çalıştırılması, azure'da Oracle DB tam Oracle Linux'ta SAP tarafından desteklenir (SAP notu 1928533 bakın). Diğer birleşimler gerekiyorsa, Oracle doğrudan başvurun.

