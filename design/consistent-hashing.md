Design Consistent Hashing

Rehashing Problem:
- Traditional method: `serverIndex = hash(key) % N`
- Problem with server addition/removal:
  - Changing the number of servers causes many keys to remap, leading to cache misses.

Consistent Hashing:
- Definition: Only a small fraction of keys need remapping when the hash table is resized.
- Hash Space and Ring:
  - Hash space: 0 to 2^160 - 1 (SHA-1 range).
  - Hash ring: Circular representation of hash space.

Hash Servers:
- Map servers onto the ring using a hash function based on server IP/name.
- Keys are also hashed onto the ring.
- Server lookup: Go clockwise from key position to find the server.

Adding/Removing Servers:
- Adding a server:
  - Only a fraction of keys are redistributed.
- Removing a server:
  - Only keys on the removed server need to be redistributed.

Issues in Basic Approach:
1. Uneven partition sizes.
2. Non-uniform key distribution.

Virtual Nodes:
- Concept: Each server is represented by multiple virtual nodes.
- Benefits:
  - More balanced key distribution.
  - Standard deviation of key distribution decreases with more virtual nodes.

Finding Affected Keys:
- Adding a server: Redistribute keys in the range from new server to the next server counterclockwise.
- Removing a server: Redistribute keys in the range from removed server to the next server counterclockwise.

Wrap Up:
- Benefits of Consistent Hashing:
  - Minimizes key redistribution when adding/removing servers.
  - Facilitates horizontal scaling.
  - Mitigates hotspot key problem.
- Real-World Uses:
  - Amazon's Dynamo database.
  - Apache Cassandra.
  - Discord chat application.
  - Akamai content delivery network.
  - Maglev network load balancer.