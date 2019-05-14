---
title: Öğretici - Azure Red Hat OpenShift kümesi oluşturma | Microsoft Docs
description: Azure CLI kullanarak bir Microsoft Azure Red Hat OpenShift kümesi oluşturmayı öğrenin
services: container-service
author: TylerMSFT
ms.author: twhitney
manager: jeconnoc
ms.topic: tutorial
ms.service: openshift
ms.date: 05/13/2019
ms.openlocfilehash: dda5df0e5b9b9509482cb6dcdcda242b4daa230f
ms.sourcegitcommit: 1fbc75b822d7fe8d766329f443506b830e101a5e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65596356"
---
# <a name="tutorial-create-an-azure-red-hat-openshift-cluster"></a>Öğretici: Azure Red Hat OpenShift kümesi oluşturun

Bu öğretici, bir dizinin birinci bölümüdür. Azure CLI kullanarak bir Microsoft Azure Red Hat OpenShift kümesi oluşturma, ölçeklemek ve kaynakları temizlemek için silin öğreneceksiniz.

Bölümünde bir dizi öğreneceksiniz nasıl yapılır:

> [!div class="checklist"]
> * Azure Red Hat OpenShift kümesi oluşturun

Bu öğretici dizisinde şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * Azure Red Hat OpenShift kümesi oluşturun
> * [Azure Red Hat OpenShift kümesini ölçeklendirme](tutorial-scale-cluster.md)
> * [Azure Red Hat OpenShift kümesini silme](tutorial-delete-cluster.md)

## <a name="prerequisites"></a>Önkoşullar

> [!IMPORTANT]
> Bu öğretici, Azure CLI'yı 2.0.65 sürümünü gerektirir

Bu öğreticiye başlamadan önce:

Emin olun [geliştirme ortamınızı ayarlama](howto-setup-environment.md), içerir:
- En yeni CLI yükleme (2.0.65 sürümü veya üzeri)
- Kiracı zaten yoksa, oluşturma
- Zaten yoksa, bir Azure uygulama nesnesi oluşturma
- Bir güvenlik grubu oluşturma
- Küme oturum açmak için bir Active Directory kullanıcısı oluşturuluyor.

## <a name="step-1-sign-in-to-azure"></a>1. Adım: Oturum açın: Azure

Azure CLI'yi yerel olarak çalıştırıyorsanız, bir Bash komut kabuğu ve çalışma açın `az login` için Azure'da oturum açın.

```bash
az login
```

 Birden çok aboneliğe erişiminiz çalıştırırsanız `az account set -s {subscription ID}` değiştirerek `{subscription ID}` kullanmak istediğiniz aboneliği ile.

## <a name="step-2-create-an-azure-red-hat-openshift-cluster"></a>2. Adım: Azure Red Hat OpenShift kümesi oluşturun

Bir Bash komut penceresinde aşağıdaki değişkenleri ayarlayın:

> [!IMPORTANT]
> İçin bir ad benzersiz olan, küme ve tüm küçük ya da küme oluşturma başarısız olur seçin.

```bash
CLUSTER_NAME=<cluster name in lowercase>
```

Kümenizi oluşturmak için bir konum seçin. Azure üzerinde OpenShift destekleyen bir azure bölgelerin listesi için bkz: [desteklenen bölgeler](supported-resources.md#azure-regions). Örneğin: `LOCATION=eastus`.

```bash
LOCATION=<location>
```

Ayarlama `APPID` adım 5 ', kaydettiğiniz değerine [bir Azure AD uygulama kaydı oluşturma](howto-aad-app-configuration.md#create-an-azure-ad-app-registration).  

```bash
APPID=<app ID value>
```

Adımda 10 kaydettiğiniz değeri 'GROUPID' ayarlayın [bir Azure AD güvenlik grubu oluşturma](howto-aad-app-configuration.md#create-an-azure-ad-security-group).

```bash
GROUPID=<group ID value>
```

Ayarlama `SECRET` , kaydettiğiniz değerine, 8. adım [istemci gizli dizi oluşturma](howto-aad-app-configuration.md#create-a-client-secret).  

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

Sanal ağ (VNET) eşlemesi aracılığıyla oluşturduğunuz kümenin mevcut bir sanal ağa bağlanmak gerekmiyorsa, bu adımı atlayın.

İlk olarak, mevcut bir VNET tanımlayıcısını alın. Tanımlayıcı biçiminde olacaktır: `/subscriptions/{subscription id}/resourceGroups/{resource group of VNET}/providers/Microsoft.Network/virtualNetworks/{VNET name}`.

Ağ adı veya mevcut bir VNET ait olduğu kaynak grubu bilmiyorsanız, Git [sanal ağlar dikey](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Network%2FvirtualNetworks) ve sanal ağınızda'ı tıklatın. Sanal ağ sayfası görünür ve ağ ve ait olduğu kaynak grubu adını listeler.

Bir BASH kabuğunda aşağıdaki CLI komutunu kullanarak bir VNET_ID değişken tanımlayın:

```bash
VNET_ID=$(az network vnet show -n {VNET name} -g {VNET resource group} --query id -o tsv)
```

Örneğin, `VNET_ID=$(az network vnet show -n MyVirtualNetwork -g MyResourceGroup --query id -o tsv`

### <a name="create-the-cluster"></a>Kümeyi oluşturma

Bir küme oluşturmak artık hazırsınız. Şu, belirtilen Azure AD'de kümeyi oluşturacak Kiracı, Azure AD uygulama nesnesi ve bir güvenlik sorumlusu ve küme için yönetici erişimi olan üyeleri içeren bir güvenlik grubu olarak kullanılacak parolayı belirtin.

Eğer **değil** kümenize bir sanal ağ eşlemesi, aşağıdaki komutu kullanın:

```bash
az openshift create --resource-group $CLUSTER_NAME --name $CLUSTER_NAME -l $LOCATION --aad-client-app-id $APPID --aad-client-app-secret $SECRET --aad-tenant-id $TENANT --customer-admin-group-id $GROUPID
```

Varsa, **olan** kümenize bir sanal ağ eşlemesi ekleyen aşağıdaki komutu kullanın `--vnet-peer` bayrağı:
 
```bash
az openshift create --resource-group $CLUSTER_NAME --name $CLUSTER_NAME -l $LOCATION --aad-client-app-id $APPID --aad-client-app-secret $SECRET --aad-tenant-id $TENANT --customer-admin-group-id $GROUPID --vnet-peer $VNET_ID
```

> [!NOTE]
> Konak adı kullanılabilir değil bir hata alırsanız, küme adınızı benzersiz olmadığı için olabilir. Deneyin, özgün bir uygulama kaydı silme ve yeni bir kullanıcı ve güvenlik grubu oluşturma adımı atlayarak farklı bir küme adı [oluştur yeni bir uygulama kaydı] (howto-aad-app-configuration.md#create-a-new-app-registration) adımlarla yineleme.

Birkaç dakika sonra `az openshift create` tamamlanır.

### <a name="get-the-sign-in-url-for-your-cluster"></a>Oturum açma URL'SİNDE kümeniz için Al

Aşağıdaki komutu çalıştırarak kümeniz için oturum açmak için URL'yi alın:

```bash
az openshift show -n $CLUSTER_NAME -g $CLUSTER_NAME
```

Aranacak `publicHostName` çıkışında, örneğin: `"publicHostname": "openshift.xxxxxxxxxxxxxxxxxxxx.eastus.azmosa.io"`

URL'de oturum kümenizin `https://` ardından `publicHostName` değeri.  Örneğin: `https://openshift.xxxxxxxxxxxxxxxxxxxx.eastus.azmosa.io`.  Bu URI sonraki adımda uygulama kayıt yeniden yönlendirme URI'si bir parçası olarak kullanır.

## <a name="step-3-update-your-app-registration-redirect-uri"></a>3. adım: Güncelleştirme, uygulama kayıt yeniden yönlendirme URI'si

Küme için URL'de oturum edindikten sonra uygulama kayıt yeniden yönlendirme UI ayarlayın:

1. Açık [uygulama kayıtları dikey penceresinde](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredAppsPreview).
2. Uygulama kayıt nesnenize tıklayın.
3. Tıklayarak **yeniden yönlendirme URI'si ekleyin**.
4. Emin **türü** olduğu **Web** ayarlayıp **yeniden yönlendirme URI'si** şu biçimi kullanarak: `https://<public host name>/oauth2callback/Azure%20AD`. Örneğin, `https://openshift.xxxxxxxxxxxxxxxxxxxx.eastus.azmosa.io/oauth2callback/Azure%20AD`
5. **Kaydet**'e tıklayın.

## <a name="step-4-sign-in-to-the-openshift-console"></a>4. Adım: OpenShift konsoluna oturum açın

Artık yeni kümenizin OpenShift konsol oturum açmaya hazırsınız. [OpenShift Web Konsolu](https://docs.openshift.com/aro/architecture/infrastructure_components/web_console.html) görselleştirin, göz atın ve OpenShift projelerinizi içeriğini yönetmek için etkinleştirir.

Azure portalında oturum açmak için normalde kullandığınız kimliğin önbelleğe edilmemiş bir yeni tarayıcı örneği gerekir.

1. Açık bir *ıncognito* penceresi (Chrome) veya *InPrivate* penceresi (Microsoft Edge).
2. Yukarıdaki örnek elde ettiğiniz oturum açma URL'sine gidin: `https://openshift.xxxxxxxxxxxxxxxxxxxx.eastus.azmosa.io`

3. adımında oluşturduğunuz kullanıcı adını kullanarak oturum [yeni bir Azure Active Directory kullanıcısı oluşturma](howto-aad-app-configuration.md#create-a-new-azure-active-directory-user).

A **istenen izinleri** iletişim kutusu görüntülenir. Tıklayın **onay kuruluşunuz adına** ve ardından **kabul**.

Artık, bir kümenin konsolda günlüğe kaydedilir.

![OpenShift küme Konsolu ekran görüntüsü](./media/aro-console.png)

 Daha fazla bilgi edinin [OpenShift konsolunu kullanarak](https://docs.openshift.com/aro/getting_started/developers_console.html) oluşturmak için ve yerleşik görüntüleri [Red Hat OpenShift](https://docs.openshift.com/aro/welcome/index.html) belgeleri.

## <a name="step-5-install-the-openshift-cli"></a>5. Adım: OpenShift CLI'yı yükleme

[OpenShift CLI](https://docs.openshift.com/aro/cli_reference/get_started_cli.html) (veya *OC Araçları*) OpenShift kümenizin çeşitli bileşenleri ile etkileşim kurmak için alt düzey yardımcı programları ve uygulamaları yönetmek için komutlar sağlar.

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
> * Azure Red Hat OpenShift kümesi oluşturun

Sonraki öğreticiye ilerleyin:
> [!div class="nextstepaction"]
> [Azure Red Hat OpenShift kümesini ölçeklendirme](tutorial-scale-cluster.md)