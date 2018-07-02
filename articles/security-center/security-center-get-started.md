---
title: Azure Güvenlik Merkezi Hızlı Başlangıç - Azure aboneliğinizi Güvenlik Merkezi Standart katmanına ekleme | Microsoft Docs
description: Bu hızlı başlangıçta, ek güvenlik kapsamı için Güvenlik Merkezinizin Standart fiyatlandırma katmanına nasıl yükseltme yapacağınız gösterilmektedir.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security-center
ms.devlang: na
ms.topic: quickstart
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/26/2018
ms.author: terrylan
ms.openlocfilehash: d10cef33ef0c325d41c9539107b9a4cab5e916d8
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37059863"
---
# <a name="quickstart-onboard-your-azure-subscription-to-security-center-standard"></a>Hızlı Başlangıç: Azure aboneliğinizi Güvenlik Merkezi Standart katmanına ekleme
Azure Güvenlik Merkezi, hibrit bulut iş yüklerinizde birleşik güvenlik yönetimi ve tehdit koruması sağlar. Ücretsiz katman yalnızca Azure kaynaklarınız için sınırlı güvenlik sunarken Standart katman bu özellikleri şirket içine ve diğer bulutlara genişletir. Güvenlik Merkezi Standart katmanı; güvenlik açıklarını bulup gidermenize, zararlı etkinlikleri engellemek için erişim ve uygulama denetimleri uygulamanıza, analizden ve bilgilerden yararlanarak tehditleri algılamanıza ve saldırı altındayken hızlıca yanıt vermenize yardımcı olur. Güvenlik Merkezi Standart katmanını ilk 60 gün boyunca hiçbir ücret ödemeden deneyebilirsiniz.

Bu makalede daha fazla güvenlik için Standart katmana yükseltecek, güvenlik açıklarını ve tehditleri izlemek için sanal makinelerinize Microsoft Monitoring Agent’ı yükleyeceksiniz.

## <a name="prerequisites"></a>Ön koşullar
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
Güvenlik Merkezi hızlı başlangıçlarının ve öğreticilerinin amacı doğrultusunda Standart katmana yükseltme yapmanız gerekir. İlk 60 gününüz ücretsizdir ve Ücretsiz katmana dilediğiniz zaman geri dönebilirsiniz.

1. Güvenlik Merkezi ana menüsünde **Gelişmiş güvenliğe ekleme** seçeneğini belirleyin.

2. **Gelişmiş güvenliğe ekleme** bölümünde Güvenlik Merkezi, ekleme işlemi için uygun abonelikleri ve çalışma alanlarını listeler. Listeden bir abonelik seçin.

  ![Abonelik seçme][4]

3. **Güvenlik ilkesi**, abonelikte yer alan kaynak grupları hakkında bilgi verir. **Fiyatlandırma** bölümü de açılır.
4. **Fiyatlandırma** bölümünde Ücretsiz katmandan Standart katmana yükseltme yapmak için **Standart** seçeneğini belirleyip **Kaydet**’e tıklayın.

  ![Standart katmanı seçme][5]

Standart katmana yükseltme yaptığınıza göre artık **uyarlamalı uygulama denetimleri**, **tam zamanında VM erişimi**, **güvenlik uyarıları**, **tehdit bilgileri** ve **otomasyon playbook’ları** da dahil olmak üzere ek Güvenlik Merkezi özelliklerine erişebilirsiniz. Güvenlik uyarılarının yalnızca Güvenlik Merkezi’nin zararlı etkinlik algılaması durumunda görüneceğini unutmayın.

  ![Güvenlik uyarıları][7]

## <a name="automate-data-collection"></a>Veri toplama işlemini otomatikleştirme
Güvenlik Merkezi, güvenlik açıklarını ve tehditleri izlemek için Azure VM’lerinizden ve Azure olmayan bilgisayarlarınızdan veri toplar. Veriler, makineden güvenlikle ilgili çeşitli yapılandırmaları ve olay günlüklerini okuyup verileri analiz için çalışma alanınıza kopyalayan Microsoft Monitoring Agent kullanılarak toplanır. Varsayılan olarak Güvenlik Merkezi sizin için yeni bir çalışma alanı oluşturur.

Otomatik sağlama etkinleştirildiğinde Güvenlik Merkezi, desteklenen tüm Azure VM’lere ve oluşturulan yeni VM’lere Microsoft Monitoring Agent’ı yükler. Otomatik sağlama önemle önerilir.

Microsoft Monitoring Agent için otomatik sağlamayı etkinleştirmek üzere:

1. Güvenlik Merkezi ana menüsünde **Güvenlik İlkesi**’ni seçin.
2. Aboneliği seçin.
3. **Güvenlik ilkesi** bölümünde **Veri Toplama**’yı seçin.
4. **Veri Toplama** bölümünde, otomatik sağlamayı etkinleştirmek için **Açık**’ı seçin.
5. **Kaydet**’i seçin.

  ![Otomatik sağlamayı etkinleştirme][6]

Azure VM’lerinize ilişkin bu yeni öngörüyle Güvenlik Merkezi, sistem güncelleştirme durumu, işletim sistemi güvenlik yapılandırmaları ve uç nokta koruması ile ilgili ek Öneriler sunmanın yanı sıra ek Güvenlik uyarıları oluşturabilir.

  ![Öneriler][8]

## <a name="clean-up-resources"></a>Kaynakları temizleme
Bu koleksiyondaki diğer hızlı başlangıçlar ve öğreticiler bu hızlı başlangıcı temel alır. Sonraki hızlı başlangıç ve öğreticilerle çalışmaya devam etmeyi planlıyorsanız Standart katmanını çalıştırmaya devam edin ve otomatik sağlamayı etkinleştirilmiş halde tutun. Devam etmeyi planlamıyorsanız veya Ücretsiz katmanına dönmek istiyorsanız:

1. Güvenlik Merkezi ana menüsüne dönüp **Güvenlik İlkesi**’ni seçin.
2. Ücretsiz katmanına döndürmek istediğiniz aboneliği veya ilkeyi seçin. **Güvenlik ilkesi** açılır.
3. **İLKE BİLEŞENLERİ** altında **Fiyatlandırma katmanı**’nı seçin.
4. Aboneliği Standart katmanından Ücretsiz katmanına geçirmek için **Ücretsiz**’i seçin.
5. **Kaydet**’i seçin.

Otomatik sağlamayı devre dışı bırakmak istiyorsanız:

1. Güvenlik Merkezi ana menüsüne dönüp **Güvenlik ilkesi**’ni seçin.
2. Otomatik sağlamayı hangi abonelik için devre dışı bırakmak istediğinizi belirtin.
3. Otomatik sağlamayı kapatmak için **Güvenlik ilkesi – Veri Toplama** altındaki **Ekleme** bölümünden **Kapalı**’yı seçin.
4. **Kaydet**’i seçin.

>[!NOTE]
> Otomatik sağlama devre dışı bırakıldığında Microsoft Monitoring Agent’ın sağlandığı Azure VM’lerinden aracı kaldırılmaz. Otomatik sağlamanın devre dışı bırakılması, kaynaklarınızın güvenliğinin izlenmesini kısıtlar.
>

## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta Standart katmana yükseltme yaptınız ve hibrit bulut iş yüklerinizle birleştirilmiş güvenlik yönetimi ve tehdit korumaması için Microsoft Monitoring Agent’ı sağladınız. Güvenlik Merkezi’ni kullanma hakkında daha fazla bilgi edinmek için şirket içi ortamda ve diğer bulutlarda bulunan Windows bilgisayarları eklemeye ilişkin hızlı başlangıca geçin.

> [!div class="nextstepaction"]
> [Hızlı Başlangıç: Windows bilgisayarları Azure Güvenlik Merkezi’ne ekleme](quick-onboard-windows-computer.md)

<!--Image references-->
[2]: ./media/security-center-get-started/overview.png
[4]: ./media/security-center-get-started/onboarding.png
[5]: ./media/security-center-get-started/pricing.png
[6]: ./media/security-center-get-started/enable-automatic-provisioning.png
[7]: ./media/security-center-get-started/security-alerts.png
[8]: ./media/security-center-get-started/recommendations.png
