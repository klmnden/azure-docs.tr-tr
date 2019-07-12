---
title: Azure Görüntü Oluşturucu (Önizleme) ile Windows VM oluşturma
description: Bir Windows VM, Azure Görüntü Oluşturucu ile oluşturun.
author: cynthn
ms.author: cynthn
ms.date: 05/02/2019
ms.topic: article
ms.service: virtual-machines-windows
manager: gwallace
ms.openlocfilehash: fec6d83870e20b7622f37c52847803d4f03cbba5
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67722682"
---
# <a name="preview-create-a-windows-vm-with-azure-image-builder"></a>Önizleme: Azure Görüntü Oluşturucu ile bir Windows VM oluşturma

Bu makale, Azure VM görüntü Oluşturucusu kullanarak özelleştirilmiş bir Windows görüntüsünü nasıl oluşturacağınızı göstermek için yazılmıştır. Bu makaledeki örnekte üç farklı kullanan [özelleştiricilerin](../linux/image-builder-json.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#properties-customize) görüntüsünü özelleştirmek için:
- PowerShell (ScriptUri) - indirme ve çalıştırma bir [PowerShell Betiği](https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/testPsScript.ps1).
- Windows yeniden başlatma - VM yeniden başlatılır.
- PowerShell (satır içi) - belirli bir komut çalıştırın. Bu örnekte, bir dizin kullanarak sanal makine oluşturur `mkdir c:\\buildActions`.
- Dosya - VM'ye github'dan bir dosyaya kopyalayın. Bu örnek kopyalar [index.md](https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/exampleArtifacts/buildArtifacts/index.html) için `c:\buildArtifacts\index.html` VM üzerinde.

Biz örnek .json şablon görüntüsünü yapılandırmak için kullanır. Kullandığımız .json dosyası aşağıda verilmiştir: [helloImageTemplateWin.json](https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/0_Creating_a_Custom_Windows_Managed_Image/helloImageTemplateWin.json). 


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

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Bu bilgileri depolamak için bazı değişkenler oluşturacağız. böylece biz bazı bilgilere tekrar tekrar kullanacaklardır.

```azurecli-interactive
# Resource group name - we are using myImageBuilderRG in this example
imageResourceGroup=myWinImgBuilderRG
# Region location 
location=WestUS2
# Name for the image 
imageName=myWinBuilderImage
# Run output name
runOutputName=aibWindows
# name of the image to be created
imageName=aibWinImage
```

Abonelik kimliğiniz için bir değişken oluşturun Bu kullanarak elde edebilirsiniz `az account show | grep id`.

```azurecli-interactive
subscriptionID=<Your subscription ID>
```

Kaynak grubunu oluşturun.

```azurecli-interactive
az group create -n $imageResourceGroup -l $location
```

Bu kaynak grubunda kaynak oluşturmak üzere Görüntü Oluşturucu izin verin. `--assignee` Uygulama kayıt kimliği Görüntü Oluşturucu hizmeti için bir değerdir. 

```azurecli-interactive
az role assignment create \
    --assignee cf32a0cc-373c-47c9-9156-0db11f6a6dfc \
    --role Contributor \
    --scope /subscriptions/$subscriptionID/resourceGroups/$imageResourceGroup
```


## <a name="download-the-json-example"></a>.Json örneği indirin

Örnek .json dosyasını indirin ve oluşturduğunuz değişkenleri ile yapılandırın.

```azurecli-interactive
curl https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/0_Creating_a_Custom_Windows_Managed_Image/helloImageTemplateWin.json -o helloImageTemplateWin.json
sed -i -e "s/<subscriptionID>/$subscriptionID/g" helloImageTemplateWin.json
sed -i -e "s/<rgName>/$imageResourceGroup/g" helloImageTemplateWin.json
sed -i -e "s/<region>/$location/g" helloImageTemplateWin.json
sed -i -e "s/<imageName>/$imageName/g" helloImageTemplateWin.json
sed -i -e "s/<runOutputName>/$runOutputName/g" helloImageTemplateWin.json

```

## <a name="create-the-image"></a>Görüntü oluşturma

Görüntü yapılandırma VM Görüntü Oluşturucu hizmete gönderme

```azurecli-interactive
az resource create \
    --resource-group $imageResourceGroup \
    --properties @helloImageTemplateWin.json \
    --is-full-object \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n helloImageTemplateWin01
```

Görüntü derlemeyi Başlat.

```azurecli-interactive
az resource invoke-action \
     --resource-group $imageResourceGroup \
     --resource-type  Microsoft.VirtualMachineImages/imageTemplates \
     -n helloImageTemplateWin01 \
     --action Run 
```

Yapı tamamlanana kadar bekleyin. Bu işlem yaklaşık 15 dakika sürebilir.

## <a name="create-the-vm"></a>Sanal makine oluşturma

Oluşturulan görüntüyü kullanarak VM'yi oluşturun. Değiştirin *<password>* kendi parolanızı ile `aibuser` VM üzerinde.

```azurecli-interactive
az vm create \
  --resource-group $imageResourceGroup \
  --name aibImgWinVm00 \
  --admin-username aibuser \
  --admin-password <password> \
  --image $imageName \
  --location $location
```

## <a name="verify-the-customization"></a>Özelleştirme doğrulayın

Kullanıcı adı ve parola VM oluşturduğunuz sırada belirlediğiniz kullanarak VM'ye Uzak Masaüstü bağlantısı oluşturun. Sanal makine içinde bir komut istemi açıp:

```console
dir c:\
```

Görüntü özelleştirme sırasında oluşturulan bu iki dizin görmeniz gerekir:
- buildActions
- buildArtifacts

## <a name="clean-up"></a>Temizleme

İşiniz bittiğinde kaynakları silin.

```azurecli-interactive
az resource delete \
    --resource-group $imageResourceGroup \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n helloImageTemplateWin01
az group delete -n $imageResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan .json dosyası bileşenleri hakkında daha fazla bilgi için bkz. [Görüntü Oluşturucu şablon başvurusu](../linux/image-builder-json.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

