---
title: Oluşturma ve ticari ortak sözleşmeleri - Azure Logic Apps'ı yönetme
description: Oluşturma ve Azure Logic Apps ve Enterprise Integration Pack kullanarak ticari ortaklar arasında sözleşmelerini yönetme
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: 447ffb8e-3e91-4403-872b-2f496495899d
ms.date: 04/05/2019
ms.openlocfilehash: 26d653b873e959f0804e0456ed87ee68c39413e5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64720679"
---
# <a name="create-and-manage-trading-partner-agreements-by-using-azure-logic-apps-and-enterprise-integration-pack"></a>Oluşturma ve Azure Logic Apps ve Enterprise Integration Pack kullanarak ticari ortak sözleşmeleri yönetme

A [ticari ortak](../logic-apps/logic-apps-enterprise-integration-partners.md) 
*sözleşmesi* organizasyonlar ve işletmeler sorunsuz bir şekilde alışverişleri sırasında kullanılacak belirli endüstri standardı protokol tanımlayarak birbirleriyle iletişim yardımcı olur. işletmeden işletmeye (B2B) iletileri. Sözleşmeleri, örneğin ortak avantajları sağlar:

* İyi bilinen bir biçimde kullanarak bilgi alışverişi kuruluşların olanak sağlar.
* B2B işlemlerini yaparken, verimliliği artırın.
* Oluşturma, yönetme ve kurumsal tümleştirme çözümleri oluşturmak için kullanmak kolaydır.

Bu makalede bir AS2, EDIFACT veya X12 oluşturulacağını gösterir kullanarak kurumsal tümleştirme çözümleri B2B senaryoları için oluşturma sırasında kullanabileceğiniz sözleşmesi [Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md) ve [Azure Logic Apps](../logic-apps/logic-apps-overview.md). Bir anlaşma oluşturduktan sonra ardından AS2, EDIFACT veya X12 kullanabilirsiniz B2B iletilerini değişimi için bağlayıcılar.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Henüz Azure aboneliğiniz yoksa, [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Bir [tümleştirme hesabı](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) sözleşmenizi ve diğer B2B yapıtlarını depolamak için. Bu tümleştirme hesabı, Azure aboneliğinizle ilişkili olmalıdır.

* En az iki [ortaklar](../logic-apps/logic-apps-enterprise-integration-partners.md) , tümleştirme hesabında zaten oluşturduğunuz. Bir anlaşma hem konak iş ortağı hem de Konuk iş ortağı gerektirir. Her iki iş ortakları, X 12 veya EDIFACT, AS2 gibi oluşturmak istediğiniz sözleşmesi olarak aynı "iş kimliği" niteleyicisi kullanmanız gerekir.

* İsteğe bağlı: Sözleşmenizi ve mantıksal uygulamanızın iş akışı başlatan tetikleyici kullanmasını istediğiniz mantıksal uygulaması. Tümleştirme hesabı ve B2B yapıtları yalnızca oluşturmak için bir mantıksal uygulama gerekmez. Ancak, mantıksal uygulama B2B yapıtları tümleştirme hesabınızdaki kullanabilmeniz için önce mantıksal uygulamanıza tümleştirme hesabınızı bağlamanız gerekir. Logic apps kullanmaya yeni başladıysanız gözden [Azure Logic Apps nedir](../logic-apps/logic-apps-overview.md) ve [hızlı başlangıç: İlk mantıksal uygulamanızı oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="create-agreements"></a>Anlaşmaları oluşturma

1. [Azure Portal](https://portal.azure.com) oturum açın.
Ana Azure menüsünde **tüm hizmetleri**. Arama kutusuna filtreniz olarak "tümleştirme" girin. Bu kaynak sonuçlardan seçin: **Tümleştirme hesaplarına genel bakış**

   ![Tümleştirme hesabı bulunamadı](./media/logic-apps-enterprise-integration-agreements/find-integration-accounts.png)

1. Altında **tümleştirme hesapları**, tümleştirme hesabı sözleşmesi oluşturmak istediğiniz yeri seçin.

   ![Tümleştirme hesabı sözleşmesi oluşturmak istediğiniz yeri seçin](./media/logic-apps-enterprise-integration-agreements/select-integration-account.png)

1. Sağ bölmede altında **bileşenleri**, seçin **sözleşmeleri** Döşe.

   !["Anlaşmaları" seçin](./media/logic-apps-enterprise-integration-agreements/agreement-1.png)

1. Altında **sözleşmeleri**, seçin **Ekle**. İçinde **Ekle** bölmesinde, örneğin, anlaşmanız hakkında bilgi sağlar:

   !["Ekle" öğesini seçin](./media/logic-apps-enterprise-integration-agreements/agreement-2.png)

   | Özellik | Gereklidir | Value | Açıklama |
   |----------|----------|-------|-------------|
   | **Ad** | Evet | <*Anlaşma adı*> | Sözleşmenize adı |
   | **Sözleşme türü** | Evet | **AS2**, **X12**, veya **EDIFACT** | Sözleşmenize protokol türü. Sözleşme dosyanızı oluşturduğunuzda, bu dosyanın içeriği sözleşme türüyle eşleşmelidir. | |  
   | **Konak iş ortağı** | Evet | <*konak iş ortağı adı*> | Konak iş ortağı sözleşmesi belirten kuruluşu temsil eder. |
   | **Konak kimliği** | Evet | <*konak iş ortağı tanımlayıcısı*> | Konak iş ortağının tanımlayıcısı |
   | **Konuk iş ortağı** | Evet | <*Konuk iş ortağı adı*> | Konuk iş ortağı yapan iş konak iş ortağı kuruluşu temsil eder. |
   | **Konuk kimliği** | Evet | <*Konuk iş ortağı tanımlayıcısı*> | Konuk iş ortağının tanımlayıcısı |
   | **Ayarları Al** | Değişir | Değişir | Bu özellikler, sözleşmesi tarafından alınan tüm gelen iletilerin nasıl ele alınacağını belirtin. Daha fazla bilgi için ilgili anlaşma türünü bakın: <p>- [AS2 ileti ayarları](../logic-apps/logic-apps-enterprise-integration-as2-message-settings.md) <br>- [EDIFACT iletisi ayarları](logic-apps-enterprise-integration-edifact.md) <br>- [Ayarları X12 iletisi](logic-apps-enterprise-integration-x12.md) |
   | **Gönderme ayarları** | Değişir | Değişir | Bu özellikler, sözleşmesi tarafından gönderilen tüm mesajları işlemek nasıl belirtin. Daha fazla bilgi için ilgili anlaşma türünü bakın: <p>- [AS2 ileti ayarları](../logic-apps/logic-apps-enterprise-integration-as2-message-settings.md) <br>- [EDIFACT iletisi ayarları](logic-apps-enterprise-integration-edifact.md) <br>- [Ayarları X12 iletisi](logic-apps-enterprise-integration-x12.md) |
   |||||

1. Bitirdiğinizde, sözleşmeniz oluşturma konusunda **Ekle** sayfasında **Tamam**ve tümleştirme hesabınıza döndürür.

   **Sözleşmeleri** listesi artık yeni sözleşmenize gösterir.

## <a name="edit-agreements"></a>Anlaşmaları Düzenle

1. İçinde [Azure portalında](https://portal.azure.com), ana Azure menüsünde **tüm hizmetleri**.

1. Arama kutusuna filtreniz olarak "tümleştirme" girin. Bu kaynak sonuçlardan seçin: **Tümleştirme hesaplarına genel bakış**

1. Altında **tümleştirme hesapları**, düzenlemek istediğiniz sözleşmesindeki tümleştirme hesabı seçin.

1. Sağ bölmede altında **bileşenleri**, seçin **sözleşmeleri** Döşe.

1. Altında **sözleşmeleri**, sözleşmenizi seçip seçin **Düzenle**.

1. Yapın ve ardından değişikliklerinizi kaydedin.

## <a name="delete-agreements"></a>Anlaşmaları Sil

1. İçinde [Azure portalında](https://portal.azure.com), ana Azure menüsünde **tüm hizmetleri**.

1. Arama kutusuna filtreniz olarak "tümleştirme" girin. Bu kaynak sonuçlardan seçin: **Tümleştirme hesaplarına genel bakış**

1. Altında **tümleştirme hesapları**, silmek istediğiniz sözleşmesindeki tümleştirme hesabı seçin.

1. Sağ bölmede altında **bileşenleri**, seçin **sözleşmeleri** Döşe.

1. Altında **sözleşmeleri**, sözleşmenizi seçip seçin **Sil**.

1. Seçilen anlaşmayı silmek istediğinizi onaylayın.

## <a name="next-steps"></a>Sonraki adımlar

* [AS2 iletilerini paylaşma](logic-apps-enterprise-integration-as2.md)
* [EDIFACT iletilerini paylaşma](logic-apps-enterprise-integration-edifact.md)
* [Exchange X12 iletileri](logic-apps-enterprise-integration-x12.md)
