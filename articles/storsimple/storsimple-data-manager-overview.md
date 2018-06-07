---
title: Microsoft Azure StorSimple veri Yöneticisi'ne genel bakış | Microsoft Docs
description: StorSimple veri Yöneticisi hizmetini genel bir bakış sağlar
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
ms.openlocfilehash: d57229ad79909aa0334cc623d727b733a1ec73f9
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34652017"
---
# <a name="storsimple-data-manager-solution-overview"></a>StorSimple veri Yöneticisi çözümüne genel bakış

## <a name="overview"></a>Genel Bakış

Microsoft Azure StorSimple bulut depolama şirket içi depolama ve bulut arasında şirket içi çözüm ve otomatik olarak katmanları verilerinin bir uzantısı olarak kullanır. Veriler, yinelenenleri kaldırılmış ve sıkıştırılmış biçimde en yüksek verimlilik ve maliyetleri düşürmek için bulutta depolanır. Veri StorSimple biçiminde depolanmış gibi kullanmak istediğiniz diğer bulut uygulamaları tarafından kolayca tüketilebilir değil.

StorSimple veri Yöneticisi'ni sorunsuz bir şekilde erişmek ve StorSimple biçim verileri bulutta kullanmanıza olanak sağlar. Bunu yerel bloblar ve Azure Media Services, Azure Hdınsights ve Azure Machine Learning gibi başka Hizmetleri ile birlikte kullanabileceğiniz dosyaları StorSimple biçimi dönüştürerek yapar.

Bu makalede, StorSimple Data Manager çözümü genel bir bakış sağlar. Ayrıca, bulutta nasıl StorSimple veri kullanan uygulamaları yazmak için bu hizmet ve diğer Azure hizmetleriyle kullanabileceğinizi açıklar.

## <a name="how-it-works"></a>Nasıl çalışır?

StorSimple Data Manager hizmeti bir StorSimple 8000 serisi şirket içi cihaz buluttan StorSimple verileri tanımlar. StorSimple verileri bulutta yinelenenleri kaldırılan, StorSimple biçimi sıkıştırılmış. Veri Yöneticisi hizmeti StorSimple biçim verileri ayıklar ve Azure BLOB'ların gibi diğer biçimlere dönüştürme için API'ler ve Azure dosyaları sağlar. Bu dönüştürülmüş verileri sonra kullanıma tüketilen Azure Hdınsight ve Azure Media services tarafından. Veri dönüştürme, böylece StorSimple 8000 serisi şirket içi cihaz dönüştürülmüş StorSimple verilerden işlem yapılacak bu hizmetleri sağlar. Bu akış aşağıdaki çizimde gösterilmiştir.

![Üst düzey diyagramı](./media/storsimple-data-manager-overview/storsimple-data-manager-overview2.png)


## <a name="data-manager-use-cases"></a>Veri Yöneticisi kullanım örnekleri

StorSimple geldiğinde verileriniz üzerinde çalışan iş akışlarının sağlamak için Azure işlevleri, Azure Otomasyonu ve Azure Data Factory ile veri Yöneticisi'ni kullanabilirsiniz. Azure Media Services ile StorSimple depolamak veya bu veriler üzerinde bir Machine Learning algoritmasını çalıştırmak veya üzerinde StorSimple depolamak verileri çözümlemek için Hadoop küme Getir içerik medyanızı işlemek isteyebilirsiniz. Değerlendirilmesine Hizmetleri Azure StorSimple verileriyle birlikte kullanılabilir, verilerinizi power kilidini açabilirsiniz.


## <a name="region-availability"></a>Bölge kullanılabilirliği

StorSimple veri Yöneticisi aşağıdaki 7 bölgelerde kullanılabilir:

 - Güneydoğu Asya
 - Doğu ABD
 - Batı ABD
 - Batı ABD 2
 - Batı Orta ABD
 - Kuzey Avrupa
 - Batı Avrupa

Ancak, StorSimple veri Yöneticisi aşağıdaki bölgelerde verileri dönüştürmek için kullanılabilir. 

![Verileri için kullanılabilir bölgeleri](./media/storsimple-data-manager-overview/data-manager-job-definition-different-regions-m.png)

Yukarıdaki bölgeler hiçbirinde kaynak dağıtım dönüştürme işlemini getiren yeteneğine sahip olduğu için bu küme daha büyük olan bölgeler aşağıda. Bu nedenle, verilerinizi 19 bölgelerinin herhangi birinde bulunduğu sürece, bu hizmet kullanarak verilerinizi dönüştürebilirsiniz.


## <a name="choosing-a-region"></a>Bir bölge seçme

Öneririz:
 - Kaynak depolama hesabınız (StorSimple Cihazınızı ile ilişkili bir) ve (istediğiniz verileri yerel biçiminde) hedef depolama hesabı aynı Azure bölgesinde olmalıdır.
 - Veri Yöneticisi ve iş tanımını oluşturan StorSimple depolama hesabının bulunduğu bölgede getirin. Bu mümkün değilse, Yukarı en yakın Azure bölgesi Data Manager getirin ve StorSimple depolama hesabınız ile aynı bölgede iş tanımı oluşturun. 

    StorSimple depolama hesabınız iş tanımı oluşturma desteği 26 bölgelerde değilse, uzun gecikme süreleri ve potansiyel olarak yüksek çıkış ücretlerini gördüğünüz gibi StorSimple veri Yöneticisi çalışmıyor öneririz.

## <a name="security-considerations"></a>Güvenlikle ilgili dikkat edilmesi gerekenler

StorSimple veri Yöneticisi'ni StorSimple biçiminden yerel biçimine dönüştürmek için hizmet verileri şifreleme anahtarı gerekir. İlk cihaz StorSimple hizmeti kaydettiğinde hizmet verileri şifreleme anahtarı oluşturulur. Bu anahtar hakkında daha fazla bilgi için Git [StorSimple güvenlik](storsimple-8000-security.md).

Bir giriş olarak sağlanan hizmet verileri şifreleme anahtarı, Data Manager oluşturduğunuzda oluşturan bir anahtar kasasında depolanır. Aynı Azure bölgesinde olarak StorSimple Data Manager kasa bulunur. Veri Yöneticisi hizmetiniz sildiğinizde, bu anahtarı silinir.

Bu anahtar tarafından sağlanan işlem kaynaklarıdır dönüştürme gerçekleştirmek için kullanılır. Bu kaynaklar, iş tanımı aynı Azure bölgesinde bulunan işlem. Bu bölge olabilir veya veri yöneticinize Getir burada bölge ile aynı olmayabilir.

Veri Yöneticisi bölgenizi iş tanımı bölgesinden farklı ise, hangi veri/meta veri her Bu bölgeler bulunduğu anlamak önemlidir. Aşağıdaki diyagram veri Yöneticisi ve iş tanımı için farklı bölgelerde sahip etkisini gösterir.

![Farklı bölgelerdeki hizmet ve iş tanımı](./media/storsimple-data-manager-overview/data-manager-job-different-regions.png)

## <a name="managing-personal-information"></a>Kişisel bilgilerini yönetme

StorSimple veri Yöneticisi'ni toplamaz veya herhangi bir kişisel bilgi görüntüler. Daha fazla bilgi için Microsoft Privacy İlkesi gözden [Güven Merkezi](https://www.microsoft.com/trustcenter).

## <a name="next-steps"></a>Sonraki adımlar

[StorSimple veri Yöneticisi'ni kullanın, verilerinizi dönüştürmek için kullanıcı Arabirimi](storsimple-data-manager-ui.md).
