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
ms.date: 05/13/2019
ms.openlocfilehash: 45eeb20e5c572ddd98244b2e751322fcce1d4b76
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65597202"
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

   1. Eylemin sağ üst köşede bulunan üç noktayı seçin ( *...* ) düğmesini ve **statik sonucu**, örneğin:

      !["Statik sonuç" seçin > "Statik sonucu etkinleştir"](./media/test-logic-apps-mock-data-static-results/select-static-result.png)

   1. Seçin **etkinleştirme statik sonucu**. Gerekli (*) özelliklerini, eylemin yanıt için iade etmek istediğiniz sahte çıkış değerleri belirtin.

      Örneğin, HTTP eylemi için gerekli özellikleri şunlardır:

      | Özellik | Açıklama |
      |----------|-------------|
      | **Durumu** | Döndürülecek eylem durumu |
      | **Durum Kodu** | Özel durum kodunu döndürmek için |
      | **Üst Bilgiler** | Geri dönmek için üst bilgi içeriği |
      |||

      !["Statik sonucu etkinleştir" seçeneğini belirleyin](./media/test-logic-apps-mock-data-static-results/enable-static-result.png)

      Sahte veri JavaScript nesne gösterimi (JSON) biçiminde girin, tercih **JSON moduna geç** (![Seç "Anahtarı için JSON modu"](./media/test-logic-apps-mock-data-static-results/switch-to-json-mode-button.png)).

   1. İsteğe bağlı özellikler için açık **isteğe bağlı alanları seçin** listelemek ve sahte istediğiniz özellikleri seçin.

      ![İsteğe bağlı Özellikler'i seçin](./media/test-logic-apps-mock-data-static-results/optional-properties.png)

1. Kaydetmeye hazır olduğunuzda seçin **Bitti**.

   Eylemin sağ üst köşedeki başlık çubuğunun artık bir test beaker simgesi gösterilir (![statik sonuçları simgesi](./media/test-logic-apps-mock-data-static-results/static-results-test-beaker-icon.png)), statik sonuçları etkinleştirdikten gösterir.

   ![Statik sonuçları simgesini gösteren etkin](./media/test-logic-apps-mock-data-static-results/static-results-enabled.png)

   Sahte verileri kullanan önceki çalıştırmaları bulmak için bkz: [statik sonuçları kullanan çalıştırmaları bulma](#find-runs-mock-data) bu konunun devamındaki.

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

1. İşiniz bittiğinde **Bitti**'yi seçin. Veya, tasarımcıya geri dönmek için seçin **Düzenleyicisi anahtar modu** (![Seç "Anahtar Düzenleyicisi modu"](./media/test-logic-apps-mock-data-static-results/switch-editor-mode-button.png)).

<a name="find-runs-mock-data"></a>

## <a name="find-runs-that-use-static-results"></a>Statik sonuçları kullanan çalıştırmaları bulma

Mantıksal uygulamanızın çalıştırma geçmişi, burada statik sonuçları eylemleri kullanın çalıştırmalar tanımlar. Bu çalıştırmaları bulmak için aşağıdaki adımları izleyin:

1. Mantıksal uygulamanızın ana menüden **genel bakış**. 

1. Sağ bölmede altında **çalıştırma geçmişi**, bulma **statik sonuçları** sütun. 

   Sonuçlarla eylemleri içeren tüm run sahip **statik sonuçları** sütun kümesine **etkin**, örneğin:

   ![Çalıştırma geçmişi - statik sonuçları sütun](./media/test-logic-apps-mock-data-static-results/run-history.png)

1. Statik sonuçları kullanan eylemlerini görüntülemek için istediğiniz yeri Çalıştır'ı seçin **statik sonuçları** sütun ayarlanmış **etkin**.

   Statik sonuçları kullanan eylemleri göster test beaker (![statik sonuçları simgesi](./media/test-logic-apps-mock-data-static-results/static-results-test-beaker-icon.png)) simgesi, örneğin:

   ![Çalıştırma geçmişi - statik sonuçları kullanan eylemleri](./media/test-logic-apps-mock-data-static-results/static-results-enabled-run-details.png)

## <a name="disable-static-results"></a>Statik sonuçları devre dışı bırak

Statik sonuçların kapatma hemen değerleri, son Kurulum'dan oluşturmaz. Bu nedenle, sonraki açışınızda statik sonuçlarına etkinleştirdiğinizde, önceki değerlerinizi kullanmaya devam edebilirsiniz.

1. Statik çıkışları devre dışı bırakmak istediğiniz eylemi bulun. Eylemin sağ üst köşede bulunan test beaker simgesini (![statik sonuçları simgesi](./media/test-logic-apps-mock-data-static-results/static-results-test-beaker-icon.png)).

   ![Statik sonuçları devre dışı bırak](./media/test-logic-apps-mock-data-static-results/disable-static-results.png)

1. Seçin **devre dışı bırakın, sonucu statik** > **Bitti**.

   ![Statik sonuçları devre dışı bırak](./media/test-logic-apps-mock-data-static-results/disable-static-results-button.png)

## <a name="reference"></a>Başvuru

Bu ayarda, temel alınan iş akışı tanımları hakkında daha fazla bilgi için bkz. [statik sonuçları - iş akışı tanımı dil şeması başvurusu](../logic-apps/logic-apps-workflow-definition-language.md#static-results) ve [runtimeConfiguration.staticResult - çalışma zamanı yapılandırma ayarları](../logic-apps/logic-apps-workflow-actions-triggers.md#runtime-configuration-settings)

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinin [Azure Logic Apps](../logic-apps/logic-apps-overview.md)