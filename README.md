# first-read-last-write-cache

This is an implementation of a cache that tracks the first read and the last write for a particular key. The cache ensures consistency between reads and writes.

For example:

|key   	|operation 1|operation 2|operation 3|(first read, last write)   |
|:---   |:---	    |:---       |:---       |:---                       |
|k      |Read(1)    |Write(3)   |Read(3)    |((Read(1), Write(3))       |
|k      |Write(3)   |Read(3)    |Read(3)    |((_      , Write(3))       |
|k      |Write(5)   |Read(3)    |...        |inconsistent               |



## first-read-last-write-cache in action
---

Example:
```rust
    let mut cache = CacheLog::default();
    let value = match cache.get_value(&key) {
        ExistsInCache::Yes(value) => value,
        ExistsInCache::No => {
            // This is some "expensive" operation, for example a db lookup.
            let new_value = Some(Arc::new(vec![4, 5, 6, 7]));
            cache_log.add_read(key, new_value);
            new_value
        }
    };
```

