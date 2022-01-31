go-buffer-pool
==================

[![](https://img.shields.io/badge/made%20by-Protocol%20Labs-blue.svg?style=flat-square)](https://protocol.ai)
[![](https://img.shields.io/badge/project-libp2p-yellow.svg?style=flat-square)](https://libp2p.io/)
[![](https://img.shields.io/badge/freenode-%23libp2p-yellow.svg?style=flat-square)](https://webchat.freenode.net/?channels=%23libp2p)
[![codecov](https://codecov.io/gh/libp2p/go-buffer-pool/branch/master/graph/badge.svg)](https://codecov.io/gh/libp2p/go-buffer-pool)
[![Travis CI](https://travis-ci.org/libp2p/go-buffer-pool.svg?branch=master)](https://travis-ci.org/libp2p/go-buffer-pool)
[![Discourse posts](https://img.shields.io/discourse/https/discuss.libp2p.io/posts.svg)](https://discuss.libp2p.io)

> A variable size buffer pool for go.

## Table of Contents

- [About](#about)
    - [Advantages over GC](#advantages-over-gc)
    - [Disadvantages over GC:](#disadvantages-over-gc)
- [Contribute](#contribute)
- [License](#license)

## About

This library provides:

1. A buffer pool for re-using byte buffers of _varying_ sizes. Unlike `sync.Pool`, this pool will always return an appropriately sized buffer (the next power of two) so it's appropriate for applications that need to pool dynamically sized buffers.
2. A `bytes.Buffer` implementation backed by a buffer pool. Unlike `bytes.Buffer`, `pool.Buffer` will automatically "shrink" on read, using the buffer pool to avoid causing too much work for the allocator. This is primarily useful for long lived buffers that usually sit empty.

### Advantages over GC

* Reduces Memory Usage:
  * We don't have to wait for a GC to run before we can reuse memory. This is essential if you're repeatedly allocating large short-lived buffers.

* Reduces CPU usage:
  * It takes some load off of the GC (due to buffer reuse).
  * We don't have to zero buffers (fewer wasteful memory writes).

### Disadvantages over GC:

* Can leak memory contents. Unlike the go GC, we *don't* zero memory.
* All buffers have a capacity of a power of 2. This is fine if you either expect these buffers to be temporary.
* Requires that buffers be explicitly put back into the pool. This can lead to race conditions and memory corruption if the buffer is released while it's still in use.

## Contribute

PRs are welcome!

Small note: If editing the Readme, please conform to the [standard-readme](https://github.com/RichardLitt/standard-readme) specification.

## License

MIT © Protocol Labs
BSD © The Go Authors

---

The last gx published version of this module was: 0.1.3: QmQDvJoB6aJWN3sjr3xsgXqKCXf4jU5zdMXpDMsBkYVNqa
