---
title: Azure'da giden SMTP bağlantı sorunlarını giderme | Microsoft Docs
description: Azure'da giden SMTP bağlantı sorunlarını giderme hakkında bilgi edinin.
services: virtual-network
author: genlin
manager: cshepard
editor: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/20/2018
ms.author: genli
ms.openlocfilehash: 13ed2dc2b304368e468c433b5abf5d056c33e406
ms.sourcegitcommit: c63e5031aed4992d5adf45639addcef07c166224
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67466474"
---
# <a name="troubleshoot-outbound-smtp-connectivity-issues-in-azure"></a>Azure'da giden SMTP bağlantı sorunlarını giderme

15 Kasım 2017 tarihinden itibaren bir sanal makineden (VM) doğrudan dış etki alanları (örneğin, outlook.com ve gmail.com) için gönderilen giden e-posta iletilerini yalnızca Microsoft azure'da bazı abonelik türleri için kullanılabilir hale getirilir. 25 numaralı TCP bağlantı noktasını kullanmak giden SMTP bağlantıları engellendi. (Bağlantı noktası 25 öncelikle kimliği doğrulanmamış bir e-posta teslimi için kullanılır.)

Bu davranış değişikliği yalnızca yeni abonelikler ve yeni dağıtımlar için 15 Kasım 2017'den itibaren geçerlidir.

## <a name="recommended-method-of-sending-email"></a>Önerilen yöntem, e-posta gönderme
(Bu genellikle 587 veya 443 numaralı TCP bağlantı bağlanma ancak diğer bağlantı noktaları, çok destekler) kimliği doğrulanmış SMTP geçiş hizmetlerini Azure vm'lerinden veya Azure uygulama hizmetleri e-posta göndermesine izin kullanmanızı öneririz. Bu hizmetleri, üçüncü taraf e-posta sağlayıcıları ileti reddeder olasılığını en aza indirmek için IP veya etki alanı saygınlığı korumak için kullanılır. Bu tür SMTP geçiş hizmetleri dahil ancak bunlarla sınırlı olmayan [SendGrid](https://sendgrid.com/partners/azure/). Çalışan kullanabileceğiniz Şirket güvenli bir SMTP geçiş hizmetini olması mümkündür.

Bu e-posta teslimat hizmetlerini kullanarak, Azure'da abonelik türü ne olursa olsun sınırlı değildir.

## <a name="enterprise-agreement"></a>Kurumsal Anlaşma
Kurumsal Anlaşma Azure kullanıcıları için kimliği doğrulanmış geçişi kullanmadan e-posta gönderme olanağı teknik bir değişiklik yoktur. Hem yeni hem de mevcut Kurumsal Anlaşma kullanıcıların Azure platformundan hiçbir kısıtlama olmadan dış e-posta sağlayıcıları için doğrudan Azure vm'lerinden giden e-posta teslimi deneyebilirsiniz. Bu e-posta sağlayıcılarının belirli bir kullanıcıdan gelen e-postayı kabul edeceğini garanti edilmez olsa da, teslim denemesi Azure platformu tarafından Kurumsal Anlaşma abonelikleri VM'ler için engellenmez. Tüm ileti teslimi veya istenmeyen özel sağlayıcıları içeren filtreleme sorunlarını doğrudan e-posta sağlayıcıları ile çalışmak zorunda kalırsınız.

## <a name="pay-as-you-go"></a>Kullandıkça Öde
Kullandıkça Öde için 15 Kasım 2017'den önce kaydolan veya Microsoft iş ortağı ağı abonelik sunar, olacaktır giden e-posta teslimi deneyin olanağı teknik değişiklik olur. Giden e-posta teslimi Azure vm'lerinden içinde bu abonelikler Azure platformundan hiçbir kısıtlama olmadan dış e-posta sağlayıcıları için doğrudan denemeniz eklemeye devam edeceğiz. Yeniden, bu e-posta sağlayıcılarının belirli bir kullanıcıdan gelen e-postayı kabul edeceğini ve tüm ileti teslimi veya istenmeyen özel sağlayıcıları içeren filtreleme sorunlarını doğrudan e-posta sağlayıcıları ile çalışmaya kullanıcıları olacaktır garanti edilmez.

15 Kasım 2017'den sonra oluşturulan Kullandıkça Öde veya Microsoft iş ortağı ağı abonelikleri için bu aboneliklerin içinde doğrudan vm'lerden gönderilen e-posta engelleyen teknik kısıtlamalar olacaktır. Özelliği Azure sanal makinelerinden (kimliği doğrulanmış SMTP geçişi kullanmadan) doğrudan dış e-posta sağlayıcılarına e-posta göndermek istiyorsanız, kısıtlamanın kaldırılması için istekte bulunabilirsiniz. İstekleri gözden geçirilebilir ve Microsoft'un kararımıza Onaylandı ve yalnızca dolandırıcılık önleme denetimleri yapıldıktan sonra onlara verilen. İstekte bulunmak için aşağıdaki sorun türünü kullanarak bir destek talebi açın: **Abonelik Yönetimi** sorun türü: **Bağlantı noktası 25 e-posta akışı etkinleştirmek üzere isteği**. Neden kimliği doğrulanmış geçişi kullanmak yerine doğrudan posta sağlayıcılarına e-posta göndermek dağıtımınızın sahip hakkında ayrıntıları eklediğinizden emin olun.

Bir Kullandıkça Öde veya Microsoft iş ortağı ağı aboneliğiniz muaf tutulan ve Vm'leri 'Durduruldu' & 'Azure portalından başlatılan' değerinin sonra bu Abonelikteki tüm sanal makineler ileriye dönük muaf. Muafiyet yalnızca istenen abonelik için geçerlidir.

> [!NOTE]
> Microsoft hizmet koşullarını ihlal oluştuğunu belirlenirse bu muafiyet iptal etme hakkını saklı tutar.

## <a name="msdn-azure-pass-azure-in-open-education-bizspark-and-free-trial"></a>MSDN, Azure Pass, Open ile Azure, Education, BizSpark ve ücretsiz deneme
MSDN, Azure Pass, Azure açık, Education, BizSpark, Azure Sponsorluğu, Azure Öğrenci, ücretsiz deneme sürümü veya herhangi bir Visual Studio aboneliğinizi 15 Kasım 2017'ye sonra oluşturduğunuz yapıyorsanız içinde Vm'lerden gönderilen blok e-posta teknik kısıtlamalara sahip olursunuz doğrudan sağlayıcıları e-posta abonelikleri. Kısıtlamaları, kötüye kullanımı önlemek için yapılır. Bu kısıtlamanın kaldırılması isteklerine izin verilir.

Bu abonelik türleri kullanıyorsanız, SMTP geçiş hizmetleri, bu makalenin önceki bölümlerinde açıklandığı veya abonelik türü değiştirme için önerilir.

## <a name="cloud-service-provider-csp"></a>Bulut hizmeti sağlayıcısı (CSP)

CSP'de Azure kaynaklarında kullanıyorsanız, güvenli bir SMTP geçiş kullandıysanız bir engellemeyi kaldırma muafiyeti isteği sizin adınıza Microsoft ile oluşturmak için CSP isteyebilir.

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun

Hala yardıma ihtiyacınız varsa [desteğe](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorununuzun aşağıdaki sorun türü kullanarak hızlıca çözülebilmesi için: **Abonelik Yönetimi** sorun türü: **Bağlantı noktası 25 e-posta akışı etkinleştirmek üzere isteği**.
