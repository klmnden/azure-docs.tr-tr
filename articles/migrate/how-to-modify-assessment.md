---
title: Azure geçişi Server değerlendirmesi için Değerlendirmeler özelleştirme | Microsoft Docs
description: Azure geçişi Server değerlendirmesi ile oluşturulan değerlendirmeleri özelleştirmeyi açıklar
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 07/09/2019
ms.author: raynew
ms.openlocfilehash: 8b200ce3d6e73a575b1b89d82a9323d58f435a48
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67807921"
---
# <a name="customize-an-assessment"></a>Bir değerlendirmeyi özelleştirme

Bu makalede, Azure geçişi Server değerlendirmesi tarafından oluşturulan değerlendirmeleri özelleştirmeyi açıklar.

[Azure geçişi](migrate-services-overview.md) bulma, değerlendirme ve şirket içi uygulamalarınızı ve iş yükleri ve private/public bulut Vm'lerinden azure'a geçişini izlemek için merkezi bir nokta sağlar. Hub araçları Azure geçişi, değerlendirme ve geçiş, ek olarak üçüncü taraf bağımsız yazılım satıcısı (ISV) teklifleri sağlar.

Azure'a geçiş için şirket içi VMware Vm'leri ve Hyper-V Vm'leri için Değerlendirmeler oluşturmak için Azure geçişi Server değerlendirmesi aracını kullanabilirsiniz. 

## <a name="about-assessments"></a>Değerlendirmeler hakkında

Azure geçişi Server değerlendirmesi'ni kullanarak çalıştırabilirsiniz değerlendirmeleri iki tür vardır.

**Değerlendirme** | **Ayrıntılar** | **Veri**
--- | --- | ---
**Performans tabanlı** | Toplanan performans verileri temel alan değerlendirmeleri | **VM boyutu önerilen**: CPU ve bellek kullanım verilerini temel alarak.<br/><br/> **Disk türü (standart veya premium yönetilen disk) önerilen**: IOPS ve aktarım hızını şirket içi disk bağlı.
**Şirket içi olarak** | Boyutlandırma şirket içinde temel alan değerlendirmeleri. | **VM boyutu önerilen**: Şirket içi VM boyutuna göre<br/><br> **Disk türü önerilen**: Değerlendirme için seçtiğiniz depolama türüne ayarına göre.


## <a name="how-is-an-assessment-done"></a>Değerlendirme nasıl gerçekleştirilir?

Azure geçişi değerlendirmesi, üç aşamadan oluşur. Boyutlandırma tarafından izlenen bir uygunluğu analiziyle değerlendirme başlar ve son olarak, bir aylık maliyet tahmini. Öncekine geçerse bir makine yalnızca bir sonraki aşamaya taşır. Azure uygunluk denetimi bir makine başarısız olursa, Azure ve boyutlandırma ve maliyet yapılmaz için örneğin, bunu uygun olarak işaretlenir.

## <a name="whats-in-an-assessment"></a>Bir değerlendirme neleri içerir?

**Özellik** | **Ayrıntılar**
--- | ---
**Hedef konum** | Geçişi yapmak istediğiniz Azure konumu.<br/> Azure geçişi şu anda bu hedef bölgeler destekler: Avustralya Doğu, Avustralya Güneydoğu, Brezilya Güney, Kanada Orta, Kanada Doğu, Orta Hindistan, Orta ABD, Çin Doğu, Kuzey Çin, Doğu Asya, Doğu ABD, Doğu ABD 2, Almanya Orta, Almanya Kuzeydoğu, Doğu, Japonya Batı, Kore Orta, Kore Japonya Güney, Kuzey Orta ABD, Kuzey Avrupa, Güney Orta ABD, Güneydoğu Asya, Hindistan Güney, UK Güney, UK Batı, ABD Devleti Arizona, ABD Devleti Texas, ABD Devleti Virginia, Batı Orta ABD, Batı Avrupa, Batı Hindistan, Batı ABD ve Batı ABD 2.<br/> Varsayılan hedef bölge, Batı ABD 2 olarak ayarlanır.
**Depolama türü** | Standart HHD diskler/standart SSD disk/Premium.<br/> Disk önerisi, değerlendirme içinde otomatik olarak depolama türü belirttiğinizde, disklerin (IOPS ve aktarım hızı) performans verileri üzerinde temel alır.<br/> Premium veya standart depolama türü belirtirseniz, SKU'nun disk depolama türü içinde bir değerlendirme önerir seçili.<br/> Tek bir örneği sanal makine SLA'sı % 99,9 elde etmek istiyorsanız, depolama türü Premium yönetilen diskler ayarlayabilirsiniz. Ardından tüm değerlendirmesi diskler, Premium yönetilen diskler önerilecektir. <br/> Azure Geçişi yalnızca yönetilen disklerin geçiş değerlendirmesini destekler.<br/> 
**Ayrılmış örnekleri (RI)** | Ayrılmış örnekleriniz varsa bu özelliği belirtin. Maliyet tahminleri değerlendirmede RI indirimleri hesaba katması. Ayrılmış örnekleri şu anda yalnızca Azure geçişi Kullandıkça Öde teklifleri için desteklenir.
**Boyutlandırma ölçütü** | Doğru boyuta VM'ler için kullanılır. Performans tabanlı boyutlandırma olabilir veya **şirket içi olarak**, performans geçmişini dikkate almadan.
**Performans geçmişi** | VM performansı değerlendirmek için dikkate alınacak süre. Bu özellik yalnızca boyutlandırma performans tabanlı olduğunda geçerlidir.
**Yüzdebirlik kullanımı** | Doğru boyutlandırma VM'ler için kullanılan performans örnek yüzdelik dilim değeri. Bu özellik yalnızca boyutlandırma performans tabanlı olduğunda geçerlidir.
**VM serisi** | VM serisi, boyut tahmini için kullanılır. Örneğin, Azure’da A serisi VM’lere geçirmeyi planlamadığınız bir üretim ortamınız varsa, A serisini liste veya serilerin dışında bırakabilirsiniz. Boyutlandırma yalnızca seçili serilerde yapılır.
**Konfor katsayısı** | Azure geçişi Server değerlendirmesi değerlendirme sırasında bir Tamponu (konfor katsayısı) göz önünde bulundurur. Bu tampon, VM’lerin makine kullanım verilerinin (CPU, bellek, disk ve ağ) üzerine uygulanır. Konfor katsayısı; sezona özgü kullanım, kısa performans geçmişi ve gelecek kullanımlarda oluşabilecek artışlar gibi konuları hesaba katar.<br/><br/> Örneğin, %20 kullanıma sahip 10 çekirdekli bir VM normalde 2 çekirdekli VM ile sonuçlanır. Ancak, 2.0x konfor katsayısı ile sonuç 4 çekirdekli VM olur.
**Teklif** | Kaydolduğunuz [Azure teklifi](https://azure.microsoft.com/support/legal/offer-details/). Azure Geçişi, buna göre bir maliyet tahmini oluşturur.
**Para Birimi** | Fatura para birimi. 
**İndirim (%)** | Azure teklifinin yanı sıra aldığınız, aboneliğe özgü indirim.<br/> Varsayılan ayar, %0’dır.
**VM çalışma süresi** | Sanal makinelerinizi Azure'da 7/24 çalıştırılması kullanmayacaksanız, bunlar çalıştırıyordur için süresi (gün / ay sayısı) ve her gün saat sayısı belirtebilirsiniz ve maliyet tahminleri buna göre uygulanır.<br/> 31 gün / ay ve günde 24 saat varsayılan değerdir.
**Azure Hibrit Avantajı** | Yazılım Güvencesine sahip ve için uygun olup olmadığını belirtir [Azure hibrit avantajı](https://azure.microsoft.com/pricing/hybrid-use-benefit/). Evet olarak ayarlanırsa, Windows sanal makineleri için Windows olmayan Azure fiyatları dikkate alınır. | Varsayılan değer Evet’tir.


## <a name="edit-assessment-properties"></a>Değerlendirme özelliklerini Düzenle

Değerlendirme oluşturduktan sonra değerlendirme özelliklerini düzenlemek için aşağıdakileri yapın:

1. Azure geçişi projesinde tıklayın **sunucuları**.
2. İçinde **Azure geçişi: Server değerlendirmesi**, değerlendirmeler sayımı'nı tıklatın.
3. İçinde **değerlendirme**, ilgili değerlendirme tıklayın > **özelliklerini düzenleme**.
5. Değerlendirme özellikleri aşağıdaki tabloya uygun olarak özelleştirin.
6. Tıklayın **Kaydet** güncelleştirme değerlendirmesi için.


Bir değerlendirme oluşturulurken değerlendirme özellikleri de düzenleyebilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

Değerlendirmelerin nasıl hesaplandığı hakkında [daha fazla bilgi](concepts-assessment-calculation.md) edinin.
