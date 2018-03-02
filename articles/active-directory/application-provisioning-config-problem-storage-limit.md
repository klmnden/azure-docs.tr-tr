---
title: "Sorun yönetici kimlik bilgileri Azure AD galeri uygulamaya kullanıcı sağlamayı yapılandırılırken kaydetme | Microsoft Docs"
description: "Azure AD uygulama galerisinde listelenen yapılandırma kullanıcı bir uygulama zaten sağlama genişlettiklerinde karşılaştığı yaygın sorunlar nasıl giderilir"
services: active-directory
documentationcenter: 
author: asmalser-msft
manager: mtillman
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/21/2018
ms.author: asmalser
ms.openlocfilehash: 6617345c8923b1fc8081b01ddfe8b4bedf10b6ea
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="problem-saving-administrator-credentials-while-configuring-user-provisioning-to-an-azure-active-directory-gallery-application"></a>Yönetici kimlik bilgilerinin Azure Active Directory Galerisi uygulamaya kullanıcı sağlamayı yapılandırılırken kaydedilirken sorun 

Azure Portalı'nı yapılandırmak için kullanılırken [otomatik kullanıcı sağlamayı](active-directory-saas-app-provisioning.md) bir kurumsal uygulama için bir durum karşılaşabilirsiniz burada:

* **Yönetici kimlik bilgileri** uygulama için geçerli, girilen ve **Bağlantıyı Sına** düğmesini çalışır. Ancak, kimlik bilgileri kaydedilemiyor ve Azure portalına genel bir hata iletisi döndürür.

SAML tabanlı çoklu oturum açma de aynı uygulama için yapılandırılmışsa, hatanın en olası nedeni, Azure AD, uygulama başına iç depolama sınır sertifikaları için kimlik bilgileri aşıldı.

Şu anda Azure AD bir kilobayt tüm sertifikalar, gizli belirteçleri, kimlik bilgileri ve tek bir örnek uygulamanın (olarak da bilinen bir hizmet asıl kayıtta Azure AD) ile ilişkilendirilmiş ilgili yapılandırma verileri için bir maksimum depolama kapasitesine sahiptir.

SAML tabanlı çoklu oturum açma yapılandırıldığında, SAML belirteçleri imzalamak için kullanılan sertifika burada depolanır ve genellikle alanı % yüzde 50 üzerinde kullanır.

Tüm gizli belirteçleri, URI, bildirim e-posta adresleri, kullanıcı adları ve kullanıcı hazırlama, Kurulum sırasında girdiğiniz parolalar depolama sınırı aşılmış neden olabilir.

## <a name="how-to-work-around-this-issue"></a>Bu sorunu çözmek nasıl 

Bugün bu sorunun geçici çözümü için iki olası yolu vardır:

1. **İki galeri uygulama örnekleri, çoklu oturum açma için diğeri kullanıcı sağlama için kullanmak** -galeri uygulama alma [LinkedIn yükseltmesine](active-directory-saas-linkedinelevate-tutorial.md) örnek olarak, LinkedIn yükseltmesine Galeriden ekleyin ve yapılandırın Bunun için çoklu oturum açma. Sağlama, Azure AD uygulama galerisinde LinkedIn yükseltmesine başka bir örneği ekleyin ve "LinkedIn yükseltme (hazırlama)." adını Bu ikinci örneği için yapılandırma [sağlama](active-directory-saas-linkedinelevate-provisioning-tutorial.md), ancak çoklu oturum açmayı değil. Bu geçici çözüm kullanırken, aynı kullanıcıları ve grupları olmasına gerek [atanan](active-directory-coreapps-assign-user-azure-portal.md) hem uygulamalar için. 

2. **Depolanan yapılandırma veri miktarını azaltın** -girilen tüm veriler [yönetici kimlik bilgileri](active-directory-saas-app-provisioning.md#how-do-i-set-up-automatic-provisioning-to-an-application) sağlama sekmesinin bölümünde SAML sertifika ile aynı yerde depolanır. Tüm bu verileri kısaltın mümkün olmayabilir olsa da, bazı isteğe bağlı yapılandırma alanları ister **bildirim e-posta** kaldırılabilir.

## <a name="next-steps"></a>Sonraki adımlar
[Kullanıcı sağlama ve SaaS uygulamaları sağlamayı kaldırma özelliklerini yapılandırın](active-directory-saas-app-provisioning.md)
