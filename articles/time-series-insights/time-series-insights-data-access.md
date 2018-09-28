---
title: Erişim ve Azure Time Series Insights'ı yönetmek için güvenlik yapılandırma | Microsoft Docs
description: Bu makalede güvenlik ve izinler yönetim erişimi yapılandırma ilkeleri ve veri erişim ilkeleri Azure Time Series Insights güvenliğini sağlamak için.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.workload: big-data
ms.topic: conceptual
ms.date: 11/15/2017
ms.openlocfilehash: c9bddf8e20524433b31793e277efd954a5d1320e
ms.sourcegitcommit: 42405ab963df3101ee2a9b26e54240ffa689f140
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/28/2018
ms.locfileid: "47423383"
---
# <a name="grant-data-access-to-a-time-series-insights-environment-using-azure-portal"></a>Azure Portal’ı kullanarak Zaman Serisi Görüşleri ortamına veri erişimi verme

Bu makalede, iki tür Time Series Insights erişim ilkeleri açıklanır.

## <a name="video"></a>Video: 

### <a name="in-this-video-we-cover-creating-and-managing-access-policies-within-time-series-insights-br"></a>Bu videoda, Time Series Insights içinde erişim ilke oluşturma ve yönetme kapsar. </br>

> [!VIDEO https://www.youtube.com/embed/5zTIdyHMJW8]

Zaman Serisi Görüşleri ortamlarının birbirinden bağımsız türde iki erişim ilkesi vardır:

* Yönetim erişimi ilkeleri
* Veri erişimi ilkeleri

Her iki ilke de Azure Active Directory asıl adlarına (kullanıcılar ve uygulamalar) belirli bir ortam üzerinde çeşitli izinler verir. Ortamı içeren abonelikle ilişkili (Azure kiracısı bilinir) active Directory asıl adlarına (kullanıcılar ve uygulamalar) ait olmalıdır.

Yönetim erişimi ilkeleri, ortamı, olay kaynaklarını, başvuru veri kümelerini oluşturma ve
*   Silme, veri erişimi ilkelerini yönetme gibi ortamın yapılandırmasıyla ilgili izinler
*   verir.

Veri erişimi ilkeleri, veri sorguları gönderme, ortamdaki başvuru verilerini işleme, ortamla ilişkilendirilmiş kaydedilen sorguları ve perspektifleri paylaşma izinleri verir.

İki ilke türü de ortamın yönetimine erişim ile ortam içindeki verilere erişim arasında net bir ayrım yapmaya olanak tanır. Örneğin, ortamın sahibinin/oluşturucusunun veri erişiminden kaldırılır, bir ortamı ayarlamak mümkündür. Ayrıca, kullanıcılar ve ortamdan veri okumak için izin verilen hizmetler ortamının yapılandırması erişim verilebilir.

## <a name="grant-data-access"></a>Veri erişim izni verme
Bir kullanıcı asıl veri erişimi vermek için aşağıdaki adımları izleyin:

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Time Series Insights ortamınızı bulun. Tür **zaman serisi** içinde **arama** kutusu. Seçin **zaman serisi ortamı** arama sonuçlarında. 

3. Listeden Zaman Serisi Görüşleri ortamınızı seçin.
   
4. Seçin **veri erişimi ilkeleri**, ardından **+ Ekle**.
  ![Zaman serisi görüşleri kaynağını yönetme - ortam](media/data-access/getstarted-grant-data-access1.png)

5. Seçin **Kullanıcı Seç**.  Eklemek istediğiniz kullanıcı bulmak kullanıcı adı veya e-posta adresi arayın. Tıklayın **seçin** Seçimi onaylamak için. 

   ![Zaman Serisi Görüşleri kaynağını yönetme - ekleme](media/data-access/getstarted-grant-data-access2.png)

6. Seçin **rol seçme**. Kullanıcı için uygun erişim rolü seçin:
   - Seçin **katkıda bulunan** başvuru verileri ve kaydedilmiş paylaşımı sorguları ve Perspektifleri ortamın diğer kullanıcılarla değiştirmek kullanıcı izin vermek istiyorsanız. 
   - Aksi takdirde seçin **okuyucu** ortamında kullanıcı veri izin verme veya kişisel (paylaşılmayan) sorguları ortama kaydetmesine.

   Seçin **Tamam** rolü seçiminizi onaylamak için.

   ![Zaman Serisi Görüşleri kaynağını yönetme - kullanıcı seçme](media/data-access/getstarted-grant-data-access3.png)

8. Seçin **Tamam** içinde **kullanıcı rolü Seç** sayfası.

   ![Zaman Serisi Görüşleri kaynağını yönetme - rol seçme](media/data-access/getstarted-grant-data-access4.png)

9. **Veri erişimi ilkeleri** sayfası, kullanıcıları ve her kullanıcı için rolleri listeler.

   ![Zaman Serisi Görüşleri kaynağını yönetme - sonuçlar](media/data-access/getstarted-grant-data-access5.png)

## <a name="provide-guest-access-to-a-user-from-another-aad-tenant"></a>Konuk erişimi bir kullanıcı başka bir AAD kiracısı sağlar.

"Konuk" Yönetim rolü değil; Bu, bir kiracıdan diğerine davet bir hesap için kullanılan bir terimdir. Konuk hesabı kiracının dizine davet edildi sonra aynı erişim denetimi gibi başka bir hesap, erişim denetimi (IAM) dikey penceresini kullanarak bir TSI ortamına yönetim erişimi vermek veya verilere erişim uygulanmış olabilir Veri erişimi ilkeleri dikey penceresi aracılığıyla ortam. AAD Kiracı Konuk erişimi hakkında daha fazla bilgi için bkz. Bu [belge](https://docs.microsoft.com/azure/active-directory/b2b/add-users-administrator).

Zaman serisi görüşleri ortamına bir AAD kullanıcısı için başka bir kiracıdaki Konuk erişimi vermek için aşağıdaki adımları izleyin:

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Time Series Insights ortamınızı bulun. Tür **zaman serisi** içinde **arama** kutusu. Seçin **zaman serisi ortamı** arama sonuçlarında.

3. Listeden Zaman Serisi Görüşleri ortamınızı seçin.

4. Seçin **veri erişimi ilkeleri**, ardından + **davet**.

    ![Zaman serisi görüşleri kaynağını yönetme - kullanıcı davet et](media/data-access/getstarted-grant-data-access6.png)

5. Davet etmek istediğiniz kullanıcının e-posta sağlayın. Bu, AAD ile ilişkili bir e-posta olması unutmayın. İsteğe bağlı olarak davete kişisel bir ileti içerebilir.

    ![Zaman Serisi Görüşleri kaynağını yönetme - kullanıcı seçme](media/data-access/getstarted-grant-data-access7.png)

6. Ekrandaki bir onay Kabarcık görmeniz gerekir.

    ![Zaman serisi görüşleri kaynağını yönetme - kullanıcı onaylayın](media/data-access/getstarted-grant-data-access8.png)

7. Seçin **Kullanıcı Seç**. Eklemek istediğiniz kullanıcı bulmak için yalnızca davet Konuk kullanıcı e-posta adresini arayın. Tıklayın **seçin** Seçimi onaylamak için.
  
    ![Zaman serisi görüşleri kaynağını yönetme - kullanıcı onaylayın](media/data-access/getstarted-grant-data-access9.png)

8. Seçin **rol seçme**. Konuk kullanıcı için uygun erişim rolü seçin:

    * Seçin **katkıda bulunan** kullanıcıyı başvuru verileri ve kaydedilmiş paylaşımı sorguları ve Perspektifleri ortamın diğer kullanıcılarla değiştirmeye izin vermek istiyorsanız.

    * Aksi takdirde seçin **okuyucu** ortamında kullanıcı veri izin verme veya kişisel (paylaşılmayan) sorguları ortama kaydetmesine.

    Seçin **Tamam** rolü seçiminizi onaylamak için.

    ![Zaman Serisi Görüşleri kaynağını yönetme - rol seçme](media/data-access/getstarted-grant-data-access10.png)

9. Seçin **Tamam** içinde **kullanıcı rolü Seç** sayfası.

10. **Veri erişimi ilkeleri** sayfası artık Konuk kullanıcı ve her bir Konuk kullanıcı için rolleri listeler.

    ![Zaman serisi görüşleri kaynağını yönetme - rol onaylayın](media/data-access/getstarted-grant-data-access11.png)

11. Konuk kullanıcı, Azure'da yer alan ortama erişmek için adımlar, yalnızca Kiracı belirli bir işlem yapmanız gerekir artık bunları davet. İlk olarak, yalnızca onlara gönderdiğiniz kendisinden daveti kabul etmesi gerekir. Bu davet, adım 5'te davet e-posta adresine e-posta ile gönderilir. 'Kabul edecek şekilde kullanmaya başlayın,' tıklatmanız gerekir.

    ![Zaman serisi görüşleri kaynağını yönetme - kullanıcı davet et](media/data-access/getstarted-grant-data-access12.png)

12. Ardından, Konuk kullanıcı, yöneticinin kuruluşla ilişkili izinlerini kabul etmenizi gerekir.

    ![Zaman serisi görüşleri kaynağını yönetme - izinleri kabul etmiş olursunuz](media/data-access/getstarted-grant-data-access13.png)

13. Sizi davet e-posta adresine Konuk kullanıcı oturum açtığında ve daveti kabul, insights.azure.com için head. Bir kez, bunlar yanındaki kendi e-posta ekranın sağ alt köşesindeki avatar seçeneğine tıklamanız gerekir. 

    ![Zaman serisi görüşleri kaynağını yönetme - izinleri kabul etmiş olursunuz](media/data-access/getstarted-grant-data-access14.png)

14. Konuk kullanıcı Azure seçip ardından, dizin aşağı açılan menüden Kiracı. Bunları davet Kiracı budur. 

    ![Zaman serisi görüşleri kaynağını yönetme - izinleri kabul etmiş olursunuz](media/data-access/getstarted-grant-data-access15.png)

15. Son olarak, Konuk kullanıcı kiracınızın seçtiğinde, yalnızca bunlara erişimi sağlanan zaman serisi görüşleri ortamı görürsünüz. Artık tüm özellikleri adım 8'deki sağlanan rol ile ilişkili olması gerekir.

## <a name="next-steps"></a>Sonraki adımlar
* Bilgi [Azure zaman serisi görüşleri ortamınıza bir Event Hub olay kaynağı ekleme](time-series-insights-how-to-add-an-event-source-eventhub.md).
* [Olayları gönderme](time-series-insights-send-events.md) olay kaynağına.
* Ortamınızı görüntüleyebilirsiniz [Time Series Insights gezgininin](https://insights.timeseries.azure.com).
