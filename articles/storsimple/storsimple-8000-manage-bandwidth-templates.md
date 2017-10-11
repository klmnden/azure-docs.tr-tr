---
title: "StorSimple 8000 serisi için bant genişliği şablonları yönetme | Microsoft Docs"
description: "Bant genişliği kullanımını denetlemenize izin StorSimple bant genişliği şablonları yönetmek açıklar."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/29/2017
ms.author: alkohli
ms.openlocfilehash: 50d0a920bef097013feddc828d2c37133b9057b0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-storsimple-device-manager-service-to-manage-storsimple-bandwidth-templates"></a>StorSimple bant genişliği şablonları yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanma

## <a name="overview"></a>Genel Bakış

Bant genişliği şablonları StorSimple cihaz verileri bulut katmanı için birden çok gün saat zamanlama arasında ağ bant genişliği kullanımını yapılandırmanıza olanak tanır.

Bant genişliği zamanlamaları azaltma ile şunları yapabilirsiniz:

* İş yükü ağ kullanımları bağlı olarak özelleştirilmiş bant genişliği zamanlamaları belirtin.
* Yönetim merkezileştirmek ve zamanlamaları birçok cihaz arasında kolay ve sorunsuz bir şekilde yeniden kullanabilirsiniz.

> [!NOTE]
> Bu özellik yalnızca StorSimple fiziksel cihazlar (modeller 8100 ve 8600) için ve StorSimple bulut cihazları (modeller 8010 hem de 8020) için kullanılabilir.


## <a name="the-bandwidth-templates-blade"></a>Bant genişliği şablonları dikey penceresi

**Bant genişliği şablonları** dikey penceresinde tüm bant genişliği şablonları hizmetiniz için bir tablo biçiminde sahip ve aşağıdaki bilgileri içerir:

* **Ad** – oluşturulduğunda atanan bant genişliği şablonu için benzersiz bir ad.
* **Zamanlama** – verilen bant genişliği şablonundaki zamanlamaları sayısı.
* **Tarafından kullanılan** – bant genişliği şablonları kullanarak birimlerin sayısı.

Bant genişliği şablonlarını yapılandırmanıza yardımcı olacak ek bilgiler de bulabilirsiniz:

* [Bant genişliği şablonları hakkında sorular ve yanıtlar](#questions-and-answers-about-bandwidth-templates)
* [Bant genişliği şablonları için en iyi yöntemler](#best-practices-for-bandwidth-templates)

## <a name="add-a-bandwidth-template"></a>Bant genişliği şablonu ekleyin

Yeni bir bant genişliği şablonu oluşturmak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-add-a-bandwidth-template"></a>Bant genişliği şablonu eklemek için

1. StorSimple cihaz Yöneticisi hizmetinize gidin, tıklatın **bant genişliği şablonları** ve ardından **+ Ekle bant genişliği şablonu**.

    ![Bant genişliği şablonu ekleyin + tıklayın](./media/storsimple-8000-manage-bandwidth-templates/addbwtemp1.png)

2. İçinde **bant genişliği şablonu Ekle** dikey penceresinde, aşağıdaki adımları uygulayın:
   
    1. Bant genişliği şablonu için benzersiz bir ad belirtin.
    2. Bir bant genişliği zamanlama tanımlayın. Bir zamanlama oluşturmak için:
   
        1. Aşağı açılan listeden seçin **gün** haftası zamanlama için yapılandırılır. Birden fazla gün seçebilirsiniz.        
        
        2. Girin bir **başlangıç saati** içinde _ss: dd_ biçimi. Bu zamanlamanın başlayacağı durumunda olur.

        3. Girin bir **bitiş saati** içinde _ss: dd_ biçimi. Bu zamanlamayı durdurur durumunda olur.
      
           > [!NOTE]
           > Çakışan zamanlamaları izin verilmiyor. Başlangıç ve bitiş zamanlarını çakışan bir zamanlamada neden olur, bunu belirten bir hata iletisi görürsünüz.

        4. Belirtin **bant genişliği Hızı**. Megabit / saniye (Mbps) StorSimple Cihazınızı (karşıya ve karşıdan yüklemeleri) bulut ilgili işlemler tarafından kullanılan bant genişliği budur. 1 ve bu alan için 1000 arasında bir sayı girin.

            ![Bant genişliği zamanlamayı tanımlayın](./media/storsimple-8000-manage-bandwidth-templates/addbwtemp2.png)
         
            Tümü tamamlanıncaya kadar şablonunuz için birden çok zamanlamaları tanımlamak için yukarıdaki adımları yineleyin.

        5. Tıklatın **Ekle** bant genişliği şablonu oluşturmaya başlamak için. Oluşturulan şablon bant genişliği şablonları listesine eklenir.
      

## <a name="edit-a-bandwidth-template"></a>Bant genişliği Şablonu Düzenle

Bant genişliği şablonu düzenlemek için aşağıdaki adımları gerçekleştirin.

### <a name="to-edit-a-bandwidth-template"></a>Bant genişliği şablonu düzenlemek için

1. ' I tıklatın ve StorSimple cihaz yöneticinize hizmeti Git **bant genişliği şablonları**.
2. Bant genişliği şablonları listesinde silmek istediğiniz şablonu seçin. Sağ tıklayın ve bağlam menüsünden seçin **silmek**.
3. Onayınız istendiğinde tıklatın **Tamam**. Bu, bant genişliği şablonu silmeniz gerekir. 
4. Silme işlemini yansıtmak için bant genişliği şablonları güncelleştirmeleri listesi.

> [!NOTE]
> Düzenlenen zamanlama değiştirdiğiniz bant genişliği şablonu mevcut bir zamanlamayı ile çakışırsa, değişiklikler kaydedilemiyor.

## <a name="delete-a-bandwidth-template"></a>Bant genişliği Şablonu Sil

Bant genişliği şablonu silmek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-delete-a-bandwidth-template"></a>Bant genişliği şablonu silmek için

1. ' I tıklatın ve StorSimple cihaz yöneticinize hizmeti Git **bant genişliği şablonları**.
2. Bant genişliği şablonları listesinde silmek istediğiniz şablonu seçin. Sağ tıklatın ve bağlam menüsünden Sil'i seçin.
3. Onayınız istendiğinde tıklatın **Tamam**. Bu, bant genişliği şablonu silmeniz gerekir.
4. Silme işlemini yansıtmak için bant genişliği şablonları güncelleştirmeleri listesi.

Şablon tüm birimlerin tarafından kullanılıyorsa, silmeden verilmez. Şablon kullanımda olduğunu belirten bir hata iletisi görürsünüz. Şablona yapılan tüm başvuruları kaldırılması gerektiğini bildiren bir hata iletisi iletişim kutusu görünür.

Şablona yapılan tüm başvuruları erişerek silebilirsiniz **birim kapsayıcıları** sayfa ve böylece başka bir şablon kullanın veya bir özel veya sınırsız bant genişliği ayarı kullanın, bu şablonu kullanan birim kapsayıcıları değiştirme. Tüm başvuruları kaldırıldıktan sonra şablonu silebilirsiniz.

## <a name="use-a-default-bandwidth-template"></a>Varsayılan bant genişliği şablonu kullanın

Varsayılan bant genişliği şablonu sağlanır ve birim kapsayıcıları tarafından bulut erişirken bant genişliği denetimleri zorlamak için varsayılan olarak kullanılır. Varsayılan şablonu, aynı zamanda kullanıcılar kendi şablonlarını oluşturmak için hazır bir başvuru olarak görev yapar. Bu varsayılan şablon ayrıntılarını şunlardır:

* **Ad** – sınırsız gece ve hafta sonları
* **Zamanlama** – Pazartesi'den tek bir zamanlama Cuma 8: 00 ve 17: 00 saatleri aygıt saat arasındaki 1 MB/sn bant genişliği oranını uygular. Bant genişliği için sınırsız hafta geri kalanı için ayarlanır.

Varsayılan şablonu düzenlenebilir. Bu şablon (düzenlenen sürümleri de dahil olmak üzere) kullanımını izlenir.

## <a name="create-an-all-day-bandwidth-template-that-starts-at-a-specified-time"></a>Belirli bir zamanda başlatır süren bir bant genişliği şablonu oluştur

Belirli bir zamanda başlatır ve tüm gün çalışan bir zamanlama oluşturmak için bu yordamı izleyin. Örnekte, zamanlama içinde sabah 09: 00'dan başlar ve 09: 00 kadar sonraki sabah çalışır. Belirli bir zamanlama için başlangıç ve bitiş zamanları her ikisi de aynı 24 saatlik zaman çizelgesine göre bulunmalıdır ve birden fazla gün yayılamaz dikkate almak önemlidir. Birden çok gün yayılan bant genişliği şablonları ayarlamak gerekiyorsa, birden çok zamanlama (örnekte gösterildiği gibi) kullanmanız gerekir.

#### <a name="to-create-an-all-day-bandwidth-template"></a>Günlük bir bant genişliği şablonu oluşturmak için

1. Sabah, 09: 00'dan başlar ve kadar gece yarısı çalışan bir zamanlama oluşturun.
2. Başka bir zamanlama ekleyin. 09: 00 sabah içinde kadar gece çalıştırmak için ikinci zamanlamayı yapılandırın.
3. Bant genişliği şablonu kaydedin.

Bileşik zamanlama sonra bir zamanda başlatmak ve günlük çalıştırın.

## <a name="questions-and-answers-about-bandwidth-templates"></a>Bant genişliği şablonları hakkında sorular ve yanıtlar

**Q**. Between zamanlama bant genişliği denetimleri için ne olur? (Bir zamanlama sona erdi ve başka bir henüz başlatılmadı.)

**A**. Böyle durumlarda, bant genişliği denetimleri işe. Bu cihaz verileri buluta katmanlama zaman sınırsız bant genişliği olarak kullanabileceğiniz anlamına gelir.

**Q**. Bant genişliği şablonları çevrimdışı cihazında değişiklik yapabilirsiniz?

**A**. Karşılık gelen cihaz çevrimdışı ise birimleri kapsayıcılarında bant genişliği şablonları değiştirmek mümkün olmaz.

**Q**. İlişkili birimler çevrimdışı olduğunda bir birim kapsayıcısı ile ilişkili bir bant genişliği şablonu düzenleyebilirsiniz.

**A**. Birimleri çevrimdışı bir birim kapsayıcısı ile ilişkili bir bant genişliği şablonu değiştirebilirsiniz. Birimlerinin çevrimdışı olduğunda, hiçbir veri CİHAZDAN buluta katmanlı olduğunu unutmayın.

**Q**. Varsayılan bir şablon silebilirsiniz?

**A**. Varsayılan bir şablon silebilirsiniz rağmen Bunu yapmak için iyi bir fikir değil. Düzenlenen sürümleri de dahil olmak üzere varsayılan bir şablon kullanımı izlenir. İzleme verilerini analiz edilir ve süresi boyunca varsayılan şablonu geliştirmek için kullanılır.

**Q**. Bant genişliği şablonlarınızı değiştirilmesi gereken nasıl belirlediğiniz?

**A**. Ağ yavaşlayabilir veya bir gün içinde birden çok kez boğma görmesini başlattığınızda bant genişliği şablonları değiştirmenize gerek işaretlerini biridir. Bu durumda, depolama ve kullanım ağ g/ç performans ve ağ üretilen grafikleri bakarak izleyin.

Ağ verimliliği verilerden günün saati ve ağ sorununu oluştuğu birim kapsayıcıları tanımlayın. Verileri buluta katmanlı olduğunda bu ortaya çıkarsa (g/ç performans bulut cihaz için tüm birim kapsayıcıları için bu bilgileri Al), sonra da, birim kapsayıcıları ile ilişkili bant genişliği şablonları değiştirmeniz gerekir.

Değiştirilen şablonlar kullanımda olan sonra önemli gecikme için tekrar izlemesi gerekir. Bunlar hala yoksa, bant genişliği şablonlarınızı yeniden ziyaret etmeniz gerekir.

**Q**. Bu çakışma zamanlar cihazımı üzerinde birden çok birim kapsayıcıları varsa ne olur, ancak farklı sınırlar her birine uygulanan?

**A**. 3 birim kapsayıcıları aygıtla olduğunu varsayalım. Bu kapsayıcılar ile tamamen ilişkilendirilmiş zamanlamaları çakışıyor. Her Bu kapsayıcıları için kullanılan bant genişliği 5, 10 ve 15 Mbps sırasıyla kısıtlamalardır. G/ç oluşan, tüm bu kapsayıcıların aynı anda en az 3 bant genişliği sınırlarının uygulanabilir: Bu durumda, bu g/ç istekleri giden olarak 5 MB/sn aynı sıraya paylaşın.

## <a name="best-practices-for-bandwidth-templates"></a>Bant genişliği şablonları için en iyi yöntemler

StorSimple cihazınız için bu en iyi uygulamaları izleyin:

* Bant genişliği şablonları değişken ağ verimliliği aygıt tarafından günün farklı zamanlarda azaltmayı etkinleştirmek için Cihazınızı yapılandırın. Yedekleme zamanlamaları ile kullanıldığında bu bant genişliği şablonları, yoğun olmayan saatlerde ek ağ bant genişliği bulut işlemleri için etkili bir şekilde yararlanabilirsiniz.
* Dağıtım ve gerekli kurtarma süresi hedefi (RTO) boyutuna göre belirli bir dağıtım için gereken gerçek bant hesaplayın.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinmek [StorSimple Cihazınızı yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanarak](storsimple-8000-manager-service-administration.md).

