redis_plugin
============

This is a collectd (<http://collectd.org/>) plugin which runs under the Python plugin (<https://collectd.org/wiki/index.php/Plugin:Python>) to collect metrics from redis (<http://redis.io/>).

This modifiy version from (<https://github.com/zerthimon/redis_plugin>) plugin include some new features:
* Socket timeout support for each instants.
* Interval options for each instants.
* Parallel running readers for multiple instants.

Requirements:
------------

*Redis*  
Read access to Redis' UNIX Domain socket or TCP port.

*collectd*  
Collectd must have the Python plugin installed. See (<https://collectd.org/wiki/index.php/Plugin:Python>)

*Python 2.6 and later*  
Plugin currently supports Python 2.6 and later.  

Options:
-------
* `Socket`  
   Path to a UNIX Domain socket of the Redis instance.  
   Default: none  
* `IP`  
   IP Address of the Redis instance.  
   When specified together with Socket, takes over Socket option.  
   Default: 127.0.0.1  
* `Port`  
   TCP Port of the Redis instance.  
   Default: 6379  
* `Auth`  
   Password to use when connecting to Redis instance.  
   Default: none  
* `Timeout`  
   Socket Timeout seconds to use when connecting to Redis instance.  
   Default: 0.1  
* `Interval`  
   Read Interval seconds for each plguin instants, default to use Interval config of collectd.  
   Default: 0  
* `Commandstats`  
   Include Redis command statistics, from "INFO COMMANDSTATS" command.  
   Default: false  
* `Verbose`  
   Provide verbose logging of plugin operation in the Collectd's log.  
   Default: false  
* `Instance`  
   There are situations when multiple instances of Redis need to run on the same host. This option opens an Instance block. 

Single-instance Plugin Config Example:
-------
    TypesDB "/usr/share/collectd/redis_types.db"

    <LoadPlugin python>
        Globals true
    </LoadPlugin>

    <Plugin python>
        # redis_plugin.py is at /usr/lib64/collectd/redis_plugin.py
        ModulePath "/usr/lib64/collectd/"

        Import "redis_plugin"

        <Module redis_plugin>
          Socket "/var/run/redis.sock"
          Commandstats true
          Verbose false
        </Module>
    </Plugin>

Multi-instance Plugin Config Example:
----------------------
    TypesDB "/usr/share/collectd/redis_types.db"`

    <LoadPlugin python>
        #Globals true
        Interval 1
    </LoadPlugin>

    <Plugin python>
        # redis_plugin.py is at /usr/lib64/collectd/redis_plugin.py
        ModulePath "/usr/lib64/collectd/"

        Import "redis_plugin"

        <Module redis_plugin>
          <Instance redis1>
              Socket "/var/run/redis1.sock"
              Commandstats false
          </Instance>
          <Instance redis2>
              IP "127.0.0.1"
              Port 6379
              Auth "foobared"
              Commandstats true
              # Interval 5
              # Timeout 0.5
          </Instance>
          Verbose false
        </Module>
    </Plugin>

Graph Examples:
--------------
These graphs were created using Graphite (<http://graphite.wikidot.com/>)

Connected Clients:
![Connected Clients](https://github.com/zerthimon/redis_plugin/raw/master/screenshots/redis_connected_clients.png)

Redis Process RSS Memory:
![Redis Process RSS Memory](https://github.com/zerthimon/redis_plugin/raw/master/screenshots/redis_memory_used_rss.png)

Commands per Second:
![Commands per Second](https://github.com/zerthimon/redis_plugin/raw/master/screenshots/redis_total_commands_per_sec.png)

Redis GET Commands per Second:
![Redis GET Commands per Second](https://github.com/zerthimon/redis_plugin/raw/master/screenshots/redis_get_commands_per_sec.png)

Cache Hits:
![Cache Hits](https://github.com/zerthimon/redis_plugin/raw/master/screenshots/redis_cache_hits.png)

RDB Changes Since Last Save:
![RDB Changes Since Last Save](https://github.com/zerthimon/redis_plugin/raw/master/screenshots/redis_changes_since_last_rdb_save.png)

Total Keys in db0:
![Total Keys in db0](https://github.com/zerthimon/redis_plugin/raw/master/screenshots/redis_db0_keys_total.png)

Stats Example 1:  
![Stats Example 1](https://github.com/zerthimon/redis_plugin/raw/master/screenshots/stats_example1.png)

Stats Example 2:  
![Stats Example 2](https://github.com/zerthimon/redis_plugin/raw/master/screenshots/stats_example2.png)

