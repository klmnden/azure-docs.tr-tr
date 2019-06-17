---
title: Azure uygulama teklif test sürüşü | Azure Market
description: Azure Marketi'nde teklif Azure uygulaması için test sürüşü yapılandırma
services: Azure, Marketplace, Cloud Partner Portal,
author: dan-wesley
ms.service: marketplace
ms.topic: conceptual
ms.date: 04/23/2019
ms.author: pabutler
ms.openlocfilehash: 42e533cdcedfb47a46934f77714d61a640a8d7d1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64942860"
---
# <a name="azure-applications-test-drive-tab"></a>Azure uygulamaları Test Sürüşü sekmesi

Müşterileriniz için bir deneme sürümü deneyimi sağlamak için Test Sürüşü sekmesini kullanın.

## <a name="test-drive-benefits"></a>Test sürücü avantajları

Müşterileriniz için bir deneme sürümü deneyimi oluşturma güvenle satın alabileceğiniz emin olmak için iyi bir uygulamadır. Deneme seçenekleri, Test Sürüşü en verimli oluşturma yüksek kaliteli bir müşteri adayları ve artan dönüştürme bu müşteri adayları var.

Müşterilere ürün uygulamasının temel özellikler ve avantajları, gerçek uygulama senaryoda gösterildiği uygulamalı, kendi kendine yol gösteren denemesini sağlar.

## <a name="how-a-test-drive-works"></a>Bir test sürüşüne nasıl çalışır?

Bir müşteri adayını arar ve uygulamanızı Marketi'nde bulur. Müşteri açar ve kullanım koşullarını kabul eder. Bu noktada, müşteri ile takip etmek için yüksek oranda tam bir müşteri adayı alırken saat için sabit sayıda denemek için önceden yapılandırılmış ortamınıza alır. Daha fazla bilgi için [Test Sürüşü nedir?](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/what-is-test-drive)

## <a name="setting-up-a-test-drive"></a>Bir test sürüşüne ayarlama

Etkinleştirmek ve bir test sürüşüne yapılandırmak için aşağıdaki adımları kullanın.

### <a name="to-enable-a-test-drive"></a>Bir test sürüşüne etkinleştirmek için:

1. Altında **yeni teklif**seçin **Test Sürüşü** sekmesi.
2. Altında **Test Sürüşü**seçin **Evet** için **Test Sürüşü etkinleştirme**.

   ![Bir test sürüşüne etkinleştir](./media/managed-app-enable-testdrive.png)

### <a name="to-configure-a-test-drive"></a>Bir test sürüşüne yapılandırmak için:

Bir test sürüşüne etkinleştirdikten sonra test sürüşü ayarlamak için aşağıdaki biçimlerden doldurun:
  
 - Ayrıntılar
 - Teknik yapılandırma
 - Test Sürüşü dağıtım Abonelik Ayrıntıları

Sonraki ekran görüntüsü yakalamayı tüm Test Sürüşü formları gösterilmektedir. Gerekli alan adına eklenmiş bir yıldız işareti (*) gösterir. 

![Bir test sürüşüne yapılandırın](./media/managed-app-configure-testdrive.png)

Aşağıdaki tabloda, yönetilen uygulamanız için test sürüşü kurmak için gereken alanları açıklar.  Bir yıldız işareti ile eklenmiş alanlar gereklidir.

|      Alan         |  Açıklama      |
|  ---------------   |  ---------------  |
| **Açıklaması\***  |  Test Sürüşünüz yapılabilir açıklanmaktadır. Bu açıklama Biçimlendirilecek temel HTML etiketlerini kullanabilirsiniz. Örneğin, &lt;p&gt;, &lt;em&gt;, &lt;ul&gt;, &lt;li&gt;, &lt;ol&gt;ve bölüm başlıkları.                |
| **Kullanıcı el kitabı\***  |  Müşterilerin Test Sürüşü deneyimi boyunca size yol için kullanabileceğiniz bir kullanıcı el ile yükleyin. Bu belgede bir .pdf dosyası olmalıdır.    |
| **Test sürücü tanıtım videosu** |  Test Sürüşünüz bir isteğe bağlı video Kılavuzu. Bir müşteri, bunlar bir test sürüşü önce bu videoyu izleyebilirsiniz. YouTube veya Vimeo video URL'sini sağlayın. Seçerseniz **+ Video ekleme**, aşağıdaki bilgileri sağlamanız istenir:<ul><li>Ad</li><li>URL'si</li><li>Küçük resim (PNG biçiminde, 533 x 324 piksel)</li></ul>  |
| **Örnekleri\***      | Kaç tane istediğiniz yapılandırmak, hangi bölgelerin ve ne kadar hızlı müşterilerin Test Sürüşü alabilirsiniz. Daha fazla bilgi için [Test Sürüşü yayımlama](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/azure-resource-manager-test-drive#how-to-publish-a-test-drive).           |
| **Test sürücü süresi (saat)\*** | Saat sayısı için bir tamsayı girin. İzin verilen aralık 1 ile 999 arasında biçimindedir. |
| **Test sürücü ARM şablonu\***     | Uygulamanız için Azure Resource Manager şablonlarınızı içeren bir sıkıştırılmış (.zip) dosyasını karşıya yükleyin. Daha fazla bilgi için [Azure Resource Manager Test Sürüşü](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/azure-resource-manager-test-drive). |
| **Erişim bilgileri\***          | Test Sürüşü müşteri aldıktan sonra erişim bilgileri sağlayın. Örneğin, test sürüşü erişim ve bilgi oturum için bir URL. . Bu açıklama Biçimlendirilecek temel HTML etiketlerini kullanabilirsiniz. Örneğin, &lt;p&gt;, &lt;em&gt;, &lt;ul&gt;, &lt;li&gt;, &lt;ol&gt;ve bölüm başlıkları. |
| **Azure abonelik kimliği\***       | Bu Azure hizmetlerini ve Azure portalına erişim verir. Burada kullanım raporlama ve Hizmetleri faturalandırılır aboneliktir. Test Sürüşleri yalnızca bir abonelik oluşturmak için zaten ayrı bir Azure aboneliğiniz yoksa.  |
| **Azure AD Kiracı kimliği\***          | Azure Active Directory'de mevcut bir kiracı sağlayın veya bu test sürüşü için bir kiracı oluşturabilirsiniz.  |
| **Azure AD uygulama kimliği\***             | Oluşturun ve yeni bir uygulama kaydedin. Microsoft bu uygulamayı Test Sürüşü örneğinizin işlemleri için kullanır.  |
| **Azure AD uygulama anahtarı\***            | Uygulama için bir kimlik doğrulama anahtarı oluşturun ve bu alanına yapıştırın.   |
|  |  |

Tüm gerekli bilgileri sağladıktan sonra seçin **Kaydet** test sürüşü ayarlama işlemini sonlandırmak için.


## <a name="next-steps"></a>Sonraki adımlar

[Market sekmesi](./cpp-marketplace-tab.md)
