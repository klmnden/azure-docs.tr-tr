<properties
    pageTitle="Azure Active Directory Cihaz Kaydına genel bakış | Microsoft Azure"
    description="cihaz temelli koşullu erişim senaryoları için altyapıdır. Bir cihaz kaydedildiğinde; Azure Active Directory Cihaz Kaydı, bir kullanıcı oturum açtığında kimlik doğrulama için kullanılan kimliğe sahip cihazı sağlar."
    services="active-directory"
    keywords="device registration, enable device registration, device registration and MDM"
    documentationCenter=""
    authors="femila"
    manager="stevenpo"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="03/29/2016"
    ms.author="femila"/>

# Azure Active Directory Cihaz Kaydına Başlarken

Azure Active Directory Cihaz Kaydı, cihaz temelli koşullu erişim senaryoları için altyapıdır. Bir cihaz kaydedildiğinde Azure Active Directory Cihaz Kaydı, bir kullanıcı oturum açtığında kimlik doğrulama için kullanılan kimliğe sahip cihazı sağlar. Kimliği doğrulanmış cihaz ve cihaz öznitelikleri böylece bulutta ve şirket içinde barındırılan uygulamalar için koşullu erişim ilkelerini zorlamak üzere kullanılabilir.

Intune gibi bir mobil cihaz yönetimi (MDM) çözümü ile birleştirildiğinde Azure Active Directory'deki cihaz öznitelikleri cihaz hakkındaki ek bilgilerle güncelleştirilir. Bu durum, güvenlik ve uyumluluğa yönelik standartlarınızı karşılamak için cihazlardan erişimi zorlayan koşullu erişim kuralları oluşturmanıza olanak sağlar.

Azure Active Directory Cihaz Kaydı, Azure Active Directory'nizden kullanılabilir. Bu hizmet iOS, Android ve Windows cihazları için destek içerir. Azure Active Directory Cihaz Kaydı'nı kullanan bireysel senaryolar daha fazla özel gereksinime ve platform desteğine sahip olabilir.

## Azure Active Directory Cihaz Kaydı tarafından etkinleştirilen senaryolar

Azure Active Directory Cihaz Kaydı iOS, Android ve Windows cihazları için destek içerir. Azure AD Cihaz Kaydı'nı kullanan bireysel senaryolar daha fazla özel gereksinime ve platform desteğine sahip olabilir. Bu senaryolar aşağıdaki gibidir:

- **Şirket içinde barındırılan uygulamalara koşullu erişim**: Windows Server 2012 R2 ile AD FS'yi kullanmak için yapılandırılan uygulamalar için erişim ilkeleri içeren kayıtlı cihazları kullanabilirsiniz. Şirket içi uygulamalara koşullu erişimi ayarlama hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory Cihaz Kaydı hizmetini kullanarak Şirket İçi Uygulamalara Koşullu Erişim](active-directory-conditional-access-on-premises-setup.md).

- **Microsoft Intune ile Office 365 uygulamaları için koşullu erişim** : BT yöneticileri kurumsal kaynakların güvenliğinin yanı sıra uyumlu cihazlar üzerindeki bilgi çalışanlarının hizmetlere erişimine olanak sağlamak için koşullu erişim cihaz ilkeleri sağlayabilirler. Daha fazla bilgi edinmek için bkz. Office 365 hizmetleri için Koşullu Erişim Cihaz İlkeleri.

##Azure Active Directory Cihaz Kaydı'nı ayarlama

Mobil cihazların iyi bilinen DNS kayıtlarına bakarak hizmeti keşfedebilmesi için Azure Portal'da Azure AD Cihaz Kaydı'nı etkinleştirmeniz gerekir. Windows 10, Windows 8.1, Windows 7, Android ve iOS cihazlarının hizmeti keşfedebilmesi ve kullanabilmesi için şirket DNS'nizi yapılandırmanız gerekir.
Azure Active Directory'de Yönetici Portalı'nı kullanarak kayıtlı cihazları görüntüleyebilir ve etkinleştirebilir/devre dışı bırakabilirsiniz.

### Azure Active Directory Cihaz Kaydı Hizmetini etkinleştirme

1. Microsoft Azure portalında Yönetici olarak oturum açın.
2. Sol bölmede **Active Directory**'yi seçin.
3. **Dizin** sekmesinde dizininizi seçin.
4. **Yapılandır** sekmesini seçin.
5. **Cihazlar** adlı bölüme kaydırın.
6. **KULLANICILAR ÇALIŞMA ALANINA CİHAZ KATABİLİR** için **TÜMÜ** seçeneğini belirleyin.
7. Kullanıcı başına yetkilendirmek istediğiniz maksimum cihaz sayısını seçin.

>[AZURE.NOTE]
>Office 365 için Microsoft Intune veya Mobil Cihaz Yönetimi ile kayıt için Çalışma Alanına Katılım gereklidir. Bu hizmetlerden herhangi birini yapılandırdıysanız TÜMÜ seçilir ve HİÇBİRİ düğmesi devre dışı bırakılır.

Varsayılan olarak hizmet için iki öğeli kimlik doğrulama etkin değildir. Yine de bir cihaz kaydedilirken iki öğeli kimlik doğrulama önerilir.

- Bu hizmet için iki öğeli kimlik doğrulama gerekmeden önce Azure Active Directory'de iki öğeli kimlik doğrulama sağlayıcısını yapılandırmanız ve kullanıcı hesaplarınızı Multi-Factor Authentication için yapılandırmanız gerekir, bkz. [Azure Active Directory'ye Multi-Factor Authentication ekleme](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md)

- Windows Server 2012 R2 ile AD FS kullanıyorsanız AD FS'de iki öğeli kimlik doğrulama modülünü yapılandırmanız gerekir, bkz. [Active Directory Federasyon Hizmetleri ile Multi-Factor Authentication kullanma](../multi-factor-authentication/multi-factor-authentication-get-started-server.md).

## Azure Active Directory Cihaz Kaydı keşfini yapılandırma
Windows 7 ve Windows 8.1 cihazları, kullanıcı hesabı adı ile iyi bilinen Cihaz Kaydı sunucu adını birleştirerek Cihaz Kaydı hizmetini bulur.

Azure Active Directory Cihaz Kaydı hizmetiniz ile ilişkili A kaydına işaret eden DNS CNAME oluşturmanız gerekir. CNAME kaydı, kuruluşunuzdaki kullanıcı hesapları tarafından kullanılan UPN sonekinin izlediği iyi bilinen enterpriseregistration önekini kullanmalıdır. Kuruluşunuz birden fazla UPN soneki kullanırsa DNS'de birden çok CNAME kaydının oluşturulması gerekir.

Örneğin, kuruluşunuzda @contoso.com ve @region.contoso.com adlı iki UPN soneki kullanırsanız aşağıdaki DNS kayıtlarını oluşturursunuz:

| Girdi                                     | Tür  | Adres                            |
|-------------------------------------------|-------|------------------------------------|
| enterpriseregistration.contoso.com        | CNAME | enterpriseregistration.windows.net |
| enterpriseregistration.region.contoso.com | CNAME | enterpriseregistration.windows.net |

## Azure Active Directory'de cihaz nesnelerini görüntüleme ve yönetme
1. Azure Yönetici portalından cihazları görüntüleyebilir, engelleyebilir ve engellerini kaldırabilirsiniz. Bu işlemden sonra, engellenen bir cihaz yalnızca kayıtlı cihazlara izin vermek için yapılandırılan uygulamalara erişim sağlayamaz.
2. Microsoft Azure Portal'da Yönetici olarak oturum açın.
3. Sol bölmede **Active Directory**'yi seçin.
4. Dizininizi seçin.
5. **Kullanıcılar** sekmesini seçin. Ardından cihazlarını görüntülemek için bir kullanıcı seçin.
6. **Cihazlar** sekmesini seçin.
7. Açılan menüden **Kayıtlı Cihazlar**'ı seçin.
8. Burada kullanıcının kayıtlı cihazlarını görüntüleyebilir, engelleyebilir veya engellerini kaldırabilirsiniz.

## Ek konu başlıkları

Azure AD Cihaz Kaydı ile Windows 7 ve Windows 8.1 Etki Alanına Katılmış cihazlarınızı kaydedebilirsiniz. Aşağıdaki konu başlığında Windows 7 ve Windows 8.1 cihazlarında cihaz kaydını yapılandırmak için gereken önkoşullar ve adımlar hakkında daha fazla bilgi sağlanmaktadır.

- [Windows Etki Alanına Katılmış Cihazlar için Azure Active Directory ile Otomatik Cihaz Kaydı](active-directory-conditional-access-automatic-device-registration.md)
- [Windows 7 etki alanına katılmış cihazlar için otomatik cihaz kaydını yapılandırma](active-directory-conditional-access-automatic-device-registration-windows7.md)
- [Windows 8.1 etki alanına katılmış cihazlar için otomatik cihaz kaydını yapılandırma](active-directory-conditional-access-automatic-device-registration-windows-8-1.md)
- [Windows 10 etki alanına katılmış cihazlar için Azure Active Directory ile otomatik cihaz kaydı](active-directory-azureadjoin-devices-group-policy.md)



<!--HONumber=Jun16_HO2-->


