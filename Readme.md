# What is this
A decoder for the zstd compression format as defined in: [This document](https://github.com/facebook/zstd/blob/dev/doc/zstd_compression_format.md#appendix-a---decoding-tables-for-predefined-codes).

It is NOT a compressor. I dont plan on implementing that part either, at least not in the near future. (If someone is motivated enough I will of course accept a pull-request!)

This is currently just a work in progress project that I might never finish. Use/fork at own risk ;) It does work correctly at least for the test-set of files I used, YMMV.

# Current Status
## Can do:
1. Parse all files in /decodecorpus_files. These were generated with [decodecorpus](https://github.com/facebook/zstd/tree/dev/tests) by the original zstd developers
1. Decode all of them correctly into the output buffer
1. Decode all the decode_corpus files (1000+) I created locally 

## Cannot do
2. decode frames partially so a user can use it as a stream rather than load the whole frame into memory at once

## Roadmap
1. Find more bugs
1. Make a nice API maybe io::Read based, maybe not, we'll see
1. More tests (especially unit-tests for the bitreaders) to find even more bugs

# What you might notice
I already have done a decoder for zstd in golang. [here](https://github.com/KillingSpark/sparkzstd). This was a first try and it turned out very inperformant. I could have tried to rewrite it to use less allocations while decoding etc etc but that seemed dull (and unecessary since klauspost has done a way better golang implementation can compress too [here](https://github.com/klauspost/compress/tree/master/zstd))

## Why another one
Well I wanted to show myself that I am actually able to learn from my mistakes. This implementation should be way more performant since I from the get go focussed on reusing allocated space instead of reallocating all the decoding tables etc.

Also this time I did most of the work without looking at the original source nor the [educational decoder](https://github.com/facebook/zstd/tree/dev/doc/educational_decoder).
So it is somewhat uninfluenced by those. (But I carried over some memories from the golang implementation). 
I used it to understand the huffman-decoding process and the process of how to exactly distribute baseline/num_bits in the fse tables on which the documentation is somewhat ambigous. 
After having written most of the code I used my golang implementation for debugging purposes (known 'good' results of the different steps).

## Known bugs:
1. Currently none. This means nothing though the fuzzer still finds possibilities to crash the library so there are most likely still some bad ones around