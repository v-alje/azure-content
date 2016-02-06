<properties 
	pageTitle="How to configure Azure Redis Cache"
	description="Understand the default Redis configuration for Azure Redis Cache and learn how to configure your Azure Redis Cache instances"
	services="redis-cache"
	documentationCenter="na"
	authors="steved0x"
	manager="dwrede"
	editor="tysonn" />
<tags 
	ms.service="cache"
	ms.devlang="na"
	ms.topic="article"
	ms.tgt_pltfrm="cache-redis"
	ms.workload="tbd"
	ms.date="12/16/2015"
	ms.author="sdanie" />

# How to configure Azure Redis Cache

This topic describes how to review and update the configuration for your Azure Redis Cache instances, and covers the default Redis server configuration for Azure Redis Cache instances.

>[AZURE.NOTE] For more information on configuring and using premium cache features, see [How to configure persistence for a Premium Azure Redis Cache](cache-how-to-premium-persistence.md), [How to configure clustering for a Premium Azure Redis Cache](cache-how-to-premium-clustering.md), and [How to configure Virtual Network support for a Premium Azure Redis Cache](cache-how-to-premium-vnet.md).

## Configure Redis cache settings

Caches can be accessed in the [Azure Portal](https://portal.azure.com) using the **Browse** blade.

![Azure Redis Cache Browse Blade](./media/cache-configure/IC796920.png)

Click **Redis Caches** to view your caches.

![Azure Redis Cache Browse Cache List](./media/cache-configure/IC796921.png)

Select the desired cache to view the properties for that cache.

![Redis Cache All Settings](./media/cache-configure/IC808312.png)

Click **Settings** or **All settings** to view and configure your cache.

![Redis Cache Settings](./media/cache-configure/IC808313.png)

## Properties

Click **Properties** to view information about your cache, including the cache endpoint and ports.

![Redis Cache Properties](./media/cache-configure/IC808314.png)

## Access keys

Click **Access keys** to view or regenerate the access keys for your cache. These keys are used along with the host name and ports from the **Properties** blade by the clients connecting to your cache.

![Redis Cache Access Keys](./media/cache-configure/IC808315.png)

## Access Ports

By default, non-SSL access is disabled for new caches. To enable the non-SSL port, click **Access Ports** blade and then click **No**.

![Redis Cache Access Ports](./media/cache-configure/IC808316.png)

## Pricing tier

Click **Pricing tier** to view or change the pricing tier for your cache. For more information on scaling, see [How to Scale Azure Redis Cache](cache-how-to-scale.md).

![Redis Cache pricing tier](./media/cache-configure/pricing-tier.png)

## Diagnostics

Click **Diagnostics** to configure the storage account used to store cache diagnostics.

![Redis Cache Diagnostics](./media/cache-configure/IC808317.png)

For more information, see [How to monitor Azure Redis Cache](cache-how-to-monitor.md).

## Maxmemory-policy and maxmemory-reserved

Click **Maxmemory policy** to configure the memory policies for the cache. The **maxmemory-policy** setting configures the eviction policy for the cache and **maxmemory-reserved** configures the memory reserved for non-cache processes.

![Redis Cache Maxmemory Policy](./media/cache-configure/IC808318.png)

**Maxmemory policy** allows you to choose from the following eviction policies.

-	volatile-lru - this is the default.
-	allkeys-lru
-	volatile-random
-	allkeys-random
-	volatile-ttl
-	noeviction

For more information about maxmemory policies, see [Eviction policies](http://redis.io/topics/lru-cache#eviction-policies).

The **maxmemory-reserved** setting configures the amount of memory in MB that is reserved for non-cache operations such as replication during failover. It can also be used when you have a high fragmentation ratio. Setting this value allows you to have a more consistent Redis server experience when your load varies. This value should be set higher for workloads which are write heavy. When memory is reserved for such operations it is unavailable for storage of cached data.

>[AZURE.IMPORTANT] The **maxmemory-reserved** setting is only available for Standard and Premium caches.

## Keyspace notifications (advanced settings)

Click **Advanced settings** to configure Redis keyspace notifications. Keyspace notifications allows clients to receive notifications when certain events occur.

![Redis Cache Advanced Settings](./media/cache-configure/IC808319.png)

>[AZURE.IMPORTANT] Keyspace notifications and the **notify-keyspace-events** setting are only available for Standard and Premium caches.

For more information, see [Redis Keyspace Notifications](http://redis.io/topics/notifications). For sample code, see the [KeySpaceNotifications.cs](https://github.com/rustd/RedisSamples/blob/master/HelloWorld/KeySpaceNotifications.cs) file in the [Hello world](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) sample.

## Redis data persistence

Click **Redis data persistence** to enable, disable, or configure data persistence for your premium cache.

![Redis data persistence](./media/cache-configure/redis-cache-persistence-settings.png)

To enable Redis persistence, click **Enabled** to enable RDB (Redis database) backup. To disable Redis persistence, click **Disabled**.

To configure the backup interval, select a **Backup Frequency** from the drop-down list. Choices include **15 Minutes**, **30 minutes**, **60 minutes**, **6 hours**, **12 hours**, and **24 hours**. This interval starts counting down after the previous backup operation successfully completes and when it elapses a new backup is initiated.

Click **Storage Account** to select the storage account to use, and choose either the **Primary key** or **Secondary key** to use from the **Storage Key** drop-down. You must choose a storage account in the same region as the cache, and a **Premium Storage** account is recommended because premium storage has higher throughput. Anytime the storage key for your persistence account is regenerated, you must re-choose the desired key from the **Storage Key** drop-down.

Click **OK** to save the persistence configuration.

>[AZURE.IMPORTANT] Redis data persistence is only available for Premium caches. For more information, see [How to configure persistence for a Premium Azure Redis Cache](cache-how-to-premium-persistence.md).

<a name="cluster-size"></a>
## Redis Cluster Size

Click **(PREVIEW) Redis Cluster Size** to change the cluster size for a running premium cache with clustering enabled.

>[AZURE.NOTE] Note that while the Azure Redis Cache Premium tier has been released to General Availability, the Redis Cluster Size feature is currently in preview.

![Redis cluster size](./media/cache-configure/redis-cache-redis-cluster-size.png)

To change the cluster size, use the slider or type a number between 1 and 10 in the **Shard count** text box and click **OK** to save.

>[AZURE.IMPORTANT] Redis clustering is only available for Premium caches. For more information, see [How to configure clustering for a Premium Azure Redis Cache](cache-how-to-premium-clustering.md).


## Users and tags

![Redis Cache Users and Tags](./media/cache-configure/IC808320.png)

The **Users** section provides support for role-based access control (RBAC) in the Azure Portal to help organizations meet their access management requirements simply and precisely. For more information, see [Role-based access control in the Azure Portal](http://go.microsoft.com/fwlink/?LinkId=512803).

The **Tags** section helps you organize your resources. For more information, see [Using tags to organize your Azure resources](../resource-group-using-tags.md).

## Default Redis server configuration

New Azure Redis Cache instances are configured with the following default Redis configuration values.

>[AZURE.NOTE] The settings in this section cannot be changed using the `StackExchange.Redis.IServer.ConfigSet` method. If this method is called with one of the commands in this section, an exception similar to the following is thrown:  
>
>`StackExchange.Redis.RedisServerException: ERR unknown command 'CONFIG'`
>  
>Any values that are configurable, such as **max-memory-policy**, are configurable through the Azure Portal or command line management tools such as Azure CLI or PowerShell.

|Setting|Default value|Description|
|---|---|---|
|databases|16|The default database is DB 0, you can select a different one on a per-connection basis using connection.GetDataBase(dbid) where dbid is a number between 0 and 15.|
|maxclients|Depends on the pricing tier<sup>1</sup>|This is the maximum number of connected clients allowed at the same time. Once the limit is reached Redis will close all the new connections sending an error 'max number of clients reached'.|
|maxmemory-policy|volatile-lru|Maxmemory policy is the setting for how Redis will select what to remove when maxmemory (the size of the cache offering you selected when you created the cache) is reached. With Azure Redis Cache the default setting is volatile-lru, which removes the keys with an expire set using an LRU algorithm. This setting can be configured in the Azure portal. For more information, see [Maxmemory-policy and maxmemory-reserved](#maxmemory-policy-and-maxmemory-reserved).|
|maxmemory-samples|3|LRU and minimal TTL algorithms are not precise algorithms but approximated algorithms (in order to save memory), so you can select as well the sample size to check. For instance for default Redis will check three keys and pick the one that was used less recently.|
|lua-time-limit|5,000|Max execution time of a Lua script in milliseconds. If the maximum execution time is reached Redis will log that a script is still in execution after the maximum allowed time and will start to reply to queries with an error.|
|lua-event-limit|500|This is the max size of script event queue.|
|client-output-buffer-limit normalclient-output-buffer-limit pubsub|0 0 032mb 8mb 60|The client output buffer limits can be used to force disconnection of clients that are not reading data from the server fast enough for some reason (a common reason is that a Pub/Sub client can't consume messages as fast as the publisher can produce them). For more information, see [http://redis.io/topics/clients](http://redis.io/topics/clients).|

<sup>1</sup>`maxclients` is different for each Azure Redis Cache pricing tier.

-	Basic and Standard caches
	-	C0 (250 MB) cache - up to 256 connections
	-	C1 (1 GB) cache - up to 1,000 connections
	-	C2 (2.5 GB) cache - up to 2,000 connections
	-	C3 (6 GB) cache - up to 5,000 connections
	-	C4 (13 GB) cache - up to 10,000 connections
	-	C5 (26 GB) cache - up to 15,000 connections
	-	C6 (53 GB) cache - up to 20,000 connections
-	Premium caches
	-	P1 (6 GB - 60 GB) - up to 7,500 connections
	-	P2 (13 GB - 130 GB) - up to 15,000 connections
	-	P3 (26 GB - 260 GB) - up to 30,000 connections
	-	P4 (53 GB - 530 GB) - up to 40,000 connections

## Redis commands not supported in Azure Redis Cache

>[AZURE.IMPORTANT] Because configuration and management of Azure Redis Cache instances is managed by Microsoft the following commands are disabled. If you try to invoke them you will receive an error message similar to `"(error) ERR unknown command"`.
>
>-	BGREWRITEAOF
>-	BGSAVE
>-	CONFIG
>-	DEBUG
>-	MIGRATE
>-	SAVE
>-	SHUTDOWN
>-	SLAVEOF

For more information about Redis commands, see [http://redis.io/commands](http://redis.io/commands).

## Redis console

You can securely issue commands to your Azure Redis Cache instances using the **Redis Console**, which is available for Standard and Premium caches.

>[AZURE.IMPORTANT] The Redis Console does not work with VNET or clustering. 
>
>-	[VNET](cache-how-to-premium-vnet.md) - When your cache is part of a VNET, only clients in the VNET can access the cache. Because the Redis Console uses the redis-cli.exe client hosted on VMs that are not part of your VNET, it can't connect to your cache.
>-	[Clustering](cache-how-to-premium-clustering.md) - The Redis Console uses the redis-cli.exe client which does not support clustering at this time. The redis-cli utility in the [unstable](http://redis.io/download) branch of the Redis repository at GitHub implements basic support when started with the `-c` switch. For more information see [Playing with the cluster](http://redis.io/topics/cluster-tutorial#playing-with-the-cluster) on [http://redis.io](http://redis.io) in the [Redis cluster tutorial](http://redis.io/topics/cluster-tutorial).

To access the Redis Console, click **Console** from the **Redis Cache** blade.

![Redis console](./media/cache-configure/redis-console-menu.png)

To issue commands against your cache instance, simply type in the desired command into the console.

![Redis console](./media/cache-configure/redis-console.png)

For list of Redis commands that are disabled for Azure Redis Cache, see the previous [Redis commands not supported in Azure Redis Cache](#redis-commands-not-supported-in-azure-redis-cache) section. For more information about Redis commands, see [http://redis.io/commands](http://redis.io/commands). 

## Next steps
-	For more information on working with Redis commands, see [How can I run Redis commands?](cache-faq.md#how-can-i-run-redis-commands).