---
title: Azure AD Connect veritabanını SQL Server Express'ten SQL Server'a taşıma. | Microsoft Docs
description: Bu belge, Azure AD Connect veritabanını yerel SQL Server Express'ten uzaktaki bir SQL Server'a nasıl taşıyacağınızı açıklar.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/29/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7ae0e87fddabee9f42cbb5506dce4cd7a5f4f082
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64918846"
---
# <a name="move-azure-ad-connect-database-from-sql-server-express-to-sql-server"></a>Azure AD Connect veritabanını SQL Server Express'ten SQL Server'a taşıma 

Bu belge, Azure AD Connect veritabanını yerel SQL Server Express'ten uzaktaki bir SQL Server'a nasıl taşıyacağınızı açıklar.  Bu görevi gerçekleştirmek için aşağıdaki yordamları kullanabilirsiniz.

## <a name="about-this-scenario"></a>Bu senaryo hakkında
Aşağıda bu senaryo hakkında bazı kısa bilgilere yer verilmiştir.  Bu senaryoda Azure AD Connect sürümü (1.1.819.0) tek bir Windows Server 2016 etki alanı denetleyicisine yüklenmiştir.  Veritabanı için yerleşik SQL Server 2012 Express Edition'ı kullanmaktadır.  Veritabanı bir SQL Server 2017 sunucusuna taşınacaktır.

![Senaryo mimarisi](media/how-to-connect-install-move-db/move1.png)

## <a name="move-the-azure-ad-connect-database"></a>Azure AD Connect veritabanını taşıma
Azure AD Connect veritabanını uzak SQL Server'a taşımak için aşağıdaki adımları takip edin.

1. Azure AD Connect sunucusunda **Hizmetler**'e gidin ve **Microsoft Azure AD Eşitleme** hizmetini durdurun.
2. Bulun **% Program Files%\Microsoft Azure AD eşitleme/Data/** klasörü ve kopyalama **ADSync.mdf** ve **ADSync_log.ldf** dosyaları uzak SQL Server için.
3. Azure AD Connect sunucusundaki **Microsoft Azure AD Eşitleme** hizmetini yeniden başlatın.
4. Denetim Masası - - Programlar - Programlar ve Özellikler sayfasından Azure AD Connect'i kaldırın.  Microsoft Azure AD Connect'e ve ardından üst taraftaki Kaldır düğmesine tıklayın.
5. Uzak SQL sunucusunda SQL Server Management Studio'yu açın.
6. Veritabanları bölümüne sağ tıklayıp Ekle'yi seçin.
7. **Veritabanı Ekle** ekranında **Ekle**'ye tıklayıp ADSync.mdf dosyasını gösterin.  **Tamam**'ı tıklatın.
   ![Veritabanı ekleme](media/how-to-connect-install-move-db/move2.png)

8. Veritabanı eklendikten sonra Azure AD Connect sunucusuna gidip Azure AD Connect'i yükleyin.
9. MSI yüklemesi tamamlandıktan sonra Azure AD Connect sihirbazı Hızlı mod kurulumu açılır. Çıkış simgesine tıklayarak ekranı kapatın.
   ![Hoş geldiniz](./media/how-to-connect-install-move-db/db1.png)
10. Yeni bir komut istemi veya PowerShell oturumu başlatın. Klasörüne gidin \<sürücüsü > \Program Azure AD Connect. Azure AD Connect sihirbazını "Var olan veritabanını kullan" modunda başlatmak için .\AzureADConnect.exe /useexistingdatabase komutunu çalıştırın.
    ![PowerShell](./media/how-to-connect-install-move-db/db2.png)
11. Azure AD Connect'e Hoş Geldiniz ekranı açılır. Lisans koşullarını ve gizlilik bildirimini kabul ettikten sonra **Devam**'a tıklayın.
    ![Hoş geldiniz](./media/how-to-connect-install-move-db/db3.png)
12. **Gerekli bileşenleri yükleme** ekranında **Mevcut bir SQL Server'ı kullanma** seçeneği etkinleştirilir. AD Eşitleme veritabanını barındıran SQL sunucusunun adını belirtin. AD Eşitleme veritabanını barındırmak için kullanılan SQL altyapısı örneği SQL sunucusundaki varsayılan örnek değilse SQL altyapısı örneği adını belirtmeniz gerekir. Ayrıca SQL göz atma özelliği etkin değilse SQL altyapısı örneği bağlantı noktası numarasını da belirtmeniz gerekir. Örneğin:         
    ![Hoş geldiniz](./media/how-to-connect-install-move-db/db4.png)           

13. **Azure AD'ye bağlan** ekranında Azure AD dizininizin genel yöneticisinin kimlik bilgilerini girmeniz gerekir. Varsayılan onmicrosoft.com etki alanındaki bir hesabı kullanmanız önerilir. Bu hesap yalnızca Azure AD'de hizmet hesabı oluşturmak için kullanılır ve sihirbaz tamamlandıktan sonra kullanılmaz.
    ![Bağlanma](./media/how-to-connect-install-move-db/db5.png)
 
14. **Connect your directories** (Dizinlerinize bağlanın) ekranında dizin eşitlemesi için yapılandırılmış olan mevcut AD ormanı yanında kırmızı bir çarpı işaretiyle listelenir. Şirket içi AD ormanında yapılan değişiklikleri eşitlemek için bir AD DS hesabı kullanmanız gerekir. Kimlik bilgileri şifrelenmiş olduğundan ve şifresi yalnızca önceki Azure AD Connect sunucusu tarafından çözülebildiğinden Azure AD Connect sihirbazı, AD Eşitleme veritabanında depolanan AD DS hesabının kimlik bilgilerini alamaz. AD ormanına ait AD DS hesabını belirtmek için **Change Credentials** (Kimlik Bilgilerini Değiştir) öğesine tıklayın.
    ![Dizinler](./media/how-to-connect-install-move-db/db6.png)
 
 
15. Açılan iletişim kutusunda (i) Kuruluş Yöneticisi kimlik bilgileri belirtip Azure AD Connect'in sizin için bir AD DS hesabı oluşturmasını sağlayabilir veya (ii) AD DS hesabını kendiniz oluşturarak kimlik bilgilerini Azure AD Connect'e girebilirsiniz. Bir seçeneği belirleyip gerekli kimlik bilgilerini girdikten sonra **Tamam**'a tıklayarak açılan iletişim kutusunu kapatın.
    ![Hoş geldiniz](./media/how-to-connect-install-move-db/db7.png)
 
 
16. Kimlik bilgileri girildikten sonra kırmızı çarpı işaretinin yerine yeşil onay işareti görünür. **İleri**’ye tıklayın.
    ![Hoş geldiniz](./media/how-to-connect-install-move-db/db8.png)
 
 
17. **Yapılandırma için hazır** ekranında **Yükle**'ye tıklayın.
    ![Hoş geldiniz](./media/how-to-connect-install-move-db/db9.png)
 
 
18. Yükleme tamamlandıktan sonra Azure AD Connect otomatik olarak Hazırlama Modunda etkinleştirilir. Hazırlama Modunu devre dışı bırakmadan önce sunucu yapılandırmasını ve bekleme durumundaki dışarı aktarma işlemlerini gözden geçirerek beklenmeyen değişikliklerin olup olmadığını kontrol etmeniz önerilir. 

## <a name="next-steps"></a>Sonraki adımlar

- [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.
- [Azure AD Connect'i mevcut bir AD Eşitleme veritabanını kullanarak yükleme](how-to-connect-install-existing-database.md)
- [SQL yönetici temsilcisi izinlerini kullanarak Azure AD Connect'i yükleme](how-to-connect-install-sql-delegation.md)

