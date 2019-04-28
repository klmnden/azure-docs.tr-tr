---
title: "Azure Active Directory etki alanı Hizmetleri: Azure Active Directory Uygulama proxy'si dağıtma | Microsoft Docs"
description: Azure Active Directory Domain Services yönetilen etki alanlarını Azure AD uygulama proxy'si kullanın
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: daveba
editor: curtand
ms.assetid: 938a5fbc-2dd1-4759-bcce-628a6e19ab9d
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/22/2018
ms.author: ergreenl
ms.openlocfilehash: 867d061e46494e5ef65340ce325a71638acc8dfa
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62104158"
---
# <a name="deploy-azure-ad-application-proxy-on-an-azure-ad-domain-services-managed-domain"></a>Azure AD Domain Services yönetilen etki alanı üzerinde Azure AD uygulama ara sunucusu dağıtma
Azure Active Directory (AD) uygulama proxy'si internet üzerinden erişilecek şirket içi uygulamalar yayımlayarak uzak çalışanları desteklemenize yardımcı olur. Azure AD Domain Services ile artık lift-and-shift ile taşıma eski uygulamaları şirket içinde çalışan Azure altyapı hizmetleri için kullanabilirsiniz. Ardından, kuruluşunuzdaki kullanıcılar için güvenli uzaktan erişim sağlamak için Azure AD uygulama proxy'si kullanarak bu uygulamaları yayımlayabilirsiniz.

Azure AD uygulama ara sunucusu için yeni başladıysanız, aşağıdaki makalede bu özellik hakkında daha fazla bilgi: [Güvenli uzaktan erişim sağlamak şirket içi uygulamalara](../active-directory/manage-apps/application-proxy.md).

[!INCLUDE [active-directory-ds-prerequisites.md](../../includes/active-directory-ds-prerequisites.md)]

## <a name="before-you-begin"></a>Başlamadan önce
Bu makalede listelenen görevleri gerçekleştirmek için gerekir:

1. Geçerli bir **Azure aboneliği**.
2. Bir **Azure AD dizini** -ya da şirket içi dizin veya bir yalnızca bulut dizini ile eşitlenir.
3. Bir **Azure AD temel veya Premium lisansı** Azure AD uygulama proxy'si kullanmak için gereklidir.
4. **Azure AD etki alanı Hizmetleri** Azure AD dizini için etkinleştirilmesi gerekir. Bunu yapmadıysanız, bölümünde açıklanan tüm görevleri izleyin [Başlarken kılavuzunda](active-directory-ds-getting-started.md).

<br>

## <a name="task-1---enable-azure-ad-application-proxy-for-your-azure-ad-directory"></a>Azure AD dizininiz için görev 1 - Enable Azure AD uygulama ara sunucusu
Azure AD dizininiz için Azure AD uygulama proxy'sini etkinleştirmek için aşağıdaki adımları gerçekleştirin.

1. Yönetici olarak oturum açın [Azure portalında](https://portal.azure.com).

2. Tıklayın **Azure Active Directory** dizin genel bakış getirilecek. Tıklayın **kurumsal uygulamalar**.

    ![Azure AD dizini seçme](./media/app-proxy/app-proxy-enable-start.png)
3. Tıklayın **uygulama proxy'si**. Bir Azure AD temel veya Azure AD Premium aboneliği yoksa, deneme sürümünü etkinleştir seçeneğine bakın. İki durumlu **uygulama ara sunucusunu etkinleştirme?** için **etkinleştirme** tıklatıp **Kaydet**.

    ![Uygulama Ara Sunucusunu etkinleştirme](./media/app-proxy/app-proxy-enable-proxy-blade.png)
4. Bağlayıcı indirmek için tıklayın **bağlayıcı** düğmesi.

    ![Bağlayıcıyı indir](./media/app-proxy/app-proxy-enabled-download-connector.png)
5. Karşıdan yükleme sayfasında lisans koşullarını ve Gizlilik Sözleşmesi kabul edin ve tıklayın **indirme** düğmesi.

    ![Yükleme Onayla](./media/app-proxy/app-proxy-enabled-confirm-download.png)


## <a name="task-2---provision-domain-joined-windows-servers-to-deploy-the-azure-ad-application-proxy-connector"></a>Görev 2 - Azure AD uygulama ara sunucusu Bağlayıcısı'nı dağıtmak için etki alanına katılmış Windows sunucuları sağlama
Windows Server sanal Azure AD uygulama ara sunucusu Bağlayıcısı'nı yükleyebilmek için makineleri etki alanına katılmış ihtiyacınız vardır. Bazı uygulamalar için birden çok sunucu bağlayıcıyı yüklediğiniz sağlamak tercih edebilirsiniz. Bu dağıtım seçeneği, daha yüksek kullanılabilirlik sağlar ve kimlik doğrulaması daha ağır yükler yardımcı işleme.

Azure AD Domain Services yönetilen etki alanınıza etkinleştirdiğiniz bağlayıcı sunucuları aynı sanal ağda (veya bağlı/eşlenmiş bir sanal ağ) sağlayın. Benzer şekilde, uygulama ara sunucusu üzerinden yayımlamayı uygulamaları barındıran sunucuları aynı Azure sanal ağ üzerinde yüklü olması gerekir.

Bağlayıcı sunucuları sağlamak için başlıklı makalede açıklanan görevler izleyin [bir Windows sanal makine için yönetilen etki alanına Katıl](active-directory-ds-admin-guide-join-windows-vm.md).


## <a name="task-3---install-and-register-the-azure-ad-application-proxy-connector"></a>Görev 3 - yükleme ve kaydetme Azure AD uygulama ara sunucusu Bağlayıcısı
Daha önce bir Windows Server sanal makinesi sağlanan ve yönetilen etki alanına. Bu görevde, bu sanal makinede Azure AD uygulama ara sunucusu Bağlayıcısı'nı yükleyin.

1. Azure AD Web uygulaması Ara sunucusu Bağlayıcısı'nı yüklediğiniz sanal makine için bağlayıcı yükleme paketini kopyalayın.

2. Çalıştırma **AADApplicationProxyConnectorInstaller.exe** sanal makinede. Yazılım Lisans Koşulları'nı kabul edin.

    ![Yüklemesi için koşulları kabul edin](./media/app-proxy/app-proxy-install-connector-terms.png)
3. Yükleme sırasında bağlayıcıyı Azure AD directory uygulama Proxy ile kaydetmeniz istenir.
   * Sağlayın, **Azure AD genel yönetici kimlik bilgilerini**. Genel yönetici kiracınız, Microsoft Azure kimlik bilgilerinizden farklı olabilir.
   * Bağlayıcıyı kaydetmek için kullanılan yönetici hesabının uygulama Proxy hizmetini etkinleştirdiğiniz aynı dizine ait olmalıdır. Örneğin, Kiracı etki alanı contoso.com ise yönetici olmalıdır admin@contoso.com ya da bu etki alanındaki tüm geçerli diğer ad.
   * IE Artırılmış güvenlik yapılandırması için sunucunun bağlayıcıyı yüklediğiniz etkinse, kayıt ekranı engellenebilir. Erişime izin vermek için hata iletisindeki yönergeleri uygulayın. Internet Explorer Artırılmış Güvenlik seçeneğinin devre dışı olduğundan emin olun.
   * Bağlayıcı kaydı başarısız olursa bkz. [Uygulama Proxy’si Sorunlarını Giderme](../active-directory/manage-apps/application-proxy-troubleshoot.md).

     ![Yüklü bağlayıcı](./media/app-proxy/app-proxy-connector-installed.png)
4. Bağlayıcı sağlamak için Azure AD uygulama ara sunucusu Bağlayıcısı giderici works düzgün bir şekilde çalıştırın. Sorun Giderici'yi çalıştırdıktan sonra başarılı bir rapor görmeniz gerekir.

    ![Sorun giderici başarılı](./media/app-proxy/app-proxy-connector-troubleshooter.png)
5. Azure AD directory uygulama proxy sayfasında listelenen yeni yüklenen bağlayıcı görmeniz gerekir.

    ![](./media/app-proxy/app-proxy-connector-page.png)

> [!NOTE]
> Bağlayıcıları Azure AD uygulaması Proxy üzerinden yayımlanan uygulamalarla kimlik doğrulaması için yüksek kullanılabilirlik sağlamak için birden çok sunucuda yüklemek tercih edebilirsiniz. Yönetilen etki alanınıza katılmış diğer sunucularda Bağlayıcısı'nı yüklemek için yukarıda listelenen adımları gerçekleştirin.
>
>

## <a name="next-steps"></a>Sonraki Adımlar
Azure AD uygulama ara sunucusunu ayarlamadıysanız ve Azure AD Domain Services yönetilen Etki Alanınızla tümleşik.

* **Uygulamalarınızı Azure sanal makineleri geçirme:** Lift-and-şirket içi sunuculardan uygulamalarınızı Azure sanal makineleri için yönetilen etki alanınıza katılmış shift ile taşıma kullanabilirsiniz. Bunun yapılması çalışan sunucular şirket içi altyapı maliyetlerini kurtulun yardımcı olur.

* **Azure AD uygulama ara sunucusu kullanarak uygulama yayımlama:** Azure AD uygulama ara sunucusu kullanarak Azure sanal makinelerinize üzerinde çalışan uygulamalar yayımlayın. Daha fazla bilgi için [Azure AD uygulama ara sunucusu kullanarak uygulama yayımlama](../active-directory/manage-apps/application-proxy-publish-azure-portal.md)


## <a name="deployment-note---publish-iwa-integrated-windows-authentication-applications-using-azure-ad-application-proxy"></a>Dağıtım Not - Azure AD uygulama proxy'si kullanarak yayımlama IWA (tümleşik Windows kimlik doğrulaması) uygulamaları
Kullanıcıların kimliğine bürünmek ve gönderip gerçekleştirilemeyeceğine ilişkin belirteçleri almak üzere uygulama Proxy Bağlayıcılarına vererek tümleşik Windows kimlik doğrulaması (IWA) kullanarak uygulamalarınızda çoklu oturum açmayı etkinleştirebilirsiniz. Kerberos Kısıtlı temsilci (KCD), bağlayıcı yönetilen etki alanındaki kaynaklara erişmek için gerekli izinleri vermek için yapılandırın. Kaynak tabanlı KCD mekanizması yönetilen etki alanları hakkında daha fazla güvenlik için kullanın.


### <a name="enable-resource-based-kerberos-constrained-delegation-for-the-azure-ad-application-proxy-connector"></a>Kaynak tabanlı kısıtlı Kerberos temsilcisi seçme için Azure AD uygulama ara sunucusu Bağlayıcısı'nı etkinleştir
Azure uygulama Proxy Bağlayıcısı kullanıcıların kimliğine bürünmek için Kerberos Kısıtlı temsilci (KCD) için yönetilen etki alanında yapılandırılmalıdır. Bir Azure AD Domain Services yönetilen etki alanında, etki alanı yönetici ayrıcalıklarına sahip değil. Bu nedenle, **geleneksel hesap düzeyinde KCD, yönetilen bir etki alanında yapılandırılamaz**.

Bu konuda açıklandığı gibi kaynak tabanlı KCD kullanın [makale](active-directory-ds-enable-kcd.md).

> [!NOTE]
> Yönetilen etki alanı AD PowerShell cmdlet'lerini kullanarak yönetmek için 'AAD DC Administrators' grubunun bir üyesi olmanız gerekir.
>
>

Azure AD uygulama ara sunucusu Bağlayıcısı'nı yüklü olduğu bilgisayarın ayarlarını almak için Get-ADComputer PowerShell cmdlet'ini kullanın.
```powershell
$ConnectorComputerAccount = Get-ADComputer -Identity contoso100-proxy.contoso100.com
```

Bundan sonra kaynak sunucu için kaynak tabanlı KCD ayarlamak için Set-ADComputer cmdlet'ini kullanın.
```powershell
Set-ADComputer contoso100-resource.contoso100.com -PrincipalsAllowedToDelegateToAccount $ConnectorComputerAccount
```

Yönetilen etki alanınızda birden çok uygulama Proxy Bağlayıcısı dağıttıysanız, kaynak tabanlı KCD gibi her bir bağlayıcı örneği için yapılandırmanız gerekir.


## <a name="related-content"></a>İlgili İçerik
* [Azure AD etki alanı Hizmetleri - başlangıç kılavuzu](active-directory-ds-getting-started.md)
* [Yönetilen bir etki alanında Kerberos sınırlı temsilini yapılandırmayı](active-directory-ds-enable-kcd.md)
* [Kerberos Kısıtlı temsilci seçmeye genel bakış](https://technet.microsoft.com/library/jj553400.aspx)
