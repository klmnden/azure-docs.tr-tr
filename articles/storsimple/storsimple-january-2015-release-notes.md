---
title: "StorSimple 8000 güncelleştirme 0.2 sürüm notları | Microsoft Docs"
description: "Ocak 2015 için yeni özellikler ve düzeltmeler, açık sorunlar ve kullanılabilir geçici çözümleri açıklar Microsoft Azure StorSimple sürüm (güncelleştirme 0.2)."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: d9684ae3-b38f-4678-9d70-e5dbc6b03350
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 05/16/2016
ms.author: v-sharos
ms.openlocfilehash: 2fc50f7faddb4b61ea5e7d328469fc3cc0eb6f3e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="storsimple-8000-series-update-02-release-notes---january-2015"></a>StorSimple 8000 serisi güncelleştirme 0.2 sürüm notları - Ocak 2015
## <a name="overview"></a>Genel Bakış
Aşağıdaki sürüm notları Microsoft Azure StorSimple Ocak 2015 sürümünde kritik açık sorunları tanımlayın. Bu sürümde bulunan StorSimple yazılımını ve bellenimini güncelleştirmeler listesini de içerir. StorSimple 8000 serisi yayın sürümü Temmuz 2014'te genel olarak kullanılabilir yaptıktan sonra bu ikinci bir sürümüdür.

Bu güncelleştirme, Ekim Update'ten fiziksel aygıt yazılım sürümü değiştirmez. Sürüm 6.3.9600.17312 olmaya devam ediyor. Sanal cihaz görüntüsü tarafından kullanılan görüntüsü bu sürümde değiştirildi. Bu nedenle, 20/1/2015 tarihinden sonra oluşturulan tüm yeni sanal cihazlar sürüm 6.3.9600.17361 gösterilir.  

Lütfen Ocak 2015 güncelleştirmesi için sürüm notları içinde yer alan aşağıdaki bilgileri gözden geçirin.

> [!IMPORTANT]
> * Bu güncelleştirme Windows Update'te kullanılabilir değil ve diğer güncelleştirmeleri gibi yüklenemez. Klasik Azure portalı kullanılarak güncelleştirmelerin uygulanması olsa bile, cihazınız Bu güncelleştirme almazsınız. Bu güncelleştirme, yalnızca 20 Ocak 2015 tarihinden sonra oluşturulan sanal aygıtlara uygulanır. 
> * StorSimple Ocak sürümü StorSimple fiziksel cihazı herhangi bir güncelleştirme içermiyor. En son güvenlik düzeltmeleri de dahil olmak üzere sanal cihaza hala kullanılabilir Windows güncelleştirmelerini uygulayabilirsiniz, ancak sürüm StorSimple fiziksel cihaza ilişkin bir değişikliği görmezsiniz.
> 
> 

## <a name="whats-new-in-the-january-release"></a>Ocak sürümde yenilikler
Bu güncelleştirme, sanal cihazda çevrimdışına birimlerle ilgili bir düzeltme içerir. (Bkz [bu sürümde giderilen sorunlar](#issues-fixed-in-the-january-release).)  

Güncelleştirme, yeni özellikler ve işlevsellik içermiyor.  

## <a name="issues-fixed-in-the-january-release"></a>Ocak sürümde giderilen sorunlar
Aşağıdaki tabloda bu güncelleştirmede giderilen sorun açıklanmaktadır.

| Hayır. | Özellik | Sorun | Fiziksel cihaz için geçerlidir | Sanal cihaz için geçerlidir |
| --- | --- | --- | --- | --- |
| 1 |Çevrimdışı duruma geçiyor birimleri |Birkaç dakika yüksek bulut gecikmeleri kalıcı, StorSimple sanal cihazı birimleri konaklarda çevrimdışı olur. Bu düzeltme, böylece birimleri konaklarda çevrimdışı duruma neden olacağından durumlarda en aza bulut gecikme eşiği artırır. |Hayır |Evet |

## <a name="known-issues-in-the-january-release"></a>Ocak sürümdeki bilinen sorunlar
Aşağıdaki tabloda bu sürümdeki bilinen sorunlara özetini sağlar.

| Hayır. | Özellik | Sorun | Yorumlar/geçici çözüm | Fiziksel cihaz için geçerlidir | Sanal cihaz için geçerlidir |
| --- | --- | --- | --- | --- | --- |
| 1 |Fabrika sıfırlaması |Bazı durumlarda, bir Fabrika sıfırlaması gerçekleştirdiğinizde StorSimple cihazı takılmış ve bu iletisini görüntüler: **Fabrika sıfırlamaya devam ediyor (aşama 8).** Cmdlet sürerken CTRL + C tuşlarına basın bu olur. |Fabrika sıfırlamasına başlatıldıktan sonra CTRL + C tuşlarına değil. Bu durumda zaten varsa, Microsoft Support sonraki adımlar için temasa geçin. |Evet |Hayır |
| 2 |Disk çekirdek |8600 cihaz EBOD muhafazası diskleri çoğunluğu hiçbir disk çekirdek kaynaklanan kesildiyse nadir durumlarda, ardından depolama havuzunun çevrimdışı olur. Diskleri yeniden bağlanır olsa bile çevrimdışı kalır. |Aygıt yeniden başlatma gerekir. Sorun devam ederse, Microsoft Support sonraki adımlar için temasa geçin. |Evet |Hayır |
| 3 |Bulut anlık görüntü hataları |Nadir durumlarda, bir bulut anlık görüntüsü hatasıyla başarısız olabilir **maksimum yedekleme sınırına ulaşıldı**. Aynı aygıttan, silinmiş olan aynı özgün birimin 255 çevrimiçi kopyada aşarsa bu oluşur. | |Evet |Evet |
| 4 |Yanlış denetleyici kimliği |Bir denetleyici değiştirme gerçekleştirildiğinde, denetleyici 0 denetleyicisi 1 olarak görünebilirler. Görüntü eş düğümden yüklendiğinde denetleyicisi değiştirme sırasında denetleyici kimliği başlangıçta eş denetleyicinin kimliği olarak gösterebilir  Nadir durumlarda, bu davranış bir sistem yeniden başlatıldıktan sonra görülebilir. |Hiçbir kullanıcı eylemi gerekli değildir. Denetleyici bu değişiklik tamamlandıktan sonra bu durum kendisini çözümleyin. |Evet |Hayır |
| 5 |Cihaz izleme grafikleri |StorSimple Yöneticisi hizmet cihaz izleme grafikleri basit çalışmıyor veya NTLM kimlik doğrulaması cihaz için proxy sunucusu yapılandırmasını etkinleştirilir. |Bu kimlik doğrulama NONE olarak ayarlanmış şekilde StorSimple Yöneticisi hizmetiniz ile kayıtlı cihaz için web proxy yapılandırması değiştirin. Bunu yapmak için çalıştırın StorSimple Set-HcsWebProxy cmdlet'i için Windows PowerShell. |Evet |Evet |
| 6 |Depolama hesapları |Depolama hesabını silmek için Depolama Birimi hizmetini kullanarak desteklenmeyen bir senaryodur. Bu kullanıcı verileri alınamıyor bir durum neden. | |Evet |Evet |
| 7 |Cihaz yük devretme |Farklı bir hedef cihazlara aynı kaynak cihazdaki birim kapsayıcısının birden çok yük devretme desteklenmiyor. |Tek bir ölü CİHAZDAN birden çok aygıt yük devretme veri sahipliği kaybetmek birim kapsayıcıları aygıt üzerinden başarısız ilk hale getirir. Bu tür bir yük devretme sonrasında, bu birim kapsayıcıları görünür veya Klasik Azure portalında görüntülediğinizde farklı şekilde davranır. |Evet |Hayır |
| 8 |Yükleme |SharePoint yükleme için StorSimple bağdaştırıcısı sırasında aygıt IP başarıyla tamamlamak bir yükleme için sırayla sağlamanız gerekir. | |Evet |Hayır |
| 9 |Web proxy |Web proxy yapılandırması belirtilen protokol olarak HTTPS varsa, cihazı hizmeti iletişimi etkilenecek ve cihaz çevrimdışı. Destek paketleri aygıtınızda önemli miktarda kaynak tüketen işleminde, aynı zamanda oluşturulur. |Web proxy URL'si belirtilen protokolü olarak HTTP sahip olduğundan emin olun. Daha fazla bilgi için nasıl bakın [cihazınız için web Proxy'yi Yapılandır](storsimple-configure-web-proxy.md). |Evet |Hayır |
| 10 |Web proxy |Yapılandırma ve web proxy bir kayıtlı cihazda etkinleştirirseniz, Cihazınızı etkin denetleyicisinde yeniden başlatmanız gerekir. | |Evet |Hayır |
| 11 |Yüksek bulut gecikme süresi ve yüksek g/ç iş yükü |StorSimple Cihazınızı çok yüksek bulut gecikme (saniye sırasını) ve yüksek g/ç iş yükü bileşimini karşılaştığında, düzeyi düşürülmüş bir duruma aygıt birimleri gidin ve g/ç "cihaz hazır değil" hatası ile başarısız. |El ile aygıt denetleyicileri yeniden başlatın veya bu durumdan kurtarmak için bir aygıt yük devretme gerçekleştirmek gerekir. |Evet |Hayır |

## <a name="physical-device-updates-in-the-january-release"></a>Ocak fiziksel aygıt güncelleştirmeleri sürüm
Bu güncelleştirme, diğer değişikliklerin StorSimple cihazı içermiyor.

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-the-january-release"></a>Seri Bağlı SCSI (SAS) denetleyicisi ve bellenim güncelleştirmeleri Ocak de serbest bırakın
Bu sürüm seri bağlı SCSI (SAS) denetleyicisi veya bellenim herhangi bir güncelleştirme içermiyor. Sürücü güncelleştirmesinde Ekim 2014 yayın oluştu. 

## <a name="virtual-device-updates-in-the-january-release"></a>Ocak sanal aygıt güncelleştirmeleri sürüm
Bu sürümde sanal cihaz için güncelleştirilmiş bir görüntü içerir. 20 Ocak 2015 tarihinden sonra oluşturulan tüm sanal cihazlar yazılım sürümü 6.3.9600.17361 gösterilir.

