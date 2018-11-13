---
title: Seçenekleri için veri aktarım gereçlerden biri kullanarak Azure'a | Microsoft Docs
description: Verileri Azure'a aktarmak için doğru gereç seçim yapmayı öğrenin
services: storsimple
author: alkohli
ms.service: storsimple
ms.topic: article
ms.date: 11/12/2018
ms.author: alkohli
ms.openlocfilehash: c28eaf22d05bfda5085f9e269bda85ca0d46a7d3
ms.sourcegitcommit: 0fc99ab4fbc6922064fc27d64161be6072896b21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/13/2018
ms.locfileid: "51578136"
---
# <a name="compare-storsimple-with-azure-file-sync-and-data-box-edge-data-transfer-options"></a>StorSimple ile Azure dosya eşitleme ve veri kutusu Edge veri aktarımı seçeneklerini karşılaştırma 
 
Bu belge, Azure'da şirket içi veri aktarımı için seçeneklerine genel bakış sağlar. karşılaştırma: Veri kutusu Edge ile. Azure dosya eşitleme ile. StorSimple 8000 serisi.

- **[Veri kutusu Edge](/azure/databox-online/data-box-edge-overview.md)**  – veri kutusu Edge olan verileri azure'a veya Azure dışına taşır ve verileri karşıya yükleme sırasında önceden işlemek için işlem Edge yapay ZEKA özellikli olan bir şirket içi ağ cihazı. Bu Ignite 2018 Duyuruldu ve genel Önizleme aşamasındadır. Veri kutusu ağ geçidi cihazı ile aynı veri aktarımı özellikleri sanal bir sürümüdür.
- **[Azure dosya eşitleme](/azure/storage/files/storage-sync-files-deployment-guide.md)**  – Azure dosya eşitleme, kuruluşunuzun dosya paylaşımlarını Azure dosyaları'nda esneklik, performans ve bir şirket içi dosya sunucusunun uyumluluğu korurken merkezileştirmek için kullanılabilir. Azure dosya eşitleme Windows Server, Azure dosya paylaşımınızın hızlı bir önbelleğine dönüştürür. Azure dosya eşitleme'nin genel kullanılabilirliğinin önceki 2018'de Duyuruldu.
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
|Fiyatlandırma          |[StorSimple](https://azure.microsoft.com/pricing/details/storsimple/) |[Azure dosya eşitleme](https://azure.microsoft.com/pricing/details/storage/files/)  |[Veri kutusu Edge](https://azure.microsoft.com/pricing/details/storage/databox/edge/)  |

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [Azure Data Box Edge](/azure/databox-online/data-box-edge-overview) ve [Azure Data Box ağ geçidi](/azure/databox-online/data-box-gateway-overview)
- Hakkında bilgi edinin [Azure dosya eşitleme](/azure/storage/files/storage-sync-files-deployment-guide)
