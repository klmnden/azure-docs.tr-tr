---
title: Logic apps sahte veriler - Azure Logic Apps ile test
description: Logic apps ile sahte veri üretim ortamlarını etkilemeden test etmek için statik sonuçları ayarlama
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: kevinlam1
ms.author: klam
ms.reviewer: estfan, LADocs
ms.topic: article
ms.date: 03/05/2019
ms.openlocfilehash: 43256e13dc1dd3263b213cc1e4a1e1c07af3b5c8
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2019
ms.locfileid: "57786587"
---
# <a name="test-logic-apps-with-mock-data-by-setting-up-static-results"></a>Logic apps ile sahte veri statik sonuçlarını ayarlayarak test edin.

Mantıksal uygulamanızı test ederken gerçekten arayın veya çeşitli nedenlerden dolayı uygulamaları, hizmetlerinize ve sistemlerinize erişmek hazır olmayabilir. Genellikle bu senaryolarda farklı koşul yolları çalıştırmak, hatalarını zorlamak, belirli bir ileti yanıt gövdeleri sağlayın veya bile bazı adımları atlama deneyin olabilir. Mantıksal uygulamanızda bir eylem için statik sonuçları ayarlayarak, çıktı verilerini bu eylemden sahte. Statik bir eylem sonuçlarına etkinleştirme eylem çalıştırmaz ancak bunun yerine sahte verileri döndürür.

Örneğin, ayarladığınız, statik sonuçları Outlook 365 için posta eylemi Gönder, Logic Apps altyapısı yalnızca statik sonuç, belirtilen sahte verileri yerine Outlook çağırmak ve bir e-posta döndürür.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>.

* Hakkında temel bilgilere [mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* Statik sonuçlarını ayarlamak istediğiniz mantıksal uygulama

<a name="set-up-static-results"></a>

## <a name="set-up-static-results"></a>Statik sonuçlarını ayarlayın

1. Henüz kaydolmadıysanız, [Azure portalında](https://portal.azure.com), Logic Apps Tasarımcısı'nda mantıksal uygulamanızı açın.

1. Eylem statik sonuçlarını ayarlamak istediğiniz yerde, aşağıdaki adımları izleyin: 

   1. Eylemin sağ üst köşede bulunan üç noktayı seçin (*...* ) düğmesini ve **statik sonucu**, örneğin:

      !["Statik sonuç" seçin > "Statik sonucu etkinleştir"](./media/test-logic-apps-mock-data-static-results/select-static-result.png)

   1. Seçin **etkinleştirme statik sonucu**. Gerekli (*) özelliklerini, eylemin yanıt için iade etmek istediğiniz sahte çıkış değerleri belirtin.

      Örneğin, HTTP eylemi için gerekli özellikleri şunlardır:

      | Özellik | Açıklama |
      |----------|-------------|
      | **Durum** | Döndürülecek eylem durumu |
      | **Durum kodu** | Özel durum kodunu döndürmek için |
      | **Üst Bilgiler** | Geri dönmek için üst bilgi içeriği |
      |||

      !["Statik sonucu etkinleştir" seçeneğini belirleyin](./media/test-logic-apps-mock-data-static-results/enable-static-result.png)

      Sahte veri JavaScript nesne gösterimi (JSON) biçiminde girin, tercih **JSON moduna geç** (![Seç "Anahtarı için JSON modu"](./media/test-logic-apps-mock-data-static-results/switch-to-json-mode-button.png)).

   1. İsteğe bağlı özellikler için açık **isteğe bağlı alanları seçin** listelemek ve sahte istediğiniz özellikleri seçin.

      ![İsteğe bağlı Özellikler'i seçin](./media/test-logic-apps-mock-data-static-results/optional-properties.png)

1. Kaydetmeye hazır olduğunuzda seçin **Bitti**.

   Eylemin sağ üst köşedeki başlık çubuğunun artık bir test beaker simgesi gösterilir (![statik sonuçları simgesi](./media/test-logic-apps-mock-data-static-results/static-results-test-beaker-icon.png)), statik sonuçları etkinleştirdikten gösterir.

   ![Statik sonuçları simgesini gösteren etkin](./media/test-logic-apps-mock-data-static-results/static-results-enabled.png)

<a name="reuse-sample-outputs"></a>

## <a name="reuse-previous-outputs"></a>Önceki yeniden çıkarır

Mantıksal uygulamanızı önceki bir varsa çıkışları sahte çıkış yeniden çalıştırma kopyalayabilir ve çalıştırılan çıkışları yapıştırın.

1. Henüz kaydolmadıysanız, [Azure portalında](https://portal.azure.com), Logic Apps Tasarımcısı'nda mantıksal uygulamanızı açın.

1. Mantıksal uygulamanızın ana menüden **genel bakış**. 

1. İçinde **çalıştırma geçmişi** çalıştırdığınız mantıksal uygulama istediğiniz bölümü, seçin.

1. Mantıksal uygulamanızın iş akışında, bulun ve istediğiniz çıkışları içeren eylem genişletin.

1. Seçin **ham çıkışları görüntüleyin** bağlantı.

1. Tam JavaScript nesne gösterimi (JSON) nesne veya örneğin, kullanmak istediğiniz özel alt bölümü, çıkış bölümü veya bile yalnızca üst bilgileri bölümüne kopyalayın.

1. Açma işlemi için adımları **statik sonucu** kutusu, eylem için [statik sonuçlarını ayarlayın](#set-up-static-results).

1. Sonra **statik sonucu** açılır kutusunda, her iki adım seçin:

   * Eksiksiz bir JSON nesnesi yapıştırmak için seçin **JSON moduna geç** (![Seç "Anahtarı için JSON modu"](./media/test-logic-apps-mock-data-static-results/switch-to-json-mode-button.png)):

     ![İçin tam nesne "Anahtarı için JSON modu" seçin](./media/test-logic-apps-mock-data-static-results/switch-to-json-mode-button-complete.png)

   * Yalnızca bir JSON bölümü, bu bölümün etiket yanındaki yapıştırın tercih **JSON moduna geç** için bu bölümü, örneğin:

     ![Çıktıların "Anahtarı için JSON modu" seçin](./media/test-logic-apps-mock-data-static-results/switch-to-json-mode-button-outputs.png)

1. JSON Düzenleyicisi'nde, önceden kopyalanan JSON yapıştırın.

   ![JSON modu](./media/test-logic-apps-mock-data-static-results/json-editing-mode.png)

1. İşiniz bittiğinde, seçim **Bitti**. Veya, tasarımcıya geri dönmek için seçin **Düzenleyicisi anahtar modu** (![Seç "Anahtar Düzenleyicisi modu"](./media/test-logic-apps-mock-data-static-results/switch-editor-mode-button.png)).

## <a name="disable-static-results"></a>Statik sonuçları devre dışı bırak

Statik sonuçların kapatma hemen değerleri, son Kurulum'dan oluşturmaz. Bu nedenle, sonraki açışınızda statik sonuçlarına etkinleştirdiğinizde, önceki değerlerinizi kullanmaya devam edebilirsiniz.

1. Statik çıkışları devre dışı bırakmak istediğiniz eylemi bulun. Eylemin sağ üst köşede bulunan test beaker simgesini (![statik sonuçları simgesi](./media/test-logic-apps-mock-data-static-results/static-results-test-beaker-icon.png)).

   ![Statik sonuçları devre dışı bırak](./media/test-logic-apps-mock-data-static-results/disable-static-results.png)

1. Seçin **devre dışı bırakın, sonucu statik** > **Bitti**.

   ![Statik sonuçları devre dışı bırak](./media/test-logic-apps-mock-data-static-results/disable-static-results-button.png)

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.

* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](https://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinin [Azure Logic Apps](../logic-apps/logic-apps-overview.md)