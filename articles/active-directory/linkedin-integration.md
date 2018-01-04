---
title: "Azure AD'de LinkedIn tümleştirmesini yapılandırma | Microsoft Docs"
description: "Etkinleştirmek veya devre dışı LinkedIn tümleştirme Azure Active Directory'de Microsoft uygulamaları için açıklanmaktadır."
services: active-directory
author: jeffgilb
manager: mtillman
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: jeffgilb
ms.reviewer: jsnow
ms.custom: it-pro
ms.openlocfilehash: 15c161542d6826e6aeb5f708a0d9c3fa1f1885e3
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="enabling-linkedin-integration-in-azure-active-directory"></a>Azure Active Directory'de LinkedIn Integration etkinleştirme
LinkedIn Integration etkinleştirme kullanıcılarınızın iki ortak LinkedIn veri erişim sağlar ve bunlar, kendi kişisel LinkedIn ağdan Microsoft uygulamaların içindeki seçerseniz. Her bir kullanıcı kendi iş hesabını LinkedIn hesaplarında bağlanmak bağımsız olarak seçebilirsiniz.

### <a name="linkedin-integration-from-your-end-users-perspective"></a>Son kullanıcılarınızın açısından LinkedIn tümleştirme
Kuruluşunuzdaki son kullanıcılar iş hesaplarını, LinkedIn hesaplarını bağlandığınızda, bunlar içinde hem de şirket dışından çalışmak kişiler daha iyi tanımlamak kullanabilirsiniz. İki hesap bağlanmak kullanıcının LinkedIn bağlantıları ve kuruluşunuzun Microsoft uygulamalarındaki kullanılacak profil verilerini sağlar, ancak bunlar her zaman bu verileri paylaşmak LinkedIn izinlerini kaldırarak iptal edilebilir. Tümleştirme genel kullanıma açık profil bilgileri de kullanır ve kullanıcıların LinkedIn gizlilik ayarları aracılığıyla Microsoft uygulamalarıyla kendi ortak profil ve ağ bilgilerini paylaşmayı seçebilirsiniz.

>[!NOTE]
> Azure AD'de LinkedIn tümleştirme etkinleştirilmesi, uygulamalar ve hizmetler, son kullanıcılarınız bilgilerin bazıları erişim sağlar. Okuma [Microsoft gizlilik bildirimi](https://privacy.microsoft.com/privacystatement/) LinkedIn tümleştirme Azure AD'de etkinleştirirken gizlilik konuları hakkında daha fazla bilgi edinmek için. 

## <a name="enable-linkedin-integration"></a>LinkedIn tümleştirmeyi etkinleştir
LinkedIn tümleştirme kuruluşlar için Azure AD içinde varsayılan olarak etkindir. Bu nedenle, bu özellik, Kiracı için kullanılabilir yapıldığında, kuruluşunuzdaki tüm kullanıcılar LinkedIn özellikleri ve profilleri görebilirsiniz. LinkedIn Integration etkinleştirme Outlook içinde bir e-posta alırken, profilleri görüntüleme gibi Microsoft Hizmetleri içinde LinkedIn özellikleri kullanmak için kuruluşunuzdaki kullanıcılar sağlar. Bu özellik devre dışı bırakma LinkedIn özelliklerine erişimi engeller ve kullanıcı hesabı bağlantıları ve veri LinkedIn ve kuruluşunuzun Microsoft Hizmetleri aracılığıyla arasında paylaşımı durdurur.

> [!IMPORTANT]
> Bu özellik yerel Git, sovereign ve kamu kiracılar için kullanılabilir değil. Ayrıca, Azure AD hizmeti, LinkedIn tümleştirme özellikleri gibi güncelleştirmeler için tüm Azure kiracılar dağıtılmaz aynı anda. Bu özellik Azure kiracınız alındı kadar Azure AD ile LinkedIn tümleştirme etkinleştiremezsiniz.

1. Oturum [Azure Active Directory Yönetim Merkezi](https://aad.portal.azure.com/) dizini için genel yönetici olan bir hesapla.
2. Seçin **kullanıcılar ve gruplar**.
3. Üzerinde **kullanıcılar ve gruplar** dikey penceresinde, select **kullanıcı ayarlarını**.
4. Altında **LinkedIn tümleştirme**seçin Evet veya Hayır etkinleştirme veya devre dışı LinkedIn tümleştirme.
   ![LinkedIn Integration etkinleştirme](./media/linkedin-integration/LinkedIn-integration.PNG)

### <a name="learn-more"></a>Daha fazla bilgi edinin 
* [LinkedIn bilgileri ve Microsoft uygulamalarınızdaki özellikleri](https://go.microsoft.com/fwlink/?linkid=850740)

* [LinkedIn Yardım Merkezi](https://www.linkedin.com/help/linkedin)

## <a name="next-steps"></a>Sonraki adımlar
Etkinleştirmek veya Azure portalından Azure AD ile LinkedIn tümleştirme devre dışı bırakmak için aşağıdaki bağlantıyı kullanın:

[LinkedIn tümleştirmesini yapılandırma](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/UserSettings) 