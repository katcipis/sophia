# WiscKey: Separating Keys from Values in SSD-conscious Storage

Article: https://www.usenix.org/system/files/conference/fast16/fast16-papers-lu.pdf 

## LSM

Tradicional LSM (Log Structured Merge Tree) is optimized towards
sequential disk reads, trading sequential reads for a lot of read/write
amplification.

This makes sense since sequential reads VS random reads performance
ratio is 1000:1 in spin disks. On SSD random reads can be slower,
but not on the same order of magnitude of spin disks.

As the article title makes clear, the core idea is to optimize for
SSD, this is the basis of the whole article.

First, why does LSM have so much amplification ? Basically because the
LSM tree has the keys + values together, this means bigger trees.

Positive points of using keys + values on LSM tree:

* Simpler range query, keys are sorted and the values are together on the tree
* Specially on range queries the reads will be sequential (great for spin disks)

Negative points:

* More expensive compaction phase
* Reads can be expensive since tree is biggers and worst case traverses entire tree

The expensive compaction and the bigger tree is the price to pay
for sequential reads.

Compaction is a process that is part of the LSM algorithm, it is when
one of the levels exceeded the size threshold and must be sort/merged
with the lower level. This process can run recursively until the last
level, and can be pretty expensive since you must move keys + values together.

Keys usually have a size of 16 bytes, but value size can vary from
64 bytes to 512 kb.

## WiscKey

The catch of WiscKey comes from the fact that SSD's can achieve
full throughput on random reads if these operations are done
concurrently and with a good enough block size.

Notice that this is extremely dependent of the SSD, testing if your
structure behaves like this is essential to have the advantages.

If random reads on your disk cant achieve a high throughput, WiscKey
is a bad idea for you.

Why ? The idea of the algorithm is to separate the values from the keys.
Basically the LSM tree will consist of tuples on the form:

```
(key, valueoffset)
```

And there will be a value file containing all files (unordered).
The ability to perform multiple concurrent reads on the disk
is necessary since indexed searches and range queries will
return sequential regions of **(key, valueoffset)** and each
**valueoffset** will require a read on a different (non-sequential)
region of the disk. SSD's have no problem doing that, and can
even achieve full disk throughput if you have enough concurrent
units.

On the tests performed the number of 32 threads where enough to
use all throughput of the disk.

Since there is an indirection, it seems like it will be slower, but
specially for value sizes greater or equal to 4K WiscKey is much
faster since the indirection overhead is compensated by a MUCH
smaller LSM tree (easier to cache and to compact).

The place where the values are stored is named vlog. The vlog has
this tuple:

```
(keysize, valuesize, key, value)
```

Why is there key information ? It is used for garbage collection.
The garbage collection follow a head/tail algorithm. Data is always
appended to the head and garbage collection always happens on the
tail (exploring concurrency again).

Since the vlog has key info it is trivial to consul the LSM tree
to check if a value is garbage or not.

Because of the vlog there is no need to keep the LSM log file,
all consistency guarantees can be performed with the vlog + LSM tree
(traditional LSM has the key+value on the tree and a LSM log file
where all writes are performed first).

Inconsistencies are handled bu checking that data is on the vlog
but not on the tree (data is saved first on the vlog).

The vlog causes WiscKey to have a bigger space amplification,
since more data is duplicated it will use more space than LevelDB
(something like 10-15%).

Usually in key-value stores there is three problems:

* space amplification
* write amplification
* read amplification

You can't optimize for all of them. WiscKey chooses to diminish
read/write amplification, increasing space amplification.

## Conclusion

The results shows WiscKey being 2.4x to 100x faster than LevelDB
LSM. The only case where WiscKey can loose is when the value size
is very small, like 64 bytes, since the benefits of a smaller
LSM tree are less obvious and it has to pay for the indirection.

Basically the pre requisites are:

* SSD with good concurrent random read performance
* Value sizes bigger than 4K

If you fit this case, WiscKey may be a good idea.
