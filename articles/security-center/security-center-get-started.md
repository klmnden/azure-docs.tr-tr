---
title: Azure Güvenlik Merkezi Hızlı Başlangıç - Azure aboneliğinizi Güvenlik Merkezi Standart katmanına ekleme | Microsoft Docs
description: Bu hızlı başlangıçta, ek güvenlik kapsamı için Güvenlik Merkezinizin Standart fiyatlandırma katmanına nasıl yükseltme yapacağınız gösterilmektedir.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security-center
ms.devlang: na
ms.topic: quickstart
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/3/2018
ms.author: rkarlin
ms.openlocfilehash: 37f85afbdd55d3f14638f0833f69bb1992770449
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60908326"
---
# <a name="quickstart-onboard-your-azure-subscription-to-security-center-standard"></a>Hızlı Başlangıç: Yerleşik Azure aboneliğinizi Güvenlik Merkezi standart
Azure Güvenlik Merkezi, hibrit bulut iş yüklerinizde birleşik güvenlik yönetimi ve tehdit koruması sağlar. Ücretsiz katman yalnızca Azure kaynaklarınız için sınırlı güvenlik sunarken Standart katman bu özellikleri şirket içine ve diğer bulutlara genişletir. Güvenlik Merkezi Standart katmanı; güvenlik açıklarını bulup gidermenize, zararlı etkinlikleri engellemek için erişim ve uygulama denetimleri uygulamanıza, analizden ve bilgilerden yararlanarak tehditleri algılamanıza ve saldırı altındayken hızlıca yanıt vermenize yardımcı olur. Ücretsiz olarak Güvenlik Merkezi standart deneyebilirsiniz. Daha fazla bilgi için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/security-center/).

Bu makalede daha fazla güvenlik için Standart katmana yükseltecek, güvenlik açıklarını ve tehditleri izlemek için sanal makinelerinize Microsoft Monitoring Agent’ı yükleyeceksiniz.

## <a name="prerequisites"></a>Önkoşullar
Güvenlik Merkezi ile çalışmaya başlamak için Microsoft Azure aboneliğinizin olması gerekir. Bir aboneliğiniz yoksa [ücretsiz hesap](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.

Bir aboneliği Standart katmana yükseltebilmeniz için size Abonelik Sahibi, Abonelik Katkıda Bulunanı veya Güvenlik Yöneticisi rolünün atanması gerekir.

## <a name="enable-your-azure-subscription"></a>Azure aboneliğinizi etkinleştirme

1. [Azure portal](https://azure.microsoft.com/features/azure-portal/) oturum açın.
2. **Microsoft Azure** menüsünde **Güvenlik Merkezi**’ni seçin. **Güvenlik Merkezi - Genel Bakış** açılır.

   ![Güvenlik Merkezine genel bakış][2]

**Güvenlik Merkezi - Genel Bakış**, hibrit bulut iş yüklerinizin güvenlik durumuna ilişkin birleştirilmiş bir görünüm sağlayarak, iş yüklerinizin güvenliğini tespit edip değerlendirmenize ve riskleri tanımlayıp en aza indirmenize olanak tanır. Güvenlik Merkezi, sizin tarafınızdan veya başka bir abonelik kullanıcısı tarafından daha önce Ücretsiz katmana eklenmemiş olan Azure aboneliklerinizi otomatik olarak etkinleştirir.

**Abonelikler** menü öğesine tıklayarak aboneliklerin listesini görüntüleyebilir ve filtreleyebilirsiniz. Güvenlik Merkezi artık güvenlik açıklarını belirlemek için bu aboneliklerin güvenliğini değerlendirmeye başlayacaktır. Değerlendirme türlerini özelleştirmek için güvenlik ilkesinde değişiklik yapabilirsiniz. Güvenlik ilkesi iş yüklerinizin istenen yapılandırmasını tanımlar ve şirketin veya yasal düzenlemelerin gerektirdiği güvenlik gereksinimlerine uyum sağlanmasına yardımcı olur.

Güvenlik Merkezi’ni başlattıktan sonraki ilk birkaç dakika içinde şunları görebilirsiniz:

- Azure aboneliklerinizin güvenliğini artırmaya yönelik yöntemlere ilişkin **öneriler**. **Öneriler** kutucuğuna tıkladığınızda bir öncelik listesi açılır.
- Güvenlik Merkezi tarafından her birinin güvenlik durumuyla birlikte değerlendirilen **İşlem ve uygulamalar**, **Ağ İletişimi**, **Veri güvenliği**, **Kimlik ve erişim** kaynaklarının bir envanteri.

Güvenlik Merkezi’nden tam olarak yararlanmak için Standart katmana yükseltme yapmak ve Microsoft Monitoring Agent’ı yüklemek üzere aşağıdaki adımları tamamlamanız gerekir.

## <a name="upgrade-to-the-standard-tier"></a>Standart katmana yükseltme
Güvenlik Merkezi hızlı başlangıçlarının ve öğreticilerinin amacı doğrultusunda Standart katmana yükseltme yapmanız gerekir. Ücretsiz deneme Güvenlik Merkezi standart sürümüne yoktur. Daha fazla bilgi için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/security-center/). 

1. Güvenlik Merkezi ana menüsü altında, **Başlarken**’i seçin.
 
   ![başlarken][4]

2. **Yükselt** altında, Güvenlik Merkezi, ekleme işlemi için uygun abonelikleri ve çalışma alanlarını listeler. 
   - Tüm abonelikleri ve çalışma alanlarını deneme sürümü uygunluk durumlarıyla birlikte listelemek için, genişletilebilir **Deneme sürümünüzü uygulayın**’a tıklayabilirsiniz.
   -    Deneme sürümü için uygun olmayan abonelikler ve çalışma alanlarını yükseltebilirsiniz.
   -    Deneme sürümünüzü başlatmak için uygun çalışma alanlarını ve abonelikleri seçebilirsiniz.
3. Seçili aboneliklerde deneme sürümünüzü başlatmak için **Deneme sürümünü başlat**’a tıklayın.


  ![Güvenlik uyarıları][9]

## <a name="automate-data-collection"></a>Veri toplama işlemini otomatikleştirme
Güvenlik Merkezi, güvenlik açıklarını ve tehditleri izlemek için Azure VM’lerinizden ve Azure olmayan bilgisayarlarınızdan veri toplar. Veriler, makineden güvenlikle ilgili çeşitli yapılandırmaları ve olay günlüklerini okuyup verileri analiz için çalışma alanınıza kopyalayan Microsoft Monitoring Agent kullanılarak toplanır. Varsayılan olarak Güvenlik Merkezi sizin için yeni bir çalışma alanı oluşturur.

Otomatik sağlama etkinleştirildiğinde Güvenlik Merkezi, desteklenen tüm Azure VM’lere ve oluşturulan yeni VM’lere Microsoft Monitoring Agent’ı yükler. Otomatik sağlama önemle önerilir.

Microsoft Monitoring Agent için otomatik sağlamayı etkinleştirmek üzere:

1. Güvenlik Merkezi ana menüsünde **Güvenlik İlkesi**’ni seçin.
2. Abonelik satırında, **Ayarları düzenle>**’yi seçin.
3. **Veri Toplama** sekmesinde, **Otomatik sağlama**’yı **Açık** olarak ayarlayın.
4. **Kaydet**’i seçin.
****
  ![Otomatik sağlamayı etkinleştirme][6]

Azure VM’lerinize ilişkin bu yeni öngörüyle Güvenlik Merkezi, sistem güncelleştirme durumu, işletim sistemi güvenlik yapılandırmaları ve uç nokta koruması ile ilgili ek Öneriler sunmanın yanı sıra ek Güvenlik uyarıları oluşturabilir.

  ![Öneriler][8]

## <a name="clean-up-resources"></a>Kaynakları temizleme
Bu koleksiyondaki diğer hızlı başlangıçlar ve öğreticiler bu hızlı başlangıcı temel alır. Sonraki hızlı başlangıç ve öğreticilerle çalışmaya devam etmeyi planlıyorsanız Standart katmanını çalıştırmaya devam edin ve otomatik sağlamayı etkinleştirilmiş halde tutun. Devam etmeyi planlamıyorsanız veya Ücretsiz katmanına dönmek istiyorsanız:

1. Güvenlik Merkezi ana menüsüne dönüp **Güvenlik İlkesi**’ni seçin.
2. Ücretsize döndürmek istediğiniz aboneliğin satırında **Ayarları düzenleyin>**’yi seçin.
3. Aboneliği Standart katmanından Ücretsiz katmanına geçirmek için **Fiyatlandırma katmanı**’nı ve **Ücretsiz**’i seçin.
5. **Kaydet**’i seçin.

Otomatik sağlamayı devre dışı bırakmak istiyorsanız:

1. Güvenlik Merkezi ana menüsüne dönüp **Güvenlik ilkesi**’ni seçin.
2. Otomatik sağlamayı devre dışı bırakmak istediğiniz abonelik satırında, **Ayarları düzenle>**’yi seçin.
3. **Veri Toplama** sekmesinde, **Otomatik sağlama**’yı **Kapalı** olarak ayarlayın.
4. **Kaydet**’i seçin.

>[!NOTE]
> Otomatik sağlama devre dışı bırakıldığında Microsoft Monitoring Agent’ın sağlandığı Azure VM’lerinden aracı kaldırılmaz. Otomatik sağlamanın devre dışı bırakılması, kaynaklarınızın güvenliğinin izlenmesini kısıtlar.
>

## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta Standart katmana yükseltme yaptınız ve hibrit bulut iş yüklerinizle birleştirilmiş güvenlik yönetimi ve tehdit korumaması için Microsoft Monitoring Agent’ı sağladınız. Güvenlik Merkezi’ni kullanma hakkında daha fazla bilgi edinmek için şirket içi ortamda ve diğer bulutlarda bulunan Windows bilgisayarları eklemeye ilişkin hızlı başlangıca geçin.

> [!div class="nextstepaction"]
> [Hızlı Başlangıç: Windows bilgisayarları Azure Güvenlik Merkezi'ne ekleme](quick-onboard-windows-computer.md)

<!--Image references-->
[2]: ./media/security-center-get-started/overview.png
[4]: ./media/security-center-get-started/get-started.png
[5]: ./media/security-center-get-started/pricing.png
[6]: ./media/security-center-get-started/enable-automatic-provisioning.png
[7]: ./media/security-center-get-started/security-alerts.png
[8]: ./media/security-center-get-started/recommendations.png
[9]: ./media/security-center-get-started/select-subscription.png
