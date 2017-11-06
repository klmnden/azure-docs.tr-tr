---
title: "Azure AD ile özel bir etki alanı ekleme | Microsoft Docs"
description: "Azure Active Directory'de özel etki alanı eklemek açıklanmaktadır."
services: active-directory
author: curtand
manager: femila
ms.assetid: 0a90c3c5-4e0e-43bd-a606-6ee00f163038
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: curtand
ms.reviewer: jsnow
ms.custom: it-pro
ms.openlocfilehash: 2ea119b56f467f57b5929dec05bfd645028578ef
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="quickstart-add-a-custom-domain-name-to-azure-active-directory"></a>Hızlı Başlangıç: bir özel etki alanı adını Azure Active Directory'ye ekleme

Bir ilk etki alanı adı biçiminde her Azure AD dizini birlikte *domainname*. onmicrosoft.com. İlk etki alanı adı değiştirilmiş ya da silinemez, ancak şirket etki alanı adınızı da Azure AD'ye ekleyebilirsiniz. Örneğin, kuruluşunuzun iş ve şirket etki alanı adınızı kullanarak oturum açan kullanıcıların yapmak için kullanılan diğer etki alanı adlarını büyük olasılıkla vardır. Azure AD ile özel etki alanı adları ekleme gibi kullanıcılarınız için tanıdık dizindeki kullanıcı adları atama olanak tanır 'alice@contoso.com.' yerine ' @ alice*<domain name>*. onmicrosoft.com'. Bu basit bir işlemdir:

1. Dizine özel etki alanı adı ekleyin
2. Etki alanı adı kayıt şirketinize etki alanı adı için bir DNS girişi ekleyin
3. Azure AD'de özel etki alanı adını doğrulayın

## <a name="add-the-custom-domain-name-to-your-directory"></a>Dizine özel etki alanı adı ekleyin
1. Oturum [Azure portal](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) dizini için genel yönetici olan bir hesapla.
2. Sol tarafta seçin **etki alanı adları**.
3. Üzerinde  ***dizin adı* -etki alanı adları**seçin **Ekle**.
   
   ![Ekle komutu seçin](./media/active-directory-domains-add-azure-portal/add-command.png)
5. Üzerinde **etki alanı adı**, kutusunda, "contoso.com" gibi özel etki alanınızın adını girin ve ardından **etki alanı Ekle**. .com, .net veya diğer üst düzey uzantıları eklemeyi unutmayın.
6. Üzerinde ***domainname*** (diğer bir deyişle, yeni etki alanı adınızı başlığı olan) özel etki alanı adını Azure AD'de doğrulamak için daha sonra kullanmak üzere DNS girdisi bilgi toplayın.
   
   ![DNS girişi bilgi alın](./media/active-directory-domains-add-azure-portal/get-dns-info.png)

> [!TIP]
> Azure AD ile şirket içi Windows Server AD birleştirmeyi planlıyor sonra seçmenize gerek **my yerel Active Directory ile bu etki alanı çoklu oturum açma için yapılandırma planlayın** dizinlerinizi eşitlemek için Azure AD Connect aracı çalıştırdığınızda, onay kutusu. Seçtiğiniz federasyona eklemek için şirket içi dizininizde ile aynı etki alanı adını kaydettirerek gerekir **Azure AD etki alanı** Sihirbazı'nda adım. [Bu yönergelerde](./connect/active-directory-aadconnect-get-started-custom.md#verify-the-azure-ad-domain-selected-for-federation) sihirbazdaki bu adımın nasıl göründüğünü görebilirsiniz. Azure AD Connect aracınız yoksa [buradan indirebilirsiniz](http://go.microsoft.com/fwlink/?LinkId=615771).

## <a name="add-a-dns-entry-for-the-domain-name-at-the-domain-name-registrar"></a>Etki alanı adı kayıt şirketinize etki alanı adı için bir DNS girişi ekleyin
Etki alanınızı Azure AD ile kullanmanın sonraki adımı, etki alanına ait DNS bölge dosyasının güncelleştirilmesidir. Azure AD, kuruluşunuzun özel etki alanı adı sahip olduğunu doğrulayabilirsiniz. Kullanabileceğiniz [Azure DNS](https://docs.microsoft.com/azure/dns/dns-getstarted-portal) Azure/Office 365/dış DNS kayıtlarınızı, azure'daki için veya DNS girdisi eklemek [farklı bir DNS kayıt şirketi](https://support.office.com/article/Create-DNS-records-for-Office-365-when-you-manage-your-DNS-records-b0f3fdca-8a80-4e8e-9ef3-61e8a2a9ab23/).

1. Etki alanına ilişkin etki alanı adı kayıt şirketinde oturum açın. DNS girişini güncelleştirmek için erişiminiz yoksa bu erişime sahip kişi ya da ekipten adım 2’yi tamamlamasını ve tamamlandığında size bildirmesini isteyin.
2. Azure AD tarafından size sağlanan DNS girişini ekleyerek etki alanına ilişkin DNS bölge dosyasını güncelleştirin. DNS girişi, posta yönlendirme veya web barındırma gibi davranışları değiştirmez.

## <a name="verify-the-custom-domain-name-in-azure-ad"></a>Azure AD'de özel etki alanı adını doğrulayın
DNS girişini ekledikten sonra etki alanı adını Azure AD ile doğrulamaya hazır olursunuz. DNS kayıtlarını yalnızca dağıtıldıktan sonra bir etki alanı adı doğrulanabilir. Bu yayma genellikle yalnızca birkaç saniye sürer, ancak bazen bir saat veya daha fazla sürebilir. İlk denemede doğrulama çalışmazsa daha sonra yeniden deneyin.

1. Oturum [Azure AD](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) dizini için genel yönetici olan bir hesapla.
2. Sol tarafta seçin **etki alanı adları**.
3. Üzerinde  ***dizin adı* -etki alanı adları**seçin **etki alanı adı Ekle** komutu. 
  ![Ekle komutu seçin](./media/active-directory-domains-add-azure-portal/add-command.png)
3. Üzerinde  ***dizin adı* -etki alanı adları**, doğrulamak istediğiniz doğrulanmamış etki alanı adı seçin.
4. Üzerinde ***domainname*** (diğer bir deyişle, seçili etki alanı adı Başlık:), seçin **doğrula** doğrulamayı tamamlamak için.

Artık [özel etki alanı adınızı içeren kullanıcı adları atayabilirsiniz](active-directory-users-create-azure-portal.md). Özel etki alanı adınızı kullanarak, şirket içi kullanıcı hesabı bilgilerini güncelleştirme daha önce Eşitlenenleri veya bulut tabanlı kullanıcı hesaplarına oluşturabilirsiniz. Eşitlenen kullanıcı hesabı etki alanı soneki bilgileri kullanarak değiştirebilirsiniz [Microsoft PowerShell](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains) veya [grafik API'si](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations).

> [!TIP]
> 900 yönetilen etki alanı adlarının maksimum kadar ekleyebilirsiniz. Tüm etki alanlarınızı Federasyon şirket içi Active Directory ile yapılandırmak istiyorsanız, her dizinde 450 etki alanı adlarının maksimum ekleyebilirsiniz. Daha fazla bilgi için bkz: [federe ve yönetilen etki alanı adları](https://docs.microsoft.com/azure/active-directory/active-directory-add-domain-concepts#federated-and-managed-domain-names).

## <a name="troubleshooting"></a>Sorun giderme
Özel etki alanı adını doğrulayamıyorsanız, aşağıdaki sorun giderme adımlarını deneyin:

1. **Bir saat bekleyin**. Azure AD etki alanı doğrulayabilirsiniz önce DNS kayıtlarını yayılması gerekir. Bu işlem, bir saat veya daha fazla sürebilir.
2. **DNS kaydının girildiğinden ve doğru olduğundan emin olun**. Bu adımı, etki alanının etki alanı adı kayıt şirketine ait web sitesinde tamamlayın. Azure AD etki alanı adı varsa doğrulanamıyor 
  * DNS girişini DNS bölge dosyasında mevcut değil
  * Azure AD, sağladığınız DNS girişi ile tam bir eşleşme değil. 
  
  Etki alanı adı kayıt şirketinde etki alanının DNS kayıtlarını güncelleştirmek için erişiminiz yoksa, DNS girişini bu erişime sahip olan kişi veya ekip ile paylaşın ve DNS girişini eklemesini isteyin.
3. **Etki alanı adını Azure AD'deki başka bir dizinden silin**. Bir etki alanı adı yalnızca tek bir dizinde doğrulanabilir. Bir etki alanı adı şu anda farklı bir dizinde doğrulandı, diğer bir silinene kadar yeni dizininizde doğrulanamıyor. Etki alanı adlarını silme hakkında bilgi edinmek için bkz. [Özel etki alanı adlarını yönetme](active-directory-domains-manage-azure-portal.md).    

## <a name="add-more-custom-domain-names"></a>Daha fazla özel etki alanı ekleme
Kuruluşunuz 'contoso.com' ve 'contosobank.com' gibi birden fazla özel etki alanı adları kullanıyorsa, en fazla 900 etki alanı adları en fazla ekleyebilirsiniz. Bu makaledeki adımları her etki alanı eklemenize yardımcı olabilir.

### <a name="learn-more"></a>Daha fazla bilgi edinin
[Azure AD'de özel etki alanı adlarına kavramsal genel bakış](active-directory-add-domain-concepts.md)

[Özel etki alanı adlarını yönetme](active-directory-domains-manage-azure-portal.md)

## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıç Azure AD ile özel bir etki alanı ekleme öğrendiniz. 

Azure portalından Azure AD içinde yeni bir özel etki alanı eklemek için aşağıdaki bağlantıyı kullanın.

> [!div class="nextstepaction"]
> [Özel bir etki alanı Ekle](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/QuickStart) 