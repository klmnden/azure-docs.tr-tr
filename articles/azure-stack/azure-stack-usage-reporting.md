---
title: Azure Stack kullanım verileri Azure'a rapor | Microsoft Docs
description: Azure Stack'te reporting kullanım verilerini ayarlama konusunda bilgi edinin.
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
ms.date: 01/16/2019
ms.author: sethm
ms.reviewer: alfredop
ms.lastreviewed: 01/16/2019
ms.openlocfilehash: 1cf0d0f6676da78d9b8b159abce2c83c8a850c15
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55252175"
---
# <a name="report-azure-stack-usage-data-to-azure"></a>Azure Stack kullanım verileri Azure'a rapor

Kullanım verileri, tüketim verilerini olarak da bilinir, kullanılan kaynakların miktarını temsil eder.

Kullanım tabanlı faturalandırma modeli kullandığınız azure Stack çok düğümlü sistemleri, faturalandırma için Azure'a kullanım verilerini bildirmeniz gerekir. Azure Stack operatörleri, kendi Azure Stack örneği kullanım verilerini raporlamaya azure'a yapılandırmanız gerekir.

> [!IMPORTANT]
> Tüm iş yükleri [Kiracı aboneliklerine altında dağıtılmalıdır](#are-users-charged-for-the-infrastructure-vms) lisans koşullarını Azure Stack ile uyum sağlamak için.

Veri Kullanım raporlaması,-,-kullandıkça modeli altında lisans Azure Stack çok düğümlü kullanıcılar için gereklidir. İsteğe bağlı kapasite modeli altında lisans müşteriler için (bkz [sayfa satın alma](https://azure.microsoft.com/overview/azure-stack/how-to-buy/). Azure Stack operatörlerinin Azure Stack geliştirme Seti'ni kullanıcılar için kullanım verileri rapor ve özelliği test etmek. Ancak, kullanıcılar bunlara uygulanmaz kullanımı için ücretlendirilmez.

![Fatura akışı](media/azure-stack-usage-reporting/billing-flow.png)

Kullanım verileri, Azure yığını, Azure köprü üzerinden azure'a gönderilir. Azure'da, ticaret sistemi kullanım verileri işler ve fatura oluşturur. Fatura oluşturulduktan sonra bir Azure aboneliği sahibi görüntüleyebilir ve indirdiği [Azure hesap Merkezi](https://account.windowsazure.com/Subscriptions). Azure Stack nasıl lisanslanır hakkında bilgi edinmek için [paketleme ve belge fiyatlandırma Azure Stack](https://go.microsoft.com/fwlink/?LinkId=842847).

## <a name="set-up-usage-data-reporting"></a>Set up reporting kullanım verileri

Kullanım verilerini raporlama ayarlamak için şunları yapmanız gerekir [Azure Stack örneğinizin Azure ile kaydedin](azure-stack-register.md). Azure Stack Azure'a bağlanır ve kullanım verileri gönderir, Azure Stack, Azure köprüsü bileşeninin kayıt işleminin bir parçası olarak yapılandırılır. Azure yığını, aşağıdaki kullanım verileri Azure'a gönderilir:

- **Ölçüm kimliği** – tüketilen kaynağın benzersiz kimliği.
- **Miktar** – kaynak kullanımının tutar.
- **Konum** – geçerli Azure Stack kaynak dağıtıldığı konum.
- **Kaynak URI** – tam olarak hangi kullanım raporlanır kaynağın URI'sini.
- **Abonelik kimliği** – yerel (Azure Stack) abonelik Azure Stack kullanıcının abonelik kimliği.
- **Zaman** – Kullanım verilerinin başlangıç ve bitiş zamanı. Bir gecikme olması arasındaki zaman bu kaynakları Azure Stack'te tüketilir ve kullanım verileri için ticaret bildirildiğinde. 24 saatte bir Azure Stack toplamalar kullanım verileri ve azure'da ticaret ardışık düzenine raporlama kullanım verileri başka bir birkaç saat sürer. Bu nedenle, kısa bir süre önce gece yarısından kullanım Azure'da sonraki gün görünebilir.

## <a name="generate-usage-data-reporting"></a>Kullanım raporlama verilerini oluştur

- Kullanım verilerini raporlama test etmek için Azure Stack'te birkaç kaynak oluşturun. Örneğin, oluşturabileceğiniz bir [depolama hesabı](azure-stack-provision-storage-account.md), [Windows Server VM](azure-stack-provision-vm.md) ve bir Linux VM çekirdek kullanımı nasıl bildirildiği görmek için temel ve standart SKU'lar ile. Farklı kaynak türleri için kullanım verilerini altında farklı ölçümleri raporlanır.

- Kaynaklarınız için kaç saat çalışan bırakın. Kullanım bilgileri yaklaşık olarak saatte bir toplanır. Topladıktan sonra bu verileri Azure'a aktarılan ve Azure ticaret sisteminde işlenir. Bu işlem birkaç saat sürebilir.

## <a name="view-usage---csp-subscriptions"></a>Görünüm kullanımı - CSP abonelikleri

Bir CSP aboneliği kullanarak, Azure Stack kaydettiyseniz, kullanımı ve ücretleri Azure tüketim görüntülediğiniz aynı şekilde görüntüleyebilirsiniz. Azure Stack kullanım faturanızı ve aracılığıyla mutabakat dosyasına dahil edilecektir [iş ortağı Merkezi](https://partnercenter.microsoft.com/partner/home). Mutabakat dosyasına aylık güncelleştirilir. En son Azure Stack kullanım bilgilerini erişmeniz gerekiyorsa, iş ortağı merkezi API'leri kullanabilirsiniz.

![iş ortağı Merkezi](media/azure-stack-usage-reporting/partner-center.png)

## <a name="view-usage--enterprise-agreement-subscriptions"></a>Kurumsal Anlaşma abonelikleri – kullanımını görüntüleyin

Azure Stack kullanarak bir Kurumsal Sözleşme aboneliğine kaydolduysanız, kullanım ve Ücret görüntüleyebilirsiniz [EA portal](https://ea.azure.com/). Azure Stack kullanım Raporlar bölümünde bu portalda Azure kullanım yanı sıra gelişmiş indirmeleri dahil edilir. 

## <a name="view-usage--other-subscriptions"></a>Diğer abonelikler – kullanımını görüntüleyin

Azure Stack kaydettiyseniz kullanarak başka bir aboneliğe yazın. Örneğin, bir Kullandıkça Öde aboneliği, Azure hesap Merkezi'nde kullanımı ve ücretleri görüntüleyebilirsiniz. Oturum [Azure hesap Merkezi](https://account.windowsazure.com/Subscriptions) Azure Hesap Yöneticisi ve Azure Stack kaydolmak için kullandığınız Azure aboneliğini seçin. Azure Stack kullanım verilerini, aşağıdaki görüntüde gösterildiği gibi her kullanılan kaynaklar için ücret tutarı görüntüleyebilirsiniz:

![Fatura akışı](media/azure-stack-usage-reporting/pricing-details.png)

Fiyat $0,00 gösterilen şekilde Azure Stack Geliştirme Seti Azure Stack kaynakları, ücretlendirilmez.

## <a name="which-azure-stack-deployments-are-charged"></a>Hangi Azure Stack dağıtımları ücretlendirilir mi?

Azure Stack Geliştirme Seti için kaynak kullanımı ücretsizdir. Azure Stack çok düğümlü sistemleri, iş yükü Vm'lerinden, depolama hizmetleri ve uygulama hizmetleri ücretlendirilir.

## <a name="are-users-charged-for-the-infrastructure-vms"></a>Kullanıcılar, altyapı Vm'leri için ücretlendirilir mi?

Hayır. Bazı Azure Stack kaynak sağlayıcısı sanal makineleri için kullanım verilerini Azure'a bildirildi, ancak bu sanal makineler için ya da Azure Stack altyapısının etkinleştirmek dağıtım sırasında oluşturulan sanal makineleri için ücretlendirme yoktur.  

Kullanıcılar, yalnızca Kiracı aboneliklerine altında çalışan VM'ler için ücretlendirilirsiniz. Azure Stack lisans koşullarına uyduğunuz için Kiracı abonelik altındaki tüm iş yükleri dağıtılması gerekir.

## <a name="i-have-a-windows-server-license-i-want-to-use-on-azure-stack-how-do-i-do-it"></a>Azure Stack'te kullanmak istediğiniz bir Windows Server lisansım, nasıl musunuz?

Mevcut lisanslarınızı kullanarak kullanım ölçümleri oluşturma önler. Mevcut Windows Server lisansları "mevcut yazılım Azure Stack ile kullanma" bölümünde açıklandığı gibi Azure Stack'te kullanılabilir [Azure Stack lisanslama Kılavuzu](https://go.microsoft.com/fwlink/?LinkId=851536). Açıklandığı, müşterilere Windows Server sanal makinelerini dağıtma gerekir [hibrit teklifi Windows Server lisansı için](../virtual-machines/windows/hybrid-use-benefit-licensing.md) makale mevcut lisanslarını kullanmak için.

## <a name="which-subscription-is-charged-for-the-resources-consumed"></a>Hangi abonelik tüketilen kaynaklar için ücretlendirilir?

Ne zaman sağlanan abonelik [Azure Stack Azure ile kaydetme](azure-stack-register.md) ücretlendirilir.

## <a name="what-types-of-subscriptions-are-supported-for-usage-data-reporting"></a>Abonelikleri hangi türde veri Kullanım raporlaması için destekleniyor mu?

Azure Stack için çok düğümlü, Kurumsal Anlaşma (EA) ve CSP abonelikleri desteklenir. Azure Stack Geliştirme Seti için Kurumsal Anlaşma (EA), Kullandıkça Öde, CSP ve MSDN Abonelikleri kullanım raporlama verilerini destekler.

## <a name="does-usage-data-reporting-work-in-sovereign-clouds"></a>Kullanım verilerini bağımsız bulutlarda iş raporlama mu?

Azure Stack geliştirme Seti'ni veri Kullanım raporlaması genel Azure sistemde oluşturulan abonelikleri gerektirir. Kullanım raporlama verilerini desteklemeyen şekilde bağımsız bulutlarda (Azure kamu, Azure Almanya ve Azure China Bulutları) birinde oluşturulmuş abonelikler Azure ile kaydedilemez.

## <a name="why-doesnt-the-usage-reported-in-azure-stack-match-the-report-generated-from-azure-account-center"></a>Neden Azure Stack'te bildirilen kullanım Azure hesap Merkezi'nden oluşturulan raporu eşleşmeyen?

Her zaman Azure Stack kullanım API'leri tarafından bildirilen kullanım verilerini ve Azure hesap Merkezi tarafından bildirilen kullanım verileri arasında bir gecikme olur. Azure ticari için Azure Stack kullanım verilerini karşıya yükleme için gereken süreyi gecikmesidir. Bu gecikme nedeniyle kısa bir süre önce gece yarısından kullanım Azure'da sonraki gün görünebilir. Kullanırsanız [Azure Stack kullanım API'leri](azure-stack-provider-resource-api.md)ve Azure fatura Portalı'nda bildirilen kullanım sonuçları karşılaştırmak, bir farkı görebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Sağlayıcı kullanım API’si](azure-stack-provider-resource-api.md)  
* [Kiracı kullanım API’si](azure-stack-tenant-resource-usage-api.md)
* [Kullanım Hakkında SSS](azure-stack-usage-related-faq.md)
* [Kullanımını yönetmenize ve bir bulut hizmeti sağlayıcısı olarak faturalama](azure-stack-add-manage-billing-as-a-csp.md)
