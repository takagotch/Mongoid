### Mongoid
---
https://github.com/mongodb/mongoid

```
gem install mongoid
gem 'mongoid', '~> 7.0'

rails g mongiod:config



```


```
development:
  clients:
    dafaults:
      database: mongoid
      hosts:
        - localhost:27017
        
development:
  clients:
    default:
      database: my_db
      hosts:
        - myhost1.mydomain.com:27017
        - myhost2.mydomain.com:27017
        - myhost3.mydomain.com:27017
      options:
        write:
          w: 1
        read:
          mode: :secondary_preferred
          tag_sets:
            - use: web
        user: 'user'
        password: 'password'
        roles:
          - 'dbOwner'
        auth_mech: :scram
        auth_source: admin
        connect: :direct
        local_threshold: 0.015
        server_selection_timeout: 30
        max_pool_size: 5
        min_pool_size: 1
        wait_queue_timeout: 1
        connect_timeout: 10
        socket_timeout: 5
        replica_set: my_replica_set
        ssl: true
        ssl_cert: /path/to/my.cert
        ssl_key: /path/to/my.key
        ssl_key_pass: /path/to/my.key
        ssl_key_pass_phrase: password
        ssl_verify: true
        ssl_ca_cert: /path/to/ca.cert
      options:
        include_root_in_json: false
        include_type_for_serialization: false
        preload_models: false
        raise_not_found_error: true
        scope_overwrite_exception: false
        use_activesupport_time_zone: true
        use_utc: false
        log_level: :info
        



```


```ruby
Mongoid.load!("path/to/your/mongiod.yml", :production)

config.generators do |g|
  g.orm :mongoid
end

Mongoid.configure do |config|
  config.clients.default = {
    hosts: ['localhost:27017'],
    database: 'my_db',
  }
  config.log_level = :warn
end

Mongoid.logger.level = Logger::DEBUG
Mongoid::Logger.logger.level = Logger::INFO

Mongoid.client(:default).logger == Mongoid::Logger.logger

after_fork do |server, worker|
  Mongoid.clients.each do |name, client|
    client.close
    client.reconnect
  end
end

before_fork do |server, worker|
  Mongoid.clients.each do |name, client|
    client.close
  end
end




```

