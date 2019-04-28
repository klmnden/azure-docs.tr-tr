---
title: Yeoman kullanarak Azure'da Terraform temel şablon oluşturma
description: Yeoman kullanarak Azure'da Terraform temel şablon oluşturmayı öğrenin.
services: terraform
ms.service: azure
keywords: terraform, devops, sanal makine, azure, yeoman
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 11/08/2018
ms.openlocfilehash: 7e66f374a1f5f4fb050f366fdad0e787292101f8
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62128191"
---
# <a name="create-a-terraform-base-template-in-azure-using-yeoman"></a>Yeoman kullanarak Azure'da Terraform temel şablon oluşturma

[Terraform](https://docs.microsoft.com/azure/terraform/
), Azure'da kolayca altyapı oluşturmanın yolunu sağlar. [Yeoman](https://yeoman.io/), modül geliştiricisinin Terraform modülleri oluşturma işini büyük ölçüde kolaylaştırırken mükemmel bir *en iyi deneyim* çerçevesi sağlar.

Bu makalede, Yeoman modül oluşturucusunu kullanarak temel Terraform şablonu oluşturma hakkında bilgi edineceksiniz. Ardından, iki farklı yöntemle, yeni Terraform şablonunuzu test etme öğreneceksiniz:

- Bu makalede oluşturduğunuz bir Docker dosyası kullanarak, Terraform modülü çalıştırın.
- Terraform modülünüzde Azure Cloud Shell'de yerel olarak çalıştırın.

## <a name="prerequisites"></a>Önkoşullar

- **Azure aboneliği**: Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.
- **Visual Studio Code'u**: Biz kullanarak [Visual Studio Code](https://www.bing.com/search?q=visual+studio+code+download&form=EDGSPH&mkt=en-us&httpsmsn=1&refig=dffc817cbc4f4cb4b132a8e702cc19a3&sp=3&ghc=1&qs=LS&pq=visual+studio+code&sk=LS1&sc=8-18&cvid=dffc817cbc4f4cb4b132a8e702cc19a3&cc=US&setlang=en-US) Oluşturucu Yeoman tarafından oluşturulan dosyaları incelemek için. Ancak, tercih ettiğiniz herhangi bir kod düzenleyiciyi kullanabilirsiniz.
- **Terraform**: Yüklemesi gerekir [Terraform](https://docs.microsoft.com/azure/virtual-machines/linux/terraform-install-configure ) Yeoman tarafından oluşturulan modülü çalıştırılacak.
- **Docker**: Biz kullanarak [Docker](https://www.docker.com/get-started) modülü çalıştırmak için Yeoman tarafından Oluşturucu oluşturulur. (Tercih ederseniz örnek modülü çalıştırmak için Docker yerine Ruby kullanabilirsiniz.)
- **Go programlama dili**: Yüklemesi gerekir [Git](https://golang.org/) Yeoman tarafından oluşturulan test çalışmalarını bir seferde yazıldığından.

>[!NOTE]
>Bu öğreticideki yordamların birçoğu, komut satırı girişleri içerir. Burada açıklanan adımlar tüm işletim sistemleri ve komut satırı araçları için geçerlidir. Bu örneklerde, PowerShell cloud shell ortamı için yerel ortam ve Git Bash için kullanmak üzere seçtik.

## <a name="prepare-your-environment"></a>Ortamınızı hazırlama

### <a name="install-nodejs"></a>Node.js yükleme

Terraform'u Cloud Shell'de kullanabilmek için [Node.js](https://nodejs.org/en/download/) 6.0+ sürümünü yüklemeniz gerekir.

>[!NOTE]
>Node.js uygulamasının yüklü olup olmadığını belirlemek için bir terminal penceresi açıp `node --version` yazın.

### <a name="install-yeoman"></a>Yeoman’ı yükleme

Bir komut isteminden `npm install -g yo` komutunu girin

![Yeoman’ı yükleme](media/terraform-vscode-module-generator/ymg-npm-install-yo.png)

### <a name="install-the-yeoman-template-for-terraform-module"></a>Terraform modülü için Yeoman şablonunu yükleyin

Bir komut isteminden `npm install -g generator-az-terra-module` komutunu girin.

![generator-az-terra-module yükleyin](media/terraform-vscode-module-generator/ymg-pm-install-generator-module.png)

>[!NOTE]
>Yeoman’ın yüklü olduğunu doğrulamak için bir terminal penceresinden `yo --version` komutunu girin.

### <a name="create-an-empty-folder-to-hold-the-yeoman-generated-module"></a>Yeoman tarafından oluşturulan modülü tutmak için boş bir klasör oluşturun

Yeoman şablonu **geçerli dizinde** dosyaları oluşturur. Bu nedenle, bir dizin oluşturmanız gerekir.

>[!Note]
>Bu boş dizinin $GOPATH/src altına yerleştirilmesi gerekir. Bunu yapmak için yönergeleri [burada](https://github.com/golang/go/wiki/SettingGOPATH) bulabilirsiniz.

Bir komut isteminden:

1. Oluşturmak üzere olduğumuz yeni, boş dizini içermesini istediğiniz üst dizine gidin.
1. `mkdir <new-directory-name>` yazın.

    > [!NOTE]
    > Değiştirin `<new-directory-name>` yeni dizin adı. Biz bu örnekte, yeni dizini `GeneratorDocSample` olarak adlandırdık.

    ![mkdir](media/terraform-vscode-module-generator/ymg-mkdir-GeneratorDocSample.png)

1. `cd <new directory's name>` yazıp **enter** tuşuna basarak yeni dizine gidin.

    ![Yeni dizininize gidin](media/terraform-vscode-module-generator/ymg-cd-GeneratorDocSample.png)

    >[!NOTE]
    >Bu dizinin boş olduğundan emin olmak için `ls` yazın. Bu komutun elde edilen çıktısında listelenmiş hiçbir dosya olmamalıdır.

## <a name="create-a-base-module-template"></a>Temel modül şablonu oluşturma

Bir komut isteminden:

1. `yo az-terra-module` yazın.

1. Ekrandaki yönergeleri izleyerek aşağıdaki bilgileri sağlayın:

    - *Terraform modülü proje adı*

        ![Proje adı](media/terraform-vscode-module-generator/ymg-project-name.png)       

        >[!NOTE]
        >Bu örnekte `doc-sample-module` girdik.

    - *Docker görüntü dosyasını eklemek ister misiniz?*

        ![Docker görüntü dosyası eklensin mi?](media/terraform-vscode-module-generator/ymg-include-docker-image-file.png) 

        >[!NOTE]
        >`y` yazın. Seçerseniz **n**, oluşturulan modülü kod yalnızca yerel modda çalışan destekleyecektir.

3. Oluşturulan dosyaları görüntülemek için `ls` girin.

    ![Oluşturulan dosyaları listeleme](media/terraform-vscode-module-generator/ymg-ls-GeneratorDocSample-files.png)

## <a name="review-the-generated-module-code"></a>Oluşturulan modülü kodunu gözden geçirme

1. Visual Studio Code'u başlatma

1. Menü çubuğundan **Dosya > Klasör Aç**'ı ve oluşturduğunuz klasörü seçin.

    ![Visual Studio Code](media/terraform-vscode-module-generator/ymg-open-in-vscode.png)

Yeoman modül oluşturucusu tarafından oluşturulan dosyalardan bazılarına göz atalım.

>[!Note]
>Bu makalede, Yeoman modül oluşturucusu tarafından oluşturulan main.tf, variables.tf ve outputs.tf dosyalarını kullanacağız. Ancak, kendi modüllerinizi oluştururken bu dosyaları Terraform modülünüzün işlevselliğine uyacak şekilde düzenleyebilirsiniz. Bu dosyaların ve kullanımlarının daha ayrıntılı bir tartışması için bkz. [Terraform Modüllerinde Terratest.](https://mseng.visualstudio.com/VSJava/_git/Terraform?path=%2FTerratest%20Introduction.md&version=GBmaster)

### <a name="maintf"></a>main.tf

*random-shuffle* adlı bir modülü tanımlar. Girdi, *string_list* şeklindedir. Çıktı, permütasyon sayısıdır.

### <a name="variablestf"></a>variables.tf

Modül tarafından kullanılan girdi ve çıktı değişkenlerini tanımlar.

### <a name="outputstf"></a>outputs.tf

Modülün çıktısını tanımlar. Burada, yerleşik bir Terraform modülü olan **random_shuffle** tarafından döndürülen değerdir.

### <a name="rakefile"></a>Rakefile

Derleme adımlarını tanımlar. Bu adımlar şunlardır:

- **Derleme**: Main.tf dosyanın biçimlendirmesini doğrular.
- **Birim**: Oluşturulan modülü çatıyı birim testi için kod içermez. Bir birim testi senaryosu belirtmek isterseniz, o kodu burada eklersiniz.
- **e2e**: Modülün bir uçtan uca testi çalıştırır.

### <a name="test"></a>test

- Test çalışmaları Go dilinde yazılır.
- Testteki tüm kodlar uçtan uca testlerdir.
- Uçtan uca testler **fixture** altında tanımlanan tüm öğeleri sağlamak üzere Terraform kullanır ve sonra **template_output.go** kodundaki çıktıyı önceden tanımlı beklenen değerler ile karşılaştırır.
- **Gopkg.LOCK** ve **Gopkg.toml**: Bağımlılıklarınızı tanımlayın. 

## <a name="test-your-new-terraform-module-using-a-docker-file"></a>Bir Docker dosyası kullanarak yeni Terraform modülünüzde test

>[!NOTE]
>Örneğimizde, modülü yerel bir modül olarak çalıştırıyoruz ve Azure’a gerçek anlamda temas etmiyoruz.

### <a name="confirm-docker-is-installed-and-running"></a>Docker’ın yüklü ve çalışır durumda olduğunu onaylayın

Bir komut isteminden `docker version` komutunu girin.

![Docker sürümü](media/terraform-vscode-module-generator/ymg-docker-version.png)

Elde edilen çıktı, Docker'ın yüklü olduğunu onaylar.

Docker’ın gerçekten çalışır durumda olduğunu onaylamak için `docker info` girin.

![Docker bilgisi](media/terraform-vscode-module-generator/ymg-docker-info.png)

### <a name="set-up-a-docker-container"></a>Docker kapsayıcısı ayarlama

1. Bir komut isteminden şu komutu girin:

    `docker build --build-arg BUILD_ARM_SUBSCRIPTION_ID= --build-arg BUILD_ARM_CLIENT_ID= --build-arg BUILD_ARM_CLIENT_SECRET= --build-arg BUILD_ARM_TENANT_ID= -t terra-mod-example .`.

    **Başarıyla derlendi** iletisi gösterilir.

    ![Başarıyla derlendi](media/terraform-vscode-module-generator/ymg-successfully-built.png)

1. Komut isteminden `docker image ls` komutunu girin.

    Yeni oluşturduğunuz *terra-mod-example* modülünü listede görürsünüz.

    ![Depo sonuçları](media/terraform-vscode-module-generator/ymg-repository-results.png)

    >[!NOTE]
    >Modül adı *terra-mod-example*, yukarıdaki 1. adımda girdiğiniz komutta belirtilmiştir.

1. `docker run -it terra-mod-example /bin/sh` yazın.

    Docker şu anda çalışmaktadır ve `ls` girilerek dosya listelenebilir.

    ![Docker dosyasını listeleme](media/terraform-vscode-module-generator/ymg-list-docker-file.png)

### <a name="build-the-module"></a>Modül oluşturma

1. `bundle install` yazın.

    **Paket tamamlandı** iletisini bekleyin, ardından sonraki adıma geçin.

1. `rake build` yazın.

    ![Rake derlemesi](media/terraform-vscode-module-generator/ymg-rake-build.png)

### <a name="run-the-end-to-end-test"></a>Uçtan uca testi çalıştırın

1. `rake e2e` yazın.

1. Birkaç dakika sonra **BAŞARILI** iletisi görünür.

    ![BAŞARILI](media/terraform-vscode-module-generator/ymg-pass.png)

1. Girin `exit` baştan sona test tamamlamak ve Docker ortamını çıkın.

## <a name="use-yeoman-generator-to-create-and-test-a-module-in-cloud-shell"></a>Oluşturma ve bir modül, Cloud Shell'de test etmek için kullanım Yeoman Oluşturucusu

Önceki bölümde, bir Docker dosyası kullanarak bir Terraform modülü test öğrendiniz. Bu bölümde, Yeoman kullanacağınız oluşturmak ve Cloud Shell'de bir modül sınamak için oluşturucu.

Bir Docker dosyası büyük ölçüde kullanmak yerine cloud Shell kullanarak bu süreci kolaylaştırır. Cloud Shell'i kullanarak:

- Node.js'yi yüklemek gerekmez
- Yeoman yükleme gerekmez
- Terraform'u yükleme gerekmez

Cloud Shell'de önceden yüklenmiş olan tüm bu öğeler.

### <a name="start-a-cloud-shell-session"></a>Cloud Shell oturumu başlatın

1. Aracılığıyla ya da bir Azure Cloud Shell oturumu başlatın [Azure portalında](https://portal.azure.com/), [shell.azure.com](https://shell.azure.com), veya [Azure mobil uygulaması](https://azure.microsoft.com/features/azure-portal/mobile-app/).

1. **Hoş Geldiniz Azure Cloud Shell** sayfası açılır. Seçin **(Linux) Bash**. (Power Shell desteklenmiyor.)

    ![Azure Cloud Shell'e hoş geldiniz](media/terraform-vscode-module-generator/ymg-welcome-to-azure-cloud-shell.png)

    >[!NOTE]
    >Bu örnekte Bash (Linux) seçilmiştir.

1. Azure depolama hesabı oluşturmadıysanız aşağıdaki ekran açılır. **Depolama oluştur**'u seçin.

    ![Hiçbir depolama takılı değil](media/terraform-vscode-module-generator/ymg-you-have-no-storage-mounted.png)

1. Azure Cloud Shell, seçtiğiniz kabukta başlatılır ve sizin adınıza oluşturulan bulut sunucusuna ait bilgiler gösterilir.

    ![Bulut sürücünüz oluşturuldu](media/terraform-vscode-module-generator/ymg-your-cloud-drive-has-been-created-in.png)

### <a name="prepare-a-folder-to-hold-your-terraform-module"></a>Terraform modülünüzde tutmak için bir klasör hazırlama

1. Bu noktada, Cloud Shell'i zaten GOPATH ortam değişkenlerinizdeki sizin için yapılandırmış olmanız. Yolun görmek için girin `go env`.

1. Henüz mevcut değilse $GOPATH klasörü oluşturun: `mkdir ~/go` yazın.

1. $GOPATH klasörde bir klasör oluşturun: `mkdir ~/go/src` yazın. Bu klasör tutun ve farklı proje klasörleri oluşturma, gibi düzenlemek için kullanılan `<your-module-name>` klasör biz sonraki adımda oluşturacaktır.

1. Terraform modülünüzde tutmak için bir klasör oluşturun: `mkdir ~/go/src/<your-module-name>` yazın.

    >[!NOTE]
    >Bu örnekte, seçtik `my-module-name` klasör adı.

1. Modül klasörünüze gidin: Girin `cd ~/go/src/<your-module-name>`

### <a name="create-and-test-your-terraform-module"></a>Oluşturma ve test etme, Terraform Modülü

1. Girin `yo az-terra-module` ve sihirbazdaki yönergeleri izleyin.

    >[!NOTE]
    >Docker dosyaları oluşturmak istiyorsanız, girmeniz istendiğinde `N`.

1. Girin `bundle install` bağımlılıklarını yükleyin.

    **Paket tamamlandı** iletisini bekleyin, ardından sonraki adıma geçin.

1. Girin `rake build` modülünüzde oluşturulacak.

    ![Rake derlemesi](media/terraform-vscode-module-generator/ymg-rake-build.png)

1. Girin `rake e2e` uçtan uca testi çalıştırmak için.

1. Birkaç dakika sonra **BAŞARILI** iletisi görünür.

    ![BAŞARILI](media/terraform-vscode-module-generator/ymg-pass.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Yükleme ve Azure Terraform Visual Studio Code uzantısı kullanma.](https://docs.microsoft.com/azure/terraform/terraform-vscode-extension)
