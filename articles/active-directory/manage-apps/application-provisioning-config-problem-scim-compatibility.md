---
title: SCIM 2.0 protokolü uyumluluğunu Azure AD kullanıcı sağlama hizmeti ile bilinen sorunlar ve çözümleri | Microsoft Docs
description: Azure AD'ye 2.0 SCIM'yi destekleyen bir galeri dışı uygulama ekleme sırasında karşılaşılan yaygın protokol uyumluluk sorunlarını çözmek nasıl
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
ms.date: 12/03/2018
ms.author: mimart
ms.reviewer: arvinh
ms.collection: M365-identity-device-management
ms.openlocfilehash: a9a0e595d2120d3cdccd42c502a83de9d5ed3ff4
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65963183"
---
# <a name="known-issues-and-resolutions-with-scim-20-protocol-compliance-of-the-azure-ad-user-provisioning-service"></a>Bilinen sorunlar ve çözümleri ile Azure AD kullanıcı sağlama hizmetinin SCIM 2.0 protokol uyumluluğu

Azure Active Directory (Azure AD) otomatik olarak kullanıcı sağlama ve herhangi bir uygulama veya arabirimi ile bir web hizmeti tarafından fronted sistem gruplarına tanımlanan [etki alanları arası Kimlik Yönetimi (SCIM) 2.0 protokolü için sistemi belirtimi](https://tools.ietf.org/html/draft-ietf-scim-api-19). 

SCIM 2.0 protokolü desteğini Azure AD'nin açıklanan [kullanarak sistemi için etki alanları arası Kimlik Yönetimi (kullanıcılar ve grupların Azure Active Directory'den uygulamalara otomatik olarak sağlamak için SCIM'yi)](use-scim-to-provision-users-and-groups.md), hangi listeler Kullanıcıları ve grupları Azure ad 2.0 SCIM'yi destekleyen uygulamalar için otomatik olarak sağlamak için uyguladığı Protokolü belirli bölümlerini.

Bu makalede, Azure AD kullanıcı SCIM 2.0 protokolünü ve bu sorunların çözüm öğrenmek için hizmetin bağlılığı sağlama ile güncel ve geçmiş sorunları açıklanır.

> [!IMPORTANT]
> Azure AD kullanıcı sağlama hizmeti SCIM istemci en son güncelleştirmesi, 18 Aralık 2018'de yapılmıştır. Bu güncelleştirme, aşağıdaki tabloda listelenen bilinen uyumluluk sorunlarına yönelik. Aşağıdaki sık sorulan sorular bu güncelleştirme hakkında daha fazla bilgi için bkz.

## <a name="scim-20-compliance-issues-and-status"></a>SCIM 2.0 uyumluluk sorunları ve durumu

| **SCIM 2.0 uyumluluk sorunu** |  **Sabit?** | **Tarihi düzeltmek**  |  
|---|---|---|
| Azure AD gerektirir "/ scım" uygulamasının kök dizininde olması için kullanıcının SCIM uç nokta URL'si  | Evet  |  18 Aralık 2018'e | 
| Uzantı özniteliklerini kullanma nokta "."öznitelik adları yerine iki nokta üst üste önce Gösterimi":" gösterimi |  Evet  | 18 Aralık 2018'e  | 
|  Patch isteklerinde birden çok değerli öznitelikler için geçersiz bir yol filtresi sözdizimi içeriyor | Evet  |  18 Aralık 2018'e  | 
|  Grup oluşturma isteği geçersiz bir şema URI içerir | Evet  |  18 Aralık 2018'e  |  

## <a name="were-the-services-fixes-described-automatically-applied-to-my-pre-existing-scim-app"></a>Hizmetleri düzeltmeler, önceden mevcut olan SCIM uygulamama otomatik olarak uygulanan açıklanan?

Hayır. Bir değişiklik ile eski davranışa şekilde kodlandıysa SCIM uygulamalara constituted gibi değişiklikleri mevcut uygulamalar için otomatik olarak uygulanmadı.

Değişiklikler tüm yeni uygulanır [galeri dışı SCIM uygulamaları](configure-single-sign-on-non-gallery-applications.md) düzeltme tarihten sonra Azure portalında yapılandırılır.

En son düzeltmeler arasında sağlama işi önceden mevcut olan kullanıcı geçirme hakkında daha fazla bilgi için sonraki bölüme bakın.

## <a name="can-i-migrate-an-existing-scim-based-user-provisioning-job-to-include-the-latest-service-fixes"></a>En son hizmet düzeltmeleri dahil etmek için iş bir mevcut SCIM tabanlı kullanıcı sağlama geçişini sağlayabilir miyim?

Evet. Bu uygulama örneği için çoklu oturum açmayı zaten kullanıyorsunuz ve mevcut sağlama işin en son düzeltmeler arasında geçiş yapmanız, aşağıdaki yordamı izleyin. Bu yordam, Microsoft Graph API'sini kullanmayı açıklar ve eski sağlama işinizi mevcut SCIM uygulamanızdan kaldırıp yeni bir tane oluşturun Microsoft Graph API Gezgini yeni davranışı sergiler.

> [!NOTE]
> Uygulamanızı hala geliştirme aşamasındadır ve henüz çoklu oturum açma veya kullanıcı sağlamayı dağıtılmamışsa, kolay bir çözüm uygulama girişi silmektir **Azure Active Directory > Kurumsal uygulamalar**bölümü Azure Portalı'nın ve sadece kullanılarak uygulama için yeni bir giriş ekleyerek **uygulaması Oluştur > galeri dışı** seçeneği. Bu aşağıdaki yordamı çalıştırmak için bir alternatifidir.
 
1. Adresinden Azure portalında oturum https://portal.azure.com.
2. İçinde **Azure Active Directory > Kurumsal uygulamalar** bölümü Azure Portalı'nın, bulun ve mevcut SCIM uygulamanızı seçin.
3. İçinde **özellikleri** mevcut SCIM uygulamanızı kopyalama bölümünü **nesne kimliği**.
4. Yeni bir web tarayıcısı penceresinde Git https://developer.microsoft.com/graph/graph-explorer ve uygulamanızı nerede eklendiğinde Azure AD kiracısı için yönetici olarak oturum açın.
5. Graph Explorer'da sağlama iş Kimliğini bulmak için aşağıdaki komutu çalıştırın. "[Object-id]" hizmet sorumlusu kimliği (nesne kimliği) Üçüncü adımda kopyaladığınız değiştirin.
 
   `GET https://graph.microsoft.com/beta/servicePrincipals/[object-id]/synchronization/jobs` 

   ![İşleri Al](./media/application-provisioning-config-problem-scim-compatibility/get-jobs.PNG "işleri Al") 


6. Sonuçlarda "customappsso" veya "scım" ile başlayan tam bir "ID" dizesini kopyalayın.
7. Öznitelik eşlemesi yapılandırmasını almak için aşağıdaki komutu çalıştırın, bu nedenle, bir yedekleme yapabilirsiniz. Aynı [nesne kimliği] olarak önce kullanın ve son adımda kopyaladığınız sağlama iş kimliği [işlem id] değiştirin.
 
   `GET https://graph.microsoft.com/beta/servicePrincipals/[object-id]/synchronization/jobs/[job-id]/schema`
 
   ![Şeması get](./media/application-provisioning-config-problem-scim-compatibility/get-schema.PNG "şema Al") 

8. Son adımda JSON çıktısını kopyalayın ve bir metin dosyasına kaydedin. Bu, tüm özel öznitelik-eski uygulamanıza eklenir ve yaklaşık birkaç bin satır JSON olmalıdır eşlemeleri içerir.
9. Sağlama işi silmek için aşağıdaki komutu çalıştırın:
 
   `DELETE https://graph.microsoft.com/beta/servicePrincipals/[object-id]/synchronization/jobs/[job-id]`

10. En son hizmet düzeltmeleri içeren yeni bir sağlama işi oluşturmak için aşağıdaki komutu çalıştırın.

 `POST https://graph.microsoft.com/beta/servicePrincipals/[object-id]/synchronization/jobs`
 `{   templateId: "scim"   }`
   
11. Son adım sonuçlarında "scım" ile başlayan tam bir "ID" dizesini kopyalayın. İsteğe bağlı olarak, eski bir öznitelik eşlemelerini altında az önce kopyaladığınız yeni iş kimliği ve JSON istek gövdesi olarak #7. adım çıktı girme [Yeni proje-kimliği] yerine komutunu çalıştırarak yeniden uygulayın.

 `POST https://graph.microsoft.com/beta/servicePrincipals/[object-id]/synchronization/jobs/[new-job-id]/schema`
 `{   <your-schema-json-here>   }`

12. İlk web tarayıcı penceresine dönün ve seçin **sağlama** uygulamanız için sekmesinde.
13. Yapılandırmanızı doğrulamak ve sağlama işlemini başlatın. 

## <a name="can-i-add-a-new-non-gallery-app-that-has-the-old-user-provisioning-behavior"></a>Eski kullanıcı davranışı sağlama olan yeni bir galeri dışı uygulama ekleyebilirim?

Evet. Düzeltmeleri önce mevcut ve yeni bir örneğini dağıtmak ihtiyaç eski davranışı uygulamaya kodlanmış, aşağıdaki yordamı izleyin. Bu yordam, Microsoft Graph API'si ve Microsoft Graph API Gezgini eski davranışı sergiler SCIM sağlama işi oluşturmak için nasıl kullanılacağını açıklar.
 
1. Adresinden Azure portalında oturum https://portal.azure.com.
2. içinde **Azure Active Directory > Kurumsal uygulamalar > uygulama oluşturma** bölümde Azure portalı, yeni bir oluşturma **galeri dışı** uygulama.
3. İçinde **özellikleri** yeni özel uygulamanızı, kopya bölümünü **nesne kimliği**.
4. Yeni bir web tarayıcısı penceresinde Git https://developer.microsoft.com/graph/graph-explorer ve uygulamanızı nerede eklendiğinde Azure AD kiracısı için yönetici olarak oturum açın.
5. Graph Explorer'da, uygulamanız için sağlama Yapılandırması'nı başlatmak için aşağıdaki komutu çalıştırın.
   "[Object-id]" hizmet sorumlusu kimliği (nesne kimliği) Üçüncü adımda kopyaladığınız değiştirin.

   `POST https://graph.microsoft.com/beta/servicePrincipals/[object-id]/synchronization/jobs`
   `{   templateId: "customappsso"   }`
 
6. İlk web tarayıcı penceresine dönün ve seçin **sağlama** uygulamanız için sekmesinde.
7. Kullanıcının normalde yaptığınız gibi sağlama yapılandırmasını tamamlayın.


## <a name="next-steps"></a>Sonraki adımlar
[Hazırlama ve sağlamayı SaaS uygulamaları hakkında daha fazla bilgi edinin](user-provisioning.md)

