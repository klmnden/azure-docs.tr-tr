---
title: Öğretici - Azure Red Hat OpenShift kümesi oluşturma | Microsoft Docs
description: Azure CLI kullanarak bir Microsoft Azure Red Hat OpenShift kümesi oluşturmayı öğrenin
services: container-service
author: TylerMSFT
ms.author: twhitney
manager: jeconnoc
ms.topic: tutorial
ms.service: openshift
ms.date: 05/06/2019
ms.openlocfilehash: 5bc71a2d0f29fed163fb5c93ebd27df7f66a1325
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65080761"
---
# <a name="tutorial-create-an-azure-red-hat-openshift-cluster"></a>Öğretici: Azure Red Hat OpenShift kümesi oluşturma

Bu öğretici, bir dizinin birinci bölümüdür. Azure CLI kullanarak bir Microsoft Azure Red Hat OpenShift kümesi oluşturma, ölçeklemek ve kaynakları temizlemek için silin öğreneceksiniz.

Bölümünde bir dizi öğreneceksiniz nasıl yapılır:

> [!div class="checklist"]
> * Azure Red Hat OpenShift kümesi oluşturma

Bu öğretici dizisinde şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * Azure Red Hat OpenShift kümesi oluşturma
> * [Azure Red Hat OpenShift kümesini ölçeklendirme](tutorial-scale-cluster.md)
> * [Azure Red Hat OpenShift küme silme](tutorial-delete-cluster.md)

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiye başlamadan önce:

Emin olun [geliştirme ortamınızı ayarlama](howto-setup-environment.md), içerir:
- En yeni CLI yükleme
- Kiracı oluşturma
- Azure uygulama nesnesi oluşturma
- Küme üzerinde çalışan uygulamalara oturum açmak için kullanılan bir Active Directory kullanıcısı oluşturuluyor.

## <a name="step-1-sign-in-to-azure"></a>1. Adım: Azure'da oturum açma

Azure CLI'yi yerel olarak çalıştırıyorsanız, bir Bash komut kabuğu ve çalışma açın `az login` için Azure'da oturum açın.

```bash
az login
```

 Birden çok aboneliğe erişiminiz çalıştırırsanız `az account set -s {subscription ID}` değiştirerek `{subscription ID}` kullanmak istediğiniz aboneliği ile.

## <a name="step-2-create-an-azure-red-hat-openshift-cluster"></a>2. Adım: Azure Red Hat OpenShift kümesi oluşturma

Bash komut pencerenizde aşağıdaki değişkenleri ayarlayın:

> [!IMPORTANT]
> Kümenizin adını harflerden oluşmalıdır veya küme oluşturma başarısız olur.

```bash
CLUSTER_NAME=<cluster name in lowercase>
```

 6. adımında seçtiğiniz küme için aynı adı kullanın [yeni uygulama kaydı oluşturma](howto-aad-app-configuration.md#create-a-new-app-registration).

```bash
LOCATION=<location>
```

Kümenizi oluşturmak için bir konum seçin. Azure üzerinde OpenShift destekleyen bir azure bölgelerin listesi için bkz: [desteklenen bölgeler](supported-resources.md#azure-regions). Örneğin: `LOCATION=eastus`.

Ayarlama `FQDN` kümenizin tam adı. Küme adı, konum, bu adı oluşur ve `.cloudapp.azure.com` sonuna. Oturum açma 6. adımında oluşturduğunuz URL'si ile aynıdır [yeni uygulama kaydı oluşturma](howto-aad-app-configuration.md#create-a-new-app-registration). Örneğin:  

```bash
FQDN=$CLUSTER_NAME.$LOCATION.cloudapp.azure.com
```

Ayarlama `APPID` , kaydettiğiniz değerine, 9. adım [yeni bir uygulama kaydı oluşturma](howto-aad-app-configuration.md#create-a-new-app-registration).  

```bash
APPID=<app ID value>
```

Ayarlama `SECRET` , kaydettiğiniz değerine, 6. adım [istemci gizli dizi oluşturma](howto-aad-app-configuration.md#create-a-client-secret).  

```bash
SECRET=<secret value>
```

Ayarlama `TENANT` , kaydettiğiniz Kiracı kimliği değeri, 7. adım [yeni Kiracı oluşturma](howto-create-tenant.md#create-a-new-azure-ad-tenant)  

```bash
TENANT=<tenant ID>
```

Küme için kaynak grubu oluşturun. Aşağıdaki komut yukarıdaki değişkenleri tanımlamak için kullanılan Bash kabuğundan çalıştırın:

```bash
az group create --name $CLUSTER_NAME --location $LOCATION
```

### <a name="optional-connect-the-clusters-virtual-network-to-an-existing-virtual-network"></a>İsteğe bağlı: Mevcut bir sanal ağa kümenin sanal ağ bağlama

Sanal ağ (VNET), oluşturduğunuz kümenin mevcut bir sanal ağa bağlanmak gerekmiyorsa, bu adımı atlayın.

İlk olarak, mevcut bir VNET tanımlayıcısını alın. Tanımlayıcı biçiminde olacaktır: `/subscriptions/{subscription id}/resourceGroups/{resource group of VNET}/providers/Microsoft.Network/virtualNetworks/{VNET name}`.

Ağ adı veya mevcut bir VNET ait olduğu kaynak grubu bilmiyorsanız, Git [sanal ağlar dikey](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Network%2FvirtualNetworks) ve sanal ağınızda'ı tıklatın. Sanal ağ sayfası görünür ve ağ ve ait olduğu kaynak grubu adını listeler.

Bir BASH kabuğunda aşağıdaki CLI komutunu kullanarak bir VNET_ID değişken tanımlayın:

```bash
VNET_ID=$(az network vnet show -n {VNET name} -g {VNET resource group} --query id -o tsv)
```

Örneğin, `VNET_ID=$(az network vnet show -n MyVirtualNetwork -g MyResourceGroup --query id -o tsv`

### <a name="create-the-cluster"></a>Kümeyi oluşturma

Bir küme oluşturmak artık hazırsınız.

 Mevcut bir sanal ağa kümenin sanal ağ bağlanmıyorsanız sondaki atlamak `--vnet-peer-id $VNET_ID` aşağıdaki örnekte parametre.

```bash
az openshift create --resource-group $CLUSTER_NAME --name $CLUSTER_NAME -l $LOCATION --fqdn $FQDN --aad-client-app-id $APPID --aad-client-app-secret $SECRET --aad-tenant-id $TENANT --vnet-peer-id $VNET_ID
```

Birkaç dakika sonra `az openshift create` başarıyla tamamlanabilmesi ve küme ayrıntılarınızı içeren bir JSON yanıtı döndürür.

> [!NOTE]
> Konak adı kullanılabilir değil bir hata alırsanız, küme adınızı benzersiz olmadığı için olabilir. Deneyin, özgün bir uygulama kaydı silme ve (son adım, önceden oluşturulan bu yana yeni bir kullanıcı oluşturma atlayarak) adımları [yeni bir uygulama kaydı oluşturma] (howto-aad-app-configuration.md#create-a-new-app-registration) ile yineleme bir farklı bir küme adı.

## <a name="step-3-sign-in-to-the-openshift-console"></a>3. Adım: OpenShift konsoluna oturum açın

Artık yeni kümenizin OpenShift konsola oturum açmanız için hazırsınız. [OpenShift Web Konsolu](https://docs.openshift.com/dedicated/architecture/infrastructure_components/web_console.html) görselleştirin, göz atın ve OpenShift projelerinizi içeriğini yönetmek için etkinleştirir.

Biz olarak oturum açmanız [yeni Azure AD kullanıcı](howto-aad-app-configuration.md#create-a-new-active-directory-user) test etmek için oluşturuldu. Bunu yapmak için Azure portalında oturum açmak için normalde kullandığınız kimliğin önbelleğe edilmemiş bir yeni tarayıcı örneği gerekir.

1. Açık bir *ıncognito* penceresi (Chrome) veya *InPrivate* penceresi (Microsoft Edge).
2. 6. adımında oluşturduğunuz oturum açma URL'sine gidin [yeni bir uygulama kaydı oluşturma](howto-aad-app-configuration.md#create-a-new-app-registration). Örneğin, https://constoso.eastus.cloudapp.azure.com

> [!NOTE]
> OpenShift Konsolu otomatik olarak imzalanan bir sertifika kullanır.
> Tarayıcınızda istendiğinde, uyarı atlayabilir ve "güvenilmeyen" sertifikayı kabul edin.

Oluşturduğunuz parola ve kullanıcı oturum [yeni bir Active Directory kullanıcısı oluşturma](howto-aad-app-configuration.md#create-a-new-active-directory-user) olduğunda **istenen izinleri** iletişim kutusu görüntülenirse, seçin **kuruluşunuz adına onayı**  ardından **kabul**.

Artık, bir kümenin konsolda günlüğe kaydedilir.

[OpenShift küme Konsolu ekran görüntüsü](./media/aro-console.png)

 Daha fazla bilgi edinebilirsiniz [OpenShift konsolunu kullanarak](https://docs.openshift.com/dedicated/getting_started/developers_console.html) oluşturmak için ve yerleşik görüntüleri [Red Hat OpenShift](https://docs.openshift.com/dedicated/welcome/index.html) belgeleri.

## <a name="step-4-install-the-openshift-cli"></a>4. Adım: OpenShift CLI'yı yükleme

[OpenShift CLI](https://docs.openshift.com/dedicated/cli_reference/get_started_cli.html) (veya *OC Araçları*) OpenShift kümenizin çeşitli bileşenleri ile etkileşim kurmak için alt düzey yardımcı programları ve uygulamaları yönetmek için komutlar sağlar.

OpenShift konsolunda, sağ üst köşedeki soru işareti ve oturum açma adınız tıklayıp **komut satırı araçları**.  İzleyin **en son sürüm** indirmek ve Linux, MacOS veya Windows için desteklenen oc CLI yüklemek için bağlantı.

> [!NOTE]
> Sağ üst köşedeki soru işareti simgesini görmüyorsanız seçin *hizmet Kataloğu* veya *uygulama Konsolu* üst sol açılır listeden.
>
> Alternatif olarak, şunları yapabilirsiniz [oc CLI indirme](https://www.okd.io/download.html) doğrudan.

**Komut satırı araçları** sayfası formunun bir komut sağlar `oc login https://<your cluster name>.<azure region>.cloudapp.azure.com --token=<token value>`.  Tıklayın *Panoya Kopyala* düğmesini bu komutu kopyalayın.  Bir terminal penceresinde, [yolunuzu ayarlamak](https://docs.okd.io/latest/cli_reference/get_started_cli.html#installing-the-cli) oc Araçları'nı yerel yüklemesinde eklenecek. Ardından kümeye kopyaladığınız oc CLI komutunu kullanarak oturum açın.

Yukarıdaki adımları kullanarak belirteç değeri alınamadı, belirteç değerini Al: `https://<your cluster name>.<azure region>.cloudapp.azure.com/oauth/token/request`.

## <a name="next-steps"></a>Sonraki adımlar

Öğreticinin bu bölümünde, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Azure Red Hat OpenShift kümesi oluşturma

Sonraki öğreticiye ilerleyin:
> [!div class="nextstepaction"]
> [Azure Red Hat OpenShift kümesini ölçeklendirme](tutorial-scale-cluster.md)