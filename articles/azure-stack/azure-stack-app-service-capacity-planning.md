---
title: "Kapasite Azure yığınında Azure App Service sunucu rolleri için planlama | Microsoft Docs"
description: "Kapasite Azure yığınında Azure App Service sunucu rolleri için planlama"
services: azure-stack
documentationcenter: 
author: apwestgarth
manager: stefsch
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2018
ms.author: anwestg
ms.openlocfilehash: 93e10235e3de4ecea4d0e356bb4b52922c8afac8
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="capacity-planning-for-azure-app-service-server-roles-in-azure-stack"></a>Kapasite Azure yığınında Azure App Service sunucu rolleri için planlama
*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Azure uygulama hizmeti Azure yığında üretim hazır dağıtımını sağlayacak desteklemek için sistem beklediğiniz kapasiteyi planlamanız gerekir.  Aşağıda, en az sayıda örnekleri ve işlem her üretim dağıtımı için kullanması gereken SKU'ları hakkında yönergeler verilmiştir.

Bu yönergeleri kullanarak, uygulama hizmeti kapasite stratejinizi planlayabilirsiniz. Gelecek sürümlerinde Azure yığın uygulama hizmeti için yüksek kullanılabilirlik seçeneklerini sağlar.

| App Service sunucu rolü | Minimum örnek sayısı önerilir | Önerilen işlem SKU|
| --- | --- | --- |
| Denetleyici | 2 | A1 |
| Ön Uç | 2 | A1 |
| Yönetim | 2 | A3 |
| Yayımcı | 2 | A1 |
| Web çalışanları - paylaşılan | 2 | A1 |
| Web çalışanları - ayrılmış | Katman başına 2 | A1 |

## <a name="controller-role"></a>Denetleyici rolü

**Önerilen minimum**: A1 standart iki örneği

Azure App Service denetleyicisi genellikle CPU, bellek ve ağ kaynaklarını düşük oranda tüketir. Ancak, yüksek kullanılabilirlik için iki denetleyicisi olmalıdır. İki denetleyicileri denetleyicileri izin verilen en fazla sayısını da etkilenir. Dağıtım sırasında ikinci Web siteleri denetleyicisi Yükleyiciden'doğrudan olarak oluşturabilirsiniz.

## <a name="front-end-role"></a>Ön uç rolü

**Önerilen minimum**: A1 standart iki örneği

Ön uç, istekleri Web çalışanı kullanılabilirliğine bağlı olarak Web çalışanlarına yönlendirir. Yüksek kullanılabilirlik için birden çok ön uç olması gerekir ve ikiden fazla olabilir. Kapasite planlama için her çekirdeğin saniyede yaklaşık 100 isteği işleyebileceğini düşünün.

## <a name="management-role"></a>Yönetim rolü

**Önerilen minimum**: A3 iki örneği

Azure uygulama hizmeti yönetim rolü, uygulama hizmeti Azure Resource Manager ve API uç noktaları, portal Uzantıları (yönetici, Kiracı, işlevleri portalına) ve veri hizmeti için sorumludur. Yönetim sunucusu rolü genellikle bir üretim ortamında 4 GB RAM yalnızca hakkında gerektirir. Ancak, çok sayıda yönetim görevi (örneğin, web sitesi oluşturma) gerçekleştirildiğinde yüksek CPU düzeyleri karşılaşabilirsiniz. Yüksek kullanılabilirlik için bu role atanmış birden fazla sunucunun olmalıdır ve en az iki sunucu başına çekirdek.

## <a name="publisher-role"></a>Yayımcı rolü

**Önerilen minimum**: A1 iki örneği

Çok sayıda kullanıcı aynı anda yayımlıyorsa, yayımcı rolü aşırı CPU kullanımı karşılaşabilirsiniz. Yüksek kullanılabilirlik için birden fazla yayımcı rolünü kullanılabilir duruma getirin.  Yayımcı yalnızca FTP/FTPS trafiğini işler.

## <a name="web-worker-role"></a>Web çalışanı rolü

**Önerilen minimum**: A1 iki örneği

Yüksek kullanılabilirlik için en az dört Web çalışanı rolünüz olmalıdır, ikisi paylaşılan web sitesi modu için ve her adanmış çalışan katmanı için iki sunmak planlama. Paylaşılan ve ayrılmış işlem modları kiracılar için farklı hizmet düzeyleri sağlar. Çok sayıda müşteriniz varsa daha fazla Web çalışanı gerekebilir:
 - (yoğun bir kaynak olan) adanmış bir işlem modu çalışan katmanlarını kullanma
 - Paylaşılan işlem modunda çalışıyor.

Bir kullanıcı bir uygulama hizmeti planı için SKU, Web App Service planı artık kullanıcılar tarafından kullanılabilir olmayacaktır, belirtilen Worker(s) sayısı adanmış bir işlem moduna oluşturduktan sonra.

Tüketim planı modelinde kullanıcılara Azure işlevleri sağlamak için paylaşılan Web çalışanı dağıtmanız gerekir.

Paylaşılan Web çalışanı rollerinin sayısını kullanmaya karar verdiğinizde, bu konuları gözden geçirin:

- **Bellek**: bir Web çalışanı rolü için en önemli kaynak bellektir. Yetersiz bellek, sanal bellek diskten değiştirildiğinde web sitesi performansını etkiler. Her sunucu işletim sistemi için yaklaşık 1,2 GB RAM gerektirir. Bu eşiğin üstünde RAM, web sitelerini çalıştırmak için kullanılabilir.
- **Etkin web sitelerinin yüzdesi**: Azure yığın dağıtımına bir Azure uygulama hizmeti uygulamaların yaklaşık yüzde 5'i genellikle etkindir. Ancak, belirli bir anda etkin olan uygulamalar yüzdesi daha yüksek veya düşük olabilir. Bir etkin uygulama oranı yüzde 5 ile en fazla sayıda Azure yığın dağıtımına bir Azure uygulama hizmeti yerleştirmek için uygulama olmalıdır küçüktür:
    - 20 kez etkin web sitesi sayısını (5 x 20 = 100).
- **Ortalama bellek kaplama alanı**: üretim ortamlarında gözlemlenen uygulamalar için ortalama bellek kaplama alanı yaklaşık 70 MB'dir. Bu nedenle, tüm Web çalışanı rolü bilgisayarları veya VM'ler ayrılan bellek aşağıdaki gibi hesaplanabilir:

    *Hazırlandı uygulama sayısı * 70 MB * 5-(Web çalışanı rollerinin sayısı * 1044 MB)*

   Örneğin, 10 Web çalışanı rolü çalıştıran ortamda 5.000 uygulamalar varsa, her Web çalışanı rolü VM 7060 MB RAM olmalıdır:

   5.000 * 70 * 0,05 – (10 * 1044) = 7060 (= yaklaşık 7 GB)

   Daha fazla çalışan örnekleri ekleme hakkında daha fazla bilgi için bkz: [daha fazla çalışan rolleri ekleme](azure-stack-app-service-add-worker-roles.md).

## <a name="file-server-role"></a>Dosya sunucusu rolü

Azure uygulama hizmeti Azure yığın Geliştirme Seti dağıtırken örneğin test Bu şablon - kullanabilirsiniz ve dosya sunucusu rolü için geliştirme için bir tek başına dosya sunucusu kullanabilirsiniz https://aka.ms/appsvconmasdkfstemplate. Üretim için önceden yapılandırılmış bir Windows dosya sunucusu veya önceden yapılandırılmış Windows olmayan dosya sunucusu kullanmanız gerekir.

Üretim ortamlarında dosya sunucusu rolü yoğun disk g/ç ile karşılaşır. Bunu tüm kullanıcı web siteleri için içerik ve uygulama dosyalarını barındırdığından bu rol için aşağıdakilerden birini önceden yapılandırmanız gerekir:
- bir Windows dosya sunucusu
- Dosya sunucusu kümesi
- Windows olmayan dosya sunucusu
- dosya sunucusu kümesi
- NAS (ağa bağlı depolama) aygıtı daha fazla bilgi için bkz: [bir dosya sunucusu sağlamak](azure-stack-app-service-before-you-get-started.md#prepare-the-file-server).

## <a name="next-steps"></a>Sonraki adımlar

[Azure yığın uygulama hizmeti ile çalışmaya başlamadan önce](azure-stack-app-service-before-you-get-started.md)
