---
title: Azure Active Directory karma kimlik tasarımı hakkında dikkat edilecek noktalar - sonraki adımlar | Microsoft Docs
description: Doğrulanır ve karma kimlik tasarımı hakkında dikkat edilecek noktalar Kılavuzu okuduktan sonra sonraki adımlar
documentationcenter: ''
services: active-directory
author: billmath
manager: mtillman
editor: ''
ms.assetid: 02d48768-ea9e-4bfe-ae54-b54c4bd0a789
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: e63df0832431ddc1502ab7b07c60c8d4abf59ac4
ms.sourcegitcommit: b7290b2cede85db346bb88fe3a5b3b316620808d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34801499"
---
# <a name="azure-active-directory-hybrid-identity-design-considerations--next-steps"></a>Azure Active Directory karma kimlik tasarımı hakkında dikkat edilecek noktalar sonraki adımlar
Gereksinimlerinizi tanımlamayı ve mobil cihaz Yönetimi çözümünüz için tüm seçenekleri incelemeyi tamamladığınıza göre sonraki adımlar kendiniz ve kuruluşunuz için destekleyici altyapının dağıtımını yapmaya hazırsınız demektir.

## <a name="hybrid-identity-solutions"></a>Karma kimlik çözümleri
-İhtiyaçlarınıza uyan belirli çözüm senaryoları geliştirmek, gözden geçirmek ve bir mobil cihaz Yönetimi altyapısının dağıtımındaki ayrıntıları planlamak için mükemmel bir yoldur. Aşağıdaki çözümlerde en yaygın mobil cihaz Yönetimi senaryolarından bazıları ana hatlarıyla gösterilir:

* [Kurumsal ortamlarda çözümde mobil cihazları ve bilgisayarları yönetme](https://technet.microsoft.com/library/dn582037.aspx) şirket içi System Center 2012 Configuration Manager altyapınızı Microsoft Intune ile buluta genişleterek mobil cihazları yönetmenize yardımcı olur. Bu karma altyapı BT uzmanlarının ortamında yardımcı olur ve büyük ortamlarda yönetim karmaşıklığını azaltırken KCG ve uzak erişimi etkinleştirin.
* [Configuration Manager 2007 çözüm için mobil cihazları yönetme](https://technet.microsoft.com/library/dn508400.aspx) altyapınız System Center Configuration Manager 2007'dururken mobil cihazları yönetmenize yardımcı olur. Bu çözüm Microsoft Intune çalıştırın ve MDM becerilerinden yararlanmak için System Center 2012 Configuration Manager çalıştıran tek bir sunucuyu nasıl kurulur gösterir.
* [Küçük ortamlarda çözümde mobil aygıtları yönetme](https://technet.microsoft.com/library/dn715906.aspx) MDM'yi desteklemesi gereken küçük işletmeler için hazırlanmıştır Microsoft Intune altyapınızı mobil cihaz yönetimi ve KCG'yi destekleyecek şekilde genişletmek için nasıl kullanılacağını açıklar. Bu çözüm, yerel sunucu olmadan yalnızca bulut yapılandırmasında tek başına Microsoft Intune kullanmak için desteklenen en temel senaryo anlatılır.

## <a name="hybrid-identity-documentation"></a>Karma kimlik belgeleri
Mobil cihaz Yönetimi çözümünüzü hayata geçirirken kavramların ve yordamların sağlandığı planlama, dağıtım ve yönetim içeriği yararlı olur:

* [Microsoft System Center](https://technet.microsoft.com/library/cc507089.aspx) çözümleri yardımcı olur, bilgileri yakalamanıza ve toplamanıza altyapı, ilkeler, süreçler ve en iyi uygulamalar hakkında BT personeliniz yönetilebilir sistemler oluşturabilir ve işlemleri otomatik hale getirebilir.
* [Microsoft Intune](https://technet.microsoft.com/library/jj676587.aspx) bilgisayarları ve mobil cihazları yönetmek ve şirket bilgilerinizin güvenliğini sağlamak için yardımcı olan bir bulut tabanlı cihaz yönetim hizmetidir.
* [Office 365 için MDM](https://technet.microsoft.com/library/ms.o365.cc.devicepolicy.aspx) yönetme ve Office 365 kuruluşunuza bağlanan mobil aygıtları güvenliğini sağlar. Cihaz güvenlik ilkeleri ve erişim kuralları ayarlama ve bunlar kaybolan veya çalınan mobil cihazları temizlemek için Office 365 için MDM kullanabilirsiniz.

## <a name="hybrid-identity-resources"></a>Karma kimlik kaynakları
Genellikle aşağıdaki kaynakları izleyerek en son haberlere ve mobil cihaz Yönetimi çözümlerimizle ilgili güncelleştirmeleri:

* [Microsoft Enterprise Mobility blogu](http://blogs.technet.com/b/enterprisemobility/)
* [Microsoft ın Cloud blogu](http://blogs.technet.com/b/in_the_cloud/)
* [Microsoft Intune blogu](http://blogs.technet.com/b/microsoftintune/)
* [Microsoft System Center Configuration Manager blogu](http://blogs.technet.com/b/configurationmgr/)
* [Microsoft System Center Configuration Manager ekip blogu](http://blogs.technet.com/b/configmgrteam/)

## <a name="see-also"></a>Ayrıca bkz.
[Tasarım konularına genel bakış](active-directory-hybrid-identity-design-considerations-overview.md)

