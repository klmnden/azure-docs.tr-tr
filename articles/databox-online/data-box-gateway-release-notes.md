---
title: Azure veri kutusu ağ geçidi genel kullanılabilirlik sürüm notları | Microsoft Docs
description: Azure veri kutusu genel kullanım sürümünde çalışan ağ geçidi için açık kritik sorunlar ve çözümleri açıklanmaktadır.
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: article
ms.date: 03/26/2019
ms.author: alkohli
ms.openlocfilehash: f4ee3a5bd754335ab1c7f124671e9c37307a6a28
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60754208"
---
# <a name="azure-data-box-edgeazure-data-box-gateway-general-availability-release-notes"></a>Azure veri kutusu Edge/Azure veri kutusu ağ geçidi genel kullanılabilirlik sürüm notları

## <a name="overview"></a>Genel Bakış

Aşağıdaki sürüm notları kritik açık sorunlar belirlemek ve Azure veri kutusu Edge ve Azure veri kutusu ağ geçidi için çözümlenen sorunlar için genel kullanılabilirlik (GA) bırakın.

Sürüm Notları sürekli olarak güncelleştirilir ve geçici bir çözüm gerektiren kritik sorunlar bulundukça eklenir. Veri kutusu Edge/veri kutusu ağ geçidi dağıtmadan önce bu sürüm notlarında yer alan bilgileri dikkatle gözden geçirin.

GA sürümü karşılık gelen yazılım sürümleri için:

- **Veri kutusu ağ geçidi 1903 (1.5.814.447)**
- **Data Box Edge 1903 (1.5.814.447)**


## <a name="whats-new"></a>Yenilikler

- **Yeni sanal disk görüntülerini** -yeni VHDX ve VMDK artık Azure portalında kullanılabilir. Bu görüntülerin sağlamanızı, yapılandırmanızı ve yeni veri kutusu ağ geçidi GA cihazlara dağıtma indirin. Önceki Önizleme sürümleri bu sürüme güncelleştirilemez oluşturulan veri kutusu ağ geçidi cihazı. Daha fazla bilgi için Git [Azure veri kutusu ağ geçidi dağıtmaya hazırlanma](data-box-gateway-deploy-prep.md).
- **NFS Destek** -NFS desteği şu anda Önizleme aşamasındadır ve v3.0 ve v4.1 kullanılabilir veri kutusu Edge ve veri kutusu ağ cihazları erişen istemciler.
- **Depolama dayanıklılık** -bilgisayarınızı veri kutusu Edge cihazı depolama dayanıklılık özelliği ile bir veri disk hatasını hataya dayanamaz. Bu özellik şu anda önizleme sürümündedir. Depolama dayanıklılık seçerek etkinleştirebilirsiniz **esnek** seçeneğini **depolama ayarlarını** yerel web kullanıcı Arabirimi.


## <a name="known-issues-in-ga-release"></a>GA sürümündeki bilinen sorunlar

Aşağıdaki tabloda, sürüm çalıştıran, veri kutusu ağ geçidi için bilinen sorunların bir Özet sağlar.

| Hayır. | Özellik | Sorun | Geçici çözüm/açıklamaları |
| --- | --- | --- | --- |
| **1.** |Dosya türleri | Aşağıdaki dosya türlerinde desteklenmez: karakter dosyaları, dosyaları engelleme, yuva, Kanallar, simgesel bağlantılar.  |Bu dosyaları kopyalama NFS oluşturulmakta 0 uzunluklu dosyalar sonuçlarında paylaşın. Bu dosyalar bir hata durumunda kalır ve ayrıca bildirilen *error.xml*. <br> Sembolik bağlantılar dizinler için hiçbir zaman çevrimdışı olarak işaretlenmiş dizinlerde neden. Sonuç olarak, gri arası dizinleri çevrimdışı olduğunu ve tüm ilişkili içeriği tamamen Azure'da yüklenen gösterir dizinlerde göremeyebilirsiniz. |
| **2.** |Silme | NFS paylaşımını silinirse, bu sürümde bir hata nedeniyle, paylaşım ardından silinemez. Paylaşım durumu *silme*.  |Desteklenmeyen dosya adını kullanarak paylaşıma yalnızca bu gerçekleşir. |
| **3.** |Kopyala | Veri kopyalama hatasıyla başarısız oluyor:  Bir dosya sistemi sınırlaması nedeniyle istenen işlem tamamlanamadı.  |Dosya boyutu 128 KB boyutundan büyük ile ilişkili diğer veri Stream (REKLAM) desteklenmiyor.   |


## <a name="next-steps"></a>Sonraki adımlar

- [Azure veri kutusu ağ geçidi dağıtmaya hazırlanma](data-box-gateway-deploy-prep.md).
- [Azure veri kutusu Edge dağıtmaya hazırlanma](data-box-edge-deploy-prep.md).
