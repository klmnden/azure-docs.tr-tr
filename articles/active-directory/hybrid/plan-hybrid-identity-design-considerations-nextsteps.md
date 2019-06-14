---
title: Azure Active Directory karma kimlik tasarımı hakkında önemli noktalar - sonraki adımlar | Microsoft Docs
description: Doğrulanır ve karma kimlik tasarım konuları Kılavuzu okuduktan sonra sonraki adımlar
documentationcenter: ''
services: active-directory
author: billmath
manager: daveba
editor: ''
ms.assetid: 02d48768-ea9e-4bfe-ae54-b54c4bd0a789
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 41741249e9b1a142d75392025236a4d333b67666
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60295135"
---
# <a name="azure-active-directory-hybrid-identity-design-considerations--next-steps"></a>Azure Active Directory karma kimlik tasarımı hakkında önemli noktalar sonraki adımlar
Gereksinimlerinizi tanımlamayı ve mobil cihaz Yönetimi çözümünüz için tüm seçenekleri inceleyerek tamamladığınıza göre sonraki adımları kendiniz ve kuruluşunuz için destekleyici altyapının dağıtımını için hazırsınız.

## <a name="hybrid-identity-solutions"></a>Karma kimlik çözümleri
-İhtiyaçlarınıza uyan belirli çözüm senaryoları yararlanarak gözden geçirin ve bir mobil cihaz Yönetimi altyapısının dağıtımındaki ayrıntıları planlamak için harika bir yoludur. Aşağıdaki çözümlerden bazıları yaygın mobil cihaz yönetim senaryolarını özetlemektedir:

* [Kurumsal ortamlarda çözümde mobil cihazları ve PC'leri yönetme](https://technet.microsoft.com/library/dn582037.aspx) , şirket içi System Center 2012 Configuration Manager altyapınızı Microsoft Intune ile buluta genişleterek mobil cihazları yönetmenize yardımcı olur. Bu karma altyapı, Orta ve BT uzmanları yardımcı olur ve büyük ortamlarda yönetimsel karmaşıklığı azaltırken KCG ve uzak erişimi etkinleştirin.
* [Çözüm Yapılandırma Yöneticisi 2007 için mobil cihazları yönetme](https://technet.microsoft.com/library/dn508400.aspx) altyapınız System Center Configuration Manager 2007'ye dayanıyorsa, mobil cihazları yönetmenize yardımcı olur. Bu çözüm, Microsoft Intune çalıştırın ve MDM özellğinden yararlanmak için System Center 2012 Configuration Manager çalıştıran tek bir sunucu kurulumu yapmak nasıl gösterir.
* [Çözümde küçük ortamlarda mobil cihazları yönetme](https://technet.microsoft.com/library/dn715906.aspx) MDM'yi desteklemesi gereken küçük işletmeler için hazırlanmıştır Bu, Microsoft Intune altyapınızı mobil cihaz yönetimi ve KCG'yi destekleyecek şekilde genişletmek için nasıl kullanılacağını açıklar. Bu çözüm, yerel sunucu olmadan yalnızca bulut yapılandırmasında tek başına Microsoft Intune kullanmak için desteklenen en temel senaryo anlatılır.

## <a name="hybrid-identity-documentation"></a>Hibrit kimlik belgeleri
Mobil cihaz Yönetimi çözümünüzü hayata geçirirken kavramsal ve yordam planlama, dağıtım ve yönetim içeriği kullanışlıdır:

* [Microsoft System Center](https://technet.microsoft.com/library/cc507089.aspx) çözümleri yardımcı olabilir, altyapı, ilkeler, süreçler ve en iyi uygulamalar hakkında toplu bilgileri yakalamanıza ve böylece BT personeliniz yönetilebilir sistemler oluşturabilir ve işlemleri otomatik hale getirin.
* [Microsoft Intune](https://technet.microsoft.com/library/jj676587.aspx) bilgisayarlar ve mobil cihazları yönetmek ve şirket bilgilerinizin güvenliğini sağlamaya yardımcı olan bir bulut tabanlı cihaz yönetim hizmetidir.
* [Office 365 için MDM](https://technet.microsoft.com/library/ms.o365.cc.devicepolicy.aspx) yönetin ve bunlar, Office 365 kuruluşunuza bağlanan mobil cihazları güvenli olanak tanır. Cihaz güvenlik ilkeleri ve erişim kuralları ayarlayın ve bunlar kaybolan veya çalınan mobil cihazları temizlemek için Office 365 için MDM kullanabilirsiniz.

## <a name="hybrid-identity-resources"></a>Karma kimlik kaynakları
Genellikle aşağıdaki kaynakları izleyerek en son haberlere ve mobil cihaz Yönetimi çözümlerimizle ilgili güncelleştirmeleri:

* [Microsoft Enterprise Mobility blogu](https://cloudblogs.microsoft.com/ENTERPRISEMOBILITY/)
* [Microsoft ın Cloud blogu](https://blogs.technet.com/b/in_the_cloud/)
* [Microsoft Intune blogu](https://blogs.technet.com/b/microsoftintune/)
* [Microsoft System Center Configuration Manager blogu](https://blogs.technet.com/b/configurationmgr/)
* [Microsoft System Center Configuration Manager ekip blogu](https://blogs.technet.com/b/configmgrteam/)

## <a name="see-also"></a>Ayrıca bkz.
[Tasarım konularına genel bakış](plan-hybrid-identity-design-considerations-overview.md)

