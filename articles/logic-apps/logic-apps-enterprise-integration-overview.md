---
title: B2B Kurumsal tümleştirme genel bakış - Azure Logic Apps | Microsoft Docs
description: Azure Logic Apps ve Enterprise Integration Pack ile Kurumsal tümleştirme çözümleri için otomatik B2B iş akışları oluşturun
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: dd517c4d-1701-4247-b83c-183c4d8d8aae
ms.date: 09/08/2016
ms.openlocfilehash: d37d5cb2b89b82bd9741dee0946b3a77d456b22a
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2018
ms.locfileid: "49405761"
---
# <a name="overview-b2b-enterprise-integration-scenarios-in-azure-logic-apps-with-enterprise-integration-pack"></a>Genel Bakış: Azure Logic apps'te ile Enterprise Integration Pack B2B Kurumsal tümleştirme senaryoları

İşletmeden işletmeye (B2B) iş akışları ve Azure Logic Apps ile sorunsuz iletişim için Microsoft'un bulut tabanlı çözüm, Enterprise Integration Pack ile Kurumsal tümleştirme senaryoları etkinleştirebilirsiniz. Kuruluşların farklı protokollerini ve biçimlerini kullanıyor olsanız bile iletileri elektronik olarak değiştirebilir. Paketi farklı biçimler kurumlarının sistemleri yorumlayıp işleyebileceği bir biçime dönüştürür. Kuruluşların dahil endüstri standardı protokoller üzerinden ileti alışverişi [AS2](../logic-apps/logic-apps-enterprise-integration-as2.md), [X12](logic-apps-enterprise-integration-x12.md), ve [EDIFACT](../logic-apps/logic-apps-enterprise-integration-edifact.md). Şifreleme ve dijital imzalar hem iletileri güvenli hale getirebilirsiniz.

BizTalk Server veya Microsoft Azure BizTalk Hizmetleri ile ilgili bilgi sahibi olduğunuz, çoğu kavramları benzer olduğundan, Kurumsal tümleştirme özellikleri kullanımı kolaydır. Temel farklardan biri, Kurumsal tümleştirme, depolama ve B2B iletişimde kullanılan yapıtları yönetimi basitleştirmek için tümleştirme hesapları kullandığı ' dir. 

Bu mimari, Enterprise Integration Pack "tümleştirme hesapları" temel alınır. Bu hesaplar depolama tüm yapıtları, şemalar, iş ortakları, sertifikaları, haritalar ve anlaşmalar gibi bulut tabanlı kapsayıcılardır. Bu yapılar, tasarlayın, dağıtın ve B2B uygulamalarınızı korumak için ve logic apps B2B iş akışları oluşturmak için de kullanabilirsiniz. Ancak bu yapıların kullanabilmeniz için mantıksal uygulamanıza tümleştirme hesabınızı bağlamanız gerekir. Bundan sonra mantıksal uygulamanızı tümleştirme hesabınızın yapıtları erişebilirsiniz.

## <a name="why-should-you-use-enterprise-integration"></a>Kurumsal tümleştirme neden kullanmalısınız?

* Kurumsal tümleştirme sayesinde, tek bir yerde--tümleştirme hesabınızı tüm yapıtlar depolayabilirsiniz.
* B2B iş akışları oluşturun ve Azure Logic Apps altyapısı ve tüm bağlayıcıları kullanarak, üçüncü taraf hizmet olarak yazılım (SaaS) uygulamaları, şirket içi uygulamalar ve özel uygulamaları ile tümleştirin.
* Azure işlevleri ile logic apps için özel kod oluşturabilirsiniz.

## <a name="how-to-get-started-with-enterprise-integration"></a>Enterprise Integration'ı kullanmaya nasıl?

Derleme ve mantıksal Uygulama Tasarımcısı'nda ile Enterprise Integration Pack ile B2B uygulamaları yönetme **Azure portalında**. Logic apps ile de yönetebilirsiniz [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.logicapp "Logic apps PowerShell").

Azure portalında uygulamaları oluşturabilmeniz için önce gerçekleştirmeniz gereken üst düzey adımlar şunlardır:

![Genel Bakış görüntüsü](media/logic-apps-enterprise-integration-overview/overview-0.png)  

## <a name="what-are-some-common-scenarios"></a>Bazı yaygın senaryolar nelerdir?

Enterprise Integration bu endüstri standartları destekler:

* EDI - elektronik veri değişimi
* EAI - kuruluş uygulaması tümleştirme

## <a name="heres-what-you-need-to-get-started"></a>Başlamak gerekenler

* Tümleştirme hesabı ile bir Azure aboneliği
* Harita ve şema oluşturmak için Visual Studio 2015
* [Microsoft Azure Logic Apps Kurumsal tümleştirme 2.0 Visual Studio 2015 için Araçlar](https://aka.ms/vsmapsandschemas)  

## <a name="try-it-now"></a>Hemen deneyin

[Tam olarak işlevsel örnek AS2 gönderme dağıtma ve mantıksal uygulama alma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-as2-send-receive) için Azure Logic Apps B2B özellikleri kullanan.

## <a name="learn-more"></a>Daha fazla bilgi edinin
* [Anlaşmaları](../logic-apps/logic-apps-enterprise-integration-agreements.md "Kurumsal tümleştirme anlaşmalar hakkında bilgi edinin")
* [İşletmeler arası (B2B) senaryolarını](../logic-apps/logic-apps-enterprise-integration-b2b.md "B2B özellikleri ile Logic apps oluşturmayı öğrenin ")  
* [Sertifikaları](logic-apps-enterprise-integration-certificates.md "Kurumsal tümleştirme sertifikaları hakkında bilgi edinin")
* [Düz dosya kodlama/kod çözme](logic-apps-enterprise-integration-flatfile.md "kodlama ve kodunu çözme düz dosya içeriği hakkında bilgi edinin")  
* [Tümleştirme hesapları](../logic-apps/logic-apps-enterprise-integration-accounts.md "tümleştirme hesapları hakkında bilgi edinin")
* [Haritalar](../logic-apps/logic-apps-enterprise-integration-maps.md "Kurumsal tümleştirme eşlemeleri hakkında bilgi edinin")
* [İş ortakları](logic-apps-enterprise-integration-partners.md "Kurumsal tümleştirme iş ortakları hakkında bilgi edinin")
* [Şemaları](logic-apps-enterprise-integration-schemas.md "Kurumsal tümleştirme şemaları hakkında bilgi edinin")
* [XML ileti doğrulama](logic-apps-enterprise-integration-xml.md "doğrulamak XML iletileri Logic apps ile öğrenin")
* [XML dönüştürme](logic-apps-enterprise-integration-transform.md "Kurumsal tümleştirme eşlemeleri hakkında bilgi edinin")
* [Enterprise Integration bağlayıcıları](../connectors/apis-list.md "enterprise Integration pack bağlayıcıları hakkında bilgi edinin")
* [Tümleştirme hesabı meta verileri](../logic-apps/logic-apps-enterprise-integration-metadata.md "tümleştirme hesabı meta veriler hakkında bilgi edinin")
* [B2B iletilerini izleme](logic-apps-monitor-b2b-message.md "B2B iletilerini izleme hakkında daha fazla bilgi edinin")
* [Azure Log analytics'te B2B iletilerini izleme](logic-apps-track-b2b-messages-omsportal.md "Azure Log analytics'te B2B iletilerini izleme hakkında daha fazla bilgi edinin")

