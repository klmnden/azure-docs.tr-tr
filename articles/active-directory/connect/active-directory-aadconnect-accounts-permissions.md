---
title: 'Azure AD Connect: Hesapları ve izinleri | Microsoft Docs'
description: Bu konuda kullanılan ve oluşturulan hesapları ve gereken izinler açıklanmaktadır.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.reviewer: cychua
ms.assetid: b93e595b-354a-479d-85ec-a95553dd9cc2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: bde8e68eeb63e76a0dde40a09eededde8a545a83
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34595096"
---
# <a name="azure-ad-connect-accounts-and-permissions"></a>Azure AD Connect: Hesapları ve izinleri
Azure AD Connect Yükleme Sihirbazı'nı iki farklı yollarını sunar:

* Hızlı ayarlar sihirbazın daha fazla ayrıcalıkları gerektirir.  Bu, böylelikle kullanıcılar oluşturmak veya izinlerini yapılandırmak gerek kalmadan yapılandırmanızı kolayca ayarlayabilirsiniz.
* Özel ayarlar, sihirbazın daha fazla seçenek ve seçenekleri sunar. Ancak, kendiniz doğru izinlere sahip olmak gereken bazı durumlar vardır.

## <a name="related-documentation"></a>İlgili belgeler
Belgeleri okuyun değil, [şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](../active-directory-aadconnect.md), aşağıdaki tabloda ilgili konulara bağlantılar sağlar.

|Konu |Bağlantı|  
| --- | --- |
|Azure AD Connect'i indirme | [Azure AD Connect’i indirme](http://go.microsoft.com/fwlink/?LinkId=615771)|
|Hızlı ayarları kullanarak yükleme | [Azure AD Connect’i hızlı yükleme](./active-directory-aadconnect-get-started-express.md)|
|Özelleştirilmiş ayarları kullanarak yükleme | [Azure AD Connect özel yüklemesi](./active-directory-aadconnect-get-started-custom.md)|
|DirSync'ten yükseltme | [Azure AD eşitleme aracından (DirSync) yükseltme](./active-directory-aadconnect-dirsync-upgrade-get-started.md)|
|Yükleme işleminden sonra | [Yüklemeyi doğrulama ve lisansları atama](active-directory-aadconnect-whats-next.md)|

## <a name="express-settings-installation"></a>Hızlı ayarları yükleme
Hızlı Ayarları'nda, Yükleme Sihirbazı'nı AD DS kuruluş yöneticisi kimlik bilgilerini ister.  Şirket içi Active Directory'nizi Azure AD Connect için gerekli izinlere sahip yapılandırılabilir şekilde budur. Dirsync'ten yükseltme yapıyorsanız, AD DS Enterprise Admins kimlik bilgileri DirSync tarafından kullanılan hesabın parolasını sıfırlamak için kullanılır. Ayrıca Azure AD genel yönetici kimlik bilgileri gerekir.

| Sihirbaz sayfası | Toplanan kimlik bilgileri | Gerekli izinler | İçin kullanılır |
| --- | --- | --- | --- |
| Yok |Yükleme Sihirbazı'nı çalıştıran kullanıcının |Yerel Sunucu Yöneticisi |<li>Olarak kullanılan yerel hesap oluşturan [eşitleme altyapısı hizmet hesabı](#azure-ad-connect-sync-service-account). |
| Azure AD'ye Bağlanma |Azure AD directory kimlik bilgileri |Azure AD genel Yönetici rolüne |<li>Azure AD dizini eşitleme etkinleştiriliyor.</li>  <li>Oluşturulmasını [Azure AD hesabının](#azure-ad-service-account) kullanılan devam eden eşitleme işlemleri için Azure AD içinde.</li> |
| AD DS'ye Bağlanma |Şirket içi Active Directory kimlik bilgileri |Active Directory'de Enterprise Admins (EA) grubunun üyesi |<li>Oluşturur bir [hesap](#active-directory-account) Active Directory ve ona izinleri verir. Bu hesabı oluşturuldu okumak ve eşitleme sırasında dizin bilgilerini yazmak için kullanılır.</li> |

### <a name="enterprise-admin-credentials"></a>Kuruluş Yöneticisi kimlik bilgileri
Bu kimlik bilgileri yalnızca yükleme sırasında kullanılır ve yükleme tamamlandıktan sonra kullanılmaz. Kuruluş yöneticisi etki alanı yöneticisi tüm etki alanlarında Active Directory izinlerini ayarlayabilirsiniz emin olmanız gerekir.

### <a name="global-admin-credentials"></a>Genel yönetici kimlik bilgileri
Bu kimlik bilgileri yalnızca yükleme sırasında kullanılır ve yükleme tamamlandıktan sonra kullanılmaz. Oluşturmak için kullanılan [Azure AD hesabının](#azure-ad-service-account) değişiklikleri Azure ad ile eşitlemek için kullanılır. Hesabı, Azure AD içinde bir özellik olarak eşitleme de sağlar.

### <a name="permissions-for-the-created-ad-ds-account-for-express-settings"></a>Hızlı ayarlar için oluşturulan AD DS izinlerini hesap
[Hesap](#active-directory-account) okuma ve yazma AD DS tarafından hızlı ayarları oluşturduğunuzda aşağıdaki izinlere sahip olmanız için oluşturulan:

| İzin | İçin kullanılır |
| --- | --- |
| <li>Dizin Değişikliklerini Çoğaltma</li><li>Tüm çoğaltma dizini değiştirir |Parola karma eşitlemesi |
| Okuma/yazma tüm özellikleri kullanıcı |İçeri aktarma ve Exchange karma |
| Okuma/yazma tüm özellikleri iNetOrgPerson |İçeri aktarma ve Exchange karma |
| Tüm özellikleri Grup okuma/yazma |İçeri aktarma ve Exchange karma |
| Tüm özellikleri okuma/yazma başvurun |İçeri aktarma ve Exchange karma |
| Parola sıfırlama |Parola geri yazma özelliğini etkinleştirmek için hazırlama |

## <a name="custom-settings-installation"></a>Özel ayarlarını yükleme
Azure AD Connect sürüm 1.1.524.0 ve daha sonra Azure AD Connect Sihirbazı'nı Active Directory'ye bağlamak için kullanılan hesabı oluşturmak için seçeneği vardır.  Önceki sürümlerde hesap yüklemeden önce oluşturulduktan gerektirir. Bu hesap vermelidir izinleri bulunabilir [AD DS hesabı oluşturma](#create-the-ad-ds-account). 

| Sihirbaz sayfası | Toplanan kimlik bilgileri | Gerekli izinler | İçin kullanılır |
| --- | --- | --- | --- |
| Yok |Yükleme Sihirbazı'nı çalıştıran kullanıcının |<li>Yerel Sunucu Yöneticisi</li><li>Tam SQL Server kullanıyorsanız, kullanıcının SQL'de Sistem Yöneticisi (SA) olması gerekir</li> |Varsayılan olarak kullanılan yerel hesap oluşturan [eşitleme altyapısı hizmet hesabı](#azure-ad-connect-sync-service-account). Belirli bir hesabı yönetici belirtmediğinde hesabı yalnızca oluşturulur. |
| Eşitleme hizmetlerini, hizmet hesabı seçeneği yükleme |AD veya yerel kullanıcı hesabı kimlik bilgileri |Kullanıcı, Yükleme Sihirbazı tarafından izin verilir |Yönetici hesabınız belirtiyorsa, bu hesap eşitleme hizmeti için hizmet hesabı olarak kullanılır. |
| Azure AD'ye Bağlanma |Azure AD directory kimlik bilgileri |Azure AD genel Yönetici rolüne |<li>Azure AD dizini eşitleme etkinleştiriliyor.</li>  <li>Oluşturulmasını [Azure AD hesabının](#azure-ad-service-account) kullanılan devam eden eşitleme işlemleri için Azure AD içinde.</li> |
| Dizinlerinizi bağlama |Azure AD ile bağlı her bir orman için şirket içi Active Directory kimlik bilgileri |İzinler hangi özellikleri etkinleştirmek ve bulunabilir bağlıdır [AD DS hesabı oluşturma](#create-the-ad-ds-account) |Bu hesap, okumak ve eşitleme sırasında dizin bilgilerini yazmak için kullanılır. |
| AD FS Sunucuları |Sihirbazı'nı çalıştıran kullanıcının oturum açma kimlik bilgilerini bağlanmak için yeterli zaman listedeki her sunucu için sihirbaz kimlik bilgilerini toplar. |Etki alanı yöneticisi |Yükleme ve AD FS sunucusu rolü yapılandırması. |
| Web uygulaması proxy sunucuları |Sihirbazı'nı çalıştıran kullanıcının oturum açma kimlik bilgilerini bağlanmak için yeterli zaman listedeki her sunucu için sihirbaz kimlik bilgilerini toplar. |Hedef makinede yerel yönetici |Yükleme ve yapılandırma WAP sunucu rolünün. |
| Ara sunucu güveni kimlik bilgileri |Federasyon Hizmeti'ne güvenen kimlik bilgilerini (proxy FS güven sertifikadan kaydetmek için kullandığı kimlik bilgileri |AD FS sunucusunun yerel yönetici olan etki alanı hesabı |İlk kayıttan FS WAP güven sertifikası. |
| "Bir etki alanı kullanıcı hesabı seçeneğini kullan" AD FS hizmet hesabı sayfası |AD kullanıcı hesabı kimlik bilgileri |Etki alanı kullanıcısı |Kimlik bilgilerini sağlanan AD kullanıcı hesabı AD FS hizmeti oturum açma hesabı olarak kullanılır. |

### <a name="create-the-ad-ds-account"></a>AD DS hesabı oluşturma
Belirttiğiniz şirket hesabının **dizinlerinizi bağlama** sayfa yükleme önce Active Directory'de mevcut olmalıdır.  Ayrıca verilen gerekli izinlere sahip olmalıdır. Yükleme Sihirbazı'nı, izinler ve herhangi bir sorun yalnızca eşitleme sırasında bulunan doğrulamaz.

İhtiyaç duyduğunuz hangi izinlerin bağlıdır isteğe bağlı özellikleri sağlar. Birden çok etki alanı varsa, ormandaki tüm etki alanları için izinleri verilmelidir. Bunlardan birine etkinleştirmezseniz özellikleri, varsayılan **etki alanı kullanıcısı** izinleri yeterli olur.

| Özellik | İzinler |
| --- | --- |
| msDS-ConsistencyGuid özelliği |Kısmında belgelenen msDS-ConsistencyGuid özniteliğine yazma izinleri [tasarım kavramları - sourceAnchor msDS-ConsistencyGuid kullanarak](active-directory-aadconnect-design-concepts.md#using-msds-consistencyguid-as-sourceanchor). | 
| Parola karma eşitlemesi |<li>Dizin Değişikliklerini Çoğaltma</li>  <li>Tüm çoğaltma dizini değiştirir |
| Exchange karma dağıtımı |İçinde belirtilen öznitelikler yazma izinleri [Exchange karma geri yazma](active-directory-aadconnectsync-attributes-synchronized.md#exchange-hybrid-writeback) kullanıcılar, gruplar ve kişiler için. |
| Exchange posta ortak klasörü |İçinde belirtilen öznitelikler için Okuma izinleri [Exchange posta ortak klasör](active-directory-aadconnectsync-attributes-synchronized.md#exchange-mail-public-folder) ortak klasörleri için. | 
| Parola geri yazma |İçinde belirtilen öznitelikler yazma izinleri [parola yönetimine Başlarken](../authentication/howto-sspr-writeback.md) kullanıcılar için. |
| Cihaz geri yazma |Bölümünde açıklandığı gibi bir PowerShell Betiği ile verilen izinler [cihaz geri yazma](active-directory-aadconnect-feature-device-writeback.md). |
| Grup geri yazma |Okuma, grup oluşturma, güncelleştirme ve silme eşitlenmesi için nesneleri **Office 365 grupları**.  Daha fazla bilgi için bkz: [grup geri yazma](active-directory-aadconnect-feature-preview.md#group-writeback).|

## <a name="upgrade"></a>Yükseltme
Bir Azure AD Connect sürümünden yeni sürüme yükselttiğinizde, aşağıdaki izinler gerekir:

>[!IMPORTANT]
>Yapı 1.1.484 ile başlayarak, Azure AD Connect SQL veritabanını yükseltmek için sysadmin izinleri gerektiren bir regresyon hata sunmuştur.  Bu hata, yapı 1.1.647 giderilmiştir.  Bu yapı yükseltiyorsanız, sysadmin izinleri gerekir.  Dbo izinleri yeterli değil.  Azure AD Connect sysadmin izinleri olmadan yükseltme çalışırsanız, yükseltme başarısız olur ve Azure AD Connect artık doğru daha sonra çalışmaz.  Microsoft, bunun farkında olduğundan ve bu sorunu gidermek için çalışma.


| Sorumlu | Gerekli izinler | İçin kullanılır |
| --- | --- | --- |
| Yükleme Sihirbazı'nı çalıştıran kullanıcının |Yerel Sunucu Yöneticisi |İkili dosyaları güncelleştirin. |
| Yükleme Sihirbazı'nı çalıştıran kullanıcının |ADSyncAdmins üyesi |Eşitleme kuralları ve diğer yapılandırma değişiklikleri yapın. |
| Yükleme Sihirbazı'nı çalıştıran kullanıcının |Tam bir SQL server kullanıyorsanız: DBO (veya benzeri) eşitleme altyapısı veritabanının |Yeni sütunlarla tablo güncelleştirme gibi veritabanı düzeyi değişiklikleri yapın. |

## <a name="more-about-the-created-accounts"></a>Oluşturulan hesaplar hakkında daha fazla bilgi
### <a name="active-directory-account"></a>Active Directory hesabı
Hızlı ayarları kullanırsanız, Active Directory'de eşitleme için kullanılan bir hesabı oluşturulur. Oluşturulan hesap orman kök etki alanı Kullanıcılar kapsayıcısı içinde bulunur ve adı önekine sahip olan **MSOL_**. Hesap dolmayan uzun karmaşık bir parola ile oluşturulur. Etki alanınızda bir parola ilkesi varsa, uzun olduğundan emin olun ve karmaşık parolalar bu hesap için izin verilir.

![AD hesabı](./media/active-directory-aadconnect-accounts-permissions/adsyncserviceaccount.png)

Özel ayarları kullanırsanız, daha sonra yüklemeye başlamadan önce hesabı oluşturmak için sorumludur.

### <a name="azure-ad-connect-sync-service-account"></a>Azure AD Connect eşitleme hizmeti hesabı
Eşitleme hizmeti farklı hesaplar altında çalıştırabilirsiniz. Altında çalıştırabilirsiniz bir **sanal hizmet hesabı** (VSA) bir **Grup yönetilen hizmet hesabı** (gMSA/sMSA) veya normal bir kullanıcı hesabı. Desteklenen seçenekler ile 2017 değiştirilmiş olan yeni bir yüklemesidir yaptığınızda Bağlan Nisan sürümü. Azure AD Connect, önceki bir sürümünden yükseltirseniz, bu ek seçenekleri kullanılamaz.

| Hesap türü | Yükleme seçeneği | Açıklama |
| --- | --- | --- |
| [Sanal hizmet hesabı](#virtual-service-account) | Express hem de özel 2017 Nisan ve sonraki sürümler | Bir etki alanı denetleyicisi yüklemelerinde dışında tüm express yüklemeleri için kullanılan seçenek budur. Başka bir seçenek kullanılmadığı sürece özel için onu varsayılan seçenektir. |
| [Grup yönetilen hizmet hesabı](#group-managed-service-account) | Özel, 2017 Nisan ve sonraki sürümler | Ardından uzak bir SQL server kullanıyorsanız, Grup yönetilen hizmet hesabı kullanılması önerilir. |
| [Kullanıcı hesabı](#user-account) | Express hem de özel 2017 Nisan ve sonraki sürümler | Bir kullanıcı hesabı ile AAD_ öneki yalnızca Windows Server 2008 ve bir etki alanı denetleyicisine yüklendiğinde yüklendiğinde yükleme sırasında oluşturulur. |
| [Kullanıcı hesabı](#user-account) | Express hem de özel 2017 Mart ve önceki sürümleri | Yerel bir hesap ile AAD_ önekli yüklemesi sırasında oluşturulur. Özel yükleme kullanırken, başka bir hesap belirtilebilir. |

Connect ile 2017 derleme Mart kullandığınız veya Windows Güvenlik nedenleriyle şifreleme anahtarları bozar beri daha önce sonra hizmet hesabı parolasını sıfırlama varsa. Azure AD Connect'i yeniden yüklemeden başka herhangi bir hesabı hesabın değiştiremezsiniz. Nisan 2017 ' bir yapıya yükseltme veya daha sonra desteklenen hizmet hesabı, ancak parolayı değiştirmek için kullanılan hesap değiştiremezsiniz.

> [!Important]
> Bu gibi durumlarda, hizmet hesabı yalnızca ilk yüklemesinde ayarlayabilirsiniz. Yükleme tamamlandıktan sonra hizmeti hesabını değiştirmek için desteklenmiyor.

Bu, varsayılan değer olan eşitleme hizmeti hesabı için önerilen ve desteklenen seçenekleri bir tablosu verilmiştir.

Gösterge:

- **Kalın** ve önerilen seçenek çoğu durumda varsayılan seçeneği gösterir.
- *İtalik* varsayılan seçenek olmadığı durumlarda önerilen seçenek gösterir.
- 2008 - Windows Server 2008'de yüklendiğinde varsayılan seçeneği
- Non-kalın - desteklenen seçeneği
- Yerel hesap - yerel kullanıcı hesabı sunucuda
- Etki alanı hesabı - etki alanı kullanıcı hesabı
- sMSA - [tek başına yönetilen hizmet hesabı](https://technet.microsoft.com/library/dd548356.aspx)
- gMSA - [Grup yönetilen hizmet hesabı](https://technet.microsoft.com/library/hh831782.aspx)

| | Yerel veritabanı</br>Express | Yerel veritabanı/LocalSQL</br>Özel | Uzak SQL</br>Özel |
| --- | --- | --- | --- |
| **tek başına/çalışma grubu makinesi** | Desteklenmiyor | **VSA**</br>Yerel hesap (2008)</br>Yerel hesap |  Desteklenmiyor |
| **Makine etki alanına katılmış** | **VSA**</br>Yerel hesap (2008) | **VSA**</br>Yerel hesap (2008)</br>Yerel hesap</br>Etki alanı hesabı</br>smsa'yı, gMSA | **gMSA**</br>Etki alanı hesabı |
| **Etki alanı denetleyicisi** | **Etki alanı hesabı** | *gMSA*</br>**Etki alanı hesabı**</br>sMSA| *gMSA*</br>**Etki alanı hesabı**|

#### <a name="virtual-service-account"></a>Sanal hizmet hesabı
Bir sanal hizmet hesabı, bir parola sahip değil ve Windows tarafından yönetilen hesap özel türüdür.

![VSA](./media/active-directory-aadconnect-accounts-permissions/aadsyncvsa.png)

VSA eşitleme altyapısı ve SQL aynı sunucu üzerinde olduğu senaryolar ile kullanılmak üzere tasarlanmıştır. Uzak SQL kullanın, sonra kullanılması önerilir, bir [Grup yönetilen hizmet hesabı](#managed-service-account) yerine.

Bu özellik, Windows Server 2008 R2 gerektirir veya sonraki bir sürümü. Windows Server 2008 üzerinde Azure AD Connect'i yüklemek sonra yükleme kullanmaya geri döner bir [kullanıcı hesabı](#user-account) yerine.

#### <a name="group-managed-service-account"></a>Grup yönetilen hizmet hesabı
Uzak bir SQL server kullanmak sonra kullanmaya öneririz bir **Grup yönetilen hizmet hesabı**. Grup yönetilen hizmet hesabı, Active Directory hazırlama hakkında daha fazla bilgi için bkz: [Grup yönetilen hizmet hesaplarına genel bakış](https://technet.microsoft.com/library/hh831782.aspx).

Bu seçeneği kullanmanız [gerekli bileşenleri yüklemeniz](active-directory-aadconnect-get-started-custom.md#install-required-components) sayfasında, **varolan hizmet hesabını kullan**seçip **yönetilen hizmet hesabı**.  
![VSA](./media/active-directory-aadconnect-accounts-permissions/serviceaccount.png)  
Kullanmak için de desteklenen bir [tek başına yönetilen hizmet hesabı](https://technet.microsoft.com/library/dd548356.aspx). Ancak, bunlar yalnızca yerel makinede kullanılabilir ve varsayılan sanal hizmet hesabı kullanılacak fayda yok.

Bu özellik, Windows Server 2012 veya sonraki sürümünü gerektirir. Daha eski bir işletim sistemi ve uzak SQL kullanmak ihtiyacınız varsa kullanmalısınız bir [kullanıcı hesabı](#user-account).

#### <a name="user-account"></a>Kullanıcı hesabı
Bir yerel hizmet hesabı (hesabını özel ayarları kullanacak şekilde belirtmediğiniz sürece) Yükleme Sihirbazı tarafından oluşturulur. Hesap önekli **AAD_** ve çalıştırmak için gerçek eşitleme hizmeti için kullanılır. Azure AD Connect etki alanı denetleyicisine yüklerseniz, hesap etki alanında oluşturulur. **AAD_** hizmet hesabı, etki alanında bulunmalıdır:
   - SQL server çalıştıran uzak bir sunucu kullanın
   - kimlik doğrulaması gerektiren bir proxy kullanın

![Eşitleme hizmeti hesabı](./media/active-directory-aadconnect-accounts-permissions/syncserviceaccount.png)

Hesap dolmayan uzun karmaşık bir parola ile oluşturulur.

Bu hesap, diğer hesaplar için parolaları güvenli bir şekilde depolamak için kullanılır. Bu diğer hesapları parolaların veritabanında şifrelenmiş olarak depolanır. Özel anahtarları şifreleme anahtarları için Windows Data Protection API (DPAPI) kullanarak şifreleme hizmetleri gizli anahtar şifreleme ile korunur.

Tam bir SQL Server kullanıyorsanız, hizmet hesabı eşitleme altyapısı için oluşturulan veritabanı DBO değil. Hizmet, diğer herhangi bir izinle beklendiği gibi çalışmayacaktır. Bir SQL oturum açma da oluşturulur.

Hesabın ayrıca dosyaları, kayıt defteri anahtarları ve eşitleme altyapısı ilgili diğer nesneler için izinleri verilir.

### <a name="azure-ad-service-account"></a>Azure AD hizmet hesabı
Bir hesap Azure AD'de eşitleme hizmetinin kullanım için oluşturulur. Bu hesap, görünen ada göre tanımlanabilir.

![AD hesabı](./media/active-directory-aadconnect-accounts-permissions/aadsyncserviceaccount.png)

Hesap kullanılır sunucunun adını, kullanıcı adı ikinci bölümünde tanımlanabilir. Aşağıdaki resimde, FABRIKAMCON sunucu adıdır. Sunucuları hazırlama varsa, her sunucu, kendi hesabı vardır.

Hizmet hesabı dolmayan uzun karmaşık bir parola ile oluşturulur. Özel bir rol verilen **Directory eşitleme hesapları** dizin eşitleme görevleri gerçekleştirmek için yalnızca izinlere sahiptir. Bu özel yerleşik rol dışında Azure AD Connect Sihirbazı verilemez. Azure portalında rol bu hesapla gösterir **kullanıcı**.

Azure AD'de 20 eşitleme hizmet hesapları bir sınırı yoktur. Azure AD içinde var olan Azure AD hizmet hesaplarının listesini almak için aşağıdaki Azure AD PowerShell cmdlet'ini çalıştırın: `Get-AzureADDirectoryRole | where {$_.DisplayName -eq "Directory Synchronization Accounts"} | Get-AzureADDirectoryRoleMember`

Kullanılmayan Azure AD kaldırmak için hizmet hesapları, aşağıdaki Azure AD PowerShell cmdlet'ini çalıştırın: `Remove-AzureADUser -ObjectId <ObjectId-of-the-account-you-wish-to-remove>`

## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](../active-directory-aadconnect.md) hakkında daha fazla bilgi edinin.
