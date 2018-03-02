---
title: "Azure Güvenlik Merkezi Hızlı Başlangıç - Güvenlik çözümlerini bağlama | Microsoft Docs"
description: "Azure Güvenlik Merkezi Hızlı Başlangıç - Güvenlik çözümlerini bağlama"
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 3263bb3d-befc-428c-9f80-53de65761697
ms.service: security-center
ms.devlang: na
ms.topic: quickstart
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/22/2018
ms.author: yurid
ms.openlocfilehash: 95cc85f0c742d465ab1ed68d6c29b61a6919dd5b
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/23/2018
---
# <a name="quickstart-connect-security-solutions-to-security-center"></a>Hızlı Başlangıç: Güvenlik çözümlerini Güvenlik Merkezi'ne bağlama

Bilgisayarlarınızdan güvenlik verileri toplamaya ek olarak, Common Event Format’ı (CEF) destekleyen güvenlik çözümleri de dahil olmak üzere diğer çeşitli güvenlik çözümlerinden güvenlik verilerini tümleştirebilirsiniz. CEF, Syslog iletilerine ek olarak sektörde standart haline gelmiş, farklı platformlar arasında olay tümleştirmesi sağlamak için birçok güvenlik hizmeti satıcısı tarafından kullanılan bir biçimdir.

Bu hızlı başlangıçta şu işlemleri nasıl yapacağınız gösterilir:
- CEF Günlükleri kullanarak bir güvenlik çözümünü Güvenlik Merkezi'ne bağlama
- Bağlantıyı güvenlik çözümü ile doğrulama

## <a name="prerequisites"></a>Ön koşullar
Güvenlik Merkezi ile çalışmaya başlamak için Microsoft Azure aboneliğinizin olması gerekir. Bir aboneliğiniz yoksa [ücretsiz hesap](https://azure.microsoft.com/free/) için kaydolabilirsiniz.

Bu hızlı başlangıcı adım adım uygulamak için Güvenlik Merkezi’nin Standart fiyatlandırma katmanında olmanız gerekir. Güvenlik Merkezi Standart katmanını ilk 60 gün boyunca hiçbir ücret ödemeden deneyebilirsiniz. [Azure aboneliğinizi Güvenlik Merkezi Standart katmanına ekleme](security-center-get-started.md) başlıklı hızlı başlangıçta Standart katmanına nasıl yükseltebileceğiniz adım adım açıklanmıştır.

Ayrıca, Güvenlik Merkezi’ne zaten bağlı bir Syslog hizmeti ile birlikte bir [Linux makine](https://docs.microsoft.com/azure/log-analytics/log-analytics-agent-linux) gerekir.

## <a name="connect-solution-using-cef"></a>CEF kullanarak çözüm bağlama

1. [Azure portal](https://azure.microsoft.com/features/azure-portal/) oturum açın.
2. **Microsoft Azure** menüsünde **Güvenlik Merkezi**’ni seçin. **Güvenlik Merkezi - Genel Bakış** açılır.

    ![Güvenlik merkezi'ni seçin](./media/quick-security-solutions/quick-security-solutions-fig1.png)  

3. Güvenlik Merkezi ana menüsünde **Güvenlik Çözümleri**’ni seçin.
4. Güvenlik Çözümleri sayfasındaki **Veri kaynakları ekle (3)** altında, **Common Event Format** içindeki **Ekle**’ye tıklayın.

    ![Veri kaynağı ekleme](./media/quick-security-solutions/quick-security-solutions-fig2.png)

5. Common Event Format Günlükleri sayfasında ikinci adım olan **Gerekli günlükleri UDP bağlantı noktası 25226'daki aracıya göndermek için Syslog iletmeyi yapılandırın**’ı genişletin ve Linux bilgisayarınızda aşağıdaki yönergeleri izleyin:

    ![Syslog yapılandırma](./media/quick-security-solutions/quick-security-solutions-fig3.png)

6. Üçüncü adım olan **Aracı yapılandırma dosyasını aracı bilgisayarına yerleştirin**’i genişletin ve Linux bilgisayarınızda aşağıdaki yönergeleri izleyin:

    ![Aracı yapılandırması](./media/quick-security-solutions/quick-security-solutions-fig4.png)

7. Dördüncü adım olan **Syslog daemon’ı ve aracıyı yeniden başlatın**’ı genişletin ve Linux bilgisayarınızda aşağıdaki yönergeleri izleyin:

    ![Syslog’u yeniden başlatma](./media/quick-security-solutions/quick-security-solutions-fig5.png)


## <a name="validate-the-connection"></a>Bağlantıyı doğrulama

Aşağıdaki adımlara geçmeden önce syslog’un Güvenlik Merkezi’ne bildirmeye başlamasını beklemeniz gerekir. Bu işlem biraz zaman alabilir ve ortamın boyutuna göre değişir.

1.  Sol bölmedeki Güvenlik Merkezi panosunda **Ara**’ya tıklayın.
2.  Syslog’un (Linux Makinesi) bağlı olduğu çalışma alanını seçin.
3.  *CommonSecurityLog* yazıp **Ara** düğmesine tıklayın.

Bu adımların sonucu aşağıdaki örnekte gösterilmiştir: ![CommonSecurityLog](./media/quick-security-solutions/common-sec-log.png)

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
Bu hızlı başlangıçta bir Linux Syslog çözümünü CEF kullanarak Güvenlik Merkezi'ne bağlamayı öğrendiniz. CEF günlüklerinizi Güvenlik Merkezi'ne bağlayarak, her günlük için arama ile özel uyarı kurallarından ve tehdit bilgileri zenginleştirme özelliğinden yararlanabilirsiniz. Güvenlik Merkezi'ni kullanma hakkında daha fazla bilgi için bir güvenlik ilkesi yapılandırma ve kaynaklarınızın güvenliğini değerlendirme ile ilgili öğreticiye geçin.

> [!div class="nextstepaction"]
> [Öğretici: Güvenlik ilkelerini tanımlama ve değerlendirme](./tutorial-security-policy.md)
