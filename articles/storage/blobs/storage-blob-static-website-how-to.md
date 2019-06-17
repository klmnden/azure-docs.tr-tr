---
title: Azure Depolama'daki statik Web sitesi barındırma
description: Bir kapsayıcıya doğrudan statik içerik (HTML, CSS, JavaScript ve görüntü dosyaları) bir Azure depolama GPv2 hesabına sunmayı öğrenin.
services: storage
author: normesta
ms.service: storage
ms.topic: article
ms.author: normesta
ms.date: 05/28/2019
ms.openlocfilehash: 61477767c59dd521e3f46db4445238a5a1ea759e
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67071448"
---
# <a name="host-a-static-website-in-azure-storage"></a>Azure Depolama'daki statik Web sitesi barındırma

Bir Azure depolama GPv2 hesabına bir kapsayıcıya doğrudan statik içerik (HTML, CSS, JavaScript ve görüntü dosyaları) görebilir. Daha fazla bilgi için bkz. [Azure Depolama'da statik Web sitesi barındırma](storage-blob-static-website.md).

Bu makalede Azure portalı, Azure CLI veya PowerShell kullanarak statik Web sitesi barındırma etkinleştirme gösterilmektedir.

<a id="portal" />

## <a name="use-the-azure-portal"></a>Azure portalı kullanma

Adım adım bir öğretici için bkz [Öğreticisi: Blob Depolama üzerinde statik Web sitesi barındırma](https://docs.microsoft.com/azure/storage/blobs/storage-blob-static-website-host).

Statik Web sitesi barındırma etkinleştirdikten sonra Web sitesinin genel URL'yi kullanarak bir tarayıcıdan site sayfaları görüntüleyebilirsiniz.

<a id="portal-find-url" />

### <a name="find-the-website-url-by-using-the-azure-portal"></a>Azure portalını kullanarak Web sitesi URL'si bulunamadı

Depolama hesabınızın hesap genel bakış sayfası görünen bölmesinde seçin **statik Web sitesi**. Sitenizin URL'sini görünür **birincil uç noktayı** alan.

![Azure depolama statik Web siteleri ölçümleri ölçüm](./media/storage-blob-static-website/storage-blob-static-website-url.png)

<a id="cli" />

## <a name="use-the-azure-cli"></a>Azure CLI kullanma

Kullanarak statik Web sitesi barındırma etkinleştirebilirsiniz [Azure komut satırı arabirimi (CLI)](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest).

1. İlk olarak, açık [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview?view=azure-cli-latest), veya [yüklü](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) Azure CLI'yi yerel olarak bir komut konsol uygulaması Windows PowerShell gibi açın.

2. Açtığınız komut penceresinden depolama Önizleme uzantıyı yükleyin.

   ```azurecli-interactive
   az extension add --name storage-preview
   ```

3. Kimliğinizi birden fazla abonelik ile ilişkilidir, etkin abonelik için abonelik statik Web sitenizi barındıran depolama hesabı ayarlayın.

   ```azurecli-interactive
   az account set --subscription <subscription-id>
   ```

   Değiştirin `<subscription-id>` yer tutucu değerini, abonelik kimliği.

4. Statik Web sitesi barındırma etkinleştirin.

   ```azurecli-interactive
   az storage blob service-properties update --account-name <storage-account-name> --static-website --404-document <error-document-name> --index-document <index-document-name>
   ```

   * Değiştirin `<storage-account-name>` yer tutucu değerini, depolama hesabınızın adı.

   * Değiştirin `<error-document-name>` yer tutucu bir tarayıcı sitenizdeki mevcut bir sayfa istediğinde, kullanıcıların göreceği hata belgenin adı.

   * Değiştirin `<index-document-name>` dizin belgesi adı ile yer tutucu. Bu belge, yaygın olarak "index.html" yöneliktir.

5. Nesnelere karşıya *$web* kaynak dizinden kapsayıcı.

   > [!NOTE]
   > Azure Cloud Shell kullanıyorsanız, eklediğinizden emin olun. bir `\` söz konusu olduğunda kaçış karakteri `$web` kapsayıcı (örneğin: `\$web`). Azure CLI'yı yerel bir yüklemesini kullanıyorsanız, kaçış karakterini kullanmanız gerekmez.

   Bu örnekte, Azure Cloud Shell oturumundan komutlarını çalıştırmakta olduğunuz varsayılır.

   ```azurecli-interactive
   az storage blob upload-batch -s <source-path> -d \$web --account-name <storage-account-name>
   ```

   * Değiştirin `<storage-account-name>` yer tutucu değerini, depolama hesabınızın adı.

   * Değiştirin `<source-path>` karşıya yüklemek istediğiniz dosyaları konumuna bir yolu ile yer tutucu.

   > [!NOTE]
   > Azure CLI'ın yükleme konumu kullanmakta olduğunuz sonra yerel bilgisayarınızda herhangi bir konuma yol kullanabilirsiniz (örneğin: `C:\myFolder`.
   >
   > Azure Cloud Shell kullanıyorsanız Cloud Shell için görünür olan bir dosya paylaşımı başvurmak zorunda kalırsınız. Bu konum, dosya paylaşımı bulutun paylaşmak kendisini veya Cloud Shell'den bağlama var olan bir dosya paylaşımı olabilir. Bunu yapma hakkında bilgi için bkz: [kalıcı dosyaları Azure Cloud shell'de](https://docs.microsoft.com/azure/cloud-shell/persisting-shell-storage).

<a id="cli-find-url" />

### <a name="find-the-website-url-by-using-the-azure-cli"></a>Azure CLI kullanarak Web sitesi URL'si bulunamadı

Bir tarayıcıdan içerik Web sitesinin genel URL'yi kullanarak görüntüleyebilirsiniz.

URL, aşağıdaki komutu kullanarak bulun:

```azurecli-interactive
az storage account show -n <storage-account-name> -g <resource-group-name> --query "primaryEndpoints.web" --output tsv
```

* Değiştirin `<storage-account-name>` yer tutucu değerini, depolama hesabınızın adı.

* Değiştirin `<resource-group-name>` yer tutucu değerini, kaynak grubunuzun adı.

<a id="powershell" />

## <a name="use-powershell"></a>PowerShell kullanma

Azure PowerShell modülü kullanarak statik Web sitesi barındırma etkinleştirebilirsiniz.

1. Bir Windows PowerShell komut penceresi açın.

2. Azure PowerShell modülü Az 0.7 veya üzeri olduğunu doğrulayın.

   ```powershell
   Get-InstalledModule -Name Az -AllVersions | select Name,Version
   ```

   Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure PowerShell Modülü yükleme](/powershell/azure/install-Az-ps).

3. `Connect-AzAccount` komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin.

   ```powershell
   Connect-AzAccount
   ```

4. Kimliğinizi birden fazla abonelik ile ilişkilidir, etkin abonelik için abonelik statik Web sitenizi barındıran depolama hesabı ayarlayın.

   ```powershell
   $context = Get-AzSubscription -SubscriptionId <subscription-id>
   Set-AzContext $context
   ```

   Değiştirin `<subscription-id>` yer tutucu değerini, abonelik kimliği.

5. Kullanmak istediğiniz depolama hesabını tanımlayan depolama hesabı bağlamını alın.

   ```powershell
   $storageAccount = Get-AzStorageAccount -ResourceGroupName "<resource-group-name>" -AccountName "<storage-account-name>"
   $ctx = $storageAccount.Context
   ```

   * Değiştirin `<resource-group-name>` yer tutucu değerini, kaynak grubunuzun adı.

   * Değiştirin `<storage-account-name>` yer tutucu değerini, depolama hesabınızın adı.

6. Statik Web sitesi barındırma etkinleştirin.

   ```powershell
   Enable-AzStorageStaticWebsite -Context $ctx -IndexDocument <index-document-name> -ErrorDocument404Path <error-document-name>
   ```

   * Değiştirin `<error-document-name>` yer tutucu bir tarayıcı sitenizdeki mevcut bir sayfa istediğinde, kullanıcıların göreceği hata belgenin adı.

   * Değiştirin `<index-document-name>` dizin belgesi adı ile yer tutucu. Bu belge, yaygın olarak "index.html" yöneliktir.

7. Nesnelere karşıya *$web* kaynak dizinden kapsayıcı.

    ```powershell
    # upload a file
    set-AzStorageblobcontent -File "<path-to-file>" `
    -Container `$web `
    -Blob "<blob-name>" `
    -Context $ctx
     ```

   * Değiştirin `<path-to-file>` yer tutucu değerini, karşıya yüklemek istediğiniz dosyanın tam yolunu (örneğin: `C:\temp\index.html`).

   * Değiştirin `<blob-name>` yer tutucu değerini, sonuçta elde edilen blob vermek istediğiniz adı (örneğin: `index.html`).

<a id="powershell-find-url" />

### <a name="find-the-website-url-by-using-powershell"></a>PowerShell kullanarak Web sitesi URL'si bulunamadı

Bir tarayıcıdan içerik Web sitesinin genel URL'yi kullanarak görüntüleyebilirsiniz.

URL, aşağıdaki komutu kullanarak bulun:

```powershell
 $storageAccount = Get-AzStorageAccount -ResourceGroupName "<resource-group-name>" -AccountName "<storage-account-name>"
Write-Output $storageAccount.PrimaryEndpoints.Web
```

* Değiştirin `<resource-group-name>` yer tutucu değerini, kaynak grubunuzun adı.

* Değiştirin `<storage-account-name>` yer tutucu değerini, depolama hesabınızın adı.

<a id="metrics" />

## <a name="enable-metrics-on-static-website-pages"></a>Statik Web sitesi sayfalarında ölçümlerini etkinleştir

Ölçümleri etkinleştirdikten sonra dosyalarda istatistikleri trafiği **$web** kapsayıcı ölçümleri panosunda bildirilir.

1. Tıklayarak **ayarları** > **izleme** > **ölçümleri**.

   Ölçüm verilerini farklı ölçümlerinde API'leri takma tarafından oluşturulur. Portal, yalnızca belirli bir zaman dilimi içinde yalnızca veri döndüren üyelerde odaklanabilmek için kullanılan API üyelerini görüntüler. Gerekli API üye seçebilir emin olmak için zaman aralığını genişletmek için ilk adım olacaktır.

2. Zaman çerçevesini düğmesine tıklayıp **son 24 saat** ve ardından **Uygula**.

   ![Azure depolama statik Web siteleri ölçümleri zaman aralığı](./media/storage-blob-static-website/storage-blob-static-website-metrics-time-range.png)

3. Seçin **Blob** gelen *Namespace* açılır.

   ![Azure depolama statik Web siteleri ölçümleri ad alanı](./media/storage-blob-static-website/storage-blob-static-website-metrics-namespace.png)

4. Ardından **çıkış** ölçümü.

   ![Azure depolama statik Web siteleri ölçümleri ölçüm](./media/storage-blob-static-website/storage-blob-static-website-metrics-metric.png)

5. Seçin **toplam** gelen *toplama* Seçici.

   ![Azure depolama statik Web siteleri ölçümleri toplama](./media/storage-blob-static-website/storage-blob-static-website-metrics-aggregation.png)

6. Tıklayın **Filtre Ekle** düğmesini tıklatın ve seçin **API adı** gelen *özelliği* Seçici.

   ![Azure depolama statik Web siteleri ölçümleri API adı](./media/storage-blob-static-website/storage-blob-static-website-metrics-api-name.png)

7. onay kutusu yanındaki **GetWebContent** içinde *değerleri* ölçümleri raporu doldurmak için Seçici.

   ![Azure depolama statik Web siteleri ölçümleri GetWebContent](./media/storage-blob-static-website/storage-blob-static-website-metrics-getwebcontent.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Depolama'da statik web sitesi barındırma](storage-blob-static-website.md)
* [HTTP'ler üzerinden özel etki alanlarıyla bloblara erişmek için Azure CDN'yi kullanma](storage-https-custom-domain-cdn.md)
* [Blob veya web uç noktanız için özel etki alanı adı yapılandırma](storage-custom-domain-name.md)
* [Azure İşlevleri](/azure/azure-functions/functions-overview)
* [Azure uygulama hizmeti](/azure/app-service/overview)
* [İlk sunucusuz web uygulamanızı oluşturun](https://docs.microsoft.com/azure/functions/tutorial-static-website-serverless-api-with-database)
* [Öğretici: Etki alanınızı Azure DNS'de barındırın](../../dns/dns-delegate-domain-azure-dns.md)
