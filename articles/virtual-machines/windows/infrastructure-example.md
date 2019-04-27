---
title: Örnek Azure altyapısı gözden geçirme | Microsoft Docs
description: Bir örnek altyapısını Azure'a dağıtmak için önemli tasarım ve uygulama yönergeleri hakkında bilgi edinin.
documentationcenter: ''
services: virtual-machines-windows
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 7032b586-e4e5-4954-952f-fdfc03fc1980
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ddbaed6704fd32f7fd4fe5a790424cbf829d2f1c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60540411"
---
# <a name="example-azure-infrastructure-walkthrough-for-windows-vms"></a>Windows Vm'leri için örnek Azure altyapı Kılavuzu
Bu makalede örnek uygulama altyapısı oluşturmaya gösterilmektedir. Biz, yönergeleri ve adlandırma kuralları, kullanılabilirlik kümeleri, sanal ağlar ve yük Dengeleyiciler kararları bir araya basit bir çevrimiçi mağaza için bir altyapıya tasarlama ve gerçekte sanal makinelerinizi (VM) dağıtma açıklamaktadır.

## <a name="example-workload"></a>Örnek iş yükü
Adventure Works Cycles, azure'da oluşan bir çevrimiçi mağaza uygulaması derleme ister:

* Bir web katmanı ön uç istemcisi çalıştıran iki IIS sunucuları
* Veri ve uygulama katmanında orders işleme, iki IIS sunucuları
* Ürün verileri ve siparişler bir veritabanı katmanında depolamak için AlwaysOn Kullanılabilirlik grupları (iki SQL sunucuları ve çoğunluk düğümü tanığı) ile iki Microsoft SQL Server örneği
* Müşteri hesaplar ve bir kimlik doğrulama katmanı sağlayıcıları için iki Active Directory etki alanı denetleyicisi
* Tüm sunucular iki alt ağlarında bulunur:
  * web sunucuları için ön uç bir alt ağ 
  * uygulama sunucuları, SQL kümesi ve etki alanı denetleyicileri için bir arka uç alt ağı

![Uygulama altyapısı için farklı bir katman diyagramı](./media/infrastructure-example/example-tiers.png)

Güvenli gelen web trafiğini gerekir yük dengeli web sunucular arasında müşteriler çevrimiçi mağaza gezindikçe. Web sunucuları, uygulama sunucuları arasında dengelenmelidir sipariş işleme trafik HTTP biçiminde ister. Ayrıca, altyapı yüksek kullanılabilirlik için tasarlanması gerekir.

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
* Sanal ağları kullanın AZOS kullanım VN **[sayı]**
* Kullanılabilirlik kümelerini kullanın azos-kullanın-olarak-**[rol]**
* Sanal makine adları azos kullanın-kullanın-vm -**[vmname]**

## <a name="azure-subscriptions-and-accounts"></a>Azure aboneliklerini ve hesaplarını
Adventure Works Cycles, bu BT iş yükü için fatura bilgilerini sağlamak için Adventure Works Enterprise aboneliğinizin adlı Enterprise aboneliğini kullanıyor.

## <a name="storage"></a>Depolama
Adventure Works Cycles, Azure yönetilen diskler kullanması gerektiğini belirledi. Vm'leri oluştururken, iki depolama katmanında kullanılır:

* **Standart depolama** web sunucuları, uygulama sunucuları ve etki alanı denetleyicileri ve veri diskleriyle.
* **Premium depolama** SQL Server Vm'leri ve veri diskleriyle.

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
Tüm dört katmanda, bir çevrimiçi mağaza, yüksek kullanılabilirliğini sürdürmek için Adventure Works Cycles dört kullanılabilirlik kümeleri verdi:

* **web olarak azos kullanım** web sunucuları için
* **uygulama olarak azos kullanım** uygulama sunucuları için
* **sql olarak azos kullanım** SQL Server'lar için
* **dc olarak azos kullanım** etki alanı denetleyicileri

## <a name="virtual-machines"></a>Sanal makineler
Şu adları kendi Azure Vm'leri için Adventure Works Cycles verdi:

* **Kullanım vm web01 azos** ilk web sunucusu
* **Kullanım vm web02 azos** ikinci web sunucusunun
* **Kullanım vm app01 azos** ilk uygulama sunucusu
* **Kullanım vm app02 azos** ikinci uygulama sunucusu
* **Kullanım vm sql01 azos** kümedeki ilk SQL Server sunucusu için
* **Kullanım vm sql02 azos** kümedeki ikinci SQL Server sunucusu için
* **Kullanım vm dc01 azos** ilk etki alanı denetleyicisi
* **Kullanım vm dc02 azos** ikinci etki alanı denetleyicisi

Sonuçta elde edilen yapılandırması aşağıda verilmiştir.

![Azure'da dağıtılan son uygulama altyapısı](./media/infrastructure-example/example-config.png)

Bu yapılandırmayı içerir:

* Yalnızca bulutta yer alan bir sanal ağ ile iki alt ağa (ön uç ve arka uç)
* Azure yönetilen diskler ile standart ve Premium diskler
* Bir çevrimiçi mağaza, her bir katman için dört kullanılabilirlik kümeleri
* Sanal makineler için dört katmanı
* Web sunucularına HTTPS tabanlı web trafiği Internet'ten bir dış yük dengeli küme
* Uygulama sunucuları için web sunucularından şifrelenmemiş web trafiği için bir iç yük dengeli
* Tek bir kaynak grubu
