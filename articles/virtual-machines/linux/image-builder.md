---
title: Azure Görüntü Oluşturucu (Önizleme) ile bir Linux VM oluşturma
description: Bir Linux VM, Azure Görüntü Oluşturucu ile oluşturun.
author: cynthn
ms.author: cynthn
ms.date: 05/02/2019
ms.topic: article
ms.service: virtual-machines-linux
manager: jeconnoc
ms.openlocfilehash: 345b10a0d66456d795a63e3aacd941ade0e0159c
ms.sourcegitcommit: c63e5031aed4992d5adf45639addcef07c166224
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67467011"
---
# <a name="preview-create-a-linux-vm-with-azure-image-builder"></a>Önizleme: Azure Görüntü Oluşturucu ile Linux VM oluşturma

Bu makalede, Azure görüntü oluşturucusu ve Azure CLI kullanarak özelleştirilmiş bir Linux görüntüsü nasıl oluşturabileceğiniz gösterilmektedir. Bu makaledeki örnekte üç farklı kullanan [özelleştiricilerin](image-builder-json.md#properties-customize) görüntüsünü özelleştirmek için:

- Kabuk (ScriptUri) - indirmeleri ve çalışan bir [Kabuk betiği](https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/customizeScript.sh).
- Shell (satır içi) - belirli komutları çalıştırır. Bu örnekte, bir dizin oluşturma ve işletim sistemi güncelleştirme satır içi komutlar içerir.
- Dosya - kopya bir [dosyasını github'dan](https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/exampleArtifacts/buildArtifacts/index.html) VM dizinine.


Biz örnek .json şablon görüntüsünü yapılandırmak için kullanır. Kullandığımız .json dosyası aşağıda verilmiştir: [helloImageTemplateLinux.json](https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/0_Creating_a_Custom_Linux_Managed_Image/helloImageTemplateLinux.json). 

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

## <a name="setup-example-variables"></a>Örnek değişkenleri ayarlama

Bu bilgileri depolamak için bazı değişkenler oluşturacağız. böylece biz bazı bilgilere tekrar tekrar kullanacaklardır.


```azurecli-interactive
# Resource group name - we are using myImageBuilderRG in this example
imageResourceGroup=myImageBuilerRGLinux
# Datacenter location - we are using West US 2 in this example
location=WestUS2
# Name for the image - we are using myBuilderImage in this example
imageName=myBuilderImage
# Run output name
runOutputName=aibLinux
```

Abonelik kimliğiniz için bir değişken oluşturun Bu kullanarak elde edebilirsiniz `az account show | grep id`.

```azurecli-interactive
subscriptionID=<Your subscription ID>
```

## <a name="create-the-resource-group"></a>Kaynak grubunu oluşturun.
Bu, görüntü ve resim yapılandırma şablonu yapıt depolamak için kullanılır.

```azurecli-interactive
az group create -n $imageResourceGroup -l $location
```

## <a name="set-permissions-on-the-resource-group"></a>Kaynak grubu izinleri ayarlama
Kaynak grubunda görüntüyü oluşturmak için Görüntü Oluşturucu 'katkıda bulunan' izin verin. Gerekli izinler olmadan, görüntü yapı başarısız olacaktır. 

`--assignee` Uygulama kayıt kimliği Görüntü Oluşturucu hizmeti için bir değerdir. 

```azurecli-interactive
az role assignment create \
    --assignee cf32a0cc-373c-47c9-9156-0db11f6a6dfc \
    --role Contributor \
    --scope /subscriptions/$subscriptionID/resourceGroups/$imageResourceGroup
```

## <a name="download-the-template-example"></a>Şablon örneği indirin

Bir parametreli bir örnek görüntü yapılandırma Şablonu kullanabilmeniz için oluşturuldu. Örnek .json dosyasını indirin ve daha önce belirlediğiniz değişkenler ile yapılandırın.

```azurecli-interactive
curl https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/0_Creating_a_Custom_Linux_Managed_Image/helloImageTemplateLinux.json -o helloImageTemplateLinux.json

sed -i -e "s/<subscriptionID>/$subscriptionID/g" helloImageTemplateLinux.json
sed -i -e "s/<rgName>/$imageResourceGroup/g" helloImageTemplateLinux.json
sed -i -e "s/<region>/$location/g" helloImageTemplateLinux.json
sed -i -e "s/<imageName>/$imageName/g" helloImageTemplateLinux.json
sed -i -e "s/<runOutputName>/$runOutputName/g" helloImageTemplateLinux.json
```

Bu örnek .json gerektiği gibi değiştirebilirsiniz. Örneğin, değerini artırabilir `buildTimeoutInMinutes` uzun çalışan derlemeler için izin vermek için. Cloud Shell kullanarak dosyayı düzenleyebilirsiniz `vi`.

```azurecli-interactive
vi helloImageTemplateLinux.json
```

> [!NOTE]
> Her zaman, kaynak görüntü için gereken [bir sürüm belirtin](https://github.com/danielsollondon/azvmimagebuilder/blob/master/troubleshootingaib.md#image-version-failure), kullanamazsınız `latest`.
>
> Burada görüntü dağıtılır kaynak grubu ekleyin veya değiştirirseniz emin olmanız gerekir [izinleri, kaynak grubu için ayarlanmış](#set-permissions-on-the-resource-group).


## <a name="submit-the-image-configuration"></a>Görüntü yapılandırması gönderin
Görüntü yapılandırma VM Görüntü Oluşturucu hizmete gönderme

```azurecli-interactive
az resource create \
    --resource-group $imageResourceGroup \
    --properties @helloImageTemplateLinux.json \
    --is-full-object \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n helloImageTemplateLinux01
```

Başarıyla tamamlarsa, bu bir başarı iletisi dönün ve bir Görüntü Oluşturucu yapılandırma şablonu yapıt içinde $imageResourceGroup oluşturma. 'Gizli türleri Göster' etkinleştirirseniz, portaldaki kaynak grubunu görebilirsiniz.

Ayrıca, arka planda Görüntü Oluşturucu, aboneliğinizdeki hazırlama bir kaynak grubu oluşturur. Görüntü oluşturucusu, görüntü derlemesi için hazırlama kaynak grubunu kullanır. Kaynak grubu adı şu biçimde olacaktır: `IT_<DestinationResourceGroup>_<TemplateName>`.

> [!IMPORTANT]
> Hazırlama kaynak grubu doğrudan silmeyin. Görüntü şablon yapıt silerseniz, otomatik olarak hazırlama kaynak grubunu siler. Daha fazla bilgi için [Temizleme](#clean-up) bu makalenin sonunda bölüm.

Hizmet görüntüsü yapılandırma şablonu gönderim sırasında bir hata bildirirse, bkz. [sorun giderme](https://github.com/danielsollondon/azvmimagebuilder/blob/master/troubleshootingaib.md#template-submission-errors--troubleshooting) adımları. Derleme gönderme yeniden denemeden önce şablonu silmek gerekir. Şablonu silmek için:

```azurecli-interactive
az resource delete \
    --resource-group $imageResourceGroup \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n helloImageTemplateLinux01
```

## <a name="start-the-image-build"></a>Görüntü derlemeyi Başlat

Görüntü derlemeyi Başlat.


```azurecli-interactive
az resource invoke-action \
     --resource-group $imageResourceGroup \
     --resource-type  Microsoft.VirtualMachineImages/imageTemplates \
     -n helloImageTemplateLinux01 \
     --action Run 
```

Beklemek için bu örnek, yapı tamamlanana kadar 10-15 dakika sürebilir.

Hatalarla karşılaşırsanız lütfen bunları inceleyin [sorun giderme](https://github.com/danielsollondon/azvmimagebuilder/blob/master/troubleshootingaib.md#image-build-errors--troubleshooting) adımları.


## <a name="create-the-vm"></a>Sanal makine oluşturma

Oluşturulan görüntüyü kullanarak VM'yi oluşturun.

```azurecli-interactive
az vm create \
  --resource-group $imageResourceGroup \
  --name myVM \
  --admin-username azureuser \
  --image $imageName \
  --location $location \
  --generate-ssh-keys
```

VM oluşturma çıktısından IP adresini alın ve bunu VM'ye SSH kullanabilirsiniz.

```azurecli-interactive
ssh azureuser@<pubIp>
```

Görüntü içeren bir ileti, SSH bağlantı hemen sonra günün özelleştirildiğinden görmeniz gerekir!

```console

*******************************************************
**            This VM was built from the:            **
**      !! AZURE VM IMAGE BUILDER Custom Image !!    **
**         You have just been Customized :-)         **
*******************************************************
```

Tür `exit` işiniz bittiğinde SSH bağlantısını kapatın.

## <a name="check-the-source"></a>Kaynak denetimi

Görüntü Oluşturucusu şablonunda 'Özellikler', kaynak görüntü görürsünüz, özelleştirme komut dosyası çalıştığında, onu ve burada dağıtılmış.

```azurecli-interactive
cat helloImageTemplateLinux.json
```

Bu bir .json dosyası hakkında daha ayrıntılı bilgi için bkz: [Görüntü Oluşturucu şablon başvurusu](image-builder-json.md)

## <a name="clean-up"></a>Temizleme

İşiniz bittiğinde kaynakları silebilirsiniz.

Görüntü Oluşturucusu şablonu silin.

```azurecli-interactive
az resource delete \
    --resource-group $imageResourceGroup \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n helloImageTemplateLinux01
```

Görüntü kaynak grubunu silin.

```bash
az group delete -n $imageResourceGroup
```


## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan .json dosyası bileşenleri hakkında daha fazla bilgi için bkz. [Görüntü Oluşturucu şablon başvurusu](image-builder-json.md).
