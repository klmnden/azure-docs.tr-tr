---
title: "Müşteri faturalama ve geri ödeme Azure yığınında | Microsoft Docs"
description: "Azure yığınından kaynak kullanım bilgilerini almak öğrenin."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/25/2018
ms.author: mabrigg
ms.reviewer: alfredop
ms.openlocfilehash: eca335797f48b7c44351655f17c8b6499f3d5999
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="usage-and-billing-in-azure-stack"></a>Kullanım ve fatura Azure yığınında

Bu makalede, Azure yığın kullanıcılar kaynak kullanım için nasıl faturalandırılır açıklanmaktadır. Analiz ve Ücret geri için faturalama bilgilerini nasıl erişilir öğrenebilirsiniz.

Azure yığın toplar ve kullanılan tüm kaynakları için kullanım verilerini gruplar ve bu verileri Azure ticaret iletir. Bunu Azure kullanımı için fatura gibi azure Commerce, Azure yığın kullanımı için aynı şekilde ödemenizi işler.

Ayrıca, kullanım verilerini ve kendi faturalama veya ücret bir fatura bağdaştırıcısı kullanarak sistem geri veya Microsoft Power BI gibi bir iş zekası araç vermek ve analizi için kullanmak verme alabilir.


## <a name="usage-pipeline"></a>Kullanım ardışık düzen

Her kaynak sağlayıcısı Azure yığınında kaynak kullanımı başına kullanım verilerini gösterir. Kullanım hizmeti düzenli aralıklarla (saatlik ve günlük) kullanım verilerini toplayan ve kullanım veritabanında depolar. Azure yığın işleçler ve kullanıcıların saklanan kullanım verilerini Azure yığın kaynak kullanım API'leri aracılığıyla erişebilirsiniz. 

Varsa [Azure yığın örneğinizi Azure Active Directory'ye](azure-stack-register.md), Azure yığını, Azure ticaret kullanım verileri göndermek için yapılandırılır. Verileri Azure'a yüklendikten sonra Fatura portalı üzerinden veya Azure kaynak kullanım API'lerini kullanarak erişebilirsiniz. Başvurmak [kullanım raporlama verilerini](azure-stack-usage-reporting.md) konu hangi kullanımı hakkında verileri Azure'a daha bildirilir öğrenin.  

Aşağıdaki resimde kullanım ardışık düzeninde anahtar bileşenleri gösterir: 

![Kullanım ardışık düzen](media\azure-stack-billing-and-chargeback\usagepipeline.png)

## <a name="what-usage-information-can-i-find-and-how"></a>Hangi kullanım bilgilerini bulabilirim, nasıl ve ne?

İşlem, depolama ve ağ gibi Azure yığın kaynak sağlayıcıları kullanım verileri her abonelik için saatlik aralıklarla oluşturur. Kullanım verileri kullanılan kaynak adı, kullanılan abonelik ve kullanılan miktarı gibi kaynak hakkında bilgi içerir. Metre kimliği kaynaklar hakkında bilgi edinmek için bkz [kullanım API SSS](azure-stack-usage-related-faq.md) makalesi.

Kullanım verilerini topladıktan sonra [Azure'a bildirilen](azure-stack-usage-reporting.md) Azure fatura portalı üzerinden görüntülenebilir bir fatura oluşturmak için. 


> [!NOTE]
> Kullanım verileri raporlama Azure yığın Geliştirme Seti için ve kapasite modeli altında lisans Azure tümleşik yığını sistem kullanıcılar için gerekli değildir. Azure yığınında lisanslama hakkında daha fazla bilgi için bkz: [paketleme ve fiyatlandırma](https://azure.microsoft.com/mediahandler/files/resourcefiles/5bc3f30c-cd57-4513-989e-056325eb95e1/Azure-Stack-packaging-and-pricing-datasheet.pdf) veri sayfası.

Azure fatura portalı ücrete tabi kaynakları için kullanım verilerini gösterir. Ücrete tabi kaynakların yanı sıra Azure yığın REST API'leri veya PowerShell ile Azure yığın ortamınızdaki erişebilmeniz için kaynakları daha geniş bir dizi için kullanım verilerini yakalar. Azure yığın işleçleri için tüm kullanıcı abonelikleri kullanım verileri alabilirsiniz. Tek tek kullanıcılar yalnızca kendi kullanım ayrıntılarını alabilirsiniz. 

## <a name="usage-reporting-for-multitenant-cloud-service-providers"></a>Çok kullanıcılı bulut hizmeti sağlayıcıları için raporlama kullanım

Sağlayıcı farklı Azure abonelikleri kullanımı şarj edebilir böylece Azure yığın kullanan çok sayıda müşteriniz sahip bir çok kullanıcılı bulut hizmeti sağlayıcısı (CSP) her müşteri kullanım ayrı olarak, rapor isteyebilirsiniz. 

Her bir müşteri farklı bir Azure Active Directory (Azure AD) Kiracı tarafından temsil edilen kimliklerini olacaktır. Azure yığını her Azure AD kiracısı atama bir CSP aboneliğine destekler. Kiracılar ve bunların abonelikleri temel Azure yığın kayıt ekleyebilirsiniz. Taban kayıt için tüm Azure yığınları yapılır. Bir abonelik için bir kiracı kayıtlı değil, kullanıcının Azure yığın kullanmaya devam edebilirsiniz ve bunların kullanım temel kayıt için kullanılan abonelikle gönderilir. 


## <a name="next-steps"></a>Sonraki adımlar

[Azure yığın ile kaydetme](azure-stack-registration.md)

[Azure yığın kullanım verileri Azure'a raporu](azure-stack-usage-reporting.md)

[Sağlayıcı kaynak kullanım API'si](azure-stack-provider-resource-api.md)

[Kiracı kaynak kullanım API'si](azure-stack-tenant-resource-usage-api.md)

[Kullanımı ile ilgili SSS](azure-stack-usage-related-faq.md)