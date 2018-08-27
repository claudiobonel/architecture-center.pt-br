---
title: Guia de design da governança do Azure
description: Orientação para configurar os controles de governança do Azure para habilitar um usuário e implantar uma carga de trabalho simples
author: petertay
ms.openlocfilehash: 78545400fc0b09262dce3d0e577442443468b2ed
ms.sourcegitcommit: c704d5d51c8f9bbab26465941ddcf267040a8459
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2018
ms.locfileid: "39229423"
---
# <a name="azure-governance-design-guide"></a><span data-ttu-id="5228b-103">Guia de design da governança do Azure</span><span class="sxs-lookup"><span data-stu-id="5228b-103">Azure governance design guide</span></span>

<span data-ttu-id="5228b-104">A audiência para este guia de design é a persona da *TI central* em sua organização.</span><span class="sxs-lookup"><span data-stu-id="5228b-104">The audience for this design guide is the *central IT* persona in your organization.</span></span> <span data-ttu-id="5228b-105">A *TI central* é responsável por projetar e implementar a arquitetura de governança de nuvem da sua organização.</span><span class="sxs-lookup"><span data-stu-id="5228b-105">*Central IT* is responsible for designing and implementing your organization's cloud governance architecture.</span></span> <span data-ttu-id="5228b-106">Como você aprendeu no explicador [o que é governança de recursos de nuvem?](governance-explainer.md), governança é o processo contínuo de gerenciar, monitorar e auditar o uso de recursos do Azure para atender as metas e requisitos da sua organização.</span><span class="sxs-lookup"><span data-stu-id="5228b-106">As you learned in the [what is cloud resource governance?](governance-explainer.md) explainer, governance refers to the ongoing process of managing, monitoring, and auditing the use of Azure resources to meet the goals and requirements of your organization.</span></span>

<span data-ttu-id="5228b-107">O objetivo deste guia é ajudar você a aprender o processo de criação da arquitetura de governança de sua organização, observando um conjunto de requisitos e objetivos de governança hipotético.</span><span class="sxs-lookup"><span data-stu-id="5228b-107">The goal of this guidance is to help you learn the process of designing your organization's governance architecture by looking at a set of hypothetical governance goals and requirements.</span></span> <span data-ttu-id="5228b-108">Em seguida, discutiremos como configurar as ferramentas de governança do Azure para atendê-las.</span><span class="sxs-lookup"><span data-stu-id="5228b-108">Then, we'll discuss how to configure Azure's governance tools to meet them.</span></span> 

<span data-ttu-id="5228b-109">No estágio básico de adoção, nossa meta é implantar uma carga de trabalho simples no Azure.</span><span class="sxs-lookup"><span data-stu-id="5228b-109">In the foundational adoption stage, our goal is to deploy a simple workload to Azure.</span></span> <span data-ttu-id="5228b-110">Isso resulta nos seguintes requisitos:</span><span class="sxs-lookup"><span data-stu-id="5228b-110">This results in the following requirements:</span></span>
* <span data-ttu-id="5228b-111">Gerenciamento de identidade para um único **proprietário de carga de trabalho**, que é responsável por implantar e manter a carga de trabalho simples.</span><span class="sxs-lookup"><span data-stu-id="5228b-111">Identity management for a single **workload owner** who is responsible for deploying and maintaining the simple workload.</span></span> <span data-ttu-id="5228b-112">O proprietário da carga de trabalho exige permissão para criar, ler, atualizar e excluir recursos, bem como permissão para delegar esses direitos a outros usuários no sistema de gerenciamento de identidade.</span><span class="sxs-lookup"><span data-stu-id="5228b-112">The workload owner requires permission to create, read, update, and delete resources as well as permission to delegate these rights to other users in the identity management system.</span></span>
* <span data-ttu-id="5228b-113">Gerencie todos os recursos para a carga de trabalho simples como uma unidade única de gerenciamento.</span><span class="sxs-lookup"><span data-stu-id="5228b-113">Manage all resources for the simple workload as a single management unit.</span></span>

## <a name="licensing-azure"></a><span data-ttu-id="5228b-114">Licenciamento do Azure</span><span class="sxs-lookup"><span data-stu-id="5228b-114">Licensing Azure</span></span>

<span data-ttu-id="5228b-115">Antes de começarmos a criação de nosso modelo de governança, é importante entender como o Azure é licenciado.</span><span class="sxs-lookup"><span data-stu-id="5228b-115">Before we begin designing our governance model, it's important to understand how Azure is licensed.</span></span> <span data-ttu-id="5228b-116">Isso ocorre porque as contas administrativas associadas à sua licença do Azure têm o maior nível de acesso para todos os seus recursos do Azure.</span><span class="sxs-lookup"><span data-stu-id="5228b-116">This is because the administrative accounts associated with your Azure license have the highest level of access to all of your Azure resources.</span></span> <span data-ttu-id="5228b-117">Essas contas administrativas formam a base do seu modelo de governança.</span><span class="sxs-lookup"><span data-stu-id="5228b-117">These administrative accounts form the basis of your governance model.</span></span>  

> [!NOTE]
> <span data-ttu-id="5228b-118">Se sua organização tiver um [Microsoft Enterprise Agreement](https://www.microsoft.com/licensing/licensing-programs/enterprise.aspx) existente que não inclui o Azure, este pode ser adicionado mediante uma confirmação de pagamento adiantado.</span><span class="sxs-lookup"><span data-stu-id="5228b-118">If your organization has an existing [Microsoft Enterprise Agreement](https://www.microsoft.com/licensing/licensing-programs/enterprise.aspx) that does not include Azure, Azure can be added by making an upfront monetary commitment.</span></span> <span data-ttu-id="5228b-119">Consulte [licenciamento do Azure para a empresa](https://azure.microsoft.com/pricing/enterprise-agreement/) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="5228b-119">See [licensing Azure for the enterprise](https://azure.microsoft.com/pricing/enterprise-agreement/) for more information.</span></span> 

<span data-ttu-id="5228b-120">Quando o Azure é adicionado ao Enterprise Agreement de sua organização, ela deve criar uma **conta do Azure**.</span><span class="sxs-lookup"><span data-stu-id="5228b-120">When Azure added to your organization's Enterprise Agreement, your organization was prompted to create an **Azure account**.</span></span> <span data-ttu-id="5228b-121">Durante o processo de criação da conta, um **proprietário da conta do Azure** foi criado, bem como um locatário do Azure Active Directory (Azure AD) com uma conta de **administrador global**.</span><span class="sxs-lookup"><span data-stu-id="5228b-121">During the account creation process, an **Azure account owner** was created, as well as an Azure Active Directory (Azure AD) tenant with a **global administrator** account.</span></span> <span data-ttu-id="5228b-122">Um locatário do Azure AD é um constructo lógico que representa uma instância segura e dedicada do Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5228b-122">An Azure AD tenant is a logical construct that represents a secure, dedicated instance of Azure AD.</span></span>

<span data-ttu-id="5228b-123">![Conta do Azure com o Gerente de Contas do Azure e o administrador global do Azure AD](../_images/governance-3-0.png)
*Figura 1. Uma conta do Azure com um gerente de conta e o Administrador Global do Azure AD.*</span><span class="sxs-lookup"><span data-stu-id="5228b-123">![Azure account with Azure Account Manager and Azure AD global administrator](../_images/governance-3-0.png)
*Figure 1. An Azure account with an Account Manager and Azure AD Global Administrator.*</span></span>

## <a name="identity-management"></a><span data-ttu-id="5228b-124">Gerenciamento de identidades</span><span class="sxs-lookup"><span data-stu-id="5228b-124">Identity management</span></span>

<span data-ttu-id="5228b-125">Somente o Azure confia no [Azure AD](/azure/active-directory) para autenticar usuários e autorizar o acesso de usuários a recursos, por isso, o Azure AD é o nosso sistema de gerenciamento de identidade.</span><span class="sxs-lookup"><span data-stu-id="5228b-125">Azure only trusts [Azure AD](/azure/active-directory) to authenticate users and authorize user access to resources, so Azure AD is our identity management system.</span></span> <span data-ttu-id="5228b-126">O administrador global do Azure AD tem o nível mais alto de permissões e pode executar todas as ações relacionadas à identidade, incluindo a criação de usuários e a atribuição de permissões.</span><span class="sxs-lookup"><span data-stu-id="5228b-126">The Azure AD global administrator has the highest level of permissions and can perform all actions related to identity, including creating users and assigning permissions.</span></span> 

<span data-ttu-id="5228b-127">Nosso requisito é o gerenciamento de identidade para um único **proprietário de carga de trabalho**, que é responsável por implantar e manter a carga de trabalho simples.</span><span class="sxs-lookup"><span data-stu-id="5228b-127">Our requirement is identity management for a single **workload owner** who is responsible for deploying and maintaining the simple workload.</span></span> <span data-ttu-id="5228b-128">O proprietário da carga de trabalho exige permissão para criar, ler, atualizar e excluir recursos, bem como permissão para delegar esses direitos a outros usuários no sistema de gerenciamento de identidade.</span><span class="sxs-lookup"><span data-stu-id="5228b-128">The workload owner requires permission to create, read, update, and delete resources as well as permission to delegate these rights to other users in the identity management system.</span></span>

<span data-ttu-id="5228b-129">Nosso administrador global do Azure Active Directory criará o **proprietário da carga de trabalho** da conta para o **proprietário da carga de trabalho**:</span><span class="sxs-lookup"><span data-stu-id="5228b-129">Our Azure AD global administrator will create the **workload owner** account for the **workload owner**:</span></span>

<span data-ttu-id="5228b-130">![O administrador global do Azure Active Directory cria a conta de proprietário da carga de trabalho](../_images/governance-1-2.png)
*Figura 2. O administrador global do Azure Active Directory cria a conta de usuário da carga de trabalho*</span><span class="sxs-lookup"><span data-stu-id="5228b-130">![The Azure AD global administrator creates the workload owner account](../_images/governance-1-2.png)
*Figure 2. The Azure AD global administrator creates the workload owner user account.*</span></span>

<span data-ttu-id="5228b-131">Não conseguimos atribuir permissão de acesso ao recurso até que esse usuário seja adicionado a uma **assinatura**, portanto, faremos isso nas próximas duas seções.</span><span class="sxs-lookup"><span data-stu-id="5228b-131">We aren't able to assign resource access permission until this user is added to a **subscription**, so we'll do that in the next two sections.</span></span> 

## <a name="resource-management-scope"></a><span data-ttu-id="5228b-132">Escopo do gerenciamento de recursos</span><span class="sxs-lookup"><span data-stu-id="5228b-132">Resource management scope</span></span>

<span data-ttu-id="5228b-133">À medida que o número de recursos implantados por sua organização cresce, a complexidade de governar esses recursos também aumenta.</span><span class="sxs-lookup"><span data-stu-id="5228b-133">As the number of resources deployed by your organization grows, the complexity of governing those resources grows as well.</span></span> <span data-ttu-id="5228b-134">O Azure implementa uma hierarquia de contêiner lógico para permitir que sua organização gerencie seus recursos em grupos em vários níveis de granularidade, o que também é conhecido como **escopo**.</span><span class="sxs-lookup"><span data-stu-id="5228b-134">Azure implements a logical container hierarchy to enable your organization to manage your resources in groups at various levels of granularity, also known as **scope**.</span></span> 

<span data-ttu-id="5228b-135">O nível superior do escopo de gerenciamento de recursos é o nível de **assinatura**.</span><span class="sxs-lookup"><span data-stu-id="5228b-135">The top level of resource management scope is the **subscription** level.</span></span> <span data-ttu-id="5228b-136">Uma assinatura é criada pelo **proprietário da conta** do Azure, que estabelece o compromisso financeiro e é responsável pelo pagamento de todos os recursos do Azure associado à assinatura:</span><span class="sxs-lookup"><span data-stu-id="5228b-136">A subscription is created by the Azure **account owner**, who establishes the financial commitment and is responsible for paying for all Azure resources associated with the subscription:</span></span>

<span data-ttu-id="5228b-137">![O proprietário da conta do Azure cria uma assinatura](../_images/governance-1-3.png)
*Figura 3. O proprietário da conta do Azure cria uma assinatura.*</span><span class="sxs-lookup"><span data-stu-id="5228b-137">![The Azure account owner creates a subscription](../_images/governance-1-3.png)
*Figure 3. The Azure account owner creates a subscription.*</span></span>

<span data-ttu-id="5228b-138">Quando a assinatura é criada, o **proprietário da conta** do Azure associa um locatário do Azure Active Directory com a assinatura e este locatário do Azure AD é usado para autenticar e autorizar usuários:</span><span class="sxs-lookup"><span data-stu-id="5228b-138">When the subscription is created, the Azure **account owner** associates an Azure AD tenant with the subscription, and this Azure AD tenant is used for authenticating and authorizing users:</span></span>

<span data-ttu-id="5228b-139">![O proprietário da conta do Azure associa o locatário do Azure AD com a assinatura](../_images/governance-1-4.png)
*Figura 4. O proprietário da conta do Azure associa o locatário do Azure AD com a assinatura.*</span><span class="sxs-lookup"><span data-stu-id="5228b-139">![The Azure account owner associates the Azure AD tenant with the subscription](../_images/governance-1-4.png)
*Figure 4. The Azure account owner associates the Azure AD tenant with the subscription.*</span></span>

<span data-ttu-id="5228b-140">Você talvez tenha observado que não há, no momento, nenhum usuário associado à assinatura, o que significa que ninguém tem permissão para gerenciar recursos.</span><span class="sxs-lookup"><span data-stu-id="5228b-140">You may have noticed that there is currently no user associated with the subscription, which means that no one has permission to manage resources.</span></span> <span data-ttu-id="5228b-141">Na realidade, o **proprietário da conta** é o proprietário da assinatura e tem permissão para executar qualquer ação em um recurso na assinatura.</span><span class="sxs-lookup"><span data-stu-id="5228b-141">In reality, the **account owner** is the owner of the subscription and has permission to take any action on a resource in the subscription.</span></span> <span data-ttu-id="5228b-142">No entanto, em termos práticos, o **proprietário da conta** é muito provavelmente uma pessoa do departamento financeiro de sua organização e não é responsável por criar, ler, atualizar e excluir recursos - essas tarefas serão executadas pelo **proprietário da carga de trabalho**.</span><span class="sxs-lookup"><span data-stu-id="5228b-142">However, in practical terms the **account owner** is more than likely a finance person in your organization and is not responsible for creating, reading, updating, and deleting resources - those tasks will be performed by the **workload owner**.</span></span> <span data-ttu-id="5228b-143">Portanto, precisamos adicionar o **proprietário da carga de trabalho** à assinatura e atribuir permissões.</span><span class="sxs-lookup"><span data-stu-id="5228b-143">Therefore, we need to add the **workload owner** to the subscription and assign permissions.</span></span>

<span data-ttu-id="5228b-144">Uma vez que o **proprietário da conta** atualmente é o único usuário com permissão para adicionar o **proprietário da carga de trabalho** à assinatura, eles adicionam o **proprietário da carga de trabalho** à assinatura:</span><span class="sxs-lookup"><span data-stu-id="5228b-144">Since the **account owner** is currently the only user with permission to add the **workload owner** to the subscription, they add the **workload owner** to the subscription:</span></span>

<span data-ttu-id="5228b-145">![O proprietário da conta do Azure adiciona o **proprietário da carga de trabalho** à assinatura](../_images/governance-1-5.png)
*Figura 5. O proprietário da conta do Azure adiciona o proprietário da carga de trabalho à assinatura.*</span><span class="sxs-lookup"><span data-stu-id="5228b-145">![The Azure account owner adds the **workload owner** to the subscription](../_images/governance-1-5.png)
*Figure 5. The Azure account owner adds the workload owner to the subscription.*</span></span>

<span data-ttu-id="5228b-146">O **proprietário da conta** do Azure concede permissões ao **proprietário da carga de trabalho** atribuindo uma função de [controle de acesso baseado em função (RBAC)](/azure/role-based-access-control/).</span><span class="sxs-lookup"><span data-stu-id="5228b-146">The Azure **account owner** grants permissions to the **workload owner** by assigning a [role-based access control (RBAC)](/azure/role-based-access-control/) role.</span></span> <span data-ttu-id="5228b-147">A função RBAC especifica um conjunto de permissões que o **proprietário da carga de trabalho** tem para um tipo de recurso individual ou um conjunto de tipos de recursos.</span><span class="sxs-lookup"><span data-stu-id="5228b-147">The RBAC role specifies a set of permissions that the **workload owner** has for an individual resource type or a set of resource types.</span></span>

<span data-ttu-id="5228b-148">Observe que neste exemplo, o **proprietário da conta** atribuiu a [função de **proprietário** interna](/azure/role-based-access-control/built-in-roles#owner):</span><span class="sxs-lookup"><span data-stu-id="5228b-148">Notice that in this example, the **account owner** has assigned the [built-in **owner** role](/azure/role-based-access-control/built-in-roles#owner):</span></span> 

<span data-ttu-id="5228b-149">![O **proprietário da carga de trabalho** foi atribuído à função de proprietário interna](../_images/governance-1-6.png)
*Figura 6. O proprietário da carga de trabalho foi atribuído à função de proprietário interna.*</span><span class="sxs-lookup"><span data-stu-id="5228b-149">![The **workload owner** was assigned the built-in owner role](../_images/governance-1-6.png)
*Figure 6. The workload owner was assigned the built-in owner role.*</span></span>

<span data-ttu-id="5228b-150">A função de **proprietário** interna concede todas as permissões ao **proprietário da carga de trabalho** no escopo da assinatura.</span><span class="sxs-lookup"><span data-stu-id="5228b-150">The built-in **owner** role grants all permissions to the **workload owner** at the subscription scope.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="5228b-151">O **proprietário de conta** do Azure é responsável pelo compromisso financeiro associado à assinatura, mas o **proprietário da carga de trabalho** tem as mesmas permissões.</span><span class="sxs-lookup"><span data-stu-id="5228b-151">The Azure **acount owner** is responsible for the financial committment associated with the subscription, but the **workload owner** has the same permissions.</span></span> <span data-ttu-id="5228b-152">O **proprietário da conta** deve confiar no **proprietário da carga de trabalho** para implantar recursos que estão dentro do orçamento da assinatura.</span><span class="sxs-lookup"><span data-stu-id="5228b-152">The **account owner** must trust the **workload owner** to deploy resources that are within the subscription budget.</span></span>

<span data-ttu-id="5228b-153">O próximo nível do escopo de gerenciamento é o nível do **grupo de recursos**.</span><span class="sxs-lookup"><span data-stu-id="5228b-153">The next level of management scope is the **resource group** level.</span></span> <span data-ttu-id="5228b-154">Um grupo de recursos é um contêiner lógico para recursos.</span><span class="sxs-lookup"><span data-stu-id="5228b-154">A resource group is a logical container for resources.</span></span> <span data-ttu-id="5228b-155">Operações aplicadas no nível do grupo de recursos se aplicam a todos os recursos em um grupo.</span><span class="sxs-lookup"><span data-stu-id="5228b-155">Operations applied at the resource group level apply to all resources in a group.</span></span> <span data-ttu-id="5228b-156">Além disso, é importante observar que as permissões para cada usuário são herdadas do próximo nível para cima, a menos que elas sejam alteradas explicitamente nesse escopo.</span><span class="sxs-lookup"><span data-stu-id="5228b-156">Also, it's important to note that permissions for each user are inherited from the next level up unless they are explicitly changed at that scope.</span></span> 

<span data-ttu-id="5228b-157">Para ilustrar isso, vamos examinar o que acontece quando o **proprietário da carga de trabalho** cria um grupo de recursos:</span><span class="sxs-lookup"><span data-stu-id="5228b-157">To illustrate this, let's look at what happens when the **workload owner** creates a resource group:</span></span>

<span data-ttu-id="5228b-158">![O **proprietário da carga de trabalho** cria um grupo de recursos](../_images/governance-1-7.png)
*Figura 7. O proprietário da carga de trabalho cria um grupo de recursos e herda a função de proprietário interna no escopo do grupo de recursos.*</span><span class="sxs-lookup"><span data-stu-id="5228b-158">![The **workload owner** creates a resource group](../_images/governance-1-7.png)
*Figure 7. The workload owner creates a resource group and inherits the built-in owner role at the resource group scope.*</span></span>

<span data-ttu-id="5228b-159">Novamente, a função de **proprietário** interna concede todas as permissões ao **proprietário da carga de trabalho** no escopo do grupo de recursos.</span><span class="sxs-lookup"><span data-stu-id="5228b-159">Again, the built-in **owner** role grants all permissions to the **workload owner** at the resource group scope.</span></span> <span data-ttu-id="5228b-160">Conforme discutido anteriormente, essa função é herdada do nível da assinatura.</span><span class="sxs-lookup"><span data-stu-id="5228b-160">As we discussed earlier, this role is inherited from the subscription level.</span></span> <span data-ttu-id="5228b-161">Se uma função diferente for atribuída a esse usuário nesse escopo, ela se aplicará somente a esse escopo.</span><span class="sxs-lookup"><span data-stu-id="5228b-161">If a different role is assigned to this user at this scope, it applies to this scope only.</span></span>

<span data-ttu-id="5228b-162">O menor nível do escopo de gerenciamento é o nível de **recurso**.</span><span class="sxs-lookup"><span data-stu-id="5228b-162">The lowest level of management scope is at the **resource** level.</span></span> <span data-ttu-id="5228b-163">Operações aplicadas ao nível de recurso se aplicam somente ao próprio recurso.</span><span class="sxs-lookup"><span data-stu-id="5228b-163">Operations applied at the resource level apply only to the resource itself.</span></span> <span data-ttu-id="5228b-164">E, mais uma vez, as permissões no nível de recurso são herdadas do escopo do grupo de recursos.</span><span class="sxs-lookup"><span data-stu-id="5228b-164">And once again, permissions at the resource level are inherited from resource group scope.</span></span> <span data-ttu-id="5228b-165">Por exemplo, vejamos o que acontece se o **proprietário da carga de trabalho** implantar uma [rede virtual](/azure/virtual-network/virtual-networks-overview) no grupo de recursos:</span><span class="sxs-lookup"><span data-stu-id="5228b-165">For example, let's look at what happens if the **workload owner** deploys a [virtual network](/azure/virtual-network/virtual-networks-overview) into the resource group:</span></span>

<span data-ttu-id="5228b-166">![O **proprietário da carga de trabalho** cria um recurso](../_images/governance-1-8.png)
*Figura 8. O proprietário da carga de trabalho cria um recurso e herda a função de proprietário interna no escopo do recurso.*</span><span class="sxs-lookup"><span data-stu-id="5228b-166">![The **workload owner** creates a resource](../_images/governance-1-8.png)
*Figure 8. The workload owner creates a resource and inherits the built-in owner role at the resource scope.*</span></span>

<span data-ttu-id="5228b-167">O **proprietário da carga de trabalho** herda a função de proprietário no escopo do recurso, o que significa que o proprietário da carga de trabalho tem todas as permissões para a rede virtual.</span><span class="sxs-lookup"><span data-stu-id="5228b-167">The **workload owner** inherits the owner role at the resource scope, which means the workload owner has all permissions for the virtual network.</span></span> 

## <a name="summary"></a><span data-ttu-id="5228b-168">Resumo</span><span class="sxs-lookup"><span data-stu-id="5228b-168">Summary</span></span>

<span data-ttu-id="5228b-169">Neste artigo, você aprendeu que:</span><span class="sxs-lookup"><span data-stu-id="5228b-169">In this article, you learned:</span></span>

* <span data-ttu-id="5228b-170">Somente o Azure confia no Azure Active Directory para o gerenciamento de identidades.</span><span class="sxs-lookup"><span data-stu-id="5228b-170">Azure only trusts Azure AD for identity management.</span></span>
* <span data-ttu-id="5228b-171">Uma assinatura tem o escopo mais alto de gerenciamento de recursos, e cada assinatura é associada a um locatário do Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="5228b-171">A subscription has the highest scope of resource management, and each subscription is associated with an Azure AD tenant.</span></span> <span data-ttu-id="5228b-172">Somente os usuários no locatário do Azure Active Directory associado podem acessar recursos na assinatura.</span><span class="sxs-lookup"><span data-stu-id="5228b-172">Only users in the associated Azure AD tenant can access resources in the subscription.</span></span>
* <span data-ttu-id="5228b-173">Há três níveis de escopo de gerenciamento de recursos: assinatura, grupo de recursos e recurso.</span><span class="sxs-lookup"><span data-stu-id="5228b-173">There are three levels of resource management scope: subscription, resource group, and resource.</span></span> <span data-ttu-id="5228b-174">As permissões são atribuídas em cada escopo usando funções RBAC.</span><span class="sxs-lookup"><span data-stu-id="5228b-174">Permissions are assigned at each scope using RBAC roles.</span></span> <span data-ttu-id="5228b-175">As funções RBAC são herdadas de um escopo superior para reduzir o escopo.</span><span class="sxs-lookup"><span data-stu-id="5228b-175">RBAC roles are inherited from higher scope to lower scope.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5228b-176">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="5228b-176">Next steps</span></span>

<span data-ttu-id="5228b-177">Volte para a [visão geral do estágio básico de adoção](overview.md) para saber como implementar esse modelo governança.</span><span class="sxs-lookup"><span data-stu-id="5228b-177">Return to the [foundational adoption stage overview](overview.md) to learn how to implement this goverance model.</span></span> <span data-ttu-id="5228b-178">Em seguida, selecione um tipo de carga de trabalho e saiba como implantá-lo.</span><span class="sxs-lookup"><span data-stu-id="5228b-178">Then, select a type of workload and learn how to deploy it.</span></span>