---
title: Yeni Azure İzleyici uyarıları deneyimi etkinlik günlüğü uyarıları kullanın
description: Azure İzleyicisi altında uyarıları (Önizleme) sekmesinden etkinlik günlüğü uyarıları oluşturma Bu makalede, bu özellik için yeni kullanıcı deneyimi ayrıntıları verilmektedir.
author: jyothirmaisuri
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 02/05/2018
ms.author: v-jysur
ms.component: alerts
ms.openlocfilehash: 10cd4e2ea14ab66a44ba2123e025b3d1b91f385c
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35263581"
---
# <a name="create-activity-log-alerts-using-the-new-alerts-experience"></a>Yeni uyarılar kullanarak uyarıları deneyimi etkinlik günlüğü oluşturma

Etkinlik günlüğü uyarıları uyarıda belirtilen koşullara uyan yeni bir etkinlik günlüğü olay ortaya çıktığında etkinleştirilmiş uyarılar.

Bu uyarılar için Azure kaynaklarını, bir Azure Resource Manager şablonu kullanılarak oluşturulabilir. Bunlar ayrıca oluşturulabilir, güncelleştirilmiş veya Azure portalında silindi. Bu makalede, etkinlik günlüğü uyarıları kavramları tanıtır. Ardından, yeni deneyimi kullanarak Etkinlik günlüğü olaylarını üzerinde bir uyarı ayarlamak için Azure Portalı'nı kullanmayı gösterir [Azure uyarıları](monitoring-overview-unified-alerts.md).

Genellikle, etkinlik genellikle belirli kaynak grupları veya kaynak için kapsamlı, Azure aboneliğindeki kaynaklar üzerinde belirli değişiklikler meydana geldiğinde bildirim almak için günlük uyarılar oluşturabilir. Örneğin, herhangi bir bildirim almak istediğiniz durumlarda (örnek kaynak grubunda) sanal makine **myProductionResourceGroup** silinir, veya bir kullanıcıya, aboneliğinizdeki herhangi bir yeni rol atanmışsa bildirim isteyebilirsiniz.

Herhangi bir üst düzey özelliği JSON nesnesinde bir etkinlik günlüğü olayı için temel bir etkinlik günlüğü uyarı yapılandırabilirsiniz. Ancak, portal aşağıdaki bölümlerde ayrıntılı olarak en yaygın yönelik seçenekleri gösterir:

>[!NOTE]

> Kategori "Yönetici" olduğunda, önceki ölçüt en az biri, Uyarınız belirtmeniz gerekir. Etkinlik günlüklerde her bir olay oluşturulduğunda etkinleştiren bir uyarı oluşturamazsınız.
>

Bir etkinlik günlüğü uyarı etkinleştirildiğinde, Eylemler veya bildirimleri oluşturmak için bir eylem grubu kullanır. Bir eylem grubu yeniden kullanılabilir e-posta adresleri gibi bildirim alıcıları Web kancası URL'leri ya da SMS telefon numaralarının kümesidir. Alıcılar merkezileştirmek ve bildirim kanallarını grup için birden çok uyarı başvurulabilir. Etkinlik günlüğü Uyarınız tanımlamak için iki seçeneğiniz vardır. Şunları yapabilirsiniz:

* Varolan bir eylem Grup etkinlik günlüğü Uyarınız kullanın.
* Yeni bir eylem grubu oluşturun.

Eylem grupları hakkında daha fazla bilgi için bkz: [oluşturma ve Eylem grupları Azure portalında yönetmek](monitoring-action-groups.md).

Hizmet durumu bildirimlerine hakkında daha fazla bilgi için bkz: [hizmet durumu bildirimlerine etkinlik günlüğü uyarılar alırsınız](monitoring-activity-log-alerts-on-service-notifications.md).


## <a name="whats-new-in-alerts-for-activity-logs"></a>Etkinlik günlükleri için uyarılar yenilikler nelerdir?

[Azure uyarıları](monitoring-overview-unified-alerts.md) şimdi etkinlik günlüğü uyarıları için geliştirilmiş kullanıcı deneyimi sağlar. İle [uyarılar için kullanıcı deneyimi Gelişmiş](monitoring-overview-unified-alerts.md), şunları yapabilirsiniz:

- [Oluşturma](#create-an-alert-rule-for-an-activity-log) ve [yönetmek](#view-and-manage-activity-log-alert-rules) etkinlik uyarı kuralları, gelen oturum **İzleyici** > **uyarıları** dikey. Daha fazla bilgi edinmek [etkinlik günlükleri](monitoring-overview-activity-logs.md).

- **Uyarıları hedef için yeni seçenekler**: yeni bir etkinlik günlüğü uyarı kuralı oluşturulurken, artık bir hedef kaynağa veya bir kaynak grubu veya bir aboneliği seçebilirsiniz.


## <a name="create-an-alert-rule-for-an-activity-log"></a>Bir etkinlik günlüğü için bir uyarı kuralı oluştur

> [!NOTE]

>  Uyarı kuralları oluştururken aşağıdakilerden emin olun:

> - Uyarının oluşturulduğu özelliği abonelik kapsamında abonelikten farklı değildir.
- Ölçüt düzeyi/durum olmalıdır/çağıran / kaynak grubu / kaynak kimliği / kaynak türü / uyarı yapılandırılmış olay kategorisi.
- "Herhangi" koşul veya iç içe geçmiş koşullar JSON uyarı yapılandırmasında yoktur (temel olarak, yalnızca bir tümü başka hiçbir tümü/herhangi izin).


Aşağıdaki yordamı kullanın:

1. Azure portalından seçin **İzleyici** > **uyarıları**
2. Tıklatın **yeni uyarı kuralı** en üstündeki **uyarıları** penceresi.

     ![Yeni uyarı kuralı](./media/monitoring-activity-log-alerts-new-experience/create-new-alert-rule.png)

     **Oluşturma kuralı** penceresi görüntülenir.

      ![Yeni uyarı kuralı seçenekleri](./media/monitoring-activity-log-alerts-new-experience/create-new-alert-rule-options.png)

3. **Tanımlamak Uyarı koşulu altında** aşağıdaki bilgileri sağlayın ve tıklayın **Bitti**.

    - **Uyarı hedefi:** görüntülemek ve yeni uyarı için hedef seçmek için **filtresi abonelik tarafından** / **kaynak türüne göre filtre** ve kaynak veya kaynak grubu seçin liste görüntülenir.

    > [!NOTE]

    > bir kaynak, kaynak grubu veya tüm bir aboneliği seçebilirsiniz.

    **Uyarı hedef örnek görünümü**

     ![Hedef seçin](./media/monitoring-activity-log-alerts-new-experience/select-target.png)

    - Altında **hedef ölçütleri**, tıklatın **ölçüt eklemek** sinyal türü olarak seçin **etkinlik günlüğü**.

    - Sinyal görüntülenen listeden seçin.

    Günlük geçmişi zaman çizelgesi ve karşılık gelen bir uyarı mantık bu hedef sinyal seçebilirsiniz:

    **Ölçüt ekranına ekleyin**

    ![Ölçüt Ekle](./media/monitoring-activity-log-alerts-new-experience/add-criteria.png)

    **Geçmiş zaman**: Son 6/12/24 saatin, geçen hafta içinde.

    **Uyarı mantığı**:

     - **Olay düzeyi**-olay önem derecesi. **Ayrıntılı, bilgi, uyarı, hata**, veya **kritik**.
     - **Durum**: olay durumu. **Başlatıldı, başarısız**, veya **başarılı**.
     - **Olayı başlatan tarafından**: çağıran; da bilinir E-posta adresi veya işlemi gerçekleştiren kullanıcının Azure Active Directory tanıtıcısı.

        **Örnek sinyal grafik uygulanan uyarı mantığı ile** :

        ![ Seçilen ölçütü](./media/monitoring-activity-log-alerts-new-experience/criteria-selected.png)

4. Altında **uyarı kuralları ayrıntılarını tanımlayın**, şu bilgileri sağlayın:

    - **Uyarı kuralı adı** – yeni bir uyarı kuralı adı
    - **Açıklama** – yeni uyarı kuralı için açıklama
    - **Kaynak grubu için uyarı Kaydet** –, Yeni kuralın kaydetmek istediğiniz kaynak grubunu seçin.

5. Altında **eylem grubu**, açılır menüden, bu yeni bir uyarı kuralı atamak istediğiniz eylem grubunu belirtin. Alternatif olarak, [yeni bir eylem grubu oluşturmak](monitoring-action-groups.md) ve yeni kural atayın. Yeni bir grup oluşturmak için tıklatın **+ yeni grup**.

6. Oluşturduktan sonra kuralları etkinleştirmek için **Evet** için **etkinleştir kural oluşturulduktan sonra** seçeneği.
7. Tıklatın **oluşturma uyarı kuralı**.

    Etkinlik günlüğü için yeni uyarı kuralı oluşturulur ve bir onay iletisi en üstünde görünür penceresinin sağ.

    ![ oluşturulan uyarı kuralı](./media/monitoring-activity-log-alerts-new-experience/alert-created.png)

    Etkinleştirebilir, devre dışı bırakmak, düzenlemek veya kural silme. [Daha fazla bilgi edinin](#view-and-manage-activity-log-alert-rules) etkinlik günlüğü kurallarını yönetme hakkında.

## <a name="view-and-manage-activity-log-alert-rules"></a>Görüntüleme ve etkinlik günlüğü uyarı kurallarını yönetme

1. Azure portalından tıklatın **İzleyici** > **uyarıları** tıklatıp **yönetmek kuralları** en pencerenin sol üst.

    ![ Uyarı kurallarını yönet](./media/monitoring-activity-log-alerts-new-experience/manage-alert-rules.png)

    Kullanılabilir kurallar listesi görüntülenir.

2. Etkinlik günlüğü kuralı değiştirmek için arama yapın.

    ![ Etkinlik günlüğü uyarı kurallarında Ara](./media/monitoring-activity-log-alerts-new-experience/searth-activity-log-rule-to-edit.png)

    Kullanılabilir filtreleri - kullanabilirsiniz **abonelik**, **kaynak grubu**, veya **kaynak** düzenlemek istediğiniz etkinliği kural bulunamadı.

    > [!NOTE]

    > Yalnızca düzenleyebilirsiniz **açıklama** , **hedef ölçütleri** ve **Eylem grupları**.

3.  Kuralı seçin ve kuralı seçeneklerini düzenlemek için çift tıklayın. Gerekli değişiklikleri yapın ve ardından **kaydetmek**.

    ![ Uyarı kurallarını yönet](./media/monitoring-activity-log-alerts-new-experience/activity-log-rule-edit-page.png)

4.  Devre dışı bırakma, etkinleştirme veya kural silme. 2. adımda ayrıntılı olarak kural seçtikten sonra pencerenin üstündeki uygun seçeneği belirleyin.


## <a name="next-steps"></a>Sonraki adımlar

- [Etkinlik günlüğü uyarıları arşivle](monitoring-archive-activity-log.md)
- [Olay hub'ları akış etkinlik günlükleri](monitoring-stream-activity-logs-event-hubs.md)
- [Etkinlik günlükleri geçirme](monitoring-migrate-management-alerts.md)
