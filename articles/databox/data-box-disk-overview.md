---
title: Microsoft Azure Data Box Disk'e genel bakış | Verilerde Microsoft Docs
description: Büyük miktarlarda verinin Azure’a aktarılmasını sağlayan bir bulut çözümü olan Azure Data Box Disk’i açıklar
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: overview
ms.date: 05/15/2019
ms.author: alkohli
Customer intent: As an IT admin, I need to understand what Data Box Disk is and how it works so I can use it to import on-premises data into Azure.
ms.openlocfilehash: 194f2b80e9cbf3a69fef6ce382e6755934f1d5bd
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65787448"
---
# <a name="what-is-azure-data-box-disk"></a>Azure Data Box Disk nedir?

Microsoft Azure Data Box Disk çözümü, terabaytlarca şirket içi veriyi Azure'a hızlı, uygun maliyetli ve güvenilir bir şekilde göndermenizi sağlar. Güvenli veri aktarımının daha hızlı gerçekleştirilmesi için 1 ila 5 katı hal sürücüsü (SSD) gönderilir. Her biri 8 TB boyutundaki bu şifrelenmiş diskler bölgesel dağıtım şirketi aracılığıyla veri merkezinize gönderilir. 

Diskleri Azure portalda Data Box hizmetiyle hızlıca yapılandırabilir, bağlayabilir ve disklerin kilidini açabilirsiniz. Verilerinizi disklere kopyalayın ve diskleri Azure'a geri gönderin. Azure veri merkezinde verileriniz sürücülerden hızlı, özel bir ağ yükleme bağlantısı kullanılarak buluta yüklenir.

## <a name="use-cases"></a>Uygulama alanları

Data Box Disk'i kullanarak sıfır veya sınırlı ağ bağlantısı olan senaryolarda terabaytlarca veriyi aktarabilirsiniz. Veri taşıma işlemi tek seferlik, düzenli veya başta toplu veri aktarımı ve ardından düzenli aktarımlardan oluşabilir. 

- **Tek seferlik geçiş**: Büyük miktardaki şirket içi verilerinin Azure'a taşınmasıdır. Çevrimdışı bantlardaki verilerin Azure seyrek erişimli depolama katmanında arşivlenmesi örnek olarak verilebilir.
- **Artımlı aktarım**: Data Box Disk (seed) ile toplu veri aktarımı yapıldıktan sonra ağ üzerinden artımlı aktarım işlemlerinin gerçekleştirilmesidir. Örneğin Commvault ve Data Box Disk ile yedek kopyalar Azure'a taşınabilir. Bu geçişin ardından Azure depolamaya ağ üzerinden artımlı veri aktarımı yapılır. 
- **Düzenli yüklemeler**: Düzenli olarak oluşturulan büyük miktarlardaki verilerin Azure'a taşınması gerektiği durumlardır. Enerji keşfinde petrol kuyularında ve rüzgar enerjisini santrallerinde oluşturulan videolar örnek olarak verilebilir.

## <a name="the-workflow"></a>İş akışı

Tipik iş akışı aşağıdaki adımlardan oluşur:

1. **Sipariş**: Azure portaldan bir sipariş oluşturup gönderim bilgilerini ve verileriniz için hedef Azure depolama hesabını belirtin. Diskler mevcutsa Azure şifreler, hazırlar ve gönderip bir gönderi takip numarası iletir.

2. **Alma**: Diskler teslim edildikten sonra paketini açın ve kopyalanacak verilerin bulunduğu bilgisayara bağlayın. Disklerin kilidini açın.
    
3. **Verileri kopyalama**: Verileri sürükleyip disklere kopyalayın.

4. **İade**: Diskleri hazırlayın ve Azure veri merkezine geri gönderin.

5. **Yükleme**: Veriler otomatik olarak disklerden Azure'a kopyalanır. Diskler Ulusal Standart ve Teknoloji Kurumu (NIST) yönergelerine uygun ve güvenli bir şekilde silinir.

Bu işlem boyunca tüm durum değişiklikleri e-posta ile bildirilir. Ayrıntılı akış hakkında daha fazla bilgi için bkz. [Azure portalda Data Box Disk dağıtma](data-box-disk-quickstart-portal.md).


## <a name="benefits"></a>Avantajlar

Data Box Disk, büyük miktarlarda veriyi ağ bağlantısını etkilemeden Azure'a taşımak için tasarlanmıştır. Çözümün şöyle avantajları vardır:

- **Hız**: Data Box Disk, USB 3.0 bağlantısını kullanarak 35 TB boyutunda veriyi bir haftadan daha kısa bir sürede Azure'a aktarır.   

- **Kullanımı kolay**: Data Box, kullanımı kolay bir çözümdür.

    - Diskler USB bağlantısı kullanır ve kuruluma neredeyse hiç ihtiyaç duymaz.
    - Disklerin fiziksel boyutu küçük olduğundan kullanımı ve gönderimi kolaydır.
    - Diskler harici güç kaynağına ihtiyaç duymaz.
    - Diskler veri merkezi sunucusu, masaüstü veya dizüstü bilgisayarlar ile kullanılabilir.
    - Bu çözüm, Azure portal üzerinden uçtan uca takip imkanı sunar.    

- **Güvenli**: Data Box Disk diskler, veriler ve hizmet için yerleşik güvenlik önlemlerine sahiptir. 
    - Diskler kurcalamaya karşı korumalıdır ve güvenli güncelleştirmeyi destekler. 
    - Disklerde bulunan veriler her zaman AES 128 bit ile şifrelenir. 
    - Disklerin kilidi yalnızca Azure portaldan alınan anahtarla açılabilir. 
    - Hizmet, Azure güvenlik özelliklerinin koruması altındadır. 
    - Verileriniz Azure'a yüklendikten sonra diskler NIST 800-88r1 standartlarına göre silinir.  
    
Daha fazla bilgi için bkz. [Azure Data Box Disk güvenliği ve veri koruması](data-box-disk-security.md).


## <a name="features-and-specifications"></a>Özellikler ve belirtimler


| Belirtimler                                          | Açıklama              |
|---------------------------------------------------------|--------------------------|
| Ağırlık                                                  | < 2 lb. kutu başına. En fazla 5 diskler kutusunda                |
| Boyutlar                                              | Disk - 2,5" SSD |            
| Kablolar                                                  | Disk başına 1 USB 3.1 kablosu|
| Sipariş başına depolama kapasitesi                              | 40 TB (kullanılabilir: ~ 35 TB)|
| Disk depolama kapasitesi                                   | 8 TB (kullanılabilir: ~ 7 TB)|
| Veri arabirimi                                          | USB   |
| Güvenlik                                                | BitLocker ile önceden şifreleme ve güvenli güncelleştirme <br> Destek anahtarı korumalı diskler <br> Veriler her zaman şifrelenir  |
| Veri aktarımı hızı                                      | dosya boyutuna bağlı olarak en fazla 430 MB/sn      |
|Yönetim                                               | Azure portal |


## <a name="region-availability"></a>Bölge kullanılabilirliği

Bölge kullanılabilirliği hakkında daha fazla bilgi için Git [Azure bölgelere göre kullanılabilir ürünler](https://azure.microsoft.com/global-infrastructure/services/?products=databox&regions=all).


## <a name="pricing"></a>Fiyatlandırma

Fiyatlandırma hakkında daha fazla bilgi için Git [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/databox/disk/).

## <a name="next-steps"></a>Sonraki adımlar

- [Data Box Disk gereksinimlerini](data-box-disk-system-requirements.md) gözden geçirin.
- [Data Box Disk sınırlarını](data-box-disk-limits.md) anlayın.
- Azure portalında [Azure Data Box Disk](data-box-disk-quickstart-portal.md)’i hızlı dağıtın.
