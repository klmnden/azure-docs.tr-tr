---
title: Azure Otomasyonu'nda kimlik doğrulamasına giriş
description: Bu makalede, Azure Otomasyonu’nda Otomasyon Hesapları için uygun Otomasyon güvenliği ve farklı kumluk doğrulasa yöntemlerine genel bakış verilmektedir.
keywords: otomasyon güvenliği, güvenli otomasyon; otomasyon kimlik doğrulaması
services: automation
ms.service: automation
ms.subservice: process-automation
author: bobbytreed
ms.author: robreed
ms.date: 03/19/2018
ms.topic: conceptual
manager: carmonm
ROBOTS: NOINDEX
ms.openlocfilehash: 88f1826191934ee76c565bd73de907a26d368c88
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67476836"
---
# <a name="introduction-to-authentication-in-azure-automation"></a>Azure Otomasyonu’nda kimlik doğrulamaya giriş  
Azure Automation, Azure’deki, şirket içindeki kaynaklara karşı ve Amazon Web Hizmetleri (AWS) gibi diğer bulut sağlayıcılarıyla görevleri otomatikleştirmenizi sağlar.  Runbook'un gerekli işlemlerini gerçekleştirebilmesi için, abonelikte gereken en düşük haklara sahip kaynaklara güvenli erişim izinlerinin olması gerekir.

Bu makale, Azure Automation’ın desteklediği çeşitli kimlik doğrulaması senaryolarını kapsamakta ve yönetmeniz gereken ortam veya ortamlar temelinde nasıl başlayacağınızı göstermektedir.  

## <a name="automation-account-overview"></a>Otomasyon Hesabına genel bakış
Azure Automation’ı ilk kez başlattığınızda, en az bir Automation hesabı oluşturmanız gerekir. Automation hesapları Automation kaynaklarınızı (runbook'lar, varlıklar, yapılandırmalar) diğer Automation hesaplarında yer alan kaynaklarından yalıtmanızı sağlar. Kaynaklarını ayrı mantıksal ortamlara ayırmak için Automation hesaplarını kullanabilirsiniz. Örneğin, geliştirme için bir hesap, üretim için başka bir hesap ve şirket içi ortamınız için de başka bir hesap kullanabilirsiniz.  Azure Automation hesabı, Azure aboneliğinizde oluşturduğunuz Microsoft hesabı veya hesaplarından farklıdır.

Her Otomasyon hesabı için Otomasyon kaynakları tek bir Azure bölgesiyle ilişkilendirilir, ancak Otomasyon hesapları aboneliğinizdeki tüm kaynakları yönetebilir. Farklı bölgelerde Automation hesapları oluşturmanın temel nedeni, veri ve kaynakların belirli bir bölgede yalıtılmasını gerektiren ilkelere sahip olmanız olabilir.

Azure Resource Manager ve Azure Otomasyonu’ndaki Azure cmdlet'lerini kullanan kaynaklara karşı gerçekleştirdiğiniz görevlerin tümü, Azure Active Directory kuruluş kimliğini kullanarak Azure’de kimlik bilgileri tabanlı kimlik doğrulamasını doğrulamalıdır.  Sertifika tabanlı kimlik doğrulaması Azure klasik ile asıl kimlik doğrulaması yöntemi olsa da, bunun kurulması karmaşıktır.  Azure AD kullanıcısının bulunduğu Azure’de kimlik doğrulaması, 2014’te yalnızca Kimlik Doğrulaması hesabını sadeleştirmek amacıyla değil, hem Azure Resource Manager hem de klasik kaynaklarla çalışan tek kullanıcı hesabıyla Azure’de etkileşimsiz kimlik doğrulamasını becerisini de destekler.   

Şu anda Azure portalında yeni bir Otomasyon hesabı oluşturduğunuzda otomatik olarak şunlar oluşturulur:

* Azure Active Directory'de yeni bir hizmet sorumlusu ve bir sertifika oluşturan ve Katkı Yapana runbook'lar kullanılarak Resource Manager kaynaklarını yönetmek için kullanılan rol tabanlı erişim denetimi (RBAC) atayan Farklı Çalıştır hesabı.
* Runbook kullanan Azure klasik kaynaklarını yönetmek için kullanılan bir yönetim sertifikasını karşıya yükleyen Klasik Farklı Çalıştır hesabı.  

Rol tabanlı erişim denetimi, Azure AD kullanıcı hesabı ve Farklı Çalıştır hesabına izin verilen eylemleri vermek, ve bu hizmet sorumlusunun kimliğini doğrulamak için Azure Resource Manager ile kullanılabilir.  Automation izinlerinin yönetilmesi için modelinizin geliştirilmesine yardımcı olma hakkında daha fazla bilgi için lütfen [Azure Automation’da rol tabanlı erişim denetimi](automation-role-based-access-control.md) makalesini okuyun.  

Veri merkezindeki Karma Runbook Çalışanı’nda veya AWS’deki bilgi işlem hizmetlerine karşı çalışan runbook'lar, genel olarak Azure kaynaklarında kimlik doğrulayan runbook’lar için kullanılan yöntemin aynısını kullanamaz.  Bunun nedeni, bu kaynakların Azure dışında çalışmasıdır; sonuç olarak da, yerel olarak erişecekleri kaynakların kimliğini doğrulamak için Otomasyon'da tanımlanan kendi güvenlik kimlik bilgileri gerekecektir.  

## <a name="authentication-methods"></a>Kimlik doğrulama yöntemleri
Aşağıdaki tabloda, Azure Automation tarafından desteklenen her ortamla ilgili farklı kimlik doğrulaması yöntemleri ve runbook’larınızın nasıl ayarlanacağını anlatan makaledeki bilgiler özetlenmiştir.

| Yöntem | Ortam | Makale |
| --- | --- | --- |
| Azure AD Kullanıcı Hesabı |Azure Resource Manager ve Azure klasik |[Azure AD Kullanıcı hesabıyla Runbook Kimlik Doğrulaması](automation-create-aduser-account.md) |
| Azure Farklı Çalıştır Hesabı |Azure Resource Manager |[Azure Farklı Çalıştır hesabıyla Runbook Kimlik Doğrulaması](automation-sec-configure-azure-runas-account.md) |
| Azure Klasik Farklı Çalıştır Hesabı |Azure klasik |[Azure Farklı Çalıştır hesabıyla Runbook Kimlik Doğrulaması](automation-sec-configure-azure-runas-account.md) |
| Windows Kimlik Doğrulaması |Şirket İçi Veri Merkezi |[Karma Runbook Çalışanları için Runbook Kimlik Doğrulaması](automation-hybrid-runbook-worker.md) |
| AWS Kimlik Bilgileri |Amazon Web Hizmetleri |[Amazon Web Hizmetleri (AWS) ile Runbook Kimlik Doğrulaması](automation-config-aws-account.md) |

