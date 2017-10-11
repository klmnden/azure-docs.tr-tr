---
title: "StorSimple yük devretme ve olağanüstü durum kurtarma | Microsoft Docs"
description: "StorSimple Cihazınızı kendisi, başka bir fiziksel aygıt veya sanal bir cihaza yük devri öğrenin."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 5751598e-49c8-42b3-8121-fea5857a7d83
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/16/2016
ms.author: alkohli
ms.openlocfilehash: bf92ffdb16b86c4033cc96ae2abb060d90f9505e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="failover-and-disaster-recovery-for-your-storsimple-device"></a>StorSimple cihazınız için yük devretme ve olağanüstü durum kurtarma
## <a name="overview"></a>Genel Bakış
Bu öğretici bir olağanüstü durumda StorSimple cihazı vermesine gereken adımları açıklar. Bir yük devretme, verilerinizi veri merkezinde kaynak aygıtından başka bir fiziksel veya bile aynı veya farklı bir coğrafi konum bulunan sanal cihazı geçirmek izin verir. 

Olağanüstü Durum Kurtarma (DR) cihaz yük devretme özelliğini düzenlenir ve başlatıldığına **aygıtları** sayfası. Bu sayfa, StorSimple Yöneticisi hizmetinize bağlanan tüm StorSimple cihazlar tablo haline getirir. Her cihaz için kolay ad, durumu, sağlanan ve maksimum kapasite, türünü ve model görüntülenir.

![Cihazlar sayfası](./media/storsimple-device-failover-disaster-recovery/IC740972.png)

Bu öğreticideki yönergeler tüm yazılım sürümleri arasında StorSimple fiziksel ve sanal cihazlar için geçerlidir.

## <a name="disaster-recovery-dr-and-device-failover"></a>Olağanüstü Durum Kurtarma (DR) ve aygıtı yük devretme
Bir olağanüstü durum kurtarma (DR) senaryosunda birincil cihaz çalışmamaya başlar. Bu durumda, birincil cihaz olarak kullanarak başka bir aygıt başarısız aygıta ilişkili bulut verilerini taşıyabilirsiniz *kaynak* ve başka bir aygıt olarak belirterek *hedef*. Hedef aygıta geçirmek için bir veya daha fazla birim kapsayıcıları seçebilirsiniz. Bu işlem olarak adlandırılır *yük devretme*. 

Yük devretme sırasında kaynak cihazdaki birim kapsayıcıları sahipliği değiştirir ve hedef aygıta aktarılır. Birim kapsayıcıları sahipliği değiştirdiğinizde, bu kaynak aygıttan silinir. Silme işlemi tamamlandıktan sonra hedef aygıt sonra geri çalışılabilir.

Genellikle bir DR en son yedeklemenin ile hedef aygıta verileri geri yüklemek için kullanılır. Ancak, aynı birimi için birden çok yedekleme ilkesi varsa, sonra en büyük birimlerin sayısı, yedekleme ilkesiyle çekilen ve bu ilkeyi en son yedeklemeden hedef aygıttaki verileri geri yüklemek için kullanılır.

Örneğin iki yedekleme ilkeleri (varsayılan ve bir özel) varsa, *defaultPol*, *customPol* aşağıdaki Ayrıntılar ile:

* *defaultPol* : tek bir birim *vol1*, günlük 10: 30'pm başlangıç çalıştırır.
* *customPol* : dört birim *vol1*, *vol2*, *vol3*, *vol4*, günlük 10: 00'dan başlayarak çalıştırır.

Bu durumda, *customPol* daha fazla birim varsa ve biz kilitlenme tutarlılığı için öncelik olarak kullanılır. Bu ilke en son yedekleme verileri geri yüklemek için kullanılır.

## <a name="considerations-for-device-failover"></a>Cihaz yük devretme için ilgili önemli noktalar
Bir olağanüstü durumda StorSimple Cihazınızı vermesine tercih edebilirsiniz:

* Bir fiziksel cihaz 
* Kendisi için
* Sanal cihaza

Tüm aygıt yük devretme için aşağıdakileri göz önünde bulundurun:

* DR için Önkoşullar birim kapsayıcıları tüm birimlerin çevrimdışı ve birim kapsayıcıları ilişkili bir sahip olan bulut anlık görüntüsü. 
* Seçili birim kapsayıcıları yerleştirmek için yeterli alanı olan aygıtları DR için kullanılabilir hedef aygıtlardır. 
* Hizmetinize bağlı, ancak yeterli alan ölçütleri karşılamayan cihazlar hedef cihazlar kullanılabilir olmaz.
* Cihaz buluttan veri erişimi ve yerel olarak depolamak ihtiyaç duyacağınız bir DR sınırlı bir süre için veri erişim performansını önemli ölçüde etkilenebilir.

#### <a name="device-failover-across-software-versions"></a>Cihaz yük devretme yazılım sürümleri arasında
Bir StorSimple Yöneticisi hizmeti bir dağıtımda birden çok aygıt, fiziksel ve sanal tüm çalışan farklı yazılım sürümleri olabilir. Yazılımın sürümüne bağlı olarak, cihazları toplu türlerinde de farklı olabilir. Güncelleştirme 2 veya daha yüksek yerel olarak sabitlenmiş ve katmanlı birimlerin örneği için bir aygıt çalışan (ile arşivleme bir alt kümesi olan katmanlı). Bir güncelleştirme öncesi 2 aygıt diğer yandan katmanlı ve arşivleme birimler. 

Aşağıdaki tabloda, farklı yazılım sürümü ve birim türleri davranışını DR sırasında çalışan başka bir cihaza yük devredebildiğini varsa belirlemek için kullanın.

| Gelen yük devri | Fiziksel cihaz için izin verilen | Sanal cihaz için izin verilen |
| --- | --- | --- |
| Güncelleştirme 2 güncelleştirme öncesi 1 (sürüm, 0.1, 0.2, 0.3) |Hayır |Hayır |
| Güncelleştirme 2 güncelleştirme 1 (1, 1.1, 1.2) |Evet <br></br>Kullanarak yerel olarak sabitlenmiş veya birimleri veya iki bir karışımını katmanlı birimlerin her zaman katmanlı gibi yük devredildi. |Evet<br></br>Kullanarak yerel olarak sabitlenmiş birimler varsa, bunlar üzerinde katmanlı olarak başarısız. |
| Güncelleştirme 2 güncelleştirme 2 (sonraki bir sürümünü) |Evet<br></br>Yerel olarak sabitlenmiş veya katmanlı birim veya iki bir karışımını kullanıyorsanız, birimleri her zaman başlangıç birim türü devredildi; Katmanlı ve yerel olarak sabitlenmiş olarak yerel olarak sabitlenmiş katmanlı. |Evet<br></br>Kullanarak yerel olarak sabitlenmiş birimler varsa, bunlar üzerinde katmanlı olarak başarısız. |

#### <a name="partial-failover-across-software-versions"></a>Yazılım sürümleri arasında kısmi yük devretme
Güncelleştirme öncesi 1 güncelleştirme 1 veya sonraki sürümünü çalıştıran bir hedef çalışan bir StorSimple kaynak aygıtı kullanarak kısmi bir yük devretme gerçekleştirmek istiyorsanız, bu yönergeleri izleyin. 

| Kısmi bir yük devretme işlemini | Fiziksel cihaz için izin verilen | Sanal cihaz için izin verilen |
| --- | --- | --- |
| 1 (sürüm, 0.1, 0.2, 0.3) güncelleştirme 1 veya daha sonraki güncelleştirme öncesi |Evet, aşağıda için en iyi yöntem ipucu konusuna bakın. |Evet, aşağıda için en iyi yöntem ipucu konusuna bakın. |

> [!TIP]
> Güncelleştirme 1 ve sonraki sürümlerde bulut meta verileri ve veri biçimi değişikliği oluştu. Bu nedenle, güncelleştirme 1'den kısmi olarak yük devretme güncelleştirme 1 veya sonraki sürümler için önermiyoruz. Kısmi bir yük devretme gerçekleştirin gerekiyorsa, ilk önce Güncelleştirme 1 veya daha sonra hem aygıtları (kaynak ve hedef) uygulayın ve yük devretme kümelemesiyle devam öneririz. 
> 
> 

## <a name="fail-over-to-another-physical-device"></a>Başka bir fiziksel cihaza yük devri
Cihazınızı bir hedef fiziksel cihaza geri yüklemek için aşağıdaki adımları gerçekleştirin.

1. Devretmek istediğiniz birim kapsayıcısı bulut anlık görüntüleri ilişkili olduğunu doğrulayın.
2. Üzerinde **aygıtları** sayfasında, **birim kapsayıcıları** sekmesi.
3. Başka bir cihaza yük devri istediğiniz bir birim kapsayıcısı seçin. Bu kapsayıcı içindeki birimlerin listesini görüntülemek için birim kapsayıcısı'ı tıklatın. Bir birim seçin ve tıklatın **Çevrimdışına Al** birimi çevrimdışı duruma getirmek için. Bu işlemi, birim kapsayıcısındaki tüm birimler için yineleyin.
4. Başka bir cihaza yük devri istediğiniz tüm birim kapsayıcıları için önceki adımı yineleyin.
5. Üzerinde **aygıtları** sayfasında, **yük devretme**.
6. Altında açılır Sihirbazı'nda **üzerinden vermesine Seç birim kapsayıcısı**:
   
   1. Birim kapsayıcıları listesinde devretmek istediğiniz birim kapsayıcıları seçin.
      **Yalnızca birim kapsayıcıları ilişkili bulut anlık görüntüleri ve çevrimdışı birimlerle birlikte görüntülenir.**
   2. Altında **hedef aygıt seçin** seçili kapsayıcılarında birimler için bir hedef cihaz kullanılabilir aygıtları aşağı açılan listeden seçin. Aşağı açılan listede yalnızca kullanılabilir kapasiteye sahip aygıtlar görüntülenir.
   3. Son olarak, tüm yük devretme ayarları altında gözden **yük devretme onaylayın**. Onay simgesine ![onay simgesi](./media/storsimple-device-failover-disaster-recovery/IC740895.png).
7. Bir yük devretme iş oluşturulur aracılığıyla izlenen **işleri** sayfası. Ardından, devredilen birim kapsayıcısı yerel birim varsa, kapsayıcısındaki (için katmanlı birimleri) yerel her birim için ayrı ayrı geri yükleme işi görürsünüz. Bu geri yükleme işlerinin tamamlanmasını oldukça biraz zaman alabilir. Yük devretme işine önceki tamamlanabilir olasıdır. Yalnızca geri yükleme işi tamamlandıktan sonra bu birimler yerel GARANTİLERİN sahip gerektiğini unutmayın. Yük devretme işlemi tamamlandıktan sonra Git **aygıtları** sayfası.                                            
   
   1. Yük devretme işlemi için hedef cihaz olarak kullanılan cihazı seçin.
   2. Git **birim kapsayıcıları** sayfası. Eski aygıt birimlerden yanı sıra tüm birim kapsayıcıları, listelenmelidir.

## <a name="failover-using-a-single-device"></a>Tek bir cihazı kullanarak yük devretme
Yalnızca tek bir aygıtta varsa ve bir yük devretme gerçekleştirmek gereken aşağıdaki adımları gerçekleştirin.

1. Tüm birimlerin bulut anlık görüntülerini kullanarak Cihazınızı alın.
2. Cihazınızı fabrika ayarlarına döndürebilir. Ayrıntılı yönergeleri izleyin [StorSimple cihazı fabrika varsayılan ayarlarına sıfırlamaya](storsimple-manage-device-controller.md#reset-the-device-to-factory-default-settings).
3. Cihazınızı yapılandırmak ve StorSimple Yöneticisi hizmetiniz ile yeniden kaydedin.
4. Üzerinde **aygıtları** eski aygıt sayfasında göster olarak **çevrimdışı**. Yeni kaydettiğiniz aygıt olarak göstermelidir **çevrimiçi**.
5. Yeni cihaz için cihaz en düşük yapılandırmayı ilk tamamlayın. 
   
   > [!IMPORTANT]
   > **En düşük yapılandırmayı ilk tamamlanmazsa, DR geçerli bir hata nedeniyle başarısız olur. Bu davranış, bir sonraki sürümde düzeltilecektir.**
   > 
   > 
6. Eski aygıt (çevrimdışı durumu) seçin ve tıklatın **yük devretme**. Sunulan sihirbazında, bu cihaz başarısız ve hedef aygıt yeni kayıtlı cihazı olarak belirtin. Ayrıntılı yönergeler için bkz [başka bir fiziksel cihaza yük devri](#fail-over-to-another-physical-device).
7. Cihaz geri yükleme işi gelen izleyebilirsiniz oluşturulacak **işleri** sayfası.
8. İş başarıyla tamamlandıktan sonra yeni cihaz erişmek ve gidin **birim kapsayıcıları** sayfası. Eski aygıt tüm birim kapsayıcıları şimdi yeni cihaz geçirilmesi.

## <a name="fail-over-to-a-storsimple-virtual-device"></a>Bir StorSimple sanal cihazı yük devri
Bir StorSimple sanal cihazı oluşturulur ve bu yordamı çalıştırılmadan önce yapılandırılmış olması gerekir. Güncelleştirme 2 çalıştıran bir 8020 sanal cihazı 64 TB sahiptir ve Premium depolama kullanan DR için kullanmayı düşünün. 

Hedef bir StorSimple sanal cihaz için cihaz geri yüklemek için aşağıdaki adımları gerçekleştirin.

1. Devretmek istediğiniz birim kapsayıcısı bulut anlık görüntüleri ilişkili olduğunu doğrulayın.
2. Üzerinde **aygıtları** sayfasında, **birim kapsayıcıları** sekmesi.
3. Başka bir cihaza yük devri istediğiniz bir birim kapsayıcısı seçin. Bu kapsayıcı içindeki birimlerin listesini görüntülemek için birim kapsayıcısı'ı tıklatın. Bir birim seçin ve tıklatın **Çevrimdışına Al** birimi çevrimdışı duruma getirmek için. Bu işlemi, birim kapsayıcısındaki tüm birimler için yineleyin.
4. Başka bir cihaza yük devri istediğiniz tüm birim kapsayıcıları için önceki adımı yineleyin.
5. Üzerinde **aygıtları** sayfasında, **yük devretme**.
6. Altında açılır Sihirbazı'nda **Seç birim kapsayıcısı yük**, aşağıdaki adımları tamamlayın:
   
    a. Birim kapsayıcıları listesinde devretmek istediğiniz birim kapsayıcıları seçin.
   
    **Yalnızca birim kapsayıcıları ilişkili bulut anlık görüntüleri ve çevrimdışı birimlerle birlikte görüntülenir.**
   
    b. Altında **bir hedef cihaz birimleri için seçilen kapsayıcılarında seçin**, StorSimple sanal cihazı kullanılabilir aygıtları aşağı açılan listeden seçin. **Yalnızca yeterli kapasiteye sahip cihazları aşağı açılan listede görüntülenir.**  
7. Son olarak, tüm yük devretme ayarları altında gözden **yük devretme onaylayın**. Onay simgesine ![onay simgesi](./media/storsimple-device-failover-disaster-recovery/IC740895.png).
8. Yük devretme işlemi tamamlandıktan sonra Git **aygıtları** sayfası.
   
    a. Yük devretme işlemi için hedef cihaz olarak kullanılan StorSimple sanal cihazı seçin.
   
    b. Git **birim kapsayıcıları** sayfası. Eski aygıt birimlerden yanı sıra tüm birim kapsayıcıları, şimdi listelenmelidir.

![Video var](./media/storsimple-device-failover-disaster-recovery/Video_icon.png) **Video var**

Nasıl, başarısız sanal cihazı bulutta fiziksel cihazı üzerinden geri yükleyebilirsiniz gösteren bir video izlemek için tıklatın [burada](https://azure.microsoft.com/documentation/videos/storsimple-and-disaster-recovery/).

## <a name="failback"></a>Yeniden çalışma
Güncelleştirme 3 ve sonraki sürümler için StorSimple geri dönme de destekler. Yük devretme işlemi tamamlandıktan sonra aşağıdaki eylemler gerçekleşir:

* Yük devredildi birim kapsayıcıları kaynak aygıttan temizlenir.
* Birim kapsayıcısı (devredilir) başına arka plan işi kaynak aygıtta başlatılır. İş işlemi devam ederken geri dönmesi çalışırsanız, bunu belirten bir bildirim alırsınız. Yeniden çalışma başlatmak için iş tamamlanana kadar beklemeniz gerekir. 
  
    Birim kapsayıcıları silme işlemini tamamlamak için geçen süre, veri miktarı, işlemi için verileri, yedekleme sayısı ve kullanılabilir ağ bant genişliği geçerlilik süresi gibi çeşitli etkenlere bağlıdır. Test yük devretmeleri/mantığın planlıyorsanız, daha az veri (GB) ile birim kapsayıcıları test etmenizi öneririz. Çoğu durumda, 24 saat yük devretme işlemi tamamlandıktan sonra yeniden başlatabilirsiniz. 

## <a name="frequently-asked-questions"></a>Sık sorulan sorular
Q. **DR başarısız veya kısmi başarılı olup olmadığını ne olur?**

A. DR başarısız olursa, yeniden deneyin öneririz. İkinci kez yaklaşık, DR ne bilir tüm yapıldı ve ne zaman işlemi durduruldu ilk kez. DR işlemi ve sonraki sürümleri, bu noktadan başlar. 

Q. **Cihaz yük devretme işlemi devam ederken bir aygıtı silme?**

A. Bir kurtarma işlemi devam ederken, bir cihaz silemezsiniz. DR tamamlandıktan sonra yalnızca cihazınız silebilirsiniz.

Q.    **Böylece kaynak cihazdaki yerel veriler silinir çöp toplama kaynak aygıtta başlattığınızda?**

A. Yalnızca aygıt tamamen temizlenir sonra çöp toplama kaynak aygıtta etkinleştirilecek. Temizleme, birimler, yedek nesnesini (verileri değil), birim kapsayıcıları ve ilkeleri gibi kaynak aygıttan devredilir nesneleri temizleniyor içerir.

Q. **Birim kapsayıcıları kaynak aygıt ile ilişkili silme işi başarısız olursa ne olur?**

A.  Silme işlemi başarısız olursa, el ile birim kapsayıcıları silinmesini tetiklemek gerekir. İçinde **aygıtları** sayfasında, kaynak Cihazınızı seçin ve tıklayın **birim kapsayıcıları**. Sayfanın alt ve üzerinden başarısız birim kapsayıcıları seçin, **silmek**. Tüm sildikten sonra başarısız kaynak cihazdaki birim kapsayıcıları yeniden başlatabilirsiniz.

## <a name="business-continuity-disaster-recovery-bcdr"></a>İş sürekliliği olağanüstü durum kurtarma (BCDR)
Bir iş sürekliliği olağanüstü durum kurtarma (BCDR) senaryosu, tüm Azure veri merkezi çalışmıyor oluşur. Bu, StorSimple Yöneticisi hizmetiniz ve ilişkili StorSimple cihazları etkileyebilir.

Yalnızca bir olağanüstü durum oluşmadan önce kaydedilen StorSimple cihazlar varsa, bu StorSimple cihazlar Fabrika sıfırlamasına uygulanabilecek gerekebilir. Olağanüstü durum sonra StorSimple cihazı çevrimdışı olarak gösterilir. StorSimple cihazı portaldan silinmelidir ve yeni bir kayıt tarafından izlenen bir Fabrika sıfırlaması yapılmalıdır.

## <a name="next-steps"></a>Sonraki adımlar
* Bir yük devretme gerçekleştirdikten sonra gerekebilir [StorSimple Cihazınızı silmek veya devre dışı bırakma](storsimple-deactivate-and-delete-device.md).
* StorSimple Yöneticisi hizmetini kullanma hakkında daha fazla bilgi için Git [StorSimple Cihazınızı yönetmek için StorSimple Yöneticisi hizmetini kullanma](storsimple-manager-service-administration.md).

