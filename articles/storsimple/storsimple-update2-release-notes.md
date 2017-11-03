---
title: "StorSimple 8000 serisi güncelleştirme 2 sürüm notları | Microsoft Docs"
description: "StorSimple 8000 serisi güncelleştirme 2 için yeni özellikler, sorunlar ve geçici çözümleri açıklar."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: e2c8bffd-7fc5-4b77-b632-a4f59edacc3a
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 09/25/2017
ms.author: v-sharos
ms.openlocfilehash: d03e45b839e3630e7f5df4b3144b823955920088
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="storsimple-8000-series-update-2-release-notes"></a>StorSimple 8000 serisi güncelleştirme 2 sürüm notları
## <a name="overview"></a>Genel Bakış
Aşağıdaki sürüm notları, yeni özellikleri açıklar ve StorSimple 8000 serisi güncelleştirme 2 için kritik açık sorunlar tanımlayın. StorSimple yazılım, sürücü ve disk Bellenim güncelleştirmeleri bu sürümde dahil listesini de içerir. 

Güncelleştirme 2 sürüm (GA) veya güncelleştirme 0.1 güncelleştirme 1.2 çalışan herhangi bir StorSimple aygıtı uygulanabilir. Güncelleştirme 2 ile ilişkili aygıt 6.3.9600.17673 sürümüdür.

Lütfen StorSimple çözümünüzde güncelleştirme dağıtmadan önce sürüm notları'nda yer alan bilgileri gözden geçirin.

> [!IMPORTANT]
> * Yaklaşık 4-7 (Windows güncelleştirmeleri dahil) bu güncelleştirmeyi yüklemek için saat sürer. 
> * Güncelleştirme 2 yazılım, LSI sürücü ve SSD Bellenim güncelleştirmeleri içerir.
> * Yeni sürümler için güncelleştirmelerinin göremeyebilirsiniz hemen biz aşamalı güncelleştirmeler yapmak için. Birkaç gün bekleyin ve ardından güncelleştirmeleri taramak üzere yeniden bunlar olarak yakında kullanılabilir hale gelecektir.
> 
> 

## <a name="whats-new-in-update-2"></a>Güncelleştirme 2'deki yenilikler
Güncelleştirme 2 aşağıdaki yeni özellikleri sunar.

* **Birimler'yerel olarak sabitlenmiş** – StorSimple 8000 serisi önceki sürümlerde kullanıma dayalı bulut için veri bloklarını katmanlı. Blokları yerel kalmasını sağlamak için hiçbir yolu yoktu. Bir birim oluştururken yerel olarak sabitlenmiş ve birincil veri birimden buluta katmanlı değil gibi güncelleştirme 2'de, bir birim belirleyebilirsiniz. Bulut veri mobility ve olağanüstü durum kurtarma işlemleri için kullanılır böylece buluta yedekleme için hala yerel olarak sabitlenmiş birimlerin anlık görüntüleri kopyalanacak. Ayrıca, birim türünü değiştirebilirsiniz (diğer bir deyişle, dönüştürme katmanlı birimlere yerel olarak sabitlenmiş birimleri ve dönüştürme yerel olarak sabitlenmiş birimler için katmanlı). 
* **StorSimple sanal cihazı geliştirmeleri** – daha önce StorSimple 8000 serisi sanal cihaz bir olağanüstü durum kurtarma ya da geliştirme ve test çözümü olarak konumlandırıldı. Sanal cihazı (modeli 1100) yalnızca bir model vardı. Güncelleştirme 2 iki sanal aygıt modeli sunar: 
  
  * 8010 (önceden 1100 olarak bilinir) – değişiklik yok; 30 TB kapasitesine sahip ve Azure standard storage kullanır.
  * 8020 – 64 TB'lık bir kapasitesine sahiptir ve iyileştirilmiş performans için Azure Premium storage kullanır.
    
    Her iki sanal aygıt modelleri (8010/8020) için tek bir VHD yoktur. Sanal cihaz ilk kez başlattığınızda, platform parametreleri algılar ve doğru model sürümü geçerlidir.
* **Ağ iyileştirmeleri** – güncelleştirme 2 aşağıdaki ağ geliştirmeleri içerir:
  
  * Yük devretme, bir NIC başarısız olursa gerçekleştirilmesi birden çok NIC bulut için etkinleştirilebilir.
  * Bulut için sabit ölçümlerle yönlendirme geliştirmeleri blokları etkin.
  * Bir yük devretme önce başarısız kaynaklarının çevrimiçi yeniden deneyin.
  * Hizmet hataları için yeni uyarıları.
* **Geliştirmeleri güncelleştirme** – 1.2 güncelleştirmek ve StorSimple 8000 serisi iki kanallar daha önce güncelleştirildi: Windows Update kümeleme, iSCSI ve benzeri ve ikili dosyaları ve bellenim için Microsoft Update için.
    Tüm güncelleştirme paketlerinin güncelleştirme 2 Microsoft Update kullanılır. Bu düzeltme eki uygulama veya yük devretme işlemlerini yaparken daha az zaman yol. 
* **Bellenim güncelleştirmeleri** – Bellenim güncelleştirmeleri eklenmiştir:
  
  * LSI: lsi_sas2.sys ürün sürümü 2.00.72.10
  * Yalnızca SSD (HDD güncelleştirme yok): XMGG, XGEG, KZ50, F6C2 ve VR08
* **Öngörülü Destek** – güncelleştirme 2 aygıttan ek tanılama bilgilerini Microsoft'a sağlar. Sorunlarınız aygıtları operations ekibimiz belirlediğinde, daha iyi aygıttan bilgi toplamak ve sorunları tanılamak donanımlı duyuyoruz. **Güncelleştirme 2 kabul ederek, bize bu öngörülü desteği sağlamak izin**.    

## <a name="issues-fixed-in-update-2"></a>Güncelleştirme 2'de giderilen sorunlar
Aşağıdaki tablolarda, güncelleştirme 2'de düzeltilen sorunlardan özetini sağlar.    

| Hayır. | Özellik | Sorun | Fiziksel cihaz için geçerlidir | Sanal cihaz için geçerlidir |
| --- | --- | --- | --- | --- |
| 1 |Ağ arabirimleri |Güncelleştirme 1'yükseltme yapıldıktan sonra StorSimple Yöneticisi hizmeti veri2 ve Data3 bağlantı noktaları bir denetleyicisinde başarısız olduğunu bildirdi. Bu sorun düzeltilmiştir. |Evet |Hayır |
| 2 |Güncelleştirmeler |Güncelleştirme 1'yükseltme yapıldıktan sonra birden çok aygıta Klasik Azure portalındaki sesli alarm uyarılar oluştu. Bu sorun düzeltilmiştir. |Evet |Hayır |
| 3 |Openstack kimlik doğrulaması |Openstack bulut hizmeti sağlayıcısı olarak kullanırken, bulut kimlik doğrulama dizesi çok uzun bir hata alabilir. Bu düzeltilmiştir. |Evet |Hayır |

## <a name="known-issues-in-update-2"></a>Güncelleştirme 2 bilinen sorunlar
Aşağıdaki tabloda bu sürümdeki bilinen sorunlara özetini sağlar.

| Hayır. | Özellik | Sorun | Yorumlar / geçici çözüm | Fiziksel cihaz için geçerlidir | Sanal cihaz için geçerlidir |
| --- | --- | --- | --- | --- | --- |
| 1 |Disk çekirdek |8600 cihaz EBOD muhafazası diskleri çoğunluğu hiçbir disk çekirdek kaynaklanan kesildiyse nadir durumlarda, ardından depolama havuzunun çevrimdışı geçer. Diskleri yeniden bağlanır olsa bile çevrimdışı kalır. |Aygıt yeniden başlatma gerekir. Sorun devam ederse, Microsoft Support sonraki adımlar için temasa geçin. |Evet |Hayır |
| 2 |Yanlış denetleyici kimliği |Bir denetleyici değiştirme gerçekleştirildiğinde, denetleyici 0 denetleyicisi 1 olarak görünebilirler. Görüntü eş düğümden yüklendiğinde denetleyicisi değiştirme sırasında denetleyici kimliği başlangıçta eş denetleyicinin kimliği olarak gösterebilir Nadir durumlarda, bu davranış bir sistem yeniden başlatıldıktan sonra görülebilir. |Hiçbir kullanıcı eylemi gerekli değildir. Denetleyici bu değişiklik tamamlandıktan sonra bu durum kendisini çözümleyin. |Evet |Hayır |
| 3 |Depolama hesapları |Depolama hesabını silmek için Depolama Birimi hizmetini kullanarak desteklenmeyen bir senaryodur. Bu kullanıcı verileri alınamıyor bir durum neden. | |Evet |Evet |
| 4 |Cihaz yük devretme |Farklı bir hedef cihazlara aynı kaynak cihazdaki birim kapsayıcısının birden çok yük devretme desteklenmiyor. Tek bir ölü CİHAZDAN birden çok aygıt yük devretme veri sahipliği kaybetmek birim kapsayıcıları aygıt üzerinden başarısız ilk hale getirir. Bu tür bir yük devretme sonrasında, bu birim kapsayıcıları görünür veya Klasik Azure portalında görüntülediğinizde farklı şekilde davranır. | |Evet |Hayır |
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
| 15 |Azure PowerShell cmdlet'leri ve yerel olarak sabitlenmiş birimleri |Azure PowerShell cmdlet'leri aracılığıyla yerel olarak sabitlenmiş bir birim oluşturamazsınız. (Azure PowerShell ile oluşturduğunuz herhangi bir birim katmanlı.) Birim türünü değiştirme istenmeyen etkisi olacağından ayrıca Azure PowerShell cmdlet'lerini yerel olarak sabitlenmiş bir birim tüm özelliklerini değiştirmek için kullanmayın çok katmanlı. |Her zaman yapılandırın veya yerel olarak sabitlenmiş birimleri değiştirmek için StorSimple Yöneticisi hizmetini kullanma. |Evet |Hayır |
| 16 |Yerel olarak sabitlenmiş birimleri için kullanılabilir alanı |Yerel olarak sabitlenmiş bir birim silerseniz, yeni birimleri için kullanılabilir alanı hemen güncelleştirilmemiş. StorSimple Yöneticisi hizmeti yerel alanınız yaklaşık olarak saatte güncelleştirir. |Yeni birim oluşturmak denemeden önce bir saat bekleyin. |Evet |Hayır |
| 17 |Yerel olarak sabitlenmiş birimleri |Geri yükleme işi geçici anlık görüntü yedekleme yedekleme kataloğunda, ancak yalnızca geri yükleme işi süresini gösterir. Ayrıca, bir sanal disk grubu önekiyle gösterir **tmpCollection** üzerinde **yedekleme ilkeleri** geri yükleme işi süresince yalnızca sayfa. |Bu davranış, geri yükleme işi birimleri veya yerel olarak sabitlenmiş ve katmanlı birimlerin bir karışımını yalnızca yerel olarak sabitlenmiş ortaya çıkabilir. Geri yükleme işi yalnızca katmanlı birimleri içeriyorsa, bu davranış gerçekleşmez. Kullanıcı müdahalesi gerekli değildir. |Evet |Hayır |
| 18 |Yerel olarak sabitlenmiş birimleri |Bir geri yükleme işi iptal edin ve daha sonra geri yükleme işi gösterecektir hemen denetleyici yük devretmesi oluşursa **başarısız** yerine **iptal edildi**. Bir geri yükleme işi başarısız olur ve daha sonra geri yükleme işi gösterecektir hemen denetleyici yük devretmesi oluşursa **iptal edildi** yerine **başarısız**. |Bu davranış, geri yükleme işi birimleri veya yerel olarak sabitlenmiş ve katmanlı birimlerin bir karışımını yalnızca yerel olarak sabitlenmiş ortaya çıkabilir. Geri yükleme işi yalnızca katmanlı birimleri içeriyorsa, bu davranış gerçekleşmez. Kullanıcı müdahalesi gerekli değildir. |Evet |Hayır |
| 19 |Yerel olarak sabitlenmiş birimleri |Geri yükleme işi iptal etmek veya geri yükleme başarısız olursa ve denetleyici yük devretmesi oluşur ek geri yükleme işi görünür **işleri** sayfası. |Bu davranış, geri yükleme işi birimleri veya yerel olarak sabitlenmiş ve katmanlı birimlerin bir karışımını yalnızca yerel olarak sabitlenmiş ortaya çıkabilir. Geri yükleme işi yalnızca katmanlı birimleri içeriyorsa, bu davranış gerçekleşmez. Kullanıcı müdahalesi gerekli değildir. |Evet |Hayır |
| 20 |Yerel olarak sabitlenmiş birimleri |Yerel olarak sabitlenmiş bir birim için katmanlı birim (oluşturulan ve güncelleştirme 1.2 ile kopyalanan veya öncesi) dönüştürmek deneyin ve Cihazınızı alana sahip değil veya bir bulut kesinti varsa, clone(s) bozulmuş. |Bu sorun, oluşturulan ve kopyalanan ile güncelleştirme öncesi 2 yazılım olan birimlerle oluşur. Bu, seyrek bir senaryo olmalıdır. | | |
| 21 |Birim dönüştürme |Bir birim dönüştürme işlemi devam ederken bir birime bağlı ACRs güncelleştirilmiyor (yerel olarak sabitlenmiş katmanlı veya tersi). ACRs güncelleştirme veri bozulmasına neden olabilir. |Gerekirse, birimi dönüştürmeden önce ACRs güncelleştirin ve dönüştürme işlemi devam ederken ACR güncelleştirmeleri başka yapmayın. | | |

## <a name="controller-and-firmware-updates-in-update-2"></a>Güncelleştirme 2 denetleyicisi ve bellenim güncelleştirmeleri
Bu sürüm, sürücü ve disk bellenim aygıtınızda güncelleştirir.

* LSI bellenim hakkında daha fazla bilgi için güncelleştirme, Microsoft Bilgi Bankası makalesi 3121900 bakın. 
* Disk bellenim hakkında daha fazla bilgi için güncelleştirme, Microsoft Bilgi Bankası makalesi 3121899 bakın.

## <a name="virtual-device-updates-in-update-2"></a>Güncelleştirme 2'deki sanal aygıt güncelleştirmeleri
Bu güncelleştirme, sanal cihaz için uygulanamaz. Yeni sanal cihazların oluşturulması gerekir. 

## <a name="next-step"></a>Sonraki adım
Bilgi edinmek için nasıl [güncelleştirme 2'yi yükleme](storsimple-install-update-2.md) , StorSimple Cihazınızda.

