---
title: Seçenekleri için veri aktarım gereçlerden biri kullanarak Azure'a | Microsoft Docs
description: Verileri Azure'a aktarmak için doğru gereç seçim yapmayı öğrenin
services: storsimple
author: alkohli
ms.service: storsimple
ms.topic: article
ms.date: 11/01/2018
ms.author: alkohli
ms.openlocfilehash: 0cb1a0bccbb989506988f36c515d59cddb832265
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51263550"
---
# <a name="compare-storsimple-with-azure-file-sync-and-data-box-edge-data-transfer-options"></a>StorSimple ile Azure dosya eşitleme ve veri kutusu Edge veri aktarımı seçeneklerini karşılaştırma 
 
Bu belge, Azure'da şirket içi veri aktarımı için seçeneklerine genel bakış sağlar. karşılaştırma: Veri kutusu Edge ile StorSimple 8000 serisi Azure dosya eşitleme (AFS) vs.

- **[Veri kutusu Edge](/azure/databox-online/data-box-edge-overview.md)**  – veri kutusu Edge olan verileri azure'a veya Azure dışına taşır ve verileri karşıya yükleme sırasında önceden işlemek için işlem Edge yapay ZEKA özellikli olan bir şirket içi ağ cihazı. Bu Ignite 2018 Duyuruldu ve genel Önizleme aşamasındadır. Veri kutusu ağ geçidi cihazı ile aynı veri aktarımı özellikleri sanal bir sürümüdür.
- **[Azure dosya eşitleme](/azure/storage/files/storage-sync-files-deployment-guide.md)**  – Azure dosya eşitleme, kuruluşunuzun dosya paylaşımlarını Azure dosyaları'nda esneklik, performans ve bir şirket içi dosya sunucusunun uyumluluğu korurken merkezileştirmek için kullanılabilir. Azure dosya eşitleme Windows Server, Azure dosya paylaşımınızın hızlı bir önbelleğine dönüştürür. Daha önce 2018'de genel kullanıma sunulduğunu AFS Duyuruldu.
- **[StorSimple](/azure/storsimple/storsimple-overview.md)**  – StorSimple olan kuruluşlar ile sıkı bir şekilde tümleştirerek birincil depolama, veri koruma, arşivleme ve olağanüstü durum kurtarma tek bir çözüm için kendi depolama altyapınızı birleştirin yardımcı olan bir karma cihaz Azure depolama. StorSimple için ürün yaşam döngüsü bulunabilir [burada](https://support.microsoft.com/lifecycle/search?alpha=Azure%20StorSimple%208000%20Series).

## <a name="comparison-summary"></a>Karşılaştırma özeti

|                           |StorSimple 8000   |Azure Dosya Eşitleme   |Veri kutusu Edge (Önizleme)           |
|---------------------------|----------------------------------------|-------------------------------|-----------------------------------------|
|Genel Bakış         |Katmanlı karma depolama ve Arşiv|Genel dosya sunucusu depolama ile bulut katmanlama ve çok siteli eşitleme.  |Verileri önceden işleme ve ağ üzerinden Azure'a göndermek için depolama çözümü.        |
|Senaryolar        |Dosya sunucusu, arşivleme, yedekleme hedefi |Dosya sunucusu, arşiv (çok siteli)   |Veri aktarımı, veri ön işleme dahil olmak üzere ML çıkarım, IOT, arşivleme    |
|Uç işlemi     |Kullanılamaz |Kullanılamaz |Azure IOT Edge'i kullanarak kapsayıcıları çalıştırmayı destekler    |
|Form faktörü      |Fiziksel cihaz   |Windows Server'da yüklü aracı |Fiziksel cihaz   |
|Donanım         |Hizmetin bir parçası Microsoft tarafından sağlanan fiziksel cihaz | Müşteri tarafından sağlanan |Hizmetin bir parçası Microsoft tarafından sağlanan fiziksel cihaz  |
|Veri biçimi      |Özel biçim   |Dosyalar         |BLOB veya dosyalar    |
|Protokol desteği |iSCSI          |SMB, NFS    | SMB veya NFS      |
|Fiyatlandırma          |[StorSimple'nın fiyatlandırma](https://azure.microsoft.com/pricing/details/storsimple/) |[Fiyatlandırma AFS](https://azure.microsoft.com/pricing/details/storage/files/)  |[Veri kutusu Edge fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/databox/edge/)  |

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [Azure Data Box Edge](/azure/databox-online/data-box-edge-overview.md) ve [Azure Data Box ağ geçidi](/azure/databox-online/data-box-gateway-overview.md)
- Hakkında bilgi edinin [Azure dosya eşitleme](/azure/storage/files/storage-sync-files-deployment-guide.md)