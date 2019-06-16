---
title: Azure IOT Hub cihazı sağlama hizmeti, çoklu müşteri mimarisi için cihaz sağlama | Microsoft Docs
description: Nasıl yapılır çoklu müşteri mimarisi için cihaz sağlama hizmeti örneği Cihazınızda sağlama
author: wesmc7777
ms.author: wesmc
ms.date: 04/10/2019
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: philmea
ms.openlocfilehash: 84e1f57175d772ad281c18b67fa1be484c0cac69
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66116066"
---
# <a name="how-to-provision-for-multitenancy"></a>Çoklu müşteri mimarisi için sağlama 

Sağlama hizmeti tarafından tanımlanan ayırma ilkeleri, çeşitli ayırma senaryolarını destekler. İki yaygın senaryolar şunlardır:

* **Coğrafi konum / GeoLatency**: Bir cihaz konumlar arasında hareket ettikçe, sağlanan her bir konuma yakın bir IOT hub için cihaz sağlayarak ağ gecikme süresi geliştirildi. Bu senaryoda, bir bölgede yayılma, IOT hub, grup kayıtları için seçilir. **En düşük gecikme** ayırma ilkesi için bu kayıtları seçili. Bu ilke, cihaz gecikme değerlendirmek ve IOT hub IOT hub'ları grubunun dışına kablo belirlemek cihaz sağlama hizmeti neden olur. 

* **Çok kiracılılık**: Bir IOT çözümü içinde kullanılan cihazlar, belirli IOT hub'ı ya da IOT hub'ları grubu atanması gerekebilir. Çözüm, belirli bir IOT hub'ları grubuyla iletişim kurmak belirli bir kiracının tüm cihazlar gerektirebilir. Bazı durumlarda, bir kiracı kendi IOT hub'ları ve kendi IOT hub'ları için atanan cihazların.

Bu iki senaryo birleştirmek için yaygındır. Örneğin, çok kiracılı bir IOT çözümü bölgeler arasında bir grup dağılmış IOT hub kullanarak Kiracı cihazları yaygın olarak atar. Bu Kiracı cihazları coğrafi konum temelinde en düşük gecikme süresine sahip, Grup IOT hub'ına atanabilir.

Bu makalede, bir sanal cihaz örnekten kullanılmıştır [Azure IOT C SDK'sı](https://github.com/Azure/azure-iot-sdk-c) bölgeler arasında çok kiracılı bir senaryoda cihaz sağlama işlemini göstermek için. Bu makalede aşağıdaki adımları gerçekleştireceksiniz:

* İki bölgesel IOT hubs oluşturmak için Azure CLI'yi kullanın (**Batı ABD** ve **Doğu ABD**)
* Çok kiracılı bir kayıt oluşturun
* İki bölgesel aynı bölge içinde cihazları olarak davranmak üzere sanal makineleri oluşturmak için Azure CLI'yi kullanma (**Batı ABD** ve **Doğu ABD**)
* Hem Linux vm'lerinde Azure IOT C SDK'sı için geliştirme ortamını ayarlama
* Bunlar aynı kiracının en yakın bölgede hazırlanan görmek için cihazların benzetimini yapma.


[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="prerequisites"></a>Önkoşullar

* Tamamlanmasından [IOT Hub cihazı sağlama hizmetini Azure portalıyla ayarlama](./quick-setup-auto-provision.md) hızlı başlangıç.


[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]


## <a name="create-two-regional-iot-hubs"></a>İki bölgesel IOT hub oluşturma

Bu bölümde, Azure Cloud Shell'i iki yeni bölgesel IOT hub'ı oluşturmak için kullanacağı **Batı ABD** ve **Doğu ABD** bölgeler için bir kiracı.


1. Bir kaynak grubu oluşturmak için Azure Cloud Shell'i kullanmak [az grubu oluşturma](/cli/azure/group#az-group-create) komutu. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. 

    Aşağıdaki örnekte adlı bir kaynak grubu oluşturur *contoso-us-resource-group* içinde *eastus* bölge. Bu makalede oluşturulan tüm kaynakları için bu grubu kullanmanızı öneririz. Tamamladıktan sonra bu kolay temizleme.

    ```azurecli-interactive 
    az group create --name contoso-us-resource-group --location eastus
    ```

2. IOT hub'ı oluşturmak için Azure Cloud Shell'i kullanmak **eastus** bölgeyle [az IOT hub oluşturma](/cli/azure/iot/hub#az-iot-hub-create) komutu. IOT hub'ı eklenecek *contoso-us-resource-group*.

    Aşağıdaki örnekte adlı bir IOT hub oluşturulur *contoso Doğu hub* içinde *eastus* konumu. Kendi benzersiz hub adı yerine kullanmalısınız **contoso Doğu hub**.

    ```azurecli-interactive 
    az iot hub create --name contoso-east-hub --resource-group contoso-us-resource-group --location eastus --sku S1
    ```
    
    Bu komutun tamamlanması birkaç dakika sürebilir.

3. IOT hub'ı oluşturmak için Azure Cloud Shell'i kullanmak **westus** bölgeyle [az IOT hub oluşturma](/cli/azure/iot/hub#az-iot-hub-create) komutu. Bu IOT hub'ı da eklenir *contoso-us-resource-group*.

    Aşağıdaki örnekte adlı bir IOT hub oluşturulur *contoso Batı hub* içinde *westus* konumu. Kendi benzersiz hub adı yerine kullanmalısınız **contoso Batı hub**.

    ```azurecli-interactive 
    az iot hub create --name contoso-west-hub --resource-group contoso-us-resource-group --location westus --sku S1
    ```

    Bu komutun tamamlanması birkaç dakika sürebilir.



## <a name="create-the-multitenant-enrollment"></a>Çok kiracılı kayıt oluşturma

Bu bölümde, Kiracı cihazlar için yeni bir kayıt grubu oluşturur.  

Kolaylık olması için bu makalede kullanan [simetrik anahtar kanıtı](concepts-symmetric-key-attestation.md) kaydı. Daha güvenli bir çözüm için kullanmayı [X.509 sertifikası kanıtlama](concepts-security.md#x509-certificates) ile bir güven zinciri.

1. Oturum [Azure portalında](https://portal.azure.com), cihaz sağlama hizmeti örneğinizi açın.

2. Seçin **kayıtları Yönet** sekmesine ve ardından **kayıt grubu Ekle** sayfanın üstünde düğme. 

3. Üzerinde **kayıt grubu Ekle**, aşağıdaki bilgileri girin ve tıklayın **Kaydet** düğmesi.

    **Grup adı**: Girin **contoso-us-cihazları**.

    **Kanıtlama türü**: Seçin **simetrik anahtar**.

    **Anahtarları otomatik olarak oluştur**: Bu onay kutusu zaten işaretlenmelidir.

    **Hub'lara cihazları atamak istediğiniz seçin**: Seçin **en düşük gecikme**.

    ![Simetrik anahtar kanıtı için çok kiracılı bir kayıt grubu Ekle](./media/how-to-provision-multitenant/create-multitenant-enrollment.png)


4. Üzerinde **kayıt grubu Ekle**, tıklayın **yeni bir IOT hub bağlantı** her iki bölgesel, hub'ları bağlamak için.

    **Abonelik**: Birden fazla aboneliğiniz varsa, bölgesel IOT hub'ları oluşturduğunuz aboneliği seçin.

    **IOT hub'ı**: Oluşturduğunuz bölgesel hub'lar birini seçin.

    **Erişim İlkesi**: Seçin **iothubowner**.

    ![Bölgesel IOT hub'ları sağlama hizmeti ile bağlama](./media/how-to-provision-multitenant/link-regional-hubs.png)


5. Her iki bölgesel IOT hub'ları bağlı sonra bunları kayıt grubunu seçin ve tıklayın gerekir **Kaydet** kayıt için bölgesel IOT hub grubu oluşturmak için.

    ![Kayıt için bölgesel hub grubu oluşturma](./media/how-to-provision-multitenant/enrollment-regional-hub-group.png)


6. Kaydı kaydetme sonra yeniden açın ve Not **birincil anahtar**. Oluşturulan anahtarları için ilk kayıt kaydetmeniz gerekir. Bu anahtarı daha sonra iki sanal cihazlar için benzersiz bir cihaz anahtarları oluşturmak için kullanılır.


## <a name="create-regional-linux-vms"></a>Bölgesel bir Linux VM oluşturma

Bu bölümde, iki bölgesel Linux sanal makineleri (VM'ler) oluşturur. Bu VM'lerin her bölgedeki iki bölgeleri Kiracı cihazlar için cihaz sağlama göstermek için bir cihaz benzetim örneği çalıştırın.

Kolay bir şekilde, bu Vm'lere temizleme yapmak için oluşturulmuş olan IOT hub'ları içeren aynı kaynak grubuna eklenecek *contoso-us-resource-group*. Bununla birlikte, Vm'leri ayrı bölge içinde çalışır (**Batı ABD** ve **Doğu ABD**).

1. Azure Cloud Shell'de oluşturmak için aşağıdaki komutu yürütün bir **Doğu ABD** komutta aşağıdaki parametre değişiklikleri yaptıktan sonra VM bölgesi:

    **--ad**: İçin benzersiz bir ad girin, **Doğu ABD** bölgesel cihaz VM. 

    **--Yönetici kullanıcı adı**: Kendi yönetici kullanıcı adı kullanın.

    **--Yönetici parolası**: Kendi yönetici parolasını kullanın.

    ```azurecli-interactive
    az vm create \
    --resource-group contoso-us-resource-group \
    --name ContosoSimDeviceEast \
    --location eastus \
    --image Canonical:UbuntuServer:18.04-LTS:18.04.201809110 \
    --admin-username contosoadmin \
    --admin-password myContosoPassword2018 \
    --authentication-type password
    ```

    Bu komutun tamamlanması birkaç dakika sürer. Komut tamamlandıktan sonra Not **Publicıpaddress** , Doğu ABD bölgesinde VM için değer.

1. Azure Cloud Shell'de oluşturmak için komutu Yürüt bir **Batı ABD** komutta aşağıdaki parametre değişiklikleri yaptıktan sonra VM bölgesi:

    **--ad**: İçin benzersiz bir ad girin, **Batı ABD** bölgesel cihaz VM. 

    **--Yönetici kullanıcı adı**: Kendi yönetici kullanıcı adı kullanın.

    **--Yönetici parolası**: Kendi yönetici parolasını kullanın.

    ```azurecli-interactive
    az vm create \
    --resource-group contoso-us-resource-group \
    --name ContosoSimDeviceWest \
    --location westus \
    --image Canonical:UbuntuServer:18.04-LTS:18.04.201809110 \
    --admin-username contosoadmin \
    --admin-password myContosoPassword2018 \
    --authentication-type password
    ```

    Bu komutun tamamlanması birkaç dakika sürer. Komut tamamlandıktan sonra Not **Publicıpaddress** VM, Batı ABD bölgesi için değer.

1. İki komut satırı Kabukları açın. Bir bölgesel sanal makinelerin her shell'de SSH kullanarak bağlanın. 

    Yönetici kullanıcı adı ve SSH için parametre olarak VM için belirtilen genel IP adresini geçirin. İstendiğinde yönetici parolasını girin.

    ```bash
    ssh contosoadmin@1.2.3.4

    contosoadmin@ContosoSimDeviceEast:~$
    ```

    ```bash
    ssh contosoadmin@5.6.7.8

    contosoadmin@ContosoSimDeviceWest:~$
    ```



## <a name="prepare-the-azure-iot-c-sdk-development-environment"></a>Azure IOT C SDK'sı geliştirme ortamınızı hazırlama

Bu bölümde, her VM üzerindeki Azure IOT C SDK'sı kopyalama. SDK, bir kiracının cihaz her bölgede sağlama benzetimini yapacak bir örnek içerir.


1. Her VM için yükleme **Cmake**, **g ++** , **gcc**, ve [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) için aşağıdaki komutları kullanın:

    ```bash
    sudo apt-get update
    sudo apt-get install cmake build-essential libssl-dev libcurl4-openssl-dev uuid-dev git-all
    ```


1. Kopya [Azure IOT C SDK'sı](https://github.com/Azure/azure-iot-sdk-c) iki VM'nin de.

    ```bash
    cd ~/
    git clone https://github.com/Azure/azure-iot-sdk-c.git --recursive
    ```

    Bu işlemin tamamlanması için birkaç dakika beklemeniz gerekebilir.

1. Her iki VM için yeni bir oluşturma **cmake** klasörün içinde depo ve bu klasöre Değiştir.

    ```bash
    mkdir ~/azure-iot-sdk-c/cmake
    cd ~/azure-iot-sdk-c/cmake
    ```

1. Her iki VM için geliştirme istemci platformunuza belirli SDK'nin bir sürümüne yapılar aşağıdaki komutu çalıştırın. 

    ```bash
    cmake -Dhsm_type_symm_key:BOOL=ON -Duse_prov_client:BOOL=ON  ..
    ```

    Derleme başarılı olduktan sonra, son birkaç çıkış satırı aşağıdaki çıkışa benzer olacaktır:

    ```bash
    -- IoT Client SDK Version = 1.2.9
    -- Provisioning client ON
    -- Provisioning SDK Version = 1.2.9
    -- target architecture: x86_64
    -- Checking for module 'libcurl'
    --   Found libcurl, version 7.58.0
    -- Found CURL: curl
    -- target architecture: x86_64
    -- target architecture: x86_64
    -- target architecture: x86_64
    -- target architecture: x86_64
    -- iothub architecture: x86_64
    -- target architecture: x86_64
    -- Configuring done
    -- Generating done
    -- Build files have been written to: /home/contosoadmin/azure-iot-sdk-c/cmake
    ```    


## <a name="derive-unique-device-keys"></a>Benzersiz cihaz anahtarları türetin

Simetrik anahtar kanıtı ile grup kayıtları kullanarak, doğrudan kayıt grubu tuşlarını kullanmayın. Bunun yerine benzersiz bir türetilen her cihaz için anahtar ve sizden oluşturduğunuz [simetrik anahtarlar ile grup kayıtları](concepts-symmetric-key-attestation.md#group-enrollments).

Cihaz anahtarı oluşturmak için işlem grubu ana anahtarı kullanmak bir [HMAC SHA256](https://wikipedia.org/wiki/HMAC) sonucu Base64 biçimine dönüştür ve cihaz için benzersiz kayıt kimliği.

Grup ana anahtarınızı cihaz kodunuzda içermez.

Bash Kabuğu örneği kullanarak her cihaz için bir türetilen cihaz anahtarı oluşturmak için kullanın **openssl**.

- Değeri Değiştir **anahtarı** ile **birincil anahtar** kaydınız için daha önce not ettiğiniz.

- Değeri Değiştir **REG_ID** kendi her cihaz için benzersiz kayıt Kimliğine sahip. Küçük alfasayısal ve kısa çizgi ('-') her iki kimlikleri tanımlamak için karakter.

Örnek cihaz anahtarı oluşturma *contoso simdevice Doğu*:

```bash
KEY=rLuyBPpIJ+hOre2SFIP9Ajvdty3j0EwSP/WvTVH9eZAw5HpDuEmf13nziHy5RRXmuTy84FCLpOnhhBPASSbHYg==
REG_ID=contoso-simdevice-east

keybytes=$(echo $KEY | base64 --decode | xxd -p -u -c 1000)
echo -n $REG_ID | openssl sha256 -mac HMAC -macopt hexkey:$keybytes -binary | base64
```

```bash
p3w2DQr9WqEGBLUSlFi1jPQ7UWQL4siAGy75HFTFbf8=
```

Örnek cihaz anahtarı oluşturma *contoso simdevice Batı*:

```bash
KEY=rLuyBPpIJ+hOre2SFIP9Ajvdty3j0EwSP/WvTVH9eZAw5HpDuEmf13nziHy5RRXmuTy84FCLpOnhhBPASSbHYg==
REG_ID=contoso-simdevice-west

keybytes=$(echo $KEY | base64 --decode | xxd -p -u -c 1000)
echo -n $REG_ID | openssl sha256 -mac HMAC -macopt hexkey:$keybytes -binary | base64
```

```bash
J5n4NY2GiBYy7Mp4lDDa5CbEe6zDU/c62rhjCuFWxnc=
```


Kiracı cihazları kiracıya IOT hub'ları sağlama sırasında kayıt grubu simetrik anahtar kanıtı gerçekleştirmek için her kendi türetilmiş cihaz anahtarı ve benzersiz kayıt Kimliğini kullanır.




## <a name="simulate-the-devices-from-each-region"></a>Her bir bölgeye ait cihazların benzetimini yapma


Bu bölümde, her iki bölgesel Vm'leri için sağlama örnek Azure IOT C SDK'sı olarak güncelleştirir. 

Örnek kod, cihaz sağlama hizmeti örneğinizi sağlama isteği gönderdiği cihaz önyükleme sırası benzetimini yapar. Önyükleme sırasını tanınır ve gecikme süresine göre en yakın IOT hub'ına atanan cihazın neden olur.

1. Azure Portal'da Cihaz Sağlama hizmetiniz için **Genel Bakış** sekmesini seçin ve **_Kimlik Kapsamı_** değerini not alın.

    ![Portal dikey penceresinden Cihaz Sağlama Hizmeti uç noktası bilgilerini ayıklama](./media/quick-create-simulated-device-x509/extract-dps-endpoints.png) 

1. Açık **~/azure-iot-sdk-c/provisioning\_istemci/samples/prov\_geliştirme\_istemci\_örnek/prov\_geliştirme\_istemci\_sample.c**iki VM'de de düzenleme.

    ```bash
    vi ~/azure-iot-sdk-c/provisioning_client/samples/prov_dev_client_sample/prov_dev_client_sample.c
    ```

1. `id_scope` sabitini bulun ve değeri daha önce kopyalamış olduğunuz **Kimlik Kapsamı** değerinizle değiştirin. 

    ```c
    static const char* id_scope = "0ne00002193";
    ```

1. Aynı dosyada `main()` işlevinin tanımını bulun. Emin `hsm_type` değişkeni ayarlanır `SECURE_DEVICE_TYPE_SYMMETRIC_KEY` kayıt grubu kanıtlama yöntemi eşleştirmek için aşağıda gösterildiği gibi. 

    Her iki VM dosyalarda yaptığınız değişiklikleri kaydedin.

    ```c
    SECURE_DEVICE_TYPE hsm_type;
    //hsm_type = SECURE_DEVICE_TYPE_TPM;
    //hsm_type = SECURE_DEVICE_TYPE_X509;
    hsm_type = SECURE_DEVICE_TYPE_SYMMETRIC_KEY;
    ```

1. İki VM'de de çağrısını bulmak `prov_dev_set_symmetric_key_info()` içinde **prov\_geliştirme\_istemci\_sample.c** hangi dışında bırakılır.

    ```c
    // Set the symmetric key if using they auth type
    //prov_dev_set_symmetric_key_info("<symm_registration_id>", "<symmetric_Key>");
    ```

    İşlev çağrıları açıklamasını kaldırın ve benzersiz kayıt kimlikleri ve her cihaz için türetilen cihaz anahtarları (açılı ayraçlar dahil) yer tutucu değerlerini değiştirin. Aşağıda gösterildiği gibi yalnızca anahtarlarıdır. Daha önce oluşturulan tuşlarını kullanın.

    Doğu ABD:
    ```c
    // Set the symmetric key if using they auth type
    prov_dev_set_symmetric_key_info("contoso-simdevice-east", "p3w2DQr9WqEGBLUSlFi1jPQ7UWQL4siAGy75HFTFbf8=");
    ```

    Batı ABD:
    ```c
    // Set the symmetric key if using they auth type
    prov_dev_set_symmetric_key_info("contoso-simdevice-west", "J5n4NY2GiBYy7Mp4lDDa5CbEe6zDU/c62rhjCuFWxnc=");
    ```

    Dosyaları kaydedin.

1. İki VM'nin de, aşağıda gösterilen örnek klasörüne gidin ve örnek oluşturun.

    ```bash
    cd ~/azure-iot-sdk-c/cmake/provisioning_client/samples/prov_dev_client_sample/
    cmake --build . --target prov_dev_client_sample --config Debug
    ```

1. Derleme başarılı olduktan sonra Çalıştır **prov\_geliştirme\_istemci\_sample.exe** iki VM'nin de her bölgeye bir kiracı CİHAZDAN benzetimini yapmak için. Her bir cihaz IOT hub'a sanal cihazın bölgesine en yakın kiracıya ayrılan dikkat edin.

    Benzetim çalıştırın:
    ```bash
    ~/azure-iot-sdk-c/cmake/provisioning_client/samples/prov_dev_client_sample/prov_dev_client_sample
    ```

    Örnek: ABD Doğu VM'den çıkış

    ```bash
    contosoadmin@ContosoSimDeviceEast:~/azure-iot-sdk-c/cmake/provisioning_client/samples/prov_dev_client_sample$ ./prov_dev_client_sample
    Provisioning API Version: 1.2.9

    Registering Device

    Provisioning Status: PROV_DEVICE_REG_STATUS_CONNECTED
    Provisioning Status: PROV_DEVICE_REG_STATUS_ASSIGNING
    Provisioning Status: PROV_DEVICE_REG_STATUS_ASSIGNING

    Registration Information received from service: contoso-east-hub.azure-devices.net, deviceId: contoso-simdevice-east
    Press enter key to exit:

    ```

    Örnek: ABD Batı VM'den çıkış
    ```bash
    contosoadmin@ContosoSimDeviceWest:~/azure-iot-sdk-c/cmake/provisioning_client/samples/prov_dev_client_sample$ ./prov_dev_client_sample
    Provisioning API Version: 1.2.9

    Registering Device

    Provisioning Status: PROV_DEVICE_REG_STATUS_CONNECTED
    Provisioning Status: PROV_DEVICE_REG_STATUS_ASSIGNING
    Provisioning Status: PROV_DEVICE_REG_STATUS_ASSIGNING

    Registration Information received from service: contoso-west-hub.azure-devices.net, deviceId: contoso-simdevice-west
    Press enter key to exit:
    ```



## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu makalede oluşturulan kaynakları ile çalışmaya devam etmeyi planlıyorsanız, bunları bırakabilirsiniz. Kaynağı kullanmaya devam etmeyi planlamıyorsanız, gereksiz ücretlerden kaçınmak için bu makaleyi tarafından oluşturulan tüm kaynakları silmek için aşağıdaki adımları kullanın.

Oluşturduğunuz tüm kaynakları bu makaledeki adlı kaynak grubunda belirtildiği gibi buradaki adımları varsayar **contoso-us-resource-group**.

> [!IMPORTANT]
> Silinen kaynak grupları geri alınamaz. Kaynak grubu ve içindeki tüm kaynaklar kalıcı olarak silinir. Yanlış kaynak grubunu veya kaynakları yanlışlıkla silmediğinizden emin olun. IoT Hub'ı tutmak istediğiniz kaynakların bulunduğu mevcut bir kaynak grubunda oluşturduysanız kaynak grubunu silmek yerine IoT Hub kaynağını silin.
>

Ada göre kaynak grubunu silmek için:

1. [Azure portalında](https://portal.azure.com) oturum açın ve **Kaynak grupları**’na tıklayın.

2. İçinde **ada göre Filtrele...**  metin kutusuna kaynak adını grubunda kaynaklarınızın içeren **contoso-us-resource-group**. 

3. Sonuç listesinde kaynak grubunuzun sağ tarafında **...** ve sonra **Kaynak grubunu sil**'e tıklayın.

4. Kaynak grubunun silinmesini onaylamanız istenir. Onaylamak için kaynak grubunuzun adını tekrar yazın ve **Sil**'e tıklayın. Birkaç dakika sonra kaynak grubu ve içerdiği kaynakların tümü silinir.

## <a name="next-steps"></a>Sonraki adımlar

- Reprovisioning daha fazla bilgi edinmek için [IOT Hub cihaz kavramları çıkış](concepts-device-reprovision.md) 
- Daha fazla sağlama kaldırmayı bilgi edinmek için [nasıl daha önce otomatik olarak sağlanan cihazları sağlamasını kaldırmak](how-to-unprovision-devices.md) 











