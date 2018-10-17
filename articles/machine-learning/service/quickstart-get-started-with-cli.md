---
title: CLI uzantısını kullanarak Azure Machine Learning hizmetiyle çalışmaya başlama | Microsoft Docs
description: Bu hızlı başlangıçta, Azure Machine Learning CLI uzantısını kullanarak Azure Machine Learning hizmetiyle çalışmaya başlamayı öğreneceksiniz.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: quickstart
author: rastala
ms.author: roastala
ms.reviewer: sgilley
ms.date: 09/24/2018
ms.openlocfilehash: 856f9629e97f8cf7cf811e7d591cbcad6067f47a
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2018
ms.locfileid: "48237169"
---
# <a name="quickstart-get-started-with-azure-machine-learning-using-the-cli-extension"></a>Hızlı Başlangıç: CLI uzantısını kullanarak Azure Machine Learning ile çalışmaya başlama

Bu hızlı başlangıçta, [Azure Machine Learning hizmetiyle](overview-what-is-azure-ml.md) (Önizleme) çalışmaya başlamak için makine öğrenmesi CLI uzantısını kullanacaksınız.

CLI'yı kullanarak aşağıdakilerin nasıl yapıldığını öğreneceksiniz:

1. Azure aboneliğinizde çalışma alanı oluşturma. Çalışma alanı bir veya birden çok kullanıcı tarafından işlem kaynaklarını, modellerini, dağıtımlarını ve çalıştırma geçmişlerini bulutta depolamak için kullanılır.
1. Çalışma alanınıza proje ekleme.   Proje, makine öğrenmesi sorununuzu çözmek için gereken betiklerin ve yapılandırma dosyalarının yer aldığı yerel bir klasördür.  
1. Projenizde birden çok yinelemeden bazı değerleri günlüğe kaydeden bir Python betiği çalıştırın.
1. Çalışma alanınızın çalıştırma geçmişinde günlüğe kaydedilmiş değerleri görüntüleyin.

> [!NOTE]
> Size kolaylık sağlamak için, şu Azure kaynakları bölgesel olarak sağlandığında otomatik olarak çalışma alanınıza eklenir: [kapsayıcı kayıt defteri](https://azure.microsoft.com/services/container-registry/), [depolama](https://azure.microsoft.com/services/storage/), [uygulama içgörüleri](https://azure.microsoft.com/services/application-insights/), ve [anahtar kasası](https://azure.microsoft.com/services/key-vault/).

Oluşturduğunuz kaynaklar, diğer Azure Machine Learning öğreticileri ve nasıl yapılır makalelerinde önkoşul olarak kullanılabilir.

Bu CLI, Azure Machine Learning hizmeti için Python tabanlı <a href="http://aka.ms/aml-sdk" target="_blank">SDK</a>'nın üzerine kurulmuştur.

## <a name="prerequisites"></a>Ön koşullar

Hızlı başlangıç adımlarına geçmeden önce aşağıdaki önkoşulları karşıladığınızdan emin olun:

+ Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.
+ [Python 3.5 veya üstü](https://www.python.org/) yüklü
+ [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) yüklü

## <a name="install-the-cli-extension"></a>CLI uzantısını yükleme

Bilgisayarınızda, komut satırı düzenleyicisini açın ve [Azure CLI'ya makine öğrenmesi uzantısını](reference-azure-machine-learning-cli.md) yükleyin.  Yükleme işleminin tamamlanması birkaç dakika sürebilir.

```azurecli
az extension add azureml-sdk
```

## <a name="install-the-sdk"></a>SDK yükle

[!INCLUDE [aml-install-sdk](../../../includes/aml-install-sdk.md)]

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Kaynak grubu, bir Azure çözümü için ilgili kaynakları bir arada tutan kapsayıcıdır. Azure CLI'yı kullanarak Azure'da oturum açın, aboneliği belirtin ve kaynak grubu oluşturun.

Komut satırı penceresinde Azure CLI komutu `az login` ile oturum açın. Etkileşimli oturum açmak için istemleri izleyin:
    
   ```azurecli
   az login
   ```

Kullanılabilir Azure aboneliklerini listeleyin ve hangisini kullanmak istediğinizi belirtin:
   ```azurecli
   az account list --output table
   az account set --subscription <your-subscription-id>
   az account show
   ```
   Burada \<your-subscription-id\>, kullanmak istediğiniz aboneliğin kimlik değeridir. Köşeli ayraçları dahil etmeyin.

Çalışma alanınızı barındıracak bir kaynak grubu oluşturun.
Bu hızlı başlangıçta:
   + Kaynak grubunun adı `docs-aml` değeridir.
   + Bölge `eastus2` bölgesidir. 

   ```azurecli
   az group create -n docs-aml -l eastus2
   ```

## <a name="create-a-workspace-and-a-project-folder"></a>Çalışma alanı ve proje klasörü oluşturma

Komut satırı penceresinde, kaynak grubunun altında Azure Machine Learning hizmeti çalışma alanını oluşturun.


   Bu hızlı başlangıçta:
   + Çalışma alanı adı `docs-ws` değeridir.
   + Kaynak grubu adı `docs-aml` değeridir

   ```azurecli
   az ml workspace create -n docs-ws -g docs-aml
   ```

Komut satırı penceresinde, yerel makinenizde Azure Machine Learning projeniz için bir klasör oluşturun.

   ```
   mkdir docs-prj
   cd docs-prj
   ```

## <a name="create-a-python-script"></a>Python betiği oluşturma

[!INCLUDE [aml-create-script-pi](../../../includes/aml-create-script-pi.md)]

## <a name="run-the-script"></a>Betiği çalıştırın

Klasörü çalışma alanına bir proje olarak ekleyin. `--history` bağımsız değişkeni, her çalıştırma için ölçümleri yakalayan çalıştırma geçmişi dosyanının adını belirtir.

   ```azurecli
   az ml project attach --history my_history -w docs-ws -g docs-aml
   ```

Betiği yerel bilgisayarınızda çalıştırın.

   ```azurecli
   az ml run submit -c local pi.py
   ```

   Bu komut kodu çalıştırır ve konsolunuza çıkış olarak bir web bağlantısı verir. Bağlantıyı kopyalayıp web tarayıcınıza yapıştırın.

Web tarayıcısında URL'yi ziyaret edin. Çalıştırmanın sonuçlarını içeren bir web portalı görüntülenir. Bu çalıştırmanın veya varsa önceki çalıştırmaların sonuçlarını inceleyebilirsiniz.

Portalın panosu yalnızca Edge, Chrome ve Firefox tarayıcılarında desteklenir.

   ![geçmişi görüntüleme](./media/quickstart-get-started/web-results.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar
Artık modelleri denemeye ve dağıtmaya başlamak için gerekli kaynakları oluşturdunuz. Ayrıca yeni bir proje oluşturdunuz, betik çalıştırdınız ve betiğin çalıştırma geçmişini incelediniz.

Ayrıntılı bir iş akışı deneyimi için, model oluşturma, eğitme ve dağıtmaya yönelik Azure Machine Learning öğreticilerini izleyin.

> [!div class="nextstepaction"]
> [Öğretici: Oluşturma, eğitme ve dağıtma](tutorial-train-models-with-aml.md)