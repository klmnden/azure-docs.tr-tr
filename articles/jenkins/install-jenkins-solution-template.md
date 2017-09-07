---
title: "Azure’da bir Jenkins sunucusu oluşturma"
description: "Jenkins çözüm şablonundan Azure Linux sanal makinesine Jenkins’i yükleyin ve örnek bir Java uygulaması oluşturun."
author: mlearned
manager: douge
ms.service: multiple
ms.workload: web
ms.devlang: java
ms.topic: hero-article
ms.date: 08/21/2017
ms.author: mlearned
ms.custom: Jenkins
ms.translationtype: HT
ms.sourcegitcommit: 7456da29aa07372156f2b9c08ab83626dab7cc45
ms.openlocfilehash: 7bb74f297d52fb25171817175cce64187b397c38
ms.contentlocale: tr-tr
ms.lasthandoff: 08/28/2017

---

# <a name="create-a-jenkins-server-on-an-azure-linux-vm-from-the-azure-portal"></a>Azure portalından Azure Linux VM'de bir Jenkins sunucusu oluşturma

Bu hızlı başlangıç, Ubuntu Linux VM'de araçları ve eklentileri Azure ile çalışmak için yapılandırılmış [Jenkins](https://jenkins.io)’in nasıl yükleneceğini gösterir. İşlemi tamamladığınızda, Azure‘da çalışan bir Jenkins sunucusuna sahip ve [GitHub](https://github.com)’dan örnek bir Java uygulaması oluşturmuş olursunuz.

## <a name="prerequisites"></a>Ön koşullar

* Bir Azure aboneliği
* Bilgisayarınızın komut satırından (Bash kabuğu veya [PuTTY](http://www.putty.org/) gibi) SSH’ye erişin.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-the-jenkins-vm-from-the-solution-template"></a>Çözüm şablonundan Jenkins VM’si oluşturma

Web tarayıcınızda [Jenkins için mağaza görüntüsü](https://azuremarketplace.microsoft.com/marketplace/apps/azure-oss.jenkins?tab=Overview) adresine gidin ve sayfanın sol tarafındaki **ŞİMDİ EDİN** bağlantısını seçin. Fiyatlandırma ayrıntılarını gözden geçirin ve **Devam**’ı seçin, ardından Azure portalında Jenkins sunucusunu yapılandırmak için **Oluştur**’u seçin. 
   
![Azure portalı iletişim kutusu](./media/install-jenkins-solution-template/ap-create.png)

**Temel ayarları yapılandırma** sekmesinde, aşağıdaki alanları doldurun:

![Temel ayarları yapılandırma](./media/install-jenkins-solution-template/ap-basic.png)

* **Ad** olarak **Jenkins** yazın.
* Bir **Kullanıcı adı** girin. Kullanıcı adı [belirli gereksinimleri](/azure/virtual-machines/linux/faq#what-are-the-username-requirements-when-creating-a-vm) karşılamalıdır.
* **Kimlik doğrulama türü** olarak **Parola**’yı seçin ve bir parola girin. Parola bir büyük harf karakter, bir sayı ve bir özel karakter içermelidir.
* **Kaynak grubu** için **myJenkinsResourceGroup** değerini kullanın.
* **Konum** açılır menüsünden **Doğu ABD** [Azure bölgesini](https://azure.microsoft.com/regions/) seçin.

**Ek seçenekleri yapılandırma** sekmesine geçmek için **Tamam**’ı seçin. Jenkins sunucusunu belirtecek benzersiz bir etki alanı adı girin ve **Tamam**’ı seçin.

![Ek seçenekleri ayarlama](./media/install-jenkins-solution-template/ap-addtional.png)  

 Doğrulama başarılı olduktan sonra **Özet** sekmesinde tekrar **Tamam**’ı seçin. Son olarak da Jenkins VM’sini oluşturmak için **Satın al**’ı seçin. Sunucunuz hazır olduğunda, Azure portalında bir bildirim alırsınız:   

![Jenkins hazır bildirimi](./media/install-jenkins-solution-template/jenkins-deploy-notification-ready.png)

## <a name="connect-to-jenkins"></a>Jenkins’e bağlanma

Web tarayıcınızdan sanal makinenize (örneğin http://jenkins2517454.eastus.cloudapp.azure.com/) gidin. Jenkins konsoluna güvenli olmayan HTTP üzerinden erişilemeyeceğinden Jenkins konsoluna bilgisayarınızdan SSH tüneli kullanarak güvenli bir şekilde erişmek için yönergeler bu sayfada sağlanmıştır.

![Jenkins’in kilidini açma](./media/install-jenkins-solution-template/jenkins-ssh-instructions.png)

Komut satırından `ssh` komutunu kullanarak tüneli ayarlayın. `username` değerini çözüm şablonundan sanal makineyi ayarlarken seçtiğiniz sanal makine yönetici kullanıcısının adıyla değiştirin.

```bash
ssh -L 127.0.0.1:8080:localhost:8080 jenkinsadmin@jenkins2517454.eastus.cloudapp.azure.com
```

Tüneli başlattıktan sonra, yerel makinenizde http://localhost:8080/ adresine gidin. 

İlk parolayı Jenkins VM’ye SSH üzerinden bağlanırken bir komut satırında aşağıdaki komutu çalıştırarak alın.

```bash
`sudo cat /var/lib/jenkins/secrets/initialAdminPassword`.
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

