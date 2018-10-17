---
title: Azure Hızlı Başlangıç - Batch AI kümesi oluşturma - Portal | Microsoft Docs
description: Hızlı Başlangıç - Makine öğrenimi ve AI modellerini eğitmek için Batch AI kümesi oluşturma - Azure portal
services: batch-ai
documentationcenter: na
author: dlepow
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.custom: ''
ms.service: batch-ai
ms.workload: ''
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 08/15/2018
ms.author: danlep
ms.openlocfilehash: 8b9daa0fbbf84e0f602498a0847c9e120f709b17
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44057939"
---
# <a name="quickstart-create-a-cluster-for-batch-ai-training-jobs-using-the-azure-portal"></a>Hızlı Başlangıç: Azure portalı kullanarak Batch AI eğitim işleri için küme oluşturma

Bu hızlı başlangıçta, yapay zeka ve makine öğrenimi modellerini eğitmek üzere kullanabileceğiniz bir Batch AI kümesi oluşturmak için Azure portalı nasıl kullanabileceğiniz gösterilmektedir. Batch AI, veri bilimcilerinin ve yapay zeka araştırmacılarının Azure sanal makine kümelerindeki yapay zeka ve makine öğrenimi modellerini ölçeğe uygun olarak eğitmesini sağlayan bir yönetilen hizmettir.

Küme, başlangıçta tek bir GPU düğümüne ve bir ekli dosya sunucusuna sahiptir. Bu hızlı başlangıcı tamamladıktan sonra, ölçeğini artırıp derin öğrenme modellerini eğitmek için kullanabileceğiniz bir kümeniz olacak. Batch AI, [Azure Machine Learning](../machine-learning/service/overview-what-is-azure-ml.md) araçları veya [Visual Studio Tools for AI](https://github.com/Microsoft/vs-tools-for-ai) kullanarak kümeye eğitim işleri gönderin.

[!INCLUDE [quickstarts-free-trial-note.md](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-ssh-key-pair"></a>SSH anahtar çifti oluşturma

Bu hızlı başlangıcı tamamlamak için bir SSH anahtar çifti gerekir. Bir SSH anahtar çiftiniz varsa bu adımı atlayabilirsiniz.

Bir SSH anahtar çifti oluşturmak için, bir Bash kabuğundan aşağıdaki komutu çalıştırın ve ekrandaki yönergeleri izleyin. Örneğin, [Azure Cloud Shell](../cloud-shell/overview.md) veya Windows’da [Linux için Windows Subsystem](/windows/wsl/install-win10)’i kullanabilirsiniz. Komut çıktısı genel anahtar dosyasının dosya adını içerir. Ortak anahtar dosyanızın (`cat ~/.ssh/id_rsa.pub`) içeriğini panoya veya sonraki bir adımda erişebileceğiniz bir konuma kopyalayın.

```bash
ssh-keygen -t rsa -b 2048
```

SSH anahtar çifti oluşturma hakkında ayrıntılı bilgi için bkz. [Azure’da Linux VM’ler için SSH ortak-özel anahtar çifti oluşturma ve kullanma](../virtual-machines/linux/mac-create-ssh-keys.md).

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="create-a-batch-ai-workspace"></a>Batch AI çalışma alanı oluşturma

Batch AI kaynaklarınızı düzenlemek için bir Batch AI çalışma alanı oluşturarak başlayın. Bir çalışma alanı, bir veya daha fazla küme veya diğer Batch AI kaynağını içerebilir.

1. **Tüm hizmetler**’i seçin ve **Batch AI** için filtre uygulayın.

2. **Çalışma Alanı Ekle** seçeneğini belirleyin.

3. **Çalışma alanı adı** ve **Kaynak grubu** değerlerini girin. İsterseniz, çalışma alanı için farklı **Abonelik** ve **Konum** seçenekleri belirleyin. **Çalışma Alanı Oluştur**’u seçin.

  ![Batch AI çalışma alanı oluşturma](./media/quickstart-create-cluster-portal/create-workspace.png)

**Dağıtım başarılı** iletisi görüntülendiğinde, oluşturduğunuz kaynağa gidip çalışma alanını seçin.

## <a name="create-a-file-server"></a>Dosya sunucusu oluşturma

Bir Batch AI dosya sunucusu, küme düğümlerine otomatik olarak eklenebilen tek düğümlü bir NFS’dir. Bu, giriş verileriniz ve eğitim işlerinizden çıkış için depolama sağlayabileceğiniz birkaç yoldan biridir.

1. Çalışma alanında, **Dosya sunucusu** > **Batch AI dosya sunucusu ekle** seçeneğini belirleyin.

2. **Dosya sunucusu adı** ve **VM boyutu** için değer girin. Bu hızlı başlangıçta, dosya sunucusu için *Standard D1_v2* VM boyutu önerilir. Eğitim işleri için büyük miktarda giriş veya çıkış verisi depolamanız gerekiyorsa, farklı bir boyut seçin.

3. Bir **Yönetici kullanıcı adı** girin ve SSH ortak anahtar dosyanızın içeriğini **SSH anahtarına** kopyalayın. Kalan değerler için varsayılanları kabul edin ve **Dosya sunucusu oluştur**’u seçin.

  ![Batch AI dosya sunucusu oluşturma](./media/quickstart-create-cluster-portal/create-file-server.png)

Dosya sunucusunun dağıtılması birkaç dakika sürer.

Sunucu oluşturulduktan sonra, **Özellikler**’e tıklayın ve **Bağlama ayarları**’nı not edin. Belirtilen dizinde (*/data*), eğitim verilerini ve çıkış dosyalarını karşıya yüklemek ve indirmek için sunucunun ortak IP adresinde SSH kullanabilirsiniz.

![Dosya sunucusu özellikleri](./media/quickstart-create-cluster-portal/file-server-properties.png)

## <a name="create-a-cluster"></a>Küme oluşturma

Aşağıdaki adımlar, tek bir GPU düğümü ile bir küme oluşturur. Küme düğümü çoğu eğitim iş yükü için kullanabileceğiniz kapsayıcı tabanlı uygulamaları barındırmak için tasarlanmış varsayılan bir Ubuntu Server görüntüsü çalıştırır. Küme düğümü, dosya sunucusuna bağlama noktasında bağlanır. 

1. Batch AI çalışma alanınızda, **Küme** > **Batch AI kümesi ekle** seçeneğini belirleyin.

2. **Küme adı** için değer girin ve aşağıdaki ayarları yapın. Önerilen VM boyutu bir NVIDIA Tesla K80 GPU’ya sahiptir.
  
   |Ayar  |Değer  |
   |---------|---------|
   |**VM boyutu**     |Standart NC6|
   |**Hedef düğüm sayısı**     |1|

3. Bir **Yönetici kullanıcı adı** girin ve SSH ortak anahtar dosyanızın içeriğini **SSH anahtarına** kopyalayın. Bu sayfadaki diğer değerler için varsayılanları kabul edin ve **Sonraki: Düğüm kurulumu** seçeneğini belirleyin.

   ![Temel küme bilgilerini girin](./media/quickstart-create-cluster-portal/create-cluster.png)

4. **Birimleri Bağla** altında, **Dosya sunucusu başvuruları** > **Ekle** seçeneğini belirleyin. Daha önce oluşturduğunuz dosya sunucusunu seçin. Dosya sunucusunun her bir küme düğümüne bağlandığı bir **Görece bağlama yolu** girin. **Kaydet ve devam et**’i seçin.

   ![Dosya sunucusu başvurusu ekleme](./media/quickstart-create-cluster-portal/file-server-reference.png)

Düğüm kurulumunu kaydedin ve **Küme Oluştur**’u seçin.

Batch AI’ın düğümü ayırması birkaç dakika sürer. Bu süre boyunca, kümenin **Ayırma durumu** **Yeniden boyutlandırılıyor** şeklindedir. Birkaç dakika sonra kümenin durumu **Sabit** olur ve düğüm başlatılır.

![Küme başlatılıyor](./media/quickstart-create-cluster-portal/cluster-resizing.png)

Düğüm durumunu denetlemek için küme adını seçin. Bir düğümün durumu **Boşta** olduğunda eğitim işlerini çalıştırmaya hazırdır.

### <a name="list-cluster-nodes"></a>Küme düğümlerini listeleme

Uygulama yüklemek veya bakım yapmak için küme düğümlerine (bu durumda, tek bir düğüme) bağlanmanız gerekiyorsa, portaldan bağlantı bilgilerini alın. Küme oluşturulduktan sonra, **Düğümler**’e tıklayın ve SSH **Bağlantı** ayarlarını (IP adresi ve bağlantı noktası numarası) not edin.

![Küme düğümleri](./media/quickstart-create-cluster-portal/cluster-nodes.png)

Düğüme bir SSH bağlantısı kurmak için bu bilgileri kullanın. Örneğin, aşağıdaki komutta düğümünüzde IP adresini ve bağlantı noktası numarasını doğru değerlerle değiştirin:

```bash
ssh myusername@137.135.82.15 -p 50000
``` 

### <a name="resize-the-cluster"></a>Kümeyi yeniden boyutlandırma

Kümenizi bir modeli eğitmek için kullandığınızda, daha fazla işlem kaynağına ihtiyacınız olabilir. Örneğin, dağıtılmış bir eğitim işi için boyutu 2 düğüme artırmak üzere, **Ölçek** seçeneğini belirleyin ve **Hedef düğüm sayısı** değerini 2 olarak ayarlayın. Yapılandırmayı kaydedin.

![Küme ölçeklendirme](./media/quickstart-create-cluster-portal/scale-cluster.png)

Kümenin yeniden boyutlandırılması birkaç dakika sürer.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Batch AI öğreticileri ve örnekleri ile devam etmek istiyorsanız, bu hızlı başlangıçta oluşturulan Batch AI çalışma alanı, dosya sunucusu ve kümesini kullanın.

Temel alınan sanal makineler çalışırken, zamanlanmış herhangi bir iş olmasa da, Batch AI kümesi ve dosya sunucusu için ücretlendirilirsiniz. Çalıştırılacak iş olmadığında küme yapılandırmasını korumak istiyorsanız, kümeyi 0 düğüm olarak yeniden boyutlandırın. Daha sonra, işlerinizi çalıştırmak için 1 veya daha fazla düğüm olarak yeniden boyutlandırın. 

Artık gerekli olmadığında, kümeyi ve dosya sunucusu içeren Batch AI çalışma alanını silin. Bunu yapmak için, Batch AI çalışma alanını seçin ve **Sil** seçeneğini belirleyin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Azure portalı kullanarak bir Batch AI kümesi ve bağlı bir dosya sunucusu oluşturmayı öğrendiniz. Model eğitmek üzere Batch AI kümesi kullanma hakkında daha fazla bilgi edinmek için, ayrıntılı bir öğrenme modelini eğitmeye yönelik hızlı başlangıç adımlarına gidin.

> [!div class="nextstepaction"]
> [Ayrıntılı bir öğrenme modeli eğitme](./quickstart-tensorflow-training-cli.md)
