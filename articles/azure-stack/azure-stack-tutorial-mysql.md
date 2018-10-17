---
title: Yüksek oranda kullanılabilir olan MySQL veritabanları Azure Stack'te teklif | Microsoft Docs
description: Azure Stack ile bir MySQL Server Kaynak sağlayıcısı ana bilgisayarı ve yüksek oranda kullanılabilir olan MySQL veritabanları oluşturmayı öğrenin.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/16/2018
ms.author: jeffgilb
ms.reviewer: quying
ms.openlocfilehash: 2197d197e68528866c892cc51323bc61a208bcc0
ms.sourcegitcommit: 6361a3d20ac1b902d22119b640909c3a002185b3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2018
ms.locfileid: "49366827"
---
# <a name="tutorial-offer-highly-available-mysql-databases"></a>Öğretici: yüksek oranda kullanılabilir olan MySQL veritabanları sunar.

Azure Stack operatör MySQL Server veritabanlarını barındırmak için server Vm'leri yapılandırabilirsiniz. Bir MySQL sonra başarıyla kümedir oluşturulan ve Azure Stack tarafından yönetilen, yüksek oranda kullanılabilir olan MySQL veritabanları MySQL hizmetlere abone olan kullanıcılar kolayca oluşturabilirsiniz.

Bu öğreticide, Azure Stack Market öğesi oluşturmak için kullanılacak gösterilmektedir bir [çoğaltma kümesi ile MySQL](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/bitnami.mysql-cluster). Bu çözüm, veritabanları, yapılandırılabilir bir yineleme sayısı için ana düğüm çoğaltmak için birden çok VM kullanır. Sonra oluşturduğunuz kümeyi daha sonra bir Azure Stack MySQL barındırma sunucusu olarak eklenebilir ve ardından kullanıcılar bir yüksek oranda kullanılabilir olan MySQL veritabanları oluşturabilir.

> [!IMPORTANT]
> **Çoğaltma ile MySQL** Azure Stack Market öğesi tüm Azure bulut aboneliği ortamlar için kullanılabilir olmayabilir. Market öğesi geri kalanında bu tutoral izleyin çalışmadan önce aboneliğinizdeki kullanılabilir olduğundan emin olun.

Ne öğreneceksiniz:

> [!div class="checklist"]
> * Market öğelerinden bir MySQL Server kümesi oluşturma
> * Barındırma sunucusu bir Azure Stack MySQL oluşturma
> * Yüksek oranda kullanılabilir bir MySQL veritabanı oluşturma

Bu öğreticide, bir üç VM MySQL Server kümesi oluşturulur ve mevcut Azure Stack Market öğesi kullanılarak yapılandırılır. 

Bu öğreticide adımları uygulamaya başlamadan önce emin olun [MySQL Server Kaynak sağlayıcısı](azure-stack-mysql-resource-provider-deploy.md) başarıyla yüklendi ve Azure Stack Market'te şu öğeler kullanılabilir:

> [!IMPORTANT]
> Aşağıdakilerin tümü, MySQL kümesi oluşturmak için gereklidir.

- [MySQL ile çoğaltma](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/bitnami.mysql-cluster). MySQL Küme dağıtımı için kullanılacak Bitnami çözüm şablonu budur.
- [Debian 8 "Jessie"](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/credativ.Debian8backports?tab=Overview). Debian 8 için Microsoft Azure ile backports çekirdek "Jessie" credativ tarafından sağlanır. Debian GNU/Linux en popüler Linux dağıtımları biridir.
- [Linux 2.0 için özel betik](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft.custom-script-linux?tab=Overview). Özel betik uzantısı, VM özelleştirme görevlerini post VM sağlama yürütmek için kullanılan bir araçtır. Bu uzantı için bir sanal makine eklendiğinde, bu betikleri Azure depolamadan indirilebilmesi ve bunları sanal makinede çalıştırın. Özel betik uzantısı görevleri Azure PowerShell cmdlet'leri ve Azure platformlar arası komut satırı arabirimi (xPlat CLI) kullanarak otomatik olarak da yapılabilir.
- Linux uzantısı 1.4.7 için VM erişimi. VM erişimi uzantısı parola, SSH anahtarı ya da SSH yapılandırmaları, sanal Makinenize erişim kazanabilirsiniz şekilde sıfırlamanıza olanak sağlar. Ayrıca, parola veya SSH anahtarı ile yeni bir kullanıcı eklemek veya bu uzantıyı kullanan bir kullanıcının silebilirsiniz. Bu uzantı, sanal makineleri hedefler.

Azure Stack marketini için öğe ekleme hakkında daha fazla bilgi için bkz: [Azure Stack Marketini genel bakış](azure-stack-marketplace.md).

Ayrıca, bir SSH istemcisi gibi gerekir [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) dağıtıldıktan sonra Linux sanal makinelerde oturum açmak için.

## <a name="create-a-mysql-server-cluster"></a>MySQL sunucusu kümesi oluşturma 
MySQL sunucusu dağıtmak için bu bölümdeki adımları küme kullanarak kullanım [çoğaltma ile MySQL](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/bitnami.mysql-cluster) Market öğesi. Bu şablon, yüksek oranda kullanılabilir bir MySQL kümesinde yapılandırılan üç MySQL Server örneklerinin dağıtır. Varsayılan olarak, aşağıdaki kaynakları oluşturur:

- Bir sanal ağ
- Bir ağ güvenlik grubu
- Bir depolama hesabı
- Bir kullanılabilirlik kümesi
- Üç ağ arabirimleri (her biri varsayılan VM'ler için bir tane)
- Bir genel IP adresi (birincil MySQL kümesi için sanal makine)
- MySQL kümesi barındırmak için üç Linux Vm'leri

1. Yönetim portalında oturum açın:
    - Çözümünüzün bölge ve dış etki alanı adını bir tümleşik sistemi dağıtımı için bir portal adresi göre değişir. Biçiminde olacaktır https://adminportal.&lt; *Bölge*&gt;.&lt; *FQDN*&gt;.
    - Azure Stack geliştirme Seti'ni (ASDK) kullanıyorsanız, portalı adresidir [ https://adminportal.local.azurestack.external ](https://adminportal.local.azurestack.external).

2. Seçin **\+** **kaynak Oluştur** > **işlem**, ardından **çoğaltma ile MySQL**.

   ![Özel şablon dağıtımı](media/azure-stack-tutorial-mysqlrp/createcluster1.png)

3. Temel dağıtım bilgilerini sağlamaları **Temelleri** sayfası. Varsayılan değerleri gözden geçirin ve gerektiği gibi değiştirin ve tıklayın **Tamam**.<br><br>En azından aşağıdakileri sağlar:
   - Dağıtım adı (varsayılan değer mysql)
   - Uygulama kök parolası. 12 karakter alfasayısal bir parolayla **hiçbir özel karakterler**
   - Uygulama veritabanı adı (varsayılan değer bitnami)
   - MySQL veritabanı çoğaltma oluşturmak için VM sayısı (varsayılan değer 2)
   - Kullanılacak aboneliği seçin
   - Kullanma veya yeni bir kaynak grubunu seçin
   - Konum seçin (varsayılan değer için ASDK yerel)

   ![Dağıtımıyla ilgili temel bilgiler](media/azure-stack-tutorial-mysqlrp/createcluster2.png)

4. Üzerinde **ortamı Yapılandırması** sayfasında aşağıdaki bilgileri sağlayın ve ardından **Tamam**: 
   - Güvenli Kabuk (SSH) kimlik doğrulaması için kullanılacak parola veya SSH ortak anahtarı. Bir parola kullanıyorsanız, harf, rakam içermelidir ve **olabilir** özel karakterlerini içeremez
   - VM boyutu (varsayılan değer standart D1 v2 sanal makinesi)
   - Veri disk boyutu GB tıklayın **Tamam**

   ![Ortam yapılandırma](media/azure-stack-tutorial-mysqlrp/createcluster3.png)

5. Dağıtım gözden **özeti**. İsteğe bağlı olarak, özelleştirilmiş şablon ve parametreleri indirebilir ve ardından **Tamam**.

   ![Özet](media/azure-stack-tutorial-mysqlrp/createcluster4.png)

6. Tıklayın **Oluştur** üzerinde **satın** dağıtımı başlatmak için sayfa.

   ![Satın Al](media/azure-stack-tutorial-mysqlrp/createcluster4.png)

    > [!NOTE]
    > Dağıtım, yaklaşık bir saat sürer. Dağıtım tamamlandıktan ve MySQL kümesi devam etmeden önce tamamen yapılandırılmış emin olun. 

7. Tüm dağıtımlar başarıyla tamamladıktan sonra kaynak grubu öğeleri gözden geçirin ve seçin **mysqlip** genel IP adresi öğesi. Genel IP adresi ve tam FQDN kümenin genel IP'nin kaydedin.<br><br>Bu MySQL kümesi yararlanarak barındırma MySQL sunucusu oluşturmak için bu Azure Stack operatör sağlamanız gerekir.


### <a name="create-a-network-security-group-rule"></a>Ağ güvenlik grubu kuralı oluşturma
Varsayılan olarak, hiçbir genel erişim konak VM'si için MySQL yapılandırılır. Azure Stack MySQL kaynak sağlayıcısı bağlanmak ve MySQL kümeyi yönetmek bir gelen ağ güvenlik grubu (NSG) kuralı oluşturulması gerekir.

1. Yönetici portalındaki MySQL küme dağıtılırken oluşturulan bir kaynak grubuna gidin ve ağ güvenlik grubunu seçin (**varsayılan alt ağ sg**):

   ![açık](media/azure-stack-tutorial-mysqlrp/nsg1.png)

2. Seçin **gelen güvenlik kuralları** ve ardından **Ekle**.<br><br>Girin **3306** içinde **hedef bağlantı noktası aralığı** ve isteğe bağlı olarak bir açıklama sağlayın **adı** ve **açıklama** alanları. Gelen güvenlik kuralı iletişim kutusunu kapatmak için Ekle'ye tıklayın.

   ![açık](media/azure-stack-tutorial-mysqlrp/nsg2.png)

### <a name="configure-external-access-to-the-mysql-cluster"></a>MySQL kümesi dış erişimi yapılandırma
Dış erişim MySQL kümenin bir Azure Stack MySQL Server konağı olarak eklenmeden önce etkinleştirilmesi gerekir.

1. Bir SSH istemcisi kullanarak, bu örnekte [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html), birincil MySQL makinenin genel IP erişebilen bir bilgisayardan oturum açın. Birincil MySQL VM adı genellikle ile biten **0** ve bir genel IP atanır.<br><br>VM kullanıcı adı ile genel IP ve günlük kullanmak **bitnami** ve daha önce oluşturduğunuz özel karakterler uygulama parolası.

   ![LinuxLogin](media/azure-stack-tutorial-mysqlrp/bitnami1.png)


2. SSH İstemcisi penceresinde, bitnami hizmeti etkin ve çalışıyor olduğundan emin olmak için aşağıdaki komutu kullanın. Bitnami parolayı yeniden istendiğinde sağlayın:

   `sudo service bitnami status`

   ![Hizmet kontrol edin](media/azure-stack-tutorial-mysqlrp/bitnami2.png)

3. Mysql'e bağlanmak ve ardından SSH istemcisi çıkmak için Azure Stack MySQL barındırma sunucusu tarafından kullanılacak bir uzaktan erişim kullanıcı hesabı oluşturun.<br><br>MySQL kök parolayla daha önce oluşturulan kök oturum için aşağıdaki komutları çalıştırın ve yeni yönetici kullanıcı oluşturma, değiştirme *\<kullanıcıadı\>* ve *\<parola\>* ortamınız için gerektiği gibi. Bu örnekte, oluşturulacak kullanıcı adlı **sqlsa** ve güçlü bir parola kullanılır:

   ```mysql
   mysql -u root -p
   create user <username>@'%' identified by '<password>';
   grant all privileges on *.* to <username>@'%' with grant option;
   flush privileges;
   ```
   ![Yönetici kullanıcı oluşturma](media/azure-stack-tutorial-mysqlrp/bitnami3.png)


4. Yeni MySQL kullanıcı bilgilerini kaydedin.<br><br>Bu kullanıcı adı ve parola, genel IP adresini veya tam FQDN'sini küme için genel IP ile birlikte bir Azure Stack kullanarak bu MySQL kümesi barındırma MySQL sunucusu oluşturabilmek işlecine sağlamanız gerekir.


## <a name="create-an-azure-stack-mysql-hosting-server"></a>Barındırma sunucusu bir Azure Stack MySQL oluşturma
MySQL Server kümesi oluşturulur ve düzgün yapılandırılmış sonra bir Azure Stack operatörü bir Azure Stack MySQL barındırma ek kapasite veritabanları oluşturmak için kullanıcıları için kullanılabilir hale getirmek için sunucu oluşturmanız gerekir. 

MySQL kümenin bulunduğu kaynak grubunun oluşturulduğu birincil VM daha önce kaydedilen MySQL kümesinin genel IP için genel IP veya tam FQDN değerini kullandığınızdan emin olun (**mysqlip**). Ayrıca, işleç MySQL sunucusu kimlik doğrulama bilgilerini uzaktan MySQL kümesi veritabanına erişmek için oluşturduğunuz bilmeniz gerekir.

> [!NOTE]
> Bu adım, Azure Stack Yönetim Portalı'ndan Azure Stack operatör tarafından çalıştırılmalıdır.

MySQL kümenin genel IP'si ve MySQL kimlik doğrulaması oturum açma bilgileri kullanarak, bir Azure Stack operatörü artık [yeni MySQL kümesi kullanarak MySQL barındırma sunucusunun oluşturma](azure-stack-mysql-resource-provider-hosting-servers.md#connect-to-a-mysql-hosting-server). 

Ayrıca planları oluşturduktan ve MySQL veritabanı oluşturma kullanıcılar için kullanılabilir hale getirmek sunar emin olun. Bir işleç eklemeniz gerekecektir **Microsoft.MySqlAdapter** hizmeti bir plana ve yüksek oranda kullanılabilir veritabanları için özel bir yeni kota oluştur. Planları oluşturma hakkında daha fazla bilgi için bkz. [Plan, teklif, kota ve abonelik genel bakış](azure-stack-plan-offer-quota-overview.md).

> [!TIP]
> **Microsoft.MySqlAdapter** hizmet planları kadar eklemek kullanılabilir olmayacak [MySQL Server Kaynak sağlayıcısı dağıtılan](azure-stack-mysql-resource-provider-deploy.md).



## <a name="create-a-highly-available-mysql-database"></a>Yüksek oranda kullanılabilir bir MySQL veritabanı oluşturma
MySQL kümesi oluşturulduğundan, yapılandırılmış ve bir Azure Stack MySQL barındırma sunucusu olarak Azure Stack operatör tarafından eklenen sonra MySQL Server veritabanı özellikleri dahil olmak üzere bir aboneliğe sahip bir kiracı kullanıcı tarafından yüksek oranda kullanılabilir olan MySQL veritabanları oluşturabilirsiniz Bu bölümdeki adımları izleyin. 

> [!NOTE]
> Bu adımları Azure Stack Kullanıcı Portalı'ndan bir abonelik MySQL Server özellikleriyle (Microsoft.MySQLAdapter hizmeti) ile bir kiracı kullanıcı olarak çalıştırın.

1. Kullanıcı portalında oturum açın:
    - Çözümünüzün bölge ve dış etki alanı adını bir tümleşik sistemi dağıtımı için bir portal adresi göre değişir. Biçiminde olacaktır https://portal.&lt; *Bölge*&gt;.&lt; *FQDN*&gt;.
    - Azure Stack geliştirme Seti'ni (ASDK) kullanıyorsanız, kullanıcı portalı adresidir [ https://portal.local.azurestack.external ](https://portal.local.azurestack.external).

2. Seçin **\+** **kaynak Oluştur** > **veri \+ depolama**, ardından **MySQL veritabanı** .<br><br>Ad, harmanlaması, kullanılacak aboneliği ve dağıtımı için kullanılacak konumu dahil olmak üzere gerekli veritabanı özellik bilgileri sağlar. 

   ![MySQL veritabanı oluşturma](./media/azure-stack-tutorial-mysqlrp/createdb1.png)

3. Seçin **SKU** uygun MySQL barındırma sunucusunu kullanmak için SKU seçin. Bu örnekte, Azure Stack operatörü oluşturduğu **MySQL-HA** MySQL küme veritabanları için yüksek kullanılabilirliği desteklemek için SKU.

   ![SKU'ları seçin](./media/azure-stack-tutorial-mysqlrp/createdb2.png)

4. Seçin **oturum açma** > **yeni bir oturum açma oluşturma** ve ardından yeni veritabanı için kullanılacak MySQL kimlik doğrulaması kimlik bilgilerini sağlayın. İşiniz bittiğinde tıklayın **Tamam** ardından **Oluştur** veritabanı dağıtım işlemini başlatmak için.

   ![Oturum açma ekleme](./media/azure-stack-tutorial-mysqlrp/createdb3.png)

5. MySQL veritabanı dağıtım başarıyla tamamlandığında yeni yüksek oranda kullanılabilir veritabanına bağlanmak için kullanılacak bağlantı dizesi bulmak için veritabanı özellikleri gözden geçirin. 

   ![Bağlantı dizesini görüntüle](./media/azure-stack-tutorial-mysqlrp/createdb4.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Market öğelerinden bir MySQL Server kümesi oluşturma
> * Barındırma sunucusu bir Azure Stack MySQL oluşturma
> * Yüksek oranda kullanılabilir bir MySQL veritabanı oluşturma

Bilgi edinmek için sonraki öğreticiye ilerleyin nasıl yapılır:
> [!div class="nextstepaction"]
> [Web apps teklifi](/azure-stack-tutorial-app-service.md)
