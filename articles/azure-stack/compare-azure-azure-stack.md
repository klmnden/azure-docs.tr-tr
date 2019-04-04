---
title: Azure yığını ve genel Azure karşılaştırın | Microsoft Docs
description: Microsoft Azure ve Azure Stack ailesi Hizmetleri'nde bir Azure ekosistemi nasıl sağladığını öğrenin
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 03/29/2019
ms.author: jeffgilb
ms.reviewer: unknown
ms.custom: ''
ms.lastreviewed: 03/29/2019
ms.openlocfilehash: 5e871d467fec476f1dac77fb879d180ea2322c80
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58650113"
---
# <a name="differences-between-global-azure-azure-stack-and-azure-stack-hci"></a>Küresel Azure, Azure Stack ve Azure Stack HCI arasındaki farklar

Microsoft Azure ve Azure Stack ailesi bir Azure ekosistemindeki hizmetleri sağlar. Aynı uygulama modelini, Self Servis portalı ve API'leri işletmenizin kullandığı genel Azure veya şirket içi kaynaklara bulut tabanlı özellikleri sunmak için Azure Resource Manager ile kullanın.

Bu makalede, Azure, Azure Stack ve Azure Stack HCI genel özelliklerini açıklar ve kuruluşunuz için Microsoft bulut tabanlı Hizmetleri sunmak için en iyi seçimi yapmanıza yardımcı olacak yaygın senaryo öneriler sağlar.

![Azure ekosistemi genel bakış](./media/compare-azure-azure-stack/azure-family.png)

## <a name="global-azure"></a>Küresel Azure

Microsoft Azure, kuruluşunuzun iş zorluklarını aşmasına yardımcı olacak şekilde tasarlanmış, sayısı her geçen gün artan bulut hizmetlerinden oluşur. En sevdiğiniz araçları ve çerçeveleri kullanarak çok büyük, küresel bir ağda uygulama oluşturma, yönetme ve dağıtma özgürlüğü sunar.

100'den fazla Hizmetleri kullanılabilir dünyanın farklı yerlerindeki 54 bölgelerde küresel Azure sunar. Genel Azure Hizmetleri en güncel listesi için bkz. [ *bölgelere göre kullanılabilir ürünler*](https://azure.microsoft.com/regions/services). Azure'da kullanılabilen hizmetler de kategoriye göre listelenen olup sunuldu oldukları gibi veya önizlemesi ile kullanılabilir.

Genel Azure hizmetleri hakkında daha fazla bilgi için bkz: [Azure ile çalışmaya başlama](https://docs.microsoft.com/azure/#pivot=get-started&panel=get-started1).

## <a name="azure-stack"></a>Azure Stack

Azure Stack, çevikliği getiren bir Azure uzantısı ve şirket içi ortamınıza bulut bilgi işlem yeniliğini ' dir. Şirket içinde dağıtılması, Azure Stack, internet bağlantısı olmayan ya da İnternet'e (ve Azure) veya bağlantısı kesilmiş ortamlarda bağlı Azure ile tutarlı hizmetler sağlamak için kullanılabilir. Azure Stack-olarak-hizmet altyapı (Iaas), hizmet olarak yazılım-a-(SaaS), temel bileşenleri içeren genel Azure aynı temel teknolojileri kullanır ve isteğe bağlı olarak bir-hizmet Platform (PaaS) özellikleri:

- Windows ve Linux için Azure sanal makineleri
- Azure Web Apps ve İşlevler
- Azure Key Vault
- Azure Resource Manager
- Azure Market
- Kapsayıcılar
- Azure IOT Hub ile Event Hubs
- Yönetim Araçları (planları, teklifleri, RBAC, vs.)

Azure Stack Microsoft tarafından işletilen değil çünkü Azure Stack PaaS özellikleri isteğe bağlıdır; müşterilerimiz tarafından çalıştırılır. Bu temel alınan altyapı ve işlemleri son kullanıcı uzağa soyutlamak için hazırsanız, son kullanıcılar için istediğiniz herhangi bir PaaS hizmetin sağlayabileceği anlamına gelir. Ancak, Azure Stack, App Service, SQL veritabanları ve MySQL veritabanları dahil olmak üzere çeşitli isteğe bağlı PaaS hizmeti sağlayıcıları içermez. Bu, çok kiracılı hazır, güncelleştirilmiş standart Azure Stack güncelleştirmelerini zamana, Azure Stack portalında görünen şekilde kaynak sağlayıcıları olarak teslim edilen ve Azure Stack ile tümleşik iyi.

Kaynak sağlayıcıları yukarıda açıklanan ek olarak kullanılabilir ve test edilmiş olarak ek PaaS Hizmetleri var. [Azure Resource Manager şablon tabanlı çözümler](https://github.com/Azure/AzureStack-QuickStart-Templates) devam ederken Iaas sağlama çalıştırın, ancak, bir Azure Stack operatörü can olarak olarak sunma PaaS Hizmetleri kullanıcılarınız için:

- Service Fabric
- Kubernetes kapsayıcı hizmeti
- IOT Hub ve Event Hub
- Etherium blok zinciri
- Cloud Foundry

### <a name="example-use-cases-for-azure-stack"></a>Azure Stack için örnek kullanım durumları:

- Finansal modelleme
- Klinik ve talep verileri
- IOT cihaz analizi
- Perakende Sınıflama en iyi duruma getirme
- Tedarik zinciri en iyi duruma getirme
- Endüstriyel IoT
- Tahmine dayalı bakım
- Akıllı Şehir
- Vatandaş etkileşimini

Azure Stack hakkında daha fazla bilgi [Azure Stack nedir](azure-stack-overview.md).

## <a name="azure-stack-hci"></a>Azure Stack HCI 

Azure Stack HCI çözümleri kolayca Azure'a bir hiper yakınsanmış altyapısı (HCI) çözümü ile bağlanma ve sanal makineleri şirket içi olanak sağlar. Yasal veya teknik gereksinimleri karşılamak için şirket içinde tutarlı Azure hizmetlerini kullanarak bulut uygulamaları oluşturun ve çalıştırın. Çalışan sanallaştırılmış şirket içi uygulamaları yanı sıra, Azure Stack HCI değiştirin ve eskime sunucu altyapınızı birleştirin ve bulut Hizmetleri için Windows Admin Center ' ı kullanarak Azure'a bağlanmak sağlar.

Azure Stack HCI Hyper-V ve depolama alanları doğrudan Windows Server 2019 yazılım tanımlı veri merkezi (SDDC) tarafından desteklenen doğrulanmış HCI çözümleri sağlar. Windows Admin Center yönetimi için kullanılır ve Azure hizmetlerine erişime izin gibi tümleşik:

- Azure Backup
- Azure Site Recovery
- Azure İzleyici ve güncelleştirme

Güncelleştirilmiş bir listesini Azure Hizmetleri için görmek için Azure Stack HCI bağlanabilir [bağlanan Windows Server için Azure karma Hizmetleri](https://docs.microsoft.com/windows-server/azure-hybrid-services/index).

### <a name="example-use-cases-for-azure-stack-hci"></a>Azure Stack HCI için kullanım örnekleri
- Uzak veya şube ofis sistemleri
- Veri Merkezi birleştirme
- Sanal Masaüstü Altyapısı
- İş açısından kritik altyapı
- Daha düşük maliyetli depolama
- Bulutta yüksek kullanılabilirlik ve olağanüstü durum kurtarma
- SQL Server gibi Kurumsal uygulamaları

Ziyaret [Azure Stack HCI Web sitesi](https://azure.microsoft.com/overview/azure-stack/hci/) 70'ten fazla Azure Stack HCI Microsoft iş ortaklarından şu anda kullanılabilir çözümleri görüntülemek için.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack yönetim temel bilgileri](azure-stack-manage-basics.md)

[Hızlı Başlangıç: Azure Stack yönetim portalını kullanın](azure-stack-manage-portals.md)
