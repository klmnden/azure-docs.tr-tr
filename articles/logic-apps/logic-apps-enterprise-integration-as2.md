---
title: "B2B Kurumsal tümleştirme - Azure Logic Apps için AS2 iletileri | Microsoft Docs"
description: "Exchange AS2 iletileri Azure Logic Apps ile B2B Kurumsal tümleştirme"
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: c9b7e1a9-4791-474c-855f-988bd7bf4b7f
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/08/2017
ms.author: LADocs; mandia
ms.openlocfilehash: 91b2f16611b88aa4b9395ca301d88042065ad9dd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="exchange-as2-messages-for-enterprise-integration-with-logic-apps"></a>Logic apps ile Kurumsal tümleştirme için Exchange AS2 iletileri

Azure mantıksal uygulamaları için AS2 iletileri değiştirebilir önce AS2 sözleşmesi oluşturun ve bu anlaşma tümleştirme hesabınızı depolamak. AS2 sözleşmesi oluşturmak için adımlar şunlardır.

## <a name="before-you-start"></a>Başlamadan önce

Gereksinim duyduğunuz öğeleri şöyledir:

* Bir [tümleştirme hesabını](../logic-apps/logic-apps-enterprise-integration-accounts.md) , daha önce tanımlanan ve Azure aboneliğinizle ilişkili
* En az iki [ortakları](logic-apps-enterprise-integration-partners.md) , zaten tümleştirme hesabınızda tanımlanır ve altında AS2 niteleyici ile yapılandırılmış **iş kimlikleri**

> [!NOTE]
> Bir sözleşme oluşturduğunuzda anlaşma dosyasının içeriğini anlaşma türünü eşleşmesi gerekir.    

Çalıştırdıktan sonra [tümleştirme hesap oluşturmak](../logic-apps/logic-apps-enterprise-integration-accounts.md) ve [ortakları eklemek](logic-apps-enterprise-integration-partners.md), aşağıdaki adımları izleyerek bir AS2 sözleşmesi oluşturabilirsiniz.

## <a name="create-an-as2-agreement"></a>AS2 sözleşmesi oluşturma

1.  [Azure portalı](http://portal.azure.com "Azure portalı") oturumunu açın.  

2.  Sol menüden seçin **daha fazla hizmet**. Arama kutusuna **tümleştirme** filtre olarak. Sonuçlar listesinde **tümleştirme hesapları**.

    > [!TIP]
    > Görmüyorsanız, **daha fazla hizmet**, menü ilk genişletmeniz gerekebilir. Daraltılmış menüsünün üstünde seçin **Göster menüsü**.

    !["Tümleştirme hesabı" daha fazla hizmet, "tümleştirme" filtresini seçin.](./media/logic-apps-enterprise-integration-agreements/overview-1.png)

3. İçinde **tümleştirme hesapları** açar, dikey penceresinde, anlaşmayı oluşturmak istediğiniz tümleştirme hesabını seçin.
Herhangi bir tümleştirme hesabı görmüyorsanız, [oluşturmak ilk](../logic-apps/logic-apps-enterprise-integration-accounts.md "tümleştirme hesapları hakkında").  

    ![Tümleştirme hesabı anlaşmayı oluşturulacağı konumu seçin](./media/logic-apps-enterprise-integration-overview/overview-3.png)

4. Seçin **anlaşmaları** döşeme. Anlaşmaları döşeme yoksa, ilk kutucuğu ekleyin.

    !["Anlaşmaları" kutucuğunu seçin](./media/logic-apps-enterprise-integration-agreements/agreement-1.png)

5. Açılır anlaşmaları dikey penceresinde, seçin **Ekle**.

    !["Ekle" yi seçin](./media/logic-apps-enterprise-integration-agreements/agreement-2.png)

6. Altında **Ekle**, girin bir **adı** sözleşmenizi için. İçin **anlaşma türünü**seçin **AS2**. Seçin **ana iş ortağı**, **konak kimliği**, **Konuk iş ortağı**, ve **Konuk kimlik** sözleşmenizi için.

    ![Anlaşma ayrıntılarını sağlayın](./media/logic-apps-enterprise-integration-agreements/agreement-3.png)  

    | Özellik | Açıklama |
    | --- | --- |
    | Ad |Anlaşma adı |
    | Anlaşma türü | AS2 olmalıdır |
    | Ana bilgisayar iş ortağı |Bir anlaşma hem konak hem de Konuk iş ortağı gerekir. Ana bilgisayar iş ortağı sözleşmesi yapılandırır kuruluşu temsil eder. |
    | Ana bilgisayar kimliği |Ana makine ortak bir tanımlayıcı |
    | Konuk iş ortağı |Bir anlaşma hem konak hem de Konuk iş ortağı gerekir. Konuk iş ortağı ana iş ortağı iş yapmakta kuruluşu temsil eder. |
    | Konuk kimlik |Konuk iş ortağı için bir tanımlayıcı |
    | Ayarlarını alma |Bu özellikleri tüm anlaşmanın tarafından alınan iletileri için geçerlidir. |
    | Gönderme ayarları |Bu özellikleri anlaşmanın tarafından gönderilen tüm iletiler için geçerlidir. |

## <a name="configure-how-your-agreement-handles-received-messages"></a>Anlaşma tanıtıcıları iletileri nasıl alınan yapılandırma

Anlaşma özelliklerini ayarladıysanız, bu anlaşmanın nasıl tanımlar ve bu anlaşma ile ortaktan alınan gelen iletileri işleme yapılandırabilirsiniz.

1.  Altında **Ekle**seçin **alma ayarı**.
İletiler birlikte alış verişleri iş ortağı ile sözleşmenizde göre bu özelliklerini yapılandırın. Özellik açıklamaları için bu bölümündeki tabloya bakın.

    !["Alma ayarlarını" yapılandırma](./media/logic-apps-enterprise-integration-agreements/agreement-4.png)

2. İsteğe bağlı olarak, gelen iletileri özelliklerini seçerek geçersiz kılabilirsiniz **ileti özelliklerini geçersiz kılma**.

3. İmzalanacak gelen tüm iletilerin gerektirecek şekilde seçin **ileti imzalanmasını**. Gelen **sertifika** listesinde, var olan seçin [Konuk iş ortağı ortak sertifika](../logic-apps/logic-apps-enterprise-integration-certificates.md) iletileri imza doğrulama. Veya, yoksa, sertifika oluşturun.

4.  Şifrelenecek gelen tüm iletilerin gerektirecek şekilde seçin **ileti şifrelenir**. Gelen **sertifika** listesinde, var olan seçin [ana iş ortağı özel sertifika](../logic-apps/logic-apps-enterprise-integration-certificates.md) gelen iletilerin şifresini çözmek üzere. Veya, yoksa, sertifika oluşturun.

5. Sıkıştırılacak iletileri gerektirecek şekilde seçin **iletisi sıkıştırılmış**.

6. Bir zaman uyumlu değerlendirme bildirim iletisi (MDN) alınan iletileri göndermek için seçin **Gönder MDN**.

7. Alınan iletilerin imzalı MDNs göndermek için seçin **gönderme imzalı MDN**.

8. Alınan iletilerin zaman uyumsuz MDNs göndermek için seçin **gönderme zaman uyumsuz MDN**.

9. Bitirdikten sonra seçerek ayarlarınızı kaydettiğinizden emin olun **Tamam**.

Artık sözleşmenizi seçili ayarlarınızı uygun gelen iletileri işlemek hazırdır.

| Özellik | Açıklama |
| --- | --- |
| İleti özelliklerini geçersiz kılma |Alınan iletilerin özelliklerinde geçersiz kılınabilir gösterir. |
| İletinin imzalanmış olması gerekir |İletileri dijital olarak imzalanmasını gerektirir. Konuk iş ortağı ortak sertifika imza doğrulaması için yapılandırın.  |
| İletinin şifrelenmiş olması gerekir |İletileri şifrelenmesini gerektirir. Olmayan şifrelenmiş iletileri reddedilir. İletilerin şifresini çözmek için ana iş ortağı özel sertifika yapılandırın.  |
| İletinin sıkıştırılmış olması gerekir |Sıkıştırılacak iletileri gerektirir. Olmayan sıkıştırılmış iletiler reddedilir. |
| MDN metin |İletiyi gönderenin gönderilmek üzere varsayılan ileti değerlendirme bildirim (MDN). |
| MDN Gönder |Gönderilecek MDNs gerektirir. |
| İmzalı MDN Gönder |MDNs imzalanmasını gerektirir. |
| MIC algoritması |İletileri imzalamak için kullanılacak algoritmayı seçin. |
| Zaman uyumsuz MDN Gönder | Zaman uyumsuz olarak gönderilecek iletilerin gerektirir. |
| URL | URL MDNs göndermek istediğiniz yeri belirtin. |

## <a name="configure-how-your-agreement-sends-messages"></a>Nasıl sözleşmenizi iletileri gönderir yapılandırın

Bu anlaşma nasıl tanımlar ve bu anlaşma ile iş ortaklarınıza göndermek giden iletilerini işleme yapılandırabilirsiniz.

1.  Altında **Ekle**seçin **gönderme ayarları**.
İletiler birlikte alış verişleri iş ortağı ile sözleşmenizde göre bu özelliklerini yapılandırın. Özellik açıklamaları için bu bölümündeki tabloya bakın.

    !["Gönderme ayarları" özelliklerini ayarla](./media/logic-apps-enterprise-integration-agreements/agreement-51.png)

2. Ortağınıza imzalanmış iletiler göndermek için seçin **ileti imzalamayı etkinleştirmek**. İletileri olarak imzalanması için **MIC algoritması** listesinde *ana iş ortağı özel sertifika MIC algoritması*. Ve **sertifika** listesinde, var olan seçin [ana iş ortağı özel sertifika](../logic-apps/logic-apps-enterprise-integration-certificates.md).

3. İş ortağı şifrelenmiş iletileri göndermek için seçin **etkinleştirmek ileti şifreleme**. İçinde iletileri şifrelemek için **şifreleme algoritması** listesinde *Konuk iş ortağı ortak sertifika algoritması*.
Ve **sertifika** listesinde, var olan seçin [Konuk iş ortağı ortak sertifika](../logic-apps/logic-apps-enterprise-integration-certificates.md).

4. İleti sıkıştırılacak seçin **ileti sıkıştırmayı etkinleştir**.

5. HTTP content-type üstbilgisi tek bir satıra unfold için seçin **Unfold HTTP üstbilgileri**.

6. Gönderilen iletiler için zaman uyumlu MDNs almak için seçin **isteği MDN**.

7. Gönderilen iletiler için imzalanmış MDNs almak için seçin **isteği imzalı MDN**.

8. Gönderilen iletiler için zaman uyumsuz MDNs almak için seçin **isteği zaman uyumsuz MDN**. Bu seçeneği belirlerseniz, MDNs gönderileceği yeri URL'sini girin.

9. İnkar giriş gerektirecek şekilde seçin **etkinleştirmek NRR**.  

10. MIC veya AS2 ileti veya MDN giden üstbilgilerinde imzalama kullanılacak algoritması biçimini belirtmek için işaretleyin **SHA2 algoritması biçimi**.  

11. Bitirdikten sonra seçerek ayarlarınızı kaydettiğinizden emin olun **Tamam**.

Artık sözleşmenizi seçili ayarlarınızı uygun giden iletileri işlemek hazırdır.

| Özellik | Açıklama |
| --- | --- |
| İleti imzalama etkinleştir |İmzalanacak sözleşmesi gönderilen tüm iletiler gerektirir. |
| MIC algoritması |İletileri imzalamak için kullanılacak algoritmayı. İletileri imzalamak için ana iş ortağı özel sertifika MIC algoritması yapılandırır. |
| Sertifika |İletileri imzalamak için kullanılacak sertifikayı seçin. İletileri imzalamak için ana iş ortağı özel sertifika yapılandırır. |
| İleti şifreleme etkinleştir |Bu anlaşma gönderilen tüm iletiler şifrelenmesi gerektirir. İletileri şifrelemek için konuk iş ortağı ortak sertifika algoritması yapılandırır. |
| Şifreleme algoritması |İleti şifreleme için kullanılacak şifreleme algoritması. İletileri şifrelemek için konuk iş ortağı ortak sertifika yapılandırır. |
| Sertifika |İletileri şifrelemek için kullanılacak sertifikayı. İletileri şifrelemek için konuk iş ortağı özel sertifika yapılandırır. |
| İleti sıkıştırmayı etkinleştir |Bu anlaşma gönderilen tüm iletiler sıkıştırılması gerektirir. |
| HTTP üstbilgileri unfold |HTTP content-type üstbilgisi tek bir satır üzerine yerleştirir. |
| İstek MDN |Bu anlaşma gönderilen tüm iletiler için bir MDN gerektirir. |
| İmzalı MDN isteği |İmzalanması için bu sözleşmeyi gönderilen tüm MDNs gerektirir. |
| Zaman uyumsuz MDN isteği |Bu anlaşma gönderilmek üzere zaman uyumsuz MDNs gerektirir. |
| URL |URL MDNs göndermek istediğiniz yeri belirtin. |
| NRR etkinleştir |İnkar edilemez makbuz (NRR) kanıt sağlar iletişimi özniteliği, gerektirir verileri gibi ele alınan. |
| SHA2 algoritması biçimi |MIC veya AS2 ileti veya MDN giden üstbilgilerinde imzalama kullanılacak algoritması biçimini seçin |

## <a name="find-your-created-agreement"></a>Oluşturulan sözleşmenizi Bul

1.  Üzerinde anlaşma özelliklerinizi ayarlama işlemini tamamladıktan sonra **Ekle** dikey penceresinde, seçin **Tamam** sözleşmenizi oluşturmayı tamamlamak ve tümleştirme hesabı dikey penceresine geri dönün.

    Yeni eklenen sözleşmenizi artık görünür, **anlaşmaları** listesi.

2.  Ayrıca, tümleştirme hesabına genel bakış sözleşmelerinizi görüntüleyebilirsiniz. Tümleştirme hesabı dikey penceresinde, seçin **genel bakış**seçeneğini belirleyip **anlaşmaları** döşeme. 

    ![Tüm anlaşmalar görüntülemek için "Anlaşmaları" kutucuğunu seçin](./media/logic-apps-enterprise-integration-agreements/agreement-6.png)

## <a name="view-the-swagger"></a>Swagger görüntüleyin
Bkz: [ayrıntıları swagger](/connectors/as2/). 

## <a name="next-steps"></a>Sonraki adımlar
* [Enterprise Integration Pack hakkında daha fazla bilgi](logic-apps-enterprise-integration-overview.md "Enterprise Integration Pack hakkında bilgi edinin")  
