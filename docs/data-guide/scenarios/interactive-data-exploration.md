---
title: "Exploração interativa de dados"
description: 
author: zoinerTejada
ms:date: 02/12/2018
ms.openlocfilehash: a9e72f4cf88c9082fe79f854dd79e98bfaa918f5
ms.sourcegitcommit: 90cf2de795e50571d597cfcb9b302e48933e7f18
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/14/2018
---
# <a name="interactive-data-exploration"></a><span data-ttu-id="89aa0-102">Exploração interativa de dados</span><span class="sxs-lookup"><span data-stu-id="89aa0-102">Interactive data exploration</span></span>

<span data-ttu-id="89aa0-103">Em muitas soluções corporativas de BI (business intelligence), os relatórios e modelos semânticos são criados por especialistas em BI e gerenciados de forma centralizada.</span><span class="sxs-lookup"><span data-stu-id="89aa0-103">In many corporate business intelligence (BI) solutions, reports and semantic models are created by BI specialists and managed centrally.</span></span> <span data-ttu-id="89aa0-104">No entanto, cada vez mais as organizações desejam permitir que os usuários tomem decisões controladas por dados.</span><span class="sxs-lookup"><span data-stu-id="89aa0-104">Increasingly, however, organizations want to enable users to make data-driven decisions.</span></span> <span data-ttu-id="89aa0-105">Além disso, um número cada vez maior de organizações está contratando *cientistas de dados* ou *analistas de dados*, cujo trabalho é explorar os dados de forma interativa e aplicar modelos estatísticos e técnicas analíticas para encontrar tendências e padrões nos dados.</span><span class="sxs-lookup"><span data-stu-id="89aa0-105">Additionally, a growing number of organizations are hiring *data scientists* or *data analysts*, whose job is to explore data interactively and apply statistical models and analytical techniques to find trends and patterns in the data.</span></span> <span data-ttu-id="89aa0-106">A exploração interativa dos dados exige ferramentas e plataformas que fornecem processamento de baixa latência para visualizações de dados e consultas ad hoc.</span><span class="sxs-lookup"><span data-stu-id="89aa0-106">Interactive data exploration requires tools and platforms that provide low-latency processing for ad-hoc queries and data visualizations.</span></span>

![](./images/data-exploration.png)

## <a name="self-service-bi"></a><span data-ttu-id="89aa0-107">BI de autoatendimento</span><span class="sxs-lookup"><span data-stu-id="89aa0-107">Self-service BI</span></span>

<span data-ttu-id="89aa0-108">BI de autoatendimento é um nome atribuído a uma abordagem moderna de tomada de decisões de negócios em que os usuários são capacitados a encontrar, explorar e compartilhar insights de dados em toda a empresa.</span><span class="sxs-lookup"><span data-stu-id="89aa0-108">Self-service BI is a name given to a modern approach to business decision making in which users are empowered to find, explore, and share insights from data across the enterprise.</span></span> <span data-ttu-id="89aa0-109">Para fazer isso, a solução de dados precisa dar suporte a vários requisitos:</span><span class="sxs-lookup"><span data-stu-id="89aa0-109">To accomplish this, the data solution must support several requirements:</span></span>

* <span data-ttu-id="89aa0-110">Descoberta de fontes de dados de negócios por meio de um catálogo de dados.</span><span class="sxs-lookup"><span data-stu-id="89aa0-110">Discovery of business data sources through a data catalog.</span></span>
* <span data-ttu-id="89aa0-111">Gerenciamento de dados mestre para garantir a consistência de definições e valores de entidade de dados.</span><span class="sxs-lookup"><span data-stu-id="89aa0-111">Master data management to ensure consistency of data entity definitions and values.</span></span>
* <span data-ttu-id="89aa0-112">Ferramentas de visualização e modelagem interativa de dados para usuários empresariais.</span><span class="sxs-lookup"><span data-stu-id="89aa0-112">Interactive data modeling and visualization tools for business users.</span></span>

<span data-ttu-id="89aa0-113">Em uma solução de BI de autoatendimento, os usuários empresariais normalmente encontram e consomem fontes de dados que são relevantes para sua área específica da empresa e usam ferramentas intuitivas e aplicativos de produtividade para definir modelos de dados pessoais e relatórios que podem ser compartilhados com seus colegas.</span><span class="sxs-lookup"><span data-stu-id="89aa0-113">In a self-service BI solution, business users typically find and consume data sources that are relevant to their particular area of the business, and use intuitive tools and productivity applications to define personal data models and reports that they can share with their colleagues.</span></span>

<span data-ttu-id="89aa0-114">Serviços do Azure relevantes:</span><span class="sxs-lookup"><span data-stu-id="89aa0-114">Relevant Azure services:</span></span>

- [<span data-ttu-id="89aa0-115">Catálogo de Dados do Azure</span><span class="sxs-lookup"><span data-stu-id="89aa0-115">Azure Data Catalog</span></span>](/azure/data-catalog/data-catalog-what-is-data-catalog)
- [<span data-ttu-id="89aa0-116">Microsoft Power BI</span><span class="sxs-lookup"><span data-stu-id="89aa0-116">Microsoft Power BI</span></span>](https://powerbi.microsoft.com/)

## <a name="data-science-experimentation"></a><span data-ttu-id="89aa0-117">Experimentação de ciência de dados</span><span class="sxs-lookup"><span data-stu-id="89aa0-117">Data science experimentation</span></span>
<span data-ttu-id="89aa0-118">Quando uma organização exige a análise avançada e a modelagem preditiva, o trabalho de preparação inicial normalmente é realizado por cientistas de dados especializados.</span><span class="sxs-lookup"><span data-stu-id="89aa0-118">When an organization requires advanced analytics and predictive modeling, the initial preparation work is usually undertaken by specialist data scientists.</span></span> <span data-ttu-id="89aa0-119">Um cientista de dados explora os dados e aplica técnicas analíticas estatísticas para encontrar relações entre *recursos* de dados e os *rótulos* previstos desejados.</span><span class="sxs-lookup"><span data-stu-id="89aa0-119">A data scientist explores the data and applies statistical analytical techniques to find relationships between data *features* and the desired predicted *labels*.</span></span> <span data-ttu-id="89aa0-120">A exploração de dados geralmente é feita com linguagens de programação, como o Python ou o R, que dão suporte nativo à visualização e modelagem estatística.</span><span class="sxs-lookup"><span data-stu-id="89aa0-120">Data exploration is typically done using programming languages such as Python or R that natively support statistical modeling and visualization.</span></span> <span data-ttu-id="89aa0-121">Os scripts usados para explorar os dados normalmente são hospedados em ambientes especializados, como o Jupyter Notebooks.</span><span class="sxs-lookup"><span data-stu-id="89aa0-121">The scripts used to explore the data are typically hosted in specialized environments such as Jupyter Notebooks.</span></span> <span data-ttu-id="89aa0-122">Essas ferramentas permitem que os cientistas de dados explorem os dados de forma programática, enquanto documentam e compartilham os insights encontrados.</span><span class="sxs-lookup"><span data-stu-id="89aa0-122">These tools enable data scientists to explore the data programmatically while documenting and sharing the insights they find.</span></span>

<span data-ttu-id="89aa0-123">Serviços do Azure relevantes:</span><span class="sxs-lookup"><span data-stu-id="89aa0-123">Relevant Azure services:</span></span>

- [<span data-ttu-id="89aa0-124">Azure Notebooks</span><span class="sxs-lookup"><span data-stu-id="89aa0-124">Azure Notebooks</span></span>](https://notebooks.azure.com/)
- [<span data-ttu-id="89aa0-125">Azure Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="89aa0-125">Azure Machine Learning Studio</span></span>](/azure/machine-learning/studio/what-is-ml-studio)
- [<span data-ttu-id="89aa0-126">Serviços de Experimentação do Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="89aa0-126">Azure Machine Learning Experimentation Services</span></span>](/azure/machine-learning/preview/experimentation-service-configuration)
- [<span data-ttu-id="89aa0-127">A Máquina Virtual de Ciência de Dados</span><span class="sxs-lookup"><span data-stu-id="89aa0-127">The Data Science Virtual Machine</span></span>](/azure/machine-learning/data-science-virtual-machine/overview)

## <a name="challenges"></a><span data-ttu-id="89aa0-128">Desafios</span><span class="sxs-lookup"><span data-stu-id="89aa0-128">Challenges</span></span>

- <span data-ttu-id="89aa0-129">**Conformidade de privacidade de dados.**</span><span class="sxs-lookup"><span data-stu-id="89aa0-129">**Data privacy compliance.**</span></span> <span data-ttu-id="89aa0-130">Você precisa ter cuidado ao disponibilizar dados pessoais para os usuários para análise e relatórios de autoatendimento.</span><span class="sxs-lookup"><span data-stu-id="89aa0-130">You need to be careful about making personal data available to users for self-service analysis and reporting.</span></span> <span data-ttu-id="89aa0-131">Provavelmente, há considerações de conformidade, devido a políticas organizacionais e também a questões regulatórias.</span><span class="sxs-lookup"><span data-stu-id="89aa0-131">There are likely to be compliance considerations, due to organizational policies and also regulatory issues.</span></span> 

- <span data-ttu-id="89aa0-132">**Volume de dados.**</span><span class="sxs-lookup"><span data-stu-id="89aa0-132">**Data volume.**</span></span> <span data-ttu-id="89aa0-133">Embora possa ser útil fornecer aos usuários o acesso à fonte de dados completa, isso poderá resultar em operações do Excel ou Power BI de execução muito longa ou consultas Spark SQL que usam muitos recursos de cluster.</span><span class="sxs-lookup"><span data-stu-id="89aa0-133">While it may be useful to give users access to the full data source, it can result in very long-running Excel or Power BI operations, or Spark SQL queries that use a lot of cluster resources.</span></span>

- <span data-ttu-id="89aa0-134">**Conhecimento do usuário.**</span><span class="sxs-lookup"><span data-stu-id="89aa0-134">**User knowledge.**</span></span> <span data-ttu-id="89aa0-135">Os usuários criar suas próprias consultas e agregações para informar decisões de negócios.</span><span class="sxs-lookup"><span data-stu-id="89aa0-135">Users create their own queries and aggregations in order to inform business decisions.</span></span> <span data-ttu-id="89aa0-136">Você tem certeza de que os usuários têm as competências analíticas e de consulta necessárias para obter resultados precisos?</span><span class="sxs-lookup"><span data-stu-id="89aa0-136">Are you confident that users have the necessary analytical and querying skills to get accurate results?</span></span>

- <span data-ttu-id="89aa0-137">**Compartilhamento de resultados.**</span><span class="sxs-lookup"><span data-stu-id="89aa0-137">**Sharing results.**</span></span> <span data-ttu-id="89aa0-138">Poderá haver considerações de segurança se os usuários puderem criar e compartilhar relatórios ou visualizações de dados.</span><span class="sxs-lookup"><span data-stu-id="89aa0-138">There may be security considerations if users can create and share reports or data visualizations.</span></span>

## <a name="architecture"></a><span data-ttu-id="89aa0-139">Arquitetura</span><span class="sxs-lookup"><span data-stu-id="89aa0-139">Architecture</span></span>

<span data-ttu-id="89aa0-140">Embora a meta deste cenário seja dar suporte à análise interativa de dados, as tarefas de limpeza de dados, amostragem e estruturação envolvidas na ciência de dados geralmente incluem processos de execução longa.</span><span class="sxs-lookup"><span data-stu-id="89aa0-140">Although the goal of this scenario is to support interactive data analysis, the data cleansing, sampling, and structuring tasks involved in data science often include long-running processes.</span></span> <span data-ttu-id="89aa0-141">Isso torna uma arquitetura de [processamento em lotes](./batch-processing.md) apropriada.</span><span class="sxs-lookup"><span data-stu-id="89aa0-141">That makes a [batch processing](./batch-processing.md) architecture appropriate.</span></span>

## <a name="technology-choices"></a><span data-ttu-id="89aa0-142">Opções de tecnologia</span><span class="sxs-lookup"><span data-stu-id="89aa0-142">Technology choices</span></span>

<span data-ttu-id="89aa0-143">As tecnologias a seguir são opções recomendadas de exploração interativa de dados no Azure.</span><span class="sxs-lookup"><span data-stu-id="89aa0-143">The following technologies are recommended choices for interactive data exploration in Azure.</span></span>

### <a name="data-storage"></a><span data-ttu-id="89aa0-144">Armazenamento de dados</span><span class="sxs-lookup"><span data-stu-id="89aa0-144">Data storage</span></span>

- <span data-ttu-id="89aa0-145">**Contêineres do Azure Storage Blob** ou **Azure Data Lake Store**.</span><span class="sxs-lookup"><span data-stu-id="89aa0-145">**Azure Storage Blob Containers** or **Azure Data Lake Store**.</span></span> <span data-ttu-id="89aa0-146">Os cientistas de dados geralmente trabalham com os dados brutos de origem, para garantir que tenham acesso a todos os possíveis recursos, exceções e erros nos dados.</span><span class="sxs-lookup"><span data-stu-id="89aa0-146">Data scientists generally work with raw source data, to ensure they have access to all possible features, outliers, and errors in the data.</span></span> <span data-ttu-id="89aa0-147">Em um cenário de Big Data, esses dados normalmente assumem a forma de arquivos em um armazenamento de dados.</span><span class="sxs-lookup"><span data-stu-id="89aa0-147">In a big data scenario, this data usually takes the form of files in a data store.</span></span>

<span data-ttu-id="89aa0-148">Para obter mais informações, consulte [Armazenamento de dados](../technology-choices/data-storage.md).</span><span class="sxs-lookup"><span data-stu-id="89aa0-148">For more information, see [Data storage](../technology-choices/data-storage.md).</span></span>

### <a name="batch-processing"></a><span data-ttu-id="89aa0-149">Processamento em lotes</span><span class="sxs-lookup"><span data-stu-id="89aa0-149">Batch processing</span></span>

- <span data-ttu-id="89aa0-150">**R Server** ou **Spark**.</span><span class="sxs-lookup"><span data-stu-id="89aa0-150">**R Server** or **Spark**.</span></span> <span data-ttu-id="89aa0-151">A maioria dos cientistas de dados usa linguagens de programação com suporte forte a pacotes matemáticos e estatísticos, como o R ou o Python.</span><span class="sxs-lookup"><span data-stu-id="89aa0-151">Most data scientists use programming languages with strong support for mathematical and statistical packages, such as R or Python.</span></span> <span data-ttu-id="89aa0-152">Ao trabalhar com grandes volumes de dados, você pode reduzir a latência usando plataformas que permitem que essas linguagens usem o processamento distribuído.</span><span class="sxs-lookup"><span data-stu-id="89aa0-152">When working with large volumes of data, you can reduce latency by using platforms that enable these languages to use distributed processing.</span></span> <span data-ttu-id="89aa0-153">O R Server pode ser usado sozinho ou em conjunto com o Spark para escalar horizontalmente as funções de processamento do R e o Spark dá suporte nativo ao Python para funcionalidades de escala horizontal semelhantes nessa linguagem.</span><span class="sxs-lookup"><span data-stu-id="89aa0-153">R Server can be used on its own or in conjunction with Spark to scale out R processing functions, and Spark natively supports Python for similar scale-out capabilities in that language.</span></span>
- <span data-ttu-id="89aa0-154">**Hive**.</span><span class="sxs-lookup"><span data-stu-id="89aa0-154">**Hive**.</span></span> <span data-ttu-id="89aa0-155">O Hive é uma boa opção para transformar os dados usando uma semântica semelhante ao SQL.</span><span class="sxs-lookup"><span data-stu-id="89aa0-155">Hive is a good choice for transforming data using SQL-like semantics.</span></span> <span data-ttu-id="89aa0-156">Os usuários podem criar e carregar tabelas usando instruções do HiveQL, que são semanticamente semelhantes ao SQL.</span><span class="sxs-lookup"><span data-stu-id="89aa0-156">Users can create and load tables using HiveQL statements, which are semantically similar to SQL.</span></span>

<span data-ttu-id="89aa0-157">Para obter mais informações, consulte [Processamento em lotes](../technology-choices/batch-processing.md).</span><span class="sxs-lookup"><span data-stu-id="89aa0-157">For more information, see [Batch processing](../technology-choices/batch-processing.md).</span></span>

### <a name="analytical-data-store"></a><span data-ttu-id="89aa0-158">Armazenamento de Dados Analíticos</span><span class="sxs-lookup"><span data-stu-id="89aa0-158">Analytical Data Store</span></span>

- <span data-ttu-id="89aa0-159">**Spark SQL**.</span><span class="sxs-lookup"><span data-stu-id="89aa0-159">**Spark SQL**.</span></span> <span data-ttu-id="89aa0-160">O Spark SQL é uma API baseada no Spark que dá suporte à criação de dataframes e tabelas que podem ser consultados com a sintaxe SQL.</span><span class="sxs-lookup"><span data-stu-id="89aa0-160">Spark SQL is an API built on Spark that supports the creation of dataframes and tables that can be queried using SQL syntax.</span></span> <span data-ttu-id="89aa0-161">Independentemente de os arquivos de dados a serem analisados serem arquivos de origem brutos ou novos arquivos que foram limpos e preparados por um processo em lote, os usuários podem definir tabelas do Spark SQL neles para a consulta posterior de uma análise.</span><span class="sxs-lookup"><span data-stu-id="89aa0-161">Regardless of whether the data files to be analyzed are raw source files or new files that have been cleaned and prepared by a batch process, users can define Spark SQL tables on them for further querying an analysis.</span></span> 
- <span data-ttu-id="89aa0-162">**Hive**.</span><span class="sxs-lookup"><span data-stu-id="89aa0-162">**Hive**.</span></span> <span data-ttu-id="89aa0-163">Além dos dados brutos de processamento em lotes com o Hive, você pode criar um banco de dados do Hive que contém tabelas e exibições do Hive com base nas pastas em que os dados estão armazenados, permitindo consultas interativas para análise e relatórios.</span><span class="sxs-lookup"><span data-stu-id="89aa0-163">In addition to batch processing raw data by using Hive, you can create a Hive database that contains Hive tables and views based on the folders where the data is stored, enabling interactive queries for analysis and reporting.</span></span> <span data-ttu-id="89aa0-164">O HDInsight inclui um tipo de cluster do Hive Interativo que usa o cache em memória para reduzir os tempos de resposta da consulta do Hive.</span><span class="sxs-lookup"><span data-stu-id="89aa0-164">HDInsight includes an Interactive Hive cluster type that uses in-memory caching to reduce Hive query response times.</span></span> <span data-ttu-id="89aa0-165">Os usuários que estão familiarizados com a sintaxe semelhante ao SQL podem usar o Hive Interativo para explorar os dados.</span><span class="sxs-lookup"><span data-stu-id="89aa0-165">Users who are comfortable with SQL-like syntax can use Interactive Hive to explore data.</span></span>

<span data-ttu-id="89aa0-166">Para obter mais informações, consulte [Armazenamentos de dados analíticos](../technology-choices/analytical-data-stores.md).</span><span class="sxs-lookup"><span data-stu-id="89aa0-166">For more information, see [Analytical data stores](../technology-choices/analytical-data-stores.md).</span></span>

### <a name="analytics-and-reporting"></a><span data-ttu-id="89aa0-167">Análise e relatórios</span><span class="sxs-lookup"><span data-stu-id="89aa0-167">Analytics and reporting</span></span>

- <span data-ttu-id="89aa0-168">**Jupyter**.</span><span class="sxs-lookup"><span data-stu-id="89aa0-168">**Jupyter**.</span></span> <span data-ttu-id="89aa0-169">O Jupyter Notebooks fornece uma interface baseada em navegador para executar o código em linguagens como o R, Python ou Scala.</span><span class="sxs-lookup"><span data-stu-id="89aa0-169">Jupyter Notebooks provides a browser-based interface for running code in languages such as R, Python, or Scala.</span></span> <span data-ttu-id="89aa0-170">Ao usar o R Server ou o Spark para processar dados em lotes ou ao usar o Spark SQL para definir um esquema de tabelas para consulta, o Jupyter pode ser uma boa opção para consultar os dados.</span><span class="sxs-lookup"><span data-stu-id="89aa0-170">When using R Server or Spark to batch process data, or when using Spark SQL to define a schema of tables for querying, Jupyter can be a good choice for querying the data.</span></span> <span data-ttu-id="89aa0-171">Ao usar o Spark, você pode usar a API de dataframe padrão do Spark ou a API do Spark SQL, bem como instruções inseridas do SQL, para consultar os dados e gerar visualizações.</span><span class="sxs-lookup"><span data-stu-id="89aa0-171">When using Spark, you can use the standard Spark dataframe API or the Spark SQL API as well as embedded SQL statements to query the data and produce visualizations.</span></span>
- <span data-ttu-id="89aa0-172">**Clientes do Hive Interativo**.</span><span class="sxs-lookup"><span data-stu-id="89aa0-172">**Interactive Hive Clients**.</span></span> <span data-ttu-id="89aa0-173">Caso você use um cluster do Hive Interativo para consultar os dados, use a exibição do Hive no painel do cluster Ambari, a ferramenta de linha de comando Beeline ou qualquer ferramenta baseada em ODBC (usando o driver ODBC do Hive), como o Microsoft Excel ou o Power BI.</span><span class="sxs-lookup"><span data-stu-id="89aa0-173">If you use an Interactive Hive cluster to query the data, you can use the Hive view in the Ambari cluster dashboard, the Beeline command line tool, or any ODBC-based tool (using the Hive ODBC driver), such as Microsoft Excel or Power BI.</span></span>

<span data-ttu-id="89aa0-174">Para obter mais informações, consulte [Tecnologia de relatórios e análise de dados](../technology-choices/analysis-visualizations-reporting.md).</span><span class="sxs-lookup"><span data-stu-id="89aa0-174">For more information, see [Data analytics and reporting technology](../technology-choices/analysis-visualizations-reporting.md).</span></span>