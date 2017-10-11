---
title: "StorSimple 8000 güncelleştirme 0,3 sürüm notları | Microsoft Docs"
description: "Şubat 2015 için yeni özellikler ve düzeltmeler, açık sorunlar ve kullanılabilir geçici çözümleri açıklar Microsoft Azure StorSimple sürüm (güncelleştirme 0.3)."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: b01bfd04-f9f8-45f4-ade8-95ac2b979e6a
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 04/18/2016
ms.author: v-sharos
ms.openlocfilehash: c059fd74854813615754e67547497b7ededbe4dd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="storsimple-8000-series-update-03-release-notes---february-2015"></a>StorSimple 8000 serisi güncelleştirme 0,3 sürüm notları - Şubat 2015
## <a name="overview"></a>Genel Bakış
Aşağıdaki sürüm notları, StorSimple 8000 serisi güncelleştirme 0.3 Şubat 2015'te yayımlanan için kritik açık sorunlar tanımlayın. Bu sürümde bulunan StorSimple yazılımını ve bellenimini güncelleştirmeler listesini de içerir. StorSimple 8000 serisi yayın sürümü Temmuz 2014'te genel olarak kullanılabilir yapıldıktan sonra üçüncü sürümüdür.

Bu güncelleştirme, Ocak Update'ten aygıt yazılım sürümü değiştirmez. Sürüm 6.3.9600.17312 olmaya devam ediyor. Kontrol ederek güncelleştirmenin yüklü olduğunu doğrulayabilirsiniz **son güncelleştirilen** tarih. Tarih 2/10/2015 veya sonraki ise, güncelleştirme başarıyla yüklendi.  

Tarama ve StorSimple Cihazınızı yüklendikten hemen sonra kullanılabilir güncelleştirmeleri uygulamak öneririz. Güncelleştirmeleri karşıdan yükleyip kullanıma sunulduklarında hemen yüksek öncelikli Microsoft'tan Otomatik Güncelleştirmeler'de açabilirsiniz. Daha fazla bilgi için bkz: [StorSimple Cihazınızı güncelleştirin](storsimple-update-device.md).  

Lütfen StorSimple çözümünüzde güncelleştirme dağıtmadan önce sürüm notları'nda yer alan bilgileri gözden geçirin.  

> [!IMPORTANT]
> * StorSimple Yöneticisi hizmetiniz ve StorSimple için Windows PowerShell değil Şubat güncelleştirmeyi yüklemek için kullanın.   
> * Bu güncelleştirmeyi yüklemek için yaklaşık bir saat sürer. Toplu güncelleştirmeler yüklüyorsanız, ancak işlem yaklaşık 3 saat tamamlamak için alabilir.  
> * StorSimple Şubat sürümü, StorSimple sanal cihazı herhangi bir güncelleştirme içermiyor. En son güvenlik düzeltmeleri de dahil olmak üzere sanal cihaza hala kullanılabilir Windows güncelleştirmelerini uygulayabilirsiniz, ancak sanal cihaza sürümünde bir değişiklik görmez.  
> 
> 

StorSimple Cihazınızı güncelleştirmeden önce aşağıdaki önkoşulların karşılandığından emin olun.  

* Güncelleştirmeleri taramadan önce her iki aygıt denetleyicileri çalıştığından emin olun. Her iki denetleyicisi çalışmıyorsa, tarama başarısız olur. Denetleyicileri sağlam durumda olduğunu doğrulamak için gidin **donanım durumu** altında **Bakım** sayfası. Bileşenleri varsa, **dikkat etmeniz gereken**, başka bir işlem yapmadan önce Microsoft Support başvurun.
* Denetleyici 0 ve denetleyici 1 için sabit IP'ler yönlendirilebilir ve bu güncelleştirmeleri aygıta bakım için kullanılan gibi Internet'e bağlanabilir emin olun. Kullanabileceğiniz [Bağlantıyı Sına cmdlet](https://technet.microsoft.com/library/hh849808.aspx) denetleyicisi dış ağ bağlantısına sahip olduğunu doğrulamak için outlook.com gibi ağ dışında bilinen adresine ping işlemi.
* 80 ve 443 numaralı bağlantı noktalarını, StorSimple Cihazınızda giden iletişim için kullanılabilir olduğundan emin olun. Daha fazla bilgi için bkz: [StorSimple cihazınız için ağ gereksinimleri](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device).
* Aygıt yazılım sürümü 6.3.9600.17312 (Ekim 2014 güncelleştirmesi) eski ise, veri 2 ve veri 3 bağlantı noktalarını güncelleştirmeden önce etkinleştirilirse, devre dışı bırakın. Veri 2 ya da veri 3 güncelleştirme uyguladığınızda etkin bağlantı noktalarını bırakarak kurtarma moduna aygıt denetleyicinizi neden olabilir. Lütfen ağ arabirimlerini devre dışı bıraktığınızda, ilişkili tüm birimlerin çevrimdışına alınır ve g/ç güncelleştirmesi süresi boyunca kesilmiş olabilir unutmayın.  

## <a name="whats-new-in-the-february-release"></a>Şubat sürümde yenilikler
Fabrika oluştu. sorunu sıfırlamak için bu güncelleştirme, bir düzeltme içerir. GA yükseltilmiş aygıtları Ekim 2014 sürümünden sürüm. Daha fazla bilgi için bkz: [bu sürümde giderilen sorunlar](#issues-fixed-in-the-february-release).   

Bu güncelleştirme, yeni özellikler ve işlevsellik içermiyor.  

## <a name="issues-fixed-in-the-february-release"></a>Şubat sürümde giderilen sorunlar
Aşağıdaki tabloda bu güncelleştirmede giderilen sorun açıklanmaktadır.  

| Hayır. | Özellik | Sorun | Fiziksel cihaz için geçerlidir | Sanal cihaz için geçerlidir |
| --- | --- | --- | --- | --- |
| 1 |Fabrika sıfırlaması |İlk olarak GA sürümü (sürüm 6.3.9600.17215) yüklü olduğu, ancak Ekim güncelleştirilmiş bir cihazda bir Fabrika gerçekleştirmeye sürümü (sürüm 6.3.9600.17312). Başarısız fabrika ayarlarına sıfırlayıp ve cihaz kararsız duruma gelmesine neden olur. |Evet |Hayır |

## <a name="known-issues-in-the-february-release"></a>Şubat sürümdeki bilinen sorunlar
Aşağıdaki tabloda bu sürümdeki bilinen sorunlara özetini sağlar.

| Hayır. | Özellik | Sorun | Yorumlar/geçici çözüm | Fiziksel cihaz için geçerlidir | Sanal cihaz için geçerlidir |
| --- | --- | --- | --- | --- | --- |
| 1 |Fabrika sıfırlaması |Bazı durumlarda, bir Fabrika sıfırlaması gerçekleştirdiğinizde StorSimple cihazı takılmış ve bu iletisini görüntüler: **Fabrika sıfırlamaya devam ediyor (aşama 8)**. Cmdlet sürerken CTRL + C tuşlarına basın bu olur. |Fabrika sıfırlamasına başlatıldıktan sonra CTRL + C tuşlarına değil. Bu durumda zaten varsa, Microsoft Support sonraki adımlar için temasa geçin. |Evet |Hayır |
| 2 |Disk çekirdek |Hiçbir disk çekirdek kaynaklanan bir 8600device EBOD muhafazada diskleri çoğunluğu bağlantısı kesildiyse nadir durumlarda, ardından depolama havuzu çevrimdışı olur. Diskleri yeniden bağlanır olsa bile çevrimdışı kalır. |Aygıt yeniden başlatma gerekir. Sorun devam ederse, Microsoft Support sonraki adımlar için temasa geçin. |Evet |Hayır |
| 3 |Bulut anlık görüntü hataları |Nadir durumlarda, bir bulut anlık görüntüsü hatasıyla başarısız olabilir **maksimum yedekleme sınırına ulaşıldı**. Aynı aygıttan, silinmiş olan aynı özgün birimin 255 çevrimiçi kopyada aşarsa bu oluşur. | |Evet |Evet |
| 4 |Yanlış denetleyici kimliği |Bir denetleyici değiştirme gerçekleştirildiğinde, denetleyici 0 denetleyicisi 1 olarak görünebilirler. Görüntü eş düğümden yüklendiğinde denetleyicisi değiştirme sırasında denetleyici kimliği başlangıçta eş denetleyicinin kimliği olarak gösterebilir Nadir durumlarda, bu davranış bir sistem yeniden başlatıldıktan sonra görülebilir. |Hiçbir kullanıcı eylemi gerekli değildir. Denetleyici bu değişiklik tamamlandıktan sonra bu durum kendisini çözümleyin. |Evet |Hayır |
| 5 |Cihaz izleme grafikleri |StorSimple Yöneticisi hizmet cihaz izleme grafikleri basit çalışmıyor veya NTLM kimlik doğrulaması cihaz için proxy sunucusu yapılandırmasını etkinleştirilir. |Bu kimlik doğrulama NONE olarak ayarlanmış şekilde StorSimple Yöneticisi hizmetiniz ile kayıtlı cihaz için web proxy yapılandırması değiştirin. Bunu yapmak için çalıştırın StorSimple Set-HcsWebProxy cmdlet'i için Windows PowerShell. |Evet |Evet |
| 6 |Depolama hesapları |Depolama hesabını silmek için Depolama Birimi hizmetini kullanarak desteklenmeyen bir senaryodur. Bu kullanıcı verileri alınamıyor bir durum neden. | |Evet |Evet |
| 7 |Cihaz yük devretme |Farklı bir hedef cihazlara aynı kaynak cihazdaki birim kapsayıcısının birden çok yük devretme desteklenmiyor.    Tek bir ölü CİHAZDAN birden çok aygıt yük devretme veri sahipliği kaybetmek birim kapsayıcıları aygıt üzerinden başarısız ilk hale getirir. Bu tür bir yük devretme sonrasında, bu birim kapsayıcıları görünür veya Klasik Azure portalında görüntülediğinizde farklı şekilde davranır. | |Evet |Hayır |
| 8 |Yükleme |SharePoint yükleme için StorSimple bağdaştırıcısı sırasında aygıt IP başarıyla tamamlamak bir yükleme için sırayla sağlamanız gerekir. | |Evet |Hayır |
| 9 |Web proxy |Web proxy yapılandırması belirtilen protokol olarak HTTPS varsa, cihazı hizmeti iletişimi etkilenecek ve cihaz çevrimdışı. Destek paketleri aygıtınızda önemli miktarda kaynak tüketen işleminde, aynı zamanda oluşturulur. |Web proxy URL'si belirtilen protokolü olarak HTTP sahip olduğundan emin olun. Hakkında daha fazla bilgi [cihazınız için web Proxy'yi Yapılandır](storsimple-configure-web-proxy.md). |Evet |Hayır |
| 10 |Web proxy |Yapılandırma ve web proxy bir kayıtlı cihazda etkinleştirirseniz, Cihazınızı etkin denetleyicisinde yeniden başlatmanız gerekir. | |Evet |Hayır |
| 11 |Yüksek bulut gecikme süresi ve yüksek g/ç iş yükü |StorSimple Cihazınızı çok yüksek bulut gecikme (saniye sırasını) ve yüksek g/ç iş yükü bileşimini karşılaştığında, düzeyi düşürülmüş bir duruma aygıt birimleri gidin ve g/ç "cihaz hazır değil" hatası ile başarısız. |El ile aygıt denetleyicileri yeniden başlatın veya bu durumdan kurtarmak için bir aygıt yük devretme gerçekleştirmek gerekir. |Evet |Hayır |

## <a name="physical-device-updates-in-the-february-release"></a>Şubat fiziksel aygıt güncelleştirmeleri sürüm
Bu güncelleştirme oluştu Fabrika sıfırlaması sorunu giderir GA yükseltilmiş aygıtları Ekim 2014 sürümünden sürüm. StorSimple cihazı herhangi bir diğer güncelleştirme içermiyor.  

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-the-february-release"></a>Seri Bağlı SCSI (SAS) denetleyicisi ve bellenim güncelleştirmeleri Şubat de serbest bırakın
Bu sürüm seri bağlı SCSI (SAS) denetleyicisi veya bellenim herhangi bir güncelleştirme içermiyor. Sürücü güncelleştirmesinde Ekim 2014 yayın oluştu.  

## <a name="virtual-device-updates-in-the-february-release"></a>Sanal cihaz güncelleştirmeleri Şubat sürüm
Bu sürümde, sanal cihaz için herhangi bir güncelleştirme içermiyor. Bu güncelleştirmeyi uyguladıktan sanal cihazı yazılımı sürümü değişmez.

