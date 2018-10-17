---
title: Öğretici - Azure Batch AI ve Horovod ile dağıtılmış eğitim | Microsoft Docs
description: Öğretici - Azure Batch AI hizmeti ve Azure CLI kullanarak Horovod ile dağıtılmış bir modeli eğitme.
services: batch-ai
author: johnwu10
manager: jeconnoc
ms.service: batch-ai
ms.topic: tutorial
ms.date: 09/03/2018
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: de19b20865127fd37cd7bc1ac854288a95a68091
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44058195"
---
# <a name="tutorial-train-a-distributed-model-with-horovod"></a>Öğretici: Horovod ile dağıtılmış bir modeli eğitme

Bu öğreticide, dağıtılmış bir derin öğrenme modelini bir Batch AI kümesinde birden fazla düğüm üzerinde paralel olarak çalıştırarak eğitirsiniz. Batch AI, Azure GPU’ları üzerinde makine öğrenimi ve yapay zeka modellerini uygun ölçekte eğitmek için yönetilen bir hizmettir. 

Bu öğretici ortak bir Batch AI iş akışı sunar ve Azure CLI üzerinden Batch AI kaynaklarıyla nasıl etkileşim kuracağınızı gösterir. Ele alınan konular:

> [!div class="checklist"]
> * Batch AI çalışma alanı, denemesi ve kümesi ayarlama
> * Giriş ve çıkış için bir Azure dosya paylaşımı ayarlama
> * Horovod kullanarak ayrıntılı bir öğrenme modelini paralelleştirme
> * Eğitim işi gönderme
> * İş izleme
> * Eğitim sonuçlarını alma

Bu öğretici için, bir nesne algılama modeli [Horovod](https://github.com/uber/horovod) ile paralel olarak çalıştırılmak için değiştirilir. Model [CIFAR-10 veri kümesi](https://www.cs.toronto.edu/~kriz/cifar.html) görüntüleri üzerinde eğitilir. Eğitim işi 24 vCPU ve 4 GPU içeren bir kümede çalıştırılır ve tamamlanması yaklaşık 60 dakika sürer.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici için Azure CLI 2.0.38 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme](/cli/azure/install-azure-cli). 

## <a name="why-use-horovod"></a>Neden Horovod kullanmalısınız?

Horovod Tensorflow, Keras ve PyTorch için dağıtılmış bir eğitim çerçevesidir ve bu öğretici için kullanılır. Horovod ile, tek bir GPU üzerinde çalışmak için tasarlanmış bir eğitim betiğini yalnızca birkaç satır kod kullanarak dağıtılmış bir sistem üzerinde verimli bir şekilde çalışan bir betiğe dönüştürebilirsiniz.

Horovod’a ek olarak, Batch AI diğer birçok popüler açık kaynak çerçeve ile dağıtılmış eğitimi destekler. Üretimde modelleri eğitmek için kullandığınız çerçevelerin lisans koşullarını gözden geçirdiğinizden emin olun.

## <a name="prepare-the-batch-ai-environment"></a>Batch AI ortamını hazırlama

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

`eastus` bölgesinde `batchai.horovod` adlı bir kaynak grubu oluşturmak için `az group create` komutunu kullanın. Batch AI kaynaklarını dağıtmak için kaynak grubunu kullanırsınız.

```azurecli-interactive
az group create --name batchai.horovod --location eastus
```

### <a name="create-a-workspace"></a>Çalışma alanı oluşturma

`az batchai workspace create` komutunu kullanarak bir Batch AI çalışma alanı oluşturun. Çalışma alanı, diğer Batch AI kaynaklarının üst düzey koleksiyonudur. Aşağıdaki komut, kaynak grubunuz altında `batchaidev` adlı bir çalışma alanı oluşturur.

```azurecli-interactive
az batchai workspace create --resource-group batchai.horovod --workspace batchaidev 
```

### <a name="create-an-experiment"></a>Deneme oluşturma

Batch AI deneme grupları birlikte sorguladığınız ve yönettiğiniz bir veya daha fazla işi gruplandırır. Aşağıdaki `az batchai experiment create` komutu, çalışma alanı ve kaynak grubu altında `cifar` adlı bir deneme oluşturur.

```azurecli-interactive
az batchai experiment create --resource-group batchai.horovod --workspace batchaidev --name cifar 
```

## <a name="set-up-a-gpu-cluster"></a>GPU kümesi ayarlama

Ardından, denemeyi çalıştırmak için bir GPU kümesi ayarlayın. Batch AI kümeleri belirli ihtiyaçlar için özelleştirmeye yönelik bir dizi esnek seçenek sunar.

Aşağıdaki `az batchai cluster create` komutu, çalışma alanınız ve kaynak grubunuz altında `nc6cluster` adlı bir 4 düğümlü küme oluşturur. Varsayılan olarak, kümedeki VM’ler kapsayıcı tabanlı uygulamaları barındırmak için tasarlanmış bir Ubuntu Server görüntüsü çalıştırır. Bu örnekteki küme düğümleri bir NVIDIA Tesla K80 GPU içeren `Standard_NC6` boyutunu kullanır.

```azurecli-interactive
az batchai cluster create --resource-group batchai.horovod --workspace batchaidev --name nc6cluster --vm-priority dedicated  --vm-size Standard_NC6 --target 4 --generate-ssh-keys
```

Küme durumunu görmek için `az batchai cluster show` komutunu çalıştırın. Kümenin tam olarak sağlanması genellikle birkaç dakika sürer.

```azurecli-interactive
az batchai cluster show --name nc6cluster --workspace batchaidev --resource-group batchai.horovod --output table
```

Küme oluşturmanın erken aşamalarında, küme `resizing` durumundadır. Küme durumu değiştirilirken aşağıdaki adımlarla devam edin. Durum `steady` olduğunda ve düğümler `idle` olduğunda küme eğitim işini çalıştırmak için hazırdır. Örnek:

```
Name        Resource Group    Workspace    VM Size       State      Idle    Running    Preparing    Leaving    Unusable
----------  ----------------  -----------  ------------  -------  ------  ---------  -----------  ---------  ----------
nc6cluster  batchai.horovod  batchaidev   STANDARD_NC6  steady        4          0            0          0           0
```

## <a name="set-up-storage"></a>Depolamayı ayarlama

Eğitim betiğiniz ve eğitim çıkışınızı depolamak üzere bir depolama hesabı oluşturmak için `az storage account create` komutunu kullanın.

```azurecli-interactive
az storage account create --resource-group batchai.horovod --name mystorageaccount --location eastus --sku Standard_LRS
```

`az storage share create` komutunu kullanarak hesapta `myshare` adlı bir Azure dosya paylaşımı oluşturun:

```azurecli-interactive
az storage share create --name myshare --account-name mystorageaccount
```

Uygulamada, bu aynı depolama alanı birden çok iş ve denemeler arasında kullanılabilir. Öğeleri düzenli bir şekilde tutmak için, dosya paylaşımı içinde bu denemeye özgü dosyaların depolanacağı bir dizin oluşturun. Aşağıdaki `az storage directory create` komutu `cifar` adlı bir dizin oluşturur.

```azurecli-interactive
az storage directory create --name cifar --share-name myshare --account-name mystorageaccount
```

Sonraki adım gerçek eğitim betiğini hazırlamaktır, bunu daha sonra yeni oluşturulan dizine yüklersiniz.

## <a name="create-the-training-script"></a>Eğitim betiğini oluşturma

Bu deneme için, Horovod kullanarak bir nesne algılama modelini paralel olarak çalıştırmak üzere birkaç değişiklik ile güncelleştirilmiş bir Python betiği çalıştırırsınız. [Özgün model](https://raw.githubusercontent.com/keras-team/keras/master/examples/cifar10_cnn.py) bir TensorFlow arka ucu ile Keras kullanır. 

Kabuğunuzdaki bir çalışma dizininde, sık kullandığınız bir metin düzenleyiciyi kullanarak aşağıdaki içerik ile `cifar_cnn_distributed.py` adlı bir dosya oluşturun. Özgün kaynak koduna yapılan değişikliklere `HOROVOD` ön eki ile bir açıklama eklenir.

```python
from __future__ import print_function
import keras
from keras.datasets import cifar10
from keras.preprocessing.image import ImageDataGenerator
from keras.models import Sequential
from keras.layers import Dense, Dropout, Activation, Flatten
from keras.layers import Conv2D, MaxPooling2D
import tensorflow as tf
import horovod.keras as hvd
import os
from keras import backend as K
import math
import argparse 

# HOROVOD: initialize Horovod.
hvd.init()

# HOROVOD: pin GPU to be used to process local rank (one GPU per process)
config = tf.ConfigProto()
config.gpu_options.allow_growth = True
config.gpu_options.visible_device_list = str(hvd.local_rank())
K.set_session(tf.Session(config=config))

batch_size = 32
num_classes = 10
# HOROVOD: adjust number of epochs based on number of GPUs.
epochs = int(math.ceil(100.0 / hvd.size()))

data_augmentation = True
num_predictions = 20
# BATCH AI: change save directory to mounted storage path
parser = argparse.ArgumentParser()
parser.add_argument("-d", "--dir", help="directory to save model to")
args = parser.parse_args()
save_dir = os.path.join(args.dir, 'saved_models')
model_name = 'keras_cifar10_trained_model.h5'

# The data, split between train and test sets:
(x_train, y_train), (x_test, y_test) = cifar10.load_data()
print('x_train shape:', x_train.shape)
print(x_train.shape[0], 'train samples')
print(x_test.shape[0], 'test samples')

# Convert class vectors to binary class matrices.
y_train = keras.utils.to_categorical(y_train, num_classes)
y_test = keras.utils.to_categorical(y_test, num_classes)

model = Sequential()
model.add(Conv2D(32, (3, 3), padding='same',
                 input_shape=x_train.shape[1:]))
model.add(Activation('relu'))
model.add(Conv2D(32, (3, 3)))
model.add(Activation('relu'))
model.add(MaxPooling2D(pool_size=(2, 2)))
model.add(Dropout(0.25))

model.add(Conv2D(64, (3, 3), padding='same'))
model.add(Activation('relu'))
model.add(Conv2D(64, (3, 3)))
model.add(Activation('relu'))
model.add(MaxPooling2D(pool_size=(2, 2)))
model.add(Dropout(0.25))

model.add(Flatten())
model.add(Dense(512))
model.add(Activation('relu'))
model.add(Dropout(0.5))
model.add(Dense(num_classes))
model.add(Activation('softmax'))

# HOROVOD: adjust learning rate based on number of GPUs.
opt = keras.optimizers.rmsprop(lr=0.0001 * hvd.size(), decay=1e-6)

# HOROVOD: add Horovod Distributed Optimizer.
opt = hvd.DistributedOptimizer(opt)

# Let's train the model using RMSprop
model.compile(loss='categorical_crossentropy',
              optimizer=opt,
              metrics=['accuracy'])

callbacks = [
    # HOROVOD: broadcast initial variable states from rank 0 to all other processes.
    # This is necessary to ensure consistent initialization of all workers when
    # training is started with random weights or restored from a checkpoint.
    hvd.callbacks.BroadcastGlobalVariablesCallback(0),
]

# HOROVOD: save checkpoints only on worker 0 to prevent other workers from corrupting them.
if hvd.rank() == 0:
    callbacks.append(keras.callbacks.ModelCheckpoint('./checkpoint-{epoch}.h5'))

x_train = x_train.astype('float32')
x_test = x_test.astype('float32')
x_train /= 255
x_test /= 255

if not data_augmentation:
    print('Not using data augmentation.')
    model.fit(x_train, y_train,
              batch_size=batch_size,
              epochs=epochs,
              validation_data=(x_test, y_test),
              shuffle=True)
else:
    print('Using real-time data augmentation.')
    # This will do preprocessing and realtime data augmentation:
    datagen = ImageDataGenerator(
        featurewise_center=False,  # set input mean to 0 over the dataset
        samplewise_center=False,  # set each sample mean to 0
        featurewise_std_normalization=False,  # divide inputs by std of the dataset
        samplewise_std_normalization=False,  # divide each input by its std
        zca_whitening=False,  # apply ZCA whitening
        zca_epsilon=1e-06,  # epsilon for ZCA whitening
        rotation_range=0,  # randomly rotate images in the range (degrees, 0 to 180)
        width_shift_range=0.1,  # randomly shift images horizontally (fraction of total width)
        height_shift_range=0.1,  # randomly shift images vertically (fraction of total height)
        shear_range=0.,  # set range for random shear
        zoom_range=0.,  # set range for random zoom
        channel_shift_range=0.,  # set range for random channel shifts
        fill_mode='nearest',  # set mode for filling points outside the input boundaries
        cval=0.,  # value used for fill_mode = "constant"
        horizontal_flip=True,  # randomly flip images
        vertical_flip=False,  # randomly flip images
        rescale=None,  # set rescaling factor (applied before any other transformation)
        preprocessing_function=None,  # set function that will be applied on each input
        data_format=None,  # image data format, either "channels_first" or "channels_last"
        validation_split=0.0)  # fraction of images reserved for validation (strictly between 0 and 1)

    # Compute quantities required for feature-wise normalization
    # (std, mean, and principal components if ZCA whitening is applied).
    datagen.fit(x_train)

    # Fit the model on the batches generated by datagen.flow().
    model.fit_generator(datagen.flow(x_train, y_train,
                                     batch_size=batch_size),
                        epochs=epochs,
                        validation_data=(x_test, y_test),
                        workers=4)

# Save model and weights
if not os.path.isdir(save_dir):
    os.makedirs(save_dir)
model_path = os.path.join(save_dir, model_name)
model.save(model_path)
print('Saved trained model at %s ' % model_path)

# Score trained model.
scores = model.evaluate(x_test, y_test, verbose=1)
print('Test loss:', scores[0])
print('Test accuracy:', scores[1])
```

Bu örnekte gösterildiği gibi, Horovod çerçevesini kullanarak dağıtılmış eğitimi etkinleştirmek için modelde yalnızca birkaç güncelleştirme yapılması gerekir. 

Bu betiğin tanıtım amacıyla görece küçük bir model ve veri kümesi kullandığını ve dağıtılmış modelin önemli bir performans iyileştirmesi sağlamayabileceğini unutmayın. Dağıtılmış eğitimin gerçek gücünü görmek için, daha büyük bir model ve veri kümesi kullanın.

## <a name="upload-the-training-script"></a>Eğitim betiğini karşıya yükleme

Betik hazır olduğunda, sonraki adım bunu daha önceden oluşturduğunuz dosya paylaşımı dizinine yüklemektir. Aşağıdaki `az storage file upload` komutu bunu yerel çalışma dizininden uygun konuma yükler.

```azurecli-interactive
az storage file upload --path cifar --share-name myshare --source cifar_cnn_distributed.py --account-name mystorageaccount
```

## <a name="submit-the-training-job"></a>Eğitim işini gönderme

Önceki adımları tamamladıktan sonra bir eğitim işi oluşturun. Batch AI içinde, işin nasıl çalıştırılacağını belirten parametreleri tanımlamak için bir `job.json` dosyası kullanırsınız. Sık kullandığınız bir metin düzenleyiciyi kullanarak, aşağıdaki içerikler ile `job.json` adlı bir iş yapılandırma dosyası oluşturun.

```json
{
    "$schema": "https://raw.githubusercontent.com/Azure/BatchAI/master/schemas/2018-05-01/job.json",
    "properties": {
        "nodeCount": 4,
        "horovodSettings": {
            "pythonScriptFilePath": "$AZ_BATCHAI_JOB_MOUNT_ROOT/myshare/cifar/cifar_cnn_distributed.py",
            "commandLineArgs": "--dir=$AZ_BATCHAI_JOB_MOUNT_ROOT/myshare/cifar"
        },
        "stdOutErrPathPrefix": "$AZ_BATCHAI_JOB_MOUNT_ROOT/myshare/cifar",
        "mountVolumes": {
            "azureFileShares": [
                {
                    "azureFileUrl": "https://<AZURE_BATCHAI_STORAGE_ACCOUNT>.file.core.windows.net/myshare",
                    "relativeMountPath": "myshare"
                }
            ]
        },
        "jobPreparation": {
            "commandLine": "apt update; apt install mpi-default-dev mpi-default-bin -y; pip install keras horovod"
        },
        "containerSettings": {
            "imageSourceRegistry": {
                "image": "tensorflow/tensorflow:1.8.0-gpu"
            }
        }
    }
}
```

Özelliklerin açıklaması:

| Özellik | Açıklama |
| --------- | --------- |
| `nodeCount` | İş için ayrılacak düğüm sayısı. Burada, iş `4` düğüm üzerinde paralel olarak çalıştırılır. |
| `horovodSettings` | `pythonScriptFilePath` alanı, önceden oluşturulan `cifar` dizininde bulunan Horovod betiğinin yolunu tanımlar. `commandLineArgs` alanı betiği çalıştırmak için komut satırı bağımsız değişkenleridir. Bu deneme için, modelin kaydedileceği dizin gerekli tek bağımsız değişkendir. `$AZ_BATCHAI_JOB_MOUNT_ROOT/myshare` dosya paylaşımının eklendiği konumun yoludur. | 
| `stdOutErrPathPrefix` | İş çıkışları ve günlüklerini depolamak için kullanılan yol (bu örnekte aynı `cifar` dizini kullanılır). |
| `jobPreparation` | İşi çalıştırmak için ortamı hazırlamaya yönelik özel talimatlar. Bu betik, belirtilen MPI ve Horovod paketlerinin yüklenmesini gerektirir. |
| `containerSettings` | İşin üzerinde çalıştığı kapsayıcının ayarları. Bu iş `tensorflow` ile oluşturulmuş bir Docker kapsayıcısı kullanır.

Yapılandırmayı kullanarak, `az batchai job create` komutu ile işi oluşturun. Aşağıdaki komut, şu ana kadar ayarlanan tüm kaynakları kullanarak `cifar_distributed` adlı yeni bir işi kuyruğa alır. 

```azurecli-interactive
az batchai job create --cluster nc6cluster --name cifar_distributed --resource-group batchai.horovod --workspace batchaidev --experiment cifar --config-file job.json --storage-account-name mystorageaccount
```

Düğümler şu anda meşgul ise, işin çalıştırılmaya başlanması biraz sürebilir. İşin yürütülme durumunu görüntülemek için `az batchai job show` komutunu kullanın.

```azurecli-interactive
az batchai job show --experiment cifar --name cifar_distributed --resource-group batchai.horovod --workspace batchaidev --query "executionState"
```

### <a name="visualize-the-distributed-training"></a>Dağıtılmış eğitimi görselleştirme

İş çalışmaya başladığında, küme düğümlerinin durumunu sorgulamak için tekrar `az batchai cluster show` komutunu kullanın. 

```azurecli-interactive
az batchai cluster show --name nc6cluster --workspace batchaidev --resource-group batchai.horovod --query "nodeStateCounts"
```

Çıkış, dördünü de çalışır durumda göstermeli aşağıdakine benzer olmalıdır. Bu sonuç dört düğümün tümünün dağıtılmış eğitimde kullanılmakta olduğunu gösterir.

```
{
  "idleNodeCount": 0,
  "leavingNodeCount": 0,
  "preparingNodeCount": 0,
  "runningNodeCount": 4,
  "unusableNodeCount": 0
}
```

## <a name="monitor-the-job"></a>İş izleme

### <a name="list-output-files"></a>Çıkış dosyalarını listeleme

İş çalıştırılırken, işin ürettiği çıkış dosyalarını listelemek için `az batchai job file list` komutunu kullanın.

```azurecli-interactive
az batchai job file list --experiment cifar --job cifar_distributed --resource-group batchai.horovod --workspace batchaidev --output table
```

Bu deneme için, çıkışın aşağıdakine benzer olması gerekir. İş için genel çıkış `stdout.txt` içinde günlüğe kaydedilirken, `stderr.txt` ana yürütme içinde gerçekleşen hataların çıkışını görüntüler. Diğer dosyalar her bir düğüme karşılık gelen çıkış, hata ve iş hazırlama günlükleridir.

```
Name                                                    Type       Size  Modified
------------------------------------------------------  ------  -------  -------------------------
execution-tvm-676767296_1-20180718t174802z-p.log        file       8801  2018-07-18T22:41:28+00:00
execution-tvm-676767296_2-20180718t174802z-p.log        file      15094  2018-07-18T22:41:55+00:00
execution-tvm-676767296_3-20180718t174802z-p.log        file       8801  2018-07-18T22:41:28+00:00
execution-tvm-676767296_4-20180718t174802z-p.log        file       8801  2018-07-18T22:41:28+00:00
stderr-job_prep-tvm-676767296_1-20180718t174802z-p.txt  file        238  2018-07-18T22:41:50+00:00
stderr-job_prep-tvm-676767296_2-20180718t174802z-p.txt  file        238  2018-07-18T22:41:50+00:00
stderr-job_prep-tvm-676767296_3-20180718t174802z-p.txt  file        238  2018-07-18T22:41:50+00:00
stderr-job_prep-tvm-676767296_4-20180718t174802z-p.txt  file        238  2018-07-18T22:41:50+00:00
stderr.txt                                              file       7653  2018-07-18T22:46:32+00:00
stdout-job_prep-tvm-676767296_1-20180718t174802z-p.txt  file      13651  2018-07-18T22:41:55+00:00
stdout-job_prep-tvm-676767296_2-20180718t174802z-p.txt  file      13651  2018-07-18T22:41:54+00:00
stdout-job_prep-tvm-676767296_3-20180718t174802z-p.txt  file      13651  2018-07-18T22:41:54+00:00
stdout-job_prep-tvm-676767296_4-20180718t174802z-p.txt  file      13651  2018-07-18T22:41:55+00:00
stdout.txt                                              file    2316480  2018-07-18T22:46:32+00:00
```

### <a name="stream-an-output-file"></a>Çıkış dosyasının akışını yapma

Bir dosyanın içeriğinin akışını yapmak için `az batchai job file stream` komutunu kullanın. Aşağıdaki örnek, ana çıkış günlüğünün akışını yapar.

```azurecli-interactive
az batchai job file stream --experiment cifar --file-name stdout.txt --job cifar_distributed --resource-group batchai.horovod --workspace batchaidev
```

İş çalıştırılırken, komut eğitim işi için standart çıkışın akışını yapar ve aşağıdakine benzer bir çıkış gösterir.

```
...
50000 train samples
10000 test samples
Using real-time data augmentation.
Epoch 1/25


   1/1563 [..............................] - ETA: 2:42:25 - loss: 2.3334 - acc: 0.0312   1/1563 [..............................] - ETA: 2:30:42 - loss: 2.2973 - acc: 0.0938
   1/1563 [..............................] - ETA: 30:36 - loss: 2.3175 - acc: 0.1250
   1/1563 [..............................] - ETA: 2:32:58 - loss: 2.3489 - acc: 0.0625
   2/1563 [..............................] - ETA: 1:21:59 - loss: 2.3230 - acc: 0.0625

   2/1563 [..............................]   2/1563 [..............................] - ETA: 1:16:09 - loss: 2.2913 - acc: 0.0938 - ETA: 1:17:15 - loss: 2.3147 - acc: 0.0781
   2/1563 [..............................] - ETA: 16:07 - loss: 2.3678 - acc: 0.0938
   3/1563 [..............................] - ETA: 55:05 - loss: 2.3232 - acc: 0.0938  
   3/1563 [..............................] - ETA: 51:57 - loss: 2.3185 - acc: 0.1146  
   3/1563 [..............................] - ETA: 51:12 - loss: 2.3179 - acc: 0.1042  
   3/1563 [..............................] - ETA: 11:13 - loss: 2.3504 - acc: 0.0833
   4/1563 [..............................] - ETA: 39:43 - loss: 2.3224 - acc: 0.1094
   4/1563 [..............................] - ETA: 42:09 - loss: 2.3049 - acc: 0.1250
   4/1563 [..............................] - ETA: 39:15 - loss: 2.3089 - acc: 0.1094
   4/1563 [..............................] - ETA: 9:16 - loss: 2.3316 - acc: 0.1016 
   5/1563 [..............................] - ETA: 39:51 - loss: 2.3153 - acc: 0.1125
   5/1563 [..............................] - ETA: 37:58 - loss: 2.3197 - acc: 0.1125
   5/1563 [..............................] - ETA: 37:35 - loss: 2.3148 - acc: 0.1062
   5/1563 [..............................] - ETA: 13:38 - loss: 2.3263 - acc: 0.1062
   6/1563 [..............................] - ETA: 35:48 - loss: 2.3168 - acc: 0.1198

   6/1563 [..............................]   6/1563 [..............................] - ETA: 34:13 - loss: 2.3142 - acc: 0.1198 - ETA: 33:51 - loss: 2.3162 - acc: 0.1042
   6/1563 [..............................] - ETA: 13:54 - loss: 2.3225 - acc: 0.1094
   7/1563 [..............................] - ETA: 30:53 - loss: 2.3181 - acc: 0.1071

   7/1563 [..............................]   7/1563 [..............................] - ETA: 29:32 - loss: 2.3149 - acc: 0.1161 - ETA: 29:13 - loss: 2.3140 - acc: 0.0938
   7/1563 [..............................] - ETA: 12:09 - loss: 2.3174 - acc: 0.1205
   8/1563 [..............................] - ETA: 26:04 - loss: 2.3113 - acc: 0.1133
   8/1563 [..............................] - ETA: 27:15 - loss: 2.3169 - acc: 0.1133
   8/1563 [..............................] - ETA: 10:51 - loss: 2.3152 - acc: 0.1172
...
```

Betik 25’in üzerinde dönem eğitir veya eğitim veri kümesinden geçer. Bu işlem yaklaşık 60 dakika sürer. 

## <a name="retrieve-the-results"></a>Sonuçları alma

Betik tamamlandığında, her şey olması gerektiği şekildeyse, doğrulamanın doğruluk oranı %70-75 civarındadır ve eğitilen model `cifar/saved_models/keras_cifar10_trained_model.h5` konumundaki dosya paylaşımına kaydedilir. 

Model eğitimi genellikle daha büyük bir iş akışının parçasıdır. Örneğin, eğitilmiş modeli başka bir uygulamada kullanıma sunabilirsiniz. Eğitilen modeli yerel olarak indirmek için `az storage file download` komutunu kullanın. 

```azurecli-interactive
az storage file download --path cifar/saved_models/keras_cifar10_trained_model.h5 --share-name myshare --account-name mystorageaccount
```
## <a name="clean-up-resources"></a>Kaynakları temizleme

İşlerin çalıştırılması tamamlandıktan sonra işlem maliyetlerinden tasarruf etmenin en iyi yollarından biri, boşta kalma süresi için ücret ödememek üzere tüm kümelerin ölçeğini küçülterek `0 nodes` olarak ayarlamaktır. Aşağıdaki `az batchai cluster resize` komutunu kullanın. 

```azurecli-interactive
az batchai cluster resize --name nc6cluster --resource-group batchai.horovod --target 0 --workspace batchaidev
```

Daha sonra, işlerinizi çalıştırmak için 1 veya daha fazla düğüm olarak yeniden boyutlandırın. 

Gelecekte çalışma alanı ve depolama hesabını kullanmayı planlamıyorsanız, `az group delete` komutunu kullanarak kaynak grubunu silin. Bir kaynak grubunun silinmesi, bu grubun parçası olan tüm kaynakların silinmesine neden olur.

```azurecli-interactive
az group delete --name batchai.horovod
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunlar hakkında bilgi edindiniz:

> [!div class="checklist"]
> * Batch AI çalışma alanı, denemesi ve kümesi ayarlama
> * Giriş ve çıkış için bir Azure dosya paylaşımı ayarlama
> * Horovod kullanarak modeli paralelleştirme
> * Eğitim işi gönderme
> * İş izleme
> * Eğitim sonuçlarını alma

Farklı çerçeveler ile Batch AI kullanım örnekleri için, GitHub’daki tariflere bakın.

> [!div class="nextstepaction"]
> [Batch AI tarifleri](https://github.com/Azure/BatchAI/tree/master/recipes)
