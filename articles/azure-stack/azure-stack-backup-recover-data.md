---
title: "Azure altyapı Yedekleme hizmetini kullanarak yığınında geri dönülemez veri kaybı kurtarmada | Microsoft Docs"
description: "Azure başarısız olmasına, can yığını geri dönülemez bir hataya sebep olduğunda, altyapı verilerinizi Azure yığın dağıtımınızı yeniden kurulmadan zaman geri yüklemek."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 2ECE8580-0BDE-4D4A-9120-1F6771F2E815
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/13/2017
ms.author: mabrigg
ms.openlocfilehash: 141641b01b338e3426861dad7424a1de1bd2c63c
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2018
---
# <a name="recover-from-catastrophic-data-loss"></a>Geri dönülemez veri kaybına karşı Kurtar

*Uygulandığı öğe: Azure yığın tümleşik sistemler.*

Azure yığını, Azure Hizmetleri, veri merkezinizde çalıştırır. Azure yığın tek bir rafa yüklü dört düğüm olabildiğince küçük ortamlarda çalıştırabilirsiniz. Buna karşılık, birden çok veri merkezi ve her bölgede birden fazla bölge 40'tan fazla bölgelerdeki Azure çalışır. Kullanıcı kaynakları birden çok sunucuları, bölmeler, veri merkezleri ve bölgeler yayılabilir. Azure yığın ile şu anda yalnızca tek bir rafa tüm bulut dağıtmak için seçeneğiniz. Bu bulut için veri merkezi veya önemli ürün hatalar nedeniyle hataları yıkıcı olayları riskini ortaya çıkarır. Bir olağanüstü durum sağlar, Azure yığın örneği çevrimdışı olur. Tüm verileri olası kurtarılamaz.

Veri kaybı kök nedenini bağlı olarak, hizmet tek altyapı onarmak veya tüm Azure yığın örneğini geri yüklemek gerekebilir. Hatta farklı donanım aynı konuma veya farklı bir konuma geri yüklemek gerekebilir.

Bu senaryo bir arıza olması durumunda tüm yüklemenizi ekipman ve özel bulutun yeniden dağıtım kurtarma giderir.

| Senaryo                                                           | Veri kaybı                            | Dikkat edilmesi gerekenler                                                             |
|--------------------------------------------------------------------|--------------------------------------|----------------------------------------------------------------------------|
| Olağanüstü durum ya da ürün hata nedeniyle geri dönülemez veri kaybı kurtarın | Tüm altyapı ve kullanıcı ve uygulama verileri | Kullanıcı uygulama ve veri altyapı verilerinden ayrı olarak korunur |

## <a name="workflows"></a>İş akışları

Azure Başlat korumanın gezisine altyapı ve uygulama/Kiracı verileri ayrı olarak yedekleme ile başlar. Bu belge, altyapısını koruma alınmaktadır. 

![Azure yığınının ilk dağıtım](media\azure-stack-backup\azure-stack-backup-workflow1.png)

Verilerin tümü olduğu kaybolur kötü örnek senaryolarda, Azure yığın kurtarma, benzersiz altyapı verileri bu dağıtımı Azure yığını ve tüm kullanıcı verilerini geri yükleme işlemidir. 

![Azure yığını yeniden dağıtın](media\azure-stack-backup\azure-stack-backup-workflow2.png)

## <a name="restore"></a>Geri Yükleme

Yeniden dağıtım Azure yığınının geri dönülemez veri kaybı yoktur, ancak donanım hala kullanılabilir gereklidir. Yeniden dağıtım sırasında yedeklemelere erişmek için gerekli kimlik bilgilerini ve depolama konumu belirtebilirsiniz. Bu modda, geri yüklenmesi gereken hizmetleri belirtmek için gerek yoktur. Altyapı yedekleme denetleyicisi dağıtım iş akışının parçası olarak denetim düzlemi durumu yerleştirir.

Yeniden dağıtım, yalnızca donanım kullanılamaz işleyen bir olağanüstü durum varsa, yeni donanıma mümkündür. Değiştirme donanım sıralanır ve veri merkezine ulaşan yeniden dağıtım birkaç hafta sürebilir. Denetim düzlemi verilerini geri yükleme herhangi bir zamanda mümkündür. Ancak, yeniden dağıtılan örneğinin sürümü, birden fazla sürümü son yedekleme kullanılan sürümden daha büyük ise geri yükleme desteklenmez. 

| Dağıtım modu | Başlangıç noktası | Uç noktası                                                                                                                                                                                                     |
|-----------------|----------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Temiz yükleme   | Taban çizgisi oluşturma | OEM Azure yığın dağıtır ve en son desteklenen sürüm olarak güncelleştirir.                                                                                                                                          |
| Kurtarma moduna   | Taban çizgisi oluşturma | OEM kurtarma modunda Azure yığın dağıtır ve kullanılabilir en son yedeklemeyi göre gereksinim eşleşen sürüm işler. OEM, en son desteklenen bir sürüme güncelleştirerek dağıtım işlemini tamamlar. |

## <a name="data-in-backups"></a>Veri yedekleri

Azure yığını bulut kurtarma moduna adlı dağıtım türünü destekler. Bu modu yalnızca Azure yığın sonra bir olağanüstü durum kurtarmayı seçerseniz veya ürün hata çözümü kurtarılamaz çizilir kullanılır. Bu dağıtım modu çözümünde depolanan kullanıcı verilerini kurtarılmıyor. Bu dağıtım modu kapsamı, aşağıdaki verileri geri yüklemek üzere sınırlıdır:

 - Dağıtım girişleri
 - İç kimlik sistemleri
 - Federasyon yapılandırma (bağlantısı kesilmiş dağıtımlar) tanımlayın
 - İç sertifika yetkilisi tarafından kullanılan kök sertifikaları
 - Azure Kaynak Yöneticisi'ni yapılandırma kullanıcı verileri, abonelikler, planları, teklifleri ve depolama için kotalar gibi ağ ve işlem kaynaklarını
 - KeyVault gizli bilgiler ve Kasaları
 - RBAC ilke atamaları ve rol atamaları 

Bir hizmet (PaaS) kaynaklar olarak altyapı (Iaas) veya Platform olarak kullanıcı hiçbiri dağıtımı sırasında kurtarılır. Diğer bir deyişle Iaas Vm'leri, depolama hesapları, BLOB'lar, tablolar, ağ yapılandırması ve benzeri kaybolur. Bulut kurtarma amacı, işleçler sağlamaktır ve dağıtım tamamlandıktan sonra kullanıcılar geri portalında oturum açabilir. Günlük geri kullanıcılar kaynaklarını görmezsiniz. Geri aboneliği kullanıcınız ve birlikte, orijinal planları ve yönetici tarafından tanımlanan ilkeleri sunar. Olağanüstü durum önce özgün çözüm tarafından belirlenen aynı kısıtlamalara altında sistemine geri oturum açan kullanıcılar çalışır. Bulut Kurtarma tamamlandıktan sonra işleci el ile geri değerini ekleyin ve üçüncü taraf RPs ve ilişkili veriler.

## <a name="next-steps"></a>Sonraki adımlar

 - İçin en iyi uygulamalar hakkında bilgi edinin [altyapı Yedekleme hizmetini kullanarak](azure-stack-backup-best-practices.md).
