---
title: Consumidores concorrentes
description: "Habilite vários consumidores simultâneos para processar as mensagens recebidas no mesmo canal de mensagens."
keywords: "padrão de design"
author: dragon119
ms.date: 06/23/2017
pnp.series.title: Cloud Design Patterns
pnp.pattern.categories: messaging
ms.openlocfilehash: d72a09ef7613bebe3701634e4eac0716400e471d
ms.sourcegitcommit: b0482d49aab0526be386837702e7724c61232c60
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/14/2017
---
# <a name="competing-consumers-pattern"></a><span data-ttu-id="ecc5a-104">Padrão de consumidores concorrentes</span><span class="sxs-lookup"><span data-stu-id="ecc5a-104">Competing Consumers pattern</span></span>

[!INCLUDE [header](../_includes/header.md)]

<span data-ttu-id="ecc5a-105">Habilite vários consumidores simultâneos para processar as mensagens recebidas no mesmo canal de mensagens.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-105">Enable multiple concurrent consumers to process messages received on the same messaging channel.</span></span> <span data-ttu-id="ecc5a-106">Isso permite que um sistema processe várias mensagens simultaneamente para otimizar a taxa de transferência, melhorar a escalabilidade e a disponibilidade e equilibrar a carga de trabalho.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-106">This enables a system to process multiple messages concurrently to optimize throughput, to improve scalability and availability, and to balance the workload.</span></span>

## <a name="context-and-problem"></a><span data-ttu-id="ecc5a-107">Contexto e problema</span><span class="sxs-lookup"><span data-stu-id="ecc5a-107">Context and problem</span></span>

<span data-ttu-id="ecc5a-108">É esperado que um aplicativo em execução na nuvem lide com um grande número de solicitações.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-108">An application running in the cloud is expected to handle a large number of requests.</span></span> <span data-ttu-id="ecc5a-109">Em vez de processar cada solicitação de forma síncrona, uma técnica comum é o aplicativo passá-las por meio de um sistema de mensagens para outro serviço (um serviço consumidor) que as trata de forma assíncrona.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-109">Rather than process each request synchronously, a common technique is for the application to pass them through a messaging system to another service (a consumer service) that handles them asynchronously.</span></span> <span data-ttu-id="ecc5a-110">Essa estratégia ajuda a garantir que a lógica de negócios do aplicativo não seja bloqueada enquanto as solicitações estão sendo processadas.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-110">This strategy helps to ensure that the business logic in the application isn't blocked while the requests are being processed.</span></span>

<span data-ttu-id="ecc5a-111">O número de solicitações pode variar significativamente com o passar do tempo por vários motivos.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-111">The number of requests can vary significantly over time for many reasons.</span></span> <span data-ttu-id="ecc5a-112">Um aumento repentino de atividade do usuário ou solicitações agregadas provenientes de vários locatários pode causar uma carga de trabalho imprevisível.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-112">A sudden increase in user activity or aggregated requests coming from multiple tenants can cause an unpredictable workload.</span></span> <span data-ttu-id="ecc5a-113">Nos horário de pico de um sistema, pode ser necessário processar centenas de solicitações por segundo, enquanto em outros momentos o número pode ser muito pequeno.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-113">At peak hours a system might need to process many hundreds of requests per second, while at other times the number could be very small.</span></span> <span data-ttu-id="ecc5a-114">Além disso, a natureza do trabalho executado para lidar com essas solicitações pode ser altamente variável.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-114">Additionally, the nature of the work performed to handle these requests might be highly variable.</span></span> <span data-ttu-id="ecc5a-115">Usar uma única instância do serviço consumidor pode fazer com que ela seja inundada por solicitações ou o sistema de mensagens pode ficar sobrecarregado por um fluxo de mensagens recebidas do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-115">Using a single instance of the consumer service can cause that instance to become flooded with requests, or the messaging system might be overloaded by an influx of messages coming from the application.</span></span> <span data-ttu-id="ecc5a-116">Para lidar com essa carga de trabalho flutuante, o sistema pode executar várias instâncias do serviço do consumidor.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-116">To handle this fluctuating workload, the system can run multiple instances of the consumer service.</span></span> <span data-ttu-id="ecc5a-117">No entanto, esses consumidores devem ser coordenados para garantir que cada mensagem seja entregue somente para um único consumidor.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-117">However, these consumers must be coordinated to ensure that each message is only delivered to a single consumer.</span></span> <span data-ttu-id="ecc5a-118">A carga de trabalho também precisa ser balanceada entre os consumidores para impedir que uma instância se torne um gargalo.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-118">The workload also needs to be load balanced across consumers to prevent an instance from becoming a bottleneck.</span></span>

## <a name="solution"></a><span data-ttu-id="ecc5a-119">Solução</span><span class="sxs-lookup"><span data-stu-id="ecc5a-119">Solution</span></span>

<span data-ttu-id="ecc5a-120">Use uma fila de mensagens para implementar o canal de comunicação entre o aplicativo e as instâncias do serviço do consumidor.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-120">Use a message queue to implement the communication channel between the application and the instances of the consumer service.</span></span> <span data-ttu-id="ecc5a-121">O aplicativo posta solicitações na forma de mensagens na fila e as instâncias de serviço do consumidor recebem mensagens da fila e as processa.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-121">The application posts requests in the form of messages to the queue, and the consumer service instances receive messages from the queue and process them.</span></span> <span data-ttu-id="ecc5a-122">Essa abordagem permite que o mesmo pool de instâncias de serviço do consumidor lide com mensagens de qualquer instância do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-122">This approach enables the same pool of consumer service instances to handle messages from any instance of the application.</span></span> <span data-ttu-id="ecc5a-123">A figura ilustra o uso de uma fila de mensagens para distribuir o trabalho para instâncias de um serviço.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-123">The figure illustrates using a message queue to distribute work to instances of a service.</span></span>

![Usar uma fila de mensagens para distribuir o trabalho para instâncias de um serviço](./_images/competing-consumers-diagram.png)

<span data-ttu-id="ecc5a-125">Essa solução oferece as seguintes vantagens:</span><span class="sxs-lookup"><span data-stu-id="ecc5a-125">This solution has the following benefits:</span></span>

- <span data-ttu-id="ecc5a-126">Ele fornece um sistema para redistribuir a carga que pode lidar com grandes variações no volume de solicitações enviadas por instâncias de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-126">It provides a load-leveled system that can handle wide variations in the volume of requests sent by application instances.</span></span> <span data-ttu-id="ecc5a-127">A fila atua como um buffer entre as instâncias do aplicativo e as instâncias de serviço do consumidor.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-127">The queue acts as a buffer between the application instances and the consumer service instances.</span></span> <span data-ttu-id="ecc5a-128">Isso pode ajudar a minimizar o impacto sobre a disponibilidade e a capacidade de resposta tanto do aplicativo quanto das instâncias de serviço, conforme descrito pelo [Padrão de nivelamento de carga baseado em fila](queue-based-load-leveling.md).</span><span class="sxs-lookup"><span data-stu-id="ecc5a-128">This can help to minimize the impact on availability and responsiveness for both the application and the service instances, as described by the [Queue-based Load Leveling pattern](queue-based-load-leveling.md).</span></span> <span data-ttu-id="ecc5a-129">Lidar com uma mensagem que requer que processamento de longa execução não impede que outras mensagens sejam tratadas simultaneamente por outras instâncias do serviço do consumidor.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-129">Handling a message that requires some long-running processing doesn't prevent other messages from being handled concurrently by other instances of the consumer service.</span></span>

- <span data-ttu-id="ecc5a-130">Isso aumenta a confiabilidade.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-130">It improves reliability.</span></span> <span data-ttu-id="ecc5a-131">Se um produtor se comunica diretamente com um consumidor em vez de usar esse padrão, mas não monitora o consumidor, há uma grande probabilidade de que as mensagens poderiam ser perdidas ou não conseguirem ser processadas se o consumidor falhar.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-131">If a producer communicates directly with a consumer instead of using this pattern, but doesn't monitor the consumer, there's a high probability that messages could be lost or fail to be processed if the consumer fails.</span></span> <span data-ttu-id="ecc5a-132">Nesse padrão, as mensagens não são enviadas para uma instância de serviço específica.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-132">In this pattern, messages aren't sent to a specific service instance.</span></span> <span data-ttu-id="ecc5a-133">Uma instância de serviço com falha não bloqueará um produtor e as mensagens podem ser processadas por qualquer instância de serviço do trabalho.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-133">A failed service instance won't block a producer, and messages can be processed by any working service instance.</span></span>

- <span data-ttu-id="ecc5a-134">Ela não requer coordenação complexa entre os consumidores ou entre as instâncias do produtor e do consumidor.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-134">It doesn't require complex coordination between the consumers, or between the producer and the consumer instances.</span></span> <span data-ttu-id="ecc5a-135">A fila de mensagens garante que cada mensagem seja entregue pelo menos uma vez.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-135">The message queue ensures that each message is delivered at least once.</span></span>

- <span data-ttu-id="ecc5a-136">É escalonável.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-136">It's scalable.</span></span> <span data-ttu-id="ecc5a-137">O sistema pode aumentar ou diminuir o número dinamicamente o número de instâncias do serviço de consumidor conforme o volume de mensagens flutua.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-137">The system can dynamically increase or decrease the number of instances of the consumer service as the volume of messages fluctuates.</span></span>

- <span data-ttu-id="ecc5a-138">Isso poderá melhorar a resiliência se a fila de mensagens fornecer operações de leitura transacionais.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-138">It can improve resiliency if the message queue provides transactional read operations.</span></span> <span data-ttu-id="ecc5a-139">Se uma instância de serviço do consumidor lê e processa a mensagem como parte de uma operação transacional e a instância de serviço do consumidor falhar, esse padrão pode garantir que a mensagem seja retornada para a fila para ser coletada e tratada por outra instância e serviço do consumidor.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-139">If a consumer service instance reads and processes the message as part of a transactional operation, and the consumer service instance fails, this pattern can ensure that the message will be returned to the queue to be picked up and handled by another instance of the consumer service.</span></span>

## <a name="issues-and-considerations"></a><span data-ttu-id="ecc5a-140">Problemas e considerações</span><span class="sxs-lookup"><span data-stu-id="ecc5a-140">Issues and considerations</span></span>

<span data-ttu-id="ecc5a-141">Considere os seguintes pontos ao decidir como implementar esse padrão:</span><span class="sxs-lookup"><span data-stu-id="ecc5a-141">Consider the following points when deciding how to implement this pattern:</span></span>

- <span data-ttu-id="ecc5a-142">**Ordenação de mensagem**.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-142">**Message ordering**.</span></span> <span data-ttu-id="ecc5a-143">A ordem na qual as instâncias de serviço de consumidor recebem as mensagens não é garantida e não reflete necessariamente a ordem na qual as mensagens foram criadas.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-143">The order in which consumer service instances receive messages isn't guaranteed, and doesn't necessarily reflect the order in which the messages were created.</span></span> <span data-ttu-id="ecc5a-144">Projete o sistema para garantir que o processamento de mensagens seja idempotente, pois isso ajudará a eliminar qualquer dependência em relação à ordem na qual as mensagens são manipuladas.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-144">Design the system to ensure that message processing is idempotent because this will help to eliminate any dependency on the order in which messages are handled.</span></span> <span data-ttu-id="ecc5a-145">Para obter mais informações, consulte [Padrões de idempotência](http://blog.jonathanoliver.com/idempotency-patterns/) no blog de Jonathon Oliver.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-145">For more information, see [Idempotency Patterns](http://blog.jonathanoliver.com/idempotency-patterns/) on Jonathon Oliver’s blog.</span></span>

    > <span data-ttu-id="ecc5a-146">As Filas do Barramento de Serviço do Microsoft Azure podem implementar ordenação primeiro a entrar, primeiro a sair garantida de mensagens usando sessões de mensagens.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-146">Microsoft Azure Service Bus Queues can implement guaranteed first-in-first-out ordering of messages by using message sessions.</span></span> <span data-ttu-id="ecc5a-147">Para obter mais informações, consulte [Padrões de mensagens usando sessões](https://msdn.microsoft.com/magazine/jj863132.aspx).</span><span class="sxs-lookup"><span data-stu-id="ecc5a-147">For more information, see [Messaging Patterns Using Sessions](https://msdn.microsoft.com/magazine/jj863132.aspx).</span></span>

- <span data-ttu-id="ecc5a-148">**Projetar serviços para resiliência**.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-148">**Designing services for resiliency**.</span></span> <span data-ttu-id="ecc5a-149">Se o sistema foi projetado para detectar e reiniciar as instâncias de serviço com falha, pode ser necessário implementar o processamento executado pelas instâncias do serviço como operações idempotentes para minimizar os efeitos de uma única mensagem sendo recuperada e processada mais de uma vez.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-149">If the system is designed to detect and restart failed service instances, it might be necessary to implement the processing performed by the service instances as idempotent operations to minimize the effects of a single message being retrieved and processed more than once.</span></span>

- <span data-ttu-id="ecc5a-150">**Detecção de mensagens suspeitas**.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-150">**Detecting poison messages**.</span></span> <span data-ttu-id="ecc5a-151">Uma mensagem malformada ou uma tarefa que exige acesso a recursos que não estão disponíveis, pode causar uma falha em uma instância de serviço.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-151">A malformed message, or a task that requires access to resources that aren't available, can cause a service instance to fail.</span></span> <span data-ttu-id="ecc5a-152">O sistema deve impedir que essas mensagens sejam retornadas para a fila, capturando-as em vez disso e armazenando os detalhes dessas mensagens em outro local para que possam ser analisados, se necessário.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-152">The system should prevent such messages being returned to the queue, and instead capture and store the details of these messages elsewhere so that they can be analyzed if necessary.</span></span>

- <span data-ttu-id="ecc5a-153">**Lidar com resultados**.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-153">**Handling results**.</span></span> <span data-ttu-id="ecc5a-154">A instância de serviço que lidar com uma mensagem é totalmente separada da lógica do aplicativo que gera a mensagem e pode não ser capaz de se comunicar diretamente.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-154">The service instance handling a message is fully decoupled from the application logic that generates the message, and they might not be able to communicate directly.</span></span> <span data-ttu-id="ecc5a-155">Se a instância de serviço gerar resultados que devem ser passados para a lógica do aplicativo, essas informações devem ser armazenadas em um local que seja acessível para ambos.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-155">If the service instance generates results that must be passed back to the application logic, this information must be stored in a location that's accessible to both.</span></span> <span data-ttu-id="ecc5a-156">Para impedir que a lógica do aplicativo recupere dados incompletos, o sistema deve indicar quando o processamento é concluído.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-156">In order to prevent the application logic from retrieving incomplete data the system must indicate when processing is complete.</span></span>

     > <span data-ttu-id="ecc5a-157">Se você estiver usando o Azure, um processo de trabalho pode retornar resultados para a lógica do aplicativo por meio de uma fila de resposta de mensagem dedicada.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-157">If you're using Azure, a worker process can pass results back to the application logic by using a dedicated message reply queue.</span></span> <span data-ttu-id="ecc5a-158">A lógica do aplicativo deve ser capaz de correlacionar esses resultados com a mensagem original.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-158">The application logic must be able to correlate these results with the original message.</span></span> <span data-ttu-id="ecc5a-159">Esse cenário é descrito em mais detalhes no [Primer de mensagens assíncronas](https://msdn.microsoft.com/library/dn589781.aspx).</span><span class="sxs-lookup"><span data-stu-id="ecc5a-159">This scenario is described in more detail in the [Asynchronous Messaging Primer](https://msdn.microsoft.com/library/dn589781.aspx).</span></span>

- <span data-ttu-id="ecc5a-160">**Dimensionando o sistema de mensagens**.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-160">**Scaling the messaging system**.</span></span> <span data-ttu-id="ecc5a-161">Em uma solução de grande escala, uma única fila de mensagens poderia ficar sobrecarregada com o número de mensagens e se tornar um gargalo no sistema.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-161">In a large-scale solution, a single message queue could be overwhelmed by the number of messages and become a bottleneck in the system.</span></span> <span data-ttu-id="ecc5a-162">Nessa situação, considere particionar o sistema de mensagens para enviar as mensagens de produtores específicos para determinada fila ou usar o balanceamento de carga para distribuir mensagens em várias filas de mensagens.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-162">In this situation, consider partitioning the messaging system to send messages from specific producers to a particular queue, or use load balancing to distribute messages across multiple message queues.</span></span>

- <span data-ttu-id="ecc5a-163">**Assegurar a confiabilidade do sistema de mensagens**.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-163">**Ensuring reliability of the messaging system**.</span></span> <span data-ttu-id="ecc5a-164">Um sistema de mensagens confiável é necessário para garantir uma mensagem não seja perdida após o aplicativo enfileirá-la.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-164">A reliable messaging system is needed to guarantee that after the application enqueues a message it won't be lost.</span></span> <span data-ttu-id="ecc5a-165">Isso é essencial para assegurar que todas as mensagens sejam entregues pelo menos uma vez.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-165">This is essential for ensuring that all messages are delivered at least once.</span></span>

## <a name="when-to-use-this-pattern"></a><span data-ttu-id="ecc5a-166">Quando usar esse padrão</span><span class="sxs-lookup"><span data-stu-id="ecc5a-166">When to use this pattern</span></span>

<span data-ttu-id="ecc5a-167">Use esse padrão quando:</span><span class="sxs-lookup"><span data-stu-id="ecc5a-167">Use this pattern when:</span></span>

- <span data-ttu-id="ecc5a-168">A carga de trabalho de um aplicativo é dividida em tarefas que podem ser executadas de forma assíncrona.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-168">The workload for an application is divided into tasks that can run asynchronously.</span></span>
- <span data-ttu-id="ecc5a-169">As tarefas são independentes e podem ser executados em paralelo.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-169">Tasks are independent and can run in parallel.</span></span>
- <span data-ttu-id="ecc5a-170">O volume de trabalho é altamente variável, o que exige uma solução escalonável.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-170">The volume of work is highly variable, requiring a scalable solution.</span></span>
- <span data-ttu-id="ecc5a-171">A solução deve fornecer alta disponibilidade e ser resiliente se o processamento de uma tarefa falhar.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-171">The solution must provide high availability, and must be resilient if the processing for a task fails.</span></span>

<span data-ttu-id="ecc5a-172">Esse padrão pode não ser útil se:</span><span class="sxs-lookup"><span data-stu-id="ecc5a-172">This pattern might not be useful when:</span></span>

- <span data-ttu-id="ecc5a-173">Não for fácil separar a carga de trabalho do aplicativo em tarefas discretas ou se há um alto grau de dependência entre as tarefas.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-173">It's not easy to separate the application workload into discrete tasks, or there's a high degree of dependence between tasks.</span></span>
- <span data-ttu-id="ecc5a-174">As tarefas devem ser executadas de forma síncrona e a lógica do aplicativo deve esperar que uma tarefa seja concluída antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-174">Tasks must be performed synchronously, and the application logic must wait for a task to complete before continuing.</span></span>
- <span data-ttu-id="ecc5a-175">As tarefas devem ser executadas em uma sequência específica.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-175">Tasks must be performed in a specific sequence.</span></span>

> <span data-ttu-id="ecc5a-176">Alguns sistemas de mensagens dão suporte a sessões que permitem que um produtor agrupe as mensagens e verifique se elas são manipuladas pelo mesmo consumidor.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-176">Some messaging systems support sessions that enable a producer to group messages together and ensure that they're all handled by the same consumer.</span></span> <span data-ttu-id="ecc5a-177">Esse mecanismo pode ser usado com mensagens priorizadas (se houver suporte para elas) para implementar uma forma de ordenação de mensagens que entrega as mensagens na sequência de um produtor para um único consumidor.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-177">This mechanism can be used with prioritized messages (if they are supported) to implement a form of message ordering that delivers messages in sequence from a producer to a single consumer.</span></span>

## <a name="example"></a><span data-ttu-id="ecc5a-178">Exemplo</span><span class="sxs-lookup"><span data-stu-id="ecc5a-178">Example</span></span>

<span data-ttu-id="ecc5a-179">O Azure fornece filas de armazenamento e filas do Barramento de Serviço que podem atuar como um mecanismo para implementar esse padrão.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-179">Azure provides storage queues and Service Bus queues that can act as a mechanism for implementing this pattern.</span></span> <span data-ttu-id="ecc5a-180">A lógica do aplicativo pode postar mensagens em uma fila e os consumidores implementados como tarefas em uma ou mais funções podem recuperá-las dessa fila e processá-las.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-180">The application logic can post messages to a queue, and consumers implemented as tasks in one or more roles can retrieve messages from this queue and process them.</span></span> <span data-ttu-id="ecc5a-181">Para garantir a resiliência, uma fila do Barramento de Serviço permite que um consumidor use o modo `PeekLock` quando recuperar uma mensagem da fila.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-181">For resiliency, a Service Bus queue enables a consumer to use `PeekLock` mode when it retrieves a message from the queue.</span></span> <span data-ttu-id="ecc5a-182">Na verdade, esse modo não remove a mensagem, mas simplesmente a oculta de outros consumidores.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-182">This mode doesn't actually remove the message, but simply hides it from other consumers.</span></span> <span data-ttu-id="ecc5a-183">O consumidor original poderá excluí-la quando tiver terminado de processá-la.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-183">The original consumer can delete the message when it's finished processing it.</span></span> <span data-ttu-id="ecc5a-184">Se o consumidor falhar, o bloqueio de inspeção atingirá o tempo limite e a mensagem se tornará visível novamente, permitindo que outro consumidor possa recuperá-la.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-184">If the consumer fails, the peek lock will time out and the message will become visible again, allowing another consumer to retrieve it.</span></span>

> <span data-ttu-id="ecc5a-185">Para ver informações detalhadas sobre como usar filas do Barramento de Serviço do Azure, consulte [Filas, tópicos e assinaturas do Barramento de Serviço](https://msdn.microsoft.com/library/windowsazure/hh367516.aspx).</span><span class="sxs-lookup"><span data-stu-id="ecc5a-185">For detailed information on using Azure Service Bus queues, see [Service Bus queues, topics, and subscriptions](https://msdn.microsoft.com/library/windowsazure/hh367516.aspx).</span></span>
<span data-ttu-id="ecc5a-186">Para obter mais informações sobre como usar filas do armazenamento do Azure, consulte [Introdução ao Armazenamento de Filas do Azure usando o .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-queues/).</span><span class="sxs-lookup"><span data-stu-id="ecc5a-186">For information on using Azure storage queues, see [Get started with Azure Queue storage using .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-queues/).</span></span>

<span data-ttu-id="ecc5a-187">O código a seguir da classe `QueueManager` na solução CompetingConsumers disponível no [GitHub](https://github.com/mspnp/cloud-design-patterns/tree/master/competing-consumers) mostra como criar uma fila usando uma instância `QueueClient` no manipulador de eventos `Start` em uma função Web ou de trabalho.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-187">The following code from the `QueueManager` class in CompetingConsumers solution available on [GitHub](https://github.com/mspnp/cloud-design-patterns/tree/master/competing-consumers) shows how you can create a queue by using a `QueueClient` instance in the `Start` event handler in a web or worker role.</span></span>

```csharp
private string queueName = ...;
private string connectionString = ...;
...

public async Task Start()
{
  // Check if the queue already exists.
  var manager = NamespaceManager.CreateFromConnectionString(this.connectionString);
  if (!manager.QueueExists(this.queueName))
  {
    var queueDescription = new QueueDescription(this.queueName);

    // Set the maximum delivery count for messages in the queue. A message
    // is automatically dead-lettered after this number of deliveries. The
    // default value for dead letter count is 10.
    queueDescription.MaxDeliveryCount = 3;

    await manager.CreateQueueAsync(queueDescription);
  }
  ...

  // Create the queue client. By default the PeekLock method is used.
  this.client = QueueClient.CreateFromConnectionString(
    this.connectionString, this.queueName);
}
```

<span data-ttu-id="ecc5a-188">O seguinte trecho de código mostra como um aplicativo pode criar e enviar um lote de mensagens para a fila.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-188">The next code snippet shows how an application can create and send a batch of messages to the queue.</span></span>

```csharp
public async Task SendMessagesAsync()
{
  // Simulate sending a batch of messages to the queue.
  var messages = new List<BrokeredMessage>();

  for (int i = 0; i < 10; i++)
  {
    var message = new BrokeredMessage() { MessageId = Guid.NewGuid().ToString() };
    messages.Add(message);
  }
  await this.client.SendBatchAsync(messages);
}
```

<span data-ttu-id="ecc5a-189">O código a seguir mostra como uma instância de serviço do consumidor pode receber mensagens da fila, seguindo uma abordagem controlada por evento.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-189">The following code shows how a consumer service instance can receive messages from the queue by following an event-driven approach.</span></span> <span data-ttu-id="ecc5a-190">O parâmetro `processMessageTask` para o método `ReceiveMessages` é um delegado que referencia o código a ser executado quando uma mensagem é recebida.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-190">The `processMessageTask` parameter to the `ReceiveMessages` method is a delegate that references the code to run when a message is received.</span></span> <span data-ttu-id="ecc5a-191">Esse código é executado de forma assíncrona.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-191">This code is run asynchronously.</span></span>

```csharp
private ManualResetEvent pauseProcessingEvent;
...

public void ReceiveMessages(Func<BrokeredMessage, Task> processMessageTask)
{
  // Set up the options for the message pump.
  var options = new OnMessageOptions();

  // When AutoComplete is disabled it's necessary to manually
  // complete or abandon the messages and handle any errors.
  options.AutoComplete = false;
  options.MaxConcurrentCalls = 10;
  options.ExceptionReceived += this.OptionsOnExceptionReceived;

  // Use of the Service Bus OnMessage message pump.
  // The OnMessage method must be called once, otherwise an exception will occur.
  this.client.OnMessageAsync(
    async (msg) =>
    {
      // Will block the current thread if Stop is called.
      this.pauseProcessingEvent.WaitOne();

      // Execute processing task here.
      await processMessageTask(msg);
    },
    options);
}
...

private void OptionsOnExceptionReceived(object sender,
  ExceptionReceivedEventArgs exceptionReceivedEventArgs)
{
  ...
}
```

<span data-ttu-id="ecc5a-192">Observe que os recursos de dimensionamento automático, como aqueles disponíveis no Azure, podem ser usados para iniciar e parar instâncias de função, à medida que o tamanho da fila varia.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-192">Note that autoscaling features, such as those available in Azure, can be used to start and stop role instances as the queue length fluctuates.</span></span> <span data-ttu-id="ecc5a-193">Para saber mais, consulte as [Diretrizes de dimensionamento automático](https://msdn.microsoft.com/library/dn589774.aspx).</span><span class="sxs-lookup"><span data-stu-id="ecc5a-193">For more information, see [Autoscaling Guidance](https://msdn.microsoft.com/library/dn589774.aspx).</span></span> <span data-ttu-id="ecc5a-194">Além disso, não é necessário manter uma correspondência um para um entre as instâncias de função e os processos de trabalho &mdash; uma única instância de função pode implementar vários processos de trabalho.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-194">Also, it's not necessary to maintain a one-to-one correspondence between role instances and worker processes&mdash;a single role instance can implement multiple worker processes.</span></span> <span data-ttu-id="ecc5a-195">Para obter mais informações, consulte [Padrão de consolidação de recursos de computação](compute-resource-consolidation.md).</span><span class="sxs-lookup"><span data-stu-id="ecc5a-195">For more information, see [Compute Resource Consolidation pattern](compute-resource-consolidation.md).</span></span>

## <a name="related-patterns-and-guidance"></a><span data-ttu-id="ecc5a-196">Diretrizes e padrões relacionados</span><span class="sxs-lookup"><span data-stu-id="ecc5a-196">Related patterns and guidance</span></span>

<span data-ttu-id="ecc5a-197">Os padrões e diretrizes a seguir também podem ser relevantes ao implementar esse padrão:</span><span class="sxs-lookup"><span data-stu-id="ecc5a-197">The following patterns and guidance might be relevant when implementing this pattern:</span></span>

- <span data-ttu-id="ecc5a-198">[Prévia de mensagens assíncronas](https://msdn.microsoft.com/library/dn589781.aspx).</span><span class="sxs-lookup"><span data-stu-id="ecc5a-198">[Asynchronous Messaging Primer](https://msdn.microsoft.com/library/dn589781.aspx).</span></span> <span data-ttu-id="ecc5a-199">Filas de mensagens são um mecanismo de comunicação assíncrona.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-199">Message queues are an asynchronous communications mechanism.</span></span> <span data-ttu-id="ecc5a-200">Se um serviço do consumidor precisa enviar uma resposta para um aplicativo, pode ser necessário implementar alguma forma de mensagens de resposta.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-200">If a consumer service needs to send a reply to an application, it might be necessary to implement some form of response messaging.</span></span> <span data-ttu-id="ecc5a-201">O Primer de mensagens assíncronas fornece informações sobre como implementar mensagens de solicitação/resposta usando filas de mensagens.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-201">The Asynchronous Messaging Primer provides information on how to implement request/reply messaging using message queues.</span></span>

- <span data-ttu-id="ecc5a-202">[Diretrizes de dimensionamento automático](https://msdn.microsoft.com/library/dn589774.aspx).</span><span class="sxs-lookup"><span data-stu-id="ecc5a-202">[Autoscaling Guidance](https://msdn.microsoft.com/library/dn589774.aspx).</span></span> <span data-ttu-id="ecc5a-203">Pode ser possível iniciar e parar instâncias de um serviço do consumidor, visto que o tamanho da fila na qual os aplicativos postam as mensagens varia.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-203">It might be possible to start and stop instances of a consumer service since the length of the queue applications post messages on varies.</span></span> <span data-ttu-id="ecc5a-204">O dimensionamento automático pode ajudar a manter a taxa de transferência durante horários de pico de processamento.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-204">Autoscaling can help to maintain throughput during times of peak processing.</span></span>

- <span data-ttu-id="ecc5a-205">[Padrão de consolidação de recursos de computação](compute-resource-consolidation.md).</span><span class="sxs-lookup"><span data-stu-id="ecc5a-205">[Compute Resource Consolidation Pattern](compute-resource-consolidation.md).</span></span> <span data-ttu-id="ecc5a-206">Pode ser possível consolidar várias instâncias de um serviço do consumidor em um único processo para reduzir os custos e a sobrecarga de gerenciamento.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-206">It might be possible to consolidate multiple instances of a consumer service into a single process to reduce costs and management overhead.</span></span> <span data-ttu-id="ecc5a-207">O Padrão de consolidação de recursos de computação descreve os benefícios e as vantagens e desvantagens de seguir essa abordagem.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-207">The Compute Resource Consolidation pattern describes the benefits and tradeoffs of following this approach.</span></span>

- <span data-ttu-id="ecc5a-208">[Padrão de nivelamento de carga baseado em fila](queue-based-load-leveling.md).</span><span class="sxs-lookup"><span data-stu-id="ecc5a-208">[Queue-based Load Leveling Pattern](queue-based-load-leveling.md).</span></span> <span data-ttu-id="ecc5a-209">Apresentar uma fila de mensagens pode agregar resiliência ao sistema, permitindo que as instâncias de serviço lidem com volumes variáveis de solicitações de instâncias do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-209">Introducing a message queue can add resiliency to the system, enabling service instances to handle widely varying volumes of requests from application instances.</span></span> <span data-ttu-id="ecc5a-210">A fila de mensagens atua como um buffer que nivela a carga.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-210">The message queue acts as a buffer, which levels the load.</span></span> <span data-ttu-id="ecc5a-211">O Padrão de nivelamento de carga com base em fila descreve esse cenário mais detalhadamente.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-211">The Queue-based Load Leveling pattern describes this scenario in more detail.</span></span>

- <span data-ttu-id="ecc5a-212">Esse padrão tem um [aplicativo de exemplo](https://github.com/mspnp/cloud-design-patterns/tree/master/competing-consumers) associado a ele.</span><span class="sxs-lookup"><span data-stu-id="ecc5a-212">This pattern has a [sample application](https://github.com/mspnp/cloud-design-patterns/tree/master/competing-consumers) associated with it.</span></span>