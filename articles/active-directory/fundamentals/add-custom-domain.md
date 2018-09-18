---
title: Azure Active Directory'ye özel etki alanınızı ekleme | Microsoft Docs
description: Azure Active Directory portalı kullanarak özel bir etki alanı eklemeyi öğrenin.
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: conceptual
ms.date: 09/10/2018
ms.author: lizross
ms.reviewer: elkuzmen
ms.custom: it-pro
ms.openlocfilehash: b33f2e809ae5758e41f7a76680347b9487f3f461
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/17/2018
ms.locfileid: "45735352"
---
# <a name="how-to-add-your-custom-domain-name-using-the-azure-active-directory-portal"></a>Nasıl yapılır: Azure Active Directory portalı kullanarak özel etki alanı adınızı ekleme
Her yeni Azure AD kiracısı bir ilk etki alanı adı ile gelir *domainname*. onmicrosoft.com. Değiştirme veya silme ilk etki alanı adı, ancak kuruluşunuzun adları listesine ekleyebilirsiniz. Özel etki alanı adları ekleme yardımcı olur, kullanıcılarınızın tanıdığı gibi kullanıcı adları oluşturmak için _alain@contoso.com_.

>[!Note]
>Her özel etki alanı için baştan sona tüm bu işlemi yinelemeniz gerekir.

## <a name="add-a-custom-domain-name"></a>Özel bir etki alanı adı ekleme
İlk olarak, özel etki alanı adınızı Azure AD kiracısına eklemeniz gerekir.

### <a name="to-add-a-custom-domain-name"></a>Özel etki alanı eklemek için
1. Oturum [Azure AD portalında](https://portal.azure.com/) dizinde genel yönetici hesabını kullanarak.

2. Seçin **Azure Active Directory**seçin **özel etki alanı adları**ve ardından **özel etki alanı Ekle**.

    ![Fabrikam - özel etki alanı ekle seçeneğinin vurgulandığı ile özel etki alanı adları dikey](media/add-custom-domain/add-custom-domain.png)

3. Yeni şirket etki alanı adınızı yazın **özel etki alanı adı** kutusunda (örneğin, _contoso.com_) ve ardından **etki alanı Ekle**.

    >[!Important]
    >.Com, .net veya diğer üst düzey uzantıları bunun düzgün çalışması için eklemeniz gerekir.

    ![Fabrikam - etki alanı Ekle düğmesi vurgulanmış ile özel etki alanı adları dikey](media/add-custom-domain/add-custom-domain-blade.png)

4. DNS girişi bilgilerini kopyalamak **Contoso** dikey penceresi.

    ![DNS girişi bilgilerle contoso dikey penceresi](media/add-custom-domain/contoso-blade-with-dns-info.png)

## <a name="add-your-domain-name-with-a-domain-name-registrar"></a>Bir etki alanı kayıt şirketinde etki alanı adınızı ekleme
Ardından, yeni özel etki alanınız için DNS bölge dosyasını güncelleştirmeniz gerekir. Kullanabileceğiniz [DNS bölgelerini](https://docs.microsoft.com/azure/dns/dns-getstarted-portal) için Azure, Office 365 veya dış DNS kayıtlarını veya farklı bir DNS kayıt şirketi kullanarak, yeni bir DNS girişi ekleyebilirsiniz (örneğin, [InterNIC](https://go.microsoft.com/fwlink/p/?LinkId=402770)).

### <a name="to-add-your-domain-name"></a>Etki alanı adınızı ekleme 
1. Özel etki alanınız için etki alanı adı kayıt şirketi için oturum açın. Giriş güncelleştirmek için doğru izinlere sahip değilseniz, bu izinlere sahip iletişime geçmeniz.

2. DNS girişini kayıt şirketi ile güncelleştirildikten sonra Azure AD tarafından sağlanan bilgilerle DNS bölge dosyasını güncelleştirmeniz gerekir.

    >[!Note]
    >DNS girişi, posta yönlendirme veya web barındırma çalışma şeklini değişmez.

## <a name="verify-your-custom-domain-name"></a>Özel etki alanı adınızı doğrulayın
Özel etki alanı adınızı kaydettikten sonra birkaç saat önce Azure AD, geçerli olarak görebileceğiniz için DNS bilgilerini yayar birkaç saniye sürebilir.

### <a name="to-verify-your-custom-domain-name"></a>Özel etki alanı adınızı doğrulamak için
1. Oturum [Azure AD portalında](https://portal.azure.com/) dizinde genel yönetici hesabını kullanarak.

2. Seçin **Azure Active Directory**ve ardından **özel etki alanı adları**.

3. Üzerinde **Fabrikam - özel etki alanı adları** dikey penceresinde, özel etki alanı adı seçin **Contoso**.

    ![Fabrikam - vurgulanmış contoso ile özel etki alanı adları dikey](media/add-custom-domain/custom-blade-with-contoso-highlighted.png)

4. Üzerinde **Contoso** dikey penceresinde **doğrulama** özel etki alanınızı düzgün şekilde kaydedildiğinden ve Azure AD için geçerli olduğundan emin olun.

    ![DNS girişi bilgileri ve Doğrula düğmesine contoso dikey penceresi](media/add-custom-domain/contoso-blade-with-dns-info-verify.png)

### <a name="common-verification-issues"></a>Sık karşılaşılan doğrulama sorunları
Azure AD'ye özel etki alanı adını doğrulayamıyorsanız, aşağıdaki önerileri deneyin:
- **En az bir saat bekleyin ve yeniden deneyin**. Azure AD etki alanı ve bu işlem bir saat veya daha fazla sürebilir doğrulamadan önce DNS kayıtlarının yayılması gerekir.

- **DNS kaydı doğru olduğundan emin olun.** Etki alanı adı kayıt şirketi siteye geri dönün ve giriş yoktur ve Azure AD tarafından sağlanan DSN giriş bilgilerini eşleştiğini doğrulayın.

    Kayıt şirketi sitenin kaydı güncelleştiremiyorsanız girdisi ekleyin ve doğru olduğundan emin olun için doğru izinlere sahip biri ile giriş paylaşmanız gerekir.

- **Etki alanı adı zaten başka bir dizinde olmadığından emin olun.** Bir etki alanı adı, yalnızca etki alanı adınızı şu anda başka bir dizinde doğrulanırsa, bu da yeni bir dizinde doğrulanamadığına anlamına gelen bir dizinde doğrulanabilir. Bu çoğaltma sorunu gidermek için etki alanı adı eski dizinden silmeniz gerekir. Etki alanı adlarını silme hakkında daha fazla bilgi için bkz. [özel etki alanı adlarını yönetme](../users-groups-roles/domains-manage.md).    

## <a name="next-steps"></a>Sonraki adımlar
- Kullanıcılar, etki alanına eklemek için bkz: [özel etki alanı adlarını yönetme](../users-groups-roles/domains-manage.md).

- Şirket içi Azure Active Directory ile birlikte kullanmak üzere istediğiniz Windows Server sürümleri varsa [şirket içi dizinlerinizi Azure Active Directory ile tümleştirme](../connect/active-directory-aadconnect.md).