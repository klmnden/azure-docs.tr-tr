---
title: Azure önizlemesindeki Gözcü bir playbook'u çalıştırma | Microsoft Docs
description: Bu makalede Azure Gözcü içinde bir playbook çalıştırmayı öğrenin.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: e4afc5c8-ffad-4169-8b73-98d00155fa5a
ms.service: sentinel
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2019
ms.author: rkarlin
ms.openlocfilehash: d5f055ce337cb43e0813bc9ff295d0958e06f561
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65205437"
---
# <a name="tutorial-set-up-automated-threat-responses-in-azure-sentinel-preview"></a>Öğretici: Otomatik tehdit yanıtları Gözcü Azure önizlemesinde ayarlama

> [!IMPORTANT]
> Azure Sentinel şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Bu öğreticide, Azure Gözcü tarafından algılanan güvenlik ile ilgili sorunları otomatik tehdit yanıtlarını ayarlamak için Azure Gözcü içinde güvenlik playbook'ları kullanmanıza yardımcı olur.


> [!div class="checklist"]
> * Playbook'ları anlama
> * Bir playbook oluşturmak
> * Playbook çalıştırın


## <a name="what-is-a-security-playbook-in-azure-sentinel"></a>Bir güvenlik playbook'ları Azure Gözcü nedir?

Güvenlik playbook'u Azure Gözcü bir uyarıya yanıt olarak çalıştırılabilir yordamları koleksiyonudur. Güvenlik playbook'u otomatikleştirmek ve düzenlemek, yanıtınız yardımcı olabilir ve el ile çalıştırmak veya belirli bir uyarı tetiklendiğinde otomatik olarak çalışacak şekilde ayarlayın. Güvenlik playbook'ları Azure Gözcü içinde temel [Azure Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-what-are-logic-apps), yani tüm güç, özelleştirmeyi ölçme ve Logic Apps, yerleşik şablonlar alın. Her playbook seçtiğiniz abonelik için oluşturulur, ancak playbook'ları sayfasına baktığınızda, tüm seçili Aboneliklerde tüm playbook'ları görürsünüz.

> [!NOTE]
> Azure Logic Apps playbook'ları yararlanın, bu nedenle ücretleri uygulanır. Ayrıntılı bilgi için [Azure Logic Apps](https://azure.microsoft.com/pricing/details/logic-apps/) fiyatlandırma sayfasını ziyaret edin.

Örneğin, kötü amaçlı kişilerin, ağ kaynaklarına erişim hakkında endişeleniyoruz, kötü amaçlı IP adresleri kullanarak ağınıza erişen benzeyen bir uyarı ayarlayabilirsiniz. Ardından aşağıdakileri yapan bir playbook oluşturabilirsiniz:
1. Uyarı tetiklendiğinde, ServiceNow veya çağrı oluşturma sistemi BT bir bilet açın.
2. Microsoft Teams veya Slack, güvenlik analistleri olay farkında olduğundan emin olmak için güvenlik işlemleri kanala bir ileti gönderin.
3. Tüm bilgileri uyarıda Kıdemli ağ yönetim ve güvenlik yöneticinize gönderin. E-posta iletisi Ayrıca iki kullanıcı seçenek düğmeleri içerir **blok** veya **Yoksay**.
4. Playbook, yanıt yöneticilerinden alındıktan sonra çalışmaya devam eder.
5. Yöneticileri seçerseniz **blok**, IP adresini Güvenlik Duvarı'nda engellenir ve Azure AD'de kullanıcının devre dışı bırakıldı.
6. Yöneticileri seçerseniz **Yoksay**Azure Gözcü içinde uyarı kapatıldı ve ServiceNow olayı kapatılır.

Güvenlik playbook'ları, elle veya otomatik olarak çalıştırılabilir. El ile çalışan bir uyarı aldığınızda, seçilen uyarı için bir yanıt olarak bir playbook isteğe bağlı çalıştırmayı seçebilirsiniz anlamına gelir. Otomatik olarak çalıştırarak bağıntı kural yazarken uyarısı tetiklendiğinde otomatik olarak bir veya daha fazla playbook'ları çalıştırmak için ayarladığınız anlamına gelir.


## <a name="create-a-security-playbook"></a>Güvenlik playbook'u oluşturmak

Azure Gözcü içinde güvenlik playbook'u oluşturmak için aşağıdaki adımları izleyin:

1. Açık **Azure Gözcü** Pano.
2. Altında **Yönetim**seçin **playbook'ları**.

   ![Mantıksal Uygulama](./media/tutorial-respond-threats-playbook/playbookimg.png)

3. İçinde **Azure Gözcü - Playbook'lar (Önizleme)** sayfasında **Ekle** düğmesi.

   ![Mantıksal uygulama oluşturma](./media/tutorial-respond-threats-playbook/create-playbook.png) 

4. İçinde **mantıksal uygulama oluştur** sayfasında, yeni mantıksal uygulamanızı oluşturun ve gerekli bilgileri yazın **Oluştur**. 

5. İçinde [ **mantıksal Uygulama Tasarımcısı**](../logic-apps/logic-apps-overview.md), kullanmak istediğiniz şablonu seçin. Kimlik BIOS'ta bir şablonu seçerseniz, bunları vermeniz gerekir. Alternatif olarak, yeni bir boş playbook sıfırdan oluşturabilirsiniz. Seçin **boş mantıksal uygulama**. 

   ![Mantıksal uygulama tasarımcısı](./media/tutorial-respond-threats-playbook/playbook-template.png)

6. Mantıksal uygulama burada yeni derleme veya şablonu Düzen Tasarımcısı alınır. Playbook ile oluşturma hakkında daha fazla bilgi için [Logic Apps](../logic-apps/logic-apps-create-logic-apps-from-templates.md).

7. İçinde boş bir playbook oluşturuyorsanız **tüm bağlayıcılar ve tetikleyiciler içinde arayın** alanına *Azure Gözcü*seçip **tetiklenenolduğundabirAzureGözcüuyarısıyanıtı**. <br>Oluşturulduktan sonra yeni playbook görünür **playbook'ları** listesi. Görünmüyorsa, tıklayın **Yenile**. 

7. Şimdi playbook'unuz tetiklendiğinde gerçekleştirilecek işlemleri tanımlayabilirsiniz. Bir eylem, mantıksal koşul, anahtar durumu koşulu veya döngü ekleyebilirsiniz.

   ![Mantıksal uygulama tasarımcısı](./media/tutorial-respond-threats-playbook/logic-app.png)

## <a name="how-to-run-a-security-playbook"></a>Güvenlik playbook'u çalıştırma

İsteğe bağlı bir playbook çalıştırabilirsiniz.

Bir playbook isteğe bağlı çalıştırmak için:

1. İçinde **çalışmaları** sayfasında bir durum seçin ve tıklayın **tam ayrıntılarını görüntüleme**.

2. İçinde **uyarılar** sekmesinde istediğiniz playbook'u çalıştırmak için uyarıyı tıklayın ve kaydırma tamamen sağa ve tıklatın **playbook'ları görüntülemek** için bir playbook'u seçip **çalıştırmak** gelen abonelikte kullanılabilir playbook'ları listesi. 




## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Azure Gözcü içinde bir playbook çalıştırması öğrendiniz. Azure Gözcü hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın: Bu öğreticide, Azure Gözcü içinde bir playbook çalıştırması öğrendiniz. Devam [nasıl proaktif olarak tehditleri hunt](hunting.md) Azure Gözcü kullanarak.
> [!div class="nextstepaction"]
> [Tehditleri Hunt](hunting.md) tehditleri ağınızda proaktif bir şekilde bulmak için.

