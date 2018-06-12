---
title: Visual Studio'da sunucusuz bir uygulama oluşturun | Microsoft Docs
description: İlk sunucusuz uygulamanızı oluşturma, dağıtma ve Visual Studio uygulamasında yönetme bu kılavuzu ile başlayın.
keywords: ''
services: logic-apps
author: jeffhollan
manager: jeconnoc
editor: ''
documentationcenter: ''
ms.assetid: d565873c-6b1b-4057-9250-cf81a96180ae
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/30/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: 75f2be44aa8cc239257d6d2a7dad448e9dbab4b4
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35300465"
---
# <a name="build-a-serverless-app-in-visual-studio-with-logic-apps-and-functions"></a>Visual Studio'da Logic Apps ve işlevleri ile sunucusuz bir uygulama oluşturun

Sunucusuz araçları ve Azure işlevleri hızlı geliştirme ve bulut uygulamalarının dağıtımını için izin verir.  Visual Studio sunucusuz bir uygulama oluşturmaya başlamak bu belgenin odaklanır.  Azure içinde sunucusuz bir genel bakış [bu makalede bulunabilir](logic-apps-serverless-overview.md).

## <a name="getting-everything-ready"></a>Her şeyi hazırlığı

Visual Studio'dan sunucusuz bir uygulama oluşturmak için gereken önkoşullar şunlardır:

* [Visual Studio 2017](https://www.visualstudio.com/vs/) veya Visual Studio 2015 - Community, Professional veya Enterprise
* [Visual Studio için Logic Apps araçları](https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio-18551)
* [En son Azure SDK'sı](https://azure.microsoft.com/downloads/) (2.9.1 veya üzeri)
* [Azure PowerShell](https://github.com/Azure/azure-powershell#installation)
* [Azure işlevleri çekirdek Araçları](https://www.npmjs.com/package/azure-functions-core-tools) hata ayıklamak için işlevleri yerel olarak
* Ekli mantıksal Uygulama Tasarımcısı'nı kullanırken web erişimi

## <a name="getting-started-with-a-deployment-template"></a>Bir dağıtım şablonu ile çalışmaya başlama

Azure kaynakları yöneten bir kaynak grubu içindeki yapılır.  Bir kaynak grubu kaynakları mantıksal bir gruplandırmasıdır.  Kaynak grupları, dağıtım ve kaynak yönetimi sağlar.  Azure içinde sunucusuz bir uygulama için hem Azure mantıksal uygulamaları hem de Azure işlevleri bizim kaynak grubunu içerir.  Visual Studio içinde kaynak grubu projesi kullanarak, biz geliştirmek, yönetmek ve uygulamanın tümünü tek bir varlık olarak dağıtabilir.

### <a name="create-a-resource-group-project-in-visual-studio"></a>Visual Studio'da bir kaynak grubu projesi oluşturma

1. Visual Studio'da eklemek için tıklatın bir **yeni proje**
1. İçinde **bulut** kategorisi, Azure oluşturmak üzere seçin **kaynak grubu** proje  
 * Kategori veya listelenen proje görmüyorsanız, Visual Studio için Azure SDK yüklü olduğundan emin olun
1. Bir ad ve konum proje verin ve seçin **Tamam** Visual Studio oluşturmak için bir şablon seçin isteyip istemediğinizi sorar.  Boş iken başlatmak için bir mantıksal uygulama ya da diğer kaynak Başlat seçebilirsiniz.  Ancak, bu durumda Azure Hızlı Başlangıç şablonu bize sunucusuz bir uygulamayı başlatmak almak için kullanırız.
1. Şablonlardan göstermeyi seçin **Azure Hızlı Başlangıç** ![seçerek Azure hızlı başlangıç şablonları][1]
1. Sunucusuz Hızlı Başlangıç şablonu seçin: **101-logic-app-and-function-app** tıklatıp **Tamam**

Hızlı Başlangıç şablonu kaynak grubu projenizi bir dağıtım şablonu oluşturur.  Bu şablon, Azure işlevlerini çağırır ve sonucu döndürür basit bir mantıksal uygulama içerir.  Açarsanız `azuredeploy.json` dosya Çözüm Gezgini'nde sunucusuz uygulama kaynaklarını görebilirsiniz.

## <a name="deploying-the-serverless-application"></a>Sunucusuz bir uygulama dağıtma

Visual Studio'da mantıksal uygulama görsel tasarımcı açmadan önce var. önceden dağıtılan Azure kaynak grubu olması gerekir.  Bu oluşturmak ve mantıksal uygulama kaynaklarını ve Hizmetleri bağlantıları kullanmaları tasarımcı sağlar.  Başlamak için biz oluşturulan çözümü dağıtmak yeterlidir.

1. Visual Studio, Seç'nde projeye sağ **dağıtma**ve oluşturma bir **yeni** dağıtım ![yeni kaynak dağıtım seçme][2]
1. Geçerli bir Azure abonelik ve kaynak grubu seçin
1. İçin Select **dağıtma** çözümü
1. Mantıksal uygulama ve Azure işlev uygulaması adını girin.  Azure işlev adı genel olarak benzersiz olması gerekir

Belirtilen kaynak grubuna sunucusuz çözümü dağıtır.  Bakarsanız **çıkış** Visual Studio'da dağıtım durumunu görebilirsiniz.

## <a name="editing-the-logic-app-in-visual-studio"></a>Visual Studio'da mantıksal uygulama düzenleme

Çözüm herhangi bir kaynak grubu dağıtıldıktan sonra görsel tasarımcı düzenlemek ve mantıksal Uygulama'ya değişiklik yapmak için kullanılabilir.

1. Sağ `azuredeploy.json` dosya Çözüm Gezgini'nde ve seçin **ile Logic Apps tasarımcıyı Aç**
1. Seçin **kaynak grubu** ve **konumu** çözümü için dağıtılan ve select **Tamam**

Mantıksal uygulama görsel tasarımcı artık Visual Studio ile görünür olması gerekir.  Adımları eklemek, iş akışını değiştirme ve değişiklikleri kaydetmek devam edebilirsiniz.  Visual Studio'dan mantıksal uygulamalar da oluşturabilirsiniz.  Sağ tıklattığınızda, **kaynakları** eklemek için seçebileceğiniz şablon Gezgini'nde bir **mantıksal uygulama** projeye.  Boş mantıksal uygulamalar yük visual Tasarımcısı'nda bir kaynak grubuna ön dağıtımını yap.

### <a name="managing-and-viewing-run-history-for-a-deployed-logic-app"></a>Yönetme ve çalıştırma geçmişi dağıtılan mantıksal uygulama için görüntüleme

Ayrıca, yönetmek ve Azure'da dağıtılan mantığı uygulamalar çalıştırma geçmişi görüntüleyebilirsiniz.  Açarsanız **Cloud Explorer** aracı Visual Studio'da, tüm mantıksal uygulama sağ tıklayın ve düzenlemek, devre dışı bırakmak, özelliklerini görüntülemek veya çalıştırma geçmişi görüntülemek seçin.  Düzen'i tıklatarak, Visual Studio kaynak grubu projesine yayımlanan mantıksal uygulama indirmek de sağlar.  Bu, Azure portalda mantıksal uygulamanızı oluşturmaya başladı olsa bile, yine, içeri aktarabilir ve Visual Studio'dan yönetmek, anlamına gelir.

## <a name="developing-an-azure-function-in-visual-studio"></a>Visual Studio'da bir Azure işlevinizi geliştirme

Dağıtım şablonu belirtilen git deposu için çözümde bulunan tüm Azure işlevleri dağıtır `azuredeploy.json` değişkenleri.  Kaynak denetimi (GitHub, Visual Studio Team Services, vb.) ve güncelleştirme işlevi proje çözümünde yazarsanız, kutuyu `repo` değişken, şablonu Azure işlevi dağıtmak.

### <a name="creating-an-azure-function-project"></a>Bir Azure işlevi projesi oluşturma

JavaScript, Python, F #, Bash, toplu veya PowerShell kullanarak, izleyin [işlevleri CLI adımları](../azure-functions/functions-run-local.md) bir proje oluşturmak için.  Bir işlev C# geliştirme, kullanabileceğiniz bir [C# sınıf kitaplığı](https://blogs.msdn.microsoft.com/appserviceteam/2017/03/16/publishing-a-net-class-library-as-a-function-app/) Azure işlevi için geçerli çözümde.

## <a name="next-steps"></a>Sonraki adımlar

* [Sunucusuz bir sosyal Pano oluşturmayı öğrenin](logic-apps-scenario-social-serverless.md)
* [Visual Studio bulut Gezgini'nden bir mantıksal uygulama yönetme](manage-logic-apps-with-visual-studio.md)
* [Mantıksal uygulama iş akışı tanımlama dili](logic-apps-workflow-definition-language.md)

<!-- Image references -->
[1]: ./media/logic-apps-serverless-get-started-vs/select-template.png
[2]: ./media/logic-apps-serverless-get-started-vs/deploy.png
