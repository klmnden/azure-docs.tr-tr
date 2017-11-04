---
title: "Azure maliyeti yönetim erişimi atayın | Microsoft Docs"
description: "Varlıklara erişim düzeyleri tanımlamak kullanıcı hesapları ile yönetim verilerini maliyet erişimi atayın."
services: cost-management
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 10/11/2017
ms.topic: tutorial
ms.service: cost-management
ms.custom: mvc
manager: carmonm
ms.openlocfilehash: a42f3b51bf6d888d0d5602887ed317c6164391ef
ms.sourcegitcommit: d03907a25fb7f22bec6a33c9c91b877897e96197
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/12/2017
---
# <a name="assign-access-to-cost-management-data"></a>Maliyet yönetim verileri için erişimi atayın

Maliyet yönetim verilerine erişim, kullanıcı veya varlık yönetimi tarafından sağlanır. Cloudyn kullanıcı hesaplarını belirlemek için erişim *varlıklar* ve yönetim işlevleri. Burada iki tür erişimi: Yönetici ve kullanıcı. Kullanıcı başına değiştirilmediği sürece yönetici erişimi tüm işlevleri Kısıtlanmamış kullanıcı kullanımına Cloudyn Portalı'nda izin verir. dahil olmak üzere: kullanıcı yönetimi, alıcı listeleri yönetimi ve kök varlık erişim tüm varlık verileri. Kullanıcı erişimi raporları görüntülemek ve bir varlığın verilerinin sahip oldukları erişim kullanarak raporları oluşturmak son kullanıcılar için tasarlanmıştır.

Varlıkları, iş kuruluşunuzun hiyerarşik bir yapı yansıtmak için kullanılır. Bunlar, Departmanlar, bölümler ve Cloudyn kuruluşunuzda ekiplerde tanımlar. Varlık hiyerarşisi varlıklar tarafından harcama doğru bir şekilde izlemenize yardımcı olur.

Azure sözleşmesi veya hesap kaydolurken tüm adımları Bu öğreticinin gerçekleştirebilmek için yönetici izni olan bir hesap Cloudyn içinde oluşturuldu. Bu öğretici, kullanıcı ve varlık yönetimi dahil olmak üzere maliyet yönetim verilerine erişim kapsar. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Yönetici erişimi olan bir kullanıcı oluşturma
> * Kullanıcı erişimi olan bir kullanıcı oluşturma
> * Varlıklar oluşturma



## <a name="create-a-user-with-admin-access"></a>Yönetici erişimi olan bir kullanıcı oluşturma

Yönetici erişimi zaten sahip olsa da, kuruluşunuzda iş arkadaşlarınızla de yönetici erişimi gerekebilir. Dişli simgesini seçin ve sağ üst Cloudyn Portalı'nda **kullanıcı yönetimi**. Tıklatın **yeni kullanıcı Ekle** yeni bir kullanıcı eklemek için.

Kullanıcı hakkında gerekli bilgileri girin. Kullanıcının ilk oturum açma üzerinde yeni bir parola ayarlayabilirsiniz parola alanı boş bırakabilirsiniz. Seçtiğinizde oturum açma bilgilerini içeren bir bağlantı kullanıcıya e-posta yoluyla Cloudyn gönderilir **e-posta ile bildir kullanıcı**. Böylece kullanıcı oluşturmak ve diğer kullanıcıları değiştiren kullanıcı yönetimi izin vermek için İzinler'i seçin. Alıcı listeler alıcı listeleri düzenlemek izin vermek için yönetim.

Altında **kullanıcı yönetici erişimi olan**, kuruluşunuzun kök varlık seçilir. Seçilen kök bırakın ve ardından kullanıcı bilgilerini kaydedin. Kök varlık seçerek kullanıcının yönetici izni yalnızca ağacındaki kök varlığa, aynı zamanda altında bulunan tüm varlıklar için sahip olmasını sağlar.  
  ![Yönetici erişimi olan yeni kullanıcı Ekle](.\media\tutorial-user-access\new-admin-access.png)

## <a name="create-a-user-with-user-access"></a>Kullanıcı erişimi olan bir kullanıcı oluşturma
Maliyet yönetim verilerine erişmesi gereken kullanıcılar panoları gibi ve raporları görüntüleyebilmesi için kullanıcı erişimi olmalıdır. Yeni bir kullanıcı kullanıcı erişimi ile aşağıdaki farklarla birlikte yönetici erişimi ile oluşturulan benzer oluşturun:

- Clear **kullanıcı yönetimine izin ver**, **izin alıcı Yönetimi listeler**ve tüm temizleyin **kullanıcı yönetici erişimi olduğundan** listesi.
- Kullanıcı Giriş erişmesi gereken varlıkları seçin **kullanıcının sahip kullanıcı erişim** listesi.
- Ayrıca, gerektiği gibi belirli varlıklara erişim için yönetici izin verebilirsiniz.

![Kullanıcı erişimi olan yeni kullanıcı Ekle](.\media\tutorial-user-access\new-user-access.png)

Kullanıcı ekleme hakkında öğretici bir video izlemek için bkz: [Azure maliyeti Yönetimi Cloudyn tarafından ekleme kullanıcılara](https://youtu.be/Nzn7GLahx30).

## <a name="create-entities"></a>Varlıklar oluşturma

Maliyet varlık hiyerarşisi tanımladığınızda, en iyi uygulama kurumunuzun yapısını belirlemektir.

Ağaç yapısı olarak nasıl istediğiniz veya Maliyet merkezleri, ortamları ve satış Departmanlar iş birimleri tarafından yinelenmeli maliyetlerini gereken göz önünde bulundurun. Cloudyn varlık ağacında varlık devralma nedeniyle esnektir. Bulut hesaplarınız için bireysel abonelikleri belirli varlıklara bağlanır. Bu nedenle, çok kiracılı varlıklardır. Belirli kullanıcıları yalnızca kendi işinizi varlıklar segment erişim atayabilirsiniz. Bunun yapılması, yan kuruluşlarının gibi bir iş büyük bölümünü hatta üzerinden yalıtılmış verileri tutar. Ve veri yalıtımı ile idare yardımcı olur.  

Cloudyn ile Azure sözleşmesi veya hesap kaydolurken kullanım, performans, faturalama ve aboneliklerinizi etiket verileri de dahil olmak üzere, Azure kaynak verilerinizi Cloudyn hesabınıza kopyalandı. Bununla birlikte, varlık ağaç el ile oluşturmanız gerekir. Azure Resource Manager kayıt atladıysanız sonra Fatura veriler yalnızca ve birkaç varlık raporları Cloudyn Portalı'nda kullanılabilir.

Cloudyn Portalı'nda tıklatın **ayarları** seçin ve sağ üst **bulut hesapları**. Tek bir varlık (kök) ile başlatmak ve, varlık Ağaç kökü altındaki oluşturun. Ağaç tamamlandıktan sonra birçok BT kuruluşları şuna benzeyebilir bir varlık hiyerarşisi bir örneği burada verilmiştir:

![Varlık ağacı](.\media\tutorial-user-access\entity-tree.png)

Yanına **varlıklar**, tıklatın **varlık Ekle**. Kişi veya eklemek istediğiniz departmanı hakkındaki bilgileri girin. **Tam adı** ve **e-posta** alanları mevcut kullanıcıları eşleşmesi gerekmez. Erişim düzeyleri listesini görmek isterseniz, Yardım için arama *varlık ekleme*.

![Varlık ekleme](.\media\tutorial-user-access\add-entity.png)

İşiniz bittiğinde, **kaydetmek** varlık.


Maliyet varlık hiyerarşisi oluşturma hakkında daha fazla öğretici bir video izlemek için bkz: [Cloudyn tarafından Yönetimi Azure maliyeti, maliyet varlık hiyerarşisi oluşturmayı](https://youtu.be/dAd9G7u0FmU).

Bir Azure Kurumsal anlaşmasına kullanıcı varsa, hesapları ve varlıkları abonelikleri ilişkilendirme hakkında öğretici bir video izlemek [için Azure Resource Manager ile Azure maliyeti Yönetimi Cloudyn tarafından bağlanma](https://youtu.be/oCIwvfBB6kk).

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Yönetici erişimi olan bir kullanıcı oluşturma
> * Kullanıcı erişimi olan bir kullanıcı oluşturma
> * Varlıklar oluşturma

Geçmiş verileri kullanarak Harcamaları tahmin etmenize öğrenmek için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [Gelecek harcamaları tahmin etme](tutorial-forecast-spending.md)
