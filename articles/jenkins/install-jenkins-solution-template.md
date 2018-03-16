---
title: "Azure’da bir Jenkins sunucusu oluşturma"
description: "Jenkins çözüm şablonundan Azure Linux sanal makinesine Jenkins’i yükleyin ve örnek bir Java uygulaması oluşturun."
author: tomarcher
manager: rloutlaw
ms.service: multiple
ms.workload: web
ms.devlang: na
ms.topic: article
ms.date: 03/12/2018
ms.author: tarcher
ms.custom: Jenkins
ms.openlocfilehash: c9f86ab2536d3c598bb8c7084524395b41f18db0
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="create-a-jenkins-server-on-an-azure-linux-vm-from-the-azure-portal"></a>Azure portalından Azure Linux VM'de bir Jenkins sunucusu oluşturma

Bu hızlı başlangıç, Ubuntu Linux VM'de araçları ve eklentileri Azure ile çalışmak için yapılandırılmış [Jenkins](https://jenkins.io)’in nasıl yükleneceğini gösterir. İşlemi tamamladığınızda, Azure‘da çalışan bir Jenkins sunucusuna sahip ve [GitHub](https://github.com)’dan örnek bir Java uygulaması oluşturmuş olursunuz.

## <a name="prerequisites"></a>Önkoşullar

* Bir Azure aboneliği
* Bilgisayarınızın komut satırından (Bash kabuğu veya [PuTTY](http://www.putty.org/) gibi) SSH’ye erişin.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-the-jenkins-vm-from-the-solution-template"></a>Çözüm şablonundan Jenkins VM’si oluşturma
Jenkins burada server temsilciler projeleri çok sayıda konak veya farklı ortamlar için gerekli sağlamak için tek bir Jenkins yüklenmesine izin vermek için bir veya daha fazla aracılar için iş Jenkins derlemeler modelini destekler veya sınar. Bu bölümdeki adımları yüklerken ve Azure üzerinde bir Jenkins sunucusunu yapılandırırken size kılavuzluk.

[!INCLUDE [jenkins-install-from-azure-marketplace-image](../../includes/jenkins-install-from-azure-marketplace-image.md)]

## <a name="connect-to-jenkins"></a>Jenkins’e bağlanma

Sanal makinenize gidin (örneğin, http://jenkins2517454.eastus.cloudapp.azure.com/) web tarayıcınızda. Jenkins konsoluna güvenli olmayan HTTP üzerinden erişilemeyeceğinden Jenkins konsoluna bilgisayarınızdan SSH tüneli kullanarak güvenli bir şekilde erişmek için yönergeler bu sayfada sağlanmıştır.

![Jenkins’in kilidini açma](./media/install-jenkins-solution-template/jenkins-ssh-instructions.png)

Komut satırından `ssh` komutunu kullanarak tüneli ayarlayın. `username` değerini çözüm şablonundan sanal makineyi ayarlarken seçtiğiniz sanal makine yönetici kullanıcısının adıyla değiştirin.

```bash
ssh -L 127.0.0.1:8080:localhost:8080 jenkinsadmin@jenkins2517454.eastus.cloudapp.azure.com
```

Tüneli başlattıktan sonra, yerel makinenizde http://localhost:8080/ adresine gidin. 

İlk parolayı Jenkins VM’ye SSH üzerinden bağlanırken bir komut satırında aşağıdaki komutu çalıştırarak alın.

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

Bu ilk yönetici parolasıyla Jenkins panosunun kilidini ilk kez açın.

![Jenkins’in kilidini açma](./media/install-jenkins-solution-template/jenkins-unlock.png)

Sonraki sayfada **Önerilen eklentileri yükle**’yi seçin ve Jenkins panosuna erişmek için kullanılacak bir Jenkins yönetici kullanıcısı oluşturun.

![Jenkins hazır!](./media/install-jenkins-solution-template/jenkins-welcome.png)

Jenkins sunucusu artık kod oluşturmak için hazırdır.

## <a name="create-your-first-job"></a>İlk işinizi oluşturma

Jenkins konsolundan **Yeni iş oluştur**’u seçin, ardından işi **mySampleApp** olarak adlandırıp **Serbest tarzda proje**seçeneğini belirleyin ve **Tamam**’ı seçin.

![Yeni bir iş oluşturma](./media/install-jenkins-solution-template/jenkins-new-job.png) 

**Kaynak Kod Yönetimi** sekmesini seçin, **Git**’i etkinleştirin ve **Depo URL'si** alanına aşağıdaki URL'yi girin :`https://github.com/spring-guides/gs-spring-boot.git`

![Git deposunu tanımlayın](./media/install-jenkins-solution-template/jenkins-job-git-configuration.png) 

**Yapı** sekmesini ve ardından **Yapı ekleme adımı**’nı ve **Gradle betiğini başlat**’ı seçin. **Gradle sarmalayıcıyı kullan**’ı seçin, ardından **Sarmalayıcı konumu**’na `complete` değerini, **Görevler** için `build` değerini girin.

![Derlemek için Gradle sarmalayıcıyı kullanma](./media/install-jenkins-solution-template/jenkins-job-gradle-config.png) 

**Gelişmiş...**’i seçin ve ardından **Kök Yapı betiği** alanına `complete` değerini girin. **Kaydet**’i seçin.

![Gradle sarmalayıcısı derleme adımında Gelişmiş ayarları belirleme](./media/install-jenkins-solution-template/jenkins-job-gradle-advances.png) 

## <a name="build-the-code"></a>Kodu oluşturma

Kodu derlemek ve örnek uygulamayı paketlemek için **Şimdi Derle**’yi seçin. Yapınız tamamlandığında proje için **Çalışma alanı** bağlantısını seçin.

![Derlemeden JAR dosyasını almak için çalışma alanına göz atma](./media/install-jenkins-solution-template/jenkins-access-workspace.png) 

Yapınızın başarılı olduğunu doğrulamak için `complete/build/libs` konumuna gidin ve `gs-spring-boot-0.1.0.jar` dosyasının olduğundan emin olun. Jenkins sunucunuz artık Azure’da projelerinizi derlemeye hazırdır.

## <a name="next-steps"></a>Sonraki Adımlar

> [!div class="nextstepaction"]
> [Jenkins aracıları olarak Azure VM'leri ekleme](jenkins-azure-vm-agents.md)
