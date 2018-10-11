---
title: Azure DNS nedir?
description: Microsoft Azure'deki DNS barındırma hizmetine genel bakış. Etki alanınızı Microsoft Azure'da barındırın.
author: vhorne
manager: jeconnoc
ms.service: dns
ms.topic: overview
ms.date: 9/24/2018
ms.author: victorh
ms.openlocfilehash: e3e04bf7e35b22a56465810f476323ed217e047a
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46967634"
---
# <a name="what-is-azure-dns"></a>Azure DNS nedir?

Azure DNS, DNS etki alanları için Microsoft Azure altyapısı kullanılarak ad çözümlemesi olanağı sağlayan bir hizmettir. Etki alanlarınızı Azure'da barındırarak DNS kayıtlarınızı diğer Azure hizmetlerinde kullandığınız kimlik bilgileri, API’ler, araçlar ve faturalarla yönetebilirsiniz.

Azure DNS'yi kullanarak etki alanı adı satın alamazsınız. Yıllık ücret karşılığında [Azure Web Apps](https://docs.microsoft.com/azure/app-service/custom-dns-web-site-buydomains-web-app#buy-the-domain) veya üçüncü taraf etki alanı adı kayıt şirketlerini kullanarak etki alanı adı satın alabilirsiniz. Ardından etki alanlarınızı kayıt yönetimi için Azure DNS'de barındırabilirsiniz. Ayrıntılar için bkz. [Azure DNS'ye bir etki alanı devretme](dns-domain-delegation.md).

Azure DNS aşağıdaki özellikleri içerir:

## <a name="reliability-and-performance"></a>Güvenilirlik ve performans

Azure DNS içindeki DNS etki alanları Azure'un global DNS ad sunucusu ağında barındırılır. Azure DNS ağı her noktaya yayın özelliğine sahip olduğundan DNS sorguları en yakın DNS sunucusu tarafından yanıtlanır. Bu sayede etki alanınız hem yüksek performans hem de yüksek kullanılabilirlik özelliklerine sahip olur.

## <a name="security"></a>Güvenlik

Azure DNS, Azure Resource Manager hizmetini temel alır. Bu sayede aşağıdakiler gibi Resource Manager özelliklerinden faydalanabilirsiniz:

* [Rol tabanlı erişim denetimi](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#access-control): Resource Manager, belirli eylemlere kuruluşunuzda kimlerin erişebildiğini denetlemenize olanak tanır.

* [Etkinlik günlükleri](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#activity-logs): Sorun giderme sırasında bir hata bulmak veya kuruluşunuzdaki kullanıcının bir kaynağı nasıl değiştirdiğini izlemek için etkinlik günlüklerini kullanın.

* [Kaynak kilitleme](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-lock-resources): Kuruluşunuzdaki diğer kullanıcıların yanlışlıkla silmesini veya kritik kaynakları değiştirmesini önleme amacıyla belirli bir aboneliği, kaynak grubunu veya kaynağı kilitleme.

Daha fazla bilgi için bkz. [DNS bölgelerini ve kayıtlarını koruma](dns-protect-zones-recordsets.md). 


## <a name="ease-of-use"></a>Kullanım kolaylığı

Azure DNS hizmeti, Azure hizmetlerinizin DNS kayıtlarını yönetebilir ve Azure dışı kaynaklar için de DNS hizmeti sunabilir. Azure DNS, Azure portala tümleştirilmiştir ve diğer Azure hizmetlerinizle aynı kimlik bilgilerini, destek ekibini ve faturalandırma özelliklerini kullanır. 

DNS faturalarında, Azure’da barındırılan DNS bölgelerinin sayısı ve DNS sorgularının sayısı temel alınır. Fiyatlandırma hakkında daha fazla bilgi için bkz. [Azure DNS Fiyatlandırması](https://azure.microsoft.com/pricing/details/dns/).

Etki alanlarınızı ve kayıtlarınızı yönetmek için Azure portalı, Azure PowerShell cmdlet'lerini ve farklı platformlarda Azure CLI özelliklerini kullanabilirsiniz. Otomatik DNS yönetimi gerektiren uygulamalar REST API ve SDK'lar ile hizmetle tümleştirilebilir.

## <a name="customizable-virtual-networks-with-private-domains"></a>Özel etki alanlarıyla özelleştirilebilir sanal ağlar

Azure DNS genel önizleme sürümündeki özel DNS etki alanlarını da destekler. Bu da özel sanal ağlarınızda Azure tarafından sağlanan adların yerine özel etki alanı adlarınızı kullanmanızı sağlar.

Daha fazla bilgi için bkz. [Azure DNS'yi özel etki alanları için kullanma](private-dns-overview.md).

## <a name="alias-records"></a>Diğer ad kayıtları

Azure DNS, diğer adı kayıt kümelerini destekler. Azure Genel IP adresi veya Traffic Manager profili gibi bir Azure kaynağına başvurmak için diğer ad kayıt kümesini kullanabilirsiniz. Temel alınan kaynağın IP adresi değişirse, diğer ad kayıt kümesi DNS çözümlemesi sırasında kendisini sorunsuz bir şekilde güncelleştirir. Diğer ad kaydı kümesi, hizmet örneğini işaret eder ve hizmet örneği bir IP adresi ile ilişkilidir. 

Ayrıca, apex veya çıplak etki alanınızı (örneğin, contoso.com) diğer ad kaydı kullanarak bir Traffic Manager profiline yönlendirebilirsiniz.

Daha fazla bilgi için bkz. [Azure DNS diğer ad kayıtlarına genel bakış](dns-alias.md).


## <a name="next-steps"></a>Sonraki adımlar

* DNS bölgeleri ve kayıtları hakkında bilgi edinin: [DNS bölgeleri ve kayıtlarına genel bakış](dns-zones-records.md).

* Azure DNS'de bölge oluşturmayı öğrenin: [DNS bölgesi oluşturma](./dns-getstarted-create-dnszone-portal.md).

* Azure DNS hakkında sık sorulan sorular için bkz. [Azure DNS Hakkında SSS](dns-faq.md).

