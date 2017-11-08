---
title: "StorSimple 8000 serisi güncelleştirme 1.2 sürüm notları | Microsoft Docs"
description: "StorSimple 8000 serisi güncelleştirme 1.2 için yeni özellikler, sorunlar ve geçici çözümleri açıklar."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 6c9aae87-6f77-44b8-b7fa-ebbdc9d8517c
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/03/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c2856cda1fde04ab61b4cf15ad0dcc3db2a9df68
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2017
---
# <a name="update-12-release-notes-for-your-storsimple-8000-series-device"></a>1.2 sürüm notları, StorSimple 8000 serisi cihazınız için güncelleştirme
> [!NOTE]
> StorSimple için Klasik portalı kullanım dışıdır. StorSimple cihaz yöneticileri yeni Azure portalına kullanımdan zamanlamaya göre otomatik olarak taşır. Bir e-posta ve bu taşıma için portal bir bildirim alırsınız. Bu belgede ayrıca yakında kullanımdan kaldırılacaktır. Taşıma hakkında herhangi bir sorunuz için bkz: [SSS: Azure portalına taşıma](storsimple-8000-move-azure-portal-faq.md).


## <a name="overview"></a>Genel Bakış
Aşağıdaki sürüm notları, yeni özellikleri açıklar ve StorSimple 8000 serisi güncelleştirme 1.2 için kritik açık sorunlar tanımlayın. StorSimple yazılım, sürücü ve disk Bellenim güncelleştirmeleri bu sürümde dahil listesini de içerir. 

Güncelleştirme 1.2 sürüm (GA), güncelleştirme 0.1, güncelleştirme 0.2 veya Update 0.3 yazılımı çalıştıran herhangi bir StorSimple cihazına uygulanabilir. Cihazınızı güncelleştirme 1 veya güncelleştirme 1.1 çalışıyorsa, güncelleştirme 1.2 kullanılamaz. Cihazınızı sürüm (GA) çalışıyorsa, lütfen [Microsoft Support başvurun](storsimple-contact-microsoft-support.md) bu güncelleştirmeyi yüklemeye yardımcı olması için.

Aşağıdaki tabloda 1, 1.1 ve 1.2 güncelleştirmeleri karşılık gelen cihaz yazılım sürümlerini listeler.

| Güncelleştirme çalıştırılıyorsa... | Bu, cihaz yazılım sürümüdür. |
| --- | --- |
| 1.2 güncelleştir |6.3.9600.17584 |
| Güncelleştirme 1.1 |6.3.9600.17521 |
| Güncelleştirme 1.0 |6.3.9600.17491 |

Lütfen StorSimple çözümünüzde güncelleştirme dağıtmadan önce sürüm notları'nda yer alan bilgileri gözden geçirin. Daha fazla bilgi için bkz: nasıl yapılır [StorSimple Cihazınızda güncelleştirme 1.2 yükleme](storsimple-install-update-1.md). 

> [!IMPORTANT]
> * Yaklaşık 5-10 (Windows güncelleştirmeleri dahil) bu güncelleştirmeyi yüklemek için saat sürer. 
> * Güncelleştirme 1.2 yazılım, LSI sürücü ve disk Bellenim güncelleştirmeleri içerir. Yüklemek için yönergeleri izleyin [StorSimple Cihazınızda güncelleştirme 1.2 yükleme](storsimple-install-update-1.md).
> * Yeni sürümler için güncelleştirmelerinin göremeyebilirsiniz hemen biz aşamalı güncelleştirmeler yapmak için. Yeniden birkaç gün içinde güncelleştirmeleri taramak üzere olarak yakında kullanılabilir hale gelecektir.
> 
> 

## <a name="whats-new-in-update-12"></a>Güncelleştirme 1.2 yenilikler nelerdir?
Bu özellikler, güncelleştirme 1'de sınırlı sayıda kullanıcılar için kullanılabilir hale ilk yayımlanmıştır. Güncelleştirme 1.2 sürümle birlikte, StorSimple kullanıcıların çoğunun aşağıdaki yeni özellikleri ve geliştirmeleri görür:

* **8000 serisi cihazlar 5000-7000 Serisi geçiş** – bu sürümde StorSimple bir StorSimple 8000 serisi fiziksel gereç ya da bir sanal gereç verilerini geçirmek 5000-7000 Serisi Gereci kullanıcılara yeni bir geçiş özelliği sunmaktadır. Geçiş özelliğini iki anahtar değer önermeleri sahiptir:                                                                  
  
  * **İş sürekliliği**, 5000-7000 Serisi uygulamalarıyla 8000 serisi cihazları üzerinde mevcut verilerin geçişini etkinleştirerek.
  * **8000 serisi cihazları özelliği teklifler geliştirilmiş**, verimli Merkezi Yönetimi StorSimple Yöneticisi hizmeti aracılığıyla birden çok Gereçleri gibi daha iyi donanım sınıfı ve üretici yazılımı, sanal gereçler, veri mobility ve gelecekteki yol haritası özelliklerinde güncelleştirildi.
    
    Başvurmak [Geçiş Kılavuzu](http://www.microsoft.com/download/details.aspx?id=47322) için bir StorSimple geçirmek nasıl 5000-7000 Serisi 8000 serisi aygıtına ayrıntıları. 
* **Azure kamu portalındaki kullanılabilirlik** – StorSimple artık Azure kamu Portalı'nda kullanılabilir durumdadır. Bkz: nasıl yapılır [Azure kamu portalındaki StorSimple Cihazınızı dağıtma](storsimple-deployment-walkthrough-gov.md).
* **Diğer bulut hizmeti sağlayıcıları için destek** – Amazon S3, Amazon S3 RRS, HP ve OpenStack (beta) ile desteklenen diğer bulut hizmeti sağlayıcıları şunlardır.
* **En son depolama API'leri güncelleştirmeye** – bu sürümle birlikte, en son Azure depolama hizmeti API StorSimple güncelleştirildi. Güncelleştirme 1 yazılımı sürümlerini çalıştıran StorSimple 8000 serisi cihazlar (sürüm, 0.1, 0.2 ve 0.3) Azure depolama hizmeti API'leri 17 Temmuz 2009'dan eski sürümlerini kullanan. Güncelleştirilmiş belirtildiği gibi [depolama hizmeti sürümleri kaldırma hakkında bildiri](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/10/19/microsoft-azure-storage-service-version-removal-update-extension-to-2016.aspx), bu API'leri 1 Ağustos 2016'da kullanım dışı kalacaktır. StorSimple 8000 serisi güncelleştirme 1 ' 1 Ağustos 2016 önce uyguladığınız zorunludur. Bunu yapmak başarısız olursa, StorSimple cihazları düzgün durdurur.
* **Bölge olarak yedekli depolama (ZRS için) desteği** – depolama API'leri en son sürümüne yükseltmeye StorSimple 8000 serisi bölge olarak yedekli depolama (ZRS) yerel olarak yedekli depolama (LRS) ve coğrafi olarak yedekli depolama (GRS) ek olarak destekleyecektir. Bu başvuruda [Azure depolama artıklığı seçeneği makale](../storage/common/storage-redundancy.md) ZRS Ayrıntılar için.
* **İlk dağıtım ve güncelleştirme deneyimini Gelişmiş** – bu sürümde yükleme ve güncelleştirme işlemleri geliştirilmiştir. Kurulum Sihirbazı'nı yükleme, güvenlik duvarı ayarları ve ağ yapılandırma doğru değilse kullanıcıyı geri bildirim sağlamak için geliştirilmiştir. Ek tanılama cmdlet'leri, ağ aygıtının gidermeye yardımcı olması için sağlanmıştır. Bkz: [deployment makalesi sorun giderme](storsimple-troubleshoot-deployment.md) sorun giderme için kullanılan yeni tanılama cmdlet'ler hakkında daha fazla bilgi için.

## <a name="issues-fixed-in-update-12"></a>Güncelleştirme 1.2 ile giderilen sorunlar
Aşağıdaki tabloda, 1.2, 1.1 ve 1 güncelleştirmelerinde düzeltilen sorunlardan özetini sağlar.    

| Hayır. | Özellik | Sorun | Düzeltilen güncelleştirme | Fiziksel cihaz için geçerlidir | Sanal cihaz için geçerlidir |
| --- | --- | --- | --- | --- | --- |
| 1 |StorSimple için Windows PowerShell |Bir kullanıcı StorSimple cihaz StorSimple için Windows PowerShell kullanarak uzaktan eriştikten sonra Kurulum Sihirbazı'nı başlattığınızda, IP giriş veri 0 olan en kısa sürede bir kilitlenme oluştu. Bu hata artık güncelleştirme 1'de düzeltilmiştir. |Güncelleştirme 1 |Evet |Evet |
| 2 |Fabrika sıfırlaması |Bazı durumlarda, bir Fabrika sıfırlaması gerçekleştirildiğinde StorSimple cihazı takılmış hale geldi ve bu ileti görüntülenir: **Fabrika sıfırlamaya devam ediyor (aşama 8)**. Bu cmdlet devam ederken CTRL + C basıldıysa oldu. Bu hata artık giderilmiştir. |Güncelleştirme 1 |Evet |Hayır |
| 3 |Fabrika sıfırlaması |Başarısız çift denetleyici üretecini sıfırladıktan sonra cihaz kayıt işlemine devam etmek için izin verildi. Desteklenmeyen Sistem Yapılandırması'nda sonuçlandı. Güncelleştirme 1'in bir hata iletisi gösterilir ve kayıt başarısız fabrika ayarlarına sıfırladı bir aygıtta engellendi. |Güncelleştirme 1 |Evet |Hayır |
| 4 |Fabrika sıfırlaması |Bazı durumlarda yanlış pozitif uyuşmazlığı uyarılar ortaya çıktı. Yanlış uyuşmazlığı uyarıları artık güncelleştirme 1 çalıştıran cihazlarda oluşturulur. |Güncelleştirme 1 |Evet |Hayır |
| 5 |Fabrika sıfırlaması |Fabrika sıfırlamasına tamamlanmadan önce kesildiyse, cihaz kurtarma moduna girilen ve StorSimple için Windows PowerShell erişmesine izin vermedi. Bu hata artık giderilmiştir. |Güncelleştirme 1 |Evet |Hayır |
| 6 |Olağanüstü durum kurtarma |Bir olağanüstü durum kurtarma (DR) hata; burada görüntülerle DR sırasında hedef aygıt yedekler keşfinin başarısız olmasına neden olabilir giderilmiştir. |Güncelleştirme 1 |Evet |Evet |
| 7 |İzleme LED'leri |Bazı durumlarda, arkasındaki Gereci LED'leri izleme doğru durum yazılmasını belirtmedi. Mavi LED kapalıdır. Veri 0 ve veri 1 LED'leri bile ne zaman bu arabirimleri yapılandırılmamış yanıp. Sorun düzeltilmiştir ve izleme LED'leri artık doğru durumunu gösterir. |Güncelleştirme 1 |Evet |Hayır |
| 8 |İzleme LED'leri |Bazı durumlarda, güncelleştirme 1'i uyguladıktan sonra etkin denetleyicisinde mavi ışık'böylece active denetleyicisi belirlemek sabit kolaylaştırarak açık. Bu düzeltme eki sürümde bu sorun düzeltilmiştir. |1.2 güncelleştir |Evet |Hayır |
| 9 |Ağ arabirimleri |Önceki sürümlerde yönlendirilemeyen bir ağ geçidi ile yapılandırılmış bir StorSimple cihazı çevrimdışı duruma. Bu sürümde, Data 0 için bir yönlendirme metriği bırakıldı en düşük önceliğe; yapılan Bu nedenle, diğer ağ arabirimleri bulut etkin olsa bile, CİHAZDAN tüm bulut trafiği veri 0 yönlendirilir. |Güncelleştirme 1 |Evet |Evet |
| 10 |Yedeklemeler |Yedeklemeleri 24 gün sonra başarısız olmasına neden güncelleştirme 1'in bir hata düzeltme eki sürümü güncelleştirme 1.1 düzeltilmiştir. |Güncelleştirme 1.1 |Evet |Evet |
| 11 |Yedeklemeler |Önceki sürümlerde bir hata performansın düşük değişiklik oranları ile bulut anlık görüntüleri için ile sonuçlandı. Bu hata, bu düzeltme eki sürümde düzeltilmiştir. |1.2 güncelleştir |Evet |Evet |
| 12 |Güncelleştirmeler |Başarısız yükseltmeyi bildirilen ve kurtarma moduna denetleyicileri neden güncelleştirme 1'in bir hata sabit bu düzeltme eki sürümde. |1.2 güncelleştir |Evet |Evet |

## <a name="known-issues-in-update-12"></a>Güncelleştirme 1.2 bilinen sorunlar
Aşağıdaki tabloda bu sürümdeki bilinen sorunlara özetini sağlar.

| Hayır. | Özellik | Sorun | Yorumlar/geçici çözüm | Fiziksel cihaz için geçerlidir | Sanal cihaz için geçerlidir |
| --- | --- | --- | --- | --- | --- |
| 1 |Disk çekirdek |8600 cihaz EBOD muhafazası diskleri çoğunluğu hiçbir disk çekirdek kaynaklanan kesildiyse nadir durumlarda, ardından depolama havuzunun çevrimdışı olur. Diskleri yeniden bağlanır olsa bile çevrimdışı kalır. |Aygıt yeniden başlatma gerekir. Sorun devam ederse, Microsoft Support sonraki adımlar için temasa geçin. |Evet |Hayır |
| 2 |Yanlış denetleyici kimliği |Bir denetleyici değiştirme gerçekleştirildiğinde, denetleyici 0 denetleyicisi 1 olarak görünebilirler. Görüntü eş düğümden yüklendiğinde denetleyicisi değiştirme sırasında denetleyici kimliği başlangıçta eş denetleyicinin kimliği olarak gösterebilir Nadir durumlarda, bu davranış bir sistem yeniden başlatıldıktan sonra görülebilir. |Hiçbir kullanıcı eylemi gerekli değildir. Denetleyici bu değişiklik tamamlandıktan sonra bu durum kendisini çözümleyin. |Evet |Hayır |
| 3 |Depolama hesapları |Depolama hesabını silmek için Depolama Birimi hizmetini kullanarak desteklenmeyen bir senaryodur. Bu kullanıcı verileri alınamıyor bir durum neden. |Evet |Evet | |
| 4 |Cihaz yük devretme |Farklı bir hedef cihazlara aynı kaynak cihazdaki birim kapsayıcısının birden çok yük devretme desteklenmiyor. Birden çok cihazı tek bir ölü CİHAZDAN cihaz yük devretme veri sahipliği kaybetmek birim kapsayıcıları aygıt üzerinden başarısız ilk hale getirir. Bu tür bir yük devretme sonrasında, bu birim kapsayıcıları görünür veya Klasik Azure portalında görüntülediğinizde farklı şekilde davranır. | |Evet |Hayır |
| 5 |Yükleme |SharePoint yükleme için StorSimple bağdaştırıcısı sırasında aygıt IP başarıyla tamamlamak bir yükleme için sırayla sağlamanız gerekir. | |Evet |Hayır |
| 6 |Web proxy |Web proxy yapılandırması belirtilen protokol olarak HTTPS varsa, cihazı hizmeti iletişimi etkilenecek ve cihaz çevrimdışı. Destek paketleri aygıtınızda önemli miktarda kaynak tüketen işleminde, aynı zamanda oluşturulur. |Web proxy URL'si belirtilen protokolü olarak HTTP sahip olduğundan emin olun. Daha fazla bilgi için [Cihazınız için web ara sunucusunu yapılandırma](storsimple-configure-web-proxy.md)’ya gidin. |Evet |Hayır |
| 7 |Web proxy |Yapılandırma ve web proxy bir kayıtlı cihazda etkinleştirirseniz, Cihazınızı etkin denetleyicisinde yeniden başlatmanız gerekir. | |Evet |Hayır |
| 8 |Yüksek bulut gecikme süresi ve yüksek g/ç iş yükü |StorSimple Cihazınızı çok yüksek bulut gecikme (saniye sırasını) ve yüksek g/ç iş yükü bileşimini karşılaştığında, düzeyi düşürülmüş bir duruma aygıt birimleri gidin ve g/ç "cihaz hazır değil" hatası ile başarısız. |El ile aygıt denetleyicileri yeniden başlatın veya bu durumdan kurtarmak için bir aygıt yük devretme gerçekleştirmek gerekir. |Evet |Hayır |
| 9 |Azure PowerShell |StorSimple cmdlet'ini kullandığınızda **Get-AzureStorSimpleStorageAccountCredential &#124; Select-Object - ilk 1 - bekleme** yeni oluşturabilmesi için ilk nesneyi seçmek için **VolumeContainer** nesnesi, cmdlet tüm nesneleri döndürür. |Cmdlet parantez içine aşağıdaki gibi kaydır: **(Get-Azure-StorSimpleStorageAccountCredential) &#124; Select-Object - ilk 1 - bekleme** |Evet |Evet |
| 10 |Geçiş |Geçiş için birden çok birim kapsayıcıları geçirildiğinde ETA son yedekleme için yalnızca ilk birim kapsayıcısı için doğru olur. Ayrıca, ilk birim kapsayıcısı ilk 4 yedeklere geçirildikten sonra paralel geçiş başlar. |Bir kerede bir birim kapsayıcısı geçirmek öneririz. |Evet |Hayır |
| 11 |Geçiş |Geri yüklendikten sonra yedekleme İlkesi ya da sanal disk grubu birimleri eklenmez. |Yedeklemeler oluşturmak için bir yedekleme İlkesi bu birimleri eklemeniz gerekir. |Evet |Evet |
| 12 |Geçiş |Geçiş tamamlandıktan sonra 5000/7000 Serisi aygıt geçirilen verileri kapsayıcıları erişmelisiniz değil. |Tam ve kaydedilmiş geçiş tamamlandıktan sonra geçirilen verileri kapsayıcıları silme öneririz. |Evet |Hayır |
| 13 |Kopya ve DR |Güncelleştirme 1 çalıştıran bir StorSimple cihazı kopyalama veya güncelleştirme 1 yazılımı öncesini çalıştıran bir cihazda olağanüstü durum kurtarma gerçekleştirin. |Hedef aygıt güncelleştirme işlemlerini izin vermek için 1 olarak güncelleştirmeniz gerekir |Evet |Evet |
| 14 |Geçiş |Birim grupları ile ilişkili birim olduğunda geçiş için yedekleme yapılandırması 5000-7000 Serisi aygıtta başarısız olabilir. |Tüm boş birim gruplarıyla ilişkili birim silin ve yapılandırma yedeklemeyi yeniden deneyin. |Evet |Hayır |

## <a name="physical-device-updates-in-update-12"></a>Güncelleştirme 1.2 fiziksel aygıt güncelleştirmeleri
Düzeltme eki güncelleştirme 1.2 (Güncelleştirme 1'den önceki sürümleri çalıştıran) bir fiziksel aygıt uygulanır, yazılım sürümü için 6.3.9600.17584 değiştirir.

## <a name="controller-and-firmware-updates-in-update-12"></a>Güncelleştirme 1.2 denetleyici ve bellenim güncelleştirmeleri
Bu sürüm, sürücü ve disk bellenim aygıtınızda güncelleştirir.

* SAS denetleyicisi güncelleştirmesi hakkında daha fazla bilgi için bkz: [Microsoft Azure StorSimple Gereci LSI SAS denetleyicileri için Güncelleştirme 1](https://support.microsoft.com/kb/3043005). 
* Disk bellenim güncelleştirme hakkında daha fazla bilgi için bkz: [Microsoft Azure StorSimple Gereci için Disk bellenim güncelleştirme 1](https://support.microsoft.com/kb/3063416).

## <a name="virtual-device-updates-in-update-12"></a>Güncelleştirme 1.2 sanal aygıt güncelleştirmeleri
Bu güncelleştirme, sanal cihaz için uygulanamaz. Yeni sanal cihazların oluşturulması gerekir. 

## <a name="next-steps"></a>Sonraki adımlar
* [Güncelleştirme 1.2 yüklemeniz](storsimple-install-update-1.md).

