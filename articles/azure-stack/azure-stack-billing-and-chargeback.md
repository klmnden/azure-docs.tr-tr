---
title: Müşteri faturalandırma ve ücreti yansıtmaya Azure Stack'te | Microsoft Docs
description: Azure yığını kaynak kullanım bilgilerini alma hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2018
ms.author: sethm
ms.reviewer: alfredop
ms.openlocfilehash: fcfd5d4e9e2f30d769818df29cf8a76bd9113d4f
ms.sourcegitcommit: ab9514485569ce511f2a93260ef71c56d7633343
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2018
ms.locfileid: "45632428"
---
# <a name="usage-and-billing-in-azure-stack"></a>Kullanım ve faturalandırma Azure Stack'te

Bu makalede, Azure Stack kullanıcıları için kaynak kullanımı nasıl faturalandırılır açıklanır. Analiz ve Ücret geri için fatura bilgilerini nasıl erişilir öğrenebilirsiniz.

Azure Stack toplar ve kullanılan kaynaklar için kullanım verilerini gruplandırır. Daha sonra Azure Stack, Azure ticari için bu verileri iletir. Azure kullanımını faturalamak için gibi azure ticaret, Azure Stack kullanım için aynı şekilde düzenler.

Ayrıca, kullanım verilerini ve kendi faturalandırma veya ücret fatura bağdaştırıcısı kullanarak sistemi yedeklemek veya Microsoft Power BI gibi bir İş Zekası aracını aktarmak dışarı aktarma da alabilirsiniz.


## <a name="usage-pipeline"></a>Kullanım işlem hattı

Azure stack'teki her kaynak sağlayıcısı, kaynak kullanımı başına kullanım verilerini gönderir. Kullanım hizmeti düzenli aralıklarla (saatlik ve günlük) kullanım verilerini toplayan ve kullanım veritabanına depolar. Azure Stack operatörleri ve kullanıcıları saklı kullanım verilerini Azure Stack kaynak kullanım API'ler aracılığıyla erişebilirsiniz. 

Varsa [Azure Stack örneğinizin Azure Active Directory'ye](azure-stack-register.md), Azure Stack, Azure ticari için kullanım verileri göndermek için yapılandırılır. Verileri Azure'a karşıya yüklendikten sonra faturalandırma portalı üzerinden ya da Azure kaynak kullanım API'leri kullanarak erişebilirsiniz. Başvurmak [veri Kullanım raporlaması](azure-stack-usage-reporting.md) makale hangi kullanım verileri Azure'a daha bildirilir öğrenin.  

Aşağıdaki görüntüde, kullanım işlem hattında anahtar bileşenleri gösterilmektedir: 

![Kullanım işlem hattı](media\azure-stack-billing-and-chargeback\usagepipeline.png)

## <a name="what-usage-information-can-i-find-and-how"></a>Hangi kullanım bilgilerini bulabilirim ve nasıl?

Azure Stack kaynak sağlayıcıları (örneğin, işlem, depolama ve ağ), her abonelik için saatlik aralıklarla kullanım verilerini oluşturur. Kullanım verileri, kaynak adı, abonelik kullanılan ve kullanılan miktarı gibi kullanılan kaynak hakkında bilgi içerir. Ölçüm kimliği kaynaklar hakkında bilgi edinmek için başvurmak [kullanım API'si SSS](azure-stack-usage-related-faq.md) makalesi.

Kullanım verilerini topladıktan sonra [Azure'a bildirilen](azure-stack-usage-reporting.md) Azure fatura portalı üzerinden görüntülenebilir bir fatura oluşturulacak. 

> [!NOTE]  
> Veri Kullanım raporlaması için Azure Stack Geliştirme Seti ve kapasite modeli altında lisans Azure Stack tümleşik sistemi kullanıcılar için gerekli değildir. Azure Stack'te lisanslama hakkında daha fazla bilgi için bkz: [paketleme ve fiyatlandırma](https://azure.microsoft.com/mediahandler/files/resourcefiles/5bc3f30c-cd57-4513-989e-056325eb95e1/Azure-Stack-packaging-and-pricing-datasheet.pdf) veri sayfası.

Azure faturalama portalını ücrete tabi kaynaklar için kullanım verilerini gösterir. Azure Stack, ek ücrete tabi kaynaklar, kurulmuştur ve Azure Stack ortamınızda REST API veya PowerShell aracılığıyla erişilen kaynaklar için kullanım verilerini yakalar. Azure Stack operatörleri, tüm kullanıcı abonelikler için kullanım verileri elde edebilirsiniz. Bireysel kullanıcılar yalnızca kendi kullanım ayrıntılarını alabilirsiniz. 

## <a name="usage-reporting-for-multitenant-cloud-service-providers"></a>Çok kiracılı bulut hizmeti sağlayıcıları için raporlama kullanım

Sağlayıcı kullanım farklı Azure abonelikleri için ücret, böylece Azure Stack kullanarak birçok müşteri olan bir çok kiracılı bulut hizmeti sağlayıcısı (CSP) ayrı ayrı her müşteri kullanımını raporlamak isteyebilirsiniz. 

Her müşteri kimliklerini farklı bir Azure Active Directory (Azure AD) kiracısı tarafından temsil edilen olacaktır. Azure Stack, her Azure AD kiracısı için atama bir CSP aboneliği destekler. Kiracılar ve abonelikleri için temel Azure Stack kayıt ekleyebilirsiniz. Temel kayıt, tüm Azure yığınları için gerçekleştirilir. Bir abonelik için bir kiracı kayıtlı değilse kullanıcı Azure Stack kullanmaya devam edebilirsiniz ve bunların kullanımını temel kayıt için kullanılan abonelik gönderilir. 


## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack ile kaydetme](azure-stack-registration.md)

[Azure Stack kullanım verileri Azure'a rapor](azure-stack-usage-reporting.md)

[Sağlayıcı kaynak kullanım API'si](azure-stack-provider-resource-api.md)

[Kiracı kaynak kullanım API'si](azure-stack-tenant-resource-usage-api.md)

[Kullanım ile ilgili SSS](azure-stack-usage-related-faq.md)