---
title: "StorSimple bant genişliği şablonlarınızı yönetme | Microsoft Docs"
description: "Bant genişliği kullanımını denetlemenize izin StorSimple bant genişliği şablonları yönetmek açıklar."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 0027b90e-91a5-437d-9bd0-06b05674aa5f
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/16/2016
ms.author: alkohli
ms.openlocfilehash: df3ae8bf775370432b3648459a7c942afe69fb17
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="use-the-storsimple-manager-service-to-manage-storsimple-bandwidth-templates"></a>StorSimple bant genişliği şablonları yönetmek için StorSimple Yöneticisi hizmetini kullanma
## <a name="overview"></a>Genel Bakış
Bant genişliği şablonları StorSimple cihaz verileri bulut katmanı için birden çok gün saat zamanlama arasında ağ bant genişliği kullanımını yapılandırmanıza olanak tanır.

Bant genişliği zamanlamaları azaltma ile şunları yapabilirsiniz:

* İş yükü ağ kullanımları bağlı olarak özelleştirilmiş bant genişliği zamanlamaları belirtin.
* Yönetim merkezileştirmek ve zamanlamaları birçok cihaz arasında kolay ve sorunsuz bir şekilde yeniden kullanabilirsiniz.

> [!NOTE]
> Bu özellik yalnızca StorSimple fiziksel cihazlar için ve sanal cihaz için kullanılabilir.
> 
> 

Tüm bant genişliği şablonları hizmetiniz için bir tablo biçiminde görüntülenir ve aşağıdaki bilgileri içerir:

* **Ad** – oluşturulduğunda atanan bant genişliği şablonu için benzersiz bir ad.
* **Zamanlama** – verilen bant genişliği şablonundaki zamanlamaları sayısı.
* **Tarafından kullanılan** – bant genişliği şablonları kullanarak birimlerin sayısı.

StorSimple Yöneticisi hizmetini kullanma **yapılandırma** bant genişliği şablonları yönetmek için Azure Klasik portalında sayfası.

Bant genişliği şablonlarını yapılandırmanıza yardımcı olacak ek bilgiler de bulabilirsiniz:

* Bant genişliği şablonları hakkında sorular ve yanıtlar
* Bant genişliği şablonları için en iyi yöntemler

## <a name="add-a-bandwidth-template"></a>Bant genişliği şablonu ekleyin
Yeni bir bant genişliği şablonu oluşturmak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-add-a-bandwidth-template"></a>Bant genişliği şablonu eklemek için
1. StorSimple Yöneticisi hizmetine **yapılandırma** sayfasında, **Ekle/Düzenle bant genişliği şablonu**.
2. İçinde **Ekle/Düzenle bant genişliği şablonu** iletişim kutusunda:
   
   1. Gelen **şablonu** aşağı açılan listesinden, **Yeni Oluştur** yeni bant genişliği şablonu eklemek için.
   2. Bant genişliği şablonu için benzersiz bir ad belirtin.
3. Tanımlayan bir **bant genişliği zamanlama**. Bir zamanlama oluşturmak için:
   
   1. Aşağı açılan listeden için yapılandırılmış zamanlamayı haftanın günleri seçin. Birden çok gün listesinde karşılık gelen gün önce bulunan onay kutularını işaretleyerek seçebilirsiniz.
   2. Seçin **tüm gün** zamanlama tüm gün için zorunlu kılındığında seçeneği. Bu seçenek işaretlendiğinde, artık belirleyebileceğiniz bir **başlangıç saati** veya bir **bitiş zamanı**. Zamanlama 12: 00'da 11:59:00 için çalışır.
   3. Aşağı açılan listesinden bir **başlangıç saati**. Bu zamanlamanın başlayacağı durumunda olur.
   4. Aşağı açılan listesinden bir **bitiş zamanı**. Bu zamanlamayı durdurur durumunda olur.
      
      > [!NOTE]
      > Çakışan zamanlamaları izin verilmiyor. Başlangıç ve bitiş zamanlarını çakışan bir zamanlamada neden olur, bunu belirten bir hata iletisi görürsünüz.
      > 
      > 
   5. Belirtin **bant genişliği Hızı**. Megabit / saniye (Mbps) StorSimple Cihazınızı (karşıya ve karşıdan yüklemeleri) bulut ilgili işlemler tarafından kullanılan bant genişliği budur. 1 ve bu alan için 1000 arasında bir sayı girin.
   6. Onay simgesine ![onay simgesi](./media/storsimple-manage-bandwidth-templates/HCS_CheckIcon.png). Hizmet üzerinde bant genişliği şablonları listesi, oluşturduğunuz şablon eklenecek **yapılandırma** sayfası.
      
      ![Yeni bant genişliği şablonu oluştur](./media/storsimple-manage-bandwidth-templates/HCS_CreateNewBT1.png)
4. Tıklatın **kaydetmek** sayfa ve ardından ekranın alt kısmındaki **Evet** onaylamanız istendiğinde. Bu, yaptığınız yapılandırma değişikliklerini kaydeder.

## <a name="edit-a-bandwidth-template"></a>Bant genişliği Şablonu Düzenle
Bant genişliği şablonu düzenlemek için aşağıdaki adımları gerçekleştirin.

### <a name="to-edit-a-bandwidth-template"></a>Bant genişliği şablonu düzenlemek için
1. Tıklatın **Ekle/Düzenle bant genişliği şablonu**.
2. İçinde **Ekle/Düzenle bant genişliği şablonu** iletişim kutusunda:
   
   1. Gelen **şablonu** aşağı açılan listesinde, değiştirmek istediğiniz var olan bir bant genişliği şablonu seçin.
   2. Değişikliklerinizi tamamlayın. (Var olan ayarları değiştirebilirsiniz.)
   3. Onay simgesine tıklayarak ![Onay simgesi](./media/storsimple-manage-bandwidth-templates/HCS_CheckIcon.png). Bant genişliği şablonları listesi değiştirilmiş şablonunda hizmet Yapılandır sayfasında görürsünüz.
3. Değişikliklerinizi kaydetmek için tıklatın **kaydetmek** sayfanın sonundaki. Tıklatın **Evet** onaylamanız istendiğinde.

> [!NOTE]
> Düzenlenen zamanlama değiştirdiğiniz bant genişliği şablonu mevcut bir zamanlamayı ile çakışırsa, değişikliklerinizi kaydetmeye izin verilecek değil.
> 
> 

## <a name="delete-a-bandwidth-template"></a>Bant genişliği Şablonu Sil
Bant genişliği şablonu silmek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-delete-a-bandwidth-template"></a>Bant genişliği şablonu silmek için
1. Hizmetiniz için bant genişliği şablonları Tablo listesinde, silmek istediğiniz şablonu seçin. Sil simgesini (**x**) seçili şablon aşırı sağında görünür. Tıklatın **x** şablonunu silmek için simge.
2. Onayınız istenir. Tıklatın **Tamam** devam etmek için.

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

**A**. 3 birim kapsayıcıları aygıtla olduğunu varsayalım. Bu kapsayıcılar ile tamamen ilişkilendirilmiş zamanlamaları çakışıyor. Her Bu kapsayıcıları için kullanılan bant genişliği 5, 10 ve 15 Mbps sırasıyla kısıtlamalardır. G/ç Bu kapsayıcılar tümünde aynı anda yaşanan, en az 3 bant genişliği sınırlarının uygulanabilir: Bu durumda, bu g/ç istekleri giden olarak 5 MB/sn aynı sıraya paylaşın.

## <a name="best-practices-for-bandwidth-templates"></a>Bant genişliği şablonları için en iyi yöntemler
StorSimple cihazınız için bu en iyi uygulamaları izleyin:

* Bant genişliği şablonları değişken ağ verimliliği aygıt tarafından günün farklı zamanlarda azaltmayı etkinleştirmek için Cihazınızı yapılandırın. Yedekleme zamanlamaları ile kullanıldığında bu bant genişliği şablonları, yoğun olmayan saatlerde ek ağ bant genişliği bulut işlemleri için etkili bir şekilde yararlanabilirsiniz.
* Dağıtım ve gerekli kurtarma süresi hedefi (RTO) boyutuna göre belirli bir dağıtım için gereken gerçek bant hesaplayın.

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinmek [StorSimple Cihazınızı yönetmek için StorSimple Yöneticisi hizmetini kullanma](storsimple-manager-service-administration.md).

