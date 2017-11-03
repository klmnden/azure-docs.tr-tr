---
title: "StorSimple 8000 yayın sürümü sürüm notları | Microsoft Docs"
description: "Yeni özellikler, açık sorunlar ve kullanılabilir geçici çözümler için Temmuz 2014 açıklanmaktadır Microsoft Azure StorSimple sürüm."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: 12f1796e-37c3-42b4-b997-a84fc1950c20
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 04/18/2016
ms.author: v-sharos
ms.openlocfilehash: 303cdffa15fdfe9b83d0612edecafc6943d218f3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="storsimple-8000-series-release-version-release-notes---july-2014"></a>StorSimple 8000 serisi yayımlanan sürümü sürüm notları - Temmuz 2014
## <a name="overview"></a>Genel Bakış
İzleme sürüm notları için StorSimple 8000 serisi kritik açık sorunları tanımlamak Temmuz 2014 genel kullanılabilirlik (GA) Microsoft Azure StorSimple sürümü. Bu sürüm 6.3.9600.17215 yazılım sürümüne karşılık gelir.  

Aksi belirtilmediği sürece, bu sürüm notları StorSimple cihazı tüm modelleri için geçerlidir. Sürüm Notları sürekli olarak güncelleştirilir; Bunlar, çözüm gerektiren kritik sorunlar bulundukça olarak eklenir. Microsoft Azure StorSimple çözümünüzün dağıtmadan önce aşağıdaki bilgileri göz önünde bulundurun.  

## <a name="known-issues-in-this-release"></a>Bu sürümdeki bilinen sorunlar
Aşağıdaki tabloda bu sürümdeki bilinen sorunlara özetini sağlar.  

| Hayır. | Özellik | Sorun | Yorumlar/geçici çözüm | Fiziksel cihaz için geçerlidir | Sanal cihaz için geçerlidir |
| --- | --- | --- | --- | --- | --- |
| 1 |Fabrika sıfırlaması |Bazı durumlarda, bir Fabrika sıfırlaması gerçekleştirdiğinizde StorSimple cihazı takılmış ve bu iletisini görüntüler: **Fabrika sıfırlamaya devam ediyor (aşama 8)**. Cmdlet sürerken CTRL + C tuşlarına basın bu olur. |Fabrika sıfırlamasına başlatıldıktan sonra CTRL + C tuşlarına değil. Bu durumda zaten varsa, Microsoft Support sonraki adımlar için temasa geçin. |Evet |Hayır |
| 2 |Disk çekirdek |8600 cihaz EBOD muhafazası diskleri çoğunluğu hiçbir disk çekirdek kaynaklanan kesildiyse nadir durumlarda, ardından depolama havuzunun çevrimdışı olur. Diskleri yeniden bağlanır olsa bile çevrimdışı kalır. |Aygıt yeniden başlatma gerekir. Sorun devam ederse, Microsoft Support sonraki adımlar için temasa geçin. |Evet |Hayır |
| 3 |Bulut anlık görüntü hataları |Nadir durumlarda, bir bulut anlık görüntüsü hatasıyla başarısız olabilir **maksimum yedekleme sınırına ulaşıldı**. Aynı aygıttan, silinmiş olan aynı özgün birimin 255 çevrimiçi kopyada aşarsa bu oluşur. | |Evet |Evet |
| 4 |Yanlış denetleyici kimliği |Bir denetleyici değiştirme gerçekleştirildiğinde, denetleyici 0 denetleyicisi 1 olarak görünebilirler. Görüntü eş düğümden yüklendiğinde denetleyicisi değiştirme sırasında denetleyici kimliği başlangıçta eş denetleyicinin kimliği olarak gösterebilir Nadir durumlarda, bu davranış bir sistem yeniden başlatıldıktan sonra görülebilir. |Hiçbir kullanıcı eylemi gerekli değildir. Denetleyici bu değişiklik tamamlandıktan sonra bu durum kendisini çözümleyin. |Evet |Hayır |
| 5 |Cihaz izleme grafikleri |StorSimple Yöneticisi hizmet cihaz izleme grafikleri basit çalışmıyor veya NTLM kimlik doğrulaması cihaz için proxy sunucusu yapılandırmasını etkinleştirilir. |Bu kimlik doğrulama NONE olarak ayarlanmış şekilde StorSimple Yöneticisi hizmetiniz ile kayıtlı cihaz için web proxy yapılandırması değiştirin. Bunu yapmak için çalıştırın StorSimple Set-HcsWebProxy cmdlet'i için Windows PowerShell. |Evet |Evet |
| 6 |Depolama hesapları |Depolama hesabını silmek için Depolama Birimi hizmetini kullanarak desteklenmeyen bir senaryodur. Bu kullanıcı verileri alınamıyor bir durum neden. | |Evet |Evet |
| 7 |Yeniden çalışma |Olağanüstü Durum Kurtarma (DR) 24 saat içinde yeniden çalışma desteklenmez. | |Evet |Hayır |
| 8 |Cihaz yük devretme |Farklı bir hedef cihazlara aynı kaynak cihazdaki birim kapsayıcısının birden çok yük devretme desteklenmiyor. Tek bir ölü CİHAZDAN birden çok aygıt yük devretme veri sahipliği kaybetmek birim kapsayıcıları aygıt üzerinden başarısız ilk hale getirir. Bu tür bir yük devretme sonrasında, bu birim kapsayıcıları görünür veya Klasik Azure portalında görüntülediğinizde farklı şekilde davranır. | |Evet |Hayır |
| 9 |Yükleme |SharePoint yükleme için StorSimple bağdaştırıcısı sırasında yükleme başarıyla tamamlanması için bir aygıt IP sağlamanız gerekir. | |Evet |Hayır |
| 10 |Ağ arabirimleri |Veri 2 ve veri 3 ağ arabirimleri yazılımda değiştirildi. |Bu arabirimleri yapılandırmanız gerekiyorsa, lütfen Microsoft Support başvurun. |Evet |Hayır |

