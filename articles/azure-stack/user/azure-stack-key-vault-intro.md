---
title: Azure Stack Key Vault giriş | Microsoft Docs
description: Azure Stack Key Vault, anahtarları ve gizli anahtarları nasıl yönettiğini öğrenin
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 70f1684a-3fbb-4cd1-bf29-9f9882e98fe9
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/05/2019
ms.author: sethm
ms.lastreviewed: 01/05/2019
ms.openlocfilehash: 87c93f77011082d3e43b1c7d238999441f1b90c1
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2019
ms.locfileid: "57783015"
---
# <a name="introduction-to-key-vault-in-azure-stack"></a>Anahtar kasası Azure Stack'te giriş

## <a name="prerequisites"></a>Önkoşullar

* Azure Key Vault hizmetini içeren bir teklife abone olması gerekir.  
* [PowerShell Azure Stack ile kullanmak için yapılandırılmış](azure-stack-powershell-configure-user.md).

## <a name="key-vault-basics"></a>Key Vault temelleri

Azure stack'teki Key Vault, şifreleme anahtarlarını korumak yardımcı olur ve bulut uygulamaları ve Hizmetleri gizli dizileri kullanabilirsiniz. Anahtar Kasası'nı kullanarak, anahtarları ve gizli anahtarları, gibi şifreleme de yapabilirsiniz:

* Kimlik doğrulaması anahtarları
* Depolama hesabı anahtarları
* Veri şifreleme anahtarları
* .pfx dosyaları
* Parolalar

Anahtar Kasası anahtar yönetimi işlemini kolaylaştırır ve verilerinize erişen ve bunları şifreleyen anahtarları denetiminizde tutmanıza olanak sağlar. Geliştiriciler, geliştirme ve test için dakikalar içinde anahtar oluşturabilir ve ardından bunları üretim anahtarlarına sorunsuz bir şekilde geçirebilir. Güvenlik yöneticileri izni (gerektiğinde anahtarlara izin ve iptal edebilir).

Azure Stack aboneliğine herkesle oluşturabilir ve anahtar kasalarını kullanabilirsiniz. Anahtar kasası geliştiricilere ve güvenlik yöneticilerine avantaj sağlasa da, bir kuruluş için diğer Azure Stack hizmetlerini yöneten işleci uygulamak ve yönetmek. Örneğin, bir Azure Stack aboneliğiyle oturum açabilir, işareti işleci Azure Stack anahtarlarını depolamak ve ardından bu işlemsel görevlerden sorumlu kuruluş için bir kasa oluşturun:

* Oluşturun veya bir anahtar veya gizli içeri aktarın.
* İptal etmek veya bir anahtar veya gizli silin.
* Kullanıcılar veya bunlar sonra yönetebilir veya kendi anahtar ve gizli anahtarları için anahtar kasasına erişmek için uygulamaları yetkilendirme.
* Anahtar kullanımı yapılandırın (örneğin, imzalama veya şifreleme).

İşleç Tekdüzen Kaynak Tanımlayıcıları (URI'lar) uygulamalarından çağırmaları için geliştiricilere ardından sağlayabilirsiniz. İşleçler, güvenlik yöneticileri anahtar kullanımı günlüğü bilgilerini de sağlayabilirsiniz.

Geliştiriciler ayrıca anahtarları doğrudan API'lerini kullanarak yönetebilir. Daha fazla bilgi için Key Vault Geliştirici Kılavuzu na bakın.

## <a name="scenarios"></a>Senaryolar

Aşağıdaki senaryolarda, anahtar kasası geliştiricilere ve güvenlik yöneticilerinin ihtiyaçlarını nasıl yardımcı olabileceğini açıklar.

### <a name="developer-for-an-azure-stack-application"></a>Azure Stack uygulamanın geliştiricisi

**Sorun:** İmzalama ve şifreleme için anahtarları kullanan Azure Stack için bir uygulama yazmak istiyorum. Bu anahtarlar, çözümün coğrafi olarak dağıtılmış bir uygulama için uygun olacak şekilde benim, uygulamamın dışında olmasını istiyorum.

**deyimi:** Anahtarlar bir kasada depolanır ve gerektiğinde URI tarafından çağrılır.

### <a name="developer-for-software-as-a-service-saas"></a>Hizmet olarak yazılım (SaaS) geliştiricisi

**Sorun:** Sorumluluk veya olası bir yükümlülük my müşterinin anahtarları ve gizli anahtarları için istemiyorum. En iyi olan çekirdek yazılım özelliklerini sağlamaya neler yapabilirim yapmayı yoğunlaşmak amacıyla, müşterilerin kendi ve kendi anahtarlarını yönetmek istiyorum.

**deyimi:** Müşteriler, kendi anahtarlarına Azure yığına içeri aktarabilir ve bunları yönetebilirsiniz.

### <a name="chief-security-officer-cso"></a>Güvenlik Başkanı (CSO)

**Sorun:** Kuruluşumun anahtar yaşam döngüsü denetimine sahip olduğundan ve anahtar kullanımını izleyebildiğinden emin olmak istiyorum.

**deyimi:** Key Vault, Microsoft'un anahtarlarınızı göremeyeceği ve ayıklayamayacağı şekilde tasarlanmıştır. Bir uygulama müşteri anahtarlarını kullanarak şifreleme işlemleri gerçekleştirmesi gerektiğinde, anahtar kasası uygulama adına anahtarları kullanır. Uygulama, müşteri anahtarlarını görmez. Birden çok Azure Stack hizmetlerinin ve kaynakları kullanıyoruz ancak tek bir konumdan Azure Stack'te anahtarları yönetebilir. Kasa, destek ve hangi uygulamaların bunları kullandığına kaç kasanız olduğuna hangi bölgeler, Azure Stack bunlar bağımsız olarak tek bir arabirim sağlar.

## <a name="next-steps"></a>Sonraki adımlar

* [Anahtar Kasası'nda Azure Stack portal ile yönetme](azure-stack-key-vault-manage-portal.md)  
* [Anahtar Kasası'nda Azure Stack PowerShell kullanarak yönetme](azure-stack-key-vault-manage-powershell.md)

