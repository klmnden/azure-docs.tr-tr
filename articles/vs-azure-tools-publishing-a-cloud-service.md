---
title: Azure araçlarını kullanarak bir bulut hizmeti yayımlama | Microsoft Docs
description: Visual Studio kullanarak Azure bulut hizmeti projeleri yayımlama hakkında bilgi edinin.
services: visual-studio-online
author: ghogen
manager: douge
assetId: 1a07b6e4-3678-4cbf-b37e-4520b402a3d9
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.workload: azure
ms.topic: conceptual
ms.date: 11/11/2017
ms.author: ghogen
ms.openlocfilehash: bde00dfbf4a7ffde90d1a9a3d57d3a2decf74cad
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="publishing-a-cloud-service-using-visual-studio"></a>Visual Studio kullanarak bir bulut hizmeti yayımlama

Visual Studio, bir uygulamayı doğrudan bir bulut hizmeti hazırlama ve üretim ortamları için destek ile Azure'a yayımlayabilirsiniz. Yayımlama sırasında dağıtım ortamı ve dağıtım paketi için geçici olarak kullanılan bir depolama hesabı seçin.

Geliştirme ve Azure uygulamanızı test etme, değişiklikleri web rolleri için artımlı olarak yayımlamak için Web Dağıtımı'ı kullanabilirsiniz. Bir dağıtım ortamı uygulamanıza yayımladıktan sonra Web dağıtımı değişiklikler doğrudan sanal web rolü çalıştıran makineye dağıtmanıza olanak tanır. Paket ve tüm Azure alt uygulamanız değişiklikleri test etmek için web rolü güncelleştirmek istediğiniz her zaman yayımlama gerekmez. Bu yaklaşımda, uygulamanız için dağıtım ortamı yayımlanan sahip beklemeden test bulutta web rolü değişikliklerinizi kullanılabilir olabilir.

Azure uygulamanızı yayımlamak için ve Web dağıtımı kullanarak bir web rolü güncelleştirmek için aşağıdaki yordamları kullanın:

- Yayımlama veya Visual Studio'dan Azure bir uygulama paketi
- Bir web rolü geliştirme ve Test döngüsünün parçası olarak güncelleştirme

## <a name="publish-or-package-an-azure-application-from-visual-studio"></a>Yayımlama veya Visual Studio'dan Azure bir uygulama paketi

Azure uygulamanızı yayımladığınızda, aşağıdaki görevlerden birini yapabilirsiniz:

- Bir hizmet paketi oluşturun: bir dağıtım ortamından uygulamanızı yayımlamak için bu paketi ve hizmet yapılandırma dosyası kullanabilirsiniz [Azure portal](https://portal.azure.com).

- Visual Studio'dan Azure projenizi yayımlayın: uygulamanıza doğrudan Azure yayımlamak için Yayımlama Sihirbazı'nı kullanın. Bilgi için bkz: [Azure uygulaması Yayımlama Sihirbazı](vs-azure-tools-publish-azure-application-wizard.md).

### <a name="to-create-a-service-package-from-visual-studio"></a>Visual Studio'dan bir hizmet paketi oluşturmak için

1. Uygulamanızı yayımlamak hazır olduğunuzda Solution Explorer'ı açın, rollerinizi içeren Azure projesi için kısayol menüsünü açın ve Yayımla seçin.

1. Yalnızca bir hizmet paketi oluşturmak için aşağıdaki adımları izleyin:

   a. Azure projesi için kısayol menüsünden seçin **paket**.

   b. İçinde **paket Azure uygulama** iletişim kutusunda, bir paket oluşturmak istediğiniz hizmet yapılandırmasını seçin ve yapı yapılandırma seçin.

   c. (İsteğe bağlı) Yayımladıktan sonra bulut hizmeti için Uzak Masaüstü'nü etkinleştirmek için seçin **tüm roller için Uzak Masaüstü'yi etkinleştirmek**ve ardından **ayarları** Uzak Masaüstü kimlik bilgilerini yapılandırmak için. Daha fazla bilgi için bkz: [etkinleştirmek Uzak Masaüstü bağlantısı için Visual Studio kullanarak Azure Cloud Services rolünde](cloud-services/cloud-services-role-enable-remote-desktop-visual-studio.md).

      Yayımladıktan sonra bulut hizmetinde hata ayıklama istiyorsanız, uzak seçerek hata ayıklama etkinleştirme **etkinleştirmek uzaktan hata ayıklayıcı tüm rolleri için**.

   d. Paket oluşturmak için seçtiğiniz **paket** bağlantı.

      Dosya Gezgini yeni oluşturulan paket dosyasının konumunu gösterir. Böylece Azure portalından kullanır, bu konuma kopyalayabilirsiniz.

   e. Bu paket için dağıtım ortamı yayımlamak için bir bulut hizmeti oluşturun ve Azure portal ile bir ortam için bu paketi dağıtırken bu konum paket konumu olarak kullanmanız gerekir.

1. (İsteğe bağlı) Etkinlik günlüğünde satır öğesi için kısayol menüsünde dağıtım işlemini iptal etmeyi **iptal edin ve kaldırma**. Bu komut, dağıtım işlemi durdurur ve Azure'dan dağıtım ortamı siler. Dağıtımdan sonra ortamı kaldırmak için Azure portalını kullanın.

1. (İsteğe bağlı) Rolü örneklerinizi başlattıktan sonra Visual Studio otomatik olarak dağıtım ortamında gösterir **bulut Hizmetleri** Sunucu Gezgininde. Buradan, tek tek rol örneklerinin durumunu görebilirsiniz. Bkz: [bulut Gezgini ile yönetme Azure kaynaklarını](vs-azure-tools-resources-managing-with-cloud-explorer.md). Bunlar hala başlatılıyor durumdayken rol örnekleri aşağıda gösterilmiştir:

    ![VST_DeployComputeNode](./media/vs-azure-tools-publishing-a-cloud-service/IC744134.png)

## <a name="update-a-web-role-as-part-of-the-development-and-testing-cycle"></a>Bir web rolü geliştirme ve Test döngüsünün parçası olarak güncelleştirme

Uygulamanızın arka uç altyapısı kararlı olduğundan, ancak web rolleri daha sık sık güncelleştirilmesi gerekmeyen, yalnızca bir web rolü projenizde güncelleştirmek için Web Dağıtımı'ı kullanabilirsiniz. Web dağıtımı istemediğinizde derlemenizi ve yeniden dağıtmanızı arka uç çalışan rolleri ya da birden çok web rolleri sahiptir ve web rolleri yalnızca birini güncelleştirmek istiyorsanız kullanışlıdır.

### <a name="requirements-for-using-web-deploy"></a>Web dağıtımı kullanma gereksinimleri

- **Geliştirme ve sınama amacıyla yalnızca**: web rolü çalıştığı sanal makineye doğrudan yapılan değişiklikler. Bu sanal makine geri dönüştürülmesi gerekiyorsa, yayımladığınız özgün paketinin sanal makine rolü için yeniden oluşturmak için kullanıldığından değişiklikler kaybolur. Web rolü için en son değişiklikleri almak için uygulamanızı yeniden yayımlayın.

- **Yalnızca web rolleri güncelleştirilebilir**: çalışan rolleri güncelleştirilemez. Ayrıca, güncelleştirilemiyor `RoleEntryPoint` içinde `web role.cs`.

- **Web rolü tek bir örneği yalnızca destekleyebilir**: dağıtım ortamınızda herhangi bir web rolü birden fazla örneğini içeremez. Ancak, birden çok web rolleri her yalnızca bir örneği ile desteklenir.

- **Uzak Masaüstü bağlantıları etkinleştirmek**: kullanıcı adı ve parola değişiklikleri Internet Information Services (IIS) çalıştıran sunucuya dağıtmak için bu sanal makineye bağlanmak için kullanılacak Web dağıtımı bu gereksinim sağlar. Ayrıca, bu sanal makinede IIS için güvenilen bir sertifika eklemek için sanal makineye bağlanmak gerekebilir. (Bu sertifika uzak bağlantı için Web dağıtımı tarafından kullanılan bir IIS güvenli olmasını sağlar.)

Aşağıdaki yordam, kullandığınızı varsayar **Azure uygulamasını Yayımla** Sihirbazı.

### <a name="enable-web-deploy-when-you-publish-your-application"></a>Uygulamanızı yayımladığınızda, Web dağıtımı etkinleştir

1. Etkinleştirmek için **tüm web rolleri için Web dağıtımını etkinleştirin** seçeneği, Uzak Masaüstü bağlantılarını yapılandır. Seçin **Uzak Masaüstü'nü etkinleştirme** tüm rolleri için ve uzaktan bağlantı için kullanılan kimlik bilgilerini sağlamanız **uzak masaüstü yapılandırması** görüntülenen kutusunda. Bkz: [etkinleştirmek Uzak Masaüstü bağlantısı için Visual Studio kullanarak Azure Cloud Services rolünde](cloud-services/cloud-services-role-enable-remote-desktop-visual-studio.md).

1. Uygulamanızdaki tüm web rolleri için Web Dağıtımı'nı etkinleştirmek için seçin **tüm web rolleri için Web dağıtımını etkinleştirin**.

    Sarı bir uyarı üçgen görünür. Web dağıtımı hassas verileri yüklemek için önerilmez varsayılan olarak, güvenilmeyen, kendinden imzalı bir sertifika kullanır. Bu işlem için hassas verileri güvenli hale getirmek gerekirse, Web dağıtımı bağlantılar için kullanılacak bir SSL sertifikası ekleyebilirsiniz. Bu sertifika, güvenilen bir sertifika olması gerekir. Daha fazla bilgi için bkz: [dağıtmak web güvenli hale getirin](#make-web-deploy-secure).

1. Seçin **sonraki** göstermek için **Özet** ekran ve ardından **Yayımla** bulut hizmeti dağıtmak için.

    Bulut hizmeti yayımlanır. Oluşturulan sanal makine için IIS Web dağıtımı web rolleri yeniden yayımlanması olmadan güncelleştirmek için kullanılabilir böylece etkin Uzak bağlantıları vardır.

   > [!NOTE]
   > Web rolü için yapılandırılmış birden fazla örneği varsa, her web rolüyle uygulamanızı yayımlamak için oluşturulan paketi tek örnek için sınırlı olduğunu belirten bir uyarı iletisi görüntülenir. Devam etmek için **Tamam**'ı seçin. Gereksinimleri bölümünde belirtildiği gibi her bir rolü yalnızca bir örneği ancak birden fazla web rolünüz olabilir.

### <a name="update-your-web-role-by-using-web-deploy"></a>Web dağıtımı kullanarak web rolünüz güncelleştirin

1. Web dağıtımı kullanabilmeniz için kod projesi için Visual Studio'da yayımlama, çözümünüzdeki bu proje düğümüne sağ tıklayın ve fareyle istediğiniz, web rollerinin herhangi birinde değişiklik **Yayımla**. **Web'i Yayımla** iletişim kutusu görüntülenir.

1. (İsteğe bağlı) IIS için Uzak bağlantılar için kullanılacak güvenilir bir SSL sertifikası eklediyseniz, temizleyebilirsiniz **izin Güvenilmeyen sertifika** onay kutusu. Web dağıtımı güvenli hale getirmek için bir sertifika ekleme hakkında daha fazla bilgi için bkz **için olun Web dağıtımı güvenli** bu makalenin ilerisinde yer.

1. Web dağıtımı kullanabilmeniz için kullanıcı adı ve paket ilk kez yayımlandığında, Uzak Masaüstü bağlantısı için ayarladığınız parolayı Yayımla mekanizması gerekir.

   a. İçinde **kullanıcı adı**, kullanıcı adını girin.

   b. İçinde **parola**, parolayı girin.

   c. (İsteğe bağlı) Bu profilde bu parolayı kaydetmek istiyorsanız seçin **Parolayı Kaydet**.

1. Web rolünüz değişiklikleri yayımlamak için seçin **Yayımla**.

    Durum satırı görüntüler **başlatılan Yayımla**. Yayımlama tamamlandığında, **başarılı Yayımla** görüntülenir. Değişiklikler, sanal makinenizde web rolü için şimdi dağıtıldığı. Şimdi Azure ortamına değişikliklerinizi test etmek için Azure uygulamanızı başlatabilirsiniz.

### <a name="make-web-deploy-secure"></a>Web dağıtımı güvenli hale getirmek

1. Web dağıtımı hassas verileri yüklemek için önerilmez varsayılan olarak, güvenilmeyen, kendinden imzalı bir sertifika kullanır. Bu işlem için hassas verileri güvenli hale getirmek gerekirse, Web dağıtımı bağlantılar için kullanılacak bir SSL sertifikası ekleyebilirsiniz. Bu sertifika bir sertifika yetkilisinden (CA) elde güvenilen bir sertifika olması gerekir.

    Web dağıtımı güvenli her sanal makine için her web rolleri için hale getirmek için web için kullanmak istediğiniz güvenilen sertifika karşıya Azure Portalı'na dağıtma. Bu sertifika, sertifika uygulamanızı yayımladığınızda, web rolü için oluşturduğunuz sanal makineye eklenir emin olur.

1. Uzak bağlantılar için kullanmak üzere IIS'i güvenilir bir SSL sertifikası eklemek için aşağıdaki adımları izleyin:

   a. Web rolü çalıştıran sanal makineye bağlanmak için web rolü örneğini seçin **Cloud Explorer** veya **Sunucu Gezgini**ve ardından **Uzak Masaüstü kullanarak bağlanmak** komutu. Sanal makineye bağlanma hakkında ayrıntılı adımlar için bkz: [etkinleştirmek Uzak Masaüstü bağlantısı için Visual Studio kullanarak Azure Cloud Services rolünde](cloud-services/cloud-services-role-enable-remote-desktop-visual-studio.md). Tarayıcınız yüklemenizi ister bir `.rdp` dosyası.

   b. Bir SSL sertifikası eklemek için IIS Yöneticisi'nde yönetim hizmeti açın. IIS Yöneticisi'nde açarak SSL'yi **bağlamaları** bağlamak **eylemi** bölmesi. **Site bağlaması Ekle** iletişim kutusu görüntülenir. Seçin **Ekle**ve ardından HTTPS **türü** aşağı açılan liste. İçinde **SSL sertifikası** listesinde, bir CA tarafından imzalanmış ve Azure portalına karşıya SSL sertifikasını seçin. Daha fazla bilgi için bkz: [yönetim hizmeti için bağlantı ayarlarını yapılandır](http://go.microsoft.com/fwlink/?LinkId=215824).

      > [!NOTE]
      > Güvenilir bir SSL sertifikası eklerseniz, sarı uyarı üçgen artık görünür **Yayımlama Sihirbazı**.

## <a name="include-files-in-the-service-package"></a>Hizmet paketinde dosyaları dahil etme

Bir rol için oluşturulan sanal makinede kullanılabilir; böylece, hizmet paketinde belirli dosyaları dahil gerekebilir. Örneğin, eklemek istediğiniz bir `.exe` veya bir `.msi` , hizmet paketi için bir başlangıç betiği tarafından kullanılan dosya. Veya bir web rolü ya da çalışan rolü projesi gerektiren derleme eklemeniz gerekebilir. Dosyaları dahil etmek için bunlar Azure uygulamanız için çözümü eklenmesi gerekir.

1. Bir hizmet paketine bir derleme eklemek için aşağıdaki adımları kullanın:

   a. İçinde **Çözüm Gezgini**, başvurulan derlemeyi eksik projesi için proje düğümünü açın.
   b. Derleme projeye eklemek için kısayol menüsünü açın **başvuruları** klasörünü ve ardından **Başvuru Ekle**. Başvuru Ekle iletişim kutusu görüntülenir.
   c. Ekleyin ve ardından istediğiniz başvuru seçin **Tamam**. Başvuru altındaki listesine eklenir **başvuruları** klasör.
   d. Eklediğiniz derleme için kısayol menüsünü açın ve seçin **özellikleri**. **Özellikleri** penceresi görüntülenir.

      Bu derleme hizmet paketinde dahil edilecek **kopya yerel listesini** seçin **doğru**.
1. İçinde **Çözüm Gezgini** başvurulan derlemeyi eksik projesi için proje düğümünü açın.

1. Derleme projeye eklemek için kısayol menüsünü açın **başvuruları** klasörünü ve ardından **Başvuru Ekle**. **Başvuru Ekle** iletişim kutusu görüntülenir.

1. Ekleyin ve ardından istediğiniz başvuru seçin **Tamam** düğmesi.

    Başvuru altındaki listesine eklenir **başvuruları** klasör.

1. Eklediğiniz derleme için kısayol menüsünü açın ve seçin **özellikleri**. Özellikler penceresi görünür.

1. Bu derleme hizmet paketinde dahil edilecek **kopya yerel** listesinde, seçin **doğru**.

1. Web rolü projeniz için eklenene hizmet paketi dosyaları eklemek için dosya için kısayol menüsünü açın ve ardından **özellikleri**. Gelen **özellikleri** penceresinde, seçin **içerik** gelen **yapı eylemi** liste kutusu.

1. Çalışan rolü projeniz için eklenene hizmet paketi dosyaları eklemek için dosya için kısayol menüsünü açın ve ardından **özellikleri**. Gelen **özellikleri** penceresinde, seçin **yeniyse Kopyala** gelen **çıktı dizinine Kopyala** liste kutusu.

## <a name="next-steps"></a>Sonraki adımlar

Visual Studio'dan Azure'a yayımlama hakkında daha fazla bilgi için bkz [Azure uygulaması Yayımlama Sihirbazı](vs-azure-tools-publish-azure-application-wizard.md).
