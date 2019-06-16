---
title: Azure Görüntü Oluşturucu (Önizleme) Windows sanal makineler için bir görüntü Galerisine ile kullanın
description: Windows görüntülerini Azure görüntü oluşturucusu ve paylaşılan görüntü Galerisi ile oluşturun.
author: cynthn
ms.author: cynthn
ms.date: 05/02/2019
ms.topic: article
ms.service: virtual-machines-widows
manager: jeconnoc
ms.openlocfilehash: 2453d37720bcf48b95b428cf78c6186de40b31aa
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65160118"
---
# <a name="preview-create-a-windows-image-and-distribute-it-to-a-shared-image-gallery"></a>Önizleme: Bir Windows görüntüsünü oluşturun ve bir paylaşılan görüntü Galerisine dağıtın 

Bu makalede bir görüntü sürümde oluşturmak için Azure Görüntü Oluşturucu nasıl kullanabileceğinizi gösterecek şekilde olduğu bir [paylaşılan görüntü Galerisi](shared-image-galleries.md), ardından görüntüyü Global olarak dağıtma.

Biz .json şablon görüntüsünü yapılandırmak için kullanır. Kullandığımız .json dosyası aşağıda verilmiştir: [helloImageTemplateforWinSIG.json](https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/1_Creating_a_Custom_Win_Shared_Image_Gallery_Image/helloImageTemplateforWinSIG.json). 

Paylaşılan görüntü Galerisi görüntüsünü dağıtmak için şablonu kullanan [sharedImage](../linux/image-builder-json.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#distribute-sharedimage) değeri olarak `distribute` şablon bölümü.

> [!IMPORTANT]
> Azure Görüntü Oluşturucu şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="register-the-features"></a>Özellikleri kaydetme
Önizleme sırasında Azure Görüntü Oluşturucu kullanmak için yeni özelliği'ni kaydetmeniz gerekir.

```azurecli-interactive
az feature register --namespace Microsoft.VirtualMachineImages --name VirtualMachineTemplatePreview
```

Özellik kaydı durumunu denetleyin.

```azurecli-interactive
az feature show --namespace Microsoft.VirtualMachineImages --name VirtualMachineTemplatePreview | grep state
```

Kaydınızı denetleyin.

```azurecli-interactive
az provider show -n Microsoft.VirtualMachineImages | grep registrationState
az provider show -n Microsoft.Storage | grep registrationState
az provider show -n Microsoft.Compute | grep registrationState
```

Kayıtlı diyor değil, aşağıdaki komutu çalıştırın:

```azurecli-interactive
az provider register -n Microsoft.VirtualMachineImages
az provider register -n Microsoft.Storage
az provider register -n Microsoft.Compute
```

## <a name="set-variables-and-permissions"></a>Değişkenleri ayarlama ve izinleri 

Bu bilgileri depolamak için bazı değişkenler oluşturacağız. böylece biz bazı bilgilere tekrar tekrar kullanacaklardır. Gibi değişkenlerin değerlerini değiştirin `username` ve `vmpassword`, kendi bilgilerinizle.

```azurecli-interactive
# Resource group name - we are using ibsigRG in this example
sigResourceGroup=myIBWinRG
# Datacenter location - we are using West US 2 in this example
location=westus
# Additional region to replicate the image to - we are using East US in this example
additionalregion=eastus
# name of the shared image gallery - in this example we are using myGallery
sigName=my22stSIG
# name of the image definition to be created - in this example we are using myImageDef
imageDefName=winSvrimages
# image distribution metadata reference name
runOutputName=w2019SigRo
# User name and password for the VM
username="azureuser"
vmpassword="passwordfortheVM"
```

Abonelik kimliğiniz için bir değişken oluşturun Bu kullanarak elde edebilirsiniz `az account show | grep id`.

```azurecli-interactive
subscriptionID="Subscription ID"
```

Kaynak grubunu oluşturun.

```azurecli-interactive
az group create -n $sigResourceGroup -l $location
```


Bu kaynak grubunda kaynak oluşturmak üzere Azure Görüntü Oluşturucu izin verin. `--assignee` Uygulama kayıt kimliği Görüntü Oluşturucu hizmeti için bir değerdir. 

```azurecli-interactive
az role assignment create \
    --assignee cf32a0cc-373c-47c9-9156-0db11f6a6dfc \
    --role Contributor \
    --scope /subscriptions/$subscriptionID/resourceGroups/$sigResourceGroup
```





## <a name="create-an-image-definition-and-gallery"></a>Bir görüntü tanımı ve galeri oluşturun

Bir görüntü Galerisine oluşturun. 

```azurecli-interactive
az sig create \
    -g $sigResourceGroup \
    --gallery-name $sigName
```

Bir görüntü tanımı oluşturun.

```azurecli-interactive
az sig image-definition create \
   -g $sigResourceGroup \
   --gallery-name $sigName \
   --gallery-image-definition $imageDefName \
   --publisher corpIT \
   --offer myOffer \
   --sku 2019 \
   --os-type Windows
```


## <a name="download-and-configure-the-json"></a>İndirme ve .json yapılandırma

.Json şablonu indirebilir ve değişkenleri ile yapılandırın.

```azurecli-interactive
curl https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/1_Creating_a_Custom_Win_Shared_Image_Gallery_Image/helloImageTemplateforWinSIG.json -o helloImageTemplateforWinSIG.json
sed -i -e "s/<subscriptionID>/$subscriptionID/g" helloImageTemplateforWinSIG.json
sed -i -e "s/<rgName>/$sigResourceGroup/g" helloImageTemplateforWinSIG.json
sed -i -e "s/<imageDefName>/$imageDefName/g" helloImageTemplateforWinSIG.json
sed -i -e "s/<sharedImageGalName>/$sigName/g" helloImageTemplateforWinSIG.json
sed -i -e "s/<region1>/$location/g" helloImageTemplateforWinSIG.json
sed -i -e "s/<region2>/$additionalregion/g" helloImageTemplateforWinSIG.json
sed -i -e "s/<runOutputName>/$runOutputName/g" helloImageTemplateforWinSIG.json
```

## <a name="create-the-image-version"></a>Görüntü sürümü oluşturma

Bu sonraki bölümü galeride görüntü sürümü oluşturur. 

Görüntü yapılandırma Azure Görüntü Oluşturucu hizmete gönderin.

```azurecli-interactive
az resource create \
    --resource-group $sigResourceGroup \
    --properties @helloImageTemplateforWinSIG.json \
    --is-full-object \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n helloImageTemplateforWinSIG01
```

Görüntü derlemeyi Başlat.

```azurecli-interactive
az resource invoke-action \
     --resource-group $sigResourceGroup \
     --resource-type  Microsoft.VirtualMachineImages/imageTemplates \
     -n helloImageTemplateforWinSIG01 \
     --action Run 
```

Görüntünüzü oluşturup her iki bölgeleri için çoğaltılması biraz zaman alabilir. Bu bölüm, bir VM oluşturmak için geçmeden önce işlemi tamamlanana kadar bekleyin.


## <a name="create-the-vm"></a>Sanal makine oluşturma

Azure Görüntü Oluşturucu tarafından oluşturulan görüntüyü sürümünden bir VM oluşturun.

```azurecli-interactive
az vm create \
  --resource-group $sigResourceGroup \
  --name aibImgWinVm001 \
  --admin-username $username \
  --admin-password $vmpassword \
  --image "/subscriptions/$subscriptionID/resourceGroups/$sigResourceGroup/providers/Microsoft.Compute/galleries/$sigName/images/$imageDefName/versions/latest" \
  --location $location
```


## <a name="verify-the-customization"></a>Özelleştirme doğrulayın
Kullanıcı adı ve parola VM oluşturduğunuz sırada belirlediğiniz kullanarak VM'ye Uzak Masaüstü bağlantısı oluşturun. Sanal makine içinde bir komut istemi açıp:

```console
dir c:\
```

Adlı bir dizin görmelisiniz `buildActions` görüntü özelleştirme sırasında oluşturuldu.


## <a name="clean-up-resources"></a>Kaynakları temizleme
Aynı görüntüyü yeni bir sürümünü oluşturmak için görüntü sürümü yeniden özelleştirme şimdi denemek isterseniz **bu adımı atlayın** ve geçin [başka bir görüntü sürümünü oluşturmak için kullanımı Azure Görüntü Oluşturucu](image-builder-gallery-update-image-version.md).


Bu, tüm diğer kaynak dosyalarla birlikte oluşturulan görüntüyü siler. İşiniz bittiğinde kaynakları silmeden önce bu dağıtımla emin olun.

Bunları oluşturmak için kullanılan da görüntü tanımı silebilmeniz için önce Resim Galerisi kaynakları silerken tüm görüntü sürümlerini silmeniz gerekmez. Bir galeri silmek için önce tüm galeri görüntüsü tanımlarında silmiş gerekir.

Görüntü Oluşturucusu şablonu silin.

```azurecli-interactive
az resource delete \
    --resource-group $sigResourceGroup \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n helloImageTemplateforWinSIG01
```

Görüntü oluşturucusu tarafından oluşturulan görüntü sürümü almak için bu her zaman ile başlayan `0.`ve görüntü sürümü silin

```azurecli-interactive
sigDefImgVersion=$(az sig image-version list \
   -g $sigResourceGroup \
   --gallery-name $sigName \
   --gallery-image-definition $imageDefName \
   --subscription $subscriptionID --query [].'name' -o json | grep 0. | tr -d '"')
az sig image-version delete \
   -g $sigResourceGroup \
   --gallery-image-version $sigDefImgVersion \
   --gallery-name $sigName \
   --gallery-image-definition $imageDefName \
   --subscription $subscriptionID
```   


Görüntü tanımını silin.

```azurecli-interactive
az sig image-definition delete \
   -g $sigResourceGroup \
   --gallery-name $sigName \
   --gallery-image-definition $imageDefName \
   --subscription $subscriptionID
```

Galeri silin.

```azurecli-interactive
az sig delete -r $sigName -g $sigResourceGroup
```

Kaynak grubunu silin.

```azurecli-interactive
az group delete -n $sigResourceGroup -y
```

## <a name="next-steps"></a>Sonraki Adımlar

Oluşturduğunuz görüntü sürümü güncelleştirme konusunda bilgi edinmek için [başka bir görüntü sürümünü oluşturmak için kullanımı Azure Görüntü Oluşturucu](image-builder-gallery-update-image-version.md).