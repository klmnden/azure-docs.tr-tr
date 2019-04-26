---
title: X12 kodlama iletileri - Azure Logic Apps | Microsoft Docs
description: EDI doğrulamak ve XML ile kodlanmış dönüştürme ileti Kodlayıcısı Azure Logic Apps ile Enterprise Integration Pack ile X12 ile iletiler
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: jonfan, divswa, LADocs
ms.topic: article
ms.assetid: a01e9ca9-816b-479e-ab11-4a984f10f62d
ms.date: 01/27/2017
ms.openlocfilehash: 3ed5cb61fef5f07913f11c4e4df309d720d5b901
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60427550"
---
# <a name="encode-x12-messages-in-azure-logic-apps-with-enterprise-integration-pack"></a>X12 kodlama Azure Logic Apps Enterprise Integration Pack ile iletileri

Encode X12 ileti bağlayıcısıyla EDI ve iş ortağı özgü özellikleri doğrulamak, XML olarak kodlanmış iletileri değişimi işlem kümeleri EDI dönüştürmek ve teknik bir bildirim, işlev bildirimi veya her ikisi de istek.
Bu bağlayıcıyı kullanmak için bağlayıcıyı mantıksal uygulamanızda için var olan bir tetikleyici eklemeniz gerekir.

## <a name="before-you-start"></a>Başlamadan önce

Gereksinim duyduğunuz öğeleri şu şekildedir:

* Bir Azure hesabı; oluşturabileceğiniz bir [ücretsiz hesap](https://azure.microsoft.com/free)
* Bir [tümleştirme hesabı](logic-apps-enterprise-integration-create-integration-account.md) zaten tanımlanmış ve Azure aboneliğinizle ilişkili. Encode X12 ileti bağlayıcıyı kullanmak üzere bir tümleştirme hesabı olması gerekir.
* En az iki [iş ortakları](logic-apps-enterprise-integration-partners.md) , tümleştirme hesabında zaten tanımlanmış
* Bir [X12 sözleşmesi](logic-apps-enterprise-integration-x12.md) , tümleştirme hesabında zaten tanımlı

## <a name="encode-x12-messages"></a>X12 kodlama iletileri

1. [Mantıksal uygulama oluşturma](quickstart-create-first-logic-app-workflow.md).

2. İstek tetikleyicisi gibi mantıksal uygulamanızı başlatmak için bir tetikleyici eklemelisiniz tetikleyicileri, kodla X12 ileti Bağlayıcısı yok. Mantıksal Uygulama Tasarımcısı'nda bir tetikleyici ekleme ve ardından mantıksal uygulamanız için bir eylem ekleyin.

3.  Arama kutusuna filtreniz için "x12" girin. Şunlardan birini seçin **X12-kodlamak için X12 sözleşme adına göre ileti** veya **X12-kodlamak için X12 kimliklere göre ileti**.
   
    !["X12" için arama](./media/logic-apps-enterprise-integration-x12-encode/x12decodeimage1.png) 

3. Daha önce tümleştirme hesabı için herhangi bir bağlantı oluşturmadıysanız, artık bu bağlantıyı oluşturmak için istenir. Bağlantınızı adlandırın ve bağlanmak istediğiniz tümleştirme hesabı seçin. 
   
    ![Tümleştirme hesabı bağlantısı](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage1.png)

    Bir yıldız işareti ile özellikleri gereklidir.

    | Özellik | Ayrıntılar |
    | --- | --- |
    | Bağlantı adı * |Bağlantınız için herhangi bir ad girin. |
    | Tümleştirme hesabı * |Tümleştirme hesabı için bir ad girin. Tümleştirme hesabı ve mantıksal uygulamanızı aynı Azure konumda olduklarından emin olun. |

5.  İşiniz bittiğinde, bağlantı ayrıntılarınızı şu örneğe benzemelidir. Bağlantınızı oluşturmayı tamamlamak için seçin **Oluştur**.

    ![oluşturulan tümleştirme hesabı bağlantısı](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage2.png)

    Bağlantınızı artık oluşturulur.

    ![Tümleştirme hesabı bağlantı ayrıntıları](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage3.png) 

#### <a name="encode-x12-messages-by-agreement-name"></a>X12 kodlama sözleşme adına göre iletiler

X12 kodlanacak seçerseniz, sözleşme adına göre iletilerini **X12 adını sözleşmesi** listesinde, girin veya seçin, mevcut X12 sözleşme. Kodlanacak XML iletisi girin.

![X12 girin anlaşma adı ve kodlanacak XML iletisi](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage4.png)

#### <a name="encode-x12-messages-by-identities"></a>X12 kodlama kimliklere göre iletiler

X12 kodlanacak seçerseniz iletileri tarafından kimlik, gönderen tanımlayıcısı, gönderen niteleyicisi, alıcı tanımlayıcısı ve alıcı niteleyicisi, X12 içinde yapılandırılan girin sözleşme. Kodlanacak XML iletisi seçin.
   
![Gönderen ve alıcı için kimlikleri sağlamak, kodlanacak XML iletisi seçin](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage5.png) 

## <a name="x12-encode-details"></a>X12 kodlama ayrıntıları

X12 kodla bağlayıcı, bu görevleri gerçekleştirir:

* Eşleşen gönderen ve alıcı bağlam özellikleri tarafından sözleşme çözümleme.
* Değişimi işlem kümeleri EDI XML olarak kodlanmış iletileri dönüştürme EDI değişim serileştirir.
* İşlem kümesi üst bilgi ve tanıtım parçaları için geçerlidir
* Bir değişim denetim numarası, bir grup denetim numarası ve her giden değişimine yönelik bir işlem kümesi denetim numarası oluşturur
* Ayırıcı olarak yük verisi değiştirir
* EDI ve iş ortağı özgü özellikleri doğrulama
  * Şema ileti karşı işlem kümesi veri öğelerinin şema doğrulaması
  * EDI doğrulaması işlem kümesi veri öğeleri üzerinde gerçekleştirilen.
  * Genişletilmiş Doğrulama işlem kümesi veri öğeleri üzerinde gerçekleştirilen
* Teknik ve/veya işlev bildirimi (yapılandırılmışsa) ister.
  * Teknik bir bildirim başlığı doğrulama sonucu olarak oluşturur. Teknik bildirimi bir değişim üstbilgi ve tanıtım adresi alıcı tarafından işlenmesini durumu raporları
  * Bir işlev bildirimi gövdesi doğrulama sonucu olarak oluşturur. İşlev bildirimi alınan belge işlerken bir hatayla karşılaştı her bir hata raporları

## <a name="view-the-swagger"></a>Swagger görüntüleyin
Bkz: [ayrıntıları swagger](/connectors/x12/). 

## <a name="next-steps"></a>Sonraki adımlar
[Enterprise Integration Pack hakkında daha fazla bilgi](logic-apps-enterprise-integration-overview.md "Enterprise Integration Pack hakkında bilgi edinin") 

