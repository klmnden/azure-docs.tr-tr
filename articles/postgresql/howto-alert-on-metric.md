---
title: "Ölçümleri uyarılar için PostgreSQL Azure veritabanı için Azure portalında yapılandırın. | Microsoft Docs"
description: "Bu makalede nasıl yapılandırılacağı ve erişim ölçüm uyarıları Azure veritabanı için PostgreSQL için Azure portalından açıklanmaktadır."
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 10/24/2017
ms.openlocfilehash: 3a09be8131b57381eb470027a134109c116467ed
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2017
---
# <a name="use-the-azure-portal-to-set-up-alerts-on-metrics-for-azure-database-for-postgresql"></a>Azure portal ölçümleri uyarılar için Azure veritabanı için PostgreSQL ayarlamak için kullanın 

Bu makalede Azure veritabanı için Azure portalını kullanarak PostgreSQL uyarıları ayarlamak nasıl gösterir. Azure hizmetlerinizi ölçümleri izleme dayalı bir uyarı alabilirsiniz.

Belirtilen bir ölçüm değerini atadığınız bir eşik kestiği olduğunda uyarı tetikler. Uyarı Tetikleyicileri her iki koşulu ilk olduğunda sınamadan ve ardından daha sonra ne zaman, koşul artık karşılanıp. 

Tetikler aşağıdaki işlemleri yapmak için bir uyarı yapılandırabilirsiniz:
* Hizmet yöneticisini ve ortak Yöneticiler e-posta bildirimleri gönderin.
* Belirttiğiniz ek e-postalar için e-posta gönderin.
* Bir Web kancası çağırın.

Yapılandırın ve uyarı kuralları kullanma hakkında bilgi alın:
* [Azure portal](../monitoring-and-diagnostics/insights-alerts-portal.md)
* [PowerShell](../monitoring-and-diagnostics/insights-alerts-powershell.md)
* [Komut satırı arabirimi (CLI)](../monitoring-and-diagnostics/insights-alerts-command-line-interface.md)
* [Azure monitör REST API'si](https://msdn.microsoft.com/library/azure/dn931945.aspx)

## <a name="create-an-alert-rule-on-a-metric-from-the-azure-portal"></a>Ölçüm Azure portalından bir uyarı kuralı oluşturma
1. İçinde [Azure portal](https://portal.azure.com/), PostgreSQL sunucu izlemek istediğiniz Azure veritabanı'nı seçin.

2. Altında **izleme** kenar, select bölümünü **uyarı kuralları** gösterildiği gibi:

   ![Uyarı kurallarını seçin](./media/howto-alert-on-metric/1-alert-rules.png)

3. Seçin **ölçüm uyarı Ekle** (+ simgesi). 

4. **Ekle kuralı** sayfası, aşağıda gösterildiği gibi açılır.  Gerekli bilgileri doldurun:

   ![Ölçüm uyarı form ekleme](./media/howto-alert-on-metric/2-add-rule-form.png)

   | Ayar | Açıklama  |
   |---------|---------|
   | Ad | Uyarı kuralı için bir ad sağlayın. Bu değer uyarı bildirim e-postayla gönderilir. |
   | Açıklama | Uyarı kuralı kısa bir açıklamasını sağlar. Bu değer uyarı bildirim e-postayla gönderilir. |
   | Uyar | Seçin **ölçümleri** bu tür bir uyarı için. |
   | Abonelik | Bu alan için PostgreSQL Azure veritabanınızı barındıran abonelik ile önceden doldurulmaz. |
   | Kaynak grubu | Bu alan, Azure veritabanınızın kaynak grubuyla PostgreSQL için önceden doldurulmaz. |
   | Kaynak | Bu alan Azure veritabanınızın adı ile PostgreSQL için önceden doldurulmaz. |
   | Ölçüm | İçin bir uyarı vermek istediğiniz ölçümü seçin. Örneğin, **depolama yüzdesi**. |
   | Koşul | Koşul ile Karşılaştırılacak ölçüm için seçin. Örneğin, **büyük**. |
   | Eşik | Eşik değeri (yüzde) ölçümü, örneğin 85 için. |
   | Dönem | Ölçüm kuralı uyarı Tetikleyicileri önce karşılanması gereken süreyi. Örneğin, **son 30 dakikadan**. |

   Örneği temel alarak, uyarı % 85 üzerinde depolama yüzde 30 dakikalık bir süre içinde arar. Ortalama depolama yüzde 30 dakika boyunca % 85 kaldırıldığında bu uyarı tetikler. İlk tetikleyici oluşur sonra ortalama depolama yüzde 30 dakika boyunca % 85 olduğunda yeniden tetikler.

5. İçin uyarı kuralı istediğiniz bildirim yöntemini seçin. 

   Denetleme **sahipleri, Katkıda Bulunanlar ve okuyucular e-posta** abonelik yöneticileri ve ortak Yöneticiler uyarı oluşturulduğunda e-posta gönderilip istiyorsanız seçeneği.

   Uyarı oluşturulduğunda bir bildirim almak için ek e-postaları istiyorsanız, bunları ekleyin **ek yönetici email(s)** alan. Birden çok e-postaları - noktalı virgülle ayırın  *email@contoso.com;email2@contoso.com*

   İsteğe bağlı olarak, geçerli bir URI sağlayın **Web kancası** adlı uyarı oluşturulduğunda istiyorsanız alan.

6. Seçin **Tamam** uyarı oluşturmak için.

   Birkaç dakika içinde uyarı etkindir ve daha önce açıklandığı şekilde tetikler.

## <a name="manage-your-alerts"></a>Uyarılarınızı yönetme
Bir uyarı oluşturduktan sonra onu seçin ve aşağıdaki eylemleri gerçekleştirebilirsiniz:

* Ölçüm eşiği ve önceki gün gerçek değerleri Bu uyarıyla ilgili gösteren bir grafik görüntüleyin.
* **Düzen** veya **silmek** uyarı kuralı.
* **Devre dışı** veya **etkinleştirmek** geçici olarak durdurmak veya bildirimleri almaya devam etmek istiyorsanız, uyarı.

## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla bilgi edinmek [Web kancalarını uyarıları yapılandırma](../monitoring-and-diagnostics/insights-webhooks-alerts.md).
* Alma bir [ölçümleri toplama genel bakış](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md) hizmetinizi kullanılabilir ve yanıt verebilir durumda olduğundan emin olmak için.
