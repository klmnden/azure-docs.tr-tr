---
title: StorSimple 8000 serisi güncelleştirme 2 sürüm notları | Microsoft Docs
description: StorSimple 8000 serisi güncelleştirme 2 için yeni özellikler, sorunlar ve geçici çözümleri açıklar.
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: ''
ms.assetid: e2c8bffd-7fc5-4b77-b632-a4f59edacc3a
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/03/2017
ms.author: v-sharos
ms.openlocfilehash: f23a507ab631be553613e22cafa037291548a8aa
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64717143"
---
# <a name="storsimple-8000-series-update-2-release-notes"></a>StorSimple 8000 serisi güncelleştirme 2 sürüm notları

## <a name="overview"></a>Genel Bakış
Aşağıdaki sürüm notları, yeni özellikleri açıklar ve StorSimple 8000 serisi güncelleştirme 2 için kritik açık sorunları tanımlayın. Ayrıca StorSimple yazılım, sürücü ve disk üretici yazılımı güncelleştirmelerini bu sürüme dahil edilen listesini içerirler. 

Güncelleştirme 2 yayın (GA) ya da güncelleştirme 0.1 güncelleştirme 1.2 çalıştıran tüm StorSimple cihazı için uygulanabilir. Güncelleştirme 2 ile ilişkili cihaz 6.3.9600.17673 sürümüdür.

Lütfen StorSimple çözümünüzde güncelleştirme dağıtmadan önce bu sürüm notlarında yer alan bilgileri gözden geçirin.

> [!IMPORTANT]
> * Yaklaşık 4. ve 7 (Windows güncelleştirmeleri dahil) bu güncelleştirmeyi yüklemek için saat sürer. 
> * Güncelleştirme 2, yazılım, LSI sürücü ve SSD üretici yazılımı güncelleştirmelerini vardır.
> * Yeni yayınlar için güncelleştirmeleri göremeyebilirsiniz hemen güncelleştirmeleri aşamalı olarak sunulmasını çünkü. Birkaç gün bekleyin ve ardından güncelleştirmeleri bu olarak yeniden tarama yakında kullanıma sunulacaktır.
> 
> 

## <a name="whats-new-in-update-2"></a>Güncelleştirme 2'deki yenilikler
Güncelleştirme 2, aşağıdaki yeni özellikleri tanıtmaktadır.

* **Birimleri'yerel olarak sabitlenmiş** – önceki sürümlerinde, StorSimple 8000 serisi, veri bloklarını kullanımını temel alarak bulut katmanlı. Vardı blokları yerel kalmasını güvence altına almak için bir yolu yoktur. Bir birim oluşturduğunuzda bu birimin yerel olarak sabitlenmiş ve birincil verileri buluta katmanlanmış değil gibi güncelleştirme 2'de, bir birim belirleyebilirsiniz. Bulut veri mobility ve olağanüstü durum kurtarma amacıyla kullanılabilir, böylece yerel olarak sabitlenmiş birimlerin anlık görüntüleri bulut yedekleme için hala kopyalanır. Ayrıca, birim türünü değiştirebilirsiniz (diğer bir deyişle, dönüştürme, yerel olarak sabitlenmiş birimler için birim katmanlı ve katmanlı dönüştürme yerel olarak sabitlenmiş birimleri için). 
* **StorSimple sanal cihazı geliştirmeleri** – daha önce StorSimple 8000 serisi sanal cihaz bir olağanüstü durum kurtarma veya geliştirme/test çözümü olarak konumlandırılmış. Yalnızca bir model (model 1100) sanal cihazın vardı. Güncelleştirme 2, iki sanal cihaz modeli sunar: 
  
  * 8010 (önceden 1100 olarak bilinir) – değişiklik yok; 30 TB kapasiteli ve Azure standart depolama kullanır.
  * 8020 – 64 TB'lık bir kapasiteye sahiptir ve daha iyi performans için Azure Premium depolama kullanır.
    
    Her iki sanal cihaz modeline (8010/8020) tek bir VHD yoktur. Sanal cihaz ilk kez başlattığınızda, platform parametreleri algılar ve doğru model sürümü için geçerlidir.
* **Ağ iyileştirmeleri** – güncelleştirme 2, aşağıdaki ağ geliştirmeleri içerir:
  
  * Bir NIC başarısız olursa yük devretme gerçekleştirilmesi, birden çok NIC bulut için etkinleştirilebilir.
  * Bulut için sabit ölçümlerle yönlendirme geliştirmeleri blokları etkin.
  * Bir yük devretmeden önce başarısız kaynaklarının çevrimiçi yeniden deneyin.
  * Yeni uyarılar Hizmeti hatası.
* **Güncelleştirme geliştirmeleri** – 1.2 güncelleştirin ve daha önce StorSimple 8000 serisi iki kanallar güncelleştirildi: Kümeleme, iSCSI ve benzeri ve Microsoft Update için ikili dosyaları ve üretici yazılımı güncelleştirmesi Windows.
    Tüm güncelleştirme paketlerinin güncelleştirme 2, Microsoft Update'i kullanır. Bu düzeltme eki uygulama veya yük devretme yaptığınızda daha az zaman yol. 
* **Üretici yazılımı güncelleştirmelerini** – üretici yazılımı güncelleştirmelerini dahil edilir:
  
  * LSI: lsi_sas2.sys 2.00.72.10 ürün sürümü
  * Yalnızca SSD (HDD güncelleştirmeler): XMGG, XGEG KZ50 F6C2 ve VR08
* **Proaktif Destek** – ek tanılama bilgilerini CİHAZDAN çekmek Microsoft Update 2 sağlar. Operasyon ekibimiz sorunlarınız cihazları belirlediğinde, daha iyi CİHAZDAN bilgi toplamak ve sorunları tanılamak donanımlı duyuyoruz. **Güncelleştirme 2 kabul ederek, bu proaktif destek sağlamamız izin**.    

## <a name="issues-fixed-in-update-2"></a>Güncelleştirme 2'de düzeltilen sorunlar
Aşağıdaki tablolarda, güncelleştirme 2'de düzeltilen sorunlara ilişkin bir Özet sağlar.    

| Hayır. | Özellik | Sorun | Fiziksel cihaz için geçerlidir | Sanal cihaza uygulanır |
| --- | --- | --- | --- | --- |
| 1 |Ağ arabirimleri |Güncelleştirme 1'yükseltme yapıldıktan sonra StorSimple Yöneticisi hizmeti veri2 ve Data3 bağlantı noktalarını bir denetleyicisinde başarısız olduğunu bildirdi. Bu sorun düzeltilmiştir. |Evet |Hayır |
| 2 |Güncelleştirmeler |Güncelleştirme 1 için bir yükseltmeden sonra Klasik Azure portalında birden fazla cihazda sesli uyarı uyarılar oluştu. Bu sorun düzeltilmiştir. |Evet |Hayır |
| 3 |Openstack kimlik doğrulaması |Openstack bulut hizmeti sağlayıcısı olarak kullanırken, bulut kimlik doğrulama dizesi çok uzun bir hata alabilir. Bu düzeltilmiştir. |Evet |Hayır |

## <a name="known-issues-in-update-2"></a>Güncelleştirme 2 bilinen sorunlar
Aşağıdaki tabloda, bu sürümdeki bilinen sorunlara ilişkin bir Özet sağlar.

| Hayır. | Özellik | Sorun | Yorumlar / geçici çözüm | Fiziksel cihaz için geçerlidir | Sanal cihaza uygulanır |
| --- | --- | --- | --- | --- | --- |
| 1 |Disk çekirdek |8600 bir cihaz EBOD muhafazası diskler çoğu hiçbir disk çekirdeği kaynaklanan kesilirse nadir durumlarda, ardından depolama havuzunun çevrimdışı geçer. Diskleri yeniden bağlantı kuruldu bile çevrimdışı kalır. |Cihaz yeniden başlatma gerekir. Lütfen sorun devam ederse, Microsoft Support için sonraki adımlara başvurun. |Evet |Hayır |
| 2 |Yanlış denetleyici kimliği |Denetleyici değişiklik yapıldığında, denetleyici 0 denetleyici 1 görünebilir. Eş düğümünden, görüntüyü yüklendiğinde denetleyicisi değiştirme sırasında denetleyici kimliği başlangıçta eş denetleyicinin kimliği olarak gösterebilirsiniz Nadir durumlarda, bu davranışı sistem yeniden başlatma sonrası görülebilir. |Hiçbir kullanıcı eylemi gerekli değildir. Denetleyici bu değişiklik tamamlandıktan sonra bu durum kendi çözülecektir. |Evet |Hayır |
| 3 |Depolama hesapları |Depolama hesabını silmek için depolama hizmeti kullanarak desteklenmeyen bir senaryodur. Bu, kullanıcı verilerini geri alınamaz bir durum için yol açacaktır. | |Evet |Evet |
| 4 |Cihaz yük devretme |Farklı bir hedef cihazlara aynı kaynak cihazdaki birim kapsayıcısının birden çok yük devretme işlemleri desteklenmiyor. Birden fazla cihaza tek bir ölü CİHAZDAN yük devretme veri sahipliği kaybetmek birim kapsayıcıları cihaz başarısız ilk neden olur. Böyle bir yük devrinden sonra bu birim kapsayıcıları görünür veya Azure Klasik Portalı'nda görüntülediğinizde farklı davranır. | |Evet |Hayır |
| 5 |Yükleme |SharePoint yüklemesi için StorSimple bağdaştırıcısını sırasında yükleme başarıyla tamamlanması için sırayla bir cihazın IP sağlamanız gerekir. | |Evet |Hayır |
| 6 |Web ara sunucusu |Belirtilen Protokolü HTTPS, web proxy yapılandırması varsa, cihazı hizmeti iletişiminizin etkilenecek ve cihaz çevrimdışı olarak geçer. Destek paketleri, Cihazınızda önemli miktarda kaynak tüketen işleminde, aynı zamanda oluşturulur. |Web proxy URL'si belirtilen protokolü olarak HTTP olduğundan emin olun. Daha fazla bilgi için [Cihazınız için web ara sunucusunu yapılandırma](storsimple-configure-web-proxy.md)’ya gidin. |Evet |Hayır |
| 7 |Web ara sunucusu |Yapılandırırsanız ve bir kayıtlı cihazda web Ara sunucusunu etkinleştirme, Cihazınızda etkin denetleyiciyi yeniden başlatmanız gerekir. | |Evet |Hayır |
| 8 |Bulut yüksek gecikme süresi ve yüksek g/ç iş yükü |StorSimple Cihazınızı çok yüksek bulut gecikme (saniye cinsinden sırası) ve yüksek g/ç iş yükü karşılaştığında, cihaz birimlerine düzeyi düşürülmüş bir duruma geçmesine ve g/ç bir "cihaz hazır değil" hatası ile başarısız olabilir. |El ile cihaz denetleyicileri yeniden veya bu durumdan kurtulmak için cihaz yük devretme gerçekleştirmeniz gerekecektir. |Evet |Hayır |
| 9 |Azure PowerShell |StorSimple cmdlet kullandığınızda **Get-AzureStorSimpleStorageAccountCredential &#124; Select-Object - ilk 1 - bekleme** yeni oluşturabilmeniz ilk nesneyi seçmek için **VolumeContainer** nesnesi, cmdlet tüm nesneleri döndürür. |Cmdlet, parantez içine şu şekilde kaydır: **(Get-Azure-StorSimpleStorageAccountCredential) &#124; Select-Object - ilk 1 - bekleme** |Evet |Evet |
| 10 |Geçiş |Geçiş için birden çok birim kapsayıcıları geçirildiğinde ETA en son yedekleme için yalnızca ilk birim kapsayıcısı için doğru olur. Ayrıca, ilk 4 yedeklemeler ilk birim kapsayıcısı, geçirildikten sonra paralel geçiş başlar. |Bir birim kapsayıcısı teker teker geçirmek öneririz. |Evet |Hayır |
| 11 |Geçiş |Geri yüklemeden sonra yedekleme İlkesi ya da sanal disk grubu birimleri eklenmez. |Yedeklemeler oluşturmak için bir yedekleme ilkesi için bu birimler eklemeniz gerekir. |Evet |Evet |
| 12 |Geçiş |5000/7000 Serisi cihaz, geçiş tamamlandıktan sonra geçirilen verileri kapsayıcıları erişmemelidir. |Geçiş tamamlandı ve kaydedilmiş duruma geldikten sonra geçirilen verileri kapsayıcıları silmenizi öneririz. |Evet |Hayır |
| 13 |Kopyalama ve DR |Güncelleştirme 1 çalıştıran bir StorSimple cihazına kopyalayın veya güncelleştirme 1 yazılımı öncesini çalıştıran bir cihazda olağanüstü durum kurtarma gerçekleştirin. |Hedef cihaz güncelleştirme 1 için güncelleştirme işlemlerini izin vermek için gerekecektir |Evet |Evet |
| 14 |Geçiş |Hiçbir ilişkili birimleri ile birim gruplarını olduğunda geçiş için yedekleme yapılandırması 5000-7000 Serisi cihazda başarısız olabilir. |Hiçbir ilişkili birimleri ile tüm boş birim gruplarını silin ve ardından yapılandırma yedeklemeyi yeniden deneyin. |Evet |Hayır |
| 15 |Azure PowerShell cmdlet'leri ve yerel olarak sabitlenmiş birimler |Azure PowerShell cmdlet'leri aracılığıyla yerel olarak sabitlenmiş bir birim oluşturamazsınız. (Azure PowerShell ile oluşturduğunuz herhangi bir birim katmanlanmış olmaz.) |Her zaman yerel olarak sabitlenmiş birimler yapılandırmak için StorSimple Yöneticisi hizmetini kullanın. |Evet |Hayır |
| 16 |Yerel olarak sabitlenmiş birimler için kullanılabilir alan |Yerel olarak sabitlenmiş bir birim silerseniz, yeni birimler için mevcut olan alanı hemen güncelleştirilmeyebilir. StorSimple Yöneticisi hizmeti, kullanılabilir yerel alanın yaklaşık saatte güncelleştirir. |Yeni birim oluşturmak denemeden önce bir saat bekleyin. |Evet |Hayır |
| 17 |Yerel olarak sabitlenmiş birimler |Yedekleme kataloğunu, ancak yalnızca geri yükleme işi süresi için geçici bir anlık görüntü yedekleme, geri yükleme işi gösterir. Ayrıca, ön ekine sahip bir sanal disk grubu kullanıma sunduğu **tmpCollection** üzerinde **yedekleme ilkeleri** sayfasında, ancak yalnızca geri yükleme işi süresi için. |Bu davranış, geri yükleme iş birimleri veya yerel olarak sabitlenmiş ve katmanlı birimlerden oluşan bir karışımı yalnızca yerel olarak sabitlenmiş ortaya çıkabilir. Geri yükleme işi yalnızca katmanlı birimleri içeriyorsa, bu davranışı gerçekleşmez. Kullanıcı müdahalesi gerekli değildir. |Evet |Hayır |
| 18 |Yerel olarak sabitlenmiş birimler |Geri yükleme işi iptal edin ve daha sonra geri yükleme işi gösterecektir hemen bir denetleyici yük devretmesi oluşursa **başarısız** yerine **iptal**. Geri yükleme işi başarısız olursa ve daha sonra geri yükleme işi gösterecektir hemen bir denetleyici yük devretme gerçekleştikten **iptal** yerine **başarısız**. |Bu davranış, geri yükleme iş birimleri veya yerel olarak sabitlenmiş ve katmanlı birimlerden oluşan bir karışımı yalnızca yerel olarak sabitlenmiş ortaya çıkabilir. Geri yükleme işi yalnızca katmanlı birimleri içeriyorsa, bu davranışı gerçekleşmez. Kullanıcı müdahalesi gerekli değildir. |Evet |Hayır |
| 19 |Yerel olarak sabitlenmiş birimler |Geri yükleme işi iptal etmek veya geri yükleme başarısız olursa ve ardından bir denetleyici yük devretme gerçekleştikten bir ek geri yükleme işi görünür **işleri** sayfası. |Bu davranış, geri yükleme iş birimleri veya yerel olarak sabitlenmiş ve katmanlı birimlerden oluşan bir karışımı yalnızca yerel olarak sabitlenmiş ortaya çıkabilir. Geri yükleme işi yalnızca katmanlı birimleri içeriyorsa, bu davranışı gerçekleşmez. Kullanıcı müdahalesi gerekli değildir. |Evet |Hayır |
| 20 |Yerel olarak sabitlenmiş birimler |Yerel olarak sabitlenmiş bir birim için (oluşturulan ve güncelleştirme 1.2 ile kopyalanan veya öncesi) katmanlı Birim dönüştürmeyi denemeden ve alana sahip Cihazınızı değil veya bir bulut kesinti varsa, clone(s) bozulabilir. |Oluşturulan ve klonlanan ile ön güncelleştirme 2 yazılım olan birimleri ile bu sorun oluşur. Bu, nadir bir senaryo olmalıdır. | | |
| 21 |Birim dönüştürme |Bir birim dönüştürme işlemi devam ederken bir birime bağlı Acr'leri güncelleştirmez (yerel olarak sabitlenmiş katmanlı ya da tam tersi). Acr'leri güncelleştiriliyor, veri bozulmasına neden olabilir. |Gerekirse, önce birim dönüştürme Acr'leri güncelleştirin ve dönüştürme işlemi devam ederken ACR güncelleştirmeleri başka yapmayın. | | |

## <a name="controller-and-firmware-updates-in-update-2"></a>Denetleyici ve üretici yazılımı güncelleştirmelerini güncelleştirme 2
Bu sürüm, sürücü ve disk üretici yazılımı Cihazınızda güncelleştirir.

* Güncelleştirmesi LSI üretici yazılımı hakkında daha fazla bilgi için 3121900 Microsoft Bilgi Bankası makalesine bakın. 
* Disk üretici yazılımı hakkında daha fazla bilgi için güncelleştirme, Microsoft Bilgi Bankası makalesi 3121899 bakın.

## <a name="virtual-device-updates-in-update-2"></a>Güncelleştirme 2'deki sanal cihaz güncelleştirmeleri
Bu güncelleştirme, sanal cihaz için uygulanamaz. Yeni sanal cihazları oluşturulması gerekir. 

## <a name="next-step"></a>Sonraki adım
Bilgi edinmek için nasıl [güncelleştirme 2'yi yükleme](storsimple-install-update-2.md) StorSimple Cihazınızda.

