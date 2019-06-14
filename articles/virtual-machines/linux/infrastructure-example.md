---
title: Örnek Azure altyapısı gözden geçirme | Microsoft Docs
description: Bir örnek altyapısını Azure'a dağıtmak için önemli tasarım ve uygulama yönergeleri hakkında bilgi edinin.
documentationcenter: ''
services: virtual-machines-linux
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 281fc2c0-b533-45fa-81a3-728c0049c73d
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 00357641f51be703d2e5c52c5b9cc6187ce05ff6
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60543078"
---
# <a name="example-azure-infrastructure-walkthrough-for-linux-vms"></a>Linux VM'ler için örnek Azure altyapı Kılavuzu
Bu makalede örnek uygulama altyapısı oluşturmaya gösterilmektedir. Biz, yönergeleri ve adlandırma kuralları, kullanılabilirlik kümeleri, sanal ağlar ve yük Dengeleyiciler kararları bir araya basit bir çevrimiçi mağaza için bir altyapı tasarımı ve gerçekte sanal makinelerinizi (VM) dağıtma açıklamaktadır.

## <a name="example-workload"></a>Örnek iş yükü
Adventure Works Cycles, azure'da oluşan bir çevrimiçi mağaza uygulaması derleme ister:

* Bir web katmanı ön uç istemcisi çalıştıran iki ngınx sunucu
* Veri ve uygulama katmanında orders işleme iki ngınx sunucu
* Ürün verileri ve siparişler bir veritabanı katmanında depolamak için bir parçalı kümenin iki MongoDB sunucuları parçası
* Müşteri hesaplar ve bir kimlik doğrulama katmanı sağlayıcıları için iki Active Directory etki alanı denetleyicisi
* Tüm sunucular iki alt ağlarında bulunur:
  * web sunucuları için ön uç bir alt ağ 
  * uygulama sunucuları, MongoDB küme ve etki alanı denetleyicileri için bir arka uç alt ağı

![Uygulama altyapısı için farklı bir katman diyagramı](./media/infrastructure-example/example-tiers.png)

Güvenli gelen web trafiğini gerekir yük dengeli web sunucular arasında gezinirken müşteriler çevrimiçi mağaza. Sipariş biçiminde HTTP trafiği işleme, Web sunucuları-uygulama sunucuları arasında dengeli yük gerekir ister. Ayrıca, altyapı yüksek kullanılabilirlik için tasarlanması gerekir.

Sonuçta elde edilen tasarım eklemeniz gerekir:

* Bir Azure aboneliği ve hesabı
* Tek bir kaynak grubu
* Azure Yönetilen Diskleri
* İki alt ağa sahip bir sanal ağ
* Benzer bir role sahip VM'ler için kullanılabilirlik kümeleri
* Sanal makineler

Tüm adlandırma kurallarına yukarıda izleyin:

* Adventure Works Cycles kullandığı **[BT iş yükü]-[konumu]-[Azure resource]** öneki olarak
  * Bu örnekte, "**azos**" (Azure çevrimiçi Store), BT iş yükü adıdır ve "**kullanın**" (Doğu ABD 2) konumdur
* Sanal ağları kullanın AZOS kullanım VN<strong>[sayı]</strong>
* Kullanılabilirlik kümelerini kullanın azos-kullanın-olarak- **[rol]**
* Sanal makine adları azos kullanın-kullanın-vm - **[vmname]**

## <a name="azure-subscriptions-and-accounts"></a>Azure aboneliklerini ve hesaplarını
Adventure Works Cycles, bu BT iş yükü için fatura bilgilerini sağlamak için Adventure Works Enterprise aboneliğinizin adlı Enterprise aboneliğini kullanıyor.

## <a name="storage"></a>Depolama
Adventure Works Cycles, Azure yönetilen diskler kullanması gerektiğini belirledi. Vm'leri oluştururken, her iki depolama alanı kullanılabilir depolama katmanları kullanılır:

* **Standart depolama** web sunucuları, uygulama sunucuları ve etki alanı denetleyicileri ve veri diskleriyle.
* **Premium depolama** MongoDB parçalı küme sunucuları ve veri diskleriyle.

## <a name="virtual-network-and-subnets"></a>Sanal ağ ve alt ağlar
Sanal ağ Adventure iş döngüleri şirket içi ağınıza bağlantı gerekmediği için yalnızca bulutta yer alan bir sanal ağda karar verdi.

Azure portalını kullanarak aşağıdaki ayarlara sahip bir yalnızca bulut sanal ağ oluşturdukları:

* Ad: AZOS-USE-VN01
* Konum: Doğu ABD 2
* sanal ağ adres alanı: 10.0.0.0/8
* İlk alt ağı:
  * Ad: FrontEnd
  * Adres alanı: 10.0.1.0/24
* İkinci alt ağı:
  * Ad: BackEnd
  * Adres alanı: 10.0.2.0/24

## <a name="availability-sets"></a>Kullanılabilirlik kümeleri
Tüm dört katman kendi çevrimiçi depolama yüksek kullanılabilirliğini sürdürmek için Adventure Works Cycles dört kullanılabilirlik kümeleri verdi:

* **web olarak azos kullanım** web sunucuları için
* **uygulama olarak azos kullanım** uygulama sunucuları için
* **db olarak azos kullanım** MongoDB parçalı kümesindeki sunucular için
* **dc olarak azos kullanım** etki alanı denetleyicileri

## <a name="virtual-machines"></a>Sanal makineler
Şu adları kendi Azure Vm'leri için Adventure Works Cycles verdi:

* **Kullanım vm web01 azos** ilk web sunucusu
* **Kullanım vm web02 azos** ikinci web sunucusunun
* **Kullanım vm app01 azos** ilk uygulama sunucusu
* **Kullanım vm app02 azos** ikinci uygulama sunucusu
* **Kullanım vm db01 azos** kümedeki ilk MongoDB sunucu için
* **Kullanım vm db02 azos** kümedeki ikinci MongoDB sunucu için
* **Kullanım vm dc01 azos** ilk etki alanı denetleyicisi
* **Kullanım vm dc02 azos** ikinci etki alanı denetleyicisi

Sonuçta elde edilen yapılandırması aşağıda verilmiştir.

![Azure'da dağıtılan son uygulama altyapısı](./media/infrastructure-example/example-config.png)

Bu yapılandırmayı içerir:

* Yalnızca bulutta yer alan bir sanal ağ ile iki alt ağa (ön uç ve arka uç)
* Standart ve Premium diskler kullanarak Azure yönetilen diskler
* Bir çevrimiçi mağaza, her bir katman için dört kullanılabilirlik kümeleri
* Sanal makineler için dört katmanı
* Web sunucularına HTTPS tabanlı web trafiği Internet'ten bir dış yük dengeli küme
* Uygulama sunucuları için web sunucularından şifrelenmemiş web trafiği için bir iç yük dengeli
* Tek bir kaynak grubu
