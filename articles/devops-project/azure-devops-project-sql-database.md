---
title: Azure DevOps Projesiyle ASP.NET Uygulamanızı ve Azure SQL Veritabanınızı dağıtma | VSTS Öğreticisi
description: DevOps Projesi, Azure’u kullanmaya başlamayı kolaylaştırır. Azure DevOps projesi, ASP.NET ile birkaç hızlı adımda Azure SQL Veritabanı'nın dağıtılmasını kolaylaştırır.
ms.author: mlearned
ms.manager: douge
ms.prod: devops
ms.technology: devops-cicd
ms.topic: tutorial
ms.date: 07/09/2018
author: mlearned
monikerRange: vsts
ms.openlocfilehash: ba15a57f92ad58a46732444890d9e670fcc75cf3
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37967421"
---
# <a name="tutorial--deploy-your-aspnet-app-and-azure-sql-database-with-the-azure-devops-project"></a>Öğretici: Azure DevOps Projesiyle ASP.NET Uygulamanızı ve Azure SQL Veritabanınızı dağıtma

Azure DevOps Projesi, mevcut kodunuzu ve Git deponuzu getirdiğiniz ya da Azure’da bir sürekli tümleştirme (CI) ve sürekli teslim (CD) işlem hattı oluşturmak üzere örnek uygulamalardan birini seçtiğiniz basitleştirilmiş bir deneyim sunar.  DevOps projesi Azure SQL Veritabanı gibi Azure kaynaklarını otomatik olarak oluşturur, CI için derleme tanımı içeren bir VSTS yayın işlem hattı oluşturur ve yapılandırır, CD için yayın tanımı oluşturur ve ardından izleme için Azure Application Insights kaynağı oluşturur.

Yapacaklarınız:

> [!div class="checklist"]
> * ASP.NET Uygulaması ve Azure SQL Veritabanı için Azure DevOps projesi oluşturma
> * VSTS'yi ve Azure aboneliğini yapılandırma 
> * VSTS CI Derleme tanımını inceleme
> * VSTS CD Release Management tanımını inceleme
> * VSTS'deki değişiklikleri işleme ve Azure'a otomatik olarak dağıtma
> * Azure SQL Server Veritabanına bağlanma 
> * Kaynakları temizleme

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/) aracılığıyla ücretsiz bir abonelik alabilirsiniz.

## <a name="create-an-azure-devops-project-for-an-aspnet-app-and-azure-sql-database"></a>ASP.NET Uygulaması ve Azure SQL Veritabanı için Azure DevOps projesi oluşturma

Azure DevOps Projesi VSTS’de CI/CD işlem hattı oluşturur.  **Yeni VSTS** hesabı oluşturabilir veya **mevcut bir hesabı** kullanabilirsiniz.  Azure DevOps Projesi ayrıca, tercih ettiğiniz **Azure aboneliğinde** Azure SQL Veritabanı gibi **Azure kaynakları** oluşturur.

1. [Microsoft Azure portalında](https://portal.azure.com) oturum açın.

1. Sol gezinti çubuğunda **Kaynak oluştur** simgesini seçin ve **DevOps projesini** arayın.  **Oluştur**’u seçin.

    ![Sürekli Teslim Başlatılıyor](_img/azure-devops-project-github/fullbrowser.png)

1. **.NET**'i ve sonra da **İleri**'yi seçin.

1. **Uygulama çerçevesi seçin** alanında **ASP.NET**’i seçin.

1. **Veritabanı ekleyin**’i, sonra da **İleri**’yi seçin.  

1. Önceki adımlarda seçtiğiniz uygulama çerçevesi, burada bulabileceğiniz Azure hizmeti dağıtım hedefinin türünü belirler.  **İleri**’yi seçin.

## <a name="configure-vsts-and-an-azure-subscription"></a>VSTS'yi ve Azure aboneliğini yapılandırma

1. **Yeni** VSTS hesabı oluşturun veya **mevcut** bir hesabı kullanın.  VSTS projeniz için bir **ad** seçin.  

1. Azure **aboneliğinizi** seçin.

1. İsteğe bağlı olarak, diğer Azure yapılandırması ayarlarını görmek için **Değiştir** bağlantısını seçin ve **Veritabanı Sunucusu Oturum Açma Ayrıntıları** bölümünde **kullanıcı adını** tanımlayın.  Bu öğreticinin sonraki adımları için **kullanıcı adını** **saklayın**.
 
1. Yukarıdaki adımı gerçekleştirdiyseniz Azure yapılandırması alanından çıkın ve **Bitti**’yi seçin.  Aksi taktirde **Bitti**’yi seçmeniz yeterlidir.

1. İşlemin tamamlanması birkaç dakika sürer.  Tamamlandıktan sonra, Azure portalına Azure DevOps **proje panosu** yüklenir.  **Azure DevOps Proje Panosu**'na doğrudan **Azure portalı** içindeki **Tüm kaynaklar**'dan da gidebilirsiniz.  Panonun sağ tarafında, çalışan uygulamanızı görmek için **Gözat**’ı seçin.
    
## <a name="examine-the-vsts-ci-build-definition"></a>VSTS CI Derleme tanımını inceleme

Azure DevOps Projesi, VSTS hesabınızda otomatik olarak tam bir VSTS CI/CD işlem hattı yapılandırır.  İşlem hattını inceleyebilir ve özelleştirebilirsiniz.  VSTS derleme tanımına alışmak için aşağıdaki adımları izleyin.

1. **Azure DevOps Projesi panosuna** gidin.

1. **Azure DevOps projesi panosunun** **üst** kısmından **Derleme İşlem Hatları**’nı seçin.  Bu bağlantı, bir tarayıcı sekmesi açar ve yeni projeniz için VSTS derleme tanımını açar.

1. Fare imlecini **Durum** alanının yanındaki derleme tanımının sağına getirin. Görüntülenen **üç noktayı** seçin.  Bu eylem, **yeni derlemeyi kuyruğa alma**, **derlemeyi duraklatma** ve **derleme tanımını düzenleme** gibi bazı etkinlikleri gerçekleştirmenizi sağlayan menüyü açar.

1. **Düzenle**’yi seçin.

1. Bu görünümden derleme tanımınızın **çeşitli görevlerini inceleyin**.  Derleme, VSTS Git deposundan kaynak getirme, bağımlılıkları geri yükleme ve dağıtım için kullanılan çıkışları yayımlama gibi çeşitli görevleri yürütür.

1. Derleme tanımının üst kısmında **derleme tanımı adını** seçin.

1. Derleme tanımınızın **adını** daha açıklayıcı bir şeyle değiştirin.  **Kaydet ve kuyruğa al**’ı ve **Kaydet**’i seçin.

1. Derleme tanımı adınızın altında **Geçmiş**’i seçin.  Derleme için yaptığınız son değişikliklere ait denetim kaydını görürsünüz.  VSTS, derleme tanımında yapılan değişiklikleri izler ve yayınları karşılaştırmanızı sağlar.

1. **Tetikleyiciler**’i seçin.  Azure DevOps projesi otomatik olarak bir CI tetikleyicisi oluşturdu. Depoya yönelik her işleme yeni bir derleme başlatır.  İsteğe bağlı olarak dalları CI işlemine dahil etmeyi veya işlemden hariç tutmayı seçebilirsiniz.

1. **Saklama**’yı seçin.  Senaryonuza bağlı olarak, belirli sayıdaki derlemeleri saklayacak veya kaldıracak ilkeleri belirtebilirsiniz.

## <a name="examine-the-vsts-cd-release-management-definition"></a>VSTS CD Release Management tanımını inceleme

Azure DevOps Projesi, VSTS hesabınızdan Azure aboneliğinize dağıtım için gereken adımları otomatik olarak oluşturur ve yapılandırır.  Bu adımlar Azure aboneliğinde VSTS'nin kimliğini doğrulamak için bir Azure hizmet bağlantısı yapılandırmayı içerir.  Otomasyon bir VSTS Yayın Tanımı da oluşturur ve yayın Azure’a CD sağlar.  VSTS yayın tanımı hakkında daha fazla inceleme yapmak için aşağıdaki adımları izleyin.

1. **Derleme ve Yayın**’ı ve ardından **Yayınlar**’ı seçin.  Azure DevOps projesi Azure'a yönelik dağıtımları yönetmek için bir VSTS yayın oluşturur.

1. Tarayıcının sol tarafında, yayın tanımınızın yanındaki **üç noktayı** ve ardından **Düzenle**’yi seçin.

1. Yayın tanımı, yayın işlemini tanımlayan bir **işlem hattı** içerir.  **Yapıtlar**'ın altında **Bırak**’ı seçin.  Önceki adımlarda incelediğiniz derleme tanımı, yapıt için kullanılan çıkışı üretir. 

1. **Bırak** simgesinin sağ tarafında **Sürekli dağıtım tetikleyicisi** **simgesini** seçin (şimşek şeklindedir).  Bu yayın tanımının etkin bir CD tetikleyicisi vardır.  Tetikleyici, her yeni derleme yapısı kullanılabilir olduğunda bir dağıtım başlatır.  İsteğe bağlı olarak, dağıtımlarınızın el ile yürütme gerektirmesi için tetikleyiciyi devre dışı bırakabilirsiniz. 

1. Azure DevOps projesi rastgele bir SQL parolası ayarlamış ve bu parolayı yayın tanımı için kullanmıştır.  Tarayıcının sol tarafında **Değişkenler**’i seçin. 

1. **Bu adımı yalnızca SQL Server parolasını değiştirdiyseniz gerçekleştirin.**  Tek bir **Parola** değişkeni vardır.  **Değer** metin kutusunun sağındaki **asma kilit** simgesini seçin.  Yeni parolayı **girin** ve **Kaydet**’i seçin.

1. Tarayıcının sol tarafında **Görevler**’i ve **ortamınızı** seçin.  

1. Görevler, dağıtım işleminizin gerçekleştirdiği etkinliklerdir ve **Aşamalar** halinde gruplandırılırlar.  Bu yayın tanımı için tek aşama vardır.  Bu aşamada bir **Azure App Service Dağıtımı** ve **Azure SQL Veritabanı Dağıtımı** görevi bulunur.

1. **Azure SQL Yürüt** görevini seçin ve SQL dağıtımı için kullanılan çeşitli özellikleri inceleyin.  **Dağıtım Paketi** altında **SQL DACPAC dosyası** kullanan görevi gözden kaçırmayın.

1. Tarayıcının sağ tarafında **Yayınları görüntüle**’yi seçin.  Bu görünümde yayın geçmişi gösterilir.

1. Yayınlarınızdan birinin yanındaki **üç nokta** simgesini seçin ve sonra da **Aç**'ı seçin.  Bu görünümde keşfedilebilecek **yayın özeti**, **ilişkili iş öğeleri** ve **Testler** gibi çeşitli menüler vardır.

1. **İşlemeler**'i seçin.  Bu görünümde, belirli bir dağıtımla ilişkilendirilmiş kod işlemeleri gösterilir. Dağıtımlar arasındaki işleme farklılıklarını görmek için yayınları karşılaştırabilirsiniz.

1. **Günlükler**'i seçin.  Günlüklerde, dağıtım işlemiyle ilgili yararlı bilgiler bulunur.  Bunlar hem dağıtım sırasında hem de sonrasında görüntülenebilir.

## <a name="commit-changes-to-vsts-and-automatically-deploy-to-azure"></a>VSTS'deki değişiklikleri işleme ve Azure'a otomatik olarak dağıtma 

 > [!NOTE]
 > Aşağıdaki adımlarda basit bir metin değişikliğiyle CI/CD işlem hattı test edilir.  SQL dağıtım işlemini test etmek için isteğe bağlı olarak tabloda bir SQL Server değişikliği de yapabilirsiniz.

Artık en son çalışmanızı otomatik olarak web sitenize dağıtan bir CI/CD işlemiyle uygulamanız üzerinde bir ekiple birlikte çalışmaya hazırsınız.  VSTS git deposunda yapılan her değişiklik VSTS'de bir derleme başlatır ve bir VSTS Release Management tanımı Azure'da bir dağıtım yürütür.  Değişikliklerinizi deponuza işlemek için aşağıdaki adımları izleyin ve başka teknikler kullanın.  Kod değişiklikleri CI/CD işlemini başlatır ve yeni değişikliklerinizi otomatik olarak Azure’e dağıtır.

1. VSTS menüsünde **Kod**'u seçin ve deponuza gidin.

1. **SampleWebApplication\Views\Home** dizinine gidin, **Index.cshtml** dosyasının yanındaki **üç nokta** simgesini seçin ve sonra da **Düzenle**'yi seçin.

1. Dosyada bir değişiklik yapın; örneğin, **div etiketlerinden** birinin içinde biraz metin ekleyin.  Sağ üst kısımda **İşle**'yi seçin.  Değişikliğinizi göndermek için **İşle**'yi yeniden seçin. 

1. Birkaç dakika içinde **VSTS'de derleme başlatılır** ve ardından değişiklerin dağıtılması için bir yayın yürütülür.  DevOps proje panosuyla veya VSTS hesabınızı kullanarak tarayıcıda **derleme durumunu** izleyebilirsiniz.

1. Yayın tamamlandığında, değişikliklerinizi gördüğünüzü doğrulamak için tarayıcıda **uygulamanızı yenileyin**.

## <a name="connect-to-the-azure-sql-server-database"></a>Azure SQL Server Veritabanına bağlanma

Azure SQL Veritabanı’na bağlanmak için uygun izinlerinizin olması gerekir.

1. Azure DevOps Projesi panosunda, SQL DB ile ilgili yönetim sayfasına gitmek için **SQL Veritabanı**’nı seçin.
   
1. **Sunucu güvenlik duvarı ayarla**’yı ve sonra da **+ İstemci IP'si ekle**’yi seçin.  

1. **Kaydet**’i seçin.  Artık istemci IP’nizin **SQL Server Azure kaynağına** erişmesine izin verilir.

1. **SQL Veritabanı** dikey penceresine geri dönün. 

1. Ekranın sağ tarafında, **SQL Server** ile ilgili yapılandırma sayfasına gitmek için **Sunucu adı**’nı seçin.

1. **Parolayı sıfırla**’yı seçin, **SQL Server yöneticisi oturum açma** parolasını girin ve sonra da **Kaydet**’i seçin.  Bu öğreticinin sonraki adımları için bu parolayı **saklayın**.

1. Azure SQL Server ve Veritabanı'na bağlanmak için **SQL Server Management Studio** veya **Visual Studio** gibi istemci araçlarını artık isteğe bağlı olarak kullanabilirsiniz.  Bağlanmak için **Sunucu adı** özelliğini kullanın.

   Başlangıçta DevOps Projesi'ni yapılandırırken DB kullanıcı adını değiştirmediyseniz, kullanıcı adınız e-posta adresinizin yerel bölümü olur.  Örneğin, e-posta adresiniz johndoe@microsoft.com olduğunda, kullanıcı adınız johndoe olur.

 > [!NOTE]
 > SQL oturum açma parolasını değiştirdiyseniz, VSTS yayın tanımı değişkeninde parolayı **VSTS CD Release Management tanımını inceleme** bölümünde açıklandığı gibi değiştirmelisiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

 > [!NOTE]
 > Aşağıdaki adımlar kaynakları kalıcı olarak siler.  Bu işlevi ancak bilgi istemlerini dikkatle okuduktan sonra kullanın.

Test yapıyorsanız,tahakkuk eden ücretleri ödemekten kaçınmak için kaynakları temizleyebilirsiniz.  Artık gerekli olmadığında, Azure DevOps Projesi panosundaki **Sil** işlevini kullanarak bu öğreticide oluşturulmuş olan Azure SQL Veritabanı kümesini ve ilgili kaynakları silebilirsiniz.  **Dikkatli olun**; sil işlevi hem Azure'da hem de VSTS'de Azure DevOps Projesi tarafından oluşturulan verileri yok eder ve yok edildikten sonra bu verileri geri alamazsınız.

1. **Azure portalında** **Azure DevOps Projesi**'ne gidin.
2. Panonun **sağ üst** tarafında **Sil**'i seçin.  İstemi okuduktan sonra, kaynakları **kalıcı olarak silmek** için **Evet**'i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Ekibinizin gereksinimlerine uygun olarak bu derleme ve yayın tanımlarını istediğiniz gibi değiştirebilirsiniz. Ayrıca bu CI/CD desenini diğer projelerinizde şablon olarak kullanabilirsiniz.  Şunları öğrendiniz:

> [!div class="checklist"]
> * ASP.NET Uygulaması ve Azure SQL Veritabanı için Azure DevOps projesi oluşturma
> * VSTS'yi ve Azure aboneliğini yapılandırma 
> * VSTS CI Derleme tanımını inceleme
> * VSTS CD Release Management tanımını inceleme
> * VSTS'deki değişiklikleri işleme ve Azure'a otomatik olarak dağıtma
> * Azure SQL Server Veritabanına bağlanma 
> * Kaynakları temizleme

VSTS işlem hattı hakkında daha fazla bilgi için şu öğreticiye bakın:

> [!div class="nextstepaction"]
> [CD işlemini özelleştirme](https://docs.microsoft.com/vsts/pipelines/release/define-multistage-release-process?view=vsts)

## <a name="videos"></a>Videolar

> [!VIDEO https://channel9.msdn.com/Events/Build/2018/BRK3308/player]