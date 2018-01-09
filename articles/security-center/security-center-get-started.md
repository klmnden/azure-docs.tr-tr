---
title: "Azure Güvenlik Merkezi Hızlı Başlangıç - yerleşik Azure aboneliğinize Güvenlik Merkezi standart | Microsoft Docs"
description: "Bu Hızlı Başlangıç, ek güvenlik için Güvenlik Merkezi'nin standart fiyatlandırma katmanı yükseltme gösterir."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/07/2018
ms.author: terrylan
ms.openlocfilehash: ac4e3b36b08223f7e3b54850ed53a8185e85f0d2
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2018
---
# <a name="quickstart-onboard-your-azure-subscription-to-security-center-standard"></a>Hızlı Başlangıç: Yerleşik Azure aboneliğinize Güvenlik Merkezi standart
Azure Güvenlik Merkezi, karma bulut yüklerinde Birleşik güvenlik yönetimi ve tehdit koruması sağlar. Ücretsiz katmanı yalnızca Azure kaynaklarınız için sınırlı güvenlik sunarken, standart katmanı şirket içi ve diğer bulut için bu özelliklerini genişletir. Güvenlik Merkezi standart bulmak ve güvenlik açıkları düzeltin, kötü amaçlı etkinliği engellemek, analiz ve Intelligence kullanarak tehditleri algılamak için erişim ve uygulama denetimleri uygulayın ve saldırı durumunda hızla yanıt yardımcı olur. Güvenlik Merkezi standart hiçbir ücret ilk 60 gün süreyle deneyebilirsiniz.

Bu makalede, ek güvenlik için standart katmana yükseltin ve güvenlik açıkları ve tehditleri izlemek için sanal makinelerde Microsoft izleme aracısı yükleyin.

## <a name="prerequisites"></a>Önkoşullar
Güvenlik Merkezi ile çalışmaya başlamak için Microsoft Azure aboneliğinizin olması gerekir. Bir aboneliğiniz yoksa [ücretsiz hesap](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.

Bir abonelik için standart katmana yükseltmek için abonelik sahibi, abonelik katkıda bulunan veya güvenlik yöneticisi rolüne atanan

## <a name="enable-your-azure-subscription"></a>Azure aboneliğinizi etkinleştirme

1. [Azure portal](https://azure.microsoft.com/features/azure-portal/) oturum açın.
2. **Microsoft Azure** menüsünde **Güvenlik Merkezi**’ni seçin. **Güvenlik Merkezi - genel bakış** açar.

 ![Güvenlik Merkezi'ne genel bakış][2]

**Güvenlik Merkezi – genel bakış** bulmak ve güvenlik iş yüklerinizin değerlendirin ve tanımlamak ve riski azaltmak etkinleştirme karma bulut iş yüklerinizi, güvenlik duruşunu içine birleştirilmiş bir görünümünü sağlar. Güvenlik Merkezi otomatik olarak etkinleştirir, Azure aboneliklerinize hiçbirini önceden siz veya başka bir abonelik kullanıcıya ücretsiz katmanı tarafından edildi.

Görüntüleyebilir ve Aboneliklerin listesini tıklatarak filtre **abonelikleri** menü öğesi. Güvenlik Merkezi güvenlik açıklarını tanımlamak için bu abonelikleri güvenlik değerlendiriliyor başlar. Değerlendirmeleri türlerini özelleştirmek için güvenlik ilkesini değiştirebilirsiniz. Güvenlik ilkesi iş yüklerinizin istenen yapılandırmasını tanımlar ve şirketin veya yasal düzenlemelerin gerektirdiği güvenlik gereksinimlerine uyum sağlanmasına yardımcı olur.

Güvenlik Merkezi ilk kez başlatmanın dakika içinde görebilirsiniz:

- **Öneriler** yolları, Azure aboneliklerinize güvenliğini artırmak için. Tıklatarak **önerileri** döşeme öncelikli listesi başlayacaktır.
- Bir envanterini **işlem**, **ağ**, **depolama & veri**, ve **uygulamaları** şimdi tarafından incelenen kaynaklar Güvenlik Merkezi güvenlik yaklaşımı, her birlikte.

Güvenlik Merkezi tam olarak yararlanmak için standart katmana yükseltin ve Microsoft izleme aracısı yüklemek için aşağıdaki adımları tamamlamanız gerekir.

## <a name="upgrade-to-the-standard-tier"></a>İçin standart katmana yükseltin
Güvenlik Merkezi Hızlı Başlangıç ipuçları ve öğreticiler amacıyla standart katmanına yükseltmeniz gerekir. İlk, 60 gün ücretsiz ve ücretsiz katmanı dilediğiniz zaman dönebilirsiniz.

1. Güvenlik Merkezi ana menüsündeki seçin **Onboarding Gelişmiş Güvenlik**.

2. Altında **Onboarding Gelişmiş Güvenlik**, Güvenlik Merkezi, abonelikleri ve hazırlanma için uygun çalışma alanları listelenir. Bir aboneliği listeden seçin.

  ![Abonelik seçin][4]

3. **Güvenlik İlkesi** abonelikte bulunan kaynak grupları hakkında bilgi sağlar. **Fiyatlandırma** da açılır.
4. Altında **fiyatlandırma**seçin **standart** ücretsiz standart ve seçeneğini yükseltmek için **kaydetmek**.

  ![Standart seçin][5]

Standart katmanına güncelleştirdiyseniz oluşturduysanız, ek Güvenlik Merkezi özellikleri erişimi **Uyarlamalı uygulama denetimleri**, **zaman VM Access'te yalnızca**, **güvenlik Uyarıları**, **tehdit Intelligence**, **Otomasyon playbooks**ve daha fazlası. Güvenlik Merkezi kötü amaçlı bir etkinlik algıladığında güvenlik uyarıları yalnızca görüntüleneceğini unutmayın.

  ![Güvenlik uyarıları][7]

## <a name="automate-data-collection"></a>Veri toplama otomatik hale getirme
Güvenlik Merkezi, Azure Vm'leri ve Azure olmayan bilgisayarlar güvenlik açıkları ve tehditleri izlemek için veri toplar. Microsoft izleme çeşitli güvenlikle ilgili yapılandırmaları ve olay günlüklerini makineden okur ve verileri analiz için çalışma alanınızda kopyalar aracısı kullanarak verileri toplanır. Varsayılan olarak, Güvenlik Merkezi sizin için yeni bir çalışma alanı oluşturur.

Otomatik sağlama etkinleştirilmişse, Güvenlik Merkezi desteklenen tüm Azure Vm'leri ve oluşturulan yeni bir tane Microsoft izleme aracısı yükler. Otomatik sağlama kesinlikle önerilir.

Microsoft Monitoring Agent ' otomatik sağlamayı etkinleştirmek için:

1. Güvenlik Merkezi ana menüsündeki seçin **Güvenlik İlkesi**.
2. Aboneliği seçin.
3. Altında **Güvenlik İlkesi**seçin **veri toplama**.
4. Altında **veri toplama**seçin **üzerinde** otomatik sağlamayı etkinleştirmek için.
5. **Kaydet**’i seçin.

  ![Otomatik sağlamayı etkinleştir][6]

Bu yeni bir anlayış ile Azure VM'ler, Güvenlik Merkezi sisteme ilgili ek öneriler güncelleştirme durumu, işletim sistemi güvenlik yapılandırmalarını, uç nokta koruma yanı sıra ek güvenlik uyarı oluşturmasını sağlayabilirsiniz.

  ![Öneriler][8]

## <a name="clean-up-resources"></a>Kaynakları temizleme
Diğer quickstarts ve öğreticiler bu koleksiyonda Bu hızlı başlangıç oluşturun. Sonraki quickstarts ve öğreticiler ile çalışmaya devam etmek planlıyorsanız, standart katmanı çalıştırmaya devam etmek ve etkin otomatik sağlamayı tutun. Devam etmek için ücretsiz katmanı döndürülmesini istediğiniz düşünmüyorsanız:

1. Güvenlik Merkezi ana menüye dönmek ve seçmek **Güvenlik İlkesi**.
2. Abonelik veya serbest döndürmek istediğiniz ilkeyi seçin. **Güvenlik İlkesi** açar.
3. Altında **İlkesi BİLEŞENLERİ**seçin **fiyatlandırma katmanı**.
4. Seçin **serbest** Standart abonelik değiştirmek için ücretsiz katmanına katmanı.
5. **Kaydet**’i seçin.

Otomatik sağlamayı devre dışı bırakmak istiyorsanız:

1. Güvenlik Merkezi ana menüye dönmek ve seçmek **Güvenlik İlkesi**.
2. Otomatik sağlamayı devre dışı bırakmak istediğiniz aboneliği seçin.
3. Altında **güvenlik ilkesi – veri toplama**seçin **kapalı** altında **Onboarding** otomatik sağlamayı devre dışı bırakmak için.
4. **Kaydet**’i seçin.

>[!NOTE]
> Otomatik sağlamayı devre dışı bırakma, Microsoft Monitoring Agent aracı bir yere sağlanan Azure VM'lerin kaldırmaz. Otomatik sağlama sınırları güvenlik kaynaklarınız için izleme devre dışı bırakılıyor.
>

## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıç standart katmanına yükseltme ve Microsoft Monitoring Agent Birleşik güvenlik yönetimi ve tehdit koruması için karma bulut yüklerinde sağlandı. Güvenlik Merkezi kullanma hakkında daha fazla bilgi için şirket içi ekleme Windows bilgisayarları ve diğer Bulutları hızlı başlangıç devam edin.

> [!div class="nextstepaction"]
> [Hızlı Başlangıç: Azure Güvenlik Merkezi yerleşik Windows bilgisayarlara](quick-onboard-windows-computer.md)

<!--Image references-->
[2]: ./media/security-center-get-started/overview.png
[4]: ./media/security-center-get-started/onboarding.png
[5]: ./media/security-center-get-started/pricing.png
[6]: ./media/security-center-get-started/enable-automatic-provisioning.png
[7]: ./media/security-center-get-started/security-alerts.png
[8]: ./media/security-center-get-started/recommendations.png
