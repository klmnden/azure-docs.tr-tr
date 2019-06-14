---
title: Var olan bir ad eşitleme veritabanını kullanarak Azure AD Connect'i yükleme | Microsoft Docs
description: Bu konu, mevcut bir ad eşitleme veritabanını kullanmayı açıklar.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.reviewer: cychua
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/30/2017
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4dc6993586063c9c99a287c51d799b44f921768d
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60245217"
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
3.  Yeni bir komut istemi veya PowerShell oturumu başlatın. Klasöre "C:\Program Files\Microsoft Azure Active Directory Connect" gidin. Azure AD Connect sihirbazını "Var olan veritabanını kullan" modunda başlatmak için .\AzureADConnect.exe /useexistingdatabase komutunu çalıştırın.

> [!NOTE]
> Anahtarını kullanarak **/useexistingdatabase** yalnızca veritabanı zaten daha önceki bir Azure AD Connect yükleme verilerini içerdiğinde. Örneğin, ne zaman tam SQL Server veritabanına yerel bir veritabanından taşıyan ve Azure AD Connect sunucusu ne zaman yeniden oluşturuldu ve Azure AD Connect'in önceki bir yüklemeden ADSync veritabanı yedeğini SQL geri. Veritabanı boş ise, diğer bir deyişle, bu olmayan herhangi bir önceki Azure AD Connect yüklemesi verileri içeren, bu adımı atlayın.

![PowerShell](./media/how-to-connect-install-existing-database/db2.png)
1. Azure AD Connect'e Hoş Geldiniz ekranı açılır. Lisans koşullarını ve gizlilik bildirimini kabul ettikten sonra **Devam**'a tıklayın.
   ![Hoş geldiniz](./media/how-to-connect-install-existing-database/db3.png)
1. **Gerekli bileşenleri yükleme** ekranında **Mevcut bir SQL Server'ı kullanma** seçeneği etkinleştirilir. AD Eşitleme veritabanını barındıran SQL sunucusunun adını belirtin. AD Eşitleme veritabanını barındırmak için kullanılan SQL altyapısı örneği SQL sunucusundaki varsayılan örnek değilse SQL altyapısı örneği adını belirtmeniz gerekir. Ayrıca SQL göz atma özelliği etkin değilse SQL altyapısı örneği bağlantı noktası numarasını da belirtmeniz gerekir. Örneğin:         
   ![Hoş geldiniz](./media/how-to-connect-install-existing-database/db4.png)           

1. **Azure AD'ye bağlan** ekranında Azure AD dizininizin genel yöneticisinin kimlik bilgilerini girmeniz gerekir. Varsayılan onmicrosoft.com etki alanındaki bir hesabı kullanmanız önerilir. Bu hesap yalnızca Azure AD'de hizmet hesabı oluşturmak için kullanılır ve sihirbaz tamamlandıktan sonra kullanılmaz.
   ![Bağlanma](./media/how-to-connect-install-existing-database/db5.png)
 
1. **Connect your directories** (Dizinlerinize bağlanın) ekranında dizin eşitlemesi için yapılandırılmış olan mevcut AD ormanı yanında kırmızı bir çarpı işaretiyle listelenir. Şirket içi AD ormanında yapılan değişiklikleri eşitlemek için bir AD DS hesabı kullanmanız gerekir. Kimlik bilgileri şifrelenmiş olduğundan ve şifresi yalnızca önceki Azure AD Connect sunucusu tarafından çözülebildiğinden Azure AD Connect sihirbazı, AD Eşitleme veritabanında depolanan AD DS hesabının kimlik bilgilerini alamaz. AD ormanına ait AD DS hesabını belirtmek için **Change Credentials** (Kimlik Bilgilerini Değiştir) öğesine tıklayın.
   ![Dizinler](./media/how-to-connect-install-existing-database/db6.png)
 
 
1. Açılan iletişim kutusunda (i) Kuruluş Yöneticisi kimlik bilgileri belirtip Azure AD Connect'in sizin için bir AD DS hesabı oluşturmasını sağlayabilir veya (ii) AD DS hesabını kendiniz oluşturarak kimlik bilgilerini Azure AD Connect'e girebilirsiniz. Bir seçeneği belirleyip gerekli kimlik bilgilerini girdikten sonra **Tamam**'a tıklayarak açılan iletişim kutusunu kapatın.
   ![Hoş geldiniz](./media/how-to-connect-install-existing-database/db7.png)
 
 
1. Kimlik bilgileri girildikten sonra kırmızı çarpı işaretinin yerine yeşil onay işareti görünür. **İleri**’ye tıklayın.
   ![Hoş geldiniz](./media/how-to-connect-install-existing-database/db8.png)
 
 
1. **Yapılandırma için hazır** ekranında **Yükle**'ye tıklayın.
   ![Hoş geldiniz](./media/how-to-connect-install-existing-database/db9.png)
 
 
1. Yükleme tamamlandıktan sonra Azure AD Connect otomatik olarak Hazırlama Modunda etkinleştirilir. Hazırlama Modunu devre dışı bırakmadan önce sunucu yapılandırmasını ve bekleme durumundaki dışarı aktarma işlemlerini gözden geçirerek beklenmeyen değişikliklerin olup olmadığını kontrol etmeniz önerilir. 

## <a name="post-installation-tasks"></a>Yükleme sonrası görevler
Azure AD Connect 1.2.65.0 önceki bir sürümü tarafından oluşturulmuş bir veritabanı yedeklemesini geri yüklerken, hazırlık sunucusu otomatik olarak bir oturum açma yöntemi seçin **yapılandırmazsanız**. Parola karma eşitlemesi ve parola geri yazma tercihlerini geri yüklenir, ancak daha sonra sunucunuz etkin eşitlemeyi yürürlükte için diğer ilkeleri eşleştirmek için oturum açma yöntemi değiştirmeniz gerekir.  Bu adımları tamamlamak için başarısızlık, kullanıcıların oturum açmaya engelleyebilir, bu sunucunun etkin hale gelir.  

Gerekli olan herhangi bir ilave adımı doğrulamak için aşağıdaki tabloyu kullanın.

|Özellik|Adımlar|
|-----|-----|
|Parola Karması eşitleme| Parola karma eşitlemesi ve parola geri yazma ayarları tam olarak Azure AD Connect sürümleri ile 1.2.65.0 başlangıç geri yüklenir.  Azure AD Connect'in eski bir sürümü kullanılarak geri yükleme eşitleme bu özellikler için bunların sunucunuz etkin eşitlemeyi eşleştiğinden emin olmak için seçenek ayarları gözden geçirin.  Başka yapılandırma adımları gerekli olmamalıdır.|
|AD FS ile Federasyon|Azure kimlik doğrulamaları sunucunuz etkin eşitlemeyi için yapılandırılmış AD FS ilkesini kullanmaya devam eder.  Azure AD Connect, AD FS grubunu yönetmek için kullanıyorsanız, AD FS federasyon bekleme sunucunuz etkin eşitlemeyi örnekli mimariye geçiş hazırlığı için oturum açma yöntemi isteğe bağlı olarak değişebilir.   Cihaz seçenekleri etkin eşitlemeyi sunucuda etkinleştirilirse, "cihaz seçeneklerini Yapılandır" görevi'ni çalıştırarak bu sunucuda bu seçenekleri yapılandırın.|
|Geçişli kimlik doğrulaması ve Masaüstü çoklu oturum açma|Etkin eşitlemeyi sunucunuzdaki yapılandırmasıyla eşleşecek şekilde oturum açma yöntemini güncelleştirin.  Bunun üzerinde birlikte sorunsuz çoklu oturum açma birincil, geçişli kimlik doğrulaması için sunucu yükseltme devre dışı bırakılır ve sahip değilseniz Kiracı URL'nizi kilitli olabilir önce uyulmazsa yedekleme olarak parola karması eşitleme seçeneği oturum açın. Ayrıca, hazırlama modunda geçişli kimlik doğrulaması etkinleştirdiğinizde, yeni bir kimlik doğrulama Aracısı yükleneceğini, Not, kayıtlı ve oturum açma istekleri kabul edeceği bir yüksek kullanılabilirlik aracısı olarak çalıştırılır.|
|PingFederate ile federasyon|Azure kimlik doğrulamaları sunucunuz etkin eşitlemeyi için yapılandırılmış PingFederate ilkesini kullanmaya devam eder.  Bekleme sunucunuz etkin eşitlemeyi örnekli mimariye geçiş için hazırlık PingFederate için oturum açma yöntemi isteğe bağlı olarak değişebilir.  Ek etki alanlarını PingFederate ile federasyona eklemek ihtiyacınız kadar bu adımı ertelenmiş.|

## <a name="next-steps"></a>Sonraki adımlar

- Azure AD Connect'i yüklediniz, artık [yüklemeyi doğrulayabilir ve lisans atayabilirsiniz](how-to-connect-post-installation.md).
- Yüklemeyle etkinleştirilen özellikler hakkında daha fazla bilgi edinin: [Yanlışlıkla silmeleri engelleme](how-to-connect-sync-feature-prevent-accidental-deletes.md) ve [Azure AD Connect Health](how-to-connect-health-sync.md).
- Şu genel konu başlıkları hakkında daha fazla bilgi edinin: [Zamanlayıcı ve eşitleme tetikleme](how-to-connect-sync-feature-scheduler.md).
- [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.
