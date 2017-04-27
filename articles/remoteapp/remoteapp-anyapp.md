---
title: "Azure RemoteApp ile herhangi bir cihazda tüm Windows uygulamalarını çalıştırma | Microsoft Belgeleri"
description: "Azure RemoteApp kullanarak herhangi bir Windows uygulamasını kullanıcılarınızla nasıl paylaşacağınızı öğrenin."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
editor: 
ms.assetid: 961d40ca-9673-4977-aa54-d6b22fc61ce1
ms.service: remoteapp
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: compute
ms.date: 04/26/2017
ms.author: mbaldwin
translationtype: Human Translation
ms.sourcegitcommit: 5cce99eff6ed75636399153a846654f56fb64a68
ms.openlocfilehash: d32d91f7bbfcea61caba6ccc3033929d307f14be
ms.lasthandoff: 03/31/2017


---
# <a name="run-any-windows-app-on-any-device-with-azure-remoteapp"></a>Azure RemoteApp ile herhangi bir cihazda tüm Windows uygulamalarını çalıştırma
> [!IMPORTANT]
> Azure RemoteApp 31 Ağustos 2017’de kullanımdan kaldırılacaktır. Ayrıntılı bilgi için [duyuruyu](https://go.microsoft.com/fwlink/?linkid=821148) okuyun.
> 
> 

Yalnızca Azure RemoteApp kullanarak şu anda bir Windows uygulamasını gerçekten de her yerde her cihazda çalıştırabilirsiniz. İster 10 yıl önce yazılmış özel bir uygulama ister bir Office uygulaması olsun, kullanıcılarınız artık birkaç uygulama için belirli bir işletim sistemini (Windows XP gibi) kullanmak zorunda değil.

Azure RemoteApp ile, kullanıcılarınız kendi Android veya Apple cihazlarını da kullanabilir ve Windows’daki (veya Windows Phone’daki) deneyimin aynısını elde edebilir. Bunun gerçekleştirilmesi Windows uygulamanızın Azure’daki bir Windows sanal makineler koleksiyonunda barındırılmasıyla sağlanır. Kullanıcılarınız, internete bağlanabildikleri her yerden bu makinelere erişebilir. 

Bunun tam olarak nasıl yapılacağı konusunda bir örneği aşağıda okuyabilirsiniz.

Bu makalede, Access’i tüm kullanıcılarımızla paylaşacağız. Ancak, HERHANGİ bir uygulama kullanabilirsiniz. Uygulamanızı Windows Server 2012 R2 kullanan bir bilgisayara yükleyebildiğiniz sürece, bu uygulamayı aşağıdaki adımları kullanarak paylaşabilirsiniz. Uygulamanızın çalışacağından emin olmak için [uygulama gereksinimlerini](remoteapp-appreqs.md) gözden geçirebilirsiniz.

Access’in bir veritabanı olduğunu ve bu veritabanının kullanışlı olmasını istediğimizi lütfen unutmayın. Bu nedenle, kullanıcıların Access veri paylaşımına erişimini sağlamak üzere birkaç ek adım gerçekleştireceğiz. Uygulamanız bir veritabanı değilse ya da kullanıcılarınızın bir dosya paylaşımına erişebilmesine ihtiyacınız yoksa, bu öğreticide buna yönelik adımları atlayabilirsiniz.

> [!NOTE]
> <a name="note"></a>Bu eğitmeni tamamlamak için bir Azure hesabınızın olması gerekir:
> 
> * [Ücretsiz bir Azure hesabı açabilirsiniz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F): Ücretli Azure hizmetlerini denemek için kullanabileceğiniz krediler alabilir ve hatta kullanıldıktan sonra bile hesabı tutabilir ve Web siteleri gibi ücretsiz Azure hizmetlerini kullanabilirsiniz. Açıkça ayarlarınızı değiştirip ücretlendirme istemediğiniz sürece kredi kartınız asla ücretlendirilmeyecektir.
> * [MSDN abone avantajlarını etkinleştirebilirsiniz](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F): MSDN aboneliğiniz, ücretli Azure hizmetlerinizi kullanabildiğiniz her ay size kredi verir.
> 
> 

## <a name="create-a-collection-in-remoteapp"></a>RemoteApp’te koleksiyon oluşturma
Önce bir koleksiyon oluşturun. Koleksiyon, uygulama ve kullanıcılarınız için bir kapsayıcı görevi görür. Her koleksiyon bir görüntüyü temel alır. Kendi görüntünüzü oluşturabilir veya aboneliğinizle birlikte sağlanan bir görüntüyü kullanabilirsiniz. Bu öğreticide, paylaşmak istediğimiz uygulamayı içeren Office 2013 deneme sürümü görüntüsünü kullanıyoruz.

1. Azure portalında, sol taraftaki gezinti ağacında RemoteApp’i görene kadar aşağı kaydırın. Bu sayfayı açın.
2. **Create a RemoteApp collection** (RemoteApp koleksiyonu oluştur) seçeneğine tıklayın.
3. **Hızlı oluştur**’a tıklayıp koleksiyonunuz için bir ad girin.
4. Koleksiyonunuzu oluşturmak için kullanmak istediğiniz bölgeyi seçin. En iyi deneyimi sağlamak için, kullanıcılarınızın uygulamaya erişecekleri konuma coğrafi olarak en yakın bölgeyi seçin. Örneğin, bu öğreticide kullanıcılar Redmond, Washington konumundadır. En yakın Azure bölgesi **Batı ABD**’dir.
5. Kullanmak istediğiniz fatura planını seçin. Temel fatura planı büyük bir Azure VM’ye 16 kullanıcı yerleştirirken, standart fatura planında büyük bir Azure VM’de 10 kullanıcı bulunur. Temel plan, örneğin veri girişi türündeki iş akışlarında harika çalışır. Office gibi bir üretkenlik uygulaması için standart planı tercih etmelisiniz.
6. Son olarak, Office 2013 Professional görüntüsünü seçin. Bu görüntü Office 2013 uygulamalarını içerir. Söz konusu görüntünün yalnızca deneme koleksiyonları ve kavram kanıtları için uygun olduğunu anımsatmak isteriz. Bu görüntüyü bir üretim koleksiyonunda kullanamazsınız.
7. **Create RemoteApp collection** (RemoteApp koleksiyonunu oluştur) seçeneğine tıklayın.

![RemoteApp’te bulut koleksiyonu oluşturma](./media/remoteapp-anyapp/ra-anyappcreatecollection.png)

Bu, koleksiyonunuzun oluşturulmasını başlatır, ancak işlemin tamamlanması bir saat kadar sürebilir.

Artık kullanıcı eklemeye hazırsınız.

## <a name="share-the-app-with-users"></a>Uygulamayı kullanıcılarıyla paylaşma
Koleksiyonunuz başarıyla oluşturulduktan sonra sırada, Access’i kullanıcılara yayımlamak ve Access’e erişimi olması gereken kullanıcıları eklemek vardır.

Koleksiyon oluşturulurken Azure RemoteApp düğümünden ayrıldıysanız ilk olarak Azure giriş sayfasından düğüme geri dönün.

1. Ek seçeneklere erişip koleksiyonu yapılandırmak için daha önce oluşturduğunuz koleksiyona tıklayın.
   ![Yeni bir RemoteApp bulut koleksiyonu](./media/remoteapp-anyapp/ra-anyappcollection.png)
2. **Yayımlama** sekmesinde ekranın alt kısmındaki **Yayımla**’ya tıklayıp ardından **Publish Start menu programs** (Başlat menüsü programlarını yayımla) seçeneğine tıklayın.
   ![RemoteApp programı yayımlama](./media/remoteapp-anyapp/ra-anyapppublish.png)
3. Listeden yayımlamak istediğiniz uygulamaları seçin. Bu konudaki hedefimiz için Access’i seçtik. **Tamamla**’ya tıklayın. Uygulamaları yayımlamanın tamamlanmasını bekleyin.
   ![RemoteApp’te Access’i yayımlama](./media/remoteapp-anyapp/ra-anyapppublishaccess.png)
4. Uygulamanın yayımlanması sona erdiğinde, uygulamalarınıza erişmesi gereken tüm kullanıcıları eklemek için **Kullanıcı Erişimi** sekmesine gidin. Kullanıcılarınızın kullanıcı adlarını (e-posta adresi) girin ve **Kaydet**’e tıklayın.

![RemoteApp’e kullanıcı ekleme](./media/remoteapp-anyapp/ra-anyappaddusers.png)

1. Şimdi, kullanıcılarınıza bu yeni uygulamalardan ve bunlara nasıl erişebileceklerinden bahsetmenin zamanı geldi. Bunu yapmak için kullanıcılarınıza, onları Uzak Masaüstü istemcisi indirme URL'sine yönlendiren bir e-posta gönderin.
   ![RemoteApp için istemci indirme URL'si](./media/remoteapp-anyapp/ra-anyappurl.png)

## <a name="configure-access-to-access"></a>Access’e erişimi yapılandırma
Bazı uygulamaların, RemoteApp aracılığıyla dağıtıldıktan sonra ek yapılandırmaya ihtiyaçları vardır. Özellikle, Access için Azure’da tüm kullanıcıların erişebileceği bir dosya paylaşımı oluşturacağız. (Bunu yapmak istemiyorsanız, kullanıcılarınızın dosya ve bilgilere yerel ağınızda erişimine olanak tanıyan bir [karma koleksiyon](remoteapp-create-hybrid-deployment.md) [bulut koleksiyonumuzun yerine] oluşturabilirsiniz.) Ardından, kullanıcılarımıza bilgisayarlarındaki yerel bir sürücüyü Azure dosya sistemine eşlemelerini söylememiz gerekir.

İlk bölümü yönetici olarak siz gerçekleştirirsiniz. Sonra, kullanıcılarınızın gerçekleştireceği bazı adımlar vardır.

1. İlk olarak komut satırı arabirimini (cmd.exe) yayımlayın. **Yayımlama** sekmesinde **cmd**’yi seçip **Publish > Publish program using path** (Yayımla > Yol kullanarak programı yayımla) seçeneğine tıklayın.
2. Uygulamanın adını ve yolunu girin. Bu konu için, ad olarak "Dosya Gezgini" ve yol olarak "%SYSTEMDRIVE%\windows\explorer.exe" kullanın.
   ![Cmd.exe dosyasını yayımlayın.](./media/remoteapp-anyapp/ra-publishcmd.png)
3. Şimdi bir Azure [depolama hesabı](../storage/storage-create-storage-account.md) oluşturmanız gerekiyor. Hesabımıza "accessdepolama" adını verdik, yani sizin için anlamlı bir ad seçin. (İskoçyalı filminde söyleneni uyarlarsak sadece bir tane "accessdepolama" olabilir.) ![Azure depolama hesabımız](./media/remoteapp-anyapp/ra-anyappazurestorage.png)
4. Depolama alanınızın yolunu (uç nokta konumu) alabilmek için şimdi panonuza geri dönün. Buna kısa bir süre sonra ihtiyacınız olacak, bu nedenle, bir yere kopyaladığınızdan emin olun.
   ![Depolama hesabı yolu](./media/remoteapp-anyapp/ra-anyappstoragelocation.png)
5. Depolama hesabı oluşturulduktan sonra birincil erişim anahtarı gerekir. **Erişim anahtarlarını yönet**’e tıklayın ve birincil erişim anahtarını kopyalayın.
6. Depolama hesabının bağlamını ayarlayın ve Access’e yönelik yeni bir dosya paylaşımı oluşturun. Yükseltilmiş bir Windows PowerShell penceresinde aşağıdaki cmdlet'leri çalıştırın:
   
        $ctx=New-AzureStorageContext <account name> <account key>
        $s = New-AzureStorageShare <share name> -Context $ctx
   
    Yani paylaşımımız için çalıştıracağımız cmdlet'ler şunlardır:
   
        $ctx=New-AzureStorageContext accessstorage <key>
        $s = New-AzureStorageShare <share name> -Context $ctx

Şimdi, kullanıcının sırası geldi. İlk olarak, kullanıcılarınızdan bir [RemoteApp istemcisi](remoteapp-clients.md) yüklemelerini isteyin. Ardından, kullanıcılarınızın oluşturduğunuz Azure dosya paylaşımına hesaplarından bir sürücüyü eşleyip kendi Access dosyalarını eklemeleri gerekir. Bunu aşağıdaki şekilde gerçekleştirirler.

1. RemoteApp istemcisinde, yayımlanan uygulamalara erişin. Cmd.exe programını başlatın.
2. Bilgisayarınızı bir sürücüyü dosya paylaşımına eşlemek için aşağıdaki komutu çalıştırın:
   
        net use z: \\<accountname>.file.core.windows.net\<share name> /u:<user name> <account key>
   
    **/persistent** parametresini evet olarak ayarlarsanız eşlenen sürücü oturumdan oturuma kalıcı olur.
3. Dosya Gezgini uygulamasını RemoteApp’ten başlatın. Paylaşılan uygulamada kullanmak istediğiniz tüm Access dosyalarını dosya paylaşımına kopyalayın.
   ![Azure paylaşımına Access dosyası yerleştirme](./media/remoteapp-anyapp/ra-anyappuseraccess.png)
4. Son olarak, Access’i açıp yeni paylaştığınız veritabanını açın. Verilerinizi buluttan çalıştırılan Access’te görebilmeniz gerekir.
   ![Buluttan çalıştırılan gerçek bir Access veritabanı](./media/remoteapp-anyapp/ra-anyapprunningaccess.png)

Artık bir RemoteApp istemcisi yüklemek şartıyla Access’i tüm cihazlarınızda kullanabilirsiniz.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Sonraki adımlar
Artık koleksiyon oluşturmayı öğrendiğinize göre [Office 365 kullanan bir koleksiyon](remoteapp-tutorial-o365anywhere.md) oluşturmayı deneyin. İsterseniz, yerel ağınıza erişebilen bir [karma koleksiyon](remoteapp-create-hybrid-deployment.md) da oluşturabilirsiniz.

<!--Image references-->


