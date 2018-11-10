---
title: Azure DNS nedir?
description: Microsoft Azure'deki DNS barındırma hizmetine genel bakış. Etki alanınızı Microsoft Azure'da barındırın.
author: vhorne
manager: jeconnoc
ms.service: dns
ms.topic: overview
ms.date: 9/24/2018
ms.author: victorh
ms.openlocfilehash: 9535ad73300406408a94a6f38f001e45ba7409a2
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50093104"
---
# <a name="what-is-azure-dns"></a>Azure DNS nedir?

Azure DNS, DNS etki alanları için Microsoft Azure altyapısı kullanılarak ad çözümlemesi olanağı sağlayan bir hizmettir. Etki alanlarınızı Azure'da barındırarak DNS kayıtlarınızı diğer Azure hizmetlerinde kullandığınız kimlik bilgileri, API’ler, araçlar ve faturalarla yönetebilirsiniz.

Azure DNS'yi kullanarak etki alanı adı satın alamazsınız. Yıllık ücret karşılığında [Azure Web Apps](https://docs.microsoft.com/azure/app-service/custom-dns-web-site-buydomains-web-app#buy-the-domain) veya üçüncü taraf etki alanı adı kayıt şirketlerini kullanarak etki alanı adı satın alabilirsiniz. Ardından etki alanlarınızı kayıt yönetimi için Azure DNS'de barındırabilirsiniz. Daha fazla bilgi için bkz. [Bir etki alanını Azure DNS'ye devretme](dns-domain-delegation.md).

Azure DNS aşağıdaki özellikleri içerir.

## <a name="reliability-and-performance"></a>Güvenilirlik ve performans

Azure DNS içindeki DNS etki alanları Azure'un global DNS ad sunucusu ağında barındırılır. Azure DNS ağı her noktaya yayın özelliğine sahiptir. DNS sorguları en yakın DNS sunucusu tarafından yanıtlanır ve bu sayede etki alanınız için yüksek performans ve kullanılabilirlik sağlar.

## <a name="security"></a>Güvenlik

 Azure DNS, Azure Resource Manager'ı temel alır ve aşağıdaki gibi özellikler sunar:

* [Rol tabanlı erişim denetimi](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#access-control): Resource Manager, belirli eylemlere kuruluşunuzda kimlerin erişebildiğini denetlemenize olanak tanır.

* [Etkinlik günlükleri](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#activity-logs): Sorun giderme sırasında bir hata bulmak veya kuruluşunuzdaki kullanıcının bir kaynağı nasıl değiştirdiğini izlemek için etkinlik günlüklerini kullanın.

* [Kaynak kilitleme](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-lock-resources): Bir aboneliği, kaynak grubunu veya kaynağı kilitlemenizi sağlar. Kilitleme işlemi kuruluşunuzdaki kullanıcıların kritik kaynakları yanlışlıkla silmesini veya değiştirmesini önler.

Daha fazla bilgi için bkz. [DNS bölgelerini ve kayıtlarını koruma](dns-protect-zones-recordsets.md). 


## <a name="ease-of-use"></a>Kullanım kolaylığı

 Azure DNS, Azure hizmetlerinizin DNS kayıtlarını yönetebilir ve Azure dışı kaynaklar için de DNS hizmeti sunabilir. Azure DNS, Azure portala tümleştirilmiştir ve diğer Azure hizmetlerinizle aynı kimlik bilgilerini, destek ekibini ve faturalandırma özelliklerini kullanır. 

DNS faturalarında, Azure’da barındırılan DNS bölgelerinin sayısı ve alınan DNS sorgularının sayısı temel alınır. Fiyatlandırma hakkında daha fazla bilgi için bkz. [Azure DNS fiyatlandırması](https://azure.microsoft.com/pricing/details/dns/).

Etki alanlarınızı ve kayıtlarınızı yönetmek için Azure portalı, Azure PowerShell cmdlet'lerini ve farklı platformlarda Azure CLI özelliklerini kullanabilirsiniz. Otomatik DNS yönetimi gerektiren uygulamalar REST API ve SDK'lar ile hizmetle tümleştirilebilir.

## <a name="customizable-virtual-networks-with-private-domains"></a>Özel etki alanlarıyla özelleştirilebilir sanal ağlar

Azure DNS genel önizleme sürümündeki bir özellikle özel DNS etki alanlarını da destekler. Bu özellik, özel sanal ağlarınızda Azure tarafından sağlanan adların yerine özel etki alanı adlarınızı kullanmanızı sağlar.

Daha fazla bilgi için bkz. [Azure DNS'yi özel etki alanları için kullanma](private-dns-overview.md).

## <a name="alias-records"></a>Diğer ad kayıtları

Azure DNS, diğer adı kayıt kümelerini destekler. Azure genel IP adresi veya Azure Traffic Manager profili gibi bir Azure kaynağına başvurmak için diğer ad kayıt kümesini kullanabilirsiniz. Temel alınan kaynağın IP adresi değişirse, diğer ad kayıt kümesi DNS çözümlemesi sırasında kendisini sorunsuz bir şekilde güncelleştirir. Diğer ad kaydı kümesi, hizmet örneğini işaret eder ve hizmet örneği bir IP adresi ile ilişkilidir. 

Ayrıca, apex veya çıplak etki alanınızı diğer ad kaydı kullanarak bir Traffic Manager profiline yönlendirebilirsiniz. Örneğin: contoso.com.

Daha fazla bilgi için bkz. [Azure DNS diğer ad kayıtlarına genel bakış](dns-alias.md).


## <a name="next-steps"></a>Sonraki adımlar

* DNS bölgeleri ve kayıtları hakkında bilgi edinmek için bkz. [DNS bölgeleri ve kayıtlarına genel bakış](dns-zones-records.md).

* Azure DNS'de bölge oluşturmayı öğrenmek için bkz. [DNS bölgesi oluşturma](./dns-getstarted-create-dnszone-portal.md).

* Azure DNS hakkında sık sorulan sorular için bkz. [Azure DNS Hakkında SSS](dns-faq.md).

