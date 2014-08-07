## Meet命令的处理流程： ##

我们的集群假设有三个Node，A、B和C。创建集群时，B和C都向A发出meet
流程如下：


1. redis-cli -p bPort cluster meet aIp aPort
1. 在node B中的cluster->nodes添加nodeA
2. 在node B中的clusterCron发现cluster->nodes有nodeA，根据nodeA的ip和port创建连接link
3. 连接创建以后（并不一定是connected）。创建node-> link 和node->link的映射关系来表明link属于是那个node的
3. 在node A的clusterAcceptHandler中接受，并且创建link

##  更新slot info流程 ##
在本node保存的sender的slot信息和node发送过来的slot信息不一致时认为有dirtySlot

<code>
if (sender) {
            sender_master = nodeIsMaster(sender) ? sender : sender->slaveof;
            if (sender_master) {
                dirty_slots = memcmp(sender_master->slots,
                        hdr->myslots,sizeof(hdr->myslots)) != 0;
            }
        }
</code>