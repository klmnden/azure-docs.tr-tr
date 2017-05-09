---
title: "Özel etki alanı adınızı Azure Active Directory&quot;ye ekleme | Microsoft Belgeleri"
description: "Şirketinizin etki alanı adlarını Azure Active Directory&quot;ye ekleme ve etki alanı adını doğrulama."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 35a6e20a-9907-432b-9d36-16b916a5c249
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/08/2016
ms.author: curtand;jeffsta
ms.translationtype: Human Translation
ms.sourcegitcommit: f1f3d6fa1ba29b0d43d29c7d9cccfc430fdd6f1e
ms.openlocfilehash: 9803b4e41223bc979e4c0b0a4f22586a441c28df
ms.contentlocale: tr-tr
ms.lasthandoff: 02/22/2017


---
# <a name="add-a-custom-domain-name-to-azure-active-directory"></a>Azure Active Directory'ye özel etki alanı adı ekleyin
> [!div class="op_single_selector"]
> * [Azure portal](active-directory-domains-add-azure-portal.md)
> * [Klasik Azure Portalı](active-directory-add-domain.md)
> 
> 

Kuruluşunuzun iş amaçlı kullandığı bir veya daha fazla etki alanı adı var ve kullanıcılarınız şirket ağınızda sizin şirket etki alanı adınızı kullanarak oturum açıyor. Artık Azure Active Directory'yi (Azure AD) kullandığınıza göre şirket etki alanı adınızı da Azure AD'ye ekleyebilirsiniz. Bu sayede dizininizde "alice@contoso.com" gibi kullanıcılarınızın aşina olduğu kullanıcı adları atayabilirsiniz. Bu basit bir işlemdir:

1. Dizine özel etki alanı adı ekleyin
2. Etki alanı adı kayıt şirketinize etki alanı adı için bir DNS girişi ekleyin
3. Azure AD'de özel etki alanı adını doğrulayın

> [!NOTE]
> Active Directory Federasyon Hizmetleri (AD FS) veya şirket ağınızdaki farklı bir güvenlik belirteci hizmeti (STS) üzerinde kullanılacak özel etki alanı adınızı yapılandırmayı planlıyorsanız [Azure Active Directory ile federasyon için etki alanı ekleme ve yapılandırma](active-directory-add-domain-federated.md) içindeki yönergelere bakın. Bunun yapılması, şirket ağınızdaki kullanıcıları Azure AD ile eşitlemeyi planlıyorsanız ve [parola karma eşitlemesi](active-directory-aadconnectsync-implement-password-synchronization.md) gereksinimlerinizi karşılamıyorsa yararlıdır.
> 
> 

## <a name="add-a-custom-domain-name-to-your-directory"></a>Dizininize özel etki alanı adı ekleme
1. Azure AD dizininizin genel yöneticisi olan bir kullanıcı hesabıyla [klasik Azure portalında](https://manage.windowsazure.com/) oturum açın.
2. **Active Directory**'de dizininizi açın ve **Domains (Etki alanları)** sekmesini seçin.
3. Komut çubuğunda **Ekle**’yi seçin. Özel etki alanınız için 'contoso.com' gibi bir ad girin. .com, .net veya diğer üst düzey uzantıları eklediğinizden emin olun ve "çoklu oturum açma" (federasyon) onay kutusunu işaretsiz bırakın.
4. **Add (Ekle)** seçeneğini belirleyin.
5. Etki Alanı Ekle sihirbazının ikinci sayfasında, kuruluşunuzun özel etki alanına sahip olduğunu doğrulamak üzere Azure AD tarafından kullanılacak DNS girişini alın.

Etki alanı adınızı ekledikten sonra, Azure AD etki alanının kuruluşunuza ait olduğunu doğrulamalıdır. Azure AD'nin bu doğrulamayı gerçekleştirebilmesi için etki alanı adına ilişkin DNS bölge dosyasına bir DNS girişi eklemeniz gerekir. Bu işlem, etki alanı adının ait olduğu etki alanı adı kayıt şirketinin web sitesinden gerçekleştirilir.

## <a name="add-the-dns-entry-at-the-domain-name-registrar-for-the-domain"></a>Etki alanının etki alanı adı kayıt şirketinde DNS girişi ekleme
Etki alanınızı Azure AD ile kullanmanın sonraki adımı, etki alanına ait DNS bölge dosyasının güncelleştirilmesidir. Bunun yapılması, kuruluşunuzun özel etki alanı adına sahip olduğunun Azure AD tarafından doğrulanmasını sağlar.

1. Etki alanına ilişkin etki alanı adı kayıt şirketinde oturum açın. DNS girişini güncelleştirmek için erişiminiz yoksa bu erişime sahip kişi ya da ekipten adım 2’yi tamamlamasını ve tamamlandığında size bildirmesini isteyin.
2. Azure AD tarafından size sağlanan DNS girişini ekleyerek etki alanına ilişkin DNS bölge dosyasını güncelleştirin. Azure AD, bu DNS girişi sayesinde etki alanının size ait olduğunu doğrulayabilir. DNS girişi, posta yönlendirme veya web barındırma gibi davranışları değiştirmez.

DNS girişi ekleme hakkında yardım için [Popüler DNS kayıt şirketlerinde DNS girişi ekleme yönergeleri](https://support.office.com/article/Create-DNS-records-for-Office-365-when-you-manage-your-DNS-records-b0f3fdca-8a80-4e8e-9ef3-61e8a2a9ab23/) bölümünü okuyun

## <a name="verify-the-domain-name-with-azure-ad"></a>Azure AD ile etki alanı adını doğrulama
DNS girişini ekledikten sonra etki alanı adını Azure AD ile doğrulamaya hazır olursunuz.

**Add domain (Etki alanı ekleme)** sihirbazı hâlâ açıksa sihirbazın üçüncü sayfasında **Verify (Doğrula)** seçeneğini belirleyin. **Doğrula**’yı seçtiğinizde Azure AD, etki alanının DNS bölge dosyasındaki DNS girişini arar. Azure AD, etki alanı adını yalnızca DNS kayıtları yayıldıktan sonra doğrulayabilir. Bu yayma genellikle yalnızca birkaç saniye sürer, ancak bazen bir saat veya daha fazla sürebilir. İlk denemede doğrulama çalışmazsa daha sonra yeniden deneyin.

**Add domain (Etki alanı ekleme)** sihirbazı açık değilse etki alanını [klasik Azure portalında](https://manage.windowsazure.com/) doğrulayabilirsiniz:

1. Azure AD dizininizin genel yöneticisi olan bir kullanıcı hesabıyla oturum açın.
2. Dizininizi açıp **Domains (Etki alanları)** sekmesini seçin.
3. Doğrulamak istediğiniz etki alanı adını ve komut çubuğundaki **Doğrula** öğesini seçin.
4. Doğrulamayı tamamlamak için iletişim kutusundaki **Doğrula** öğesini seçin.

Artık [özel etki alanı adınızı içeren kullanıcı adları atayabilirsiniz](active-directory-add-domain-add-users.md).

## <a name="troubleshooting"></a>Sorun giderme
Bir özel etki alanı adını doğrulayamıyorsanız aşağıdakileri deneyin. En sık karşılaşılan ile başlayıp en az karşılaşılana kadar devam edeceğiz.

1. **Bir saat bekleyin**. Azure AD’nin etki alanını doğrulayabilmesi için DNS kayıtlarının yayılması gerekir. Bu işlem bir saat veya daha fazla sürebilir.
2. **DNS kaydının girildiğinden ve doğru olduğundan emin olun**. Bu adımı, etki alanının etki alanı adı kayıt şirketine ait web sitesinde tamamlayın. DNS girişi DNS bölge dosyasında mevcut değilse veya Azure AD’nin size sağladığı DNS girişi ile tam eşleşmiyorsa Azure AD etki alanı adını doğrulayamaz. Etki alanı adı kayıt şirketinde etki alanının DNS kayıtlarını güncelleştirmek için erişiminiz yoksa, DNS girişini bu erişime sahip olan kişi veya ekip ile paylaşın ve DNS girişini eklemesini isteyin.
3. **Etki alanı adını Azure AD'deki başka bir dizinden silin**. Bir etki alanı adı yalnızca tek bir dizinde doğrulanabilir. Bir etki alanı adı önce başka bir dizinde doğrulandıysa yeni dizininizde doğrulanabilmesi için oradan silinmelidir. Etki alanı adlarını silme hakkında bilgi edinmek için bkz. [Özel etki alanı adlarını yönetme](active-directory-add-manage-domain-names.md).

## <a name="add-more-custom-domain-names"></a>Daha fazla özel etki alanı ekleme
Kuruluşunuz "contoso.com" ve "contosobank.com" gibi birden fazla özel etki alanı adı kullanıyorsa 900'e kadar etki alanı adı ekleyebilirsiniz. Her bir etki alanı adını eklemek için bu makaledeki adımları tekrarlayın.

## <a name="next-steps"></a>Sonraki adımlar
* [Özel etki alanı adınızı içeren kullanıcı adları atama](active-directory-add-domain-add-users.md)
* [Özel etki alanı adlarını yönetme](active-directory-add-manage-domain-names.md)
* [Azure AD'deki etki alanı yönetimi kavramları hakkında bilgi edinme](active-directory-add-domain-concepts.md)
* [Kullanıcılarınız oturum açtığında şirketinizin markasını gösterme](active-directory-add-company-branding.md)
* [PowerShell'i kullanarak Azure AD'deki etki alanı adlarını yönetme](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)


