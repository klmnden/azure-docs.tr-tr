---
title: "Azure Güvenlik Merkezi Hızlı Başlangıç Kılavuzu | Microsoft Docs"
description: "Bu makale, güvenlik izleme ve ilke yönetimi bileşenleri hakkında size rehberlik ederek ve sonraki adımlara yönlendirerek Azure Güvenlik Merkezi ile hızlıca çalışmaya başlamanıza yardımcı olur."
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
ms.date: 09/14/2017
ms.author: terrylan
ms.openlocfilehash: c28f92af96f31d1c386cf072f83fc142b9a7f588
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-security-center-quick-start-guide"></a>Azure Güvenlik Merkezi hızlı başlangıç kılavuzu
Bu makale, Güvenlik Merkezi’nin güvenlik izleme ve ilke yönetimi bileşenleri hakkında size rehberlik ederek Azure Güvenlik Merkezi ile hızlıca çalışmaya başlamanıza yardımcı olur.

## <a name="prerequisites"></a>Ön koşullar
Güvenlik Merkezi ile çalışmaya başlamak için Microsoft Azure aboneliğinizin olması gerekir. Bir aboneliğiniz yoksa [ücretsiz hesap](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.

Ücretsiz katmanı Güvenlik Merkezi, tüm Azure abonelikleri üzerinde otomatik olarak etkinleştirilir ve güvenlik ilkesi, sürekli güvenlik değerlendirmesi ve Azure kaynaklarınızı korumanıza yardımcı olmak için kullanılabilir güvenlik önerileri sağlar.

Güvenlik Merkezi'ne [Azure portalından](https://azure.microsoft.com/features/azure-portal/) erişebilirsiniz. Azure portalı hakkında daha fazla bilgi için bkz. [portal belgeleri](https://azure.microsoft.com/documentation/services/azure-portal/).

## <a name="permissions"></a>İzinler
Güvenlik Merkezi'nde, yalnızca kaynak sahibi, katkıda bulunan veya okuyucu rolü abonelik veya kaynak ait olduğu kaynak grubu için atandığında ilgili bilgileri görebilirsiniz. Bkz: [izinleri Azure Güvenlik Merkezi'nde](security-center-permissions.md) rolleri ve Güvenlik Merkezi'nde izin verilen eylemleri hakkında daha fazla bilgi için.

## <a name="data-collection"></a>Veri toplama
Güvenlik Merkezi, Azure sanal makineleri (VM'ler) ve güvenlik açıkları ve tehditleri izlemek üzere Azure olmayan bilgisayarları veri toplar. Microsoft izleme çeşitli güvenlikle ilgili yapılandırmaları ve olay günlüklerini makineden okur ve verileri analiz için çalışma alanınızda kopyalar aracısı kullanarak verileri toplanır. Güvenlik Merkezi hükümleri tüm mevcut Microsoft İzleme Aracısı, Azure Vm'leri ve oluşturulan yeni bir tane desteklenir. Bkz: [veri koleksiyonunu etkinleştir](security-center-enable-data-collection.md) veri toplama nasıl çalıştığı hakkında daha fazla bilgi için.

Otomatik sağlama önemle tavsiye edilir ve Güvenlik Merkezi'nin standart katmanında abonelikler için gereklidir. Otomatik sağlama sınırları güvenlik kaynaklarınız için izleme devre dışı bırakılıyor.

Bkz: [Güvenlik Merkezi fiyatlandırma](security-center-pricing.md) ücretsiz ve standart katmanları fiyatlandırma hakkında daha fazla bilgi edinmek için.

Aşağıdaki adımlar Güvenlik Merkezi’ne erişme ve bileşenlerini kullanma hakkında bilgi vermektedir.

> [!NOTE]
> Bu makale örnek bir dağıtım kullanarak hizmeti tanıtır. Bu makale adım adım ilerleyen bir kılavuz değildir.
>
>

## <a name="access-security-center"></a>Güvenlik Merkezi'ne erişme
Güvenlik Merkezi'ne erişmek için portalda şu adımları izleyin:

1. **Microsoft Azure** menüsünde **Güvenlik Merkezi**’ni seçin.

   ![Azure menüsü][1]
2. Güvenlik Merkezi’ne ilk kez erişiyorsanız **Hoş Geldiniz** dikey penceresi açılır. Seçin **başlatma Güvenlik Merkezi** açmak için **Güvenlik Merkezi**.
   ![Hoş Geldiniz ekranı][10]
3. Güvenlik Merkezi'ne Hoş Geldiniz dikey penceresinden başlatın veya Güvenlik Merkezi Microsoft Azure menüsünden seçtikten sonra **Güvenlik Merkezi** açar. Gelecekte **Güvenlik Merkezi** dikey penceresine kolay erişim için **Panoya dikey pencereyi sabitleme** seçeneğini (sağ üstte) seçin.
   ![Dikey pencereyi panoya sabitleme seçeneği][2]

## <a name="use-security-center"></a>Güvenlik Merkezi'ni Kullanma
Azure abonelikleriniz ve kaynak grupları için güvenlik ilkelerini yapılandırabilirsiniz. Aboneliğiniz için bir güvenlik ilkesi yapılandıralım:

1. Güvenlik Merkezi ana menüde seçin **Güvenlik İlkesi**.
2. Altında **Güvenlik Merkezi - Güvenlik İlkesi**, bir abonelik seçin.
3. Altında **güvenlik ilkesi - veri toplama**, **otomatik sağlamayı** açıktır. Güvenlik Merkezi hükümleri tüm mevcut Microsoft İzleme Aracısı, Azure Vm'leri ve oluşturulan yeni bir tane desteklenir.

    ![Güvenlik ilkesi][12]

4. Üzerinde **Güvenlik İlkesi** dikey penceresinde ilke bileşeni seçin **Güvenlik İlkesi**.

     ![Güvenlik ilkesi][11]

5. Altında **göstermek için öneriler**, güvenlik ilkenizin bir parçası olarak görmek istediğiniz önerileri açın. Örnekler:

   * Ayarı **sistem güncelleştirmeleri** için **üzerinde** taramaları tüm işletim sistemi güncelleştirmeleri eksik VM'ler desteklenir.
   * Ayarı **işletim sistemi güvenlik açıkları** için **üzerinde** taramaları tüm desteklenen VM saldırı karşısında daha savunmasız hale işletim sistemi yapılandırmasını tanımlamak için VM'ler.

6. **Kaydet**’i seçin.

### <a name="view-recommendations"></a>Önerileri görüntüleme
1. **Güvenlik Merkezi** dikey penceresine geri dönüp **Öneriler** kutucuğunu seçin. Güvenlik Merkezi düzenli aralıklarla Azure kaynaklarınızın güvenlik durumunu çözümler. Güvenlik Merkezi olası güvenlik açıklarını belirlediğinde **Öneriler** dikey penceresinde öneriler gösterir.
   ![Azure Güvenlik Merkezi'nde öneriler][5]
2. Daha fazla bilgi görüntülemek ve/veya sorunu çözmeye yönelik işlemler yapmak için **Öneriler** dikey penceresinde bir öneri seçin.

### <a name="view-the-security-state-of-your-resources"></a>Kaynaklarınızın güvenlik durumunu görüntüleyin
1. **Güvenlik Merkezi** dikey penceresine geri dönün. **Önleme** bölümü, ağ, veri ve uygulamaları VM'ler için güvenlik durumu göstergelerini içerir.
2. Seçin **işlem** daha fazla bilgi görüntülemek için. **İşlem** gösteren üç sekme dikey pencere açılır:

  - **Genel Bakış** -izleme içerir ve VM öneriler.
  - **VM'ler ve bilgisayarlar** -tüm sanal makineleri ve bilgisayarlar geçerli güvenlik durumunu listeler.
  - **Bulut Hizmetleri** -Güvenlik Merkezi tarafından izlenen web ve çalışan rolleri listeler.

    ![Güvenlik durumu işlem][6]

3. Üzerinde **genel bakış** sekmesinde, altında daha fazla bilgi görüntülemek ve/veya gerekli denetimleri yapılandırmak amacıyla eyleme geçmek için bir öneri seçin.
4. Üzerinde **VM'ler ve bilgisayarlar** sekmesinde, ek ayrıntıları görüntülemek için bir kaynak seçin.

### <a name="view-security-alerts"></a>Güvenlik uyarılarını görüntüleme
1. **Güvenlik Merkezi** dikey penceresine geri dönüp **Güvenlik Uyarıları** kutucuğunu seçin. **Güvenlik uyarıları** dikey penceresi açılır ve uyarıların bir listesini gösterir. Güvenlik günlüklerinize ve ağ etkinliğinize ilişkin Güvenlik Merkezi çözümlemesi bu uyarıları oluşturur. Tümleşik iş ortağı çözümlerinden gelen uyarılar buna dahildir.
   ![Azure Güvenlik Merkezi'ndeki güvenlik uyarıları][7]

2. Ek bilgileri görüntülemek için bir uyarı seçin. Bu örnekte, şimdi seçin **sistem ikili döküm filtrede bulunan değiştiren**. Bunun yapılması uyarı hakkında ek bilgiler sağlayan dikey pencereler açar.
   ![Azure Güvenlik Merkezi'nde güvenlik uyarısı ayrıntıları][8]

### <a name="view-the-health-of-your-partner-solutions"></a>İş ortağı çözümlerinizin sistem durumunu görüntüleme
1. **Güvenlik Merkezi** dikey penceresine geri dönün. **Güvenlik çözümleri** kutucuğu, Azure aboneliğinizle tümleşik iş ortağı çözümlerinizin sistem durumunu bir bakışta izlemenizi sağlar.
2. Seçin **güvenlik çözümleri** döşeme. Güvenlik Merkezi'ne bağlı iş ortağı çözümlerinizin listesini görüntüleyen dikey pencere açılır.
   ![İş ortağı çözümleri][9]
3. İş ortağı çözümü seçin. İş ortağı çözümünün durumunu ve çözümle ilişkili kaynakları gösteren dikey pencere açılır. Bu çözüme ilişkin iş ortağı yönetimi deneyimini açmak için **Çözüm konsolunu** seçin.

   ![İş ortağı çözümleri][13]

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede Güvenlik Merkezi'nin güvenlik izleme ve ilke yönetimi bileşenleri hakkında bilgi edindiniz. Güvenlik Merkezi hakkında bilgi sahibi olduğunuza göre aşağıdaki adımları deneyebilirsiniz:

* Azure aboneliğiniz için bir güvenlik ilkesi yapılandırma, bkz: [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](security-center-policies.md).
* Azure kaynaklarınızı korumak için bkz: yardımcı olması için Güvenlik Merkezi'nde öneriler kullanın [Azure Güvenlik Merkezi'nde güvenlik önerilerini yönetme](security-center-recommendations.md).
* Gözden geçirin ve mevcut güvenlik uyarılarınızı bkz [yönetme ve Azure Güvenlik Merkezi'nde güvenlik uyarılarını yanıtlama](security-center-managing-and-responding-alerts.md).
- Genel güvenliğinizi artırmak için bkz: iş ortakları ile tümleştirme hakkında daha fazla bilgi edinin [iş ortakları ve çözümleri tümleştirme](security-center-partner-integration.md).
- Verileri nasıl yönetildiğini öğrenin ve Güvenlik Merkezi'nde korunması, bkz: [Azure Güvenlik Merkezi veri güvenliği](security-center-data-security.md).
* Güvenlik Merkezi’nin [Standart katmanı](security-center-pricing.md) ile birlikte sunulan [gelişmiş tehdit algılama özellikleri](security-center-detection-capabilities.md) hakkında daha fazla bilgi edinin. Standart katman ilk 60 gün boyunca ücretsizdir.
* Güvenlik Merkezi’ni kullanma hakkında sorularınız varsa bkz. [Azure Güvenlik Merkezi SSS](security-center-faq.md).

<!--Image references-->
[1]: ./media/security-center-get-started/azure-menu.png
[2]: ./media/security-center-get-started/security-center-pin.png
[5]: ./media/security-center-get-started/recommendations.png
[6]: ./media/security-center-get-started/resources-health.png
[7]: ./media/security-center-get-started/security-alert.png
[8]: ./media/security-center-get-started/security-alert-detail.png
[9]: ./media/security-center-get-started/partner-solutions.png
[10]: ./media/security-center-get-started/welcome.png
[11]: ./media/security-center-get-started/show-recommendations-for.png
[12]: ./media/security-center-get-started/automatic-provisioning.png
[13]: ./media/security-center-get-started/partner-solutions-detail.png
