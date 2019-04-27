---
title: StorSimple 8000 serisi güncelleştirme 1.2 sürüm notları | Microsoft Docs
description: StorSimple 8000 serisi güncelleştirme 1.2 için yeni özellikler, sorunlar ve geçici çözümleri açıklar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: 6c9aae87-6f77-44b8-b7fa-ebbdc9d8517c
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/03/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 11138857e33eec0f854ddb61956ea24c858c49a5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60531011"
---
# <a name="update-12-release-notes-for-your-storsimple-8000-series-device"></a>1.2 sürüm notları için StorSimple 8000 serisi Cihazınızı güncelleştirme

## <a name="overview"></a>Genel Bakış
Aşağıdaki sürüm notları, yeni özellikleri açıklar ve kritik açık sorunlar için StorSimple 8000 serisi güncelleştirme 1.2 belirleyin. Ayrıca StorSimple yazılım, sürücü ve disk üretici yazılımı güncelleştirmelerini bu sürüme dahil edilen listesini içerirler. 

Güncelleştirme 1.2 sürümü (GA), güncelleştirme 0.1, güncelleştirme 0.2 veya Update 0.3 yazılımı çalıştıran tüm StorSimple cihazına uygulanabilir. Cihazınızı güncelleştirme 1 veya güncelleştirme 1.1 çalıştırıyorsa güncelleştirmeyi 1.2 kullanılamıyor. Yayın (GA) Cihazınızı çalışıyorsa, lütfen [Microsoft Support başvurun](storsimple-contact-microsoft-support.md) bu güncelleştirmenin yüklenmesi ile yardımcı olmak için.

Aşağıdaki tabloda 1, 1.1 ve 1.2 güncelleştirmeleri karşılık gelen cihaz yazılım sürümlerini listeler.

| Güncelleştirme çalıştırılıyorsa... | Bu, cihaz yazılım sürümüdür. |
| --- | --- |
| 1.2 güncelleştir |6.3.9600.17584 |
| Güncelleştirme 1.1 |6.3.9600.17521 |
| Güncelleştirme 1.0 |6.3.9600.17491 |

Lütfen StorSimple çözümünüzde güncelleştirme dağıtmadan önce bu sürüm notlarında yer alan bilgileri gözden geçirin. Daha fazla bilgi için bkz. nasıl [güncelleştirme 1.2 StorSimple cihazınıza yüklemeniz](storsimple-install-update-1.md). 

> [!IMPORTANT]
> * Yaklaşık 5-10 (Windows güncelleştirmeleri dahil) bu güncelleştirmeyi yüklemek için saat sürer. 
> * Yazılım, LSI sürücü ve disk üretici yazılımı güncelleştirmelerini güncelleştirme 1.2 sahiptir. Yüklemek için yönergeleri izleyin. [güncelleştirme 1.2 StorSimple cihazınıza yüklemeniz](storsimple-install-update-1.md).
> * Yeni yayınlar için güncelleştirmeleri göremeyebilirsiniz hemen güncelleştirmeleri aşamalı olarak sunulmasını çünkü. Yeniden birkaç günde güncelleştirmeleri için tarama olarak yakında kullanıma sunulacaktır.
> 
> 

## <a name="whats-new-in-update-12"></a>Güncelleştirme 1.2 ile yenilikler nelerdir?
Bu özellikler, güncelleştirme 1'de sınırlı sayıda kullanıcıları için kullanılabilir olan ilk kullanıma sunulmuştur. Güncelleştirme 1.2 sürümle birlikte, aşağıdaki yeni özellikler ve geliştirmeler StorSimple kullanıcıların çoğunun görür:

* **5000-7000 serisinden bir geçiş 8000 serisi cihazlar için** – bu sürüm, StorSimple 5000-7000 Serisi Gereci kullanıcılar verilerine bir StorSimple 8000 serisi fiziksel gereç veya bir sanal geçirmenizi sağlayan yeni bir geçiş özelliğini sunmaktadır. Gereç. Geçiş özelliğini iki temel değer önermeleri sahiptir:                                                                  
  
  * **İş sürekliliği**, 5000-7000 Serisi gereçlere 8000 serisi gereçler için üzerinde mevcut verilerin geçişini etkinleştirerek.
  * **8000 serisi Gereçleri özellik teklifleri geliştirilmiş**verimli Merkezi Yönetimi StorSimple Yöneticisi hizmeti aracılığıyla birden çok Gereçleri gibi daha iyi donanım sınıfı ve üretici yazılımı, sanal gereçler, veri mobility güncelleştirildi ve gelecekteki yol haritasında özellikleri.
    
    Başvurmak [Geçiş Kılavuzu](https://gallery.technet.microsoft.com/Azure-StorSimple-50007000-c1a0460b) için nasıl bir StorSimple geçirmek 5000-7000 Serisi için 8000 serisi cihaz ayrıntıları. 
* **Azure kamu Portalı'nda kullanılabilirlik** – StorSimple Azure kamu portalında kullanıma sunuldu. Bkz. nasıl [Azure kamu Portalı'nda bir StorSimple Cihazınızı dağıtma](storsimple-deployment-walkthrough-gov.md).
* **Diğer bulut hizmeti sağlayıcıları için destek** – Amazon S3, Amazon S3 RRS, HP ve OpenStack (beta) ile desteklenen diğer bulut hizmeti sağlayıcıları şunlardır.
* **En son depolama API'leri güncelleştirmeye** – bu sürümle birlikte, en son Azure depolama hizmetine API'leri StorSimple güncelleştirildi. Güncelleştirme 1 öncesi yazılım sürümleri çalıştıran StorSimple 8000 serisi cihazlar (sürüm, 0.1, 0.2 ve 0,3) Azure depolama hizmeti API'lerini 17 Temmuz 2009'dan eski bir sürümünü kullanıyorsunuz. Güncelleştirilmiş belirtildiği gibi [depolama hizmeti sürümleri kaldırılmasını hakkında duyuru](https://blogs.msdn.com/b/windowsazurestorage/archive/2015/10/19/microsoft-azure-storage-service-version-removal-update-extension-to-2016.aspx), 1 Ağustos 2016 tarihine kadar bu API kullanım dışı bırakılacaktır. StorSimple 8000 Series güncelleştirme 1 1 Ağustos 2016'dan önce uyguladığınız zorunludur. Bunu yapmak başarısız olursa, StorSimple cihazları doğru bir şekilde çalışmayı durdurur.
* **Bölgesel olarak yedekli depolama (ZRS için) desteği** – bölgesel olarak yedekli depolama (ZRS) yerel olarak yedekli depolama (LRS) ve coğrafi olarak yedekli depolama (GRS ek olarak StorSimple 8000 serisi depolama API'leri, en son sürüme yükseltmeye destekler ). Bu [Azure depolama yedekliliği seçenekleri hakkında daha fazla makale](../storage/common/storage-redundancy.md) ZRS Ayrıntılar için.
* **İlk dağıtım ve güncelleştirme deneyimini Gelişmiş** – bu sürümde yükleme ve güncelleştirme işlemleri geliştirilmiştir. Kurulum Sihirbazı'nı yükleme, güvenlik duvarı ayarları ve ağ yapılandırması doğru değilse, kullanıcıya geri bildirim sağlamak için geliştirilmiştir. Ek tanılama cmdlet'leri cihazın ağ gidermeye yardımcı olması için sağlanmıştır. Bkz: [dağıtma makalesi sorun giderme](storsimple-troubleshoot-deployment.md) sorun giderme için kullanılan yeni tanılama cmdlet'ler hakkında daha fazla bilgi için.

## <a name="issues-fixed-in-update-12"></a>Güncelleştirme 1.2 ile giderilen sorunlar
Aşağıdaki tabloda, güncelleştirmeleri 1.2, 1.1 ve 1'de düzeltilen sorunlara ilişkin bir Özet sağlar.    

| Hayır. | Özellik | Sorun | Düzeltilen güncelleştirme | Fiziksel cihaz için geçerlidir | Sanal cihaza uygulanır |
| --- | --- | --- | --- | --- | --- |
| 1 |StorSimple için Windows PowerShell |Bir kullanıcı StorSimple için Windows PowerShell kullanarak StorSimple cihazı uzaktan erişilir ve ardından Kurulum Sihirbazı'nı kullanmaya bir kilitlenme IP girişi Data 0 olan en kısa sürede oluştu. Bu hata, güncelleştirme 1'de artık düzeltildi. |Güncelleştirme 1 |Evet |Evet |
| 2 |Fabrika sıfırlaması |Bazı durumlarda, bir Fabrika sıfırlaması gerçekleştirildiğinde StorSimple cihazı takılı hale geldi ve bu ileti görüntülenir: **Fabrika sıfırlama ediyor (aşama 8)**. Bu cmdlet devam ederken CTRL + C basıldıysa oldu. Bu hata artık düzeltildi. |Güncelleştirme 1 |Evet |Hayır |
| 3 |Fabrika sıfırlaması |Başarısız olan çift denetleyici Fabrika sıfırlaması sonra cihaz kayıt işlemine devam etmek için izin verildi. Bu desteklenmeyen sistem yapılandırması sonuçlandı. Güncelleştirme 1'de bir hata iletisi gösterilir ve kayıt başarısız bir Fabrika sıfırlaması sahip bir cihazda engellenir. |Güncelleştirme 1 |Evet |Hayır |
| 4 |Fabrika sıfırlaması |Bazı durumlarda, yanlış pozitif uyuşmazlığı uyarılar ortaya çıktı. Yanlış uyuşmazlığı uyarıları güncelleştirme 1 çalıştıran cihazlarda artık oluşturulur. |Güncelleştirme 1 |Evet |Hayır |
| 5 |Fabrika sıfırlaması |Fabrika sıfırlaması tamamlanmadan kesildiyse, cihaz kurtarma moduna ve StorSimple için Windows PowerShell erişmesine izin vermedi. Bu hata artık düzeltildi. |Güncelleştirme 1 |Evet |Hayır |
| 6 |Olağanüstü durum kurtarma |Burada görüntülerle DR bulunması sırasında hedef cihazda yedeklemeler başarısız olur bir olağanüstü durum kurtarma (DR) hata düzeltildi. |Güncelleştirme 1 |Evet |Evet |
| 7 |İzleme LED'lerini |Bazı durumlarda, gereç arkasına izleme LED'lerini doğru durum belirtmeyen. Mavi LED kapatıldı. DATA 0 ve 1 LED'lerini veri bile, bu arabirimler yapılandırılmamış yanıp. Sorun düzeltilmiştir ve izleme LED'lerini artık doğru durumunu gösterir. |Güncelleştirme 1 |Evet |Hayır |
| 8 |İzleme LED'lerini |Bazı durumlarda, güncelleştirme 1'i uyguladıktan sonra etkin denetleyiciyi mavi ışığını'böylece onu etkin denetleyiciyi belirleme zor yapma açık. Bu düzeltme eki sürümde bu sorun düzeltilmiştir. |1.2 güncelleştir |Evet |Hayır |
| 9 |Ağ arabirimleri |Önceki sürümlerde yönlendirilemeyen bir ağ geçidi ile yapılandırılmış bir StorSimple cihazı çevrimdışı duruma. Bu sürümde, yönlendirme ölçüm için veri 0 olan bir düşük; yapılan Bu nedenle, bulut özellikli diğer ağ arabirimleri olsa bile, CİHAZDAN tüm bulut trafiği Data 0 yönlendirilir. |Güncelleştirme 1 |Evet |Evet |
| 10 |Yedeklemeler |Güncelleştirme 1.1 yama sürümünde yedeklemeleri 24 gün sonra başarısız olmasına neden güncelleştirme 1'de bir hata düzeltildi. |Güncelleştirme 1.1 |Evet |Evet |
| 11 |Yedeklemeler |Önceki sürümlerde hata düşük değişiklik oranları bulut anlık görüntüleri için zayıf performans sonuçlandı. Bu düzeltme eki sürümde bu hata düzeltildi. |1.2 güncelleştir |Evet |Evet |
| 12 |Güncelleştirmeler |Güncelleştirme 1, başarısız bir yükseltmeyi bildirilen ve denetleyiciler kurtarma moduna gitmek neden bir hata bu yama sürümünde düzeltilmiştir. |1.2 güncelleştir |Evet |Evet |

## <a name="known-issues-in-update-12"></a>Güncelleştirme 1.2'de bilinen sorunlar
Aşağıdaki tabloda, bu sürümdeki bilinen sorunlara ilişkin bir Özet sağlar.

| Hayır. | Özellik | Sorun | Yorumlar/geçici çözüm | Fiziksel cihaz için geçerlidir | Sanal cihaza uygulanır |
| --- | --- | --- | --- | --- | --- |
| 1 |Disk çekirdek |8600 bir cihaz EBOD muhafazası diskler çoğu hiçbir disk çekirdeği kaynaklanan kesilirse nadir durumlarda, ardından depolama havuzunun çevrimdışı olur. Diskleri yeniden bağlantı kuruldu bile çevrimdışı kalır. |Cihaz yeniden başlatma gerekir. Lütfen sorun devam ederse, Microsoft Support için sonraki adımlara başvurun. |Evet |Hayır |
| 2 |Yanlış denetleyici kimliği |Denetleyici değişiklik yapıldığında, denetleyici 0 denetleyici 1 görünebilir. Eş düğümünden, görüntüyü yüklendiğinde denetleyicisi değiştirme sırasında denetleyici kimliği başlangıçta eş denetleyicinin kimliği olarak gösterebilirsiniz Nadir durumlarda, bu davranışı sistem yeniden başlatma sonrası görülebilir. |Hiçbir kullanıcı eylemi gerekli değildir. Denetleyici bu değişiklik tamamlandıktan sonra bu durum kendi çözülecektir. |Evet |Hayır |
| 3 |Depolama hesapları |Depolama hesabını silmek için depolama hizmeti kullanarak desteklenmeyen bir senaryodur. Bu, kullanıcı verilerini geri alınamaz bir durum için yol açacaktır. |Evet |Evet | |
| 4 |Cihaz yük devretme |Farklı bir hedef cihazlara aynı kaynak cihazdaki birim kapsayıcısının birden çok yük devretme işlemleri desteklenmiyor. Birden çok cihaz için cihaz yük devretme tek bir ölü CİHAZDAN veri sahipliği kaybetmek birim kapsayıcıları cihaz başarısız ilk neden olur. Böyle bir yük devrinden sonra bu birim kapsayıcıları görünür veya Azure Klasik Portalı'nda görüntülediğinizde farklı davranır. | |Evet |Hayır |
| 5 |Yükleme |SharePoint yüklemesi için StorSimple bağdaştırıcısını sırasında yükleme başarıyla tamamlanması için sırayla bir cihazın IP sağlamanız gerekir. | |Evet |Hayır |
| 6 |Web proxy'si |Belirtilen Protokolü HTTPS, web proxy yapılandırması varsa, cihazı hizmeti iletişiminizin etkilenecek ve cihaz çevrimdışı olarak geçer. Destek paketleri, Cihazınızda önemli miktarda kaynak tüketen işleminde, aynı zamanda oluşturulur. |Web proxy URL'si belirtilen protokolü olarak HTTP olduğundan emin olun. Daha fazla bilgi için [Cihazınız için web ara sunucusunu yapılandırma](storsimple-configure-web-proxy.md)’ya gidin. |Evet |Hayır |
| 7 |Web proxy'si |Yapılandırırsanız ve bir kayıtlı cihazda web Ara sunucusunu etkinleştirme, Cihazınızda etkin denetleyiciyi yeniden başlatmanız gerekir. | |Evet |Hayır |
| 8 |Bulut yüksek gecikme süresi ve yüksek g/ç iş yükü |StorSimple Cihazınızı çok yüksek bulut gecikme (saniye cinsinden sırası) ve yüksek g/ç iş yükü karşılaştığında, cihaz birimlerine düzeyi düşürülmüş bir duruma geçmesine ve g/ç bir "cihaz hazır değil" hatası ile başarısız olabilir. |El ile cihaz denetleyicileri yeniden veya bu durumdan kurtulmak için cihaz yük devretme gerçekleştirmeniz gerekecektir. |Evet |Hayır |
| 9 |Azure PowerShell |StorSimple cmdlet kullandığınızda **Get-AzureStorSimpleStorageAccountCredential &#124; Select-Object - ilk 1 - bekleme** yeni oluşturabilmeniz ilk nesneyi seçmek için **VolumeContainer** nesnesi, cmdlet tüm nesneleri döndürür. |Cmdlet, parantez içine şu şekilde kaydır: **(Get-Azure-StorSimpleStorageAccountCredential) &#124; Select-Object - ilk 1 - bekleme** |Evet |Evet |
| 10 |Geçiş |Geçiş için birden çok birim kapsayıcıları geçirildiğinde ETA en son yedekleme için yalnızca ilk birim kapsayıcısı için doğru olur. Ayrıca, ilk 4 yedeklemeler ilk birim kapsayıcısı, geçirildikten sonra paralel geçiş başlar. |Bir birim kapsayıcısı teker teker geçirmek öneririz. |Evet |Hayır |
| 11 |Geçiş |Geri yüklemeden sonra yedekleme İlkesi ya da sanal disk grubu birimleri eklenmez. |Yedeklemeler oluşturmak için bir yedekleme ilkesi için bu birimler eklemeniz gerekir. |Evet |Evet |
| 12 |Geçiş |5000/7000 Serisi cihaz, geçiş tamamlandıktan sonra geçirilen verileri kapsayıcıları erişmemelidir. |Geçiş tamamlandı ve kaydedilmiş duruma geldikten sonra geçirilen verileri kapsayıcıları silmenizi öneririz. |Evet |Hayır |
| 13 |Kopyalama ve DR |Güncelleştirme 1 çalıştıran bir StorSimple cihazına kopyalayın veya güncelleştirme 1 yazılımı öncesini çalıştıran bir cihazda olağanüstü durum kurtarma gerçekleştirin. |Hedef cihaz güncelleştirme 1 için güncelleştirme işlemlerini izin vermek için gerekecektir |Evet |Evet |
| 14 |Geçiş |Hiçbir ilişkili birimleri ile birim gruplarını olduğunda geçiş için yedekleme yapılandırması 5000-7000 Serisi cihazda başarısız olabilir. |Hiçbir ilişkili birimleri ile tüm boş birim gruplarını silin ve ardından yapılandırma yedeklemeyi yeniden deneyin. |Evet |Hayır |

## <a name="physical-device-updates-in-update-12"></a>Güncelleştirme 1.2 fiziksel cihaz güncelleştirmeleri
Düzeltme eki güncelleştirme 1.2 (Güncelleştirme 1'in önceki sürümlerini çalıştıran) bir fiziksel cihaza uygulanır, yazılım sürümü için 6.3.9600.17584 değişir.

## <a name="controller-and-firmware-updates-in-update-12"></a>Güncelleştirme 1.2 denetleyici ve üretici yazılımı güncelleştirmeleri
Bu sürüm, sürücü ve disk üretici yazılımı Cihazınızda güncelleştirir.

* SAS denetleyicisi güncelleştirme hakkında daha fazla bilgi için bkz. [güncelleştirme 1'de Microsoft Azure StorSimple Gereci LSI SAS denetleyicilerini](https://support.microsoft.com/kb/3043005). 
* Disk üretici yazılımı güncelleştirme hakkında daha fazla bilgi için bkz. [Disk üretici yazılımı güncelleştirme 1 için Microsoft Azure StorSimple Gereci](https://support.microsoft.com/kb/3063416).

## <a name="virtual-device-updates-in-update-12"></a>Sanal cihaz güncelleştirmeleri güncelleştirme 1.2
Bu güncelleştirme, sanal cihaz için uygulanamaz. Yeni sanal cihazları oluşturulması gerekir. 

## <a name="next-steps"></a>Sonraki adımlar
* [Güncelleştirme 1.2 cihazınıza yüklemeniz](storsimple-install-update-1.md).

