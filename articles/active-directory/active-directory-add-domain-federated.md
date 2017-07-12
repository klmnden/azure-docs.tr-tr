---
title: "Azure Active Directory&quot;de özel etki alanı adınızı ekleme ve federasyon oturumu açma | Microsoft Docs"
description: "Azure Active Directory ile şirket içi federasyon çözümünüz arasında federasyon oturumu açma özelliğini ayarlamak için şirketinizin etki alanı adlarını Azure Active Directory&quot;ye ekleme"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 27126c7e-e6d6-4ef3-a4fb-f5f0706e749d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/16/2017
ms.author: curtand;jeffsta
ms.translationtype: Human Translation
ms.sourcegitcommit: c308183ffe6a01f4d4bf6f5817945629cbcedc92
ms.openlocfilehash: 7f257dff6fdd423b89d1c52a84b64951dcf5915f
ms.contentlocale: tr-tr
ms.lasthandoff: 05/17/2017


---
<a id="add-your-custom-domain-name-to-azure-active-directory" class="xliff"></a>

# Özel etki alanı adınızı Azure Active Directory'ye ekleme
‘contoso.com’ gibi özel bir etki alanı adını, contoso.com içindeki kullanıcılar şirket ağınızdan çoklu federasyon oturumu açma deneyimi yaşayabilsin diye yapılandırabilirsiniz. Active Directory Federasyon Hizmetleri (AD FS) veya şirket ağınızda çalışan farklı bir federasyon sunucunuz zaten varsa, Azure AD Connect aracını kullanarak Azure AD’yi özel etki alanınızı kullanacak şekilde yapılandırabilirsiniz. Azure AD Connect’i ayrıca yeni bir AD FS ortamı dağıtmak ve Azure AD’de federasyon çoklu oturum açma için yapılandırmak üzere kullanabilirsiniz.

AD FS veya başka bir federasyon sunucunuz yoksa ve dağıtmayı planlamıyorsanız şu yönergeleri izleyin: [Azure Active Directory’ye özel etki alanı ekleme](active-directory-add-domain.md).

<a id="add-a-custom-domain-name-to-your-directory" class="xliff"></a>

## Dizininize özel etki alanı adı ekleme
1. Azure AD dizininizin genel yöneticisi olan bir kullanıcı hesabıyla [klasik Azure portalında](https://manage.windowsazure.com/) oturum açın.
2. **Active Directory**'de dizininizi açın ve **Domains (Etki alanları)** sekmesini seçin.
3. Komut çubuğunda **Add (Ekle)** seçeneğini belirleyin ve ardından özel etki alanınızın adını (örneğin, "contoso.com") girin. .com, .net veya diğer üst düzey uzantıları eklemeyi unutmayın.
4. **Bu etki alanını, yerel Active Directory dizinimde çoklu oturum açmak üzere yapılandırmak istiyorum** onay kutusunu seçin.
5. **Add (Ekle)** seçeneğini belirleyin.

Azure AD’nin etki alanını doğrulamak için kullanacağı DNS girişini almak için Azure AD Connect aracını çalıştırın. DNS girişini sihirbazdaki **Azure AD Etki Alanı** adımında görürsünüz. [Bu yönergelerde](connect/active-directory-aadconnect-get-started-custom.md#verify-the-azure-ad-domain-selected-for-federation) sihirbazdaki bu adımın nasıl göründüğünü görebilirsiniz. Azure AD Connect aracınız yoksa [buradan indirebilirsiniz](http://go.microsoft.com/fwlink/?LinkId=615771).

<a id="add-the-dns-entry-at-the-domain-name-registrar-for-the-domain" class="xliff"></a>

## Etki alanının etki alanı adı kayıt şirketinde DNS girişi ekleme
Etki alanınızı Azure AD ile kullanmanın sonraki adımı, etki alanına ait DNS bölge dosyasının güncelleştirilmesidir. Bunun yapılması, kuruluşunuzun özel etki alanı adına sahip olduğunun Azure AD tarafından doğrulanmasını sağlar.

1. Etki alanı adınız için etki alanı adı kayıt şirketinin web sitesinde oturum açın. Bunu yapmak için erişiminiz yoksa kuruluşunuzda bu erişime sahip kişi ya da ekipten adım 2’yi tamamlamasını ve tamamlandığında size bildirmesini isteyin.
2. Azure AD tarafından size sağlanan DNS girişini ekleyerek etki alanına ilişkin DNS bölge dosyasını güncelleştirin. Azure AD, bu DNS girişi sayesinde etki alanının size ait olduğunu doğrulayabilir. DNS girişi, posta yönlendirme veya web barındırma gibi davranışları değiştirmez.

Bu adımla ilgili yardım için [Popüler DNS kayıt şirketlerinde DNS girişi ekleme yönergeleri](https://support.office.com/article/Create-DNS-records-for-Office-365-when-you-manage-your-DNS-records-b0f3fdca-8a80-4e8e-9ef3-61e8a2a9ab23/) bölümünü okuyun

<a id="verify-the-domain-name-with-azure-ad" class="xliff"></a>

## Azure AD ile etki alanı adını doğrulama
DNS girişini ekledikten sonra etki alanı adını Azure AD ile doğrulamaya hazır olursunuz.

Etki alanını doğrulamak için Azure AD Connect sihirbazının **Azure AD Etki Alanı** adımındaki **İleri**’yi seçin. Azure AD, etki alanının DNS bölge dosyasındaki DNS girişini arar. Azure AD, etki alanı adını yalnızca DNS kayıtları yayıldıktan sonra doğrular. Yayma genellikle yalnızca birkaç saniye sürer, ancak bazen bir saat veya daha fazla sürebilir. İlk denemede doğrulama çalışmazsa daha sonra yeniden deneyin.

Ardından Azure AD Connect sihirbazındaki diğer adımlara devam edin. Bunun yapılması Windows Server AD ile Azure AD arasında kullanıcıları eşitler. Federasyon için yapılandırdığınız etki alanındaki eşitlenmiş kullanıcılar şirket ağınızdan Azure AD’ye federasyon çoklu oturum açma deneyimini yaşayabilir.

<a id="troubleshooting" class="xliff"></a>

## Sorun giderme
Bir özel etki alanı adını doğrulayamıyorsanız aşağıdakileri deneyin. En sık karşılaşılan ile başlayıp en az karşılaşılana kadar devam edeceğiz.

1. **Bir saat bekleyin**. Azure AD’nin etki alanını doğrulayabilmesi için DNS kayıtlarının yayılması gerekir. Bu işlem bir saat veya daha fazla sürebilir.
2. **DNS kaydının girildiğinden ve doğru olduğundan emin olun**. Bu adımı, etki alanının etki alanı adı kayıt şirketine ait web sitesinde tamamlayın. DNS girişi DNS bölge dosyasında mevcut değilse veya Azure AD’nin size sağladığı DNS girişi ile tam eşleşmiyorsa Azure AD etki alanı adını doğrulayamaz. Etki alanı adı kayıt şirketinde etki alanının DNS kayıtlarını güncelleştirmek için erişiminiz yoksa, DNS girişini bu erişime sahip olan kişi veya ekip ile paylaşın ve DNS girişini eklemesini isteyin.
3. **Etki alanı adını Azure AD'deki başka bir dizinden silin**. Bir etki alanı adı yalnızca tek bir dizinde doğrulanabilir. Bir etki alanı adı önce başka bir dizinde doğrulandıysa yeni dizininizde doğrulanabilmesi için oradan silinmelidir. Etki alanı adlarını silme hakkında bilgi edinmek için bkz. [Özel etki alanı adlarını yönetme](active-directory-add-manage-domain-names.md).

<a id="add-more-custom-domain-names" class="xliff"></a>

## Daha fazla özel etki alanı ekleme
Kuruluşunuz "contoso.com" ve "contosobank.com" gibi birden fazla özel etki alanı adı kullanıyorsa 900'e kadar etki alanı adı ekleyebilirsiniz. Her bir etki alanı adını eklemek için bu makaledeki adımları tekrarlayın.

<a id="next-steps" class="xliff"></a>

## Sonraki adımlar
* [Özel etki alanı adlarını yönetme](active-directory-add-manage-domain-names.md)
* [Azure AD'deki etki alanı yönetimi kavramları hakkında bilgi edinme](active-directory-add-domain-concepts.md)
* [Kullanıcılarınız oturum açtığında şirketinizin markasını gösterme](active-directory-add-company-branding.md)
* [PowerShell'i kullanarak Azure AD'deki etki alanı adlarını yönetme](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)


