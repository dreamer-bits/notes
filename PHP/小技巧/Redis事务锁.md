# Redis事务锁

```php
<?php

/**
 * Redis事务锁（并发安全）
 *        
 */
class RedisLock {

    /**
     *
     * @var \Redis
     */
    protected $redis;

    /**
     * 锁值
     *
     * @var string
     */
    protected $lockFlag;

    public function __construct() {
      	// 连接redis
        $this->redis = getRedis();
    }

    public function lock($key, $expire = 5) {
      	// 使用lua脚本生成事务锁，保证原子性
        $script = <<<EOF
                    
            local key = KEYS[1]
            local value = ARGV[1]
            local ttl = ARGV[2]
            
            if (redis.call('setnx', key, value) == 1) then
                return redis.call('expire', key, ttl)
            elseif (redis.call('ttl', key) == -1) then
                return redis.call('expire', key, ttl)
            end
            
            return 0
            
EOF;

        $this->lockFlag = md5(microtime(true));
        return $this->_eval($script, [
            $key,
            $this->lockFlag,
            $expire
        ]);
    }

    public function unlock($key) {
      	// 使用lua脚本删除事务锁，保证原子性
        $script = <<<EOF
            
            local key = KEYS[1]
            local value = ARGV[1]
            
            if (redis.call('exists', key) == 1 and redis.call('get', key) == value)
            then
                return redis.call('del', key)
            end
            
            return 0
            
EOF;

        if ($this->lockFlag) {
            return $this->_eval($script, [
                $key,
                $this->lockFlag
            ]);
        }
        return NULL;
    }

  	/**
  	 * redis调用lua脚本
  	 */
    private function _eval($script, array $params, $keyNum = 1) {
        $hash = $this->redis->script('load', $script);
        return $this->redis->evalSha($hash, $params, $keyNum);
    }
}
```

