---
title: "Özel bağlayıcılar genel bakış - Azure Logic Apps | Microsoft Docs"
description: "Destek ve tümleştirme senaryolarına genişletmek için özel bağlayıcılar oluşturma hakkında genel bakış"
author: ecfan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/22/2017
ms.author: LADocs; estfan
ms.openlocfilehash: 0515b21bc7c7cc737e14fda016606bbea1aab476
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="custom-connectors-overview"></a>Özel bağlayıcılar genel bakış

Herhangi bir kod yazmak zorunda kalmadan, iş akışları veya uygulamalarla oluşturabilirsiniz [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/), [Microsoft Flow](https://flow.microsoft.com), veya [Microsoft PowerApps](https://powerapps.microsoft.com). Verileri ve İş süreçlerinizi tümleştirmenize yardımcı olmak için bu hizmetler sunan [100 + Bağlayıcılar](../connectors/apis-list.md)yalnızca Microsoft hizmetlerini ve Azure ve SQL Server gibi ürün için ancak diğer hizmetlerin de GitHub, Salesforce, Twitter ve daha fazla bilgi . 

Ancak bazen API'leri, hizmetleri ve önceden oluşturulmuş bağlayıcıları olarak kullanılabilen olmayan sistemleri çağrı isteyebilirsiniz. Kullanıcılarınızın iş ve üretkenlik gereksinimleri daha uyarlanmış senaryoları desteklemek için oluşturabileceğiniz *özel Bağlayıcılar* kendi tetikleyiciler ve Eylemler ile.
Özel bağlayıcılar tümleştirmeler, ulaşma, bulunabilirliği ve hizmet veya artırmak ve müşteri benimseme artırmanıza yardımcı olabilecek ürün kullan'ı genişletin.

Örneğin, bu diyagramda, bir Web API, API ve özel Bağlayıcısı'nı kullanarak bu API ile çalışan bir mantıksal uygulama için oluşturulan özel bir Logic Apps bağlayıcı arasındaki etkileşimi gösterilmektedir:

![Bir Web API, özel bağlayıcı ve mantıksal uygulama için kavramsal genel bakış](./media/custom-connector-overview/custom-connector-conceptual.png)

Bu genel bakışta genel üst düzey görevler oluşturma, güvenli hale getirme, kaydetme ve kullanarak artı isteğe bağlı olarak paylaşımı veya, bağlayıcılar onaylıyor özetlenmektedir:

![Özel bağlayıcı adımları yazma](./media/custom-connector-overview/authoring-steps.png)

## <a name="prerequisites"></a>Ön koşullar

Bağlayıcınızı başlangıçtan bitişe kadar oluşturmak için bu öğeler gerekir:

* Azure aboneliği. Bir aboneliğiniz yoksa ile başlayabilirsiniz bir [ücretsiz Azure hesabı](https://azure.microsoft.com/free/). Aksi takdirde kaydolun bir [Kullandıkça Öde aboneliğine](https://azure.microsoft.com/pricing/purchase-options/).

* Bir RESTful API'si ile bazı tür kimliği doğrulanmış erişim. Daha fazla bilgi için bkz: [Web API'özel Bağlayıcıyı oluşturmak](../logic-apps/custom-connector-build-web-api-app-tutorial.md). Bağlantı ucunun tetikleyiciler ve Eylemler için kullanabileceğiniz desenleri görmek [mantığı uygulama akışlarından çağırabilirsiniz özel API oluşturma](../logic-apps/logic-apps-create-api-app.md).

* Herhangi bir öğeyi burada:

  * Bir [OpenAPI 2.0 dosyasını](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md), Swagger olarak bilinen daha önce
  * Bir OpenAPI tanımı URL'si
  * A [Postman koleksiyonu](../logic-apps/custom-connector-api-postman-collection.md) API'nizi için 

  Bu öğelerden herhangi birini yoksa, sizin için yönergeler sağlıyoruz.

* İsteğe bağlı: özel bağlayıcı için bir simgesi olarak kullanılacak bir görüntü

## <a name="1-build-your-restful-api"></a>1. RESTful API'si derleme

Teknik olarak, bağlayıcı üzerinde bir OpenAPI dayalı bir REST API çevresinde bir sarmalayıcı olan (önceki adıyla Swagger) belirtimi ve Logic Apps, akış veya PowerApps konuşun, temel alınan hizmet sağlar. Bu nedenle ilk olarak, özel Bağlayıcınızı oluşturmadan önce tam olarak işlevsel bir API gerekir. 

Tüm dil ve platform API için kullanabilirsiniz. Microsoft teknolojileri için bu platformlar birini öneririz:

* [Azure İşlevleri](https://azure.microsoft.com/services/functions/)
* [Azure Web Apps](https://azure.microsoft.com/services/app-service/web/)
* [Azure API uygulamaları](https://azure.microsoft.com/services/app-service/api/)

Örneğin, bu öğreticide gösterilmiştir [Web API öğesinden özel bir bağlayıcı oluşturmak nasıl](../logic-apps/custom-connector-build-web-api-app-tutorial.md). Bağlantı ucunun tetikleyiciler ve Eylemler için kullanabileceğiniz desenleri görmek [mantığı uygulama akışlarından çağırabilirsiniz özel API oluşturma](../logic-apps/logic-apps-create-api-app.md).

## <a name="2-secure-your-api"></a>2. API'nizin güvenliğini sağlayın

Bağlayıcılar ve API için bu kimlik doğrulama standartlarını kullanarak şunları yapabilirsiniz:

   * [OAuth 2.0](https://oauth.net/2/)dahil [Azure Active Directory](https://azure.microsoft.com/develop/identity/) veya Dropbox, GitHub ve SalesForce gibi belirli hizmetleri
   * Genel OAuth 2.0
   * [Temel kimlik doğrulaması](https://swagger.io/docs/specification/authentication/basic-authentication/)
   * [API anahtarı](https://swagger.io/docs/specification/authentication/api-keys/)

Kimlik doğrulama kodu aracılığıyla uygulamak zorunda kalmamak için Azure Active Directory (Azure AD) kimlik doğrulaması API'nizi Azure portalında için ayarlayabilirsiniz. Veya gerektirir ve kimlik doğrulama, API'nin kodlarda zorlayın. 

Daha fazla bilgi için uygun öğreticileri izleyin:

* [Logic Apps: özel Bağlayıcınızı güvenli](../logic-apps/custom-connector-azure-active-directory-authentication.md)
* [Akış: özel Bağlayıcınızı güvenli](https://ms.flow.microsoft.com/documentation/customapi-azure-resource-manager-tutorial/)
* [PowerApps: özel Bağlayıcınızı güvenli](https://powerapps.microsoft.com/tutorials/customapi-azure-resource-manager-tutorial/)

## <a name="3-describe-your-api"></a>3. API'nizi açıklayın 

API'nizi bazı kimliği doğrulanmış erişim türü varsayılarak, Logic Apps, akış veya PowerApps API'si ile iletişim kurabilmesi için API'nin arabirimi ve işlemleri tanımlamak gerekir.
Bu sektör standart tanımları birini kullanın:

* Bir [OpenAPI 2.0 dosyasını](https://swagger.io/) yapı tarafından olan mevcut bir OpenAPI dosya başlatabilirsiniz.

  OpenAPI için yeni, ziyaret [Swagger ile çalışmaya başlama](http://swagger.io/getting-started/) swagger.io sitesinde.
  Örnek bir OpenAPI dosyası için bkz: [metin Analytics API belgelerine](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/export?DocumentFormat=Swagger&ApiName=Azure). 

* Bir OpenAPI dosyası sizin için otomatik olarak oluşturur Postman koleksiyonu. Zaten bir OpenAPI dosyası yok ve oluşturmak istemediğiniz zaman Postman koleksiyonu kullanarak özel bir bağlayıcı yine kolayca oluşturabilirsiniz.

  Postman için yeni başladıysanız [Postman uygulama yükleme](https://www.getpostman.com/apps) kendi sitelerinden. Daha fazla bilgi için bkz: [Postman ile özel bağlayıcılar açıklamak](../logic-apps/custom-connector-api-postman-collection.md).

> [!IMPORTANT]
> Dosya boyutu 1 MB'tan az olması gerekir.

Planda, Logic Apps, akış ve PowerApps sonuçta OpenAPI kullanın, Postman koleksiyonu ayrıştırır ve bir OpenAPI tanım dosyası koleksiyona çevirir. OpenAPI 2.0 ve Postman koleksiyonları farklı biçimlerde kullansa da, her ikisi de, API'nin işlemlerini ve parametrelerini açıklayan dilden bağımsız, makine tarafından okunabilir belgelerdir. Dile göre çeşitli Araçları'ndan bu belgeleri oluşturabilir ve platform API'nizi kullanılır. Bağlayıcınızı kaydederken bir OpenAPI dosyası da oluşturabilirsiniz.

Örneğin, bir OpenAPI dosyası veya Postman koleksiyonu tüm REST API uç noktasından oluşturabileceğiniz dahil olmak üzere:

* Genel kullanıma açık Bağlayıcılar [Spotify](https://developer.spotify.com/), [Slack'e](https://api.slack.com/), [Rackspace](http://docs.rackspace.com/), vb.

* Oluşturulan ve Azure, Heroku, Google Cloud ve daha fazlası gibi tüm bulut barındırma sağlayıcısına dağıttığınız bir API

* Bu API, ağ ancak yalnızca dağıtmış olan özel bir iş API Genel internet'te sunulan

## <a name="4-register-your-connector"></a>4. Bağlayıcınızı kaydetme

Açıklama, gerekli kimlik doğrulama ve parametreleri ve her bir işlemin çıkışları da dahil işlemleri dahil olmak üzere, API özellikleri PowerApps anlamak veya Logic Apps, akış, kayıt işlemine yardımcı olur. Kayıt işlemini başlattığınızda OpenAPI dosya veya Kayıt Sihirbazı'ndaki meta veri alanlarını otomatik olarak dolduran bir Postman koleksiyonu sağlayabilir. Bu alanların değerlerini herhangi bir zamanda düzenleyebilirsiniz.

![API'nizi açıklayın](./media/custom-connector-overview/choose-api-definition-file.png)

Bağlayıcınızı kaydettirmek için uygun öğreticiyi izleyin:

* [Logic Apps: Bağlayıcınızı kaydetme](../logic-apps/logic-apps-custom-connector-register.md)
* [Akış: Bağlayıcınızı kaydetme](https://ms.flow.microsoft.com/documentation/register-custom-api/#register-your-custom-connector)
* [PowerApps: Bağlayıcınızı kaydetme](https://powerapps.microsoft.com/tutorials/register-custom-api/#register-your-custom-connector)

## <a name="5-use-your-connector-in-a-logic-app-flow-or-app"></a>5. Bir mantıksal uygulama, akış ya da uygulama Bağlayıcınızı kullanın 

Bağlayıcınızı Microsoft tarafından yönetilen bağlayıcıların kullandığınız şekilde kullanabilirsiniz. Örneğin, API için bir bağlantı oluşturabilir ve Microsoft tarafından yönetilen bağlayıcıların işlemlerini çağırma aynı şekilde API sağlayan herhangi bir işlem çağırmak için bir mantıksal uygulama iş akışında özel Bağlayıcınızı ekleyin.

## <a name="6-share-your-connector"></a>6. Bağlayıcınızı paylaşma 

Logic Apps, akış veya PowerApps kaynakları paylaşmak aynı şekilde yararlı kuruluşunuzda kullanıcılarla Bağlayıcınızı paylaşabilirsiniz. Paylaşımı isteğe bağlı olsa da, bağlayıcılar diğer kullanıcılarla paylaşmak istediğiniz senaryolar olabilir.

Kayıtlı ancak onaylı olmayan özel bağlayıcılar Microsoft tarafından yönetilen bağlayıcıların gibi çalışır, ancak görünür ve kullanılabilir *yalnızca* bağlayıcı'nın yazar ve aynı Azure Active Directory kiracısına ve Azure aboneliğine sahip kullanıcılar Bu uygulamaları'nın dağıtıldığı bölgede Logic apps için. Sonraki adım nasıl Bağlayıcınızı harici kullanıcılar bu sınırlar dışında Örneğin, tüm Logic Apps, akış ve PowerApps kullanıcılarla paylaşabileceğiniz açıklanır.

> [!IMPORTANT]
> Bir bağlayıcı paylaşıyorsanız, diğerlerinin bu bağlayıcıda bağımlı başlayabilir. 
> ***Bağlayıcınızı silindiğinde, bu bağlayıcı için tüm bağlantılar silinir.***

* [Logic Apps: Bağlayıcınızı paylaşma](../logic-apps/logic-apps-custom-connector-register.md)
* [Akış: Bağlayıcınızı paylaşma](https://ms.flow.microsoft.com/documentation/register-custom-api/#share-your-custom-connector)
* [PowerApps: Bağlayıcınızı paylaşma](https://powerapps.microsoft.com/tutorials/register-custom-api/#share-your-custom-connector)

## <a name="7-certify-your-connector"></a>7. Bağlayıcınızı Onayla

İsteğe bağlı olarak Bağlayıcınızı Logic Apps, akış ve PowerApps tüm kullanıcılarla paylaşmak için Microsoft Sertifika Bağlayıcınızı gönderin. Bu işlem Bağlayıcınızı inceler, teknik ve içerik uyumluluğunu denetler ve Microsoft Bağlayıcınızı yayımlamadan önce işlevselliği Logic Apps, akış ve PowerApps doğrular. Bilgi [Bağlayıcınızı Microsoft sertifika için gönderme](../logic-apps/custom-connector-submit-certification.md).

## <a name="get-support"></a>Destek alın

* Ekleme ve geliştirme desteği için e-posta [ condevhelp@microsoft.com ](mailto:condevhelp@microsoft.com). Microsoft etkin şekilde Geliştirici sorular ve sorunlar için bu hesabı izler ve uygun ekibine yönlendirir. 

* Sık sorulan sorular için bkz: [özel bağlayıcı SSS](../logic-apps/custom-connector-faq.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Bir Web API öğesinden özel bir bağlayıcı oluşturun](../logic-apps/custom-connector-build-web-api-app-tutorial.md)
* [Kimlik doğrulaması özel bağlayıcılar için ayarlama](../logic-apps/custom-connector-azure-active-directory-authentication.md)
* [Postman koleksiyonlarıyla özel bağlayıcılar açıklayın](../logic-apps/custom-connector-api-postman-collection.md)
* [Microsoft sertifika için özel bağlayıcılar gönderme](../logic-apps/custom-connector-submit-certification.md)
