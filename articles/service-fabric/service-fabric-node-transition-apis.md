---
title: Başlatma ve durdurma Azure Service Fabric uygulamalarını test etmek için küme düğümlerinin | Microsoft Docs
description: Hata ekleme, başlatma ve durdurma küme düğümleri bir Service Fabric uygulamasını test etmek için kullanmayı öğrenin.
services: service-fabric
documentationcenter: .net
author: LMWF
manager: rsinha
editor: ''
ms.assetid: f4e70f6f-cad9-4a3e-9655-009b4db09c6d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 6/12/2017
ms.author: lemai
ms.openlocfilehash: df0e53736c08fd2c26c467def7328e85f2989f26
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60718147"
---
# <a name="replacing-the-start-node-and-stop-node-apis-with-the-node-transition-api"></a>Düğüm geçişi API ile Başlat ve Durdur düğümlerini API'leri değiştirme

## <a name="what-do-the-stop-node-and-start-node-apis-do"></a>Düğüm durdurmak ve başlatmak düğüm API'leri ne yapacaksınız?

Durdurma düğüm API (yönetilen: [StopNodeAsync()][stopnode], PowerShell: [Stop-ServiceFabricNode][stopnodeps]) bir Service Fabric düğüm durdurur.  Bir Service Fabric düğüm işlemi, bir VM veya değil makine – VM veya makine hala çalıştırırsınız.  Belgenin geri kalanında için Service Fabric düğümü "düğüm" anlamına gelir.  Bir düğümü durdurma, koyar içine bir *durduruldu* burada kümesinin bir üyesi değil ve Hizmetleri, bu nedenle benzetimi barındıramaz durumu bir *aşağı* düğümü.  Bu, hataları uygulamanızı test etmek için sisteme ekleme için kullanışlıdır.  Başlangıç düğümü API (yönetilen: [StartNodeAsync()][startnode], PowerShell: [Start-ServiceFabricNode][startnodeps]]) düğüm normal bir duruma getirir Durdur düğüm API tersine çevirir.

## <a name="why-are-we-replacing-these"></a>Neden Biz bu değiştirdiğiniz?

Daha önce açıklandığı bir *durduruldu* Service Fabric düğümü olan bir düğümü durdurma düğüm API'sini kullanarak kasıtlı olarak hedeflenen.  A *aşağı* (örneğin, VM veya makine kapalıdır) herhangi bir nedenle kapalı bir düğümü düğümüdür.  Durdurma düğüm API'SİYLE birbirinden ayırmak için bilgi sistemini açığa çıkarmamak *durduruldu* düğümleri ve *aşağı* düğümleri.

Ayrıca, bu API'ları tarafından döndürülen bazı hatalar olması olabilir olarak tanımlayıcı değildir.  Örneğin, üzerinde Durdur düğüm API çağırma zaten bir *durduruldu* düğüm hata döndürecektir *InvalidAddress*.  Bu deneyimi geliştirildi.

Ayrıca, bir düğüm için durduruldu süresi başlangıç düğümü API çağrılana kadar "sonsuz" olur.  Biz bu sorunlara neden olabilir ve hata yapmaya açık buldunuz.  Örneğin, burada bir kullanıcı bir düğümde Durdur düğüm API çağrılır ve ardından unuttum sorunları gördük.  Daha sonra düğüm varsa belirsiz *aşağı* veya *durduruldu*.


## <a name="introducing-the-node-transition-apis"></a>Düğüm geçişi API'lerini ile tanışın

Biz, yeni bir API kümesi üzerinde bu sorunları ele aldık.  Yeni düğüm geçiş API (yönetilen: [StartNodeTransitionAsync()][snt]) bir Service Fabric düğümünü geçiş için kullanılabilir bir *durduruldu* durumu veya ondan geçiş için bir *durduruldu* durumunu bir normal çalışır durumda.  API adı "Başlangıç" ile başlayarak bir düğüm başvurmuyor unutmayın.  Sistem ya da düğümü geçmeye yürütecek bir zaman uyumsuz işlem başlamadan için başvurduğu *durduruldu* veya durumu başlatıldı.

**Kullanım**

Düğüm geçişi API çağrıldığında bir özel durum oluşturmaz, sonra sistem zaman uyumsuz işlemi kabul etti ve bunu çalıştırır.  Başarılı bir çağrı işlemi henüz tamamlandı göstermez.  İşlemi geçerli durumu hakkında bilgi almak için düğüm geçiş ilerleme API çağrısı (yönetilen: [GetNodeTransitionProgressAsync()][gntp]) düğüm geçiş API bu işlemi için DTA çağrılırken kullanılan GUID'e sahip.  Düğüm geçişi ilerleme API NodeTransitionProgress nesneyi döndürür.  Bu nesnenin durumu özelliği, geçerli işlemin durumunu belirtir.  Durumu "çalışıyor"durumunda, sonra işlemi yürütülüyor.  Tamamlandıysa, hatasız işlemi tamamlandı.  Hatalı, işlem yürütülürken bir hata oluştu.  Sonuç özelliğin özel durum özelliği, sorunun ne olduğunu gösterir.  Bkz: https://docs.microsoft.com/dotnet/api/system.fabric.testcommandprogressstate State özelliği ve kod örnekleri için aşağıdaki "Örnek kullanım" bölümüne hakkında daha fazla bilgi.


**Durdurulmuş bir düğüm ile aşağı düğümü arasında ayrım** bir düğümü ise *durduruldu* düğümü sorgusu çıkışını düğüm geçiş API'sini kullanarak (yönetilen: [GetNodeListAsync()][nodequery], PowerShell: [Get-ServiceFabricNode][nodequeryps]) bu düğüm olduğunu gösterecek bir *IsStopped* özellik değeri true.  Bu değerinden farklı olduğunu unutmayın *NodeStatus* kilit ekranında özelliği *aşağı*.  Varsa *NodeStatus* özellik değerine sahip *aşağı*, ancak *IsStopped* sonra düğüm düğüm geçiş API'sini kullanarak durdurulmadı false ise ve *aşağı*  herhangi bir nedenden dolayı.  Varsa *IsStopped* özelliği true ise ve *NodeStatus* özelliği *aşağı*, ardından düğümü geçiş API'sini kullanarak durduruldu.

Başlangıç bir *durduruldu* düğüm geçiş API'sini kullanarak düğüm kümesinin normal bir üyesi tekrar çalışabilmesi için döndürecektir.  Düğüm sorgu API'si çıktısını gösterir *IsStopped* false olarak ve *NodeStatus* aşağı (örneğin, Up) olmayan bir şey olarak.


**Sınırlı bir süre** bir düğüm, gerekli parametrelerden birini durdurmak için düğüm geçiş API'si kullanılırken *stopNodeDurationInSeconds*, düğüm tutmak için saniye cinsinden süreyi temsil eden *durduruldu*.  Bu değer, en az 600 ve 14400 en fazla izin verilen aralıkta olmalıdır.  Bu süre dolduktan sonra düğümü kendi içine durumunu otomatik olarak yeniden başlatılır.  Kullanım örneği için örnek 1'e bakın.

> [!WARNING]
> Düğüm geçişi API'lerini ve düğüm durdurmak ve başlangıç düğümü API'leri karıştırmayın.  Yalnızca düğüm geçiş API'yi kullanmak için önerilir.  >, Bir düğüm olup, zaten durduruldu. durdurmak düğüm API'sini kullanarak, bu başlangıç düğümü ilk önce kullanarak API'yi kullanarak başlatılmalıdır > düğüm geçişi API'lerini.

> [!WARNING]
> Düğüm geçişi API'lerini aramasına aynı düğümde paralel yapılamaz.  Böyle bir durumda, düğüm geçiş API olacak > NodeTransitionInProgress ErrorCode özellik değeri olan bir FabricException atar.  Belirli bir düğümde bir düğüm geçişi olduğunda > bırakıldı işlemi başlamadan önce (tamamlandı, hatalı veya ForceCancelled) terminal durumuna ulaştığında başlatıldı, beklemeniz bir > aynı düğümde yeni geçişi.  Düğüm paralel geçiş çağrıları farklı düğümlere izin verilir.


#### <a name="sample-usage"></a>Örnek kullanımı


**Örnek 1** -aşağıdaki örnek, bir düğüm durdurmak için düğüm geçiş API'sini kullanır.

```csharp
        // Helper function to get information about a node
        static Node GetNodeInfo(FabricClient fc, string node)
        {
            NodeList n = null;
            while (n == null)
            {
                n = fc.QueryManager.GetNodeListAsync(node).GetAwaiter().GetResult();
                Task.Delay(TimeSpan.FromSeconds(1)).GetAwaiter();
            };

            return n.FirstOrDefault();
        }

        static async Task WaitForStateAsync(FabricClient fc, Guid operationId, TestCommandProgressState targetState)
        {
            NodeTransitionProgress progress = null;

            do
            {
                bool exceptionObserved = false;
                try
                {
                    progress = await fc.TestManager.GetNodeTransitionProgressAsync(operationId, TimeSpan.FromMinutes(1), CancellationToken.None).ConfigureAwait(false);
                }
                catch (OperationCanceledException oce)
                {
                    Console.WriteLine("Caught exception '{0}'", oce);
                    exceptionObserved = true;
                }
                catch (FabricTransientException fte)
                {
                    Console.WriteLine("Caught exception '{0}'", fte);
                    exceptionObserved = true;
                }

                if (!exceptionObserved)
                {
                    Console.WriteLine("Current state of operation '{0}': {1}", operationId, progress.State);

                    if (progress.State == TestCommandProgressState.Faulted)
                    {
                        // Inspect the progress object's Result.Exception.HResult to get the error code.
                        Console.WriteLine("'{0}' failed with: {1}, HResult: {2}", operationId, progress.Result.Exception, progress.Result.Exception.HResult);

                        // ...additional logic as required
                    }

                    if (progress.State == targetState)
                    {
                        Console.WriteLine("Target state '{0}' has been reached", targetState);
                        break;
                    }
                }

                await Task.Delay(TimeSpan.FromSeconds(5)).ConfigureAwait(false);
            }
            while (true);
        }

        static async Task StopNodeAsync(FabricClient fc, string nodeName, int durationInSeconds)
        {
            // Uses the GetNodeListAsync() API to get information about the target node
            Node n = GetNodeInfo(fc, nodeName);

            // Create a Guid
            Guid guid = Guid.NewGuid();

            // Create a NodeStopDescription object, which will be used as a parameter into StartNodeTransition
            NodeStopDescription description = new NodeStopDescription(guid, n.NodeName, n.NodeInstanceId, durationInSeconds);

            bool wasSuccessful = false;

            do
            {
                try
                {
                    // Invoke StartNodeTransitionAsync with the NodeStopDescription from above, which will stop the target node.  Retry transient errors.
                    await fc.TestManager.StartNodeTransitionAsync(description, TimeSpan.FromMinutes(1), CancellationToken.None).ConfigureAwait(false);
                    wasSuccessful = true;
                }
                catch (OperationCanceledException oce)
                {
                    // This is retryable
                }
                catch (FabricTransientException fte)
                {
                    // This is retryable
                }

                // Backoff
                await Task.Delay(TimeSpan.FromSeconds(5)).ConfigureAwait(false);
            }
            while (!wasSuccessful);

            // Now call StartNodeTransitionProgressAsync() until the desired state is reached.
            await WaitForStateAsync(fc, guid, TestCommandProgressState.Completed).ConfigureAwait(false);
        }
```

**Örnek 2** -aşağıdaki örnekleme başlatılır bir *durduruldu* düğümü.  İlk örnekteki bazı yardımcı yöntemler kullanır.

```csharp
        static async Task StartNodeAsync(FabricClient fc, string nodeName)
        {
            // Uses the GetNodeListAsync() API to get information about the target node
            Node n = GetNodeInfo(fc, nodeName);

            Guid guid = Guid.NewGuid();
            BigInteger nodeInstanceId = n.NodeInstanceId;

            // Create a NodeStartDescription object, which will be used as a parameter into StartNodeTransition
            NodeStartDescription description = new NodeStartDescription(guid, n.NodeName, nodeInstanceId);

            bool wasSuccessful = false;

            do
            {
                try
                {
                    // Invoke StartNodeTransitionAsync with the NodeStartDescription from above, which will start the target stopped node.  Retry transient errors.
                    await fc.TestManager.StartNodeTransitionAsync(description, TimeSpan.FromMinutes(1), CancellationToken.None).ConfigureAwait(false);
                    wasSuccessful = true;
                }
                catch (OperationCanceledException oce)
                {
                    Console.WriteLine("Caught exception '{0}'", oce);
                }
                catch (FabricTransientException fte)
                {
                    Console.WriteLine("Caught exception '{0}'", fte);
                }

                await Task.Delay(TimeSpan.FromSeconds(5)).ConfigureAwait(false);

            }
            while (!wasSuccessful);

            // Now call StartNodeTransitionProgressAsync() until the desired state is reached.
            await WaitForStateAsync(fc, guid, TestCommandProgressState.Completed).ConfigureAwait(false);
        }
```

**Örnek 3** -yanlış kullanımı aşağıdaki örnekte gösterilmektedir.  Bu kullanım yanlış olduğundan *stopDurationInSeconds* sağlar, izin verilen aralık büyüktür.  Bu yana StartNodeTransitionAsync() önemli bir hata ile başarısız olur işlemi kabul ve ilerleme API çağrılmamalıdır.  Bu örnek, ilk örnekteki bazı yardımcı yöntemler kullanır.

```csharp
        static async Task StopNodeWithOutOfRangeDurationAsync(FabricClient fc, string nodeName)
        {
            Node n = GetNodeInfo(fc, nodeName);

            Guid guid = Guid.NewGuid();

            // Use an out of range value for stopDurationInSeconds to demonstrate error
            NodeStopDescription description = new NodeStopDescription(guid, n.NodeName, n.NodeInstanceId, 99999);

            try
            {
                await fc.TestManager.StartNodeTransitionAsync(description, TimeSpan.FromMinutes(1), CancellationToken.None).ConfigureAwait(false);
            }

            catch (FabricException e)
            {
                Console.WriteLine("Caught {0}", e);
                Console.WriteLine("ErrorCode {0}", e.ErrorCode);
                // Output:
                // Caught System.Fabric.FabricException: System.Runtime.InteropServices.COMException (-2147017629)
                // StopDurationInSeconds is out of range ---> System.Runtime.InteropServices.COMException: Exception from HRESULT: 0x80071C63
                // << Parts of exception omitted>>
                //
                // ErrorCode InvalidDuration
            }
        }
```

**Örnek 4** -düğüm geçiş API'sı tarafından başlatılan bir işlemin kabul edilir, ancak daha sonra yürütülürken başarısız olduğunda düğüm geçiş ilerleme API'den döndürülen hata bilgileri aşağıdaki örnekte gösterilmektedir.  Düğüm geçişi API var olmayan düğüm çalıştığı durumunda başarısız olur.  Bu örnek, ilk örnekteki bazı yardımcı yöntemler kullanır.

```csharp
        static async Task StartNodeWithNonexistentNodeAsync(FabricClient fc)
        {
            Guid guid = Guid.NewGuid();
            BigInteger nodeInstanceId = 12345;

            // Intentionally use a nonexistent node
            NodeStartDescription description = new NodeStartDescription(guid, "NonexistentNode", nodeInstanceId);

            bool wasSuccessful = false;

            do
            {
                try
                {
                    // Invoke StartNodeTransitionAsync with the NodeStartDescription from above, which will start the target stopped node.  Retry transient errors.
                    await fc.TestManager.StartNodeTransitionAsync(description, TimeSpan.FromMinutes(1), CancellationToken.None).ConfigureAwait(false);
                    wasSuccessful = true;
                }
                catch (OperationCanceledException oce)
                {
                    Console.WriteLine("Caught exception '{0}'", oce);
                }
                catch (FabricTransientException fte)
                {
                    Console.WriteLine("Caught exception '{0}'", fte);
                }

                await Task.Delay(TimeSpan.FromSeconds(5)).ConfigureAwait(false);

            }
            while (!wasSuccessful);

            // Now call StartNodeTransitionProgressAsync() until the desired state is reached.  In this case, it will end up in the Faulted state since the node does not exist.
            // When StartNodeTransitionProgressAsync()'s returned progress object has a State if Faulted, inspect the progress object's Result.Exception.HResult to get the error code.
            // In this case, it will be NodeNotFound.
            await WaitForStateAsync(fc, guid, TestCommandProgressState.Faulted).ConfigureAwait(false);
        }
```

[stopnode]: https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.faultmanagementclient?redirectedfrom=MSDN
[stopnodeps]: https://msdn.microsoft.com/library/mt125982.aspx
[startnode]: https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.faultmanagementclient?redirectedfrom=MSDN
[startnodeps]: https://msdn.microsoft.com/library/mt163520.aspx
[nodequery]: https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient
[nodequeryps]: https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricnode
[snt]: https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.testmanagementclient
[gntp]: https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.testmanagementclient
