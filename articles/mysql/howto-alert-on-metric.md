---
title: Ölçümleri uyarıları Azure veritabanı için MySQL için Azure portalında yapılandırın
description: Bu makalede nasıl yapılandırılacağı ve erişim ölçüm uyarıları Azure veritabanı için MySQL için Azure portalından açıklanmaktadır.
services: mysql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 3accc31f433e6db40c7d1de2b56dfbd4180b4933
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35265198"
---
# <a name="use-the-azure-portal-to-set-up-alerts-on-metrics-for-azure-database-for-mysql"></a>Azure portal ölçümleri uyarılar Azure veritabanı için MySQL için ayarlamak için kullanın 

Bu makalede Azure veritabanı için Azure portalını kullanarak MySQL uyarıları ayarlamak nasıl gösterir. Azure hizmetlerinizi ölçümleri izleme dayalı bir uyarı alabilirsiniz.

Belirtilen bir ölçüm değerini atadığınız bir eşik kestiği olduğunda uyarı tetikler. Uyarı Tetikleyicileri her iki koşulu ilk olduğunda sınamadan ve ardından daha sonra ne zaman, koşul artık karşılanıp. 

Tetikler aşağıdaki işlemleri yapmak için bir uyarı yapılandırabilirsiniz:
* Hizmet yöneticisini ve ortak Yöneticiler e-posta bildirimleri gönder
* Belirttiğiniz ek e-postalar için e-posta gönderin.
* Web kancası çağırma

Yapılandırın ve uyarı kuralları kullanma hakkında bilgi alın:
* [Azure portal](../monitoring-and-diagnostics/insights-alerts-portal.md)
* [PowerShell](../monitoring-and-diagnostics/insights-alerts-powershell.md)
* [Komut satırı arabirimi (CLI)](../monitoring-and-diagnostics/insights-alerts-command-line-interface.md)
* [Azure monitör REST API'si](https://msdn.microsoft.com/library/azure/dn931945.aspx)

## <a name="create-an-alert-rule-on-a-metric-from-the-azure-portal"></a>Ölçüm Azure portalından bir uyarı kuralı oluşturma
1. İçinde [Azure portal](https://portal.azure.com/), izlemek istediğiniz MySQL sunucusu için Azure veritabanı'nı seçin.

2. Altında **izleme** kenar, select bölümünü **uyarı kuralları** gösterildiği gibi:

   ![Uyarı kurallarını seçin](./media/howto-alert-on-metric/1-alert-rules.png)

3. Seçin **ölçüm uyarı Ekle** (+ simgesi). 

4. **Ekle kuralı** sayfası, aşağıda gösterildiği gibi açılır.  Gerekli bilgileri doldurun:

   ![Ölçüm uyarı form ekleme](./media/howto-alert-on-metric/2-add-rule-form.png)

   | Ayar | Açıklama  |
   |---------|---------|
   | Ad | Uyarı kuralı için bir ad sağlayın. Bu değer uyarı bildirim e-postayla gönderilir. |
   | Açıklama | Uyarı kuralı kısa bir açıklamasını sağlar. Bu değer uyarı bildirim e-postayla gönderilir. |
   | Uyarı verilecek olay | Seçin **ölçümleri** bu tür bir uyarı için. |
   | Abonelik | Bu alan için MySQL Azure veritabanınızı barındıran abonelik ile önceden doldurulmaz. |
   | Kaynak grubu | Bu alan, Azure veritabanınızın kaynak grubuyla MySQL için önceden doldurulmaz. |
   | Kaynak | Bu alan, Azure veritabanı adıyla MySQL için önceden doldurulmaz. |
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
