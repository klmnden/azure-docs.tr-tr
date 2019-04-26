---
title: Dropbox - Azure Logic Apps'ı bağlama
description: Karşıya yükleme ve Dropbox REST API'lerini ve Azure Logic Apps ile dosyaları yönetme
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.date: 03/01/2019
tags: connectors
ms.openlocfilehash: 5a1bfe8ca38fc23f09b13195fb8ca5bd443a4afd
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60312568"
---
# <a name="upload-and-manage-files-in-dropbox-by-using-azure-logic-apps"></a>Karşıya yükleme ve Azure Logic Apps kullanarak dropbox dosyalarını yönetme

Dropbox Bağlayıcısı ve Azure Logic Apps ile karşıya yüklemek ve yönetmek, Dropbox hesabınızdaki dosyaları otomatik iş akışları oluşturabilirsiniz. 

Bu makalede, mantıksal uygulamanızdan Dropbox'a bağlanın ve Dropbox eklemek gösterilmektedir **bir dosya oluşturulduğunda** tetikleyici ve Dropbox **yolunu kullanarak dosya içeriğini Al** eylem.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>.

* A [Dropbox hesabı](https://www.dropbox.com/), hangi ücretsiz kaydolabilirsiniz. Hesap kimlik bilgilerinizi, mantıksal uygulamanız ve Dropbox hesabınız arasında bir bağlantı oluşturmak için gereklidir.

* Hakkında temel bilgilere [mantıksal uygulamalar oluşturmak nasıl](../logic-apps/quickstart-create-first-logic-app-workflow.md). Bu örnekte, boş bir mantıksal uygulama gerekir.

## <a name="add-trigger"></a>Tetikleyici ekle

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Arama kutusunun altındaki seçin **tüm**. Arama kutusuna filtreniz olarak "dropbox" girin.
Tetikleyiciler listesinden şu tetikleyiciyi seçin: **Bir dosya oluşturulduğunda**

   ![Dropbox tetikleyicisini seçin](media/connectors-create-api-dropbox/select-dropbox-trigger.png)

1. Dropbox hesabı kimlik bilgilerinizle oturum açın ve Azure Logic Apps için Dropbox verilerinize erişimi yetkilendirin.

1. Tetikleyicinize ait gerekli bilgileri sağlayın. 

   Bu örnekte, dosya oluşturma izlemek istediğiniz klasörü seçin. Klasörlere gözatmak için klasör simgesine yanındaki seçin **klasör** kutusu.

## <a name="add-action"></a>Eylem ekle

Şimdi her yeni dosya içeriği alır bir eylem ekleyin.

1. Tetikleyici altında seçin **sonraki adım**. 

1. Arama kutusunun altındaki seçin **tüm**. Arama kutusuna filtreniz olarak "dropbox" girin.
Eylem listesinden şu eylemi seçin: **Yolu kullanarak dosya içeriğini Al**

1. Dropbox'a erişmek için Azure Logic Apps yetkilendirdiğiniz zaten yapmadıysanız, artık erişimi yetkisi verme.

1. Yanında, kullanmak istediğiniz dosya yoluna göz atmak için **dosya yolu** kutusunda, elipsleri seçin (**...** ) düğmesi. 

   İçinde de tıklayabilirsiniz **dosya yolu** kutusuna ve dinamik içerik listesinden **dosya yolu**, çıktı değeri olarak kullanılabilir tetikleyiciden önceki bölümde eklediğiniz.

1. İşiniz bittiğinde mantıksal uygulamanızı kaydedin.

1. Mantıksal uygulamanızı tetikleyecek şekilde Dropbox'ta yeni bir dosya oluşturun.

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Tetikleyiciler ve Eylemler sınırları, bağlayıcının Openapı'nin açıklandığı gibi teknik ayrıntılar için (önceki adıyla Swagger) dosyası, bkz: [bağlayıcının başvuru sayfası](/connectors/dropbox/).

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](https://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer hakkında bilgi edinin [Logic Apps bağlayıcıları](../connectors/apis-list.md)
