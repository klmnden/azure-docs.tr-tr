---
title: Microsoft Azure FXT Edge dosyalayıcı belirtimleri | Microsoft Docs
description: Fiziksel ve ortam Azure FXT Edge dosyalayıcı donanım belirtimleri
author: ekpgh
ms.service: fxt-edge-filer
ms.topic: overview
ms.date: 06/20/2019
ms.author: v-erkell
ms.openlocfilehash: 0679bce8eae515aa6b90e34fcfd15ee9b4e56b31
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67542882"
---
# <a name="azure-fxt-edge-filer-specifications"></a>Azure FXT Edge dosyalayıcı belirtimleri

Bu makalede, Azure FXT Edge dosyalayıcı donanım düğümleri için donanım belirtimleri açıklanmaktadır. Uygulamada, üç veya daha fazla düğüm kümelenmiş önbellek sistem sağlamak için birlikte yapılandırılır.

## <a name="hardware-specifications"></a>Donanım belirtimleri

| Bileşen | FXT 6600 | FXT 6400 |
|----------|-----------|-----------|
| CPU çekirdekleri |  16 | 16 |
| DİRHEMİ  | 1536 GB | 768 GB |
| Ağ bağlantı noktaları | 6 x 25/10 Gb + 2 x 1 Gb | 6 x 25/10 Gb + 2 x 1 Gb |
| NVMe SSD kapasitesi | 25.6 TB | 12,8 TB |

## <a name="drive-specifications"></a>Sürücü özellikleri

Sistem on sürücü yuvaları, Önden erişilebilir sahiptir. Her doldurulmuş bir sürücü kapasite bilgileri ile sağdaki etiketlenir. 

Sürücü numaraları sürücüleri arasındaki boşluk yazdırılır. Azure FXT Edge dosyalayıcı 0 sürücü sol üst sürücüdür ve 1 doğrudan o altında sürücüdür.

![Fotoğraf bir sabit sürücünün sürücü numaraları ve kapasite etiketleri gösteren FXT kasa içinde bölmesi](media/fxt-drives-photo.png)

| Sürücü numaraları    |  Kullanım   |  Belirtimler |
|------------------|--------|-----------------|
| 0, 1             | OS     | 480 GB SATA SSD |
| 2, 3, 4, 5, 6, 7, 8, 9 | Data   | FXT 6600: 3.2 TB NVMe SSD <br> FXT 6400: 1.6 TB NVMe SSD |


## <a name="dimensions-and-weight"></a>Boyut ve Ağırlık

Azure FXT Edge dosyalayıcı standart 19" ekipman rafa uyacak şekilde tasarlanmıştır ve bir raf üstü yüksek (1U) birimidir. 

<!-- 10x2.5 inches version -->

| Dosyalayıcı boyutları           |                          |
|-----------------------------|--------------------------|
| Yükseklik                      | 42.8 mm (1,68 inç)    |
| Genişlik (raf kulağına dahil) | 482.0 mm (18.97 inç)  |
| Genişlik - ana kutu      | 434.0 mm (17.08 inç) |
| Derinlik - için raf kulağına arkasına ana kutu                   | 733.82 mm (29.61 inç) |
| Derinlik - en arka protrusion için raf kulağına                 | 772.67 mm (30.42 inç) |
| Derinlik - çerçeve olmadan en ön protrusion için raf kulağına | 22.0 mm (0.87 inç)  |
| Derinlik - kasa ile en ön protrusion için raf kulağına    | 35.84 mm (1.41 inç) |

| Ağırlık | |
|-----------------|----------------------|
| Düğüm ağırlık (olmadan, paketleme, Donatılar'olmadan) | 40 lbs (18.1 kg) |
| Net Ağırlık (olmadan, paketleme, Donatılar'dahil olmak üzere) | 51 lbs (23.1 kg)|
| (Gibi sevk, tüm paket dahil) brüt ağırlığı |  64 lbs (29.0 kg) |

### <a name="shipping-dimensions"></a>Sevkiyat boyutları

| Paket boyutu | Milimetre | İnç yükseklik |
|-------------------|-------------|--------|
| Yükseklik            | 311.2       | 12.25" |
| Genişlik             | 642.8       | 25.31" |
| Uzunluk            | 1,051.1     | 41.38" |

## <a name="power-and-thermal-specifications"></a>Güç ve sıcaklık belirtimleri

Bu bölüm, güç derecelendirmelerini ve Azure FXT Edge dosyalayıcı ölçüleri sağlar.

### <a name="nameplate-ratings"></a>Ad plakası derecelendirmeleri

| FXT 6000 serisi modellerine yönelik ad plakası derecelendirmeleri |
|----------------|
| 100 - 240V~    |
| 10A - 5A (X2)  |
| 50/60Hz         |

<!-- matches the Dell regulatory label exactly -->

### <a name="power-and-thermal-measurements"></a>Güç ve sıcaklık ölçümleri 

Azure FXT Edge dosyalayıcı düğümleri değişken hız fanlar, kullandığından, güç sıcaklığını ve yük bağlıdır. En fazla fanı hızı yüksek yük ve yükseltilmiş ortam sıcaklığı belirli birleşimlerini erişilebilir. 

Bu grafikler güç tüketimini ve yaygın olarak kullanılan voltaj sıklığı birleşimleri için çıkış sıcaklık ölçümleri sağlar. 

| Oda FXT 6600 güç <br />(22° C, 71.6° F) | 100 V, 60 Hz | 120 V, 60 Hz | 208 V, 60 Hz | 230 V, 50 Hz | 240 V, 50 Hz | 
|---------|---|---|---|---|---|
| Voltaj (V) | 100 | 120 | 208 | 230 | 240 | 
| Sıklık (Hz) | 60 | 60 | 60 | 50 | 50 |
| Geçerli (A) | 5.02 | 4.16 |2.40 | 2.20 | 2.16 |
| Görünen Power (VA) | 502 | 499 | 499 | 506 | 518|
| Güç faktörü | 0.99 | 0.99 |0.98 | 0.98 | 0.98 |
| Gerçek gücü (W) | 497 |494 | 489 | 496 | 508 |
| Isı kaybı (BTU/saat) |1696 | 1686 | 1669 | 1692 | 1733 |

| En fazla fanı hızlarında FXT 6600 güç | 100 V, 60 Hz | 120 V, 60 Hz | 208 V, 60 Hz | 230 V, 50 Hz | 240 V, 50 Hz | 
|---------|---|---|---|---|---|
| Voltaj (V) | 100 |120 | 208 | 230 | 240| 
| Sıklık (Hz) | 60 | 60 | 60 | 50 | 50 |
| Geçerli (A) | 5.98 | 5.01 | 2.81 | 2.55 | 2.48 |
| Görünen Power (VA) | 598 | 601 | 584 | 587 | 595 |
| Güç faktörü | 0.99 | 0.99 | 0.98 | 0.98 | 0.98 |
| Gerçek gücü (W) | 592 | 595 | 573 | 575 | 583 |
| Isı kaybı (BTU/saat) | 2020 |2031 | 1954 | 1961 | 1990 |

| Oda FXT 6400 güç <br />(22° C, 71.6° F) | 100 V, 60 Hz | 120 V, 60 Hz | 208 V, 60 Hz | 230 V, 50 Hz | 240 V, 50 Hz | 
|---------|---|---|---|---|---|
| Voltaj (V) | 100 | 120 | 208 | 230 | 240 |
| Sıklık (Hz) |60 | 60 | 60 | 50 | 50 |
| Geçerli (A) | 4.63 | 3.86 | 2.24 | 2.04 | 1.94 |
| Görünen Power (VA) | 463 | 463 | 466 | 469 | 466 |
| Güç faktörü | 0.99 | 0.99 | 0.98 | 0.98 | 0.98 | 
| Gerçek gücü (W) | 458 | 459 | 457 | 460 | 456 |
| Isı kaybı (BTU/saat) | 1564 | 1565 | 1558 | 1569 | 1557 |

| En fazla fanı hızlarında FXT 6400 güç | 100 V, 60 Hz | 120 V, 60 Hz | 208 V, 60 Hz | 230 V, 50 Hz | 240 V, 50 Hz |
|---------|---|---|---|---|---|
| Voltaj (V) | 100 | 120 | 208 | 230 | 240 |
| Sıklık (Hz) | 60 | 60 | 60 | 50 | 50 |
| Geçerli (A) | 5.15 | 4.28 | 2.48 | 2.28 | 2.13 |
| Görünen Power (VA) | 515 | 514 | 516 | 524 | 511 |
| Güç faktörü | 0.99 | 0.99 | 0.98 | 0.98 | 0.98 |
| Gerçek gücü (W) | 510 | 508 | 506 | 514 | 501 |
| Isı kaybı (BTU/saat) | 1740 | 1735 | 1725 | 1753 | 1709 |

## <a name="environmental-requirements"></a>Ortam gereksinimleri

Bu bölüm, donanım ortam ortam özellikleri sağlar.

### <a name="temperature-and-humidity"></a>Sıcaklık ve nem oranı

| Ortam özniteliği   | Aralığı                   | Olmayan işletim aralığı         |
|---------------------------|-----------------------------------|-----------------------------|
| Ortam sıcaklığı aralığı | 10° C-35° C (50-86° F)          | -40 ° C 65 ° C (-40-149 ° F) |
| Ortam Bağıl nem oranı | %10 - % 80 yoğunlaşmayan          | %95 5 - % yoğunlaşmayan     |
| En fazla dew noktası         | 29° C (84° F)                       | 33° C (91° F)                 |
| Yükseklik                  | 3048 sıcaklık XML'deki derecelendirme tabi ölçümleri (10.000 feet) varan aşağıda belirtildiği | en fazla 12.000 ölçümleri (39,370 fit) |

> [!NOTE] 
> **Yükseklik sıcaklık XML'deki derecelendirmesi:** En yüksek sıcaklık tarafından ° C 1/300 m (1 ° F/547 ft) 950 milyon (3,117 ft) azaltılır.

### <a name="airflow-shock-and-vibration"></a>Hava akışı ve darbe Titreşim 

| Öznitelik         | Belirtimi |
|-------------------|---------------|
| Hava akışı                    | Sistem hava akışı arkadan öne ' dir. Sistem low-pressure arka Egzozu yüklemesiyle çalıştırılması gerekir. |
| Darbe işletimsel         | 6 G 11 milisaniye (6 yönleri içinde test) |
| Darbe çalışmaz     | 71 G 2 milisaniye (6 yönleri içinde test) |
| Titreşim işletimsel     | 0, 26 G<sub>RMS</sub> 350 Hz rastgele için 5 Hz         |
| Titreşim çalışmaz | 1.88 G<sub>RMS</sub> 10 Hz ila 15 dakika (test altı dışlamada) 500 Hz  |

## <a name="safety-regulation-compliance"></a>Güvenlik Mevzuata uyumluluk 

Azure FXT Edge dosyalayıcı listelenen yönetmelikleri ile uyumludur. 

| Category       | Yasal belirtimi | 
|----------------|--------------------------|
| Genel güvenliği | TR 60950-1:2006 A1:2010 + A2:2013 + A11:2009 + A12:2011 / IEC 60950-1:2005 ed2 + A1:2009 A2:2013 <br>TR 62311:2008 | 
| EMC            | FCC A, ICES-003  <br>EN 55032:2012/CISPR 32:2012  <br>EN 55032:2015/CISPR 32:2015  <br>EN 55024:2010 +A1:2015/CISPR 24:2010 +A1:2015  <br>TR 61000-3-2:2014 / IEC 61000-3-2:2014 (sınıfı D)   <br>EN 61000-3-3:2013/IEC 61000-3-3:2013 |
| Enerji         | Commission Regulation (EU) No. 617/2013  |
| RoHS           |    TR 50581:2012   |