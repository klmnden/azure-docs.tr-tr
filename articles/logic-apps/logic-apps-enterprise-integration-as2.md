---
title: B2B Kurumsal tümleştirme - Azure Logic Apps için AS2 iletileri
description: Azure Logic Apps Enterprise Integration Pack ile Exchange AS2 iletileri
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.date: 04/22/2019
ms.openlocfilehash: b494f6524e5105a95bc8a24a6fa2521abcca3f7b
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64729404"
---
# <a name="exchange-as2-messages-for-b2b-enterprise-integration-in-azure-logic-apps-with-enterprise-integration-pack"></a>AS2 iletilerini paylaşma için Enterprise Integration Pack ile Azure Logic apps'teki B2B Kurumsal tümleştirme

AS2 iletileri Azure Logic Apps ile çalışmak için tetikleyiciler ve Eylemler AS2 iletişimi Yönetimi sağlayan AS2 Bağlayıcısı kullanabilirsiniz. Örneğin, güvenlik ve güvenilirlik ileti iletilirken kurmak için bu eylemleri kullanabilirsiniz:

* [**AS2 iletisine kodlayın** eylem](#encode) şifreleme, dijital imza ve onayları ileti değerlendirme bildirimleri (MDN) aracılığıyla sağlamaya yönelik Yardımı takası destekler. Örneğin, bu eylem, AS2/HTTP üst bilgilerini uygular ve yapılandırıldığında bu görevleri gerçekleştirir:

  * Giden iletileri imzalar.
  * Giden iletileri şifreler.
  * İleti sıkıştırır.
  * Dosya adını arar iletir.

* [**AS2 iletisinin kodunu çözün** eylem](#decode) şifre çözme, dijital imza ve bildirimleri üzerinden ileti değerlendirme bildirimleri (MDN) sağlamak için. Örneğin, bu eylem, bu görevleri gerçekleştirir: 

  * AS2/HTTP üst bilgilerini işler.
  * Özgün giden iletileri ile alınan çok Mdn'leri mutabık kılar.
  * Güncelleştirir ve takası veritabanında kayıt ilişkilendirir.
  * AS2 durum raporlama için kayıtları yazar.
  * Çıktı yükü içeriği base64 ile kodlanmış olarak.
  * Mdn'leri gerekli olup olmadığını belirler. AS2 üzerinde sözleşme belirler Mdn'leri zaman uyumlu veya zaman uyumsuz olması gerekir.
  * AS2 anlaşmasına göre zaman uyumlu veya zaman uyumsuz Mdn'leri oluşturur.
  * Özellikler ve bağıntı belirteçleri üzerinde Mdn'leri ayarlar.

  Bu eylem ayrıca yapılandırıldığında bu görevleri gerçekleştirir:

  * İmzayı doğrular.
  * İletilerin şifresini çözer.
  * İleti açar. 
  * Denetleyin ve ileti kimliği yinelenmesine izin verme.

Bu makalede, AS2 kodlama ekleme ve var olan bir mantıksal uygulama kod çözme eylemleri gösterir.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Henüz Azure aboneliğiniz yoksa, [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* AS2 bağlayıcı ve mantıksal uygulamanızın iş akışı başlatan tetikleyici olarak kullanmak istediğiniz mantıksal uygulaması. AS2 bağlayıcı yalnızca Eylemler, Tetikleyiciler değil sağlar. Logic apps kullanmaya yeni başladıysanız gözden [Azure Logic Apps nedir](../logic-apps/logic-apps-overview.md) ve [hızlı başlangıç: İlk mantıksal uygulamanızı oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

* Bir [tümleştirme hesabı](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) , Azure aboneliğinizle ilişkili olan ve AS2 Bağlayıcısı'nı kullanmayı planladığınız burada mantıksal uygulamaya bağlandı. Hem, mantıksal uygulama ve tümleştirme hesabı aynı konumda veya Azure bölgesinde bulunmalıdır.

* En az iki [ortaklar](../logic-apps/logic-apps-enterprise-integration-partners.md) zaten tümleştirme hesabınızdaki AS2 kimliği niteleyicisi kullanarak tanımladığınız olduğunu.

* AS2 Bağlayıcısı'nı kullanabilmeniz için önce bir AS2 oluşturmalısınız [sözleşmesi](../logic-apps/logic-apps-enterprise-integration-agreements.md) ticaret iş ortakları ve deposu arasındaki bu, tümleştirme hesabındaki sözleşmeyi.

* Kullanırsanız [Azure anahtar kasası](../key-vault/key-vault-overview.md) kasa anahtarlarınızı izin vermek için sertifika yönetimi, denetleyin **şifrele** ve **şifresini** operations. Aksi takdirde, kodlama ve kod çözme eylemleri başarısız.

  Azure portalında, anahtar Kasası'na gidin, kasa anahtarının görüntülemek **işlemlerine izin**, doğrulayın **şifrele** ve **şifresini** operations seçilir.

  ![Anahtar kasası işlemleri denetleyin](media/logic-apps-enterprise-integration-as2/vault-key-permitted-operations.png)

<a name="encode"></a>

## <a name="encode-as2-messages"></a>AS2 iletilerini kodlar

1. Henüz kaydolmadıysanız, [Azure portalında](https://portal.azure.com), mantıksal Uygulama Tasarımcısı'nda mantıksal uygulamanızı açın.

1. Tasarımcısı'nda, mantıksal uygulamanızın yeni bir eylem ekleyin. 

1. Altında **eylem seçin** ve arama kutusunda **tüm**. Arama kutusuna "as2 kodlama" girin ve şu eylemi seçin: **AS2 iletisine kodlayın**.

   !["Encode AS2 iletisi" seçin](./media/logic-apps-enterprise-integration-as2/select-as2-encode.png)

1. Mevcut bir bağlantıyı tümleştirme hesabınız yoksa, artık bu bağlantıyı oluşturmak için istenir. Bağlantınızı adlandırın, bağlanma ve istediğiniz tümleştirme hesabı seçin **Oluştur**.

   ![Tümleştirme hesabına bağlantı oluşturma](./media/logic-apps-enterprise-integration-as2/as2-create-connection.png)  
 
1. Artık bu özellikler için bilgileri sağlayın:

   | Özellik | Açıklama |
   |----------|-------------|
   | **AS2-gelen** | AS2 sözleşmenize tarafından belirtilen ileti gönderen tanımlayıcısı |
   | **AS2-için** | AS2 sözleşmenize tarafından belirtilen ileti alıcı tanımlayıcısı |
   | **Gövde** | İleti yükü |
   |||

   Örneğin:

   ![İleti kodlama özellikleri](./media/logic-apps-enterprise-integration-as2/as2-message-encoding-details.png)

<a name="decode"></a>

## <a name="decode-as2-messages"></a>AS2 iletilerini kodunu çözme

1. Henüz kaydolmadıysanız, [Azure portalında](https://portal.azure.com), mantıksal Uygulama Tasarımcısı'nda mantıksal uygulamanızı açın.

1. Tasarımcısı'nda, mantıksal uygulamanızın yeni bir eylem ekleyin. 

1. Altında **eylem seçin** ve arama kutusunda **tüm**. Arama kutusuna "as2 kod çözme" girin ve şu eylemi seçin: **AS2 iletisinin kodunu çözün**

   !["Decode AS2 message" seçin](media/logic-apps-enterprise-integration-as2/select-as2-decode.png)

1. Mevcut bir bağlantıyı tümleştirme hesabınız yoksa, artık bu bağlantıyı oluşturmak için istenir. Bağlantınızı adlandırın, bağlanma ve istediğiniz tümleştirme hesabı seçin **Oluştur**.

   ![Tümleştirme hesabına bağlantı oluşturma](./media/logic-apps-enterprise-integration-as2/as2-create-connection.png)  

1. İçin **gövdesi** ve **üstbilgileri**, önceki tetikleyici veya eylemi çıkışları bu değerleri seçin.

   Örneğin, mantıksal uygulamanızı istek tetikleyicisi aracılığıyla iletileri alan varsayalım. Bu tetikleyiciyi çıkışları seçebilirsiniz.

   ![Gövde ve üstbilgileri, istek çıkışları seçin](media/logic-apps-enterprise-integration-as2/as2-message-decoding-details.png) 

## <a name="sample"></a>Örnek

Bir tam olarak işlevsel bir mantıksal uygulama ve örnek AS2 senaryo dağıtmak denemek için bkz [AS2 mantıksal uygulama şablonunu ve senaryo](https://azure.microsoft.com/documentation/templates/201-logic-app-as2-send-receive/).

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Tetikleyiciler ve Eylemler sınırları, bağlayıcının Openapı'nin açıklandığı gibi teknik ayrıntılar için (önceki adıyla Swagger) dosyası, bkz: [bağlayıcının başvuru sayfası](/connectors/as2/).

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinin [Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md)
