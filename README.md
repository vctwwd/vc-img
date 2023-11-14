# vc-img
![](https://raw.githubusercontent.com/vctwwd/vc-img/main/img/test.png)
# Introduction
## Brief
* ############

## Installation
### Preinstallation
* GNU Multi Precision 6.2.1 (built with C++ suuport)
* Boost 1.80
* OPENSSL (1.1.1 is suitable)
* github.com/fmtlib/fmt (9.1.0 is suitable)
* relic (for OT RCurve)
* google benchmark & googletest (for network test and context benchmark)

### Installation Steps
* $ git clone https://github.com/qazw52/PPPU.git
* $ unzip PPPU.zip
* $ cd PPPU
* Edit CMakeLists.txt and modify the paths for CMAKE_CXX_COMPILER(should be newest), boost, fmt, Google benchmark, and Google test in CMakeLists.txt to your own installation path.
* $ mkdir build
* $ cd build
* $ cmake ..
* $ make
* For now the executable file is located in build.

### Quick Start
* 

## Layers
### Hierarchical Structure
```
                                  Based on          Based on          Used in
  Data Layer          : Datatype <-------- NDArray <-------- Context --------> Function Layer (MPC)

                                       Based on          Used in
  Communication Layer : Serialization <-------- Network --------> Function Layer (MPC)

                            Based on
  Function Layer      : OT <-------- PSI
                         |            |  Used in      Used in
                         -----------------------> ML <-------- Context, Network
                         |
                        MPC

                               Read Config                     Used in
  Other Layer         : Config -----------> Network     Tools --------> All the other layers
```
### Data Layer
* Context

  Contains a Value object that encapsulates all the elements required for MPC computation, including the MPC protocol, plain data type, and shared data type.  
  Each Value object can only be in three privacy states (public, private, share, the first two collectively referred to as plain), and each privacy state may have different Datatypes stored under different MPC protocols.  

  Basic   - Implemented arithmetic operations including input and output, bitwise decomposition, and addition, substraction, multiplication and so on, apis are in basic.h.  
            The factory function used to generate Value objects is located at factory.h.  
            The special implementation for arithmetic operations on fixed point numbers is located in fxp.h.  
            The base function and the warp function based on privacy conditions and protocols is located at raw.h, primitive.h and prot_wrapper respectively.  
            Encode to Value and Decode to NDArray functions are in util.h.  
  Compare - Implemented comparation operations such as lesser and greater, apis are in compare.h.  
  Math    - Implemented mathematical operations such as division, exponential, logarithmic, polynomial, power, round to, sigmoid, sqrt operations, apis are in corresponding headers.  
  Shape   - Implemented shape changed operations such as concatenate, reduce and sort, apis are in corresponding headers.  

* NDArray
  Contains a NDArray object that implements an n-dimensional array and also its computation, apis are in ndarray_ref.h.  

  Implementation of One-dimensional array is in array_ref.h. Data storage is in buffer.h.  
  
  Other headers are the functions, such as concatenate, iterator, packbits, serialization, slice, apis are in corresponding headers.  

* Datatype

  Contains Z2k, Zp, FixedPoint objects that implements a kinds of data types and their corresponding operations, apis are in corresponding headers.  


  The implementation of operations of the above-mentioned data types are stored in mpx##.h, based on gmp.  

  Z2k        - A finite field whose modulus is 2^K, and provides basic operations and operations on finite fields, apis are in Z2k.h.  
  Zp         - A finite field whose modulus is prime, and provides basic operations and operations on finite fields, apis are in Zp.h.  
  FixedPoint - Based on Z2k, a implementation of fixed point number, and provides basic operations, apis are in fix_point.h.  

### Communication Layer
* Network
  Contains a MultiPartyPlayer object that implements communication among n party players. Also provides a TwoPartyPlayer object used between 2 party players.

  Both MultiPartyPlayer and TwoPartyPlayer have Secure and Plain version, which used SSL and TCP as its sockets respectively, which are contains real apis used in computation, in multi_party_player.h and two_party_player.h respectively.

  Connection functions are in mp_connect.h, the sockets are in socket_package.h, player id are in playerid.h and communication information are in statistics.h. Other headers contains some implementations.

* Serialization
  Contains a Serializer and a Deserializer used in communicaton. Serializer converts ArrayRef to ByteVector and Deserializer convert ByteVector to ArrayRef. ByteVector is used in communication. Apis are in serializer.h and deserializer.h.

### Function Layer
* ML
  Machine learning functions based on PPPU.
  
  ##########

* MPC
  Contains MPC protocols used in computations. APIs are in protocol/protocol.hpp. To use protocols, we have Protocol object and Preprocessing object to contains a protocol, apis are in protocol.hpp and preprocessing.hpp.

  Now we implements semi2k protocol.

* PSI
  Contains PSI (Private Set Intersection) functions with now 2 algorithms (crypto20, EcdhPSI). APIs are in algorithm/PSISender.h and algorithm/PSIReceiver.h.

* OT
  Contains base OT (Oblivious Transfer) functions. APIs are in OTInterface.h and BasicDefines.h.

### Other Layers
* Tools
  Contains the math operations and data structures used in above layers.

* Config
  Used to read network config based on boost program options.

## Protocol
* Semi2k-SPDZ
  A semi-honest NPC-protocol similar to SPDZ but requires a trusted third party to generate offline randoms. By default this protocol now uses trusted first party. Hence, it should be used for debugging purposes only.

## APIs
### Context


### Network


### MPC


### ML


### OT


### PSI
