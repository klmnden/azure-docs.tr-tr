---
title: Azure Güvenlik Merkezi Öğreticisi - Güvenlik ilkelerini tanımlama ve değerlendirme | Microsoft Docs
description: Azure Güvenlik Merkezi Öğreticisi - Güvenlik ilkelerini tanımlama ve değerlendirme
services: security-center
documentationcenter: na
author: terrylan
manager: mbaldwin
editor: ''
ms.assetid: 2d248817-ae97-4c10-8f5d-5c207a8019ea
ms.service: security-center
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/22/2018
ms.author: yurid
ms.openlocfilehash: 16dc8553fdc1209d1973934a87660ff61df8e68a
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
ms.locfileid: "32779477"
---
# <a name="tutorial-define-and-assess-security-policies"></a>Öğretici: Güvenlik ilkelerini tanımlama ve değerlendirme
Güvenlik Merkezi, iş yüklerinizin istenen yapılandırmasını tanımlamak için güvenlik ilkeleri kullanarak şirketin veya yasal düzenlemelerin gerektirdiği güvenlik gereksinimlerine uyum sağlanmasına yardımcı olur. Azure abonelikleriniz için ilkeler tanımlayıp bunları iş yükü türüne veya verilerinizin duyarlılığına göre uyarladığınızda, Güvenlik Merkezi işlem, SQL ve depolama, ağ ve uygulama kaynaklarınıza güvenlik önerileri sağlayabilir. Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Güvenlik ilkesi yapılandırma
> * Kaynaklarınızın güvenliğini değerlendirme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.

## <a name="prerequisites"></a>Ön koşullar
Bu öğreticide ele alınan özellikleri adım adım görmek için Güvenlik Merkezi’nin Standart fiyatlandırma katmanında olmanız gerekir. Güvenlik Merkezi Standart katmanını ilk 60 gün boyunca hiçbir ücret ödemeden deneyebilirsiniz. [Azure aboneliğinizi Güvenlik Merkezi Standart katmanına ekleme](security-center-get-started.md) başlıklı hızlı başlangıçta Standart katmanına nasıl yükseltebileceğiniz adım adım açıklanmıştır.

## <a name="configure-security-policy"></a>Güvenlik ilkesi yapılandırma
Güvenlik Merkezi, Azure aboneliklerinizin her biri için otomatik olarak varsayılan bir güvenlik ilkesi oluşturur. Güvenlik ilkeleri, ilgili aboneliğin güvenlik gereksinimlerine göre açıp kapatabileceğiniz önerilerden oluşur. Varsayılan güvenlik ilkesinde değişiklik yapmak için aboneliğin sahibi, katkıda bulunanı veya güvenlik yöneticisi olmanız gerekir.

1. Güvenlik Merkezi ana menüsünde **Güvenlik ilkesi**’ni seçin. Kullanmak istediğiniz aboneliği seçin. **İLKE BİLEŞENLERİ** altında **Güvenlik ilkesi**’ni seçin:

  ![Güvenlik İlkesi](./media/tutorial-security-policy/tutorial-security-policy-fig1.png)  

2. İzlemek istediğiniz her güvenlik yapılandırması için **Açık**’ı seçin. Güvenlik Merkezi ortamınızın yapılandırmasını sürekli olarak değerlendirir ve bir güvenlik açığıyla karşılaştığında bir güvenlik önerisi oluşturur. Güvenlik yapılandırması önerilmiyorsa veya gerekli değilse **Kapalı**’yı seçin. Örneğin, bir geliştirme ve test ortamında üretim ortamlarındakiyle aynı güvenlik düzeyine sahip olmanız gerekmeyebilir. Ortamınız için geçerli olan ilkeleri seçtikten sonra **Kaydet**’e tıklayın.

Güvenlik Merkezi’nin bu ilkeleri işlemesini ve öneriler oluşturmasını bekleyin. Sistem güncelleştirmeleri ve işletim sistemi yapılandırmaları gibi bazı yapılandırma işlemlerinin tamamlanması 12 saati bulabilirken ağ güvenlik grupları ve şifreleme yapılandırmaları neredeyse anlık olarak değerlendirilir. Güvenlik Merkezi panosunda öneriler göründükten sonra bir sonraki adıma geçebilirsiniz.

## <a name="assess-security-of-resources"></a>Kaynakların güvenliğini değerlendirme
1. Güvenlik Merkezi, etkinleştirilen güvenlik ilkelerine göre gerekli durumlarda bir dizi güvenlik önerisi sağlar. İlk olarak sanal makine ve bilgisayar önerilerini incelemelisiniz. Güvenlik Merkezi panosunda **Genel Bakış**'a ve sonra **İşlem**’e tıklayın.

  ![İşlem](./media/tutorial-security-policy/tutorial-security-policy-fig2.png)

  Kırmızı renkli (yüksek öncelikli) önerilere öncelik vererek her bir öneriyi inceleyin. Bu önerilerden bazıları ([uç nokta koruma sorunları](https://docs.microsoft.com/azure/security-center/security-center-install-endpoint-protection) gibi), doğrudan Güvenlik Merkezi’nden uygulanabilecek bir düzeltme yöntemine sahip olabilir. Bazı önerilerse (eksik disk şifrelemesi önerisi gibi) yalnızca düzeltmeyi uygulamaya yönelik yönergeler içerir.

2. Tüm ilgili işlem önerileri için gerekenleri yaptıktan sonra bir sonraki iş yüküne geçmelisiniz: ağ. Güvenlik Merkezi panosunda **Genel Bakış**'a ve sonra **Ağ**’a tıklayın.

  ![Ağ](./media/tutorial-security-policy/tutorial-security-policy-fig3.png)

  Ağ önerileri sayfasında ağ yapılandırması, İnternet’e yönelik uç noktalar ve ağ topolojisi ile ilgili güvenlik sorunlarının listesi yer alır. **İşlem**’de olduğu gibi, bazı ağ önerileri tümleşik düzeltme olanağı sağlarken bazıları sağlamaz.

3. Tüm ilgili ağ önerileri için gerekenleri yaptıktan sonra bir sonraki iş yüküne geçmelisiniz: depolama ve veriler. Güvenlik Merkezi panosunda **Genel Bakış**'a ve sonra **Depolama ve veriler**’e tıklayın.

  ![Veri kaynakları](./media/tutorial-security-policy/tutorial-security-policy-fig4.png)

  **Veri kaynakları** sayfası, Azure SQL sunucuları ve veritabanları için denetimi etkinleştirme, SQL veritabanları için şifrelemeyi etkinleştirme ve Azure depolama hesabınızın şifrelenmesini etkinleştirme odaklı öneriler sağlar. Bu iş yüklerine sahip değilseniz herhangi bir öneri görmezsiniz. **İşlem**’de olduğu gibi, bazı SQL ve depolama önerileri tümleşik düzeltme olanağı sağlarken bazıları sağlamaz.

4. Tüm ilgili SQL ve depolama önerileri için gerekenleri yaptıktan sonra bir sonraki iş yüküne geçmelisiniz: uygulamalar. Güvenlik Merkezi panosunda **Genel Bakış**'a ve sonra **Uygulamalar**’a tıklayın.

  ![Uygulamalar](./media/tutorial-security-policy/tutorial-security-policy-fig5.png)

  **Uygulamalar** sayfası, web uygulaması güvenlik duvarı dağıtımına yönelik önerilerin yanı sıra uygulamaların sağlamlaştırılmasına yönelik genel öneriler sağlar. Internet Information Service (IIS) üzerinde çalışan web uygulamaları içeren sanal makineniz veya bilgisayarlarınız yoksa bu önerileri görmezsiniz.

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
Bu öğreticide, Güvenlik Merkezi ile temel ilke tanımı oluşturmayı ve aşağıdaki konularda iş yükünüzün güvenlik değerlendirmesini yapmayı öğrendiniz:

> [!div class="checklist"]
> * Şirketinizin veya yasal düzenlemelerin gerektirdiği güvenlik gereksinimlerine uyum sağlanması için güvenlik ilkesi yapılandırma
> * İşlem, SQL ve depolama, ağ ve uygulama kaynaklarınız için güvenlik değerlendirmesi

Güvenlik Merkezi’ni kullanarak kaynaklarınızı korumayı öğrenmek için bir sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Kaynaklarınızı koruma](tutorial-protect-resources.md)
