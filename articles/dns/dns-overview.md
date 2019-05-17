---
title: Azure DNS nedir?
description: Microsoft Azure'deki DNS barındırma hizmetine genel bakış. Etki alanınızı Microsoft Azure'da barındırın.
author: vhorne
ms.service: dns
ms.topic: overview
ms.date: 3/21/2019
ms.author: victorh
ms.openlocfilehash: 530730dcf303a54803816e6f2af8787298c1c658
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65795587"
---
# <a name="what-is-azure-dns"></a>Azure DNS nedir?

Azure DNS, DNS etki alanları için Microsoft Azure altyapısı kullanılarak ad çözümlemesi olanağı sağlayan bir hizmettir. Etki alanlarınızı Azure'da barındırarak DNS kayıtlarınızı diğer Azure hizmetlerinde kullandığınız kimlik bilgileri, API’ler, araçlar ve faturalarla yönetebilirsiniz.

Azure DNS'yi kullanarak etki alanı adı satın alamazsınız. Bir yıllık ücret karşılığı bir etki alanı adı kullanarak alabilirsiniz [App Service etki alanları](https://docs.microsoft.com/azure/app-service/manage-custom-dns-buy-domain#buy-the-domain) veya bir üçüncü taraf etki alanı adı kayıt şirketi. Ardından etki alanlarınızı kayıt yönetimi için Azure DNS'de barındırabilirsiniz. Daha fazla bilgi için bkz. [Bir etki alanını Azure DNS'ye devretme](dns-domain-delegation.md).

Azure DNS aşağıdaki özellikleri içerir.

## <a name="reliability-and-performance"></a>Güvenilirlik ve performans

Azure DNS içindeki DNS etki alanları Azure'un global DNS ad sunucusu ağında barındırılır. Azure DNS ağı her noktaya yayın özelliğine sahiptir. DNS sorguları en yakın DNS sunucusu tarafından yanıtlanır ve bu sayede etki alanınız için yüksek performans ve kullanılabilirlik sağlar.

## <a name="security"></a>Güvenlik

 Azure DNS, Azure Resource Manager'ı temel alır ve aşağıdaki gibi özellikler sunar:

* [Rol tabanlı erişim denetimi](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview): Resource Manager, belirli eylemlere kuruluşunuzda kimlerin erişebildiğini denetlemenize olanak tanır.

* [Etkinlik günlükleri](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview): Sorun giderme sırasında bir hata bulmak veya kuruluşunuzdaki kullanıcının bir kaynağı nasıl değiştirdiğini izlemek için etkinlik günlüklerini kullanın.

* [Kaynak kilitleme](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-lock-resources): Bir aboneliği, kaynak grubunu veya kaynağı kilitlemenizi sağlar. Kilitleme işlemi kuruluşunuzdaki kullanıcıların kritik kaynakları yanlışlıkla silmesini veya değiştirmesini önler.

Daha fazla bilgi için bkz. [DNS bölgelerini ve kayıtlarını koruma](dns-protect-zones-recordsets.md). 

## <a name="dnssec"></a>DNSSEC

Azure DNS, DNSSEC şu anda desteklemiyor. Çoğu durumda, uygulamalarınızı sürekli olarak HTTPS/TLS kullanarak DNSSEC gereksinimini azaltabilir. DNSSEC DNS bölgeleriniz için kritik bir gereksinimse üçüncü taraf DNS barındırma sağlayıcıları ile bu bölgelerde barındırabilir.

## <a name="ease-of-use"></a>Kullanım kolaylığı

 Azure DNS, Azure hizmetlerinizin DNS kayıtlarını yönetebilir ve Azure dışı kaynaklar için de DNS hizmeti sunabilir. Azure DNS, Azure portala tümleştirilmiştir ve diğer Azure hizmetlerinizle aynı kimlik bilgilerini, destek ekibini ve faturalandırma özelliklerini kullanır. 

DNS faturalarında, Azure’da barındırılan DNS bölgelerinin sayısı ve alınan DNS sorgularının sayısı temel alınır. Fiyatlandırma hakkında daha fazla bilgi için bkz. [Azure DNS fiyatlandırması](https://azure.microsoft.com/pricing/details/dns/).

Etki alanlarınızı ve kayıtlarınızı yönetmek için Azure portalı, Azure PowerShell cmdlet'lerini ve farklı platformlarda Azure CLI özelliklerini kullanabilirsiniz. Otomatik DNS yönetimi gerektiren uygulamalar REST API ve SDK'lar ile hizmetle tümleştirilebilir.

## <a name="customizable-virtual-networks-with-private-domains"></a>Özel etki alanlarıyla özelleştirilebilir sanal ağlar

Azure DNS genel önizleme sürümündeki bir özellikle özel DNS etki alanlarını da destekler. Bu özellik, özel sanal ağlarınızda Azure tarafından sağlanan adların yerine özel etki alanı adlarınızı kullanmanızı sağlar.

Daha fazla bilgi için bkz. [Azure DNS'yi özel etki alanları için kullanma](private-dns-overview.md).

## <a name="alias-records"></a>Diğer ad kayıtları

Azure DNS, diğer adı kayıt kümelerini destekler. Bir Azure genel IP adresi, bir Azure Traffic Manager profili veya bir Azure Content Delivery Network (CDN) uç noktası gibi bir Azure kaynağına başvurmak için bir diğer ad kayıt kümesi kullanabilirsiniz. Temel alınan kaynağın IP adresi değişirse, diğer ad kayıt kümesi DNS çözümlemesi sırasında kendisini sorunsuz bir şekilde güncelleştirir. Diğer ad kaydı kümesi, hizmet örneğini işaret eder ve hizmet örneği bir IP adresi ile ilişkilidir.

Ayrıca, artık apex veya çıplak etki için bir Traffic Manager profili veya bir diğer ad kaydı kullanılarak CDN uç noktası işaret edebilir. Örneğin: contoso.com.

Daha fazla bilgi için bkz. [Azure DNS diğer ad kayıtlarına genel bakış](dns-alias.md).

## <a name="next-steps"></a>Sonraki adımlar

* DNS bölgeleri ve kayıtları hakkında bilgi edinmek için bkz. [DNS bölgeleri ve kayıtlarına genel bakış](dns-zones-records.md).

* Azure DNS'de bölge oluşturmayı öğrenmek için bkz. [DNS bölgesi oluşturma](./dns-getstarted-create-dnszone-portal.md).

* Azure DNS hakkında sık sorulan sorular için bkz. [Azure DNS Hakkında SSS](dns-faq.md).

