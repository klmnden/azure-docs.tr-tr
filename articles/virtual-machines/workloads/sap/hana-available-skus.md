---
title: (Büyük örnekler) Azure üzerinde SAP HANA için SKU'ları | Microsoft Docs
description: (Büyük örnekler) Azure üzerinde SAP HANA için SKU'ları.
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
ms.openlocfilehash: b80f872c82061c0cb87f4f1e2714183e71cf02cd
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60794014"
---
# <a name="available-skus-for-hli"></a>HLI için kullanılabilir SKU'lar

SAP HANA (büyük örnekler) Azure hizmeti üzerinde çeşitli yapılandırmalarda ABD Batı ve ABD Doğu, Avustralya Doğu, Avustralya Güneydoğu, Batı Avrupa, Kuzey Avrupa, Japonya Doğu ve Japonya Batı, Azure bölgelerinde kullanılabilir.

[SAP HANA sertifikalı ve SKU'ları, HANA büyük örnekleri](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure) gibi listeleyin:

| SAP çözümü | CPU | Bellek | Depolama | Kullanılabilirlik |
| --- | --- | --- | --- | --- |
| OLAP için İyileştirildi: SAP BW, BW/4HANA<br /> veya genel OLAP iş yükü için SAP HANA | Azure S72 üzerinde SAP HANA<br /> – 2 x Intel® Xeon® İşlemci E7-8890 v3<br /> 36 CPU Çekirdeği ve 72 CPU iş parçacıkları |  768 GB |  3 TB | Artık önerilmez |
| --- | Azure S144 üzerinde SAP HANA<br /> -4 x Intel® Xeon® İşlemci E7-8890 v3<br /> 72 CPU Çekirdeği ve 144 CPU iş parçacıkları |  1,5 TB |  6 TB | Artık önerilmez |
| --- | Azure S192 üzerinde SAP HANA<br /> -4 x Intel® Xeon® İşlemci E7-8890 v4<br /> 96 CPU Çekirdeği ve 192 CPU iş parçacıkları |  2.0 TB |  8 TB | Kullanılabilir |
| --- | Azure S384 üzerinde SAP HANA<br /> -8 x Intel® Xeon® İşlemci E7-8890 v4<br /> 192 CPU Çekirdeği ve 384 CPU iş parçacıkları |  4.0 TB |  16 TB | Kullanılabilir |
| OLTP için iyileştirilmiştir: SAP Business Suite<br /> SAP HANA veya S/4HANA (OLTP)<br /> Genel OLTP | Azure S72m üzerinde SAP HANA<br /> – 2 x Intel® Xeon® İşlemci E7-8890 v3<br /> 36 CPU Çekirdeği ve 72 CPU iş parçacıkları |  1,5 TB |  6 TB | Artık önerilmez |
|---| Azure S144m üzerinde SAP HANA<br /> -4 x Intel® Xeon® İşlemci E7-8890 v3<br /> 72 CPU Çekirdeği ve 144 CPU iş parçacıkları |  3.0 TB |  12 TB | Artık önerilmez |
|---| Azure S192m üzerinde SAP HANA<br /> -4 x Intel® Xeon® İşlemci E7-8890 v4<br /> 96 CPU Çekirdeği ve 192 CPU iş parçacıkları  |  4.0 TB |  16 TB | Kullanılabilir |
|---| Azure S384m üzerinde SAP HANA<br /> -8 x Intel® Xeon® İşlemci E7-8890 v4<br /> 192 CPU Çekirdeği ve 384 CPU iş parçacıkları |  6.0 TB |  18 TB | Kullanılabilir |
|---| Azure S384xm üzerinde SAP HANA<br /> -8 x Intel® Xeon® İşlemci E7-8890 v4<br /> 192 CPU Çekirdeği ve 384 CPU iş parçacıkları |  8.0 TB |  22 TB |  Kullanılabilir |
|---| Azure S576m üzerinde SAP HANA<br /> – 12 x Intel® Xeon® İşlemci E7-8890 v4<br /> 288 CPU Çekirdeği ve 576 CPU iş parçacıkları |  12.0 TB |  28 TB | Kullanılabilir |
|---| Azure S768m üzerinde SAP HANA<br /> – 16 x Intel® Xeon® İşlemci E7-8890 v4<br /> 384 CPU Çekirdeği ve 768 CPU iş parçacıkları |  16,0 TB |  36 TB | Kullanılabilir |
|---| Azure S960m üzerinde SAP HANA<br /> -20 x Intel® Xeon® İşlemci E7-8890 v4<br /> 480 CPU Çekirdeği ve 960 CPU iş parçacıkları |  20.0 TB |  46 TB | Kullanılabilir |


SAP HANA TDIv5 altında SAP müşteriye özgü boyutlandırma ve içinde onaylı olarak listelenen değil sunucu yapılandırmaları için yol açabilecek müşteriye özgü projeleri sağlar:

- [SAP HANA sertifikalı cihazları](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/appliances.html)
- [SAP HANA sertifikalı ve Iaas platformları](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure)

İçinde çok sayıda durumlarda, bu müşteriye özgü sunucu yapılandırmaları ile SAP sertifikalı sunucu birimlerini daha fazla bellek taşır. SAP kullanmaya çalışırken, müşteriler SAP destek almak ve müşteriye özgü ölçekli sunucu yapılandırmalarını onaylamak için olanağına sahip olursunuz. Azure'da HANA büyük örneği aşağıdaki standart SKU'lar kullanılabilir ve Microsoft gibi TDIv5 müşteriye özgü boyutlandırma projeler için liste fiyatı.

| SKU|CPU | Bellek | Depolama | Kullanılabilirlik |
| ---| --- | --- | --- | --- |
| S96 | Azure S96 üzerinde SAP HANA<br /> – 2 x Intel® Xeon® İşlemci E7-8890 v4<br /> 48 CPU Çekirdeği ve 96 CPU iş parçacıkları |  768 GB |  3 TB | Kullanılabilir |


| Olabilir özgün SKU <br /> bellekte genişletilmiş | CPU | Bellek | Depolama | Kullanılabilirlik |
| --- | --- | --- | --- | --- |
| S192m Genişletilebilir | Azure S192xm üzerinde SAP HANA<br /> -4 x Intel® Xeon® İşlemci E7-8890 v4<br /> 96 CPU Çekirdeği ve 192 CPU iş parçacıkları |  6.0 TB |  16 TB | Kullanılabilir |
| S384xm Genişletilebilir | Azure S384xxm üzerinde SAP HANA<br /> -8 x Intel® Xeon® İşlemci E7-8890 v4<br /> 192 CPU Çekirdeği ve 384 CPU iş parçacıkları |  12.0 TB |  28 TB | Kullanılabilir |
| S576m Genişletilebilir | Azure S576xm üzerinde SAP HANA<br /> – 12 x Intel® Xeon® İşlemci E7-8890 v4<br /> 288 CPU Çekirdeği ve 576 CPU iş parçacıkları |  18.0 TB |  41 TB | Kullanılabilir |
| S768m Genişletilebilir | Azure S768xm üzerinde SAP HANA<br /> – 16 x Intel® Xeon® İşlemci E7-8890 v4<br /> 384 CPU Çekirdeği ve 768 CPU iş parçacıkları |  24.0 TB |  56 TB | Kullanılabilir |

- CPU çekirdekleri toplam olmayan-hyper-Threading Teknolojisi CPU çekirdek işlemcileri sunucusu birimi toplamı =.
- CPU iş parçacıklarını hiper iş parçacıklı CPU çekirdeği sunucusu birimi işlemcilerin toplamı tarafından sağlanan işlem iş parçacıklarının toplam =. Çoğu birimleri Hyper-Threading teknolojisini kullanmak için varsayılan olarak yapılandırılır.
- Tedarikçi önerilerine S768m göre S768xm ve S960m SAP HANA çalıştırmak için Hyper-Threading kullanmak için yapılandırılmamış.


Seçilen belirli yapılandırmalar, iş yükü, CPU kaynaklarını ve istenen bellek bağımlıdır. OLTP iş yükünün OLAP iş yükü için iyileştirilmiştir SKU'ları kullanmak mümkündür. 

Müşteriye özgü boyutlandırma projeleri için birim dışında teklifler için temel donanım SAP HANA TDI sertifikalı. Donanım iki farklı sınıflardaki SKU'ları halinde bölmek:

- S72 S72m, S96, S144, S144m, S192, S192m ve S192xm, "ı sınıf türü olarak" adlandırılan SKU.
- S384, S384m, S384xm, S384xxm, S576m, S576xm S768m, S768xm ve denir S960m, "Type II sınıfı" SKU.

Eksiksiz bir HANA büyük örnek damgası için tek bir müşteriye özel olarak ayrılmış değil&#39;s kullanın. Bu olgu bölmelerin de Azure'da dağıtılan bir ağ yapısı bağlı işlem ve depolama kaynakları için geçerlidir. Azure gibi HANA büyük örneği altyapı, farklı müşteri dağıtır &quot;kiracılar&quot; birbirlerinden aşağıdaki üç düzeyden birinde yalıtılmış olan:

- **Ağ**: HANA büyük örneği damgasında içindeki sanal ağlar arasında yalıtım sağlar.
- **Depolama**: Depolama birimi olan depolama sanal makineler için yalıtımı, atanan ve kiracılar arasında depolama birimleri yalıtır.
- **İşlem**: Sunucu birimleri için tek bir kiracının adanmış atama. Hayır sabit veya geçici sunucu ölçü bölümleme. Hiçbir tek bir sunucu veya konak birimi kiracılar arasında paylaştırma. 

HANA büyük örneği birimleri arasında farklı kiracıların dağıtımları birbirine görünmez. HANA büyük örneği birimleri farklı kiracıda dağıtılan diğer HANA büyük örnek damgası düzeyinde ile doğrudan iletişim kuramaz. Yalnızca tek bir kiracı içindeki HANA büyük örneği birimleri HANA büyük örnek damgası düzeyine birbiriyle iletişim kurabilir.

Büyük örneği damgasında dağıtılan bir kiracıda bir Azure aboneliği için faturalandırma için atanır. Bir ağ için diğer Azure aboneliklerine aynı Azure kayıt içinde olan sanal ağlardan erişilebilir. Aynı Azure bölgesindeki başka bir Azure aboneliğiyle dağıtırsanız, ayrılmış bir HANA büyük örneği kiracısı için sorulacak seçebilirsiniz.

HANA büyük örneği üzerinde SAP HANA ve Azure'da dağıtılan VM'ler üzerinde çalışan SAP HANA çalıştırma arasındaki önemli farklar vardır:

- SAP hana (büyük örnekler) azure'da sanallaştırma katmanı yoktur. Temel alınan çıplak bilgisayar donanım performansını sahip olursunuz.
- Azure, SAP HANA (büyük örnekler) Azure sunucusunda belirli bir müşteriye ayrılmış. Sunucusu birimi veya ana bilgisayar sabit veya geçici bölümlenmiş, kaybetme riski yoktur. Sonuç olarak, HANA büyük örneği birim bir bütün olarak bir kiracı ve, size atanmış olarak kullanılır. Bir yeniden başlatma veya kapatma sunucusunun işletim sistemi ve başka bir sunucuya dağıtılmasını SAP HANA için otomatik olarak yol değil. (Türünün miyim SKU'ları sınıfı, tek özel durum bir sunucu karşılaştığı sorunları ve yeniden dağıtma işlemi başka bir sunucuda gerçekleştirilmesi gerekir.)
- Burada ana bilgisayar İşlemci türleri için en iyi fiyat/performans oranını seçilir, Azure (büyük örnekler) Azure üzerinde SAP HANA için seçilen işlemci Intel E7v3 ve E7v4 işlemci satırının en yüksek gerçekleştirme türleridir.

**Sonraki adımlar**
- Başvuru [HLI boyutlandırma](hana-sizing.md)
