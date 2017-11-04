---
title: "StorSimple 8000 güncelleştirme 0,1 sürüm notları | Microsoft Docs"
description: "Yeni özellikler ve düzeltmeler, açık sorunlar ve kullanılabilir geçici çözümler için Ekim 2014 açıklar Microsoft Azure StorSimple sürüm (güncelleştirme 0.1)."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: fd35e3c3-4770-460c-999d-f72ab7053a20
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 09/21/2016
ms.author: alkohli
ms.openlocfilehash: 4dfd3973593a94adfc15a6e15d69c697e13998af
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="storsimple-8000-series-update-01-release-notes--october-2014"></a>StorSimple 8000 serisi güncelleştirme 0,1 sürüm notları – Ekim 2014
## <a name="overview"></a>Genel Bakış
Aşağıdaki sürüm notları, StorSimple 8000 serisi güncelleştirme 0.1 Ekim 2014'te yayımlanan için kritik açık sorunlar tanımlayın. Bu sürümde bulunan StorSimple yazılımını ve bellenimini güncelleştirmeler listesini de içerir. StorSimple 8000 serisi yayın sürümü Temmuz 2014'te genel olarak kullanılabilir yapıldı ve 6.3.9600.17312 yazılım sürümüne karşılık gelen sonra ilk sürüm budur.  

Tarama ve cihaz yüklendikten hemen sonra kullanılabilir güncelleştirmeleri uygulamak öneririz. Güncelleştirmeleri karşıdan yükleyip kullanıma sunulduklarında hemen yüksek öncelikli Microsoft'tan Otomatik Güncelleştirmeler'de açabilirsiniz. Daha fazla bilgi için bkz: nasıl yapılır [StorSimple Cihazınızı güncelleştirin](storsimple-update-device.md).  

Lütfen StorSimple çözümünüzde güncelleştirmelerini dağıtmadan önce sürüm notları'nda yer alan bilgileri gözden geçirin.  

> [!IMPORTANT]
> * Ekim güncelleştirmeleri yüklemek için StorSimple Yöneticisi hizmeti ve değil StorSimple için Windows PowerShell kullanın.  
> * Güncelleştirmeler genellikle tamamlamak için yaklaşık 3 saat sürer.  
> * StorSimple Ekim sürümü, StorSimple sanal cihazı herhangi bir güncelleştirme içermiyor. Hala en son güvenlik düzeltmeleri de dahil olmak üzere tüm kullanılabilir Windows güncelleştirmelerini uygulayabilirsiniz, ancak sanal cihaza sürümünde bir değişiklik görmez.  
> 
> 

StorSimple Cihazınızı güncelleştirmeden önce aşağıdaki önkoşulların karşılandığından emin olun.  

* Güncelleştirmeleri taramadan önce her iki aygıt denetleyicileri çalıştığından emin olun. Her iki denetleyicisi çalışmıyorsa, tarama başarısız olur. Denetleyicileri sağlam durumda olduğunu doğrulamak için gidin **donanım durumu** altında **Bakım** sayfası. Bileşenleri varsa, **dikkat etmeniz gereken**, başka bir işlem yapmadan önce Microsoft Support başvurun.  
* Sabit IP'ler için her iki denetleyici 0 ve denetleyici 1 yönlendirilebilir ve bu güncelleştirmeleri aygıta bakım için kullanılan gibi Internet'e bağlanabilir emin olun. Kullanabileceğiniz [Bağlantıyı Sına cmdlet](https://technet.microsoft.com/library/hh849808.aspx) Ağ denetleyicisi dış ağ bağlantısına sahip olduğunu doğrulamak için outlook.com gibi dışında bilinen adresine ping işlemi.  
* Gerekli giden bağlantı noktaları, StorSimple Cihazınızda giden iletişim için kullanılabilir olduğundan emin olun. Daha fazla bilgi için bkz: [StorSimple cihazınız için ağ gereksinimleri](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device).  
* Aygıt yazılım sürümü 6.3.9600.17312 (Ekim 2014 güncelleştirmesi) eski ise, veri 2 ve veri 3 bağlantı noktalarını güncelleştirmeden önce etkinleştirilirse, devre dışı bırakın. Güncelleştirme uygulanırken etkin bağlantı noktaları veri 2 ya da veri 3 bırakırsanız kurtarma moduna aygıt denetleyicinizi neden olabilir. Lütfen ağ arabirimlerini devre dışı bıraktığınızda, ilişkili tüm birimlerin çevrimdışına alınır ve g/ç güncelleştirmesi süresi boyunca kesilmiş olabilir unutmayın.  

## <a name="whats-new-in-the-october-release"></a>Ekim sürümde yenilikler
Bu güncelleştirme aşağıdaki geliştirmeleri içerir:

* StorSimple Yöneticisi hizmeti kullanıcı Arabirimi şimdi aygıt denetleyicileri yönetmek için de kullanabilirsiniz. Eylemler dahil yönetimi, kapatma, yeniden başlatın veya bir denetleyicisinde kapatın. Daha fazla bilgi için Git [yönetmek StorSimple cihaz denetleyicilerinin](storsimple-manage-device-controller.md).  
* Haftanın günü ve saat, gün birleşimlerini göre WAN bant genişliği ayırma zamanlayabilirsiniz. Bu, daha iyi WAN bant genişliğinden saatlerde kullanmasına olanak sağlar. Farklı bant genişliği şablonları farklı bir birim kapsayıcıları için izin verilir. Daha fazla bilgi için Git [StorSimple bant genişliği şablonlarınızı yönetmeye](storsimple-manage-bandwidth-templates.md).  
* Proaktif olarak yöneticileri ve diğerleri varolan veya büyük olasılıkla yaklaşan sorunları bildirmek için e-posta bildirimleri yapılandırabilirsiniz. Daha fazla bilgi için Git [uyarı ayarlarını yapılandırmak](storsimple-manage-alerts.md#configure-alert-settings).  

## <a name="issues-fixed-in-the-october-release"></a>Ekim sürümde giderilen sorunlar
Aşağıdaki tabloda bu güncelleştirmede giderilen sorunlar özetini sağlar.  

| Hayır. | Özellik | Sorun | Fiziksel cihaz için geçerlidir | Sanal cihaz için geçerlidir |
| --- | --- | --- | --- | --- |
| 1 |Ağ arabirimleri |Önceki sürümde Ağ arabirimleri veri 2 ve veri 3 yazılımda değiştirildi. Bu güncelleştirmede düzeltilmiştir. Lütfen ayarları temizleyin ve güncelleştirmeyi yüklemeden önce bu ağ arabirimlerini devre dışı bırakın. Güncelleştirmeyi yükledikten sonra bu arabirimleri yeniden yapılandırmanız gerekir. |Evet |Hayır |
| 2 |Destek Paketi |Önceki sürümde Windows PowerShell çalıştırdıysanız **verme HcsSupportPackage** temel kart yönetim denetleyicisi (BMC) almak için cmdlet günlükleri, işlem şu uyarıyı vererek başarısız oldu: "üzerinde işlemi başarılı Bu denetleyici ancak başarısız oldu eş denetleyicisinde aşağıdaki hatalar nedeniyle. Lütfen eş sağlıklı olup olmadığını ve geçerli düğüm eşler arası olup bağlanabilir denetleyin." Şimdi bu sorun giderilmiştir. |Evet |Hayır |
| 3 |Cihaz yük devretme |Önceki sürümde oluştu veri tutarsızlığı olasılığını varsa bir **yedekleme Bul** işi aygıt yük devretme sırasında başarısız oldu. Şimdi bu sorun giderilmiştir. |Evet |Hayır |
| 4 |Cihaz yük devretme |Önceki sürümde, bir aygıt yük devretme işleminden sonra yedeklemeler görünür ancak ilişkili birim kapsayıcısı hedef cihazda mevcut değildi. Şimdi bu sorun giderilmiştir. |Evet |Hayır |
| 5 |Cihaz yük devretme |Önceki sürümde içinde bulut yedekleme numaralandırma bulut bağlantı sorunları varsa, veri tutarsızlığına neden olabilecek kayıt defterini geri yükleme işlemi sırasında bir hata vardı. |Evet |Hayır |
| 6 |Bellenim güncelleştirme |Önceki sürümde, cihazı üretici yazılımı güncelleştirme işi başarısız oldu ve cmdlet'leri tanınabilir değildi ve güncelleştirme sonucunda başarısız belirtilen bir hata görüntülenir. Denetleyici sonra kurtarma moduna geçmiştir. Şimdi bu sorun giderilmiştir. |Evet |Hayır |
| 7 |Yükleme |Doğru yükleme sırasında görüntüsü değil aygıt tarafından kaynaklanan hataları şimdi düzeltildi. |Evet |Hayır |
| 8 |Fabrika sıfırlaması |Fabrika sıfırlaması bellenim denetle artık isteğe bağlı olarak atlayabilirsiniz. Bu, önceki sürümünden farklıdır. |Evet |Hayır |
| 9 |Fabrika sıfırlaması |Bir Fabrika sıfırlaması cmdlet'ini çalıştırdığınızda önceki sürümünde yalnızca bazı donanım bileşenleri için bellenim sürümü denetimleri yapıldı. Ek bellenim denetimleri sıfırlama başarısız olmasına neden olabilecek işleminde, ilk yeniden başlatma işleminden sonra yapıldı. Bu düzeltme, tüm Fabrika cmdlet sıfırladığınızda bellenim denetimleri yapılır çalıştırılır ve önce ilk sistem yeniden sağlar. |Evet |Hayır |
| 10 |Depolama hesabı anahtar döndürme |**Invoke-HcsmServiceDataEncryptionKeyChange** şimdi depolama hesabı anahtarlarını döndürmek için kullanılan cmdlet hizmet verileri şifreleme anahtarı girmesini ister. Bu hizmet verileri şifreleme anahtarı bir satır içi parametresi olarak geçirilen önceki sürümünden farklıdır. |Evet |Hayır |
| 11 |24 saat içinde geri dönme |Olağanüstü durum kurtarma sırasında kaynak cihazdaki temizleme başarısız olmasına neden olan, düzgün bir şekilde geri dönme gerçekleşmedi. Bu, bu sürümde düzeltilmiştir. |Evet |Hayır |

## <a name="known-issues-in-the-october-release"></a>Ekim sürümdeki bilinen sorunlar
Aşağıdaki tabloda bu sürümdeki bilinen sorunlara özetini sağlar.

| Hayır. | Özellik | Sorun | Yorumlar/geçici çözüm | Fiziksel cihaz için geçerlidir | Sanal cihaz için geçerlidir |
| --- | --- | --- | --- | --- | --- |
| 1 |Fabrika sıfırlaması |Bazı durumlarda, bir Fabrika sıfırlaması gerçekleştirdiğinizde StorSimple cihazı takılmış ve bu iletisini görüntüler: **Fabrika sıfırlamaya devam ediyor (aşama 8)**. Cmdlet sürerken CTRL + C tuşlarına basın bu olur. |Fabrika sıfırlamasına başlatıldıktan sonra CTRL + C tuşlarına değil. Bu durumda zaten varsa, Microsoft Support sonraki adımlar için temasa geçin. |Evet |Hayır |
| 2 |Fabrika sıfırlaması |GA Ekim 2014'e güncelleştirildiğinde StorSimple cihazını Fabrika sıfırlaması yapmak serbest bırakın. |Bu işlem yalnızca bir düzeltme eki yüklüyse çalışır. Microsoft bu gerekli bir düzeltme eki için Support başvurun. |Evet |Hayır |
| 3 |Disk çekirdek |8600 cihaz EBOD muhafazası diskleri çoğunluğu hiçbir disk çekirdek kaynaklanan kesildiyse nadir durumlarda, ardından depolama havuzunun çevrimdışı olur. Diskleri yeniden bağlanır olsa bile çevrimdışı kalır. |Aygıt yeniden başlatma gerekir. Sorun devam ederse, Microsoft Support sonraki adımlar için temasa geçin. |Evet |Hayır |
| 4 |Bulut anlık görüntü hataları |Nadir durumlarda, bir bulut anlık görüntüsü hatasıyla başarısız olabilir **maksimum yedekleme sınırına ulaşıldı**. Aynı aygıttan, silinmiş olan aynı özgün birimin 255 çevrimiçi kopyada aşarsa bu oluşur. | |Evet |Evet |
| 5 |Yanlış denetleyici kimliği |Bir denetleyici değiştirme gerçekleştirildiğinde, denetleyici 0 denetleyicisi 1 olarak görünebilirler. Görüntü eş düğümden yüklendiğinde denetleyicisi değiştirme sırasında denetleyici kimliği başlangıçta eş denetleyicinin kimliği olarak gösterebilir Nadir durumlarda, bu davranış bir sistem yeniden başlatıldıktan sonra görülebilir. |Hiçbir kullanıcı eylemi gerekli değildir. Denetleyici bu değişiklik tamamlandıktan sonra bu durum kendisini çözümleyin. |Evet |Hayır |
| 6 |Cihaz izleme grafikleri |StorSimple Yöneticisi hizmet cihaz izleme grafikleri basit çalışmıyor veya NTLM kimlik doğrulaması cihaz için proxy sunucusu yapılandırmasını etkinleştirilir. |Bu kimlik doğrulama NONE olarak ayarlanmış şekilde StorSimple Yöneticisi hizmetiniz ile kayıtlı cihaz için web proxy yapılandırması değiştirin. Bunu yapmak için çalıştırın StorSimple Set-HcsWebProxy cmdlet'i için Windows PowerShell. |Evet |Evet |
| 7 |Depolama hesapları |Depolama hesabını silmek için Depolama Birimi hizmetini kullanarak desteklenmeyen bir senaryodur. Bu kullanıcı verileri alınamıyor bir durum neden. | |Evet |Evet |
| 8 |Cihaz yük devretme |Farklı bir hedef cihazlara aynı kaynak cihazdaki birim kapsayıcısının birden çok yük devretme desteklenmiyor. |Tek bir ölü CİHAZDAN birden çok aygıt yük devretme veri sahipliği kaybetmek birim kapsayıcıları aygıt üzerinden başarısız ilk hale getirir. Bu tür bir yük devretme sonrasında, bu birim kapsayıcıları görünür veya Klasik Azure portalında görüntülediğinizde farklı şekilde davranır. |Evet |Hayır |
| 9 |Yükleme |SharePoint yükleme için StorSimple bağdaştırıcısı sırasında aygıt IP başarıyla tamamlamak bir yükleme için sırayla sağlamanız gerekir. | |Evet |Hayır |
| 10 |Web proxy |Web proxy yapılandırması belirtilen protokol olarak HTTPS varsa, cihazı hizmeti iletişimi etkilenecek ve cihaz çevrimdışı. Destek paketleri aygıtınızda önemli miktarda kaynak tüketen işleminde, aynı zamanda oluşturulur. |Web proxy URL'si belirtilen protokolü olarak HTTP sahip olduğundan emin olun. Hakkında daha fazla bilgi [cihazınız için web Proxy'yi Yapılandır](storsimple-configure-web-proxy.md). |Evet |Hayır |
| 11 |Web proxy |Yapılandırma ve web proxy bir kayıtlı cihazda etkinleştirirseniz, Cihazınızı etkin denetleyicisinde yeniden başlatmanız gerekir. | |Evet |Hayır |
| 12 |Yüksek bulut gecikme süresi ve yüksek g/ç iş yükü |StorSimple Cihazınızı çok yüksek bulut gecikme (saniye sırasını) ve yüksek g/ç iş yükü bileşimini karşılaştığında, düzeyi düşürülmüş bir duruma aygıt birimleri gidin ve g/ç "cihaz hazır değil" hatası ile başarısız. |El ile aygıt denetleyicileri yeniden başlatın veya bu durumdan kurtarmak için bir aygıt yük devretme gerçekleştirmek gerekir. |Evet |Hayır |

## <a name="physical-device-updates-in-the-october-release"></a>Ekim fiziksel aygıt güncelleştirmeleri sürüm
Bu güncelleştirmeler için fiziksel bir aygıtı uygulandığında, yazılım sürümü için 6.3.9600.17312 değiştirin. Aksi belirtilmediği sürece, bu sürüm notları StorSimple cihazı tüm modelleri için geçerlidir. Bu güncelleştirmeler hakkında daha fazla bilgi için bkz: [Ekim 2014 fiziksel Gereci yazılım güncelleştirmesi için Microsoft Azure StorSimple Gereci](http://support.microsoft.com/kb/2986997).  

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-the-october-release"></a>Seri Bağlı SCSI (SAS) denetleyicisi ve bellenim güncelleştirmeleri Ekim de serbest bırakın
Bu sürüm, sürücü ve bellenim fiziksel cihazınızın SAS denetleyicisinde güncelleştirir. SAS denetleyicisi güncelleştirmesi hakkında daha fazla bilgi için bkz: [Microsoft Azure StorSimple Gereci LSI SAS denetleyicileri için Ekim 2014 güncelleştirmesi](http://support.microsoft.com/kb/2987020).   

Bu sürüm ayrıca aygıt donanım bileşenleriyle birlikte güvenilirlik sorunlarını ele toplu üretici yazılımı güncelleştirmesi için geçerlidir. Üretici yazılımı güncelleştirmesi hakkında daha fazla bilgi için bkz: [Microsoft Azure StorSimple Gereci Ekim 2014 üretici yazılımı güncelleştirmesi](http://support.microsoft.com/kb/2987015).  

## <a name="virtual-device-updates-in-the-october-release"></a>Ekim sanal aygıt güncelleştirmeleri sürüm
Bu sürümde, sanal cihaz için herhangi bir güncelleştirme içermiyor. Bu güncelleştirmeyi uyguladıktan sanal cihazı yazılımı sürümü değişmez.

