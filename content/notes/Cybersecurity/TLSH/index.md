---
title: TLSH
weight: 210
menu:
  notes:
    name: TLSH
    identifier: notes-cybersecurity-tlsh
    parent: notes-cybersecurity
    weight: 10
---


<!-- TLSH -->
{{< note title="TLSH" >}}
Trend Locality Sensitive Hash.

Standard TLSH hash is 70 characters long.

All 3-grams from a sliding window of 5 bytes are used to compute an array of bucket counts, which are used to form the digest body.

Based on the calculation of bucket counts (as calculated above) the three quartiles are calculated (referred to as q1, q2, and q3 respectively).

The digest body is constructed based on the values of the quartiles in the array of bucket counts, using two bits per 128 buckets to construct a 32 byte digest.

The digest header is composed of a checksum, the logarithm of the byte string length and a compact representation of the histogram of bucket counts using the ratios between the quartile points for q1:q3 and q2:q3.

The TLSH distance of zero represents that the files are likely identical, and scores greater than that indicate greater degrees of dissimilarity.




{{< /note >}}