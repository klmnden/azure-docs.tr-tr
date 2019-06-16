---
title: EDIFACT iletilerini - Azure Logic Apps kodlama | Microsoft Docs
description: EDIFACT ileti Kodlayıcı için Azure Logic Apps Enterprise Integration Pack ile XML oluşturmak ve EDI doğrula
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: jonfan, divswa, LADocs
ms.topic: article
ms.assetid: 974ac339-d97a-4715-bc92-62d02281e900
ms.date: 01/27/2017
ms.openlocfilehash: 7396aee56acdf2476ed1bb3cc5a9909349662dc7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64705527"
---
# <a name="encode-edifact-messages-for-azure-logic-apps-with-enterprise-integration-pack"></a>EDIFACT iletileri için Azure Logic Apps Enterprise Integration Pack ile kodlayın

EDIFACT kodlama ileti bağlayıcısıyla EDI ve iş ortağı özgü özellikleri doğrulamak, her işlem kümesi için bir XML belgesi oluşturmak ve teknik bir bildirim, işlev bildirimi veya her ikisi de istek.
Bu bağlayıcıyı kullanmak için bağlayıcıyı mantıksal uygulamanızda için var olan bir tetikleyici eklemeniz gerekir.

## <a name="before-you-start"></a>Başlamadan önce

Gereksinim duyduğunuz öğeleri şu şekildedir:

* Bir Azure hesabı; oluşturabileceğiniz bir [ücretsiz hesap](https://azure.microsoft.com/free)
* Bir [tümleştirme hesabı](logic-apps-enterprise-integration-create-integration-account.md) zaten tanımlanmış ve Azure aboneliğinizle ilişkili. EDIFACT kodlama ileti bağlayıcıyı kullanmak üzere bir tümleştirme hesabı olması gerekir. 
* En az iki [iş ortakları](logic-apps-enterprise-integration-partners.md) , tümleştirme hesabında zaten tanımlanmış
* Bir [EDIFACT sözleşmesi](logic-apps-enterprise-integration-edifact.md) , tümleştirme hesabında zaten tanımlı

## <a name="encode-edifact-messages"></a>EDIFACT ileti kodlama

1. [Mantıksal uygulama oluşturma](quickstart-create-first-logic-app-workflow.md).

2. EDIFACT kodlama ileti Bağlayıcısı'nı tetikleyicileri, sahip değil. istek tetikleyicisi gibi mantıksal uygulamanızı başlatmak için bir tetikleyici eklemelisiniz. Mantıksal Uygulama Tasarımcısı'nda bir tetikleyici ekleme ve ardından mantıksal uygulamanız için bir eylem ekleyin.

3.  Arama kutusuna filtreniz olarak "EDIFACT" girin. Şunlardan birini seçin **sözleşme adına göre EDIFACT iletisine kodlayın** veya **kimliklere göre EDIFACT iletisine kodla**.
   
    ![EDIFACT arama](media/logic-apps-enterprise-integration-edifact-encode/edifactdecodeimage1.png)  

3. Daha önce tümleştirme hesabı için herhangi bir bağlantı oluşturmadıysanız, artık bu bağlantıyı oluşturmak için istenir. Bağlantınızı adlandırın ve bağlanmak istediğiniz tümleştirme hesabı seçin.

    ![Tümleştirme hesabı bağlantısı oluşturma](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage1.png)  

    Bir yıldız işareti ile özellikleri gereklidir.

    | Özellik | Ayrıntılar |
    | --- | --- |
    | Bağlantı adı * |Bağlantınız için herhangi bir ad girin. |
    | Tümleştirme hesabı * |Tümleştirme hesabı için bir ad girin. Tümleştirme hesabı ve mantıksal uygulamanızı aynı Azure konumda olduklarından emin olun. |

5.  İşiniz bittiğinde, bağlantı ayrıntılarınızı şu örneğe benzemelidir. Bağlantınızı oluşturmayı tamamlamak için seçin **Oluştur**.

    ![Tümleştirme hesabı bağlantı ayrıntıları](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage2.png)

    Bağlantınızı artık oluşturulur.

    ![oluşturulan tümleştirme hesabı bağlantısı](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage4.png)

#### <a name="encode-edifact-message-by-agreement-name"></a>Sözleşme adına göre EDIFACT iletisine kodlayın

Sözleşme adına göre EDIFACT iletileri kodlama tercih ediyorsanız, açın **adı, EDIFACT sözleşmesi** listesinde, girin veya EDIFACT sözleşmesi adınızı seçin. Kodlanacak XML iletisi girin.

![EDIFACT sözleşmesi adını ve kodlanacak XML iletisi girin](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage6.png)

#### <a name="encode-edifact-message-by-identities"></a>Kimliklere göre EDIFACT iletisine kodlayın

Kimliklere göre EDIFACT iletileri kodlanacak seçerseniz, gönderen tanımlayıcısı, gönderen niteleyicisi, alıcı tanımlayıcısı ve EDIFACT anlaşması'nda yapılandırılan alıcı niteleyicisi girin. Kodlanacak XML iletisi seçin.

![Gönderen ve alıcı için kimlikleri sağlamak, kodlanacak XML iletisi seçin](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage7.png)

## <a name="edifact-encode-details"></a>EDIFACT kodlama ayrıntıları

EDIFACT kodlama bağlayıcı, bu görevleri gerçekleştirir: 

* Gönderen niteleyicisi & tanımlayıcı ve alıcı niteleyicisi ve tanımlayıcı eşleştirerek sözleşmesini çözümleyin
* Değişimi işlem kümeleri EDI XML olarak kodlanmış iletileri dönüştürme EDI değişim serileştirir.
* İşlem kümesi üst bilgi ve tanıtım parçaları için geçerlidir
* Bir değişim denetim numarası, bir grup denetim numarası ve her giden değişimine yönelik bir işlem kümesi denetim numarası oluşturur
* Ayırıcı olarak yük verisi değiştirir
* EDI ve iş ortağı özgü özellikleri doğrulama
  * İleti şemayla işlem kümesi veri öğelerinin şema doğrulaması.
  * EDI doğrulaması işlem kümesi veri öğeleri üzerinde gerçekleştirilen.
  * Genişletilmiş Doğrulama işlem kümesi veri öğeleri üzerinde gerçekleştirilen
* Her işlem kümesi için bir XML belgesi oluşturur.
* Teknik ve/veya işlev bildirimi (yapılandırılmışsa) ister.
  * Bir teknik bildirim bir değişim giriş kontrol iletisi gösterir.
  * Bir işlev bildirimi kontrol iletisi, kabul veya alınan Değişim, Grup veya ileti reddine hataları veya desteklenmeyen işlevleri listesini gösterir.

## <a name="view-swagger-file"></a>Swagger dosyasını görüntüle
EDIFACT bağlayıcının Swagger ayrıntılarını görüntülemek için bkz: [EDIFACT](/connectors/edifact/).

## <a name="next-steps"></a>Sonraki adımlar
[Enterprise Integration Pack hakkında daha fazla bilgi](logic-apps-enterprise-integration-overview.md "Enterprise Integration Pack hakkında bilgi edinin") 

