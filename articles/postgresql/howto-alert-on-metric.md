---
title: Ölçüm uyarıları için Azure veritabanı PostgreSQL için Azure portalında yapılandırma
description: Bu makalede nasıl yapılandırılacağını ve ölçüm uyarıları erişim için Azure veritabanı, PostgreSQL için Azure portalından açıklanır.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 02/28/2018
ms.openlocfilehash: 26b7e92bf8fa6c42320f604643bc996794ed52ca
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/17/2018
ms.locfileid: "53540733"
---
# <a name="use-the-azure-portal-to-set-up-alerts-on-metrics-for-azure-database-for-postgresql"></a>Ölçümler ile ilgili uyarılar için Azure veritabanı için PostgreSQL ayarlamak için Azure portalını kullanma 

Bu makalede, Azure portalını kullanarak PostgreSQL uyarılar için Azure veritabanı ayarlama işlemini göstermektedir. Azure hizmetleriniz için ölçümleri izleme temel alan bir uyarı alabilirsiniz.

Belirtilen bir ölçüm değerini atadığınız eşiği aştığında uyarı tetiklenir. Uyarı tetiklenmeden her iki koşul ilk olduğunda karşılanması ve ardından daha sonra ne zaman koşulu artık karşılanmıyor. 

Bir uyarı tetiklendiğinde aşağıdaki işlemleri yapmak için yapılandırabilirsiniz:
* Hizmet Yöneticisi ve ortak yöneticilerine e-posta bildirimleri gönderin.
* Belirttiğiniz ek e-postalar için e-posta gönderin.
* Web kancası çağırma.

Yapılandırın ve uyarı kuralları kullanma hakkında bilgi edinin:
* [Azure portal](../monitoring-and-diagnostics/insights-alerts-portal.md)
* [PowerShell](../azure-monitor/platform/alerts-classic-portal.md)
* [Komut satırı arabirimi (CLI)](../azure-monitor/platform/alerts-classic-portal.md)
* [Azure İzleyici REST API](https://msdn.microsoft.com/library/azure/dn931945.aspx)

## <a name="create-an-alert-rule-on-a-metric-from-the-azure-portal"></a>Azure portalından bir ölçüm üzerinde uyarı kuralı oluşturma
1. İçinde [Azure portalında](https://portal.azure.com/), izlemek istediğiniz PostgreSQL sunucusu için Azure veritabanı'nı seçin.

2. Altında **izleme** select yan bölümünü **uyarı kuralları** gösterildiği gibi:

   ![Uyarı kuralları seçin](./media/howto-alert-on-metric/1-alert-rules.png)

3. Seçin **ölçüm uyarısı Ekle** (+ simgesi). 

4. **Kuralı Ekle** sayfası aşağıda gösterildiği gibi açılır.  Gerekli bilgileri doldurun:

   ![Ölçüm uyarı formu Ekle](./media/howto-alert-on-metric/2-add-rule-form.png)

   | Ayar | Açıklama  |
   |---------|---------|
   | Ad | Uyarı kuralı için bir ad belirtin. Bu değer uyarı bildirim e-postayla gönderilir. |
   | Açıklama | Uyarı kuralı kısa bir açıklamasını sağlayın. Bu değer uyarı bildirim e-postayla gönderilir. |
   | Uyarı verilecek olay | Seçin **ölçümleri** bu tür bir uyarı için. |
   | Abonelik | Bu alan, PostgreSQL için Azure veritabanınızı barındıran abonelik ile doldurulur. |
   | Kaynak grubu | Bu alan, PostgreSQL için Azure veritabanı ile kaynak grubu doldurulur. |
   | Kaynak | Bu alan, PostgreSQL için Azure veritabanı adıyla doldurulur. |
   | Ölçüm | Uyarı vermek istediğiniz ölçümü seçin. Örneğin, **depolama yüzdesi**. |
   | Koşul | İle Karşılaştırılacak ölçüm için koşul seçin. Örneğin, **büyüktür**. |
   | Eşik | Ölçüm için eşik değeri, örneğin 85 (yüzde). |
   | Dönem | Uyarı tetiklenmeden önce ölçüm kuralının karşılanması gereken sürede. Örneğin, **son 30 dakikadan**. |

   Örneği temel alarak, uyarı % 85 üzerinde depolama yüzde 30 dakikalık dönem boyunca arar. Ortalama depolama yüzde %85 30 dakika boyunca olduğunda bu uyarı tetikler. İlk tetikleyici gerçekleşir sonra ortalama depolama yüzdesi %85 30 dakika içinde olduğunda tekrar tetikler.

5. Uyarı kuralı için istediğiniz bildirim yöntemini seçin. 

   Denetleme **e-posta sahipleri, Katkıda Bulunanlar ve okuyucular** abonelik yöneticilerine ve ortak yöneticilerin uyarı tetiklendiğinde almayacağınızı istiyorsanız seçeneği.

   Uyarı tetiklendiğinde bildirim alacak başka e-postalar istiyorsanız, bunları eklemek **ek yönetici email(s)** alan. Noktalı virgülle - birden çok e-postaları ayırın  *email@contoso.com;email2@contoso.com*

   İsteğe bağlı olarak, geçerli bir URI sağlayın **Web kancası** adlı bir uyarı tetiklendiğinde istiyorsanız alan.

6. Seçin **Tamam** uyarı oluşturmak için.

   Birkaç dakika içinde uyarı etkin ve daha önce açıklandığı gibi tetikler.

## <a name="manage-your-alerts"></a>Uyarılarınızı yönetme
Bir uyarı oluşturulduktan sonra seçin ve aşağıdaki eylemleri gerçekleştirebilirsiniz:

* Ölçüm eşiği ve gerçek değerler önceki günden itibaren Bu uyarıyla ilgili gösteren bir grafiği görüntüleyin.
* **Düzen** veya **Sil** uyarı kuralı.
* **Devre dışı** veya **etkinleştirme** geçici olarak durdurmak veya bildirimleri almaya devam etmek istiyorsanız uyarı.

## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla bilgi edinin [uyarıları Web kancalarını yapılandırma](../azure-monitor/platform/alerts-webhooks.md).
* Alma bir [ölçümleri koleksiyonun genel bakış](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md) hizmetinizin kullanılabilir ve yanıt verdiğinden emin olmak için.
