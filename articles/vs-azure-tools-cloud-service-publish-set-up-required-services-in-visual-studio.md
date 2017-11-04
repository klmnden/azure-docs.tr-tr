---
title: "Yayımlamak veya Visual Studio'dan Azure bir uygulamayı dağıtmak hazırlama | Microsoft Docs"
description: "Bulut ve depolama hesabı Services'i ayarlamak ve Azure uygulamanızı yapılandırmak için yordamlar hakkında bilgi edinin."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 92ee2f9e-ec49-4c7a-900d-620abe5e9d8a
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: cc4fb87e559f554634ae062a59bee31f0831da64
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="prepare-to-publish-or-deploy-an-azure-application-from-visual-studio"></a>Yayımlamak veya Visual Studio'dan Azure bir uygulamayı dağıtmak hazırlama
## <a name="overview"></a>Genel Bakış
Bir bulut hizmeti projesini yayımlamadan önce aşağıdaki hizmetleri ayarlamanız gerekir:

* A **bulut hizmeti** rollerinizi Azure ortamında çalıştırmak için
* A **depolama hesabı** Blob, kuyruk ve tablo hizmetlerine erişim sağlar.

Kurulum bu hizmetleri ve uygulamanızı yapılandırmak için aşağıdaki yordamları kullanın

## <a name="create-a-cloud-service"></a>Bulut hizmeti oluşturun
Bir bulut hizmeti için Azure yayımlama için ilk rollerinizi Azure ortamında çalışan bir bulut hizmeti oluşturmanız gerekir. Bir bulut hizmetinde oluşturabilirsiniz [Klasik Azure portalı](http://go.microsoft.com/fwlink/?LinkID=213885)bölümünde açıklandığı gibi **Klasik Azure portalı kullanarak bir bulut hizmeti oluşturmak için**, bu konunun devamındaki. Bir bulut hizmeti Visual Studio Yayımlama Sihirbazı'nı kullanarak da oluşturabilirsiniz.

### <a name="to-create-a-cloud-service-by-using-visual-studio"></a>Visual Studio kullanarak bir bulut hizmeti oluşturmak için
1. Azure projesi için kısayol menüsünü açın ve seçin **Yayımla**.

    ![VST_PublishMenu](./media/vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio/vst-publish-menu.png)
2. Olarak oturum yapmadıysanız, oturum, kullanıcı adı ve parolayla Microsoft hesabı veya Azure aboneliğinizle ilişkili kuruluş hesabı için.
3. Seçin **sonraki** ilerletmek için düğmesi **ayarları** sayfası.

    ![Yayımlama Sihirbazı ortak ayarları](./media/vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio/publish-settings-page.png)
4. İçinde **bulut Hizmetleri** listesinde, seçin **Yeni Oluştur**. **Azure hizmetleri oluşturmak** iletişim kutusu görüntülenir.
5. Bulut hizmeti adını girin. Ad hizmeti için URL parçası oluşturur ve bu nedenle genel olarak benzersiz olmalıdır. Adı büyük küçük harfe duyarlı değil.

### <a name="to-create-a-cloud-service-by-using-the-azure-classic-portal"></a>Klasik Azure portalı kullanarak bir bulut hizmeti oluşturmak için
1. Oturum [Klasik Azure portalı](http://go.microsoft.com/fwlink/?LinkId=253103) Microsoft Web sitesinde.
2. (isteğe bağlı) Önceden oluşturduğunuz bulut hizmetlerin bir listesini görüntülemek için sayfanın sol tarafında bulut Hizmetleri bağlantısını seçin.
3. Seçin  **+**  simgesine sol alt köşede ve ardından **bulut hizmeti** menüsünde görüntülenir. İki seçenek, başka bir ekran **hızlı Oluştur** ve **özel Oluştur**, görüntülenir. Seçerseniz **hızlı Oluştur**, yalnızca URL'sini ve burada, fiziksel olarak barındırılacağı bölge belirterek bir bulut hizmeti oluşturabilir. Seçerseniz **özel Oluştur**, bir paketi (.cspkg dosyası), bir yapılandırma (.cscfg) dosyası ve bir sertifika belirterek, bir bulut hizmeti hemen yayımlayabilirsiniz. Kullanarak bulut hizmetinizi yayımlayın istiyorsanız, özel Oluştur gerekli değil **Yayımla** Azure projesinde komutu. **Yayımla** komutu, bir Azure projesi için kısayol menüsünde kullanılabilir.
4. Seçin **hızlı Oluştur** daha sonra bulut hizmeti Visual Studio kullanarak yayımlamak için.
5. Bulut hizmetiniz için bir ad belirtin. Tam URL ve adının yanındaki görüntülenir.
6. Listede, kullanıcılarınızın çoğu bulunduğu bölgeyi seçin.
7. Pencerenin alt kısmındaki seçin **bulut hizmeti oluşturma** bağlantı.

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma
Bir depolama hesabı Blob, kuyruk ve tablo hizmetlerine erişim sağlar. Visual Studio kullanarak bir depolama hesabı oluşturabilirsiniz veya [Klasik Azure portalı](http://go.microsoft.com/fwlink/?LinkId=253103).

### <a name="to-create-a-storage-account-by-using-visual-studio"></a>Visual Studio kullanarak bir depolama hesabı oluşturmak için
1. İçinde **Çözüm Gezgini**, kısayol menüsünü açın **depolama** düğümünü ve ardından **depolama hesabı oluştur**.

    ![Yeni bir Azure depolama hesabı oluşturma](./media/vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio/IC744166.png)
2. Yeni depolama hesabı için aşağıdaki bilgileri girin veya seçin **depolama hesabı oluştur** iletişim kutusu.

   * Depolama hesabı eklemek istediğiniz Azure aboneliği.
   * Yeni depolama hesabı için kullanmak istediğiniz adı.
   * Bölge veya benzeşim grubunda (örneğin, Batı ABD veya Doğu Asya).
   * Coğrafi olarak yedekli gibi depolama hesabı için kullanmak istediğiniz çoğaltma türü.
3. İşiniz bittiğinde seçin **oluşturma**. Yeni depolama hesabı görünür **depolama** listesinde **Sunucu Gezgini**.

### <a name="to-create-a-storage-account-by-using-the-azure-classic-portal"></a>Klasik Azure portalı kullanarak bir depolama hesabı oluşturmak için
1. Oturum [Klasik Azure portalı](http://go.microsoft.com/fwlink/?LinkId=253103) Microsoft Web sitesinde.
2. (İsteğe bağlı) Depolama hesaplarınızı görüntülemek için seçin **depolama** sayfanın sol tarafında panelinde bağlantı.
3. Sayfanın sol alt köşedeki seçin  **+**  simgesi.
4. Görüntülenen menüde seçin **depolama**ve ardından **hızlı Oluştur**.
5. Depolama hesabı benzersiz bir url neden olacak bir ad verin.
6. Bulut hizmetiniz bir ad verin. Tam URL ve adının yanındaki görüntülenir.
7. Bölgeler listesinden kullanıcılarınızın çoğu bulunduğu bir bölge seçin.
8. Coğrafi çoğaltma'yı etkinleştirmek isteyip istemediğinizi belirtin. Coğrafi çoğaltma devre dışı bırakırsanız, veri kaybı olasılığını azaltmak için birden fazla fiziksel konuma kaydedilir. Bu özellik depolama daha maliyetli hale getirir, ancak daha sonra özellik eklemek yerine depolama hesabı oluşturduğunuzda, coğrafi konum sağlayarak maliyetini azaltabilir. Daha fazla bilgi için bkz: [coğrafi çoğaltma](http://go.microsoft.com/fwlink/?LinkId=253108).
9. Pencerenin alt kısmındaki seçin **depolama hesabı oluştur** bağlantı.

Depolama hesabınızı oluşturduktan sonra her Azure storage Hizmetleri ve hesabınız için birincil ve ikincil erişim anahtarları kaynaklara erişmek için kullanabileceğiniz URL'leri görürsünüz. Depolama Hizmetleri karşı yapılan isteklerin kimliğini doğrulamak için bu tuşlarını kullanın.

> [!NOTE]
> İkincil erişim anahtarını depolama hesabınız için birincil erişim anahtarı ile aynı erişim sağlar ve bir yedek olarak birincil erişim anahtarınız riske oluşturulur. Ayrıca, erişim anahtarlarınızı düzenli olarak yeniden önerilir. Birincil anahtarı yeniden oluşturmak ikincil anahtar yeniden oluşturuldu birincil anahtarı kullanım için değiştirebileceğiniz sonra ikincil anahtarı kullanmak için bir bağlantı dizesi ayarı değiştirebilirsiniz.
>
>

## <a name="configure-your-app-to-use-services-provided-by-the-storage-account"></a>Depolama hesabı tarafından sağlanan hizmetlerin kullanmak için uygulamanızı yapılandırma
Oluşturduğunuz Azure storage hizmetleri kullanmak için depolama hizmetleri erişen herhangi bir rolü yapılandırmanız gerekir. Bunu yapmak için Azure projeniz için birden çok hizmet yapılandırması kullanabilirsiniz. Varsayılan olarak, iki Azure projenizde oluşturulur. Birden çok hizmet yapılandırması kullanarak, aynı bağlantı dizesini kodunuzda kullanabilirsiniz ancak her hizmet yapılandırmasında bir bağlantı dizesi için farklı bir değere sahip. Örneğin, çalıştırmak ve Azure storage öykünücüsü kullanarak yerel olarak uygulamanızda hata ayıklama için bir hizmet yapılandırmasını ve Azure uygulamanızı yayımlamak için farklı hizmet yapılandırmasını kullanabilirsiniz. Hizmet yapılandırması hakkında daha fazla bilgi için bkz: [yapılandırma bilgisayarınızı Azure projesi kullanarak birden çok hizmet yapılandırması](vs-azure-tools-multiple-services-project-configurations.md).

### <a name="to-configure-your-application-to-use-services-that-the-storage-account-provides"></a>Uygulamanızı depolama hesabı sağladığı hizmetler kullanacak şekilde yapılandırmak için
1. Visual Studio'da Azure çözümünüzü açın. Çözüm Gezgini'nde, depolama hizmetleri erişen Azure projenizde her rol için kısayol menüsünü açın ve seçin **özellikleri**. Visual Studio düzenleyicisinde rolün adını içeren bir sayfa görüntülenir. Sayfa alanları görüntüler **yapılandırma** sekmesi.
2. Rolü için özellik sayfalarında seçin **ayarları**.
3. İçinde **hizmet yapılandırmasını** listesinde, düzenlemek istediğiniz hizmet yapılandırmasının adı seçin. Tüm bu rol için hizmet yapılandırmalarını değişiklik yapmak istiyorsanız, seçebileceğiniz **tüm yapılandırmaları**.  Hizmet yapılandırması güncelleştirme hakkında daha fazla bilgi için bkz **depolama hesapları için bağlantı dizelerini Yönet** konusunda [Visual Studio ile Azure bulut hizmeti için rolleri yapılandırmak](vs-azure-tools-configure-roles-for-cloud-service.md).
4. Herhangi bir bağlantı dizesi ayarı değiştirmeyi seçerek **...** ayarın yanındaki düğmesi. **Depolama bağlantı dizesi oluştur** iletişim kutusu görüntülenir.
5. Altında **bağlanırken**, seçin **aboneliğinizi** seçeneği.
6. İçinde **abonelik** listesinde, aboneliğinizi seçin. Aboneliklerin listesini istediğiniz bir içermiyorsa, seçin **yayımlama ayarları indirme** bağlantı.
7. İçinde **hesap adı** listesinde, depolama hesabınızın adını seçin. Azure Araçları edinir depolama hesabının kimlik bilgilerini otomatik olarak .publishsettings dosyasını kullanarak. Depolama hesabı kimlik bilgilerinizi el ile belirtmek için **kimlik bilgileri'el ile girilen** seçeneğini ve ardından bu yordama devam edin. Depolama hesabı adı ve birincil anahtardan alabilirsiniz [Klasik Azure portalı](http://go.microsoft.com/fwlink/p/?LinkID=213885). Ayarları el ile seçin, depolama hesabınızı belirtmek istemiyorsanız **Tamam** düğmesi iletişim kutusunu kapatın.
8. Seçin **depolama hesabını girin** kimlik bilgileri bağlantı.
9. İçinde **hesap adı** kutusuna, depolama hesabınızın adını girin.

   > [!NOTE]
   > İçine oturum [Klasik Azure portalı](http://go.microsoft.com/fwlink/?LinkID=213885)ve ardından **depolama** düğmesi. Portal depolama hesaplarının listesini gösterir. Bir hesap seçerseniz, onun için bir sayfa açar. Bu sayfadan depolama hesabının adını kopyalayabilirsiniz. Klasik Portalı'nın önceki bir sürümünü kullanıyorsanız, depolama hesabınızın adını görünür **depolama hesapları** görünümü. Bu ad kopyalamak için içinde vurgulayın **özellikleri** bu pencereyi görüntülemek ve sonra da Ctrl-C tuş'u seçin. Visual Studio'ya adı yapıştırmak için seçin **hesap adı** metin kutusuna ve ardından Ctrl + V tuşlarını seçin.
   >
   >
10. İçinde **hesap anahtarı** kutusunda, birincil anahtarınızı girin veya kopyalayıp buradan [Klasik Azure portalı](http://go.microsoft.com/fwlink/?LinkID=213885).
     Bu anahtar kopyalamak için:

    1. Uygun depolama hesabı için sayfanın alt kısmındaki seçin **anahtarları Yönet** düğmesi.
    2. Üzerinde **anahtarları erişimi Yönet** sayfa için birincil erişim anahtarını metni seçip Ctrl + C tuşlarını seçin.
    3. Azure Araçları'nda, içine anahtarını yapıştırın **hesap anahtarı** kutusu.
    4. Hizmet depolama hesabının nasıl erişecek belirlemek için aşağıdaki seçeneklerden birini seçmeniz gerekir:

       * **HTTP kullanmak**. Bu standart bir seçenektir. Örneğin, `http://<account name>.blob.core.windows.net`.
       * **HTTPS kullanmak** güvenli bir bağlantı için. Örneğin, `https://<accountname>.blob.core.windows.net`.
       * **Özel uç noktaları belirtin** üç hizmetlerinin her biri için. Bu uç noktalar, hizmete alanına sonra yazabilirsiniz.

         > [!NOTE]
         > Özel uç noktaları oluşturursanız, daha karmaşık bir bağlantı dizesi oluşturabilirsiniz. Bu dize biçimi kullandığınızda, depolama hesabınızla Blob hizmeti için kayıtlı bir özel etki alanı adını içeren Depolama Hizmeti uç noktaları belirtebilirsiniz. Aynı zamanda yalnızca tek bir kapsayıcı paylaşılan erişim imzası aracılığıyla blob kaynaklara erişim izni verebilir. Özel uç noktaları oluşturma hakkında daha fazla bilgi için bkz: [Azure Storage bağlantı dizelerini yapılandırma](storage/common/storage-configure-connection-string.md).
         >
         >
11. Bu bağlantı dizesi değişiklikleri kaydetmek üzere seçim yapın **Tamam** düğmesine tıklayın ve ardından **kaydetmek** araç çubuğunda. Bu değişiklikleri kaydettikten sonra bu bağlantı dizesi değerini kodunuzda kullanarak alabileceğiniz [GetConfigurationSettingValue](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.getconfigurationsettingvalue.aspx). Uygulamanızı Azure yayımladığınızda, Azure depolama hesabı bağlantı dizesi için içeren hizmet yapılandırması seçin. Uygulamanızı yayımlandıktan sonra uygulamayı Azure storage hizmetlerine karşı beklendiği gibi çalıştığını doğrulayın

## <a name="next-steps"></a>Sonraki adımlar
Visual Studio'dan için Azure yayımlama uygulamalar hakkında daha fazla bilgi için bkz [yayımlama Azure araçlarını kullanarak bir bulut hizmeti](vs-azure-tools-publishing-a-cloud-service.md).
