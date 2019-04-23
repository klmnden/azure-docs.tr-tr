---
title: AS2 iletilerini - Azure Logic Apps kodlama | Microsoft Docs
description: İLETİLERİ Azure Logic Apps ve Enterprise Integration Pack ile kodlayın
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: jonfan, divswa, LADocs
ms.topic: article
ms.assetid: 332fb9e3-576c-4683-bd10-d177a0ebe9a3
ms.date: 08/08/2018
ms.openlocfilehash: 455056feaa1b13022be3ab3c15880a87c50dedd9
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60003556"
---
# <a name="encode-as2-messages-with-azure-logic-apps-and-enterprise-integration-pack"></a>AS2 iletilerini Azure Logic Apps ve Enterprise Integration Pack ile kodlayın

Güvenlik ve güvenilirlik aktaran iletileri çalışırken'kurmak için kodlama AS2 iletisi Bağlayıcısı'nı kullanın. Bu bağlayıcı, dijital imzalama, şifreleme ve üzerinden ileti değerlendirme bildirimleri (için inkar desteklemek için de sağlar. MDN), bildirimleri sağlar.

## <a name="before-you-start"></a>Başlamadan önce

Gereksinim duyduğunuz öğeleri şu şekildedir:

* Bir Azure hesabı; oluşturabileceğiniz bir [ücretsiz hesap](https://azure.microsoft.com/free)
* Bir [tümleştirme hesabı](logic-apps-enterprise-integration-create-integration-account.md) zaten tanımlanmış ve Azure aboneliğinizle ilişkili. AS2 kodlama ileti bağlayıcıyı kullanmak üzere bir tümleştirme hesabı olması gerekir.
* En az iki [iş ortakları](logic-apps-enterprise-integration-partners.md) , tümleştirme hesabında zaten tanımlanmış
* Bir [AS2 sözleşmesi](logic-apps-enterprise-integration-as2.md) , tümleştirme hesabında zaten tanımlı

## <a name="encode-as2-messages"></a>AS2 iletilerini kodlar

1. [Mantıksal uygulama oluşturma](quickstart-create-first-logic-app-workflow.md).

2. AS2 kodlama ileti Bağlayıcısı'nı tetikleyicileri, sahip değil. istek tetikleyicisi gibi mantıksal uygulamanızı başlatmak için bir tetikleyici eklemelisiniz. Mantıksal Uygulama Tasarımcısı'nda bir tetikleyici ekleme ve ardından mantıksal uygulamanız için bir eylem ekleyin.

3.  Arama kutusuna filtreniz için "AS2" girin. Seçin **AS2 - kodlama AS2 iletisinin**.
   
    !["AS2" için arama](./media/logic-apps-enterprise-integration-as2-encode/as2decodeimage1.png)

4. Daha önce tümleştirme hesabı için herhangi bir bağlantı oluşturmadıysanız, artık bu bağlantıyı oluşturmak için istenir. Bağlantınızı adlandırın ve bağlanmak istediğiniz tümleştirme hesabı seçin. 
   
    ![Tümleştirme hesabına bağlantı oluşturma](./media/logic-apps-enterprise-integration-as2-encode/as2encodeimage1.png)  

    Bir yıldız işareti ile özellikleri gereklidir.

    | Özellik | Ayrıntılar |
    | --- | --- |
    | Bağlantı adı * |Bağlantınız için herhangi bir ad girin. |
    | Tümleştirme hesabı * |Tümleştirme hesabı için bir ad girin. Tümleştirme hesabı ve mantıksal uygulamanızı aynı Azure konumda olduklarından emin olun. |

5.  İşiniz bittiğinde, bağlantı ayrıntılarınızı şu örneğe benzemelidir. Bağlantınızı oluşturmayı tamamlamak için seçin **Oluştur**.
   
    ![Tümleştirme bağlantı ayrıntıları](./media/logic-apps-enterprise-integration-as2-encode/as2encodeimage2.png)

6. Bu örnekte gösterildiği gibi bağlantınızı oluşturulduktan sonra için ayrıntıları sağlayın **AS2-gelen**, **AS2-tanımlayıcılara** , sözleşmede yapılandırılan ve **gövdesi**, olduğu İletinin yükü.
   
    ![zorunlu alanlar sağlayın](./media/logic-apps-enterprise-integration-as2-encode/as2encodeimage3.png)

## <a name="as2-encoder-details"></a>AS2 Kodlayıcı ayrıntıları

AS2 kodlama bağlayıcı, bu görevleri gerçekleştirir: 

* AS2/HTTP üstbilgileri geçerlidir
* İmzalar (yapılandırılmışsa) giden iletileri
* Giden iletiler (yapılandırılmışsa) şifreler.
* İletisi (yapılandırılmışsa) sıkıştırır.
* MIME üstbilgi dosya adını (yapılandırıldıysa) iletme


  > [!NOTE]
  > Sertifika yönetimi için Azure anahtar kasası kullanıyorsanız, izin vermek için anahtarları yapılandırdığınızdan emin olun. **şifrele** işlemi.
  > Aksi takdirde, AS2 kodlama başarısız olur.
  >
  > ![Keyvault şifresini çözer.](media/logic-apps-enterprise-integration-as2-encode/keyvault1.png)

## <a name="try-this-sample"></a>Bu örnek deneyin

Bir tam olarak işlevsel bir mantıksal uygulama ve örnek AS2 senaryo dağıtmak denemek için bkz [AS2 mantıksal uygulama şablonunu ve senaryo](https://azure.microsoft.com/documentation/templates/201-logic-app-as2-send-receive/).

## <a name="view-the-swagger"></a>Swagger görüntüleyin
Bkz: [ayrıntıları swagger](/connectors/as2/). 

## <a name="next-steps"></a>Sonraki adımlar
[Enterprise Integration Pack hakkında daha fazla bilgi](logic-apps-enterprise-integration-overview.md "Enterprise Integration Pack hakkında bilgi edinin") 

