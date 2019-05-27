---
author: rothja
ms.service: virtual-machines-sql
ms.topic: include
ms.date: 10/26/2018
ms.author: jroth
ms.openlocfilehash: 297317ff33d88d6390220980ef35f2538579e310
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66165547"
---
### <a name="open-tcp-ports-in-the-windows-firewall-for-the-default-instance-of-the-database-engine"></a>Veritabanı Altyapısı’nın varsayılan örneği için Windows güvenlik duvarında TCP bağlantı noktalarını açma
1. Uzak Masaüstü kullanarak sanal makineye bağlanın. Sanal makineye bağlanma işleminin ayrıntılı yönergeleri için bkz. [Uzak Masaüstü ile SQL VM’yi Açma](../articles/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision.md#remotedesktop).
2. ' De, başlangıç ekranında imzalandıktan sonra yazın **WF.msc**ve ardından ENTER tuşuna basın.
   
    ![Güvenlik Duvarı Programını başlatın](./media/virtual-machines-sql-server-connection-steps/12Open-WF.png)
3. **Gelişmiş Güvenlik Özellikli Windows Güvenlik Duvarı**’nın sol bölmesinde **Gelen Kuralları**’na tıklayın ve ardından eylem bölmesinde **Yeni Kural**’a tıklayın.
   
    ![Yeni Kural](./media/virtual-machines-sql-server-connection-steps/13New-FW-Rule.png)
4. **Yeni Gelen Kuralı Sihirbazı** iletişim kutusunda, **Kural Türü** başlığının altındaki **Bağlantı noktası**’nı seçin ve **İleri**’ye tıklayın.
5. **Protokol ve Bağlantı Noktaları** iletişim kutusunda varsayılan **TCP**’yi kullanın. **Belirli yerel bağlantı noktaları** kutusunda, Veritabanı Altyapısı örneğinin bağlantı noktası numarasını yazın (varsayılan örnek için **1433** değeri veya uç nokta adımında kendi seçtiğiniz özel bağlantı noktası).
   
    ![TCP Bağlantı Noktası 1433](./media/virtual-machines-sql-server-connection-steps/14Port-1433.png)
6. **İleri**’ye tıklayın.
7. **Eylem** iletişim kutusunda **Bağlantıya izin ver**’i seçin ve **İleri**’ye tıklayın.
   
    **Güvenlik Notu:** Seçme **Bağlantı güvenliyse izin** ek güvenlik sağlayabilir. Ortamınızda ek güvenlik seçenekleri yapılandırmak istiyorsanız bu seçeneği belirtin.
   
    ![Bağlantılara İzin Verme](./media/virtual-machines-sql-server-connection-steps/15Allow-Connection.png)
8. **Profil** iletişim kutusunda **Genel**, **Özel** ve **Etki Alanı**’nı seçin. Ardından **İleri**'ye tıklayın.
   
    **Güvenlik Notu:**  Seçme **genel** internet üzerinden erişim sağlar. Mümkün olduğu durumlarda daha kısıtlayıcı bir profil seçin.
   
    ![Genel Profil](./media/virtual-machines-sql-server-connection-steps/16Public-Private-Domain-Profile.png)
9. **Ad** iletişim kutusunda, bu kural için ad ve açıklama yazın ve **Son**’a tıklayın.
   
    ![Kural Adı](./media/virtual-machines-sql-server-connection-steps/17Rule-Name.png)

Gerekirse diğer bileşenler için ek bağlantı noktaları açın. Daha fazla bilgi için bkz. [Windows Güvenlik Duvarı’nı SQL Server Erişimine İzin Verecek Şekilde Yapılandırma](https://msdn.microsoft.com/library/cc646023.aspx).

### <a name="configure-sql-server-to-listen-on-the-tcp-protocol"></a>SQL Server’ı TCP protokolünde dinleyecek şekilde yapılandırma

[!INCLUDE [Enable TCP](virtual-machines-sql-server-connection-tcp-protocol.md)]

### <a name="configure-sql-server-for-mixed-mode-authentication"></a>SQL Server’ı karma mod kimlik doğrulaması için yapılandırma
SQL Server Veritabanı Altyapısı, etki alanı ortamı olmadan Windows Kimlik Doğrulaması’nı kullanamaz. Başka bir bilgisayardan Veritabanı Altyapısı’na bağlanmak için, SQL Server’ı karma mod kimlik doğrulamasıyla yapılandırın. Karma mod kimlik doğrulaması hem SQL Server Kimlik Doğrulaması’na hem de Windows Kimlik Doğrulaması’na izin verir.

> [!NOTE]
> Yapılandırılmış bir etki alanı ortamıyla Azure Sanal Ağı’nı yapılandırdıysanız, karma mod kimlik doğrulamasını yapılandırmanız gerekmeyebilir.
> 
> 

1. Sanal makineye bağlı durumdayken, Başlangıç sayfasında **SQL Server Management Studio** yazın ve seçili simgeye tıklayın.
   
    İlk kez açtığınızda Management Studio’nun, kullanıcıların Management Studio ortamını oluşturması gerekir. Bu birkaç dakika sürebilir.
2. Management Studio, **Sunucuya Bağlan** iletişim kutusunu gösterir. **Sunucu adı** kutusuna, Nesne Gezgini’yle Veritabanı Altyapısı’na bağlanacak sanal makinenin adını yazın. (Sanal makine adı yerine **Sunucu adı** olarak **(yerel)** veya tek nokta da kullanabilirsiniz). Seçin **Windows kimlik doğrulaması**, bırakıp ***your_VM_name\your_local_administrator*** içinde **kullanıcı adı** kutusu. **Bağlan**'a tıklayın.
   
    ![Sunucuya bağlanma](./media/virtual-machines-sql-server-connection-steps/19Connect-to-Server.png)
3. SQL Server Management Studio Nesne Gezgini’nde, SQL Server örneğinin adına (sanal makine adı) sağ tıklayın ve ardından **Özellikler**’e tıklayın.
   
    ![Sunucu Özellikleri](./media/virtual-machines-sql-server-connection-steps/20Server-Properties.png)
4. **Güvenlik** sayfasındaki **Sunucu kimlik doğrulaması** bölümünde **SQL Server ve Windows Kimlik Doğrulaması modu**’nu seçin ve **Tamam**’a tıklayın.
   
    ![Kimlik Doğrulaması Modunu Seçme](./media/virtual-machines-sql-server-connection-steps/21Mixed-Mode.png)
5. SQL Server Management Studio iletişim kutusunda **Tamam**’a tıklayarak SQL Server’ı yeniden başlatma gereksinimine ilişkin istemi kabul edin.
6. Nesne Gezgini’nde, sunucunuza sağ tıklayın ve ardından **Yeniden Başlat**’a tıklayın. (SQL Server Agent çalışıyorsa, onun da yeniden başlatılması gerekir.)
   
    ![Yeniden Başlatma](./media/virtual-machines-sql-server-connection-steps/22Restart2.png)
7. SQL Server’ı yeniden başlatmak istediğinizi kabul etmek için SQL Server Management Studio iletişim kutusunda **Evet**’e tıklayın.

### <a name="create-sql-server-authentication-logins"></a>SQL Server kimlik doğrulaması için oturum açma kimliği oluşturma
Başka bir bilgisayardan Veritabanı Altyapısı’na bağlanmak için, en az bir SQL Server kimlik doğrulaması oturum açma kimliği oluşturmalısınız.

1. SQL Server Management Studio Nesne Gezgini’nde, yeni oturum açma kimliğini oluşturmak istediğiniz sunucu örneğinin klasörünü genişletin.
2. **Güvenlik** klasörüne sağ tıklayın, **Yeni**’nin üzerine gelin ve **Oturum Açma...** öğesini seçin.
   
    ![Yeni Oturum Açma Kimliği](./media/virtual-machines-sql-server-connection-steps/23New-Login.png)
3. **Oturum Açma - Yeni** iletişim kutusunun **Genel** sayfasında, **Oturum açma adı** kutusuna yeni kullanıcının adını girin.
4. **SQL Server kimlik doğrulaması**’nı seçin.
5. **Parola** kutusuna, yeni kullanıcı için bir parola girin. **Parolayı Onayla** kutusuna parolayı yeniden girin.
6. Gerekli parola zorlama seçeneklerini belirtin (**Parola ilkesi uygula**, **Parola için süre sonu uygula** ve **Kullanıcı bir sonraki oturum açışta parolayı değiştirmeli**). Bu oturum açma kimliğini kendiniz kullanıyorsanız, parolayı bir sonraki oturum açışta değiştirilecek şekilde ayarlamanız gerekmez.
7. **Varsayılan veritabanı** listesinde, oturum açma kimliği için varsayılan veritabanını seçin. Bu seçeneği varsayılan değeri **asıl** veritabanıdır. Henüz bir kullanıcı veritabanı oluşturmadıysanız, bu ayarı **asıl** olarak bırakın.
   
    ![Oturum Açma Özellikleri](./media/virtual-machines-sql-server-connection-steps/24Test-Login.png)
8. Bu oluşturduğunuz ilk oturum açma kimliğiyse, bu kimliği SQL Server yöneticisi olarak belirlemeniz yararlı olabilir. Bunu yapmak istiyorsanız, **Sunucu Rolleri** sayfasında **sysadmin** öğesini işaretleyin.
   
   > [!NOTE]
   > Sysadmin sabit sunucu rolünün üyeleri, Veritabanı Altyapısı üzerinde tam denetime sahip olur. Bu rolün üyeliğini dikkatle kısıtlamalısınız.
   > 
   > 
   
   ![sysadmin](./media/virtual-machines-sql-server-connection-steps/25sysadmin.png)
9. Tamam'a tıklayın.

SQL Server oturum açma kimlikleri hakkında daha fazla bilgi için bkz. [Oturum Açma Kimliği Oluşturma](https://msdn.microsoft.com/library/aa337562.aspx).

