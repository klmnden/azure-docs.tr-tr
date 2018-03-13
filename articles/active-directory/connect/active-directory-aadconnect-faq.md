---
title: 'Azure Active Directory Connect: SSS - | Microsoft Docs'
description: "Bu sayfa, Azure AD Connect hakkında sık bir sorulan sorular."
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
ms.assetid: 4e47a087-ebcd-4b63-9574-0c31907a39a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/09/2017
ms.author: billmath
ms.openlocfilehash: 07b0209ef94f91c00b98b8801323a58cd9d14494
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="frequently-asked-questions-for-azure-active-directory-connect"></a>Azure Active Directory Connect için sık sorulan sorular

## <a name="general-installation"></a>Genel yükleme
**S: Azure AD genel yönetici etkin 2FA varsa yükleme çalışacak mı?**  
Şubat 2016 yapılar ile bu desteklenir.

**S: Azure AD Connect Katılımsız yüklemenin bir yolu var mı?**  
Yalnızca Azure AD Connect Yükleme Sihirbazı'nı kullanarak yüklemek için desteklenir. Katılımsız ve sessiz bir yükleme desteklenmez.

**S: olduğu bir etki alanına erişilemiyor bir ormana sahibim. Azure AD Connect yükleme nasıl yaparım?**  
Şubat 2016 yapılar ile bu desteklenir.

**S: AD DS Sistem Durumu Aracısı Sunucu Çekirdeği yüklemesinde çalışmıyor?**  
Evet. Aracıyı yükledikten sonra aşağıdaki PowerShell cmdlet'ini kullanarak kayıt işlemini tamamlamak: 

`Register-AzureADConnectHealthADDSAgent -Credentials $cred`

**S: AADConnect, iki etki alanlarından üzerinde Azure AD eşitleme destekliyor mu?**</br>
Evet, bu desteklenir. Başvurmak [birden çok etki alanı](active-directory-aadconnect-multiple-domains.md)
 
**S: Azure AD'de aynı Active Directory etki alanı için birden çok bağlayıcı bağlanma sahip destekliyoruz?**</br> Hayır, bu olmayan desteklenmiyor 

## <a name="network"></a>Ağ
**S: sahibim bir güvenlik duvarı, ağ aygıtını veya başka bir şey en uzun süre bağlantıları sınırlayan Ağımdaki açık kalabilir. Ne kadar süreyle my istemci tarafı zaman aşımı eşiği Azure AD Connect kullanırken olmalıdır?**  
Tüm ağ yazılımı, fiziksel aygıtların ya da başka bir şey bağlantıları açık kalabileceği en uzun süre sınırlayan bir eşik en az 5 dakika (300 saniye olarak) Azure Active Directory ve Azure AD Connect istemcisinin yüklü olduğu sunucu arasındaki bağlantıyı için kullanmanız gerekir. Bu durum, tüm daha önce yayımlanmış Microsoft Identity eşitleme araçları için de geçerlidir.

**S: desteklenen SLD'ler (tek etiketli etki alanları) misiniz?**  
Hayır, Azure AD Connect, şirket içi ormanlar/SLD'ler kullanarak etki alanları desteklemez.

**S: desteklenen ayrık AD etki alanları ile ormanları misiniz?**  
Hayır, Azure AD Connect, şirket içi ormanları ayrık ad alanları içeren desteklemez.

**S: "noktalı" adlı NetBIOS desteklenir?**  
Hayır, Azure AD Connect, şirket içi ormanlar/etki, NetBIOS adını içerdiği bir süre desteklemiyor "." adında.

**S: desteklenen saf IPv6 ortam mi?**  
Hayır, Azure AD Connect saf IPv6 ortamında desteklemez.

## <a name="federation"></a>Federasyon
**S: t, bana my Office 365 sertifikayı yenilemek için isteyen bir e-posta almaya devam ederseniz ne yapmalıyım**  
İçinde açıklanan yönergeleri kullanmanız [sertifikaları Yenile](active-directory-aadconnect-o365-certs.md) sertifikayı yenilemek nasıl konu.

**S: "Otomatik olarak bağlı olan taraf O365 bağlı olan taraf için ayarlanmış güncelleştir" sahibim. My belirteç imzalama sertifikası otomatik olarak geldiğinde herhangi bir eylemde bulunmanız gerekir mi?**  
Makalede açıklanan yönergeleri kullanmanız [sertifikaları Yenile](active-directory-aadconnect-o365-certs.md).

## <a name="environment"></a>Ortam
**S: Azure AD Connect yüklendikten sonra sunucuyu yeniden adlandırmak için destekleniyor mu?**  
Hayır. Sunucu adının değiştirilmesi SQL veritabanına bağlanabilmek için olmaması eşitleme altyapısı neden olur ve hizmeti başlatmak mümkün olmaz.

## <a name="identity-data"></a>Kimlik verileri
**S: Azure AD UPN (userPrincipalName) özniteliğinde şirket içi UPN - neden eşleşmiyor?**  
Bu makalelere bakın:

* [Office 365, Azure veya Intune kullanıcı adları şirket içi UPN veya alternatif oturum açma kimliği eşleşmiyor](https://support.microsoft.com/en-us/kb/2523192)
* [Farklı bir Federasyon etki alanını kullanmak için bir kullanıcı hesabının UPN değiştirdikten sonra değişiklikleri Azure Active Directory eşitleme aracı tarafından eşitlenen değil](https://support.microsoft.com/en-us/kb/2669550)

UserPrincipalName açıklandığı şekilde güncelleştirmek eşitleme altyapısı izin vermek için Azure AD de yapılandırabilirsiniz [Azure AD Connect eşitleme hizmeti özelliklerini](active-directory-aadconnectsyncservice-features.md).

**S: olan yazılım eşleşen desteklenen şirket içi AD grup/kişisi nesneleri ile var olan Azure AD Grup/kişi nesneleri?**  
Evet, bu proxyAddress üzerinde temel alır.  Yazılım eşleşen posta-etkin olmayan grupları için desteklenmiyor.

**S: olan desteklenen el ile ayarlamak için sabit kendisine eşleştirmek için var olan Azure AD Grup/kişi nesneleri İmmutableıd özniteliği şirket içi AD Grup/kişi nesneleri?**  
Hayır, bu şu anda desteklenmiyor.

## <a name="custom-configuration"></a>Özel yapılandırma
**S: Burada Azure AD Connect için PowerShell cmdlet'leri belgelenen?**  
Bu sitede belgelenen cmdlet'leri hariç olmak üzere, Azure AD Connect içinde bulunan diğer PowerShell cmdlet'leri müşteri kullanım için desteklenmez.

**S: "sunucu dışa aktarma/sunucu alma bulundu" kullanabilirim *Eşitleme Hizmeti Yöneticisi'ni* yapılandırma sunucuları arasında taşımak için?**  
Hayır. Bu seçenek, tüm yapılandırma ayarlarını almaz ve kullanılmamalıdır. Bunun yerine, temel yapılandırma ikinci sunucusunda oluşturmak ve herhangi bir özel kural sunucular arasında taşımak için PowerShell komut dosyaları oluşturmak için eşitleme kuralı Düzenleyicisi'ni kullanmak için Sihirbazı'nı kullanmanız gerekir. Bkz: [çarpma geçiş](active-directory-aadconnect-upgrade-previous-version.md#swing-migration).

**S: Azure oturum açma sayfası için parolaları önbelleğe ve bir parola input öğesi otomatik tamamlama ile içerdiğinden bu engellenebilir = "false" özniteliği?**</br>
Şu anda parola giriş alanını otomatik tamamlama etiketi de dahil olmak üzere, HTML özniteliklerini değiştirme desteklemiyoruz. Şu anda herhangi bir öznitelik parola alanına eklemenize olanak tanıyan özel Javascript için izin veren bir özellik üzerinde çalışıyoruz. Bu 2017 kullanılabilir sonraki parçası olmalıdır.

**S: Azure oturum açma sayfasında, daha önce başarıyla oturum açtıktan kullanıcılar için kullanıcı adları gösterilir.  Bu davranış kapalı?**</br>
Şu anda oturum açma sayfasının HTML özniteliklerini değiştirme desteklemiyoruz. Şu anda herhangi bir öznitelik parola alanına eklemenize olanak tanıyan özel Javascript için izin veren bir özellik üzerinde çalışıyoruz. Bu 2017 kullanılabilir sonraki parçası olmalıdır.

**S: eşzamanlı oturum önlemek için bir yol var mı?**</br>
Hayır.

## <a name="troubleshooting"></a>Sorun giderme
**S: Azure AD Connect ile ilgili Yardım nasıl alabilirim?**

[Microsoft Bilgi Bankası'nda (KB) arama](https://www.microsoft.com/en-us/Search/result.aspx?q=azure%20active%20directory%20connect&form=mssupport)

* Ortak onarım sorunları Azure AD Connect desteği hakkında teknik çözümleri için Microsoft Bilgi Bankası (KB) arayın.

[Microsoft Azure Active Directory Forums](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD)

* Arama ve teknik sorular ve yanıtlar topluluktan veya kendi tıklayarak soru sorun göz atmak [burada](https://social.msdn.microsoft.com/Forums/azure/en-US/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required).

[Azure AD için destek alma](https://docs.microsoft.com/azure/active-directory/active-directory-troubleshooting-support-howto)

* Azure portalı üzerinden destek almak için bu bağlantıyı kullanın.

