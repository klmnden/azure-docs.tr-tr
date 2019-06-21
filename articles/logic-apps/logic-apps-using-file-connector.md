---
title: Şirket içi - Azure Logic Apps dosya sistemlerine bağlanın
description: Görevler ve şirket içi dosya sistemlerine dosya sistemi bağlayıcısı aracılığıyla şirket içi veri ağ geçidi, Azure Logic Apps ile iş akışlarını otomatikleştirin
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: derek1ee
ms.author: deli
ms.reviewer: klam, estfan, LADocs
ms.topic: article
ms.date: 01/13/2019
ms.openlocfilehash: 5a6a57fb05d59e70df13f6800c8fa7bf87df91c6
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67295872"
---
# <a name="connect-to-on-premises-file-systems-with-azure-logic-apps"></a>Azure Logic Apps ile şirket içi dosya sistemlerine bağlanın

Dosya sistemi Bağlayıcısı ve Azure Logic Apps ile otomatik görevler oluşturabilir ve bir şirket içi dosya çubuğunda dosyaları oluşturmak ve yönetmek, iş akışları paylaşın, örneğin:  

- Oluşturma, alma, ekleme, güncelleştirme ve dosyaları silin.
- Klasörleri veya kök klasörleri dosyalarını listeleyin.
- Dosya içeriğini ve meta verileri alın.

Bu makalede bu örnek senaryo tarafından açıklandığı gibi bir şirket içi dosya sistemine nasıl bağlayabileceğini gösterir: Dropbox'a bir dosya paylaşımına karşıya yüklenen dosya kopyalayın ve ardından bir e-posta gönderin. Güvenli bir şekilde bağlanın ve şirket içi sistemler, logic apps kullanım [şirket içi veri ağ geçidi](../logic-apps/logic-apps-gateway-connection.md). Logic apps kullanmaya yeni başladıysanız gözden [Azure Logic Apps nedir?](../logic-apps/logic-apps-overview.md). Bağlayıcısı özel teknik bilgiler için bkz. [dosya sistemi Bağlayıcısı başvurusu](/connectors/filesystem/).

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Şirket içi sistemlere mantıksal uygulamalar gibi dosya sistemi sunucunuza bağlanabilmesi için önce yapmanız [yükleyin ve bir şirket içi veri ağ geçidi ayarlama](../logic-apps/logic-apps-gateway-install.md). Böylece, ağ geçidi yüklemenizi mantıksal uygulamanızdan dosya sistemi bağlantısı oluştururken kullanılacak belirtebilirsiniz.

* A [Dropbox hesabı](https://www.dropbox.com/), hangi ücretsiz kaydolabilirsiniz. Hesap kimlik bilgilerinizi, mantıksal uygulamanız ve Dropbox hesabınız arasında bir bağlantı oluşturmak için gereklidir.

* Kullanmak istediğiniz dosya sistemi olan bilgisayar erişim. Örneğin, veri ağ geçidi, dosya sistemi ile aynı bilgisayara yüklerseniz, bu bilgisayar için hesap kimlik bilgileri gerekir.

* Office 365 Outlook, Outlook.com veya Gmail gibi Logic Apps tarafından desteklenen bir sağlayıcıdan e-posta hesabı. Diğer sağlayıcılar için [buradaki bağlayıcı listesini inceleyin](https://docs.microsoft.com/connectors/). Bu mantıksal uygulama bir Office 365 Outlook hesabı kullanır. Başka bir e-posta hesabı kullanıyorsanız genel adımlar aynıdır, ancak kullanıcı arabirimi biraz farklı olabilir.

* Hakkında temel bilgilere [mantıksal uygulamalar oluşturmak nasıl](../logic-apps/quickstart-create-first-logic-app-workflow.md). Bu örnekte, boş bir mantıksal uygulama gerekir.

## <a name="add-trigger"></a>Tetikleyici ekleme

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Oturum [Azure portalında](https://portal.azure.com)ve Logic Apps Tasarımcısı'nda mantıksal uygulamanızı açın, açık değilse.

1. Arama kutusuna filtreniz olarak "dropbox" girin. Tetikleyiciler listesinden şu tetikleyiciyi seçin: **Bir dosya oluşturulduğunda**

   ![Dropbox tetikleyicisini seçin](media/logic-apps-using-file-connector/select-dropbox-trigger.png)

1. Dropbox hesabı kimlik bilgilerinizle oturum açın ve Azure Logic Apps için Dropbox verilerinize erişimi yetkilendirin.

1. Tetikleyicinize ait gerekli bilgileri sağlayın.

   ![Dropbox tetikleyicisi](media/logic-apps-using-file-connector/dropbox-trigger.png)

## <a name="add-actions"></a>Eylemler ekleyin

1. Tetikleyici altında seçin **sonraki adım**. Arama kutusuna filtreniz olarak "dosya sistemi" girin. Eylem listesinden şu eylemi seçin: **Dosya oluşturma**

   ![Dosya sistemi bağlayıcısını bulun](media/logic-apps-using-file-connector/find-file-system-action.png)

1. Dosya sisteminize zaten bir bağlantınız yoksa, bir bağlantı oluşturmanız istenir.

   ![Bağlantı oluşturma](media/logic-apps-using-file-connector/file-system-connection.png)

   | Özellik | Gereklidir | Value | Açıklama |
   | -------- | -------- | ----- | ----------- |
   | **Bağlantı Adı** | Evet | <*Bağlantı adı*> | Bağlantınız için istediğiniz adı |
   | **Kök klasör** | Evet | <*kök klasör adı*> | Örneğin, şirket içi veri ağ geçidinin yüklendiği bilgisayarda yerel bir klasör gibi şirket içi veri ağ geçidi yüklediyseniz, dosya sistemi için kök klasör veya bilgisayarın erişebileceği bir ağ paylaşımı için bir klasör. <p>Örneğin, `\\PublicShare\\DropboxFiles` <p>Kök klasörüne göreli yollar için dosya ile ilgili tüm eylemleri için kullanılan ana üst klasördür. |
   | **Kimlik doğrulaması türü** | Hayır | <*kimlik doğrulama türü*> | Dosya sistemini kullanır, örneğin, kimlik doğrulaması türü **Windows** |
   | **Kullanıcı Adı** | Evet | <*etki alanı*>\\<*kullanıcı adı*> | Dosya sisteminizi sahip olduğunuz bilgisayar için kullanıcı adı |
   | **Parola** | Evet | <*Parolanızı*> | Dosya sisteminizi sahip olduğu bilgisayar parolası |
   | **Ağ geçidi** | Evet | <*yüklü-gateway-name*> | Daha önce yüklü ağ geçidi adı |
   |||||

1. İşiniz bittiğinde **Oluştur**’u seçin.

   Logic Apps, yapılandırır ve bağlantının düzgün çalıştığından emin olun, bağlantınızı test eder. Bağlantı doğru şekilde ayarlanıp ayarlanmadığını daha önce seçtiğiniz eylemi için seçenekler görüntülenir.

1. İçinde **dosya oluştur** eylem, şirket içi dosya paylaşımınızdaki kök klasörüne dosyalar Dropbox'tan kopyalamak için ayrıntıları sağlayın. Önceki adımlardan çıktısı eklemek için kutularının içine tıklayın ve dinamik içerik listesi göründüğünde kullanılabilir alanları seçin.

   ![Dosya Eylem oluştur](media/logic-apps-using-file-connector/create-file-filled.png)

1. Şimdi, uygun kullanıcılara yeni dosya hakkında bilmesi bir e-posta gönderen bir Outlook eylem ekleyin. Alıcılar, başlık ve e-posta gövdesini girin. Test için kendi e-posta adresinizi kullanabilirsiniz.

   ![E-posta eylemi Gönder](media/logic-apps-using-file-connector/send-email.png)

1. Mantıksal uygulamanızı kaydedin. Dropbox'a bir dosya karşıya yükleyerek uygulamanızı test edin.

   Mantıksal uygulamanızı, şirket içi dosya paylaşımına dosya kopyalayın ve alıcıları kopyalanan dosya hakkında bir e-posta Gönder gerekir.

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Tetikleyiciler ve Eylemler sınırları hakkında teknik ayrıntılar için bağlayıcının Openapı'nin açıklanmıştır (önceki adıyla Swagger) açıklama, bağlayıcının gözden [başvuru sayfası](/connectors/fileconnector/).

## <a name="next-steps"></a>Sonraki adımlar

* Bilgi edinmek için nasıl [şirket içi verilere bağlanma](../logic-apps/logic-apps-gateway-connection.md) 
* Diğer hakkında bilgi edinin [Logic Apps bağlayıcıları](../connectors/apis-list.md)
