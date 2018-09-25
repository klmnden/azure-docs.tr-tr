---
title: Azure güvenlik duvarı FQDN etiketleri genel bakış
description: Azure Güvenlik Duvarı'nda FQDN etiketleri hakkında bilgi edinin
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 9/24/2018
ms.author: victorh
ms.openlocfilehash: 536c0915cae17aa6f4201c62eae5f5b077805274
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46999486"
---
# <a name="fqdn-tags-overview"></a>FQDN etiketleri genel bakış

FQDN etiketi tam etki alanı adlarını (FQDN) bilinen Microsoft hizmetleriyle ilişkili bir grubu temsil eder. Gerekli giden ağ trafiği, güvenlik duvarı üzerinden izin vermek için uygulama kuralları bir FQDN etiketi kullanabilirsiniz.

>[!NOTE]
>FQDN etiketler özelliği yalnızca Azure PowerShell ve REST şu anda kullanılabilir.

Örneğin, el ile Windows Update ağ trafiği, güvenlik duvarı üzerinden izin vermek için Microsoft belgelerine başına birden çok uygulama kuralı oluşturmanız gerekir. FQDN etiketleri kullanarak, bir uygulama kuralı oluşturun, dahil **Windows güncelleştirmeleri** etiketi ve artık ağ trafiği uç noktaları, güvenlik duvarı aracılığıyla akış Microsoft Windows Update.

Kendi FQDN etiket oluşturamaz veya FQDN'ler bir etikete dahil olan belirtebilirsiniz. Microsoft FQDN etiketi tarafından çevrelenmiş FQDN'leri yönetir ve FQDN'ler değişiklik olarak güncelleştirilir. 

<!--- screenshot of application rule with a FQDN tag.-->

Aşağıdaki tabloda kullanabileceğiniz geçerli FQDN etiket gösterilmektedir. Microsoft bu etiketleri korur ve düzenli aralıklarla eklenecek ek etiketler bekleyebilirsiniz.

|FQDN etiketi  |Açıklama  |
|---------|---------|
|Windows Update     |Bölümünde anlatıldığı gibi Microsoft Update giden erişime izin vermek [yazılım güncelleştirmeleri için güvenlik duvarı yapılandırma](https://technet.microsoft.com/library/bb693717.aspx).|
|Windows Tanılama Özellikleri|Tüm giden erişime izin vermek [Windows Tanılama uç noktaları](https://docs.microsoft.com/windows/privacy/configure-windows-diagnostic-data-in-your-organization#endpoints).|
|Microsoft Etkin Koruma Hizmeti (MAPS)|Giden erişime izin ver [HARİTALAR](https://cloudblogs.microsoft.com/enterprisemobility/2016/05/31/important-changes-to-microsoft-active-protection-service-maps-endpoint/).|
|App Service ortamı (ASE)|ASE platform trafiği giden erişim sağlar. Bu etiket, müşteriye özgü depolama ve SQL uç noktaları ASE tarafından oluşturulan ele alınmamıştır. Bunlar üzerinden etkinleştirilmelidir [hizmet uç noktaları](../virtual-network/tutorial-restrict-network-access-to-resources.md) veya el ile eklenmiş.|
|Azure Backup|Azure Backup hizmetlerine giden erişim sağlar.

## <a name="next-steps"></a>Sonraki adımlar

Bir Azure güvenlik duvarı dağıtma konusunda bilgi edinmek için [öğretici: Azure Azure portalını kullanarak güvenlik duvarı yapılandırma ve dağıtma](tutorial-firewall-deploy-portal.md).