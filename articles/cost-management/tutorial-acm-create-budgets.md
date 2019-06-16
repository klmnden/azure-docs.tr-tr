---
title: Öğretici - Azure bütçelerini yönetin ve oluşturun | Microsoft Docs
description: Bu öğretici, plan ve hesap Fiyatlar, kullandığınız Azure Hizmetleri için yardımcı olur.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 05/14/2019
ms.topic: conceptual
ms.service: cost-management
manager: dougeby
ms.custom: seodec18
ms.openlocfilehash: eab45948b5f931377396d93d93e8955ba0f3e767
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65792847"
---
# <a name="tutorial-create-and-manage-azure-budgets"></a>Öğretici: Oluşturma ve Azure bütçelerini yönetin

Maliyet Yönetimi hizmetindeki bütçe işlevi, kuruluşunuzda sorumluluk kültürünü planlamanıza ve güçlendirmenize yardımcı olur. Bütçeleri kullanarak belirli bir dönem içinde kullandığınız veya abone olduğunuz Azure hizmetlerini takip edebilirsiniz. Diğerleri kendi proaktif olarak maliyetleri yönetmek ve harcama zaman içinde nasıl ilerledikçe izlemek için harcama hakkında bildirmenize yardımcı olurlar. Oluşturduğunuz bütçe eşikleri aşıldığında yalnızca bildirimleri tetiklenir. Kaynaklarınızı hiçbiri etkilenir ve tüketiminiz durduruldu değil. Bütçe karşılaştırın ve maliyetleri analiz ederken harcama izlemek için kullanabilirsiniz.

Aylık bütçeleri dört saatte bir harcama karşı değerlendirilir. Ancak, veri ve tüketilen kaynaklar için bildirimler sekiz saat içinde kullanılabilir.  

Bütçe gelecekte bir sona erme tarihi seçtiğinizde aynı bütçe tutarını (aylık, üç aylık veya yıllık) bir süre sonunda otomatik olarak sıfırlama. Bütçelenen zaman ayrı bütçelerini oluşturmanıza gerek bunlar ile aynı bütçe miktarını sıfırlamak için para birimi tutarlar gelecek dönemlere ait farklı.

Bu öğreticideki örneklerde oluşturmak ve düzenlemek için Azure Kurumsal Anlaşma (EA) aboneliğiniz bir bütçe size yol gösterir.

İzleme [Azure maliyet yönetimi ile harcama izlemek için bir bütçe oluşturma](https://www.youtube.com/watch?v=ExIVG_Gr45A) video bütçelerini harcama izlemek için Azure'da nasıl oluşturabileceğinizi öğrenin.


Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure portalında bir bütçe oluşturun
> * Bütçe Düzenle

## <a name="prerequisites"></a>Önkoşullar

Bütçe çeşitli Azure hesap türleri için desteklenir. Desteklenen bir hesap türleri için tam listesini görüntülemek için bkz: [anlamak maliyet Yönetimi verilerine](understand-cost-mgt-data.md). Bütçelerini görüntülemek için en az Azure hesabınız için okuma erişimi.

 Azure EA abonelikleri için bütçeler görüntülemek için okuma erişimi olmalıdır. Oluşturma ve bütçelerini yönetmek için katkıda bulunan izni olmalıdır. EA aboneliklerinde ve kaynak gruplarınız için ayrı ayrı bütçeden oluşturabilirsiniz. Ancak, EA hesapları faturalama bütçelerini oluşturulamıyor.

Aşağıdaki Azure izinleri veya kapsamlar, bütçeler için abonelik başına kullanıcı ve grup tarafından desteklenir. Kapsamlar hakkında daha fazla bilgi için bkz: [anlayın ve kapsamlı iş](understand-work-scopes.md).

- Sahip – Bir abonelik için bütçe oluşturabilir, değiştirebilir veya silebilir.
- Katkıda bulunan ve maliyet Yönetimi katkıda bulunan – oluşturmak, değiştirmek veya kendi bütçelerini silin. Başkaları tarafından oluşturulan bütçelerin miktarlarını değiştirebilir.
- Okuyucu ve maliyet Yönetimi okuyucu – iznine sahip oldukları bütçelerini görüntüleyebilirsiniz.

Maliyet Yönetimi verilerine izin atama hakkında daha fazla bilgi için bkz. [maliyet Yönetimi verilerine erişim atama](assign-access-acm-data.md).

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

- https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="create-a-budget-in-the-azure-portal"></a>Azure portalında bir bütçe oluşturun

Aylık, üç aylık veya yıllık bir dönem için bir Azure aboneliği bütçe oluşturabilirsiniz. Azure portalında gezinme içeriğinizi bir abonelik için veya bir yönetim grubu için bütçe oluşturduğunuz olup olmadığını belirler.

İstenen kapsamı oluşturmak veya bir bütçe görüntülemek için Azure portal ve select açın **bütçelerini** menüsünde. Örneğin, gidin **abonelikleri**listeden aboneliği seçin ve ardından **bütçelerini** menüsünde. Kullanım **kapsam** bir yönetim grubunda bütçelerini gibi farklı bir kapsam geçmek zehirli. Kapsamlar hakkında daha fazla bilgi için bkz: [anlayın ve kapsamlı iş](understand-work-scopes.md).

Bütçe oluşturduktan sonra bunlar geçerli bunlara karşı harcamalarınızı basit bir görünümü gösterir.

**Ekle**'ye tıklayın.

![Azure portalında gösterilen yönetim bütçelerini maliyeti](./media/tutorial-acm-create-budgets/budgets01.png)

İçinde **Oluştur bütçe** penceresinde bir bütçe adı ve bütçe miktarı girin. Ardından, bir aylık, üç aylık, veya yıllık süresi seçin. Ardından, bir bitiş tarihi seçin. Bütçe en az bir maliyet eşiği (% bütçe) ve karşılık gelen e-posta adresi gerektirir. İsteğe bağlı olarak, en fazla beş eşikleri ve tek bir bütçe içinde beş e-posta adresi ekleyebilirsiniz. Bütçe eşiği karşılandığında, e-posta bildirimleri normal olarak sekiz saatten kısa bir süre içinde alınır. Bildirimleri hakkında daha fazla bilgi için bkz. [kullanım maliyeti uyarılar](cost-mgt-alerts-monitor-usage-spending.md).

Bir Kullandıkça Öde, MSDN veya Visual Studio aboneliğiniz varsa, fatura fatura döneminize Takvim ayına hizalanmıyor Abonelikler ve kaynak gruplarını bu tür için fatura döneminize veya takvim ayı hizalanmış bir bütçe oluşturabilirsiniz. Fatura döneminize hizalanmış bir bütçe oluşturmak için bir faturalandırma ayı, faturalandırma üç aylık dönem veya faturalama yıl sıfırlama süresini seçin. Takvim ayına göre bir bütçe oluşturmak için bir sıfırlama süre aylık, üç aylık veya yıllık seçin.

4\.500 için aylık bir bütçe oluşturma örneği aşağıda verilmiştir. %90 bütçenin ulaşıldığında bir e-posta uyarısı oluşturulan.

![Oluştur bütçe kutusunda gösterilen örnek bilgileri](./media/tutorial-acm-create-budgets/monthly-budget01.png)

Üç aylık bir bütçe oluşturduğunuzda, bir aylık bütçe aynı şekilde çalışır. Üç aylık dönem arasında üç aylık üç aylık dönem için bütçe miktarını şekilde eşit bölünür farktır. Bekleyebileceğiniz gibi yıllık bir bütçe tutarı tüm 12 ay takvim yılı boyunca şekilde eşit bölünür.

Maliyet Yönetimi güncelleştirilmiş fatura veri aldığında, geçerli harcama bütçenize göre güncelleştirilir. Genellikle, her gün.

![Geçerli harcama bütçenize göre gösteren örnek bilgiler](./media/tutorial-acm-create-budgets/budgets-current-spending.png)

Bütçe oluşturduktan sonra maliyet analizi gösterilmektedir. Bütçenize göre harcama eğilimi görüntüleme olduğunda ilk adımlarından biri için başlangıç [maliyetlerinizi analiz edin ve harcama](quick-acm-cost-analysis.md).

![Örnek bütçe ve maliyet analizi gösterilen harcama](./media/tutorial-acm-create-budgets/cost-analysis.png)

Önceki örnekte, bir abonelik için bir bütçe oluşturuldu. Ancak, bir kaynak grubu için bütçe oluşturabilirsiniz. Bir kaynak grubu için bütçe oluşturmak istiyorsanız, gitmek **maliyet Yönetimi + faturalandırma** &gt; **abonelikleri** &gt; bir aboneliği seçin > **kaynak grupları** > bir kaynak grubu seçin > **bütçelerini** > ardından **Ekle** bütçe.

## <a name="edit-a-budget"></a>Bütçe Düzenle

Sahip olduğunuz erişim düzeyine bağlı olarak, bir bütçe özelliklerini değiştirmek için düzenleyebilirsiniz. Kullanıcı yalnızca abonelik için katkıda bulunan izni olduğundan aşağıdaki örnekte, bazı özellikleri salt okunurdur. Şu anda **sona erme tarihi** devre dışı bırakılır ve bir kez ayarlandıktan sonra değiştirilemez.

![Örneği, çeşitli özelliklerini değiştirmek için bir bütçe düzenleme](./media/tutorial-acm-create-budgets/edit-budget.png)

## <a name="trigger-an-action-group"></a>Tetikleyici bir eylem grubu

Oluşturduğunuzda veya düzenlediğinizde bir abonelik veya kaynak grubu kapsamı için bütçe, bir eylem grubu çağıracak şekilde yapılandırabilirsiniz. Bütçe eşiğine karşılandığında eylem grubu çeşitli farklı eylemler gerçekleştirebilirsiniz. Eylem grupları hakkında daha fazla bilgi için bkz. [oluştur ve Eylem grupları Azure portalında yönetmek](../azure-monitor/platform/action-groups.md). Bütçe tabanlı otomasyon ile Eylem grupları kullanma hakkında daha fazla bilgi için bkz. [Azure bütçe ile maliyetleri yönetme](../billing/billing-cost-management-budget-scenario.md).

Eylem grupları oluştur veya güncelleştir için tıklatın **Eylem grupları yönetme** oluşturma ya da bir bütçe düzenleme.

![Eylem grupları yönetme gösterilecek bir bütçe oluşturma örneği](./media/tutorial-acm-create-budgets/manage-action-groups01.png)

Ardından, **eylem grubu Ekle** ve eylem grubu oluşturun.


![Eylem grubu Ekle kutusunun görüntüsü](./media/tutorial-acm-create-budgets/manage-action-groups02.png)

Sonra eylem grubu oluşturulur, bütçenizi için döndürülecek kutusunu kapatın.

Bütçenizi bir bireysel eşiğine ulaşıldığında, Eylem grubunu kullanmak üzere yapılandırın. En fazla beş farklı eşikler desteklenir.

![Eylem grubu seçimi için bir uyarı durumu gösteren örnek](./media/tutorial-acm-create-budgets/manage-action-groups03.png)

Aşağıdaki örnek, % 50, %75 ve % 100 olarak ayarlayın, bütçe eşikleri gösterir. Her, belirtilen eylem grubu içinde belirtilen eylemleri tetiklemek için yapılandırılır.

![Çeşitli eylem grupları ve eylem türü ile yapılandırılmış uyarı koşullarına gösteren örnek](./media/tutorial-acm-create-budgets/manage-action-groups04.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Azure portalında bir bütçe oluşturun
> * Bütçe Düzenle

Yinelenen bir dışarı aktarma, maliyet Yönetimi verilerine için oluşturmak için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Oluşturma ve dışarı aktarılan verileri yönetme](tutorial-export-acm-data.md)
