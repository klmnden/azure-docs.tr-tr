---
title: Kullanım ve bulut hizmeti sağlayıcısı olarak Azure Stack için faturalandırma yönetme | Microsoft Docs
description: Bir bulut sağlayıcısı (CSP) olarak Azure Stack kaydetme ve faturalandırma için müşterilerin ekleme yoluyla Yürüme.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/13/2019
ms.author: sethm
ms.reviewer: alfredop
ms.lastreviewed: 10/15/2018
ms.openlocfilehash: 1ba3697b5c683ed2e892396db71328e0ed41fdce
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2019
ms.locfileid: "56266926"
---
# <a name="manage-usage-and-billing-for-azure-stack-as-a-cloud-service-provider"></a>Kullanım ve faturalandırma için Azure Stack bulut hizmeti sağlayıcısı olarak yönetme

*Uygulama hedefi: Azure Stack tümleşik sistemleri*

Bu makalede bir bulut sağlayıcısı (CSP) olarak Azure Stack kaydetme ve müşterilerin ekleme size yol gösterir.

CSP olarak Azure Stack kullanarak çeşitli müşteriler ile çalışır. Her müşteri, Azure'da bir CSP aboneliği sahiptir. Azure Stack kullanım her kullanıcı aboneliği olarak doğrudan gerekir.

Aşağıdaki şekilde, paylaşılan hizmetler hesabınızı seçin ve Azure Stack hesabı ile Azure hesabını kaydetmek için gerekli adımları gösterilmektedir. Kaydedildikten sonra son müşterileriniz ekleme yapabilirsiniz:

[![Kullanımı ve bir bulut hizmeti sağlayıcısı Yönetimi etkinleştirmek için işlem](media/azure-stack-add-manage-billing-as-a-csp/process-add-useage-as-a-csp.png "işlem kullanımı ve bir bulut hizmeti sağlayıcısı Yönetimi etkinleştirmek için")](media/azure-stack-add-manage-billing-as-a-csp/process-add-useage-as-a-csp.png#lightbox)

## <a name="create-a-csp-or-apss-subscription"></a>CSP veya APSS abonelik oluşturma

### <a name="cloud-service-provider-subscription-types"></a>Bulut hizmeti sağlayıcısı abonelik türleri

Azure Stack için kullandığınız paylaşılan hizmetler hesap türünü seçin. Çok kiracılı Azure Stack, kaydı için kullanılabilen abonelikler türleri şunlardır:

- Bulut hizmeti sağlayıcısı
- İş ortağı paylaşılan Services aboneliği

#### <a name="azure-partner-shared-services"></a>Azure iş ortağı paylaşılan hizmetler

Azure iş ortağı paylaşılan hizmetler (APSS) abonelikleri doğrudan bir CSP, kayıt için tercih edilen seçenektir veya CSP dağıtıcı Azure Stack'te çalışır.

APSS abonelikler, paylaşılan hizmetler Kiracı ile ilişkilidir. Azure Stack kaydolduğunuzda, bir aboneliğin sahibi olan bir hesap için kimlik bilgilerini sağlayın. Azure Stack kaydetmek için kullandığınız hesabın, dağıtım için kullandığınız yönetici hesabına ait farklı olabilir. Ayrıca, iki hesap aynı etki alanına ait gerekmez; kullanmakta olduğunuz kiracıyı kullanarak dağıtabilirsiniz. Örneğin, kullanabileceğiniz `ContosoCSP.onmicrosoft.com`, ardından farklı bir kiracıya; kullanarak kaydetme gibi `IURContosoCSP.onmicrosoft.com`. Kullanarak oturum açmanız hatırlamalısınız `ContosoCSP.onmicrosoft.com` günlük Azure Stack yönetim gerçekleştirdiğinizde. Azure kullanarak oturum `IURContosoCSP.onmicrosoft.com` kayıt işlemleri yapmanız gerektiğinde.

APSS abonelikleri ve bunların nasıl oluşturulacağı açıklaması için bkz: [Azure iş ortağı Paylaşılan Hizmetler](https://msdn.microsoft.com/partner-center/shared-services).

#### <a name="csp-subscriptions"></a>CSP abonelikleri

Bulut hizmeti sağlayıcısı (CSP) abonelikleri CSP satıcısı, kayıt için tercih edilen seçenektir ya da son müşteri Azure Stack'te çalışır.

## <a name="register-azure-stack"></a>Azure Stack kaydetme

Azure Stack Azure ile kaydetmek için önceki bölümde verilen bilgileri kullanarak oluşturulan APSS abonelik kullanın. Daha fazla bilgi için [kaydetme Azure Stack, Azure aboneliğiniz ile](azure-stack-registration.md).

## <a name="add-end-customer"></a>Son müşteri ekleme

Azure Stack, böylece yeni bir kiracı kaynaklarını kullandığında, bulut hizmet sağlayıcısı (CSP) aboneliğini kullanımlarını bildirilir yapılandırmak için bkz [Kiracı kullanımı için ekleyin ve Azure Stack'e faturalama](azure-stack-csp-howto-register-tenants.md).

## <a name="charge-the-right-subscriptions"></a>Doğru abonelik ücret

Azure Stack, kayıt adında bir özellik kullanır. Bir kayıt, Azure'da depolanan bir nesnedir. Kayıt nesne belirli bir Azure Stack için kaydedilecek kullanmak için hangi Azure abonelikleri belgeler. Bu bölümde kayıt önemini yöneliktir.

Azure Stack kayıt kullanabilirsiniz:

- Azure ticari için Azure Stack kullanım verilerini iletmek ve bir Azure aboneliğine fatura.
- Her bir müşterinin rapor '' çok kiracılı bir Azure Stack dağıtımı ile farklı bir abonelikte s kullanım. Azure Stack, farklı kuruluşların aynı Azure Stack örneğinde desteklemek çok kiracılı mimari sağlar.

Her Azure Stack için varsayılan bir abonelik ve birçok abonelikleri Kiracı. Varsayılan abonelik kiracıya özgü abonelik yoksa ücretlendirilir bir Azure aboneliği ' dir. Kaydedilecek ilk abonelik olmalıdır. Çalışmak için raporlama çok kiracılı kullanım için abonelik bir CSP veya APSS aboneliğiniz olması gerekir.

Ardından, kayıt, Azure Stack kullanan her bir kiracı için bir Azure aboneliği ile güncelleştirilir. Kiracı abonelik CSP türünde olmalıdır ve varsayılan abonelik sahibi iş ortağı toplamak gerekir. Başka birisinin müşteriler kaydedilemiyor.

Azure Stack için genel Azure kullanım bilgilerini ilettiğinde, bir Azure hizmeti kayıt bakar ve her bir kiracının kullanım için uygun Kiracı aboneliği eşler. Bir kiracı kayıtlı değil, bu kullanım kaynağı Azure Stack örneği için varsayılan abonelik gider.

Kiracı aboneliklerine CSP abonelikleri olduğundan, CSP iş ortağı kendi fatura gönderilir ve kullanım bilgilerini son müşterinin görünür değil.

## <a name="next-steps"></a>Sonraki adımlar

- CSP programı hakkında daha fazla bilgi için bkz: [bulut çözümü sağlayıcısı programı](https://partner.microsoft.com/solutions/microsoft-cloud-solutions).
- Azure yığını kaynak kullanım bilgilerini alma hakkında daha fazla bilgi için bkz: [kullanım ve faturalandırma Azure Stack'te](azure-stack-billing-and-chargeback.md).
