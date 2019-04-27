---
title: Azure Güvenlik Merkezi'nde Olayları ve Uyarıları Araştırma | Microsoft Docs
description: Bu belge Azure Güvenlik Merkezi'ndeki araştırma özelliğini kullanarak güvenlik olaylarını ve uyarılarını araştırmanıza yardımcı olur.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: a8e894a9-8781-4749-ae8f-8c8e01658566
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/29/2018
ms.author: rkarlin
ms.openlocfilehash: 6ba21c6eacd5b72d13706c08f0cc9883ccc91388
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60704834"
---
# <a name="investigate-incidents-and-alerts-in-azure-security-center-preview"></a>Azure Güvenlik Merkezi'nde Olayları ve Uyarıları Araştırma (Önizleme)
Bu belge Azure Güvenlik Merkezi'ndeki araştırma özelliğini kullanarak güvenlik olaylarını ve uyarılarını araştırmanıza yardımcı olur.

## <a name="what-is-investigation-in-security-center"></a>Güvenlik Merkezi'ndeki araştırma özelliği nedir?
Güvenlik Merkezi'ndeki araştırma özelliği olası bir [güvenlik olayını](https://docs.microsoft.com/azure/security-center/security-center-incident) önceliklendirmenizi, kapsamını anlamanızı ve kök nedenini tespit etmenizi sağlar.

Bu özelliğin amacı, araştırmakta olduğunuz olayla ilgili bulunan tüm varlıklar ([güvenlik uyarıları](https://docs.microsoft.com/azure/security-center/security-center-alerts-type), kullanıcılar, bilgisayarlar ve olaylar) arasında bağlantı kurarak araştırma sürecini kolaylaştırmaktır.  Güvenlik Merkezi bunu ilgili verilerle ilgili varlıklar arasında bağlantı kurarak ve bu bağlantıyı nesneler arasında gezinmenize ve ilgili bilgileri görselleştirmenize yardımcı olan bir canlı grafik kullanarak gerçekleştirir.


> [!NOTE]
> * [Özel uyarılar](security-center-custom-alert.md) Güvenlik Merkezi'nin araştırma özelliği desteklenmez.
> * Araştırma, yalnızca Windows sunuculardan toplanan verilere bağlı uyarılar için desteklenir.


## <a name="how-investigation-works"></a>Araştırma özelliği nasıl çalışır?
Araştırma özelliği araştırma panosunun ortasında bulunan bir grafikten oluşur. Graf her zaman belirli bir varlığa odaklanmış durumdadır ve bu varlıkla ilgili olan diğer varlıkları gösterir. Varlık bir güvenlik uyarısı, kullanıcı, bilgisayar veya olay olabilir.

![Eşleme](./media/security-center-investigation/security-center-investigation-fig1.png)

Kullanıcı grafik üzerindeki öğelere tıklayarak bir varlıktan diğerine geçebilir. Graf seçilen varlığı ve onunla ilgili varlıkları otomatik olarak merkeze alır. Seçimin ardından ilgili olmayan varlıklar grafikten kaldırılabilir.

### <a name="investigation-path"></a>Araştırma yolu
Araştırma yolu, kullanıcı farklı varlıklarda gezinirken araştırma bağlamının korunmasına yardımcı olur ve gezinmenin hızlı gerçekleşmesini sağlar. Araştırma sonuçlarını içeren olay her zaman araştırma yolunun en solundaki olay olur.

![Yol](./media/security-center-investigation/security-center-investigation-fig2.png)

### <a name="general-information"></a>Genel bilgiler
Sekmeler, grafikte yer alan bir varlıkla ilgili ek bilgileri gösterir. **Bilgi** sekmesi varlıkla ilgili farklı bilgi kaynaklarından toplanmış olan genel bilgileri gösterir.

![Genel bilgiler](./media/security-center-investigation/security-center-investigation-fig3.png)

Bilgi sekmesi haritada seçilen olayla ilgili bilgileri gösterir. Olay, bir araştırmanın sonuçlarını içeren bir kapsayıcıdır. Her araştırma bir olay bağlamında gerçekleşir.

Bir olay yalnızca kullanıcı belirli bir uyarı için **Araştırma başlat** düğmesine tıkladığında oluşturulur. Araştırmacının faydalanabileceği temel özellikler kullanıcı, bilgisayar veya uyarı gibi varlıkları işaretlemektir. Bir varlık ilgili olarak işaretlendiğinde bir neden belirtilir. Bu noktadan itibaren ilgili varlık grafikte doğrudan olayın altında ve olay varlıkları listesinde görünür.

### <a name="entities"></a>Varlıklar

**Varlıklar** sekmesi ilgili tüm varlıkları türe göre gruplanmış şekilde gösterir. Bu sekme grafikte gösterilemeyecek kadar çok fazla varlık olduğunda ve varlıkların adı çok uzun ve tablo halinde incelemesi daha kolay olduğunda olmak üzere iki durumda kullanım kolaylığı sunar.

![Varlıklar](./media/security-center-investigation/security-center-investigation-fig4.png)

### <a name="search"></a>Arama

**Arama** sekmesi varlık için var olan tüm günlük türlerini sunar. Her bir günlük türünde var olan kayıt sayısını görebilirsiniz. Kayıt türlerinden birine tıkladığınızda arama ekranı açılır. Arama ekranında aramanıza filtre uygulayabilir ve uyarı ayarlama gibi çeşitli arama özelliklerinden faydalanabilirsiniz. Mevcut sürümde arama sekmesi yalnızca kullanıcı ve bilgisayar varlıkları için kullanılabilmektedir.

![Arama](./media/security-center-investigation/security-center-investigation-fig5.png)

### <a name="exploration"></a>Araştırma

**Araştırma** sekmesi araştırmacının varlıkla ilgili çeşitli sorunlara ait verileri incelemesini sağlar. Örneğin bir makine araştırıldığında üzerinde yürütülen işlemlerin listesi araştırma sekmesinde görüntülenir. Bazı durumlarda araştırma sekmesinde şüpheli sorunlara işaret edebilecek veriler sunulur. Araştırmacı verilere sekme içinde göz atabilir veya arama ekranında açarak büyük veri kümelerini incelemenin yanı sıra filtreleme ve Excel'e aktarma gibi gelişmiş arama seçeneklerini kullanabilir.

![Araştırma](./media/security-center-investigation/security-center-investigation-fig6.png)

### <a name="timeline"></a>Zaman çizelgesi

Grafta yer verilen verilerin çoğu ve çeşitli sekmeler belirli bir zaman aralığı için geçerlidir. Bu zaman kapsamı, grafiğin sol üst kısmında yer alan zaman kapsamı seçici ile ayarlanır. Araştırmacı zaman kapsamını farklı yöntemlerle belirleyebilir.

![Zaman çizelgesi](./media/security-center-investigation/security-center-investigation-fig7.png)

Aşağıdaki öğeler zaman kapsamından etkilenir:

- Grafta görüntülenen kullanıcı-bilgisayar ilişkisi: Yalnızca bu zaman kapsamında ilgili bilgisayarda oturum açmış olan kullanıcılar gösterilir.
- Bilgisayarlar ve kullanıcılar incelendiğinde görüntülenecek uyarılar: Yalnızca zaman kapsamında gerçekleşen uyarılar gösterilir.
- Varlıklar sekmesi grafikle aynı mantığı kullanır.

Aşağıdaki öğeler seçilen zaman kapsamından bağımsız olarak gösterilir:

- Bir uyarı gösterildiğinde içindeki kullanıcılar ve bilgisayarlar gibi tüm varlıklar her zaman gösterilir.
- Bir olayda bir varlık varsa gösterilir.

> [!NOTE]
> **Arama** ve **Araştırma** sekmesinde yalnızca bu zaman kapsamı içindeki kayıtlar gösterilir.

## <a name="how-to-perform-an-investigation"></a>Nasıl araştırma yapabilirim?

Araştırmanızı bir güvenlik olayı veya bir uyarıdan başlatabilirsiniz. Belirlediğiniz seçenek ihtiyaçlarınıza göre değişiklik gösterecektir. Aşağıdaki adımlar bir uyarıdan araştırma başlatmak için kullanılır:

1.  **Güvenlik Merkezi** panosunu açın.
2.  **Güvenlik Uyarıları**'na tıklayın ve araştırmak istediğiniz olayı seçin.
3.  Olay sayfasında **Araştırma başlat** düğmesine tıklayın. **Araştırma** panosu açılır.

    ![Uyarı](./media/security-center-investigation/security-center-investigation-fig8.png)

4. Bu panodan eşleşen varlığı seçebilirsiniz ve bu varlıkla ilgili bilgiler ekranın sağ tarafında görüntülenir.

    ![Araştırma panosu](./media/security-center-investigation/security-center-investigation-fig9.png)

Bu noktadan bu olayla ilgili varlıkları araştırabilir ve her biriyle ilgili ayrıntılı araştırma gerçekleştirebilirsiniz.

## <a name="see-also"></a>Ayrıca bkz.
Bu belgede Güvenlik Merkezi'ndeki araştırma özelliğini nasıl kullanacağınızı öğrendiniz. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve ele alma](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts). Güvenlik Merkezi’nde uyarıları yönetme ve güvenlik olaylarına yanıt vermeyi öğrenin.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md). Azure kaynaklarınızı durumunu izleme hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'ndeki güvenlik uyarılarını anlama](https://docs.microsoft.com/azure/security-center/security-center-alerts-type). Farklı güvenlik uyarısı türleri hakkında bilgi edinin.
* [Azure Güvenlik Merkezi Sorun Giderme Kılavuzu](https://docs.microsoft.com/azure/security-center/security-center-troubleshooting-guide). Güvenlik Merkezi’nde sık karşılaşılan sorunları gidermeyi öğrenin.
* [Azure Güvenlik Merkezi SSS](security-center-faq.md). Hizmet kullanımı ile ilgili sık sorulan soruları bulun.
* [Azure Güvenlik Blogu](https://blogs.msdn.com/b/azuresecurity/). Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulun.
