---
title: Azure SaaS uygulaması teklif test sürücü yapılandırması | Microsoft Docs
description: Test sürüşü için SaaS uygulama teklifi, Azure Marketi'nde yapılandırın.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 04/24/2019
ms.author: pbutlerm
ms.openlocfilehash: b12ba53f847b46479b3100c088c29372b58c1b8e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60594221"
---
# <a name="saas-application-test-drive-tab"></a>SaaS uygulama Test Sürüşü sekmesi

Müşterileriniz için bir deneme sürümü deneyimi sağlamak için Test Sürüşü sekmesini kullanın.

## <a name="test-drive-benefits"></a>Test sürücü avantajları

Müşterileriniz için bir deneme sürümü deneyimi oluşturma güvenle satın alabileceğiniz emin olmak için iyi bir uygulamadır. Deneme seçenekleri, Test Sürüşü en verimli oluşturma yüksek kaliteli bir müşteri adayları ve artan dönüştürme bu müşteri adayları var.

Test sürüşü, müşterilere ürün uygulamasının temel özellikler ve avantajları, gerçek uygulama senaryoda gösterildiği uygulamalı, kendi kendine yol gösteren denemesini sağlar.


## <a name="how-a-test-drive-works"></a>Bir test sürüşüne nasıl çalışır?

Bir müşteri adayını arar ve uygulamanızı Marketi'nde bulur. Müşteri açar ve kullanım koşullarını kabul eder. Bu noktada, müşteri ile takip etmek için yüksek oranda tam bir müşteri adayı alırken saat için sabit sayıda denemek için önceden yapılandırılmış ortamınıza alır. Daha fazla bilgi için [Test Sürüşü nedir?](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/what-is-test-drive)


## <a name="publishing-steps"></a>Yayımlama adımları

Bir test sürücü eklemek için ana yayımlama adımlar şunlardır:

1. Test Sürüşü senaryonuzu tanımlama
2. Derleme ve/veya Resource Manager şablonunuzu değiştirme
3. Test Sürüşü hakkında adım adım el ile oluşturma
4. Teklifinizi yeniden yayımlayın


## <a name="setting-up-a-test-drive"></a>Bir test sürüşüne ayarlama

Her ürün, senaryo ve Market üzeresiniz türüne göre kullanılabilir Test Sürüşleri, dört farklı türleri vardır.

|  **Tür**          |  **Açıklama**  |  **Kurulum yönergeleri**  |
|  ---------------   |  ---------------  |  ---------------  |
|     Azure Resource Manager               |    Bir Azure Resource Manager Test Sürüşü yayımcı tarafından oluşturulan bir çözümünü oluşturan tüm Azure kaynaklarını içeren bir dağıtım şablonudur. Bu tür bir Test Sürüşü uygun ürünler, yalnızca Azure kaynakları şunlardır.               |       [Azure Resource Manager Test Sürüşü](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/azure-resource-manager-test-drive)            |
|       Şirket Dışında Barındırılan             |       Bir barındırılan Test Sürüşü Microsoft barındırma tarafından kurulumun karmaşıklığını ortadan kaldırır ve Test Sürüşü kullanıcı sağlama ve sağlamayı kaldırma gerçekleştiren hizmetin bakımını.             |         [Barındırılan Test Sürüşü](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/hosted-test-drive)          |
|      Logic App              |       Mantıksal uygulamayı Test Sürüşü, tüm karmaşık çözüm mimarileri kapsayacak şekilde tasarlanmıştır bir dağıtım şablonudur. Tüm Dynamics uygulamaları ve özel ürünler bu tür bir Test Sürüşü kullanmanız gerekir.            |      [Mantıksal uygulamayı Test Sürüşü](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/logic-app-test-drive)             |
|       Power BI             |         Power BI Test Sürüşü özel oluşturulan bir panoya katıştırılmış bir bağlantı içerir. Test Sürüşü bu tür bir etkileşimli Power BI görsel kullanması gereken göstermek istediği herhangi bir ürünü. Karşıya yüklemek için ihtiyacınız olan, katıştırılmış Power BI URL'si.          |        [Power BI Test Sürüşü](#power-bi-test-drive)           |
|   |   |   |


### <a name="power-bi-test-drive"></a>Power BI test sürüşü

Bir test sürüşüne yapılandırmak için aşağıdaki adımları kullanın.

1. Yeni Teklif altında seçin **Test Sürüşü**.
2. Test Sürüşü üzerinde seçin **Evet**.

   ![Test sürüşü etkinleştir](./media/saas-enable-test-drive.png)

   Bir test sürüşüne etkinleştirdiğinizde, ayrıntıları ve teknik yapılandırma görürsünüz ve sonraki ekran görüntüsünde gösterilen formları.

   ![Test sürücüsü yapılandırma formu](./media/saas-test-drive-yes.png)

3. Altında **ayrıntıları**, aşağıdaki alanlar için bilgileri sağlayın:
  
   - Açıklama: test sürüşünüz ve kullanıcılar ile neler yapabileceğinizi açıklar. Bu açıklama Biçimlendirilecek temel HTML etiketlerini kullanabilirsiniz.
   - Kullanıcı el ile-karşıya yükleme müşterilerinizin bunlar test sürüşü yönlendiriyoruz zaman kullanabileceğiniz bir kullanıcı el ile belge. Bu kılavuz, bir pdf dosyası olmalıdır.
   - Test sürücü tanıtım görüntü (isteğe bağlı) - bir video (YouTube veya Vimeo) kullanarak müşterileriniz için önce bunlar Test sürüşü izlemek sağlayabilir. Videonun URL'sini sağlayın.

4. Altında **teknik yapılandırma**, aşağıdaki alanlar için bilgileri sağlayın:

   - Test Sürüşü – Select türü **Power BI** aşağı açılan listeden.
   - Power BI panosu için bağlantı: panonun bağlantısını sağlar.

5. Test sürüşü yapılandırma bitirdikten sonra Seç **Kaydet**.


## <a name="next-steps"></a>Sonraki adımlar

[Vitrin Ayrıntıları sekmesi](./cpp-storefront-tab.md)
