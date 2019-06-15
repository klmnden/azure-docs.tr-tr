---
title: Bilmek SAP HANA (büyük örnekler) azure'da koşulları | Microsoft Docs
description: SAP HANA (büyük örnekler) azure'da koşullarını bildirin.
services: virtual-machines-linux
documentationcenter: ''
author: RicksterCDN
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 11/20/2018
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a8131bc953c2aba3c8d33f866cbbe9b1e232e168
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61481046"
---
# <a name="know-the-terms"></a>Terimleri öğrenme

Mimari ve Teknik Dağıtım Kılavuzu, birkaç ortak tanımlara yaygın olarak kullanılır. Aşağıdaki terimler ve anlamlarını dikkat edin:

- **Iaas**: Hizmet olarak altyapı.
- **PaaS**: Bir hizmet olarak Platform.
- **SaaS**: Bir hizmet olarak yazılım.
- **SAP bileşen**: Tek bir SAP uygulamayı, ERP merkezi bileşeni (ECC), Business Warehouse (BW), çözüm Yöneticisi ya da Enterprise Portal (EP) gibi. SAP bileşenleri geleneksel ABAP veya Java teknolojileri ya da bir olmayan-NetWeaver tabanlı uygulama iş nesneleri gibi temel alabilir.
- **SAP ortamı**: Bir veya daha fazla SAP bileşenleri geliştirme, kalite güvencesi, eğitim, olağanüstü durum kurtarma veya üretim gibi bir iş işlevi gerçekleştirmek için mantıksal olarak gruplandırılır.
- **SAP ortamı**: BT yatay tamamı SAP varlıkları gösterir. SAP ortamı, tüm üretim ve üretim dışı ortamlar içerir.
- **SAP sistemine**: DBMS katmanı ve uygulama katmanı sisteminin, örneğin, bir SAP ERP geliştirme birleşimi bir SAP BW sistem ve SAP CRM üretim sistemi sınayın. Azure dağıtımlar, bu iki şirket içi ile Azure arasında bölünen desteklemez. Azure'da dağıtılan şirket içinde dağıtılabilir veya onun bir SAP sistemiyle değil. Azure veya şirket içine bir SAP ortamının farklı sistemleri dağıtabilirsiniz. Örneğin, SAP CRM geliştirme dağıtabilir ve sistemleri, SAP CRM üretim sistemi şirket içi dağıtırken, Azure'da test etme. (Büyük örnekler) Azure üzerinde SAP HANA, SAP sistemlerini vm'lerde SAP uygulama katmanı ve SAP hana (büyük örnekler) Azure damgası üzerinde bir birim ilgili SAP HANA örneğinde ana bilgisayar içindir.
- **Büyük örneği damgasında**: SAP HANA TDI sertifikalı ve azure'da SAP HANA örnekleri çalıştırmak için adanmış bir donanım altyapısı yığını.
- **SAP HANA (büyük örnekler) azure'da:** Farklı Azure bölgelerindeki büyük örnek Damgalar dağıtılmış SAP HANA TDI sertifikalı donanımlarda HANA örnekleri çalıştırmak Azure teklifi resmi adı. İlgili dönem *HANA büyük örneği* kısaltması olduğundan *SAP HANA (büyük örnekler) azure'da* ve bu teknik Dağıtım Kılavuzu'nda yaygın olarak kullanılır.
- **Şirketler arası**: Siteden siteye, çok siteli veya Azure ile şirket içi veri merkezleri arasında Azure ExpressRoute bağlantısı olan bir Azure aboneliğine VM'ler dağıtıldığı bir senaryo açıklanır. Azure ortak belgeler, bu tür dağıtımlar şirketler arası senaryoları açıklanmıştır. Bağlantı için şirket içi etki alanları, şirket içi Azure Active Directory/OpenLDAP ve şirket içi DNS Azure'a genişletmek için nedenidir. Şirket içi yatay Azure aboneliklerini Azure varlıkları için genişletilir. Bu uzantı, sanal makineleri şirket içi etki alanının parçası olabilir. 

   Şirket içi etki alanının etki alanı kullanıcıları, sunuculara erişmek ve Hizmetleri (DBMS hizmetler gibi) bu sanal makineler üzerinde çalıştırın. Şirket içi sanal makineler arasında iletişim ve ad çözümlemesini dağıtılır ve Azure tarafından dağıtılan Vm'leri mümkündür. Bu senaryo, çoğu SAP varlıklar dağıtıldığı yol, tipik bir durumdur. Daha fazla bilgi için [Azure VPN ağ geçidi](../../../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) ve [Azure portalını kullanarak siteden siteye bağlantı ile sanal ağ oluşturma](../../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
- **Kiracı**: HANA büyük örneği damgasında dağıtılan bir müşteri olarak yalıtılmış bir *Kiracı.* Bir kiracı, ağ, depolama ve bilgi işlem katmanını diğer kiracılardan içinde yalıtılır. Farklı kiracıların atanan depolama ve işlem birimleri birbirine göremez veya birbirleri ile iletişim kurmak HANA büyük örnek damgası düzeyine. Bir müşteri farklı kiracıların dağıtımlarını sahip olmayı seçebilirsiniz. Daha sonra HANA büyük örnek damgası düzeyinde kiracılar arasında hiçbir iletişim yok.
- **SKU kategori**: HANA büyük örneği için aşağıdaki iki kategoriden SKU sunulmaktadır:
    - **Ben sınıf türü**: S72, S72m, S96, S144, S144m, S192, S192m ve S192xm
    - **Türü II sınıfı**: S384, S384m, S384xm, S384xxm, S576m, S576xm, S768m, S768xm ve S960m


Çeşitli ek kaynaklara bulutta, SAP iş yükünü dağıtmak nasıl kullanılabilir. Azure'da SAP HANA dağıtımı yürütmek planlıyorsanız, deneyimli ile ve Azure Iaas ilkeleri ve SAP iş yüklerini Azure ıaas dağıtımını farkında olması gerekir. Devam etmeden önce bkz [kullanım SAP çözümlerini Azure sanal makinelerinde](get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) daha fazla bilgi için. 

**Sonraki adımlar**
- Başvuru [HLI sertifika](hana-certification.md)