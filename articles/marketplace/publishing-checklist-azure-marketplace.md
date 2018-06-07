---
title: Azure Marketi'nde yayımlama denetim listesi | Azure
description: Bulut iş ortağı portalını kullanarak Azure Marketi'nde yayımlama denetim listesi.
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
documentationcenter: ''
author: jm-aditi-ms
manager: pabutler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 06/05/2018
ms.author: ellacroi
ms.openlocfilehash: 5e40ccc5a11df00b63c99fb606e17fa17e23a2a0
ms.sourcegitcommit: 3017211a7d51efd6cd87e8210ee13d57585c7e3b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2018
ms.locfileid: "34825344"
---
# <a name="publishing-checklist-for-azure-marketplace"></a>Azure Marketi'nde yayımlama denetim listesi    
Yayımlama işlemi başlamadan önce gerekli bileşenleri gözden geçirin.  

Aşağıdaki yapılar oluşturma teklif yayımlama iş akışında bulut iş ortağı portalını tamamlamak için gereklidir.  

## <a name="checklist"></a>Denetim listesi  

| Liste türü | Teklif türü | Yayımlama yapı |   
|:--- |:--- |:--- |  
| Tümü | Tümü | <table> <tr><th>StoreFront ayrıntıları</th></tr> <tr><td>Teklif adı (200 karakter)</td></tr> <tr><td>Açıklama (2.000 karakter)</td></tr> <tr><td>MPN Kimliği</td></tr> <tr><td>Ülke/bölge kullanılabilirliği</td></tr> <tr><td>Geçerli sektörü, kategoriler ve arama anahtar sözcükleri</td></tr> <tr><td>Ekran görüntüleri (1280 x 720; en fazla 5)</td></tr> <tr><td>Pazarlama belgeleri (en fazla 3)</td></tr> <tr><td>Hedef yol</td></tr> <tr><td>Ürün genel bakış videosu (isteğe bağlı)</td></tr> </table> <table> <tr><th>Kişiler</th></tr> <tr><td>Bilgi (desteği, mühendislik, ticari) başvurun</td></tr> </table> <table> <tr><th>Teknik bilgiler</th></tr> <tr><td>Kullanım ve gizlilik ilkesi URL'si koşulları</td></tr> </table> <table> <tr><th>Test Drive</th></tr> <tr><td>Azure kaynak grubu adı</td></tr> </table> |  
| Tümü | Sanal Makine | <table> <tr><th>Teknik bilgiler</th></tr> <tr><td>Destek URL'si</td></tr> </table> |
| Liste | Hizmet danışmanlık | <table> <tr><th>StoreFront ayrıntıları</th></tr> <tr><td>Katılım süresi</td></tr> <tr><td>Şirket logoları (48 x 48, 216 x 216)</td></tr> </table> |  
| Deneme | Tümü | <table> <tr><th>Teknik bilgiler</th></tr> <tr><td>Deneme URL'si</td></tr> <tr><td>Desteklenen diller</td></tr> <tr><td>Uygulama sürüm numarası</td></tr> <tr><td>Uygulama yayın tarihi</td></tr> <tr><td>Destek URL'si</td></tr> </table> |  
| Deneme | Test sürüşü | <table> <tr><th>Test Drive</th></tr> <tr><td>Açıklama</td></tr> <tr><td>Süre</td></tr> <tr><td>Kullanıcı el ile</td></tr> <tr><td>Test sürücü video (maksimum 1)</td></tr> <tr><td>Sınama sürücü ülke/bölge kullanılabilirliği</td></tr> <tr><td>Azure abonelik kimliği</td></tr> <tr><td>Azure AD Kiracı kimliği</td></tr> <tr><td>Azure AD uygulama kimliği</td></tr> <tr><td>Azure AD uygulama anahtarı</td></tr> </table> |  
| İşlem | Sanal Makine | <table> <tr><th>Veriş</th></tr> <tr><td>Başlık (50 karakter)</td></tr> <tr><td>Özet (200 karakter)</td></tr> <tr><td>Uzun özeti (256 karakter)</td></tr> <tr><td>HTML tabanlı açıklaması (3000 karakter)</td></tr> <tr><td>Şirket logoları (40 x 40, 90 x 90, 115 x 115, 255 x 115, 815 x 290)</td></tr> </table> <table> <tr><th>SKU</th></tr> <tr><td>İşletim sistemi ayrıntıları</td></tr> <tr><td>Kullanımdaki bağlantı noktaları</td></tr> <tr><td>Kullanımdaki protokolleri</td></tr> <tr><td>Kullanımdaki her VHD için disk sürümü</td></tr> <tr><td>Kullanımdaki her VHD için SAS URL'si</td></tr> </table> |  
| İşlem | Azure uygulamaları: Çözüm şablonu | <table> <tr><th>Veriş</th></tr> <tr><td>Başlık (50 karakter)</td></tr> <tr><td>Özet (200 karakter)</td></tr> <tr><td>Uzun özeti (256 karakter)</td></tr> <tr><td>HTML tabanlı açıklaması (3000 karakter)</td></tr> <tr><td>Şirket logoları (40 x 40, 90 x 90, 115 x 115, 255 x 115, 815 x 290)</td></tr> </table> <table> <tr><th>SKU</th></tr> <tr><td>Sürüm numarası</td></tr> <tr><td>İçeren paket dosyası<ul> <li>tüm şablon dosyaları</li> <li>createUIDefinition dosyası</li> </ul> </td></tr> </table> |  
| İşlem | Azure uygulamaları: yönetilen bir uygulama | <table> <tr><th>Veriş</th></tr> <tr><td>Başlık (50 karakter)</td></tr> <tr><td>Özet (200 karakter)</td></tr> <tr><td>Uzun özeti (256 karakter)</td></tr> <tr><td>HTML tabanlı açıklaması (3000 karakter)</td></tr> <tr><td>Şirket logoları (40 x 40, 90 x 90, 115 x 115, 255 x 115, 815 x 290)</td></tr> </table> <table> <tr><th>SKU</th></tr> <tr><td>Sürüm numarası</td></tr> <tr><td>İçeren paket dosyası<ul> <li>tüm şablon dosyaları</li> <li>createUIDefinition dosyası</li> </ul> </td></tr> </table> |  
| İşlem | Kapsayıcı | <table> <tr><th>Veriş</th></tr> <tr><td>Başlık (50 karakter)</td></tr> <tr><td>Özet (200 karakter)</td></tr> <tr><td>Uzun özeti (256 karakter)</td></tr> <tr><td>HTML tabanlı açıklaması (3000 karakter)</td></tr> <tr><td>Şirket logoları (40 x 40, 90 x 90, 115 x 115, 255 x 115, 815 x 290)</td></tr> </table> <table> <tr><th>SKU</th></tr> <tr><td>Azure kapsayıcı kayıt defteri (ACR) görüntü deposu ayrıntıları: abonelik kimliği</td></tr> <tr><td>ACR görüntü deposu ayrıntıları: kaynak grubu adı</td></tr> <tr><td>ACR görüntü deposu ayrıntıları: kayıt defteri adı</td></tr> <tr><td>ACR görüntü deposu ayrıntıları: Depo adı</td></tr> <tr><td>ACR görüntü deposu ayrıntıları: kullanıcı adı</td></tr> <tr><td>ACR görüntü deposu ayrıntıları: parola</td></tr> <tr><td>ACR görüntü deposu ayrıntıları: görüntü etiketler (isteğe bağlı)</td></tr> </table> |  
| İşlem | SaaS uygulama | <table> <tr><th>Veriş</th></tr> <tr><td>Başlık (50 karakter)</td></tr> <tr><td>Özet (200 karakter)</td></tr> <tr><td>Uzun özeti (256 karakter)</td></tr> <tr><td>HTML tabanlı açıklaması (3000 karakter)</td></tr> <tr><td>Şirket logoları (40 x 40, 90 x 90, 115 x 115, 255 x 115, 815 x 290)</td></tr> </table> |  

## <a name="next-steps"></a>Sonraki adımlar
*   Ziyaret [Azure Marketi ve AppSource yayımcı Kılavuzu](./marketplace-publishers-guide.md) sayfası.  
 
---  
