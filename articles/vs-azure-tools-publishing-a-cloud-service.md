---
title: "Azure araçlarını kullanarak bir bulut hizmeti yayımlama | Microsoft Docs"
description: "Visual Studio kullanarak Azure bulut hizmeti projeleri yayımlama hakkında bilgi edinin."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 1a07b6e4-3678-4cbf-b37e-4520b402a3d9
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 8/14/2017
ms.author: kraigb
ms.openlocfilehash: e617d600dbc8287eea737fc4969833e873365288
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="publishing-a-cloud-service-using-the-azure-tools"></a>Azure araçlarını kullanarak bir bulut hizmeti yayımlama
Microsoft Visual Studio için Azure Araçları'nı kullanarak, Azure uygulamanızı doğrudan Visual Studio'dan yayımlayabilirsiniz. Visual Studio, bulut hizmetinin Hazırlık veya Üretim ortamına tümleşik yayımlamayı destekler.

Azure bir uygulamayı yayımlamadan önce bir Azure aboneliğinizin olması gerekir. Ayrıca, uygulamanız tarafından kullanılacak bir bulut hizmeti ve depolama hesabı ayarlamanız gerekir. Bunlar, ayarlayabilirsiniz [Klasik Azure portalı](http://go.microsoft.com/fwlink/?LinkID=213885).

> [!IMPORTANT]
> Yayımladığınızda, bulut hizmetiniz için dağıtım ortamı seçebilirsiniz. Ayrıca, dağıtım için uygulama paketini depolamak için kullanılan bir depolama hesabı seçmeniz gerekir. Dağıtımdan sonra uygulama paketi depolama hesabından kaldırılır.
> 
> 

Geliştirme ve Azure uygulamanızı test ederken, değişiklikleri web rolleri için artımlı olarak yayımlamak için Web Dağıtımı'ı kullanabilirsiniz. Bir dağıtım ortamı uygulamanıza yayımladıktan sonra Web dağıtımı değişiklikler doğrudan sanal web rolü çalıştıran makineye dağıtmanıza olanak tanır. Paket ve tüm Azure alt uygulamanız değişiklikleri test etmek için web rolü güncelleştirmek istediğiniz her zaman yayımlama gerekmez. Bu yaklaşımda, dağıtım ortamı için yayımlanan uygulamanız için beklemeden test bulutta web rolü değişikliklerinizi kullanılabilir olabilir.

Azure uygulamanızı yayımlamak için ve Web dağıtımı kullanarak bir web rolü güncelleştirmek için aşağıdaki yordamları kullanın:

* Yayımlama veya Visual Studio'dan Azure bir uygulama paketi
* Bir web rolü geliştirme ve Test döngüsünün parçası olarak güncelleştirme

## <a name="publish-or-package-an-azure-application-from-visual-studio"></a>Yayımlama veya Visual Studio'dan Azure bir uygulama paketi
Azure uygulamanızı yayımladığınızda, aşağıdaki görevlerden birini yapabilirsiniz:

* Bir hizmet paketi oluşturun: bir dağıtım ortamından uygulamanızı yayımlamak için bu paketi ve hizmet yapılandırma dosyası kullanabilirsiniz [Klasik Azure portalı](http://go.microsoft.com/fwlink/?LinkID=213885).
* Visual Studio'dan Azure projenizi yayımlayın: uygulamanıza doğrudan Azure yayımlamak için Yayımlama Sihirbazı'nı kullanın. Bilgi için bkz: [Azure uygulaması Yayımlama Sihirbazı](vs-azure-tools-publish-azure-application-wizard.md).

### <a name="to-create-a-service-package-from-visual-studio"></a>Visual Studio'dan bir hizmet paketi oluşturmak için
1. Uygulamanızı yayımlamak hazır olduğunuzda Solution Explorer'ı açın, rollerinizi içeren Azure projesi için kısayol menüsünü açın ve Yayımla seçin.
2. Yalnızca bir hizmet paketi oluşturmak için aşağıdaki adımları izleyin:  
   
   1. Azure projesi için kısayol menüsünden seçin **paket**.
   2. İçinde **paket Azure uygulama** iletişim kutusunda, bir paket oluşturmak istediğiniz hizmet yapılandırmasını seçin ve yapı yapılandırma seçin.
   3. (isteğe bağlı) Yayımladıktan sonra bulut hizmeti için Uzak Masaüstü'nü etkinleştirmek için seçin **tüm roller için Uzak Masaüstü'yi etkinleştirmek** onay kutusunu işaretleyin ve ardından **ayarları** Uzak Masaüstü'nü yapılandırmak için. Yayımladıktan sonra bulut hizmetinde hata ayıklama istiyorsanız, uzak seçerek hata ayıklama etkinleştirme **etkinleştirmek uzaktan hata ayıklayıcı tüm rolleri için**.
      
      Daha fazla bilgi için bkz: [Azure rolleri ile Uzak Masaüstü kullanarak](vs-azure-tools-remote-desktop-roles.md).
   4. Paket oluşturmak için seçtiğiniz **paket** bağlantı.
      
      Dosya Gezgini yeni oluşturulan paket dosyasının konumunu gösterir. Ondan kullanabilmesi için bu konuma kopyalayabilirsiniz [Klasik Azure portalı](http://go.microsoft.com/fwlink/?LinkID=213885).
   5. Bir bulut hizmeti oluşturun ve bir ortam için bu paketi dağıtırken bu paket için dağıtım ortamı yayımlamak için bu konum paket konumu olarak kullanmalısınız [Klasik Azure portalı](http://go.microsoft.com/fwlink/?LinkID=213885).
3. (İsteğe bağlı) Etkinlik günlüğünde satır öğesi için kısayol menüsünde dağıtım işlemini iptal etmeyi **iptal edin ve kaldırma**. Bu dağıtım işlemi durdurur ve Azure'dan dağıtım ortamı siler.
   
   > [!NOTE]
   > Bunu dağıtıldıktan sonra bu dağıtım ortamı kaldırmak için kullanmanız gerekir [Klasik Azure portalı](http://go.microsoft.com/fwlink/?LinkID=213885).
   > 
   > 
4. (İsteğe bağlı) Rolü örneklerinizi başlattıktan sonra Visual Studio otomatik olarak dağıtım ortamında gösterir **bulut Hizmetleri** Sunucu Gezgininde. Buradan, tek tek rol örneklerinin durumunu görebilirsiniz. Bkz: [bulut Gezgini ile yönetme Azure kaynaklarını](vs-azure-tools-resources-managing-with-cloud-explorer.md). Bunlar hala başlatılıyor durumdayken rol örnekleri aşağıda gösterilmiştir:
   
    ![VST_DeployComputeNode](./media/vs-azure-tools-publishing-a-cloud-service/IC744134.png)

## <a name="update-a-web-role-as-part-of-the-development-and-testing-cycle"></a>Bir Web rolü geliştirme ve Test döngüsünün parçası olarak güncelleştirme
Uygulamanızın arka uç altyapısı kararlı olduğundan, ancak web rolleri daha sık sık güncelleştirilmesi gerekmeyen, yalnızca bir web rolü projenizde güncelleştirmek için Web Dağıtımı'ı kullanabilirsiniz. Bu, istemediğinizde derlemenizi ve yeniden dağıtmanızı arka uç çalışan rolleri ya da birden çok web rolleri sahiptir ve web rolleri yalnızca birini güncelleştirmek istiyorsanız kullanışlıdır.

### <a name="requirements"></a>Gereksinimler
Web rolünüz güncelleştirmek için Web Dağıtımı'nı kullanmak için gereksinimler şunlardır:

* **Geliştirme ve sınama amacıyla yalnızca:** web rolü çalıştığı sanal makineye doğrudan yapılan değişiklikler. Bu sanal makine geri dönüştürülmesi gerekiyorsa, yayımladığınız özgün paketinin sanal makine rolü için yeniden oluşturmak için kullanıldığından değişiklikler kaybolur. Web rolü için en son değişiklikleri almak için uygulamanızı yeniden yayımlamanız gerekir.
* **Yalnızca web rolleri güncelleştirilebilir:** çalışan rolleri güncelleştirilemez. Ayrıca, web role.cs içinde RoleEntryPoint güncelleştirilemiyor.
* **Web rolü tek bir örneği yalnızca destekleyebilir:** dağıtım ortamınızda herhangi bir web rolü birden fazla örneğini içeremez. Ancak, birden çok web rolleri her yalnızca bir örneği ile desteklenir.
* **Uzak Masaüstü bağlantılarını etkinleştirmeniz gerekir:** Web dağıtımı kullanıcı adı ve parola değişiklikleri Internet Information Services (IIS) çalıştıran sunucuya dağıtmak için bu sanal makineye bağlanmak için kullanabilmesi için bu gereklidir. Ayrıca, bu sanal makinede IIS için güvenilen bir sertifika eklemek için sanal makineye bağlanmak gerekebilir. (Bu uzak bağlantı Web dağıtımı tarafından kullanılan bir IIS güvenli olmasını sağlar.)

Aşağıdaki yordam, kullandığınızı varsayar **Azure uygulamasını Yayımla** Sihirbazı.

### <a name="to-enable-web-deploy-when-you-publish-your-application"></a>Web etkinleştirmek için uygulamanızın yayımladığınızda dağıtımı
1. Etkinleştirmek için **Web dağıtımını etkinleştirin** tüm rolleri onay kutusunu web için Uzak Masaüstü bağlantıları ilk yapılandırmanız gerekir. Seçin **Uzak Masaüstü'nü etkinleştirme** tüm rolleri için ve uzaktan bağlantı için kullanılacak kimlik bilgilerini sağlamanız **uzak masaüstü yapılandırması** görüntülenen kutusunda. Bkz: [Azure rolleri ile Uzak Masaüstü kullanarak](vs-azure-tools-remote-desktop-roles.md) daha fazla bilgi için.
2. Uygulamanızdaki tüm web rolleri için Web Dağıtımı'nı etkinleştirmek için seçin **tüm web rolleri için Web dağıtımını etkinleştirin**.
   
    Sarı bir uyarı üçgen görünür. Web dağıtımı hassas verileri yüklemek için önerilmez varsayılan olarak, güvenilmeyen, kendinden imzalı bir sertifika kullanır. Bu işlem için hassas verileri güvenli hale getirmek gerekirse, Web dağıtımı bağlantılar için kullanılacak bir SSL sertifikası ekleyebilirsiniz. Bu sertifika, güvenilen bir sertifika olması gerekir. Bunun nasıl yapılacağı hakkında daha fazla bilgi için bkz **için olun dağıtmak güvenli Web** bu konuda daha sonra.
3. Seçin **sonraki** göstermek için **Özet** ekran ve ardından **Yayımla** bulut hizmeti dağıtmak için.
   
    Bulut hizmeti yayımlanır. Oluşturulan sanal makine için IIS Web dağıtımı web rolleri yeniden yayımlanması olmadan güncelleştirmek için kullanılabilir böylece etkin Uzak bağlantıları vardır.
   
   > [!NOTE]
   > Web rolü için yapılandırılmış birden fazla örneği varsa, her web rolüyle uygulamanızı yayımlamak için oluşturulan paketi tek örnek için sınırlı olacağını belirten bir uyarı iletisi görüntülenir. Seçin **Tamam** devam etmek için. Gereksinimleri bölümünde belirtildiği gibi her bir rolü yalnızca bir örneği ancak birden fazla web rolünüz olabilir.
   > 
   > 

### <a name="to-update-your-web-role-by-using-web-deploy"></a>Web dağıtımı kullanarak Web rolünüz güncelleştirmek için
1. Web dağıtımı kullanabilmeniz için kod projesi için Visual Studio'da yayımlama, çözümünüzdeki bu proje düğümüne sağ tıklayın ve fareyle istediğiniz, web rollerinin herhangi birinde değişiklik **Yayımla**. **Web'i Yayımla** iletişim kutusu görüntülenir.
2. (İsteğe bağlı) IIS için Uzak bağlantılar için kullanılacak güvenilir bir SSL sertifikası eklediyseniz, temizleyebilirsiniz **izin Güvenilmeyen sertifika** onay kutusu. Web dağıtımı güvenli hale getirmek için bir sertifika ekleme hakkında daha fazla bilgi için bkz **için olun Web dağıtımı güvenli** bu konuda daha sonra.
3. Web Dağıtımı kullanmak için kullanıcı adı ve paket ilk kez yayımlandığında, Uzak Masaüstü bağlantısı için ayarladığınız parolayı Yayımla mekanizması gerekir.
   
   1. İçinde **kullanıcı adı**, kullanıcı adını girin.
   2. İçinde **parola**, parolayı girin.
   3. (İsteğe bağlı) Bu profilde bu parolayı kaydetmek istiyorsanız seçin **Parolayı Kaydet**.
4. Web rolünüz değişiklikleri yayımlamak için seçin **Yayımla**.
   
    Durum satırı görüntüler **başlatılan Yayımla**. Yayımlama tamamlandığında, **başarılı Yayımla** görüntülenir. Değişiklikler, sanal makinenizde web rolü için şimdi dağıtıldığı. Şimdi Azure ortamına değişikliklerinizi test etmek için Azure uygulamanızı başlatabilirsiniz.

### <a name="to-make-web-deploy-secure"></a>Web dağıtımı güvenli hale getirmek için
1. Web dağıtımı hassas verileri yüklemek için önerilmez varsayılan olarak, güvenilmeyen, kendinden imzalı bir sertifika kullanır. Bu işlem için hassas verileri güvenli hale getirmek gerekirse, Web dağıtımı bağlantılar için kullanılacak bir SSL sertifikası ekleyebilirsiniz. Bu sertifika bir sertifika yetkilisinden (CA) elde güvenilen bir sertifika olması gerekir.
   
    Web dağıtımı güvenli her sanal makine için her web rolleri için hale getirmek için web için kullanmak istediğiniz güvenilen sertifika karşıya dağıtmak [Klasik Azure portalı](http://go.microsoft.com/fwlink/?LinkID=213885). Bu sertifika, uygulamanızın yayımladığınızda, web rolü için oluşturulan sanal makineye eklenir emin olur.
2. Uzak bağlantılar için kullanmak üzere IIS'i güvenilir bir SSL sertifikası eklemek için aşağıdaki adımları izleyin:
   
   1. Web rolü çalıştıran sanal makineye bağlanmak için web rolü örneğini seçin **Cloud Explorer** veya **Sunucu Gezgini**ve ardından **Uzak Masaüstü kullanarak bağlanmak** komutu. Sanal makineye bağlanma hakkında ayrıntılı adımlar için bkz: [Azure rolleri ile Uzak Masaüstü kullanarak](vs-azure-tools-remote-desktop-roles.md).
      
      Tarayıcınız yüklemenizi ister bir. RDP dosyası.
   2. Bir SSL sertifikası eklemek için IIS Yöneticisi'nde yönetim hizmeti açın. IIS Yöneticisi'nde açarak SSL'yi **bağlamaları** bağlamak **eylemi** bölmesi. **Site bağlaması Ekle** iletişim kutusu görüntülenir. Seçin **Ekle**ve ardından HTTPS **türü** açılır liste. İçinde **SSL sertifikası** listesinde, bir CA tarafından imzalanmış ve için karşıya SSL sertifikasını seçin [Klasik Azure portalı](http://go.microsoft.com/fwlink/?LinkID=213885). Daha fazla bilgi için bkz: [yönetim hizmeti için bağlantı ayarlarını yapılandır](http://go.microsoft.com/fwlink/?LinkId=215824).
      
      > [!NOTE]
      > Güvenilir bir SSL sertifikası eklerseniz, sarı uyarı üçgen artık görünür **Yayımlama Sihirbazı**.
      > 
      > 

## <a name="include-files-in-the-service-package"></a>Hizmet paketinde dosyaları dahil etme
Bir rol için oluşturulan sanal makinede kullanılabilir; böylece, hizmet paketinde belirli dosyaları dahil gerekebilir. Örneğin, bir .exe veya hizmet paketi için bir başlangıç betiği tarafından kullanılan bir .msi dosyasına eklemek isteyebilirsiniz. Veya bir web rolü ya da çalışan rolü projesi gerektiren derleme eklemeniz gerekebilir. Dosyaları dahil etmek için Azure uygulamanız için çözümü eklenmesi gerekir.

### <a name="to-include-files-in-the-service-package"></a>Hizmet paketinde dosyaları eklemek için
1. Bir hizmet paketine bir derleme eklemek için aşağıdaki adımları kullanın:
   
   1. İçinde **Çözüm Gezgini** başvurulan derlemeyi eksik projesi için proje düğümünü açın.
   2. Derleme projeye eklemek için kısayol menüsünü açın **başvuruları** klasörünü ve ardından **Başvuru Ekle**. Başvuru Ekle iletişim kutusu görüntülenir.
   3. Ekleyin ve ardından istediğiniz başvuru seçin **Tamam** düğmesi.
      
      Başvuru altındaki listesine eklenir **başvuruları** klasör.
   4. Eklediğiniz derleme için kısayol menüsünü açın ve seçin **özellikleri**. **Özellikleri** penceresi görüntülenir.
      
      Bu derleme hizmet paketinde dahil edilecek **kopya yerel listesini** seçin **doğru**.
2. İçinde **Çözüm Gezgini** başvurulan derlemeyi eksik projesi için proje düğümünü açın.
3. Derleme projeye eklemek için kısayol menüsünü açın **başvuruları** klasörünü ve ardından **Başvuru Ekle**. **Başvuru Ekle** iletişim kutusu görüntülenir.
4. Ekleyin ve ardından istediğiniz başvuru seçin **Tamam** düğmesi.
   
    Başvuru altındaki listesine eklenir **başvuruları** klasör.
5. Eklediğiniz derleme için kısayol menüsünü açın ve seçin **özellikleri**. Özellikler penceresi görünür.
6. Bu derleme hizmet paketinde dahil edilecek **kopya yerel** listesinde, seçin **doğru**.
7. Web rolü projeniz için eklenene hizmet paketi dosyaları eklemek için dosya için kısayol menüsünü açın ve ardından **özellikleri**. Gelen **özellikleri** penceresinde, seçin **içerik** gelen **yapı eylemi** liste kutusu.
8. Çalışan rolü projeniz için eklenene hizmet paketi dosyaları eklemek için dosya için kısayol menüsünü açın ve ardından **özellikleri**. Gelen **özellikleri** penceresinde, seçin **yeniyse Kopyala** gelen **çıktı dizinine Kopyala** liste kutusu.

## <a name="next-steps"></a>Sonraki adımlar
Visual Studio'dan Azure'a yayımlama hakkında daha fazla bilgi için bkz [Azure uygulaması Yayımlama Sihirbazı](vs-azure-tools-publish-azure-application-wizard.md).

