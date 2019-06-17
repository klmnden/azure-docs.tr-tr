---
title: StorSimple 8000 serisi için bant genişliği şablonlarını yönetme | Microsoft Docs
description: Bant genişliği kullanımını denetlemenize izin StorSimple bant genişliği şablonlarını yönetme işlemi açıklanır.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/29/2017
ms.author: alkohli
ms.openlocfilehash: 13a3e57bb27c075fc045e87790dbe13369ed9f8e
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60699486"
---
# <a name="use-the-storsimple-device-manager-service-to-manage-storsimple-bandwidth-templates"></a>StorSimple bant genişliği şablonlarını yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanma

## <a name="overview"></a>Genel Bakış

Bant genişliği şablonları birden çok günün saati zamanlama StorSimple cihazından verileri buluta katmanı arasında ağ bant genişliği kullanımını yapılandırmanıza olanak sağlar.

Bant genişliği azaltma zamanlamaları ile şunları yapabilirsiniz:

* Özelleştirilmiş bant genişliği zamanlamaları iş yükü ağ kullanımları bağlı olarak belirtin.
* Yönetimini merkezden gerçekleştirin ve zamanlamaları birden çok cihazda kolay ve sorunsuz bir şekilde yeniden.

> [!NOTE]
> Bu özellik yalnızca StorSimple fiziksel cihazlar (model 8100 ve 8600) ve StorSimple bulut Gereçleri (model 8010 ve 8020) için kullanılabilir.


## <a name="the-bandwidth-templates-blade"></a>Bant genişliği şablonları dikey penceresi

**Bant genişliği şablonları** dikey penceresinde tüm bant genişliği şablonları hizmetiniz için bir tablo biçiminde olan ve aşağıdaki bilgileri içerir:

* **Adı** – bant genişliği şablonu oluşturulduğunda atanan benzersiz bir ad.
* **Zamanlama** – belirli bir bant genişliği şablonu içinde yer alan zamanlamaların sayısı.
* **Tarafından kullanılan** – bant genişliği şablonları kullanarak birim sayısı.

Bant genişliği şablonlarını yapılandırmanıza yardımcı olacak ek bilgiler de bulabilirsiniz:

* [Bant genişliği şablonları hakkında sorular ve cevaplar](#questions-and-answers-about-bandwidth-templates)
* [Bant genişliği şablonları için en iyi yöntemler](#best-practices-for-bandwidth-templates)

## <a name="add-a-bandwidth-template"></a>Bir bant genişliği şablonu ekleme

Yeni bir bant genişliği şablonu oluşturmak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-add-a-bandwidth-template"></a>Bant genişliği şablonu eklemek için

1. StorSimple cihaz Yöneticisi hizmetinize gidin, tıklayın **bant genişliği şablonları** ve ardından **+ Ekle bant genişliği şablonu**.

    ![Bant genişliği şablonu Ekle + tıklayın](./media/storsimple-8000-manage-bandwidth-templates/addbwtemp1.png)

2. İçinde **Ekle bant genişliği şablonu** dikey penceresinde aşağıdaki adımları uygulayın:
   
    1. Bant genişliği şablonu için benzersiz bir ad belirtin.
    2. Bir bant genişliği zamanlamayı tanımlar. Bir zamanlama oluşturmak için:
   
        1. Aşağı açılan listeden seçin **gün** haftalık zamanlama için yapılandırılmış. Birden çok gün seçebilirsiniz.        
        
        2. Girin bir **başlattığınızda** içinde _ss: dd_ biçimi. Zamanlamanın başlayacağı andır.

        3. Girin bir **bitiş zamanı** içinde _ss: dd_ biçimi. Zamanlama durdurur andır.
      
           > [!NOTE]
           > Çakışan zamanlamaları izin verilmez. Başlangıç ve bitiş zamanlarını çakışan bir zamanlamayı gösterecekse belirten bir hata iletisi görürsünüz.

        4. Belirtin **bant genişliği Hızı**. StorSimple Cihazınızı (karşıya yükleme ve indirmeleri) bulut ilgili işlemler tarafından kullanılan saniye (Mbps) başına megabit cinsinden bant genişliği budur. Bu alana 1 ile 1000 arasında bir sayı girin.

            ![Bant genişliği zamanlamayı tanımlayın](./media/storsimple-8000-manage-bandwidth-templates/addbwtemp2.png)
         
            Bunlar tamamlanana kadar şablonunuz için birden çok zamanlama tanımlamak için yukarıdaki adımları yineleyin.

        5. Tıklayın **Ekle** bant genişliği şablonu oluşturmaya başlamak için. Oluşturulan şablonun, bant genişliği şablonları listesine eklenir.
      

## <a name="edit-a-bandwidth-template"></a>Bant genişliği Şablonu Düzenle

Bir bant genişliği şablonunu düzenlemek için aşağıdaki adımları gerçekleştirin.

### <a name="to-edit-a-bandwidth-template"></a>Bir bant genişliği şablonunu düzenlemek için

1. ' A tıklayın ve StorSimple cihaz Yöneticisi hizmetinize gidin **bant genişliği şablonları**.
2. Bant genişliği şablonları listesinde, silmek istediğiniz şablonu seçin. Sağ tıklayın ve bağlam menüsünden **Sil**.
3. Onayınız istendiğinde tıklayın **Tamam**. Bu, bant genişliği şablonu silmeniz gerekir. 
4. Bant genişliği şablonları güncelleştirmeleri silinmesini yansıtacak şekilde listesi.

> [!NOTE]
> Düzenlenen zamanlama değiştirmekte olduğunuz bant genişliği şablonu mevcut bir zamanlamayı ile çakışırsa, değişikliklerinizi kaydedemezsiniz.

## <a name="delete-a-bandwidth-template"></a>Bir bant genişliği şablonunu silme

Bir bant genişliği şablonunu silme için aşağıdaki adımları gerçekleştirin.

#### <a name="to-delete-a-bandwidth-template"></a>Bir bant genişliği şablonu silinemiyor

1. ' A tıklayın ve StorSimple cihaz Yöneticisi hizmetinize gidin **bant genişliği şablonları**.
2. Bant genişliği şablonları listesinde, silmek istediğiniz şablonu seçin. Sağ tıklayın ve bağlam menüsünden Sil'i seçin.
3. Onayınız istendiğinde tıklayın **Tamam**. Bu, bant genişliği şablonu silmeniz gerekir.
4. Bant genişliği şablonları güncelleştirmeleri silinmesini yansıtacak şekilde listesi.

Şablon tüm birimleri tarafından kullanılıyor, silmek verilmez. Şablonu kullanımda olduğunu belirten bir hata iletisi görürsünüz. Şablon yapılan tüm başvurular kaldırılması gerektiğini bildiren bir hata iletisi iletişim kutusu görünür.

Şablon yapılan tüm başvurular erişerek silebilirsiniz **birim kapsayıcıları** sayfası ve böylece başka bir şablon kullanın veya özel veya sınırsız bant genişliği ayarı, bu şablonu kullanın. birim kapsayıcılarını değiştirme. Tüm başvuruları kaldırıldıktan sonra şablonu silebilirsiniz.

## <a name="use-a-default-bandwidth-template"></a>Varsayılan bant genişliği şablonu kullanın

Varsayılan bant genişliği şablonu sağlanır ve birim kapsayıcıları tarafından bulut erişirken bant genişliği denetimlerini zorlamak için varsayılan olarak kullanılır. Varsayılan şablonu, ayrıca kendi şablonlarını oluşturan kullanıcılar için hazır bir başvuru olarak görev yapar. Bu varsayılan şablon ayrıntılarını şunlardır:

* **Adı** – sınırsız gece ve hafta sonları
* **Zamanlama** – 8: 00 ve 17: 00 cihaz saat arasında 1 MB/sn bant genişliği ücreti uygulanır cumaya Pazartesi'den tek bir zamanlamanın. Bant genişliği sınırsız için haftanın geri kalanı için ayarlanır.

Varsayılan şablonu düzenleyebilirsiniz. Bu şablonu (düzenlenen sürümleri dahil) kullanımı izlenir.

## <a name="create-an-all-day-bandwidth-template-that-starts-at-a-specified-time"></a>Belirli bir zamanda başlatan bir günlük bant genişliği şablonu oluşturma

Belirli bir zamanda başlatır ve tüm gün çalışan bir zamanlama oluşturmak için bu yordamı izleyin. Örnekte, zamanlama sabah, 09: 00'dan başlar ve sonraki sabah 09: 00 kadar çalışır. Belirli bir zamanlama için başlangıç ve bitiş zamanları her ikisi de aynı 24 saatlik zaman çizelgesinde bulunmalıdır ve birden çok gün yayılamaz unutulmaması önemlidir. Birden çok günü kapsayan bant genişliği şablonları ayarlamak gerekiyorsa, birden çok zamanlama (örnekte gösterildiği gibi) kullanmak gerekir.

#### <a name="to-create-an-all-day-bandwidth-template"></a>Bir günlük bant genişliği şablonu oluşturmak için

1. Sabah, 09: 00'dan başlar ve gece yarısına kadar çalışan bir zamanlama oluşturun.
2. Başka bir zamanlama ekleyin. Gece yarısı sabah, 09: 00 kadar çalıştırmak için ikinci zamanlamayı yapılandırın.
3. Bant genişliği şablonu kaydedin.

Bileşik zamanlama, ardından bir zamanda başlatmak ve günlük çalıştırın.

## <a name="questions-and-answers-about-bandwidth-templates"></a>Bant genişliği şablonları hakkında sorular ve cevaplar

**Q**. Bant genişliği denetimleri, arasındaki çizelgeleri olduğunda ne olur? (Bir zamanlama sona erdiyse ve başka bir henüz başlatılmadı.)

**BİR**. Böyle durumlarda, hiçbir bant genişliği denetimleri işe. Bu, cihaz verileri buluta katmanlama, sınırsız bant genişliği olarak kullanabileceğiniz anlamına gelir.

**Q**. Bant genişliği şablonları çevrimdışı bir cihaz üzerinde değişiklik yapabilir?

**BİR**. Karşılık gelen cihaz çevrimdışıysa birim kapsayıcılarına bant genişliği şablonları değiştirmek mümkün olmayacaktır.

**Q**. İlişkili birimleri çevrimdışı olduğunda, bir birim kapsayıcısı ile ilişkili bir bant genişliği şablonu düzenleyebilir miyim?

**BİR**. Bir bant genişliği şablonu, birimlerin çevrimdışı olduğundan birim kapsayıcısı ile ilişkili değiştirebilirsiniz. Birimleri çevrimdışı olduğunda, veri CİHAZDAN buluta katmanlanmış olmaz olduğunu unutmayın.

**Q**. Varsayılan bir şablon silebilir miyim?

**BİR**. Varsayılan bir şablon silebilmeniz için olsa da, bunu yapmak için iyi bir fikir değil. Düzenlenen sürümleri dahil olmak üzere varsayılan şablon kullanımı izlenir. İzleme verilerini analiz edilir ve süre boyunca, varsayılan şablonu geliştirmek için kullanılır.

**Q**. Nasıl, bant genişliği şablonları değiştirilmesi gerektiğini belirlemek?

**BİR**. Ağ yavaşlamasına veya bir gün içinde birden çok kez boğma görmeye başlattığınızda bant genişliği şablonları değiştirme gerektiğini işaretleri biridir. Bu durumda, depolama ve kullanım ağ g/ç performansını ve ağ aktarım hızı grafiklerine baktığımızda tarafından izleyin.

Ağ aktarım hızı verilerden günün saati ve ağ sorununu oluştuğu birim kapsayıcıları belirleyin. Buluta katmanlanmış verileri kullandığınızda ortaya çıkar, (Bu bilgiyi tüm birim kapsayıcıları CİHAZDAN buluta GÇ performansı alın), sonra da, birim kapsayıcıları ile ilişkili bant genişliği şablonları değiştirmeniz gerekecektir.

Değiştirilen şablonlar kullanılıyor sonra yeniden önemli bir gecikme için Ağ İzleyicisi'ni gerekir. Bunlar yine de varsa, bant genişliği şablonları yeniden ziyaret etmeniz gerekir.

**Q**. Bu çakışma zamanlar birden çok birim kapsayıcılarının cihazım varsa ne olur, ancak her farklı sınırlar geçerlidir?

**BİR**. 3 birim kapsayıcıları ile bir cihaz olduğunu varsayalım. Bu kapsayıcılar ile tamamen ilişkili zamanlamaları çakışıyor. Her biri bu kapsayıcıları için kullanılan bant genişliği 15 MB/sn 5 ve 10 sırasıyla limitlerdir. G/ç oluşan, tüm bu kapsayıcıların aynı anda en az 3 bant genişliği sınırlarını uygulanabilir: Bu durumda, bu g/ç istekleri giden olarak 5 MB/sn aynı kuyruğa paylaşın.

## <a name="best-practices-for-bandwidth-templates"></a>Bant genişliği şablonları için en iyi yöntemler

Bu StorSimple cihazınız için en iyi uygulamaları izleyin:

* Değişken ağ verimliliği ve cihaz tarafından günün farklı zamanlarında azaltmayı etkinleştirmek için cihaz bant genişliği şablonlarını yapılandırın. Yedekleme zamanlamaları ile kullanıldığında bu bant genişliği şablonları, yoğun olmayan saatlerde ek ağ bant genişliği bulut işlemleri için etkili bir şekilde yararlanabilirsiniz.
* Dağıtımı ve gereken kurtarma süresi hedefi (RTO) boyutuna bağlı olarak belirli bir dağıtım için gereken gerçek bant genişliği hesaplayın.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinin [StorSimple Cihazınızı yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanarak](storsimple-8000-manager-service-administration.md).

