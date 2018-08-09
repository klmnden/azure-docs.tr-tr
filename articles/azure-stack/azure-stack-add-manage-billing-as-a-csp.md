---
title: Kullanım ve bulut hizmeti sağlayıcısı olarak Azure Stack için faturalandırma yönetme | Microsoft Docs
description: Bir bulut sağlayıcısı (CSP) olarak Azure Stack kaydetme ve faturalandırma için müşterilerin ekleme yoluyla Yürüme.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2018
ms.author: brenduns
ms.reviewer: alfredo
ms.openlocfilehash: 66bab9f4b5099099039aa0f60cd86ef23e73cd1f
ms.sourcegitcommit: d16b7d22dddef6da8b6cfdf412b1a668ab436c1f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39715694"
---
# <a name="manage-usage-and-billing-for-azure-stack-as-a-cloud-service-provider"></a>Kullanım ve faturalandırma için Azure Stack bulut hizmeti sağlayıcısı olarak yönetme 

*İçin geçerlidir: Azure Stack tümleşik sistemleri*

Bu makalede bir bulut sağlayıcısı (CSP) olarak Azure Stack kaydetme ve müşterilerin ekleme size yol gösterir.

CSP olarak Azure Stack kullanarak çeşitli müşteriler ile çalışır. Her müşteri, Azure'da bir CSP aboneliği sahiptir. Azure Stack kullanımına her kullanıcı aboneliği doğrudan gerekecektir.

Aşağıdaki diyagramda, paylaşılan hizmetler hesabınızı seçin ve Azure Stack hesabı ile Azure hesabını kaydetmek için gereken adımları gösterilmektedir. Kayıtlı, son müşterileriniz ekleme yapabilirsiniz.

**Kullanımı izleme CSP olarak ekleme adımları**

![Kullanımı ve bir bulut hizmeti sağlayıcısı Yönetimi etkinleştirme işlemi.](media\azure-stack-add-manage-billing-as-a-csp\process-add-useage-as-a-csp.png)

## <a name="create-a-csp-or-cspss-subscription"></a>CSP veya CSPSS abonelik oluşturma

### <a name="cloud-service-provider-subscription-types"></a>Bulut hizmeti sağlayıcısı abonelik türleri

Azure Stack için kullandığınız paylaşılan hizmetler hesap türünü seçmeniz gerekir. Çok kiracılı Azure Stack, kaydı için kullanılabilen abonelikler türleri şunlardır:

 - Bulut hizmeti sağlayıcısı 
 - İş ortağı paylaşılan Services aboneliği 

#### <a name="csp-shared-services"></a>CSP paylaşılan hizmetler

Bulut hizmet sağlayıcısı paylaşılan hizmetler (CSPSS) abonelikleri doğrudan bir CSP, kayıt için tercih edilen seçenektir veya CSP dağıtıcı Azure Stack'te çalışır.

CSPSS abonelikler, paylaşılan hizmetler Kiracı ile ilişkilidir. Azure Stack kaydolduğunuzda, bir aboneliğin sahibi olan bir hesap için kimlik bilgilerini sağlamanız gerekir. Azure Stack kaydetmek için kullandığınız hesabın, dağıtım için kullandığınız yönetici hesabına ait farklı olabilir. Ayrıca, iki hesap yapmak *değil* aynı etki alanına ait olmaları gerekir. Diğer bir deyişle, kullanmakta olduğunuz kiracıyı kullanarak dağıtabilirsiniz. Örneğin, ContosoCSP.onmicrosoft.com kullanın ve sonra Örneğin IURContosoCSP.onmicrosoft.com farklı bir kiracıya kullanarak kaydedin. Bunu yapmak için günlük Azure Stack yönetim yaptığınızda ContosoCSP.onmicrosoft.com kullanarak oturum unutmayın gerekecektir. Kayıt işlemleri yapmanız gerektiğinde IURContosoCSP.onmicrosoft.com kullanarak Azure'a ne zaman oturum açın.

Açıklamasını CSPSS abonelikleri ve yönergeler için aşağıdaki abonelik oluşturma başvurmak [Azure iş ortağı Paylaşılan Hizmetler](https://msdn.microsoft.com/partner-center/shared-services).

#### <a name="csp-subscriptions"></a>CSP abonelikleri

Bulut hizmeti sağlayıcısı (CSP) abonelikleri CSP satıcısı, kayıt için tercih edilen seçenektir ya da son müşteri Azure Stack'te çalışır.

## <a name="register-azure-stack"></a>Azure Stack kaydetme

Azure Stack ile kaydetmek için bkz: [kaydetme Azure Stack, Azure aboneliğiniz ile](azure-stack-registration.md).

## <a name="add-end-customer"></a>Son müşteri ekleme

Azure Stack, bulut hizmet sağlayıcısı (CSP) aboneliğini kullanımlarını yeni bir kiracı kaynaklarını kullanır zaman raporlanacaktır şekilde yapılandırmak için bkz. [Kiracı kullanımı için ekleyin ve Azure Stack'e faturalama](azure-stack-csp-howto-register-tenants.md).

## <a name="charge-the-right-subscriptions"></a>Doğru abonelik ücret

Azure Stack, kayıt adında bir özellik kullanır. Bir kayıt, Azure'da depolanan bir nesnedir. Kayıt nesne belirli bir Azure Stack için kaydedilecek kullanmak için hangi Azure abonelikleri belgeler. Bu bölümde kayıt önemini yöneliktir.

Azure Stack kayıt kullanabilirsiniz:
 - Azure ticari için Azure Stack kullanım verilerini iletmek ve bir Azure aboneliğine fatura.
 - Her bir müşterinin kullanım çok kiracılı bir Azure Stack dağıtımı ile farklı bir abonelikte rapor. Azure Stack, farklı kuruluşların aynı Azure Stack örneğinde desteklemek çok kiracılı mimari sağlar.

Her Azure Stack için varsayılan bir abonelik ve birçok abonelikleri Kiracı. Varsayılan bir kiracıya özgü abonelik yoksa ücretlendirilir bir Azure aboneliğine aboneliktir. Bu ilk aboneliğe kayıtlı olmalıdır. Çalışmak için raporlama çok kiracılı kullanım için abonelik bir CSP veya CSPSS aboneliğiniz olması gerekir.

Ardından, kayıt, Azure Stack kullanacak her Kiracı için bir Azure aboneliği ile güncelleştirilir. Kiracı abonelik CSP türünde olmalıdır ve varsayılan abonelik sahibi iş ortağı toplamak gerekir. Diğer bir deyişle, başkasının müşteriler kaydedilemiyor.

Azure Stack için genel Azure kullanım bilgilerini ilettiğinde, bir Azure hizmeti kayıt bakar ve her bir kiracının kullanım için uygun Kiracı aboneliği eşler. Bir kiracı kayıtlı değil, bu kullanım kaynağı Azure Stack örneği için varsayılan abonelik gider.

Kiracı aboneliklerine CSP abonelikleri olduğundan, CSP iş ortağı kendi fatura gönderilir ve kullanım bilgilerini son müşterinin görünür değil.

## <a name="next-steps"></a>Sonraki adımlar

 - CSP programı hakkında daha fazla bilgi için bkz: [bulut çözümü sağlayıcısı programı](https://partner.microsoft.com/solutions/microsoft-cloud-solutions).
 - Azure yığını kaynak kullanım bilgilerini alma hakkında daha fazla bilgi için bkz: [kullanım ve faturalandırma Azure Stack'te](azure-stack-billing-and-chargeback.md).
