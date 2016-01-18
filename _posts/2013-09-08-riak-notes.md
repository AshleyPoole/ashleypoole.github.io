---
layout: post
title: Riak DB Server Notes
date: 2013-09-08 19:00
categories: [databases, knowledge-base]
---
<strong>Configuration</strong>

{% highlight bash %}
# Start Riak.
riak start

# Check node is running. Will return pong if successful.
riak ping

# Check the node can read and write data.
riak-admin test

# Or using curl. Will get a http 200 if working.
curl -v http://127.0.0.1:8098/riak/test

# Verify basic configuration and general health. Also makes recommendations for optimal operation.
riak-admin diag

# Riak cluster member status
riak-admin memeber-status

{% endhighlight %}

<strong>Reading an object</strong>

http://docs.basho.com/riak/1.3.0/tutorials/querying/Basic-Operations/

{% highlight bash %}

# Riak default is to read from two replicas unless overridden.

# Retrieve a object called key from a bucket called bucket (if exists).
# A 404 status is returned if the key doesn't exist. 200 status if Ok.
curl -v http://127.0.0.1:8098/riak/bucket/key
{% endhighlight %}

<strong>Store and delete an object</strong>

{% highlight bash %}

# Status 201 Created
# If you store a value in a bucket without passing a key name, a new one will be created for you.
# In the post result, the location header will show you the location of the key that stores your data.
curl -v -d 'this is a test' -H "Content-Type: text/plain" http://127.0.0.1:8098/riak/test

# Delete an object
# Either receive 204 no content status or 404 not found status
curl -v -X DELETE http://127.0.0.1:8098/riak/test/testkey

{% endhighlight %}

<a href="http://docs.basho.com/riak/1.3.0/tutorials/fast-track/Building-a-Development-Environment/" target="_blank">http://docs.basho.com/riak/1.3.0/tutorials/fast-track/Building-a-Development-Environment/</a>

<a title="Basho - Riak Docs" href="http://docs.basho.com/riak/" target="_blank">http://docs.basho.com/riak/</a>

<a href="http://littleriakbook.com/" target="_blank">http://littleriakbook.com/</a>

<a href="http://basho.com/assets/RelationaltoRiak.pdf" target="_blank">http://basho.com/assets/RelationaltoRiak.pdf</a>
