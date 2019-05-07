---
title: PostgreSQL - Azure portalında tek bir sunucu için Azure veritabanı için ölçüm uyarıları yapılandırma
description: Bu makalede nasıl yapılandırılacağını ve erişim ölçüm uyarıları, PostgreSQL - Azure portalında tek bir sunucu için için Azure veritabanı açıklanır.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 000dfe2d3e594c71f9c7ebbff7bce7141243668a
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65067327"
---
# <a name="use-the-azure-portal-to-set-up-alerts-on-metrics-for-azure-database-for-postgresql---single-server"></a>Ölçümler ile ilgili uyarılar için Azure veritabanı PostgreSQL - tek bir sunucu ayarlamak için Azure portalını kullanma

Bu makalede, Azure portalını kullanarak PostgreSQL uyarılar için Azure veritabanı ayarlama işlemini göstermektedir. Azure hizmetleriniz için ölçümleri izleme temel alan bir uyarı alabilirsiniz.

Belirtilen bir ölçüm değerini atadığınız eşiği aştığında uyarı tetiklenir. Uyarı tetiklenmeden her iki koşul ilk olduğunda karşılanması ve ardından daha sonra ne zaman koşulu artık karşılanmıyor. 

Bir uyarı tetiklendiğinde aşağıdaki işlemleri yapmak için yapılandırabilirsiniz:
* Hizmet Yöneticisi ve ortak yöneticilerine e-posta bildirimleri gönderin.
* Belirttiğiniz ek e-postalar için e-posta gönderin.
* Web kancası çağırma.

Yapılandırın ve uyarı kuralları kullanma hakkında bilgi edinin:
* [Azure portal](../azure-monitor/platform/alerts-metric.md#create-with-azure-portal)
* [Azure CLI](../azure-monitor/platform/alerts-metric.md#with-azure-cli)
* [Azure İzleyici REST API](https://docs.microsoft.com/rest/api/monitor/metricalerts)

## <a name="create-an-alert-rule-on-a-metric-from-the-azure-portal"></a>Azure portalından bir ölçüm üzerinde uyarı kuralı oluşturma
1. İçinde [Azure portalında](https://portal.azure.com/), izlemek istediğiniz PostgreSQL sunucusu için Azure veritabanı'nı seçin.

2. Altında **izleme** select yan bölümünü **uyarılar** gösterildiği gibi:

   ![Uyarı kuralları seçin](./media/howto-alert-on-metric/2-alert-rules.png)

3. Seçin **ölçüm uyarısı Ekle** (+ simgesi).

4. **Oluşturma kuralı** sayfası aşağıda gösterildiği gibi açılır. Gerekli bilgileri doldurun:

   ![Ölçüm uyarı formu Ekle](./media/howto-alert-on-metric/4-add-rule-form.png)

5. İçinde **koşul** bölümünden **koşul Ekle**.

6. Uyarı almak sinyalleri listesinden bir ölçüm seçin. Bu örnekte, "Depolama yüzdesi" seçin.
   
   ![Ölçüm seçin](./media/howto-alert-on-metric/6-configure-signal-logic.png)

7. Uyarı mantığı dahil olmak üzere yapılandırma **koşul** (ör. "Büyüktür"), **eşiği** (ör. yüzde 85 '), **zaman toplama**, **süresi** süresini (ör. uyarı tetiklenmeden önce ölçüm kuralının sağlanmalıdır "Üzerinden, son 30 dakika"), ve **sıklığı**.
   
   Seçin **Bitti** tamamlandığında.

   ![Ölçüm seçin](./media/howto-alert-on-metric/7-set-threshold-time.png)

8. İçinde **Eylem grupları** bölümünden **Yeni Oluştur** Uyarı bildirimlerini alacak yeni bir grup oluşturmak için.

9. Ad, kısa adı, abonelik ve kaynak grubu "Eylem Grup Ekle" formunu doldurun.

10. Yapılandırma bir **e-posta/SMS/anında iletme/ses** eylem türü.
    
    "E-posta Azure Resource Manager abonelik sahipleri, Katkıda Bulunanlar ve okuyucular bildirimleri almak için seçmek için rolü" seçin.
   
    İsteğe bağlı olarak, geçerli bir URI sağlayın **Web kancası** adlı bir uyarı tetiklendiğinde istiyorsanız alan.

    Seçin **Tamam** tamamlandığında.

    ![Eylem grubu](./media/howto-alert-on-metric/10-action-group-type.png)

11. Bir uyarı kuralı adını, açıklamasını ve önem derecesini belirtin.

    ![Eylem grubu](./media/howto-alert-on-metric/11-name-description-severity.png) 

12. Seçin **uyarı kuralı oluştur** uyarı oluşturmak için.

    Birkaç dakika içinde uyarı etkin ve daha önce açıklandığı gibi tetikler.

## <a name="manage-your-alerts"></a>Uyarılarınızı yönetme
Bir uyarı oluşturulduktan sonra seçin ve aşağıdaki eylemleri gerçekleştirebilirsiniz:

* Ölçüm eşiği ve gerçek değerler önceki günden itibaren Bu uyarıyla ilgili gösteren bir grafiği görüntüleyin.
* **Düzen** veya **Sil** uyarı kuralı.
* **Devre dışı** veya **etkinleştirme** geçici olarak durdurmak veya bildirimleri almaya devam etmek istiyorsanız uyarı.

## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla bilgi edinin [uyarıları Web kancalarını yapılandırma](../azure-monitor/platform/alerts-webhooks.md).
* Alma bir [ölçümleri koleksiyonun genel bakış](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md) hizmetinizin kullanılabilir ve yanıt verdiğinden emin olmak için.
