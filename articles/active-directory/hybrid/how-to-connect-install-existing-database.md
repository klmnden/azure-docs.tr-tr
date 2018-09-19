---
title: Var olan bir ad eşitleme veritabanını kullanarak Azure AD Connect'i yükleme | Microsoft Docs
description: Bu konu, mevcut bir ad eşitleme veritabanını kullanmayı açıklar.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.reviewer: cychua
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/30/2017
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 36db41308678f3f1bd713561f9a844288f5db401
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2018
ms.locfileid: "46312245"
---
# <a name="install-azure-ad-connect-using-an-existing-adsync-database"></a>Mevcut bir ad eşitleme veritabanını kullanarak Azure AD Connect'i yükleme
Azure AD Connect, verileri depolamak için SQL Server veritabanı gerektirir. SQL Server 2012 Express LocalDB Azure AD Connect ile yüklenen varsayılan kullanabilir veya kendi tam SQL sürümü kullanın. Daha önce Azure AD Connect yüklendiğinde, her zaman ADSync adlı yeni bir veritabanı oluşturuldu. Azure AD Connect sürümü 1.1.613.0 (veya sonra), mevcut bir ad eşitleme veritabanına işaret ederek Azure AD Connect'i yükleme seçeneğiniz vardır.

## <a name="benefits-of-using-an-existing-adsync-database"></a>Var olan bir ad eşitleme veritabanını kullanmanın avantajları
Mevcut bir ad eşitleme veritabanına işaret ederek:

- Kimlik bilgileri dışında (özel eşitleme kuralları, bağlayıcı, filtreleme ve isteğe bağlı özellikleri yapılandırma dahil) ADSync veritabanında depolanan eşitleme yapılandırması otomatik olarak kurtarıldı ve yükleme sırasında kullanılan . Değişiklikleri eşitlemek için Azure AD Connect tarafından kullanılan kimlik bilgileri ile şirket içi AD ile Azure AD şifrelenir ve yalnızca önceki Azure AD Connect sunucusu tarafından erişilebilir.
- Tüm kimlik verilerini (Bağlayıcı alanları ve meta veri deposu ile ilişkili) ve eşitleme tanımlama bilgisi ADSync veritabanında depolanan da kurtarılabilir. Sunucu burada önceki Azure AD Connect sunucusu kapalı, tam eşitleme gerçekleştirmek için gereken sahip olmak yerine sol gelen eşitlemeye devam edebilirsiniz yeni yüklenen Azure AD Connect.

## <a name="scenarios-where-using-an-existing-adsync-database-is-beneficial"></a>Mevcut bir ad eşitleme veritabanını kullanarak faydalı olduğu senaryolar
Bu avantajlar, aşağıdaki senaryolarda kullanışlıdır:


- Mevcut bir Azure AD Connect dağıtımım var. Mevcut Azure AD Connect sunucunuzu artık çalışıyor ancak ADSync veritabanını içeren SQL server'ın hala çalışıyor. Yeni bir Azure AD Connect sunucusu yüklemek ve var olan ad eşitleme veritabanına işaret. 
- Mevcut bir Azure AD Connect dağıtımım var. Ad eşitleme veritabanını içeren SQL server artık çalışmıyor. Ancak yeni bir veritabanı vardır ve yedekleyin. Önce yeni bir SQL Server'a ADSync veritabanını geri yükleyebilirsiniz. Sonra yeni bir Azure AD Connect sunucusu yükleyin ve geri yüklenen ad eşitleme veritabanına işaret.
- LocalDB kullanarak mevcut bir Azure AD Connect dağıtımının var. LocalDB tarafından uygulanan 10GB sınırı nedeniyle tam SQL'e geçirme ister misiniz? Localdb'den ADSync veritabanını yedekleme ve bir SQL Server'a geri yükleyebilirsiniz. Sonra yeni bir Azure AD Connect sunucusu yeniden yükleyin ve geri yüklenen ad eşitleme veritabanına işaret.
- Bir hazırlık sunucusu kurmak çalışıyor ve geçerli etkin sunucunun yapılandırmasıyla eşleşen emin olmak ister. Ad eşitleme veritabanını yedekleyin ve başka bir SQL Server'a geri yükleyebilirsiniz. Sonra yeni bir Azure AD Connect sunucusu yeniden yükleyin ve geri yüklenen ad eşitleme veritabanına işaret.

## <a name="prerequisite-information"></a>Önkoşul bilgileri

Devam etmeden önce gerçekleştirilecek önemli notlar dikkat edin:

- Donanım ve önkoşullar ve hesabı ve Azure AD Connect'i yüklemek için gereken izinleri Azure AD Connect'i yüklemeye yönelik önkoşulları gözden geçirdiğinizden emin olun. "Varolan veritabanını kullan" modu kullanarak Azure AD Connect'i yüklemek için gerekli izinler "özel" yükleme ile aynı olur.
- Azure AD Connect var olan bir ADSync karşı dağıtma veritabanı yalnızca tam SQL ile desteklenir. SQL Express LocalDB ile desteklenmiyor. Kullanmak istediğiniz LocalDB içinde var olan bir ad eşitleme veritabanını varsa, ilk (LocalDB) ad eşitleme veritabanını yedekleme ve tam SQL'e geri yüklemelisiniz. Sonra Azure AD Connect bu yöntemi kullanarak geri yüklenen veritabanıyla dağıtabilirsiniz.
- Yükleme için kullanılan Azure AD Connect sürümü, aşağıdaki ölçütleri karşılaması gerekir:
    - 1.1.613.0 veya üstü ve
    - Aynı veya Azure AD Connect ile ad eşitleme veritabanını en son kullanılan sürümden daha yüksek. Yükleme için kullanılan Azure AD Connect sürümü ile ad eşitleme veritabanını en son kullanılan sürümden daha yüksek ise, bir tam eşitleme gerekli olabilir.  İki sürümü bir şema veya eşitleme kuralı değişiklikleri varsa tam eşitleme gereklidir. 
- Kullanılan ad eşitleme veritabanını nispeten yeni bir eşitleme durumu içermelidir. Var olan ad eşitleme veritabanını ile son eşitleme etkinliği, son üç hafta içinde olmalıdır.
- Azure AD Connect "Varolan veritabanını kullan" yöntemiyle yüklerken, oturum açma yöntemi önceki Azure AD Connect sunucusunda yapılandırılan korunmaz. Ayrıca, yükleme sırasında oturum açma yöntemi yapılandıramazsınız. Yükleme tamamlandıktan sonra yalnızca oturum açma yöntemi yapılandırabilirsiniz.
- Aynı ad eşitleme veritabanını paylaşan birden çok Azure AD Connect sunucuları sahip olamaz. "Varolan veritabanını kullan" yöntemi, mevcut bir ad eşitleme veritabanını yeni bir Azure AD Connect sunucusu ile yeniden olanak tanır. Paylaşım desteklemez.

## <a name="steps-to-install-azure-ad-connect-with-use-existing-database-mode"></a>"Varolan veritabanını kullan" modu ile Azure AD Connect'i yüklemek için adımları
1.  Windows server için Azure AD Connect yükleyicisini (AzureADConnect.MSI) indirin. Azure AD Connect'i yüklemeye başlamak için Azure AD Connect yükleyicisini çift tıklayın.
2.  MSI yüklemesi tamamlandıktan sonra Azure AD Connect sihirbazı Hızlı mod kurulumu açılır. Çıkış simgesine tıklayarak ekranı kapatın.
![Hoş geldiniz](./media/how-to-connect-install-existing-database/db1.png)
3.  Yeni bir komut istemi veya PowerShell oturumu başlatın. <drive>\program files\Microsoft Azure AD Connect klasörüne gidin. Azure AD Connect sihirbazını "Var olan veritabanını kullan" modunda başlatmak için .\AzureADConnect.exe /useexistingdatabase komutunu çalıştırın.
![PowerShell](./media/how-to-connect-install-existing-database/db2.png)
4.  Azure AD Connect'e Hoş Geldiniz ekranı açılır. Lisans koşullarını ve gizlilik bildirimini kabul ettikten sonra **Devam**'a tıklayın.
![Hoş geldiniz](./media/how-to-connect-install-existing-database/db3.png)
5.  **Gerekli bileşenleri yükleme** ekranında **Mevcut bir SQL Server'ı kullanma** seçeneği etkinleştirilir. AD Eşitleme veritabanını barındıran SQL sunucusunun adını belirtin. AD Eşitleme veritabanını barındırmak için kullanılan SQL altyapısı örneği SQL sunucusundaki varsayılan örnek değilse SQL altyapısı örneği adını belirtmeniz gerekir. Ayrıca SQL göz atma özelliği etkin değilse SQL altyapısı örneği bağlantı noktası numarasını da belirtmeniz gerekir. Örneğin:         
![Hoş geldiniz](./media/how-to-connect-install-existing-database/db4.png)           

6.  **Azure AD'ye bağlan** ekranında Azure AD dizininizin genel yöneticisinin kimlik bilgilerini girmeniz gerekir. Varsayılan onmicrosoft.com etki alanındaki bir hesabı kullanmanız önerilir. Bu hesap yalnızca Azure AD'de hizmet hesabı oluşturmak için kullanılır ve sihirbaz tamamlandıktan sonra kullanılmaz.
![Bağlanma](./media/how-to-connect-install-existing-database/db5.png)
 
7.  **Connect your directories** (Dizinlerinize bağlanın) ekranında dizin eşitlemesi için yapılandırılmış olan mevcut AD ormanı yanında kırmızı bir çarpı işaretiyle listelenir. Şirket içi AD ormanında yapılan değişiklikleri eşitlemek için bir AD DS hesabı kullanmanız gerekir. Kimlik bilgileri şifrelenmiş olduğundan ve şifresi yalnızca önceki Azure AD Connect sunucusu tarafından çözülebildiğinden Azure AD Connect sihirbazı, AD Eşitleme veritabanında depolanan AD DS hesabının kimlik bilgilerini alamaz. AD ormanına ait AD DS hesabını belirtmek için **Change Credentials** (Kimlik Bilgilerini Değiştir) öğesine tıklayın.
![Dizinler](./media/how-to-connect-install-existing-database/db6.png)
 
 
8.  Açılan iletişim kutusunda (i) Kuruluş Yöneticisi kimlik bilgileri belirtip Azure AD Connect'in sizin için bir AD DS hesabı oluşturmasını sağlayabilir veya (ii) AD DS hesabını kendiniz oluşturarak kimlik bilgilerini Azure AD Connect'e girebilirsiniz. Bir seçeneği belirleyip gerekli kimlik bilgilerini girdikten sonra **Tamam**'a tıklayarak açılan iletişim kutusunu kapatın.
![Hoş geldiniz](./media/how-to-connect-install-existing-database/db7.png)
 
 
9.  Kimlik bilgileri girildikten sonra kırmızı çarpı işaretinin yerine yeşil onay işareti görünür. **İleri**’ye tıklayın.
![Hoş geldiniz](./media/how-to-connect-install-existing-database/db8.png)
 
 
10. **Yapılandırma için hazır** ekranında **Yükle**'ye tıklayın.
![Hoş geldiniz](./media/how-to-connect-install-existing-database/db9.png)
 
 
11. Yükleme tamamlandıktan sonra Azure AD Connect otomatik olarak Hazırlama Modunda etkinleştirilir. Hazırlama Modunu devre dışı bırakmadan önce sunucu yapılandırmasını ve bekleme durumundaki dışarı aktarma işlemlerini gözden geçirerek beklenmeyen değişikliklerin olup olmadığını kontrol etmeniz önerilir. 

## <a name="next-steps"></a>Sonraki adımlar

- Azure AD Connect'i yüklediniz, artık [yüklemeyi doğrulayabilir ve lisans atayabilirsiniz](how-to-connect-post-installation.md).
- Yüklemeyle etkinleştirilen özellikler hakkında daha fazla bilgi edinin: [Yanlışlıkla silmeleri engelleme](how-to-connect-sync-feature-prevent-accidental-deletes.md) ve [Azure AD Connect Health](how-to-connect-health-sync.md).
- Şu genel konu başlıkları hakkında daha fazla bilgi edinin: [Zamanlayıcı ve eşitleme tetikleme](how-to-connect-sync-feature-scheduler.md).
- [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.
