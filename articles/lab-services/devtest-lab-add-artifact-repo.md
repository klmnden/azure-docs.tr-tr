---
title: Azure DevTest Labs laboratuarda bir Git deposu ekleme | Microsoft Docs
description: Azure DevTest Labs'de bir GitHub ya da Visual Studio Team Services Git deposu için özel yapılar kaynağınız eklemeyi öğrenin.
services: devtest-lab,virtual-machines,visual-studio-online
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.assetid: 01b459f7-eaf2-45a8-b4b5-2c0a821b33c8
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: spelluru
ms.openlocfilehash: 80724a7d8d2b5cec19bdbce27cdafd4a9c09eb47
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33787580"
---
# <a name="add-a-git-repository-to-store-custom-artifacts-and-resource-manager-templates"></a>Özel yapılar ve Resource Manager şablonları depolamak için bir Git deposu ekleme

Yapabilecekleriniz [özel yapılar oluşturma](devtest-lab-artifact-author.md) laboratuvarınızda, VM'ler veya [bir özel test ortamı oluşturmak için Azure Resource Manager şablonlarını kullanma](devtest-lab-create-environment-from-arm.md). Yapıları veya ekibinizin oluşturduğu Resource Manager şablonları için özel bir Git deposuna eklemeniz gerekir. Depo üzerinde barındırılan [GitHub](https://github.com) veya [Visual Studio Team Services](https://visualstudio.com).

Sunduğumuz bir [GitHub deposunu yapılarının](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts) olarak dağıtabileceğiniz-olan veya Laboratuvarları için özelleştirebilirsiniz. Özelleştirme veya bir yapıya oluşturduğunuzda, genel havuzda yapıyı depolanamıyor. Kendi özel bir depo oluşturduğunuz yapıları ve özel yapılar için oluşturmanız gerekir. 

Bir VM oluşturduğunuzda, Resource Manager şablonu kaydetme, istiyorsanız ve ardından daha sonra daha fazla sanal makineleri oluşturmak için kullanmak istiyorsanız özelleştirin. Özel Resource Manager şablonlarınızı depolamak için kendi özel depo oluşturmanız gerekir.  

* GitHub depo oluşturmayı öğrenmek için bkz: [GitHub Bootcamp](https://help.github.com/categories/bootcamp/).
* Bir Git deposu sahip bir Team Services projesi oluşturmayı öğrenmek için bkz: [Visual Studio Team Services Bağlan](https://www.visualstudio.com/get-started/setup/connect-to-visual-studio-online).

Aşağıdaki şekilde yapıları sahip bir depo Github'da nasıl görünebileceği bir örnektir:  

![Örnek GitHub yapıları depo](./media/devtest-lab-add-repo/devtestlab-github-artifact-repo-home.png)

## <a name="get-the-repository-information-and-credentials"></a>Depo bilgileri ve kimlik bilgilerini alma
Laboratuvarınızı için bir depo eklemek için ilk olarak, depodan anahtar bilgi alın. Aşağıdaki bölümlerde, GitHub veya Visual Studio Team Services üzerinde barındırılan depoları için gerekli bilgileri almak nasıl açıklanmaktadır.

### <a name="get-the-github-repository-clone-url-and-personal-access-token"></a>GitHub depo kopya URL'si ve kişisel erişim belirteci alma

1. Yapıyı veya Resource Manager şablonu tanımlarını içeren GitHub deposunu giriş sayfasına gidin.
2. Seçin **Kopyala veya indir**.
3. URL'yi panoya kopyalamak için seçin **HTTPS kopyalayın url** düğmesi. Daha sonra kullanmak için URL kaydedin.
4. GitHub sağ üst köşesindeki profilinin resmi seçin ve ardından **ayarları**.
5. İçinde **kişisel ayarları** menüsünü seçin sol taraftaki **kişisel erişim belirteçleri**.
6. Seçin **yeni belirteç Oluştur**.
7. Üzerinde **yeni kişisel erişim belirteci** sayfasında **belirteci açıklama**, bir açıklama girin. Altında varsayılan öğeleri kabul **kapsamı Seç**ve ardından **belirteç Oluştur**.
8. Oluşturulan belirteç kaydedin. Belirteci daha sonra kullanın.
9. GitHub kapatın.   
10. Devam [Laboratuvarınızı deposuna Bağlan](#connect-your-lab-to-the-repository) bölümü.

### <a name="get-the-visual-studio-team-services-repository-clone-url-and-personal-access-token"></a>Visual Studio Team Services depo kopya URL'si ve kişisel erişim belirteci alma

1. Takım koleksiyonunuz giriş sayfasına gidin (örneğin, https://contoso-web-team.visualstudio.com)ve projenizi seçin.
2. Proje giriş sayfasında seçin **kod**.
3. Projede kopya URL görüntülemek için **kod** sayfasında, **kopya**.
4. URL kaydedin. URL daha sonra kullanın.
5. Kullanıcı hesabı açılır menüde bir kişisel erişim belirteci oluşturmak için seçin **Profilim**.
6. Profil bilgileri sayfasında seçin **güvenlik**.
7. Üzerinde **güvenlik** sekmesine **Ekle**.
8. Üzerinde **kişisel erişim belirteci oluşturma** sayfa:
   1. Girin bir **açıklama** belirteci.
   2. İçinde **süresi içinde** listesinde **180 gün**.
   3. İçinde **hesapları** listesinde **tüm erişilebilir hesapları**.
   4. Seçin **tüm kapsamlar** seçeneği.
   5. Seçin **belirteç Oluştur**.
9. Yeni bir belirteç görünür **kişisel erişim belirteçleri** listesi. Seçin **kopyalama belirteci**ve daha sonra kullanmak için belirteç değeri kaydedin.
10. Devam [Laboratuvarınızı deposuna Bağlan](#connect-your-lab-to-the-repository) bölümü.

## <a name="connect-your-lab-to-the-repository"></a>Laboratuvarınızı deposuna Bağlan
1. [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
2. Seçin **daha Hizmetleri**ve ardından **DevTest Labs** Hizmetleri listesinden.
3. Laboratuvarınızı labs listesinden seçin. 
4. Seçin **yapılandırma ve ilkeleri** > **depoları** > **+ Ekle**.

    ![Ekle deposu düğmesi](./media/devtest-lab-add-repo/devtestlab-add-repo.png)
5. İkinci **depoları** sayfasında, aşağıdaki bilgileri belirtin:
  1. **Ad**. Havuz için bir ad girin.
  2. **Git kopya URL'si**. GitHub veya Visual Studio Team Services daha önce kopyaladığınız Git HTTPS kopya URL'si girin.
  3. **Şube**. Tanımlarınızı almak için dal girin.
  4. **Kişisel erişim belirteci**. Daha önce GitHub veya Visual Studio Team Services aldığınız kişisel erişim belirteci girin.
  5. **Klasör yollarını**. Yapı ya da Resource Manager şablonu tanımlarını içeren kopya URL göreli en az bir klasör yolu girin. Bir alt belirttiğinizde, klasörün yolunu eğik eklediğinizden emin olun.

     ![Depoları alanı](./media/devtest-lab-add-repo/devtestlab-repo-blade.png)
6. **Kaydet**’i seçin.

### <a name="related-blog-posts"></a>İlgili blog gönderileri
* [DevTest Labs yapıları başarısız olan ilgili sorunları giderme](devtest-lab-troubleshoot-artifact-failure.md)
* [DevTest Labs'de bir Resource Manager şablonu kullanarak bir VM mevcut bir Active Directory etki alanına katılma](http://www.visualstudiogeeks.com/blog/DevOps/Join-a-VM-to-existing-AD-domain-using-ARM-template-AzureDevTestLabs)

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Sonraki adımlar
Özel Git deponuzu oluşturduktan sonra gereksinimlerinize bağlı olarak biri veya her ikisi, aşağıdakileri yapabilirsiniz:
* Mağaza, [özel yapılar](devtest-lab-artifact-author.md). Bunları daha sonra yeni VM'ler oluşturmak için kullanabilirsiniz.
* [Resource Manager şablonları kullanarak çoklu VM ortamları ve PaaS kaynaklarına oluşturma](devtest-lab-create-environment-from-arm.md). Ardından, özel bağlantıların bulunması şablonlar depolayabilir.

Bir VM oluştururken yapıları veya şablonları Git deponuzu eklenen doğrulayabilirsiniz. Bunlar hemen yapıları veya şablonları listesinde kullanılabilir. Özel depo adını kaynağını belirtir sütununda gösterilir. 
