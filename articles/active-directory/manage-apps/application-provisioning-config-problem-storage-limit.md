---
title: Bir Azure AD galeri uygulamasına kullanıcı hazırlamayı yapılandırırken yönetici kimlik bilgilerini kaydetme sorunu | Microsoft Docs
description: Azure AD uygulama galerisinde bulunan yapılandırma kullanıcı uygulamaya zaten sağlama listelenen genişlettiklerinde karşılaştığı yaygın sorunları giderme
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/21/2018
ms.author: mimart
ms.reviewer: arvinh
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7e4af70ae8628f612b8858b99c0d5ae57e78ace4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65963610"
---
# <a name="problem-saving-administrator-credentials-while-configuring-user-provisioning-to-an-azure-active-directory-gallery-application"></a>Azure Active Directory Galerisi uygulamaya kullanıcı hazırlamayı yapılandırırken yönetici kimlik bilgilerini kaydetme sorunu 

Yapılandırmak için Azure portalını kullanarak zaman [otomatik kullanıcı hazırlama](user-provisioning.md) bir kurumsal uygulama için bir durumla karşılaşabilirsiniz. Burada:

* **Yönetici kimlik bilgileri** geçerli uygulama için girilen ve **Test Bağlantısı** düğmesini çalışır. Ancak, kimlik bilgileri kaydedilemiyor ve Azure portalında genel bir hata iletisi döndürür.

SAML tabanlı çoklu oturum açma de aynı uygulama için yapılandırılmışsa, hatanın en olası nedeni, Azure AD'nin, uygulama başına iç depolama sınır sertifikalar ve kimlik bilgilerini aşıldı.

Azure AD şu anda tüm sertifikalar, gizli belirteçleri, kimlik bilgileri ve ilgili yapılandırma verilerini (olarak da bilinen hizmet sorumlusu kaydını Azure AD'de) uygulamanın tek bir örneği ile ilişkili için 1024 bayt bir en büyük depolama kapasitesine sahiptir.

SAML tabanlı çoklu oturum açma yapılandırıldığında, SAML belirteçlerini imzalamak için kullanılan sertifikanın burada depolanır ve genellikle alanının yüzde 50 %'den kullanır.

Herhangi gizli belirteçleri, URI'ler, bildirim e-posta adresleri, kullanıcı adları ve kullanıcı hazırlama, Kurulum sırasında girdiğiniz parolaları depolama sınırı olması neden olabilir.

## <a name="how-to-work-around-this-issue"></a>Bu sorunu çözmek nasıl 

Bu sorunun geçici çözümü bugün için olası iki yolu vardır:

1. **İki galeri uygulama örnekleri, çoklu oturum açma için diğeri için kullanıcı sağlamayı kullanın** -galeri uygulama alma [LinkedIn yükseltmesine](../saas-apps/linkedinelevate-tutorial.md) örnek olarak, LinkedIn yükseltmesine galeri ekleme ve yapılandırma Bunun için çoklu oturum açma. Sağlama, LinkedIn yükseltmesine başka bir örneği Azure AD uygulama Galerisi ekleyin ve "LinkedIn yükseltme (hazırlama)." olarak adlandırın Bu ikinci örneği için yapılandırma [sağlama](../saas-apps/linkedinelevate-provisioning-tutorial.md), ancak çoklu oturum açmayı değil. Bu geçici çözüm kullanırken, aynı kullanıcıları ve grupları olmasına gerek [atanan](assign-user-or-group-access-portal.md) hem uygulamalar için. 

2. **Depolanan yapılandırma veri miktarını azaltmak** -girilen tüm verileri [yönetici kimlik bilgileri](user-provisioning.md#how-do-i-set-up-automatic-provisioning-to-an-application) sağlama sekmesinde bölümünü aynı yerde SAML sertifikası olarak depolanır. Tüm bu verileri kısaltın mümkün olmayabilir ancak bazı isteğe bağlı yapılandırma alanları ister **bildirim e-posta** kaldırılabilir.

## <a name="next-steps"></a>Sonraki adımlar
[Kullanıcı hazırlama ve sağlamayı SaaS uygulamaları için yapılandırma](user-provisioning.md)
