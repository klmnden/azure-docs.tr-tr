---
title: Azure görüntü Oluşturucusu bir görüntü Galerisine ile Linux sanal makineleri (Önizleme) için kullanın.
description: Linux görüntüleri, Azure görüntü oluşturucusu ve paylaşılan görüntü Galerisi ile oluşturun.
author: cynthn
ms.author: cynthn
ms.date: 04/20/2019
ms.topic: article
ms.service: virtual-machines-linux
manager: jeconnoc
ms.openlocfilehash: e9a8a30d9f5f170073c0ad671a248703b1078864
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65159503"
---
# <a name="preview-create-a-linux-image-and-distribute-it-to-a-shared-image-gallery"></a>Önizleme: Bir Linux görüntüsü oluşturun ve bir paylaşılan görüntü Galerisine dağıtın 

Bu makalede, bir görüntü sürümde oluşturmak için Azure Görüntü Oluşturucu nasıl kullanabileceğinizi gösterir. bir [paylaşılan görüntü Galerisi](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/shared-image-galleries), ardından görüntüyü Global olarak dağıtma.


Biz örnek .json şablon görüntüsünü yapılandırmak için kullanır. Kullandığımız .json dosyası aşağıda verilmiştir: [helloImageTemplateforSIG.json](https://github.com/danielsollondon/azvmimagebuilder/blob/master/quickquickstarts/1_Creating_a_Custom_Linux_Shared_Image_Gallery_Image/helloImageTemplateforSIG.json). 

Paylaşılan görüntü Galerisi görüntüsünü dağıtmak için şablonu kullanan [sharedImage](image-builder-json.md#distribute-sharedimage) değeri olarak `distribute` şablon bölümü.

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
```

Kayıtlı diyor değil, aşağıdaki komutu çalıştırın:

```azurecli-interactive
az provider register -n Microsoft.VirtualMachineImages

az provider register -n Microsoft.Storage
```

## <a name="set-variables-and-permissions"></a>Değişkenleri ayarlama ve izinleri 

Bu bilgileri depolamak için bazı değişkenler oluşturacağız. böylece biz bazı bilgilere tekrar tekrar kullanacaklardır.

Önizleme için Görüntü Oluşturucu yalnızca aynı kaynak grubunda kaynak yönetilen görüntüyü özel görüntü oluşturmada destekleyecektir. Bu örnekte, kaynak yönetilen bir görüntü ile aynı kaynak grubunda olması için kaynak grubu adını güncelleştirin.

```azurecli-interactive
# Resource group name - we are using ibLinuxGalleryRG in this example
sigResourceGroup=ibLinuxGalleryRG
# Datacenter location - we are using West US 2 in this example
location=westus2
# Additional region to replicate the image to - we are using East US in this example
additionalregion=eastus
# name of the shared image gallery - in this example we are using myGallery
sigName=myIbGallery
# name of the image definition to be created - in this example we are using myImageDef
imageDefName=myIbImageDef
# image distribution metadata reference name
runOutputName=aibLinuxSIG
```

Abonelik kimliğiniz için bir değişken oluşturun Bu kullanarak elde edebilirsiniz `az account show | grep id`.

```azurecli-interactive
subscriptionID=<Subscription ID>
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
   --publisher myIbPublisher \
   --offer myOffer \
   --sku 18.04-LTS \
   --os-type Linux
```


## <a name="download-and-configure-the-json"></a>İndirme ve .json yapılandırma

.Json şablonu indirebilir ve değişkenleri ile yapılandırın.

```azurecli-interactive
curl https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/1_Creating_a_Custom_Linux_Shared_Image_Gallery_Image/helloImageTemplateforSIG.json -o helloImageTemplateforSIG.json
sed -i -e "s/<subscriptionID>/$subscriptionID/g" helloImageTemplateforSIG.json
sed -i -e "s/<rgName>/$sigResourceGroup/g" helloImageTemplateforSIG.json
sed -i -e "s/<imageDefName>/$imageDefName/g" helloImageTemplateforSIG.json
sed -i -e "s/<sharedImageGalName>/$sigName/g" helloImageTemplateforSIG.json
sed -i -e "s/<region1>/$location/g" helloImageTemplateforSIG.json
sed -i -e "s/<region2>/$additionalregion/g" helloImageTemplateforSIG.json
sed -i -e "s/<runOutputName>/$runOutputName/g" helloImageTemplateforSIG.json
```

## <a name="create-the-image-version"></a>Görüntü sürümü oluşturma

Bu sonraki bölümü galeride görüntü sürümü oluşturur. 

Görüntü yapılandırma Azure Görüntü Oluşturucu hizmete gönderin.

```azurecli-interactive
az resource create \
    --resource-group $sigResourceGroup \
    --properties @helloImageTemplateforSIG.json \
    --is-full-object \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n helloImageTemplateforSIG01
```

Görüntü derlemeyi Başlat.

```azurecli-interactive
az resource invoke-action \
     --resource-group $sigResourceGroup \
     --resource-type  Microsoft.VirtualMachineImages/imageTemplates \
     -n helloImageTemplateforSIG01 \
     --action Run 
```

Görüntünüzü oluşturup her iki bölgeleri için çoğaltılması biraz zaman alabilir. Bu bölüm, bir VM oluşturmak için geçmeden önce işlemi tamamlanana kadar bekleyin.


## <a name="create-the-vm"></a>Sanal makine oluşturma

Azure Görüntü Oluşturucu tarafından oluşturulan görüntüyü sürümünden bir VM oluşturun.

```azurecli-interactive
az vm create \
  --resource-group $sigResourceGroup \
  --name myAibGalleryVM \
  --admin-username aibuser \
  --location $location \
  --image "/subscriptions/$subscriptionID/resourceGroups/$sigResourceGroup/providers/Microsoft.Compute/galleries/$sigName/images/$imageDefName/versions/latest" \
  --generate-ssh-keys
```

SSH VM uygulayın.

```azurecli-interactive
ssh aibuser@<publicIpAddress>
```

Görüntü ile özelleştirilmiş görmelisiniz bir *günün iletisini* SSH bağlantınız kurulur hemen sonra!

```console
*******************************************************
**            This VM was built from the:            **
**      !! AZURE VM IMAGE BUILDER Custom Image !!    **
**         You have just been Customized :-)         **
*******************************************************
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Aynı görüntüyü yeni bir sürümünü oluşturmak için görüntü sürümü yeniden özelleştirme şimdi denemek isterseniz sonraki adımı atlayın ve üzerinde Git [başka bir görüntü sürümünü oluşturmak için kullanımı Azure Görüntü Oluşturucu](image-builder-gallery-update-image-version.md).


Bu, tüm diğer kaynak dosyalarla birlikte oluşturulan görüntüyü siler. İşiniz bittiğinde kaynakları silmeden önce bu dağıtımla emin olun.

Bunları oluşturmak için kullanılan da görüntü tanımı silebilmeniz için önce Resim Galerisi kaynakları silerken tüm görüntü sürümlerini silmeniz gerekmez. Bir galeri silmek için önce tüm galeri görüntüsü tanımlarında silmiş gerekir.

Görüntü Oluşturucusu şablonu silin.

```azurecli-interactive
az resource delete \
    --resource-group $sigResourceGroup \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n helloImageTemplateforSIG01
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

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinin [Azure paylaşılan resim galerileri](shared-image-galleries.md).