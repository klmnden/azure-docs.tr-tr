---
title: StorSimple bölge kullanılabilirliği | Microsoft Docs
description: Çeşitli StorSimple cihaz modelleri kullanılabilir olduğu Azure bölgelerini açıklar.
services: storsimple
documentationcenter: ''
author: alkohli
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/16/2017
ms.author: alkohli
ms.openlocfilehash: e290feb278a1cddf1cfecfcb66458d8290ec122a
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64943605"
---
# <a name="available-regions-for-your-storsimple"></a>StorSimple'ınızı için kullanılabildiği bölgeler

## <a name="overview"></a>Genel Bakış

Azure veri merkezleri, performans, gereksinimleri ve veri konumuna ilişkin tercihlerini müşteri taleplerini karşılamak üzere dünyanın dört bir yanındaki birden çok coğrafi olarak çalışır. Her Azure coğrafyası dünyanın en az bir Azure bölgesi içeren tanımlanmış bir alandır. Bir Azure bölgesine bir veya daha fazla veri içeren bir coğrafyadaki alanıdır.

Bir Azure bölgesini seçmek çok önemlidir ve bölge seçimi veri yerleşikliği ve özerkliği, hizmet kullanılabilirliği, performans, maliyet ve yedeklilik gibi faktörlere tarafından etkilenir. Bir bölge seçin hakkında daha fazla bilgi için Git [Azure bölgesidir bana uygun?](https://azure.microsoft.com/overview/datacenters/how-to-choose/)

StorSimple çözümü için, tercih ettiğiniz bölgede özellikle aşağıdaki etkenlere göre belirlenir:

- StorSimple cihaz Yöneticisi hizmeti kullanılabildiği bölgeler.
- StorSimple fiziksel, Bulut veya sanal aygıt kullanılabilir olduğu ülkeleri.
- Burada, StorSimple verileri depolayan depolama hesapları en iyi performans için klasöründe bulunmalıdır bölgeleri.

Bu öğreticide StorSimple cihaz Yöneticisi hizmeti, şirket içi fiziksel ve bulut cihazlar için bölge kullanılabilirliği açıklanmaktadır. Bu makalede yer alan bilgileri, StorSimple 8000 ve 1200 Serisi cihazlar için geçerlidir.

## <a name="region-availability-for-storsimple-device-manager-service"></a>StorSimple cihaz Yöneticisi hizmeti için bölge kullanılabilirliği

StorSimple cihaz Yöneticisi hizmeti, şu anda 12 genel bölgeler ve 2 Azure kamu bölgelerinde desteklenmez.

StorSimple cihaz Yöneticisi hizmetine ilk oluşturduğunuzda bir bölge veya konumunu tanımlayın. Genel olarak, cihazın dağıtıldığı coğrafi bölgeye yakın bir konum seçilir. Ancak cihazını ve hizmetini de farklı konumlarda dağıtılabilir.

Burada, StorSimple cihaz Yöneticisi hizmeti Azure genel bulut için kullanılabilir ve dağıtılabilir bölgelerin bir listesi aşağıda verilmiştir.

![storsimple-cihaz-manager-hizmet-bölgeleri](./media/storsimple-region/storsimple-device-manager-service-regions.png)

Azure kamu bulutu için StorSimple cihaz Yöneticisi hizmeti, ABD Devleti Iowa ve ABD Devleti Virginia veri merkezlerinde kullanılabilir.

## <a name="region-availability-for-data-stored-in-storsimple"></a>Storsimple'da depolanan veriler için bölge kullanılabilirliği

StorSimple veri fiziksel olarak Azure depolama hesaplarında depolanır ve bu hesapları tüm Azure bölgelerinde kullanılabilir. Bir Azure depolama hesabı oluşturduğunuzda, depolama hesabının birincil konum seçilir ve, verilerin bulunduğu bölgeyi belirler.

İlk StorSimple cihaz Yöneticisi hizmeti oluşturun ve bir depolama hesabı ile ilişkilendirmek, StorSimple cihaz Yöneticisi hizmeti ve Azure storage iki ayrı konumda olabilir. Böyle bir durumda, StorSimple Cihaz Yöneticisi ve Azure Storage hesabını ayrı ayrı oluşturmanız gerekir.

Genel olarak, depolama hesabınız için hizmetinize en yakın bölgeyi seçin. Ancak, Microsoft Azure bölgesine en yakın bölgeyi düşük gecikme ile gerçekten olmayabilir. Ağ Hizmeti performansını belirleyen gecikme olduğunu ve bu nedenle çözümün performans. Bu nedenle farklı bir bölgede bir depolama hesabı seçme, hizmetiniz, depolama hesabıyla ilişkili bölge arasındaki gecikme süreleri nelerdir bilmek önemlidir.

Ardından bir StorSimple Cloud Appliance'ı kullanıyorsanız, hizmeti ve ilişkili depolama hesabı aynı bölgede olmasını öneririz. Farklı bir bölgede depolama hesapları performansın düşmesine neden.

## <a name="availability-of-storsimple-device"></a>StorSimple cihaz kullanılabilirliği

Modeline bağlı olarak, StorSimple cihazları farklı coğrafyalara veya ülkeler/bölgeler kullanılabilir olabilir.

### <a name="storsimple-physical-device-models-81008600"></a>StorSimple fiziksel cihazının (model 8100/8600)

Cihaz, StorSimple 8100 veya 8600 fiziksel cihaz kullanıyorsanız, aşağıdaki ülkelerde/bölgelerde kullanılabilir.

| #  | Ülke/Bölge        | #  | Ülke/Bölge     | #  | Ülke/Bölge      | #  | Ülke/Bölge             |
|----|-----------------------|----|--------------------|----|---------------------|----|----------------------------|
| 1  | Avustralya             | 16 | Hong Kong Çin ÖİB      | 31 | Yeni Zelanda         | 46 | Güney Afrika               |
| 2  | Avusturya               | 17 | Macaristan            | 32 | Nijerya             | 47 | Güney Kore                |
| 3  | Bahreyn               | 18 | İzlanda            | 33 | Norveç              | 48 | İspanya                      |
| 4  | Belçika               | 19 | Hindistan              | 34 | Peru                | 49 | Sri Lanka                  |
| 5  | Brezilya                | 20 | Endonezya          | 35 | Filipinler         | 50 | İsveç                     |
| 6  | Kanada                | 21 | İrlanda            | 36 | Polonya              | 51 | İsviçre                |
| 7  | Şili                 | 22 | İsrail             | 37 | Portekiz            | 52 | Tayvan                     |
| 8  | Kolombiya              | 23 | İtalya              | 38 | Porto Riko         | 53 | Tayland                   |
| 9  | Çek Cumhuriyeti        | 24 | Japonya              | 39 | Katar               | 54 | Türkiye                     |
| 10 | Danimarka               | 25 | Kenya              | 40 | Romanya             | 55 | Ukrayna                    |
| 11 | Mısır                 | 26 | Kuveyt             | 41 | Rusya              | 56 | Birleşik Arap Emirlikleri       |
| 12 | Finlandiya               | 27 | Makao ÖİB          | 42 | Suudi Arabistan        | 57 | Birleşik Krallık             |
| 13 | Fransa                | 28 | Malezya           | 43 | Singapur           | 58 | Amerika Birleşik Devletleri              |
| 14 | Almanya               | 29 | Meksika             | 44 | Slovakya            | 59 | Vietnam                    |
| 15 | Yunanistan                | 30 | Hollanda        | 45 | Slovenya            | 60 | Hırvatistan                    |

Daha fazla ülkeler/bölgeler eklendikçe bu listede değişiklik. Depolama dizisi koşulları ekte coğrafyaları en güncel listesi için Git [ürün koşulları](https://www.microsoft.com/en-us/licensing/product-licensing/products).

Microsoft, fiziksel donanım gönderin ve yukarıdaki listede coğrafyalar için StorSimple için donanım yedek parça değişimi sağlayın.

> [!IMPORTANT]
> StorSimple fiziksel cihazı StorSimple desteklenmediği bir bölgeye yerleştirmeyin. Microsoft StorSimple desteklenmediği ülkeler/bölgeler için bir yedek parçaları sevk etmek mümkün olmayacaktır.

### <a name="storsimple-cloud-appliance-models-80108020"></a>StorSimple bulut Gereci (8010/8020 modeller)

Ardından bir StorSimple Cloud Appliance 8010 veya 8020 kullanıyorsanız ve desteklenen cihaz ve temel alınan VM'ın desteklendiği tüm bölgelerde kullanılabilir. 8010 kullandığı bir _Standard_A3_ VM'nin tüm Azure bölgelerinde desteklenir.

8020 premium depolama kullanır ve _Standard_DS3_ bulut Gereci oluşturmak için VM. 8020 Premium Storage destekleyen Azure bölgelerinde desteklenir ve _Standard_DS3_ Azure Vm'leri. Bölgenizde hem **Sanal Makineler > DS serisi** hem de **Depolama > Disk depolamanın** mevcut olup olmadığını görmek için [bu listeyi](https://azure.microsoft.com/regions/services/) kullanın.

### <a name="storsimple-virtual-array-model-1200"></a>StorSimple sanal dizisi (Model 1200)

Ardından sanal disk görüntüsünün 1200 Serisi StorSimple sanal dizisi kullanıyorsanız, tüm Azure bölgelerinde desteklenir.

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinin [çeşitli StorSimple modelleri için fiyatlandırma](https://azure.microsoft.com/pricing/calculator/#storsimple2).
* Daha fazla bilgi edinin [StorSimple depolama hesabınızı yönetme](storsimple-8000-manage-storage-accounts.md).
* Kullanma hakkında daha fazla bilgi edinin [StorSimple Cihazınızı yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanma](storsimple-8000-manager-service-administration.md).
