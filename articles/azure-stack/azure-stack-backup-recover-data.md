---
title: Azure Stack altyapısını Yedekleme hizmetini kullanarak, geri dönülemez veri kaybı kurtarma | Microsoft Docs
description: Azure Stack, başarısız olmasına kullanabilirsiniz geri dönülemez bir hataya sebep olduğunda Azure Stack dağıtımınıza kurulmadan olduğunda altyapı verilerinizi geri yükleme.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: 2ECE8580-0BDE-4D4A-9120-1F6771F2E815
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2018
ms.author: jeffgilb
ms.reviewer: hectorl
ms.openlocfilehash: ed89468d65f35d0a8cda90da365e8625f46e3f92
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2018
ms.locfileid: "42056542"
---
# <a name="recover-from-catastrophic-data-loss"></a>Geri dönülemez veri kaybından kurtarma

*İçin geçerlidir: Azure Stack tümleşik sistemleri.*

Azure Stack, veri merkezinizde Azure hizmetleri çalıştırır. Azure Stack, tek bir rafa yüklü dört düğüm kadar küçük ortamlarda çalıştırabilirsiniz. Buna karşılık, Azure birden çok veri merkezinde ve her bölgede birden fazla bölge 40'tan fazla bölgede çalışır. Kullanıcı kaynaklarını birden çok sunucuları, raflar, veri merkezleri ve bölgeleri yayılabilir. Azure Stack ile şu anda yalnızca tüm bulut için tek bir rafa dağıtmayı seçebilirsiniz. Bu bulut için veri merkezi veya önemli ürün hataları nedeniyle hataları yıkıcı olaylara riskini ortaya çıkarır. Bir olağanüstü durumla karşılaştığınızda, Azure Stack örneği çevrimdışı olur. Tüm veriler büyük olasılıkla kurtarılamaz.

Veri kaybı kök nedenine bağlı olarak, hizmet bir tek altyapı onarmak veya tüm Azure Stack örneğini geri yüklemek gerekebilir. Hatta farklı donanım aynı konumda veya farklı bir konuma geri yüklemek gerekebilir.

Bu senaryo, bir arıza olması durumunda, tüm yükleme ekipmanları ve özel bulutun yeniden dağıtma işlemi kurtarma giderir.

| Senaryo                                                           | Veri kaybı                            | Dikkat edilmesi gerekenler                                                             |
|--------------------------------------------------------------------|--------------------------------------|----------------------------------------------------------------------------|
| Olağanüstü durum ya da ürün hatası nedeniyle geri dönülemez veri kaybından kurtarma | Tüm altyapı ve kullanıcı ve uygulama verileri | Kullanıcı uygulama ve veri altyapısı verilerinden ayrı olarak korunur |

## <a name="workflows"></a>İş akışları

Azure başlangıç korumanın yolculuğu, altyapı ve uygulama/Kiracı verilerini ayrı olarak yedekleme ile başlar. Bu belge altyapısını koruma nasıl etkinleştireceğinizi de açıklar. 

![İlk Azure Stack dağıtımı](media\azure-stack-backup\azure-stack-backup-workflow1.png)

Tüm veriler kaybolur burada kötü örneği senaryoları Azure Stack kurtarma, Azure Stack ve tüm kullanıcı verilerini o dağıtımı için altyapı verileri benzersiz geri işlemidir. 

![Azure Stack'i yeniden dağıtma](media\azure-stack-backup\azure-stack-backup-workflow2.png)

## <a name="restore"></a>Geri Yükleme

Azure Stack bilgisayarını yeniden dağıtma işlemi, geri dönülemez veri kaybı, ancak donanım hala kullanılabilir, gereklidir. Dağıtım sırasında yedeklemelere erişmek için gerekli kimlik bilgilerini ve depolama konumunu belirtebilirsiniz. Bu modda, geri yüklenmesi gereken hizmetleri belirtmek için gerek yoktur. Altyapı yedekleme denetleyicisi denetim düzlemi durum dağıtım iş akışının bir parçası olarak ekler.

Yeniden dağıtma işlemi, yalnızca donanım kullanılamaz işleyen bir olağanüstü durum varsa, yeni donanıma mümkündür. Değiştirme donanımının sıralanır ve veri merkezinde ulaşan yeniden dağıtma işlemi birkaç hafta sürebilir. Denetim düzlemi verilerini geri yükleme, herhangi bir zamanda mümkündür. Ancak, bilgisayarına örneğinin sürümü, birden fazla sürümü son yedeklemeden kullanılan sürümden daha büyük ise geri yükleme desteklenmiyor. 

| Dağıtım modu | Başlangıç noktası | Uç noktası                                                                                                                                                                                                     |
|-----------------|----------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Temiz yükleme   | Ana hat derlemesi | OEM, Azure Stack dağıtır ve desteklenen en son sürüme güncelleştirir.                                                                                                                                          |
| Kurtarma modu   | Ana hat derlemesi | OEM, Azure Stack kurtarma modunda dağıtır ve kullanılabilir en son yedeği temel gereksinimleri eşleşen sürüm işler. OEM dağıtımı için desteklenen son sürümü güncelleştirerek tamamlar. |

## <a name="data-in-backups"></a>Yedekleme verileri

Azure Stack bulut kurtarma moduna adlı dağıtım türünü destekler. Bu mod yalnızca Azure Stack sonra bir olağanüstü durum kurtarmayı seçerseniz veya ürün hata çözüm kurtarılamaz işlenen kullanılır. Bu dağıtım modu çözümünde depolanan kullanıcı verilerini kurtarılmıyor. Bu dağıtım modu kapsamını, aşağıdaki geri yüklenmesi için sınırlıdır:

 - Dağıtım giriş
 - İç kimlik sistemleri
 - Federasyon yapılandırması (bağlantısı kesilmiş dağıtımlar) tanımlayın
 - İç sertifika yetkilisi tarafından kullanılan kök sertifikalar
 - Azure Resource Manager kullanıcı gibi yapılandırma verilerini abonelikler, planlar, teklifler ve depolama kotalarını, ağ ve işlem kaynakları
 - KeyVault gizli dizileri ve Kasaları
 - RBAC ilke atamaları ve rol atamaları 

Dağıtım sırasında bir hizmet (PaaS) kaynak olarak altyapı (Iaas) veya platformu olarak kullanıcı hiçbiri kurtarılabilir. Diğer bir deyişle Iaas Vm'leri, depolama hesapları, bloblar, tablolar, ağ yapılandırması ve benzeri kaybolur. Bulut kurtarma amacı, işleçler sağlamaktır ve dağıtım tamamlandıktan sonra kullanıcılar portalına geri dönüp oturum açabilir. Kullanıcı yeniden oturum kaynaklarını görmezsiniz. Kullanıcılar geri aboneliklerini ve yanı sıra, özgün planları ve yönetici tarafından tanımlanan ilkeleri sunar. Sisteme oturum açan kullanıcılar özgün çözüm olağanüstü durumdan önce tarafından belirlenen aynı kısıtlamalara altında çalışır. Bulut Kurtarma tamamlandıktan sonra işleci el ile geri değerini ekleyin ve üçüncü taraf RPs ve ilişkili veriler.

## <a name="next-steps"></a>Sonraki adımlar

 - İçin en iyi uygulamalar hakkında bilgi edinin [altyapısını Yedekleme hizmetini kullanarak](azure-stack-backup-best-practices.md).
