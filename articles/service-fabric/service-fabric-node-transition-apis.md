---
title: Başlatma ve durdurma Azure mikro test etmek için küme düğümlerinin | Microsoft Docs
description: Service Fabric uygulaması başlatma ve küme düğümleri durdurma test etmek için hata ekleme kullanmayı öğrenin.
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
ms.openlocfilehash: 0ed18097fa18101c237b4408d26dd1bc9c5d5648
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="replacing-the-start-node-and-stop-node-apis-with-the-node-transition-api"></a>Başlatma ve durdurma düğümlerini API'leri düğümü geçiş API ile değiştirme

## <a name="what-do-the-stop-node-and-start-node-apis-do"></a>Düğüm durdurmak ve başlatmak düğümü API'leri ne yapacaksınız?

Durdur düğümü API (yönetilen: [StopNodeAsync()][stopnode], PowerShell: [Stop-ServiceFabricNode][stopnodeps]) bir Service Fabric düğümü durdurur.  Service Fabric düğümü işlem veya değil, VM makine – VM veya makine hala çalıştırırsınız.  Belgenin geri kalanında için Service Fabric düğümü "düğümü" anlamına gelir.  Bir düğümü durdurma, koyar içine bir *durduruldu* burada kümesinin bir üyesi değil ve Hizmetleri, bu nedenle benzetimi barındıramaz durumu bir *aşağı* düğümü.  Bu, uygulamanızı test etmek için sisteme hataları injecting için kullanışlıdır.  Başlangıç düğümü API (yönetilen: [StartNodeAsync()][startnode], PowerShell: [başlangıç ServiceFabricNode][startnodeps]]) durdurun düğümü API tersine çevirir  hangi düğümün normal bir duruma getirir.

## <a name="why-are-we-replacing-these"></a>Neden Biz bu değiştirdiğiniz?

Daha önce açıklandığı gibi bir *durduruldu* Service Fabric düğümdür bilerek Durdur düğümü API kullanarak hedeflenen bir düğüm.  A *aşağı* (örneğin VM veya makine kapalıdır) herhangi bir nedenle çalışmıyor bir düğüm düğümdür.  Durdur düğümü API'si ile birbirinden ayırmak için bilgi sistem kullanıma sunmuyor *durduruldu* düğümleri ve *aşağı* düğümleri.

Ayrıca, bu API'ları tarafından döndürülen bazı hatalar olması olabilir olarak tanımlayıcı değildir.  Durdur düğümü API harekete Örneğin, zaten bir *durduruldu* düğümü hata döndürecektir *InvalidAddress*.  Bu deneyim geliştirilmiş.

Ayrıca, başlangıç düğümü API çağrılan kadar bir düğüm için durduruldu süresi "sonsuz" olur.  Biz bu sorunlara neden olabilir ve hataya yatkın olabilir buldunuz.  Örneğin, burada bir kullanıcı bir düğüm üzerinde Durdur düğümü API çağrılır ve bilgiyi unuttunuz sorunları gördük.  Daha sonra düğüm varsa belirsiz *aşağı* veya *durduruldu*.


## <a name="introducing-the-node-transition-apis"></a>Düğüm geçiş API'leri Tanıtımı

Biz, yeni bir API kümesi yukarıda bu sorunları ele aldık.  Yeni düğüm geçiş API (yönetilen: [StartNodeTransitionAsync()][snt]) bir Service Fabric düğüme geçiş için kullanılabilir bir *durduruldu* durumu veya ondan geçiş için bir *durduruldu* çalışır durumda normal durum.  Lütfen bir düğümü başlatma için API adına "Başlat" başvurmuyor unutmayın.  Sistem ya da düğüme geçiş yürütecek zaman uyumsuz bir işlem başlangıcı için başvurduğu *durduruldu* veya durumu başlatıldı.

**Kullanım**

Düğüm geçiş API çağrıldığında bir özel durum oluşturmadığını, ardından sistemi zaman uyumsuz işlemi kabul etti ve yürütülmez.  Başarılı bir çağrı işlemi henüz tamamlandı göstermez.  İşlemi geçerli durumu hakkında bilgi almak için düğüm geçiş ilerleme API çağrısı (yönetilen: [GetNodeTransitionProgressAsync()][gntp]) düğüm geçiş API çağrılırken kullanılan GUID ile Bu işlem için.  Düğüm geçiş ilerleme API NodeTransitionProgress nesneyi döndürür.  Bu nesnenin durumu özelliği işlemi geçerli durumunu belirtir.  Durumu "çalışıyorsa," işlemi yürütüyor.  Tamamlandığında, hatasız işlemi tamamlandı.  Hatalı, işlem yürütülürken bir sorun oluştu.  Sonuç özelliğin özel durum özelliği ne sorun olduğunu belirtir.  Bkz: https://docs.microsoft.com/dotnet/api/system.fabric.testcommandprogressstate State özelliği ve kod örnekleri için aşağıdaki "Örnek kullanım" bölümüne hakkında daha fazla bilgi için.


**Durdurulmuş bir düğümü ve bir aşağı düğümü arasında ayrım yapma** bir düğümü ise *durduruldu* bir düğümü sorgusu çıktısını düğümü geçiş API kullanarak (yönetilen: [GetNodeListAsync()] [ nodequery], PowerShell: [Get-ServiceFabricNode][nodequeryps]) bu düğüm olduğunu gösterecek bir *IsStopped* özellik değerinin true.  Bu değerinden farklı Not *NodeStatus* yazacaktır özelliği *aşağı*.  Varsa *NodeStatus* özellik değerine sahip *aşağı*, ancak *IsStopped* düğümünü düğüm geçiş API kullanarak durdurulmadı sonra yanlış olduğundan ve *aşağı*  başka bir nedenle son.  Varsa *IsStopped* özelliği true, ise ve *NodeStatus* özelliği *aşağı*, düğüm geçiş API kullanarak durduruldu sonra.

Başlangıç bir *durduruldu* düğümü geçiş API kullanarak düğümünü yeniden normal bir küme üyesi olarak çalışabilmesi için döndürecektir.  Düğümü sorgusu API çıktısını gösterir *IsStopped* false olarak ve *NodeStatus* aşağı (örneğin yukarı) olmayan bir şey olarak.


**Sınırlı bir süre** düğümü geçiş API'si bir düğüm, gereken parametrelerden biri durdurmak için kullanılırken *stopNodeDurationInSeconds*, düğüm tutmak için saniye cinsinden süreyi temsil eden *durduruldu*.  Bu değer 600 en az ve en fazla 14400 olan izin verilen aralığında olmalıdır.  Bu süre dolduktan sonra düğüm kendi içine durumunu otomatik olarak yeniden başlatılır.  Kullanım örneği örnek 1 bakın.

> [!WARNING]
> Düğümü geçiş API'leri ve düğüm durdurmak ve başlatmak düğümü API'leri karıştırma kaçının.  Yalnızca düğüm geçiş API'sini kullanmanız önerilir.  > Bir düğüm olup zaten yapıldıysa Durdur düğümü API kullanarak durduruldu, onu başlangıç düğümü ilk önce kullanarak API kullanılarak başlatılmaması gerekir > düğümü geçiş API'leri.

> [!WARNING]
> Birden çok düğüm geçiş API çağrıları, aynı düğümde paralel yapılamaz.  Böyle bir durumda düğüm geçiş API olacak > NodeTransitionInProgress ErrorCode özellik değeri olan bir FabricException atar.  Belirli bir düğümde bir düğüm geçiş sonra > edilmiş işlemi başlamadan önce bir terminal durumu (tamamlandı, Faulted veya ForceCancelled) ulaşana kadar başlatıldı, beklemeniz gerekir bir > aynı düğümde yeni geçişi.  Paralel düğümü geçiş çağrıları farklı düğümlerde izin verilir.


#### <a name="sample-usage"></a>Örnek Kullanım


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

            // Now call StartNodeTransitionProgressAsync() until hte desired state is reached.
            await WaitForStateAsync(fc, guid, TestCommandProgressState.Completed).ConfigureAwait(false);
        }
```

**Örnek 2** -aşağıdaki örnek başlatır bir *durduruldu* düğümü.  İlk örnekten bazı yardımcı yöntemler kullanır.

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

            // Now call StartNodeTransitionProgressAsync() until hte desired state is reached.
            await WaitForStateAsync(fc, guid, TestCommandProgressState.Completed).ConfigureAwait(false);
        }
```

**Örnek 3** -aşağıdaki örnek yanlış kullanımını gösterir.  Bu kullanım yanlış olduğundan *stopDurationInSeconds* sağladığı izin verilen aralığından daha büyük.  StartNodeTransitionAsync() önemli bir hata ile başarısız olur olduğundan, işlem kabul ve ilerleme API çağrılmamalı.  Bu örnek ilk örnekten bazı yardımcı yöntemler kullanır.

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

**Örnek 4** -aşağıdaki örnek düğüm geçiş API tarafından başlatılan işlemi kabul edilir, ancak daha sonra yürütülürken başarısız olduğunda düğüm geçiş ilerleme API'SİNDEN döndürülen hata bilgilerini gösterir.  Düğüm geçiş API var olmayan bir düğümü başlatma girişiminde bulunduğu için durumunda başarısız olur.  Bu örnek ilk örnekten bazı yardımcı yöntemler kullanır.

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

[stopnode]: https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.faultmanagementclient?redirectedfrom=MSDN#System_Fabric_FabricClient_FaultManagementClient_StopNodeAsync_System_String_System_Numerics_BigInteger_System_Fabric_CompletionMode_
[stopnodeps]: https://msdn.microsoft.com/library/mt125982.aspx
[startnode]: https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.faultmanagementclient?redirectedfrom=MSDN#System_Fabric_FabricClient_FaultManagementClient_StartNodeAsync_System_String_System_Numerics_BigInteger_System_String_System_Int32_System_Fabric_CompletionMode_System_Threading_CancellationToken_
[startnodeps]: https://msdn.microsoft.com/library/mt163520.aspx
[nodequery]: https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient#System_Fabric_FabricClient_QueryClient_GetNodeListAsync_System_String_
[nodequeryps]: https://docs.microsoft.com/powershell/servicefabric/vlatest/Get-ServiceFabricNode?redirectedfrom=msdn
[snt]: https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.testmanagementclient#System_Fabric_FabricClient_TestManagementClient_StartNodeTransitionAsync_System_Fabric_Description_NodeTransitionDescription_System_TimeSpan_System_Threading_CancellationToken_
[gntp]: https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.testmanagementclient#System_Fabric_FabricClient_TestManagementClient_GetNodeTransitionProgressAsync_System_Guid_System_TimeSpan_System_Threading_CancellationToken_
