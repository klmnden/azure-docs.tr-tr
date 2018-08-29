---
title: AS2 iletilerini B2B Kurumsal tümleştirme - Azure Logic Apps | Microsoft Docs
description: AS2 iletilerini paylaşma için Enterprise Integration Pack ile Azure Logic apps'teki B2B Kurumsal tümleştirme
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: c9b7e1a9-4791-474c-855f-988bd7bf4b7f
ms.date: 06/08/2017
ms.openlocfilehash: 2604cdd6bf758858328c2d30fc4cde535f0a7148
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2018
ms.locfileid: "43124671"
---
# <a name="exchange-as2-messages-for-b2b-enterprise-integration-in-azure-logic-apps-with-enterprise-integration-pack"></a>AS2 iletilerini paylaşma için Enterprise Integration Pack ile Azure Logic apps'teki B2B Kurumsal tümleştirme

AS2 iletilerini Azure Logic Apps için exchange önce bir AS2 sözleşmesi oluşturun ve bu sözleşme, tümleştirme hesabında depolamak gerekir. Bir AS2 sözleşmesi oluşturmak için adımlar aşağıda verilmiştir.

## <a name="before-you-start"></a>Başlamadan önce

Gereksinim duyduğunuz öğeleri şu şekildedir:

* Bir [tümleştirme hesabı](../logic-apps/logic-apps-enterprise-integration-accounts.md) zaten tanımlanmış ve Azure aboneliğinizle ilişkili
* En az iki [iş ortakları](logic-apps-enterprise-integration-partners.md) , zaten tümleştirme hesabınızdaki tanımlanır ve AS2 niteleyicisi altında yapılandırılmış **iş kimlikleri**

> [!NOTE]
> Bir sözleşme oluşturduğunuzda sözleşme dosyasının içeriğini sözleşme türüyle eşleşmelidir.    

Çalıştırdıktan sonra [tümleştirme hesabı oluşturma](../logic-apps/logic-apps-enterprise-integration-accounts.md) ve [iş ortakları ekleme](logic-apps-enterprise-integration-partners.md), aşağıdaki adımları izleyerek bir AS2 sözleşmesi oluşturabilirsiniz.

## <a name="create-an-as2-agreement"></a>Bir AS2 sözleşmesi oluşturma

1.  [Azure portalı](http://portal.azure.com "Azure portalı") oturumunu açın.  

2. Ana Azure menüsünde **tüm hizmetleri**. Arama kutusuna "tümleştirme" girin ve ardından **tümleştirme hesapları**.

   ![Tümleştirme hesabı bulunamadı](./media/logic-apps-enterprise-integration-as2/overview-1.png)

   > [!TIP]
   > Görmüyorsanız **tüm hizmetleri**, menü ilk genişletmeniz gerekebilir. Daraltılmış menüsünün üstünde, seçin **metin etiketlerini göster**.

3. Altında **tümleştirme hesapları**, tümleştirme hesabı sözleşmesi oluşturmak istediğiniz yeri seçin.

   ![Tümleştirme hesabı sözleşmesi oluşturmak istediğiniz yeri seçin](./media/logic-apps-enterprise-integration-overview/overview-3.png)

4. Seçin **sözleşmeleri** Döşe. Anlaşmaları kutucuk yoksa kutucuk önce ekleyin.

    !["Anlaşmaları" kutucuğunu seçin](./media/logic-apps-enterprise-integration-as2/agreement-1.png)

5. Altında **sözleşmeleri**, seçin **Ekle**.

    !["Ekle" öğesini seçin](./media/logic-apps-enterprise-integration-as2/agreement-2.png)

6. Altında **Ekle**, girin bir **adı** sözleşmenize için. İçin **sözleşme türü**seçin **AS2**. Seçin **konak iş ortağı**, **konak kimliği**, **Konuk iş ortağı**, ve **Konuk kimlik** sözleşmenize için.

    ![Anlaşma ayrıntılarını sağlayın](./media/logic-apps-enterprise-integration-as2/agreement-3.png)  

    | Özellik | Açıklama |
    | --- | --- |
    | Ad |Anlaşma adı |
    | Sözleşme Türü | AS2 olmalıdır |
    | Konak İş Ortağı |Bir anlaşma hem konak hem de Konuk iş ortağı gerekir. Konak iş ortağı sözleşmesi yapılandıran kuruluşu temsil eder. |
    | Konak Kimliği |Konak iş ortağı için bir tanımlayıcı |
    | Konuk İş Ortağı |Bir anlaşma hem konak hem de Konuk iş ortağı gerekir. Konuk iş ortağı, konak iş ortağı iş yapmakta kuruluşu temsil eder. |
    | Konuk Kimliği |Konuk iş ortağı için bir tanımlayıcı |
    | Ayarları Al |Bu özellikler, bir anlaşma tarafından alınan tüm iletileri için geçerlidir. |
    | Gönderme Ayarları |Bu özellikler, bir anlaşma tarafından gönderilen tüm iletiler için geçerlidir. |

## <a name="configure-how-your-agreement-handles-received-messages"></a>Nasıl iletileri, anlaşma tanıtıcıları alınan yapılandırma

Sözleşme özelliklerini ayarladıysanız, işbu sözleşme nasıl tanımlar ve işbu sözleşme aracılığıyla iş ortağınız alınan gelen iletileri işleyen yapılandırabilirsiniz.

1.  Altında **Ekle**seçin **alma ayarı**.
Sizinle iletiler birbiriyle değiştirir iş ortaklarıyla sözleşmenize göre bu özelliklerini yapılandırın. Özellik açıklamaları için bu bölümdeki tabloya bakın.

    !["Alma ayarlarını" yapılandırma](./media/logic-apps-enterprise-integration-as2/agreement-4.png)

2. İsteğe bağlı olarak, gelen iletileri özelliklerini seçerek geçersiz kılabilirsiniz **ileti özelliklerini geçersiz kılma**.

3. İmzalanacak gelen tüm iletileri zorunlu kılmak için seçin **ileti imzalanmasını**. Gelen **sertifika** listesinde, var olan bir seçin [Konuk iş ortağı ortak sertifika](../logic-apps/logic-apps-enterprise-integration-certificates.md) iletileri imzayı doğrulamak için. Veya, yoksa, bir sertifika oluşturun.

4.  Şifrelenmiş gelen tüm iletileri zorunlu kılmak için seçin **ileti şifrelenir**. Gelen **sertifika** listesinde, var olan bir seçin [konak iş ortağı özel sertifika](../logic-apps/logic-apps-enterprise-integration-certificates.md) gelen iletilerin şifresini çözmek için. Veya, yoksa, bir sertifika oluşturun.

5. Sıkıştırılacak iletileri zorunlu kılmak için seçin **iletisi sıkıştırılmış**.

6. Alınan iletiler için (MDN) zaman uyumlu ileti değerlendirme bildirim göndermek için seçin **MDN Gönder**.

7. İmzalı Mdn'leri alınan iletileri göndermek için seçin **imzalı MDN Gönder**.

8. Alınan iletiler için zaman uyumsuz Mdn'leri göndermek için seçin **zaman uyumsuz MDN Gönder**.

9. Bitirdikten sonra seçerek ayarlarınızı kaydettiğinizden emin olun **Tamam**.

Artık, anlaşmanız seçili ayarlarınıza uygun gelen iletileri işlemek hazırdır.

| Özellik | Açıklama |
| --- | --- |
| İleti özelliklerini geçersiz kıl |Alınan iletilerin özelliklerinde kılınabileceğini belirtir. |
| İletinin imzalanmış olması gerekir |İletileri dijital olarak imzalanmasını gerektirir. Konuk iş ortağı ortak sertifika imza doğrulaması için yapılandırın.  |
| İletinin şifrelenmiş olması gerekir |İletileri şifrelenmesini gerektirir. Olmayan şifrelenmiş iletileri reddedilir. İletilerin şifresini çözmek için konak iş ortağı özel sertifika yapılandırın.  |
| İletinin sıkıştırılmış olması gerekir |Sıkıştırılacak iletileri gerektirir. İletileri olmayan sıkıştırılmış reddedilir. |
| MDN Metni |İletiyi gönderenin için gönderilecek varsayılan ileti değerlendirme bildirim (MDN). |
| MDN gönder |Gönderilecek Mdn'leri gerektirir. |
| İmzalı MDN gönder |Mdn'leri imzalanmasını gerektirir. |
| MIC Algoritması |İletileri imzalamak için kullanılacak algoritmayı seçin. |
| Zaman uyumsuz MDN gönder | Zaman uyumsuz olarak gönderilecek iletilerin gerektirir. |
| URL'si | URL'yi Mdn'leri gönderileceği adresi belirtin. |

## <a name="configure-how-your-agreement-sends-messages"></a>Sözleşmenize iletileri nasıl göndereceğini yapılandırın

İşbu sözleşme nasıl tanımlar ve işbu sözleşme aracılığıyla iş ortaklarınızla göndermek giden iletileri işleyen yapılandırabilirsiniz.

1.  Altında **Ekle**seçin **gönderme ayarları**.
Sizinle iletiler birbiriyle değiştirir iş ortaklarıyla sözleşmenize göre bu özelliklerini yapılandırın. Özellik açıklamaları için bu bölümdeki tabloya bakın.

    !["Gönderme ayarları" özelliklerini ayarlama](./media/logic-apps-enterprise-integration-as2/agreement-51.png)

2. Ortağınıza imzalı ileti göndermek için seçin **ileti imzalamayı etkinleştir**. Buna, iletileri imzalamak için **MIC algoritması** listesinden *konak iş ortağı özel sertifika MIC algoritması*. Ve **sertifika** listesinde, var olan bir seçin [konak iş ortağı özel sertifika](../logic-apps/logic-apps-enterprise-integration-certificates.md).

3. İş ortağı şifrelenmiş iletileri göndermek için şunu seçin **ileti şifrelemeyi etkinleştir**. İçinde iletileri şifrelemek için **şifreleme algoritması** listesinden *Konuk iş ortağı ortak sertifika algoritması*.
Ve **sertifika** listesinde, var olan bir seçin [Konuk iş ortağı ortak sertifika](../logic-apps/logic-apps-enterprise-integration-certificates.md).

4. İleti sıkıştırılacak seçin **ileti sıkıştırmayı etkinleştir**.

5. HTTP content-type üstbilgisi tek bir satıra unfold seçin **Unfold HTTP üstbilgileri**.

6. Zaman uyumlu Mdn'leri gönderilen iletileri almak için seçin **MDN iste**.

7. İmzalı Mdn'leri gönderilen iletileri almak için seçin **imzalı MDN isteği**.

8. Zaman uyumsuz Mdn'leri gönderilen iletileri almak için seçin **isteği zaman uyumsuz MDN**. Bu seçeneği belirlerseniz Mdn'leri gönderileceği URL'sini girin.

9. İnkar giriş zorunlu kılmak için seçin **etkinleştirme NRR**.  

10. MIC veya imzalama AS2 iletisinin veya MDN'nin giden üst bilgilerinde kullanılacak algoritması biçimi belirtmek için seçin **SHA2 algoritması biçimi**.  

11. Bitirdikten sonra seçerek ayarlarınızı kaydettiğinizden emin olun **Tamam**.

Artık, anlaşmanız seçili ayarlarınıza uygun giden iletileri işlemek hazırdır.

| Özellik | Açıklama |
| --- | --- |
| İleti imzalamayı etkinleştir |İmzalanacak anlaşmamdan gönderilen tüm iletiler gerektirir. |
| MIC Algoritması |İletileri imzalamak için kullanılacak algoritmayı. Konak iş ortağı özel sertifika MIC algoritması, iletileri imzalamak için yapılandırır. |
| Sertifika |İletileri imzalamak için kullanılacak sertifikayı seçin. İletileri imzalamak için konak iş ortağı özel sertifikasını yapılandırır. |
| İleti şifrelemeyi etkinleştir |Bu anlaşmamdan gönderilen tüm iletilerin şifreleme gerektirir. Konuk iş ortağı ortak sertifika algoritması'iletileri şifrelemek için yapılandırır. |
| Şifreleme Algoritması |İleti şifreleme için kullanılacak şifreleme algoritması. İletileri şifrelemek için konuk iş ortağı ortak sertifikayı yapılandırır. |
| Sertifika |İletileri şifrelemek için kullanılacak sertifika. İletileri şifrelemek için konuk iş ortağı özel sertifikasını yapılandırır. |
| İleti sıkıştırmayı etkinleştir |Bu anlaşmamdan gönderilen tüm iletilerin sıkıştırma gerektirir. |
| HTTP üst bilgilerini aç |HTTP content-type üstbilgisi tek bir satır üzerine yerleştirir. |
| MDN iste |Bu anlaşmamdan gönderilen tüm iletiler için bir MDN gerektirir. |
| İmzalı MDN iste |İşbu sözleşme ile imzalanması için gönderilen tüm Mdn'leri gerektirir. |
| Zaman uyumsuz MDN iste |Zaman uyumsuz Mdn'leri, işbu sözleşme gönderilmesini gerektirir. |
| URL'si |URL'yi Mdn'leri gönderileceği adresi belirtin. |
| NRR'yi etkinleştir |İnkar edilemez makbuz (NRR) kanıt sağlayan bir iletişim özniteliği, gerektiren veri olarak ele alınan. |
| SHA2 Algoritması biçimi |MIC veya imzalama AS2 iletisinin veya MDN'nin giden üst bilgilerinde kullanılacak algoritması biçimi seçin |

## <a name="find-your-created-agreement"></a>Oluşturulan sözleşmenize Bul

1. Üzerinde anlaşma özelliklerinizi ayarlama işlemini tamamladıktan sonra **Ekle** sayfasında **Tamam** sözleşmenize oluşturma işlemini tamamladıktan ve tümleştirme hesabınıza döndürmek için.

    Yeni eklenen sözleşmenize artık görünür, **sözleşmeleri** listesi.

2. Tümleştirme hesabı genel bakış sözleşmelerinizi da görüntüleyebilirsiniz. Tümleştirme hesabı menüsünde **genel bakış**, ardından **sözleşmeleri** Döşe. 

   ![Tüm anlaşmalar görüntülemek için "Anlaşmaları" kutucuğunu seçin](./media/logic-apps-enterprise-integration-as2/agreement-6.png)

## <a name="view-the-swagger"></a>Swagger görüntüleyin
Bkz: [ayrıntıları swagger](/connectors/as2/). 

## <a name="next-steps"></a>Sonraki adımlar
* [Enterprise Integration Pack hakkında daha fazla bilgi](logic-apps-enterprise-integration-overview.md "Enterprise Integration Pack hakkında bilgi edinin")  
