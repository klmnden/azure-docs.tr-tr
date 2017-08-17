---
title: "Azure’daki bir Linux (Ubuntu) sanal makinesinde ilk Jenkins Ana Şablonunuzu oluşturma"
description: "Jenkins dağıtmak için çözüm şablonundan yararlanın."
services: app-service\web
documentationcenter: 
author: mlearned
manager: douge
editor: 
ms.assetid: 8bacfe3e-7f0b-4394-959a-a88618cb31e1
ms.service: multiple
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 6/7/2017
ms.author: mlearned
ms.custom: Jenkins
ms.translationtype: HT
ms.sourcegitcommit: 80fd9ee9b9de5c7547b9f840ac78a60d52153a5a
ms.openlocfilehash: 06d6d305eb9711768dc62a04726359e6280d1b69
ms.contentlocale: tr-tr
ms.lasthandoff: 08/14/2017

---

# <a name="create-your-first-jenkins-master-on-a-linux-ubuntu-vm-on-azure"></a>Azure’daki bir Linux (Ubuntu) sanal makinesinde ilk Jenkins Ana Şablonunuzu oluşturma

Bu hızlı başlangıçta, bir Linux (Ubuntu 14.04 LTS) sanal makinesinde en son kararlı Jenkins sürümünün yanı sıra Azure ile çalışacak şekilde yapılandırılmış araç ve eklentilerin nasıl yükleneceği açıklanmaktadır. Araçlar şunları içerir:
<ul>
<li>Kaynak denetimi için Git</li>
<li>Güvenli bağlantı için Azure kimlik bilgisi eklentisi</li>
<li>Esnek derleme, test ve sürekli tümleştirme için Azure VM Aracıları</li>
<li>Yapıların depolanması için Azure Depolama eklentisi</li>
<li>Betikleri kullanarak uygulama dağıtmak için Azure CLI</li>
</ul>

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Ücretsiz bir Azure hesabı oluşturun.
> * Azure VM’de çözüm şablonlarından birini kullanarak bir Jenkins Ana Şablonu oluşturun. 
> * Jenkins için ilk yapılandırmayı gerçekleştirin.
> * Önerilen eklentileri yükleyin.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="create-the-vm-in-azure-by-deploying-the-solution-template-for-jenkins"></a>Jenkins için çözüm şablonunu dağıtarak Azure’da sanal makineyi oluşturma

Azure hızlı başlangıç şablonları, Azure’da karmaşık teknolojileri hızlı ve güvenilir bir şekilde dağıtmanıza olanak sağlar.  Azure Resource Manager, uygulamalarınızı [bildirim temelli bir şablon](https://azure.microsoft.com/en-us/resources/templates/?term=jenkins) aracılığıyla sağlamanıza olanak tanır. Tek bir şablonda birden çok hizmeti bağımlılıklarıyla birlikte dağıtabilirsiniz. Uygulama yaşam döngüsünün her aşamasında uygulamanızı tekrar tekrar dağıtmak için aynı şablonu kullanırsınız.

Maliyet seçeneklerini anlamak için bu şablonun [plan ve fiyatlandırma](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/azure-oss.jenkins?tab=Overview) bilgilerini görüntüleyin.

[Jenkins için market görüntüsüne](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/azure-oss.jenkins?tab=Overview) gidip **ŞİMDİ AL**’a tıklayın  

Azure portalında **Oluştur**’a tıklayın.  Bu şablon Resource Manager kullanımı gerektirdiğinden, şablon modeli açılan menüsü devre dışıdır.
   
![Azure portalı iletişim kutusu](./media/install-jenkins-solution-template/ap-create.png)

**Temel ayarları yapılandırma** sekmesinde:

![Temel ayarları yapılandırma](./media/install-jenkins-solution-template/ap-basic.png)

* Jenkins örneğiniz için bir ad sağlayın.
* Bir VM disk türü seçin.  Üretim işleri için daha büyük bir VM; daha iyi performans içinse SSD’yi seçin.  Azure disk türleri hakkında daha fazla bilgiye [buradan](https://docs.microsoft.com/en-us/azure/storage/storage-premium-storage) ulaşabilirsiniz.
* Kullanıcı adı: Uzunluk gereksinimlerini karşılamalı ve ayrılmış sözcükler ya da desteklenmeyen karakterler içermemelidir. "Admin" gibi adlara izin verilmez.  Kullanıcı adı ve parola gereksinimleri hakkında daha fazla bilgi edinmek için [buraya](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/faq) bakın.
* Kimlik doğrulaması türü: Bir parolayla veya [SSH ortak anahtarı](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/ssh-from-windows) ile korunan bir örnek oluşturun. Parola kullanırsanız, parolanın şu gereksinimlerden 3’ünü karşılaması gerekir: Bir küçük harf, bir büyük harf, bir rakam ve bir özel karakter.
* **LTS** olan Jenkins yayın türünü değiştirmeyin
* Bir abonelik seçin.
* Bir kaynak grubu oluşturun veya var olan boş bir kaynak grubunu kullanın. 
* Bir konum seçin.

**Ek seçenekleri yapılandırma** sekmesinde:

![Ek seçenekleri ayarlama](./media/install-jenkins-solution-template/ap-addtional.png)

* Jenkins ana şablonunu benzersiz olarak tanımlamak için bir etki alanı adı etiketi sağlayın.

Bir sonraki adıma geçmek için **Tamam**’a tıklayın. 

Doğrulama başarılı olduğunda, şablonu ve parametreleri indirmek için **Tamam**’a tıklayın. 

Daha sonra, tüm kaynakları sağlamak için **Satın al**’a tıklayın.

## <a name="setup-ssh-port-forwarding"></a>SSH bağlantı noktası iletme kurulumu

Jenkins örneği varsayılan olarak http protokolünü kullanır ve 8080 bağlantı noktası üzerinde dinleme yapar. Kullanıcıların korumasız protokoller üzerinden kimlik doğrulaması yapmaması gerekir.
    
Yerel makinenizde Jenkins kullanıcı arabirimini görüntülemek için bağlantı noktası iletmeyi ayarlayın.

### <a name="if-you-are-using-windows"></a>Windows kullanıyorsanız:

PuTTY aracını yükleyin ve Jenkins’i korumak için parola kullanıyorsanız şu komutu çalıştırın:
```
putty.exe -ssh -L 8080:localhost:8080 <username>@<Domain name label>.<location>.cloudapp.azure.com
```
* Oturum açmak için parolayı girin.

![Oturum açmak için parolayı girin.](./media/install-jenkins-solution-template/jenkins-pwd.png)

SSH kullanıyorsanız şu komutu çalıştırın:
```
putty -i <private key file including path> -L 8080:localhost:8080 <username>@<Domain name label>.<location>.cloudapp.azure.com
```

### <a name="if-you-are-using-linux-or-mac"></a>Linux veya Mac kullanıyorsanız:

Jenkins ana şablonunuzu korumak için bir parola kullanıyorsanız şu komutu çalıştırın:
```
ssh -L 8080:localhost:8080 <username>@<Domain name label>.<location>.cloudapp.azure.com
```
* Oturum açmak için parolayı girin.

SSH kullanıyorsanız şu komutu çalıştırın:
```
ssh -i <private key file including path> -L 8080:localhost:8080 <username>@<Domain name label>.<location>.cloudapp.azure.com
```

## <a name="connect-to-jenkins"></a>Jenkins’e bağlanma
Tünelinizi başlattıktan sonra, yerel makinenizde http://localhost:8080/ adresine gidin.

İlk admin parolasıyla Jenkins panosunun kilidini ilk kez açın.

![Jenkins’in kilidini açma](./media/install-jenkins-solution-template/jenkins-unlock.png)

Belirteç edinmek için SSH ile VM’ye girin ve şunu çalıştırın: `sudo cat /var/lib/jenkins/secrets/initialAdminPassword`.

![Jenkins’in kilidini açma](./media/install-jenkins-solution-template/jenkins-ssh.png)

Önerilen eklentileri yüklemeniz istenir.

Ardından, Jenkins ana şablonunuz için bir yönetici kullanıcı oluşturun.

Jenkins örneğiniz kullanıma hazır! http://\<Oluşturduğunuz örneğin genel DNS adı\> adresine giderek salt okunur görünüme erişebilirsiniz.

![Jenkins hazır!](./media/install-jenkins-solution-template/jenkins-welcome.png)

## <a name="next-steps"></a>Sonraki Adımlar

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Çözüm şablonuyla bir Jenkins Ana Şablonu oluşturdunuz.
> * Jenkins’in ilk yapılandırmasını gerçekleştirdiniz.
> * Eklentileri yüklediniz.

Jenkins ile sürekli tümleştirme için Azure VM Aracılarını nasıl kullanacağınızı öğrenmek için bu bağlantıyı izleyin.

> [!div class="nextstepaction"]
> [Jenkins aracıları olarak Azure VM'leri](jenkins-azure-vm-agents.md)

