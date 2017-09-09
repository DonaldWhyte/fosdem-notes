### gRPC 101 - building fast and efficient microservices

Date: Sun 5th Feb
Time: 11:00
Speaker: Ray Tsang

#### Why RPC?

Efficient due to binary encodings

Strongly typed due to IDL (interface definition language) 

Provides perations not expressive by REST (not expressable by HTML CRUD verbs)

Challenge 1: making RPC protocols interoperable with different languages and platforms

Challenge 2: existing RPCs is difficult for developers to use

Google uses Stubby for RPC -- `O(10^10)` RPC per second

gRPC -- the open-sourced version of Stubby

#### gRPC

Uses protobuf3 for IDL

Payloads are binary in protobuf3

Underlying transfer between services is HTTP/2
    - HTTP/2 is a **binary protocol**, unlike HTTP
    - more efficient encoding, less bytes used
    - HTTP/2 streams are multiplexed by default -- means you have to open up less connections
    - Streaming data (not just one-off request sending) is default in HTTP/2 as well
    - HTTP/2 can push data to the client **before they even ask for it**

Greater throughput per CPU, by a large amount

gRPC was built for mobile-first

Most commonly used languages are supported

#### Service Versioning

gRPC handles service versioning in a similar way to BAS. Uses unique tags/IDs
for each field.
