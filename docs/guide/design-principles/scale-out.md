---
title: "扩大式设计"
description: "云应用程序应设计为适合水平缩放。"
author: MikeWasson
layout: LandingPage
ms.openlocfilehash: 8f9b3e99a53f5941f708b0de124f37e6ff7e5ab2
ms.sourcegitcommit: b0482d49aab0526be386837702e7724c61232c60
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/14/2017
---
# <a name="design-to-scale-out"></a>扩大式设计

## <a name="design-your-application-so-that-it-can-scale-horizontally"></a>设计应用程序，使其可进行水平缩放

云的主要优点是可以弹性缩放 &mdash; 能够根据需要使用容量，在负载增加时扩大，在不需要额外容量时缩小。 设计应用程序，使其能够扩大，根据需要添加或删除新实例。

## <a name="recommendations"></a>建议

**避免实例粘性**。 在来自相同客户端的请求始终路由至同一台服务器时，才会出现粘性或会话相关性。 粘性限制了应用程序扩大的能力。例如，来自高容量用户的流量不会跨实例分布。 粘性的成因包括在内存中存储会话状态以及使用特定于计算的密钥加密。 请确保任何实例都可处理任何请求。 

**识别瓶颈**。 扩大并不是解决每个性能问题的万能方式。 例如，如果后端数据库是瓶颈，添加再多 Web 服务器也于事无补。 首先识别并解析系统中的瓶颈，然后针对该问题引发更多实例。 系统中有状态的部分最有可能引起瓶颈问题。 

**通过可伸缩性要求分解工作负荷**  应用程序通常由多个工作负荷组成，它们的缩放要求各不相同。 例如，应用程序可能有面向公众的网站和单独的管理网站。 公众网站可能会遇到流量突然激增的情况，而管理网站的负荷更小，且更具可预测性。 

**卸载资源密集型任务** 需要大量的 CPU 或 I/O 资源的任务应尽可能移动到[后台作业][background-jobs]以减少处理用户请求的前端上的负载。

**使用内置自动缩放功能**。 许多 Azure 计算服务有内置的自动缩放支持。 如果应用程序具有可预测的常规工作负荷，则可按计划扩大。 例如，在营业时间期间扩大。 否则，如果工作负荷不可预测，则可使用 CPU 或请求队列长度等性能指标来触发自动缩放。 有关自动缩放最佳做法的信息，请参阅[自动缩放][autoscaling]。

**请为关键工作负荷考虑自动缩放**。 对于关键工作负荷，你希望供应一直超过需求。 在负荷加重的情况下，最好快速增加新实例以处理额外的流量，然后再逐渐缩减。

**缩小式设计**。  请记住，使用弹性缩放时，应用程序会有缩小时期，此时实例会被移除。 应用程序必须妥善处理正在移除的实例。 以下是一些处理缩小功能的方法：

- 侦听关闭事件（可用时），然后全部关闭。 
- 服务的客户端/使用者应支持暂时性故障处理和重试。 
- 对于运行时间长的任务，请考虑使用检查点，或者[管道和筛选器][pipes-filters-pattern]模式分解工作。 
- 如果实例是在处理过程中移除的，请将工作项放在队列中，以便另一个实例可以接收工作。 


<!-- links -->

[autoscaling]: ../../best-practices/auto-scaling.md
[background-jobs]: ../../best-practices/background-jobs.md
[pipes-filters-pattern]: ../../patterns/pipes-and-filters.md