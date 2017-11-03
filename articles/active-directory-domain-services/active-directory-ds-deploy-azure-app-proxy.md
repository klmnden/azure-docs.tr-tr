---
title: "Azure Active Directory etki alanı Hizmetleri: Azure Active Directory Uygulama Ara sunucusu dağıtma | Microsoft Docs"
description: "Azure Active Directory etki alanı Hizmetleri yönetilen etki alanları üzerinde Azure AD uygulama proxy'si kullanın"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 938a5fbc-2dd1-4759-bcce-628a6e19ab9d
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
ms.openlocfilehash: c158c67a82e12501386179e19bc75fd852d7e308
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-azure-ad-application-proxy-on-an-azure-ad-domain-services-managed-domain"></a>Azure AD uygulama proxy'si bir Azure AD etki alanı Hizmetleri yönetilen etki alanında dağıtma
Azure Active Directory (AD) uygulama proxy'si, internet üzerinden erişilebilmesi için şirket içi uygulamaları yayımlama tarafından uzaktan çalışanlar destek yardımcı olur. Azure AD etki alanı Hizmetleri ile Azure altyapı hizmetleri için şirket içi çalışan şimdi yükseltme-ve-shift eski uygulamalar olabilir. Ardından, kuruluşunuzdaki kullanıcılar için güvenli uzaktan erişim sağlamak için Azure AD uygulama proxy'si kullanarak bu uygulamaları yayımlayabilirsiniz.

Azure AD uygulama proxy'si yeniyseniz, bu özellik aşağıdaki makaleyi ile hakkında daha fazla bilgi: [güvenli uzaktan erişim sağlamak şirket içi uygulamalar](../active-directory/active-directory-application-proxy-get-started.md).


## <a name="before-you-begin"></a>Başlamadan önce
Bu makalede listelenen görevleri gerçekleştirmek için gerekir:

1. Geçerli bir **Azure aboneliği**.
2. Bir **Azure AD dizini** -ya da bir şirket içi dizin veya bir yalnızca bulut dizini ile eşitlenir.
3. Bir **Azure AD Basic veya Premium lisansı** Azure AD uygulama proxy'si kullanmak için gereklidir.
4. **Azure AD etki alanı Hizmetleri** Azure AD dizini için etkinleştirilmesi gerekir. Bunu yapmadıysanız, özetlenen tüm görevleri izleyin [Getting Started guide](active-directory-ds-getting-started.md).

<br>

## <a name="task-1---enable-azure-ad-application-proxy-for-your-azure-ad-directory"></a>Görev 1 - etkinleştir Azure AD uygulama proxy'si Azure AD dizininiz için
Azure AD dizininiz için Azure AD uygulama proxy'si etkinleştirmek için aşağıdaki adımları gerçekleştirin.

1. Bir yönetici olarak oturum açın [Azure portal](http://portal.azure.com).

2. Tıklatın **Azure Active Directory** directory genel bakış getirmek için. Tıklatın **kurumsal uygulamalar**.

    ![Azure AD dizini seçin](./media/app-proxy/app-proxy-enable-start.png)
3. Tıklatın **uygulama proxy'si**. Bir Azure AD temel veya Azure AD Premium aboneliğiniz yoksa, bir deneme sürümü'nü etkinleştirmek için bir seçenek görürsünüz. İki durumlu **uygulama ara sunucusunu etkinleştirme?** için **etkinleştirmek** tıklatıp **kaydetmek**.

    ![Uygulama Ara sunucusunu etkinleştirme](./media/app-proxy/app-proxy-enable-proxy-blade.png)
4. Bağlayıcısı'nı indirmek için tıklayın **bağlayıcı** düğmesi.

    ![Bağlayıcıyı indir](./media/app-proxy/app-proxy-enabled-download-connector.png)
5. İndirme sayfasında, lisans koşullarını ve Gizlilik Sözleşmesi kabul edin ve tıklayın **karşıdan** düğmesi.

    ![Yükleme Onayla](./media/app-proxy/app-proxy-enabled-confirm-download.png)


## <a name="task-2---provision-domain-joined-windows-servers-to-deploy-the-azure-ad-application-proxy-connector"></a>Görev 2 - Azure AD uygulama ara sunucusu Bağlayıcısı'nı dağıtmak için etki alanına katılmış Windows sunucuları hazırlama
Windows Server sanal Azure AD uygulama ara sunucusu Bağlayıcısı'nı yükleyebilmek için makinelerin etki alanına katılmış gerekir. Yayımlanan uygulamalara bağlı olarak, bağlayıcı yüklü olduğu birden çok sunucuları sağlamak tercih edebilirsiniz. Yardımcı daha ağır kimlik doğrulama yükü işlemek ve bu dağıtım seçeneğini daha yüksek kullanılabilirlik sağlar.

Azure AD etki alanı Hizmetleri yönetilen etki alanınızı etkinleştirdiğiniz bağlayıcı sunucuları aynı sanal ağ (veya bağlı ve eşlenen bir sanal ağda) sağlayın. Benzer şekilde, uygulama proxy'si yayımlamak uygulamaları barındıran sunucuları aynı Azure sanal ağ üzerinde yüklü olması gerekir.

Bağlayıcı sunucuları sağlamak için başlıklı makalesinde ana hatlarıyla görevleri izleyin [Windows sanal makinesini yönetilen bir etki alanına katma](active-directory-ds-admin-guide-join-windows-vm.md).


## <a name="task-3---install-and-register-the-azure-ad-application-proxy-connector"></a>Görev 3 - yükleme ve kaydetme Azure AD uygulama ara sunucusu Bağlayıcısı
Daha önce Windows Server sanal makine sağlanan ve yönetilen etki alanına katılan. Bu görevde, bu sanal makine üzerinde Azure AD uygulama ara sunucusu Bağlayıcısı'nı yükler.

1. Bağlayıcı yükleme paketini Azure AD Web uygulaması Ara sunucusu Bağlayıcısı'nı yüklemek VM kopyalayın.

2. Çalıştırma **AADApplicationProxyConnectorInstaller.exe** sanal makinede. Yazılım Lisans Koşulları'nı kabul edin.

    ![Yükleme için koşulları kabul edin](./media/app-proxy/app-proxy-install-connector-terms.png)
3. Yükleme sırasında bağlayıcı Azure AD dizininizi uygulaması proxy'si ile kaydetmeniz istenir.
    * Sağlayın, **Azure AD genel yönetici kimlik bilgileri**. Genel yönetici kiracınız, Microsoft Azure kimlik bilgilerinizden farklı olabilir.
    * Bağlayıcı kaydetmek için kullanılan yönetici hesabını uygulaması Ara sunucusu hizmetini etkinleştirdiğiniz dizinine ait olmalıdır. Örneğin, Kiracı etki alanı contoso.com ise, yönetici olmalıdır admin@contoso.com veya diğer geçerli diğer o etki alanı üzerinde.
    * IE Artırılmış Güvenlik Yapılandırması sunucusu Bağlayıcısı'nı yüklemekte olduğunuz açıksa, kayıt ekranı engellenebilir. Erişime izin vermek için hata iletisindeki yönergeleri izleyin. Internet Explorer Artırılmış Güvenlik seçeneğinin devre dışı olduğundan emin olun.
    * Bağlayıcı kaydı başarısız olursa bkz. [Uygulama Proxy’si Sorunlarını Giderme](../active-directory/active-directory-application-proxy-troubleshoot.md).

    ![Yüklü bağlayıcı](./media/app-proxy/app-proxy-connector-installed.png)
4. Bağlayıcı emin olmak için Azure AD uygulama ara sunucusu Bağlayıcısı sorun'gidericisini düzgün çalışır çalıştırın. Sorun gidericisini çalıştırdıktan sonra başarılı bir rapor görmeniz gerekir.

    ![Sorun gidericisini başarılı](./media/app-proxy/app-proxy-connector-troubleshooter.png)
5. Azure AD dizininizi uygulama proxy sayfasında listelenen yeni yüklenen bağlayıcı görmeniz gerekir.

    ![](./media/app-proxy/app-proxy-connector-page.png)

> [!NOTE]
> Azure AD uygulaması Proxy üzerinden yayımlanan uygulamalarla kimlik doğrulaması için yüksek kullanılabilirlik sağlamak için birden çok sunucuya bağlayıcıları yüklemeyi tercih edebilirsiniz. Bağlayıcı, yönetilen etki alanına katılmış diğer sunuculara yüklemek için yukarıda listelenen aynı adımları gerçekleştirin.
>
>

## <a name="next-steps"></a>Sonraki Adımlar
Azure AD uygulama ara sunucusunu ayarlamadıysanız sahip ve Azure AD etki alanı Hizmetleri yönetilen etki alanınız ile tümleşiktir.

* **Uygulamalarınızı Azure sanal makineleri geçirme:** yükseltme ve-şirket içi sunucular uygulamalarınızdan Azure sanal makineler için yönetilen etki alanına katılmış kaydırma yapabilirsiniz. Bunun yapılması, çalışmakta olan sunucuları şirket içi altyapı maliyetinden kurtulun yardımcı olur.

* **Azure AD uygulama proxy'si ile uygulama yayımlama:** , Azure Azure AD uygulama proxy'si kullanarak sanal makineleri üzerinde çalışan uygulamaları yayımlama. Daha fazla bilgi için bkz: [Azure AD uygulama proxy'si ile uygulama yayımlama](../active-directory/application-proxy-publish-azure-portal.md)


## <a name="deployment-note---publish-iwa-integrated-windows-authentication-applications-using-azure-ad-application-proxy"></a>Dağıtım Not - Azure AD uygulama proxy'si kullanarak yayımlama IWA (tümleşik Windows kimlik doğrulaması) uygulamaları
Kullanıcıların, kimliğine ve göndermek ve şirket adına belirteçleri almak için uygulama proxy'si bağlayıcıları izin vererek tümleşik Windows kimlik doğrulaması (IWA) kullanarak uygulamalarınızı için çoklu oturum açmayı etkinleştir. Kerberos Kısıtlı temsilci (KCD) yönetilen etki alanı kaynaklarına erişmek için gerekli izinleri vermek bağlayıcı için yapılandırın. Kaynak tabanlı KCD mekanizması yönetilen etki alanlarında daha yüksek güvenlik için kullanın.


### <a name="enable-resource-based-kerberos-constrained-delegation-for-the-azure-ad-application-proxy-connector"></a>Kaynak tabanlı kerberos Kısıtlı temsilci için Azure AD uygulama ara sunucusu Bağlayıcısı'nı etkinleştir
Kullanıcıların kimliğine bürünebileceği şekilde Azure uygulama ara sunucusu Bağlayıcısı'nı kerberos Kısıtlı temsilci (KCD), yönetilen etki alanında yapılandırılması gerekir. Bir Azure AD etki alanı Hizmetleri tarafından yönetilen etki alanında, etki alanı yönetici ayrıcalıklarına sahip değil. Bu nedenle, **geleneksel hesap düzeyinde KCD, yönetilen bir etki alanında yapılandırılamaz**.

Bu konuda açıklandığı gibi kaynak tabanlı KCD kullanın [makale](active-directory-ds-enable-kcd.md).

> [!NOTE]
> AD PowerShell cmdlet'lerini kullanarak yönetilen etki alanını yönetmek için 'AAD DC Yöneticiler' grubunun bir üyesi olmanız gerekir.
>
>

Azure AD uygulama ara sunucusu Bağlayıcısı'nı yüklü olduğu bilgisayarın ayarlarını almak için Get-ADComputer PowerShell cmdlet'ini kullanın.
```
$ConnectorComputerAccount = Get-ADComputer -Identity contoso100-proxy.contoso100.com
```

Bundan sonra kaynak sunucu için kaynak tabanlı KCD ayarlamak için Set-ADComputer cmdlet'ini kullanın.
```
Set-ADComputer contoso100-resource.contoso100.com -PrincipalsAllowedToDelegateToAccount $ConnectorComputerAccount
```

Yönetilen etki alanınızda birden çok uygulama proxy'si bağlayıcı dağıttıysanız, kaynak tabanlı KCD her tür bağlayıcı örneği için yapılandırmanız gerekir.


## <a name="related-content"></a>İlgili İçerik
* [Azure AD etki alanı Hizmetleri - başlangıç kılavuzu](active-directory-ds-getting-started.md)
* [Yönetilen bir etki alanında Kerberos Kısıtlanmış temsilci seçimini yapılandırın](active-directory-ds-enable-kcd.md)
* [Kerberos kısıtlanmış Temsile genel bakış](https://technet.microsoft.com/library/jj553400.aspx)
