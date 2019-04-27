---
title: Microsoft Azure StorSimple veri Yöneticisi'ne genel bakış | Microsoft Docs
description: StorSimple veri Yöneticisi hizmetine genel bir bakış sağlar
services: storsimple
documentationcenter: NA
author: vidarmsft
manager: syadav
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 05/21/2018
ms.author: vidarmsft
ms.openlocfilehash: c5ffe3ec2ec3cb06297df6be4ba7021f692633bf
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60630703"
---
# <a name="storsimple-data-manager-solution-overview"></a>StorSimple veri Yöneticisi çözümüne genel bakış

## <a name="overview"></a>Genel Bakış

Şirket içi depolama ve bulutta Microsoft Azure StorSimple şirket içi çözüm ve otomatik olarak katmanları veri uzantısı bulut depolama kullanır. Veriler buluttaki en yüksek verimlilik ve maliyetleri düşürmek için yinelenenleri kaldırılmış ve sıkıştırılmış biçimde depolanır. StorSimple biçiminde depolanan veri olarak kullanmak istediğiniz diğer bulut uygulamaları tarafından hemen kullanılabilir değildir.

StorSimple veri Yöneticisi'ni, sorunsuz bir şekilde erişebilir ve StorSimple biçim verileri bulutta sağlar. Bunu yerel bloblar ve Azure Media Services, Azure Hdınsights ve Azure Machine Learning gibi başka hizmetler ile kullanabileceğiniz dosyaları, StorSimple biçim dönüştürme yapar.

Bu makalede, StorSimple veri Yöneticisi çözüme genel bakış sağlar. Ayrıca, bulutta nasıl StorSimple veriler kullanan uygulamalar yazmak için bu hizmet ve diğer Azure Hizmetleri kullanabileceğini açıklar.

## <a name="how-it-works"></a>Nasıl çalışır?

StorSimple veri Yöneticisi hizmeti, StorSimple 8000 serisi bir şirket içi CİHAZDAN buluta StorSimple verileri tanımlar. StorSimple veri bulutta yinelenenleri kaldırılan, StorSimple biçimi sıkıştırılmış. Veri Yöneticisi hizmeti, StorSimple biçim verileri ayıklamak ve Azure BLOB'ları gibi diğer biçimlere dönüştürme API ve Azure dosyaları sağlar. Dönüştürülen bu veriler daha sonra kolayca kullanılır Azure HDInsight ve Azure Media services tarafından. Veri dönüştürme, bu nedenle StorSimple 8000 serisi şirket içi cihaz StorSimple dönüştürülen verilerden işlem yapılacak bu hizmetleri sağlar. Bu akışı aşağıdaki diyagramda gösterilmiştir.

![Üst düzey diyagramı](./media/storsimple-data-manager-overview/storsimple-data-manager-overview2.png)


## <a name="data-manager-use-cases"></a>Veri Yöneticisi'ni kullanım örnekleri

StorSimple geldiğinde verileriniz üzerinde çalışan iş akışları için Azure işlevleri, Azure Otomasyonu ve Azure Data Factory ile veri Yöneticisi'ni kullanabilirsiniz. StorSimple ile Azure Media Services, mağaza veya bir makine öğrenimi algoritması bu veriler üzerinde çalıştırmak veya StorSimple depoladığınız verileri analiz etmek için bir Hadoop kümesini getirmek içerik medyanızı işlemek isteyebilirsiniz. Birçok çeşit StorSimple verileriyle birlikte azure'da kullanılabilen hizmetler ile verilerinizin gücünü açığa çıkarabilirsiniz.


## <a name="region-availability"></a>Bölge kullanılabilirliği

StorSimple veri Yöneticisi'ni 7 aşağıdaki bölgelerde kullanılabilir:

 - Güneydoğu Asya
 - Doğu ABD
 - Batı ABD
 - Batı ABD 2
 - Batı Orta ABD
 - Kuzey Avrupa
 - Batı Avrupa

Ancak, StorSimple veri Yöneticisi'ni, verileri aşağıdaki bölgelerde dönüştürmek için kullanılabilir. 

![Verileri için kullanılabilir bölgeleri](./media/storsimple-data-manager-overview/data-manager-job-definition-different-regions-m.png)

Yukarıdaki bölgelerden kaynak dağıtım dönüştürme işleminde getirme çünkü bu küme daha büyük olan bölgeler aşağıda. Bu nedenle, 19 bölgede herhangi birinde verilerinizin bulunduğu sürece, bu hizmeti kullanarak verilerinizi dönüştürebilirsiniz.


## <a name="choosing-a-region"></a>Bir bölge seçme

Olmasını öneririz:
 - Kaynak depolama hesabınıza (StorSimple Cihazınızı ile ilişkili bir) ve (istediğiniz verileri yerel biçiminde) hedef depolama hesabı aynı Azure bölgesinde olmalıdır.
 - StorSimple depolama hesabının bulunduğu bölgede veri yöneticiniz ve iş tanımını oluşturan duruma getirin. Bu mümkün değilse, Yukarı en yakın Azure bölgesine veri Yöneticisi getirin ve ardından StorSimple depolama hesabınız ile aynı bölgede iş tanımı oluşturun. 

    StorSimple depolama hesabınız iş tanımı oluşturmayı destekleyen 26 bölgede değilse, uzun gecikme süreleri ve olası çıkış ücretlerini gördüğünüz gibi StorSimple veri Yöneticisi çalıştırmamanızı öneririz.
    
Microsoft Azure hizmetlerini her zaman tüm bölgelerde kullanılabilir olmasını sağlamak çalışır. Ancak, belirli bir bölgede kısa sürelerle plansız bir hizmet kesintileri ortaya çıkabilir. Böyle durumlarda veri yöneticiniz ve iş tanımını oluşturan bir bölgede kesintiden etkilenmezler Getir ve dönüştürme işlemini çalıştırın. Böyle bir senaryoda ek gecikme sürelerine karşılaşabilirsiniz, ancak bu nadir bölgesel bir kesinti kurtarma stratejinize olabilir.

## <a name="security-considerations"></a>Güvenlikle ilgili dikkat edilmesi gerekenler

StorSimple veri Yöneticisi, StorSimple biçiminden yerel biçimine dönüştürmek için hizmet veri şifreleme anahtarı gerekir. İlk cihaz StorSimple hizmetine kaydeder. hizmet veri şifreleme anahtarı üretilir. Bu anahtar hakkında daha fazla bilgi için Git [StorSimple güvenlik](storsimple-8000-security.md).

Hizmet veri şifreleme anahtarı giriş olarak sağlanan Data Manager'ı oluşturduğunuzda, oluşturulan bir anahtar kasasında depolanır. Kasayla aynı bölgede Azure StorSimple veri Yöneticisi olarak bulunur. Veri Yöneticisi hizmetinize sildiğinizde bu anahtarı silindi.

Bu anahtar, dönüştürmeyi gerçekleştirmek için işlem kaynakları tarafından kullanılır. Bu işlem, kaynakları iş tanımınızın ile aynı Azure bölgesinde bulunur. Bu bölge olabilir veya Data Manager'ı Getir burada bölge ile aynı olmayabilir.

Veri Yöneticisi bölgeniz, iş tanımı bölgeden farklı ise, hangi veri/meta verilerini her bu bölgelerin bulunduğu anlamak önemlidir. Aşağıdaki diyagram, farklı bölgelerdeki veri yöneticiniz ve iş tanımı için olan etkisini gösterir.

![Farklı bölgelerdeki hizmet ve iş tanımı](./media/storsimple-data-manager-overview/data-manager-job-different-regions.png)

## <a name="managing-personal-information"></a>Kişisel bilgilerini yönetme

StorSimple veri Yöneticisi'ni toplamaz veya herhangi bir kişisel bilgi görüntüler. Daha fazla bilgi için, [Güven Merkezi](https://www.microsoft.com/trustcenter)’nde Microsoft Gizlilik ilkesini gözden geçirin.

## <a name="known-limitations"></a>Bilinen Sınırlamalar

Hizmet şu anda aşağıdaki sınırlamalara sahiptir:
- StorSimple veri Yöneticisi şu anda bitlocker şifrelenmiş birimler ile çalışmaz. Hizmet ile şifrelenmiş bir sürücüde çalıştırmayı denerseniz, işi hataları görürsünüz.
- Dönüştürülen verileri bazı meta verilerden (ACL'leri dahil) dosyaları korunmaz.
- Bu hizmet, yalnızca NTFS birimleri ile çalışır.
- Dosya yolu uzunluğu en fazla 256 karakter başka işi başarısız olur olması gerekir.

## <a name="next-steps"></a>Sonraki adımlar

[StorSimple veri Yöneticisi'ni kullanın, verilerinizi dönüştürmek için kullanıcı Arabirimi](storsimple-data-manager-ui.md).
