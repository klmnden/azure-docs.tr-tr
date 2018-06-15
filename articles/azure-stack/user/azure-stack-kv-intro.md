---
title: Azure anahtar kasası yığın giriş | Microsoft Docs
description: Anahtarları ve gizli anahtarları Azure yığın anahtar kasası nasıl yönettiğini öğrenin
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 70f1684a-3fbb-4cd1-bf29-9f9882e98fe9
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/04/2017
ms.author: mabrigg
ms.openlocfilehash: a50a03e70ccf014a8a9d33e0f177febed560853f
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
ms.locfileid: "26636735"
---
# <a name="introduction-to-key-vault-in-azure-stack"></a>Anahtar kasası Azure yığınında giriş

## <a name="prerequisites"></a>Ön koşullar 

* Azure anahtar kasası hizmetindeki içeren bir teklif abone olmalısınız.  
* [PowerShell Azure yığını ile kullanılmak üzere yapılandırılmış](azure-stack-powershell-configure-user.md).
 
## <a name="key-vault-basics"></a>Anahtar kasası temelleri
Anahtar kasası Azure yığınında şifreleme anahtarları korumaya yardımcı olur ve uygulamaları ve Hizmetleri bulut gizlilikleri kullanın. Anahtar Kasası'nı kullanarak anahtarları ve gizli anahtarları, gibi şifreleme:
   * Kimlik doğrulama anahtarları 
   * Depolama hesabı anahtarları
   * Veri şifreleme anahtarları
   * .pfx dosyaları
   * Parolalar

Anahtar Kasası anahtar yönetimi işlemini kolaylaştırır ve verilerinize erişen ve bunları şifreleyen anahtarları denetiminizde tutmanıza olanak sağlar. Geliştiriciler, geliştirme ve test için dakikalar içinde anahtar oluşturabilir ve ardından bunları üretim anahtarlarına sorunsuz bir şekilde geçirebilir. Güvenlik yöneticileri gerektiğinde anahtarlara izin verebilir (ve iptal edebilir).

Bir Azure yığın abonelik herkesle oluşturabilir ve anahtar kasalarını kullanabilirsiniz. Anahtar kasası geliştiricilere ve güvenlik yöneticilerine avantaj sağlasa da, bir kuruluş için diğer Azure yığın Hizmetleri yöneten işleci uygulamak ve yönetmek. Örneğin, Azure işleci bir Azure yığın abonelikle oturum açmak yığın anahtarları depolamak ve bu işlemsel görevlerden sorumlu kuruluş için bir kasa oluşturun:

* Oluşturun veya bir anahtar veya gizli içeri aktarın.
* İptal etmek veya bir anahtar veya gizli silin.
* Kullanıcılar veya uygulamalar bunlar ardından yönetebilir veya ve gizli anahtarları kullanmak için anahtar kasasına erişim yetkisi verir.
* Anahtar kullanımı yapılandırın (örneğin, imzalama veya şifreleme).

İşleç Tekdüzen Kaynak Tanımlayıcıları (URI'ler) uygulamalarından çağırmaları için geliştiricilere sonra sağlayabilirsiniz. İşleçler, ayrıca güvenlik yöneticileri anahtar kullanımı günlüğü bilgilerini sağlayabilir.

Geliştiriciler ayrıca anahtarları doğrudan API'lerini kullanarak yönetebilir. Daha fazla bilgi için anahtar kasası Geliştirici Kılavuzu'na bakın.

## <a name="scenarios"></a>Senaryolar
Anahtar kasası geliştiricilere ve güvenlik yöneticilerinin ihtiyaçlarını karşılamak nasıl yardımcı olabileceğini aşağıdaki senaryolar açıklanmaktadır.

### <a name="developer-for-an-azure-stack-application"></a>Bir Azure yığın uygulama geliştiricisi
**Sorun:** için imzalama ve şifreleme için anahtarları kullanan Azure yığın uygulama yazmak istiyorum. Çözümün coğrafi olarak dağıtılmış bir uygulama için uygun olmasını sağlamak my uygulamasından harici olarak bu anahtarları istiyorum.

**Deyimi:** anahtarlar bir kasada depolanır ve gerektiğinde URI tarafından çağrılır.

### <a name="developer-for-software-as-a-service-saas"></a>Yazılım geliştirici olarak hizmet (SaaS)
**Sorun:** my Müşteri'nin anahtarları ve gizli anahtarları için sorumluluk veya olası yükümlülük istemiyorum. En iyi olan çekirdek yazılım özelliklerini sağlamaya ne yapabilirim yapmayı yoğunlaşmak amacıyla, müşterilerin kendi ve bunların anahtarlarını yönetmek istiyorsunuz.

**Deyimi:** müşteriler kendi anahtarları Azure yığına almak ve bunları yönetebilir. 

### <a name="chief-security-officer-cso"></a>Baş güvenlik Başkanı (CSO)
**Sorun:** kuruluşumun anahtar yaşam döngüsü denetiminde ve anahtar kullanımını izleyebildiğinden emin olmak istiyorum.

**Deyimi:** anahtar kasası, böylece Microsoft bakın veya anahtarlarınızı ayıklamak tasarlanmıştır. Bir uygulama müşterilerin anahtarlarını kullanarak şifreleme işlemleri gerçekleştirmesi gerektiğinde, anahtar kasası uygulama adına anahtarları kullanır. Uygulama, müşterilerin anahtarlarını görmez. Birden çok Azure yığın Hizmetleri ve kaynakları kullanırız rağmen Azure yığınında tek bir konumdan anahtarları yönetebilir. Kasa, destek ve hangi uygulamaların bunları kullandığına kaç kasanız sahip olduğunuz Azure yığını, hangi bölgelerin bunlar bağımsız olarak tek bir arabirim sağlar.

## <a name="next-steps"></a>Sonraki adımlar

* [Anahtar kasası Azure yığınında Portalı'nı kullanarak yönetme](azure-stack-kv-manage-portal.md)  
* [Anahtar kasası Azure yığınında PowerShell kullanarak yönetme](azure-stack-kv-manage-powershell.md)

