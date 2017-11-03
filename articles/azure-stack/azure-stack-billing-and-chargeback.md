---
title: "Müşteri faturalama ve geri ödeme Azure yığınında | Microsoft Docs"
description: "Azure yığınından kaynak kullanım bilgilerini almak öğrenin."
services: azure-stack
documentationcenter: 
author: AlfredoPizzirani
manager: byronr
editor: 
ms.assetid: a9afc7b6-43da-437b-a853-aab73ff14113
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: alfredop
ms.openlocfilehash: ea7510c239ee07a9a27f3e682e61a6b08eb5694d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="usage-and-billing-in-azure-stack"></a>Kullanım ve fatura Azure yığınında

Kullanım, bir kullanıcı tarafından tüketilen kaynaklarının miktarını temsil eder. Azure yığın her kullanıcı için kullanım bilgilerini toplar ve bunları faturalandırmak üzere kullanır. Bu makalede Azure yığın kullanıcılar kaynak kullanım için nasıl faturalandırılır ve nasıl faturalama bilgilerini erişilen analytics, geri ödeme, vb. için açıklanır.

Azure yığını tüm kaynaklar için kullanım verilerini toplama ve bu verileri Azure ticaret iletmek için altyapı içerir. Bu verilere erişmek ve fatura bağdaştırıcısı kullanarak bir faturalandırma sistemine dışarı veya Microsoft Power BI gibi bir iş zekası araç verin. Verdikten sonra bu fatura bilgilerini analizi için kullanılan veya bir geri ödeme sisteme aktarılan.

![Kavramsal model için faturalama Azure yığın bağlanma bir fatura bağdaştırıcısı uygulama](media/azure-stack-billing-and-chargeback/image1.png)

## <a name="usage-pipeline"></a>Kullanım ardışık düzen

Her kaynak sağlayıcısı Azure yığınında kaynak kullanımı başına kullanım verilerini gösterir. Kullanım hizmeti düzenli aralıklarla (saatlik veya günlük) bu kullanım verilerini toplayan ve kullanım veritabanında depolar. Saklanan kullanım verilerini Azure yığın işleçler ve kullanıcılar tarafından kullanım API'lerini kullanarak yerel olarak erişilebilir. 

Varsa [Azure yığın örneğinizi Azure Active Directory'ye](azure-stack-register.md), kullanım köprüsü Azure ticaret kullanım verileri göndermek için yapılandırılır. Verileri Azure içinde kullanılabilir duruma getirildikten sonra Fatura portalı üzerinden veya Azure kullanım API'nin kullanarak erişebilirsiniz. Başvurmak [kullanım raporlama verilerini](azure-stack-usage-reporting.md) konu hangi kullanımı hakkında verileri Azure'a daha bildirilir öğrenin. 

Aşağıdaki resimde kullanım ardışık düzeninde anahtar bileşenleri gösterir:

![Kullanım ardışık düzen](media/azure-stack-billing-and-chargeback/usagepipeline.png)

## <a name="what-usage-information-can-i-find-and-how"></a>Hangi kullanım bilgilerini bulabilirim, nasıl ve ne?

İşlem, depolama ve ağ gibi Azure yığın kaynak sağlayıcıları kullanım verileri her abonelik için saatlik aralıklarla oluşturur. Kullanım verileri kullanılan kaynak adı, kullanılan abonelik, kullanılan miktarı gibi kaynak, vb. hakkında bilgiler içerir. Metre kimliği kaynaklar hakkında bilgi edinmek için bkz [kullanım API SSS](azure-stack-usage-related-faq.md) makalesi. 

Kullanım verilerini topladıktan sonra [Azure'a bildirilen](azure-stack-usage-reporting.md) Azure fatura portalı üzerinden görüntülenebilir bir fatura oluşturmak için. 

> [!NOTE]
> Kullanım verileri raporlama Azure yığın Geliştirme Seti için ve kapasite modeli altında lisans Azure tümleşik yığını sistem kullanıcılar için gerekli değildir. Azure yığınında lisanslama hakkında daha fazla bilgi için bkz: [paketleme ve fiyatlandırma](https://azure.microsoft.com/mediahandler/files/resourcefiles/5bc3f30c-cd57-4513-989e-056325eb95e1/Azure-Stack-packaging-and-pricing-datasheet.pdf) veri sayfası.

Azure fatura portalı kullanım verileri yalnızca ücrete tabi kaynakları gösterir. Ücrete tabi kaynakların yanı sıra Azure yığın REST API'leri veya PowerShell ile Azure yığın ortamınızdaki erişebilmeniz için kaynakları daha geniş bir dizi için kullanım verilerini yakalar. Bir kullanıcı yalnızca kendi kullanım ayrıntılarını alabilirsiniz ancak azure yığın işleçleri tüm kullanıcı aboneliklerini kullanım verileri alabilir.

## <a name="retrieve-usage-information"></a>Kullanım bilgilerini alma

Kullanım verilerini oluşturmak için çalışan ve etkin olarak sistem, örneğin kullanan kaynaklar, etkin bir sanal makineye veya bazı veri vb. içeren bir depolama hesabı olmalıdır. Dikey emin olmak için izleme olup Azure yığın Marketi'nde çalıştıran herhangi bir kaynağı var, bir sanal makine (VM) dağıtmanıza ve VM doğrulayın konusunda emin değilseniz çalıştığı. Kullanım verileri görüntülemek için aşağıdaki PowerShell cmdlet'lerini kullanın:

1. [PowerShell için Azure yığın yükleyin.](azure-stack-powershell-install.md)
2. [Azure yığın kullanıcının yapılandırmak](user/azure-stack-powershell-configure-user.md) veya [Azure yığın işlecin](azure-stack-powershell-configure-admin.md) PowerShell ortamı 

3. Kullanım verilerini almak için kullanın [Get-UsageAggregates](/powershell/module/azurerm.usageaggregates/get-usageaggregates) PowerShell cmdlet:

   ```powershell
   Get-UsageAggregates -ReportedStartTime "<Start time for usage reporting>" -ReportedEndTime "<end time for usage reporting>" -AggregationGranularity <Hourly or Daily>
   ```

## <a name="next-steps"></a>Sonraki adımlar

[Azure yığın kullanım verileri Azure'a raporu](azure-stack-usage-reporting.md)

[Sağlayıcı kaynak kullanım API'si](azure-stack-provider-resource-api.md)

[Kiracı kaynak kullanım API'si](azure-stack-tenant-resource-usage-api.md)

[Kullanımı ile ilgili SSS](azure-stack-usage-related-faq.md)

