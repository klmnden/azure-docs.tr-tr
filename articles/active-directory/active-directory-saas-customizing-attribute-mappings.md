---
title: "Azure AD öznitelik eşlemelerini özelleştirme | Microsoft Docs"
description: "Hangi öznitelik eşlemelerini Azure Active Directory'de SaaS uygulamaları için bunları iş ihtiyaçlarınızı karşılamak için nasıl değiştirebileceğiniz olduğunu öğrenin."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 549e0b8c-87ce-4c9b-b487-b7bf0155dc77
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/17/2017
ms.author: markvi
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6921ca86efeea9d1255bb2d1773f55daa48b9b4a
ms.sourcegitcommit: 6acb46cfc07f8fade42aff1e3f1c578aa9150c73
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2017
---
# <a name="customizing-user-provisioning-attribute-mappings-for-saas-applications-in-azure-active-directory"></a>Kullanıcı Azure Active Directory'de SaaS uygulamaları için öznitelik eşlemelerini hazırlama özelleştirme
Microsoft Azure AD Salesforce, Google Apps ve diğerleri gibi üçüncü taraf SaaS uygulamalarına kullanıcı hazırlama için destek sağlar. Etkin bir üçüncü taraf SaaS uygulaması için sağlama kullanıcı varsa, Azure Yönetim Portalı, öznitelik değerleri "özellik eşlemesi." adlı bir yapılandırma biçiminde denetler.

Önceden yapılandırılmış öznitelik eşlemelerini Azure AD kullanıcı ve her SaaS uygulamanın kullanıcı nesneleri arasında birtakım yoktur. Bazı uygulamalar, diğer grupların veya kişilerin gibi nesne türlerini yönetin. <br> 
 Varsayılan öznitelik eşlemelerini iş gereksinimlerinize göre özelleştirebilirsiniz. Bu anlamına gelir, değiştirin veya varolan öznitelik eşlemelerini silebilir veya yeni öznitelik eşlemelerini oluşturun.

Azure AD portalında tıklatarak bu özellik erişebilirsiniz bir **eşlemeleri** yapılandırmada **sağlama** içinde **Yönet** bölümünü bir **Kurumsal uygulama**.


![Salesforce][5] 

Tıklatarak bir **eşlemeleri** yapılandırma, ilgili açılır **eşleme özniteliği** dikey.  
Doğru çalışması için bir SaaS uygulaması tarafından gerekli öznitelik eşlemelerini vardır. Gerekli öznitelikler için **silmek** özelliği kullanılamıyor.


![Salesforce][6]  

Yukarıdaki örnekte, gördüğünüz **kullanıcıadı** Salesforce içinde yönetilen bir nesnenin özniteliği ile doldurulur **userPrincipalName** bağlı Azure Active Directory nesne değeri.

Varolan özelleştirebilirsiniz **öznitelik eşlemelerini** eşleme tıklatarak. Bu açılır **öznitelik Düzenle** dikey.

![Salesforce][7]  


  

## <a name="understanding-attribute-mapping-types"></a>Öznitelik eşleme türlerini anlama
Öznitelik eşlemelerini ile öznitelikleri bir üçüncü taraf SaaS uygulamasına nasıl doldurulur denetler. Desteklenen dört farklı eşleme türleri şunlardır:

* **Doğrudan** – Hedef öznitelik Azure AD'de bağlantılı nesne özniteliği değeri ile doldurulur.
* **Sabit** – Hedef öznitelik belirttiğiniz için belirli bir dizeyi doldurulur.
* **İfade** -Hedef öznitelik sonucuna göre bir betik benzeri ifadesi doldurulur. 
  Daha fazla bilgi için bkz: [Azure Active Directory'de özellik eşlemeleri için ifade yazma](active-directory-saas-writing-expressions-for-attribute-mappings.md).
* **Hiçbiri** -Hedef öznitelik sol değiştirilmemiş. Ancak, hedef öznitelik sürekli boşsa, belirttiğiniz varsayılan değeri ile doldurulur.

Bu dört temel özniteliği eşleme türlerine ek olarak özel öznitelik eşlemelerini kavramı, isteğe bağlı destek **varsayılan** değer atama. Hedef nesne ya da Azure AD'de hiçbiri bir değer varsa bir target özniteliği değeri ile doldurulur ve varsayılan değer atamasını sağlar. Bu alanı boş bırakın için en yaygın yapılandırmadır bakın.


## <a name="understanding-attribute-mapping-properties"></a>Öznitelik Eşleme Özellikleri Anlama

Önceki bölümde, zaten öznitelik eşleme türü özelliği tanıtılmıştır.
Bu özellik ek olarak, öznitelik eşlemelerini ayrıca aşağıdaki özniteliklere destekler:

- **Kaynak özniteliği** -kaynak sistemden kullanıcı özniteliği (örn: Azure Active Directory).
- **Hedef öznitelik** – hedef sistem kullanıcı özniteliğinde (örn: ServiceNow).
- **Bu öznitelik kullanarak nesneleri eşleşen** – bu eşlemeyi kullanıcılar kaynak ve hedef sistemler arasında benzersiz şekilde tanımlamak için kullanılması gereken olup olmadığına bakılmaksızın. Bu genellikle, userPrincipalName veya posta özniteliği genellikle bir hedef uygulama username alanına eşlenen Azure AD'de ayarlanır.
- **Öncelik eşleşen** – birden çok öznitelikleri eşleşen ayarlanabilir. Olduğunda birden çok, bu alana göre tanımlanan sırayla değerlendirilir. Bir eşleşme olarak başka eşleştirme öznitelikleri değerlendirilir.
- **Bu eşleme Uygula**
    - **Her zaman** – hem kullanıcı oluşturulması bu eşleme uygulamak ve güncelleştirme eylemleri
    - **Yalnızca oluşturma sırasında** -bu eşlemenin yalnızca kullanıcı oluşturma eylemlerini uygulamak


## <a name="what-you-should-know"></a>Bilmeniz gerekenler

Microsoft Azure AD eşitleme işlemini verimli bir uygulamasını sağlar. Başlatılmış bir ortamda, bir eşitleme döngüsü sırasında yalnızca güncelleştirmeleri gerektiren nesneler işlenir. Öznitelik eşlemelerini güncelleştirme bir eşitleme döngüsü performans üzerinde bir etkisi vardır. Öznitelik eşleme yapılandırması için bir güncelleştirme tüm yönetilen nesnelerin olmasını gerektirir. Bu ardışık değişiklik sayısı, öznitelik eşlemelerini en az tutmak için bir önerilen en iyi uygulamadır.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Active Directory'de Uygulama Yönetimi için Makale Dizini](active-directory-apps-index.md)
* [Kullanıcı sağlama/sağlamayı SaaS uygulamaları için otomatik hale getirme](active-directory-saas-app-provisioning.md)
* [Özellik eşlemeleri için ifade yazma](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [Kapsam belirleme filtreleri kullanıcı sağlama](active-directory-saas-scoping-filters.md)
* [Kullanıcıların ve grupların Azure Active Directory'den uygulamalara otomatik olarak hazırlanmasını etkinleştirmek için SCIM'yi kullanma](active-directory-scim-provisioning.md)
* [Hesap sağlama bildirimleri](active-directory-saas-account-provisioning-notifications.md)
* [SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)

<!--Image references-->
[1]: ./media/active-directory-saas-customizing-attribute-mappings/ic765497.png
[2]: ./media/active-directory-saas-customizing-attribute-mappings/ic775419.png
[3]: ./media/active-directory-saas-customizing-attribute-mappings/ic775420.png
[4]: ./media/active-directory-saas-customizing-attribute-mappings/ic775421.png
[5]: ./media/active-directory-saas-customizing-attribute-mappings/21.png
[6]: ./media/active-directory-saas-customizing-attribute-mappings/22.png
[7]: ./media/active-directory-saas-customizing-attribute-mappings/23.png

