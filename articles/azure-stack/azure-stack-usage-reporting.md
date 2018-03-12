---
title: "Raporu Azure yığın kullanım verilerini Azure'a | Microsoft Docs"
description: "Azure yığınında raporlama Kullanım verilerinin ayarlanacağını öğrenin."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/22/2018
ms.author: mabrigg
ms.reviewer: alfredop
ms.openlocfilehash: 29d53f63bf3d551823ca27df04f0e385a92cdec7
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="report-azure-stack-usage-data-to-azure"></a>Azure yığın kullanım verileri Azure'a raporu 

Kullanım verileri Tüketim verileri olarak da bilinir kullanılan kaynakları miktarını temsil eder. 

Tüketim tabanlı faturalama modelini kullanan azure yığını çok düğümlü sistemler için Azure fatura amaç için kullanım verilerini bildirmeniz gerekir.  Azure yığın işleçleri kendi Azure yığın örneği Azure kullanım verilerini raporlamaya yapılandırmanız gerekir.

> [!NOTE]
> Kullanım verileri raporlama, ödeme olarak-size-kullanım modeli altında lisans Azure yığını çok düğümlü kullanıcılar için gereklidir. Kapasite modeli altında lisans müşterileri için isteğe bağlı (bkz [sayfa satın alma](https://azure.microsoft.com/overview/azure-stack/how-to-buy/ to learn more about pricing in Azure Stack)). Azure yığın Geliştirme Seti kullanıcılar için Azure yığın işleçleri kullanım verileri rapor ve özelliği test etmek. Ancak, kullanıcıların bunlar kullanan tüm kullanımı için ücret alınmaz. 


![Fatura akışı](media/azure-stack-usage-reporting/billing-flow.png)

Kullanım verileri Azure yığınından Azure Azure köprü üzerinden gönderilir. Azure, ticaret sistem kullanım verilerini işler ve fatura oluşturur. Fatura oluşturulduktan sonra Azure abonelik sahibi görüntüleyebilir ve indirin [Azure hesap Merkezi](https://account.windowsazure.com/Subscriptions). Azure yığın nasıl lisanslanır hakkında bilgi edinmek için bkz [Azure paketleme ve belge fiyatlandırma yığını](https://go.microsoft.com/fwlink/?LinkId=842847&clcid=0x409).

## <a name="set-up-usage-data-reporting"></a>Set up reporting kullanım verileri

Kullanım verileri raporlama yukarı ayarlamak için şunları yapmalısınız [Azure yığın örneğinizi Azure ile kaydedin](azure-stack-register.md). Azure yığın Azure'a bağlanır ve kullanım verileri gönderir, Azure yığın Azure köprüsü bileşeninin kayıt işleminin bir parçası olarak yapılandırılır. Aşağıdaki kullanım verilerini Azure yığınından Azure'a gönderilir:

- **Ölçüm kimliği** – tüketilen kaynak için benzersiz kimlik.
- **Miktar** – kaynak kullanım miktarı.
- **Konum** – geçerli Azure yığın kaynak dağıtıldığı konumu.
- **Kaynak URI'si** – tam olarak hangi için kullanım bildirilir kaynak URI'si.
- **Abonelik kimliği** – Azure yığın kullanıcının abonelik kimliği. Yerel (Azure yığını) abonelik budur.
- **Zaman** – Kullanım verilerinin başlangıç ve bitiş zamanı. Bir gecikme olması arasındaki zaman bu kaynakları Azure yığınında tüketilen ve kullanım verileri için ticaret bildirildiğinde. 24 saatte için Azure yığın toplamalar kullanım verilerini ve ticaret ardışık düzeninde Azure raporlama kullanım verileri başka bir birkaç saat sürer. Bu nedenle, kısa süre önce gece yarısından kullanım Azure'da sonraki gün görünebilirler.

## <a name="generate-usage-data-reporting"></a>Kullanım verileri raporlama oluştur

1. Raporlama Kullanım verilerinin sınamak için Azure yığınında birkaç kaynakları oluşturun. Örneğin, oluşturabileceğiniz bir [depolama hesabı](azure-stack-provision-storage-account.md), [Windows Server VM](azure-stack-provision-vm.md) ve bir Linux VM çekirdek kullanım nasıl bildirilir görmek için temel ve standart SKU'ları ile. Farklı türdeki kaynakların kullanım verileri altında farklı ölçümler raporlanır.

2. Birkaç saat çalışması kaynaklarınızı bırakın. Kullanım bilgilerini yaklaşık olarak saatte toplanır. Topladıktan sonra bu verileri Azure'a aktarılan ve Azure ticaret sisteme işlenir. Bu işlem birkaç saat sürebilir.

## <a name="view-usage---csp-subscriptions"></a>CSP abonelikleri - kullanımını görüntüleyin

Bir CSP aboneliği kullanarak Azure yığın kaydettiyseniz, Azure tüketim görüntülemek aynı şekilde kullanımı ve ücretleri görüntüleyebilirsiniz. Azure yığın kullanımı, fatura ve aracılığıyla kullanılabilen uzlaştırma dosya dahil olacak [ortağı Merkezi'nde](https://partnercenter.microsoft.com/partner/home). Uzlaştırma dosyası aylık güncelleştirilir. En son Azure yığın kullanım bilgileri erişmeniz gerekiyorsa, iş ortağı merkezi API'ları kullanabilirsiniz.

   ![iş ortağı Merkezi](media/azure-stack-usage-reporting/partner-center.png)


## <a name="view-usage--enterprise-agreement-subscriptions"></a>Kurumsal Anlaşma abonelikleri – kullanımını görüntüleyin

Bir Kurumsal Anlaşma aboneliği kullanarak Azure yığın kaydettiyseniz, kullanım ve ücretlere görüntüleyebilirsiniz [EA Portal](https://ea.azure.com/). Azure yığın kullanımı EA portal Raporlar bölümünde Azure kullanım yanı sıra gelişmiş yüklemeleri eklenecektir. 

## <a name="view-usage--other-subscriptions"></a>Görünüm kullanımı – diğer abonelikler

Başka abonelik türü, Kullandıkça Öde aboneliğine, kullanarak Azure yığın kaydolduysanız kullanımı ve ücretleri Azure hesap Merkezi'nde görüntüleyebilirsiniz. Oturum [Azure hesap Merkezi](https://account.windowsazure.com/Subscriptions) Azure Hesap Yöneticisi ve Azure yığın kaydetmek için kullanılan Azure aboneliğini seçin. Her kullanılan kaynaklar için aşağıdaki resimde gösterildiği gibi ücret Azure yığın kullanım verilerini görüntüleyebilirsiniz:

   ![Fatura akışı](media/azure-stack-usage-reporting/pricing-details.png)

Fiyat 0,00 gösterilen şekilde Azure yığın Geliştirme Seti için Azure yığın kaynakları ücretlendirilen değil. Azure yığını çok düğümlü genel olarak kullanılabilir hale geldiğinde, bu kaynakların her biri için gerçek maliyet görebilirsiniz.

## <a name="which-azure-stack-deployments-are-charged"></a>Hangi Azure yığın dağıtımlara ücretlendirilen?

Azure yığın Geliştirme Seti için kaynak kullanımı ücretsizdir. Azure yığını çok düğümlü sistemler için iş yükü sanal makineleri ve depolama hizmetleri uygulama hizmetleri ücretlendirilen ise.

## <a name="are-users-charged-for-the-infrastructure-vms"></a>Kullanıcıların sanal makineleri altyapısı için ücretlendirilirsiniz?

Hayır. Bazı Azure yığın kaynak sağlayıcısı sanal makineleri için kullanım verilerini Azure'a bildirdi ancak Azure yığın altyapı etkinleştirmek dağıtım sırasında oluşturulan VM'ler veya bu sanal makineleri için hiçbir ücret vardır.  

Kullanıcılar yalnızca Kiracı aboneliklerine altında Çalıştır VM'ler için sizden ücret kesilir. Tüm iş yükleri, Kiracı abonelikler Azure yığınının lisans koşullarına uymanız altında dağıtılmalıdır.

## <a name="i-have-a-windows-server-license-i-want-to-use-on-azure-stack-how-do-i-do-it"></a>Azure yığında kullanacağınız bir Windows Server Lisans sahip, bunu nasıl yaparım?

Varolan lisanslarla kullanım ölçümler oluşturma önler. Var olan Windows Server lisansları "mevcut yazılım Azure yığın ile kullanarak" bölümünde açıklandığı gibi Azure yığınında kullanılabilir [Azure yığın lisans Kılavuzu](https://go.microsoft.com/fwlink/?LinkId=851536&clcid=0x409). Müşteriler ihtiyaç açıklandığı gibi Windows Server sanal makineleri dağıtmak [Windows Server Lisans için karma avantajı](https://docs.microsoft.com/azure/virtual-machines/windows/hybrid-use-benefit-licensing) varolan lisanslarını kullanmak için konu.

## <a name="which-subscription-is-charged-for-the-resources-consumed"></a>Kullanılan kaynaklar için hangi abonelik doludur?
Ne zaman sağlanan abonelik [Azure ile Azure yığın kaydetme](azure-stack-register.md) doludur.

## <a name="what-types-of-subscriptions-are-supported-for-usage-data-reporting"></a>Hangi türde abonelik verileri Kullanım raporlaması için destekleniyor mu?

Kurumsal Anlaşma (Kurumsal Sözleşme) ve CSP abonelikleri Azure yığın multinode için desteklenir. Azure yığın Geliştirme Seti için kullanım verileri raporlama Kurumsal Anlaşma (Kurumsal Sözleşme), Kullandıkça Öde, CSP ve MSDN aboneliği destekler.

## <a name="does-usage-data-reporting-work-in-sovereign-clouds"></a>Sovereign bulut iş raporlama Kullanım verilerinin mu?

Azure yığın Development Kit'te kullanım verileri raporlama genel Azure sistemde oluşturulan abonelikleri gerektirir. Kullanım verileri raporlama desteklemeyen şekilde sovereign Bulutlar (Azure kamu, Azure Almanya ve Azure Çin bulut) biri ile oluşturulmuş abonelikleri Azure ile kaydedilemedi.

## <a name="how-can-users-identify-azure-stack-usage-data-in-the-azure-billing-portal"></a>Nasıl kullanıcılar Azure yığın kullanım verilerini Azure fatura Portalı'nda belirleyebilir misiniz?

Kullanıcılar, Azure yığın kullanım verileri kullanım ayrıntılarını dosyasında görebilirsiniz. Kullanım ayrıntılarını dosya elde etme hakkında bilmeniz başvurmak [Azure hesap merkezi makaleden kullanım dosyasını karşıdan](https://docs.microsoft.com/azure/billing/billing-download-azure-invoice-daily-usage-date#download-usage-from-the-account-center-csv). Kullanım ayrıntılarını dosyasını Azure yığın depolama ve sanal makineleri belirlemek Azure yığın ölçümler içerir. Azure yığında kullanılan tüm kaynakları "Azure yığını." adlı bölge altında bildirilir

## <a name="why-doesnt-the-usage-reported-in-azure-stack-match-the-report-generated-from-azure-account-center"></a>Neden Azure yığınında bildirilen kullanım Azure hesap Merkezi'nden oluşturulan rapor eşleşmiyor?

Kullanım verileri Azure yığın kullanımı API'leri ve Azure hesap Merkezi tarafından bildirilen kullanım verileri tarafından bildirilen delaybetween olduğu her zaman... Bu gecikme Azure yığın kullanım verileri Azure ticaret yüklemek için gereken zamandır. Bu gecikme nedeniyle kısa süre önce gece yarısından kullanım Azure'da sonraki gün görünebilirler. Kullanırsanız [Azure yığın kullanımı API'leri](azure-stack-provider-resource-api.md)ve Azure fatura Portalı'nda bildirilen kullanım sonuçları karşılaştırmak, bir farkı görebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Sağlayıcı kullanım API’si](azure-stack-provider-resource-api.md)  
* [Kiracı kullanım API’si](azure-stack-tenant-resource-usage-api.md)
* [Kullanım Hakkında SSS](azure-stack-usage-related-faq.md)
* [Kullanım yönetmek ve bir bulut hizmeti sağlayıcısı olarak faturalama](azure-stack-add-manage-billing-as-a-csp.md)
