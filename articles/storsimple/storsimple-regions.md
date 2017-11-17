---
title: "StorSimple bölge kullanılabilirliği | Microsoft Docs"
description: "Çeşitli StorSimple cihaz modelleri kullanılabilir olduğu Azure bölgelerini açıklar."
services: storsimple
documentationcenter: 
author: alkohli
manager: jeconnoc
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/16/2017
ms.author: alkohli
ms.openlocfilehash: d47109d541a3df93d9234e27e53d1538f6bc4c6e
ms.sourcegitcommit: c7215d71e1cdeab731dd923a9b6b6643cee6eb04
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/17/2017
---
# <a name="available-regions-for-your-storsimple"></a>StorSimple için kullanılabilir bölgeleri

## <a name="overview"></a>Genel Bakış

Azure veri merkezleri performans, gereksinimleri ve veri konumu ilgili tercihlerini müşteri taleplerini karşılamak üzere dünyanın birden çok coğrafi çalışır. Bir Azure Coğrafya, en az bir Azure bölgesi içeren dünya tanımlı bir alandır. Bir Azure bölgesine bir veya daha fazla veri merkezleri içeren bir coğrafi konum içinde bir alandır.

Bir Azure bölgesi seçme çok önemlidir ve bölge seçimi veri residency ve Egemenlik, hizmet kullanılabilirliği, performans, maliyet ve artıklık gibi etkenlere göre etkilenir. Bir bölge seçmek nasıl daha fazla bilgi için Git [hangi Azure bölgesi benim için en uygun olan?](https://azure.microsoft.com/overview/datacenters/how-to-choose/)

StorSimple çözümü için bölge seçimi özellikle aşağıdaki etkenlere göre belirlenir:

- StorSimple cihaz Yöneticisi hizmeti kullanılabilir olduğu bölgeleri.
- StorSimple fiziksel, Bulut veya sanal aygıt kullanılabilir olduğu ülkelerin.
- Bölgeleri nerede StorSimple verileri depolamak depolama hesapları için en iyi performansı bulunması gerekir.

Bu öğretici StorSimple cihaz Yöneticisi hizmeti, şirket içi fiziksel ve bulut aygıtlar için bölge kullanılabilirliği açıklar. Bu makalede yer alan bilgileri, StorSimple 8000 ve 1200 Serisi cihazlar için geçerlidir.

## <a name="region-availability-for-storsimple-device-manager-service"></a>StorSimple cihaz Yöneticisi hizmeti için bölge kullanılabilirliği

StorSimple cihaz Yöneticisi hizmeti şu anda 12 genel bölgeler ve 2 Azure kamu bölgeleri desteklenmez.

StorSimple cihaz Yöneticisi hizmeti ilk oluşturduğunuzda bir bölge veya konum tanımlayın. Genel olarak, cihazın dağıtıldığı coğrafi bölgeye yakın bir konum seçilir. Ancak cihazını ve hizmetini de farklı konumlarda dağıtılabilir.

Burada StorSimple cihaz Yöneticisi hizmeti Azure genel bulut için kullanılabilir ve dağıtılabilir bölgelerin bir listesi aşağıda verilmiştir.

![storsimple-aygıt-manager-service-bölgeleri](./media/storsimple-region/storsimple-device-manager-service-regions.png)

Azure kamu bulut için StorSimple cihaz Yöneticisi hizmeti BİZE kamu Iowa ve BİZE kamu Virginia veri merkezlerinde kullanılabilir.

## <a name="region-availability-for-data-stored-in-storsimple"></a>StorSimple içinde depolanan veriler için bölge kullanılabilirliği

StorSimple veriler Azure depolama hesaplarında fiziksel olarak depolanır ve bu hesapları Azure tüm bölgelerde kullanılabilir. Bir Azure depolama hesabı oluşturduğunuzda, depolama hesabının birincil konumda seçilir ve verilerin bulunduğu bölgeyi belirler.

İlk StorSimple Aygıt Yöneticisi'ni hizmet oluşturduğunuzda ve bir depolama hesabı ile ilişkilendirin StorSimple cihaz Yöneticisi hizmeti ve Azure depolama iki ayrı konumda olabilir. Böyle bir durumda, StorSimple Cihaz Yöneticisi ve Azure Storage hesabını ayrı ayrı oluşturmanız gerekir.

Genel olarak, depolama hesabınız için hizmetinize en yakın bölgeyi seçin. Ancak, en yakın Microsoft Azure bölgesindeki en düşük gecikme süresine sahip bölge gerçekte olmayabilir. Ağ Hizmeti performansını belirleyen gecikme olduğunu ve bu nedenle çözümün performans. Bu nedenle farklı bir bölgede bir depolama hesabı seçerek, hizmetiniz ile depolama hesabınızla ilişkili bölgesi arasındaki gecikme nelerdir bilmeniz önemlidir.

Ardından bir StorSimple bulut uygulaması kullanıyorsanız, hizmet ve ilişkili depolama hesabının aynı bölgede olduğunu öneririz. Farklı bir bölgede depolama hesapları performansın düşmesine neden olabilir.

## <a name="availability-of-storsimple-device"></a>StorSimple cihazı kullanılabilirliği

Modeline bağlı olarak, StorSimple cihazlar farklı coğrafi veya ülkelerde kullanılabilir olabilir.

### <a name="storsimple-physical-device-models-81008600"></a>StorSimple fiziksel cihazı (modelleri 8100/8600)

Cihaz StorSimple 8100 veya 8600 fiziksel cihaz kullanıyorsanız, aşağıdaki ülkede kullanılamıyor.

| #  | Ülke        | #  | Ülke     | #  | Ülke      | #  | Ülke              |
|----|----------------|----|-------------|----|--------------|----|----------------------|
| 1  | Avustralya      | 16 | Hong Kong   | 31 | Yeni Zelanda  | 46 | Güney Afrika         |
| 2  | Avusturya        | 17 | Macaristan     | 32 | Nijerya      | 47 | Güney Kore          |
| 3  | Bahreyn        | 18 | İzlanda     | 33 | Norveç       | 48 | İspanya                |
| 4  | Belçika        | 19 | Hindistan       | 34 | Peru         | 49 | Sri Lanka            |
| 5  | Brezilya         | 20 | Endonezya   | 35 | Filipinler  | 50 | İsveç               |
| 6  | Kanada         | 21 | İrlanda     | 36 | Polonya       | 51 | İsviçre          |
| 7  | Şili          | 22 | İsrail      | 37 | Portekiz     | 52 | Tayvan               |
| 8  | Kolombiya       | 23 | İtalya       | 38 | Porto Riko  | 53 | Tayland             |
| 9  | Çek Cumhuriyeti | 24 | Japonya       | 39 | Katar        | 54 | Türkiye               |
| 10 | Danimarka        | 25 | Kenya       | 40 | Romanya      | 55 | Ukrayna              |
| 11 | Mısır          | 26 | Kuveyt      | 41 | Rusya       | 56 | Birleşik Arap Emirlikleri |
| 12 | Finlandiya        | 27 | Makau       | 42 | Suudi Arabistan | 57 | Birleşik Krallık       |
| 13 | Fransa         | 28 | Malezya    | 43 | Singapur    | 58 | Amerika Birleşik Devletleri        |
| 14 | Almanya        | 29 | Meksika      | 44 | Slovakya     | 59 | Vietnam              |
| 15 | Yunanistan         | 30 | Hollanda | 45 | Slovenya     | 60 | Hırvatistan              |

Daha fazla ülke eklendikçe bu listeyi değiştirir. Depolama dizisi koşulları ekte coğrafyalara en güncel bir listesi için Git [ürün koşulları](https://www.microsoft.com/en-us/Licensing/product-licensing).

Microsoft, fiziksel donanım sevk ve StorSimple için yukarıdaki listede coğrafyalardaki, donanım yedek parçaların değiştirme sağlayın.

> [!IMPORTANT]
> StorSimple fiziksel cihazı nerede StorSimple desteklenmeyen bir bölgede yerleştirmeyin. Microsoft, tüm yedek parça nerede StorSimple desteklenmiyor ülkelere sevk mümkün olmaz.

### <a name="storsimple-cloud-appliance-models-80108020"></a>StorSimple bulut uygulaması (modeller 8010/8020)

Bir StorSimple bulut Gereci 8010 veya 8020 kullanıyorsanız, ardından cihaz temel VM burada desteklenen tüm bölgelerde desteklenen kullanılabilir. 8010 kullandığı bir _Standard_A3_ tüm Azure bölgelerinde desteklenmeyen VM.

Premium depolama 8020 kullanır ve _Standard_DS3_ bulut uygulaması oluşturmak için VM. 8020 Premium Storage destekleyen Azure bölgelerinde desteklenir ve _Standard_DS3_ Azure VM'ler. Bölgenizde hem **Sanal Makineler > DS serisi** hem de **Depolama > Disk depolamanın** mevcut olup olmadığını görmek için [bu listeyi](https://azure.microsoft.com/regions/services/) kullanın.

### <a name="storsimple-virtual-array-model-1200"></a>StorSimple sanal dizinin (modeli 1200)

1200 Serisi StorSimple sanal dizinin kullanıyorsanız, sanal disk görüntüsü tüm Azure bölgelerinde desteklenir.

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinmek [çeşitli StorSimple modelleri için fiyatlandırma](https://azure.microsoft.com/pricing/calculator/#storsimple2).
* Daha fazla bilgi edinmek [StorSimple depolama hesabınızı yönetme](storsimple-8000-manage-storage-accounts.md).
* Nasıl yapılır hakkında daha fazla bilgi [StorSimple Cihazınızı yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanma](storsimple-8000-manager-service-administration.md).
