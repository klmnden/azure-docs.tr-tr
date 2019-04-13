---
title: Azure DevOps Azure DevTest labs'deki bir görüntü fabrikası çalıştırılan | Microsoft Docs
description: Azure DevTest Labs'de bir özel görüntü fabrikası oluşturma hakkında bilgi edinin.
services: devtest-lab, lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/25/2019
ms.author: spelluru
ms.openlocfilehash: 5a3d6e51a71f6aab742fe042d6e6e281192319a4
ms.sourcegitcommit: 1c2cf60ff7da5e1e01952ed18ea9a85ba333774c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/12/2019
ms.locfileid: "59523027"
---
# <a name="run-an-image-factory-from-azure-devops"></a>Azure DevOps’tan bir görüntü fabrikası çalıştırma
Bu makale, Azure DevOps (eski adıyla Visual Studio Team Services) görüntü Fabrika çalıştırmak için gerekli hazırlıklar kapsar.

> [!NOTE]
> Herhangi bir düzenleme altyapısı çalışır! Azure DevOps zorunlu değildir. Görüntü Fabrika, Windows Görev Zamanlayıcısı, CI/CD sistemlerde kullanarak el ile çalıştırın ve benzeri olabilir, Azure PowerShell betiklerini kullanarak çalıştırın.

## <a name="create-a-lab-for-the-image-factory"></a>Görüntü fabrikası için bir laboratuvar oluşturma
Görüntü Fabrika ilk adımı Azure DevTest Labs'de Laboratuvar oluşturma sağlamaktır. Bu Laboratuvar, burada sanal makineler oluşturmak ve özel görüntüleri kaydetme görüntü Fabrika Laboratuvar olur. Bu Laboratuvar, genel görüntü Fabrika işleminin bir parçası kabul edilir. Bir laboratuvarı oluşturduktan sonra adı daha sonra ihtiyaç duyacağınız kaydettiğinizden emin olun.

## <a name="scripts-and-templates"></a>Komut dosyaları ve şablonları
Ekibiniz için görüntü Fabrika benimsenmesi bir sonraki adım neler olduğunu anlamaktır. Görüntü fabrikası komut dosyaları ve şablonları genel olarak kullanılabilir [DevTest Labs GitHub deposunu](https://github.com/Azure/azure-devtestlab/tree/master/Scripts/ImageFactory). Aşağıda, parçaları bir özetini verilmiştir:

- Görüntü üreteci. Bunu kök klasördür. 
    - Yapılandırma. Görüntü factory'ye giriş
        - GoldenImages. Bu klasör, özel görüntüleri tanımını temsil eden JSON dosyalarını içerir.
        - Labs.json. Dosya, belirli özel görüntüler kullanmak için burada takımlar kaydolun.
- Komut dosyaları. Görüntü fabrikasının altyapısı.

Bu bölümdeki makaleler, bu komut dosyaları ve şablonları hakkında daha fazla ayrıntı sağlar. 

## <a name="create-an-azure-devops-team-project"></a>Bir Azure DevOps takım projesi oluşturma
Azure DevOps, kaynak kodunu depolamak için Azure PowerShell tek bir yerde çalıştırın olanak tanır. Görüntüleri güncel tutmak için yinelenen çalıştırmalarını zamanlayabilirsiniz. Sorunları tanılamak için sonuçları günlüğe kaydetme iyi olanakları vardır.  Azure DevOps, ancak bir gereksinim değildir kullanarak bir bandı/Azure'a bağlanabilir ve Azure PowerShell çalıştırabilirsiniz altyapısı kullanabilirsiniz.

Bir DevOps hesabınız veya bunun yerine kullanmak istediğiniz proje varsa, bu adımı atlayın.

Başlamak için Azure DevOps içinde ücretsiz bir hesap oluşturun. Ziyaret https://www.visualstudio.com/ seçip **ücretsiz olarak kullanmaya başlayın** hemen altında **Azure DevOps** (eski adıyla VSTS). Benzersiz bir hesap adı seçin ve Git kullanarak kod yönetilecek seçtiğinizden emin olun gerekecektir. Bu oluşturulduktan sonra takım projeniz için URL'yi kaydedin. Bir örnek URL şu şekildedir: `https://<accountname>.visualstudio.com/MyFirstProject`.

## <a name="check-in-the-image-factory-to-git"></a>Git için görüntü factory'de denetleyin
Tüm PowerShell, şablonları ve görüntü Fabrika yapılandırması bulunur [genel DevTest Labs GitHub deposunu](https://github.com/Azure/azure-devtestlab/tree/master/Scripts/ImageFactory). Yeni takım projenize kodu almanın en hızlı yolu, bir depo içeri aktarma sağlamaktır. (Ek belgelerine ve örneklerine elde edecekleriniz şekilde) bu tüm DevTest Labs depoda çeker. 

1. Önceki adımda oluşturduğunuz Azure DevOps projesi ziyaret edin (URL şöyle **https:\//\<accountname >.visualstudio.com/MyFirstProject**).
2. Seçin **depoyu içeri aktarın**.
3. Girin **kopya URL'si** DevTest Labs deponun: `https://github.com/Azure/azure-devtestlab`.
4. Seçin **alma**.

    ![Git deposunu içeri aktarma](./media/set-up-devops-lab/import-git-repo.png)

Yalnızca tam olarak hangi (görüntü Fabrika dosyaları) gerekli denetlemek karar verirseniz, adımları [burada](https://www.visualstudio.com/en-us/docs/git/share-your-code-in-git-vs) Git deposunu kopyalayın ve yalnızca bulunan dosyaları göndermek için **betikleri/ImageFactory** dizin.

## <a name="create-a-build-and-connect-to-azure"></a>Bir yapı oluşturmak ve Azure'a bağlanma
Bu noktada, Azure DevOps Git deposunda depolanan kaynak dosyalar içeriyor. Şimdi, Azure PowerShell'i çalıştırmak üzere bir işlem hattı ayarlayın gerekir. Birçok bu adımları uygulamak için seçenek vardır. Bu makalede, kolaylık olması için yapı tanımını kullanacak ancak DevOps sürüm (tek veya birden çok ortamda), Windows Görev Zamanlayıcısı'nı gibi diğer yürütme motoru veya Azure PowerShell yürütebilir başka bir bandı gibi DevOps derleme ile çalışır.

> [!NOTE]
> Akılda tutulması gereken önemli bir nokta bazı PowerShell dosyaları oluşturmak için çok (10 +) özel görüntülerinizi olduğunda çalıştırılacak uzun zaman alıyor. Ücretsiz barındırılan DevOps derleme/yayın aracıları, bir zaman aşımı değerini 30 dakika, sahip, bu nedenle, çok sayıda görüntü oluşturmaya başladığınızda boş bir barındırılan aracı kullanamazsınız. Bu zaman aşımı sınama geçerlidir kullanmaya karar herhangi bir bandı, uzun süre çalışan Azure PowerShell betiklerinin tipik zaman aşımlarını genişletebilirsiniz Önden doğrulamak iyi olur. Azure DevOps söz konusu olduğunda için ücretli bir barındırılan aracıları kullanabilir veya kendi Yapı aracınızı kullanın.

1. Başlamak için seçim **derlemeyi Ayarla** DevOps projenizin giriş sayfasındaki:

    ![Kurulum derleme düğmesi](./media/set-up-devops-lab/setup-build-button.png)
2. Belirtin bir **adı** derleme için (örneğin: Derleme ve görüntüler için DevTest Labs sunar). 
3. Seçin bir **boş** derleme tanımı ve seçin **Uygula** yapı oluşturmak için. 
4. Bu aşamada, seçtiğiniz **barındırılan** derleme aracısı için. 
5. **Kaydet** derleme tanımı.

    ![Derleme tanımı](./media/set-up-devops-lab/build-definition.png)

## <a name="configure-the-build-variables"></a>Yapı değişkenleri yapılandırın
Komut satırı parametreleri basitleştirmek için görüntü Fabrika yapı değişkenleri kümesi için sürücü anahtar değerlerini kapsüller. Seçin **değişkenleri** sekmesine tıkladığınızda birkaç varsayılan değişkenler listesini göreceksiniz. Azure DevOps girmek için değişkenleri listesi aşağıda verilmiştir:


| Değişken adı | Değer | Notlar |
| ------------- | ----- | ----- |
| ConfigurationLocation | / Betikleri/ImageFactory/yapılandırma | Bu tam bir depoya yoludur **yapılandırma** klasör. Yukarıdaki tüm depo aldıysanız, sola doğru değeridir. Aksi durumda yapılandırma konumuna işaret edecek şekilde güncelleştirin. |
| DevTestLabName | MyImageFactory | Fabrika görüntüleri oluşturmak için kullanılan Azure DevTest Labs Laboratuvar adı. Yoksa, bir tane oluşturun. Laboratuar hizmet uç noktası erişimi olan aynı abonelikte olduğundan emin olun. |
| ImageRetention | 1 | Her tür kaydetmek istediğiniz görüntü sayısı. Varsayılan değer 1 olarak ayarlayın. |
| MachinePassword | ******* | Sanal makineler için yerleşik yönetici hesabı parolası. Bu geçici bir hesap, bu nedenle güvenli olduğundan emin olun. Güvenli bir dize olduğundan emin olmak için sağ taraftaki küçük kilit simgesini seçin. |
| MachineUserName | ImageFactoryUser | Yerleşik yönetici hesabı kullanıcı adı sanal makineler için. Bu geçici bir hesaptır. |
| StandardTimeoutMinutes | 30 | Biz normal Azure işlemleri için beklemesi gereken zaman aşımı. |
| SubscriptionId |  0000000000-0000-0000-0000-0000000000000 | Burada Laboratuvar var olduğundan ve erişimi hizmet uç noktası olan abonelik kimliği. |
| VMSize | Standard_A3 | Kullanılacak sanal makine boyutu **Oluştur** adım. Oluşturulan VM'ler geçicidir. Boyut biri olmalıdır bu [Laboratuvar için etkin](devtest-lab-set-lab-policy.md). Yeterli olup olmadığını onaylamak [abonelik çekirdek kotasına](../azure-subscription-service-limits.md). 

![Yapı değişkenleri](./media/set-up-devops-lab/configure-build-variables.png)

## <a name="connect-to-azure"></a>Azure'a Bağlanma
Hizmet sorumlusu ayarlamak üzere sonraki adımdır bakın. Azure Active Directory'de kullanıcı adına Azure'da çalışan DevOps yapı aracısının sağlayan bir kimlik budur. Bunu ayarlamak için ilk Azure PowerShell derleme adımı ekleme ile başlayın.

1. Seçin **görevi**.
2. Arama **Azure PowerShell**. 
3. Bulduğunuzda, seçin **Ekle** yapı görev eklemek için. Bunu yaptığınızda, sol taraftaki eklenen görünür görev görürsünüz.

![PowerShell adım ayarlayın](./media/set-up-devops-lab/set-up-powershell-step.png)

Azure DevOps bunu bize bildirmek için hizmet sorumlusu ayarlamak üzere en hızlı yolu var. 

1. Seçin **görev** eklemiş.
2. İçin **Azure bağlantı türü**, seçin **Azure Resource Manager**. 
3. Seçin **Yönet** bağlantı hizmet sorumlusu ayarlamak için. 

Daha fazla bilgi için bkz. Bu [blog gönderisi](https://devblogs.microsoft.com/devops/automating-azure-resource-group-deployment-using-a-service-principal-in-visual-studio-online-buildrelease-management/). Seçtiğinizde, **Yönet** bağlantı, kavuşmak DevOps doğru yerde (blog gönderisinde ikinci ekran görüntüsüne) azure'a bağlantı ayarlamak için. Seçtiğinizden emin olun **Azure Resource Manager hizmet uç noktası** bu ayarlamaya.

## <a name="complete-the-build-task"></a>Derleme görevi tamamlayın
Derleme görevi seçerseniz, doldurulacak sağ bölmedeki tüm ayrıntıları görürsünüz. 

1. İlk olarak, derleme görevi adlandırın: **Sanal makine oluşturma**. 
2. Seçin **hizmet sorumlusu** seçerek oluşturduğunuz **Azure Resource Manager**
3. Seçin **hizmet uç noktası**. 
4. İçin **betik yolu**seçin **... (üç nokta)**  sağ.
5. Gidin **MakeGoldenImageVMs.ps1** betiği. 
6. Betik parametreleri gibi görünmelidir: `-ConfigurationLocation $(System.DefaultWorkingDirectory)$(ConfigurationLocation) -DevTestLabName $(DevTestLabName) -vmSize $(VMSize) -machineUserName $(MachineUserName) -machinePassword (ConvertTo-SecureString -string '$(MachinePassword)' -AsPlainText -Force) -StandardTimeoutMinutes $(StandardTimeoutMinutes)`

    ![Derleme tanımı'nı tamamlayın](./media/set-up-devops-lab/complete-build-definition.png)


## <a name="queue-the-build"></a>Derlemeyi kuyruğa al
Her şeyi doğru bir şekilde yeni bir yapıyı kuyruğa alarak sahip olduğunuzu doğrulayın. Derleme devam ederken, geçiş [Azure portalı](https://portal.azure.com) ve select deyiminde **tüm sanal makineler** görüntü Fabrika laboratuvarınızda her şeyin doğru şekilde çalıştığını onaylayın. Laboratuvarda oluşturulan üç sanal makine görmeniz gerekir.

![Vm'leri Laboratuvardaki](./media/set-up-devops-lab/vms-in-lab.png)

## <a name="next-steps"></a>Sonraki adımlar 
Azure DevTest Labs hakkında temel görüntü Fabrika ayarlama ilk adımı tamamlanmıştır. Serinin sonraki makalede, bu genelleştirilmiş hem de özel görüntüleri kaydedilen VM'ler elde edebilirsiniz. Daha sonra bunları tüm diğer laboratuvarlarınızı için Dağıtılmış sağlayın. Serinin sonraki makaleye bakın: [Özel görüntüleri kaydetmek ve dağıtmak için birden çok labs](image-factory-save-distribute-custom-images.md).
