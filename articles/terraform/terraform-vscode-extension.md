---
title: Azure Terraform VS Code uzantısı | Microsoft Docs
description: Bu makalede, yükleyip Visual Studio Code'da Terraform uzantısını kullanmayı öğrenin.
keywords: terraform, devops, sanal makine, azure
author: v-mavick
ms.author: v-mavick
ms.date: 08/14/2018
ms.topic: article
ms.prod: vs-code
ms.openlocfilehash: 0c88879faae100372055479ad4edb8c36d8f557d
ms.sourcegitcommit: f057c10ae4f26a768e97f2cb3f3faca9ed23ff1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/17/2018
ms.locfileid: "40190358"
---
# <a name="azure-terraform-vs-code-extension"></a>Azure Terraform VS Code uzantısı

Microsoft Azure Terraform Visual Studio Code (VS Code) uzantısı, Terraform ile Azure kullanarak geliştirme ve test ederken Geliştirici üretkenliğini artırmak için tasarlanmıştır. Uzantı, Terraform komut desteği, Kaynak grafiğin Görselleştirme ve VS Code'un içindeki CloudShell tümleştirme sağlar.

## <a name="what-you-do"></a>Neler

- Açık kaynak HashiCorp Terraform yürütülebilir dosyayı makinenize yükleyin.
- Azure Terraform VS kod uzantısı yerel VS Code yüklemenizi yükleyin.

## <a name="what-you-learn"></a>Öğrenecekleriniz

Bu öğreticide şunları öğrenirsiniz:

- Nasıl Terraform otomatikleştirin ve basitleştirin Azure hizmetleri sağlama.
- Yükleme ve Azure Hizmetleri için Microsoft Terraform VS Code uzantısı kullanma nasıl.
- Yazmayı, planlama ve Terraform planları yürütmek için VS Code kullanma

## <a name="what-you-need"></a>Ne gerekiyor

- Windows 10, Linux veya macOS 10.10 + çalıştıran bir bilgisayar.
- [Visual Studio Code](https://www.bing.com/search?q=visual+studio+code+download&form=EDGSPH&mkt=en-us&httpsmsn=1&refig=dffc817cbc4f4cb4b132a8e702cc19a3&sp=3&ghc=1&qs=LS&pq=visual+studio+code&sk=LS1&sc=8-18&cvid=dffc817cbc4f4cb4b132a8e702cc19a3&cc=US&setlang=en-US).
- Etkin bir Azure aboneliği. [Ücretsiz 30 günlük deneme Microsoft Azure hesabı etkinleştirme](https://azure.microsoft.com/free/).
- Yüklemesini [Terraform](https://www.terraform.io/) yerel makinenizde açık kaynak araçtır.
  
## <a name="prepare-your-dev-environment"></a>Geliştirme ortamınızı hazırlama

### <a name="install-git"></a>Git'i yükleyin

Makaleyi alıştırmalarda tamamlanması için gereken [Git yükleme](https://git-scm.com/).

### <a name="install-hashicorp-terraform"></a>HashiCorp Terraform'u yükleme

HashiCorp yönergeleri [yükleme Terraform](https://www.terraform.io/intro/getting-started/install.html) kapsayan Web sayfası:

- Terraform indiriliyor
- Terraform'u yükleme
- Terraform doğru yüklü olduğu doğrulanıyor

>[!Tip]
>Yönergeleri izlediğinizden emin olun, PATH sistem değişkeni ayarlama ile ilgili.

### <a name="install-nodejs"></a>Node.js yükleme

Terraform Cloud Shell'de kullanmak için yapmanız [Node.js yükleme](https://nodejs.org/) 6.0 ve üstü.

>[!NOTE]
>Node.js yüklü olup olmadığını doğrulamak için bir terminal penceresi açıp: `node -v`

### <a name="install-graphviz"></a>GraphViz yükleyin

Terraform kullanılacak işlevi görselleştirin, yapmanız [GraphViz yükleme](http://graphviz.org/).

>[!NOTE]
>GraphViz yüklü olup olmadığını doğrulamak için bir terminal penceresi açıp: `dot -V`

### <a name="install-the-azure-terraform-vs-code-extension"></a>Azure Terraform VS Code uzantısı yükleyin:

1. VS Code'u başlatın.
2. Tıklayarak **uzantıları** simgesi.

    ![Uzantılar düğmesi](media/terraform-vscode-extension/tf-vscode-extensions-button.png)

3. Kullanım **Market'te arama uzantıları** Azure Terraform uzantısı aramak için metin kutusunu:

    ![VS Code uzantılarını Market içinde Ara](media/terraform-vscode-extension/tf-search-extensions.png)

4. **Yükle**'ye tıklayın.

    >[!NOTE]
    >Tıkladığınızda **yükleme** Azure Terraform uzantısı için VS Code otomatik olarak Azure hesabı uzantısını yükler. Azure hesabı, Azure abonelik kimlik doğrulama ve Azure ile ilgili kod uzantıları gerçekleştirmek için kullandığı Azure Terraform uzantısı için bir bağımlılık dosyasıdır.

#### <a name="verify-the-terraform-extension-is-installed-in-visual-studio-code"></a>Terraform uzantı Visual Studio Code'da yüklendiğini doğrulayın

1. Uzantıları simgesine tıklayın.
2. Tür `@installed` arama metin kutusuna.

    ![Yüklü uzantılar](media/terraform-vscode-extension/tf-installed-extensions.png)

Azure Terraform uzantısı yüklü uzantılar listesinde görünür.

![Terraform yüklü uzantılar](media/terraform-vscode-extension/tf-installed-terraform-extension-button.png)

Artık, VS Code'un içindeki Cloud Shell ortamınızdaki tüm desteklenen Terraform komutları çalıştırabilirsiniz.

## <a name="exercise-1-basic-terraform-commands-walk-through"></a>Alıştırma 1: Temel Terraform komutları gözden geçirme

Bu alıştırmada, oluşturma ve yeni bir Azure kaynak grubu sağlayan bir temel Terraform yapılandırma dosyasını yürütün.

### <a name="prepare-a-test-plan-file"></a>Bir test planı hazırlayın

1. VS Code'da seçin **Dosya > yeni dosya** menü çubuğundan.
2. Gidin [azurerm_resource_group](https://www.terraform.io/docs/providers/azurerm/r/resource_group.html#) ve kodda kopyalayın **örnek kullanım** kod bloğu:
3. Kopyalanan kodun VS Code'da oluşturduğunuz yeni dosyasına yapıştırın.

    ![Örnek kullanım kodu yapıştırın](media/terraform-vscode-extension/tf-paste-example-usage-code.png)

    >[!NOTE]
    >Değişebilir **adı** değerini kaynak grubunu, ancak Azure aboneliğiniz için benzersiz olmalıdır.

4. Menü çubuğundan seçin **Dosya > Farklı Kaydet**.
5. İçinde **Kaydet** iletişim kutusunda, seçtiğiniz bir konuma gidin ve ardından **yeni klasör**. (Yeni klasörün adını daha açıklayıcı bir şey değiştirmek *yeni klasör*.)

    >[!NOTE]
    >Bu örnekte, TERRAFORM-TEST-planı klasör olarak adlandırılır.

6. Yeni klasör vurgulanana emin olun (Seçili) ve ardından **açık**.
7. İçinde **Kaydet** iletişim kutusunda, dosya için varsayılan adı değiştirmek *main.tf*.

    ![Main.tf Kaydet](media/terraform-vscode-extension/tf-save-as-main.png)

8. **Kaydet**’e tıklayın.
- Menü çubuğunda **Dosya > Klasör Aç**. Gidin ve oluşturduğunuz yeni bir klasör seçin.

### <a name="run-terraform-init-command"></a>Terraform çalıştırma *init* komutu

1. Visual Studio Code'u başlatın.
2. VS Code menü çubuğundan seçin **Dosya > Klasör Aç...**  bulun ve seçin, *main.tf* dosya.

    ![Main.tf dosyası](media/terraform-vscode-extension/tf-main-tf.png)

3. Menü çubuğundan seçin **Görüntüle > komut paleti... > Azure Terraform: Init**.
4. Birkaç dakika sonra sorulduğunda *Cloud Shell'i açmak istiyor musunuz?* **Tamam** düğmesine tıklayın.

    ![Cloud Shell'i açmak istiyor musunuz?](media/terraform-vscode-extension/tf-do-you-want-to-open-cloud-shell.png)

5. İlk kez yeni bir klasör Cloud Shell'den başlatıldığında web uygulamasını ayarlama istenir. Tıklayın **açık**.

    ![Cloud Shell ilk kez başlatıldığında](media/terraform-vscode-extension/tf-first-launch-of-cloud-shell.png)

6. Hoş Geldiniz Azure Cloud Shell sayfası açılır. Bash veya PowerShell'i seçin.

    ![Azure Cloud Shell'e hoş geldiniz](media/terraform-vscode-extension/tf-welcome-to-azure-cloud-shell.png)

    >[!NOTE]
    >Bu örnekte, Bash (Linux) seçildi.

7. Zaten bir Azure depolama hesabı ayarlamadıysanız, aşağıdaki ekran görünür. Tıklayın **depolama oluşturma**.

    ![Hiçbir depolama takılı değil](media/terraform-vscode-extension/tf-you-have-no-storage-mounted.png)

1. Azure Cloud Shell, önceden seçili ve sizin için oluşturulan bulut sürücüsü bilgilerini görüntüler Kabuğu'nda başlatılır.

    ![Bulut drive'ınızdaki oluşturuldu](media/terraform-vscode-extension/tf-your-cloud-drive-has-been-created-in.png)

9. Artık Cloud Shell sonlandırılabilir
10. Menü çubuğundan seçin **görünümü** > **komut paleti** > **Azure Terraform: init**.

    ![Terraform başarıyla başlatıldı](media/terraform-vscode-extension/tf-terraform-has-been-successfully-initialized.png)

### <a name="visualize-the-plan"></a>Plan görselleştirin

Bu öğreticide daha önce GraphViz yüklü. Terraform GraphViz yapılandırma veya yürütme planı görsel bir temsilini oluşturmak için kullanabilirsiniz. Bu özelliği aracılığıyla Azure Terraform VS Code uzantısını uygulayan *görselleştirme* komutu.

- Gelen **menü çubuğu**seçin**Görüntüle > komut paleti > Azure Terraform: Görselleştirme**.

    ![Plan görselleştirin](media/terraform-vscode-extension/tf-graph.png)

### <a name="run-terraform-plan-command"></a>Terraform çalıştırma *planı* komutu

Terraform *planı* komutu, istenen bir değişiklik kümesini için yürütme planı yapıp yapmayacağını denetlemek için kullanılır.

>[!NOTE]
>Terraform *planı* gerçek Azure kaynaklarınıza herhangi bir değişiklik yapmaz. Terraform kullanırız gerçekte, planınızda yer alan değişiklik yapmak için *uygulamak* komutu.

- Menü çubuğundan seçin **görünümü** > **komut paleti** > **Azure Terraform: plan**.

    ![Terraform planı](media/terraform-vscode-extension/tf-terraform-plan.png)

### <a name="run-terraform-apply-command"></a>Terraform çalıştırma *uygulamak* komutu

Terraform'un Sonuçlardan memnun sonra *planı*, çalıştırabileceğiniz *uygulamak* komutu.

1. Menü çubuğundan seçin **görünümü** > **komut paleti** > **Azure Terraform: uygulama**.

    ![Terraform Uygula](media/terraform-vscode-extension/tf-terraform-apply.png)

2. Tür *Evet*.

    ![Terraform Evet Uygula](media/terraform-vscode-extension/tf-terraform-apply-yes.png)

### <a name="verify-your-terraform-plan-was-executed"></a>Terraform planınızı yürütülmesi doğrulayın

Yeni Azure kaynak grubunuz başarıyla oluşturulup oluşturulmadığını görmek için:

1. Azure portalı açın.
2. Seçin **kaynak grupları** sol gezinti bölmesinde.

    ![Yeni kaynak doğrulayın](media/terraform-vscode-extension/tf-verify-resource-group-created.png)

Yeni kaynak grubunuz yer alması **adı** sütun.

>[!NOTE]
>Azure portalı pencereniz şu an için açık bıraktığınız; sonraki adımda size bu kullanacaklardır.

### <a name="run-terraform-destroy-command"></a>Terraform çalıştırma *yok* komutu

1. Menü çubuğundan seçin **görünümü** > **komut paleti** > **Azure Terraform: yok**.

    ![Terraform yok](media/terraform-vscode-extension/tf-terraform-destroy.png)

2. Tür *Evet*.

    ![Terraform Evet yok](media/terraform-vscode-extension/tf-terraform-destroy-yes.png)

### <a name="verify-your-resource-group-was-destroyed"></a>Kaynak grubunuz yok edildi doğrulayın

Terraform başarıyla yeni kaynak grubunuz yok doğrulamak için:

1. Tıklayın **Yenile** Azure portalında *kaynak grupları* sayfası.
2. Kaynak grubunuz artık listelenir.

    ![Kaynak grubu yok edildi doğrulayın](media/terraform-vscode-extension/tf-refresh-resource-groups-button.png)

## <a name="exercise-2-terraform-compute-module"></a>Alıştırma 2: Terraform *işlem* Modülü

Bu alıştırmada, Terraform yükleme hakkında bilgi edinmek *işlem* VS Code ortamına modülü.

### <a name="clone-the-terraform-azurerm-compute-module"></a>Terraform azurerm işlem modülü kopyalama

1. Kullanım [bu bağlantıyı](https://github.com/Azure/terraform-azurerm-compute) GitHub üzerinde Terraform Azure Rm işlem modülü erişmek için.
2. Tıklayın **Kopyala veya indir**.

    ![Kopyala veya indir](media/terraform-vscode-extension/tf-clone-with-https.png)

    >[!NOTE]
    >Bu örnekte, bizim klasör olarak adlandırılıyordu *terraform azurerm işlem*.

### <a name="open-the-folder-in-vs-code"></a>VS Code'da klasörü açın

1. Visual Studio Code'u başlatın.
2. Gelen **menü çubuğu**seçin **Dosya > Klasör Aç** gidin ve önceki adımda oluşturduğunuz klasörü seçin.

    ![terraform azurerm işlem klasörü](media/terraform-vscode-extension/tf-terraform-azurerm-compute-folder.png)

### <a name="initialize-terraform"></a>Terraform Başlat

VS Code'un içindeki Terraform komutlarını kullanarak başlamadan önce eklentiler için iki Azure sağlayıcı indirin: rastgele hem de azurerm.

1. VS Code IDE'nin Terminal bölmesine yazın: `terraform init`

    ![terraform init komutu](media/terraform-vscode-extension/tf-terraform-init-command.png)

2. Tür `az login` ve ekrandaki yönergeleri.

### <a name="module-test-lint"></a>Modül test: *lint*

1. Gelen **menü çubuğu**seçin **Görüntüle > komut paleti > Azure Terraform: Test yürütme**.
2. Test türü seçeneklerini listeden seçin **lint**.

    ![Test türü seçin](media/terraform-vscode-extension/tf-select-type-of-test-lint.png)

3. Sorulduğunda *CloudShell açmak istiyor musunuz?* tıklayın **Tamam** ve ekrandaki yönergeleri.

    ![CloudShell açmak istiyor musunuz?](media/terraform-vscode-extension/tf-do-you-want-to-open-cloudshell-small.png)

>[!NOTE]
>Yürüttüğünüzde ya da **lint** veya **uçtan uca** test, Azure kapsayıcı hizmeti gerçek testi gerçekleştirmek için bir test makinesi sağlama için kullanır. Bu nedenle, test sonuçlarınızı genellikle döndürülmesi birkaç dakika sürebilir.

Birkaç dakika sonra Terminal bölmesinde bu örnektekine benzer bir liste görürsünüz:

![Lint test sonuçları](media/terraform-vscode-extension/tf-lint-test-results.png)

### <a name="module-test-end-to-end"></a>Modül test: *uçtan uca*

1. Gelen **menü çubuğu**seçin **Görüntüle > komut paleti > Azure Terraform: Test yürütme**.
2. Test türü seçeneklerini listeden seçin **uçtan uca**.

    ![Test türü seçin](media/terraform-vscode-extension/tf-select-type-of-test-end-to-end.png)

3. Sorulursa *CloudShell açmak istiyor musunuz?* tıklayın **Tamam** ve ekrandaki yönergeleri.

    ![CloudShell açmak istiyor musunuz?](media/terraform-vscode-extension/tf-do-you-want-to-open-cloudshell-small.png)

>[!NOTE]
>Yürüttüğünüzde ya da **lint** veya **uçtan uca** test, Azure kapsayıcı hizmeti gerçek testi gerçekleştirmek için bir test makinesi sağlama için kullanır. Bu nedenle, test sonuçlarınızı genellikle döndürülmesi birkaç dakika sürebilir.

Birkaç dakika sonra Terminal bölmesinde bu örnektekine benzer bir liste görürsünüz:

![Baştan sona test sonuçları](media/terraform-vscode-extension/tf-end-to-end-test-results.png)

## <a name="next-steps"></a>Sonraki adımlar

Visual Studio Code içinde Azure hizmetlerinden birini sağlama Terraform basitleştirebilirsiniz yollardan bazılarını gördünüz. Şimdi bu kaynaklardan bazıları, gözden geçirmek isteyebilirsiniz:
- [Terraform modülü kayıt defteri](https://registry.terraform.io/) için desteklenen sağlayıcılar Azure ve diğer tüm kullanılabilir Terraform modüllerini listeler.

Bu modüllerin her biri için aşağıdaki bilgileri sağlanır:

- Modülün genel özellikleri ve özellikleri açıklaması
- Kullanım örneği
- Test çalıştırması nasıl oluşturacağınızı gösteren yapılandırmaları ve yerel geliştirme makinenizdeki her modülü test
- Bir modülü geliştirme ortamını yerel olarak oluşturup çalıştırmak için izin vermek için bir Dockerfile.
