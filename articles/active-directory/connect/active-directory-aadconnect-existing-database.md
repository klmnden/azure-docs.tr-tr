---
title: "Var olan bir ADSync veritabanı kullanarak Azure AD Connect yükleme | Microsoft Docs"
description: "Bu konuda, var olan bir ADSync veritabanı kullanmayı açıklar."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.reviewer: cychua
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/30/2017
ms.author: billmath
ms.openlocfilehash: 61652d97429336dad23ba14f7349e27bf52d33d7
ms.sourcegitcommit: ce934aca02072bdd2ec8d01dcbdca39134436359
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/08/2017
---
# <a name="install-azure-ad-connect-using-an-existing-adsync-database"></a>Var olan bir ADSync veritabanı kullanarak Azure AD Connect'i yükleme
Azure AD Connect verileri depolamak için bir SQL Server veritabanı gerektirir. Varsayılan olarak Azure AD Connect ile SQL Server 2012 Express LocalDB yüklenen kullanabilir veya kendi SQL tam sürümünü kullanın. Daha önce Azure AD Connect yüklendiğinde ADSync adlı yeni bir veritabanı her zaman oluşturuldu. Azure AD Connect sürüm 1.1.613.0 (veya sonra), varolan bir ADSync veritabanına işaret ederek Azure AD Connect yükleme seçeneğiniz vardır.

## <a name="benefits-of-using-an-existing-adsync-database"></a>Var olan bir ADSync veritabanı kullanmanın yararları
Varolan bir ADSync veritabanına göstererek:

- Kimlik bilgileri dışında (özel eşitleme kuralları, bağlayıcılar, filtreleme ve isteğe bağlı özellikleri yapılandırma gibi) ADSync veritabanında depolanan eşitleme yapılandırması otomatik olarak kurtarıldı ve yükleme sırasında kullanılan . Değişiklikleri eşitlemek için Azure AD Connect tarafından kullanılan kimlik bilgileri ile şirket içi AD ve Azure AD şifrelenir ve yalnızca önceki Azure AD Connect sunucusu tarafından erişilebilir.
- Tüm kimlik verilerini (Bağlayıcı alanları ve meta veri deposu ile ilişkili) ve ADSync veritabanında depolanan eşitleme tanımlama bilgilerini ayrıca kurtarıldı. Sunucu burada önceki Azure AD Connect sunucusu kapalı, tam eşitleme gerçekleştirmek için gereken sahip olmak yerine sol gelen eşitlemeye devam edebilirsiniz yeni yüklenen Azure AD Connect.

## <a name="scenarios-where-using-an-existing-adsync-database-is-beneficial"></a>Varolan bir ADSync veritabanı kullanımı faydalı olduğu senaryolar
Aşağıdaki senaryolarda yararlı yararlar şunlardır:


- Var olan bir Azure AD Connect dağıtıma sahip. Var olan Azure AD Connect sunucunuz artık çalışıyor ancak ADSync veritabanını içeren SQL server hala çalışıyor. Yeni bir Azure AD Connect sunucusu yüklemek ve var olan ADSync veritabanının üzerine getirin. 
- Var olan bir Azure AD Connect dağıtıma sahip. ADSync veritabanını içeren SQL server'ınızdaki artık çalışmıyor. Ancak, en son veritabanı sahip yedekleyin. Önce yeni bir SQL server ADSync veritabanı geri yükleyebilirsiniz. Sonra yeni bir Azure AD Connect sunucusu yüklemek ve geri yüklenen ADSync veritabanının üzerine getirin.
- Yerel veritabanı kullanarak var olan bir Azure AD Connect dağıtım var. Yerel veritabanı tarafından uygulanan 10GB sınırını nedeniyle, tam SQL geçirmek istediğiniz. LocalDB ADSync veritabanını yedekleyin ve bir SQL Server'a geri yükleyin. Sonra yeni bir Azure AD Connect sunucusu yükleyin ve geri yüklenen ADSync veritabanının üzerine getirin.
- Hazırlama sunucusu kurmak çalışıyorsunuz ve geçerli etkin sunucunun yapılandırmasıyla eşleşen emin olmak istiyor. ADSync veritabanını yedekleyin ve başka bir SQL Server'a geri yükleyebilirsiniz. Sonra yeni bir Azure AD Connect sunucusu yükleyin ve geri yüklenen ADSync veritabanının üzerine getirin.

## <a name="prerequisite-information"></a>Önkoşul bilgileri

Devam etmeden önce olabilmesi için önemli notlar dikkat edin:

- Donanım ve önkoşullar ve hesap ve Azure AD Connect'i yüklemek için gereken izinler Azure AD Connect'i yüklemeye yönelik önkoşulları gözden geçirdiğinizden emin olun. "Varolan veritabanını kullan" modu kullanarak Azure AD Connect'i yüklemek için gerekli izinleri "özel" yükleme ile aynı olur.
- Azure AD Connect varolan ADSync karşı dağıtma veritabanı yalnızca tam SQL ile desteklenir. SQL Express LocalDB ile desteklenmiyor. Kullanmak istediğiniz yerel veritabanı var olan bir ADSync veritabanı varsa, ilk (LocalDB) ADSync veritabanı yedekleme ve tam SQL geri yüklemelisiniz. Sonrasında, Azure AD Connect bu yöntemi kullanarak geri yüklenen veritabanı karşı dağıtabilirsiniz.
- Yükleme için kullanılan Azure AD Connect sürümü aşağıdaki ölçütleri karşılamalıdır:
    - 1.1.613.0 veya üstü ve
    - Aynı veya son ADSync veritabanı ile kullanılan Azure AD Connect sürümünden daha yüksek. Yükleme için kullanılan Azure AD Connect sürümü son ADSync veritabanı ile kullanılan sürümden daha yüksek ise, bir tam eşitleme gerekli olabilir.  İki sürümü arasında şema veya eşitleme kuralı değişiklikler yapıldıysa, tam eşitleme gereklidir. 
- Kullanılan ADSync veritabanı, nispeten yeni bir eşitleme durumu içermelidir. Son eşitleme etkinliği varolan ADSync veritabanı ile son üç hafta içinde olmalıdır.
- "Varolan veritabanını kullan" yöntemi kullanarak Azure AD Connect'i yükleme sırasında oturum açma yöntemi önceki Azure AD Connect sunucusunda yapılandırılan korunmaz. Ayrıca, yükleme sırasında oturum açma yöntemi yapılandıramazsınız. Yükleme tamamlandıktan sonra yalnızca oturum açma yöntemi yapılandırabilirsiniz.
- Aynı ADSync veritabanını paylaşan birden çok Azure AD Connect sunucusu olamaz. "Varolan veritabanını kullan" yöntemi, yeni bir Azure AD Connect sunucusu var olan bir ADSync veritabanı yeniden kullanmanıza olanak sağlar. Paylaşımı desteklemez.

## <a name="steps-to-install-azure-ad-connect-with-use-existing-database-mode"></a>"Varolan veritabanını kullan" modu ile Azure AD Connect'i yüklemek için adımları
1.  Azure AD Connect yükleyicisini (AzureADConnect.MSI) Windows server için indirin. Azure AD Connect'i yüklemeye başlamak için Azure AD Connect yükleyicisini çift tıklatın.
2.  MSI yükleme işlemi tamamlandıktan sonra Azure AD Connect Sihirbazı ile hızlı mod Kurulum başlatır. Ekran çıkış simgesini tıklatarak kapatın.
![Hoş Geldiniz](media/active-directory-aadconnect-existing-database/db1.png)
3.  Yeni komut istemi veya PowerShell oturumu başlatın. Klasöre gidin <drive>\Program Azure AD Connect. Azure AD Connect Sihirbazı "Varolan veritabanını kullan" Kurulum modunda başlatmak için komut.\AzureADConnect.exe /useexistingdatabase çalıştırın.
![PowerShell](media/active-directory-aadconnect-existing-database/db2.png)
4.  Hoş Geldiniz ekranında Azure AD Connect ile greeted. Lisans koşullarını ve gizlilik bildirimini kabul sonra tıklayın **devam**.
![Hoş Geldiniz](media/active-directory-aadconnect-existing-database/db3.png)
5.  Üzerinde **gerekli bileşenleri yüklemeniz** ekranında **mevcut bir SQL Server kullanmak** seçeneği etkinleşir. ADSync veritabanını barındıran SQL server adını belirtin. ADSync veritabanı barındırmak için kullanılan SQL altyapısı örneği SQL Server'daki varsayılan örnek değilse, SQL altyapısı örneği adı belirtmeniz gerekir. Ayrıca, SQL gözatma etkin değilse, SQL altyapısı örnek bağlantı noktası numarasını da belirtmeniz gerekir. Örneğin:         
![Hoş Geldiniz](media/active-directory-aadconnect-existing-database/db4.png)           

6.  Üzerinde **Azure ad Connect** ekran, Azure AD dizininizin genel yönetici kimlik bilgilerini sağlamanız gerekir. Varsayılan onmicrosoft.com etki alanında bir hesabı kullanmanız önerilir. Bu hesap yalnızca Azure AD'de hizmet hesabı oluşturmak için kullanılır ve sihirbaz tamamlandıktan sonra kullanılmaz.
![Bağlanma](media/active-directory-aadconnect-existing-database/db5.png)
 
7.  Üzerinde **dizinlerinizi bağlama** ekran, dizin eşitleme yanında kırmızı çarpı simgesi ile listelenen için yapılandırılmış mevcut AD orman. Bir şirket içi değişiklikleri eşitlemek için AD ormanı, bir AD DS hesabı gereklidir. Azure AD Connect Sihirbazı çünkü kimlik bilgilerini şifrelenir ve yalnızca önceki Azure AD Connect sunucusu tarafından çözülecek şekilde ADSync veritabanında depolanan AD DS hesabının kimlik bilgilerini alamıyor. Tıklatın **değişiklik kimlik** AD ormanı için AD DS hesabı belirtmek için.
![Dizinleri](media/active-directory-aadconnect-existing-database/db6.png)
 
 
8.  Açılan iletişim kutusunda, aşağıdakilerden birini yapabilirsiniz (i) bir kuruluş yöneticisi kimlik bilgilerini girin ve AD DS hesabı oluşturduğunuzda Azure AD Connect izin veya (II) AD DS hesabı kendiniz oluşturmanız ve Azure AD Connect'e kendi kimlik bilgilerini belirtin. Bir seçenek seçtiyseniz ve gerekli kimlik bilgilerini sağlamak sonra tıklayın **Tamam** açılır iletişim kutusunu kapatmak için.
![Hoş Geldiniz](media/active-directory-aadconnect-existing-database/db7.png)
 
 
9.  Sağlanan kimlik bilgileri sonra kırmızı çarpı simgesi yeşil onay simgesi ile değiştirilir. **İleri**’ye tıklayın.
![Hoş Geldiniz](media/active-directory-aadconnect-existing-database/db8.png)
 
 
10. Üzerinde **yapılandırma için hazır** ekranında **yükleme**.
![Hoş Geldiniz](media/active-directory-aadconnect-existing-database/db9.png)
 
 
11. Yükleme tamamlandıktan sonra Azure AD Connect sunucusu hazırlama modu için otomatik olarak etkinleştirilir. Sunucu yapılandırmasını gözden geçirmeniz önerilir ve hazırlama modunu devre dışı bırakmadan önce dışarı aktarmalar beklenmeyen değişiklikler için bekleniyor. 

## <a name="next-steps"></a>Sonraki adımlar

- Azure AD Connect'i yüklediniz, artık [yüklemeyi doğrulayabilir ve lisans atayabilirsiniz](active-directory-aadconnect-whats-next.md).
- Yüklemeyle etkinleştirilen özellikler hakkında daha fazla bilgi edinin: [Yanlışlıkla silmeleri engelleme](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md) ve [Azure AD Connect Health](../connect-health/active-directory-aadconnect-health-sync.md).
- Şu genel konu başlıkları hakkında daha fazla bilgi edinin: [Zamanlayıcı ve eşitleme tetikleme](active-directory-aadconnectsync-feature-scheduler.md).
- [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md) hakkında daha fazla bilgi edinin.
