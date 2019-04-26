---
title: Git deposu için Azure DevTest labs'deki bir laboratuvara ekleme | Microsoft Docs
description: Azure DevTest Labs'de bir GitHub veya Azure DevOps Services Git deposunda özel yapıtlar kaynağınız için eklemeyi öğrenin.
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
ms.openlocfilehash: 5d7665cbfdf855e194f61910f0c8ee2bce5469b1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60311731"
---
# <a name="add-a-git-repository-to-store-custom-artifacts-and-resource-manager-templates"></a>Özel yapıtlar ve Resource Manager şablonları depolamak için bir Git deposu ekleme

Yapabilecekleriniz [özel yapıtlar oluşturma](devtest-lab-artifact-author.md) laboratuvarınızda, VM'ler veya [özel test ortamı oluşturmak için Azure Resource Manager şablonlarını kullanma](devtest-lab-create-environment-from-arm.md). Yapılar veya takımınızın oluşturan Resource Manager şablonları için özel bir Git deposuna eklemeniz gerekir. Depo barındırılabilen [GitHub](https://github.com) veya [Azure DevOps Hizmetleri](https://visualstudio.com).

Sunuyoruz bir [GitHub deposu yapıtları](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts) olarak dağıtabileceğiniz-olan veya laboratuvarlarınızı için özelleştirebilirsiniz. Özelleştirme veya bir yapı oluşturmak, genel depoda yapıt depolanamıyor. Özel yapıtlar ve oluşturduğunuz yapıtları için kendi özel deponuzu oluşturmanız gerekir. 

Bir VM oluşturduğunuzda, Resource Manager şablonu kaydedin, özelleştirin ve ardından daha sonra daha fazla sanal makine oluşturmak için bunu kullanın. Özel Resource Manager şablonlarınızı depolamak için kendi özel deponuzu oluşturmanız gerekir.  

* Bir GitHub deposu oluşturmayı öğrenmek için bkz: [GitHub Bootcamp](https://help.github.com/categories/bootcamp/).
* Bir Git deposu olan bir Azure DevOps Services projesi oluşturmayı öğrenmek için bkz: [Azure DevOps hizmetlere bağlanma](https://www.visualstudio.com/get-started/setup/connect-to-visual-studio-online).

Aşağıdaki şekilde Github'da yapıt yok bir depoyu nasıl görünebileceği, bir örnek verilmiştir:  

![Örnek GitHub yapıt deposu](./media/devtest-lab-add-repo/devtestlab-github-artifact-repo-home.png)

## <a name="get-the-repository-information-and-credentials"></a>Depo bilgilerini ve kimlik bilgilerini alma
Laboratuvarınız için bir depo eklemek için ilk olarak, deponuzdan anahtarı bilgilerini alın. Aşağıdaki bölümlerde, GitHub veya Azure DevOps hizmetleriyle ilgili barındırılan depolar için gerekli bilgileri almak nasıl açıklanmaktadır.

### <a name="get-the-github-repository-clone-url-and-personal-access-token"></a>GitHub depo kopya URL'si ve kişisel erişim belirteci alma

1. Yapıt ya da Resource Manager şablonu tanımlarını içeren GitHub deposunun giriş sayfasına gidin.
2. **Clone or download**'u (Kopyala veya indir) seçin.
3. URL'yi panoya kopyalamak için seçin **HTTPS kopyası URL'si** düğmesi. Daha sonra kullanmak için URL'yi kaydedin.
4. GitHub sağ üst köşesindeki profil resmini seçin ve ardından **ayarları**.
5. İçinde **kişisel ayarlarını** seçin sol taraftaki menüsünden **kişisel erişim belirteçleri**.
6. Seçin **yeni belirteç Oluştur**.
7. Üzerinde **yeni bir kişisel erişim belirteci** sayfasındaki **belirteç açıklaması**, bir açıklama girin. Altında varsayılan öğeleri kabul **kapsamları seçin**ve ardından **belirteç Oluştur**.
8. Oluşturulan belirtecin kaydedin. Belirteci daha sonra kullanın.
9. GitHub'ı kapatın.   
10. Devam [Laboratuvarınızı depoya bağlanmak](#connect-your-lab-to-the-repository) bölümü.

### <a name="get-the-azure-repos-clone-url-and-personal-access-token"></a>Azure depolarda kopya URL'si ve kişisel erişim belirteci alma

1. Takım koleksiyonunuzun giriş sayfasına gidin (örneğin, https://contoso-web-team.visualstudio.com)ve ardından projenizi seçin.
2. Proje giriş sayfasında **kod**.
3. Kopya URL'si projede görüntülemek için **kod** sayfasında **kopya**.
4. URL'yi kaydedin. Daha sonra URL'yi kullanın.
5. Kullanıcı hesabı aşağı açılan menüden bir kişisel erişim belirteci oluşturmak için seçin **Profilimi**.
6. Profil bilgileri sayfasında seçin **güvenlik**.
7. Üzerinde **güvenlik** sekmesinde **Ekle**.
8. Üzerinde **bir kişisel erişim belirteci oluşturma** sayfası:
   1. Girin bir **açıklama** belirteç için.
   2. İçinde **süresi içinde** listesinden **180 gün**.
   3. İçinde **hesapları** listesinden **tüm erişilebilir hesaplar**.
   4. Seçin **tüm kapsamlar** seçeneği.
   5. Seçin **belirteci oluşturma**.
9. Yeni belirteç görünür **kişisel erişim belirteçleri** listesi. Seçin **kopyalama belirteci**ve daha sonra kullanmak için belirteç değeri kaydedin.
10. Devam [Laboratuvarınızı depoya bağlanmak](#connect-your-lab-to-the-repository) bölümü.

## <a name="connect-your-lab-to-the-repository"></a>Laboratuvarınızı depoya bağlanmak
1. [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
2. Seçin **diğer hizmetler**ve ardından **DevTest Labs** hizmetler listesinden.
3. Laboratuvarınızı labs listesinden seçin. 
4. Seçin **yapılandırması ve ilkelerini** > **depoları** > **+ Ekle**.

    ![Depo Ekle düğmesi](./media/devtest-lab-add-repo/devtestlab-add-repo.png)
5. İkinci **depoları** sayfasında, aşağıdaki bilgileri belirtin:
   1. **Ad**. Depo için bir ad girin.
   2. **Git kopya URL'si**. Daha önce kopyaladığınız Azure DevOps Services veya GitHub Git HTTPS kopya URL girin.
   3. **Dal**. Tanımlarınızı almak için dal girin.
   4. **Kişisel erişim belirteci**. Azure DevOps Services veya GitHub daha önce aldığınız kişisel erişim belirteci girin.
   5. **Klasör yolları**. Yapı ya da Resource Manager şablonu tanımlarını içeren kopya URL göreli en az bir klasör yolu girin. Bir alt belirttiğinizde, klasör yoluna eğik çizgi dahil emin olun.

      ![Depoları alanı](./media/devtest-lab-add-repo/devtestlab-repo-blade.png)
6. **Kaydet**’i seçin.

### <a name="related-blog-posts"></a>İlgili blog gönderileri
* [DevTest Labs yapılar başarısız olan ilgili sorunları giderme](devtest-lab-troubleshoot-artifact-failure.md)
* [DevTest Labs'de bir Resource Manager şablonu kullanarak bir sanal makine mevcut bir Active Directory etki alanına katılın](https://www.visualstudiogeeks.com/blog/DevOps/Join-a-VM-to-existing-AD-domain-using-ARM-template-AzureDevTestLabs)

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Sonraki adımlar
Özel Git deponuza oluşturduktan sonra gereksinimlerinize bağlı olarak biri veya her ikisi şunları yapabilirsiniz:
* Store, [özel yapıtlar](devtest-lab-artifact-author.md). Bunları daha sonra yeni sanal makineler oluşturmak için kullanabilirsiniz.
* [Resource Manager şablonlarını kullanarak çoklu VM ortamları ve PaaS kaynakları oluşturma](devtest-lab-create-environment-from-arm.md). Ardından, şablonları, özel bir depoda saklayabilirsiniz.

Bir VM oluşturduğunuzda, şablonlar ve yapıtlar Git deponuza eklendiğini doğrulayabilirsiniz. Bunlar yapıları veya şablonları listesinde hemen kullanılabilir. Özel deponuzu Adı kaynağını belirtir sütununda gösterilir. 
