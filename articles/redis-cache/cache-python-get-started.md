<properties
	pageTitle="How to use Azure Redis Cache with Python | Microsoft Azure"
	description="Get started with Azure Redis Cache using Python"
	services="redis-cache"
	documentationCenter=""
	authors="steved0x"
	manager="dwrede"
	editor="v-lincan"/>

<tags
	ms.service="cache"
	ms.devlang="python"
	ms.topic="hero-article"
	ms.tgt_pltfrm="cache-redis"
	ms.workload="tbd"
	ms.date="12/03/2015"
	ms.author="sdanie"/>

# How to use Azure Redis Cache with Python

> [AZURE.SELECTOR]
- [.Net](cache-dotnet-how-to-use-azure-redis-cache.md)
- [Node.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

This topic shows you how to get started with Azure Redis Cache using Python.


## Prerequisites

Install [redis-py](https://github.com/andymccurdy/redis-py).


## Create a Redis cache on Azure

In the [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=398536), click **New**, **Data + Storage**, and select **Redis Cache**.

  ![][1]

Enter a DNS hostname. It will have the form `<name>
  .redis.cache.windows.net`. Click **Create**.

  ![][2]

  Once you create the cache, [browse to it](cache-configure.md#configure-redis-cache-settings) to view the cache settings. You will need:

  - **Hostname.** You entered this name when you created the cache.
  - **Port.** Click the link under **Ports** to view the ports. Use the SSL port.
  - **Access Key.** Click the link under **Keys** and copy the primary key.

  ## Add something to the cache and retrieve it

  >>> import redis
  >>> r = redis.StrictRedis(host='<name>.redis.cache.windows.net',
          port=6380, db=0, password='<key>', ssl=True)
    >>> r.set('foo', 'bar')
    True
    >>> r.get('foo')
    b'bar'

Replace *&lt;name&gt;* with your cache name and *&lt;key&gt;* with your access key.


<!--Image references-->
[1]: ./media/cache-python-get-started/cache01.png
[2]: ./media/cache-python-get-started/cache02.png
