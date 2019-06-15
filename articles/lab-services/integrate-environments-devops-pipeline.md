---
title: Azure DevTest Labs hatlarında Azure ortamları tümleştirin | Microsoft Docs
description: Azure DevTest Labs ortamları, Azure DevOps sürekli tümleştirme (CI) ve sürekli teslim (CD) işlem hatlarından tümleştirme konusunda bilgi edinin.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/02/2019
ms.author: spelluru
ms.openlocfilehash: deb5595ac6a8b0d189e5594fda8e4b60480d038c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61318404"
---
# <a name="integrate-environments-into-your-azure-devops-cicd-pipelines"></a>Ortamları, Azure DevOps CI/CD işlem hatlarıyla tümleştirin
Sürekli Tümleştirme (CI) kolayca tümleştirmek amacıyla Azure DevOps (eski adıyla Visual Studio Team Services da bilinir) Hizmetleri yüklü olan Azure DevTest Labs görevlerini uzantısı kullanabileceğiniz / sürekli teslim (CD) derleme-ve-yayın işlem hattı, Azure ile DevTest Labs. Bu uzantıları hızla dağıtmanızı kolaylaştırır bir [ortam](devtest-lab-test-env.md) belirli bir test görevi ve test bittiğinde silin. 

Bu makalede, oluşturma ve bir ortamı dağıtmak ve ardından tam bir işlem hattı içindeki tüm ortamı silmemeyi gösterilmektedir. Normalde bu görevlerin her birini tek tek kendi özel yapı test etme-dağıtma hattında gerçekleştirmelisiniz. Bu makalede kullanılan uzantıları yanı sıra bunlar [DTL VM görevlerini oluşturma/silme](devtest-lab-integrate-ci-cd-vsts.md):

- Ortam oluşturma
- Bir ortamı silin.

## <a name="before-you-begin"></a>Başlamadan önce
Azure DevTest Labs ile CI/CD ardışık düzeninizi tümleştirebilirsiniz önce yüklemeniz [Azure DevTest Labs görevlerini](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) Visual Studio Market'ten uzantı. 

## <a name="create-and-configure-the-lab-for-environments"></a>Oluşturma ve laboratuvar ortamları için yapılandırma
Bu bölümde, oluşturma ve Azure ortamına dağıtılacağı Laboratuvar yapılandırma açıklanmaktadır.

1. [Bir laboratuvar oluşturma](devtest-lab-create-lab.md) zaten yoksa. 
2. Laboratuvar yapılandırma ve bu makaledeki yönergeleri izleyerek bir ortam şablonu oluşturun: [Azure Resource Manager şablonları ile çoklu VM ortamları ve PaaS kaynakları oluşturma](devtest-lab-create-environment-from-arm.md).
3. Bu örnekte, mevcut bir Azure Hızlı Başlangıç şablonu kullanmak [ https://azure.microsoft.com/resources/templates/201-web-app-redis-cache-sql-database/ ](https://azure.microsoft.com/resources/templates/201-web-app-redis-cache-sql-database/).
4. Kopyalama **201-web-app-redis-cache-sql-database** klasörüne **ArmTemplate** adım 2'de yapılandırdığınız depoda klasör.

## <a name="create-a-release-definition"></a>Yayın tanımı oluşturma
Yayın tanımı oluşturmak için aşağıdakileri yapın:

1.  Üzerinde **yayınlar** sekmesinde **derleme ve yayınlama hub**seçin **artı (+)** düğmesi.
2.  İçinde **Oluştur yayın tanımı** penceresinde **boş** şablonu ve ardından **sonraki**.
3.  Seçin **daha sonra seçin**ve ardından **Oluştur** bağlantılı yapıt yok ve bir varsayılan ortam ile yeni bir yayın tanımı oluşturmak için.
4.  Yeni yayın tanımında kısayol menüsünü açın, seçin **üç nokta (...)**  ortam adının yanındaki ve ardından **değişkenlerini yapılandırma**.
5.  İçinde **Yapılandır - ortam** yayın tanımı görevlerinde kullanan değişkenler penceresinde aşağıdaki değerleri girin:
1.  İçin **administratorLogin**, SQL Yöneticisi oturum açma adı girin.
2.  İçin **administratorLoginPassword**, SQL Yöneticisi oturum açma tarafından kullanılacak bir parola girin. Parolayı güvenli ve gizleme için "asma kilide" simgesini kullanın.
3.  İçin **databaseName**, SQL veritabanı adı girin.
4.  Bu değişkenler belirli yönelik örnek ortamlar, farklı ortamlarda farklı değişken olabilir.

## <a name="create-an-environment"></a>Ortam oluşturma
Dağıtım sonraki aşama, geliştirme veya sınama için kullanılacak ortamı oluşturmaktır.

1. Yayın tanımında seçin **görev ekleme**.
2. Üzerinde **görevleri** sekmesinde, bir Azure DevTest Labs ortamı oluşturma görevi ekleyin. Görev aşağıdaki gibi yapılandırın:
    1. İçin **Azure RM abonelik**, bir bağlantı seçin **kullanılabilir Azure hizmeti bağlantıları** listelemek ya da Azure aboneliğinize daha kısıtlı izinler bağlantı oluşturun. Daha fazla bilgi için [Azure Resource Manager hizmet uç noktası](/azure/devops/pipelines/library/service-endpoints).
2. İçin **Laboratuvar adı**, daha önce oluşturduğunuz örneğinin adını seçin. *.
3. İçin **depo adı**, burada Resource Manager şablonu (201) gönderilen için depoyu seçin *.
4. İçin **şablon adı**, kaydettiğiniz, kaynak kod deposu * için ortam adını seçin. 
5. **Laboratuvar adı**, **depo adı**, ve **şablon adı** Azure kaynak kimliklerini kolay temsillerini olan. Kolay ad el ile girmek hatalarına neden, bilgileri seçmek için açılır listeleri kullanın.
6. İçin **ortam adı**, laboratuvar ortamı örneğinde benzersiz olarak tanımlanabilmesi için bir ad girin.  Bu Laboratuvar içinde benzersiz olmalıdır.
7. **Parametre dosyası** ve **parametreleri**, ortama geçirilecek özel parametreler olanak tanır. Ya da parametre değerlerini ayarlamak için kullanılabilir. Bu örnekte, Parametreler bölümü kullanılır. Ortamında, örneğin tanımladığınız değişkenlerin adlarını kullanın: `-administratorLogin “$(administratorLogin)” -administratorLoginPassword “$(administratorLoginPassword)” -databaseName “$(databaseName)” -cacheSKUCapacity 1`
8. Ortam şablonu içindeki bilgileri aracılığıyla şablon çıkış bölümünde geçirilebilir. Denetleme **Oluştur çıkış değişkenleri ortam şablonu çıktıya dayalı** diğer görevleri verileri kullanabilirsiniz. `$(Reference name.Output Name)` izlenecek modelidir. Örneğin, başvuru adı DTL ve çıkış adı şablonda konumu şeklindeydi değişkeni olurdu `$(DTL.location)`.

## <a name="delete-the-environment"></a>Ortamı silme
Azure DevTest Labs Örneğinizde dağıtılan ortamı silmek için son aşamadır bakın. Geliştirme görevleri yürütmek ya da ihtiyaç duyduğunuz dağıtılan kaynaklar üzerinde testler sonra normalde Ortamı siler.

Yayın tanımında seçin **görev ekleme**ve ardından **Dağıt** sekmesinde, ekleme bir **Azure DevTest Labs ortamı silme** görev. Aşağıdaki gibi yapılandırın:

1. Sanal Makineyi silmek için bkz: [Azure DevTest Labs görevlerini](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks):
    1. İçin **Azure RM abonelik**, bir bağlantı seçin **kullanılabilir Azure hizmeti bağlantıları** listelemek ya da Azure aboneliğinize daha kısıtlı izinler bağlantı oluşturun. Daha fazla bilgi için [Azure Resource Manager hizmet uç noktası](/azure/devops/pipelines/library/service-endpoints).
    2. İçin **Laboratuvar adı**, laboratuvar ortamı bulunduğu seçin.
    3. İçin **ortam adı**, kaldırılacak ortam adını girin.
2. Yayın tanımı için bir ad girin ve kaydedin.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın: 
- [Resource Manager şablonları ile çoklu VM ortamları oluşturma](devtest-lab-create-environment-from-arm.md).
- DevTest Labs Otomasyon hızlı başlangıç Resource Manager şablonları [DevTest Labs GitHub deposu](https://github.com/Azure/azure-quickstart-templates).
- [VSTS sorun giderme sayfası](/azure/devops/pipelines/troubleshooting)

