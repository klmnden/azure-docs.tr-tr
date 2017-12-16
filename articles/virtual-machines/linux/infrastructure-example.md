---
title: "Örnek Azure altyapı gözden geçirme | Microsoft Docs"
description: "Bir örnek altyapısını Azure'a dağıtmak için anahtar tasarım ve uygulama yönergeleri hakkında bilgi edinin."
documentationcenter: 
services: virtual-machines-linux
author: iainfoulds
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 281fc2c0-b533-45fa-81a3-728c0049c73d
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ae7df08e7502fbfd500944f89a3fa6ee4806522a
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/16/2017
---
# <a name="example-azure-infrastructure-walkthrough-for-linux-vms"></a>Linux VM'ler için örnek Azure altyapı gözden geçirme
Bu makalede örnek uygulama altyapısı oluşturmaya anlatılmaktadır. Biz yönergeleri ve adlandırma kuralları, kullanılabilirlik kümeleri, sanal ağlar ve yük dengeleyici kararları bir araya getirir basit bir çevrimiçi mağaza için bir altyapı tasarlama ve gerçekte sanal makineleri (VM'ler) dağıtma ayrıntılı olarak açıklanmaktadır.

## <a name="example-workload"></a>Örnek iş yükü
Adventure Works Cycles, azure'da oluşan bir çevrimiçi mağaza uygulama oluşturmak ister:

* Bir web katmanı ön uç istemcisi çalıştıran iki nginx sunucuları
* Veri ve uygulama katmanına siparişler işleme iki nginx sunucuları
* Ürün veri ve siparişler bir veritabanı katmanı depolamak için bir parçalı küme iki MongoDB sunucuları parçası
* Müşteri hesapları ve bir kimlik doğrulama katmanı sağlayıcıları için iki Active Directory etki alanı denetleyicisi
* Tüm sunucular iki alt ağ içinde bulunur:
  * web sunucuları için ön uç bir alt ağ 
  * uygulama sunucuları, MongoDB küme ve etki alanı denetleyicileri için bir arka uç alt ağ

![Uygulama altyapısı için farklı katmanlara diyagramı](./media/infrastructure-example/example-tiers.png)

Güvenli gelen web trafiği gerekir yük dengeli web sunucular arasında müşteriler çevrimiçi mağaza gezinirken. Sunucular-uygulama sunucuları arasında dengeli yük gerekir Web trafiği HTTP biçiminde işlem sırası ister. Ayrıca, altyapı yüksek kullanılabilirlik için tasarlanmış olması gerekir.

Sonuçta elde edilen tasarım eklemeniz gerekir:

* Bir Azure aboneliği ve hesabı
* Tek bir kaynak grubu
* Azure Yönetilen Diskleri
* İki alt ağa sahip bir sanal ağ
* Benzer bir rolü olan VM'ler için kullanılabilirlik kümeleri
* Sanal makineler

Tüm yukarıdaki adlandırma kurallarına izleyin:

* Adventure Works Cycles kullandığı **[BT iş yükü]-[konum]-[Azure kaynak]** öneki olarak
  * Bu örneğin, "**azos**" (Azure çevrimiçi) deposudur BT iş yükü adı ve "**kullanmak**" (Doğu ABD 2) konumudur
* Sanal ağları kullanın AZOS kullanım VN**[sayı]**
* Kullanılabilirlik kümeleri kullanan azos-kullanın-olarak-**[rol]**
* Sanal makine adları azos kullanın-kullanın-vm -**[vmname]**

## <a name="azure-subscriptions-and-accounts"></a>Azure abonelikleri ve hesapları
Adventure Works Cycles, bu BT iş yükü için fatura sağlamak için Adventure Works Kurumsal aboneliği adlı kurumsal aboneliğini kullanıyor.

## <a name="storage"></a>Depolama
Adventure Works Cycles Azure yönetilen diskleri kullanması gerektiğini belirledi. Sanal makineleri oluştururken, her iki depolama kullanılabilir depolama katmanları kullanılır:

* **Standart depolama** web sunucuları, uygulama sunucuları ve etki alanı denetleyicileri ve kendi veri diskleri için.
* **Premium depolama** MongoDB parçalı küme sunucuları ve kendi veri diskleri için.

## <a name="virtual-network-and-subnets"></a>Sanal ağ ve alt ağlar
Sanal ağ Adventure iş döngüsü şirket ağına sürekli bağlantı gerekmediği için bir yalnızca bulut sanal ağda karar verdi.

Azure Portalı'nı kullanarak aşağıdaki ayarlara sahip bir yalnızca bulut sanal ağ oluşturma:

* Name: Kullanım AZOS VN01
* Konumu: Doğu ABD 2
* Sanal ağ adres alanı: 10.0.0.0/8
* İlk alt ağ:
  * Ad: ön uç
  * Adres alanı: 10.0.1.0/24
* İkinci alt ağ:
  * Ad: arka uç
  * Adres alanı: 10.0.2.0/24

## <a name="availability-sets"></a>Kullanılabilirlik kümeleri
Çevrimiçi depolarındaki tüm dört katmanların yüksek kullanılabilirliği sürdürmek için Adventure Works Cycles dört kullanılabilirlik kümeleri hakkında karar:

* **web olarak azos kullanım** web sunucuları için
* **uygulama olarak azos kullanım** uygulama sunucuları için
* **db olarak azos kullanım** MongoDB parçalı kümesindeki sunucular için
* **dc olarak azos kullanım** etki alanı denetleyicileri

## <a name="virtual-machines"></a>Sanal makineler
Adventure Works Cycles Azure Vm'leri için aşağıdaki adlar karar:

* **Kullanım vm web01 azos** ilk web sunucusu için
* **Kullanım vm web02 azos** ikinci web sunucusu için
* **Kullanım vm app01 azos** için ilk uygulama sunucusu
* **Kullanım vm app02 azos** ikinci uygulama sunucusu için
* **Kullanım vm db01 azos** kümedeki ilk MongoDB sunucu için
* **Kullanım vm db02 azos** kümedeki ikinci MongoDB sunucu için
* **Kullanım vm dc01 azos** ilk etki alanı denetleyicisi
* **Kullanım vm dc02 azos** ikinci etki alanı denetleyicisi

Sonuçta elde edilen yapılandırma aşağıda verilmiştir.

![Azure üzerinde dağıtılan son uygulama altyapısı](./media/infrastructure-example/example-config.png)

Bu yapılandırma bir araya getirir:

* Bir yalnızca bulut sanal ağla iki alt ağ (ön uç ve arka uç)
* Azure yönetilen standart ve Premium diskleri kullanarak diskleri
* Bir çevrimiçi mağaza, her katman için dört kullanılabilirlik kümeleri
* Sanal makineler için dört katman
* Internet'ten HTTPS tabanlı web trafiği web sunucuları için bir dış yük dengeli küme
* Bir iç yük dengeli küme web sunucularından şifrelenmemiş web trafiği uygulama sunucuları için
* Tek bir kaynak grubu
