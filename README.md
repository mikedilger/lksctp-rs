lksctp-rs
=========

Rust bindings to the Linux Kernel's SCTP network functionality.

Apologies
---------
I apologize for the long acronym of a name, but "lksctp" is the project name that
I have created bindings to, so I thought that should be preserved.

* LK = Linux Kernel
* SCTP = Stream Control Transmission Protocol
* RS = (for) RuSt

SCTP
----

SCTP is a Transport Layer protocol.  It sits on top of IP directly, along
side of TCP, UDP, and others.  In many ways, it is a new-and-improved
version of TCP, originally designed for the needs of digital telephony.

In particular, it supports all of the following features that TCP supports:

* Connection oriented
* Reliable
* Sequence preservation
* Flow Control & Congestion Control

In addition, it adds:

* **Multiplexed streams**:  Over a single connection you can multiplex multiple streams
  of information.
* **Multihoming of end-points**:  An end point can have multiple IP addresses, allowing
  for network failover/reliability  (Note: it will need to be a single machine, or
  replicate the state information of the endpoint, which is probably hard).
* **Preservation of message boundaries**:  If you find yourself writing some scheme to
  frame messages under TCP, you may want to consider SCTP
* **Optional unordered delivery**:  If subsequent packets supercede former ones, you
  generally don't want to wait for the former.  With TCP, you have to wait, but
  with SCTP you can proceed with the latest packet and ignore the former one that hasn't
  arrived yet.
* **SYN flooding resiliance**: SYN flooding which has plagued TCP as Denial-of-Service
  attacks, is much less successful against SCTP as it was designed with this attack in
  mind.  It uses a 4-way handshake and large cookies for association verification and
  authenticity (with md5 or sha-1)

LKSCTP:
-------

SCTP could be implemented entirely in user space.  However, the kernel implementation
will be faster.  This package creates bindings to that kernel implementation.

System/Installation Issues:
---------------------------

You will need to enable CONFIG_IP_SCTP in your kernel, and you will also need libsctp.so from
http://lksctp.sourceforge.net/

These bindings were developed on a 64-bit linux system.  There may be some issues on 32-bit:

> The sctp.h header file external types were manually defined, as bindgen/clang couldn't
> seem to parse through all the linux system headers, and I didn't check closely to verify
> it was going to be correct on 32-bit
